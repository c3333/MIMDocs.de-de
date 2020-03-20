---
title: Konzepthandbuch für Microsoft BHOLD Suite | Microsoft-Dokumentation
description: Erste Schritte mit MIM 2016-Komponenten durch Installation und Konfiguration von Synchronization Service
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: f120709e517d82d4f94e72f4d0a44361f5552a1c
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042303"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Konzepthandbuch für Microsoft BHOLD Suite

Microsoft Identity Manager 2016 (MIM) ermöglicht es Organisationen, den gesamten Lebenszyklus von Benutzeridentitäten und deren zugeordneten Anmeldeinformationen zu verwalten. MIM kann dafür konfiguriert werden, Identitäten zu synchronisieren, Zertifikate und Passwörter zentral zu verwalten und Benutzer über heterogene Systeme hinweg bereitzustellen. Mit MIM können IT-Organisationen den Prozess der Identitätsverwaltung von der Erstellung bis zur Deaktivierung definieren und automatisieren.

Microsoft BHOLD Suite erweitert diese Funktionen von MIM durch Hinzufügen der rollenbasierten Zugriffssteuerung. BHOLD ermöglicht es Organisationen, Benutzerrollen zu definieren und den Zugriff auf vertrauliche Daten und Anwendungen zu steuern. Der Zugriff basiert darauf, was für diese Rollen geeignet ist. BHOLD Suite enthält Dienste und Tools, die das Modellieren von Rollenbeziehungen innerhalb der Organisation vereinfachen. BHOLD ordnet diesen Rollen Rechte zu und überprüft, ob die Rollendefinitionen und die zugeordneten Rechte ordnungsgemäß auf die Benutzer angewendet werden. Diese Funktionen sind vollständig in MIM integriert, wodurch eine nahtlose Nutzung für Benutzer und IT-Mitarbeiter ermöglicht wird.

Dieser Leitfaden bietet Ihnen einen Überblick darüber, wie BHOLD Suite mit MIM arbeitet und umfasst die folgenden Themen:

- Rollenbasierte Zugriffssteuerung
- Nachweis
- Analysen
- Berichterstellung
- Zugriffsverwaltungsconnector
- MIM-Integration

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Die häufigste Methode für das Steuern von Benutzerzugriff auf Daten und Anwendungen ist die besitzverwaltete Zugriffskontrolle (Discretionary Access Control, DAC). In den am häufigsten verwendeten Implementierungen verfügt jedes wichtige Objekt über einen festgelegten Besitzer. Der Besitzer kann den Zugriff auf das Objekt basierend auf einzelnen Identitäten oder Gruppenmitgliedschaften für andere Benutzer gewähren oder verweigern. In der Praxis resultiert DAC üblicherweise in einer Fülle von Sicherheitsgruppen. Einige davon stellen Organisationsstrukturen dar, andere funktionale Gruppierungen (z.B. Auftragstypen oder Projektzuweisungen) und andere bestehen aus provisorischen Sammlungen von Benutzern und Geräten, die für temporäre Zwecke verknüpft sind. Wenn Organisationen wachsen, wird die Verwaltung der Mitgliedschaft in diesen Gruppen zunehmend schwieriger. Wenn ein Mitarbeiter zum Beispiel von einem Projekt auf ein anderes übertragen wird, müssen die Gruppen, die für das Steuern des Zugriffs auf die Projektressourcen verwendet werden, manuell aktualisiert werden. In solchen Fällen ist es nicht ungewöhnlich, dass Fehler auftreten, die die Projektsicherheit oder Produktivität beeinträchtigen können.

MIM enthält Features, die Sie bei der Umgehung dieses Problems unterstützen, indem eine automatisierte Steuerung für die Mitgliedschaften in Gruppen und Verteilerlisten bereitgestellt wird. Dabei wird jedoch nicht die systeminterne Komplexität der sich stark vermehrenden Gruppen berücksichtigt, die nicht notwendigerweise strukturiert miteinander verknüpft sind.

Eine Möglichkeit, diese Vermehrung zu reduzieren, ist die rollenbasierte Zugriffssteuerung (Role-based acces control, RBAC). RBAC ersetzt nicht DAC.  RBAC baut auf DAC auf, indem ein Framework für das Klassifizieren von Benutzern und IT-Ressourcen bereitgestellt wird. Dadurch wird es Ihnen ermöglicht, deren Beziehungen und die Zugriffsrechte, die gemäß dieser Klassifizierung geeignet sind, deutlich zu machen. Durch das Zuweisen eines Benutzerattributs, das die Position und die Projektzuweisungen eines Benutzers angibt, kann dem Benutzer beispielsweise Zugriff auf die Tools und Daten gewährt werden, die für dessen Auftrag und zum Mitwirken an einem bestimmten Projekt benötigt werden. Wenn der Benutzer einen anderen Auftrag und andere Projektzuweisungen annimmt, blockiert das Ändern der Attribute, die die Position und die Projekte des Benutzers angeben, automatisch den Zugriff auf die Ressourcen, die nur für die vorherigen Position des Benutzers erforderlich waren.

Da Rollen in einer hierarchischen Beziehung innerhalb von anderen Rollen enthalten sein können (z.B. können die Rollen des Vertriebsleiters und des Vertriebsmitarbeiters in der allgemeinen Rolle des Vertriebs enthalten sein), ist es einfach, die geeigneten Rechte bestimmten Rollen zuzuweisen und dennoch die geeigneten Rechte für jeden bereitzustellen, der auch die umfassendere Rolle innehat. In einem Krankenhaus kann zum Beispiel dem gesamten medizinischen Personal das Recht erteilt werden, die Patientenakte einzusehen, aber nur Ärzten (eine Unterrolle der medizinischen Rollen) kann das Recht erteilt werden, dem Patienten Rezepte auszustellen. Auf ähnliche Weise kann Benutzern in der Rolle des Büropersonals der Zugriff auf Patientenakten verweigert werden. Eine Ausnahme stellen die Fakturisten dar (eine Unterrolle der Rolle des Büropersonals), denen der Zugriff auf die Patientenakten der Patienten erteilt werden kann, denen eine Rechnung für die durch das Krankenhaus bereitgestellten Dienste gestellt werden muss.

