---
title: 'Schritt 3: Konfigurieren von SQL'
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
ms.openlocfilehash: 375a34e5255c90559fc0ffb3a80fc7c92ebd27a2


---
# <a name="step-3-configuring-sql"></a>Schritt 3: Konfigurieren von SQL

>[!div class="step-by-step"]
[« Schritt 2](sp1-step2-configuring-corp-domain.md)
[Schritt 4 »](sp1-step4-configuring-sharepoint.md)

Bevor Sie mit den folgenden Schritten fortfahren, stellen Sie sicher, dass Sie SQL Server 2012 SP1 oder höher bzw. SQL Server 2014 verwenden. Melden Sie sich auf Servern, die mit einer Domäne verknüpft sind, mit dem MIMAdmin-Konto an. Andernfalls melden Sie sich als lokaler Administrator an.
1. Führen Sie PowerShell als Administrator aus.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wählen Sie Menüoption 3 (SQL Server-Setup)

  Wenn der Server noch nicht mit der PRIV-Domäne verknüpft ist, werden Sie aufgefordert, Anmeldeinformationen einzugeben und den Server mit der Domäne zu verknüpfen.
  Nach dem Domänenbeitritt wird der Computer neu gestartet. Melden Sie sich nach dem Neustart mit dem MIMAdmin-Konto auf dem Server an.

1. Führen Sie PowerShell als Administrator aus.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wählen Sie Menüoption 3 (SQL Server-Setup)

Geben Sie bei Aufforderung das Kennwort für das MIMAdmin-Dienstkonto ein und setzen Sie die Installation fort. Nachdem der Vorgang abgeschlossen ist, fahren Sie mit Schritt 4 fort.

>[!div class="step-by-step"]
[« Schritt 2](sp1-step2-configuring-corp-domain.md)
[Schritt 4 »](sp1-step4-configuring-sharepoint.md)



<!--HONumber=Nov16_HO2-->


