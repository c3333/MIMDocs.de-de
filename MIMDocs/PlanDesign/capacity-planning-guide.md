---
# required metadata

title: Kapazitätsplanungshandbuch | Microsoft Identity Manager
description: Verwenden Sie dieses Handbuch, um die Variablen zu verstehen, die vor dem Bereitstellen von MIM 2016 berücksichtigt werden sollten, einschließlich Auslastungsgrad und Richtlinienentscheidungen.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Kapazitätsplanungshandbuch

Dieses Handbuch wurde erstellt, um bei der Kundenplanung zu helfen. Die Bestimmung der entsprechenden Server, der Hardware oder Topologien, die für eine Bereitstellung von Microsoft Identity Manager (MIM) benötigt werden, sollte jedoch nicht alleine auf Grundlage dieses Handbuchs erfolgen. Organisationen werden dazu ermutigt, ihre eigenen Testumgebungen zu konfigurieren, um die Kapazität und Leistung genauer einzuschätzen. Dies wird auch von ihnen erwartet. Microsoft kann nicht garantieren, dass bei Organisationen dieselbe Kapazität oder dieselben Leistungsmerkmale auftreten, auch wenn die MIM 2016-Komponenten auf identische Weise auf den Komponenten bereitgestellt und konfiguriert werden, wie in diesem Handbuch beschrieben.

Um sich ordnungsgemäß für die MIM-Bereitstellung vorzubereiten, simulieren Sie die erwartete Produktionsumgebung in einer Testumgebung und testen Sie sie. Sie können verschiedene Topologien testen, die auf verschiedenen Arten von Hardware laufen. Dabei führen Sie verschiedene Skalierungs- und Auslastungsszenariotests durch, die Ihnen dabei helfen können, besser einzuschätzen, was bei einer Bereitstellung von MIM 2016 in Ihrer Umgebung geschehen kann.


## Übersicht
Es gibt eine ganze Anzahl von Variablen, die sich auf die gesamte Kapazität und Leistung Ihrer Microsoft Identity Manager-Bereitstellung auswirken können. Die Methoden, die Sie nutzen, um die MIM-Komponenten (Topologie) physisch bereitzustellen sowie die Hardware, auf der diese Komponenten laufen, sind wichtige Faktoren bei der Ermittlung der Leistung und Kapazität, die Sie von Ihrer MIM-Bereitstellung erwarten können. Die Anzahl und Komplexität der MIM-Richtlinienkonfigurationsobjekte mögen zwar weniger offensichtlich sein, jedoch sind sie weiterhin wichtige Faktoren, die beim Planen der Kapazität bedacht werden müssen. Schließlich sind die erwartete Größenordnung der Bereitstellung sowie die Auslastung, die darauf platziert werden soll, in der Regel offensichtlichere Faktoren, die die Leistung und Kapazität beeinflussen.

Die wichtigsten Faktoren, die Einfluss auf die erwartete Kapazität und Leistung einer MIM 2016-Bereitstellung haben, werden in der folgenden Tabelle erläutert.

| Designfaktor | Überlegungen |
| ------------- | -------------- |
| Topologie | Die Verteilung der MIM-Dienste zwischen Computern im Netzwerk. Wird z.B. MIM 2016 Synchronization Service auf demselben Computer gehostet, der die Datenbank hostet? |
| Hardware | Die physische Hardware und alle virtualisierten Hardwarespezifikationen, die für jede MIM-Komponenten ausgeführt werden. Dies umfasst die Konfiguration der CPU, des Arbeitsspeichers und Netzwerkadapters und der Festplatte. |
| MIM-Richtlinienkonfigurationsobjekte | Die Anzahl und der Typ der MIM-Richtlinienkonfigurationsobjekte, einschließlich Sätze, Verwaltungsrichtlinien (Management Policy Rules; MPRs) und Workflows. Beispiel: Wie viele Workflows werden für Vorgänge ausgelöst? Wie viele Definitionen für Sets sind vorhanden und was ist die relative Komplexität für jede Definition? |
| Skalieren | Die Anzahl der Benutzer, Gruppen, berechneten Gruppen und benutzerdefinierten Objekttypen, z.B. Computer, die von MIM 2016 verwaltet werden sollen. Außerdem sollten Sie auf die Komplexität der dynamische Gruppen achten und auf die Auswirkungen von Gruppenverschachtelung in Betracht ziehen. |
| Laden | Häufigkeit der erwarteten Verwendung. Z.B. wie oft Sie erwarten, dass innerhalb eines gegebenen Zeitraums neue Gruppen oder Benutzer erstellt, Kennwörter zurückgesetzt oder das Portal besucht wird. Beachten Sie, dass die Auslastung im Zeitraum von einer Stunde, einem Tag, einer Woche oder einem Jahr variieren kann. Es hängt von der Komponente ab, ob diese für durchschnittliche Auslastung oder auch für sehr hohe Auslastung ausgelegt sein muss.


