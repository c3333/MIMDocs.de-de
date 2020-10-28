---
title: Microsoft Identity Manager 2016 SP1-Terminologie | Microsoft-Dokumentation
description: Umfassende Liste der Begriffe, auf die in Microsoft Identity Manager 2016 SP1 verwiesen wird.
keywords: Begriff
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/28/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: afe167cdcd6ca548ef34e802f5606bee6ba5b31e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760896"
---
# <a name="microsoft-identity-manager-2016-sp1-terminology"></a>Microsoft Identity Manager 2016 SP1-Terminologie

Dieses Dokument ist eine umfassende Liste der Begriffe, auf die in Microsoft Identity Manager 2016 SP1 verwiesen wird.

## <a name="a"></a>Ein

**Active Directory Gruppen Validierung** : eine in MIM 2016 implementierte Prozedur, die die Eindeutigkeit des Konto namens einer Gruppe innerhalb einer in Active Directory gespeicherten Domäne sicherstellt.

**Aktions Workflow** : ein Workflow, der eine Aktion ausführt. Dies umfasst das Senden einer Benachrichtigungs-e-Mail und das Übertragen von Änderungen an die MIM

**Aktivität** : eine Workflow Aktivität ist der Grundbaustein von Windows Workflow Foundation (WF)-Workflows. Sie enthält die Logik, die beim Erstellen und Ausführen von Workflows zur Entwurfszeit und zur Laufzeit initiiert wird.

**Aktivitätsassembly** : A. DLL oder ein. EXE-Datei, die eine .NET-Assembly enthält, die die Logik für eine Workflow Aktivität implementiert.

**Anker** : ein oder mehrere eindeutige Attribute eines Objekt Typs, der sich nicht ändert und ein Objekt in der verbundenen Datenquelle darstellt, mit dem das Verbindungsraum Objekt verknüpft ist (z. b. eine Mitarbeiternummer oder eine Benutzer-ID).

**Genehmigung** : eine Genehmigung ist ein Workflow Entscheidungspunkt, der zum Abrufen der Autorisierung durch eine Person verwendet werden kann, bevor der Workflow fortgesetzt wird.

**Genehmigungs-e-Mail** : Wenn für eine Anforderung vor dem Commit eine Genehmigung erforderlich ist, wird eine Genehmigungs-e-Mail an identifizierte genehmigende Personen gesendet.

**Genehmigungs Anforderung** : eine Anforderung, die eine Genehmigung erfordert. Beispielsweise eine e-Mail-Nachricht, die von MIM 2016 als Teil der Verarbeitung einer Genehmigungs Aktivität von MIM an eine genehmigende Person gesendet wurde.

**Genehmigungs Antwort** : eine Antwort auf eine Genehmigungs Anforderung. Sie enthält Informationen darüber, ob die Anforderung genehmigt wurde oder nicht. Beispielsweise eine e-Mail-Nachricht, die vom MIM-Add-in für Outlook als Antwort auf eine Genehmigungs Anforderung gesendet wird.

**Genehmigungs Suchordner** : die Suchordner, die vom MIM-Add-in für Outlook erstellt wurden, mit dem der Benutzer ausstehende und abgeschlossene Genehmigungen und Genehmigungs Anforderungs Updates anzeigen kann.

**Genehmigungs Schwellenwert** : die Anzahl der positiven Genehmigungs Antwort Nachrichten, die erforderlich sind, um eine Anforderung für die Fortsetzung der Verarbeitung zuzulassen.

genehmigende **Person: die** Person, die eine Genehmigung für die Anforderung erhält, mit der nächsten Phase fortzufahren. Sie erhalten Genehmigungs Anforderungs Nachrichten, wenn das MIM-Add-in für Outlook verwendet wird. Siehe auch den Eintrag für "Ausweitung genehmigerin".

**Attribut Fluss** : Hiermit wird die Richtung definiert, in der Attributwerte zwischen dem MIM-Dienst und anderen externen Systemen fließen.

**Authentifizierungs Aktivität** : eine Workflow Aktivität, die die Identität eines Benutzers überprüft. Beispielsweise das Kennwort für die Kenn Wort Zurücksetzung und die Smartcard-Authentifizierung. Siehe auch den Eintrag für "QA Gate" und "Lockout Gate".

**Authentifizierungs Aufforderung** : ein Dialogfeld, in dem der Benutzer eine Antwort zum Authentifizieren bei MIM 2016 angeben muss. Beispielsweise Fragen, die Benutzer beantworten müssen, um Ihre Kenn Wörter zurückzusetzen.

**Authentifizierungs Aufforderungs Aktivität** : eine Windows Workflow Foundation Aktivität, die verwendet wird, um eine Herausforderung zu konfigurieren, die für einen Benutzer zur Authentifizierung bei MIM 2016 ausgegeben wird.

**Autorisierungs Workflow** : ein Workflow mit Aktivitäten, die abgeschlossen werden müssen, bevor für die Anforderung ein Commit an die Datenbank ausgeführt wird. Beispiele sind die Datenvalidierung und-Genehmigung.
<br/>

## <a name="c"></a>C

**Registrierungs Attribut löschen** : mit diesem Attribut wird die einem Authentifizierungs Workflow zugeordnete Registrierung gelöscht. Beispielsweise werden in einer Frage-und Antwort Aufforderung Antworten in MIM 2016 in Form von Registrierungsdaten gespeichert. Wenn das Kontrollkästchen Registrierung löschen aktiviert ist und ein Workflow gespeichert wird, werden die Registrierungsdaten gelöscht, sodass sich die Benutzer erneut registrieren müssen.

**berechnetes Element (oder Member)** : ein Schreib geschützter Satz von Ressourcen, der aus der Kombination von manuell verwalteten Membern und einem Filter berechnet wird.

**Connector** : ein Objekt im Connector-Bereich, das ein Objekt in einer verbundenen Datenquelle darstellt und aktuell mit einem Objekt im Metaverse mithilfe von vordefinierten Regeln verknüpft ist. Das Metaverzeichnis verwendet Connector-Objekte, um Attributwerte zwischen einer verbundenen Datenquelle und dem Metaverse zu synchronisieren.

**connectorfilter** : eine Regel, die verwendet wird, um zu verhindern, dass connectorleerobjekte mit Metaverse-Objekten verknüpft werden.

**Connector** -Bereich: ein Stagingbereich, der Darstellungen ausgewählter Objekte und Attribute in einer verbundenen Datenquelle enthält. Ein Connector Space-Objekt ist ein Objekt im Connector-Bereich, das entweder durch einen Datenimport aus der verbundenen Datenquelle oder durch die Verwendung von Regeln in MIM zum Erstellen neuer Objekte in unterschiedlichen verbundenen Datenquellen erstellt wird. Diese Objekte enthalten Attributwerte, die von entsprechenden Objekten in der verbundenen Datenquelle importiert oder exportiert werden können.

**count XPath** : ein XPath-Ausdruck, der einen numerischen Wert zurückgibt, der innerhalb von Klammern nach dem anzeigen amen der Ressource gerendert werden soll.

**Kriterienbasiertes Mitglied** : ein Schreib geschützter Satz von Ressourcen, der aus der Kombination von statischen Gruppen Membern und einem Filter berechnet wird.

**kriterienbasierte Mitgliedschaft** : eine Gruppe, in der die Mitgliedschaft der Gruppe durch einen Filter bestimmt wird. Siehe auch den Eintrag "statische Mitgliedschaft".

Gesamtstruktur **übergreifende Mitglieder** : ein Mitglied einer Sicherheitsgruppe, deren Benutzerkonto sich in einer anderen Gesamtstruktur als das Gruppenkonto befindet.

Gesamtstruktur **übergreifende Gruppen Berechnung** : eine Out-of-Box-Aktivität, die über Gesamtstruktur Mitglieder einer Gruppe im fremd Sicherheits Prinzipal Satz (FSP), der der Gesamtstruktur zugeordnet ist, in der sich die Gruppe befindet, platziert.


**Benutzerdefinierter Ausdruck** : die beschreibende Sprache, die verwendet wird, um Funktionen oder Attribut Flüsse im erweiterten Modus zu definieren.
<br/>

