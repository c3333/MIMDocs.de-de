---
title: "Schritt 2: Konfigurieren der CORP-Domäne"
description: "Dieser Artikel beschreibt den zweiten Schritt zur Konfiguration der CORP-Domäne. Hierzu gehört das Ausführen eines Skripts, nachdem die Datei „SIDs.txt“ auf den CORP-DC kopiert wurde."
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
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.lasthandoff: 01/10/2017


---

# <a name="step-2-configuring-the-corp-domain"></a>Schritt 2: Konfigurieren der CORP-Domäne

>[!div class="step-by-step"]
[« Schritt 1](sp1-step1-configuring-priv-domain.md)
[Schritt 3 »](sp1-step3-installing-configuring-sql.md)

Sobald die Datei „SIDs.txt“ auf den CORP-DC kopiert wurde **für PRIVOnly-Bereitstellungen nicht erforderlich**:

1. Melden Sie sich als Administrator auf dem CORP-DC an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wählen Sie Menüoption 2 (CORP-Gesamtstrukturkonfiguration)

>[!div class="step-by-step"]
[« Schritt 1](sp1-step1-configuring-priv-domain.md)
[Schritt 3 »](sp1-step3-installing-configuring-sql.md)

