---
title: Der Microsoft Identity Manager-Verwaltungs-Agent für Microsoft Graph | Microsoft-Dokumentation
author: fimguy
description: Microsoft Graph (Vorschau) ermöglicht die Lebenszyklusverwaltung für externe AD-Benutzerkonten. In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren.
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 77d322f447546897ad18f0981e5faad12efafef1
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Azure AD-Business-to-Business-Kollaboration (B2B) mit Microsoft Identity Manager(MIM) 2016 SP1 mit Azure-Anwendungsproxy (öffentliche Vorschau)
============================================================================================================================

Das Anfangsszenario in der Vorschau ist die Lebenszyklusverwaltung für externe AD-Benutzerkonten.   In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen über den [Azure AD-Anwendungsproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) oder andere Gatewaymechanismen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren. Der Azure AD-Anwendungsproxy erfordert, dass jeder Benutzer zur Identifikation und Delegierung ein eigenes AD DS-Konto hat.

## <a name="scenario-specific-supported-guidance"></a>Anweisungen zu bestimmten Szenarien

In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen über den [Azure AD-Anwendungsproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) oder andere Gatewaymechanismen gewähren. Der Azure AD-Anwendungsproxy erfordert, dass jeder Benutzer zur Identifikation und Delegierung ein eigenes AD DS-Konto hat.

Einige Annahmen bei der Konfiguration von B2B mit MIM und dem Azure-Anwendungsproxy

-   Der [Graph-Verwaltungs-Agent](microsoft-identity-manager-2016-connector-graph.md) ist bereits installiert.

-   Es ist eine lokale AD- und Azure AD Connect-Konfiguration für die Synchronisierung von Benutzern und Gruppen mit Azure AD vorhanden.

    -   Office-Gruppen steuern den Anwendungszugriff mit [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory).

-   Anwendungsproxyconnectors und Connectorgruppen sind bereits eingerichtet. Falls dies nicht der Fall ist, finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) Unterstützung für die Installation und Konfiguration.

-   Eine oder mehrere Anwendungen wurden veröffentlicht, die auf der integrierten Windows-Authentifizierung oder auf individuellen AD-Konten via Azure AD-Anwendungsproxy basieren.

-   Sie haben einen oder mehrere Gäste, die in Azure AD erstellt wurden, eingeladen und laden diese ein. (<https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-self-service-portal>)

-   Microsoft Identity Manager ist installiert, und die Basiskonfiguration von Dienst, Portal und Active Directory-Verwaltungs-Agent ist erfolgt.
    <https://docs.microsoft.com/en-us/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B-End-to-End-Bereitstellung

Szenario

Contoso Pharmaceuticals arbeitet mit Trey Research Inc. als Teil ihrer F&E-Abteilung. Die Mitarbeiter von Trey Research müssen auf die von Contoso Pharmaceuticals zur Verfügung gestellte Anwendung für Forschungsberichte zugreifen.

-   Contoso Pharmaceuticals ist ein unabhängiger Mandant mit einer eigenen Domäne.

-   Ein externer Benutzer wird zum Mandanten von Contoso Pharmaceuticals eingeladen.
    Dieser Benutzer hat die Einladung angenommen und kann auf freigegebene Ressourcen zugreifen.

-   Veröffentlicht von einer Anwendung über Anwendungsproxy und in diesem Szenario als Beispiel dafür, wie MIM-Dienst und -Portal für Gastbenutzer zur Teilnahme am MIM-Prozess verwendet werden können (Beispiel: Helpdeskszenarien).

## <a name="create-the-graph-management-agent"></a>Erstellen des Graph-Verwaltungs-Agents

Hinweis: Vor dem Erstellen des Connectors stellen Sie sicher, dass Sie [Graph-Verwaltungs-Agents](microsoft-identity-manager-2016-connector-graph.md) gelesen haben.

In der Benutzeroberfläche von Synchronization Service Manager wählen Sie **Connectors** und **Erstellen** aus.
Wählen Sie **Graph (Microsoft)** aus, und geben Sie ihm einen aussagekräftigen Namen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Verbindung

