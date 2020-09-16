---
title: Der Microsoft Identity Manager-Connector für Microsoft Graph | Microsoft-Dokumentation
description: Der Microsoft Identity Manager-Connector für Microsoft Graph ermöglicht die Lebenszyklusverwaltung über das AD-Konto des externen Benutzers. In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: c426dff583ca51ca77bcb18fe024bf38698e3530
ms.sourcegitcommit: 3ff309115a0f3de114e3dff4eb3927dd7b01df4d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90570758"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Identity Manager-Connector für Microsoft Graph


## <a name="summary"></a>Zusammenfassung 


Der [Microsoft Identity Manager-Connector für Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495) bietet zusätzliche Integrationsszenarios für Azure AD Premium-Kunden.  Er taucht in der MIM-Oberfläche beim Synchronisieren zusätzlicher Metaverse-Objekte auf, die aus der [Microsoft Graph-API](https://developer.microsoft.com/en-us/graph/) Version 1 und Betaversion abgerufen werden.

## <a name="scenarios-covered"></a>Abgedeckte Szenarios


### <a name="b2b-account-lifecycle-management"></a>Lebenszyklusverwaltung von B2B-Konten


Im Anfangsszenario des Microsoft Identity Manager-Connector für Microsoft Graph hilft dieser als Connector beim Automatisieren der Lebenszyklusverwaltung über das AD DS-Konto für externe Benutzer. In diesem Szenario synchronisiert eine Organisation über Azure AD Connect Mitarbeiter aus AD DS mit Azure AD. Zudem hat die Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen. Das Einladen eines Gasts führt dazu, dass ein externes Benutzerobjekt in dem Azure AD-Verzeichnis dieser Organisation enthalten ist, das sich nicht in den AD DS dieser Organisation befindet. Anschließend möchte die Organisation diesen Gästen über den [Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) oder andere Gatewaymechanismen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren. Der Azure AD-Anwendungsproxy erfordert, dass jeder Benutzer zur Identifikation und Delegierung ein eigenes AD DS-Konto hat.  

Wenn Sie weitere Informationen zum Konfigurieren der MIM-Synchronisierung für die automatische Erstellung und Verwaltung von AD DS-Konten für Gäste erhalten möchten, lesen Sie nach den Anweisungen in diesem Artikel die Informationen im Artikel [Azure AD-B2B-Zusammenarbeit mit MIM 2016 SP1 mit dem Azure-Anwendungsproxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Dieser Artikel veranschaulicht die Synchronisierungsregeln, die für den Connector erforderlich sind.

### <a name="other-identity-management-scenarios"></a>Weitere Szenarios der Identitätsverwaltung


Der Connector kann für bestimmte weitere Szenarios der Identitätsverwaltung verwendet werden. Zu diesen zählen das Erstellen, Lesen, Aktualisieren und Löschen von Benutzer-, Gruppen- und Kontaktobjekten in Azure AD, jenseits der Benutzer- und Gruppensynchronisierung mit Azure AD. Bedenken Sie bei der Auswertung möglicher Szenarios Folgendes: Dieser Connector kann nicht in einem Szenario verwendet werden, das zu einer Datenflussüberlappung, einem tatsächlichen oder einem potenziellen Synchronisierungskonflikt mit einer Azure AD Connect-Bereitstellung führen würde.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) wird für die Integration lokaler Verzeichnisse in Azure AD durch die Synchronisierung von Benutzern und Gruppen aus lokalen Verzeichnissen mit Azure AD als Ansatz empfohlen.  Azure AD Connect bietet viele weitere Synchronisierungsfeatures und ermöglicht Szenarios, wie z.B. das Kennwort- und Geräterückschreiben, die für mit MIM erstellte Objekte nicht ausgeführt werden können. Wenn Daten in AD DS zusammengetragen werden, sollten Sie z.B. sicherstellen, dass diese aus Azure AD Connect ausgeschlossen werden, indem Sie versuchen, diese Objekte wieder mit dem Azure AD-Verzeichnis abzugleichen.  Zudem kann dieser Connector nicht verwendet werden, um Änderungen an Azure AD-Objekten vorzunehmen, die von Azure AD Connect erstellt wurden.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Vorbereiten der Verwendung des Connectors für Microsoft Graph

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorisieren Sie den Connector zum Abrufen oder Verwalten von Objekten in Ihrem Azure AD-Verzeichnis


1.  Für den Connector muss in Azure AD eine Web-App/API-Anwendung erstellt werden, damit dieser mit entsprechenden Berechtigungen autorisiert werden kann, um über Microsoft Graph in Azure AD-Objekten verwendet zu werden.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Abbildung 1. Registrierung einer neuen Anwendung

2.  Öffnen Sie die erstellte Anwendung im Azure-Portal, und speichern Sie die Anwendungs-ID als Client-ID für die spätere Verwendung auf der MA-Konnektivitätsseite:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Abbildung 2. Anwendungs-ID

3.  Generieren Sie neue geheime Clientschlüssel, indem Sie „Alle Einstellungen -\> Schlüssel“ öffnen. Geben Sie eine Schlüsselbeschreibung an, und wählen Sie die gewünschte Dauer. Speichern Sie die Änderungen. Ein geheimer Wert ist nach dem Verlassen der Seite nicht mehr verfügbar.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Abbildung 3. Neuer geheimer Clientschlüssel

4.  Fügen Sie „Microsoft Graph-API“ zur Anwendung hinzu, indem Sie „Erforderliche Berechtigungen“ öffnen.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Abbildung 4. Hinzufügen einer neuen API

Abhängig vom Szenario sollte die folgende Berechtigung zur Anwendung hinzugefügt werden, damit diese die „Microsoft Graph-API“ verwenden darf:

| Vorgang mit Objekt | Erforderliche Berechtigung                                                                  | Berechtigungstyp |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Gruppe importieren          | `Group.Read.All` oder `Group.ReadWrite.All`                                                | Anwendung     |
| Benutzer importieren           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` oder `Directory.ReadWrite.All` | Anwendung     |

Weitere Details zu erforderlichen Berechtigungen finden Sie [hier](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Erteilen Sie der Anwendung die erforderlichen Berechtigungen.


## <a name="installing-the-connector"></a>Installieren des Connectors


6.  Stellen Sie vor dem Installieren des Connectors sicher, dass auf dem Synchronisierungsserver Folgendes festgelegt ist: 

 - Microsoft .NET Framework 4.5.2 oder höher
 - Microsoft Identity Manager 2016 SP1; darüber hinaus ist die Verwendung von Hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) oder höher erforderlich.

7. Der Connector für Microsoft Graph steht neben anderen Connectors für Microsoft Identity Manager 2016 SP1 als Download im [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495) verfügbar.

8.  Starten Sie den MIM-Synchronisierungsdienst neu.
 
## <a name="connector-configuration"></a>Connectorkonfiguration



9.  In der Benutzeroberfläche von Synchronization Service Manager wählen Sie **Connectors** und **Erstellen** aus.
Wählen Sie **Graph (Microsoft)** aus, erstellen Sie einen Connector, und benennen Sie ihn mit einem beschreibenden Namen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. Geben Sie in der Benutzeroberfläche des MIM-Synchronisierungsdiensts die Anwendungs-ID und den generierten geheimen Clientschlüssel an. Alle bei der MIM-Synchronisierung konfigurierten Verwaltungs-Agents sollten in Azure AD über eine eigene Anwendung verfügen, um zu verhindern, dass für dieselbe Anwendung parallel ein Import ausgeführt wird.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Abbildung 5. Konnektivitätsseite

Die Konnektivitätsseite (Abbildung 5) enthält die verwendete Version der Graph-API und den Namen des Mandanten. Die Client-ID und der geheime Clientschlüssel stellen die Anwendungs-ID und den Schlüsselwert der WebAPI-Anwendung dar, die in Azure AD erstellt werden müssen.

11. Nehmen Sie auf der Seite „Globale Parameter“ alle erforderlichen Änderungen vor:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Abbildung 6. Seite „Globale Parameter“

Die Seite „Globale Parameter“ enthält die folgenden Einstellungen:

- DateTime-Format: Format, das für jedes Attribut mit Edm.DateTimeOffset-Typ verwendet wird. Alle Daten werden mithilfe dieses Formats beim Import in eine Zeichenfolge konvertiert. Das festgelegte Format wird für alle Attribute übernommen, die das Datum speichern.

 - HTTP-Timeout (Sekunden): Zeitlimit in Sekunden, das während jedes HTTP-Aufrufs an die WebAPI-Anwendung verwendet wird.

 - Kennwortänderung für erstellten Benutzer beim nächsten Zeichen erzwingen: Diese Option wird für neue Benutzer verwendet, die während des Exports erstellt werden. Wenn die Option aktiviert ist, dann wird die Eigenschaft [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) auf „true“ gesetzt, andernfalls auf „false“.

## <a name="configuring-the-connector-schema-and-operations"></a>Konfigurieren von Connectorschema und -vorgängen


12.   Konfigurieren Sie das Schema.  Der Connector unterstützt die folgende Liste von Objekttypen:

-   Benutzer

    -   Vollständiger Import/Deltaimport

    -   Export (Hinzufügen, Aktualisieren, Löschen)

-   Gruppe

    -   Vollständiger Import/Deltaimport

    -   Export (Hinzufügen, Aktualisieren, Löschen)


Die Liste der unterstützten Attributtypen:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (Zeichenfolge im Connectorbereich)

-   `microsoft.graph.directoryObject` (Verweis im Connectorbereich auf eines der unterstützten Objekte)

-   `microsoft.graph.contact`

Mehrwertige Attribute (Collection) werden für jeden Typ aus der obigen Liste ebenfalls unterstützt.

Der Connector verwendet das Attribut `id` zum Verankern und DN für alle Objekte.  Daher ist keine Umbenennung erforderlich, da die Graph-API nicht zulässt, dass ein Objekt das zugehörige Attribut „id“ ändert.


## <a name="access-token-lifetime"></a>Lebensdauer von Zugriffstoken


Eine Graph-Anwendung erfordert für den Zugriff auf die Graph-API ein Zugriffstoken. Ein Connector fordert für jede Importiteration ein neues Zugriffstoken an (die Importiteration hängt von der Seitengröße ab). Beispiel:

-   Azure AD enthält 10.000 Objekte.

-   Die im Connector konfigurierte Seitengröße ist 5000.

In diesem Fall gibt es während des Imports zwei Iterationen, von denen jedes 5.000 zu synchronisierende Objekte zurückgibt. Es wird also zweimal ein neues Zugriffstoken angefordert.

Während des Exports wird für jedes Objekt, das hinzugefügt, aktualisiert oder gelöscht werden muss, ein neues Zugriffstoken angefordert.

## <a name="troubleshooting"></a>Problembehandlung


**Aktivieren von Protokollen**

Wenn in Graph Probleme auftreten, können Protokolle verwendet werden, um diese zu lokalisieren. So können Ablaufverfolgungen [auf die gleiche Weise aktiviert werden wie für generische Connectors](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Oder einfach durch Hinzufügen des Folgenden zu `miiserver.exe.config` (in Abschnitt `system.diagnostics/sources`):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>Wenn „Diesen Verwaltungs-Agent in einem separaten Prozess ausführen“ aktiviert ist, sollte `dllhost.exe.config` anstelle von `miiserver.exe.config` verwendet werden.

**Fehler durch abgelaufenes Zugriffstoken**

Connector meldet möglicherweise HTTP-Fehler 401 Unauthorized, Meldung „Das Zugriffstoken ist abgelaufen.“:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Abbildung 7. „Das Zugriffstoken ist abgelaufen.“ Fehler

Die Ursache für dieses Problem könnte die Konfiguration der Zugriffstoken-Lebensdauer auf Azure-Seite sein. Standardmäßig verfällt das Zugriffstoken nach 1 Stunde. Um die Ablaufzeit zu verlängern, lesen Sie [diesen Artikel](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Beispiel hierfür mit [Azure AD PowerShell Module Public Preview Release](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"** }}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

## <a name="next-steps"></a>Nächste Schritte

- [Graph-Tester (sehr hilfreich für die Problembehandlung bei HTTP-Aufrufen)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versionsverwaltung, Support und Richtlinien für wesentliche Änderungen für Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Download des Microsoft Identity Manager-Connectors für Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
[Azure AD-Business-to-Business-Kollaboration (B2B) mit Microsoft Identity Manager(MIM) 2016 SP1 mit Azure-Anwendungsproxy]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