## <a name="d"></a>D

**Zielsatz (oder Ziel Ressourcendefinition nach Anforderung)** : eine Gruppe, in die eine Ressource verschoben wird, weil eine Anforderung die Attribute der Ressource ändert.

**standardmäßige Gruppen Validierungs Aktivität** : eine Out-of-Box-Workflow Aktivität, die bestimmt, ob eine Gruppen Verwaltungs Anforderung gegen die MIM 2016-oder Active Directory-Konfiguration oder-Richtlinie verstoßen würde.

**disconnectorobjekt** : ein Objekt im Connector-Bereich, das ein Objekt in einer verbundenen Datenquelle darstellt und derzeit nicht mit einem Objekt im Metaverse verknüpft ist.

**disconnectorobjekte** : Es gibt drei Typen von disconnectorobjekten: Disconnectors, explizite Disconnectors und gefilterte Disconnectors.

**Anzeige Name** : ein Attribut einer Ressource, die auf einer Benutzeroberfläche angezeigt wird, um diese Ressource zu identifizieren. Ein in einem anzeigen Amen verwendeter Wert sollte eindeutig und lesbarer Typ sein. Es ist wichtig, den anzeigen Amen anzugeben, wenn die Ressource in verschiedenen MIM-Portal Steuerelementen, z. b. in der Ressourcen Auswahl, verwendet werden soll.

**Verteiler Gruppe** : eine Sammlung von Ressourcen, bei denen es sich am häufigsten um Benutzer und andere Gruppen handelt, die gleichzeitig per e-Mail an das Postfach der Gruppe gesendet werden können.

**Domänen Konfiguration** : eine Konfigurations Ressource, mit der Active Directory Domänen modelliert werden.

**lokale Domänen Gruppe** : eine Gruppe mit lokalem Domänen Bereich ist eine Active Directory Gruppe, die Ressourcen in einer bestimmten Domäne schützt und die Mitglieder aus dieser Gesamtstruktur oder einer beliebigen vertrauenswürdigen Gesamtstruktur enthalten kann.

**Drop File** : eine Drop-Datei ist eine XML-Protokolldatei, die einen möglichen oder auftretenden Export oder Import darstellt.

**dynamischer Attribut Wert** : der Wert eines Attributs, das auf der Grundlage anderer Attribute berechnet wird. Beispielsweise wird ein Name-Attribut berechnet, indem der angegebene Name und der Nachname verkettet werden.

**dynamische Gruppe** : eine Gruppe, deren Mitgliedschaft automatisch von MIM 2016 ermittelt und auf dem neuesten Stand gehalten wird, indem sichergestellt wird, dass die Gruppe alle Ressourcen (z. b. Personen, Gruppen, Computer) enthält, die in Bedingungen liegen, die mithilfe von XPath ausgedrückt werden.
<br/>

## <a name="e"></a>E

**Enumeration** : eine Liste von Ressourcen, die vom MIM 2016-Dienst zurückgegeben werden.

**Eskalation** : Wenn eine Genehmigung nicht innerhalb der angegebenen Zeit abgeschlossen wird, wird die Genehmigung eskaliert, und der Genehmigung werden weitere genehmigende Personen hinzugefügt.

ausweitungs genehmigende **Person: der** Benutzer, der die Genehmigungs Anforderungs Nachricht empfängt, wenn die genehmigenden Personen nicht Antworten. Siehe auch den Eintrag für "Approver".

**expliziter Connector** : ein Objekt im Connector-Bereich, das mit einem Objekt im Metaverse verknüpft ist und nicht von einem Connector-Filter getrennt werden kann. Ein expliziter Connector kann nur manuell mit Joiner erstellt werden, und er kann nur durch Bereitstellung oder mithilfe von Joiner getrennt werden.

**expliziter disconnectorobjekt** : ein Objekt im Connector-Bereich, das nicht mit einem Objekt im Metaverse verknüpft ist und nur mit Joiner verknüpft werden kann. Ein Objekt wird zu einem expliziten disconnectorobjekt, indem das Objekt mithilfe von "Joiner" manuell getrennt wird.

**Export** : der Prozess zum Übertragen von Änderungen an die verbundenen Datenquellen wird exportiert. Bei einem Export handelt es sich immer um einen Delta Vorgang, der auf dem letzten erfolgreichen Import basiert. MIM verarbeitet die Änderungen, die Änderungen im Connector-Bereich werden jedoch erst verarbeitet, wenn die Änderungen durch einen neuen Import bestätigt wurden. Abhängig vom Typ des verwendeten Verwaltungs-Agents können sich die von einem Export implementierten Änderungen auf Attribut-, Objekt-oder Wert Ebene befinden.

**Export Attribut Fluss** : der Prozess, bei dem die Attribute eines Objekts aus der Metaverse in den Connector-Bereich übersetzt werden. Dieser Prozess kann 1:1-Zuordnungen beinhalten und Regelerweiterungen zum Ändern von Attributen oder festlegen statischer Attributwerte anwenden. Exportierte Attribute werden im Connector-Bereich für den nächsten Export in die verbundene Datenquelle bereitgestellt.

**Extensible Assert Markup Language (XAML)** : eine XML-basierte Sprache, in der Workflow Definitionen dargestellt werden.

**externer System Bereichs Filter** : bestimmt die Ressourcen, die Sie identifizieren und Filtern, basierend auf einer bestimmten Bedingung aus einem Quellverzeichnis.

**externer System Ressourcentyp** : Dies ist der Ressourcentyp im externen System, mit dem die MIM 2016-Ressourcen verbunden sind.

**Flag für die Erstellung externer Systemressourcen** : ein Parameter einer Synchronisierungs Regel, mit dem angegeben wird, ob eine Ressource im Connector-Bereich erstellt werden soll, wenn basierend auf den Beziehungs Kriterien eine solche Ressource nicht im externen System vorhanden ist. Siehe MIM 2016 ressourcenerstellungsflag. <!-- Not sure what this is, should this be a term? -->

**externer Systembereich** : ein Parameter einer Synchronisierungs Regel, der einen Filter enthält, der Ressourcen auf dem externen System darstellt, für das die Regel gilt.
<br/>

## <a name="f"></a>F

**Filter** : ein Ausdruck, der Filterbedingungen enthält. Ein Filter stimmt mit einer Ressource überein, wenn jede der im Filter enthaltenen Filterbedingungen mit der Ressource übereinstimmt. In MIM 2016 verwendet der Filter die XPath-Syntax.

**gefilterter disconnectorobjekt** : ein Objekt im Connector-Bereich, das aufgrund von Connector-Filterregeln im zugeordneten Verwaltungs-Agent nicht mit einem Objekt im Metaverse verknüpft oder projiziert werden kann.

**FIM-Verwaltungs-Agent** : ein Verwaltungs-Agent, der zwischen dem MIM 2016-Dienst und dem MIM 2016-Synchronisierungs Dienst synchronisiert.

**FIM/MIM-Client Dienst** für die Kenn Wort Zurücksetzung: Dies bezieht sich auf den Proxy Dienst, der sich auf dem Computer des Endbenutzers befindet, der mit dem MIM 2016-Server kommuniziert.

**FIM/MIM-Kenn Wort** Zurücksetzungs Erweiterungen: bezieht sich auf den Code, der sich auf dem Computer des Endbenutzers befindet, der die Funktionalität der Windows-Anmeldung erweitert, um die Self-Service-Kenn Wort Zurücksetzung einzubeziehen

**FIM/MIM-ressourcenerstellungsflag** : ein Parameter einer Synchronisierungs Regel, der angibt, ob eine Ressource in der MIM 2016-Datenbank erstellt werden soll, wenn die Ressourcen basierend auf den Beziehungs Kriterien nicht vorhanden sind. Siehe auch den Eintrag "Flag für die Erstellung externer Systemressourcen".

**Function** : eine Komponente, die in eine Synchronisierungs Regel oder eine Workflow Definition eingefügt werden kann, um Datenwerte zu verarbeiten.
<br/>

## <a name="g"></a>G

