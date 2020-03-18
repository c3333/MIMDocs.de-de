---
title: 'Schritt 5: Installieren/Konfigurieren von PAM'
description: Dies ist Schritt 5 der Konfiguration des Privileged Identity Managers mithilfe von Skripts. Es werden die Bereitstellungsschritte auf dem PAM-Server erläutert.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 58a70336af4f79d87d6175aa99dc79fc81aa62dd
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043782"
---
# <a name="step-5-installingconfiguring-pam"></a>Schritt 5: Installieren/Konfigurieren von PAM

> [!div class="step-by-step"]
> [« Schritt 4](sp1-step4-configuring-sharepoint.md)
> [Schritt 6 »](sp1-step6-setup-pam-trust.md)

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

> [!div class="step-by-step"]
> [« Schritt 4](sp1-step4-configuring-sharepoint.md)
> [Schritt 6 »](sp1-step6-setup-pam-trust.md)
