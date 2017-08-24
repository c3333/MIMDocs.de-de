---
title: "Schritt 1: Konfigurieren der PRIV-Domäne"
description: "Bereiten Sie die CORP-Domäne mit vorhandenen oder neuen Identitäten vor, die vom Privileged Identity Manager mithilfe von Skripts verwaltet werden sollen."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 40822bb7702cf3d7ac23ecd6e98ac392f2d3a480
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2017
---
# <a name="step-1-configuring-the-priv-domain"></a>Schritt 1: Konfigurieren der PRIV-Domäne

>[!div class="step-by-step"]
[Schritt 2 »](sp1-step2-configuring-corp-domain.md)

1. Melden Sie sich als Administrator auf dem PRIV-DC an.
  * Wenn dies nur eine Nur-PRIV-Umgebung ist, melden Sie sich auf dem CORP-DC an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wählen Sie Menüoption 1 (PRIV-Gesamtstrukturkonfiguration)


Die erforderlichen Dienstkonten für die Verwaltung von SQL/SharePoint und MIM werden automatisch erstellt, sofern nicht bereits in der Domäne vorhanden sind. Während der Ausführung des Skripts werden Sie aufgefordert, die Kennwörter für die Erstellung dieser Dienstkonten einzugeben.
Wenn sich die PRIV-Domäne auf Windows Server 2016 befindet und die Funktionsebene auf „Server 2016 Technical Preview 5“ festgelegt ist, werden Sie vom Skript zum Aktivieren der optionalen Active Directory-Funktion „Privileged Access Management “ aufgefordert. Klicken Sie auf „Ja“, um den Vorgang fortzusetzen.
Für Funktionsebenen unter Windows Server 2016 schließen Sie die Warnung, dass keine zusätzliche Konfiguration ausgeführt wird. Wenn der Administrator die Funktionsebenen auf „Windows Server 2016“ erhöht, müssen Sie „PAMDeployment.ps1“ und die PAM-Gesamtstrukturkonfiguration erneut ausführen.

>[!NOTE]
>Die folgenden Schritte sind für PRIVOnly-Konfigurationen nicht erforderlich.

Kopieren Sie die Datei „SIDs.txt“, die in „$env:SYSTEMDRIVE\PAM“ erzeugt wird, in den entsprechenden Ordner auf dem CORP-DC. Diese wird vom CORP-DC benötigt, um Berechtigungen für PRIV-Benutzer zum Lesen der CORP-Benutzereigenschaften einzurichten.
Nach Abschluss des Skripts werden Sie zum Neustart des Computers aufgefordert, damit die Änderungen wirksam werden.

>[!div class="step-by-step"]
[Schritt 2 »](sp1-step2-configuring-corp-domain.md)
