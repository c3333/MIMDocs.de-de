---
title: Konfigurieren des Microsoft Identity Manager-Connectors für Microsoft Graph für B2B | Microsoft-Dokumentation
author: billmath
description: Der Microsoft Graph-Connector ermöglicht die Lebenszyklusverwaltung für externe AD-Benutzerkonten. In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 60af5cee7d05eb8b8c5fb279f4f182d901e91632
ms.sourcegitcommit: 80507a128d2bc28ff3f1b96377c61fa97a4e7529
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83280115"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure AD-Business-to-Business-Kollaboration (B2B) mit Microsoft Identity Manager(MIM) 2016 SP1 mit Azure-Anwendungsproxy
============================================================================================================================

Das Anfangsszenario ist die Lebenszyklusverwaltung für externe AD-Benutzerkonten.   In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen über den [Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) oder andere Gatewaymechanismen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren. Der Azure AD-Anwendungsproxy erfordert, dass jeder Benutzer zur Identifikation und Delegierung ein eigenes AD DS-Konto hat.

## <a name="scenario-specific-guidance"></a>Szenariospezifische Anleitungen

Einige Annahmen bei der Konfiguration von B2B mit MIM und dem Azure AD-Anwendungsproxy:

-   Sie haben bereits ein lokales AD bereitgestellt und Microsoft Identity Manager installiert sowie die Grundkonfiguration von MIM Service, MIM Portal, Active Directory Management Agent (AD MA) und FIM Management Agent (FIM MA) vorgenommen.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Sie haben bereits die Anweisungen im Artikel zum Herunterladen und Installieren des [Graph-Connectors](microsoft-identity-manager-2016-connector-graph.md) befolgt.

-   Sie haben Azure AD Connect für die Synchronisierung von Benutzern und Gruppen mit Azure AD konfiguriert.

-   Anwendungsproxyconnectors und Connectorgruppen sind bereits eingerichtet. Falls dies nicht der Fall ist, finden Sie [hier](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) Unterstützung für die Installation und Konfiguration.

-   Sie haben bereits eine oder mehrere Anwendungen veröffentlicht, die auf der integrierten Windows-Authentifizierung oder auf individuellen AD-Konten via Azure AD-Anwendungsproxy basieren.

-   Sie haben einen oder mehrere Gäste eingeladen oder laden diese ein, wodurch ein bzw. mehrerer Benutzer in Azure AD erstellt wurden. <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Beispielszenario für die B2B-End-to-End-Bereitstellung

Dieses Handbuch baut auf das folgende Szenario auf:

Contoso Pharmaceuticals arbeitet mit Trey Research Inc. als Teil ihrer F&E-Abteilung. Die Mitarbeiter von Trey Research müssen auf die von Contoso Pharmaceuticals zur Verfügung gestellte Anwendung für Forschungsberichte zugreifen.

-   Contoso Pharmaceuticals befindet sich auf einem eigenen Mandanten mit einer eigenen Domäne.

-   Jemand hat einen externen Benutzer zum Mandanten von Contoso Pharmaceuticals eingeladen.
    Dieser Benutzer hat die Einladung angenommen und kann auf freigegebene Ressourcen zugreifen.

-   Contoso Pharmaceuticals hat eine Anwendung über App-Proxy veröffentlicht. In diesem Szenario ist die Beispielanwendung das MIM-Portal. Dies würde es einem Gastbenutzer ermöglichen, an MIM-Prozessen, z. B. Helpdeskszenarien, teilzunehmen oder den Zugriff auf Gruppen in MIM anzufordern.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Konfigurieren Sie AD und Azure AD Connect, um Benutzer auszuschließen, die von Azure AD hinzugefügt wurden.

Standardmäßig geht Azure AD Connect davon aus, dass Benutzer, die keine Administratoren in Active Directory sind, mit Azure AD synchronisiert werden müssen.  Wenn Azure AD Connect einen vorhandenen Benutzer in Azure AD findet, der mit dem Benutzer des lokalen AD übereinstimmt, ordnet Azure AD Connect die beiden Konten einander zu und geht davon aus, dass es sich um eine frühere Synchronisierung des Benutzers handelt, und macht das lokale AD autoritativ.  Dieses Standardverhalten ist jedoch nicht für den B2B-Fluss geeignet, bei dem das Benutzerkonto seinen Ursprung in Azure AD hat. 