Ein zusätzlicher Vorteil von RBAC ist die Möglichkeit, die Trennung von Aufgaben (Separation of Duties, SoD) zu definieren und zu erzwingen. Dadurch wird es Organisationen ermöglicht, Kombinationen von Rollen zu definieren, die Berechtigungen erteilen, die nicht dem gleichen Benutzer zugewiesen werden sollten. Somit kann ein Benutzer nicht zu Rollen zugewiesen werden, die es diesem beispielsweise ermöglichen, eine Zahlung zu initiieren und diese zu autorisieren. RBAC bietet die Möglichkeit, eine solche Richtlinie automatisch zu erzwingen, anstatt die effektive Implementierung der Richtlinie auf Basis einzelner Benutzer auszuwerten.

### <a name="bhold-role-model-objects"></a>BHOLD-Objekte für Rollenmodelle

Mit BHOLD Suite können Sie Rollen innerhalb Ihrer Organisation angeben und organisieren, Benutzern Rollen zuweisen und Rollen die geeigneten Berechtigungen zuweisen. Diese Struktur wird als Rollenmodell bezeichnet. Es enthält und verbindet fünf Objekttypen: 

- Organisationseinheiten
- -Benutzer
- Rollen
- Berechtigungen
- Applications

#### <a name="organizational-units"></a>Organisationseinheiten

Organisationseinheiten (OrgUnits) sind das wichtigste Mittel, mit dem Benutzer im BHOLD-Rollenmodell organisiert werden. Jeder Benutzer muss mindestens einer OrgUnit angehören. (Wenn ein Benutzer aus der letzten Organisationseinheit in BHOLD gelöscht wird, wird der Datensatz des Benutzers aus der BHOLD-Datenbank gelöscht.)

> [!Important]
> Organisationseinheiten im BHOLD-Rollenmodell dürfen nicht mit Organisationseinheiten in Active Directory Domain Services (AD DS) verwechselt werden. In der Regel basiert die Struktur der Organisationseinheit in BHOLD auf der Organisation und den Richtlinien Ihres Unternehmens, nicht auf den Anforderungen Ihrer Netzwerkinfrastruktur.

Obwohl es nicht erforderlich ist, werden Organisationseinheiten meistens in BHOLD strukturiert, um die hierarchische Struktur der tatsächlichen Organisation etwa folgendermaßen darzustellen:

![](media/bhold-concepts-guide/org-chart.png)

In diesem Beispiel organisiert das Rollenmodell die Organisationseinheit des Unternehmens als Ganzes (dargestellt durch den Präsidenten, da der Präsident nicht Teil einer Organisationseinheit ist). Die Stammorganisationseinheit von BHOLD (die immer vorhanden ist) kann ebenfalls für diesen Zweck verwendet werden. OrgUnits, die die Abteilungen des Unternehmens darstellen, die von den Vizepräsidenten geleitet werden, würden in den Organisationseinheiten des Unternehmens platziert werden. Organisationseinheiten, die den Marketing- und Vertriebsleitern entsprechen, würden zu den Organisationseinheiten für Marketing und Vertrieb hinzugefügt werden, und Organisationseinheiten, die den Regionalverkaufsleiter darstellen, würden in der Organisationseinheit des Vertriebsleiters für die Region USA Ost platziert werden. Verkaufsmitarbeiter, die keine anderen Benutzer verwalten, wären Mitglieder der Organisationseinheit des Vertriebsleiters für die Region USA Ost. Beachten Sie, dass jeder Benutzer Mitglied einer Organisationseinheit auf jeder Ebene sein kann. Beispielsweise wäre ein Verwaltungsassistent, der kein Manager ist und direkt an einen Vizepräsidenten berichtet, ein Mitglied der Organisationseinheit des Vizepräsidenten.

Zusätzlich zum Darstellen der Organisationsstruktur können Organisationseinheiten auch verwendet werden, um Benutzer und andere Organisationseinheiten gemäß den funktionalen Kriterien (z.B. Projekte oder Spezialisierung) zu gruppieren. Im folgenden Diagramm wird dargestellt, wie Organisationseinheiten für das Gruppieren von Verkaufsmitarbeiten nach Kundentyp verwendet werden:

![](media/bhold-concepts-guide/org-chart-02.png)

In diesem Beispiel gehört jeder Verkaufsmitarbeiter zu zwei Organisationseinheiten: eine, die die Stellung des Mitarbeiters in der Struktur der Unternehmensverwaltung darstellt und eine, die den Kundenstamm des Mitarbeiters (Einzelhandel oder Unternehmen) darstellt. Jeder Organisationseinheit können verschiedene Rollen zugewiesen werden. Diesen wiederum können verschiedene Berechtigungen für den Zugriff auf die IT-Ressourcen des Unternehmens zugewiesen werden. Darüber hinaus können Rollen von übergeordneten Organisationseinheiten geerbt werden, wodurch das Verteilen von Rollen an Benutzer vereinfacht wird. Andererseits kann verhindert werden, dass bestimmte Rollen geerbt werden. Dadurch wird sichergestellt, dass eine bestimmte Rolle nur der geeigneten Organisationseinheit zugewiesen wird.

OrgUnits können in BHOLD Suite erstellt werden, indem das BHOLD Core-Webportal oder der BHOLD-Modellgenerator verwendet wird.

#### <a name="users"></a>-Benutzer

