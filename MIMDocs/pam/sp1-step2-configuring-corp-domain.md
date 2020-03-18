---
title: 'Schritt 2: Konfigurieren der CORP-Domäne'
description: Dieser Artikel beschreibt den zweiten Schritt zur Konfiguration der CORP-Domäne. Hierzu gehört das Ausführen eines Skripts, nachdem die Datei „SIDs.txt“ auf den CORP-DC kopiert wurde.
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
ms.openlocfilehash: 1215e0be4d978e879ebc09ecdd99223fb3667a85
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043833"
---
# <a name="step-2-configuring-the-corp-domain"></a>Schritt 2: Konfigurieren der CORP-Domäne

> [!div class="step-by-step"]
> [« Schritt 1](sp1-step1-configuring-priv-domain.md)
> [Schritt 3 »](sp1-step3-installing-configuring-sql.md)

Sobald die Datei „SIDs.txt“ auf den CORP-DC kopiert wurde **für PRIVOnly-Bereitstellungen nicht erforderlich**:

1. Melden Sie sich als Administrator auf dem CORP-DC an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wählen Sie Menüoption 2 (CORP-Gesamtstrukturkonfiguration)

> [!div class="step-by-step"]
> [« Schritt 1](sp1-step1-configuring-priv-domain.md)
> [Schritt 3 »](sp1-step3-installing-configuring-sql.md)