**Gate** : eine Workflow Aktivität, die in der Authentifizierungs Phase der Anforderungs Verarbeitung verwendet wird. Siehe auch den Eintrag für "QA Gate" und "Lockout Gate".

**Gruppen** Schachtelung: ein Feld einer Gruppen Definition, das angibt, ob die Gruppe andere Gruppen als Mitglieder der aktuellen Gruppe enthält.

**Gruppenbereich** : ein Feld einer Gruppen Definition, ein-oder ' local ', ' Global ' oder ' Universal '. Weitere Informationen finden Sie unter Active Directory Gruppen Bereichs.
<br/>

## <a name="i"></a>I

**Import** : der Prozess, bei dem verbundene Datenquellen Objekte zum Erstellen, ändern, löschen oder verifizieren in den Connector-Bereich verschoben werden. Ein Import kann ein vollständiger oder ein Delta Vorgang sein. Bei einem vollständigen Import fordert MIM alle festgelegten Objekte aus der verbundenen Datenquelle an und löscht alle Staging-Objekte, für die während des Imports kein entsprechendes Objekt empfangen wurde. Dies führt dazu, dass dieser Schritt zum Ausführen eines Profils zum Bereinigen der stagingobjekte im Connector-Bereich nützlich ist. Die Objekte, die von der verbundenen Datenquelle empfangen wurden, werden im Connector-Bereich bereitgestellt. Damit der Delta Import die gewünschten Ergebnisse bereitstellt, muss die verbundene Datenquelle eine Form des Wasserzeichens implementieren. Die verbundene Datenquelle verwendet das Wasserzeichen, um anzugeben, wann die letzten Änderungen an einem Objekt aufgetreten sind. MIM liest das Wasserzeichen, um zu bestimmen, was im Delta Import enthalten sein soll. Ein Beispiel hierfür ist die Active Directory-Verwendung.

**Import Attribut Fluss (IAF)** : der Import Attribut Fluss ist der Prozess des Importierens eines Attributs aus dem Connector-Bereich in die Metaverse. Dieser Vorgang kann das Anwenden von 1-zu-eins-Attribut Zuordnungen beinhalten, indem Regelerweiterungen zum Ändern von Attributen oder festlegen statischer Attribute verwendet werden.

**Bild-URL** : URL für eine Bilddatei, die in der MIM 2016-Portal-Benutzeroberfläche gerendert wird.

**ursprünglicher Flow** : ein ursprünglicher Fluss ist ein Attribut Wert Fluss, der nur einmal angewendet wird, wenn die Ressource zum ersten Mal erstellt wird. Das heißt, dass ein erstes Kennwort nur erstellt wird, wenn Sie zum ersten Mal ein Konto erstellen.

**interaktiver Workflow** : ein Workflow, der eine Antwort des Benutzers erfordert, der eine Änderung anfordert, z. b. zum Ausführen zusätzlicher Authentifizierungs Überprüfungen.
<br/>

## <a name="j"></a>J

**Join** : bei einem Join handelt es sich um den Prozess der Verknüpfung eines connectorbereichsobjektes mit einem vorhandenen Metaverse-Objekt. Attributwerte fließen nur zwischen verknüpften Objekten.

**Gruppen Anforderung beitreten** : eine Anforderung zum Hinzufügen eines Benutzers zu einer Gruppe.
<br/>

## <a name="l"></a>L

**Sperrungs** : eine Konfigurationseinstellung für eine Person-Ressource in der MIM 2016-Datenbank, die diese Person daran hindert, sich bei MIM 2016 zu authentifizieren oder die Kenn Wort Zurücksetzung auszuführen.

**sperrgate** : eine Workflow Aktivität in der Authentifizierungs Phase der Anforderungs Verarbeitung, um einen Benutzer zu sperren, der nicht authentifiziert werden konnte. Siehe auch den Eintrag für "Lock out" und für "QA Gate".

Sperrungs **Schwellenwert** : Dies ist ein ganzzahliges Steuerelement, das angibt, wie oft ein Benutzer den Authentifizierungs Workflow nicht beenden kann, bevor Sie für die Sperr Dauer gesperrt werden.Die Standardeinstellung hierfür ist 3. Der untere Grenzwert ist 0, und die Obergrenze ist 99.

**Sperr Dauer** : Dies ist ein ganzzahliges Steuerelement, das die Dauer in Minuten angibt, für die der Benutzer nach dem Sperr Schwellenwert gesperrt wird.Die Standardeinstellung hierfür beträgt 15 Minuten.Der untere Grenzwert für diese Einstellung ist 1, und die Obergrenze ist 9999.Die Obergrenze ermöglicht es dem Administrator, die Obergrenze auf mehr als einen Tag festzulegen.

**Sperr Schwellenwert Anzahl vor permanenter Sperrung** : Dies ist ein ganzzahliges Steuerelement, mit dem der Administrator einen numerischen Wert so konfigurieren kann, wie oft ein Benutzer den Sperr Schwellenwert erreichen kann, bevor er dauerhaft gesperrt wird.  Die permanente Sperre impliziert, dass der Benutzer vom Systemadministrator entsperrt werden muss. Standardmäßig ist dieser Wert auf 3 festgelegt.Der Bereich für diese Einstellung liegt zwischen 1 und 99.
<br/>

## <a name="m"></a>M

**Verwaltungs-Agent** : ein Verwaltungs-Agent (Management Agent, MA) verbindet eine bestimmte verbundene Datenquelle mit dem Metaverzeichnis. Er ist verantwortlich für das Verschieben von Daten aus der verbundenen Datenquelle in MIM und die Regeln, die die Eignung der Identitätsdaten bestimmen, die an MIM und anderen verbundenen Datenquellen teilnehmen sollen. Wenn die Daten im Metaverzeichnis geändert werden, kann der Verwaltungs-Agent die Daten in die verbundenen Datenquellen exportieren, damit die verbundene Datenquelle mit den Daten in MIM synchronisiert bleibt. Ein Verwaltungs-Agent wird verwendet, anstatt über einen Agent-basierten Connector zu verfügen.

**Verwaltungsrichtlinien Regel (MPR)** : Verwaltungsrichtlinien Regeln (MPRs) bieten einen Mechanismus zum Modellieren von Geschäfts Verarbeitungs Regeln für eingehende Anforderungen an den MIM 2016-Server. Sie steuern die Berechtigungen für das Anfordern von Vorgängen für MIM 2016-Ressourcen in Verbindung mit den Workflows, die von diesen Anforderungen ausgelöst werden.
   
   Es gibt zwei Arten von MPRs:
   
- Anforderungs-MPRs: Erteilen von Berechtigungen und Ausführen von Workflows (vor dem angeforderten Vorgang aufgerufen).
- Übergangs-MPRs festlegen: nur Workflows ausführen (Reaktion auf eine angewendete Zustandsänderung).
   
  Die Haupt Entwurfs Objekte für MPRs sind:
   
- Modellierungs Berechtigungen
- Modellieren von Workflow Zuordnungen
- Modellieren von Übergängen
- Modellieren von reflexiven Definitionen
- Modellieren des nicht authentifizierten Benutzer Zugriffs
- Modellieren Temporaler Richtlinien
- Modellierungs Filter Berechtigungen

**manuell verwaltetes Mitglied** : die Mitgliedschaft der Gruppe oder Gruppe, die aus einer manuell ausgewählten Liste von Benutzern, Gruppen oder anderen Ressourcen besteht.

**Metaverse** : der zentrale Datenspeicher, der von MIM verwendet wird, um die aggregierten Identitätsinformationen aus mehreren verbundenen Datenquellen zu enthalten, wobei eine einzelne globale, integrierte Ansicht aller kombinierten Objekte bereitgestellt wird. Der Metaverse kann nicht als Tabelle oder Sicht von aggregierten Identitätsdaten für andere Anwendungen als MIM verwendet werden, da dies zu einer Beschädigung führen kann.

**überwachtes Postfach** : ein Postfach, das von MIM 2016-Dienst überwacht wird, um die Genehmigung zu erhalten und e-Mails vom MIM-Add-in für Outlook anzufordern.
<br/>

## <a name="n"></a>N