Wie bereits erwähnt, muss jeder Benutzer mindestens einer Organisationseinheit (OrgUnit) angehören. Da Organisationseinheiten den wichtigsten Mechanismus darstellen, um einem Benutzer Rollen zuzuweisen, gehört ein bestimmter Benutzer in den meisten Organisationen mehreren OrgUnits an, um das Zuweisen von Rollen zu diesem Benutzer zu vereinfachen. In einigen Fällen kann es jedoch erforderlich sein, einem Benutzer eine Rolle zuzuweisen, die von den OrgUnits, denen der Benutzer angehört, ausgenommen ist. Folglich kann ein Benutzer direkt einer Rolle zugewiesen werden und Rollen von den OrgUnits erhalten, denen dieser angehört.

Wenn ein Benutzer innerhalb der Organisation nicht aktiv ist (z.B. durch krankheitsbedingte Fehlzeit), kann der Benutzer gesperrt werden, wodurch alle Berechtigungen des Benutzers widerrufen werden, ohne den Benutzer aus dem Rollenmodell zu entfernen. Bei seiner Rückkehr kann der Benutzer reaktiviert werden. Dadurch werden alle Berechtigungen wiederhergestellt, die durch die Rolle des Benutzers erteilt werden.

Objekte für Benutzer können über das BHOLD Core-Webportal einzeln in BHOLD erstellt werden. Sie können aber auch in großen Mengen importiert werden, indem der BHOLD-Modellgenerator oder der Zugriffsverwaltungsconnector mit dem FIM-Synchronisierungsdienst verwendet wird, um Benutzerinformationen von Quellen wie Active Directory Domain Services oder Personalanwendungen zu importieren.

Benutzer können direkt in BHOLD erstellt werden, ohne den FIM-Synchronisierungsdienst zu verwenden. Dies kann beim Modellieren von Rollen in einer Präproduktions- oder Testumgebung nützlich sein. Sie können auch externen Benutzern (z.B. Mitarbeiter eines Zulieferers) Rollen zuweisen und somit Zugriff auf die IT-Ressourcen gewähren, ohne diese zu der Mitarbeiterdatenbank hinzuzufügen. Diese Benutzer können jedoch nicht die Self-Service-Features von BHOLD verwenden.

#### <a name="roles"></a>Rollen

Wie bereits erwähnt, werden Berechtigungen im Modell für rollenbasierte Zugriffssteuerung (RBAC) eher Rollen als einzelnen Benutzern zugewiesen. Dadurch können jedem Benutzer die Berechtigungen erteilt werden, die zum Ausführen seiner Aufgaben erforderlich sind, indem die Rolle des Benutzers geändert wird, statt dem Benutzer einzeln Berechtigungen zu erteilen oder zu verweigern. Dadurch erfordert das Zuweisen von Berechtigungen nicht mehr die Beteiligung der IT-Abteilung, sondern kann im Rahmen der Verwaltung des Unternehmens ausgeführt werden. Eine Rolle kann die Berechtigungen für den Zugriff auf verschiedene Systeme verbinden. Dies kann entweder direkt oder durch das Verwenden von Unterrollen geschehen, wodurch die Notwendigkeit für die Beteiligung der IT-Abteilung am Verwalten von Benutzerberechtigungen weiter reduziert wird.

Beachten Sie, dass es sich bei Rollen um Features des RBAC-Modells selbst handelt, deshalb können Rollen üblicherweise nicht für Zielanwendungen bereitgestellt werden. Dadurch kann RBAC zusammen mit bestehenden Anwendungen verwendet werden, die nicht für das Verwenden von Rollen oder zum Ändern der Rollendefinitionen ausgelegt wurden. Außerdem können die Anforderungen von sich ändernden Geschäftsmodellen erfüllt werden, ohne die Anwendung selbst ändern zu müssen. Wenn eine Zielanwendung dafür ausgelegt ist, Rollen zu verwenden, können Sie die Rollen des BHOLD-Rollenmodells den entsprechenden Anwendungsrollen zuweisen, indem Sie die anwendungsspezifischen Rollen als Berechtigungen behandeln.

In BHOLD können Sie einem Benutzer eine Rolle in erster Linie durch zwei Mechanismen zuweisen:

- Durch das Zuweisen einer Rolle zu einer Organisationseinheit, der der Benutzer angehört
- Durch das direkte Zuweisen einer Rolle zu einem Benutzer

Eine Rolle, die einer übergeordneten Organisationseinheit zugewiesen ist, kann optional an die untergeordneten Organisationseinheiten derselben vererbt werden. Wenn eine Rolle von einer Organisationseinheit zugewiesen oder geerbt wird, kann diese als effektive oder vorgeschlagene Rolle festgelegt werden. Wenn es sich um eine effektive Rolle handelt, wird diese Rolle allen Benutzern in der Organisationseinheit zugewiesen. Wenn es sich um eine vorgeschlagene Rolle handelt, muss diese für jeden Benutzer oder jede untergeordnete Organisationseinheit aktiviert werden, um für diesen Benutzer oder die Mitglieder der Organisationseinheit wirksam zu werden. Dies ermöglicht es, Benutzern eine Teilmenge der Rollen zuzuweisen, die einer Organisationseinheit zugeordnet sind, statt automatisch alle Rollen einer Organisationseinheit allen Mitgliedern zuzuweisen. Zusätzlich können Rollen mit einem Anfangs- und einem Enddatum versehen werden, weiterhin können Grenzwerte für einen Prozentsatz der Benutzer innerhalb einer Organisationseinheit festgelegt werden, für den eine Rolle wirksam sein kann.

Das folgende Diagramm stellt dar, wie einem einzelnen Benutzer eine Rolle in BHOLD zugewiesen werden kann:

![](media/bhold-concepts-guide/org-chart-flow.png)

