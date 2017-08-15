---
title: "XML-Referenz für die Ressourcensteuerungs-Anzeigekonfiguration | Microsoft-Dokumentation"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>XML-Referenz für die Ressourcensteuerungs-Anzeigekonfiguration

RCDC-Ressourcen (Ressourcensteuerungs-Anzeigekonfiguration) sind benutzerdefinierte Ressourcen, mit denen Sie steuern können, wie andere Ressourcen im MIM 2016 SP1-Datenspeicher (Microsoft Identity Manager) auf der Benutzeroberfläche (UI) für den Endbenutzer angezeigt werden. Jede RCDC-Ressource enthält eine XML-Konfigurationsdatei, die Sie zum Hinzufügen, Ändern oder Entfernen von Benutzeroberflächentext und Steuerelementen der Benutzeroberfläche bearbeiten können. MIM 2016 SP1 stellt zwar mehrere standardmäßige RCDC-Ressourcen bereit, Sie können jedoch auch eigene benutzerdefinierte RCDC-Ressourcen als benutzerdefinierte Ressourcen erstellen. Weitere Informationen über die Verwendung der RCDC-Benutzeroberfläche im FIM-Portal finden Sie in der FIM-Dokumentation unter [Einführung in die Konfiguration und Anpassung des FIM-Portals](http://go.microsoft.com/fwlink/?LinkID=165848).


## <a name="known-issues"></a>Bekannte Probleme

Der Standardwert für viele Steuerelemente der Ressourcensteuerungs-Anzeigekonfiguration wird nicht unterstützt.

In diesem Release wird das Festlegen von Standardwerten in Steuerelementen einer Ressourcensteuerung nicht unterstützt, ausgenommen für das Optionsfeld-Steuerelement. Sie können dieses Problem bei einem Dropdownfeld umgehen, indem Sie einen Standardwert angeben, der nicht mit einem Wert verknüpft ist, um den Benutzer zum Ändern der Auswahl zu zwingen. Um dieses Problem bei anderen Steuerelementen zu umgehen, müssen Sie einen Autorisierungsworkflow verwenden, um während der Übermittlung der Anforderung einen Standardwert anzugeben.

## <a name="basic-structure"></a>Grundlegende Struktur

Die XML-Daten für eine RCDC-Ressource enthält das einzelne XML-Element **ObjectControlConfiguration**.

>[!NOTE]
Das vollständige XSD-Schema finden Sie weiter unten in diesem Dokument unter „Anhang A: XSD-Standardschema“.

Im Folgenden finden Sie das XSD-Schema für das Element „ObjectControlConfiguration“:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



Das Element **ObjectControlConfiguration** enthält Folgendes:

1.  **ObjectDataSource**: Dieses Element gibt die TypeName-Eigenschaft einer Datenquellklasse an, die die Ressourcensteuerung (Resource Control, RC) verwendet. Eine Beschreibung und die Schemadefinition finden Sie in diesem Dokument im nachfolgenden Abschnitt „Datenquellen“. Ein **ObjectControlConfiguration**-Element kann bis zu 32 Knoten des Elements **ObjectDataSource** enthalten.

2.  **XmlDataSource**: Dies ist eine einfache Datenquelle, die am häufigsten zum Festlegen des Entwurfs einer Zusammenfassungsseite verwendet wird. Eine Beschreibung und die Schemadefinition finden Sie in diesem Dokument im nachfolgenden Abschnitt „Datenquellen“. Ein **ObjectControlConfiguration**-Element kann bis zu 32 Knoten des Elements **XmlDataSource** enthalten.

3.  **Panel**: Der Administrator kann das Layout der RCDC-Seite durch Ändern der Elemente in den Panel-Elementen anpassen. Weitere Informationen finden Sie weiter unten in diesem Dokument im Abschnitt „Panel“. Ein **ObjectControlConfiguration**-Element darf nur ein Panel-Element aufweisen.

4.  **Events**: Administratoren können keinen angepassten CodeBehind bereitstellen, da diese Funktion beschränkt ist. Dies ist das Ereignis, das von einem Panel oder einem Steuerelement basierend auf einer Statusänderung ausgegeben werden kann. Weitere Informationen finden Sie weiter unten in diesem Dokument im Abschnitt „Events“. Ein **ObjectControlConfiguration**-Element kann optional ein **Event**-Element enthalten. Im Allgemeinen gilt: Die Verwendung von benutzerdefinierten **Events** wird nicht unterstützt, es sei denn, sie werden im Zuge nachfolgender Verbesserungen speziell entwickelt.

## <a name="data-sources"></a>Datenquellen

Microsoft Identity Manager verwendet Datenquellen als Möglichkeit, um Daten an UI-Komponenten zu binden. Dadurch wird die Trennung der Daten von der Darstellungsschicht vereinfacht. Es gibt zwei Arten von Datenquellen in RCDC-Ressourcenkonfigurationsdaten: **ObjectDataSource** und **XmlDataSource**.

-   **ObjectDataSources** gibt eine Microsoft .NET-Klasse an, die Daten für die RC bereitstellt. Es gibt einen festen Satz von verfügbaren ObjectDataSources-Typen, vorausgesetzt, der Administrator kann beim Erstellen von RCDCs selbst über deren Nutzung entscheiden.

-   **XMLDataSources** bietet eine einfache Möglichkeit zur Strukturierung von XML-basierten Daten und kann von Administratoren zur Bereitstellung benutzerdefinierter Daten verwendet werden. Die XML-Daten müssen direkt in der RCDC angegeben werden, es sei denn, Sie verwenden die integrierte, vordefinierte XML-Struktur. Die integrierte XML-Struktur wird zum Generieren von Zusammenfassungsseiten in der RC verwendet.

Diese Datenquellen können Sie in der RCDC an die Attribute der UI-Steuerelemente binden, die in der RCDC zum Generieren der Benutzeroberfläche angegeben werden.

### <a name="objectdatasource"></a>ObjectDataSource

In der folgenden Tabelle werden die von Microsoft Identity Manager bereitgestellten, allgemeinen Datenquellentypen angegeben, die für alle Ressourcentypen verfügbar sind (sofern nicht anders vermerkt).

| TypeName                        | Beschreibung     | Unterstützung für bidirektionale Bindung | Unterstützte Bindungssyntax        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Dies ist die FIM 2010-Ressource, die erstellt, bearbeitet oder angezeigt wird. Der Pfad in der Bindungszeichenfolge ist der Attributname. Beachten Sie, dass der Ressourcentyp vom Attribut „TargetObjectType“ der RCDC statt im Attribut „RCDC.ConfigurationData“ angegeben wird. | Ja                     | [AttributeName] Dies ist der Wert des Objektattributs, das durch dessen Namen angegeben wird.    |
| PrimaryResourceDeltaDataSource  | Diese Datenquelle erstellt das Delta-XML, das den ursprünglichen Zustand mit dem aktuellen Zustand der FIM 2010-Ressource vergleicht. Das generierte Delta-XML wird vom RC-Zusammenfassungssteuerelement genutzt, um die Benutzeroberfläche für die vom Benutzer übermittelte Anforderung zu rendern.                                    | Nein                      | DeltaXml: </br> Diese Syntax wird mit dem Zusammenfassungssteuerelement zum Anzeige des Deltas verwendet.                                                 |
| PrimaryResourceRightsDataSource | Diese Datenquelle stellt die Inlinerechte für jedes Attribut der FIM 2010-Ressource bereit. Hierdurch kann die RC vor der Übermittlung feststellen, welche Berechtigungen dieser Benutzer bezüglich dieses Attributs besitzt, und anschließend die Benutzeroberfläche für dieses Attribut entsprechend rendern.                     | Nein                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Mit dieser Datenquelle kann auf schemabezogene Informationen zugegriffen werden, z.B. den Anzeigenamen, die Beschreibung, die Notwendigkeit des Attributs sowie Informationen über den Ressourcentyp.                                                                                             | Nein                      | [AttributName].**Required** Dies ist der boolesche Wert, der angibt, ob das Attribut einen Wert aufweisen muss, um als gültig erachtet zu werden. <br/> [AttributName].**DisplayNameString** Dieser Wert gibt den Anzeigenamen der Bindung an. <br/>[AttributName].**DescriptionString** Dieser Wert gibt die Beschreibung der Bindung an. <br/>[AttributeName].StringRegexString Dieser Wert gibt den regulären Ausdruck für die Zeichenfolge einer Bindung an. <br/>[AttributeName].**DisplayName** <br/> [AttributeName].**Description** <br/> [AttributeName]. [AttributeName].**IntergerValueMinimum** <br/>[AttributeName].**IntergerValueMaximum** <br/>[AttributeName].**LocalizedAllowedValues**|
| DomainDataSource                | Diese Datenquelle stellt eine Enumeration von Domänen basierend auf Domänenkonfigurationsressourcen bereit. Beachten Sie, dass diese Datenquelle nur in RCDCs verwendet werden kann, die für Gruppen- und Benutzerressourcen bestimmt sind.                                                                           | Ja                     | Domain           |

Im Folgenden finden Sie einen RCDC-Beispielausschnitt, der für die Bearbeitung des Description-Attributs einer Gruppe drei Datenquellen an das Steuerelement „UocTextBox“ bindet:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Mithilfe einer **XMLDataSource**-Klasse können Sie benutzerdefinierte Daten angeben, die die RCDC für eine bestimmte Ressource nutzen kann. In diesem Fall müssen die XML-Daten in der RCDC angegeben werden. Alternativ können Sie mithilfe dieser Datenquelle eine integrierte XML-Datenstruktur referenzieren, um die Benutzeroberfläche für Zusammenfassungsseiten zu rendern. Beim Festlegen der **XMLDataSource**-Klasse in der RCDC können Sie festlegen, welcher Klassentyp verwendet werden soll.


| TypeName                 | Beschreibung   | | |
|--------------------------|------------|
| **XMLDataSource**            | Die Datenquelle stellt XML-Daten dar. Sie kann entweder das XSL- oder das eingebettete XSL-Format aufweisen: <br/>**XSL-Format:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters=”Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl”> </my:XmlDataSource><br/> **Eingebettetes XSL-Format:** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |Nein | ```Xpath[;namespaces]``` <br/> „Where:Xpath“ ist ein gültiger XML-XPath-Ausdruck, um den erforderlichen Hinweis, am häufigsten mit „/“ (Stamm), auszuwählen. <br/>Namespaces ist eine optionale Liste mit durch Semikolons getrennten „prefix=URI“-Zeichenfolgen. Diese wird nur benötigt, wenn XPath bei einem XML-Ausdruck mit Namespaces verwendet werden muss. |
| **ReferenceDeltaDataSource** | Die Datenquelle stellt die Deltas von mehrwertigen Verweisattributen dar. Sie wird nur in der RCDC für „Group“ und „Set“ verwendet. <br/> Obwohl die Datenquelle nicht auf Gruppen oder Listen beschränkt ist, sind Codeänderungen im RCDC-Host erforderlich, um derartige Deltas zu übermitteln. „Group“ und „Set“ sind derzeit die einzigen Hosts, die diese Datenquelle erkennen.  | Ja                      | [AttributeName].Add, wobei [AttributeName] ein Verweisattribut darstellt. Die zurückgegebenen Daten fungieren als Deltaerweiterungen. <br/> Beispiel: [ReferenceAttribute].Add <br/>Beispiel: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributName].Remove, wobei [AttributName] ein Verweisattribut darstellt. Die zurückgegebenen Daten fungieren als Deltaentfernungen. <br/> DeltaXml |
|**RequestDetailsDataSource**| Die Datenquelle stellt das Attribut „RequestParameter“ der Request-Objekte dar. Der Parameter legt die maximale Anzahl der Attributwerte fest, die pro mehrwertiges Attribut angezeigt werden. Dieser Typ wird in der RCDC nur für Anforderungen verwendet. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Nein | DeltaXml |
|**RequestStatusDataSource**| Die Datenquelle stellt das Attribut **RequestStatusDetails** der Request-Objekte dar. <br/>Dieser Typ wird in der RCDC nur für Anforderungen verwendet.  | Nein | DeltaXml |

-   So definieren Sie eine benutzerdefinierte XML-Datenquelle:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Um das integrierte XSL-Zusammenfassungssteuerelement zu verwenden, definieren Sie die Datenquelle wie folgt:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Wenn Sie eine RCDC für einen benutzerdefinierten Ressourcentyp erstellen, können Sie mithilfe dieser Methode automatisch eine Zusammenfassungsseite für diese benutzerdefinierte Ressource rendern.

