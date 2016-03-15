---
Titel: Definieren von Rollen für die privilegierte Zugriffsverwaltung
MS.Custom: Na
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Service: Active Directory
MS.Suite: Na
MS.Technology:
  - Active-Directory-Domänendienste
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 1a368e8e-68e1-4f40-a279-916e605581bc
Autor: Kgremban
---
# Definieren von Rollen für Privileged Access Management
## Übersicht

Zum Definieren der für Privileged Access Management erforderlichen Rollen wird üblicherweise eine Tabelle erstellt und geprüft, in der die Rollen aufgeführt sind und die Governance-Anforderungen und die für die einzelnen Rollen erforderlichen Berechtigungen identifiziert werden.

Die Governance-Anforderungen variieren je nach den vorhandenen Identitäts- und Zugriffsrichtlinien oder Kompatibilitätsanforderungen, denen die Privileged Access Management-Steuerelemente entsprechen sollen. Die für jede Rolle zu identifizierenden Parameter umfassen beispielsweise die Identifizierung des Besitzers der Rolle und der Kandidatenbenutzer, denen diese Rolle zugewiesen werden kann, sowie die Festlegung, welche Steuerelemente zur Authentifizierung, Genehmigung oder Benachrichtigung mit der Verwendung der Rolle verknüpft werden sollen.

Die Rollenberechtigungen hängen von den verwalteten Anwendungen ab. In diesem Artikel wird Active Directory als Beispielanwendung verwendet. Die Berechtigungen sind hier in zwei Kategorien unterteilt:

- Berechtigungen, die zum Verwalten des Active Directory-Diensts erforderlich sind (z. B. Konfigurieren der Replikationstopologie)

- Berechtigungen, die zum Verwalten der in Active Directory gespeicherten Daten erforderlich sind (z. B. Erstellen von Benutzern und Gruppen)

## Überlegungen zur Auswahl der Rollen

Erstellen Sie zunächst eine Registerkarte in einer Tabelle, und listen Sie jede mögliche Rolle in einer Zeile auf. Zur Festlegung der entsprechenden Rollen orientieren Sie sich an den zu verwaltenden Anwendungen (befinden sie sich auf Ebene 0, Ebene 1 oder Ebene 2?) und den Berechtigungen, die sich auf die Vertraulichkeit, Integrität oder Verfügbarkeit der jeweiligen Anwendung auswirken. Berücksichtigen Sie auch die Abhängigkeiten, die die Anwendung möglicherweise in Bezug auf andere Komponenten des Systems aufweist, z. B. auf Datenbanken, die Netzwerk- oder Sicherheitsinfrastruktur oder die Virtualisierungs- oder Hostingplattform. Legen Sie dann fest, wie diese Berechtigungen gruppiert werden, sodass Administratoren einfach die entsprechende Rolle auswählen können, ohne versehentlich zu viele Berechtigungen zu erhalten.

Zielsetzung der Rollenmodellierung ist die Zuweisung der geringsten Berechtigungen. Diese Zuweisung kann auf den aktuellen (oder geplanten) organisatorischen Aufgaben für Benutzer beruhen und umfasst die für die Aufgaben eines Benutzers erforderlichen Berechtigungen. Dies kann zudem die Berechtigungen umfassen, die Vorgänge erleichtern, ohne dass Risiken entstehen.

Weitere Überlegungen zum Definieren der Berechtigungen für eine Rolle:

- Wie viele Personen arbeiten in einer bestimmten Rolle? Wenn es sich nicht um mindestens zwei Personen handelt, ist es möglicherweise voreilig, eine Rolle zu definieren, wenn kein Benutzer diese Rolle übernimmt oder sie lediglich durch die Aufgaben einer bestimmten Person definiert ist.

- Wie viele Rollen übernimmt eine Person? Sind Benutzer in der Lage, die richtige Rolle für ihre Aufgabe auszuwählen?

- Sind die Anzahl der Benutzer und ihre Interaktion mit den Anwendungen kompatibel mit Privileged Access Management?

- Ist es möglich, Administration und Überwachung zu trennen, sodass ein Benutzer in einer Administratorrolle nicht die Überwachungsdatensätze seiner Aktionen löschen kann?

Am Ende dieses Abschnitts ist eine beispielhafte Aufstellung von Rollen aufgeführt.

## Governance-Anforderungen für Rollen

Füllen Sie beim Bestimmen der möglichen Rollen die Tabelle aus. Erstellen Sie Spalten für die Anforderungen, die für Ihre Organisation relevant sind, und geben Sie für jede Rolle die Auswahlmöglichkeiten an:

- Wer ist Besitzer der Rolle, die verantwortlich für die weiteren Definition der Rolle Berechtigungen auswählen und die Governance-Einstellungen (ändern Sie so wie berücksichtigt werden zugegriffen, verwendet, usw.) für die Rolle warten?