In diesem Diagramm wird Rolle A als vererbbare Rolle zu einer Organisationseinheit hinzugefügt. Dadurch wird diese von den untergeordneten Organisationseinheiten und allen Benutzern innerhalb dieser Organisationseinheiten geerbt. Rolle B wird als vorgeschlagene Rolle für eine Organisationseinheit zugewiesen. Diese muss aktiviert werden, bevor ein Benutzer in der Organisationseinheit für die Berechtigungen der Rolle autorisiert werden kann. Rolle C ist eine effektive Rolle, sodass deren Berechtigungen sofort für alle Benutzer in der Organisationseinheit gelten. Rolle D ist direkt mit dem Benutzer verknüpft, sodass deren Berechtigungen sofort für diesen Benutzer gelten.

Darüber hinaus kann eine Rolle basierend auf den Attributen eines Benutzers für einen Benutzer aktiviert werden. Weitere Informationen finden Sie unter „Attribute-based authorization“ (Attributbasierte Autorisierung).

#### <a name="permissions"></a>Berechtigungen

Eine Berechtigung in BHOLD entspricht einer Autorisierung, die von einer Zielanwendung importiert wurde. Wenn BHOLD für das Arbeiten mit einer Anwendung konfiguriert ist, erhält diese eine Liste der Autorisierungen, die BHOLD mit Rollen verknüpfen kann. Wenn beispielsweise Active Directory Domain Services (AD DS) als Anwendung zu BHOLD hinzugefügt wird, erhält dieses eine Liste der Sicherheitsgruppen, die als BHOLD-Berechtigungen mit Rollen in BHOLD verknüpft werden können.

Berechtigungen sind anwendungsspezifisch. BHOLD stellt eine einzige, einheitliche Sicht der Berechtigungen bereit, sodass Berechtigungen Rollen zugeordnet werden können, ohne dass Rollen-Manager erforderlich sind, um die Implementierungsdetails der Berechtigungen zu verstehen. In der Praxis können verschiedene Systemen eine Berechtigung auf unterschiedliche Arten erzwingen. Der anwendungsspezifische Connector des FIM-Synchronisierungsdiensts für die Anwendung bestimmt, wie Änderungen an den Berechtigungen eines Benutzers für diese Anwendung bereitgestellt werden. 

#### <a name="applications"></a>Applications

BHOLD implementiert eine Methode für das Anwenden der rollenbasierten Zugriffssteuerung (RBAC) auf externe Anwendungen. Wenn die Benutzer und Berechtigungen einer Anwendung für BHOLD bereitgestellt werden, kann BHOLD diese Berechtigungen den Benutzern zuordnen, indem die Rollen den Benutzern zugewiesen und anschließend die Berechtigungen mit den Rollen verknüpft werden. Der Hintergrundprozess der Anwendung kann dann die richtigen Berechtigungen basierend auf der Zuordnung der Rollen/Berechtigungen in BHOLD zu deren Benutzern zuordnen.

### <a name="developing-the-bhold-suite-role-model"></a>Entwickeln des BHOLD Suite-Rollenmodells

BHOLD Suite stellt einen Modellgenerator bereit, um Sie beim Entwickeln Ihres Rollenmodells zu unterstützen. Dieses Tool ist umfangreich und leicht zu verwenden.

Bevor Sie den Modellgenerator verwenden, müssen Sie eine Reihe von Dateien erstellen, die die Objekte definieren, die der Modellgenerator zum Erstellen des Rollenmodells verwendet. Informationen zum Erstellen dieser Dateien finden Sie unter „Microsoft BHOLD Suite Technical Reference“ (Technischer Verweis zu Microsoft BHOLD Suite).

Der erste Schritt beim Verwenden des BHOLD-Modellgenerators ist das Importieren dieser Dateien, um die grundlegenden Bausteine in den Modellgenerator zu laden. Wenn die Dateien erfolgreich geladen wurden, können Sie Kriterien angeben, die der Modellgenerator zum Erstellen mehrerer Klassen von Rollen verwendet:

- Mitgliedschaftsrollen, die einem Benutzer basierend auf den OrgUnits (Organisationseinheiten) zugewiesen werden, zu denen dieser gehört
- Attributrollen, die einem Benutzer basierend auf den Attributen des Benutzers in der BHOLD-Datenbank zugewiesen werden
- Vorgeschlagene Rollen, die mit einer Organisationseinheit verknüpft sind, aber für bestimmte Benutzer aktiviert werden müssen
- Besitzerrollen, die einem Benutzer die Steuerung über Organisationseinheiten und Rollen gewähren, für die kein Besitzer in den importierten Dateien angegeben ist

> [!Important]
> Aktivieren Sie das Kontrollkästchen **Retain Existing Model** (Vorhandenes Modell beibehalten) beim Hochladen von Dateien nur in Testumgebungen. In Produktionsumgebungen müssen Sie den Modellgenerator verwenden, um das ursprüngliche Rollenmodell zu erstellen. Sie können diesen jedoch nicht benutzen, um ein bestehendes Rollenmodell in der BHOLD-Datenbank zu ändern.

Nachdem der Modellgenerator diese Rollen im Rollenmodell erstellt hat, können Sie das Rollenmodell in Form einer XML-Datei in die BHOLD-Datenbank exportieren.

### <a name="advanced-bhold-features"></a>Erweiterte BHOLD-Features

In den vorherigen Abschnitten wurden die grundlegenden Features der rollenbasierten Zugriffssteuerung in BHOLD beschrieben. In diesem Abschnitt werden zusätzliche Features in BHOLD beschrieben, die eine erhöhte Sicherheit und Flexibilität für die RBAC-Implementierung Ihrer Organisation bereitstellen. Dieser Abschnitt enthält eine Übersicht über die folgenden BHOLD-Features:

- Kardinalität
- Trennung von Aufgaben
- An Kontext anpassbare Berechtigungen
- Attributbasierte Autorisierung
- Flexible Attributtypen


#### <a name="cardinality"></a>Kardinalität