Im Folgenden finden Sie ein Beispiel dazu, wie Sie eine Zusammenfassungsregisterkarte in der RCDC erstellen, indem Sie „PrimaryResourceDeltaDataSource“ mit der „XMLDataSource“-Klasse anhand der integrierten XSL verwenden:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Alternativ kann der Benutzer das zuvor angegebene XmlDataSource-Element durch das folgende Format ersetzen, um ein benutzerdefiniertes Layout von einer Zusammenfassungsseite zu definieren. Als Referenz ist weiter unten in diesem Dokument unter „Anhang B: Standardmäßige Zusammenfassungs-XSL“ die standardmäßige Zusammenfassungs-XSL von FIM 2010 beigefügt.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schema für Datenquellen
Im Folgenden wird das XSD-Schema für beide Arten von Datenquellen aufgeführt:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Ereignisse
Ein Event-Element definiert den Änderungsstatus eines Steuerelements. Die Erweiterbarkeit dieser Funktion ist beschränkt, da Sie keine benutzerdefinierte Funktion (Handler) schreiben können, um das Verhalten nach der Auslösung eines Ereignisses zu definieren. Dieses Event-Element kann im Panel-Element verwendet werden. Weitere Informationen finden Sie weiter unten in diesem Dokument im Abschnitt „Panel“. Im Folgenden wird das XSD-Schema für das Event-Element aufgeführt:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Ein Event-Element ist ein leeres Element mit zwei Attributen.

**Attribute:**

1.  **Name**: Dies ist der eindeutige Name eines Ereignisses. Das einzige unterstützte Ereignis in **ObjectControlConfiguration** ist das Load-Ereignis. Dieses Ereignis wird ausgelöst, wenn die Seite zum ersten Mal geladen wird.

2.  **Handler**: Dies ist der eindeutige Name eines Handlers. Beim Auslösen des Ereignisses wird in der Regel eine Programmmethode aufgerufen, um die Änderung des Steuerelementstatus zu behandeln. Die folgenden Fälle werden nicht unterstützt: Entfernen eines vorhandenen Handlers von einem vorhandenen Steuerelement, Erstellen einen neuen Handlers und Anfügen eines Handlers an ein vorhandenes oder neues Steuerelement.

„Beispiel:

Nachfolgend wird ein Beispiel für ein Events-Element vorgestellt:
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


Das Panel-Element ist das Kernelement in einem RCDC-Layout. Im Folgenden wird das XSD-Schema für das Panel-Element aufgeführt:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Dieses Element enthält das sich wiederholende Element „Grouping“. Weitere Informationen finden Sie im Abschnitt „Grouping“ dieses Dokuments.

Das Panel-Element enthält vier Attribute:

1.  **Name**: Dieses Attribut ist der Name des Panels. Dies ist ein erforderliches Attribut vom Typ „String“.

2.  **DisplayAsWizard**: Dieses Attribut ist derzeit als veraltet markiert. Das entsprechende VerbContext-Attribut in der RCDC steuert, ob sich das Ressourcenlayout im Assistent- oder Registerkartenmodus befindet. Wenn es auf „0“ (Erstellungsmodus) festgelegt ist, befindet es sich auch im Assistentmodus. Andernfalls befindet es sich im Registerkartenmodus. Weitere Informationen finden Sie in der Dokumentation unter „Einführung in die Konfiguration und Anpassung des FIM-Portals“.

3.  **Caption**: Dieses Attribut ist derzeit als veraltet markiert. Der Benutzer kann die Beschriftungen für eine Seite angeben, indem er eine Gruppe angibt, die nur Headerinformationen enthält. Weitere Informationen finden Sie im Abschnitt „Grouping“ dieses Dokuments.

4.  **AutoValidate**: Dies ist ein optionales boolesches Attribut. Wenn es auf „true“ festgelegt ist, wird die Validierung für jedes Steuerelement auf der aktuellen Registerkarte ausgelöst. Wenn das Attribut fehlt, wird es standardmäßig auf „true“ festgelegt. Es kann in Kombination mit der Eigenschaft „RegularExpression“ verwendet werden. Weitere Informationen finden Sie unter „RegularExpression“ in einem Abschnitt weiter unten in diesem Dokument.

## <a name="grouping"></a>Grouping

Das Grouping-Element definiert das allgemeine Layout eines Panels. Es dient als Container, der einzelne Steuerelemente in verschiedene Abschnitte und Registerkarten gruppiert. Im Folgenden wird das XSD-Schema für das Grouping-Element aufgeführt:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Es gibt drei Arten von **Grouping**-Elementen:

1.  **Header Grouping**: Ein Header Grouping-Element ist optional. Es darf nur ein Header Grouping-Element in einem **Panel** vorhanden sein. Ein Header Grouping-Element wird im oberen Teil eines Panels als Beschriftung angezeigt.
    In dieser Gruppierung darf nur eine UocCaptionControl-Klasse verwendet werden. Ein Beispiel für ein Header Grouping-Element finden Sie im Abschnitt „Beispiel“.

2.  **Content Grouping**: Es ist mindestens ein Content Grouping-Element erforderlich. In einem Panel können mehrere Content Grouping-Elemente vorhanden sein. Ein Content Grouping-Element wird als Hauptinhalt einer RCDC-Seite angezeigt. Jedes Content Grouping-Element wird als Registerkarte im selben Panel angezeigt und kann bis zu 256 Steuerelemente umfassen. Ein Beispiel für ein **Content Grouping**-Element finden Sie im Abschnitt „Beispiel“.

3.  **Summary Grouping**: Ein Summary Grouping-Element ist optional. Es darf nur ein Summary Grouping-Element in einem Panel vorhanden sein. Ein Summary Grouping-Element wird als letzte Registerkarte eines Panels angezeigt. In einem Summary Grouping-Element darf nur ein **UocHtmlSummary**-Steuerelement verwendet werden, um die Änderungen anzuzeigen, die der Benutzer vor dem Senden einer Anforderung vorgenommen hat. Ein Beispiel für ein Summary Grouping-Element finden Sie im Abschnitt „Beispiel“.

Jeder Grouping-Typ enthält folgende Elemente:

1.  **Help**: Dieses Element stellt Hilfetext auf einer Registerkarte bereit. Sie können damit auch einen Link zu einer Hilfedatei für die Registerkarte hinzufügen.

2.  **Controls**: Informationen zu diesem Element finden Sie im Abschnitt „Control“ in diesem Dokument. Jede Gruppierung muss abhängig vom Gruppierungstyp zwischen 1 und einschließlich 256 Steuerelementen aufweisen.

3.  **Events**: Informationen zu diesem Element finden Sie in diesem Dokument im Abschnitt „Events“. Jede Gruppierung kann optional ein Event-Element enthalten. In einem Grouping-Element werden folgende Ereignisse unterstützt:

    - **BeforeLeave**: Dieses Ereignis wird ausgelöst, wenn der Benutzer eine Registerkarte in einem a Content Grouping-Element verlassen möchte.
    - **AfterEnter**: Dieses Ereignis wird ausgelöst, wenn der Benutzer eine Registerkarte in einem Content Grouping-Element aufrufen möchte.

Attribute:

1.  **Name**: Dies ist der erforderliche Name des Grouping-Elements. Dieser **Name** muss innerhalb des **Panel**-Elements eindeutig sein.

2.  **Caption**: Das **Caption**-Element wird als Headerbeschriftung in einem Header Grouping-Element angezeigt. Es wird als Registerkartenbeschriftung eines Content Grouping- oder Summary Grouping-Elements angezeigt.

3.  **Description**: Das optionale Zeichenfolgenattribut **Description** funktioniert nur, wenn es in einem Content Grouping-Element verwendet wird. Verwenden Sie dieses Element, um dem Endbenutzer auf derselben Registerkarte Details über die Informationen bereitzustellen.

  >[!NOTE]
  Wenn dieses Attribut in einem Summary Grouping-Element verwendet wird, gilt der XML-Code als ungültig. Wenn dieses Attribut in einem Header Grouping-Element verwendet wird, gilt der XML-Code zwar als gültig, wird jedoch ignoriert.

4.  **Enabled**: Das optionale boolesche Attribut „Enabled“ wird auf „true“ festgelegt, wenn es fehlt. Wenn „Enabled“ auf „false“ festgelegt wird, wird dem Endbenutzer die Registerkarte „Deaktiviert“ angezeigt. Dieses Attribut funktioniert nur in einem Content Grouping-Element.

  >[!NOTE]
  Wenn dieses Attribut in einem Summary Grouping-Element verwendet wird, gilt der XML-Code als ungültig. Wenn dieses Attribut in einem Header Grouping-Element verwendet wird, gilt der XML-Code zwar als gültig, wird jedoch ignoriert.

5.  **Visible**: Sie können eine Registerkarte der RCDC-Seite oder deren Überschrift ausblenden, indem Sie dieses Attribut auf „false“ festlegen. Standardmäßig ist dieses optionale Attribut vom Typ „Boolean“ auf „true“ festgelegt. Dieses Attribut funktioniert nur in einem Content Grouping-Element.

  >[!NOTE]
  Wenn nur ein Content Grouping-Element in einem Panel vorhanden ist, funktioniert diese Funktion nicht. Wenn mehrere Content Grouping-Elemente in einem Panel vorhanden sind, wird das oben beschriebene Verhalten ausgelöst.

6.  **IsHeader**: Dieses Attribut ist ein optionales boolesches Attribut, das definiert, ob es sich bei dem Grouping-Element um ein Header Grouping-Element handelt. Wenn dieses Attribut nicht angegeben ist, wird es auf „false“ festgelegt.

7.  **IsSummary**: Dies ist ein optionales boolesches Attribut, das definiert, ob es sich bei dem Grouping-Element um ein Summary Grouping-Element handelt. Wenn dieses Attribut nicht angegeben ist, wird es auf „false“ festgelegt.

![RCD-Konfigurations-XML](media/rcd-configuration-xml-reference/image005.jpg)

Mit dem folgenden XML-Beispielcode wird der voranstehende Header Grouping-Text generiert. Das Header Grouping-Element ist der Bereich mit dem Text „Header Grouping – Beispiel“.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![RCD-Konfigurations-XML](media\rcd-configuration-xml-reference/image007.jpg)

Mit dem folgenden XML-Beispielcode wird der voranstehende Content Grouping-Text generiert. Das Content Grouping-Element ist die Registerkarte ganz links mit dem Text **Content Grouping – Beispiel**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![RCD-Konfigurations-XML](media/rcd-configuration-xml-reference/image010.jpg)

Mit dem folgenden XML-Beispielcode wird die voranstehende Summary Grouping-Text generiert. Das Summary Grouping-Element ist die Registerkarte ganz rechts mit dem Text **Zusammenfassung**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Hilfe

Das Help-Element kann optional in ein Grouping- oder Control-Element eingeschlossen werden. Wenn es in einem Grouping-Element verwendet wird, muss es als erstes Element verwendet werden. Es enthält textbasierte Hilfe für Endbenutzer, um ihnen genaue Informationen bereitstellen zu können. Im Folgenden wird das XSD-Schema für das Help-Element aufgeführt:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Im Folgenden wird ein Beispiel für das Help-Element vorgestellt:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Control

Ein Grouping-Element enthält ein oder mehrere Control-Elemente. Control-Elemente sind die Hauptelemente in einer RCDC. Sie können das Grouping-Element anpassen, indem Sie die verschiedenen Control-Elemente, die es enthalten, definieren. Im Folgenden wird das XSD-Schema für das Control-Element aufgeführt:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Ein Control-Element enthält folgende Elemente:

1.  **Help**: Dieses Element wird ignoriert. Es funktioniert nur im Grouping-Element.

2.  **CustomProperties**: Dieses Element wird nicht unterstützt.

3.  **Options**: Dieses Element wird nur in Kombination mit den Steuerelementen **UocDropDownList** oder **UocRadioButtonList** verwendet. Es funktioniert nicht mit anderen Steuerelementen. Die Struktur dieses Elements finden Sie in diesem Dokument im Abschnitt „Options“. Bei den einzelnen Steuerelementen wird erläutert, wie es im Kontext eines Steuerelements verwendet wird.

4.  **Buttons**: Dieses Element wird nur in Kombination mit dem Steuerelement **UocListView** verwendet. Es funktioniert nicht bei anderen Steuerelementen. Weitere Informationen finden Sie im Abschnitt „UocListView“ dieses Dokuments.

