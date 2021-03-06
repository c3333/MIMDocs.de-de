---
title: Übersicht über die PAM-Umgebung | Microsoft Docs
description: Ermitteln der erforderlichen Anzahl und der erforderlichen Konfiguration virtueller Computer für eine erfolgreiche Bereitstellung von Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c758bd924c6f7272a44567c2e9dc919215cdd273
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044003"
---
# <a name="environment-overview"></a>Übersicht über die Umgebung

Privileged Access Management (PAM) arbeitet mit virtuellen Computern (VMs) mit separaten Laufwerken, die in einem gemeinsamen Netzwerk miteinander verbunden sind. Diese virtuellen Computer können von Windows 8.1, Windows Server 2012 R2 oder anderen Betriebssystemplattformen gehostet werden.

![PAM-Server: Beziehungen und unterstützte Plattformen – Diagramm](media/pam-test-lab-architecture.png)

Sie benötigen mindestens drei virtuelle Computer.  Wenn Sie noch nicht über eine AD-Domäne verfügen, die über PAM verwaltet werden kann, benötigen Sie einen zusätzlichen virtuellen Computer als CORP-Domänencontroller.  Wenn Sie die PRIV-Software für hohe Verfügbarkeit konfigurieren möchten, benötigen Sie zwei zusätzliche virtuelle Computer.

Auf den Laufwerken, auf denen die Datenträgerimages der virtuellen Computer gespeichert werden, müssen mindestens 120 GB Speicherplatz verfügbar sein.  Stellen Sie bei einer Bereitstellung für Hochverfügbarkeit sicher, dass das Datenträgersubsystem die Anforderungen für freigegebenen SQL-Speicher erfüllt.  Der freigegebene Speicher kann in Form von Windows Server Failover Clustering-Clusterdatenträgern, Datenträgern auf einem Storage Area Network (SAN) oder Dateifreigaben auf einem SMB-Server vorliegen.

> [!IMPORTANT]
> Der geschützten Umgebung muss Speicher zugewiesen sein. Das Freigeben von Speicher für andere Workloads außerhalb der geschützten Umgebung wird nicht empfohlen, da die Integrität der geschützten Umgebung gefährdet werden könnte.

## <a name="next-steps"></a>Nächste Schritte

- [Privileged Access Management für Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) ist eine Übersicht über PAM und dessen Funktionsweise.
- [Verstehen der Komponenten von PAM](principles-of-operation.md) bietet eine Übersicht über die verschiedenen PAM-Komponenten.