**Benachrichtigungs Aktivität** : eine Workflow Aktivität innerhalb der Aktionsphase der Anforderungs Verarbeitung, in der MIM 2016 e-Mails an einen oder mehrere Benutzer sendet, um Sie über die Anforderung zu benachrichtigen.

**Benachrichtigungs Meldung** : eine von einer Benachrichtigungs Aktivität gesendete e-Mail-Nachricht. Siehe auch den Eintrag für "Benachrichtigungs Aktivität".
<br/>

## <a name="o"></a>O

**ObjectID (ResourceId)** : ein Attribut, das eine von MIM 2016 für jede Ressource zugewiesene Globally Unique Identifier (GUID) enthält, wenn diese erstellt wird. Dies wird auch als Ressourcen-ID bezeichnet.

**Objekt Bezeichner** : eine Sequenz von Zahlen, die als Bezeichner für ein Feld in einem digitalen X. 509-Zertifikat verwendet wird, oder für einen Attributtyp oder eine Objektklasse in einem LDAP-basierten Verzeichnisdienst. Objekt Bezeichner werden in der Regel von Softwareanbietern und Standard Textteilen zugewiesen.

**Vorgangstyp** : der Vorgangstyp wird für die Ressource angefordert, die von MIM 2016 über den Webdienst verwaltet wird. Dies umfasst das Erstellen und Löschen von Ressourcen und das Lesen und Ändern von Ressourcen Attributen. Darüber hinaus können Sie mit dem hinzufügen/entfernen-Vorgang Weitere Steuerungsmöglichkeiten auf den Modify-Vorgang anwenden, um das Hinzufügen von Werten zu Attributen oder deren Entfernung zu steuern.

**Operator** : ein Element eines Filters, das einen Vergleich oder eine andere Beziehung zwischen Datenwerten angibt.

**Ursprungs Satz (oder Ziel Ressourcendefinition vor Anforderung)** : eine Gruppe, in der eine Ressource vor einer Änderung in den Attributen der Ressource gehörte.
<br/>

## <a name="p"></a>P

**Parameter** : Wenn Sie neue Ressourcen bereitstellen, ist es manchmal möglich, Attributwerte aus einer externen Quelle, wie z. b. einem Benutzer, bereitzustellen. Ein Attribut Wert wird als Parameter übergeben, um erfolgreich eine neue Ressource erstellen zu können.

**Partition** : eine logische Datenmenge im Connector-Bereich. Ein Verwaltungs-Agent kann eine oder mehrere Partitionen erstellen, um Daten logisch in separate logische Gruppierungen aufzuteilen. Jedes Datenvolumen wird während der Synchronisierung einzeln verarbeitet.

**Zurücksetzen des Kennworts** : eine Prozedur, mit der das Kennwort eines Benutzers in Situationen, in denen der Benutzer das Kennwort vergessen oder verloren hat, in einen bekannten Wert geändert werden kann. Siehe auch den Eintrag für "Registrierung".

**Phase** : jede Anforderung zum Erstellen, aktualisieren oder Löschen von Ressourcen wird in drei Workflow Phasen verarbeitet. In der Authentifizierungs Phase können zusätzliche Authentifizierungsprüfungen des anfordernden Benutzers durchgeführt werden. In der Autorisierungs Phase werden alle notwendigen Genehmigungen gesammelt. In der Aktionsphase werden die Aktivitäten ausgeführt, nachdem für die Anforderung zum Ändern der Ressource ein Commit ausgeführt wurde.

**Platzhalter Objekte** : Objekte im Connector-Bereich, die eine einzelne Ebene der Hierarchie der verbundenen Datenquelle darstellen. Wenn Sie z. b. Objekte mit einer Active Directory-Gesamtstruktur synchronisieren möchten, müssen Sie die Container importieren, die den Pfad für die Active Directory Objekte bilden. Im Beispiel "CN = Mikedan, ou = users, DC = Microsoft, DC = com" werden Platzhalter Objekte für "DC = com" und "DC = Microsoft, DC = com" erstellt. Außerdem kann ein Platzhalter Objekt ein Objekt in der verbundenen Datenquelle darstellen, auf das sich ein importiertes Verweis Attribut Wert bezieht (z. b. das Objekt, auf das das Manager-Attribut in einem Benutzerobjekt verweist). Platzhalter Objekte enthalten keine Attributwerte und können nicht mit der Metaverse verknüpft werden.

**Richtlinien Verwaltung** : die Richtlinien Verwaltung in MIM 2016 wird von einer SharePoint-basierten Konsole für das Erstellen und Erzwingen von Richtlinien ermöglicht. Erweiterbare Windows Workflow Foundation basierte Workflows ermöglichen es Benutzern, Identitäts Verwaltungsrichtlinien zu definieren, zu automatisieren und zu erzwingen. Die Richtlinien Verwaltung umfasst auch heterogene Identitäts Synchronisierung und Konsistenz, die durch die Integration einer Vielzahl von Netzwerk Betriebssystemen, e-Mail, Datenbank, Verzeichnis, Anwendung und Flatfile-Zugriff erreicht wird.

**Richtlinien Aktualisierung (oder bei Richtlinien Aktualisierung ausführen)** : Wenn ein Workflow als Auswirkung einer Änderung an den festzulegenden Sätzen oder MPRs erneut ausgeführt werden soll, wird das Flag für die Richtlinien Aktualisierung verwendet, um dies anzugeben.

**Rangfolge** : eine Reihenfolge der Synchronisierungs Regeln.

**Prinzipal Satz** : ein Satz, der in der Verwaltungsrichtlinien Regel verwendet wird, um den Satz von Ressourcen (normalerweise Benutzer) anzugeben, die die Auswertung der Verwaltungsrichtlinien Regel initiieren.

**Prinzipal Satz relativ zur Ressource** : Dies ist eine reflexive Eigenschaft. Der Wert wird in Bezug auf eine der Ressourcen Eigenschaften definiert. Sie wird verwendet, um dynamische Verwaltungsrichtlinien Regeln zu definieren, deren Bedingungen im Kontext der einzelnen verarbeiteten Ziel Ressourcen ausgewertet werden.

**Projektion** : der Prozess, bei dem ein Objekt im Metaverse basierend auf Projektions Regeln erstellt und dann automatisch mit einem vorhandenen Objekt im Connectorbereich verknüpft wird.

**Bereitstellung** : der Prozess zum Erstellen, umbenennen und Aufheben der Bereitstellung von Objekten in vordefinierten Connectorbereichen auf der Grundlage von Änderungen an einem Objekt im Metaverse. Bereitstellungs Regeln können so eingerichtet werden, dass Sie immer dann aufgerufen werden, wenn ein Metaverse-Objekt geändert wird. Diese Regeln können Aktionen auf Objektebene ausführen, wie z. b. das Erstellen eines neuen Verbinder-Objekt Objekts oder das Trennen vorhandener Connector-Bereichs Objekte, die mit dem Metaverse-Objekt verknüpft sind.
<br/>

## <a name="q"></a>Q

**QA Gate** : eine Workflow Aktivität in einer Authentifizierungs Phase, in der der anfordernde Benutzer Antworten auf eine oder mehrere vordefinierte Fragen bereitstellen muss. Diese Aktivität wird in der Regel bei der Kenn Wort Zurücksetzung verwendet, um den Benutzer dazu zu bringen, seine Identität zu beweisen, indem er dem Benutzer eine Auswahl vorgestellte Fragen bereitstellt, für die der Benutzer die richtige Antwort angeben muss. Siehe auch den Eintrag für "sperrgate".

**QA Challenge** : eine Herausforderung, die erfordert, dass der Benutzer eine Reihe von Fragen beantworten muss, um sich bei MIM 2016 zu authentifizieren.
<br/>

## <a name="r"></a>R

**zufällige Kenn Wort Einstellungen** : eine Einstellung, mit der die Anzahl der Zeichen bestimmt wird, die zum Festlegen eines Kennworts im externen Verzeichnis erforderlich sind.

**Verweis Attributtyp** : ein Attributtyp, bei dem die Werte des Attributs die ObjectID-Attributwerte (Global Unique Identifier) anderer Ressourcen in MIM 2016 sind.

