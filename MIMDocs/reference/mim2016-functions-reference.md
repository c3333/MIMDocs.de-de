---
title: "Funktionsreferenz für Microsoft Identity Manager 2016 | Microsoft-Dokumentation"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Funktionsreferenz für Microsoft Identity Manager 2016


In Microsoft Identity Manager (MIM) 2016 ermöglichen Ihnen Funktionen, Attributwerte zu ändern, bevor Sie sie in ein Ziel in einer Funktionsaktivität oder deklarativen Bereitstellung einfließen lassen. Dieses Dokument bietet Ihnen einen Überblick über die verfügbaren Funktionen und eine Beschreibung ihrer Verwendung.

Konfigurieren von Attributflusszuordnungen ist eine elementare Aufgabe beim Konfigurieren von Synchronisierungsregeln. Die einfachste Form einer Attributflusszuordnung ist eine direkte Zuordnung. Wie der Name suggeriert, nimmt eine direkte Zuordnung den Wert eines Quellattributs und wendet ihn auf das konfigurierte Zielattribut an. In manchen Fällen müssen entweder vorhandene Attributwerte geändert oder neue Attributwerte berechnet werden, bevor das System sie auf ein Ziel anwendet.

Funktionen sind eine integrierte Methode zum Definieren des Änderungstyps, den das Synchronisierungsmodul beim Generieren eines Attributwerts für ein Ziel anwenden muss.

In MIM können Sie die vorhandenen Funktionen in folgende Kategorien gruppieren:

-   **Datenbearbeitungsfunktionen**. Funktionen zum Ausführen zahlreicher Vorgänge zur Zeichenfolgenbearbeitung.

-   **Funktionen für den Datenabruf**. Funktionen zum Extrahieren von Daten von Attributwerten.

-   **Funktionen zum Generieren von Daten**. Funktionen zum Generieren von Werten.

-   **Logikfunktionen**. Funktionen zur Durchführung von Vorgängen, die auf Bedingungen basieren.

Die folgenden Abschnitte enthalten weitere Informationen zu den Funktionen in jeder Kategorie.

## <a name="data-manipulation-functions"></a>Datenbearbeitungsfunktionen

Datenbearbeitungsfunktionen werden zum Ausführen zahlreicher Vorgänge zur Zeichenfolgenbearbeitung verwendet.

| Concatenate        |   |
|--------------------|-------------------------|
| Beschreibung        | Die Concatenate-Funktion wird verwendet, um zwei oder mehr Zeichenfolgen zu verketten.                                                                                                       |
| Funktionssignatur | Zeichenfolge1 + Zeichenfolge2...                                                                                                                                                     |
| Eingaben             | Zwei oder mehr Zeichenfolgen                                                                                                                                                        |
| Vorgänge         | Alle Parameter der Eingabezeichenfolge werden miteinander verkettet.                                                                                                              |
| Ausgabe             | Eine Zeichenfolge        |


| UpperCase         |         |
|-------------------|---------|
| Beschreibung        | Die UpperCase-Funktion konvertiert alle Zeichen in einer Zeichenfolge in Großbuchstaben.         |
| Funktionssignatur | String UpperCase(Zeichenfolge)                                                                                                                                                   |
| Eingaben             | Eine Zeichenfolge                                                                                                                                                                 |
| Vorgänge         | Alle Kleinbuchstaben des Eingabeparameters werden in Großbuchstaben konvertiert. Beispiel: UpperCase("test") ergibt „TEST“.                                     |
| Ausgabe             | Eine Zeichenfolge                                                              |


| LowerCase          |                                 |
|--------------------|---------------------------------|
| Beschreibung        | Die LowerCase-Funktion konvertiert alle Zeichen in einer Zeichenfolge in Kleinbuchstaben.                                                                                                  |
| Funktionssignatur | String LowerCase(Zeichenfolge)                                                                                                                                                   |
| Eingaben             | Eine Zeichenfolge                                                                                                                                                                 |
| Vorgänge         | Alle Großbuchstaben des Eingabeparameters werden in Kleinbuchstaben konvertiert. Beispiel: LowerCase(“TeSt”) ergibt „test“.                                     |
| Ausgabe             | Eine Zeichenfolge               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Beschreibung        | Die ProperCase-Funktion konvertiert das erste Zeichen jedes mit Leerzeichen getrennten Worts in eine Zeichenfolge mit Großbuchstaben, und alle anderen Zeichen werden in Kleinbuchstaben konvertiert.           |
| Funktionssignatur | String ProperCase(Zeichenfolge)                                                                                                                                                  |
| Eingaben             | Eine Zeichenfolge                                                                                                                                                                 |
| Vorgänge         | Das erste Zeichen jedes mit Leerzeichen getrennten Worts im Eingabeparameter wird in einen Großbuchstaben und alle anderen Zeichen in Großbuchstaben werden in Kleinbuchstaben konvertiert. Wenn ein Wort im Eingabeparameter mit einem nicht-alphabetischen Zeichen beginnt, wird das erste Zeichen des Worts nicht in Großbuchstaben konvertiert. <br/> Beispiele: <br/> – ProperCase("TEsT") ergibt „Test“. <br/> – ProperCase("britta simon") ergibt „Britta Simon“. <br/>– ProperCase(" TEsT") ergibt „ Test“. <br/> – ProperCase("\$TEsT") ergibt „\$Test“.|
| Ausgabe             | Eine Zeichenfolge      |


