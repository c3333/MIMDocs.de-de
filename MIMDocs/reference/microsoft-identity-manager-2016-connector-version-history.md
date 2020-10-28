---
title: Connector – Versionsveröffentlichungsverlauf | Microsoft Docs
description: Dieses Thema listet alle Versionen des Connectors für Forefront Identity Manager (FIM) und Microsoft Identity Manager (MIM) auf.
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: daveba
editor: ''
reviewer: markwahl-msft
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3062058bc53a75c66959803ebb5b33849c11868b
ms.sourcegitcommit: babd0299472aa7e8c8d9af1b464bf4e91318aed8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2020
ms.locfileid: "92761061"
---
# <a name="connector-version-release-history"></a>Connector – Versionsveröffentlichungsverlauf

Connectors verknüpfen bestimmte verbundene Datenquellen mit Microsoft Identity Manager (MIM) und Azure AD Connect. (In Forefront Identity Manager wurden Connectors als Verwaltungs-Agents bezeichnet.) Viele der Connectors (z. b. Connectors zum Bereitstellen von Benutzern in Active Directory) werden als Teil der MIM-Synchronisierungs Dienst Installation und des Installationspakets Azure AD Connect bereitgestellt. Darüber hinaus werden weitere Connectors, wie z. b. an Verzeichnisserver von Drittanbietern, als separater Download bereitgestellt, damit Sie häufiger aktualisiert werden können, um Unterstützung für das Herstellen einer Verbindung mit von MIM aktualisierten Versionen von Zielsystemen von Drittanbietern hinzuzufügen.  

> [!NOTE]
> Dieses Thema ist in erster Linie nur auf FIM-und MIM-Connectors beschränkt. Diese Connectors werden für die Installation auf Azure AD Connect nicht unterstützt, es sei denn, Sie werden unten explizit aufgerufen. Freigegebene Connectors werden auf Azure AD Connect vorinstalliert, wenn ein Upgrade auf den angegebenen Build ausgeführt wird.


In diesem Thema werden alle Versionen des Pakets für generische Connectors aufgelistet, die separat von MIM freigegeben wurden.  Eine Liste der Connectors, die mit MIM unterstützt werden, finden Sie [unter Unterstützte Connectors in MIM 2016 SP1](../supported-management-agents.md).  Einige Partner haben auf diese Weise ihre eigenen Connectors erstellt, und eine vollständige Liste ist im wiki [FIM 2010 und MIM 2016 verfügbar: Verwaltungs-Agents von Partnern](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx).


Verwandte Links:

* [Die neuesten Connectors herunterladen](https://go.microsoft.com/fwlink/?LinkId=717495)
* [Generischer LDAP-Connector](microsoft-identity-manager-2016-connector-genericldap.md) – Referenzdokumentation
* [Generischer SQL-Connector](microsoft-identity-manager-2016-connector-genericsql.md) – Referenzdokumentation
* [Web Services-Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) – Referenzdokumentation
* [PowerShell-Connector](microsoft-identity-manager-2016-connector-powershell.md) – Referenzdokumentation
* [Lotus Domino-Connector](microsoft-identity-manager-2016-connector-domino.md) – Referenzdokumentation
* Referenz Dokumentation zum [SharePoint-Benutzerprofil Speicher-Connector](https://go.microsoft.com/fwlink/?LinkID=331344)

## <a name="1113020-september-2020"></a>1.1.1302.0 (September 2020)
### <a name="fixed-issues"></a>Behobene Probleme
- Graph-Connector
  - Ein Problem mit der Schema Aktualisierung für den */Beta* -Endpunkt wurde behoben.

## <a name="1113010-august-2020"></a>1.1.1301.0 (2020)
### <a name="fixed-issues"></a>Behobene Probleme
- Graph-Connector
  - Es wurde ein Fehler behoben, der durch einen leeren *usertype* -Attribut Wert verursacht wurde.
  - Es wurde ein Fehler behoben, der bewirkt, dass der Connector-Cache für Delta Importe aufgrund von Ermittlungs Fehlern nicht lesbar
  - Dienst-und Proxy gemeldete Timeouts können automatisch wiederholt werden.
  - Aktualisierte Schema Ermittlung für den */Beta* -Endpunkt 
### <a name="enhancements"></a>Erweiterungen
- Graph-Connector
  - Unterstützung für das Lesen und Schreiben von Werten von benutzerdefinierten Verzeichnis Erweiterungs Attributen hinzugefügt
  - Unterstützung für das Lesen der *Dienst Prinzipal* Mitglieder von Gruppen im */Beta* -Endpunkt hinzugefügt 
  - Leistungsverbesserungen bei Delta Import Läufen im Zusammenhang mit der Schema Ermittlung
  - Der Graph-Connector kann jetzt externe Mitglieder Benutzer einladen.

  > [!NOTE]
  > Wenn Sie die Gast Einladung in Build 1.1.1170.0 des-Verbindungs-Connector verwendet haben, aktualisieren Sie Ihre Synchronisierungs Regeln mit der folgenden Logik:

  - Ausgehende Datenflüsse
    - Ein Benutzer wird eingeladen, wenn die Erstellung des Benutzers exportiert wird, und der Export enthält ein e- *Mail* -Attribut, aber kein *userPrincipalName-Attribut* .  Wenn " *userPrincipalName* " angegeben wird, wird ein Benutzer erstellt, anstatt eingeladen zu werden.
    - Das *usertype* -Attribut definiert nur, ob ein Benutzer ein *Mitglied* oder ein *Gast* wird (standardmäßig *Member* , wenn nicht festgelegt).
  - Eingehende Datenflüsse
    - *UserPrincipalName* -Attributwerte externer Benutzer werden "unverändert" gerendert.

## <a name="1111700-april-2020"></a>1.1.1170.0 (2020)
### <a name="fixed-issues"></a>Behobene Probleme
- Generischer SQL-Connector
   - Korrigiert eines Fehlers mit Updates für Abfrage basierte Exportstrategie und mehrwertige Attribute
- Lotus Notes-Connector
   - Gruppen aus sekundären Notizen Adressbücher werden nicht mehr durch den *Adminp* -Prozess gelöscht. Jetzt wird ein direkter Löschvorgang verwendet.
- Generischer LDAP-Connector
   - Es wurde ein Fehler mit LDAP-Verzeichnis Vorgangs Attributen behoben, z. b. " *pwdupdatetime* ", nicht im Schema sichtbar.
### <a name="enhancements"></a>Erweiterungen
- Graph-Connector   
   - UPNs von externen Gastbenutzern werden nicht mehr unverändert gerendert, sondern im Connector-Bereich angezeigt, sodass Sie wie e-Mails aussehen
   - Zusätzliche Unterstützung für die Bereitstellung von B2B-Gastbenutzern

   Zum Einladen von B2B-Gastbenutzern müssen Sie folgende Schritte ausführen:
   - Erteilen von Berechtigungen zum Einladen von Gästen zu ihrer Azure AD Anwendung, die dem Graph-Connector zugeordnet ist
   - Vervollständigen der Connector-Konfiguration zum einladen externer Benutzer: Festlegen der Umleitungs-URL für Einladungen (obligatorisch) und auswählen, ob Einladungs-e-Mails gesendet werden
   - Festlegen obligatorischer Attribute in der Regel für die ausgehende Synchronisierung:
     - "Guest" => *usertype* (nur ursprünglicher Flow)
     - externe e-Mail-Adresse => *userPrincipalName*
     - CustomExpression ("CN =" + csobjectid + ", Object = User") => *DN* (nur ursprünglicher Flow)
     - csobjectid => *ID* (nur ursprünglicher Flow)

## <a name="1111300-february-2020"></a>1.1.1130.0 (Februar 2020)
### <a name="fixed-issues"></a>Behobene Probleme
- Graph-Connector
   - Der Export schlägt nicht mehr fehl, wenn versucht wird, ein Mitglied zu einer Gruppe hinzuzufügen, die bereits hinzugefügt wurde.
   - die Unterstützung des virtuellen Attributs "export_password" ist korrigiert, es muss nicht mehr der Attribut Fluss "Password" eingerichtet werden.
   - Der Export Fluss von mehrwertigen Zeichen folgen Attributen wurde korrigiert.
   - Es wurden mehrere Fehler behoben, die den Delta Import
   - Verschiedene potenzielle Fehler im datetime-Format behoben
- Generischer SQL-Connector
   - Diverse Fehler bei der Benutzeroberfläche
   - Korrigieren falscher Verweis Behandlung beim Delta Import
   - Es wurde ein Fehler mit der SQL Änderungsnachverfolgung-Delta Import Strategie und mehrwertigen Tabellen zum ordnungsgemäßen Importieren von Gruppen Mitgliedschafts Änderungen behoben.
   - Es wurde ein Fehler behoben, bei dem Attributwerte beim Export nicht gelöscht wurden.
   - Es wurde ein Fehler behoben, bei dem das letzte Element eines mehrwertigen Verweis Attributs beim Export nicht gelöscht wurde.
   - Es wurde ein Fehler mit der Schema Aktualisierung behoben, sodass Verweis Attribute auf Zeichen folgen festgelegt wurden.
   - Es wurde ein Fehler behoben, bei dem Werte gespeicherter Prozeduren auf 397 bytes gekürzt wurden.
   - Korrigiert eines Fehlers bei Oracle-Tabellen und-Sichten die Groß-/Kleinschreibung beachten
- Lotus Notes-Connector
   - Verbesserte Leistung beim Importieren von Gruppenmitgliedern
   - Der vollständige Import schlägt mit NULL-Verweis Fehlern nicht mehr fehl.
   - Behebung eines Fehlers mit Notizen zum Löschen von Postfächern beim Festlegen der ACL
   - Leere Gruppennamen verursachen keine Delta Import Fehler mehr
   - Es wurde ein Fehler behoben, bei dem nicht Druck bare Zeichen nach Zeichen folgen Werten in Attributen verbleiben.
- Generischer LDAP-Connector
   - Beim Delta Import wird der Literalwert "Replace" nicht mehr angezeigt, wenn im Quell Protokoll kein Wert festgelegt ist.
### <a name="enhancements"></a>Erweiterungen
- Graph-Connector
   - Unterstützung für unabhängige Clouds und Möglichkeit zum Konfigurieren von Anmelde-und Graph-Ressourcen-URLs hinzugefügt
   - Nicht unterstützte Attribute werden nach der Schema Ermittlung herausgefiltert und in den Connector-Eigenschaften ausgeblendet.

## <a name="4418001-july-2019"></a>4.4.1800.1 (Juli 2019)
### <a name="enhancements"></a>Verbesserungen:
- SharePoint-Benutzerprofil Speicher-Connector
   - Unterstützung für SharePoint Server 2019 hinzugefügt. Der Connector steht im [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=279713) als Download zur Verfügung. 

## <a name="119530-june-2019"></a>1.1.953.0 (Juni 2019)

### <a name="fixed-issues"></a>Behobene Probleme:
- Generischer LDAP-Connector
   - Der Delta Import schlägt nicht mehr fehl, wenn das Feld "Änderungen" in Oracle Internet Directory leer ist
- Generischer SQL-Connector
   - Es wurde ein Problem mit einer benutzerdefinierten SQL-Abfrage Import Strategie mit startIndex-und endIndex-Parametern behoben, sodass nur die erste Seite importiert werden konnte
   - Es wurde ein Problem mit der Import Strategie der Tabelle/Sicht behoben, wenn aus MS SQL gelesen wurde, sodass nur die erste Seite importiert werden konnte.
- Graph-Connector:
   - Organisations Kontakte werden nun ordnungsgemäß behandelt, e-Mails fehlen nicht mehr
   - Es wurde ein Problem mit den Import-und Export Vorgängen der Manager-Attribute Der Connector schlägt nicht mehr fehl, wenn der Manager leer ist. Der Manager-Wert wurde in Azure AD ordnungsgemäß aktualisiert.
   - Ein Problem mit dem Delta Import leerer Werte wurde behoben.
   - Es wurde ein Problem behoben, bei dem der Connector beim Delta Import abstürzen konnte, wenn die lokale Cachedatei beschädigt
   - Es wurden mehrere Probleme durch einen falschen Export von leeren Werten behoben, oder wenn nur der Wert des Attribut Werts geändert wurde.
   - Der Connector verarbeitet nun Lösch-/hinzugriffsvorgänge für dasselbe Attribut während des Export Vorgangs ordnungsgemäß.
   - Es wurden mehrere Probleme bei lang andauernden Importen und Exporten behoben, wenn während der Connector-Ausführung Zugriffs Token ablaufen. Der Connector erneuert nun bei Bedarf Zugriffs Token, ohne dass ein Fehler auftritt.
   - Es wurde ein Problem behoben, bei dem das letzte Mitglied einer Gruppe nicht gelöscht wurde.
### <a name="enhancements"></a>Verbesserungen:
- Generischer SQL-Connector
   - der CommandTimeout-Parameter eines Daten Readers ist auf das Connector-Timeout festgelegt. Wenn Sie Abfragen mit langer Ausführungsdauer ausführen, die mehr als 30 Sekunden dauern müssen, können Sie den Parameterwert "Command Timeout" im Abschnitt "Konnektivität" erhöhen.
- Graph-Connector: 
   - Vollständige Import Strategie für Multithread-Gruppenmitgliedschaften zur Verbesserung der Import Leistung Der Delta Import ist ein Single Thread-Vorgang.
   - Unterstützung für komplexe Schema Typen wurde hinzugefügt, sodass Attribute wie onpremisesextentionattribute. * jetzt verfügbar sind.
   - Unterstützung für export_password-Attribut wurde hinzugefügt, um Export-Change-not-imporportiert-Fehler zu vermeiden und das anfängliche Kennwort im Connector-Bereich nicht anzuzeigen. Das Verhalten ähnelt anderen ECMA2-Connectors.
   - Es wurde ein Handler zur Unterstützung der Einschränkung von HTTP-Anforderungen hinzugefügt. Wenn Azure AD Replikat zu viele Anforderungen von einem Client empfängt, kann es mit Retry-After Anweisung Antworten. Der Connector hält an und führt einen Wiederholungsversuch aus.
   - Das Delta Import Profil wird nicht mehr gestartet, wenn Abfrage Filter definiert sind. Wenn Sie nur bestimmte Objekte aus Azure AD importieren möchten, z. b. Wenn Benutzer den Nachnamen haben, der mit "*" beginnt, wird die Delta Importfunktion blockiert.


## <a name="119130-january-2019"></a>1.1.913.0 \( Januar 2019\)

### <a name="fixed-issues"></a>Behobene Probleme:

* SQL (generisch):
    * Es wurde ein Regressions Fehler auf dem generischen SQL-Connector behoben, bei dem nur die ersten 5000 Objekte gelesen wurden.
* Graph-Connector:
    * Es wurde ein Problem behoben, bei dem der Graph-API Connector einen Mandanten nicht lesen/schreiben konnte, oder einen neuen Connector erstellen, wenn die Option "Beta" bei der Konnektivität ausgewählt ist.

### <a name="enhancements"></a>Verbesserungen:

* Graph-Connector:
    * Legen Sie das TLS 1,2-Protokoll als Standardwert für den Graph-Connector fest.
* Generischer SQL-Connector:
    * Der generische SQL-Connector wurde mit Oracle 12C und Oracle 18C getestet.

## <a name="119110"></a>1.1.911.0

> [!NOTE]  
> Diese Connector-Buildversion dient nur zur Verwendung in Azure AD Connect Bereitstellungen und wird nicht für die Verwendung in MIM-bereit Stellungen bereitgestellt.
> 
> Im Vergleich zur vorherigen Connector-Version enthält Sie keine Verbesserungen oder Updates für MIM-Kunden.

-  Aktualisiert die nicht Standardconnectors (z. b. generischer LDAP-Connector und generischer SQL-Connector), die mit Azure AD Connect ausgeliefert wurden.  

## <a name="118610"></a>1.1.861.0

### <a name="fixed-issues"></a>Behobene Probleme:
* Titel für alle Connector-Setup von "Forefront zu Microsoft" ändern

* Lotus Notes: 
    * Fehler in com mit Lotus haben manchmal keine Fehler zurückgegeben.
* LDAP (generisch):
    * Gldap-zusätzliches Symbol für di mit Ping-Verzeichnisserver
* Webdienste (generisch):
    * Fehler beim Exportieren der JSON-Verarbeitung.
* SQL (generisch):
    * Binäres Attribut exportieren
    * Objekttypen können nicht Teil Zeichenfolgen sein.
    * Änderungen in mehrwertigen Tabellen werden im Vorgang "Delta Import" nicht nachverfolgt, wenn "Delta-Strategie" "Änderungsnachverfolgung" ist.
* Graph-Connector (Public Preview)
    * Fehler beim Löschen von Gruppen.
    * Aktualisieren von User-Agent auf den HTTP-Header
    * Der Connector überprüft Client-ID und geheimer Client Schlüssel nicht.
    * Der Mandanten Name muss gekürzt werden.

### <a name="enhancements"></a>Verbesserungen:
* Lotus Notes: * Hinzufügen der Fähigkeit zum Erhöhen des Timeouts über die Benutzeroberfläche
* Der Graph-Connector (Public Preview) * das Kenn Wort Attribut wird nach dem Import gefiltert, um "Export-not-imporportiert" auszuschließen.
    * Unterstützung für $Filter Abfrage Parameter-beschränkt auf Vorgänge mit allen Filtern, die in Delta Abfragen funktionieren, funktioniert auch im Connector * aktualisiert, um Nextlink direkt zu verwenden, anstatt skiptoken zu extrahieren. [here](https://developer.microsoft.com/en-us/graph/docs/concepts/paging)

## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Behobene Probleme:
* Das Problem mit ConnectorsLog System.Diagnostics.EventLogInternal.InternalWriteEvent wurde behoben (Meldung: „Ein an das System angeschlossenes Gerät funktioniert nicht.“)
* Sie müssen in diesem Connectorrelease die Bindungsumleitung von 3.3.0.0-4.1.3.0 zu 4.1.4.0 in „miiserver.exe.config“ aktualisieren.
* Webdienste (generisch):
    * Das Problem, dass gültige JSON-Antworten nicht im Konfigurationstool gespeichert werden konnten, wurde behoben.
* SQL (generisch):
    * Beim Export wird stets nur eine Updateabfrage für den Löschvorgang generiert. Ein Feature zum Generieren einer Löschabfrage wurde hinzugefügt.
    * Die SQL-Abfrage, mit der Objekte für den Delta Import Vorgang abgerufen werden, wenn "Delta Strategie" den Wert "Änderungsnachverfolgung" hat, wurde korrigiert. In dieser Implementierung bekannte Einschränkung: beim Delta Import mit dem Modus "Änderungsnachverfolgung" werden keine Änderungen in mehrwertigen Attributen nachverfolgt.
    * Es wurde die Möglichkeit hinzugefügt, eine DELETE-Abfrage zu generieren, wenn es erforderlich ist, den letzten Wert eines mehrwertigen Attributs zu löschen, und diese Zeile enthält keine anderen Daten mit Ausnahme von Wert, die Sie löschen müssen.
    * Das Problem mit der Behandlung von System.ArgumentException bei der Implementierung von OUTPUT-Parametern durch SP wurde behoben.
    * Falsche Abfrage zum Ausführen des Export Vorgangs in das Feld mit dem varbinary (max)-Typ.
    * Das Problem, dass die parameterList-Variable zweimal initialisiert wird (in den Funktionen ExportAttributes und GetQueryForMultiValue), wurde behoben.


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Behobene Probleme:

* Lotus Notes:
  * Option zum Filtern benutzerdefinierter Zertifizierer
  * Beim Importieren der Klasse „ImportOperations“ wurde die Definition korrigiert, welche Vorgänge im Modus „Ansichten“ und im Modus „Suche“ ausgeführt werden können.
* LDAP (generisch):
  * Das Verzeichnis „OpenLDAP“ verwendet DN als Anker, statt „entryUUI“. Neue Option für den gldap-Connector, mit dem der Anker geändert werden kann
* SQL (generisch):
  * Der Export in das Feld wurde korrigiert, das einen varbinary (max)-Typ aufweist.
  * Beim Hinzufügen von Binärdaten aus einer Datenquelle zum CSEntry-Objekt tritt ein Fehler bei der DataTypeConversion-Funktion bei null Bytes auf. Korrigierte DataTypeConversion-Funktion der CSEntryOperationBase-Klasse.




### <a name="enhancements"></a>Verbesserungen:

* SQL (generisch):
  * Die Möglichkeit, den Modus zum Ausführen gespeicherter Prozeduren mit benannten oder nicht benannten Parametern zu konfigurieren, wurde in einem Konfigurationsfenster des Agents für die generische SQL-Verwaltung auf der Seite „Globale Parameter“ hinzugefügt. Auf der Seite "globale Parameter" befindet sich das Kontrollkästchen mit der Bezeichnung "benannte Parameter zum Ausführen einer gespeicherten Prozedur verwenden", die für den Modus für die Ausführung gespeicherter Prozeduren mit benannten Parametern zuständig ist.
    * Derzeit kann die Funktion zur Ausführung gespeicherter Prozeduren mit benannten Parametern nur für IBM DB2- und MSSQL-Datenbanken genutzt werden. Bei Oracle-und MySQL-Datenbanken funktioniert dieser Ansatz nicht: 
      * Die SQL-Syntax von MySQL unterstützt keine benannten Parameter in gespeicherten Prozeduren.
      * Der ODBC-Treiber für Oracle unterstützt benannte Parameter für benannte Parameter in gespeicherten Prozeduren nicht.)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Behobene Probleme:

* Webdienste (generisch):
  * Ein Problem, das die Erstellung eines SOAP-Projekts verhindert, wenn zwei oder mehr Endpunkte vorhanden sind, wurde behoben.
* SQL (generisch):
  * Beim Importvorgang wurde die Zeit von GSQL nicht ordnungsgemäß konvertiert, wenn die Speicherung im Connectorbereich erfolgte. Das Standardformat für Datum und Uhrzeit für den Connectorbereich von GSQL wurde von „JJJJ-MM-TT hh:mm:ssZ“ in „JJJJ-MM-TT HH:mm:ssZ“ geändert.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Behobene Probleme:

* Webdienste (generisch):
  * Das wsconfig-Tool hat das JSON-Array nicht ordnungsgemäß aus "Sample Request" für die Rest-Dienst Methode konvertiert. Dadurch wurden Probleme bei der Serialisierung dieses JSON-Arrays für die REST-Anforderung verursacht.
  * Das Konfigurationstool für Webdienstconnectors unterstützt in Namen von JSON-Attributen keine Bereichssymbole. 
    * Ein Ersetzungs Muster kann der WSConfigTool.exe.config Datei manuell hinzugefügt werden, z. b.  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > Der JSONSpaceNamePattern-Schlüssel ist erforderlich, da beim Export die folgende Fehlermeldung angezeigt wird: „Ein leerer Name ist unzulässig.“. 

* Lotus Notes:
  * Wenn die Option **benutzerdefinierte Zertifikate für Organisation/Organisationseinheiten zulassen** deaktiviert ist, schlägt der Connector während des Exports (aktualisieren) fehl, nachdem der Export Fluss alle Attribute in Domino exportiert wurde. zum Zeitpunkt des Exports wird jedoch KeyNotFoundException an Sync zurückgegeben. 
    * Der Grund: Beim Umbenennungsvorgang tritt ein Fehler auf, wenn versucht wird, DN (UserName-Attribut) durch Ändern eines der folgenden Attribute zu ändern:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  * Wenn die Option zum **Zulassen benutzerdefinierter Zertifizierer für Organisationen/Organisationseinheiten** aktiviert ist, die erforderlichen Zertifizierer aber noch leer sind, tritt ein KeyNotFoundException-Fehler auf.

### <a name="enhancements"></a>Verbesserungen:

* SQL (generisch):
  * **Szenario: überarbeitet implementiert:** „*“-Feature
  * **Lösungsbeschreibung:** geänderter Ansatz für die [Behandlung referenzierter, mehrwertiger Attribute](microsoft-identity-manager-2016-connector-genericsql.md)


### <a name="fixed-issues"></a>Behobene Probleme:

* Webdienste (generisch):
  * Server Konfiguration kann nicht importiert werden, wenn der Webservice-Connector vorhanden ist
  * Der WebService-Connector funktioniert nicht mit mehreren Webdiensten.

* SQL (generisch):
  * Für referenzierte Einzelwertattribute werden keine Objekttypen aufgeführt.
  * Der Deltaimport bei der Strategie der Änderungsnachverfolgung löscht Objekte, wenn der Wert aus der mehrwertigen Tabelle entfernt wird.
  * OverflowException im GSQL-Connector mit DB2 bei AS/400

Lotus:
  * Eine Option zum Aktivieren/Deaktivieren der Suche von Organisationseinheiten vor dem Öffnen der Seite „GlobalParameters“ wurde hinzugefügt.

## <a name="114430"></a>1.1.443.0

Veröffentlicht: März 2017

### <a name="enhancements"></a>Erweiterungen

* SQL (generisch):</br>
  **Szenariosymptome:**   Es ist eine bekannte Einschränkung mit dem SQL-Connector, bei der nur ein Verweis auf einen Objekttyp zulässig ist und der Querverweis mit Membern erforderlich ist. </br>
  **Lösungsbeschreibung:** Im Verarbeitungsschritt für Verweise, bei denen die Option „*“ ausgewählt wird, werden ALLE Kombinationen von Objekttypen an das Synchronisierungsmodul zurückgegeben.

> [!Important]
> - Es werden viele Platzhalter erstellt.
> - Es muss sichergestellt werden, dass die Benennung objekttypübergreifend eindeutig ist.


* LDAP (generisch):</br>
  **Szenario:** Wenn in einer bestimmten Partition nur einige Container ausgewählt werden, wird die Suche trotzdem in der gesamten Partition durchgeführt. Bestimmte werden vom Synchronisierungs Dienst, aber nicht von Ma gefiltert, was zu Leistungseinbußen führen kann. </br>

  **Lösungsbeschreibung:** Der Code des GLDAP-Connectors wurde geändert, um es zu ermöglichen, dass jeweils alle Container und Suchobjekte durchlaufen werden, anstatt die gesamte Partition zu durchsuchen.


* Lotus Domino:

  **Szenario:** Unterstützung für das Löschen von Domino-E-Mails zum Entfernen einer Person während eines Exportvorgangs. </br>
  **Lösung:** Unterstützung eines konfigurierbaren E-Mail-Löschvorgangs zum Entfernen einer Person während eines Exportvorgangs.

### <a name="fixed-issues"></a>Behobene Probleme:
* Webdienste (generisch):
  * Wenn die Dienst-URL in standardmäßigen SAP wsconfig-Projekten über das Webservice-Konfigurations Tool geändert wird, tritt der folgende Fehler auf: ein Teil des Pfads konnte nicht gefunden werden.

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* LDAP (generisch):
  * Für den GLDAP-Connector sind in AD LDS nicht alle Attribute sichtbar
  * Fehler im Assistenten, wenn für das LDAP-Verzeichnisschema keine UPN-Attribute erkannt werden
  * Deltaimporte schlagen mit Ermittlungsfehlern fehl, die während des vollständigen Imports nicht vorhanden sind, wenn das „objectclass“-Attribut nicht ausgewählt ist
  * Die Konfigurationsseite "Partitionen und Hierarchien konfigurieren" zeigt keine Objekte an, deren Typ gleich der Partition für neuartige Server im generischen  
  LDAP MA (generisch) entspricht. Es wurden nur Objekte der RootDSE-Partition angezeigt.


* SQL (generisch):
  * Fehler vom Typ „Wasserzeichen für SQL (generisch): Mehrwertiges Attribut für Deltaimport nicht importiert“ behoben
  * Beim Exportieren von gelöschten/hinzugefügten Werten von mehrwertigen Attributen werden diese in der Datenquelle nicht gelöscht/hinzugefügt.  


* Lotus Notes:
  * Ein spezifisches Feld für den „Vollständigen Namen“ wird im Metaverse richtig angezeigt, aber beim Exportieren nach Notes ist der Wert für das Attribut ein Nullwert oder leer.
  * Fehler aufgrund von doppeltem Zertifizierer behoben
  * Wenn das Objekt ohne Daten im Lotus Domino Connector mit anderen Objekten ausgewählt wird, tritt beim vollständigen Import der Ermittlungsfehler auf.
  * Wenn der Deltaimport auf dem Lotus Domino Connector ausgeführt wird, gibt der Dienst „Microsoft.IdentityManagement.MA.LotusDomino.Service.exe“ am Ende der Ausführung ggf. einen Anwendungsfehler aus.
  * Die Gesamtzahl der Gruppenmitgliedschaften funktioniert problemlos und wird beibehalten, außer wenn Sie den Export zum Entfernen eines Benutzers aus der Mitgliedschaft ausführen, wird der Benutzer mit einem Update als erfolgreich angezeigt, aber der Benutzer wird nicht aus der Mitgliedschaft in Lotus Notes entfernt.
  * Die Möglichkeit, den Export Modus als "am unteren Rand anfügen" zu wählen, wurde in der Konfigurations-GUI von Lotus MA hinzugefügt, um beim Export für mehrwertige Attribute am Ende neue Elemente anzufügen.
  * Der Connector fügt die erforderliche Logik zum Löschen der Datei aus dem Mailordner und dem ID-Tresor hinzu.
  * Das Löschen der Mitgliedschaft für übergreifende NAB-Member funktioniert nicht.
  * Werte sollten erfolgreich aus dem mehrwertigen Attribut gelöscht werden

## <a name="111170"></a>1.1.117.0
Veröffentlicht März 2016

**Neuer Connector**  
Erste Version des [Generischer SQL-Connector](microsoft-identity-manager-2016-connector-genericsql.md)

**Neue Features:**

* Generischer LDAP-Connector:
  * Zusätzliche Unterstützung für den Deltaimport mit Isode.
* Webdienst-Connector:
  * Die csEntryChangeResult- und setImportErrorCode-Aktivität wurde aktualisiert, um das Zurückgeben von Fehlern der Objektebene an das Synchronisierungsmodul zu ermöglichen.
  * Aktualisierte die „SAP6“ und „SAP6User“ Vorlagen dahingehend, das die neue Objektebenenfehler-Funktionalität verwendet wird.
* Lotus Domino-Connector:
  * Für den Export benötigen Sie einen Zertifizierer pro Adressbuch. Sie können jetzt dasselbe Kennwort für alle Zertifizierer verwenden, um die Verwaltung zu vereinfachen.

**Behobene Probleme:**

* Generischer LDAP-Connector:
  * Für IBM Tivoli DS wurden einige Verweisattribute nicht richtig erkannt.
  * Für Open LDAP wurden während eines Deltaimports Leerzeichen am Anfang und Ende der Zeichenfolge abgeschnitten.
  * Fehler beim Export für Novell und NetIQ bei dem ein Objekt zwischen Organisationseinheiten/Containern verschoben und gleichzeitig umbenannt wurde.
* Webdienst-Connector:
  * Wenn der Webdienst mehrere Endpunkte für dieselbe Bindung hat, hat der Connector diese Endpunkte nicht ordnungsgemäß ermittelt.
* Lotus Domino-Connector:
  * Fehler beim Export des Attributs fullName in eine „Mail-in-Datenbank“.
  * Ein Export, der einer Gruppe Elemente sowohl hinzufügte als auch aus ihr entfernte, exportierte nur die hinzugefügten Elemente.
  * Ist ein Notes-Dokument ungültig (weil FALSE für das Attribut „isValid“ festgelegt ist), kann der Connector keine Verbindung herstellen.

## <a name="older-releases"></a>Ältere Versionen
Vor März 2016 wurden die Connectors als Support-Themen veröffentlicht.

**Generisches LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, September 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015 März
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, Januar 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, September 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014 März

**Webservices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, September 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, September 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, September 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015 März
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, August 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, Februar 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, Oktober 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, August 2013

## <a name="troubleshooting"></a>Problembehandlung 

> [!NOTE]
> Beim Aktualisieren von Microsoft Identity Manager oder AADConnect mit Nutzung eines ECMA2-Connectors. 

Sie müssen die Connectordefinition nach dem Upgrade entsprechend aktualisieren, oder Sie erhalten den folgenden Fehler im Ereignisprotokoll der Anwendung mit der Warnungs-ID 6947: „Assemblyversion in der AAD-Connector-Konfiguration („X.X.XXX.X“) ist älter als die aktuelle Version („X.X.XXX.X“) von „C:\Programme\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll“.“

So aktualisieren Sie die Definition
* Öffnen Sie die Eigenschaften für die Connectorinstanz.
* Klicken Sie auf die Registerkarte „Verbindung/Verbinden mit“.
  * Geben Sie das Kennwort für das Connectorkonto ein.
* Klicken Sie nacheinander auf die einzelnen Eigenschaftenregisterkarten.
  * Wenn dieser Connectortyp eine Registerkarte „Partitionen“ mit einer Schaltfläche „Aktualisieren“ aufweist, klicken Sie auf die Schaltfläche „Aktualisieren“ auf dieser Registerkarte.
* Nachdem Sie auf alle Eigenschaftenregisterkarten zugegriffen haben, klicken Sie auf die Schaltfläche „OK“, um die Änderungen zu speichern.

## <a name="other-connectors"></a>Weitere Connectors

Zusätzlich zu den oben aufgeführten Connectors wurden Connectors für SharePoint und ein Legacy-Connector für Windows Azure Active Directory ebenfalls getrennt von MIM verteilt.

### <a name="sharepoint-user-profile"></a>SharePoint-Benutzerprofil

Der Forefront Identity Manager-Connector für SharePoint-Benutzerprofil Speicher unterstützt Sie bei der Synchronisierung von Identitätsinformationen mit dem Benutzerprofil Speicher in SharePoint 2013 und SharePoint 2016.   Version 4.3.2430.0 dieses Connector, veröffentlicht 12/19/2016, kann aus dem [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41164)heruntergeladen werden.

Weitere Informationen zu diesem Connector finden Sie unter [Hotfixrollup](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man) und Anweisungen zur [Verwendung einer MIM-Beispiellösung in SharePoint Server 2016](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### <a name="forefront-identity-manager-connector-for-windows-azure-active-directory-legacy-connector"></a>Forefront Identity Manager-Connector für Windows Azure Active Directory (Legacy-Connector)

Der Azure AD-Connector für FIM war eine frühe Technologie zum Synchronisieren von Identitätsinformationen mit Azure Active Directory. Der Azure AD-Connector für FIM, Version 1.0.6635.0069 ab dem 19. Februar 2014, ist am Funktions einfrieren. [Er](https://www.microsoft.com/en-us/download/details.aspx?id=41166) empfängt keine Updates, wird jedoch weiterhin unterstützt. Die Lösung der Verwendung von FIM und der Azure AD-Connector wurde durch [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect)abgelöst. Microsoft empfiehlt, mit diesem Connector keine neue Bereitstellung zu starten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [allgemeinen LDAP-Connector](microsoft-identity-manager-2016-connector-genericldap.md) -Referenz Dokumentation erfahren Sie mehr über die generische Referenz Dokumentation für den[SQL-Connector](microsoft-identity-manager-2016-connector-genericsql.md) Weitere Informationen zur Referenz Dokumentation für den [Web Services](microsoft-identity-manager-2016-ma-ws.md) -Connector Weitere Informationen zur [PowerShell-Connector](microsoft-identity-manager-2016-connector-powershell.md) -Referenz Dokumentation weitere Informationen zur Referenz Dokumentation für den [Lotus Domino](microsoft-identity-manager-2016-connector-domino.md) -Connector
