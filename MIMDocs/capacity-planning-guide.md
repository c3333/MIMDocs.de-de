---
title: Kapazitätsplanungsleitfaden | Microsoft Docs
description: Verwenden Sie dieses Handbuch, um die Variablen zu verstehen, die vor dem Bereitstellen von MIM 2016 berücksichtigt werden sollten, einschließlich Auslastungsgrad und Richtlinienentscheidungen.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 15eb35d01ed5c5c6e125c45f238bb2f7a7c564d7
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042116"
---
# <a name="capacity-planning-guide"></a>Kapazitätsplanungshandbuch

Microsoft Identity Manager (MIM) ermöglicht es Ihnen, Benutzerkonten in Ihrer gesamten Organisation zu erstellen, zu aktualisieren und zu entfernen. Außerdem gibt MIM Endbenutzern die Möglichkeit, ihre eigenen Konten mit Self-Service-Features zu verwalten. Selbst in einer kleinen Umgebung können Sie diese Aktionen schnell summieren.

Bevor Sie mit MIM beginnen, sollten Sie diese Anleitung zusammen mit Testumgebungen verwenden, um den entsprechenden Aufgabenbereich für Ihre Bereitstellung zu verstehen. In diesem Artikel werden viele allgemeine Faktoren angesprochen, die Sie berücksichtigen sollten. Da jede Bereitstellung ihre eigenen Besonderheiten hat, ist ein Testen Ihrer Szenarien in einer Testumgebung aber nach wie vor die beste Möglichkeit, die Server, Hardware oder Topologien zu ermitteln, die für Ihre Anforderungen geeignet sind.

Wenn Sie noch nicht mit MIM 2016 und den zugehörigen Komponenten vertraut sind, sollten Sie weitere Informationen zu [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) lesen, bevor Sie die nächsten Schritte ausführen.

## <a name="overview"></a>Overview

Es gibt eine ganze Anzahl von Faktoren, die sich auf die gesamte Kapazität und Leistung Ihrer Microsoft Identity Manager-Bereitstellung auswirken können:

- Die Methoden, mit denen Sie die MIM-Komponenten (Topologie) physisch bereitstellen
- Die Hardware, auf der diese Komponenten ausgeführt werden
- Die Anzahl und Komplexität der MIM-Richtlinienkonfigurationsobjekte sind wichtige Faktoren, die beim Planen der Kapazität bedacht werden müssen.
- Die erwartete Größenordnung der Bereitstellung sowie die erwartete Auslastung sind in der Regel offensichtlichere Faktoren, die die Leistung und Kapazität beeinflussen.

Die wichtigsten Faktoren, die Einfluss auf die Kapazität einer MIM 2016-Bereitstellung haben, werden in der folgenden Tabelle erläutert:

| Designfaktor | Überlegungen |
| ------------- | -------------- |
| Topologie | Die Verteilung der MIM-Dienste zwischen Computern im Netzwerk. |
| Hardware | Die physische Hardware (physisch oder virtuell) für jede MIM-Komponente, einschließlich CPU, Arbeitsspeicher, Netzwerkadapter und Festplattenkonfiguration |
| MIM-Richtlinienkonfigurationsobjekte | Die Anzahl und der Typ der MIM-Richtlinienkonfigurationsobjekte, einschließlich Sätze, Verwaltungsrichtlinien (Management Policy Rules; MPRs) und Workflows. |
| Skalieren | Die Benutzer, Gruppen, berechneten Gruppen und benutzerdefinierten Objekttypen, die von MIM 2016 verwaltet werden sollen. Außerdem sollten Sie auf die Komplexität der dynamische Gruppen achten und auf die Auswirkungen von Gruppenverschachtelung in Betracht ziehen. |
| Last | Nutzungshäufigkeit. Vorgänge, z.B. die Erstellung einer neuen Gruppe oder neuer Benutzer, das Zurücksetzen von Kennwörtern oder Portalbesuche pro Minute oder Stunde. Beachten Sie, dass die Auslastung im Zeitraum von einer Stunde, einem Tag, einer Woche oder einem Jahr variieren kann. Je nach Komponente können Sie diese für Spitzenlast oder für durchschnittliche Last auslegen. |

## <a name="hosting-microsoft-identity-manager-components"></a>Hosten von Microsoft Identity Manager-Komponenten

Die Komponenten von Microsoft Identity Manager müssen sich nicht auf demselben Computer befinden. Ein Betrachten dieser Komponenten und der physischen oder virtuellen Computer, auf denen die Komponenten gehostet werden sollen, ist ein wichtiger Bestandteil der Kapazitätsplanung.

Hardwarefaktoren können sich auf die Leistung der MIM-Umgebung auswirken. Beispiele:

- Wie sieht die Konfiguration der physischen Datenträger für den Computer aus, auf dem die MIM 2016 Service-SQL-Datenbank ausgeführt wird? Die Anzahl der Spindeln, aus denen die Datenträgerkonfiguration oder die Verteilung der Protokoll- und Datendateien bestehen, kann die Leistung des Systems erheblich beeinflussen.

Berücksichtigen Sie außerdem die externen Faktoren in Ihrer Konfiguration. Beispiele:

- Falls Sie ein SAN als die MIM 2016 Dienstdatenbank-Konfiguration verwenden, beachten Sie, welche anderen Anwendungen ebenfalls ein SAN verwenden. Diese Anwendungen wirken sich möglicherweise auf die Leistung der Datenbank aus, wenn sie um die freigegebenen Datenträgerressourcen im SAN konkurrieren.