**referenzielle Integrität** : eine Einschränkung in MIM 2016, in der ein Verweis Attribut nicht als Wert eine ObjectID einer gelöschten Ressource aufweisen kann.

**Registrierung** : ein Verfahren zum Konfigurieren der Self-Service-Kenn Wort Zurücksetzung für einen Benutzer. Siehe auch den Eintrag für "QA Gate".

**erneute Registrierung** : das Aktualisieren der Registrierung für eine Authentifizierungs Aufforderung in MIM 2016. Dies ist in der Regel nach einer Änderung der administrativen Richtlinie für die Registrierung der Kenn Wort Zurücksetzung erforderlich.

**Beziehungs Erstellung** : Konfigurationsflags einer Synchronisierungs Regel, die bestimmt, ob Ressourcen automatisch in MIM 2016 oder im externen System erstellt werden sollen, wenn die Ressourcen nicht vorhanden sind.

**Beziehungs Kriterium** : das Festlegen einer Synchronisierungs Regel, die verwendet wird, um Ressourcen in MIM-Server und Ressourcen in externen Systemen abzugleichen.

**Beendigung der Beziehung** : gibt an, ob verwandte Ressourcen in anderen externen Systemen getrennt (und möglicherweise gelöscht) werden sollen, wenn die Synchronisierungs Regel nicht mehr angewendet wird.

**Anforderungs Verwaltung** : die Fähigkeit eines Benutzers, mit übermittelten Anforderungen und zugeordneten Workflows zu interagieren und diese zu verwalten.

**richtlinienverwaltungs-Richtlinien Regel (rmpr)** : ein Regeltyp für die Verwaltungs Richtlinie, der ausgewertet und auf eingehende Anforderungen zum Ausführen von Vorgängen angewendet wird. Rmprs werden primär zum Erstellen von Zugriffsrichtlinien Definitionen in MIM verwendet. Anders ausgedrückt: die Antwort auf die Verarbeitung der Anforderung. Wenn Sie eine rmpr konfigurieren, befindet sich der Anforderer in dem Satz, der zum Ausführen eines Vorgangs entworfen wurde.

   Die MIM-Architektur definiert sechs verschiedene Vorgänge, für die eine rmpr definiert werden kann:
   
- Dient zum Erstellen einer Ressource.
- Löschen Sie eine Ressource.
- Lesen einer Ressource.
- Fügen Sie einem mehrwertigen Attribut einen Wert hinzu.
- Entfernen Sie einen Wert aus einem mehrwertigen Attribut.
- Ändern eines einwertigen Attributs.
   
  Wenn Sie eine rmpr definieren, müssen Sie mindestens einen dieser sechs Vorgänge auswählen. Die Vorgänge werden immer im Kontext der anfordernden Person definiert. Jede Bedingung erfordert die Definition eines Ziels. Ein Vorgang, der auf ein Ziel angewendet wird, kann zu einem Zustandsübergang der Ziel Ressource führen. Eine rmpr wird immer aufgerufen, bevor ein angeforderter Vorgang durchgeführt wurde. Um das Ziel ihrer Bedingung effektiv zu bezeichnen, müssen Sie zwei verschiedene Zustände konfigurieren:
   
- Ziel Ressourcen Definition vor Anforderung: der Status des Ziels, bevor die Anforderung angewendet wird.
- Ziel Ressourcen Definition nach Anforderung: der Status des Ziels nach dem Anwenden der Anforderung.
   
  Ob beide Zustände definiert werden müssen, hängt von den Vorgängen ab, die von ihrer rmpr definiert werden. Bei einem Erstellungs Vorgang hat die angeforderte Ressource keinen Anfangszustand. Daher müssen Sie die Ziel Ressourcen Definition nach der Anforderung "für einen Erstellungs Vorgang" konfigurieren.
   
  Ein Lese-oder Löschvorgang führt nicht zu einem Zustandsübergang. Für diese beiden Arten von Vorgängen müssen Sie nur die Ziel Ressourcen Definition vor der Anforderung angeben.
   
  Für einen Änderungs Vorgang oder alle anderen Kombinationen von Vorgängen müssen Sie beide Zustände konfigurieren, die möglicherweise dieselben Werte aufweisen, wenn kein Zustandsübergang stattfindet.
   
  Sie können die verknüpften Ressourcen relativ zum Anforderer (z. b. das eigene Benutzerobjekt des Anforderers, den Vorgesetzten eines Ziel Benutzers oder den Besitzer einer Zielgruppe) Ausdrücken.
   
  Die einfachste Form einer Reaktion auf eine Bedingung besteht darin, Berechtigungen zum Ausführen des angeforderten Vorgangs zu erteilen. Zusätzlich zum Erteilen von Berechtigungen können Sie andere Vorgänge auch als Antwort auf eine Bedingung in einer rmpr definieren. In der MIM-Architektur werden diese Vorgänge in Form von Workflows definiert. Wenn eine bestimmte rmpr verarbeitet wird, verfügt das System möglicherweise nicht über genügend Informationen, um die Berechtigung zu erteilen. In diesem Fall können Sie zusätzliche Authentifizierungs-und Autorisierungs Schritte in ihrer rmpr definieren, die für die Person gelten, die eine bestimmte Anforderung ausführt. Um z. b. die Berechtigung zum Ausführen des angeforderten Vorgangs zu erteilen, benötigen Sie möglicherweise eine manuelle Interaktion eines Benutzers, um den Vorgang zu genehmigen.
   
  In der folgenden Abbildung wird die gesamte Architektur einer rmpr erläutert:
   
  ![Rmpr-Architektur](media/mim-2016-sp1-terms/8f5054080d3f8bd9c73d93a5e3ed51f9.gif)
   
  Wenn ein neues Anforderungs Objekt in MIM erstellt wird, fragt das System die konfigurierten rmprs nach übereinstimmenden Objekten ab, indem die Anforderungs Bedingungen mit den konfigurierten Bedingungen in ihrer Verwaltungs-rmprs verglichen werden. Wenn übereinstimmende rmprs gefunden werden, werden Sie auf das Anforderungs Objekt in der Warteschlange angewendet. In der folgenden Abbildung wird dieser Vorgang dargestellt:
   
  ![Übereinstimmende rmprs werden gefunden und auf das Anforderungs Objekt in der Warteschlange angewendet.](media/mim-2016-sp1-terms/65bdb65aaffa55a90707ff6f7b13632d.gif)
   
  Im MIM-Portal müssen Berechtigungen für Vorgänge explizit erteilt werden. Anders ausgedrückt, es sei denn, Sie werden durch eine rmpr erteilt, alle Vorgänge für Ressourcen werden verweigert. Jedes Anforderungs Objekt erfordert mindestens eine rmpr, die die Berechtigung zum Ausführen des angeforderten Vorgangs für ein Ziel erteilt.

**Anforderungs Objekt** : Wenn ein Benutzer eine Aufgabe im MIM-Portal oder im MIM-Add-in für Outlook ausführt, wird er als Anforderungs Objekt dargestellt. Anforderungs Objekte stellen einen praktischen Berichterstattungs Mechanismus für Aktivitäten in Ihrem System dar.

   Jedes Anforderungs Objekt verfügt über die folgenden Komponenten:
   
- Requester: eine Ressource, die zum Ausführen eines Vorgangs auffordert.
- Operation: die Aktion, die der Anforderer ausführen möchte.
- Target: die Ressource, die das Ziel des angeforderten Vorgangs ist.
   
  Logisch ist ein Anforderungs Objekt eine Implementierung der folgenden Anweisung:
   
  `The requester attempts to perform the following operation on this target...`
      
  In der folgenden Abbildung wird die allgemeine Architektur eines Anforderungsobjekts erläutert:
   
  ![Objektarchitektur anfordern](media/mim-2016-sp1-terms/788e9b3a4fcddb78ff4a05dcb5e61192.gif)
   
  Jedes Anforderungs Objekt verfügt über eine Status-Eigenschaft, um den Verarbeitungs Status anzugeben. Verarbeitungsanforderungen erfordern möglicherweise eine manuelle Interaktion, um eine Anforderung abzuschließen. Beispielsweise kann der Besitzer einer Gruppe die Anforderung eines anderen Benutzers zum beitreten zu einer Gruppe manuell genehmigen. Zusätzlich zu einer manuellen Interaktion können Sie auch MIM so konfigurieren, dass eine bestimmte Anforderung automatisch verarbeitet wird, ohne dass eine menschliche Interaktion erforderlich ist.
   
