---
title: 'Schritt 5: Installieren/Konfigurieren von PAM'
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
ms.openlocfilehash: c641865548f753a609ccee8dbf12c329bb6a1c9f


---
# <a name="step-5-installingconfiguring-pam"></a>Schritt 5: Installieren/Konfigurieren von PAM

>[!div class="step-by-step"]
[« Schritt 4](sp1-step4-configuring-sharepoint.md)
[Schritt 6 »](sp1-step6-setup-pam-trust.md)

Melden Sie sich auf einem PAM-Server, der mit einer Domäne verknüpft ist, mit dem MIMAdmin-Konto an, andernfalls melden Sie sich als lokaler Administrator an.
1. Führen Sie PowerShell als Administrator aus.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Wählen Sie Menüoption 5 (MIM PAM-Einrichtung)

>[!NOTE]
>Wenn der Computer nicht bereits mit der PRIV-Domäne verknüpft ist, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Nach dem Domänenbeitritt wird der Computer neu gestartet.

Nach dem Neustart des PAM-Servers melden Sie sich wieder mit dem MIMAdmin-Konto am Computer an.

1. Führen Sie PowerShell als Administrator aus.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wählen Sie Menüoption 5 (MIM PAM-Einrichtung)

  Geben Sie bei Aufforderung die Kennwörter für das MIM-Überwachungskonto, MIM-Komponentenkonto, MIM MA-Konto, MIM-Dienstkonto, MIM-Administratorkonto und SharePoint-Konto ein.
  Nach Abschluss der Installation wird der Computer neu gestartet.

>[!div class="step-by-step"]
[« Schritt 4](sp1-step4-configuring-sharepoint.md)
[Schritt 6 »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Nov16_HO2-->


