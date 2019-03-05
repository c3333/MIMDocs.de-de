---
title: Microsoft Identity Manager – Lizenzierung und Downloads | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Ansätze für die Lizenzierung von Microsoft Identity Manager (MIM) 2016 – mit Zeigern auf die Speicherorte, von denen Sie die Software herunterladen können.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 9e74b7ccf332c1572ce395fad18b8629673c756a
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56954530"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 – Lizenzierung und Downloads

Dieser Artikel beschreibt die Ansätze für die Lizenzierung von Microsoft Identity Manager (MIM) 2016 – mit Zeigern auf die Speicherorte, von denen Sie die Software herunterladen können.

## <a name="licensing-mim-for-your-organization"></a>Lizenzierung von MIM für Ihre Organisation

Microsoft Identity Manager 2016 wird pro Benutzer lizenziert.  Details zur Lizenzierung sind in den Produktbedingungen und zugehörigen Dokumenten enthalten, die Sie auf der [Lizenzbestimmungenseite](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) herunterladen können.

### <a name="licensing-for-azure-ad-premium-customers"></a>Lizenzierung für Azure AD Premium-Kunden

Microsoft Identity Manager 2016 ist in Azure Active Directory Premium (P1 und P2) enthalten, was ein Teil von Enterprise Mobility + Security ist.

