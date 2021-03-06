---
title: Definieren von privilegierten Rollen für PAM | Microsoft Docs
description: Entscheiden Sie, welche privilegierten Rollen verwaltet werden sollen, und legen Sie die Verwaltungsrichtlinie für jede fest.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f05769a7d1db38ecde200e18e45c6ca29a75b756
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044037"
---
# <a name="define-roles-for-privileged-access-management"></a>Definieren von Rollen für Privileged Access Management

Mit Privileged Access Management können Sie Benutzern privilegierte Rollen zuweisen, die sie nach Bedarf für den Just-in-Time-Zugriff aktivieren können. Diese Rollen werden manuell definiert und in der geschützten Umgebung eingerichtet. Dieser Artikel führt Sie durch den Prozess der Entscheidung, welche Rollen durch PAM verwaltet werden sollen, und wie sie mit entsprechenden Berechtigungen und Einschränkungen definiert werden.

Ein einfaches Verfahren zum Definieren von Rollen für Privileged Access Management ist, alle Informationen in einer Kalkulationstabelle zu kompilieren. Listen Sie die Rollen in den Rollen auf, und verwenden Sie die Spalten, um Berechtigungen und Governanceanforderungen zu identifizieren.

Die Governanceanforderungen variieren je nach vorhandener Identität und bestehenden Zugriffsrichtlinien oder Complianceanforderungen. Folgende Parameter müssen u.a. für jede Rolle identifiziert werden:

- Der Besitzer der Rolle.
- Die Kandidatenbenutzer, die sich in der Rolle befinden können.
- Die Steuerelemente für Authentifizierung, Genehmigung oder Benachrichtigung, die der Verwendung der Rolle zugeordnet werden sollten.

Die Rollenberechtigungen hängen von den verwalteten Anwendungen ab. In diesem Artikel wird Active Directory als Beispielanwendung verwendet. Die Berechtigungen sind hier in zwei Kategorien unterteilt:

- Berechtigungen, die zum Verwalten des Active Directory-Diensts erforderlich sind (z. B. Konfigurieren der Replikationstopologie)

- Berechtigungen, die zum Verwalten der in Active Directory gespeicherten Daten erforderlich sind (z. B. Erstellen von Benutzern und Gruppen)

## <a name="identify-roles"></a>Identifizieren von Rollen

Starten Sie mit dem Identifizieren aller Rollen, die Sie möglicherweise mit PAM verwalten möchten. In der Kalkulationstabelle hat jede mögliche Rolle eine eigene Zeile.

Berücksichtigen Sie jede Anwendung im Bereich der Verwaltung, um die entsprechenden Rollen zu suchen:

