---
title: 'Schritt 6: Einrichten der PAM-Vertrauensstellung'
description: "Bereiten Sie die CORP-Domäne mit vorhandenen oder neuen Identitäten vor, die vom Privileged Identity Manager mithilfe von Skripts verwaltet werden sollen."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 5bcf4f4ef201236746ec1bf75c1c8900841a6c79


---

# <a name="step-6-set-up-the-pam-trust"></a>Schritt 6: Einrichten der PAM-Vertrauensstellung

>[!div class="step-by-step"]
[« Schritt 5](sp1-step5-configuring-pam.md)
[Schritt 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Dieser Schritt ist für eine Nur-PRIV-Umgebung nicht erforderlich.** Melden Sie sich mit dem MIMAdmin-Konto auf dem PAM-Server an.

1. Melden Sie sich mit dem MIMAdmin-Konto auf dem PAM-Server an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wählen Sie Menüoption 6 (Einrichten der PAM-Vertrauensstellung)

  Geben Sie die Anmeldeinformationen für das CORP-Administratorkonto ein, wenn Sie dazu aufgefordert werden. Nachdem die Anmeldeinformationen angegeben wurden, wird die Vertrauensstellung eingerichtet, und die Konfiguration ist abgeschlossen.

>[!div class="step-by-step"]
[« Schritt 5](sp1-step5-configuring-pam.md)
[Schritt 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Nov16_HO2-->


