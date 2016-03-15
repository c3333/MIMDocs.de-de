---
Titel: Anleitung zur Kapazitätsplanung
MS.Custom:
  - Identitätsmanagement
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId:
Autor: Kgremban
---
# Capacity planning guide

Dieses Handbuch wurde erstellt, bei der Planung Kunden unterstützen, jedoch sollte allein, um zu bestimmen, den entsprechenden Server, Hardware oder Topologien, die für eine Bereitstellung von Microsoft Identity Manager (MIM) werden, nicht verwendet werden. Organisationen gefördert und erwartet, dass Sie ihre eigenen testumgebungen, um Kapazität und Leistung genauer einschätzen zu konfigurieren. Microsoft kann nicht garantieren, dass Organisationen die gleiche Kapazität oder die Leistungsmerkmale, auftreten, wenn die MIM 2016-Komponenten bereitgestellt und Konfiguration der Komponenten, die in diesem Handbuch beschrieben werden.

Um ordnungsgemäß für die MIM-Bereitstellung vorzubereiten, simulieren Sie die erwartete produktionsumgebung in einer Lab-Umgebung und Testen Sie es. Sie können verschiedene Topologien, die auf verschiedene Arten von Hardware testen möchten und führen Sie andere Skala und Auslastungstests Szenario kann nützlich sein, eine bessere Schätzung, was geschehen kann, wenn Sie MIM 2016 in Ihrer Umgebung bereitstellen.


## Übersicht
Es gibt eine Anzahl von Variablen, die gesamte Kapazität und Leistung der Microsoft Identity Manager-Bereitstellung auswirken können. Die Weise, in denen Sie physisch (Topologie), die MIM-Komponenten bereitstellen sowie die Hardware, auf denen diese Komponenten ausführen, sind wichtige Faktoren bei der Ermittlung der Leistung und Kapazität, die Sie von der MIM-Bereitstellung erwarten können. Die Anzahl und Komplexität der MIM-Richtlinie-Konfigurationsobjekte weniger offensichtlich werden, sind sie weiterhin wichtige Faktoren beim Planen der Kapazität. Schließlich sind in der Regel deutlicher Faktoren, die Leistung und Kapazität beeinflussen die erwartete Skalierung der Bereitstellung sowie die Last, die darauf platziert werden soll.

Die wichtigsten Faktoren, die Einfluss auf die Kapazität und Leistung, die aus einer Bereitstellung für MIM 2016 erwartet werden kann, werden in der folgenden Tabelle erläutert.

| Faktor | Überlegungen |
| ------------- | -------------- |
| Topologie | Die Verteilung der MIM Services zwischen Computern im Netzwerk. Werden die MIM 2016-Synchronisierungsdienst wird z. B. auf demselben Computer gehostet, der die Datenbank hostet? |
| Hardware | Die physische Hardware und alle virtualisiert Hardwarespezifikationen, die Sie für die einzelnen MIM-Komponenten ausgeführt werden. Dies umfasst CPU, Arbeitsspeicher, Netzwerkadapter und Festplattenkonfiguration. |
| Konfigurationsobjekte für MIM-Richtlinie | Die Anzahl und Typ der MIM-Richtlinie-Konfigurationsobjekte, einschließlich Gruppen, Management-Richtlinienregeln (MPRs) und Workflows. Z. B. wie viele Workflows für Vorgänge ausgelöst werden? Wie viele Definitionen vorhanden sind, und was ist die relative Komplexität der einzelnen? |
| Skalierung | Die Anzahl der Benutzer, Gruppen, berechnete Gruppen und benutzerdefinierte Objekttypen, z. B. Computer, die von MIM 2016 verwaltet werden. Außerdem sollten Sie die Komplexität der dynamische Gruppen, und achten Sie darauf, dass Sie in der Verschachtelung einbezogen. |
| Laden | Häufigkeit der erwarteten verwenden. Z. B. die Anzahl der Fälle, in denen Sie neue Gruppen oder Benutzer Kennwörter zurückgesetzt werden, erstellt werden sollen, oder das Portal, die in einem bestimmten Zeitraum besucht werden erwarten. Beachten Sie, dass die Last im Rahmen einer Stunde, Tag, Woche oder Jahr variieren kann. Abhängig von der Komponente müssen Sie möglicherweise für Spitzenlast oder durchschnittliche Auslastung zu entwerfen.


## Hosten von Microsoft Identity Manager-Komponenten
Microsoft Identity Manager verfügt über viele verschiedene Komponenten. In vielen Fällen sind diese Komponenten auf demselben Computer nicht gefunden. Wenn Sie sich vorstellen, MIM eine Kapazität planen im Hinblick auf die Komponenten und der physische Computer (und möglicherweise virtuelle Maschinen) auf dem sie gehostet werden, sind wichtige Aspekte. Viele Faktoren können die Leistung der MIM-Umgebung, z. B. den physischen Datenträger-Konfiguration für den Computer mit der MIM 2016-SQL-Datenbank beeinträchtigen. Die Anzahl der Spindeln, aus denen die Datenträgerkonfiguration oder die Verteilung der Protokoll- und Datendateien kann die Leistung des Systems erheblich beeinträchtigen. Berücksichtigen Sie auch die externe Faktoren in Ihrer Konfiguration. Wenn Sie ein SAN als die Datenbankkonfiguration MIM 2016-Dienst verwenden, welche anderen Programmen freigeben SAN? Wie wirken Anwendung Leistung der Datenbank, wie sie freigegebenen Datenträgerressourcen im SAN zugreifen? Wenn mehrere Anwendungen die gleichen Datenträgerressourcen beanspruchen, könnte Engpässe und unregelmäßige Leistung die Folge sein.


