---
title: Konfigurieren von PAM mithilfe von Skripts
description: "Dieser Artikel ist Teil der Reihe über die PAM-Konfiguration mithilfe von Skripts. Darin werden die Änderungen an der XML-Datei erläutert, die von den PAM-Bereitstellungsskripts verwendet wird."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: bd73f43a096d58e1f7250e28b59e33f4411e88a3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# Konfigurieren von PAM mithilfe von Skripts
<a id="configure-pam-using-scripts" class="xliff"></a>

Wenn Sie SQL und SharePoint auf unterschiedlichen Servern installieren, müssen beide Anwendungen mithilfe der unten stehenden Anleitung konfiguriert werden. Wenn SQL, SharePoint und die PAM-Komponenten auf demselben Computer installiert werden, müssen auf diesem Computer die folgenden Schritte ausgeführt werden.

Bei den folgenden Schritten wird davon ausgegangen, dass bereits eine PRIV-Domäne eingerichtet ist. Anweisungen zum Konfigurieren einer PRIV-Domäne finden Sie im Nachtrag am Ende des Dokuments.

Schritte:

1. Extrahieren Sie die komprimierte Datei „PAMDeploymentScripts.zip“ auf allen Computern im Ordner „%SYSTEMDRIVE%\PAM“.
2. Öffnen Sie auf einem Computer die Datei **PAMDeploymentConfig.xml**, und aktualisieren Sie die Details mithilfe des folgenden Diagramms oder der Anleitung in der XML-Datei selbst. Wenn die CORP- und PRIV-Gesamtstrukturen bereits eingerichtet sind, müssen Sie nur den **DNS-Namen** und den **NetBIOS-Namen** aktualisieren.
3. Aktualisieren Sie im Abschnitt „Rollen“ das **Dienstkonto**, die **Computerdetails**, und den **Speicherort der Binärdateien der Installation** für SQL, SharePoint und MIM-Rollen.
    1. Der MIM-Speicherort für Binärdateien muss auf den Ordner „Dienst und Portal“ verweisen. Der Client-Speicherort für Binärdateien muss auf das Verzeichnis mit der Datei „Add-ins and Extensions.msi“ verweisen.

4. Wenn es sich um eine PRIVOnly-Umgebung handelt, muss das PRIVOnly-Tag auf „true“ festgelegt werden.
    1. Aktualisieren Sie für PRIVOnly Umgebungen den **DNS-Namen** und den **NetBIOS-Namen** der PRIV-Domäne so, dass sie mit der CORP-Domäne übereinstimmen. Stellen Sie sicher, dass die Suffixe für die Computer, auf denen SQL, SharePoint und MIM installiert werden, korrekt sind, da in der Standardvorlagedatei von einer CORP- und PRIV-Konfiguration ausgegangen wird.
    2. Klicken Sie hier, um weitere Informationen zu PRIVOnly-Umgebungen anzuzeigen.

5. Kopieren Sie die gleiche PAMDeploymentConfig.xml-Datei auf allen Computern, CORP-DCs, PRIV-DCs, PAM-Servern, SQL-Servern und SharePoint-Servern in den Ordner „%SYSTEMDRIVE%\PAM“.


## Arbeitsblatt für die Bereitstellung
<a id="deployment-worksheet" class="xliff"></a>

Bevor Sie fortfahren aktualisieren Sie die Datei „PAMDeploymentConfig.xml“ und legen die aktualisierte Kopie auf allen Computern ab.

### Setup
<a id="setup" class="xliff"></a>

|Computer   | Ausführung als   |Befehle   |
|---|---|---|
|  PRIV-DC |PRIV-Domänenadministrator   | .\PAMDeployment.ps1; Wählen Sie Menüoption 1 (PRIV-Gesamtstrukturkonfiguration)   |
|   |   |  Der oben genannte Schritt generiert die Datei „SIDs.txt“. Diese Datei muss auf dem CORP-DC in „$envDrive:PAM“ kopiert werden, bevor der nächste Schritt ausgeführt wird. |
| CORP-DC  |CORP-Domänenadministrator   | .\PAMDeployment.ps1; Wählen Sie Menüoption 2 (CORP-Gesamtstrukturkonfiguration)   |
| PAM-Server (oder SQL-Server)   |CORP-Domänenadministrator   |  .\PAMDeployment.ps1; Wählen Sie Menüoption 2 (CORP-Gesamtstrukturkonfiguration)  |
|  PAM-Server |  Lokaler Administrator (MIM-Administrator nach dem Domänenbeitritt) |  .\PAMDeployment.ps1; Wählen Sie Menüoption 4 (SharePoint-Setup)  |
| PAM-Server  | Lokaler Administrator (MIM-Administrator nach dem Domänenbeitritt)  | .\PAMDeployment.ps1; Wählen Sie Menüoption 5 (MIM PAM-Einrichtung)   |
|  PAM-Server |MIM-Administrator   | .\PAMDeployment.ps1; Wählen Sie Menüoption 6 (Einrichten der PAM-Vertrauensstellung) .\PAMDeployment.ps1; Wählen Sie Menüoption 6 (Einrichten der PAM-Vertrauensstellung) |

### Validation
<a id="validation" class="xliff"></a>

|  Computer | Ausführung als   | Befehle   |
|---|---|---|
| CORP-Client  | CORP-Benutzer (lokaler Administrator)  |   .\PAMDeployment.ps1; Wählen Sie Menüoption 7 (MIM PAM-Clienteinrichtung)  |
| CORP-DC  | CORP-Domänenadministrator   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAM-Server   | MIM-Administrator  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORP-Client  | CORP-Benutzer (lokaler Administrator)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORP-Client | Benutzer: <PRIV>\PRIV.pamRequestor und bei PRIVOnly : <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


>[!div class="step-by-step"]
[Start »](sp1-step1-configuring-priv-domain.md)