| LTrim              |      |
|--------------------|------|
| Beschreibung        | Die LTrim-Funktion entfernt führende Leerzeichen aus einer Zeichenfolge.                                                                                                             |
| Funktionssignatur | String LTrim(Zeichenfolge)                                                                                                                                                       |
| Eingaben             | Eine Zeichenfolge                                                                                                                                                                 |
| Vorgänge         | Die führenden Leerzeichen im Eingabeparameter werden entfernt. <br/><br/>Beispiel: LTrim(" Test ") ergibt „Test “.                                              |
| Ausgabe             | Eine Zeichenfolge      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beschreibung        | Die RTrim-Funktion entfernt nachgestellte Leerzeichen aus einer Zeichenfolge.                                                                 |
| Funktionssignatur | String RTrim(Zeichenfolge)                                                                                                            |
| Eingaben             | Eine Zeichenfolge                                                                                                                      |
| Vorgänge         | Die nachfolgenden Leerzeichen im Eingabeparameter werden entfernt. Beispiel: RTrim(" Test ") ergibt „ Test“.  |
| Ausgabe             | Eine Zeichenfolge                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beschreibung        | Die Trim-Funktion entfernt führende und nachgestellte Leerzeichen aus einer Zeichenfolge.                                                      |
| Funktionssignatur | String Trim(Zeichenfolge)                                                                                                             |
| Eingaben             | Eine Zeichenfolge                                                                                                                      |
| Vorgänge         | Die führenden und nachfolgenden Leerzeichen in der Zeichenfolge werden entfernt. Beispiel: Trim(" Test ") ergibt „Test“. |
| Ausgabe             | Eine Zeichenfolge                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beschreibung        | Die RightPad-Funktion füllt eine Zeichenfolge mithilfe eines angegebenen Auffüllzeichens nach rechts bis zu einer angegebenen Länge auf.                          |
| Funktionssignatur | String RightPad(Zeichenfolge, Länge, Auffüllzeichen)                                                                                   |
| Vorgänge         | Wenn die Länge der Zeichenfolge kleiner als die angegebene Länge ist, wird das Auffüllzeichen wiederholt am Ende der Zeichenfolge hinzugefügt, bis deren Länge gleich der angegebenen Länge ist. <br/> Beispiele: <br/> – RightPad("User", 10, "0") ergibt „User000000“. <br/> – RightPad(RandomNum(1,10), 5, "0") könnte „90000“ ergeben.   |
| Ausgabe                                                                                                                                                          | Wenn die Zeichenfolge eine Länge größer oder gleich Länge hat, wird eine mit der Zeichenfolge identische Zeichenfolge zurückgegeben. Wenn die Länge der Zeichenfolge kleiner als Länge ist, wird eine neue Zeichenfolge der gewünschten Länge zurückgegeben, die eine mit einem Auffüllzeichen aufgefüllte Zeichenfolge enthält. Wenn die Zeichenfolge null ist, gibt die Funktion eine leere Zeichenfolge zurück. |   |   |
>[!NOTE]
**Auffüllzeichen** darf ein Leerzeichen, kann jedoch kein NULL-Wert sein. Wenn die Länge der **Zeichenfolge** gleich **Länge** oder größer ist, wird die **Zeichenfolge** unverändert zurückgegeben.