Auf der Seite „Konnektivität“ müssen Sie angeben, dass die Graph-API-Version für Produktionsumgebungen **V 1.0** und für Testumgebungen **Beta** ist.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Funktionen

Auf der Seite „Globale Parameter“ konfigurieren Sie den DN für das Deltaänderungsprotokoll und weitere LDAP-Funktionen. Die Seite ist bereits mit den Informationen des LDAP-Servers ausgefüllt.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Globale Parameter

Auf der Seite „Globale Parameter“ konfigurieren Sie den DN für das Deltaänderungsprotokoll und weitere LDAP-Funktionen. Die Seite ist bereits mit den Informationen des LDAP-Servers ausgefüllt.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Bereitstellungshierarchie konfigurieren

Diese Seite dient dazu, die DN-Komponente, z.B. OE, dem bereitzustellenden Objekttyp zuzuordnen, z.B. organizationalUnit. Übernehmen Sie die Standardeinstellung, und klicken Sie auf „Weiter“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partitionen und Hierarchien konfigurieren

Wählen Sie auf der Seite „Partitionen und Hierarchien“ alle Namespaces mit Objekten aus, die Sie importieren und exportieren möchten.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Objekttypen auswählen

Wählen Sie auf der Seite „Partitionen und Hierarchien“ alle Namespaces mit Objekten aus, die Sie importieren und exportieren möchten.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Attribute auswählen

Wählen Sie auf der Seite „Attribute“ die Attribute aus, die Sie zum Verwalten von B2B-Benutzern benötigen. Das Attribut „id“ ist erforderlich.

-   id

-   displayName

-   mail

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Anker konfigurieren

Beim Konfigurieren eines Ankers ist es erforderlich, einen Anker zu konfigurieren. Verwenden Sie standardmäßig das „id“-Attribut für die Zuordnung.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Connector-Filter konfigurieren

Auf der Seite „Connector-Filter konfigurieren“ können Sie Objekte anhand eines Attributfilters filtern. In diesem B2B-Szenario besteht das Ziel lediglich darin, Benutzer mit dem „userType“ einzuschließen, der einem Gast und nicht einem Mitglied entspricht.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Zusammenführungs- und Projektionsregeln konfigurieren

Die Konfiguration der Zusammenführungs- und Projektionsregeln erfolgt über die Synchronisierungsregel. Es ist nicht notwendig, eine Zusammenführung und eine Projektion auf den Connector selbst zu identifizieren. Behalten Sie die Standardeinstellung bei, und klicken Sie auf „OK“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Attributfluss konfigurieren

Wie bei der Zusammenführung und Projektion ist es nicht notwendig, den Attributfluss zu definieren, da er von der später zu erstellenden Synchronisierungsregel abgewickelt wird. Behalten Sie die Standardeinstellung bei, und klicken Sie auf „OK“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Aufheben der Bereitstellung konfigurieren

Konfigurieren Sie das Aufheben der Bereitstellung, um das Objekt zu löschen, wenn das Metaverse-Objekt gelöscht wird. In diesem Test werden daraus Disconnectors gemacht, da diese in Azure bleiben sollen. Zudem wird nichts aus Azure exportiert, da es hier nur um den Import geht.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Erweiterungen konfigurieren

Die Konfiguration von Erweiterungen auf diesem Verwaltungs-Agent ist optional und nicht erforderlich, da eine Synchronisierungsregel verwendet wird. Hätten Sie sich zu einem früheren Zeitpunkt für eine Vorabregel im Attributfluss entschieden, dann wäre dies eine Option, die festgelegt werden muss.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Erstellen von Synchronisierungsregeln für den MIM-Dienst

In den folgenden Schritten beginnt die Zuordnung des B2B-Gastkontos und des Attributflusses. Hier wird davon ausgegangen, dass Sie bereits den Active Directory-Verwaltungs-Agent konfiguriert haben und der FIM-Verwaltungs-Agent für die Kommunikation mit dem MIM-Dienst und -Portal konfiguriert ist.

