---
title: 'Schritt 5: Installieren/Konfigurieren von PAM'
description: "Dies ist Schritt 5 der Konfiguration des Privileged Identity Managers mithilfe von Skripts. Es werden die Bereitstellungsschritte auf dem PAM-Server erläutert."
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
ms.openlocfilehash: 862f62ab9bac87bcee31c35e249db34740e9fb14
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# Schritt 5: Installieren/Konfigurieren von PAM
<a id="step-5-installingconfiguring-pam" class="xliff"></a>

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
