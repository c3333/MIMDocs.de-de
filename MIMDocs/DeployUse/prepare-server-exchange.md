---
# required metadata

title: Einrichten eines Identitätsverwaltungsservers&#58; Exchange | Microsoft Identity Manager
description: Sie können optional Exchange Server zum Aktivieren von MIM 2016 bereitstellen, damit MIM E-Mails versenden und Postfächer einrichten kann. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 34a8c16e-3bed-4e16-939b-b9fe17dd834b

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Einrichten eines Identitätsverwaltungsservers: Exchange

>[!div class="step-by-step"]
[« SharePoint](prepare-server-sharepoint.md)
[MIM Synchronization Service »](install-mim-sync.md)

> [!NOTE]
> In allen folgenden Beispielen stellt **mimservername** den Namen Ihres Domänencontrollers dar, während **contoso** Ihren Domänennamen und **Pass@word1** ein Beispielkennwort darstellen.

## Bereitstellen von Microsoft Exchange Server
Wenn Sie MIM zum Senden und Empfangen von E-Mails oder Bereitstellen von Postfächern konfigurieren möchten, ist es notwendig, dass Exchange in der Umgebung vorhanden ist. Wenn Exchange noch nicht bereitgestellt wurde, können Sie zu Evaluierungszwecken eine Testversion installieren.

1. Laden Sie Microsoft Office 2010 Filter Packs, Version 2.0, und Microsoft Office 2010 Filter Packs, Version 2.0 SP1, herunter, und installieren Sie diese anschließend.

    - [MS Office10 FP2.0](http://www.microsoft.com/en-us/download/details.aspx?id=17062)

    - [MS Office10 FP2.0 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=26604)

2. Herunterladen und Installieren von [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-Bit](http://www.microsoft.com/en-us/download/details.aspx?id=34992)

3. Herunterladen und Installieren von [MS Exchange Server 2013 180-Tage-Testversion](http://www.microsoft.com/en-us/evalcenter/evaluate-exchange-server-2013)

>[!div class="step-by-step"]  
[« SharePoint](prepare-server-sharepoint.md)
[MIM Synchronization Service »](install-mim-sync.md)


<!--HONumber=Apr16_HO2-->