Bevor Sie die Synchronisierungsregel erstellen, müssen Sie ein Attribut namens „Benutzerprinzipalname“ erstellen, das mithilfe des MV-Designers an das Personenobjekt gebunden ist.

Wählen Sie im Synchronisierungsclient den „Metaverse-Designer“ aus.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Wählen Sie dann den Objekttyp „Person“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klicken Sie unter „Aktionen“ auf „Attribut hinzufügen“.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Machen Sie schließlich die folgenden Angaben:

Attributname: **Benutzerprinzipalname**

Attributtyp: **Zeichenfolge (indizierbar)**

Indiziert = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Die nächsten Schritte erfordern die minimale Konfiguration des FIM-Dienstverwaltungs-Agents und des Active Directory Domain Services-Verwaltungs-Agents.

Weitere Details zur Konfiguration finden Sie hier: <https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx> – How Do I Provision Users to AD DS (Bereitstellen von Benutzern in AD DS).

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Synchronisierungsregel: Gastbenutzer in Metaverse importieren, um Dienst-Metaverse aus Azure Active Directory zu synchronisieren<br>

Navigieren Sie zum MIM-Dienst und -Portal, wählen Sie „Synchronisierungsregeln“, und klicken Sie auf „Neu“.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Wählen Sie „Ressource in FIM erstellen“ aus.![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Flussregeln:

| **Nur Anfangsfluss** | **Verwenden als Existenzprüfung** | **Fluss (FIM-Wert ⇒ Zielattribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Synchronisierungsregel: Gastbenutzerkonto in Active Directory erstellen 

Anhand dieser Synchronisierungsregel wird der Benutzer in Active Directory erstellt.

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
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Synchronisierungsregel: B2B-Gastbenutzerobjekte SID importieren, um Anmeldung bei MIM zuzulassen 

Anhand dieser Synchronisierungsregel wird der Benutzer in Active Directory erstellt.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Schließlich laden Sie den Benutzer ein, und führen dann die Verwaltung in der folgenden Reihenfolge durch:

-   Vollständiger Import und Synchronisierung auf dem MIMMA-Verwaltungs-Agent

-   Vollständiger Import und Synchronisierung auf dem ADMA_SCONTOSO_B2B-Verwaltungs-Agent

-   Vollständiger Import und Synchronisierung auf dem B2B-Graph-Verwaltungs-Agent

-   Export, Deltaimport und Synchronisierung auf dem ADMA_SCONTOSO_B2B-Verwaltungs-Agent

-   Export, Deltaimport und Synchronisierung auf dem MIMMA-Verwaltungs-Agent

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Endlich: Anwendungsproxy mit B2B-Gast und Anmeldung bei MIM

Jetzt sind die Synchronisierungsregeln in MIM erstellt. Definieren Sie nun in der Anwendungsproxykonfiguration die Verwendung des Cloudprinzips, um KCD auf dem Anwendungsproxy zuzulassen.
Fügen Sie dann als Nächstes den Benutzer manuell zu den verwalteten Benutzern und Gruppen hinzu. Die Optionen, den Benutzer erst nach dessen Erstellung in MIM anzuzeigen, um den Gast zu einer Office-Gruppe hinzuzufügen, sobald er bereitgestellt wurde, erfordern weitere Konfigurationsschritte, die in diesem Dokument nicht behandelt werden.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Nur Anfangsfluss** | **Verwenden als Existenzprüfung** | **Fluss (FIM-Wert ⇒ Zielattribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

Sobald alle konfiguriert sind

Schließlich verfügen Sie über die Anmeldeinformationen des B2B-Benutzers und sehen die Anwendung.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Nächste Schritte
----------

[Bereitstellen von Benutzern in AD DS](https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx)

[Funktionenreferenz für FIM 2010](https://technet.microsoft.com/en-us/library/ff800820(v=ws.10).aspx)

[Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

[Download des Microsoft Identity Manager-Verwaltungs-Agents für Microsoft Graph (Vorschau)](http://go.microsoft.com/fwlink/?LinkId=717495)