Daher müssen die Benutzer, die von MIM aus Azure AD in AD DS eingebracht wurden, so gespeichert werden, dass Azure AD nicht versucht, diese Benutzer wieder mit Azure AD zu synchronisieren.
Eine Möglichkeit, dies zu tun, besteht darin, eine neue Organisationseinheit in AD DS anzulegen und Azure AD Connect so zu konfigurieren, dass diese Organisationseinheit ausgeschlossen wird.  

Weitere Informationen finden Sie unter [Azure AD Connect-Synchronisierung: Konfigurieren der Filterung](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Erstellen der Azure AD-Anwendung 


Anmerkung: Bevor Sie im MIM-Synchronisierungsdienst den Verwaltungs-Agent für den Graph-Connector erstellen, sollten Sie die Anleitung zur Bereitstellung des [Graph-Connectors](microsoft-identity-manager-2016-connector-graph.md) gelesen und eine Anwendung mit einer Client-ID und einem Geheimnis erstellt haben.
Stellen Sie sicher, dass die Anwendung für mindestens eine dieser Berechtigungen autorisiert wurde: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` oder `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Erstellen des neuen Verwaltungs-Agents


In der Benutzeroberfläche von Synchronization Service Manager wählen Sie **Connectors** und **Erstellen** aus.
Wählen Sie **Graph (Microsoft)** aus, und geben Sie ihm einen aussagekräftigen Namen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Verbindung

Auf der Seite „Konnektivität“ müssen Sie die Version der Graph-API angeben. Die PAI für die Produktionsumgebung ist **V 1.0**, für die Testumgebung **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globale Parameter

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Bereitstellungshierarchie konfigurieren

Diese Seite dient dazu, die DN-Komponente, z.B. OE, dem bereitzustellenden Objekttyp zuzuordnen, z.B. organizationalUnit. Dies ist für dieses Szenario nicht erforderlich, also behalten Sie die Standardeinstellung bei, und klicken Sie auf „Weiter“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partitionen und Hierarchien konfigurieren

Wählen Sie auf der Seite „Partitionen und Hierarchien“ alle Namespaces mit Objekten aus, die Sie importieren und exportieren möchten.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Objekttypen auswählen

Wählen Sie auf der Seite „Objekttypen“ die Objekttypen aus, die Sie importieren möchten. Sie müssen mindestens „Benutzer“ auswählen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Attribute auswählen

Wählen Sie auf dem Bildschirm „Attribute auswählen“ Attribute aus Azure AD aus, die für die Verwaltung von B2B-Benutzern in AD benötigt werden. Das Attribut „ID“ ist erforderlich.  Die Attribute `userPrincipalName` und `userType` werden später in dieser Konfiguration verwendet.  Andere Attribute sind optional, einschließlich

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Anker konfigurieren

Auf dem Bildschirm „Anker konfigurieren“ ist das Konfigurieren des Ankerattributs ein erforderlicher Schritt. Verwenden Sie standardmäßig das Attribut „ID“ für die Benutzerzuordnung.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Connectorfilter konfigurieren

Auf der Seite „Connector-Filter konfigurieren“ können Sie Objekte in MIM anhand eines Attributfilters filtern. In diesem Szenario für B2B besteht das Ziel darin, nur Benutzer einzubringen, deren `userType`-Attribut den Wert `Guest` aufweist, und keine Benutzer mit dem userType `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Zusammenführungs- und Projektionsregeln konfigurieren

In diesem Handbuch wird davon ausgegangen, dass Sie eine Synchronisierungsregel erstellen.  Da die Konfiguration der Zusammenführungs- und Projektionsregeln über die Synchronisierungsregel erfolgt, ist es nicht notwendig, eine Zusammenführung und eine Projektion auf den Connector selbst zu identifizieren. Behalten Sie die Standardeinstellung bei, und klicken Sie auf „OK“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Attributfluss konfigurieren

In diesem Handbuch wird davon ausgegangen, dass Sie eine Synchronisierungsregel erstellen.  Projektion ist nicht erforderlich, um den Attributfluss bei der MIM-Synchronisierung zu definieren, da er von der später erstellten Synchronisierungsregel abgewickelt wird. Behalten Sie die Standardeinstellung bei, und klicken Sie auf „OK“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Aufheben der Bereitstellung konfigurieren

Mit der Einstellung zur Konfiguration der Aufhebung der Bereitstellung können Sie die MIM-Synchronisierung so konfigurieren, dass das Objekt gelöscht wird, wenn das MetaVerse-Objekt gelöscht wird. In diesem Szenario machen Sie sie zu Trennern, da das Ziel darin besteht, sie in Azure AD zu belassen. In diesem Szenario werden keine Elemente nach Azure AD exportiert, und der Connector wird nur für den Import konfiguriert.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Erweiterungen konfigurieren

Die Konfiguration von Erweiterungen auf diesem Verwaltungs-Agent ist optional und nicht erforderlich, da eine Synchronisierungsregel verwendet wird. Hätten Sie sich zu einem früheren Zeitpunkt für die Verwendung einer Vorabregel im Attributfluss entschieden, gäbe es eine Option zum Definieren der Regelerweiterung.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Das MetaVerse-Schema erweitern


Bevor Sie die Synchronisierungsregel erstellen, müssen Sie ein Attribut namens „Benutzerprinzipalname“ erstellen, das mithilfe des MV-Designers an das Personenobjekt gebunden ist.

Wählen Sie im Synchronisierungsclient den „Metaverse-Designer“ aus.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Wählen Sie dann den Objekttyp „Person“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klicken Sie unter „Aktionen“ auf „Attribut hinzufügen“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Machen Sie dann die folgenden Angaben

Attributname: **Benutzerprinzipalname**

Attributtyp: **String (Indexable)** (Zeichenfolge (indizierbar))

Indiziert = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Erstellen von Synchronisierungsregeln für den MIM-Dienst

In den folgenden Schritten beginnt die Zuordnung des B2B-Gastkontos und des Attributflusses. Hier wird davon ausgegangen, dass Sie bereits den Active Directory-Verwaltungs-Agent konfiguriert haben und der FIM-Verwaltungs-Agent so konfiguriert ist, dass die Benutzer zum MIM-Dienst und -Portal weitergeleitet werden.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Die nächsten Schritte erfordern die Hinzufügung einer minimalen Konfiguration zum FIM- und AD-Verwaltungs-Agent.

Weitere Details zur Konfiguration finden Sie hier: <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> – How Do I Provision Users to AD DS (Bereitstellen von Benutzern in AD DS).

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Synchronisierungsregel: Gastbenutzer in Metaverse importieren, um Dienst-Metaverse aus Azure Active Directory zu synchronisieren<br>

Navigieren Sie zum MIM-Portal, wählen Sie „Synchronisierungsregeln“ aus, und klicken Sie auf „Neu“.  Erstellen Sie eine eingehende Synchronisierungsregel für den B2B-Fluss über den Graph-Connector.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Achten Sie darauf, dass Sie im Schritt „Beziehungskriterien“ die Option „Ressource in FIM erstellen“ auswählen.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Konfigurieren Sie die folgenden Regeln für den eingehenden Attributfluss.  Stellen Sie sicher, dass Sie die Attribute `accountName`, `userPrincipalName` und `uid` füllen, da sie später in diesem Szenario verwendet werden:

| **Nur Anfangsfluss** | **Verwenden als Existenzprüfung** | **Fluss (Quellwert ⇒ FIM-Attribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | `[displayName⇒displayName](javascript:void(0);)`                        |
|                       |                           | `[Left(id,20)⇒accountName](javascript:void(0);)`                        |
|                       |                           | `[id⇒uid](javascript:void(0);)`                                         |
|                       |                           | `[userType⇒employeeType](javascript:void(0);)`                          |
|                       |                           | `[givenName⇒givenName](javascript:void(0);)`                            |
|                       |                           | `[surname⇒sn](javascript:void(0);)`                                     |
|                       |                           | `[userPrincipalName⇒userPrincipalName](javascript:void(0);)`            |
|                       |                           | `[id⇒cn](javascript:void(0);)`                                          |
|                       |                           | `[mail⇒mail](javascript:void(0);)`                                      |
|                       |                           | `[mobilePhone⇒mobilePhone](javascript:void(0);)`                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Synchronisierungsregel: Gastbenutzerkonto für Active Directory Domain Services erstellen 

Anhand dieser Synchronisierungsregel wird der Benutzer in Active Directory erstellt.  Stellen Sie sicher, dass der Fluss für `dn` den Benutzer in die Organisationseinheit einordnen muss, die von Azure AD Connect ausgeschlossen wurde.  Aktualisieren Sie außerdem den Fluss für `unicodePwd` so, dass sie Ihrer AD-Kennwortrichtlinie entspricht. Der Benutzer muss das Kennwort nicht kennen.  Beachten Sie, dass der Wert von `262656` für `userAccountControl` die Flags `SMARTCARD_REQUIRED` und `NORMAL_ACCOUNT` kodiert.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Flussregeln:

| **Nur Anfangsfluss** | **Verwenden als Existenzprüfung** | **Fluss (FIM-Wert ⇒ Zielattribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Optionale Synchronisierungsregel: B2B-Gastbenutzerobjekte-SID importieren, um Anmeldung bei MIM zuzulassen 

Diese eingehende Synchronisierungsregel bringt das SID-Attribut des Benutzers aus Active Directory wieder in MIM, sodass der Benutzer auf das MIM-Portal zugreifen kann.  Für das MIM-Portal ist es erforderlich, dass die Attribute `samAccountName`, `domain` und `objectSid` des Benutzers in der Datenbank des MIM-Diensts gefüllt wurden.

Konfigurieren Sie das externe Quellsystem als `ADMA`, da das Attribut `objectSid` von AD automatisch festgelegt wird, wenn MIM den Benutzer erstellt.
 
Beachten Sie Folgendes: Wenn Sie konfigurieren, dass Benutzer im MIM-Dienst erstellt werden sollen, müssen Sie sicherstellen, dass sie nicht in den Geltungsbereich von Sets fallen, die für die Regeln der Mitarbeiter-SSPR-Verwaltungsrichtlinien bestimmt sind.  Möglicherweise müssen Sie Ihre festgelegten Definitionen ändern, um Benutzer auszuschließen, die vom B2B-Fluss erstellt wurden. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Nur Anfangsfluss** | **Verwenden als Existenzprüfung** | **Fluss (Quellwert ⇒ FIM-Attribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Synchronisierungsregeln ausführen

Anschließend laden Sie den Benutzer ein, und führen dann die Synchronisierungsregeln des Verwaltungs-Agents in der folgenden Reihenfolge aus:

-   Vollständiger Import und Synchronisierung auf dem `MIMMA`-Verwaltungs-Agent.  Dadurch wird sichergestellt, dass für die MIM-Synchronisierung die neuesten Synchronisierungsregeln konfiguriert wurden.

-   Vollständiger Import und Synchronisierung auf dem `ADMA`-Verwaltungs-Agent.  Dadurch wird sichergestellt, dass MIM und Active Directory konsistent sind.  Zu diesem Zeitpunkt wird es noch keine anstehenden Exporte für Gäste geben.

-   Vollständiger Import und Synchronisierung auf dem B2B-Graph-Verwaltungs-Agent.  Dies bringt die Gastbenutzer in MetaVerse.  Zu diesem Zeitpunkt stehen ein oder mehrere zu exportierende Konten für `ADMA` aus.  Wenn keine Exporte ausstehen, vergewissern Sie sich, dass Gastbenutzer in den Connectorbereich importiert wurden und dass für sie Regeln konfiguriert wurden, um ihnen AD-Konten zuzuweisen.

-   Export, Deltaimport und Synchronisierung auf dem `ADMA`-Verwaltungs-Agent.  Wenn die Exporte fehlgeschlagen sind, überprüfen Sie die Regelkonfiguration, und überprüfen Sie, ob Schemaanforderungen fehlen. 

-   Export, Deltaimport und Synchronisierung auf dem `MIMMA`-Verwaltungs-Agent.  Wenn dies abgeschlossen ist, sollte es keine ausstehenden Exporte mehr geben.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Optional: Anwendungsproxy für B2B-Gäste, die sich beim MIM-Portal anmelden

Jetzt sind die Synchronisierungsregeln in MIM erstellt. Definieren Sie nun in der Anwendungsproxykonfiguration die Verwendung des Cloudprinzipals, um KCD auf dem Anwendungsproxy zuzulassen.
Fügen Sie dann als Nächstes den Benutzer manuell zu den verwalteten Benutzern und Gruppen hinzu. Die Optionen, den Benutzer erst nach dessen Erstellung in MIM anzuzeigen, um den Gast zu einer Office-Gruppe hinzuzufügen, sobald er bereitgestellt wurde, erfordern weitere Konfigurationsschritte, die in diesem Dokument nicht behandelt werden.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Nach Abschluss der Konfiguration verfügen Sie über die Anmeldeinformationen des B2B-Benutzers und sehen die Anwendung.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Nächste Schritte
----------

[Bereitstellen von Benutzern in AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Funktionenreferenz für FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Download des Microsoft Identity Manager-Connectors für Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
