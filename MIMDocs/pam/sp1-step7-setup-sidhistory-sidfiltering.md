---
title: 'Schritt 7: Einrichten des SID-Verlaufs/der SID-Filterung'
description: Dies ist Schritt 7 der Konfiguration des Privileged Identity Managers mithilfe von Skripts. In diesem Schritt wird die Einrichtung des SID-Verlaufs bzw. der SID-Filterung erläutert.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: f85dd4eff32d5207948ec332bf2e9850b14a86fe
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379327"
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>Schritt 7: Einrichten des SID-Verlaufs/der SID-Filterung

> [!div class="step-by-step"]
> [« Schritt 6](sp1-step6-setup-pam-trust.md)
> [Schritt 8 »](sp1-step8-pam-deployment-verification.md)

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

> [!div class="step-by-step"]
> [« Schritt 6](sp1-step6-setup-pam-trust.md)
> [Schritt 8 »](sp1-step8-pam-deployment-verification.md)
