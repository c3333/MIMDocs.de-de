---
title: BHOLD-Entwicklerreferenz für Microsoft Identity Manager 2016 | Microsoft-Dokumentation
description: BHOLD-Entwicklerreferenz
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: edf9f88a4d96d212cef4aa41cbad7d0b4446534f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760788"
---
# <a name="bhold-developer-reference-for-microsoft-identity-manager-2016"></a>BHOLD-Entwicklerreferenz für Microsoft Identity Manager 2016

Das BHOLD-Core-Modul kann Skript Befehle verarbeiten. Dies kann direkt mithilfe der bscript.dll in einem .net-Projekt erfolgen. Auch Interaktion mit der b1scriptservice. asmx-Schnittstelle des Webdiensts. 

Vor der Ausführung eines Skripts sollten alle Informationen innerhalb des Skripts gesammelt werden, um dieses Skript zu verfassen. Diese Informationen können aus den folgenden Quellen gesammelt werden:

   * Benutzereingabe
   * BHOLD-Daten
   * Anwendungen
   * Sonstige

Die BHOLD-Daten können mithilfe der GetInfo-Funktion des Skript Objekts abgerufen werden. Es gibt eine vollständige Liste der Befehle, die alle in der BHOLD-Datenbank gespeicherten Daten darstellen können. Die dargestellten Daten unterliegen jedoch den View-Berechtigungen des angemeldeten Benutzers. Das Ergebnis ist in Form eines XML-Dokuments, das analysiert werden kann.

Eine andere Informationsquelle kann eine der Anwendungen sein, die von BHOLD gesteuert werden. Das Anwendungs-Snap-in verfügt über eine spezielle Funktion, die functiondispatch, die zur Darstellung anwendungsspezifischer Informationen verwendet werden kann. Dies wird auch als XML-Dokument dargestellt.

Wenn es keine andere Möglichkeit gibt, kann das Skript Befehle direkt für andere Anwendungen oder Systeme enthalten. Nothenstallationen zusätzlicher Software auf dem BHOLD-Server können die Sicherheit des gesamten Systems untergraben.

Alle diese Informationen werden in einem XML-Dokument abgelegt und dem BHOLD-Skript Objekt zugewiesen. Das-Objekt kombiniert dieses Dokument mit einer vordefinierten Funktion. Die vordefinierte Funktion ist ein XSL-Dokument, das das Skript Eingabe Dokument in ein BHOLD-Befehls Dokument übersetzt.

![BHOLD-Skript Verarbeitung](media/mim2016-bhold-developer-reference/image001.png)

Die Befehle werden in derselben Reihenfolge wie im Dokument ausgeführt. Wenn eine Funktion fehlschlägt, wird für alle ausgeführten Befehle ein Rollback ausgeführt.

## <a name="script-object"></a>Skript Objekt
In diesem Abschnitt wird die Verwendung des-Skript Objekts beschrieben.

### <a name="retrieve-bhold-information"></a>BHOLD-Informationen abrufen
Die **GetInfo** -Funktion wird verwendet, um Informationen aus den verfügbaren Daten im BHOLD-Autorisierungssystem abzurufen. Die Funktion erfordert einen Funktionsnamen und schließlich einen oder mehrere Parameter. Wenn diese Funktion erfolgreich ausgeführt wird, wird ein BHOLD-Objekt oder eine Auflistung in Form eines XML-Dokuments zurückgegeben.

Wenn die Funktion nicht erfolgreich ist, gibt die GetInfo-Funktion eine leere Zeichenfolge oder einen Fehler zurück. Die Fehlerbeschreibung und-Nummer können verwendet werden, um weitere Informationen zum Fehler zu erhalten.

Die GetInfo-Funktion "functiondispatch" kann zum Abrufen von Informationen aus einer Anwendung verwendet werden, die vom BHOLD-System gesteuert wird. Diese Funktion erfordert drei Parameter: die ID der Anwendung, die dispatchfunktion, wie Sie in der ASI definiert ist, und ein XML-Dokument mit unterstützenden Informationen für die ASI. Wenn die Funktion erfolgreich ausgeführt wird, ist das Ergebnis im XML-Format im Ergebnis Objekt verfügbar.

Der folgende Code Ausschnitt ist ein einfaches c#-Beispiel von GetInfo:

```C#
ScriptProcessor myScriptProcessor = new ScriptProcessor();
myScriptProcessor.Initializae("CORP\\b1user");
myScriptProcessor.GetInfo("OrgUnit", "1");
```

Ebenso kann auf das **bscript** -Objekt auch über den Webdienst zugegriffen werden `b1scriptservice` . Dies erfolgt durch Hinzufügen eines Webverweises zum Projekt mithilfe von http:// <server> : 5151/BHOLD/Core/b1scriptservice. asmx, wobei <server> der Server mit den BHOLD-Binärdateien ist. Weitere Informationen finden Sie unter [Hinzufügen eines Webdienst Verweises zu einem Visual Studio-Projekt](https://msdn.microsoft.com/library/d9w023sx(v=vs.71).aspx).

Im folgenden Beispiel wird gezeigt, wie die **GetInfo** -Funktion aus einem Webdienst verwendet wird. Mit diesem Code wird die Organisationseinheit abgerufen, die eine OrgId von 1 hat, und anschließend wird der Name der Organisationseinheit auf dem Bildschirm angezeigt.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