5.  Properties: Dieses Element wird in allen Steuerelementen verwendet, um zusätzliche Verhalten eines Steuerelements anzugeben. Informationen zu diesem Element finden Sie in diesem Dokument im Abschnitt „Properties“.

6.  **Events**: Die Struktur dieses Elements finden Sie weiter oben in diesem Dokument im Abschnitt „Events“. Welches Ereignis in diesem Steuerelement verwendet wird, wird in den Definitionen der einzelnen Steuerelemente beschrieben.

Ein Steuerelement enthält die folgenden Attribute:

1.  **Name**: Dies ist der Name des Steuerelements. Der Name eines Steuerelements muss in jedem Panel eindeutig sein. Dies ist ein erforderliches Attribut vom Typ „String“.

2.  **TypeName**: Dieses Attribut gibt an, um welche Art von Steuerelement es sich handelt. Dies ist ein erforderliches Attribut vom Typ „String“. Die einzelnen Steuerelementnamen finden Sie in diesem Dokument im Abschnitt „Einzelne Steuerelemente“.

3.  **Caption**: Mit diesem Attribut können Sie eine Beschriftung für das Steuerelement einschließen.
    Die Beschriftung ist in der Regel der Anzeigename der Daten, die das Steuerelement anzeigt oder eingibt. Sie können explizit einen Wert für die Beschriftung eingeben oder es an Informationen zu Schemaattribut-Anzeigenamen binden. Die Beschriftung wird links neben einem Steuerelement mit normaler Größe angezeigt. Wenn ein Steuerelement die gesamte Anzeige ausfüllt, wird die Beschriftung auf dem Steuerelement angezeigt. Dies ist ein optionales Attribut vom Typ „String“. Informationen zum Binden einer Datenquelle an ein Attribut oder einen Eigenschaftswert finden Sie im Abschnitt „Properties“.

   Das folgende Beispiel zeigt, wie eine Beschriftung explizit verwendet werden kann:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     Das folgende Beispiel zeigt, wie eine Beschriftung mit einer Datenquelle verwendet werden kann. Wenn Sie die Vorlage für eine weiter oben in diesem Dokument vorgestellte Datenquelle verwendet haben, lautet Ihre Datenquelle „schema“. Es wird empfohlen, das Attribut „DisplayName“ an ein Caption-Attribut zu binden.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled: Dies ist ein optionales Attribut vom Typ „Boolean“. Wenn Sie diesen Attributwert auf „false“ festlegen, kann der Benutzer ein Steuerelement deaktivieren. Standardmäßig ist der Wert auf „true“ festgelegt.

5.  Visible: Dies ist ein optionales Attribut vom Typ „Boolean“. Mithilfe dieses Attributs können Sie das gesamte Steuerelement ausblenden. Standardmäßig ist der Wert auf „true“ festgelegt.

6.  Description: Fügen Sie mit diesem optionale Attribut vom Typ „String“ eine Beschreibung ein, um dem Endbenutzer darüber zu informieren, was in das Steuerelement eingegeben werden soll oder welche Funktionen das Steuerelement hat. Sie können explizit einen Wert für die Beschriftung eingeben oder es an Informationen von Schemaattributbeschreibungen binden. <br/>Die Beschreibung wird unterhalb der Beschriftung links neben einem Steuerelement mit normaler Größe angezeigt. Wenn ein Steuerelement die gesamte Anzeige ausfüllt, wird die Beschreibung auf dem Steuerelement unterhalb der Beschriftung angezeigt. Informationen zum Binden einer Datenquelle an ein Attribut oder einen Eigenschaftswert finden Sie in diesem Dokument im Abschnitt „Properties“.

7.  Das folgende Beispiel zeigt, wie eine Beschreibung explizit verwendet werden kann:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  Das Beispiel zeigt, wie eine Beschreibung mit einer Datenquelle verwendet werden kann. Wenn Sie die Vorlage für eine weiter oben in diesem Dokument vorgestellte Datenquelle verwendet haben, lautet Ihre Datenquelle **schema**. Es wird empfohlen, das Attribut **Description** an ein Description-Attribut zu binden.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: Dieses Attribut gibt an, ob das Steuerelement die gesamte Anzeige ausfüllt. Dies ist ein optionales Attribut vom Typ „Boolean“. Standardmäßig ist der Wert auf „false“ festgelegt.

    >[!NOTE]
    Wenn dieses Attribut auf „true“ festgelegt wird, werden die Attribute „Caption“ und „Description“ deaktiviert. Sie müssen das Steuerelement „UocLabel“ verwenden, um eine Beschriftung für ein erweitertes Steuerelement anzugeben.
9. **Hint**: Dies ist ein optionales Attribut vom Typ „String“. Der Text im Hint-Attribut unterstützt den Endbenutzer bei der Entscheidung, welche Eingabe für das Steuerelement gültig ist. Der Hinweis wird unterhalb des Steuerelements angezeigt.

10.  **AutoPostback**: Dies ist ein optionales Attribut vom Typ „Boolean“. Der Standardwert lautet "False". Wenn dieses Attribut auf „false“ festgelegt wird, wird beim Aktualisieren der Seite möglicherweise nicht das Steuerelement aktualisiert. Informationen zu AutoPostback finden Sie im Abschnitt zur Microsoft ASP.NET UI-Steuerelementeigenschaft mit demselben Namen.

11. **RightsLevel**: Dies ist ein optionales Attribut vom Typ „String“. Sie können dieses Attribut nur mit Inlinerechten an eine Datenquelle binden. Das Steuerelement wird basierend auf den Benutzerrechten dynamisch aktiviert oder deaktiviert. Informationen zum Binden von Datenquellen an ein Attribut oder einen Eigenschaftswert finden Sie in diesem Dokument im Abschnitt „Properties“.

    Dieses Beispiel zeigt, wie ein **RightsLevel**-Attribut mit einer Datenquelle verwendet werden kann. Wenn Sie die Vorlage für eine weiter oben in diesem Dokument vorgestellte Datenquelle verwendet haben, lautet Ihre Datenquelle **rights**. Verwenden Sie den Attributnamen als Pfad.

### <a name="properties"></a>Eigenschaften

Mit einem Property-Element können Sie das Verhalten der einzelnen Steuerelemente weitergehend anpassen. Ein Property-Element ist ein leeres Element. Im Folgenden wird das XSD-Schema für das Property-Element aufgeführt:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Jedes Property-Element weist zwei erforderliche Attribute auf:

1.  **Name**: Dieses Attribut vom Typ „String“ ist der eindeutige Name des Property-Elements.
    Verschiedene Steuerelemente verfügen über unterschiedliche Eigenschaften. Es gibt gemeinsame Eigenschaften, die von allen Steuerelementen verwendet werden können. Weitere Informationen dazu, welche Namen für ein bestimmtes Steuerelement verfügbar sind, finden Sie im Abschnitt „Allgemeine Eigenschaften“ und „Einzelne Steuerelemente“.

2.  **Value**: Dies ist der Wert des Property-Elements. Der Datentyp des Value-Elements hängt von der zugewiesenen Eigenschaft ab. Im folgenden Abschnitt finden Sie Informationen über das zulässige Wertformat für bestimmte Eigenschaften.

An einige Eigenschaften können Informationen aus einer Datenquelle gebunden werden. Für diesen Zweck müssen Sie das folgende Zeichenfolgenformat verwenden. Informationen darüber, wie einzelne Eigenschaften an eine Datenquelle gebunden werden, finden Sie im Abschnitt „Einzelne Eigenschaften“.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**BEISPIEL:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Allgemeine Eigenschaften
-----------------

Alle RCDC-Steuerelemente, die in diesem Dokument angegeben sind, können folgende allgemeine Eigenschaften aufweisen. Sie können diese Eigenschaften zusammen mit anderen Eigenschaften verwenden, die speziell für verschiedene Steuerelemente gelten.

1.  Required: Diese Eigenschaft gibt an, dass es sich bei dem Feld um ein Pflichtfeld oder um ein optionales Feld handelt. Ein Pflichtfeld muss mit einem Wert aufgefüllt werden. Ein leerer Wert wird im Falle einer Zeichenfolgeneingabe nicht unterstützt. Ein optionales Feld kann leer gelassen werden. Wenn dieses Feld ein Pflichtfeld ist, das nicht mit einem Wert aufgefüllt wurde, wird eine Fehlermeldung auf dem Eingabesteuerelement angezeigt. Sie können explizit angeben, ob ein Feld ein Pflichtfeld oder ein optionales Feld sein soll. Sie können das Feld auch an Schemainformationen einer bestimmten Bindung zwischen einem Attribut und einem Ressourcentyp binden. Wenn diese Eigenschaft fehlt, bedeutet dies standardmäßig, dass es sich bei dem Steuerelement um ein optionales Eingabesteuerelement handelt.

   Im Folgenden wird ein Beispiel vorgestellt, in dem ein expliziter Wert für diese Eigenschaft verwendet wird:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Bei diesem Beispiel wird eine dynamische Datenquelle für diese Eigenschaft verwendet. Wenn Sie die Vorlage für eine im vorherigen Abschnitt des Dokuments vorgestellte Datenquelle verwendet haben, lautet Ihre Datenquelle „schema“. Verwenden Sie \<Attributname\>.Required als Pfad.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: Durch Festlegen dieser Eigenschaft auf „true“ wird das Steuerelement für den Endbenutzer im schreibgeschützten Modus ausgeführt. Dies ist ein optionales Attribut vom Typ „Boolean“.
    Standardmäßig ist der Wert auf „false“ festgelegt. In einigen Fällen wird das Verhalten dieser Eigenschaft jedoch durch die Art der Rechte überschrieben, die eine Person für die Datenbindung an das Steuerelement besitzt. Wenn ein Benutzer beispielsweise keine Rechte zum Aktualisieren eines Felds besitzt und das Feld an Inlinerechte gebunden ist, sieht der Benutzer die Daten im schreibgeschützten Modus, selbst wenn diese Eigenschaft auf „false“ festgelegt ist.