## Hosten von Microsoft Identity Manager-Komponenten
Microsoft Identity Manager verfügt über viele verschiedene Komponenten. In vielen Fällen befinden sich diese Komponenten nicht auf demselben Computer. Wichtige Überlegungen sind vom Standpunkt der Kapazitätsplanung aus gesehen jene, die die Komponenten von MIM und die physischen Computer (und möglicherweise auch virtuelle Computer) betreffen, auf der sich die Komponenten befinden. Viele potenzielle Faktoren können die Leistung der MIM-Umgebung beeinflussen, wie z.B. die Konfiguration des physischen Datenträgers für den Computer, der die SQL-Datenbank des MIM 2016-Diensts ausführt. Die Anzahl der Spindeln, aus denen die Datenträgerkonfiguration oder die Verteilung der Protokoll- und Datendateien bestehen, kann die Leistung des Systems erheblich beeinflussen. Berücksichtigen Sie auch die externe Faktoren in Ihrer Konfiguration. Falls Sie ein SAN als die MIM 2016 Dienstdatenbank-Konfiguration verwenden, beachten Sie, welche anderen Anwendungen ebenfalls ein SAN verwenden. Wie wirken sich diese Anwendungen auf die Leistung der Datenbank aus, wenn sie um die freigegebenen Datenträgerressourcen im SAN konkurrieren? Wenn mehrere Anwendungen die gleichen Datenträgerressourcen beanspruchen, könnten Engpässe und eine unregelmäßige Datenträgerleistung die Folge sein.


## Benutzer und Gruppen
Die Anzahl von Benutzern und Gruppen in Ihrer Umgebung spielt eine Rolle bei den Überlegungen zur Größenordnung einer Bereitstellung. Es gibt jedoch verschiedene weitere Aspekte diesbezüglich, die Sie auch in die Planung miteinbeziehen sollten. Diese Aspekte sind beispielsweise:

- Können Benutzer Gruppen erstellen? Falls ja, sollten Sie abschätzen, wie dies das Wachstum von Gruppen in Ihrer Umgebung beeinflusst, wenn Benutzer neue Gruppen erstellen.

- Werden dynamische Gruppen bereitgestellt?
  - Welche Arten von dynamischen Gruppen werden bereitgestellt?
  - Wie viele dynamische Gruppen werden wahrscheinlich bereitgestellt?


## Erwarteter Auslastungsgrad
Sie sollten auch die Art der Auslastung bedenken, die auf den MIM-Komponenten platziert wird. Berücksichtigen Sie auch die folgenden relevanten Fragen:

- Wie oft erwarten Sie eine dass Einladungen zum Beitritt oder eine Aufforderung zum Verlassen einer Gruppe versendet werden?

- Wie oft erwarten Sie, dass ein Benutzer eine statische oder dynamische Gruppe erstellt?

- Können Sie diese Informationen von vorhandenen Anwendungen in Ihrer Umgebung erhalten?

