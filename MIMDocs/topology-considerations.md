---
title: Topologieleitfaden für die Bereitstellung | Microsoft Docs
description: Grundlegendes zu den Komponenten von MIM 2016 und Vorschläge, wie Sie diese in Ihrer Umgebung bereitstellen können.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 42562e92b3fe0daa63110d33d8952a3a1fc3de17
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358079"
---
# <a name="topology-considerations"></a>Überlegungen zur Topologie
Sie können MIM-Komponenten (Microsoft Identity Manager) auf demselben Server oder auf mehreren Servern in mehreren Konfigurationen bereitstellen. Die für die Bereitstellung ausgewählte Topologie hat Auswirkungen auf die Leistung, die Sie in MIM erreichen können. In diesem Artikel werden mehrere Bereitstellungstopologien vorgestellt, die Sie implementieren können.


> [!NOTE]
> Diese Optionen gelten für Bereitstellungen, die ausschließlich MIM Sync, MIM-Dienst und MIM-Portal zur Identitätsverwaltung verwenden.  Bereitstellungen mithilfe von MIM CM, MIM BHOLD-Suite oder für privilegierte Zugriffsverwaltung haben andere Bereitstellungsoptionen.


## <a name="mim-components"></a>MIM-Komponenten
Beim Entwerfen der Bereitstellungstopologie sollten Sie wissen, was die einzelnen Komponenten bewirken und wie sie alle interagieren.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**MIM-Portal** – eine Schnittstelle für das Zurücksetzen von Kennwörtern, die Gruppenverwaltung und administrative Vorgänge.
    -
- **MIM-Dienst** – ein Webdienst, der MIM 2016-Identitätsverwaltungsfunktionen implementiert.
- **MIM Synchronization Service** – synchronisiert Daten mit anderen Identitätssystemen.
- **Microsoft SQL Server** – Der MIM-Dienst und MIM Synchronization Service speichern ihre Daten beide in SQL-Datenbanken.

In der folgenden Tabelle sind die Optionen zum Hosten der einzelnen MIM-Komponenten dargestellt. Sie können auf demselben Computer gehostet oder auf mehrere Server und Cluster verteilt werden.

| | MIM-Portal | MIM-Dienst | MIM Synchronization Service | SQL Server |
| --- | --- | --- | --- | --- |
| Derselbe Computer | Ja  | Ja  | Ja  | Ja  |
| Separater Server | Ja  | Ja  | Ja  | Ja  |
| Netzwerklastenausgleichs-Cluster | Ja  | Ja  | | |
| Server-Cluster | | | | Ja  |


## <a name="multitier-topology"></a>Mehrschichtige Topologie
Die mehrschichtige Topologie ist die am häufigsten verwendete Topologie. Sie bietet die größte Flexibilität. Das MIM-Portal, der MIM-Dienst und die Datenbanken sind in Schichten unterteilt und auf mehreren Computern bereitgestellt. Diese Topologie bietet mehr Flexibilität bei der Skalierung der verschiedenen MIM-Komponenten. Beispielsweise können Sie das MIM-Portal horizontal skalieren, indem Sie einem Netzwerklastenausgleichs-Cluster (NLB) zusätzliche Server hinzufügen. Auf ähnliche Weise können Sie den MIM-Dienst skalieren, indem Sie einen NLB-Cluster verwenden und die Anzahl von Computern (Knoten) im Cluster je nach Bedarf erhöhen.

Bei der mehrschichtigen Topologie wird ein dedizierter Computer zum Hosten jeder SQL-Datenbank zugewiesen (einer für den MIM-Dienst und ein anderer für MIM Synchronization Service). Die Skalierbarkeit der Leistung der Computer, die die SQL-Datenbanken hosten, kann durch das Hinzufügen oder Upgraden von Hardware, z.B. durch CPU-Upgrades, das Erhöhen oder Upgraden des Arbeitsspeichers (RAM) oder das Upgraden der Festplattenkonfigurationen erzielt werden. Dadurch wird auch der Lese- und Schreibzugriff verbessert und die Wartezeit verringert.

![Diagramm: mehrschichtige Topologie in MIM](media/MIM-topo-multitier.png)

In dieser Konfiguration werden MIM Synchronization Service und dessen Datenbank auf demselben Computer gehostet. Jedoch sollten Sie die gleiche Leistung erzielen können, wenn zwischen MIM Synchronization Service und dessen Datenbank eine 1 GBit dedizierte Netzwerkverbindung besteht, wenn sie auf separaten Computern gehostet werden.


## <a name="multitier-topology-with-multiple-mim-services"></a>Mehrschichtige Topologie mit mehreren MIM-Diensten
Synchronisieren von Daten mit externen Systemen kann sehr lange dauern und in diesem Zeitraum eine erhebliche zusätzliche Last für das System bringen. Wenn die Synchronisierungskonfiguration Richtlinien mit Workflows auslöst, konkurrieren diese Richtlinien mit Endbenutzer-Workflows um Ressourcen. Solche Probleme können bei Authentifizierungsworkflows wie Kennwortzurücksetzungen besonders stark auftreten. Diese Workflows werden in Echtzeit ausgeführt, während der Endbenutzer auf die Fertigstellung des Vorgangs wartet. Sie können eine bessere Reaktionsfähigkeit für Endbenutzervorgänge bieten, indem Sie eine Instanz des MIM-Diensts für Endbenutzervorgänge und ein separates Portal für die Synchronisierung administrativer Daten bereitstellen.

![Diagramm: mehrschichtige Topologie in mehreren MIM-Diensten](media/MIM-topo-multitier-multiservice.png)

Wie bei der standardmäßigen mehrschichtigen Topologie können Sie die Leistung des MIM-Portals erhöhen, indem Sie einen NLB-Cluster verwenden und die Anzahl von Knoten im Cluster je nach Bedarf erhöhen.

Die Gesamtleistung Ihrer MIM-Bereitstellung hängt stark von den Computern ab, auf denen SQL Server installiert ist und die als Host für MIM Synchronization Service und die MIM-Dienstdatenbank fungieren. Befolgen Sie daher die Ratschläge in der SQL Server-Dokumentation zum Optimieren der Datenbankleistung. Weitere Informationen finden Sie in folgenden Dokumenten:

## <a name="see-also"></a>Siehe auch
- Der zum Download bereitstehende [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Handbuch zur Kapazitätsplanung des Forefront Identity Manager (FIM) 2010) bietet weitere Informationen zu einem Test-Build und Leistungstestergebnissen.