*Kardinalität* bezieht sich auf die Implementierung von Geschäftsregeln, die entwickelt wurden, um die Häufigkeit einzuschränken, mit der zwei Entitäten miteinander verknüpft werden können. Im Fall von BHOLD können Kardinalitätsregeln für Rollen, Berechtigungen und Benutzer eingerichtet werden.

Sie können eine Rolle konfigurieren, um Folgendes zu beschränken:

- Die maximale Anzahl der Benutzer, für die eine vorgeschlagene Rolle aktiviert werden kann
- Die maximale Anzahl der Unterrollen, die mit der Rolle verknüpft werden können
- Die maximale Anzahl der Berechtigungen, die mit der Rolle verknüpft werden können

Sie können eine Berechtigung konfigurieren, um Folgendes zu beschränken:

- Die maximale Anzahl der Rollen, die mit der Berechtigung verknüpft werden können
- Die maximale Anzahl der Benutzer, denen eine Berechtigung erteilt werden kann

Sie können einen Benutzer konfigurieren, um Folgendes zu beschränken:

- Die maximale Anzahl der Rollen, die mit dem Benutzer verknüpft werden können
- Die maximale Anzahl der Berechtigungen, die einem Benutzer über Rollenzuweisungen zugewiesen werden können

#### <a name="separation-of-duties"></a>Trennung von Aufgaben

Bei der Trennung von Aufgaben (SoD) handelt es sich um ein Geschäftsprinzip, das verhindern soll, dass Einzelpersonen die Fähigkeit zum Durchführen von Aktionen erhalten, die nicht für eine einzelne Person verfügbar sein sollten. Ein Mitarbeiter darf beispielsweise keine Zahlung anfordern und diese autorisieren. Durch das SoD-Prinzip wird es Organisationen ermöglicht, ein System gegenseitiger Kontrolle zu implementieren, um die Gefährdung durch Mitarbeiterfehler oder Fehlverhalten zu minimieren.

BHOLD implementiert SoD, indem Ihnen ermöglicht wird, nicht kompatible Berechtigungen zu definieren. Wenn diese Berechtigungen definiert sind, wird SoD von BHOLD erzwungen, indem das Erstellen von Rollen, die mit nicht kompatiblen Berechtigungen verknüpft sind, unabhängig davon verhindert wird, ob diese direkt verknüpft oder geerbt sind. Außerdem wird verhindert, dass Benutzern mehrere Rollen (durch direkte Zuweisung oder Vererbung) zugewiesen werden, deren Kombination zu nicht kompatiblen Berechtigungen führt. Optional können Konflikte überschrieben werden.

#### <a name="context-adaptable-permissions"></a>An Kontext anpassbare Berechtigungen

Indem Sie Berechtigungen erstellen, die basierend auf dem Objektattribut automatisch geändert werden können, können Sie die Gesamtanzahl der Berechtigungen reduzieren, die Sie verwalten müssen. An Kontext anpassbare Berechtigungen (Context-adaptable permissions, CAP) ermöglichen Ihnen das Definieren einer Formel als Berechtigungsattribut, die das Anwenden der Berechtigung basierend auf der Anwendung ändert, die der Berechtigung zugeordnet ist. Sie können zum Beispiel eine Formel erstellen, die die Zugriffsberechtigung für einen Dateiordner ändert (mithilfe einer Sicherheitsgruppe, die der Zugriffssteuerungsliste des Ordners zugeordnet ist). Dies basiert darauf, ob ein Benutzer zu einer Organisationseinheit gehört, die Angestellte oder Vollzeitmitarbeiter enthält. Wenn der Benutzer von einer Organisationseinheit zu einer anderen verschoben wird, wird die neue Berechtigung automatisch angewendet, und die alte Berechtigung wird deaktiviert. 

Die CAP-Formel kann die Werte von Attributen abfragen, die auf Anwendungen, Berechtigungen, Organisationseinheiten und Benutzer angewendet wurden.

#### <a name="attribute-based-authorization"></a>Attributbasierte Autorisierung

Eine Möglichkeit, um zu steuern, ob eine Rolle, die mit einer Organisationseinheit verknüpft ist, für einen bestimmten Benutzer in der Organisationseinheit aktiviert ist, ist das Verwenden der attributbasierten Autorisierung (attribute-based authorization, ABA). Mithilfe von ABA können Sie eine Rolle automatisch aktivieren, wenn bestimmte Regeln, die auf den Attributen des Benutzers basieren, erfüllt werden. Beispielsweise können Sie eine Rolle mit einer Organisationseinheit verknüpfen, die nur dann für einen Benutzer wirksam wird, wenn die Position des Benutzers der Position in der ABA-Regel entspricht. Dadurch ist es nicht mehr nötig, eine vorgeschlagene Rolle für einen Benutzer manuell zu aktivieren. Stattdessen kann eine Rolle für alle Benutzer in einer Organisationseinheit aktiviert werden, die einen Attributwert besitzen, der die ABA-Regel der Rolle erfüllt. Regeln können kombiniert werden, sodass eine Rolle nur aktiviert wird, wenn die Attribute eines Benutzers alle ABA-Regeln erfüllen, die für die Rolle angegeben sind.

Beachten Sie, dass die Testergebnisse von ABA-Regeln durch Kardinalitätseinstellungen begrenzt werden. Wenn die Kardinalitätseinstellung für eine Regel beispielsweise angibt, dass nicht mehr als zwei Benutzer der Rolle zugewiesen werden können und die ABA-Regel eine Rolle für vier Benutzer aktivieren würde, wird die Rolle nur für die ersten zwei Benutzer aktiviert, die den ABA-Test bestehen.

#### <a name="flexible-attribute-types"></a>Flexible Attributtypen

Das Attributsystem in BHOLD ist in hohem Maße erweiterbar. Sie können neue Attributtypen für Objekte wie Benutzer, Organisationseinheit und Rollen definieren. Attribute können für Ganzzahlwerte, boolesche Werte (Ja/Nein), alphanumerische Werte, Datumswerte, Zeitwerte und E-Mail-Adressen definiert werden. Attribute können als Einzelwerte oder als Werteliste angegeben werden.