- Wie hoch erwarten Sie die Auslastung, die von den nicht-benutzergesteuerten Operationen stammt, wie z.B. der Synchronisierung von Änderungen aus externen Systemen? Stellen Sie sicher, dass Sie die Last berücksichtigen, die von Synchronisierungsvorgängen von Identitätsinformationen mit externen Systemen generiert wird.

- Welche Art von Szenarien möchten Sie bereitstellen? Verschiedene Szenarien tragen zu verschiedenen Auslastungsmustern bei. Clientcomputer, auf denen der MIM 2016-Client installiert ist, validieren beispielsweise in regelmäßigen Abständen, ob bei der Anmeldung die Registrierung benötigt wird. Dies erhöht Ihre Systemlast.

- Erwarten Sie große Variationen im Auslastungsgrad von normaler Auslastung bis Spitzenlast ? Erwarten Sie z.B. viele Kennwortzurücksetzungen nach Urlaubszeiten? Stellen Sie sicher, dass Sie Ihre Systemwartung und Synchronisierungszeitpläne außerhalb der erwarteten Auslastungsspitzen laufen lassen. Wenn Sie sich mit der Kapazitätsplanung beschäftigen, stellen Sie sicher, dass Sie die Spitzenlastzeiten berücksichtigen.


## Richtlinienkonfigurationsobjekte

Microsoft Identity Manager-Richtlinienkonfigurationsobjekte stellen die Geschäftslogik für die MIM-Bereitstellung dar. Dies ist ein Bereich, in dem jede MIM-Implementierung wahrscheinlich einmalig ist, da die Richtlinienkonfiguration für die Bedürfnisse jeder Bereitstellung spezifisch ist. MIM-Richtlinienkonfigurationsobjekte enthalten die MPRs, Sätze, Workflows und Synchronisierungsregeln für eine bestimmte Bereitstellung. Entscheidende Überlegungen zur Leistungsfähigkeit im Zusammenhang mit MIM-Richtlinienkonfigurationsobjekten enthalten Folgendes:

- **Sätze** Jeder Vorgang im System muss gegenüber vorhandener Listenmitgliedschaften und Updates bewertet werden, die Änderungen in der Listenmitgliedschaft verursachen. Beispielswiese hat eine einfache Änderung, wie die Änderung der Gebäudenummer des Büros eines Mitarbeiters, möglicherweise keine großen Auswirkungen. Jedoch können andere Änderungen möglicherweise verschiedene Objekte auf mehreren Ebenen beeinträchtigen, wie z.B. der Austausch eines Managers.

- **Verwaltungsrichtlinienregeln** MPRs dienen zum Steuern von Zugriffssteuerungsregeln und zum Auslösen von Workflows. Beim Erstellen von MPRs müssen Sie möglicherweise die Anzahl der Sätze erhöhen, damit Sie verschiedene Übergangszustände von Objekten erfassen können. Diese zusätzlichen Sätze können zusätzliche Workflows auslösen, wobei jeder Workflow eindeutigen Anforderungen im System zugeordnet werden kann. Dieser Vorgang wird dann zu einem anderen Element, das bei der Kapazitätsplanung miteinbezogen werden muss.

Bei der Arbeit an MIM-Richtlinienkonfigurationsobjekten, sollten Sie auch Folgendes berücksichtigen:

- Werden Sie fremde Sicherheitsprinzipale über mehrere Gesamtstrukturen der Active Directory-Domänendienste (AD DS) hinweg bereitstellen? Auf diese Weise werden mehr Workflows und Anforderungen generiert, die zu zusätzlicher Auslastung des Systems führen.

- Verwenden Sie die Bereitstellung ohne Code? Falls ja, wirkt sich dies auf die Anzahl der erwarteten Regeleinträge aus sowie auf zugehörige Anforderungen und Workflows im System.


## Weitere Informationen:
- Der zum Download bereitstehende [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Handbuch zur Kapazitätsplanung des Forefront Identity Manager (FIM) 2010) bietet weitere Informationen zu einem Test-Build und Leistungstestergebnissen.


<!--HONumber=Apr16_HO2-->