- Wer sind die Rolleninhaber (Benutzer), die die Aufgaben der Rolle ausführen?

- Welche Zugriffsmethode (im nächsten Abschnitt erläutert) eignet sich für die Rolleninhaber?

- Ist eine manuelle Genehmigung durch einen Rollenbesitzer erforderlich, wenn ein Benutzer seine Rolle aktiviert?

- Ist eine Benachrichtigung erforderlich, wenn ein Benutzer seine Rolle aktiviert?

- Soll bei Verwendung der Rolle zur Nachverfolgung eine Warnung oder Benachrichtigung in einem SIEM-System generiert werden?

- Ist die Einschränkung notwendig, dass Benutzer, die die Rolle aktivieren, sich nur an Computern anmelden können, auf denen der Zugriff zum Ausführen der Aufgaben der Rolle erforderlich ist und auf denen eine ausreichende Überprüfung des Hosts erfolgt, sodass es möglich ist, die Berechtigungen und Anmeldeinformationen vor Missbrauch zu schützen?

- Ist es erforderlich, eine dedizierte Administratorarbeitsstation für die Rolleninhaber bereitzustellen?

- Welche Anwendungsberechtigungen (siehe Beispielaufstellung für AD) sind der Rolle zugeordnet?

## Auswählen einer Zugriffsmethode

Möglicherweise sind in einem Privileged Access Management-System mehrere Rollen vorhanden, denen die gleichen Berechtigungen zugewiesen sind, wenn für verschiedene Benutzergruppen unterschiedliche Governance-Anforderungen für den Zugriff vorliegen. Beispielsweise können in einer Organisation andere Richtlinien für interne Vollzeitmitarbeiter gelten, die als Administratoren fungieren, als für externe IT-Mitarbeiter einer anderen Organisation, die als Administratoren fungieren.

In einigen Fällen kann ein Benutzer dauerhaft einer Rolle zugewiesen sein und daher keine Rollenzuweisung anfordern oder aktivieren müssen. Beispiele für Szenarios mit dauerhafter Rollenzuweisung:

- Ein verwaltetes Dienstkonto in einer vorhandenen Gesamtstruktur

- Ein Benutzerkonto in der vorhandenen Gesamtstruktur, dessen Anmeldeinformationen außerhalb von PAM verwaltet werden (z. B. ein Notfallkonto („Break-Glass-Konto“), dem eine Rolle wie „Wartung Domäne/DC“ für die Behebung von Vertrauensstellungs- und DC-Integritätsproblemen mit einem physisch gesicherten Kennwort dauerhaft zugewiesen ist)

- Ein Benutzerkonto in der administrativen Gesamtstruktur, bei dem die Authentifizierung mit einem Kennwort erfolgt (z. B. ein Benutzer, der Administratorberechtigungen rund um die Uhr benötigt und sich über ein Gerät anmeldet, das keine strenge Authentifizierung unterstützt)

- Ein Benutzerkonto in der administrativen Gesamtstruktur mit einer Smartcard oder virtuellen Smartcard (z. B. ein Konto mit einer Offline-Smartcard für seltene Wartungsaufgaben)

MIM ermöglicht dynamische rollenbasierte Zuweisungen, bei denen die Berechtigungen einem Benutzer mit einem Workflow- und einem optionalen Genehmigungsprozess temporär zugewiesen werden. Dies kann für ein Benutzerkonto in der administrativen Gesamtstruktur gelten, bei dem die Authentifizierung mit einem Kennwort erfolgt, oder für ein Benutzerkonto, bei dem die Authentifizierung mit einer Smartcard oder einer virtuellen Smartcard erfolgt. Für Organisationen, die das Potenzial für den Diebstahl oder den Missbrauch, sorgen die [mithilfe von Azure MFA für die Aktivierung](use-azure-mfa-for-activation.md) Handbuch enthält Anweisungen zum Konfigurieren von MIM eine zusätzliche Out-Band-Kontrollkästchen zum Zeitpunkt der rollenaktivierung erforderlich ist.

## Modellieren delegierter Berechtigungen für Active Directory

Wenn eine neue Domäne in Active Directory erstellt wird, erstellt Windows Server automatisch bekannte Gruppen wie z. B. „Domänen-Admins“. Diese Gruppen erleichtern den Einstieg und mögen sich in unveränderter Form für kleinere Organisationen eignen. In größeren Organisationen oder in Organisationen, die eine größere Isolierung von Administratorrechten erfordern, besteht eine Zielsetzung für Privileged Access Management-Projekte jedoch darin, Gruppen wie „Domänen-Admins“ zu entleeren und durch zusätzliche Gruppen zu ersetzen, die differenzierte Berechtigungen umfassen.