## <a name="attestation"></a>Nachweis

BHOLD Suite stellt Tools bereit, die Sie verwenden können, um zu überprüfen, ob einzelnen Benutzern die geeigneten Berechtigungen erteilt wurden, um ihre geschäftlichen Aufgaben auszuführen. Der Administrator kann das Portal verwenden, das vom BHOLD-Nachweismodul bereitgestellt wird, um den Nachweisprozess zu entwerfen und zu verwalten.

Der Nachweisprozess wird mithilfe von Kampagnen durchgeführt, bei denen die Kampagnenverantwortlichen zur Überprüfung befähigt werden, dass der Benutzer, für den sie zuständig sind, über den geeigneten Zugriff auf von BHOLD verwaltete Anwendungen und die richtigen Berechtigungen innerhalb dieser Anwendungen verfügt. Ein Kampagnenbesitzer wird festgelegt, um zu überwachen und zu versichern, dass die Kampagne ordnungsgemäß ausgeführt wird. Eine Kampagne kann für einmalige oder mehrfache Ausführung erstellt werden.

In der Regel handelt es sich bei dem Verantwortlichen für die Kampagne um einen Manager, der die Zugriffsrechte von Benutzern nachweist, die zu einer oder mehreren Organisationseinheiten gehören, für die der Manager zuständig ist. Verantwortliche können automatisch für die Benutzer ausgewählt werden, die in einer auf Benutzerattributen basierenden Kampagne nachgewiesen werden. Die Verantwortlichen für eine Kampagne können auch definiert werden, indem diese in einer Datei aufgelistet werden, die jeden Benutzer, der in der Kampagne nachgewiesen wurde, einem Verantwortlichen zuordnen.

Zu Beginn einer Kampagne sendet BHOLD eine E-Mail-Benachrichtigung an den Kampagnenverantwortlichen und den Kampagnenbesitzer. Anschließend werden regelmäßige Erinnerungen gesendet, um diese beim Verwalten des Fortschritts der Kampagne zu unterstützen. Verantwortliche werden zu einem Kampagnenportal weitergeleitet, auf dem eine Liste der Benutzer angezeigt wird, für die sie zuständig sind sowie die Rollen, die diesen Benutzern zugewiesen sind. Der Verantwortliche kann dann bestätigen, dass er für die aufgelisteten Benutzer zuständig ist und die Zugriffsrechte für jeden Benutzer genehmigen oder verweigern.

Kampagnenbesitzer verwenden das Portal ebenfalls, um den Fortschritt der Kampagne zu überwachen. Außerdem werden Kampagnenaktivitäten protokolliert, sodass Kampagnenbesitzer die Aktionen, die während der Kampagne ausgeführt wurden, analysieren können.

## <a name="analytics"></a>Analysen

Ein wichtiger Aspekt beim Implementieren eines umfassenden, auf Rechten basierenden Zugriffssteuerungssystems (RBAC) ist das Gleichgewicht zwischen dem Erhalten der strikten Zugriffssteuerung und dem Vermeiden von unnötigen (oder schlimmer, unerwarteten) Barrieren für den Zugriff. Der Aufwand zum Erhalten dieses Gleichgewichts führt häufig zu einer Struktur der Zugriffssteuerung, die so komplex ist, dass unerwartete Interaktionen zwischen Richtlinien fast unvermeidlich sind.

Aus diesem Grund ist es wichtig, die Auswirkungen der Zugriffssteuerungsrichtlinien analysieren zu können, bevor diese tatsächlich angewendet werden. Das Analysemodul von BHOLD Suite ermöglicht Ihnen, diese Analyse auszuführen, indem Sie Regeln entwickeln können, die Ihre Richtlinien darstellen. Anschließend werden die Benutzer angezeigt, deren Berechtigungen der Regel entsprechen oder mit dieser in Konflikt stehen. Auf Grundlage dieser Analyse können Sie die Richtlinie anpassen oder Rollen und Berechtigungen ändern, um die Konflikte mit der Richtlinie zu entfernen.

Das BHOLD-Analyseportal ermöglicht es Ihnen, Regelsätze zu erstellen, die aus einer oder mehreren Regeln bestehen, die Sie zum Testen einer bestimmten Richtlinie oder Gruppe von Richtlinien erstellen. Eine Regel besteht aus folgenden Hauptkomponenten:

- Ein Titel und eine Beschreibung, durch die Sie die Regel identifizieren und beschreiben können
- Ein Status, der angibt, ob die Regel überprüft wird oder für die Überprüfung bzw. die Genehmigung bereit ist
- Eine Elementgruppe (z.B. Benutzer oder Berechtigungen), für deren Test die Regel entworfen wurde
- Optional Teilmengenfilter, bei denen es sich um Ausdrücke handelt, die Sie zum Auswählen einer geeigneten Untergruppe des Elements verwenden können, das untersucht werden soll
- Ein oder mehrere Regelfilter, bei denen es sich um Ausdrücke handelt, die die Richtlinie darstellen, die getestet wird

Eine Regel kann folgende Elementgruppen testen:

- -Benutzer
- Organisationseinheiten
- Rollen
- Berechtigungen
- Applications
- Konten

Das folgende Diagramm veranschaulicht eine einfache Regel, die aus je zwei Regeln für Teilmengen und Filter besteht:

![](media/bhold-concepts-guide/rules.png)

Beachten Sie die unterschiedlichen Auswirkungen von einem Fehler eines Teilmengenfilters und einem Fehler eines Regelfilters: Ein Fehler eines Teilmengenfilters entfernt ein Elementobjekt aus den Tests durch nachfolgende Filter, während ein Fehler eines Regelfilters bewirkt, dass das Objekt als nicht kompatibel gemeldet wird. Nur die Objekte, die alle Teilmengenfilter und Regelfilter passieren, werden als kompatibel gemeldet.

