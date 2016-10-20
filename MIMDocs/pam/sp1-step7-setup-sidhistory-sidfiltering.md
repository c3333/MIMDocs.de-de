---
title: 'Schritt 7: Einrichten des SID-Verlaufs/der SID-Filterung'
description: "Bereiten Sie die CORP-Domäne mit vorhandenen oder neuen Identitäten vor, die vom Privileged Identity Manager mithilfe von Skripts verwaltet werden sollen."
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 4ad5d755de100ad598c6cebd209296edd361e8c5


---

# Schritt 7: Einrichten des SID-Verlaufs/der SID-Filterung

**Dieser Schritt ist für eine Nur-PRIV-Umgebung nicht erforderlich.** Melden Sie sich mit dem MIMAdmin-Konto auf dem PAM-Server an.

1. Melden Sie sich als Administrator auf dem CORP-DC an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wählen Sie Menüoption 8 (Einrichten des SID-Verlaufs/der SID-Filterung)

Nach erfolgreicher Ausführung werden folgende Meldungen angezeigt:<br/></br>
Für SID-Filterung: <br/></br>
„Die Vertrauensstellung wird dafür festgelegt, keine SIDs zu filtern“ oder „Für diese Vertrauensstellung ist keine SID-Filterung aktiviert“. </br></br>
Für SID-Verlauf: </br></br>
„Der SID-Verlauf für diese Vertrauensstellung wird aktiviert“ oder „Der SID-Verlauf ist für diese Vertrauensstellung bereits aktiviert“.

>[!div class="step-by-step"]
[« Schritt 6](sp1-step6-setup-pam-trust.md)
[Schritt 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Oct16_HO1-->