- Befindet sich die Anwendung auf [Ebene 0, 1 oder 2](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Welche Berechtigungen beeinflussen die Vertraulichkeit, Integrität oder Verfügbarkeit der Anwendung?
- Bestehen für die Anwendung Abhängigkeiten mit anderen Komponenten des Systems? Bestehen z.B. Abhängigkeiten mit Datenbanken, Netzwerkfunktionen, Sicherheitsinfrastrukturen, Virtualisierung oder der Hostingplattform?

Bestimmen Sie, wie diese zu berücksichtigenden Aspekte der App gruppiert werden sollen. Sie wünschen Rollen, die klare Grenzen aufweisen, und nur genügend Berechtigungen zum Ausführen von Verwaltungsaufgaben innerhalb der App bieten.

Sie möchten Rollen immer nach dem Prinzip der minimalen Zuweisung von Berechtigungen entwerfen. Diese Zuweisung kann auf den aktuellen (oder geplanten) organisatorischen Aufgaben für Benutzer beruhen und umfasst die für die Aufgaben eines Benutzers erforderlichen Berechtigungen. Dies kann zudem die Berechtigungen umfassen, die Vorgänge erleichtern, ohne dass Risiken entstehen.

Weitere Überlegungen zum Definieren der Berechtigungen für eine Rolle:

- Wie viele Personen arbeiten in einer bestimmten Rolle? Wenn nicht mindestens 2, dann ist sie möglicherweise zu eng definiert, um nützlich zu sein, oder Sie haben Aufgaben einer bestimmten Person definiert.

- Wie viele Rollen übernimmt eine Person? Sind Benutzer in der Lage, die richtige Rolle für ihre Aufgabe auszuwählen?

- Sind die Anzahl der Benutzer und ihre Interaktion mit den Anwendungen kompatibel mit Privileged Access Management?

- Ist es möglich, Administration und Überwachung zu trennen, sodass ein Benutzer in einer Administratorrolle nicht die Überwachungsdatensätze seiner Aktionen löschen kann?

## <a name="establish-role-governance-requirements"></a>Einrichten von Governanceanforderungen für Rollen

Füllen Sie beim Bestimmen der möglichen Rollen die Tabelle aus. Erstellen Sie Spalten für die Anforderungen, die für Ihre Organisation relevant sind. Zu den zu berücksichtigenden Anforderungen zählen:

- Wer ist der Rollenbesitzer, der für die weitere Definition der Rolle zuständig ist, für die Rolle Berechtigungen auswählt und die Governanceeinstellungen verwaltet?

- Wer sind die Rolleninhaber (Benutzer), die die Aufgaben der Rolle ausführen?

- Welche Zugriffsmethode (im nächsten Abschnitt erläutert) eignet sich für die Rolleninhaber?

- Ist eine manuelle Genehmigung durch einen Rollenbesitzer erforderlich, wenn ein Benutzer seine Rolle aktiviert?

- Ist eine Benachrichtigung erforderlich, wenn ein Benutzer seine Rolle aktiviert?

- Soll bei Verwendung der Rolle zur Nachverfolgung eine Warnung oder Benachrichtigung in einem SIEM-System generiert werden?

- Ist die Einschränkung notwendig, dass Benutzer, die die Rolle aktivieren, sich nur an Computern anmelden können, auf denen der Zugriff zum Ausführen der Aufgaben der Rolle erforderlich ist und auf denen eine ausreichende Überprüfung des Hosts erfolgt, sodass es möglich ist, die Berechtigungen und Anmeldeinformationen vor Missbrauch zu schützen?

- Ist es erforderlich, eine dedizierte Administratorarbeitsstation für die Rolleninhaber bereitzustellen?

- Welche Anwendungsberechtigungen (siehe folgende Beispielaufstellung für AD) sind der Rolle zugeordnet?

## <a name="select-an-access-method"></a>Auswählen einer Zugriffsmethode

In einem PAM-System können mehrere Rollen mit den gleichen Berechtigungen existieren. Dies kann vorkommen, wenn in verschiedenen Benutzergruppen unterschiedliche Governanceanforderungen in Bezug auf den Zugriff vorliegen. Beispielsweise können in einer Organisation andere Richtlinien für Vollzeitmitarbeiter gelten als für externe IT-Mitarbeiter einer anderen Organisation.

In einigen Fällen kann ein Benutzer dauerhaft einer Rolle zugewiesen sein. In diesem Fall müssen diese Benutzer keine Rollenzuweisung anfordern oder aktivieren. Beispiele für Szenarios mit dauerhafter Rollenzuweisung:

- Ein verwaltetes Dienstkonto in einer vorhandenen Gesamtstruktur

- Ein Benutzerkonto in der vorhandenen Gesamtstruktur mit Anmeldeinformationen, die außerhalb von PAM verwaltet werden. Hierbei kann es sich um ein Notfallkonto handeln. Das Notfallkonto benötigt möglicherweise eine Rolle wie z.B. „Domänen-/Domänencontrollerverwaltung“, um Probleme mit der Vertrauensstellung und Domänencontrollerintegrität zu beheben. Als Notfallkonto wird diesem die Rolle mit einem physisch gesicherten Kennwort dauerhaft zugewiesen.

- Ein Benutzerkonto in der administrativen Gesamtstruktur, das sich mit einem Kennwort authentifiziert. Hierbei kann es sich um einen Benutzer handeln, der dauerhaft rund um die Uhr Administratorrechte benötigt und sich über ein Gerät anmeldet, das keine starke Authentifizierung unterstützt.

- Ein Benutzerkonto in der administrativen Gesamtstruktur mit einer Smartcard oder virtuellen Smartcard (z. B. ein Konto mit einer Offline-Smartcard für seltene Wartungsaufgaben)

Für Organisationen, die in Bezug auf den möglichen Diebstahl oder Missbrauch von Anmeldeinformationen Bedenken haben, enthält die Anleitung [Verwenden von Azure MFA zur Aktivierung](use-azure-mfa-for-activation.md) Anweisungen dazu, wie MIM so konfiguriert werden kann, dass zum Zeitpunkt der Rollenaktivierung eine zusätzliche Out-of-Band-Prüfung erforderlich ist.

## <a name="delegate-active-directory-permissions"></a>Delegieren von Berechtigungen für Active Directory

Windows Server erstellt automatisch Standardgruppen, wie z. B. „Domänen-Admins“, wenn neue Domänen erstellt werden. Diese Gruppen vereinfachen erste Schritte und sind möglicherweise für kleinere Organisationen eignet. Größere Organisationen oder Organisationen, die mehr Isolation von Administratorrechten benötigen, sollten diese Gruppen leeren und durch Gruppen ersetzen, die differenziertere Berechtigungen bereitstellen.

Eine Einschränkung der Gruppe „Domänen-Admins“ besteht darin, dass Mitglieder einer externen Domäne nicht zulässig sind. Eine weitere Einschränkung ist, dass Berechtigungen für drei separate Funktionen gewährt werden:

- Verwalten des Active Directory-Diensts selbst
- Verwalten der in Active Directory gespeicherten Daten
- Ermöglichen der Remoteanmeldung bei Computern, die in die Domäne eingebunden sind.

Erstellen Sie anstatt der Standardgruppen wie „Domänenadministratoren“ neue Sicherheitsgruppen, die nur die erforderlichen Berechtigungen bereitstellen. Dann sollten Sie MIM verwenden, um Administratorkonten mit diesen Gruppenmitgliedschaften dynamisch bereitzustellen.

### <a name="service-management-permissions"></a>Dienstverwaltungsberechtigungen

Die folgende Tabelle enthält Beispiele für Berechtigungen, die relevant für Rollen zur Verwaltung von AD sind.

| Rolle | Description |
| ---- | ---- |
| Wartung Domäne/DC | Die Mitgliedschaft in der Gruppe „Domäne\Administratoren“ ermöglicht die Problembehandlung und die Änderung des Betriebssystems auf dem Domänencontroller. Vorgänge wie das Höherstufen eines neuen Domänencontrollers in eine vorhandene Domäne in der Gesamtstruktur und das Delegieren von AD-Rollen.
|Virtuelle DCs verwalten | Verwalten der virtuellen Computer (VMs) des Domänencontrollers (DC) mithilfe der Virtualization Management-Software. Diese Berechtigung kann über Vollzugriff auf alle virtuellen Computer im Management-Tool oder mit der Funktion der rollenbasierten Zugriffskontrolle (RBAC) gewährt werden. |
| Schema erweitern | Verwalten des Schemas, z. B. Hinzufügen neuer Objektdefinitionen, Ändern von Berechtigungen für Schemaobjekte und Ändern von Schemastandardberechtigungen für Objekttypen |
| Active Directory-Datenbank sichern | Erstellen einer Sicherungskopie der gesamten Active Directory-Datenbank, einschließlich aller geheimen Schlüssel für den Domänencontroller und die Domäne. |
| Vertrauensstellungen und Funktionsebenen verwalten | Erstellen und Löschen von Vertrauensstellungen mit externen Domänen und Gesamtstrukturen. |
| Verwalten von Standorten, Subnetzen und Replikation | Verwalten der Active Directory-Replikationstopologieobjekte, z. B. Ändern von Standorten, Subnetzen und Standortverknüpfungsobjekten sowie Initiieren von Replikationsvorgängen |
| GPOs verwalten | Erstellen, Löschen und Ändern von Gruppenrichtlinienobjekten in der Domäne |
| Zonen verwalten | Erstellen, Löschen und Ändern von DNS-Zonen und Objekten in Active Directory |
| OEs der Ebene 0 ändern | Ändern der Organisationseinheiten der Ebene 0 und der darin enthaltenen Objekte in Active Directory |

### <a name="data-management-permissions"></a>Datenverwaltungsberechtigungen

Die folgende Tabelle enthält Beispiele für Berechtigungen, die relevant für Rollen zur Verwaltung oder Verwendung der in AD gespeicherten Daten sind.

| Rolle | Description |
| ---- | ---- |
| Admin-OE der Ebene 1 ändern                 | Ändern von Organisationseinheiten, die Admin-Objekte der Ebene 1 enthalten, in Active Directory |
| Admin-OE der Ebene 2 ändern                 | Ändern von Organisationseinheiten, die Admin-Objekte der Ebene 2 enthalten, in Active Directory |
| Kontenverwaltung: erstellen/löschen/verschieben | Ändern von Standardbenutzerkonten                                      |
| Kontenverwaltung: zurücksetzen/entsperren       | Zurücksetzen von Kennwörtern und Entsperren von Konten                                  |
| Sicherheitsgruppe: erstellen/ändern          | Erstellen und Ändern von Sicherheitsgruppen in Active Directory              |
| Sicherheitsgruppe: löschen                 | Löschen von Sicherheitsgruppen in Active Directory                         |
| GPO-Verwaltung                         | Verwalten aller Gruppenrichtlinienobjekte in der Domäne/Gesamtstruktur, die sich nicht auf Server der Ebene 0 auswirken             |
| PC verknüpfen/lokaler Administrator                    | Lokale Administratorrechte für alle Arbeitsstationen                               |
| Server verknüpfen/lokaler Administrator                   | Lokale Administratorrechte für alle Server                                    |

## <a name="example-role-definitions"></a>Beispiele für Rollendefinitionen

Die Auswahl der Rollendefinitionen richtet sich nach der Ebene der Server, die verwaltet werden. Auch die Auswahl der verwalteten Anwendungen spielt eine Rolle. Anwendungen wie Exchange oder Unternehmensprodukte von Drittanbietern wie SAP verfügen häufig über ihre eigenen zusätzlichen Rollendefinitionen für die delegierte Verwaltung.

Die folgenden Abschnitte enthalten Beispiele für typische Enterprise-Szenarios.

### <a name="tier-0---administrative-forest"></a>Ebene 0 – administrative Gesamtstruktur

Die für Konten in der geschützten Umgebung geeigneten Rollen können Folgendes umfassen:

- Notfallzugriff auf die administrative Gesamtstruktur
- „Rote Karte“-Administratoren: Benutzer, die Administratoren der administrativen Gesamtstruktur sind
- Benutzer, die Administratoren der Produktionsgesamtstruktur sind
- Benutzer, an die eingeschränkte Administratorrechte für Anwendungen in der Produktionsgesamtstruktur delegiert werden

### <a name="tier-0---enterprise-production-forest"></a>Ebene 0 – Enterprise-Produktionsgesamtstruktur

Die zur Verwaltung von Konten und Ressourcen der Produktionsgesamtstruktur der Ebene 0 geeigneten Rollen können Folgendes umfassen:

- Notfallzugriff auf die Produktionsgesamtstruktur
- Administratoren für Gruppenrichtlinien
- DNS-Administratoren
- PKI-Administratoren
- Administratoren für AD-Topologie und -Replikation
- Administratoren für Virtualisierung für Server der Ebene 0
- Administratoren für Speicher
- Administratoren für Antimalware für Server der Ebene 0
- SCCM-Administratoren für SCCM der Ebene 0
- SCOM-Administratoren für SCOM der Ebene 0
- Administratoren für Backup für Ebene 0
- Benutzer von Out-of-Band- und Baseboard-Verwaltungscontrollern (für KVM- oder Lights-Out-Management), die mit Hosts der Ebene 0 verbunden sind

### <a name="tier-1"></a>Ebene 1

Rollen zur Verwaltung und Sicherung von Servern auf Ebene 1 können Folgendes umfassen:

- Serverwartung
- Administratoren für Virtualisierung für Server der Ebene 1
- Konto für Sicherheitsüberprüfung
- Administratoren für Antimalware für Server der Ebene 1
- SCCM-Administratoren für SCCM der Ebene 1
- SCOM-Administratoren für SCOM der Ebene 1
- Administratoren für Backup für Ebene 1
- Benutzer von Out-of-Band- und Baseboard-Verwaltungscontrollern (für KVM- oder Lights-Out-Management), für Hosts der Ebene 1

Zudem können Rollen zur Verwaltung von Enterprise-Anwendungen auf Ebene 1 Folgendes umfassen:

- DHCP-Administratoren
- Exchange-Administratoren
- Skype for Business-Administratoren
- SharePoint-Farmadministratoren
- Administratoren eines Cloud-Diensts, z. B. einer Website oder eines öffentlichen DNS eines Unternehmens
- Administratoren für HCM- Finanz- oder juristische Systeme

### <a name="tier-2"></a>Ebene 2

Rollen für Benutzer ohne Administratorrechte und zur Computerverwaltung können Folgendes umfassen:

- Kontoadministratoren
- Helpdesk
- Administratoren für Sicherheitsgruppen
- Deskside-Support für Arbeitsstationen

## <a name="next-steps"></a>Nächste Schritte

- [Schützen des privilegierten Zugriffs – Referenzmaterial](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Verwenden von Azure MFA zur Aktivierung](use-azure-mfa-for-activation.md)
