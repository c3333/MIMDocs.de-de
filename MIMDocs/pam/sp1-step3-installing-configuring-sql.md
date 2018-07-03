---
title: 'Schritt 3: Konfigurieren von SQL'
description: Dieser Artikel ist Schritt 3 in der Reihe von Artikeln über die Konfiguration des Privileged Identity Managers mithilfe von Skripts und erläutert die Schritte zur Konfiguration von SQL Server.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1196d16e03eed13056877584626e3ea7ab4739d6
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289246"
---
# <a name="step-3-configuring-sql"></a>Schritt 3: Konfigurieren von SQL

> [!div class="step-by-step"]
> [« Schritt 2](sp1-step2-configuring-corp-domain.md)
> [Schritt 4 »](sp1-step4-configuring-sharepoint.md)

Bevor Sie mit den folgenden Schritten fortfahren, stellen Sie sicher, dass Sie SQL Server 2012 SP1 oder höher bzw. SQL Server 2014 verwenden. Melden Sie sich auf Servern, die mit einer Domäne verknüpft sind, mit dem MIMAdmin-Konto an. Andernfalls melden Sie sich als lokaler Administrator an.
1. Führen Sie PowerShell als Administrator aus.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wählen Sie Menüoption 3 (SQL Server-Setup)

   Wenn der Server noch nicht mit der PRIV-Domäne verknüpft ist, werden Sie aufgefordert, Anmeldeinformationen einzugeben und den Server mit der Domäne zu verknüpfen.
   Nach dem Domänenbeitritt wird der Computer neu gestartet. Melden Sie sich nach dem Neustart mit dem MIMAdmin-Konto auf dem Server an.

5. Führen Sie PowerShell als Administrator aus.
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Wählen Sie Menüoption 3 (SQL Server-Setup)

Geben Sie bei Aufforderung das Kennwort für das MIMAdmin-Dienstkonto ein und setzen Sie die Installation fort. Nachdem der Vorgang abgeschlossen ist, fahren Sie mit Schritt 4 fort.

> [!div class="step-by-step"]
> [« Schritt 2](sp1-step2-configuring-corp-domain.md)
> [Schritt 4 »](sp1-step4-configuring-sharepoint.md)