| LeftPad      |     |
|----|-------|
| Beschreibung  | Die LeftPad-Funktion füllt eine Zeichenfolge mithilfe eines angegebenen Auffüllzeichens nach links bis zu einer angegebenen Länge auf.    |
| Funktionssignatur      | String LeftPad(Zeichenfolge, Länge, Auffüllzeichen)     |
| Eingaben |  - **Zeichenfolge.** Die aufzufüllende Zeichenfolge. <br/> - **Länge.** Eine Ganzzahl, die die gewünschte Länge der Zeichenfolge darstellt. <br/> - **Auffüllzeichen.** Eine Zeichenfolge, die aus einem einzelnen Zeichen besteht, das als Auffüllzeichen verwendet werden soll. |
| Vorgänge  | Wenn die Länge der Zeichenfolge kleiner als die angegebene Länge ist, wird das Auffüllzeichen wiederholt am Anfang der Zeichenfolge hinzugefügt, bis deren Länge gleich der angegebenen Länge ist. <br/> Beispiele: <br/> – LeftPad("User", 10, "0") ergibt „000000User“. <br/> – LeftPad(RandomNum(1,10), 5, "0") könnte „00009“ ergeben. |  
|Ausgabe | Wenn die Zeichenfolge eine Länge größer oder gleich Länge hat, wird eine mit der Zeichenfolge identische Zeichenfolge zurückgegeben. <br/> Wenn die Länge der Zeichenfolge kleiner als Länge ist, wird eine neue Zeichenfolge der gewünschten Länge zurückgegeben, die eine mit einem Auffüllzeichen aufgefüllte Zeichenfolge enthält. <br/>  Wenn die **Zeichenfolge** NULL ist, gibt die Funktion eine leere Zeichenfolge zurück.                                                   |

<[!NOTE]
**Auffüllzeichen** darf ein Leerzeichen, kann jedoch kein NULL-Wert sein. Wenn die Länge der **Zeichenfolge** gleich **Länge** oder größer ist, wird die **Zeichenfolge** unverändert zurückgegeben.

| BitOr    |  |
|----- |------|
| Beschreibung  | Die BitOr-Funktion legt ein angegebenes Bit auf einem Flag auf 1 fest.     |
| Funktionssignatur  | Int BitOr(Maske, Flag)       |  
| Eingaben     | 1. **Maske.** Ein Hexadezimalwert, der angibt, welches Bit auf dem Flag festgelegt wird. <br/> 2. **Flag.** Ein Hexadezimalwert, für den ein bestimmtes Bit geändert wird.    |   
| Vorgänge         | Diese Funktion konvertiert beide Parameter in binäre Darstellung und vergleicht sie: <br/> – Ein Bit wird auf 1 festgelegt, wenn eines oder beide der entsprechenden Bits in Maske und Flag den Wert 1 haben, und legt ein Bit auf 0 (null) fest, wenn die beiden entsprechenden Bits den Wert 0 (null) haben. <br/> – Sie gibt in allen Fällen 1 zurück, es sei denn, die entsprechenden Bits der beiden Parameter haben den Wert 0 (null). <br/> – Das resultierende Bitmuster besteht aus den „festgelegten“ Bits (1 oder „true“) der beiden Operanden. Mehrere Flagbits können festgelegt werden, wenn mehrere Bits in der Maske den Wert 1 haben.  |
| Ausgabe             | Eine neue Version des **Flags**, wobei die in **Maske** angegebenen Bits auf 1 festgelegt sind.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Beschreibung        | Die BitAnd-Funktion legt ein angegebenes Bit auf einem Flag auf 0 (null) fest.                           |
| Funktionssignatur | Int BitAnd(Maske, Flag)                                                              |
| Eingaben             | 1. **Maske.** Ein Hexadezimalwert, der angibt, welches Bit auf dem Flag geändert wird. <br/> 2. **Flag.** Ein Hexadezimalwert, für den ein bestimmtes Bit geändert wird.   |
| Vorgänge         | Diese Funktion konvertiert beide Parameter in binäre Darstellung und vergleicht sie: <br/> – Ein Bit wird auf 0 (null) festgelegt, wenn eines oder beide der entsprechenden Bits in **Maske** und **Flag** den Wert 0 (null) haben, und legt ein Bit auf 1 fest, wenn die beiden entsprechenden Bits den Wert 1 haben. <br/> – Sie gibt in allen Fällen 0 (null) zurück, es sei denn, die entsprechenden Bits der beiden Parameter haben den Wert 1. Mehrere Flagbits können auf 0 (null) festgelegt werden, wenn mehrere Bits in der **Maske** den Wert 0 haben. |
| Ausgabe             | Eine neue Version des **Flags**, wobei die in **Maske** angegebenen Bits auf 0 (null) festgelegt sind.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Beschreibung       | Die DateTimeFormat-Funktion dient dazu, einen DatumUhrzeit-Wert als Zeichenfolge mit einem angegebenen Format zu formatieren.     |
| Funktionssignatur   | String DateTimeFormat(DatumUhrzeit, Format)      |
| Eingaben   | 1. DatumUhrzeit. Eine Zeichenfolge, die den zu formatierenden DatumUhrzeit-Wert darstellt.  <br/> 2. **Format.** Eine Zeichenfolge, die das Format darstellt, in das konvertiert werden soll.  |   
| Vorgänge           | Die im Format angegebene Formatzeichenfolge wird auf DatumUhrzeit in der DatumUhrzeit-Zeichenfolge angewendet. <br/> Die im Format angegebene Zeichenfolge muss ein gültiges DatumUhrzeit-Format haben. Wenn dies nicht der Fall ist, wird ein Fehler zurückgegeben, der angibt, dass das Format kein gültiges DatumUhrzeit-Format ist. <br/> Beispiel: DateTimeFormat("12/25/2007", "yyyy-MM-dd") ergibt „2007-12-25“.|   
| Ausgabe     | Eine Zeichenfolge, die aus der Anwendung von **Format** auf **DatumUhrzeit** resultiert.   |