Ein Nachteil der Gruppe „Domänen-Admins“ besteht darin, dass sie Berechtigungen für drei separate Funktionen gewährt: Verwalten des Active Directory-Diensts, Verwalten der in Active Directory gespeicherten Daten und Aktivieren der Remoteanmeldung an mit Domänen verbundenen Computern. Eine weitere Einschränkung der Gruppe „Domänen-Admins“ besteht darin, dass Mitglieder einer externen Domäne nicht zulässig sind. Stattdessen kann eine Organisation beispielsweise neue Sicherheitsgruppen erstellen, die nur die erforderlichen Berechtigungen umfassen, und mit MIM dynamisch Administratorkonten mit diesen Gruppenmitgliedschaften bereitstellen.

### Beispiele für Berechtigungen zur Verwaltung des Active Directory-Diensts

Die folgende Tabelle enthält Beispiele für Berechtigungen, die relevant für Rollen zur Verwaltung von AD sind.

| Role-Eigenschaft | Beschreibung |
| ---- | ---- |
| Wartung Domäne/DC | Mitgliedschaft in der Gruppe „Domäne\Administratoren“ mit folgenden Berechtigungen: Problembehandlung und Ändern des Domänencontroller-Betriebssystems, Heraufstufen eines neuen Domänencontrollers in einer vorhandenen Domäne in der Gesamtstruktur und Delegierung der Active Directory-Rollen.
|Virtuelle DCs verwalten | Verwalten der virtuellen Computer (VMs) des Domänencontrollers (DC) mithilfe der Virtualization Management-Software. Diese Berechtigung kann über Vollzugriff auf alle virtuellen Computer im Management-Tool oder mit der Funktion der rollenbasierten Zugriffskontrolle (RBAC) gewährt werden. |
| Schema erweitern | Verwalten des Schemas, z. B. Erweitern des Schemas durch Einfügen neuer Objektdefinitionen, Ändern von Berechtigungen für Schemaobjekte und Ändern von Schemastandardberechtigungen für Objekttypen |
| Active Directory-Datenbank sichern | Erstellen einer Sicherungskopie der gesamten Active Directory-Datenbank, einschließlich aller geheimen Schlüssel für den Domänencontroller und die Domäne. |
| Vertrauensstellungen und Funktionsebenen verwalten | Erstellen und Löschen von Vertrauensstellungen mit externen Domänen und Gesamtstrukturen. |
| Standorte, Subnetze und Replikation verwalten | Verwalten der Active Directory-Replikationstopologieobjekte, z. B. Ändern von Standorten, Subnetzen und Standortverknüpfungsobjekten sowie Initiieren von Replikationsvorgängen |
| GPOs verwalten | Erstellen, Löschen und Ändern von Gruppenrichtlinienobjekten in der Domäne |
| Zonen verwalten | Erstellen, Löschen und Ändern von DNS-Zonen und Objekten in Active Directory |
| OEs der Ebene 0 ändern | Ändern der Organisationseinheiten der Ebene 0 und der darin enthaltenen Objekte in Active Directory |

### Beispiele für Berechtigungen zur Verwaltung von Active Directory-Daten

Die folgende Tabelle enthält Beispiele für Berechtigungen, die relevant für Rollen zur Verwaltung oder Verwendung der in AD gespeicherten Daten sind.

| Role-Eigenschaft | Beschreibung |
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

## Beispiele für Rollendefinitionen

Die Auswahl der Rollendefinitionen hängt von der Ebene der Server ab, die über die privilegierten Konten verwaltet werden. Sie hängt außerdem von der Auswahl der verwalteten Anwendungen ab, da Anwendungen wie Exchange oder Enterprise-Produkte von Drittanbietern wie SAP häufig über ihre eigenen zusätzlichen Rollendefinitionen für die delegierte Administration verfügen.

Die folgenden Abschnitte enthalten Beispiele für typische Enterprise-Szenarios.

### Ebene 0 – administrative Gesamtstruktur

Die für Konten in der geschützten Umgebung geeigneten Rollen können Folgendes umfassen:

- Notfallzugriff auf die administrative Gesamtstruktur
- „Rote Karte“-Administratoren: Benutzer, die Administratoren der administrativen Gesamtstruktur sind
- Benutzer, die Administratoren der Produktionsgesamtstruktur sind
- Benutzer, an die eingeschränkte Administratorrechte für Anwendungen in der Produktionsgesamtstruktur delegiert werden

### Ebene 0 – Enterprise-Produktionsgesamtstruktur

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

### Ebene 1

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

### Ebene 2

Rollen für Benutzer ohne Administratorrechte und zur Computerverwaltung können Folgendes umfassen:

- Kontoadministratoren
- Helpdesk
- Administratoren für Sicherheitsgruppen
- Deskside-Support für Arbeitsstationen
<!--HONumber=Mar16_HO1-->
