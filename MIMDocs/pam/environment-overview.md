---
title: "Übersicht über die PAM-Umgebung | Microsoft Docs"
description: "Ermitteln der erforderlichen Anzahl und der erforderlichen Konfiguration virtueller Computer für eine erfolgreiche Bereitstellung von Privileged Access Management"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.lasthandoff: 05/02/2017


---

# <a name="environment-overview"></a>Übersicht über die Umgebung

Privileged Access Management (PAM) arbeitet mit virtuellen Computern (VMs) mit separaten Laufwerken, die in einem gemeinsamen Netzwerk miteinander verbunden sind. Diese virtuellen Computer können von Windows 8.1, Windows Server 2012 R2 oder anderen Betriebssystemplattformen gehostet werden.

![PAM-Server: Beziehungen und unterstützte Plattformen – Diagramm](media/pam-test-lab-architecture.png)

Sie benötigen mindestens drei virtuelle Computer.  Wenn Sie nicht bereits eine AD-Domäne haben, die PAM verwalten kann, benötigen Sie einen zusätzlichen virtuellen Computer als CORP-Domänencontroller.  Wenn Sie die PRIV-Software für hohe Verfügbarkeit konfigurieren möchten, benötigen Sie außerdem zwei zusätzliche virtuelle Computer.

Die Laufwerke, auf denen die Datenträgerimages der virtuellen Computer gespeichert werden, müssen mindestens 120 GB freien Speicherplatz haben, um alle virtuellen Computer aufzunehmen.  Stellen Sie bei einer Bereitstellung für hohe Verfügbarkeit sicher, dass das Datenträgersubsystem die Anforderungen für freigegebenen SQL-Speicher erfüllt.  Der freigegebene Speicher kann in Form von Windows Server Failover Clustering-Clusterdatenträgern, Datenträgern auf einem Storage Area Network (SAN) oder Dateifreigaben auf einem SMB-Server vorliegen. Beachten Sie, dass diese der geschützten Umgebung zugewiesen werden müssen. Freigeben von Speicher für andere Workloads außerhalb der geschützten Umgebung wird nicht empfohlen, da die Integrität der geschützten Umgebung gefährdet werden könnte.

> [!NOTE]
> Diese MIM-CTP-Version (Customer Technical Preview, technische Vorschau für Kunden) ist nicht mit den Datenbank- oder Verzeichnisinhalten der vorherigen CTP-Version kompatibel. Wenn Sie zuvor bereits MIM für PAM oder andere Szenarien ausgewertet haben, sichern und archivieren Sie die für diesen Test verwendeten virtuellen Computer, und starten Sie die Bereitstellung mit neuen Images von virtuellen Computern, die nicht bereits für MIM-Szenarien verwendet wurden.