Azure AD Premium ist über ein [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), das [Microsoft Open License Programm](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), und das [Cloud Solution Provider](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409)-Programm verfügbar. Azure- und Office 365-Abonnenten können Azure Active Directory Premium P1 und P2 auch online erwerben.  Weitere Informationen finden Sie in der [Preisübersicht für Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>MIM-Clientzugriffslizenzen

Wenn Sie keine Azure Active Directory Premium-Abonnements für Ihre Benutzer haben und weitere MIM-Funktionen außer der Synchronisierung verwenden, ist eine [(Clientzugriffslizenz (Client Access License, CAL))](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) für jeden Benutzer erforderlich, dessen Identität in MIM verwaltet wird. Wenn Sie wünschen, dass externe Benutzer – z.B. Geschäftspartner, externe Vertragspartner oder Kunden – auf MIM zugreifen können, können Sie CALs für die einzelnen externen Benutzern oder externen Connectors (EC) erwerben. Microsoft Identity Manager 2016-Clientzugriffslizenzen sind nicht erforderlich für Benutzer, deren Identität nur im Microsoft Identity Manager-Synchronisierungsdienst vorhanden ist und nicht in einer anderen MIM-Komponente verwaltet wird.

### <a name="licenses-for-platform-components"></a>Lizenzen für Plattformkomponenten

Zur Verwendung der Serversoftware von Microsoft Identity Manager 2016 als Add-On für Windows Server ist eine Windows Server-Lizenz erforderlich. Eine MIM-Bereitstellung erfordert auch eine SQL Server-Installation.  Windows Server- und SQL Server-Lizenzen sind nicht in MIM enthalten.

## <a name="obtaining-mim-software"></a>Abrufen von MIM-Software

Stellen Sie vor dem Starten einer neuen Installation von MIM oder einem Upgrade von einer früheren Version sicher, dass Sie über die neuesten Versionen verfügen.

Wenn Sie eine neue Installation beginnen, müssen Sie die Installationsdateien für jede MIM-Komponente herunterladen, die für Ihr Szenario relevant ist. Laden Sie dann aus dem Download Center alle Updates für diese Dateien herunter, und dann ggf. zusätzliche Komponenten, die separate Downloads sind.


| Szenario | Komponente | Erforderlich für welches Szenario? | DVD-ISO-Ordnername | Kommentare |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synchronisierung| Synchronisierungsdienst (einschließlich Connector zu AD) | Ja | `Synchronization Service` | |
| Synchronisierung | PCNS | Nein | `Password Change Notification Service` |  Zur Installation auf Domänencontrollern |
| Synchronisierung | Connectors für LDAP, SQL, Webdienste, PowerShell, Lotus Domino, Graph | Nein | N/V | Über das Download Center verteilt |
| Privileged Access Management (Schützen von Windows und Microsoft Azure Active Directory mit Privileged Access Management) | MIM-Dienst | Ja | `Service and Portal` | |
| Self-Service | MIM-Dienst, MIM-Portal | Ja | `Service and Portal` | |
| Self-Service | Add-Ins und -Erweiterungen | Nein | `Add-ins and extensions` | Zur Installation auf Endbenutzer-PCs |
| Self-Service | SCSM-Berichterstellung | Nein | `Data Warehouse Support Scripts` | |
| Self-Service | Hybridberichterstellungs-Agent | Nein | N/V | Über das Download Center verteilt |
| Self-Service | Sprachpakete | Nein | `LANGUAGE Packs` | |
| Zertifikatverwaltung | CM | Ja | `Certificate Management` | |
| Zertifikatverwaltung | CM-Sammelclient | Nein | `CM Bulk Client` | |
| Zertifikatverwaltung | CM-Client | Nein | `CM Client`  | |
| Zertifikatverwaltung | CM-App für Windows | Nein | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Abrufen von Windows Installer-Paketen

Für eine neue Installation laden die meisten Organisationen die MIM-Installationspakete aus dem [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx) herunter. 


Die DVD-ISO-Datei enthält einen Ordner für jede MIM-Komponente: Synchronisierungsdienst, Dienst und Portal usw. Wenn Sie die Software auf einem anderen Computer installieren möchten als dem, von dem aus Sie sie heruntergeladen haben, stellen Sie sicher, dass Sie entweder die gesamte ISO-Datei oder den Ordner für die Komponente kopieren: kopieren Sie nicht einfach nur eine MSI-Datei aus einem Ordner, ohne den Rest der Dateien und Unterordner zu kopieren.

Wenn Sie keinen Zugriff auf das Volume Licensing Service Center haben, können Sie als Kunde mit einem entsprechenden Entwicklerabonnement auch MIM 2016 SP1 als ISO-Datei von [Visual Studio My Benefits Downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=) herunterladen.  Suchen Sie nach „Microsoft Identity Manager 2016 mit Service Pack 1.“  

Wenn Sie keinen Zugriff auf das Volume Licensing Service Center haben und lediglich die MIM-Software für einen begrenzten Zeitraum testen möchten, können Sie eine [Evaluierungsversion von MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244) herunterladen. Diese Software ist nicht für Produktionszwecke vorgesehen und kann 180 Tage nach der erstmaligen Installation nicht mehr ausgeführt und nicht aktualisiert werden. Die Installation der Evaluierungsversion erfordert Windows Server 2008 R2, Windows Server 2012 oder Windows Server 2012 R2.  Wenn Sie in MIM einsteigen und die Technologie erlernen, bedenken Sie, dass alle MIM-Szenarien eine Active Directory-Domäne, einen Windows Server und SQL Server erfordern. Wenn Windows Server oder SQL Server noch nicht vorhanden sind, können Sie die [Bereitstellung eines virtuellen Computers mit SQL Server 2016 und Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/) ausprobieren.

### <a name="obtaining-updates"></a>Abrufen von Updates

Nach der Installation von MIM aus der MSI-Datei sollten Sie als Nächstes die erforderlichen Hotfixes installieren.

Überprüfen Sie den [Identity Manager-Versionsfreigabeverlauf](./reference/version-history.md) auf das neueste Updaterelease, das über einen Link zur Downloadsite für die Installerpatchdateien verfügt.

Um zu ermitteln, welche Updatedateien erforderlich sind, sind in dieser Tabelle die Komponenten und der Namen der entsprechenden Patchdatei (MSP) in einem Update aufgelistet.

| Szenario | Komponente | DVD-ISO-Ordnername | Entsprechender Updatepatchdatei-Name |
|----------|-----------|-   |-------------------|----------|--------------|
|Synchronisierung| Synchronisierungsdienst | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Self-Service | MIM-Dienst, MIM-Portal | `Service and Portal` | `FIMService_x64*msp` |
| Self-Service | Add-Ins und -Erweiterungen | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Self-Service | Sprachpakete | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Zugriffsverwaltung (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Zertifikatverwaltung | CM |  `Certificate Management` | `FIMCM*.msp` |
| Zertifikatverwaltung | CM-Sammelclient |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Zertifikatverwaltung | CM-Client | CM-Client |`FIMCMClient*msp` |

Lesen Sie vor der Installation der MSP-Datei unbedingt alle Versionshinweise zu dem Update.

Updates für [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) werden nicht als MSP-Dateien, nur als MSI-Installer verteilt.

### <a name="additional-downloads"></a>Weitere Downloads

Die folgenden Downloads sind unter Umständen auch relevant:

- [MIM-Hybridberichterstellungs-Agent](https://www.microsoft.com/download/details.aspx?id=55112)

- [Generischer LDAP-Connector, generischer SQL-Connector, Graph-Connector, Lotus Domino-Connector, PowerShell-Connector, Connector für Webdienste](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Connector für SharePoint User Profile Store](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Wenn Sie noch keine Active Directory-Domäne besitzen und ein PAM-Szenario für Experimente einrichten, lesen Sie [Bereitstellungsskripts für MIM2016 SP1 PAM](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie unter [Neuigkeiten und Updates zu Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) mehr über Szenarios, die in MIM 2016 bereitgestellt werden.
- Lesen Sie das [Kapazitätsplanungshandbuch](capacity-planning-guide.md).
- Stellen Sie MIM für ein [Synchronisierungsszenario](microsoft-identity-manager-deploy.md) oder das [Privileged Access Management-Szenario](./pam/privileged-identity-management-for-active-directory-domain-services.md) bereit.