Jeder Filter besteht aus einem Typ, einem Operator (der typabhängig ist), einem Schlüssel (des Elements) und einem Wert, auf den der Schlüssel vom Operator getestet wird. Der folgende Filter testet zum Beispiel, ob die Anzahl der Benutzer in der Teilmenge eines Elements 10 überschreitet:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Anzahl von   |
| **Schlüssel:**  | -Benutzer  |
| **Operator**  | >  |
| **Wert:** | 10 |

Es gibt drei Typen von Regelfiltern. Diese verwenden, wie angegeben, für ihren Typ spezifische Operatoren:

- Attribut
  - < and >
  - = and !=
  - **Enthält**
  - **Enthält nicht**
- Anzahl von
  - < and >
  - = and !=
- Restriktiv
  - **„Muss eins besitzen“ und „Muss alle besitzen“**
  - **„Kann keins besitzen“ und „Kann nicht alle besitzen“**
  - **„Kann nur eins besitzen“ und „Kann nur alle besitzen“**
  - **„Besitzt exklusiv eins“ und „Besitzt exklusiv alle“**

> [!Note]
> Restriktive Filter können die angezeigten Operatoren verwenden, um einen Schlüssel auf mehrere Werte zu testen.

Wenn Sie beispielsweise die Implementierung einer Richtlinie für die Trennung von Aufgaben (SoD) testen möchten, die angibt, dass kein Benutzer, der über die Berechtigung für das Anfordern von Zahlungen verfügt, auch über die Berechtigung für das Genehmigen von Zahlungen verfügen darf, können Sie eine Regel wie die folgende erstellen:

|   |  |
|---|--|
|Name:| SoD-Test für Zahlungen|
|Element:| -Benutzer|
|Teilmengenfilter:| Besitzt eine Berechtigung für das Anfordern von Zahlungen|
|Regelfilter: | Kann keine Berechtigung für das Genehmigen von Zahlungen besitzen|

Wenn Sie diese Regel ausführen, zeigt das BHOLD-Analysemodell die Anzahl von Benutzern in der ausgewählten Teilmenge an (die Anzahl von Benutzern mit der Berechtigung für das Anfordern von Zahlungen) sowie die Anzahl von Benutzern, die der Regel entsprechen und die Anzahl von Benutzern, die der Regel nicht entsprechen. Sie können dann die nicht kompatiblen Benutzer anzeigen lassen, sodass Sie die Inkonsistenz korrigieren können.

Zusätzlich zum Anzeigen der Ergebnisse können Sie den Bericht auch als einen durch Trennzeichen getrennten Wert (comma-separated value, CSV) oder als XML-Datei speichern, damit Sie die Ergebnisse später analysieren können. Sie können den resultierenden Bericht auch anpassen, damit zusätzliche Informationen angezeigt werden, durch die Sie die Auswirkungen besser verstehen können. Wenn Sie beispielsweise Benutzer testen, können Sie die Konten der kompatiblen oder nicht kompatiblen Benutzer anzeigen (oder melden), damit Ihnen angezeigt wird, welche Anwendungen beteiligt sind.

Da eine Regel mehrere Filter enthalten kann, können Sie Filter verbinden, um zu testen, ob eine bestimmte Kombination von Bedingungen vorhanden ist. Standardmäßig ist das Ergebnis das Produkt eines booleschen AND-Tests aller Filter. Sie können jedoch auch angeben, dass ein OR-Test der Filterkombinationen ausgeführt wird.

Wenn Ihre Geschäftsrichtlinie beispielsweise erfordert, dass Manager entweder über die Berechtigung zum Ändern von Zahlungen oder die Berechtigung zum Genehmigen von Zahlungen verfügen, können Sie die Kompatibilität mit der Regel testen, indem Sie eine Regel wie die folgende erstellen:


|  |  |
|--|--|
|Name: | Ändern des SoD-Tests für Zahlungen|
|Element: | -Benutzer |
|Teilmengenfilter: | Besitzt einen Rollen-Manager|
| Regelfilter: |Muss eine Berechtigung zum Ändern von Zahlungen besitzen </br> Muss eine Berechtigung zum Genehmigen von Zahlungen besitzen|

Standardmäßig wird jeder Benutzer, der ein Manager ist und über die Berechtigungen zum Ändern und Anfordern von Zahlungen verfügt, als kompatibel gemeldet. Die Richtlinie erfordert jedoch nur, dass ein Manager über eine der Berechtigung verfügt, nicht notwendigerweise über beide. Sie müssen den booleschen OR-Operator mit der Regel verwenden, um die tatsächliche Kompatibilität mit der Richtlinie zu testen. So bestimmen Sie, ob es Manager gibt, die weder über die Berechtigung zum Ändern von Zahlungen noch über die Berechtigung zum Genehmigen von Zahlungen verfügen.

Im Gegensatz zu anderen Operatoren geben die Operatoren **Besitzt exklusiv eins** und **Besitzt exklusiv alle** die Kompatibilität für Objekte an, die andernfalls durch einen Teilmengenfilter ausgeschlossen würden. Wenn Sie zum Beispiel eine Richtlinie testen möchten, laut der alle Manager (und nur Manager) über die Berechtigung zum Genehmigen von Reviews verfügen sollen, können Sie eine Regel wie die folgende erstellen:

|  |  |
|--|--|
|Name: | Test zum Genehmigen von Reviews|
|Element: | -Benutzer|
| Teilmengenfilter: | Besitzt einen Rollen-Manager
|Regelfilter: | Besitzt exklusiv eine Berechtigung zum Genehmigen von Reviews|