3.  **RegularExpression**: Diese Eigenschaft gibt die Beschränkungen an, die für den Wert im Steuerelement gelten. Bei den Formaten dieses Eigenschaftswerts handelt es sich um die im .NET StringRegex-Standard unterstützten Formate. Weitere Informationen finden Sie unter [Reguläre Ausdrücke von .NET Framework](http://go.microsoft.com/fwlink/?LinkId=165361). Wird das Steuerelement zur Eingabe eines Werts verwendet, wird der Wert auf die in der Eigenschaft angegebenen Beschränkung geprüft, wenn der Benutzer versucht, die aktuelle Seite zu verlassen.
    Die Fehlermeldung wird auf dem Steuerelement angezeigt, das eine ungültige Eingabe aufweist. Der Benutzer kann explizit einen regulären Ausdruck angeben. Der Benutzer kann den Wert auch an Schemainformationen eines bestimmten Attributs binden. Wenn diese Eigenschaft fehlt, bedeutet dies standardmäßig, dass das Steuerelement keine Prüfung auf Beschränkungen für Eingabezeichenfolgen durchführt.
    Im Folgenden wird ein Beispiel vorgestellt, in dem ein expliziter Wert für diese Eigenschaft verwendet wird:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Bei diesem Beispiel wird eine dynamische Datenquelle für diese Eigenschaft verwendet. Wenn Sie die Vorlage für eine weiter oben in diesem Dokument vorgestellte Datenquelle verwendet haben, lautet Ihre Datenquelle „schema“. Verwenden Sie <attribute name>.StringRegex als Pfad.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible: Dies ist ein optionales Attribut vom Typ „Boolean“. Mithilfe dieses Attributs können Sie das gesamte Steuerelement ausblenden. Standardmäßig ist der Wert auf „true“ festgelegt.

### <a name="options"></a>Optionen

Das Options-Element enthält einen oder mehrere Option-Unterknoten. Das Options-Element wird nur mit den Steuerelementen „UocDropDownList“ oder „UocRadioButtonList“ verwendet. Weitere Informationen über die Verwendung finden Sie im Abschnitt „Einzelne Steuerelemente“. Im Folgenden finden Sie das XSD-Schema für das Options-Element:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Attribute:

1.  Value: Dies ist ein erforderliches Attribut vom Typ „String“. Das Value-Attribut muss innerhalb desselben Steuerelements eindeutig sein. Es dürfen nur Zeichen von A bis Z ohne Berücksichtigung der Groß-/Kleinschreibung verwendet werden.

2.  Caption: Dieses erforderliche Attribut ist der Anzeigename für die einzelnen Option-Elemente.

3.  Hint: Dies ist ein optionales Attribut. Verwenden Sie dieses Attribut, um weitere Informationen und Hinweise für den Endbenutzer bereitzustellen.

### <a name="environment-variables"></a>Umgebungsvariablen

Die Umgebungsvariablen in der folgenden Tabelle können in jeder RCDC-Konfiguration verwendet werden.

| Variable | description |
|--------|--------|
| `<LoginID>`       | Zeigt die ID des derzeit angemeldeten Benutzers an.           |
| `<LoginDomain>`   | Zeigt die Domäne des derzeit angemeldeten Benutzers an.       |
| `<Today>  `       | Zeigt das aktuelle Datum und die Uhrzeit an.                                |
| `<FromToday_nnn>` | Zeigt das aktuelle Datum, plus nnn und die Uhrzeit an. „nnn“ steht hierbei für eine ganze Zahl.  |
| `<ObjectID> `     | Zeigt die primäre Ressourcen-ID der RCDC an.                                     |
| `<Attribute_xxx>` | Gibt ein angegebenes Attribut, xxx, der primären RCDC-Ressource zurück. |

### <a name="debugging-xml-configuration-files"></a>Debuggen von XML-Konfigurationsdateien


Sie können bei der Entwicklung oder Änderung von XML-Konfigurationsdateien für eine RCDC dazu beitragen, dass weniger Fehler auftreten, indem Sie das XML-Dokument mithilfe eines Editors (z.B. Microsoft Visual Studio®) anhand von XSD-Dateien prüfen. Weitere Informationen finden Sie unter [Einführung in XML-Tools in Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>Anpassen einer Hilfedatei

Wenn Sie neue Ressourcen und Attribute erstellen, sollten Sie die vorhandenen Hilfedateien im FIM-Portal mit dem Inhalt für Ihre benutzerdefinierten Ressourcen aktualisieren. Hilfedateien im FIM-Portal weisen das HTM-Format auf und können manuell bearbeitet werden.

>[!IMPORTANT]
Weitere Informationen zum Erstellen von benutzerdefinierten Attributen finden Sie in der FIM 2010-Dokumentation unter „Einführung in die Verwaltung von benutzerdefinierten Ressourcen und Attributen“.

>[!IMPORTANT]
Dieser Abschnitt enthält keine Informationen zu den Grundlagen der Formatierung oder Bearbeitung von HTML-Dateien. Um Hilfedateien zu ändern, sollten Sie bereits mit der Bearbeitung von HTML-Dateien vertraut sein.


**Speicherort der Hilfedateien**: Alle Hilfedateien für das Microsoft Identity Management 2016 SP1-Portal befinden sich auf dem MIM-Dienstserver im folgenden Ordner:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Vorgehensweise zum Suchen nach der entsprechenden Hilfedatei**: Alle Hilfedateien für das FIM-Portal sind nach einer GUID (Globally Unique Identifier) benannt. So suchen Sie nach der richtigen Datei für Ihre benutzerdefinierte Ressource:

1.  Öffnen Sie im FIM-Portal die Hilfedatei auf der Portalseite, die angepasst werden soll.

2.  Klicken Sie mit der rechten Maustaste auf die Hilfedatei und dann auf **Eigenschaften**.

3.  Markieren Sie im Feld „URL-Adresse“ die Datei „`<GUID\>.htm`“, und kopieren Sie sie.

4.  Navigieren Sie zum Ordner, in dem die Hilfedateien gespeichert sind, und suchen Sie nach der Datei.

**Hinzufügen von Inhalt für ein neues Attribut**: So fügen Sie einen aussagekräftigen Inhalt für ein neues Attribut innerhalb eines vorhandenen Grouping-Elements hinzu (Registerkarte):

1.  Identifizieren Sie die entsprechende Hilfedatei, und suchen Sie danach.

2.  Öffnen Sie die Datei in einem HTML-Editor.

3.  Navigieren Sie zu der Stelle, an die der Inhalt hinzugefügt werden soll. Diese befindet sich normalerweise in einem zusätzlichen Absatz. Beispiel:

`<p xmlns="">A new paragraph with customized information.</p>`

Sie kann sich auch in einem Element befinden, das in eine vorhandene Liste eingefügt wurde. Beispiel:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Hinzufügen von Inhalt für ein neues Grouping-Element**: Die meisten Seiten des FIM-Portals weisen mehrere Grouping-Elemente (oder Registerkarten) auf. Die zugehörigen Hilfedateien enthalten Abschnitte mit Textmarken, die sich auf die einzelnen Grouping-Elemente beziehen. Die Textmarken im HTML-Code sind in den Abschnitten angegeben. Im Folgenden wird z.B. der HTML-Code für die Registerkarte „Arbeitsinformationen“ aus der Hilfedatei für die Seite „Benutzer erstellen“ im FIM-Portal aufgeführt:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Sie verweist auf das Grouping-Element **WorkInfo** in der Konfigurationsdaten-XML-Datei zur **Konfiguration für die Erstellung des Benutzers** in der RCDC. Beachten Sie, dass der Dateiname `\<GUID\>.htm` und die Textmarke im Parameter „my:Link“ angegeben werden:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Einfache Steuerelemente – Beispiele**

Die folgende Abbildung zeigt ein Beispiel für verschiedene einfache Textfeld-Steuerelemente in unterschiedlichen Modi.

Beispiel:

![](media/rcd-configuration-xml-reference/image016.gif)

Mit dem folgenden Codesegment wird das erste Textfeld-Steuerelement erstellt, das expliziten Text für alle Attribute und Eigenschaften verwendet:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


Mit dem folgenden Codesegment wird das zweite Textfeld-Steuerelement erstellt, das dynamische Bindungsmethoden zur Verknüpfung des Steuerelements mit einer anderen Datenquelle verwendet:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


Mit dem folgenden Codesegment wird die dritte erweiterte Bezeichnung sowie das dritte Textfeld-Steuerelement erstellt:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

Mit dem folgenden Codesegment wird das vierte deaktivierte Textfeld-Steuerelement erstellt:
Obwohl bei diesem Steuerelement kein Unterschied im deaktivierten und im aktivierten Status sichtbar ist, kann der Benutzer keine Daten mehr in das Textfeld eingeben.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Einzelne Steuerelemente

### <a name="uocbutton"></a>UocButton

**Name**: UocButton

**Beschreibung**: Dies ist ein einfaches Schaltflächen-Steuerelement, mit dem Sie bestimmte Aktionen auslösen können. Da Sie jedoch keinen eigenen Handler angeben können, ist die Verwendung dieses Steuerelements beschränkt.

**Eigenschaften**:

1.  **All Common Properties**: Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **Text**: Diese Eigenschaft gibt den Text an, der auf der Schaltfläche angezeigt wird. Dies ist ein optionales Attribut vom Typ „String“. Die Text-Eigenschaft erfordert einen expliziten Zeichenfolgenwert.

Event:

   • **OnButtonClicked**: Das Ereignis wird ausgegeben, wenn auf die Schaltfläche geklickt wird.

Beispiel:

![](media/rcd-configuration-xml-reference/image017.png)


Mit dem folgenden XML-Segment wird das Steuerelement „SimpleButton“ erzeugt:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Name**: UocCaptionControl

**Beschreibung**: Dieses Steuerelement wird verwendet, um die Beschriftung einer RCDC-Seite anzuzeigen. Dieses Steuerelement ist nur für die Verwendung als einzelnes Steuerelement in einem Header Grouping-Element vorgesehen.
Wenn Sie es in einem anderen Kontext verwenden, kann dies zu Renderingproblemen oder Fehlern am Portal führen.

**Modus**: Schreibgeschützt (OneWay)

**Eigenschaften**:

1.  **All Common Properties**: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **MaxHeight**: Diese Eigenschaft gibt die maximale Höhe des Symbols im Beschriftungsbereich an. Diese Eigenschaft ist optional. Diese Eigenschaft akzeptiert einen ganzzahligen Wert in Pixel. Standardmäßig ist der Wert auf „32 Pixel“ festgelegt.

Beispiel:

![](media/rcd-configuration-xml-reference/image020.jpg)

Mit dem folgenden Codesegment wird die **Headerbeschriftung** generiert:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Mit diesem Codesegment wird die **explizite Inhaltsbeschriftung** generiert:

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Mit dem folgenden Codesegment wird die dynamische Beschriftung **Anzeigename** generiert:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Name**: UocCheckBox

Beschreibung: Dies ist ein einfaches Kontrollkästchen-Steuerelement. Dem Benutzer wird empfohlen, dieses Steuerelement an Daten vom Typ „Boolean“ zu binden. Dieses Steuerelement kann abhängig von den Daten, an denen es gebunden wird, als schreibgeschütztes oder aktualisierbares Steuerelement verwendet werden.

>[!NOTE]
Beim Anzeigen eines booleschen Attributs mithilfe das Kontrollkästchen-Steuerelements im Bearbeitungsmodus ist in diesem Release Folgendes zu beachten: Wenn dem Attribut noch kein Wert zugewiesen wurde und im Bearbeitungsmodus auf **OK** geklickt wird, fügt die Ressourcensteuerung den Wert **false** zum Attribut hinzu. Um das Problem zu umgehen, erstellen Sie stets ein boolesches Attribut, bei dem davon ausgegangen wird, dass Nichtvorhandensein gleichbedeutend mit **false** ist, oder verwenden Sie andere Steuerelemente wie ein Optionsfeld für boolesche Attribute.

**Eigenschaften**:

1.  **All Common Properties**: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **DefaultValue**: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Standardmäßig ist der Wert auf „false“ festgelegt. Dieses Feld gibt das Standardverhalten eines Kontrollkästchens an.
    Dies kann explizit angegeben werden.

3.  **Checked**: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Standardmäßig ist der Wert auf „false“ festgelegt. Dieser Wert überschreibt die Eigenschaft „DefaultValue“, wenn er zusammen mit „DefaultValue“ verwendet wird. Dieses Feld gibt das Verhalten eines Kontrollkästchens an. Wie bei „DefaultValue“ kann es explizit angegeben oder an Daten vom Server gebunden werden.

4.  **Text**: Dies ist ein optionales Attribut vom Typ „String“. Der Text wird rechts vom Kontrollkästchen angezeigt. Mithilfe dieser Eigenschaft können Sie den Text angeben, der weitere Informationen für den Endbenutzer bereitstellt.

**Ereignisse**:

   • CheckedChanged: Dieses Ereignis wird ausgegeben, wenn das Kontrollkästchen in einen anderen Zustand wechselt.

Im folgenden Beispiel wird eine benutzerdefinierte Bindung zwischen dem benutzerdefinierten Ressourcentyp und dem Attribut **IsConfigurationType** erstellt. Der XML-Code wird in der RCDC eines benutzerdefinierten Ressourcentyps verwendet.

Beispiel:

![](media/rcd-configuration-xml-reference/image022.png)

Im folgenden Codesegment wird ein **dynamisches Kontrollkästchen** erzeugt, wie in der vorherigen Abbildung als dynamisches Kontrollkästchen dargestellt wird. Dieser Bindungstyp ist generell vielseitiger und nützlicher als ein explizites Kontrollkästchen. Das Attribut muss dem aktuellen Ressourcentyp zugeordnet sein.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Beschreibung**: Dies ist ein mehrzeiliges Textfeld-Steuerelement, das eine spezielle Zeichenfolgenformatierung unterstützt. Die Werte in den mehrwertigen Einträgen werden durch ein Semikolon oder einen Zeilenumbruch im Textfeld voneinander getrennt. Es wird empfohlen, dieses Steuerelement an mehrwertige Daten vom Typ „Short“, „String“ oder „Integer“ zu binden. Dieses Steuerelement unterstützt sowohl den schreibgeschützten Modus als auch den aktualisierbaren Modus.

**Eigenschaften**:

1.  **All Common Properties**: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **DataType**: Dies ist ein erforderliches Attribut vom Typ „String“. Sie können dieses explizit als Typ **String, Integer** oder **DateTime** angeben. Sie können auch ein Attribut an die Eigenschaft **DataType** des Schemaattributs binden. Ein mehrwertiger Verweistyp sollte von **UOCListView** oder **UOCIdentityPicker** verarbeitet werden. Ein mehrwertiger boolescher Wert wird nicht als Datentyp unterstützt.

3.  **Rows**: Dies ist ein optionales Attribut vom Typ „Integer“. Sie können die Höhe des Felds durch die Anzahl von Zeichen definieren. Standardmäßig ist der Wert auf „1“ festgelegt.

4.  **Columns**: Dies ist ein optionales Attribut vom Typ „Integer“. Sie können die Breite des Felds durch die Anzahl von Zeichen definieren. Der Standardwert lautet
    20.

5.  **Value**: Dies ist ein optionales Attribut vom Typ „String“. Sie können dieses Attribut nur an eine Datenquelle binden.

Ereignisse:

   • **ValueListChanged**: Dieses Ereignis wird ausgelöst, wenn sich der aktuelle Wert im Steuerelement ändert.

Im folgenden Beispiel wird ein mehrwertiges Zeichenfolgenattribut mit dem Namen **AMultiValueString** erstellt und an den benutzerdefinierten Ressourcentyp gebunden. Dieses Beispiel funktioniert nur, nachdem diese Bindung erstellt wurde.

**Beispiel:**

![](media/rcd-configuration-xml-reference/image024.jpg)

Mit dem folgenden Codesegment wird die voranstehende Abbildung generiert:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Name**: UocDateTimeControl

**Beschreibung**: Dieses Steuerelement ist mit einem Textfeld-Steuerelement vergleichbar, das Attribut **Description** akzeptiert jedoch nur ein bestimmtes Format. Im schreibgeschützten Modus wird es wie eine Bezeichnung angezeigt. Das Format der unterstützten Eingabezeichenfolge finden Sie in diesem Abschnitt unter der Eigenschaft **DateTimeFormat**.

**Eigenschaften**:

1.  **All Common Properties**: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **DateTimeFormat**: Dies ist ein optionales Attribut vom Typ „String“. Die unterstützten Formate sind „DateTime“ oder „DateOnly“. Standardmäßig ist der Wert auf das Format „DateTime“ festgelegt.
    a. DateTime-Format: Dieses Attribut ist als „mm/dd/yyyy hh:mm:ss AM“ formatiert.

      <[!NOTE]
      Sowohl das Format **DateTime** als auch **DateOnly** werden unabhängig vom Benutzer, der den Unterschied angibt, unterstützt.
3.  **Value**: Dies ist ein optionales Attribut vom Typ „String“. Dieses Attribut binden Sie an eine Ressourcendatenquelle. Der Wert dieses Attributs muss dem richtigen DateTime-Format entsprechen.

Ereignisse:

   • **DateTimeChanged**: Das Ereignis tritt auf, wenn sich der DateTime-Wert ändert.

Beispiel:

![](media/rcd-configuration-xml-reference/image027.jpg)

Das folgende Codesegment erzeugt das erste **DateTime**-Steuerelement.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


Das folgende Codesegment erzeugt das zweite **DateTime**-Steuerelement. Wenn Sie den Beispielcode im Abschnitt „Datenquellen“ verwendet haben, wird das **ExpirationTime**-Attribut an alle Ressourcentypen gebunden. Aus diesem Grund können Sie ihn mit folgendem Code verwenden:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Name**: UocDropDownList

Beschreibung: Dies ist ein einfaches Dropdownfeld-Steuerelement. Dieses Steuerelement wird in der Regel verwendet, wenn Sie Optionen aus einem definierten Satz von Optionen auswählen möchten. Geeignete Möglichkeiten für dieses Steuerelement sind die Datentypen „String“, „Integer“, „DateTime“ und „Boolean“.

**Eigenschaften**:

1.  **All Common Properties**: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **ValuePath**: Dies ist die Eigenschaft, um das Value-Attribut von „ItemSource“ abzurufen. Wenn „ItemSource“ als benutzerdefiniert angegeben wird, wird der Wertpfad auf „Value“ festgelegt. Es wird an das Feld „Value“ des Option-Elements gebunden, das weiter unten in diesem Dokument definiert ist.

3.  **CaptionPath**: Dies ist die Eigenschaft, um das Value-Attribut von „ItemSource“ abzurufen. Wenn „ItemSource“ als „Benutzerdefiniert“ angegeben wird, wird der Wertpfad auf „Caption“ festgelegt. Es wird an das Feld „Caption“ des Option-Elements gebunden, das weiter unten in diesem Dokument definiert ist.

4.  **HintPath**: Dies ist die Eigenschaft, um das Value-Attribut von „ItemSource“ abzurufen. Wenn „ItemSource“ als „Benutzerdefiniert“ angegeben wird, wird der Wertpfad auf „Hint“ festgelegt. Es wird an das Feld „Hint“ des Option-Elements gebunden, das weiter unten in diesem Dokument definiert ist.

5.  **ItemSource**: Dies ist eine Sammlung von ListControlItems, die die Optionen in der Liste definiert. Der Benutzer kann dies explizit auf „Benutzerdefiniert“ festlegen und das Option-Element verwenden, um den Zeichenfolgenwert anzugeben.

6.  **SelectedValue**: Dies ist der zurzeit ausgewählte Wert. Dies ist eine erforderliche Eigenschaft vom Typ „String“. Diese Eigenschaft wird an Zeichenfolgendaten aus der Datenquelle gebunden.

Ereignisse:

  • SelectedIndexChanged: Das Ereignis tritt auf, wenn sich die Auswahl im Dropdownfeld ändert.

Optionen:

Die Struktur eines Option-Elements finden Sie in diesem Dokument im Abschnitt „Option“.

1.  **Value**: Das Attribut „Value“ eines einzelnen Option-Elements kann auf eine beliebige Zeichenfolge festgelegt werden, die die gültige Eingabe der Datenquelle, an die das Steuerelement gebunden wird, darstellt.

2.  **Caption**: Das Attribut „Caption“ kann einen beliebigen Zeichenfolgenwert enthalten.

3.  **Hint**: Das Attribut „Hint“ kann einen beliebigen Zeichenfolgenwert enthalten.

Beispiel:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Damit das Beispiel funktioniert, müssen Sie ein vorhandenes **Scope**-Attribut vom Typ „String“ an den benutzerdefinierten Ressourcentyp binden, der für die RCDC gilt.


Mit diesem Codesegment wird die Dropdownliste generiert:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Name: UocFileDownload

Beschreibung: Dieses Steuerelement enthält einen Link. Wenn auf den Link geklickt wird, wird die Windows-Seite „Datei speichern“ angezeigt. Der Benutzer kann die Datei auf dem lokalen Laufwerk speichern.
Die Option „Öffnen“ wird ebenfalls unterstützt, wenn der Internet Explorer das Dateiformat rendern kann. Die empfohlenen Datentypen, die mit diesem Steuerelement zu verwenden sind, sind formatierte Zeichenfolgen (XML) und Binärtypen.

>[!NOTE]
In diesem Release von Microsoft Identity Manager 2016 SP1 muss der Benutzer das Internet Explorer-Fenster schließen, in dem die Datei geöffnet ist, und dann die Seite aktualisieren. Nach der Aktualisierung des Internet Explorer-Fensters kann der Benutzer anschließend den Download starten, um diese Datei wieder im ursprünglichen Fenster zu speichern oder zu öffnen.


Eigenschaften:

1.  **All Common Properties**: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  **Text**: Dies ist ein optionales Attribut vom Typ „String“, das den Linktext definiert. Der Benutzer kann eine explizite Zeichenfolge für diese Eigenschaft angeben.

3.  **Value**: Dies ist ein erforderliches Attribut. Es gibt die Attributbindung auf dem Server an, dessen Inhalt heruntergeladen werden soll.

4.  **PromptedFileName**: Dies ist ein optionales Attribut vom Typ „String“. Dies ist der Dateiname, der dem Benutzer vorgeschlagen wird, wenn die heruntergeladene Datei gespeichert wird.

5.  **ContentType**: Dies ist ein erforderliches Attribut vom Typ „String“. Dies ist der Dateityp, in dem die Daten gespeichert werden. Die beiden unterstützten Zeichenfolgenoptionen sind Text- oder Binärdaten. Wenn es sich um Text handelt, wird der Rückgabewert als lange Zeichenfolge behandelt.
    Andernfalls wird der Rückgabewert bei Binärdateien als „byte[]“ behandelt. Wenn Text markiert ist, kann der Benutzer als Option ein Suffix hinzufügen, um den Formattyp des Texts anzugeben. Ein gültiger Wert ist z.B. „text/xml“.


>[!NOTE]
Wenn der an dieses Steuerelement gebundene Wert leer ist, fehlt der Link im Steuerelement, der zum Auslösen der Downloadaktion verwendet wird. Dies ist darauf zurückzuführen, dass keine Downloadinhalte zur Verfügung stehen.


**Ereignisse**:

Es sind keine Ereignisse für dieses Steuerelement vorhanden.

**Beispiel**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Bevor diese Beispieldatei hochgeladen wird, muss der Benutzer eine Bindung zwischen einem benutzerdefinierten Ressourcentyp und dem vorhandene ConfigurationData-Attribut erstellen.


Mit dem folgenden Codesegment wird das Dateidownload-Steuerelement in der voranstehenden Abbildung generiert:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Name**: UocFileUpload

**Beschreibung**: Dieses Steuerelement enthält ein Textfeld, das den Speicherort der lokalen hochzuladenden Datei, eine Schaltfläche zum Durchsuchen von Dateien und eine Schaltfläche zum Hochladen anzeigt. Klickt der Endbenutzer auf eine Schaltfläche zum Durchsuchen, wird das Windows-Fenster „Datei öffnen“ angezeigt. Der Endbenutzer kann eine hochzuladende Datei auf seinem lokalen Laufwerk auswählen. Wenn die Datei ausgewählt ist, wird der Speicherort der Datei im Textfeld angezeigt. Wenn auf die Schaltfläche „Hochladen“ geklickt wird, wird die Datei in die clientseitige lokale Datenquelle hochgeladen. Der Dateiinhalt wird noch nicht an den Server gesendet. Für die Verwendung mit diesem Steuerelement werden folgende Datentypen empfohlen: formatierte Zeichenfolgen (XML) oder Binärtypen.

>[!NOTE]
Der Uploadfortschritt bzw. -status wird nicht angezeigt. Wenn die Datei in die lokale Datenquelle hochgeladen wird, wird das Textfeld gelöscht.


Eigenschaften:

1.  All Common Properties: Informationen hierzu finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  Value: Dies ist ein erforderliches Attribut. Es gibt die Schemaattributbindung auf dem Server an, auf dem die Daten hochgeladen werden.

3.  ContentType: Dies ist ein optionales Attribut vom Typ „String“. Dies ist der Datentyp, in dem die Datei auf dem Server gespeichert wird. Dieser kann auf „Text“ oder „Binär“ festgelegt werden. Wenn die Eigenschaft fehlt, lautet der Standardwert „Binär“.

4.  MaxFileSize: Dies ist ein optionales Attribut vom Typ „String“. „MaxFileSize“ definiert die mögliche Größe der hochzuladenden Datei. Wenn die Eigenschaft fehlt, beträgt die maximale Größe standardmäßig 1 Megabyte (MB).

5.  PromptedForNoValue: Dies ist ein optionales Attribut vom Typ „String“. Es definiert den Text, der dem Benutzer angezeigt wird, wenn eine Datei nicht hochgeladen wurde.

Ereignisse:

   • FileUploaded: Dieses Ereignis wird ausgegeben, wenn die Datei erfolgreich hochgeladen wurde.

Beispiel:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Damit der folgende Beispielcode funktioniert, müssen Sie ein neues Attribut vom Binärtyp mit dem Namen „ABinaryAttribute“ erstellen und dann eine neue Bindung zwischen einem benutzerdefinierten Ressourcentyp und diesem Attribut erstellen.


Mit dem folgenden Codesegment wird das Uploadsteuerelement in der voranstehenden Abbildung generiert:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Name: UocFilterBuilder

Beschreibung: Dies ist ein komplexes Steuerelement, mit dem der Benutzer einen MIM 2016 XPath-Ausdruck rendern kann. Einige XPath-Ausdrücke werden nicht unterstützt. Informationen zum Verwenden des Filtergenerators finden Sie in der Hilfe für den Filtergenerator.

Eigenschaften:

1.  All Common Properties: Weitere Informationen finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  PermittedObjectTypes: Dieses Attribut definiert eine Liste von Ressourcentypen, die in der select-Anweisung eines Filtergenerators angezeigt werden sollen. Informationen zum Verwenden des Filtergenerators finden Sie in der Hilfe des Filtergenerators. Die Zeichenfolge befindet sich im Format „ResourceTypeA, ResourceTypeB“, wobei die einzelnen Ressourcentypen durch ein Komma (,) getrennt werden.

3.  Value: Dies ist der Wert, mit dem der Filtergenerator gerendert wird.
    Es werden nur Bindungen an Daten vom Typ „String“ mit einem XPath-Ausdruck unterstützt. Das Filterattribut ist ein empfohlenes Attribut zum Binden dieses Steuerelements.

4.  PreviewButtonVisible: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Wenn diese Eigenschaft auf „false“ festgelegt ist, wird dem Benutzer nicht die Schaltfläche „Vorschau“ angezeigt. Standardmäßig ist der Wert auf „true“ festgelegt. Diese Schaltfläche kann in Kombination mit einem Listenansicht-Steuerelement verwendet werden, um eine Vorschau für die Ergebnisse eines XPath-Ausdrucks anzuzeigen.

5.  ExcludeGroupMembership: Hierbei handelt es sich um eine boolesche Eigenschaft. Wenn diese Eigenschaft auf „true“ festgelegt ist, können Sie keinen Filter mit einem \<Verweisattribut\> (z.B. ResourceID) erstellen, das Mitglied eines \<Group-Objekts\> ist. Das heißt, wenn diese Eigenschaft auf „true“ festgelegt ist, können Sie keinen Filter erstellen, der das Gruppenmitgliedschaftsverzeichnis verwendet.

6.  PreviewButtonCaption: Dies ist eine optionale Zeichenfolge. Wenn „PreviewButtonVisible“ auf „true“ festgelegt ist, können Sie der Schaltfläche mithilfe dieser Eigenschaft einen angepassten Text zuweisen. Der Text wird auf der Schaltfläche „Vorschau“ angezeigt.

Ereignisse:

   • OnFilterChanged: Dieses Ereignis wird ausgelöst, wenn sich der Inhalt des Filtergenerators ändert.

Beispiel:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Erstellen Sie vor der Verwendung dieses Beispielcodes eine neue Bindung zwischen einem vorhandenen Filterattribut und einem benutzerdefinierten Ressourcentyp.


Im Folgenden wird der Beispielcode vorgestellt, der ein UOCLabel-Steuerelement, einen einfachen Filtergenerator mit „PermittedObjectTypes“ und eine Vorschaulistenansicht enthält. Um die ListFilter-Eigenschaft der Listenansicht und die Value-Eigenschaft des Filtergenerators zu verknüpfen, müssen Sie diese auf dasselbe Datenquellattribut festlegen.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Name: UocHtmlSummary

Beschreibung: Mithilfe dieses Steuerelements können Sie eine Zusammenfassungsseite auf einer RCDC-Seite definieren.
Diese Zusammenfassungsseite wird angezeigt, nachdem der Endbenutzer eine Anforderung übermittelt hat. Dieses Steuerelement darf nur in einem Summary Grouping-Element verwendet werden und muss das einzige Steuerelement sein. Es wird dringend empfohlen, den bereitgestellten Beispielcode zu verwenden.

>[!NOTE]
Dieses Steuerelement wurde nicht umfassend getestet.


Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  ModificationsXml: Diese Eigenschaft muss als {Binding Source=delta, Path=DeltaXml} formatiert sein, wobei „delta“ im Konfigurationsheader „ObjectDataSource“ definiert ist.

3.  TransformXsl: Diese Eigenschaft ist in der Regel als {Binding Source=summaryTransformXsl, Path=/} formatiert, wobei „summaryTransformXsl“ im Konfigurationsheader „XmlDataSource“ definiert ist.

Ein Beispiel für dieses Steuerelement finden Sie weiter oben in diesem Dokument im Abschnitt „Gruppierung“ unter „Zusammenfassungsgruppierung“.

### <a name="uochyperlink"></a>UocHyperLink

Name: UocHyperLink

Beschreibung: Dies ist ein einfaches Linksteuerelement. Mit diesem Steuerelement können Informationen als Link angezeigt werden.

Eigenschaften:

1.  All Common Properties: Informationen hierzu finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  ObjectReference: Dies ist eine optionale Eigenschaft vom Verweistyp. Wenn eine gültige Ressource durch die GUID verwiesen wird, die in dieser Eigenschaft definiert ist, bietet der Link dem Endbenutzer eine Möglichkeit für den Zugriff auf die Ressource. Diese Eigenschaft und die Eigenschaft „NavigateUrl“ (siehe unten) schließen sich gegenseitig aus.

3.  Text: Dies ist eine optionale Eigenschaft vom Typ „String“. Mithilfe dieser Eigenschaft können Sie den als Link angezeigten Text definieren.

4.  NavigateUrl: Dies ist eine optionale Eigenschaft vom Typ „String“. Mithilfe dieser Eigenschaft können Sie den vollständigen Pfad der URL definieren, mit dem der Link verknüpft wird. Diese Eigenschaft und die Eigenschaft „ObjectReference“ (siehe unten) schließen sich gegenseitig aus.

Beispiel:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
Sie benötigen eine gültige GUID einer Ressource, um diese verknüpfen zu können. In diesem Fall wird der zweite Link mit einer gültigen GUID generiert. Der erste Link kann eine beliebige Website sein.


Mit dem folgenden Codesegment wird ein Umleitungslink generiert:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

Mit dem folgenden Codesegment wird ein Link generiert, der auf eine Ressource verweist. Der explizite Verweis kann durch den Ausdruck {Binding Source=object, Path=Creator} ersetzt werden, um ihn an eine Datenquelle zu binden. Dieser ist nur gültig, wenn der Ressourcen-Manager vorhanden ist und ein Wert vom Verweistyp enthalten ist.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Name: UocIdentityPicker

Beschreibung: Dieses Steuerelement besteht aus einem optionalen Feld zum Auflösen und einem Suchfenster. Das optionale Feld zum Auflösen besteht aus einem optionalen Textfeld zum Eingeben der Identität, der Schaltfläche „Auflösen“ zum Auflösen der Identität und der Schaltfläche „Durchsuchen“ zum Aufrufen eines Popupsuchfensters. Mithilfe des Suchfensters kann der Benutzer Identitäten durch ein Listenansicht-Steuerelement auswählen. Die über das Suchfenster ausgewählte Identität wird im Feld „Auflösen“ angezeigt.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  UsageKeywords: Dies ist eine optionale Zeichenfolgeneigenschaft. Sie können eine Liste von Suchbereichen definieren, die in der Ressourcenauswahl verwendet werden sollen, indem Sie eine Liste der von der Struktur „SearchScopeConfiguration“ unterstützten Verwendungsschlüsselwörter angeben. Jedes Schlüsselwort wird dabei durch ein (') getrennt.

3.  Filter: Dies ist eine optionale Zeichenfolgeneigenschaft. Der Benutzer gibt einen XPath-Ausdruck zum Festlegen des Bereichs für die Ressourcenauswahl an, um nur die Elemente anzuzeigen, die in einem definierten Bereich liegen. Diese Eigenschaft und die Eigenschaft „UsageKeywords“ (siehe oben) schließen sich gegenseitig aus. Die Anwendung des Suchbereichs hat keine Auswirkungen auf diese Eigenschaft.

4.  ResultObjectType: Dies ist eine optionale Zeichenfolgeneigenschaft. Der Ressourcentyp wird zum Rendern von Ressourcen in der Liste der Popupdialogfelder verwendet. Dieser wird mit dem Filter verwendet, um die Identifizierung des vom Filter zurückgegebenen Ressourcentyps bei der Identitätsauswahl zu ermöglichen und die Daten entsprechend zu rendern. Diese Eigenschaft und die Eigenschaft „UsageKeywords“ (siehe oben) schließen sich gegenseitig aus. Die Anwendung des Suchbereichs hat keinerlei Auswirkungen. Die für diese Eigenschaft akzeptierte Zeichenfolge ist ein beliebiger einzelner, gültiger Name eines Ressourcentyps, z.B. „Person“.
    Wenn der Filter mehrere Ressourcentypen zurückgeben muss, wird die Ressource verwendet.

5.  PreviewTitle: Hierbei handelt es sich um den in einer Listenansicht verwendeten Vorschautitel. Informationen zu dieser Eigenschaft finden Sie im Abschnitt „UocListView“.

6.  ListViewTitle: Dies ist eine optionale Zeichenfolgeneigenschaft. Mithilfe dieser Eigenschaft können Sie den Text definieren, der oben in der Listenansicht als Titel angezeigt wird.

7.  Value: Dies ist eine optionale Zeichenfolgeneigenschaft. Es wird empfohlen, diese Eigenschaft an ein Schemaattribut zu binden, um den Wert mit einer Datenquelle zu verbinden.

8.  Mode: Dies ist eine optionale Zeichenfolgeneigenschaft. Mithilfe dieser Eigenschaft können Sie festlegen, ob ein Wert von der Identitätsauswahl ausgewählt werden kann oder mehrere Identitäten ausgewählt werden können. Die zulässigen Werte sind „SingleResult“ und „MultipleResult“. Standardmäßig wird sie auf „SingleResult“ festgelegt.

9.  ObjectTypes: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie können eine Liste von Ressourcentypen definieren, mit denen der Endbenutzer Einträge anhand des Felds „Auflösen“ in der Identitätsauswahl auflösen kann. Die Liste enthält eine Liste von Ressourcentypnamen, die durch ein Komma (,) getrennt sind.

10. AttributesToSearch: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie können eine Liste von Attributen definieren, die zum Auflösen des Elements in der Identitätsauswahl verwendet werden sollen, wobei es sich um eine Liste von durch ein Komma (,) getrennten Schemaattributen handelt. Wenn „AttributesToSearch“ beispielsweise auf den Alias „DisplayName“ festgelegt ist, bedeutet dies, dass der Benutzer mit „DisplayName = \<Suchwert\>“ oder „Alias=\<Suchwert\>“ nach den Elementen suchen kann. Es wird empfohlen, für die hier eingegebenen Attributnamen gültige Attribute für Zielressourcentypen der Datenquelle zu verwenden, die im Attribut „Value“ positioniert ist. Die Zielressourcentypen befinden sich im Feld „ObjectTypes“. Alle Attribute für bestimmte Ressourcentypen, die im Feld „ObjectTypes“ angeführt werden, müssen gültig sein.

11. ColumnsToDisplay: Dies ist eine optionale Eigenschaft vom Typ „String“. Der Benutzer gibt eine Liste von Schemaattributnamen an, die durch ein Komma (,) getrennt. Die hier definierten Attribute bilden zusammen die Spalte der Listenansicht in der Identitätsauswahl.

12. Rows: Dies ist eine optionale ganzzahlige Eigenschaft. Sie funktioniert nur, wenn der Modus auf „MultipleResult“ festgelegt ist. Verwenden Sie diese Eigenschaft, um die Höhe des Textfelds „Auflösen“ auf eine bestimmte Größe in Zeicheneinheiten festzulegen.

13. MainSearchScreenText: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie gibt den angepassten Text an, der während des Suchvorgangs im Suchfenster angezeigt wird.

Ereignisse:

 • SelectedObjectChanged: Dieses Ereignis wird ausgegeben, wenn der Benutzer die ausgewählten Ressourcen ändert.

Beispiel:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Für die ordnungsgemäße Ausführung dieses Beispiels müssen Sie eine neue Bindung zwischen dem Manager-Attribut und einem beliebigen benutzerdefinierten Ressourcentyp erstellen, dem diese XML entspricht.


Mit dem folgenden Codesegment wird im Rahmen der RCDC mithilfe der Filter- und ResultObjectType-Eigenschaften eine Identitätsauswahl im Einzelmodus generiert:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Beispiel:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Damit dieser Beispielcode funktioniert, müssen Sie das ExplicitMember-Attribut (ein mehrwertiges Verweisattribut) an den benutzerdefinierten Ressourcentyp binden. Zudem müssen Sie Suchbereiche erstellen, bei denen die Eigenschaft „UsageKeyword“ auf „Person“ und „Group“ festgelegt ist.


Mit dem folgenden Codesegment wird das Steuerelement in der voranstehenden Abbildung erstellt:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Name: UocLabel

Beschreibung: Dies ist ein einfaches, schreibgeschütztes Beschriftung-Steuerelement. Es wird empfohlen, dieses Steuerelement zum Anzeigen von schreibgeschützten Daten zu verwenden.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  Text: Dies ist ein Attribut vom Typ „String“. Diese Eigenschaft wird durch Angabe eines expliziten Zeichenfolgenwerts oder durch deren Bindung an eine Datenquelle definiert. Eine Bindung, die den Wert dieser Eigenschaft zuweist, lautet beispielsweise {Binding Source=object, Path=\<gültiger Attributname\>.

Ein Beispiel für das Steuerelement „UocLabel“ finden Sie im Abschnitt „Einfache Steuerelemente – Beispiele“.

### <a name="uoclistview"></a>UocListView

Name: UocListView

Beschreibung: Dies ist ein erweitertes Listenansicht-Steuerelement. Es besteht aus einer einfachen Listenansicht, einem optionalen Steuerelement für die einfache Suche, einem optionalen Steuerelement für die erweiterte Suche, einem optionalen Auswahlvorschaufeld und einer Aktionsschaltflächenleiste. Das optionale Steuerelement für die einfache Suche besteht aus einem Suchbereich und einem Textfeld für die einfache Suche. Bei dem Steuerelement für die erweiterte Suche handelt es sich um einen Filtergenerator. Die Listenansicht zeigt eine vorab gerenderte Liste mit Ressourcen an. Zudem können in diesem Steuerelement Suchergebnisse, die über die Steuerelemente für die Suche ermittelt wurden, anzeigt werden. Die Aktionsschaltflächenleiste definiert, welche Aktion basierend auf der Auswahl in der Listenansicht ausgeführt werden kann. Das Auswahlvorschaufeld zeigt an, welche Elemente von der Listenansicht ausgewählt sind.

>[!IMPORTANT]
Das Steuerelement „UocListView“ funktioniert nicht bei einwertigen Verweisattributen. Es kann nur mit mehrwertigen Verweisattributen verwendet werden. Einwertige Verweisattribute finden Sie in diesem Dokument unter „UocIdentityPicker“.


Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  SelectedValue: Dies ist eine optionale Eigenschaft vom Typ „String“. Diese wird in der Regel an ein mehrwertiges Verweisattribut gebunden, das eine Liste von GUID-formatierten Zeichenfolgen akzeptiert.

3.  PageSize: Dies ist eine optionale ganzzahlige Eigenschaft. Der Benutzer kann in einem Listenansicht-Steuerelement angeben, wie viele Einträge auf einer Seite passen. Standardmäßig ist der Wert auf „10 Einträge“ festgelegt. Gültig ist eine beliebige positive ganze Zahl.

4.  UsageKeyword: Dies ist eine optionale Eigenschaft vom Typ „String“. Der Benutzer kann eine Liste mit Schlüsselwörtern angeben, die definieren, welcher Suchbereich im Listenansicht-Steuerelement für die Suche verwendet wird. Es gibt Suchbereichsressourcen auf dem FIM 2010-Server. Das Attribut in einer SearchScopeConfiguration-Struktur, auch als „UsageKeyword“ bezeichnet, wird verwendet, um einen Satz von Suchbereichen zu gruppieren. Diese Liste mit Schlüsselwörtern wird von der Listenansicht genutzt. Jedes Schlüsselwort wird durch ein Komma (,) getrennt.
    Dies ist die für den entsprechenden Suchbereich verwendete „UsageKeyword“-Eigenschaft, die in dieser Listenansicht angezeigt werden soll. Diese ist nur wirksam, wenn die ShowSearchControl-Eigenschaft auf „true“ festgelegt ist.

5.  SearchControlAutoPostback: Hierbei handelt es sich um eine optionale boolesche Eigenschaft. Legen Sie den Wert dieser Eigenschaft auf „true“ fest, um das AutoPostBack auszuführen, wenn eine Suche ausgelöst wird. Standardmäßig ist „SearchControlAutoPostback“ auf „false“ festgelegt.

6.  EmptyResultText: Dies ist eine optionale Eigenschaft vom Typ „String“. Diese ist standardmäßig auf „Keine Elemente“ festgelegt, kann jedoch auf einen beliebigen Zeichenfolgenwert festgelegt werden. Dieser Text wird angezeigt, wenn ein Suchergebnis leer ist.

7.  ButtonHeight: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Legen Sie den Wert dieser Eigenschaft auf einen beliebigen positiven, ganzzahligen Wert fest. Diese Eigenschaft definiert die Höhe der Schaltflächen in der Aktionsleiste in Pixel. Standardmäßig ist der Wert auf „32 Pixel“ festgelegt.

8.  ButtonWidth: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Legen Sie den Wert dieser Eigenschaft auf einen beliebigen positiven, ganzzahligen Wert fest. Diese Eigenschaft definiert die Breite der Schaltflächen in der Aktionsleiste in Pixel. Standardmäßig ist der Wert auf „32 Pixel“ festgelegt.

9.  CaptionImageMaxHeight: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Legen Sie den Wert dieser Eigenschaft auf eine beliebige positive ganze Zahl fest. Diese Eigenschaft definiert die maximale Symbolhöhe einer optionalen Beschriftung. Standardmäßig ist der Wert auf „32 Pixel“ festgelegt.

10. CaptionImageMaxWidth: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Legen Sie den Wert dieser Eigenschaft auf eine beliebige positive ganze Zahl fest. Diese Eigenschaft definiert die maximale Symbolbreite einer optionalen Beschriftung. Standardmäßig ist der Wert auf „32 Pixel“ festgelegt.

11. CaptionImageUrl: Dies ist eine optionale Eigenschaft vom Typ „String“. Diese Eigenschaft definiert eine URL, die mit einem als Beschriftungsbild angezeigtes Bild verknüpft ist.

12. PreviewTitle: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie können mithilfe dieser Eigenschaft den Text definieren, der auf dem Auswahlvorschaufeld angezeigt wird.

13. EnableSelection: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Sie können mithilfe dieser Eigenschaft definieren, ob sich eine Listenansicht im Auswahlmodus befindet. Wenn sich eine Listenansicht im Auswahlmodus befindet, wird eine Spalte mit Kontrollkästchen in der Spalte ganz links in der Listenansicht sowie ein Vorschauauswahlfeld im unteren Bereich der Listenansicht angezeigt. Standardmäßig ist der Wert dieser Eigenschaft auf „true“ festgelegt.

14. SingleSelection: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Wenn der Auswahlmodus für die Listenansicht aktiviert ist, wird die Auswahl des Endbenutzers durch Festlegen dieses Werts auf „true“ auf lediglich ein Element aus der Liste beschränkt. Standardmäßig ist der Wert dieser Eigenschaft auf „false“ festgelegt. Dies bedeutet, dass der Endbenutzer standardmäßig mehrere Elemente aus der Liste auswählen kann.

15. RedirectUrl: Dies ist eine optionale Eigenschaft vom Typ „String“. Geben Sie anhand dieser Eigenschaft eine Seite an, an die die Umleitung erfolgen soll, wenn auf ein mit einem Link versehenes Element in der Liste geklickt wird. Diese URL kann Platzhalter enthalten, die während der Laufzeit durch den tatsächlichen Wert ersetzt werden. Die Platzhalter lauten wie folgt:

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Verwenden Sie diese Eigenschaft, um anzugeben, ob die Titelleiste sichtbar sein soll. Standardmäßig ist der Wert dieser Eigenschaft auf „false“ festgelegt.

17.  ShowActionBar: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Verwenden Sie diese Eigenschaft, um anzugeben, ob der Aktionsleistenbereich sichtbar sein soll. Standardmäßig ist der Wert dieser Eigenschaft auf „true“ festgelegt.

18.  ShowPreview: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Verwenden Sie diese Eigenschaft, um anzugeben, ob der Vorschaubereich sichtbar sein soll. Standardmäßig ist der Wert dieser Eigenschaft auf „true“ festgelegt.

19.  ShowSearchControl: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Verwenden Sie diese Eigenschaft, um anzugeben, ob das Steuerelement für die Suche sichtbar sein soll. Standardmäßig ist der Wert dieser Eigenschaft auf „true“ festgelegt.

20.  ResultObjectType: Dies ist eine optionale Eigenschaft vom Typ „String“. Verwenden Sie diese Eigenschaft, um den erwarteten Objekttyp der Suchergebnisse anzugeben. Standardmäßig ist der Wert dieser Eigenschaft auf „Resource“ festgelegt. Wenn das Suchergebnis mehrere Ressourcentypen enthält, sollte dieser Wert auf „Ressource“ festgelegt werden.

21.  ColumnsToDisplay: Dies ist eine optionale Eigenschaft. Verwenden Sie diese Eigenschaft, um anzugeben, welche Attribute in der Listenansicht als Spalten angezeigt werden sollen. Standardmäßig ist der Wert dieser Eigenschaft auf „DisplayName“ oder „ResourceType“ festgelegt. Jede Spalte wird durch den Systemnamen eines Attributs dargestellt. Jede Spalte wird durch ein Komma (,) getrennt. Sie müssen keinen Wert für diese Eigenschaft angeben, wenn die Listenansicht im Auswahlmodus verwendet wird. Im Auswahlmodus wird die Spalteneinstellung von dem Attribut „SearchScopeColumn“ des derzeit ausgewählten Suchbereichs ausgewählt.

22.  ListFilter: Dies ist eine optionale Eigenschaft vom Typ „String“. Dies ist der XPath-Ausdruck, der zum Rendern der Listenansicht verwendet wird und nur wirksam ist, wenn die Eigenschaft „ShowSearchControl“ auf „false“ festgelegt ist. Wenn dieser Wert angegeben ist, verwendet die Listenansicht diesen Eigenschaftswert für Abfragen, und die Listenansicht befindet sich nicht im Auswahlmodus. Der Filter kann folgendermaßen an ein Zeichenfolgenattribut der Ressource gebunden werden:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     Gleichermaßen kann der Filter auch an eine Zeichenfolge gebunden werden, die vordefinierte Umgebungsvariablen enthält: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: Dies ist eine veraltete Eigenschaft. Als Wert sollte der Systemname eines mehrwertigen Attributs, auf das verwiesen wird, festgelegt werden. Von der weiteren Verwendung dieser Eigenschaft wird abgeraten. Beispielsweise sollte bei der Gruppenverwaltung Folgendes vermieden werden:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Stattdessen sollte Folgendes verwendet werden: `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: Dies ist eine optionale Eigenschaft vom Typ „String“. Legen Sie anhand dieser Eigenschaft fest, ob beim Klicken auf ein Listenansichtselement ein Serverpostback ausgelöst oder eine Detailansicht des Elements angezeigt werden soll. Zwei Werte werden unterstützt: „ModelessDialog“ und „Server“. Standardmäßig ist der Wert auf „ModelessDialog“ festgelegt.

25.  SearchOnLoad: Dies ist eine optionale Eigenschaft vom Typ „Boolean“, die angibt, ob das Listenansicht-Steuerelement beim Laden eine Abfrage ausgeführt werden soll. Diese Eigenschaft ist nur anwendbar, wenn sich die Listenansicht im Auswahlmodus befindet. Standardmäßig ist der Wert dieser Eigenschaft auf „true“ festgelegt. Sie können diese Eigenschaft deaktivieren, wenn Sie davon ausgehen, dass der Benutzer den Text in der Regel in das Steuerelement für die Suche eingibt, um ein aussagekräftiges Ergebnis zu erhalten. In diesem Fall zeigt die Listenansicht zuerst eine Meldung an, die dem Benutzer mitteilt, wie eine Suche durchgeführt wird. Der Text kann durch die folgenden Eigenschaften angepasst werden:

26.  MainSearchScreenText: Diese optionale Eigenschaft vom Typ „String“ ist nur anwendbar, wenn „SearchOnload“ auf „true“ festgelegt ist. Wenn die Listenansicht keine automatische Suche durchführt, können anhand dieser Eigenschaft Texte angepasst werden, die in der Mitte der Listenansicht angezeigt werden. Standardmäßig ist der Wert dieser Eigenschaft auf „Suchen Sie die gewünschten Ressourcen mit der obigen Suchfunktion“ festgelegt. Um den Text stärker an Ihr Szenario anzupassen, können Sie einen Wert angeben.

27.  SubSearchScreenText: Mithilfe dieser optionalen Eigenschaft vom Typ „String“ wird der Text angepasst, der unterhalb von „MainSearchScreenText“ angezeigt wird. In der Regel müssen Sie nur dann einen Wert für diese Eigenschaft angeben, wenn Sie weitere Anweisungen zur Verwendung der Listenansicht hinzufügen möchten.

Beispiele für die Verwendung der Listenansicht zusammen mit dem Steuerelement „UocFilterBuilder“ als Vorschauliste finden Sie weiter oben in diesem Dokument im Abschnitt mit den Beispielen zu „UocFilterBuilder“. Das Steuerelement „UocListView“ kann auch ohne Filtergenerator verwendet werden.

### <a name="uocnumericbox"></a>UocNumericBox

Name: UocNumericBox

Beschreibung: Dies ist ein einfaches Textfeld, das nur ganzzahlige Werte akzeptiert. Dieses Steuerelement unterstützt sowohl den schreibgeschützten Modus als auch den aktualisierbaren Modus.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  MaxValue: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Verwenden Sie diese Eigenschaft, um eine clientseitige Validierung für das Steuerelement zu definieren. Der vom Benutzer eingegebene Wert darf nicht diesen Wert überschreiten. Sie können entweder eine explizite ganze Zahl eingeben oder diese durch {Binding Source=schema Path=IntegerMaximum} an Daten vom Typ „Integer“ aus einer Datenquelle binden.

3.  MinValue: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Verwenden Sie diese Eigenschaft, um eine clientseitige Validierung für das Steuerelement zu definieren. Der vom Endbenutzer eingegebene Wert darf nicht unter diesem Wert liegen. Sie können entweder eine explizite ganze Zahl eingeben oder diese durch {Binding Source=schema, Path=IntegerMinimum} mit Daten vom Typ „Integer“ aus einer Datenquelle binden.

4.  DefaultValue: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Wenn mit dem Steuerelement neue Daten erstellt werden, definieren Sie mithilfe dieser Eigenschaft einen Standardwert für das Steuerelement. Dieser Wert kann nur auf eine statische ganze Zahl explizit festgelegt werden.

5.  Value: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Wenn Sie diese Eigenschaft mit Daten vom Typ „Integer“ aus einer Datenquelle binden, wird der Wert dieses Attributs beim Laden der Seite angezeigt und dann nach der Übermittlung mit der Datenquelle gespeichert.

Handler:

   • TextChanged: Dieses Ereignis wird ausgegeben, wenn sich der aktuelle Wert im Steuerelement ändert.

Beispiel:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
Mit dem folgenden Beispielcode wird das erste numerische Feld generiert. Das numerische Feld ist nicht mit einer Datenquelle oder mit Schemainformationen verbunden.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

Mit dem folgenden Beispielcode wird das zweite numerische Feld generiert.

>[!NOTE]
Für die ordnungsgemäße Ausführung dieses Beispiels müssen Sie zunächst ein neues Attribut vom Typ „Integer“ namens „AnIntegerAttribute“ erstellen und mit dem benutzerdefinierten Ressourcentyp binden.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Name: UocPictureBox

Beschreibung: Dieses Steuerelement wird zum Rendern von Bilddaten vom Typ „Binary“ verwendet. Es wird empfohlen, dieses Steuerelement mit Daten vom Typ „Binary“ zu verwenden. Das Bild kann durch eine angegebene Bild-URL mit Daten vom Typ „Binary“ oder die Attributquelle, die Daten vom Typ „Picture“ enthält, gerendert werden.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  ImageUrl: Dies ist eine optionale Eigenschaft vom Typ „String“. Geben Sie die URL des Zielbilds ein.

3.  MaxHeight: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie definiert die maximale Höhe des zu rendernden Bilds in Pixel.

4.  MaxWidth: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie definiert die maximale Breite des zu rendernden Bilds in Pixel.

5.  ImageData: Dies ist eine Eigenschaft vom Typ „Binary“. Verwenden Sie diese Eigenschaft, um eine Datenquelle an das angezeigte Bild zu binden. Die gebundene Datenquelle muss vom Typ „Binary“ sein.
    Mithilfe dieses Felds können Sie auch ein Bild explizit festlegen, indem Sie Daten im byte[]-Format bereitstellen.

6.  ImageResource: Dies ist eine optionale Eigenschaft vom Typ „Binary“.

7.  AlternativeText: Dies ist eine optionale Eigenschaft vom Typ „String“. Diese Eigenschaft wird als alternativer Text angezeigt, wenn das Bild nicht angezeigt werden kann.

„Beispiel:

>[!NOTE]
Um dieses Beispiel verwenden zu können, müssen Bilddaten an das Steuerelement gebunden sein.


Mit dem folgenden Codesegment wird ein Bildfeld-Steuerelement, das eine Datenquelle an das Steuerelement bindet, generiert:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

Mit dem folgenden Codesegment wird ein Bildfeld-Steuerelement, das ein URL-Bild an das Steuerelement bindet, generiert:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Name: UocRadioButtonList

Beschreibung: Dies ist eine einfache Liste mit Optionsfeldern. Die Auswahlmöglichkeiten schließen sich in dieser Liste gegenseitig aus. Dieses Steuerelement wird empfohlen, wenn Benutzer aus bis zu fünf Optionen wählen können. Andernfalls wird UOCListView empfohlen.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  ValuePath: Der Wertpfad ist auf „Value“ festgelegt. Es wird an das Feld „Value“ des Option-Elements gebunden, das in diesem Dokument definiert ist.

3.  CaptionPath: Der Wertpfad ist auf „Caption“ festgelegt. Es wird an das Feld „Caption“ des Option-Elements gebunden, das in diesem Dokument definiert ist.

4.  HintPath: Der Wertpfad ist auf „Hint“ festgelegt. Es wird an das Feld „Hint“ des Option-Elements gebunden, das in diesem Dokument definiert ist.

5.  SelectedValue: Dies ist der zurzeit ausgewählte Wert. Dies ist eine erforderliche Eigenschaft vom Typ „String“. Diese Eigenschaft wird an Zeichenfolgendaten aus der Datenquelle gebunden.

Ereignisse:

1.  SelectedIndexChanged

2.  CheckedChanged

Optionen:

Es dürfen nur zwei Option-Elemente in den Optionen für dieses Steuerelement vorhanden sein.

1.  Value: Das Feld „Value“ in einem einzigen Option-Element muss auf „true“ oder „false“ festgelegt werden.

2.  Caption: Dieses Attribut kann einen beliebigen Zeichenfolgenwert enthalten.

3.  Hint: Dieses Attribut kann einen beliebigen Zeichenfolgenwert enthalten.

Beispiel:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Zur ordnungsgemäßen Ausführung dieses Beispiels müssen Sie ein neues boolesches Attribut, „ABooleanAttribute“, erstellen und es an Ihren benutzerdefinierten Ressourcentyp binden.

Mit dem folgenden Codesegment wird die Liste der Optionsfelder in der voranstehenden Abbildung erstellt:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Name: UocSimpleRadioButton

Beschreibung: Dies ist ein einfaches Optionsfeld-Steuerelement. Die Verwendung dieses Steuerelements ist mit der eines einfachen Kontrollkästchens vergleichbar. Es gibt zwei Optionsfelder, die nebeneinander mit der Beschriftung angezeigt werden. Es wird empfohlen, das Steuerelement an Daten vom Typ „Boolean“ zu binden.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  TrueText: Dies ist eine optionale Eigenschaft vom Typ „String“. Dies ist der Text, der angezeigt wird, wenn das Optionsfeld ausgewählt wird.

3.  FalseText: Dies ist eine optionale Eigenschaft vom Typ „String“. Dies ist der Text, der angezeigt wird, wenn das Optionsfeld nicht ausgewählt wird.

4.  SelectedItem: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Dieser Wert gibt an, dass das Optionsfeld ausgewählt ist. Dieser kann an Daten vom Typ „Boolean“ von einer Datenquelle gebunden werden. Standardmäßig ist der Wert auf „false“ festgelegt.

Ereignisse:

   • CheckedChanged: Wenn sich der Status des Optionsfelds von ausgewählt zu nicht ausgewählt ändert oder umgekehrt, wird dieses Signal ausgegeben.

Beispiel:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Zur ordnungsgemäßen Ausführung dieses Beispiels müssen Sie ein neues boolesches Attribut, „ABooleanAttribute“, erstellen und es an Ihren benutzerdefinierten Ressourcentyp binden. Die RCDC-Daten werden auf denselben benutzerdefinierten Ressourcentyp angewendet.

Mit dem folgenden Codesegment wird die Liste der Optionsfelder in der voranstehenden Abbildung generiert:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Name: UocTextBox

Beschreibung: Dies ist ein einfaches Textfeld, das Eingaben vom Typ „String“ unterstützt. Es wird empfohlen, dieses Steuerelement an Daten vom Typ „String“ zu binden.

Eigenschaften:

1.  All Common Properties : Informationen zu dieser Eigenschaft finden Sie im Abschnitt „Allgemeine Eigenschaften“ dieses Dokuments.

2.  MaxLength: Dies ist ein optionales Attribut vom Typ „Integer“. Diese Eigenschaft gibt die maximale Länge für eine Zeichenfolgeneingabe an. Standardmäßig ist der Wert dieser Eigenschaft auf „128 Zeichen“ festgelegt.

3.  Text: Dies ist eine optionale Eigenschaft vom Typ „String“. Dies ist der Text, der im Textfeld angezeigt wird. Sie können eine explizite Zeichenfolge definieren, die beim erstmaligen Laden des Steuerelements im Textfeld angezeigt wird, oder es an ein Schemaattribut vom Typ „String“ zu binden.

4.  Rows: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Diese Eigenschaft definiert die Höhe des Textfelds in Zeicheneinheiten. Der Standardwert ist „1 Zeichen“.

5.  Columns: Dies ist eine optionale Eigenschaft vom Typ „Integer“. Diese Eigenschaft definiert die Breite des Textfelds in Zeicheneinheiten. Der Standardwert ist „20 Zeichen“.

6.  Wrap: Dies ist eine optionale Eigenschaft vom Typ „Boolean“. Durch Festlegen dieses Eigenschaftenwerts auf „true“ aktiviert der Benutzer die Zeilenumbruchfunktion im Textfeld. Standardmäßig ist der Wert dieser Eigenschaft auf „true“ festgelegt.

7.  UniquenessValidationXPath: Dies ist eine optionale Eigenschaft vom Typ „String“. Sie akzeptiert einen gültigen FIM XPath-Filterausdruck und stellt sicher, dass der vom Benutzer eingegebene Wert innerhalb der Ressourcen, die sich im Filterbereich befinden, eindeutig ist.
    Um beispielsweise sicherzustellen, dass der vom Benutzer angeforderte Anzeigename innerhalb aller E-Mail-aktivierter Sicherheitsgruppen in der Datenbank des FIM-Diensts eindeutig ist, verwenden Sie den XPath-Ausdruck `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. Die Validierungsaktion wird ausgeführt, wenn der Benutzer die Seite verlässt. Diese Eigenschaft wird nur für die Erstellung einer Ressource in der RCDC unterstützt.

8.  UniquenessErrorMessage: Dies ist eine optionale Eigenschaft vom Typ „String“. Diese Zeichenfolge wird verwendet, um eine Fehlermeldung anzuzeigen, wenn bei der UniquenessValidationXPath-Validierung ein Fehler auftritt. Sie kann expliziten Text oder eine Zeichenfolgenressourcenvariable enthalten. Wenn diese Eigenschaft nicht angegeben wird, wird für eine fehlerhafte Validierung standardmäßig folgende Fehlermeldung angezeigt: „Der Wert %VALUE% ist bereits vorhanden. Verwenden Sie einen anderen Wert.“

Ereignisse:

   • TextChanged: Dieses Ereignis wird ausgegeben, wenn sich der Text innerhalb des Textfelds ändert.

Ein vollständiges Beispiel für dieses Steuerelement finden Sie im Abschnitt „Einfache Steuerelemente – Beispiele“.

## <a name="appendix-a-default-xsd-schema"></a>Anhang A: XSD-Standardschema

Im Folgenden wird das vollständige XSD-Schema für alle standardmäßigen RCDCs aufgeführt, die im Lieferumfang von Microsoft Identity Manager 2016 SP1 inbegriffen ist:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Anhang B: Standardmäßige Zusammenfassungs-XSL

Im Folgenden wird die vollständige Zusammenfassung-XSL aufgeführt, die im Lieferumfang von Microsoft Identity Manager 2016 SP1 inbegriffen ist:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
