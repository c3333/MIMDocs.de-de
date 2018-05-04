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
ms.openlocfilehash: a0e2e280c3678867efc2ae8afa46c04ed38a1e11
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>Der Microsoft Identity Manager-Verwaltungs-Agent für Microsoft Graph (öffentliche Vorschau)
=======================================================================================

<a name="summary"></a>Zusammenfassung 
=======

[Der Microsoft Identity Manager-Verwaltungs-Agent für Microsoft Graph (Vorschau)](http://go.microsoft.com/fwlink/?LinkId=717495) bietet zusätzliche Integrationsszenarien für Azure AD Premium-Kunden.

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integriert lokale Verzeichnisse in Azure AD und stellt sicher, dass Benutzer eine gemeinsame Identität und konsistente Authentifizierung über AD DS, Office 365, Azure und SaaS-Anwendungen haben, die in Azure AD integriert sind, indem Benutzer und Gruppen aus lokalen Verzeichnissen mit Azure AD synchronisiert werden.   Dieser Verwaltungs-Agent kann für spezialisierte Identitäts- und Zugriffsverwaltungsvorgänge über die Benutzer- und Gruppensynchronisierung mit Azure AD hinaus eingesetzt werden.  Dieser Verwaltungs-Agent taucht in der MIM-Oberfläche beim Synchronisieren zusätzlicher Metaverse-Objekte auf, die aus der [Microsoft Graph-API](https://developer.microsoft.com/en-us/graph/) Version 1 und Betaversion abgerufen werden. 

<a name="scenarios-covered"></a>Abgedeckte Szenarien
=================

<a name="b2b-account-lifecycle-management"></a>Lebenszyklusverwaltung von B2B-Konten
--------------------------------

Das Ausgangsszenario in der Vorschau für den Microsoft Identity Manager-Verwaltungs-Agent für Microsoft Graph (Vorschau) ist die Lebenszyklusverwaltung eines externen AD-Benutzerkontos. In diesem Szenario hat eine Organisation Gäste in ihr Azure AD-Verzeichnis eingeladen und möchte diesen Gästen über den [Azure AD-Anwendungsproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) oder andere Gatewaymechanismen Zugriff auf die lokale integrierte Windows-Authentifizierung oder Kerberos-basierte Anwendungen gewähren. Der Azure AD-Anwendungsproxy erfordert, dass jeder Benutzer zur Identifikation und Delegierung ein eigenes AD DS-Konto hat.

Weitere Szenarien werden möglicherweise künftig hinzugefügt und [hier dokumentiert](./microsoft-identity-manager-2016-graph-b2b-scenario.md).

<a name="determining-your-deployment-topology"></a>Festlegen Ihrer Bereitstellungstopologie
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Vorbereiten der Verwendung des Verwaltungs-Agents für Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autorisieren des Verwaltungs-Agents zum Verwalten Ihres Azure AD-Verzeichnisses
----------------------------------------------------

1.  Der Graph-Verwaltungs-Agent setzt voraus, dass die Web-App bzw. API-Anwendung in Azure AD erstellt wurde.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Abbildung 1. Registrierung einer neuen Anwendung

2.  Öffnen Sie die erstellte Anwendung, und verwenden Sie eine Anwendungs-ID wie z.B. die Client-ID auf der Konnektivitätsseite des Verwaltungs-Agents:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Abbildung 2. Anwendungs-ID

2.  Generieren Sie neue geheime Clientschlüssel, indem Sie „Alle Einstellungen -\> Schlüssel“ öffnen. Geben Sie eine Schlüsselbeschreibung an, und wählen Sie die gewünschte Dauer. Speichern Sie die Änderungen. Ein geheimer Wert ist nach dem Verlassen der Seite nicht mehr verfügbar.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Abbildung 3. Neuer geheimer Clientschlüssel

3.  Fügen Sie „Microsoft Graph-API“ zur Anwendung hinzu, indem Sie „Erforderliche Berechtigungen“ öffnen.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Abbildung 4. Hinzufügen einer neuen API

Die folgende Berechtigung sollte zu „Microsoft Graph-API“ hinzugefügt werden:

| Vorgang mit Objekt | Erforderliche Berechtigung                                                                  | Berechtigungstyp |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Gruppe importieren          | Group.Read.All or Group.ReadWrite.All                                                | Anwendung     |
| Benutzer importieren           | User.Read.All or User.ReadWrite.All or Directory.Read.All or Directory.ReadWrite.All | Anwendung     |

Weitere Details zu erforderlichen Berechtigungen finden Sie [hier](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

1.  Erstellen Sie einen Connector mit der Anwendungs-ID und dem generierten geheimen Clientschlüssel. Jeder Verwaltungs-Agent sollte seine eigene Anwendung in AzureAD haben, um zu vermeiden, dass der Import für dieselbe Anwendung parallel ausgeführt wird. Der Graph-Connector unterstützt die folgende Liste von Objekttypen:

-   Benutzer

    -   Vollständiger Import/Deltaimport

    -   Export (Hinzufügen, Aktualisieren, Löschen)

-   Gruppe

    -   Vollständiger Import/Deltaimport

    -   Export (Hinzufügen, Aktualisieren, Löschen)


Die Liste der unterstützten Attributtypen:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (Zeichenfolge im Connectorbereich)

-   microsoft.graph.directoryObject (Verweis im Connectorbereich auf eines der unterstützten Objekte)

-   microsoft.graph.contact

Mehrwertige Attribute (Collection) werden für jeden Typ aus der vorherigen Liste ebenfalls unterstützt.

Graph-Connector verwendet „ID“-Attribute zum Verankern und DN für alle Objekte.

Das Umbenennen wird zurzeit nicht unterstützt, da die Graph-API keine Änderung des Objektes in das „ID“-Attribut für vorhandene Objekte zulässt.

<a name="access-token-lifetime"></a>Lebensdauer von Zugriffstoken
=====================

Eine Graph-Anwendung erfordert für den Zugriff auf die Graph-API ein Zugriffstoken. Ein Connector fordert für jede Importiteration ein neues Zugriffstoken an (die Importiteration hängt von der Seitengröße ab). Beispiel:

-   AzureAD enthält 10000 Objekte.

-   Die im Connector konfigurierte Seitengröße ist 5000.

In diesem Fall gibt es während des Imports zwei Iterationen, von denen jedes 5000 zu synchronisierende Objekte zurückgibt. Es wird also zweimal ein neues Zugriffstoken angefordert.

Beachten Sie, dass während des Exports für jedes Objekt, das hinzugefügt, aktualisiert oder gelöscht werden muss, ein neues Zugriffstoken angefordert wird.

<a name="installing-the-connector"></a>Installieren des Connectors
========================

Bevor Sie den Connector verwenden, stellen Sie sicher, dass sich Folgendes auf dem Synchronisierungsserver befindet: Microsoft .NET Framework 4.5.2 oder höher, Microsoft Identity Manager 2016 SP1 mit erforderlichem Hotfix 4.4.1642.0 oder höher ([KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794)).

Connectors für Microsoft Identity Manager 2016 SP1, der Graph-Connector ist als Download im [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495) verfügbar.

<a name="connector-configuration"></a>Connectorkonfiguration
=======================

Konnektivitätsseite:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Abbildung 5. Seite Konnektivität

Die Konnektivitätsseite (Abbildung 1) enthält die verwendete Graph-API-Version und den Namen des Mandanten. Die Client-ID und der geheime Clientschlüssel stellen die Anwendungs-ID und den Schlüsselwert der WebAPI-Anwendung dar, die in Azure AD erstellt werden müssen.

Seite „Globale Parameter“:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Abbildung 6. Seite „Globale Parameter“

Die Seite „Globale Parameter“ enthält die folgenden Einstellungen:

DateTime-Format: Format, das für jedes Attribut mit Edm.DateTimeOffset-Typ verwendet wird. Alle Daten werden mithilfe dieses Formats beim Import in eine Zeichenfolge konvertiert. Das festgelegte Format wird für alle Attribute übernommen, die das Datum speichern.

HTTP-Timeout (Sekunden): Zeitlimit in Sekunden, das während jedes HTTP-Aufrufs an die WebAPI-Anwendung verwendet wird.

Kennwortänderung für erstellten Benutzer beim nächsten Zeichen erzwingen: Diese Option wird für neue Benutzer verwendet, die während des Exports erstellt werden. Wenn die Option aktiviert ist, dann wird die Eigenschaft [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) auf „true“ gesetzt, andernfalls auf „false“.

<a name="troubleshooting"></a>Problembehandlung
===============

**Aktivieren von Protokollen**

Wenn in Graph Probleme auftreten, können Protokolle verwendet werden, um diese zu lokalisieren. Der Graph-Connector verwendet die gleiche Quelle wie alle generischen Connectors. So können Ablaufverfolgungen [auf die gleiche Weise aktiviert werden wie für generische Connectors (siehe Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Oder einfach, indem Sie Folgendes zu miiserver.exe.config (im Abschnitt system.diagnostics/sources) hinzufügen:

\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

Beachten Sie: Wenn „Diesen Management-Agent in einem separaten Prozess ausführen“ aktiviert ist, sollte „dllhost.exe.config“ anstelle von „miiserver.exe.config“ verwendet werden.

**Fehler durch abgelaufenes Zugriffstoken**

Connector meldet möglicherweise HTTP-Fehler 401 Unauthorized, Meldung „Das Zugriffstoken ist abgelaufen.“:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Abbildung 7. „Das Zugriffstoken ist abgelaufen.“ Fehler

Die Ursache für dieses Problem könnte die Konfiguration der Zugriffstoken-Lebensdauer auf Azure-Seite sein. Standardmäßig verfällt das Zugriffstoken nach 1 Stunde. Um die Ablaufzeit zu verlängern, lesen Sie [diesen Artikel](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes).

Beispiel hierfür mit [Azure AD PowerShell Module Public Preview Release](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>Nächste Schritte
----------
- [Graph-Tester (sehr hilfreich für die Problembehandlung bei HTTP-Aufrufen)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versionsverwaltung, Support und Richtlinien für wesentliche Änderungen für Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Download des Microsoft Identity Manager-Verwaltungs-Agents für Microsoft Graph (Vorschau)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Referenzen zu bestimmten Szenarien
----------------------------------
[MIM B2B End to End Deployment]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md) (MIM-B2B-End-to-End-Bereitstellung)