Diese Regel meldet Benutzer als kompatibel, die Manager sind und über die Berechtigung zum Genehmigen von Reviews verfügen sowie Benutzer, die keine Manager sind und nicht über die Berechtigung zum Genehmigen von Reviews verfügen. Im Gegensatz dazu werden Manager, die nicht über diese Berechtigung verfügen und Benutzer, die keine Manager sind, aber über diese Berechtigung verfügen, als nicht kompatibel gemeldet.

Wie bereits erwähnt, können Sie Regeln zu einem Regelsatz kombinieren, um das Kategorisieren und Verwalten von Regeln zu vereinfachen, damit diese Ihre geschäftlichen Anforderungen erfüllen.

Sie können auch ein globales Filterset definieren, das auf jede getestete Regel angewendet wird, wenn es aktiviert ist. Wenn Sie beim Testen von Regeln in verschiedenen Regelsätzen häufig eine bestimmte Teilmenge von Berichten ausschließen müssen, können Sie globale Filter angeben, die Sie nach Bedarf aktivieren oder deaktivieren können.

## <a name="reporting"></a>Berichterstellung

Das BHOLD-Berichtmodul ermöglicht es Ihnen, die Informationen im Rollenmodell über eine Vielzahl von Berichten anzuzeigen. Das BHOLD-Berichtmodul bietet eine breite Palette von integrierten Berichten. Außerdem ist ein Assistent enthalten, den Sie zum Erstellen von grundlegenden und erweiterten benutzerdefinierten Berichten verwenden können. Wenn Sie einen Bericht ausführen, können Sie die Ergebnisse sofort anzeigen lassen oder diese in einer Microsoft Excel-Datei (XLSX) speichern. Zum Anzeigen dieser Datei mithilfe von Microsoft Excel 2000, Microsoft Excel 2002 oder Microsoft Excel 2003 können Sie das Microsoft Office Compatibility Pack für Word, Excel und PowerPoint-Dateiformate herunterladen und installieren.


Sie verwenden das BHOLD-Berichtmodul in erster Linie zum Erstellen von Berichten, die auf aktuellen Rolleninformationen basieren. Für das Generieren von Berichten für das Überwachen von Änderungen an Identitätsinformationen verfügt Forefront Identity Manager 2010 R2 über eine Berichterstellungsfunktion für den FIM-Dienst, die in System Center Service Manager-Data Warehouse implementiert ist. Weitere Informationen über die Berichterstellung in FIM finden Sie in der Dokumentation zu Forefront Identity Manager 2010 und Forefront Identity Manager 2010 R2 in der technischen Bibliothek für Forefront Identity Management.

Folgende Kategorien werden von den integrierten Berichten abgedeckt:

- Verwaltung
- Nachweis
- Steuerelemente
- Innere Zugriffsteuerung
- Protokollierung
- Modell
- Statistiken
- Workflow

Sie können Berichte erstellen und diese zu diesen Kategorien hinzufügen, oder Sie können Ihre eigenen Kategorien definieren, in denen Sie benutzerdefinierte und integrierte Berichte ablegen können.

Beim Erstellen eines Berichts führt der Assistent Sie durch das Angeben der folgenden Parameter:

- Identifizieren von Informationen, einschließlich Name, Beschreibung, Kategorie, Nutzung und Zielgruppe
- Felder, die im Bericht angezeigt werden
- Abfragen, die angeben, welche Elemente analysiert werden sollen
- Reihenfolge, in der die Zeilen sortiert werden
- Felder, die verwendet werden, um den Bericht in Abschnitte zu unterteilen
- Filter, um die Elemente einzuschränken, die im Bericht zurückgegeben werden

In jeder Phase des Assistenten können Sie eine Vorschau des bisherigen Berichts anzeigen lassen und diesen speichern, wenn Sie keine zusätzlichen Parameter angeben müssen. Sie können auch zu vorherigen Schritten zurückkehren, um Parameter zu ändern, die Sie zuvor im Assistenten angegeben haben.

## <a name="access-management-connector"></a>Zugriffsverwaltungsconnector

Das Modul des BHOLD Suite-Zugriffsverwaltungsconnectors unterstützt die laufende und die Erstsynchronisierung von Daten in BHOLD. Der Zugriffsverwaltungsconnector arbeitet mit dem FIM-Synchronisierungsdienst, um Daten zwischen der BHOLD Core-Datenbank, der MIM-Metaverse und den Zielanwendungen und -identitätsspeichern zu verschieben.

Frühere Versionen von BHOLD erfordern mehrere MAs, um den Datenfluss zwischen MIM und BHOLD-Zwischendatenbanktabellen zu steuern. In BHOLD Suite SP1 können Sie mit dem Zugriffsverwaltungsconnector Verwaltungs-Agents (Management Agents, MAs) in MIM definieren, die eine direkte Datenübertragung zwischen BHOLD und MIM bereitstellen.

## <a name="mim-integration"></a>MIM-Integration

Ein wichtiges und effektives Feature von Forefront Identity Manager 2010 und Forefront Identity Manager 2010 R2 ist das Self-Service-Portal, in dem Benutzer Ihre Identitäts- und Mitgliedschaftsinformationen anzeigen und verwalten können. Die MIM-Integration erweitert das MIM-Portal mit der Self-Service-Rollenverwaltung. Indem Sie beispielsweise die BHOLD-Features im MIM-Portal verwenden, kann ein Benutzer eine Rollenzuweisung anfordern und aktive Rollen und ausstehende Anforderungen anzeigen. Delegierten Administratoren können zusätzliche Funktionen gewährt werden, z.B. das Anfordern von Rollenzuweisungen für andere Benutzer.

Beachten Sie, dass die BHOLD-Erweiterungen für das MIM-Portal die Self-Service-Rollenverwaltung und -Workflowverwaltung sowie die Berichterstellung unterstützt. Andere BHOLD-Verwaltungsfunktionen und der Nachweis werden von den Webportalen der BHOLD-Module bereitgestellt, die separat vom MIM-Portal gehostet werden.

## <a name="next-steps"></a>Nächste Schritte

- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)
