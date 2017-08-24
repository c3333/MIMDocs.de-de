---
title: 'Schritt 6: Einrichten der PAM-Vertrauensstellung'
description: "Schritt 6 der PAM-Konfiguration mithilfe von Skripts. In diesem Abschnitt wird die Einrichtung der erforderlichen Vertrauensstellung zwischen CORP- und PRIV-Domänen erläutert."
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
ms.openlocfilehash: a24b2a5a196e633379b696ee82e9428dd67454ca
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2017
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