>[!Note]                                                                                                                                                                             
Zeichen, die zum Erstellen von benutzerdefinierten Formaten akzeptiert werden, finden Sie unter [Benutzerdefinierte Datums-/Zeitformate (Format-Funktion)](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Beschreibung       | ConvertSidToString konvertiert ein Bytearray, das eine Sicherheits-ID enthält, in eine Zeichenfolge.         |
| Funktionssignatur      | String ConvertSidToString(Objekt-SID)    |
| Eingaben  | **Objekt SID.** Ein Bytearray, das eine Sicherheits-ID (SID) enthält.   |
| Vorgänge    | Die angegebene binäre SID wird in eine Zeichenfolge konvertiert.    |
| Ausgabe              | Eine Zeichenfolgendarstellung der SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Beschreibung         | **Die ConvertStringToGuid**-Funktion konvertiert die Zeichenfolgendarstellung einer GUID in eine binäre Darstellung der GUID.      |
| Funktion            | Byte [] ConvertStringToGuid(stringGuid)  |  
| Eingaben              | **GUID.** Eine in folgendem Muster formatierte Zeichenfolge: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, wobei der Wert der GUID als Folge von hexadezimalen Ziffern in Gruppen von 8, 4, 4, 4 und 12 Ziffern dargestellt und durch Bindestriche getrennt ist. Ein Beispiel eines Rückgabewerts ist „382c74c3-721d-4f34-80e557657b6cbc27“.  |
| Vorgänge          | Die im Parameter 1 angegebene Zeichenfolge **GUID** wird in die binäre Darstellung konvertiert. <br/> Wenn die Zeichenfolge keine Darstellung einer gültigen **GUID** ist, weist das Argument die Funktion mit dem folgenden Fehler zurück: <br/> **Der Parameter für die ConvertStringToGuid-Funktion muss eine Zeichenfolge sein, die eine gültige GUID darstellt.**  |
| Ausgabe              | Eine Binärdarstellung der GUID.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Beschreibung         | Die ReplaceString-Funktion ersetzt alle Vorkommen einer Zeichenfolge durch eine andere Zeichenfolge.  |   
| Funktion            | String ReplaceString(Zeichenfolge, AlterWert, NeuerWert)    |                                                                          
| Eingaben              | 1. **Zeichenfolge.** Eine Zeichenfolge, in der Werte ersetzt werden sollen. <br/> 2. **AlterWert.** Die zu suchende und zu ersetzende Zeichenfolge. <br/> 3. NeuerWert. Die Ersatzzeichenfolge. |
| Vorgänge          | Alle Vorkommen von AlterWert in der Zeichenfolge werden durch NeuerWert ersetzt. Die Funktion muss die folgenden Sonderzeichen behandeln können: <br/> - **\n.** Neue Zeile. <br/> - **\r.** Wagenrücklauf. <br/> - **\t.** Tabulator. <br/> Example: ReplaceString("One\n\rMicrosoft\n\r\Way","\n\r"," ") gibt „One Microsoft Way“ zurück. |   
| Ausgabe              | Eine Zeichenfolge, in der alle Vorkommen von **AlterWert** in der Zeichenfolge durch **NeuerWert** ersetzt werden.      |

## <a name="data-retrieval-functions"></a>Funktionen für den Datenabruf

Mit Funktionen für den Datenabruf werden Vorgänge ausgeführt, die das gewünschte Zeichen aus einer Zeichenfolge abrufen.

| Word       |        |
|--------------------|---------------|
| Beschreibung        | Die Word-Funktion gibt ein Wort innerhalb einer Zeichenfolge auf Basis von Parametern zurück, welche die zu verwendenden Trennzeichen und die zurückzugebende Wortnummer beschreiben.                                                                |
| Funktionssignatur | Zeichenfolge Word(Zeichenfolge, Nummer, Trennzeichen)                                                                                                                                                                        |
| Eingaben             | 1. **Zeichenfolge.** Die Zeichenfolge, aus der ein Wort zurückgegeben werden soll. <br/> 2. **Nummer.** Eine Nummer, die angibt, welches Wort zurückgegeben werden soll. <br/> 3. **Trennzeichen.** Eine Zeichenfolge, welche das/die Trennzeichen darstellt, das/die zum Identifizieren von Wörtern verwendet werden soll/en. |
| Vorgänge         | Jede Teilzeichenfolge in einer Zeichenfolge, die durch eines der als Trennzeichen definierten Zeichen abgetrennt ist, wird als Wort identifiziert. Das an der im Parameter 2 (Nummer) angegebenen Position gefundene Wort wird zurückgegeben: <br/> – Wenn „Nummer“ < 1, wird eine leere Zeichenfolge zurückgegeben. <br/> – Wenn „Zeichenfolge“ NULL ist, wird eine leere Zeichenfolge zurückgegeben. <br/><br/> Beispiele: <br/> 1. Word("Test;of%function;", 3, ";$&%") gibt „function“ zurück. <br/> 2. Word("Test;;Function", 2, ";") gibt „“ (eine leere Zeichenfolge) zurück. 3. Word("Test;of%function;", 0, ";$&%") gibt „“ (eine leere Zeichenfolge) zurück.
| Ausgabe             | Eine Zeichenfolge, die das Wort enthält, das sich an der vom Benutzer abgefragten Position befindet. Wenn die **Zeichenfolge** weniger Worte enthält, als mit **Nummer** angegeben ist, oder wenn die Zeichenfolge keine durch **Trennzeichen** identifizierten Wörter enthält, wird eine leere Zeichenfolge zurückgegeben. |  


| Links               |   |
|-------|-------|
| Beschreibung        | Die Left-Funktion gibt eine bestimmte Anzahl von Zeichen von der linken Seite einer Zeichenfolge zurück.       |
| Funktionssignatur | String Left(Zeichenfolge, numChars)     |
| Eingaben             | 1. **Zeichenfolge.** Die Zeichenfolge, aus der Zeichen zurückgegeben werden sollen. 2. **numChars.** Eine Zahl, die die Anzahl der Zeichen angibt, die ab dem Anfang der Zeichenfolge zurückgegeben werden sollen.         |
| Vorgänge         | **numChars** Zeichen werden ab der ersten Position der Zeichenfolge zurückgegeben. <br/> Beispiel: Left("Britta Simon", 3) gibt "Bri" zurück.   |
| Ausgabe             | Eine Zeichenfolge, die die ersten mit numChars angegebenen Zeichen in der Zeichenfolge enthält.  <br/> – Wenn numChars = 0, wird eine leere Zeichenfolge zurückgegeben. <br/> – Wenn numChars < 0, wird eine Eingabezeichenfolge zurückgegeben. <br/> – Wenn „Zeichenfolge“ NULL ist, wird eine leere Zeichenfolge zurückgegeben. |




| Right       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Beschreibung | Die Right-Funktion gibt eine angegebene Anzahl von Zeichen von der rechten Seite (Ende) einer Zeichenfolge zurück.                                 |
| Funktionssignatur   | String Right(Zeichenfolge, numChars)   |
| Eingaben      | 1. **Zeichenfolge.** Die Zeichenfolge, aus der Zeichen zurückgegeben werden sollen. <br/> 2. **numChars.** Eine Zahl, die die Anzahl der Zeichen angibt, die ab dem Ende der Zeichenfolge zurückgegeben werden sollen.  |
| Vorgänge  | **numChars.** Zeichen werden ab dem Ende einer Zeichenfolge zurückgegeben. <br/> Beispiel: Right("Britta Simon", 3) gibt "mon" zurück.                  |
| Ausgabe      | Eine Zeichenfolge, die die letzten mit numChars angegebenen Zeichen in einer Zeichenfolge enthält. Wenn numChars = 0, wird eine leere Zeichenfolge zurückgegeben. <br/> Wenn **numChars** < 0, wird eine Eingabezeichenfolge zurückgegeben. <br/> Wenn „Zeichenfolge“ NULL ist, wird eine leere Zeichenfolge zurückgegeben. <br/> – Wenn die Zeichenfolge weniger Zeichen enthält als die in numChars angegebene Anzahl Zeichen, wird eine mit der Zeichenfolge identische Zeichenfolge zurückgegeben. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Beschreibung | Die Mid-Funktion gibt eine angegebene Anzahl von Zeichen aus einer angegebenen Position in einer Zeichenfolge zurück.                              |
| Funktionssignatur    | String Mid(Zeichenfolge, pos, numChars)                                                                                             |
| Eingaben      | 1. **Zeichenfolge.** Die Zeichenfolge, aus der Zeichen zurückgegeben werden sollen.   <br/> 2. **pos.** Eine Zahl, die in einer Zeichenfolge die Anfangsposition für die Rückgabe von Zeichen angibt. <br/> 3. **numChars.** Eine Zahl, die die Anzahl der Zeichen angibt, die ab einer Position in der Zeichenfolge zurückgegeben werden sollen.  |
| Vorgänge  | Gibt eine mit **numChars** angegebene Anzahl von Zeichen ab der Position **pos** in der Zeichenfolge zurück. <br/>Beispiel: Mid("Britta Simon", 3, 5) würde „itta“ zurückgeben. |
| Ausgabe      | Eine Zeichenfolge, die eine mit **numChars** angegebene Anzahl Zeichen ab der Position **pos** in einer Zeichenfolge enthält: <br/> – Wenn **numChars** = 0, wird eine leere Zeichenfolge zurückgegeben. <br/> – Wenn **numChars** < 0, wird eine leere Zeichenfolge zurückgegeben. <br/> – Wenn **pos** > die Länge der Zeichenfolge, wird eine Eingabezeichenfolge zurückgegeben. <br/> – Wenn **pos** ≤ 0, wird eine Eingabezeichenfolge zurückgegeben. <br/> – Wenn **Zeichenfolge** NULL ist, wird eine leere Zeichenfolge zurückgegeben. <br/> Enthält die **Zeichenfolge** ab der Position **pos** weniger als mit **numChars** angegebene Zeichen, werden so viele Zeichen wie möglich zurückgegeben.

## <a name="data-generation-functions"></a>Funktionen zum Generieren von Daten

Mit Funktionen zum Generieren von Daten werden Werte für bestimmte Datentypen generiert.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Beschreibung        | CRLF generiert einen Wagenrücklauf/Zeilenvorschub. Mit dieser Funktion fügen Sie eine neue Zeile hinzu. |
| Funktionssignatur | String CRLF                                                                              |
| Eingaben             | Keine Parameter                                                                            |
| Vorgänge         | Ein CRLF wird zurückgegeben.                                                                      |
|                    | Beispiel: Addresszeile1 + CRLF() + Addresszeile2 ergibt: <br/> – Addresszeile1 <br/> – Addresszeile2 |
| Ausgabe             | Ein CRLF wird ausgegeben.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Beschreibung        | Die RandomNum-Funktion gibt eine Zufallszahl zurück, die in einem angegebenen Intervall liegt.                                       |   
| Funktionssignatur | Int RandomNum(Start, Ende)                                                                                         |   
| Eingaben             | - **Start**. Eine Zahl, welche die untere Grenze des zu generierenden Zufallswerts angibt.   <br/> - **Ende**. Eine Zahl, welche die obere Grenze des zu generierenden Zufallswerts angibt.  |
| Vorgänge         | Eine Zufallszahl wird generiert, die mindestens so groß wie **Start** und höchstens so groß wie **Ende** ist. <br/>  Beispiel: Random(0, 999) könnte 100 zurückgeben.                      |
| Ausgabe             | Eine Zufallszahl innerhalb des von **Start** und **Ende** angegebenen Bereichs.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Beschreibung        | Die *EscapeDNComponent*-Methode verarbeitet die Eingabezeichenfolge auf Basis des verwendeten Verwaltungs-Agent-Typs. |
| Funktionssignatur | String EscapeDNComponent(Zeichenfolge)                                                                                  |
| Eingaben     | **Zeichenfolge**. Eine Zeichenfolge, die zum Verarbeiten eines Distinguished Name verwendet wird. Die Zeichenfolge sollte keine Escapezeichen enthalten. |
| Vorgänge | Die EscapeDNComponent-Methode von MIISUtils wird verwendet, um diesen Vorgang auszuführen. Diese Methode verarbeitet die Eingabezeichenfolge auf Basis des verwendeten Verwaltungs-Agent-Typs. <br/> Da verschiedene Verwaltungs-Agents verschiedene Distinguished Name-Formate erfordern, verarbeitet diese Methode die Eingabezeichenfolgen auf Basis des verwendeten Verwaltungs-Agent-Typs. Die Typen sind Lightweight Directory Access Protocol-Distinguished Name-Typen (LDAP), z.B. asActive Directory® Domain Services, Sun Directory Server (ehemals iPlanet Directory Server), Microsoft Exchange-Server; hierarchische Nicht-LDAP-Typen, z.B. Microsoft Lotus Notes; und extrinsische Typen, z.B. Datenbank und XML ohne LDAP-Distinguished Names. <br/> **LDAP-Distinguished Name: ** <br/> – Beliebige ungültige XML-Zeichen im Wertteil eines bestimmten Teils sind hexadezimal codiert. <br/>– Beliebige ungültige Zeichen (einschließlich ungültiger XML-Zeichen) im Namensteil eines bestimmten Teils generieren einen Fehler. <br/> – Die folgenden Zeichen werden mit Escapezeichen versehen: <br/> &nbsp;&nbsp;&nbsp; – Komma (',') <br/> &nbsp;&nbsp;&nbsp; – Gleichheitszeichen ('=') <br/> &nbsp;&nbsp;&nbsp; – Pluszeichen ('+') <br/> &nbsp;&nbsp;&nbsp; – Kleiner-als-Zeichen ('<') <br/> &nbsp;&nbsp;&nbsp; – Größer-als-Zeichen ('>') <br/> &nbsp;&nbsp;&nbsp; – Nummernzeichen ('#') <br/> &nbsp;&nbsp;&nbsp; – Semikolon (';') <br/> &nbsp;&nbsp;&nbsp; – Umgekehrter Schrägstrich ('\') <br/> &nbsp;&nbsp;&nbsp; – Anführungszeichen ('"') <br/> – Wenn das letzte Zeichen in der Zeichenfolge ein Leerzeichen ist, dann wird dieses Leerzeichen mit Escapezeichen versehen. <br/> – Jegliche überflüssigen führenden oder nachfolgenden Leerzeichen um einen Teilenamen werden entfernt. <br/> – Für den XML-Verwaltungs-Agent werden mehrere vorhandene Teile alphabetisch sortiert. <br/> – Wenn mehrere Teile angegeben sind, ist die zusammengesetzte DN-Zeichenfolge die Verkettung der einzelnen durch Pluszeichen getrennten Zeichenfolgen. <br/> – Ein Fehler wird generiert, wenn die Eingabezeichenfolge keine ordnungsgemäß formatierte DN-Zeichenfolge im LDAP-Format ist. <br/><br/> **Hierarchisch Nicht-LDAP** <br/> – Diese Verwaltungs-Agents unterstützen keine mehrteiligen Komponenten. Wenn mehrere Zeichenfolgen an EscapeDNComponent übergeben werden, wird eine ArgumentException ausgelöst. <br/> – Wenn die Eingabezeichenfolge ungültige XML-Zeichen enthält, wird eine ArgumentException ausgelöst. <br/> – Alle Kommas und umgekehrten Schrägstriche in der Eingabezeichenfolge werden mit Escapezeichen versehen. <br/> – Wenn das letzte Zeichen in der Zeichenfolge ein Leerzeichen ist, dann wird dieses Leerzeichen mit Escapezeichen versehen. <br/><br/> **Extrinsisch:** <br/> 1. Wenn ein beliebiger Teil binär ist oder ein ungültiges XML-Zeichen enthält, wird dieser Teil als hexadezimalcodierte Version der Rohdaten mit dem Präfixzeichen „#“ am Anfang der Zeichenfolge gespeichert. Wäre ein Teil z.B. „AxC“ (wobei „x“ für ein ungültiges XML-Zeichen wie z.B. „0x10“ steht), würde dieser Teil z.B. als „#410010004300“ codiert. <br/> 2. Im Übrigen werden alle Instanzen der folgenden Zeichen mit Escapezeichen versehen: <br/> &nbsp;&nbsp;&nbsp; – Umgekehrter Schrägstrich ('\') <br/> &nbsp;&nbsp;&nbsp; – Komma (',') <br/> &nbsp;&nbsp;&nbsp; – Pluszeichen ('+') <br/> &nbsp;&nbsp;&nbsp; – Nummernzeichen ('#') <br/> 3. – Wenn das letzte Zeichen in einer bestimmten Teilzeichenfolge ein Leerzeichen ist, dann wird dieses Leerzeichen mit Escapezeichen versehen. <br/> 4. – Wenn mehrere Teile angegeben sind, ist die zusammengesetzte DN-Zeichenfolge die Verkettung der einzelnen durch Pluszeichen getrennten Zeichenfolgen.
| Ausgabe      | Eine Zeichenfolge mit einem gültigen Domänennamen.                                                                                                                  |   

>[!NOTE]
Die Überprüfung von Distinguished Names ist weniger streng als die in den LDAP-Spezifikationen definierte Syntax. EscapeDNComponent(String[]) ermöglicht, dass ein Teilename eine beliebige Kombination aus einem oder mehreren der Zeichen „a“-„z“, „A“-„Z“, „0“-„9“, „-“ und „.“ enthält. <br/>
Es ist nicht möglich, mit dieser Methode einen binären Teil anzugeben. Allerdings ist ein binärer Teil in **CommitNewConnector** möglich, wenn der Distinguished Name aus Ankerattributen erstellt wird und eines der Ankerattribute ein binärer Typ ist.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Beschreibung | Die NULL-Funktion wird verwendet, um zu definieren, dass dieser Verwaltungs-Agent kein beizusteuerndes Attribut aufweist, und der Attributvorrang mit dem nächsten Verwaltungs-Agent fortgesetzt werden sollte. |   
| Funktionssignatur    | Zeichenfolge NULL    |
| Eingaben      | Keine Parameter                                                                                                                                             |   
| Vorgänge  | Ein NULL-Wert wird zurückgegeben. <br/> Beispiel: IIF(Eq(Domäne), "unbekannt", Null())                                                                                           |   
| Ausgabe      | Ein NULL-Wert wird ausgegeben.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Logikfunktionen
Logikfunktionen dienen zum Ausführen eines Vorgangs basierend auf Bedingungen, die vom System ausgewertet werden.

| IIF        |  |
|-------------|---|
| Beschreibung | Die IIF-Funktion gibt einen Wert aus einem Satz möglicher Werte entsprechend einer angegebenen Bedingung zurück.    |
| Funktionssignatur   | Objekt IIF(Bedingung, valueIfTrue, valueIfFalse)   |                                                 |
| Eingaben      | 1. **Bedingung**. Jeder Wert oder Ausdruck, der mit „true“ oder „false“ ausgewertet werden kann. 2. **valueIfTrue**: Ein Wert, der zurückgegeben wird, wenn die Bedingung mit „true“ ausgewertet wird. <br/> 3. **valueIfFalse**: Ein Wert, der zurückgegeben wird, wenn die Bedingung mit „false“ ausgewertet wird. <br/><br/> Folgende Funktionen stehen zur Verwendung als Ausdrücke für die **Bedingung** in der IIF-Funktion zur Verfügung: <br/> **Eq**. Diese Funktion vergleicht zwei Argumente auf Gleichheit. <br/> **NotEquals**. Diese Funktion vergleicht zwei Argumente auf Ungleichheit, wobei „true“ zurückgegeben wird, wenn sie nicht gleich sind, und „false“, wenn sie gleich sind.<br/> Beispiel: NotEquals(EmployeeType, "Auftragnehmer")<br/> **LessThan**. Diese Funktion vergleicht zwei Zahlen, wobei „true“ zurückgegeben wird, wenn die erste kleiner ist als die zweite, und andernfalls „false“.<br/>Beispiel: LessThan(Gehalt, 100000) <br/>**GreaterThan**. Diese Funktion vergleicht zwei Zahlen, wobei „true“ zurückgegeben wird, wenn die erste größer ist als die zweite, und andernfalls „false“.<br/> Beispiel: GreaterThan(Gehalt, 100000) <br/> **LessThanOrEquals**. Diese Funktion vergleicht zwei Zahlen, wobei „true“ zurückgegeben wird, wenn die erste Zahl so groß ist wie die zweite oder kleiner, und andernfalls „false“.<br/>Beispiel: LessThanOrEquals(Gehalt, 100000) <br/> **GreaterThanOrEquals*. Diese Funktion vergleicht zwei Zahlen, wobei „true“ zurückgegeben wird, wenn die erste Zahl so groß ist wie die zweite oder größer, und andernfalls „false“. <br/>Beispiel: GreaterThanOrEquals(Gehalt, 100000)<br/> IsPresent. Diese Funktion akzeptiert als Eingabe ein Attribut im ILM-Schema und gibt „true“ zurück, wenn das Attribut nicht NULL ist, und „false“, wenn das Attribut NULL ist.|
| Vorgänge  | Wenn **Bedingung** „true“ ergibt, wird **valueIfTrue** zurückgegeben. Andernfalls wird **valueIfFalse** zurückgegeben. <br/>Beispiel: IIF(Eq(EmployeeType, "Intern"),"t-" + Alias, Alias) gibt den Alias eines Benutzers zurück, wobei „t-“ am Anfang des Alias hinzugefügt wird, wenn es sich um einen internen Benutzer handelt. Andernfalls wird der Alias des Benutzers unverändert zurückgegeben. |
| Ausgabe      | Die Ausgabe lautet **valueIfTrue**, wenn die Bedingung „true“ ist, oder **valueIfFalse**, wenn die Bedingung „false“ ist. |      