**Anforderungs Verarbeitungsmodell** : das Anforderungs Verarbeitungsmodell in MIM besteht aus drei Hauptphasen:

- Phase 1: Authentifizierung
- Phase 2: Autorisierung
- Phase 3: Aktion
   
  Workflows, von denen jede eine oder mehrere Aktivitäten enthält, können an jede dieser Phasen angefügt und im Kontext der Ausführung einer einzelnen Anforderung ausgeführt werden. Eine Anforderung kann von einem einzelnen Benutzer Aufrufe eines Webdienst-Endpunkts oder durch einen Benutzer, der eine Anforderung im MIM-Portal erstellt, initiiert werden.
   
  In der folgenden Abbildung wird die Beziehung zwischen den Komponenten der Anforderungs Verarbeitung veranschaulicht:
   
  ![Beziehung von Komponenten für die Anforderungs Verarbeitung](media/mim-2016-sp1-terms/0e57445d8b9a3216c31add80eb26493f.gif)
   
  Anforderungen werden in der folgenden Reihenfolge verarbeitet:
   
- Anforderungs Objekt Erstellung: MIM 2016 erstellt ein Anforderungs Objekt als Reaktion auf einen-Webdienst-Endpunkt oder eine Anforderung, die über das MIM-Portal initiiert wurde.
   
- MPR-Auswertung: die Rechte des Anforderers zum Anfordern der Aktion werden überprüft, und die Berechnung der anwendbaren Workflows wird durchgeführt. Die Anforderung wird anhand von Zuordnungen zu beliebigen MPR-Objekten überprüft. Zum Zuordnen zu einem MPR müssen alle anwendbaren Felder der MPR für den angeforderten Vorgang abgeglichen werden. Dies schließt die Anforderer, den Vorgang, die Ziel Ressource und Attribute ein. Wenn alle diese Bedingungen einschließlich der betroffenen Attribute für eine eingehende Anforderung zutreffen, wird die entsprechende MPR mit der Anforderung abgeglichen. Eine Anforderung muss mindestens einem MPR zugeordnet sein, das die Berechtigung als Teil ihrer Definition erteilt. Wenn dies zutrifft, durchläuft die Anforderung die Überprüfung der Berechtigungsstufe der Anforderungs Verarbeitung. Wenn dies nicht zutrifft, schlägt die Anforderung fehl. Das System bestimmt auch die festgelegten Übergänge, die Teil der Anforderung sind, und sucht alle zugehörigen Satz Übergangs basierten MPRs.
   
- Authentifizierung: MIM 2016 führt Authentifizierungs Workflows nacheinander in einer nicht deterministischen Reihenfolge aus, um die Identität des Anforderers zu bestätigen.
   
- Autorisierung: MIM 2016 bestätigt die Berechtigung der anfordernden Person, den angeforderten Vorgang für die in der Anforderung angegebene Ressource auszuführen. Alle abhängigen Autorisierungs Workflows werden parallel ausgeführt, aber eine Anforderung wird nicht in den MIM-Objektspeicher committet, es sei denn, alle Workflows wurden abgeschlossen, und alle waren erfolgreich.
   
- Verarbeitung: MIM 2016 führt den angeforderten Vorgang für den MIM-Anwendungs Speicher aus.
   
- Aktion: MIM 2016 führt alle Prozesse aus, die aufgrund des angeforderten Vorgangs stattfinden. Alle Aktions Workflows werden parallel ausgeführt. Für Lesevorgänge sind keine Workflows auf ihre Verarbeitung angewendet. Dies schließt die konfigurierten Workflows in der rmpr sowie die Workflows in den auf dem Set Übergangs basierten MPRs ein.
   
  >[!NOTE]
  >Durch das Synchronisierungs Konto initiierte Anforderungen werden alle Authentifizierungs-und Autorisierungs Workflows umgangen, die für Sie gelten. Alle anwendbaren Aktions Workflows werden angewendet.

**Anforderer** : die Identität des Benutzers oder dienstanders, von dem eine Anforderung an MIM 2016 übermittelt wurde.

**Anforderer-Bereich** : eine konfigurierte Sammlung von Benutzern, die eine Anforderung übermitteln können. Kann "alle" oder eine bestimmte Gruppe von Benutzern sein, die durch einen Filter definiert sind.

**Ressource** : eine Instanz eines bestimmten Ressourcentyps in MIM 2016. Jede Ressource wird durch ihr ObjectID-Attribut (ResourceId) eindeutig identifiziert.

**Ressourcensteuerungs-Anzeige Konfiguration (sscdc)** : rcdcs sind Konfigurations Ressourcen, die zum Rendering der Benutzeroberfläche im Ressourcen Steuerelement (RC) zum Erstellen eines bestimmten Ressourcentyps in MIM 2016 verwendet werden.

**aktuelle Ressourcengruppe** : Teil der Bedingungs Definition der Verwaltungsrichtlinien Regeln (MPR). Die Auflistung der Ziel Ressourcen zu dem Zeitpunkt, zu dem die Anforderung empfangen wird. Gilt für Lese-, Lösch-und Änderungs Vorgangs Typen.

**endgültige Ressourcen Menge** : Teil der Bedingungs Definition in Verwaltungsrichtlinien Regeln. Die Auflistung von Ziel Ressourcen nach der Verarbeitung der Anforderung. Gilt nur für die Erstellung und Änderung von Vorgangs Typen.

**Ressourcen Hierarchie** : in einem Verzeichnisdienst ist die Hierarchie eines Ressourcen Eintrags die Auflistung der Verzeichniseinträge zwischen der Basis eines Namens Kontexts und diesem Ressourcen Eintrag.

**Ressourcenbereich** : eine Gruppe von Ressourcen, über die eine Anforderung übermittelt werden kann.

**Ressourcentyp** : ein Teil eines Schemas, das die Darstellung einer Ressource in MIM 2016 definiert.

**Ressourcentyp Zuordnung** : eine Beziehung zwischen einem Ressourcentyp, der zum Darstellen einer Ressource in MIM 2016 verwendet wird, und einer Ressourcen Klasse, mit der diese Ressource in der Metaverse dargestellt wird.

**Rolle** : ein organisatorisch zugewiesener Sicherheits Prinzipal, der zum Verwalten von Zugriffsrechten verwendet wird.

**Regel Erweiterung** : eine Regel Erweiterung ist eine DLL-Datei (Dynamic Link Library), die einen definierten Satz von Regeln für die Datenverwaltung enthält. Sie können Regelerweiterungen während der Synchronisierung verwenden, um die Funktionalität zu erweitern. Beispielsweise können Sie eine Regel Erweiterung verwenden, um Daten aus zwei Quell Attributen zu kombinieren und zu einem Ziel Attribut (z. b. `sn` und `givenName` in) zu fließen `displayName` .

Ausführungs **Verlauf** : ein Satz von Statistiken, die die Ergebnisse einer einzelnen Ausführungsdauer eines Verwaltungs-Agents anzeigen.

Ausführungs **Profil** : ein Ausführungsprofil stellt eine Reihe von Schritten dar, die angeben, wie ein Verwaltungs-Agent und Konfigurationseinstellungen ausgeführt werden, die bestimmen, wie ein Verwaltungs-Agent ausgeführt wird. Ein Verwaltungs-Agent kann über mehrere Lauf Profile verfügen, die mit dem Verwaltungs-Agent gespeichert werden. Ein Testlauf-Profil besteht aus mindestens einem Schritt für den Testlauf.
<br/>

## <a name="s"></a>E

**Suchordner** : siehe den Eintrag für den Ordner "Genehmigungen Search".