namespace bhold_console
{
    class Program
    {
        static void Main(string[] args)
        {
             var bholdService = new BHOLDCORE.B1ScriptService();
             bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";
             string orgname= "";

             if (args.Length == 3)
             {
                 //Use explicit credentials from command line
                 bholdService.UseDefaultCredentials = false;
                 bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                 bholdService.PreAuthenticate = true;
             }
             else
             {
                 bholdService.UseDefaultCredentials = true;
                 bholdService.PreAuthenticate = true;
             }

             //Load BHOLD information into an xml document and loop through document to find the bholdDescription value
             var myOrgUnit = new System.Xml.XmlDocument();
             myOrgUnit.LoadXml(bholdService.GetInfo("OrgUnit","1","","");

            XmlNodeList myList = myOrgUnit.SelectNodes(("//item");

            foreach (XmlNode myNode in myList)
            {
                for (int i = 0; i < myNode.ChildNodes.Count; i++)
                {
                    if (myNode.ChildNodes[i].InnerText.ToString() == "bholdDescription")
                    {
                        orgname = myNode.ChildNodes[i + 1].InnerText.ToString();
                    }
                }
            }

            System.Console.WriteLine("The Organizational Unit Name is: " + orgname);

        }
    }
}
```

Im folgenden VBScript-Beispiel wird der Webdienst über SOAP und die Verwendung von GetInfo verwendet. Weitere Beispiele für SOAP 1,1, SOAP 1,2 und HTTP Post finden Sie im Abschnitt verwaltete Referenz zu BHOLD, oder Sie können direkt in einem Browser zum Webdienst navigieren und dort anzeigen.

```VB
Dim SOAPRequest
Dim SOAPParameters
Dim SOAPResponse
Dim xmlhttp

Set xmlhttp = CreateObject("Microsoft.XMLHTTP")

xmlhttp.open "POST", "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx", False, "CORP\Administrator", "abc123*2k"

xmlhttp.setRequestHeader "Content-type", "text/xml; charset=utf-8"
xmlhttp.setRequestHeader "SOAPAction", "http://B1/B1ScriptService/GetInfo"

SOAPRequest = "<?xml version='1.0' encoding='utf-8'?> <soap:Envelope" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsi=""http://" & vbCRLF
SOAPRequest = SOAPRequest & " www.w3.org/2001/XMLSchema-instance""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsd=""http://www.w3.org/2001/XMLSchema""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/"">" & vbCRLF
SOAPRequest = SOAPRequest & " <soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " <GetInfo xmlns=""http://B1/B1ScriptService"">" & vbCRLF
SOAPRequest = SOAPRequest & " <functionName>OrgUnit</functionName>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter1>1</parameter1>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter2></parameter2>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter3></parameter3>" & vbCRLF
SOAPRequest = SOAPRequest & " </GetInfo>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Envelope>"
MsgBox SOAPRequest

xmlhttp.send SOAPRequest 

SOAPResponse = xmlhttp.responseText

MsgBox SOAPResponse
```

## <a name="execute-scripts"></a>Skripts ausführen

Die **executeScript** -Funktion des **bscript** -Objekts kann zum Ausführen von Skripts verwendet werden. Diese Funktion erfordert zwei Parameter. Der erste Parameter ist das XML-Dokument, das die benutzerdefinierten Informationen enthält, die vom Skript verwendet werden sollen. Der zweite Parameter ist der Name des vordefinierten Skripts, das verwendet werden soll. Im Verzeichnis "BHOLD-vordefinierte Skripts" sollte hier ein XSL-Dokument mit dem gleichen Namen wie die Funktion, aber mit der Erweiterung ". xsl" vorliegen.

Wenn die Funktion nicht erfolgreich ist, gibt die executeScript-Funktion den Wert "false" zurück. Die Fehlerbeschreibung und die Nummer können verwendet werden, um herauszufinden, was schief gelaufen ist. Im folgenden finden Sie ein Beispiel für die Verwendung der executexml-Webmethode. Diese Methode ruft executeScript auf.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            var bholdService = new BHOLDCORE.B1ScriptService();
            bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";

            if (args.Length == 3)
            {
                //Use explicit credentials from command line
                bholdService.UseDefaultCredentials = false;
                bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                bholdService.PreAuthenticate = true;
            }
            else
            {
                bholdService.UseDefaultCredentials = true;
                bholdService.PreAuthenticate = true;
            }
            System.Console.WriteLine( "Add user #3 to role #44, result: {0}", bholdService.ExecuteXml(roleAddUser("44", "3")) );
            System.Console.WriteLine("Add user D1 to role 'MR-OU2 No Users', result: {0}", bholdService.ExecuteXml(roleAddUser("MR-OU2 No Users", "D1")));

        }

        private static System.Xml.XmlNode roleAddUser(string roleId, string userId)
        {
            var script = new System.Xml.XmlDocument();
            script.LoadXml(string.Format("<functions>"
                                        +"  <function name='roleadduser' roleid='{0}' userid='{1}' />"
                                        +"</functions>",
                                        roleId,
                                        userId)
                           );
            return script.DocumentElement;
```

## <a name="bholdscriptresult"></a>Bholdscriptresult

Diese GetInfo-Funktion ist verfügbar, nachdem die **executeScript** -Funktion ausgeführt wurde. Die Funktion gibt eine XML-formatierte Zeichenfolge zurück, die den vollständigen Ausführungs Bericht enthält. Der Skript Knoten enthält die XML-Struktur des ausgeführten Skripts.

Für jede Funktion, die während der Ausführung des Skripts fehlschlägt, wird eine Node-Funktion mit dem Knoten Namen hinzugefügt. "Executexml" und "Error" werden am Ende des Dokuments hinzugefügt. alle generierten IDs werden hinzugefügt.

Beachten Sie, dass nur die Funktionen hinzugefügt werden, die einen Fehler enthalten. Die Fehlernummer ' 0 ' bedeutet, dass die Funktion nicht ausgeführt wird. 

```
<Bhold>
  <Script>
    <functions>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>     
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>      
    </functions>
  </Script>
  <Function>
    <Name>OrgUnitadd</Name>
    <ExecutedXML>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>
    </ExecutedXML>
    <Error Number="5" Description="Violation of UNIQUE KEY constraint 'IX_OrgUnits'. Cannot insert duplicate key in object 'dbo.OrgUnits'.
The statement has been terminated."/>
  </Function>
  <Function>
    <Name>roleaddOrgUnit</Name>
    <ExecutedXML>
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>
    </ExecutedXML>
    <Error Number="0" Description=""/>
  </Function>
  <IDS>
    <ID name="@ID@">35</ID>
  </IDS>
</Bhold>
```

## <a name="id-parameters"></a>ID-Parameter
ID-Parameter werden besonders behandelt. Nicht numerische Werte werden als Suchwert zum Suchen der entsprechenden Entitäten im BHOLD-Datenspeicher verwendet. Wenn der Suchwert nicht eindeutig ist, wird die erste Entität zurückgegeben, die mit dem Suchwert übereinstimmt.

Um numerische Suchwerte von IDs zu unterscheiden, ist es möglich, ein Präfix zu verwenden. Wenn die ersten sechs Zeichen des Suchwerts "no_id:" entsprechen, werden diese Zeichen entfernt, bevor der Wert für die Suche verwendet wird. Die SQL-Platzhalter Zeichen "%" können verwendet werden.

Die folgenden Felder werden mit dem Suchwert verwendet:

| ID-Datentyp   | Suchen (Feld) |
|---|---|
| Orgunitid | BESCHREIBUNG  |
| roleID    | BESCHREIBUNG  |
| TaskID    | BESCHREIBUNG  |
| userID    | Defaultalias |

## <a name="script-access-and-permissions"></a>Skript Zugriff und-Berechtigungen
Der Server seitige Code auf den Active Server Seiten wird zum Ausführen der Skripts verwendet. Der Zugriff auf das Skript bedeutet daher den Zugriff auf diese Seiten. Das BHOLD-System verwaltet Informationen zu den Einstiegspunkten der benutzerdefinierten Seiten. Diese Informationen umfassen die Startseite und Funktionsbeschreibung (mehrere Sprachen sollten unterstützt werden).

Ein Benutzer ist autorisiert, die benutzerdefinierten Seiten einzugeben und ein Skript auszuführen. Jeder Einstiegspunkt wird als Aufgabe dargestellt. Jeder Benutzer, der diese Aufgabe über eine Rolle oder eine Einheit erlangt hat, kann die entsprechende Funktion ausführen.

Eine neue Funktion im Menü zeigt alle benutzerdefinierten Funktionen an, die vom Benutzer ausgeführt werden können. Da ein Skript Aktionen im BHOLD-System unter einer anderen Identität als der angemeldete Benutzer ausführen kann. Es ist möglich, die Berechtigung zu erteilen, eine bestimmte Aktion auszuführen, ohne dass ein Objekt überwacht werden muss. Dies kann beispielsweise für einen Mitarbeiter nützlich sein, der nur neue Kunden in das Unternehmen eingeben darf. Diese Skripts können auch verwendet werden, um selbst Registrierungsseiten zu erstellen.

## <a name="command-script"></a>Befehls Skript
Das Befehls Skript enthält eine Liste der Funktionen, die vom BHOLD-System ausgeführt werden. Die Liste wird in ein XML-Dokument geschrieben, das den folgenden Definitionen entspricht:


|   Befehls Skript   |              `<functions>functions</functions>`               |
|--------------------|---------------------------------------------------------------|
|     functions      |                      Funktion {Function}                      |
|      Funktion      | <Function Name = "FunctionName" functionparameters [Return] (/> \| > parameterlist </Function>) |
|    functionName    | Ein gültiger Funktionsname, wie in den folgenden Abschnitten beschrieben. |
| functionparameters |                     {FunctionParameter}                     |
| FunctionParameter  |               Parameter Name = "ParameterValue"                |
|   parameterName    |                    Ein gültiger Parameter Name.                    |
|   ParameterValue   |                      @variable@ \| Wert                      |
|       value        |                   Ein gültiger Parameterwert.                    |
|   Parameterliste    |          <parameters> {parameteritem}  </parameters>          |
|   parametritem    | <parameter name="parameterName"> ParameterValue </parameter>  |
|       return       |                      Return = " @variable @"                      |
|      Variable      |                    Ein benutzerdefinierter Variablenname.                    |

XML hat die folgenden Übersetzungen von Sonderzeichen:

| XML | Zeichen |
|---|---|
| ```&amp;```  | & |
| ```&lt;```   | < |
| ```&gt;```   | > |
| ```&quot;``` | " |
| ```&apos;``` | ' |

Diese XML-Zeichen können in bezeichnerbezeichnerzeichen verwendet werden, aber Sie werden nicht empfohlen.

Der folgende Code zeigt ein Beispiel für ein gültiges Befehls Dokument mit drei Funktionen:

```
<functions>

   <functionname="OrgUnitAdd" parentID="34" description="Acme Inc." orgtypeID="5" return="@UnitID@" />

   <function name="UserAdd" description="John Doe" alias="jdoe" languageID="1" OrgUnitID="@UnitID@" />

   <function name="TaskAddFile" taskID="93" path="/customers/purchase">
      <parameters>
         <parameter name="history"> True</parameter>
      </parameters>
   </function>

</functions>
```

Die Funktion **orgunitadd** speichert die ID der erstellten Einheit in einer Variablen namens unitid. Diese Variable wird als Eingabe für die useradd-Funktion verwendet. Der Rückgabewert dieser Funktion wird nicht verwendet. In den folgenden Abschnitten werden alle verfügbaren Funktionen, die erforderlichen Parameter und deren Rückgabewerte beschrieben.

## <a name="execute-functions"></a>Funktionen ausführen
In diesem Abschnitt wird die Verwendung der Execute-Funktionen beschrieben.

### <a name="abaattributeruleadd"></a>Abaattributeruleadd
Erstellen Sie eine neue Attribut Regel für einen bestimmten Attributtyp. Attribut Regeln können nur mit einem Attributtyp verknüpft werden.

Die angegebene Attribut Regel kann mit allen möglichen Attributtypen verknüpft werden. 

Der RuleType kann nicht mit dem Befehl "abaattributeruletypeupdate" geändert werden. Erfordert, dass die Beschreibung des Attributs eindeutig ist.


|    Argumente    |                                                                                                                                                                                                                  type                                                                                                                                                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   BESCHREIBUNG   |                                                                                                                                                                                                                  Text                                                                                                                                                                                                                  |
|    RuleType     | Geben Sie die Art der Attribut Regel an. Abhängig von der Art des Attribut Regel Typs müssen andere Argumente eingeschlossen werden. Die folgenden Regeltyp Werte sind gültig:<ul><li>0: regulärer Ausdruck (Argument "Value" hinzufügen).</li><li>1: Wert (Argumente "Operator" und "Value" hinzufügen).</li><li>2: Liste der Werte.</li><li>3: Bereich (Argumente hinzufügen "RangeMin" und "RangeMax").</li><li>4: Alter (Argumente "Operator" und "Value" hinzufügen).</li></ul> |
|  Invertresult   | ```["0"|"1"|"N"|"Y"]``` |
| Attributetypeid |                                                                                                                                                                                                                  Text                                                                                                                                                                                                                  |

| Optionale Argumente | Typ |
|---|---|
| Operator | Text <br>**Hinweis** : dieses Argument ist obligatorisch, wenn **RuleType** 1 oder 4 ist. Mögliche Werte sind ' = ', ' < ' oder ' > '. &gt;Für ' < ' müssen die XML-Tags ' ' für ' > ' und ' ' verwenden &lt; . |
| RangeMin | Number <br>**Hinweis** : dieses Argument ist obligatorisch, wenn **RuleType** den Wert 3 hat. |
| RangeMax | Number <br>**Hinweis** : dieses Argument ist obligatorisch, wenn **RuleType** den Wert 3 hat. |
| Wert    | Text <br>**Hinweis** : dieses Argument ist obligatorisch, wenn **RuleType** den Wert 0, 1 oder 4 hat. Bei dem Argument muss es sich um einen numerischen oder alphanumerischen Wert handeln. |
| Attributeruleid für Rückgabetyp | Text   |


### <a name="applicationadd"></a>applicationadd
Erstellt eine neue Anwendung und gibt die ID der neuen Anwendung zurück.

| Argumente | type |
|---|---|
| description |      |
| Computer     |      |
| module      |      |
| parameter   |      |
| Protokoll    |      |
| username    |      |
| password    |      |
| svroleid (optional)                | Wenn dieses Argument nicht vorhanden ist, wird eine Supervisor-Rolle des aktuellen Benutzers verwendet. |
| Applicationaliasformula (optional) | Die Alias Formel wird verwendet, um einen Alias für einen Benutzer zu erstellen, wenn er einer Berechtigung der Anwendung zugewiesen wird. Der Alias wird erstellt, wenn der Benutzer nicht bereits über einen Alias für diese Anwendung verfügt. Wenn kein Wert angegeben wird, wird der Standardalias des Benutzers als Alias für die Anwendung verwendet. Die Formel wird als formatiert ` [<<objecttype>>.<<nameofobjecttypeattribute>>(startindexoffset,length offset)]` . Der Offset ist optional. Es können nur Benutzer-und Anwendungs Attribute verwendet werden. Der freie Text kann verwendet werden. Die reservierten Zeichen sind die linke eckige Klammer ([) und die rechte eckige Klammer (]). Beispiel: `[Application.bholdDescription]\[User.bholdDefAlias(1,5)]`. |
| Rückgabetyp | Die ID der neuen Anwendung. |

### <a name="attributesetvalue"></a>Attributesetvalue
Legt den Wert eines Attribut Typs fest, der mit dem Objekttyp verbunden ist. Erfordert, dass die Beschreibungen von Objekttyp und Attributtyp eindeutig sind.

| Argumente | type |
|---|---|
| ObjectTypeId     | Text |
| ObjectID         | Text |
| Attributetypeid  | Text |
| Wert            | Text |
| Rückgabetyp      | type |

### <a name="attributetypeadd"></a>Attributetypeadd
Fügt einen neuen Attributtyp/Eigenschaftentyp ein.


|        Argumente        |                                               type                                                |
|-------------------------|---------------------------------------------------------------------------------------------------|
|       Datatypeid        |                                               Text                                                |
| Beschreibung (= Identity) | Text <br>**Hinweis** : reservierte Wörter können nicht verwendet werden, einschließlich "a", "frm", "ID", "usr" und "BHOLD". |
|        MaxLength        |                                       Zahl in [1,.., 255]                                        |
| ListOf Values (Boolean)  |                                          ```["0"|"1"|"N"|"Y"]```                               |
|      DefaultValue       |                                               Text                                                |
|       Rückgabetyp       |                                               type                                                |
|     Attributetypeid     |                                               Text                                                |

### <a name="attributetypesetadd"></a>Attributetypesetadd
Fügt einen neuen Attributtyp Satz ein. Erfordert, dass die Beschreibung eines Attribut Typs eindeutig ist.

| Argumente | type |
|---|---|
| Beschreibung (= Identity) |Text |
| Rückgabetyp | type |
| Attributetypesetid | Text |

### <a name="attributetypesetaddattributetype"></a>Attributetypesetaddattributetype
Fügt einen neuen Attributtyp in einen vorhandenen Attributtyp ein. Erfordert, dass die Beschreibungen von Attributtyp und Attributtyp eindeutig sind.


|     Argumente      |                              type                              |
|--------------------|----------------------------------------------------------------|
| Attributetypesetid |                              Text                              |
|  Attributetypeid   |                              Text                              |
|       Order        |                             Number                             |
|     LocationID     | Text <br>**Hinweis** : der Speicherort ist entweder "Gruppe" oder "einfach". |
|     Obligatorisch.      |                    ```["0"|"1"|"N"|"Y"]```                  |
|    Rückgabetyp     |                              type                              |

### <a name="objecttypeaddattributetypeset"></a>Objecttyetaddattributetypeset
Fügt einem Objekttyp einen Attributtyp hinzu. Erfordert, dass die Beschreibung des Objekt Typs und des Attribut Typs eindeutig sind. Die Objekttypen lauten "System", "OrgUnit", "User", "Task".

| Argumente | type |
|---|---|
| ObjectTypeId | Text | 
| Attributetypesetid | Text | 
| Order | Number | 
| Sichtbar | <ul><li>0: der Attributtyp ist sichtbar.</li><li>2: der Attributtyp ist sichtbar, wenn die Schaltfläche " **Weitere Informationen** " ausgewählt ist.</li><li>1: der Attributtyp ist nicht sichtbar.</li></ul> | 
| Rückgabetyp | type | 

### <a name="orgunitadd"></a>Orgunitadd
Erstellt eine neue Organisationseinheit und gibt die ID der neuen Organisationseinheit zurück.

| Argumente | type |
|---|---|
| description | | 
| orgtypeid | | 
| parentID | | 
| Orgunitinheritedroles (optional) | | 
| Rückgabetyp | type | 
| ID der neuen Einheit | Der Parameter orgunitinheritedroles <br>hat entweder den Wert "yes" oder "No". | 

### <a name="orgunitaddsupervisor"></a>Orgunitaddsupervisor
Machen Sie einen Benutzer zu einem Vorgesetzten einer Organisationseinheit.

| Argumente | type |
|---|---|
| svroleid | Das Argument UserID kann auch verwendet werden. In diesem Fall ist die Rolle "Standard Supervisor" ausgewählt. Eine Standard-Supervisor-Rolle hat einen Namen wie **__svrole** gefolgt von einer Zahl. Das Argument UserID kann aus Gründen der Abwärtskompatibilität verwendet werden. | 
| Orgunitid | | 

### <a name="orgunitadduser"></a>Orgunitadduser
Machen Sie einen Benutzer zu einem Mitglied einer Organisationseinheit.

| Argumente | type |
|---|---|
| userID | | 
| Orgunitid | | 

### <a name="orgunitdelete"></a>Orgunitdelete
Entfernt eine Organisationseinheit.

| Argumente | type |
|---|---|
| Orgunitid | | 

### <a name="orgunitdeleteuser"></a>Orgunitdeleteuser
Entfernt einen Benutzer als Mitglied einer Organisationseinheit.

| Argumente | type |
|---|---|
| userID | | 
| Orgunitid | | 

### <a name="roleadd"></a>roleadd
Erstellt eine neue Rolle.

| Argumente | type |
|---|---|
| BESCHREIBUNG | | 
| svrole | | 
| svroleid (optional) | Wenn dieses Argument nicht vorhanden ist, wird eine Supervisor-Rolle des aktuellen Benutzers verwendet. | 
| Contextanpassbar (optional) | ```["0","1","N","Y"]``` | 
| Maxberechtigungs (optional) | Integer | 
| Maxrollen (optional) | Integer | 
| MaxUsers (optional) | Integer | 
| Rückgabetyp | type | 
| ID der neuen Rolle | | 

### <a name="roleaddorgunit"></a>roleaddorgunit
Weist einer Organisationseinheit eine Rolle zu.


|    Argumente    |                                      type                                      |
|-----------------|--------------------------------------------------------------------------------|
|    Orgunitid    |                                     roleID                                     |
| vererthisrole | "true" oder "false" gibt an, ob die Rolle den zugrunde liegenden Einheiten vorgeschlagen wird. |

### <a name="roleaddrole"></a>roleaddrole
Weist eine Rolle als unter Rolle einer anderen Rolle zu.

| Argumente | type |
|---|---|
| roleID | | 
| subroleid | | 

### <a name="roleaddsupervisor"></a>roleaddsupervisor
Machen Sie einen Benutzer zu einem Vorgesetzten einer Rolle.

| Argumente | type |
|---|---|
| svroleid | Das Argument UserID kann auch verwendet werden. In diesem Fall ist die Rolle "Standard Supervisor" ausgewählt. Eine Standard-Supervisor-Rolle hat einen Namen wie **__svrole** gefolgt von einer Zahl. Das Argument UserID kann aus Gründen der Abwärtskompatibilität verwendet werden. | 
| roleID | | 

### <a name="roleadduser"></a>roleadduser
Weist einem Benutzer eine Rolle zu. Die Rolle kann keine Kontext anpassbare Rolle sein, wenn keine ContextId angegeben wird.

| Argumente | type |
|---|---|
| userID | | 
| roleID | | 
| durationtype (optional) | Kann die Werte "Free", "Hours" und "Days" enthalten. | 
| durationlength (optional) | Erforderlich, wenn durationtype ' hours ' oder ' Days ' ist. sollte den ganzzahligen Wert für die Anzahl der Stunden oder Tage enthalten, zu denen die Rolle einem Benutzer zugewiesen ist. | 
| Start (optional) | Datum und Uhrzeit der Zuweisung der Rolle. Wenn dieses Attribut weggelassen wird, wird die Rolle sofort zugewiesen. Das Datumsformat ist "yyyy-mm-ddThh: NN: SS", wobei nur Jahr, Monat und Tag benötigt werden. Beispielsweise sind "2004-12-11" und "2004-11-28t08:00" gültige Werte. | 
| Ende (optional) | Datum und Uhrzeit, zu der die Rolle widerrufen wird. Wenn durationtype und durationlength angegeben werden, wird dieser Wert ignoriert. Das Datumsformat ist "yyyy-mm-ddThh: NN: SS", wobei nur Jahr, Monat und Tag benötigt werden. Beispielsweise sind "2004-12-11" und "2004-11-28t08:00" gültige Werte. | 
| linkreason | Erforderlich, wenn Start, Ende oder Dauer angegeben wird, andernfalls ignoriert. | 
| ContextId (optional) | Die ID der Organisationseinheit, die nur für Kontext anpassbare Rollen erforderlich ist. | 

### <a name="roledelete"></a>roledelete
Löscht eine Rolle.

| Argumente | type |
|---|---|
| roleID | | 

### <a name="roledeleteuser"></a>roledeleteuser
Entfernt die Rollenzuweisung zu einem Benutzer. Geerbte Rollen durch den Benutzer werden durch diesen Befehl widerrufen.

| Argumente | type |
|---|---|
| userID | | 
| roleID | | 
| ContextId (optional) |  | 

### <a name="roleproposeorgunit"></a>rolepropoeinorgunit
Schlägt eine Rolle vor, um Sie den Membern und den untergeordneten orgunits einer OrgUnit zuzuweisen.

| Argumente | type |
|---|---|
| Orgunitid | | 
| roleID | | 
| durationtype (optional) | Kann die Werte "Free", "Hours" und "Days" enthalten. | 
| durationlength | Erforderlich, wenn durationtype den Wert ' hours ' oder ' Days ' hat, sollte den ganzzahligen Wert für die Anzahl der Stunden oder Tage enthalten, zu denen die Rolle einem Benutzer zugewiesen ist. | 
| durationfixed | "true" oder "false" gibt an, ob die Zuweisung dieser Rolle zu einem Benutzer "durationlength" entsprechen soll. | 
| vererthisrole | "true" oder "false" gibt an, ob die Rolle den zugrunde liegenden Einheiten vorgeschlagen wird. | 

### <a name="taskadd"></a>Task hinzufügen
Erstellt eine neue Aufgabe, gibt die ID der neuen Aufgabe zurück.

| Argumente | type |
|---|---|
| applicationID | | 
| description | Text mit maximal 254 Zeichen. | 
| Taskname | Text mit maximal 254 Zeichen. | 
| "$ kengroupid" | | 
| svroleid (optional) | Wenn dieses Argument nicht vorhanden ist, wird eine Supervisor-Rolle des aktuellen Benutzers verwendet. | 
| contextanpassbar (optional) | ```["0","1","N","Y"]``` | 
| Unterbau (optional) | ```["0","1","N","Y"]``` | 
| Audit Action (optional) | <ul><li>0: unbekannt (Standard)</li><li>1: reportonly</li><li>2: alertappall</li><li>3: alertappobsolete</li><li>4: alertappmissing</li><li>5: enforceappall</li><li>6: enforceappobsolete</li><li>7: enforceappmissing</li><li>8: alertenforceappall</li><li>9: alertenforceappobsolete</li><li>10: alertenforceappmissing</li><li>11: importall</li></ul> | 
| auditalertmail (optional) | Die e-Mail-Adresse für Warnungen zu dieser Berechtigung wird vom Prüfer gesendet. Wenn dieses Argument nicht vorhanden ist, wird die e-Mail-Adresse des Prüfers verwendet. | 
| Maxrollen (optional) | Integer | 
| MaxUsers (optional) | Integer | 
| Rückgabetyp | ID des neuen Tasks. | 

### <a name="taskadditask"></a>taskadditask
Geben Sie an, dass zwei Tasks nicht kompatibel sind.

| Argumente | type |
|---|---|
| TaskID | | 
| taskID2 | | 

### <a name="taskaddrole"></a>taskaddrole
Weist einer Rolle eine Aufgabe zu.

| Argumente | type |
|---|---|
| roleID | | 
| TaskID | | 

### <a name="taskaddsupervisor"></a>taskaddsupervisor
Machen Sie einen Benutzer zu einem Vorgesetzten einer Aufgabe.

| Argumente | type |
|---|---|
| svroleid | Das Argument UserID kann auch verwendet werden. In diesem Fall ist die Rolle "Standard Supervisor" ausgewählt. Eine Standard-Supervisor-Rolle hat einen Namen wie **__svrole** gefolgt von einer Zahl. Das Argument UserID kann aus Gründen der Abwärtskompatibilität verwendet werden. | 
| TaskID | | 

### <a name="useradd"></a>Benutzer hinzufügen
Erstellt einen neuen Benutzer, gibt die ID des neuen Benutzers zurück.

| Argumente | type |
|---|---|
| description | | 
| alias | | 
| languageID | <ul><li>1: Englisch</li><li>2: Niederländisch</li></ul> | 
| Orgunitid | | 
EndDate (optional) |Das Datumsformat ist "yyyy-mm-ddThh: NN: SS", wobei nur Jahr, Monat und Tag benötigt werden. Beispielsweise sind "2004-12-11" und "2004-11-28t08:00" gültige Werte. | 
| deaktiviert (optional) | <ul><li>0: aktiviert</li><li>1: deaktiviert</li></ul> | 
| Maxberechtigungs (optional) | Integer | 
| Maxrollen (optional) | Integer | 
| Rückgabetyp | ID des neuen Benutzers. | 

### <a name="useraddrole"></a>Useraddrole
Fügt eine Benutzerrolle hinzu.
<!--- missing content -->

| Argumente | type |
|---|---|
|  | | 

### <a name="userdeleterole"></a>Userdeleterole 
Löscht eine Benutzerrolle.
<!--- missing content -->

| Argumente | type |
|---|---|
|  | | 

### <a name="userupdate"></a>Userupdate
Aktualisiert einen Benutzer.

| Argumente | type |
|---|---|
| UserID | | 
| Beschreibung (optional)|  | 
| language | <ul><li>1: Englisch</li><li>2: Niederländisch</li></ul> | 
| userdeaktiviert (optional) |<ul><li>0: aktiviert</li><li>1: deaktiviert</li></ul> | 
| Userenddate (optional) | Das Datumsformat ist "yyyy-mm-ddThh: NN: SS", wobei nur Jahr, Monat und Tag benötigt werden. Beispielsweise sind "2004-12-11" und "2004-11-28t08:00" gültige Werte. | 
| FirstName (optional) | | 
| MiddleName (optional) | | 
| LastName (optional) | | 
| maxberechtigungs (optional) | Integer |
| maxrollen (optional) | Integer | 


## <a name="getinfo-functions"></a>GetInfo-Funktionen
Der in diesem Abschnitt beschriebene Satz von Funktionen kann verwendet werden, um Informationen abzurufen, die im BHOLD-System gespeichert sind. Jede Funktion kann mit der GetInfo-Funktion aus dem bscript-Objekt aufgerufen werden. Einige Objekte erfordern Parameter. Die zurückgegebenen Daten unterliegen den View-Berechtigungen und den überwachten Objekten des angemeldeten Benutzers.

### <a name="getinfo-arguments"></a>GetInfo-Argumente

| Name | BESCHREIBUNG |
|---|---|
| applications | Gibt eine Liste der Anwendungen zurück. | 
| attributetypes | Gibt eine Liste von Attributtypen zurück. | 
| orgtypes | Gibt eine Liste der Organisations Einheits Typen zurück. | 
| OrgUnits | Gibt eine Liste der Organisationseinheiten ohne die Attribute der Organisationseinheiten zurück. | 
| Orgunitpropoerdroles | Gibt eine Liste der vorgeschlagenen Rollen zurück, die mit der Organisationseinheit verknüpft sind. | 
| Orgunitroles | Gibt eine Liste der direkt verknüpften Rollen der angegebenen Organisationseinheit zurück. | 
| Objecttypeer attributetypes | | 
| Berechtigungen | | 
| permissionusers | | 
| roles | Gibt eine Liste der Rollen zurück. | 
| roletasks | Gibt eine Liste der Aufgaben der angegebenen Rolle zurück. | 
| Tasks | Gibt alle Aufgaben zurück, die von BHOLD bekannt sind. | 
| users | Gibt eine Liste der Benutzer zurück. | 
| usersrollen | Gibt die Liste der verknüpften Supervisor-Rollen des angegebenen Benutzers zurück. | 
| Benutzerberechtigungen | Gibt die Liste der Berechtigungen für den angegebenen Benutzer zurück. | 

### <a name="orgunit-info"></a>OrgUnit-Informationen

| Name | Parameter | Rückgabetyp |
|---|---|---|
| OrgUnit | Orgunitid | OrgUnit | 
| Orgunitasiattribute | Orgunitid | Sammlung | 
| OrgUnits| Filter (optional), proptypeid (optional)<br> Sucht nach Einheiten, die die in " **Filter** " beschriebene Zeichenfolge in dem in " **proptypeid** " beschriebenen propType enthalten. Wenn diese ID weggelassen wird, gilt der Filter für die Einheits Beschreibung. Wenn kein Filter angegeben wird, werden alle sichtbaren Einheiten zurückgegeben. | Sammlung | 
| Orgunitorigunits | Orgunitid | Sammlung | 
| Orgunitparents | Orgunitid | Sammlung | 
| Orgunitpropertyvalues | Orgunitid | Sammlung | 
| Orgunitproptypes |  | Sammlung | 
| Orgunitusers | Orgunitid | Sammlung | 
| Orgunitpropoerdroles | Orgunitid | Sammlung | 
| Orgunitroles | Orgunitid | Sammlung | 
| Orgunitinheritedroles | Orgunitid | Sammlung | 
| Orgunitsupervisor | Orgunitid | Sammlung | 
| Orgunitinheritedsupervisor| Orgunitid | Sammlung | 
| Orgunitsupervisorrollen | Orgunitid | Sammlung | 

### <a name="role-information"></a>Rollen Informationen

| Name | Parameter | Rückgabetyp |
|---|---|---|
| Rolle (role) | roleID | Objekt | 
| roles | Filter (optional) | Sammlung | 
| roleasiattribute | roleID | Sammlung | 
| roleorgunits | roleID | Sammlung | 
| roleparser-Rollen | roleID | Sammlung | 
| rolesubrollen | roleID | Sammlung | 
| rolesupervisor | roleID| Sammlung | 
| rolesupervisorrollen | roleID | Sammlung | 
| roletasks | roleID | Sammlung | 
| roleusers | roleID | Sammlung | 
| rolesupervisorrollen | roleID | Sammlung | 
| "proposerdroleorgunits" | roleID | Sammlung | 
| propoerdroleusers | roleID | Sammlung | 

### <a name="permission---task-information"></a>Berechtigung: Aufgabeninformationen

| Name | Parameter | Rückgabetyp |
|---|---|---|
| Berechtigung (permission) | TaskID | Berechtigung | 
| Berechtigungen | Filter (optional) | Sammlung | 
| permissionasiattribute | TaskID | Sammlung | 
| permissionanlagen | TaskID | Sammlung | 
| permissionattributetypes | - | Sammlung | 
| permissionparametriams | TaskID | Sammlung | 
| permissionrollen | TaskID | Sammlung | 
| permissionsupervisor | TaskID | Sammlung | 
| permissionsupervisorrollen | TaskID | Sammlung | 
| permissionusers | TaskID | Sammlung | 
| task | TaskID | Aufgabe | 
| Tasks| Filter (optional) | Sammlung | 
| taskattachments | TaskID | Sammlung | 
| taskparametriams | TaskID | Sammlung | 
| taskroles | TaskID | Sammlung | 
| Task Supervisor | TaskID | Sammlung | 
| tasksupervisorrollen | TaskID | Sammlung | 
| taskusers | TaskID | Sammlung | 

### <a name="user-information"></a>Benutzerinformationen

| Name | Parameter | Rückgabetyp |
|---|---|---|
| user | UserID | Benutzer | 
| users | Filter (optional), attributetypeid (optional) <br>Sucht nach Benutzern, die in dem von attributetypeid angegebenen attributeType die durch Filter angegebene Zeichenfolge enthalten. Wenn diese ID weggelassen wird, gilt der Filter für den Standardalias des Benutzers. Wenn kein Filter angegeben wird, werden alle sichtbaren Benutzer zurückgegeben. Beispiel:<ul><li>`GetInfo("users")` Gibt alle Benutzer zurück.</li><li>`GetInfo("users", "%dmin%")` Gibt alle Benutzer mit der Zeichenfolge "DMin" im Standardalias zurück.<br></li><li>Angenommen, Benutzer haben ein zusätzliches Attribut mit dem Namen `"City".GetInfo("users", "%msterda%", "City")` . Dieser Rückruf gibt alle Benutzer zurück, die die Zeichenfolge "msterda" im City-Attribut haben.</li></ul> | UserCollection | 
| usersapplications | UserID | Sammlung | 
| Benutzerberechtigungen | UserID | Sammlung | 
| UserRoles " | UserID | Sammlung | 
| usersrollen | UserID | Sammlung | 
| userstasks | UserID | Sammlung | 
| usersunits | UserID | Sammlung | 
| usertasks | UserID | Sammlung | 
| userunits | UserID | Sammlung | 

### <a name="return-types"></a>Rückgabetypen
In diesem Abschnitt werden die Rückgabe Typen der GetInfo-Funktion beschrieben.

| Name | Rückgabetyp |
|---|---|
| Sammlung |```=<ITEMS>{<ITEM description="..." id="..." />}</ITEMS>``` | 
| Objekt |```=<ITEM type="…" description="..." />``` | 
| OrgUnit |```= <ITEM id="…" description="..." orgtype="..." parent="..."> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Berechtigung |```= <ITEM id="…" description="…" name="…" tokengroup="…" application="…" > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Rollen |```= <ITEMS> {<ITEM id="…" description="…" />} </ITEMS>``` | 
| Role |```= <ITEM id="…" description="… " > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Aufgabe | Siehe Berechtigung | 
| Benutzer | ```= <ITEMS> {<ITEM description="…" id="…" alias="…" />} </ITEMS>``` | 
| Benutzer |```= <ITEM id="…" description="…" alias="…" firstname="…" lastname="…" uuid="…" language="…"> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 

## <a name="script-sample"></a>Skript Beispiel

Ein Unternehmen verfügt über einen BHOLD-Server und möchte ein automatisiertes Skript, das neue Kunden erstellt. Die Informationen über das Unternehmen und seinen Kauf-Manager werden in eine angepasste Webseite eingegeben. Alle Kunden werden im Modell als Einheit unter den Einheiten Kunden präsentiert. Der Kauf-Manager ist ebenso ein Mitglied als Supervisor dieser Einheit. Eine Rolle wird erstellt, die den Besitzern das Recht erhält, den Namen des neuen Kunden zu erwerben.

Der Kunde ist jedoch nicht in der Anwendung vorhanden. Es gibt eine spezielle Funktion, die in der "ASI functiondispatch" implementiert ist, die ein neues Kundenkonto in der Kauf Anwendung erstellt. Jeder Kunde verfügt über einen Customer-Typ.

Die möglichen Typen können auch von der functiondispatch-Funktion dargestellt werden. Der AA wählt den richtigen Typ für den neuen Kunden aus.

Erstellen Sie eine Rolle und einen Task, um die Kauf Berechtigungen zu präsentieren. Das eigentliche Kauf Privileg wird von der ASI als-Datei präsentiert `/customers/customer id/purchase` . Diese Datei sollte mit der neuen Aufgabe verknüpft werden.

Die Active Server Seite, auf der die Informationen gesammelt werden, sieht wie folgt aus:

```VB
<%@ Language=VBScript %>
<% Option Explicit %>
<html>
<body>
<form action="MySubmit.asp" method=post>
<input type="hidden" name="OrgUnitID" 
     value="<% = Request("ID") %>">
Company <input type="text" name="Description"> <br>
Type <select name="OrgType">
<%Dim oOrgType
For Each oOrgType on bscript.getinfo("Orgtypes") %>
<option value="<% = oOrgType.OrgTypeID %>">
<% = oOrgType.Description %>
</option> <%
Next %>
</select>  <br>
Manager <input type="text" name=" manager"> <br>
Alias <input type=" text" name=" alias"> <br>
e-mail <input type=" text" name=" email"> <br>
<input type="submit">
</form>
</body>
</html>
```

Alle angepassten Seiten müssten die richtigen Informationen anfordern und ein XML-Dokument mit den angeforderten Informationen erstellen. In diesem Beispiel transformiert die Seite mysubmit die Daten im XML-Dokument und weist Sie der b1script zu **. Parameter** -Objekt, und schließlich wird die- `b1script.ExecuteScript("MyScript")` Funktion aufgerufen.

Das folgende Eingabe Skript zeigt dieses Beispiel:

```
<customer>
<description>ACME inc.</description>
<orgtype>5<orgtype>
<name>John Doe</name>
<alias>jdoe</alias>
<email>jdoe@acme.com</email>
</customer>
```

Dieses Eingabe Skript enthält keine Befehle für BHOLD. Dies liegt daran, dass dieses Skript nicht direkt von BHOLD ausgeführt wird. anstatt Sie zu verwenden, handelt es sich hierbei um die Eingabe für eine vordefinierte Funktion. Diese vordefinierte Funktion übersetzt dieses Objekt mit BHOLD-Befehlen in ein XML-Dokument. Dieser Mechanismus besteht darin, dass der Benutzer keine Skripts an das BHOLD-System sendet, die Funktionen enthalten, die der Benutzer nicht ausführen darf, wie z. b. SETUSER und Funktionsverteilung an einen ASI.

```
<?xml version="1.0" encoding="utf-8" ?> 
- <functions xmlns="http://tempuri.org/BscriptFunctions.xsd">

  <function name="roleadduser" roleid="" userid="" /> 
  <function name="roledeleteuser" roleid="" userid="" /> 
  </functions>
```

## <a name="next-steps"></a>Nächste Schritte
- [BHOLD concepts guide (Leitfaden für BHOLD-Konzepte)](../bhold/bhold-concepts-guide.md)
- [BHOLD-Versionsverlauf](version-bhold-history.md)