## <a name="users-and-groups"></a>Benutzer und Gruppen

Die Anzahl von Benutzern und Gruppen in Ihrer Umgebung spielt eine Rolle bei den Überlegungen zur Größenordnung einer Bereitstellung. Es gibt jedoch verschiedene weitere Aspekte diesbezüglich, die Sie auch in die Planung miteinbeziehen sollten.

- Können Benutzer Gruppen erstellen? Falls ja, sollten Sie abschätzen, wie dies das Wachstum von Gruppen in Ihrer Umgebung beeinflusst, wenn Benutzer neue Gruppen erstellen.

- Werden dynamische Gruppen bereitgestellt? Ermitteln Sie, wie viele und welche Arten von dynamischen Gruppen in Ihrer Umgebung zu erwarten sind.

## <a name="expected-load-levels"></a>Erwarteter Auslastungsgrad

Sie sollten auch die Art der Auslastung bedenken, die auf den MIM-Komponenten platziert wird. Sie müssen die Auslastung durch Betrachten der aktuellen Anwendungen in Ihrer Umgebung abschätzen. Berücksichtigen Sie auch die folgenden relevanten Fragen:

- Wie oft erwarten Sie eine dass Einladungen zum Beitritt oder eine Aufforderung zum Verlassen einer Gruppe versendet werden?

- Wie oft erwarten Sie, dass ein Benutzer eine statische oder dynamische Gruppe erstellt?

- Wie viele nicht-benutzergesteuerte Operationen, etwa die Synchronisierung von Änderungen aus externen Systemen, erwarten Sie? Stellen Sie sicher, dass Sie die Last berücksichtigen, die durch Synchronisieren von Identitätsinformationen mit externen Systemen generiert wird.

- Welche Art von Szenarien möchten Sie bereitstellen? Verschiedene Szenarios tragen zu verschiedenen Auslastungsmustern bei. Clientcomputer, auf denen der MIM 2016-Client installiert ist, validieren beispielsweise in regelmäßigen Abständen, ob bei der Anmeldung die Registrierung benötigt wird.

- Erwarten Sie große Variationen im Auslastungsgrad von normaler Auslastung bis Spitzenlast ? Beispielsweise gibt es die Tendenz, dass nach einer Ferienzeit viele Kennwörter zurückgesetzt werden. Stellen Sie sicher, dass Sie Ihre Systemwartung und Synchronisierungszeitpläne außerhalb der erwarteten Auslastungsspitzen laufen lassen. Wenn Sie sich mit der Kapazitätsplanung beschäftigen, stellen Sie sicher, dass Sie die Spitzenlastzeiten berücksichtigen.

## <a name="policy-configuration-objects"></a>Richtlinienkonfigurationsobjekte

MIM-Richtlinienkonfigurationsobjekte enthalten die MPRs, Sätze, Workflows und Synchronisierungsregeln für eine Bereitstellung. MIM-Bereitstellungen sind für jeden Kunden einmalig, da Richtlinienkonfigurationen geändert werden, um den Anforderungen jeder Bereitstellung zu entsprechen. Entscheidende Überlegungen zur Leistungsfähigkeit enthalten folgende MIM-Richtlinienkonfigurationsobjekte:

- **Sätze** Jeder Vorgang im System muss gegenüber vorhandener Listenmitgliedschaften und Updates bewertet werden, die Änderungen in der Listenmitgliedschaft verursachen. Beispielsweise hat die Änderung der Gebäudenummer des Büros eines Mitarbeiters möglicherweise keine großen Auswirkungen. Jedoch können andere Änderungen möglicherweise verschiedene Objekte auf mehreren Ebenen beeinträchtigen, wie z.B. der Austausch eines Managers.

- **Verwaltungsrichtlinienregeln** (Management Policy Rules) Mit MPRs werden Zugriffssteuerungsregeln verwaltet und Workflows ausgelöst. Beim Erstellen von MPRs muss möglicherweise die Anzahl von Sätzen erhöht werden, damit Sie verschiedene Übergangszustände von Objekten erfassen können. Diese zusätzlichen Sätze können zusätzliche Workflows auslösen, wobei jeder Workflow eindeutigen Anforderungen im System zugeordnet werden kann. Dieser Vorgang wird dann zu einem anderen Element, das bei der Kapazitätsplanung miteinbezogen werden muss.

Die Konfiguration von MIM-Richtlinien umfasst außerdem Entscheidungen zur Bereitstellung in Ihrer Umgebung. Sie sollten unbedingt die folgenden Aspekte berücksichtigen:

- Werden Sie fremde Sicherheitsprinzipale über mehrere Gesamtstrukturen der Active Directory-Domänendienste (AD DS) hinweg bereitstellen? Auf diese Weise werden mehr Workflows und Anforderungen generiert, die zu zusätzlicher Auslastung des Systems führen.

- Verwenden Sie die Bereitstellung ohne Code? Falls ja, wirkt sich dies auf die Anzahl der erwarteten Regeleinträge aus sowie auf zugehörige Anforderungen und Workflows im System.

## <a name="next-steps"></a>Nächste Schritte

- [Topologieaspekte für die Bereitstellung von MIM](topology-considerations.md)
- Der zum Download bereitstehende [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide (Handbuch zur Kapazitätsplanung des Forefront Identity Manager (FIM) 2010)](https://www.microsoft.com/en-us/download/details.aspx?id=7437) bietet weitere Informationen zu einem Test-Build und Leistungstestergebnissen.