**Suchbereich** : gibt die Eigenschaften für einen bestimmten Such Kontext an, den ein Benutzer über das MIM 2016-Portal ausführen kann. Beispielsweise kann ein Benutzer einen Suchbereich aus einer Dropdown Liste für "alle Benutzer", "alle Verteilerlisten" und "meine ausstehenden Genehmigungen" auswählen, und die Suchergebnisse werden auf Elemente beschränkt, die diese Kriterien erfüllen, zusätzlich zu den vom Benutzer angegebenen Suchbegriffen.

**Sicherheits Beschreibung** : eine Struktur und zugehörige Daten, die die Sicherheitsinformationen für eine Sicherungs fähige Ressource enthalten. Eine Sicherheits Beschreibung identifiziert den Besitzer und die primäre Gruppe der Ressource. Sie kann auch eine DACL enthalten, die den Zugriff auf die Ressource steuert, und eine SACL, die die Protokollierung von Zugriffsversuchen auf die Ressource steuert.

**Sicherheits Prinzipal** : eine Identität, die für die Sicherheitsverwaltung verwendet wird, z. b. ein Benutzerkonto, das sich bei einem Dienst authentifizieren kann.

**Sicherheits Token** : ein Protokoll Element, das Authentifizierungs-und Autorisierungs Informationen auf Grundlage von Anmelde Informationen überträgt. In den Webdienst Protokollen wird ein Sicherheits Token als XML-Element in einem SOAP-Header dargestellt, wie von WS-Security definiert.

**Sicherheitstokendienst** : ein Dienst, der das WS-Trust Protokoll implementiert, um die Vertrauensstellung zwischen Clients und Diensten basierend auf dem Austausch von Sicherheits Token zu verwalten.

**sequenzieller Workflow** : alle Workflows in MIM 2016 werden vom sequenziellen Workflow von Windows Workflow Foundation abgeleitet. Es umfasst mehrere Workflows in sequenzieller Reihenfolge.

**Dienst Konto** : ein Windows-Konto, das von einem Windows-Dienst verwendet wird und nicht von einem Benutzer zum Anmelden an einem Computersystem verwendet werden kann. Es stellt das Systemkonto von MIM dar.

**Set** : eine benannte Auflistung von Ressourcen. In der Regel werden Sätze verwendet, um Ressourcen basierend auf Regeln zu organisieren. Die Mitgliedschaft in einer Gruppe ist manuell – verwaltet oder Kriterienbasiert. Dies bedeutet, dass Sie Ressourcen manuell zu einer Gruppe hinzufügen können, und Sie können Kriterien definieren, mit denen Ressourcen automatisch einer Menge basierend auf einer Filter Anweisung hinzugefügt werden. Wenn eine Ressource die Filterkriterien erfüllt, wird Sie automatisch dem zugehörigen Satz hinzugefügt.

**Festlegen der Übergangs Verwaltungsrichtlinien Regel (Tmpr)** : eine Verwaltungsrichtlinien Regel, die auf Änderungen an der Mitgliedschaft einer Gruppe angewendet wird. Legen Sie für tmprs Aktions Workflows fest, wenn der Objekt Übergang in eine oder aus einer angegebenen Menge in der MPR besteht.

   Es gibt zwei Arten von tmprs:
   
- Übergang in: eine Ressource wird zu einem Mitglied der Übergangs Menge.
- Übergang ausgehend: eine Ressource verlässt den übergangssatz.
   
   >[!NOTE]
   >Wenn eine Übergangsgruppe gelöscht wird, behandelt das System den Löschvorgang als ein Übergangs Ereignis für die betroffenen Objekte.
      
  Die Antwort ist eine Reaktion auf eine angewendete Zustandsänderung. Wenn die zugehörige MPR aufgerufen wird, wurde die Bedingung bereits angewendet. Dies bedeutet, dass sich die betroffenen Ressourcen bereits in einen oder aus einem übergangssatz geändert haben. Bei tmprs besteht das Ziel der Antwort darin, die Reaktion auf einen angeforderten Vorgang nicht zu definieren, sondern die Antwort auf einen angewendeten Vorgang zu definieren. Anders ausgedrückt: für ein auf einem Set Übergangs basiertes MPR ist es unerheblich, wie ein Zustand erreicht wurde. Was relevant ist, sind die Folgen einer Zustandsänderung.
   
  Wenn Sie eine auf dem Set-Basis basierende MPR in MIM konfigurieren, müssen Sie die folgenden drei Einstellungen angeben:
   
- Übergangssatz
- Übergangstyp
- Richtlinienworkflows
   
  Bei den Richtlinien Workflows handelt es sich um Definitionen der Prozesse, die als Reaktion auf die Zustandsänderung aufgerufen werden müssen. Die häufigsten Anwendungsfälle für Zustands basierte MPRs sind das gewähren oder widerrufen von Berechtigungen und bereitstellen und Aufheben der Bereitstellung in externen Datenquellen.
   
  In der folgenden Abbildung wird die gesamte Architektur einer Satz Übergangs basierten MPR erläutert:

  ![Festlegen der Tmpr-Architektur](media/mim-2016-sp1-terms/e12154d35b48cec676f160930ff33662.gif)
  
  Festlegen Übergangs basierter MPRs werden von Anforderungen aktiviert. Wenn eine Anforderung verarbeitet und durch eine rmpr genehmigt wird, bestimmt der MIM-Dienst auch, ob eine genehmigte Anforderung zu einem Zustandsübergang führt und ob ein Zustands Übergangs basiertes MPR vorhanden ist, das die Zustandsänderung behandelt.
  
  In der folgenden Abbildung wird die Beziehung zwischen einer Anforderung und einer festgelegten Übergangs basierten MPR dargestellt:
  
  ![Beziehung zwischen einer Anforderung und einer Set-Tmpr](media/mim-2016-sp1-terms/426be3a3a5380720fa1eaa3e7df360df.gif)

**Sid** : ein eindeutiger Wert, der verwendet wird, um ein Benutzerkonto, ein Gruppenkonto oder eine Anmelde Sitzung zu identifizieren.

**SOAP** : ein Protokoll zum Austauschen strukturierter Informationen zwischen Softwarekomponenten.

**Synchronisierung** : der Prozess, bei dem ausgewählte Daten in mehreren Datenquellen in einer Vereinbarung aufbewahrt werden. Synchronisierung stellt Vorgänge für Objekte dar, die sich ausschließlich in MIM befinden Die Synchronisierung kann ein Vorgang für den gesamten Satz von Daten sein, für den Sie für einen Verwaltungs-Agent oder einen Delta Vorgang gemäß den Änderungen seit dem letzten bekannten Vorgang definiert ist. Der Schritt zum Ausführen des Synchronisierungs Profils definiert die eingehenden und ausgehenden Synchronisierungs Prozesse.

   Der Schritt zum Ausführen des Synchronisierungs Profils weist zwei Untertypen auf:
   
- Deltasynchronisierung
- Vollständige Synchronisierung
   
  Während der Delta Synchronisierung verarbeitet MIM nur importierte Objekte, bei denen es sich um stagingobjekte handelt, die als ausstehenden Import gekennzeichnet sind. Dieser Schritt für das Ausführen eines Profils ist hilfreich, wenn nur die Objekte verarbeitet werden sollen, die ausstehende Änderungen aufweisen, während einer vorherigen Synchronisierung jedoch nicht verarbeitet wurden.
   
  Die Delta Synchronisierung wird in zwei vordefinierten Lauf Profilen verwendet und verhält sich in den einzelnen Profilen etwas anders. Beim ersten Testlauf-Profil handelt es sich um eine Delta Synchronisierung, bei der kein Import aus einer verbundenen Quelle ausgeführt wird, aber alle Objekte im Connector-Bereich ausgewertet werden und alle Objekte mit ausstehenden Änderungen verarbeitet werden. Beim zweiten Testlauf-Profil handelt es sich um Delta Import und Delta Synchronisierung. Dieses Testlauf-Profil importiert nur die Objekte und Attribute aus der verbundenen Datenquelle, deren Werte seit dem letzten Ausführen des Verwaltungs-Agents geändert wurden. Verwaltungs-Agent-Regeln werden dann nur auf Objekte angewendet, die ausstehende Änderungen aus dem Delta Import aufweisen. Objekte ohne ausstehende Änderungen von diesem Delta Import werden nicht ausgewertet.
   
  Bei der vollständigen Synchronisierung wertet MIM die Synchronisierungs Regeln aus und wendet Sie auf alle stagingobjekte in einem Connector-Bereich an. Die vollständige Synchronisierung sollte immer dann initiiert werden, wenn Änderungen auf die Regeln einer bestimmten Umgebung angewendet wurden. Abhängig von der Anzahl der Objekte im Connector-Bereich kann dies ein Zeit-und ressourcenintensiver Vorgang sein. Daher sollten häufige Änderungen an Synchronisierungs Regeln in der Produktionsumgebung vermieden werden.

