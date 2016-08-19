---
title: Softwareanforderungen von PAM | Microsoft Identity Manager
description: "Ermitteln der Hardware- und Softwareanforderungen für eine erfolgreiche Bereitstellung von Privileged Access Management"
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 75a748f7035cfb10e833e4fdbfdc208b5245d3ea


---

# Hardware- und Softwareanforderungen

Die Hardwareanforderungen von Privileged Access Management gehen nicht über diejenigen der zugrunde liegenden Softwareplattformen hinaus. Sie müssen lediglich sicherstellen, dass Sie über ausreichend Arbeitsspeicher bzw. Speicherplatz und Netzwerkkonnektivität verfügen.

Dieser Artikel enthält die Mindestanforderungen für eine einfache Bereitstellung. Sie ist nicht dafür vorgesehen, die Leistung, Skalierbarkeit oder hohe Verfügbarkeit zu veranschaulichen, und sie stellt keine empfohlene Bereitstellungstopologie für große Unternehmen oder Produktionsumgebungen dar.

## Installieren von Softwarepaketen

Die folgende Software kann aus dem TechNet Evaluation Center oder MSDN heruntergeladen werden:  
- Microsoft Identity Manager 2016
  - Dienst und Portal: Enthält das Installationsprogramm für den MIM-Dienst und das MIM-Portal für das PAM-Szenario.
  - Add-Ins und Erweiterungen: Enthält das Installationsprogramm für die PowerShell-Cmdlets des Requestors.

Die folgende Software kann von GitHub heruntergeladen werden:  
- PAMSamplePortal: Enthält eine Beispielwebanwendung für die REST-API.

## Erforderliche Software

- Windows Server 2012 R2  
- Windows 8.1 Enterprise oder Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 oder SQL Server 2014  

## Evaluierungssoftware

Wenn Sie keine Lizenzen für Windows, SQL Server oder Windows Server besitzen, können Sie Evaluierungsversionen herunterladen.

### TechNet-Evaluierungscenter

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8,1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft Download Center

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 und vorausgesetzte Software](https://www.microsoft.com/download/details.aspx?id=42039)

## Hardwareanforderungen

Beachten Sie für jede PAM-Komponente die Systemanforderungen der Softwareprodukte.

Für CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) oder früher

Für CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Für PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Für PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) oder [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jul16_HO3-->