## Benutzer und Gruppen
Die Anzahl von Benutzern und Gruppen in Ihrer Umgebung spielt eine Regel, wenn Sie denken, dass die Skalierung einer Bereitstellung. Es gibt jedoch verschiedene andere zu berücksichtigenden Aspekte, die Sie auch in Planung zu berücksichtigen sind. Diese andere Aspekte umfassen Folgendes:

- Werden Benutzer können Gruppen erstellt? Wenn dies der Fall ist, sollten Sie schätzen, wie Benutzer neue Gruppen erstellen können, das Wachstum der Gruppen in Ihrer Umgebung auswirken.

- Werden dynamische Gruppen bereitgestellt?
  - Welche Arten von dynamischen Gruppen werden bereitgestellt?
  - Wie viele dynamische Gruppen sind wahrscheinlich bereitgestellt werden?


## Erwartete Auslastung
Sie sollten auch den Typ der Last, die auf den MIM-Komponenten platziert werden. Die folgenden: relevanten Fragen berücksichtigen

- Wie oft erwarten Sie eine Anforderung zum beitreten oder eine Gruppe verlassen?

- Wie oft voraussichtlich einen Benutzer zum Erstellen einer statischen oder dynamischen?

- Erhalten diese Informationen Sie bei vorhandenen Anwendungen in Ihrer Umgebung?

- Wie stark erwarten Sie nicht von benutzergesteuerten bei Vorgängen, z. B. die Synchronisierung von Änderungen aus externen Systemen? Sicherstellen Sie, dass die Last zu berücksichtigen, die von Synchronisierungsvorgänge von Identitätsinformationen mit externen Systemen generiert wird.

- Welche Art von Szenarien möchten Sie bereitstellen? Verschiedene Szenarien trägt zu verschiedenen Auslastungsmustern. Überprüfen z. B. Clientcomputer, auf denen die MIM 2016-installiert in regelmäßigen Abständen Client auf, wenn die Registrierung bei der Anmeldung, muss die Systemlast erhöhen.

- Erwarten Sie große Variationen in der Auslastung von normal in Spitzenzeiten? Angenommen, erwarten Sie große Anzahl von Kennwörtern nach der Weihnachtszeit? Stellen Sie sicher, dass Sie Ihr System Wartung und Synchronisierung außerhalb der erwarteten Peaks Rechenvorgänge. Wenn Sie sich vorstellen, kapazitätsplanung, sicherzustellen Sie, dass es sich bei Spitzenlastzeiten berücksichtigt werden.


## Konfiguration der Gruppenrichtlinienobjekte

Microsoft Identity Manager-Richtlinie-Konfigurationsobjekte darstellen, die Geschäftslogik für die MIM-Bereitstellung. Dies ist ein Bereich, in dem jede Implementierung MIM wahrscheinlich eindeutig ist, da Richtlinienkonfiguration spezifischen jeder Bereitstellung. MIM Konfiguration Gruppenrichtlinienobjekte enthalten die MPRs, Gruppen, Workflows und Synchronisierungsregeln für eine bestimmte Bereitstellung. Wichtige Überlegungen zur Leistung im Zusammenhang mit der Konfigurationsobjekte für MIM-Richtlinie umfassen Folgendes:

- **Legt** jeden Vorgang im System für vorhandene Satz Mitgliedschaften und Updates, die dazu führen, Änderungen an der Listenmitgliedschaft dass ausgewertet werden muss. Eine einfache Änderung, z. B. Änderung an die Gebäudenummer eines einzelnen Benutzers Office möglicherweise z. B. keine große Auswirkung. Jedoch möglicherweise andere Änderungen eine kaskadierende Auswirkungen, z. B. die Änderung des Managers, die mehrere Objekte auf mehreren Ebenen beeinträchtigen können.

- **Management-Richtlinienregeln** MPRs dienen zum Steuern von Zugriffssteuerungsregeln und auslösenden Workflows. Beim Erstellen von MPRs möglicherweise muss die Anzahl der Gruppen erhöhen, damit verschiedene Objekt Übergänge zwischen Zuständen erfasst werden können. Diese zusätzliche Einstellungssätze können zusätzliche Workflows, wobei jedes eindeutige Anforderungen im System Workflow ausgelöst werden. Dies wird dann ein anderes Element beim Planen der Kapazität einbezogen werden sollen.

Bei der Arbeit an MIM Richtlinie Konfigurationsobjekte, sollten Sie auch Folgendes berücksichtigen:

- Werden Sie Fremde Sicherheitsprinzipale in mehreren Gesamtstrukturen der Active Directory-Domänendienste (AD DS) die Bereitstellung? Auf diese Weise generiert zusätzliche Workflows und Anforderungen, die zusätzliche Belastung des Systems führt.

- Verwenden Bereitstellung ohne Code Sie? Wenn dies der Fall ist, dies wirkt sich auf die Anzahl der erwarteten Regeln Einträge sowie Anforderungen und Workflows im System zugeordnet.


## Weitere Informationen:
- Der herunterladbare [Forefront Identity Manager (FIM) 2010-Planungshandbuch für die Capactity](http://go.microsoft.com/fwlink/?LinkId=200180) wird ausführlicher über eine Test-Build und die Ergebnisse der Leistungstests.
<!--HONumber=Mar16_HO2-->