**Synchronisierungs Filter** : ein Filter, mit dem verhindert wird, dass Ressourcen im Metaverse an die MIM 2016-Datenbank übertragen werden.

**Synchronisierungs Regel** : eine Regel für den Fluss von Ressourcen Informationen zwischen dem MIM-Server (einschließlich der MIM-Synchronisierungs-Engine) und dem verbundenen externen System.
<br/>

## <a name="t"></a>T

**Temporale Richtlinie** : ein Set Transition MPR, das an einen temporalen Satz gebunden ist. Die Richtlinie wird auf den Zeitverlauf angewendet, da Objekte basierend auf der Definition der temporalen Sätze in den und aus dem Satz übergehen.

**Temporale Menge** : ein Typ eines Set-Objekts, das auf relativen Datumsangaben basiert. Temporale Mengen bieten einen Mechanismus, mit dem der Prozess des Übergangs in eine oder aus einer Menge basierend auf dem Zeitverlauf vollständig automatisiert werden kann. Beispielsweise kann ein Temporaler Satz für alle Gruppen definiert werden, die eine Woche von heute ablaufen. Das System wertet die Objekte im System automatisch aus und fügt diese dem Satz täglich hinzu. In anderen Beispielen werden dynamische Definitionen eines Zeit Verweises ermöglicht, z. b. ein Filter, der auf "x Tage ab heute" basiert.

Zeitgesteuertes **Ereignis** : ein Übergangs Ereignis, das nach einem konfigurierten Zeitintervall auftritt oder ein bestimmtes Datum und eine bestimmte Uhrzeit erreicht wurde.

**Timeout** : ein Zeitraum, in dem MIM 2016 auf Genehmigungs Antworten wartet, bis eine Aktivität eskaliert wird.

**übergangssatz** : eine Menge, die in einer Definition einer Richtlinien Regel für die festgelegte Übergangsverwaltung verwendet wird. Die Richtlinie wird auf die Änderungen in der Set-Mitgliedschaft angewendet. dabei kann es sich um Objekte handeln, die die Gruppe eingeben oder belassen, abhängig von der Konfiguration von Tmpr.
<br/>

## <a name="u"></a>U

**entsperrende Gruppe** : eine Gruppe, in der die Mitgliedschaft der Gruppe von anderen Benutzern als dem Besitzer der Gruppe geändert werden kann.

**universelle Gruppe** : eine Gruppe mit universellem Bereich ist eine Active Directory Gruppe, die Mitglieder aus einer bestimmten Gesamtstruktur enthalten kann. Einer universellen Gruppe können Berechtigungen in einer beliebigen Domäne oder Gesamtstruktur zugewiesen werden. Verteilerlisten verfügen in der Regel über einen universellen Bereich.Eine Sicherheitsgruppe mit universellem Bereich kann Ressourcen innerhalb derselben Gesamtstruktur sichern.

**Aktualisierungs Anforderung** : eine Anforderung zum Ändern der Attribute einer Ressource.

**Usage-Schlüsselwort** : ein Verwendungs Schlüsselwort wird verwendet, um zu bestimmen, welche Such Bereiche für eine bestimmte Seite in der Benutzeroberfläche des Portals angezeigt werden. Jede Listen Ansichts Seite in der Benutzeroberfläche gibt NULL oder mehr Verwendungs Schlüsselwörter an, und die Benutzeroberfläche für diese Seite enthält alle Such Bereiche, die übereinstimmende Schlüsselwörter enthalten. Beim Erstellen von Such Bereichen können Kunden NULL oder mehr Schlüsselwörter pro Suchbereich angeben, um anzupassen, welche Such Bereiche für eine bestimmte Seite in der Benutzeroberfläche angezeigt werden. Sie wird auch verwendet, um zu bestimmen, welche Startseiten Ressource und Navigationsleisten Ressource welche Benutzergruppe angezeigt wird. Sie wird auch bei der Schema Verwaltung verwendet, um Schema Elemente zu schützen und zu bezeichnen, die von verschiedenen Komponenten von MIM benötigt werden.
<br/>

## <a name="w"></a>W

**Webportal** : eine Benutzeroberfläche, die von einer Software Anwendung über eine Komponente eines Webservers (z. b. IIS) implementiert wird.

**Webdienst** : eine Protokoll Schnittstelle für einen Dienst, der mit einem HTTP-basierten Protokoll implementiert wird.

**Workflow** : bei einem Workflow handelt es sich um einen Satz von elementaren Einheiten, die als ein Modell gespeichert werden, das einen realen Prozess beschreibt. Workflows bieten eine Möglichkeit, die Reihenfolge und abhängige Beziehungen zwischen Arbeits Elementen zu beschreiben. Diese Arbeit durchläuft das Modell von Anfang bis Ende, und Aktivitäten können von Personen oder Systemfunktionen ausgeführt werden. Anders ausgedrückt: Wenn eine Antwort auf eine Anforderung eine komplexe Verarbeitung erfordert, werden die Schritte in einem Workflow Objekt gekapselt. Workflows sind optionale Komponenten, die eng mit MPRs verknüpft sind. Workflows definieren eine Aktivität oder Aktivitäten, die während der Verarbeitung einer MPR auftreten müssen. MIM installiert mehrere Standard Workflows, die unverändert oder als Grundlage für einen benutzerdefinierten Workflow verwendet werden können.

   Beispiele für Workflow Aktivitäten:

- Senden einer automatisierten e-Mail-Nachricht, um eine Genehmigung anzufordern.
- Einschränken der Attribute, die ein Benutzer während einer benutzerdefinierten Suche sehen kann.
- Überprüfen einer neuen Gruppe mit AD DS-oder MIM-Richtlinien
- Hinzufügen oder Entfernen des Objekts aus dem Gültigkeitsbereich einer Synchronisierungs Regel.
   
  Um alle Verarbeitungsanforderungen in Ihrer Umgebung zu erfüllen, definiert die MIM-Architektur drei Arten von Workflows:
   
- Authentifizierung: führt eine zusätzliche Validierung der Benutzeridentität aus, bevor die Anforderung fortgesetzt wird.
- Autorisierung: überprüft eine Anforderung durch eine Abfolge von Aktivitäten, z. b. das Abrufen erforderlicher außerhalb der Genehmigung vor der Verarbeitung einer Anforderung.
- Aktion: verarbeitet alle weiteren Aktivitäten, nachdem die ursprüngliche Anforderung erfolgreich abgeschlossen wurde.
   
  Diese drei Workflows bilden einen Teil des Anforderungs Verarbeitungs Modells.

**Workflow Definition** : die Workflow Definition wird im vom Windows Workflow Foundation (WF) definierten XOML-Format gespeichert. Definiert die Aktivitäten, die Parameter für die Aktivitäten und die Reihenfolge, in der Sie ausgeführt werden sollen.

**Workflow-Designer** : die Entwurfszeit für die Erstellung von Workflows.

**Workflow Host** : die Serverkomponente, die die Ausführung von Workflows behandelt. In MIM 2016 ist der MIM 2016-Dienst der Workflow Host.

**Workflow Instanz** : eine Instanz einer Workflow Definition, die als Auswirkung auf eine Anforderung ausgeführt wird.

**Workflow Verwaltung** : eine MIM 2016-Funktion, die das Entwerfen von Workflows, deren Ausführung und deren Verwaltung behandelt. Die Workflow Verwaltung besteht aus den Workflow-Designer, der Anforderungs Verwaltung und dem Workflow Host.
