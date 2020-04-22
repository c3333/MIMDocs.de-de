---
title: Softwareanforderungen von PAM | Microsoft Docs
description: Ermitteln der Hardware- und Softwareanforderungen für eine erfolgreiche Bereitstellung von Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79f9eeabdd9ac9206c4232217c7cbcdf870c3a3a
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043986"
---
# <a name="hardware-and-software-requirements"></a>Hardware- und Softwareanforderungen

Die Hardwareanforderungen von Privileged Access Management gehen nicht über diejenigen der zugrunde liegenden Softwareplattformen hinaus. Sie müssen lediglich sicherstellen, dass Sie über ausreichend Arbeitsspeicher bzw. Speicherplatz und Netzwerkkonnektivität verfügen.

> [!IMPORTANT]
> Dieser Artikel enthält die Mindestanforderungen für eine einfache Bereitstellung. Er soll nicht die Leistung, Skalierbarkeit oder Hochverfügbarkeit demonstrieren. Der Artikel stellt keine empfohlene Bereitstellungstopologie für große Unternehmen oder Produktionsumgebungen bereit.

## <a name="installing-from-software-packages"></a>Installieren von Softwarepaketen

Die folgende Software kann aus dem TechNet Evaluation Center oder MSDN heruntergeladen werden:

- Microsoft Identity Manager 2016
  - Dienst und Portal: Enthält das Installationsprogramm für den MIM-Dienst und das MIM-Portal für das PAM-Szenario.
  - Add-Ins und Erweiterungen: Enthält das Installationsprogramm für die PowerShell-Cmdlets des Requestors.

Die folgende Software kann von GitHub heruntergeladen werden:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): Enthält eine Beispielwebanwendung für die REST-API.

## <a name="required-software"></a>Erforderliche Software

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 oder SQL Server 2014

## <a name="evaluation-software"></a>Evaluierungssoftware

Wenn Sie keine Lizenzen für Windows, SQL Server oder Windows Server besitzen, können Sie Evaluierungsversionen herunterladen.

### <a name="technet-evaluation-center"></a>TechNet-Evaluierungscenter

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Microsoft Download Center

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 und Systemanforderungen](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Hardwareanforderungen

Beachten Sie für jede PAM-Komponente die Systemanforderungen der Softwareprodukte.

Für CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) oder früher

Für CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Für PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Für PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) oder [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
