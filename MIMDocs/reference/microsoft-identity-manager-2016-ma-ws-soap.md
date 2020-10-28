---
title: Workflow Handbuch zum webdienstconnector für SOAP | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mit dem Webdienst-Konfigurations Tool ein neues Projekt für Ihre SOAP-Datenquelle erstellen.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 54f9eb08ce8c400aac5c66467a797bcd3cb097a0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760848"
---
# <a name="web-service-connector-workflow-guide-for-soap"></a>Workflow Handbuch zum webdienstconnector für SOAP

In diesem Artikel wird beschrieben, wie ein neues Projekt für die Datenquelle im Webdienst-Konfigurations Tool erstellt wird. Führen Sie diese Schritte aus, um ein Projekt zu erstellen.

1.  Öffnen Sie das Webdienst-Konfigurations Tool. Es wird ein leeres Projekt geöffnet.

    ![Webdienst-Konfigurations Tool](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-01.png)

2.  Wählen Sie **SOAP-Projekt** und dann **Hinzufügen** aus.

    ![SOAP-Projekt](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-02.png)

3.  Geben Sie auf der nächsten Seite die folgenden Informationen an, und klicken Sie dann auf **weiter** :

    - Der Name des neuen Webdiensts
    - Address (WSDL-Pfad) zum Abrufen der verfügbar gemachten Dienste, Endpunkte und Vorgänge
    - Namespace
    - Sicherheitsmodus (Authentifizierungstyp)
  
4.  In diesem Beispiel wird die Seite **Anmelde** Informationen mit den Anforderungen für den *grundlegenden* Sicherheitsmodus (der Modus, der im vorherigen Schritt ausgewählt wurde) angezeigt. Wenn für den Sicherheitsmodus "None" angegeben wurde, wird keine Seite mit den Anmelde Informationen angezeigt. Wählen Sie **Weiter** aus.

    ![SOAP-Dienst Bildschirm mit Benutzername und Kennwort](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service.png)

5.  Der Zugriff auf den WSDL-Pfad erfolgt, um die Dienst Informationen abzurufen, und die Liste der verfügbar gemachten Funktionen wird angezeigt. Wenn der eingegebene WSDL-Pfad falsch ist, kann das Konfigurationstool die Dienst Informationen nicht abrufen und löst einen Fehler aus.

    ![Bildschirm zum Download des Webdiensts](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-progress.png)

6.  Nachdem die Ermittlung ausgeführt wurde, werden der Endpunkt und die ermittelten Vorgänge aufgelistet. Wählen Sie **Fertig stellen** aus.

    ![Erkannte SOAP-Dienst Endpunkte und-Vorgänge](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service-endpoints.png)

7.  Die Kompilierung wird ausgeführt. Die Kompilierung ist ein Prozess der Kompilierung der datenvertragsassembly, die ein zeitaufwändiger Vorgang sein kann. Der Benutzer wird über etwaige Kompilierungsfehler informiert. Nachdem die Ermittlung ausgeführt wurde, zeigt das Tool die folgende Seite an:

    ![SOAP-Ermittlung](media/microsoft-identity-manager-2016-ma-ws-soap/soap-discovery.png)

8.  Erweitern Sie das **SOAP-Projekt** , und wählen Sie den unten angezeigten verfügbar gemachten Endpunkt Auf diesem Bildschirm werden die Vorgänge aufgeführt, die unter dem Endpunkt deklariert werden.

    ![Unter dem Endpunkt deklarierte Vorgänge](media/microsoft-identity-manager-2016-ma-ws-soap/basic-http-binding.png)

9.  Erweiterender Endpunkt zeigt eine Liste der Vorgänge an. Ein Vorgang ist eine Funktion, die von einem Endpunkt deklariert wird. Jeder Vorgang adressiert einen Tasktyp, der innerhalb des Dienstanbieter ausgeführt werden kann. Auf diesem Bildschirm werden die Argumente aufgelistet, die für den Vorgang deklariert werden. Diese Argumente werden dann definiert, wenn der Vorgang zum Konfigurieren der Workflows verwendet wird.

    ![Erweiterte Endpunkte](media/microsoft-identity-manager-2016-ma-ws-soap/get-employee-byid.png)

10. Der nächste Schritt besteht darin, das Verbindungs Schema des-Verbindungs Bereichs zu definieren. Dies wird erreicht, indem der Objekttyp erstellt und die Objekttypen definiert werden Wählen Sie **Objekttypen** und dann **Hinzufügen** aus. Fügen Sie im neuen Fenster einen neuen Objekttyp hinzu, und geben Sie einen Namen an. Klicken Sie auf **OK** .

    ![Definieren des Objekt Typs](media/microsoft-identity-manager-2016-ma-ws-soap/object-types.png)

11. Das Hinzufügen eines Objekt Typs bietet den folgenden Bildschirm.

    ![Anzeigen des neu erstellten Objekt Typs](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-employee.png)

12. Der Rechte Bereich, der dem Objekttyp entspricht, ermöglicht Ihnen die Verwaltung der Attribute und ihrer Eigenschaften für den ausgewählten Objekttyp. Wählen Sie **Hinzufügen** . Ein neues Fenster wird geöffnet, in dem Attribute hinzugefügt werden:

    ![Attribut und Datentyp](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-firstname.png)

    ![Attribut und Datentyp mit ausgewählter Anker Option](media/microsoft-identity-manager-2016-ma-ws-soap/employeeid-string.png)

13. Nachdem Sie alle erforderlichen Attribute hinzugefügt haben, wird der folgende Bildschirm angezeigt:

    ![Objekttyp mit Attributinformationen](media/microsoft-identity-manager-2016-ma-ws-soap/soap-project.png)

14. Objekttyp und Attribute, nachdem Sie erstellt wurden, stellt leere Workflows bereit, die für die in Microsoft Identity Manager 2016 (MIM) ausgeführten Vorgänge geeignet sind.

    ![Objekttypen zeigt Vorgänge an, die Mitarbeiter ausführen können.](media/microsoft-identity-manager-2016-ma-ws-soap/object-types-operations.png)


## <a name="configure-workflows-in-the-web-service-configuration-tool"></a>Konfigurieren von Workflows im Webdienst-Konfigurations Tool

Der nächste Schritt besteht darin, die Workflows für Ihren Objekttyp zu konfigurieren. Workflow Dateien sind eine Reihe von Aktivitäten, die von Web Services Connector zur Laufzeit verwendet werden. Die Workflows werden verwendet, um den entsprechenden MIM-Vorgang zu implementieren. Das Webdienst-Konfigurations Tool unterstützt Sie beim Erstellen von vier verschiedenen Workflows:

- Import: Importieren Sie Daten aus einer Datenquelle für die folgenden beiden Arten von Workflows:

    - Vollständiger Import: ein vollständiger Import, der konfiguriert werden kann.
    - Delta Import: wird vom Webdienst-Konfigurations Tool nicht unterstützt.

- Export: Exportieren von Daten aus MIM in eine verbundene Datenquelle. Die folgenden drei Aktionen werden für den-Vorgang unterstützt. Sie können diese Aktionen basierend auf Ihren Anforderungen konfigurieren.

    - Hinzufügen
    - Löschen
    - Replace

- Password: führt die Kenn Wort Verwaltung für den Benutzer (Objekttyp) aus. Für diesen Vorgang sind zwei Aktionen verfügbar:

    - Festlegen des Kennworts
    - Kennwort ändern

- Verbindung testen: Konfigurieren Sie einen Workflow, um zu überprüfen, ob die Verbindung mit dem Datenquellen Server erfolgreich hergestellt wurde.

>[!NOTE]
>Sie können diese Workflows für Ihr Projekt konfigurieren oder das Standard Projekt aus dem [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=29944)herunterladen.


### <a name="workflow-designer"></a>Workflow-Designer
Der Workflow-Designer öffnet den Arbeitsbereich, um den Workflow nach Bedarf zu konfigurieren. Für jeden Objekttyp (New/existing) stellt das Konfigurationstool die Knoten für Workflows bereit, die vom Tool unterstützt werden. 

![Workflow-Designer](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-workflow.png)

Der Workflow-Designer besteht aus den folgenden Benutzeroberflächen Elementen:

   - **Knoten im linken** Bereich: mithilfe dieser Knoten können Sie auswählen, welchen Workflow Sie entwerfen möchten.

   - **Zentrales Workflow-Designer** : Hier können Sie die Aktivitäten zum Konfigurieren der Workflows löschen. Zum Erreichen verschiedener MIM-Vorgänge (exportieren, importieren, Kenn Wort Verwaltung) können Sie die standardmäßigen und benutzerdefinierten Workflow Aktivitäten von .net Workflow Framework 4 verwenden. Das Webdienst-Konfigurationstool verwendet standardmäßige und benutzerdefinierte Workflow Aktivitäten. Weitere Informationen zu Standardaktivitäten finden Sie unter [Verwenden von Aktivitäts Designern](http://msdn.microsoft.com/library/ee829528.aspx).

      - Im zentralen Workflow-Designer gibt ein roter Kreis mit Ausrufezeichen neben einer Aktivität an, dass der Vorgang gelöscht wurde und nicht ordnungsgemäß und vollständig definiert ist. Zeigen Sie auf den roten Kreis, um den genauen Fehler zu ermitteln. Nachdem die Aktivität ordnungsgemäß definiert wurde, ändert sich der rote Kreis in das gelbe Informations Zeichen.
      
      - Im zentralen Workflow-Designer gibt eine gelbe Dreiecks Information neben einer Aktivität an, dass die Aktivität definiert ist, aber es gibt noch mehr, was Sie tun können, um die Aktivität abzuschließen. Bewegen Sie den Mauszeiger über das gelbe Dreieck, um weitere Informationen anzuzeigen.

   - **Toolbox** : verpackt alle Tools, einschließlich der System-und benutzerdefinierten Aktivitäten, und vordefinierte Anweisungen zum Entwerfen des Workflows. Weitere Informationen finden Sie unter [Toolbox](http://msdn.microsoft.com/library/aa480213.aspx).
   
   - **Toolbox Abschnitte** : die Toolbox umfasst die folgenden Abschnitte und Kategorien:
   
      - **Description** : der Header der Toolbox. Eine Registerkarte greift auf die Toolbox und die Eigenschaften der ausgewählten Workflow Aktivität zu. 

      - **Workflow importieren** : benutzerdefinierte Aktivitäten zum Konfigurieren von Import Workflows.
      
      - **Workflow exportieren** : benutzerdefinierte Aktivitäten zum Konfigurieren von Export Workflows.
      
      - **Allgemein** : benutzerdefinierte Aktivitäten zum Konfigurieren von Workflows.
      
      - **Debug** : System Workflow Aktivitäten für das Debuggen, das in Workflow 4 definiert ist. Diese Aktivitäten ermöglichen die Problemverfolgung für einen Workflow.
      
      - **Anweisungen** : in Workflow 4 definierte System Workflow Aktivitäten. Weitere Informationen finden Sie unter [Verwenden von Aktivitäts Designern](http://msdn.microsoft.com/library/ee829528.aspx).            

   - **Eigenschaften** : auf der Registerkarte Eigenschaften werden die Eigenschaften einer bestimmten Workflow Aktivität angezeigt, die im Designer Bereich abgelegt und ausgewählt wird. In der Abbildung auf der linken Seite werden die Eigenschaften der **assign** -Aktivität angezeigt. Für jede Aktivität unterscheiden sich die Eigenschaften und werden bei der Konfiguration des benutzerdefinierten Workflows verwendet. Auf dieser Registerkarte können Sie die Attribute des ausgewählten Tools definieren, die im zentralen Workflow-Designer abgelegt wurden. Weitere Informationen finden Sie unter [Eigenschaften](http://msdn.microsoft.com/library/ee342461.aspx)definiert sind.

   - **Task Leiste:** Die Taskleiste enthält drei Elemente: **Variablen** , **Argumente** und **Importe** . Diese Elemente werden in Verbindung mit Workflow Aktivitäten verwendet. Weitere Informationen finden [Sie in der Einführung von Entwickler in Windows Workflow Foundation (WF) in .NET 4](http://msdn.microsoft.com/library/ee342461.aspx).


<h2 id="full-import-workflows">Konfigurieren eines vollständigen Import Workflows im Webdienst-Konfigurations Tool</h2>
Die folgenden Schritte zeigen, wie Sie vollständige Import Workflows für SOAP mit dem Webdienst-Konfigurations Tool konfigurieren.

>[!WARNING]
>In diesem Beispiel wird nur ein Workflow erstellt. Änderungen am Workflow, wie z. b. die Verwendung von benutzerdefinierter Logik in der API, sind möglicherweise erforderlich.

1. Wählen Sie den vollständigen Import Workflow zum Konfigurieren aus. Die **Argumente** und **Importe** sind bereits definiert und spezifisch für die Aktivitäten. Weitere Informationen finden Sie auf den folgenden Bildschirmen.

   ![Vollständige Import-Workflow Argumente](media/microsoft-identity-manager-2016-ma-ws-soap/arguments.png)
 
   ![Importierte Namespaces](media/microsoft-identity-manager-2016-ma-ws-soap/imports.png)

   Ändern Sie nach der Neukonfiguration der Aufrufe die Namen der Attribute, die sich ändern, und fügen Sie den Namespace zu Variablen hinzu, die auf die Rückgabe Struktur der API und Objekttypen verweisen, die auf den alten Namespace verweisen. Die Toolbox im rechten Bereich enthält alle benutzerdefinierten Workflow spezifischen Aktivitäten, die Sie für die Konfiguration benötigen. Weisen Sie die Werte den Variablen zu, die Sie für Ihre Logik verwenden möchten. Wechseln Sie zum unteren Abschnitt des zentralen Workflow-Designers, und deklarieren Sie die Variablen. Variablen werden im nächsten Schritt deklariert.

2. Fügen Sie eine Sequenz Aktivität hinzu. Ziehen Sie den **Sequence** -Aktivitäts Designer aus der **Toolbox** , und legen Sie ihn auf der Windows-Workflow-Designer Oberfläche ab. Weitere Informationen finden Sie in den folgenden Bildschirmen. Die [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) -Aktivität enthält eine geordnete Auflistung von untergeordneten Aktivitäten, die in der angegebenen Reihenfolge ausgeführt werden.
   
    ![Sequence-Aktivität](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-sequence.png)

3. Suchen Sie zum Hinzufügen einer Variablen **Create Variable** . Geben Sie _wsresponse als_ **Name** ein, wählen Sie die Dropdown Liste **Variablentyp** aus, und wählen Sie dann **nach Typen suchen aus** . Ein Dialogfeld wird angezeigt. Wählen Sie **generierte**  >  **Standard**  >  **Antwort** aus. Lassen Sie den **Bereich** und die **Standard** Werte deaktiviert. Sie können diese Werte auch mithilfe der **Eigenschaften** Ansicht festlegen.

   ![Standardantwort](media/microsoft-identity-manager-2016-ma-ws-soap/default-response.png)

   ![Vollständige Import Eigenschaften](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-properties.png)

4. Fügen Sie nun alle anderen Variablen und unten den letzten Bildschirm hinzu.

   ![Vollständige Import Variablen](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-variables.png)

5. Ziehen Sie einen weiteren **Sequenz** Aktivitäts Designer aus der **Toolbox** innerhalb der bereits hinzugefügten Sequenz Aktivität.

6. Ziehen Sie eine **webservicecallactivity** , die unter Common angezeigt wird **.** Diese Aktivität wird verwendet, um den nach der Ermittlung verfügbaren Webdienst Vorgang aufzurufen. Dabei handelt es sich um eine benutzerdefinierte Aktivität, die in verschiedenen Vorgangs Szenarios üblich ist. 

    ![Dienstnamen Vorgang](media/microsoft-identity-manager-2016-ma-ws-soap/service-name-operation.png)

   Um den Webdienst Vorgang zu verwenden, legen Sie die folgenden Eigenschaften fest:
   
      - **Dienst Name** : Geben Sie einen Namen für den Webdienst ein.
      - **Endpunkt Name** : Geben Sie einen Endpunkt Namen für den ausgewählten Dienst an.
      - **Vorgangs Name** : Geben Sie den jeweiligen Vorgang für den Dienst an.
      - **Argument** : Wählen Sie **Argumente** aus. Weisen Sie im nächsten Dialogfeld die Argument Werte zu, wie in der folgenden Abbildung dargestellt:
      
         ![Argumente zuweisen](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

         >[!IMPORTANT]
         >Ändern Sie den **Namen** , die **Richtung** oder den **Typ** für ein Argument nicht mithilfe dieses Dialog Felds. Wenn einer dieser Werte geändert wird, wird die Aktivität ungültig. Legen Sie nur den **Wert** für das-Argument fest. Wie in dieser Abbildung gezeigt, wird der Wert *wsresponse* festgelegt.

7. Fügen Sie eine **foreach** -Aktivität direkt unterhalb von **webservicecallactivity hinzu.** Diese Aktivität wird zum Durchlaufen aller Attribute (sowohl Anker als auch nicht-Anker) des Objekt Typs verwendet. Beim Ziehen dieser Aktivität auf die Workflow-Designer Oberfläche werden automatisch alle Attributnamen für das Objekt aufgelistet. Legen Sie die erforderlichen Werte gemäß dem folgenden Bildschirm fest:

   ![Aktivität für Webdienst Aufrufe](media/microsoft-identity-manager-2016-ma-ws-soap/webservicecallactivity.png)

8. Ziehen Sie eine Aktivität " **kreatecsentrychangescope** " in **foreach** -Text. Diese Aktivität wird verwendet, um beim Abrufen von Daten aus der Ziel Datenquelle eine Instanz des csentrychange-Objekts in der Workflow Domäne für jeden einzelnen Datensatz zu erstellen. Durchziehen dieser Aktivität wird der folgende Bildschirm angezeigt. Die Aktivitäten von " **kreateanchorattribute** " werden automatisch geerbt.

    ![Bereich für Änderungs Bereich für CS-Eintrag erstellen](media/microsoft-identity-manager-2016-ma-ws-soap/createcsentrychangescope.png)

9.  Legen Sie den Wert des DN-Ausdrucks als fest `‘string.Concat ("Employee",item.EmployeeID)’` . Legen _Sie den_ **anchorvalue** für die Mitarbeiter-ID auf **' Convert. $ String (Item) fest.** Mitarbeiter-ID) ". Legen Sie den **objecttyname** als _Mitarbeiter_ fest. Nachdem Sie diese Änderungen vorgenommen haben, wird der folgende Bildschirm angezeigt:

    ![Mitarbeiter-ID erhalten](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

    >[!NOTE]
    >Anker Werte und Objektnamen variieren je nach dem verfügbar gemachten Webdienst. Die Abbildung zeigt ein Beispiel.

10. Ziehen Sie eine Aktivität " **kreateattributechange** " unterhalb der Aktivität " **kreateanchorattribute** ". Die Anzahl der zu ziehenden Aktivitäten ist gleich der Anzahl der nicht Anker Attribute. Weitere Informationen finden Sie in der folgenden Abbildung.

    ![Anker erstellen](media/microsoft-identity-manager-2016-ma-ws-soap/create-anchor.png)

11. Ziehen Sie " **kreatevaluechangeactivity** **" in die** Aktivität "Aktivität" in "Aktivität", und legen Sie Attribut Wert gemäß dem Bildschirm fest.

    ![Ändern eines Attributs](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change.png)

    >[!NOTE]
    >Um diese Aktivität zu verwenden, wählen Sie die entsprechenden Felder aus der Dropdown Liste aus, und weisen Sie die Werte zu. Legen Sie für mehrwertige Attribute mehrere Aktivitäten vom Typ " **kreatevaluechangeactivity** " innerhalb einer Aktivität " **kreateattributechangeactivity** " ab.

12. Fügen Sie eine **if** -Aktivität hinzu, um Bedingungen für ein Attribut hinzuzufügen, wie in der folgenden Abbildung dargestellt:

    ![If-Aktivität](media/microsoft-identity-manager-2016-ma-ws-soap/if.png)

13. Fügen Sie abschließend eine **assign** -Aktivität hinzu, und legen Sie den Ausdruck fest, wie in der folgenden Abbildung dargestellt:

    ![Aktivität zuweisen und den Ausdruck festlegen](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change-email.png)

14. Speichern Sie dieses Projekt am Speicherort `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .
    
    Standardprojekte sollten heruntergeladen und am Speicherort `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` auf dem Zielsystem gespeichert werden. Die Projekte werden dann im Assistenten für Webdienst-Connector angezeigt.
    
    Wenn Sie die ausführbare Datei ausführen, werden Sie aufgefordert, den Speicherort für die Installation anzugeben. Geben Sie den Speicherort ein.
    
    >[!IMPORTANT]
    >Die Projektdatei kann an einem beliebigen Speicherort gespeichert und geöffnet werden (mit den entsprechenden Zugriffsberechtigungen Ihres Executor). Nur Projektdateien, die im Ordner gespeichert werden, `Synchronization Service\Extension` können im Assistenten für webdienstconnector ausgewählt werden, auf den über die Benutzeroberfläche der MIM-Synchronisierung zugegriffen wird.
    
    Der Benutzer, der das Webdienst-Konfigurationstool ausführen muss, benötigt die folgenden Berechtigungen:
    
       - Vollzugriff auf den Synchronisierungs Dienst-Erweiterungs Ordner.
       - Lesezugriff auf den Registrierungsschlüssel `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` , über den sich der Pfad des Erweiterungs Ordners befindet.


## <a name="configure-export-workflows-in-the-web-service-configuration-tool"></a>Konfigurieren von Export Workflows im Webdienst-Konfigurations Tool
In den folgenden Abschnitten wird gezeigt, wie Sie Ihre Workflows mit dem Webdienst-Konfigurations Tool exportieren.

<h3 id="attribute-change-anchor">Workflows hinzufügen</h3>
Fügen Sie Export Workflows hinzu, indem Sie die folgenden Schritte im Webdienst-Konfigurations Tool ausführen.

1. Wählen Sie den zu konfigurier Tier-Export Workflow aus. Wählen Sie unter **exportieren** die Option **Hinzufügen** aus. Die **Argumente** und **Importe** sind bereits definiert und spezifisch für die Aktivitäten. Weitere Informationen finden Sie in den folgenden Bildschirmen.

    ![Hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/add.png)

2. Fügen Sie eine **Sequenz** Aktivität hinzu. Ziehen Sie den **Sequence** -Aktivitäts Designer aus der **Toolbox** , und legen Sie ihn auf der Windows-Workflow-Designer Oberfläche ab. Die [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) -Aktivität enthält eine geordnete Auflistung von untergeordneten Aktivitäten, die in der angegebenen Reihenfolge ausgeführt werden. Wählen Sie **Variable erstellen** aus. Weisen Sie die Werte den Variablen zu, die Sie für Ihre Logik verwenden möchten.

    ![Exportieren](media/microsoft-identity-manager-2016-ma-ws-soap/export-add.png)

    >[!NOTE]
    >Die Schritte zum Hinzufügen einer Variablen werden im Abschnitt zum Erstellen <a href="#full-import-workflows">Vollständiger Import Workflows</a>beschrieben.

3. Ziehen Sie eine **foreach** -Aktivität innerhalb der bereits hinzugefügten **Sequenz** Aktivität, um Anker Attributwerte zu durchlaufen.

4. Wählen Sie **Eigenschaften** aus, und legen Sie die **Werte** gemäß dem Bildschirm fest. Hier ist **objectto Export** ein Argument.

    ![Festlegen der Eigenschaften für die ForEach-Aktivität](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-sequence.png)

5. **Display Name** als **ForEach- \< anchorattribute \>** festlegen

   ![Festlegen des anzeigen Amens](media/microsoft-identity-manager-2016-ma-ws-soap/add-sequence.png)

6. Legen Sie **TypeArgument** als fest `Microsoft.MetadirectoryServices.AnchorAttribute` .

   ![Festlegen des Typarguments](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor.png)

7. Fügen Sie im **foreach** -Text von **anchorattribute** eine **Switch** -Aktivität hinzu.

   ![Switch-Aktivität hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-types.png)

8. Fügen Sie einen Ausdruck gemäß dem folgenden Bildschirm hinzu.

   ![Ausdruck hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Wählen Sie **neuen Fall hinzufügen** aus, und geben Sie einen Wert **für die Mitarbeiter** -ID ein. Ziehen Sie eine **Sequence** -Aktivität, und fügen Sie darin eine **assign** -Aktivität hinzu.

    ![Fügen Sie einen neuen Fall hinzu, und weisen Sie ihn der Sequenz zu.](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-employeeid.png)

10. Weisen **Sie die to** -und **value** -Eigenschaften der **assign** -Aktivität zu.

    ![Zuweisen der to-und Value-Eigenschaften](media/microsoft-identity-manager-2016-ma-ws-soap/anchor-attribute-name.png)

11. Die **foreach** -Aktivität wird für Anker Werte verwendet. Fügen Sie eine weitere **foreach** -Aktivität hinzu, um nicht-Anker Werte zuzuweisen. In diesem Beispiel wird der **attributechange** -Anker verwendet.

    ![Hinzufügen einer weiteren ForEach-Aktivität mit dem attributechange-Anker](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-change.png)

12. Fügen Sie im **foreach** -Text des **attributechange** -Ankers eine **Switch** -Aktivität hinzu.   

    ![Switch-Aktivität für den attributechange-Anker hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-name-wrapper.png)

13. Fügen Sie einen Ausdruck gemäß dem folgenden Bildschirm hinzu.

    ![Fügen Sie einen Ausdruck für die Switch-Aktivität hinzu.](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-expression.png)

14. Wählen Sie **neuen Fall hinzufügen** aus, und geben Sie einen Wert für **FirstName** ein. Ziehen Sie eine **Sequence** -Aktivität, und fügen Sie darin eine **assign** -Aktivität hinzu. Weisen **Sie die to** -und **value** -Eigenschaften der **assign** -Aktivität zu.

    ![Neuen Fall für die Sequenz hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/switch-firstname.png)

15. Fügen Sie Werte für die erforderlichen Attribute wie **LastName** , **Email** usw. hinzu. 

    ![Werte für erforderliche Attribute hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

16. Ziehen Sie unter **Allgemein** eine **webservicecallactivity** , und legen Sie **Werte** für Ihre **Argumente** fest.

    ![Hinzufügen einer Aktivität zum Abrufen eines Webdiensts und Festlegen der Werte](media/microsoft-identity-manager-2016-ma-ws-soap/add-employee-attribute.png)

    >[!IMPORTANT]
    >Ändern Sie den **Namen** , die **Richtung** oder den **Typ** für ein Argument nicht mithilfe dieses Dialog Felds. Wenn einer dieser Werte geändert wird, wird die Aktivität ungültig. Legen Sie nur den **Wert** für das-Argument fest. Wie in dieser Abbildung gezeigt, wird der Wert *wsresponse* festgelegt.

17.  Fügen Sie abschließend eine **if** -Aktivität hinzu, um Antworten zu überprüfen, die vom Webdienst Vorgang zurückgegeben werden.

Die Erstellung des Export Workflows mit dem Vorgang zum **Hinzufügen** ist fertiggestellt:

![Workflow wurde abgeschlossen](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

Speichern Sie dieses Projekt am Speicherort `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="delete-workflows"></a>Workflows löschen
Löschen Sie Export Workflows, indem Sie die folgenden Schritte im Webdienst-Konfigurations Tool ausführen.

1. Wählen Sie den zu konfigurier Tier-Export Workflow aus. Klicken Sie unter **exportieren** auf **Löschen** . Die **Argumente** und **Importe** sind bereits definiert und spezifisch für die Aktivitäten. Weitere Informationen finden Sie in den folgenden Bildschirmen.

   ![Exportieren von Lösch Workflows](media/microsoft-identity-manager-2016-ma-ws-soap/export-delete.png)

2. Fügen Sie eine **Sequenz** Aktivität hinzu. Wählen Sie **Variable erstellen** aus. Weisen Sie die Werte den Variablen zu, die Sie für Ihre Logik verwenden möchten.

   ![Sequenz Aktivität hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/sequence-variables.png)

   >[!NOTE]
   >Die Schritte zum Hinzufügen einer Variablen werden im Abschnitt zum Erstellen <a href="#full-import-workflows">Vollständiger Import Workflows</a>beschrieben.

3. Ziehen Sie eine **foreach** -Aktivität innerhalb der bereits hinzugefügten **Sequenz** Aktivität, um Anker Attributwerte zu durchlaufen.

4. Wählen Sie **Eigenschaften** aus, und legen Sie die **Werte** pro Bildschirm fest. Hier ist **objectto Export** ein Argument.

   ![Festlegen der Eigenschaften für die ForEach-Aktivität](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-object-to-export.png)

5. Legen Sie **Display Name** wie folgt fest `ForEach\<AnchorAttribute\>` :

   ![Festlegen des anzeigen Amens](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor-type.png)

6. Legen Sie **TypeArgument** wie folgt fest `Microsoft.MetadirectoryServices.AnchorAttribute` : 

   ![Festlegen des Typarguments](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-type-argument.png)

7. Fügen Sie im **foreach** -Text von **anchorattribute** eine **Switch** -Aktivität hinzu.

   ![Switch-Aktivität hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-type.png)

8. Fügen Sie einen Ausdruck gemäß dem folgenden Bildschirm hinzu.

   ![Ausdruck hinzufügen](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Wählen Sie **neuen Fall hinzufügen** aus, und geben Sie einen Wert **für die Mitarbeiter** -ID ein. Ziehen Sie eine **Sequence** -Aktivität, und fügen Sie darin eine **assign** -Aktivität hinzu.

   ![Fügen Sie einen neuen Fall hinzu, und weisen Sie ihn der Sequenz zu.](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-default.png)

10. Weisen **Sie die to** -und **value** -Eigenschaften der **assign** -Aktivität zu.

    ![Zuweisen der to-und Value-Eigenschaften](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-attribute-flow.png)

11. Ziehen Sie unter **Allgemein** eine **webservicecallactivity** , und legen Sie **Werte** für Ihre **Argumente** fest.

    ![Hinzufügen einer Aktivität zum Abrufen eines Webdiensts und Festlegen der Werte](media/microsoft-identity-manager-2016-ma-ws-soap/delete-employee.png)

    >[!IMPORTANT]
    >Ändern Sie den **Namen** , die **Richtung** oder den **Typ** für ein Argument nicht mithilfe dieses Dialog Felds. Wenn einer dieser Werte geändert wird, wird die Aktivität ungültig. Legen Sie nur den **Wert** für das-Argument fest. Wie in dieser Abbildung gezeigt, wird die *Wert Mitarbeiter* -ID festgelegt.

12. Fügen Sie schließlich eine **if** -Aktivität hinzu, um die vom Webdienst Vorgang zurückgegebenen Antworten zu überprüfen.

Das Löschen des Export Workflows mit dem **Lösch** Vorgang ist beendet:

![Gelöschter Export Workflow](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

<!-- Image of completed Delete operation is missing from document -->

Speichern Sie dieses Projekt am Speicherort `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="replace-workflows"></a>Workflows ersetzen
Ersetzen Sie Export Workflows, indem Sie die folgenden Schritte im Webdienst-Konfigurations Tool ausführen.

1. Wählen Sie den zu konfigurier Tier-Export Workflow aus. Klicken Sie unter **exportieren** auf **ersetzen** . Die **Argumente** und **Importe** sind bereits definiert und spezifisch für die Aktivitäten. Weitere Informationen finden Sie im folgenden Bildschirm.

   ![Workflow ersetzen](media/microsoft-identity-manager-2016-ma-ws-soap/replace.png)

2. Fügen Sie eine **Sequenz** Aktivität hinzu.

3. Ziehen Sie eine **foreach** -Aktivität für das **\< anchorattribute->.**

4. Fügen Sie eine weitere **ForEach \< attributechange->** Aktivität hinzu, um nicht-Anker Werte zuzuweisen.

5. Zum Schluss sieht der Bildschirm wie in der folgenden Abbildung aus. Die Anweisungen zum Konfigurieren dieser Aktivität finden Sie im Abschnitt zum <a href="#attribute-change-anchor">Hinzufügen von Export Workflows</a>.

   ![Foreach mit einer Switch-Aktivität und einem Anchor-Attribut](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

6. Ziehen Sie unter **Allgemein** eine **webservicecallactivity** , und legen Sie **Werte** für Ihre **Argumente** fest.

   ![Hinzufügen einer Aktivität zum Abrufen eines Webdiensts und Festlegen der Werte](media/microsoft-identity-manager-2016-ma-ws-soap/wsresponse.png)

   >[!IMPORTANT]
   >Ändern Sie den **Namen** , die **Richtung** oder den **Typ** für ein Argument nicht mithilfe dieses Dialog Felds. Wenn einer dieser Werte geändert wird, wird die Aktivität ungültig. Legen Sie nur den **Wert** für das-Argument fest. Wie in dieser Abbildung gezeigt, wird der Wert *Mitarbeiter* festgelegt.

7. Fügen Sie abschließend eine **if** -Aktivität hinzu, um die Antworten zu überprüfen, die vom Webdienst Vorgang zurückgegeben werden.

Der Export Workflow wurde durch den **Ersetzungs** Vorgang ersetzt:

![Export Workflow ersetzen](media/microsoft-identity-manager-2016-ma-ws-soap/operation-name-update-employee.png)

Speichern Sie dieses Projekt am Speicherort `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


## <a name="debug-activities"></a>Aktivitäten Debuggen
Die folgenden benutzerdefinierten Aktivitäten sind zum Debuggen der Workflow Vorlage verfügbar.

### <a name="log-activity"></a>Log-Aktivität
Die **Log** -Aktivität wird verwendet, um Textnachrichten in die Protokolldatei zu schreiben. Weitere Informationen finden Sie unter [Protokollierung](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx).

>[!NOTE]
>Wenn Sie den Workflow nicht problemlos Debuggen können, versuchen Sie, den Workflow in der Produktionsumgebung zu debuggen.  

Legen Sie die folgenden Eigenschaften fest, um die **Log** -Aktivität zu verwenden. Die Eigenschaften sind sichtbar, wenn Sie die Aktivität in Workflow-Designer auswählen und die **Eigenschaften** der Aktivität anzeigen.
<!-- Properties missing from document -->
<!-- Image of Log activity GUI missing from document -->

### <a name="writeline-activity"></a>Aktivität "Write teline"
Die " **Write teline** "-Aktivität wird verwendet, um Textnachrichten in den Writer eines Anbieters zu schreiben. Wenn kein Writer verfügbar ist, schreibt die [Schreibaktivität](http://127.0.0.1:47873/help/1-7016/ms.help?method=page&id=T%3ASYSTEM.ACTIVITIES.STATEMENTS.WRITELINE&product=VS&productVersion=100&topicVersion=100&locale=EN-US&topicLocale=EN-US&embedded=true) den Text in das Konsolenfenster.

<!-- Image of WriteLine activity GUI missing from document -->

Schreiben Sie im Textfeld die Meldung, die Sie im Writer-Ziel sichtbar sein möchten.

>[!IMPORTANT]
>Das Konsolenfenster kann für diese Aktivität nicht verwendet werden. Verwenden Sie für diese Aufgabe einen anderen Fenster Ausgabewriter.

Legen Sie die folgenden Eigenschaften fest, um die " **Write teline** "-Aktivität zu verwenden. Die Eigenschaften sind sichtbar, wenn Sie die Aktivität in Workflow-Designer auswählen und die **Eigenschaften** der Aktivität anzeigen.

- **Protokollebene** : gibt die Menge an Inhalt an, der in den Protokoll Wert geschrieben werden soll. Mögliche Werte:

    - Hoch: Schreiben Sie die **LogText** -Nachricht in die Protokolldatei, wenn der Schweregrad des Protokolls auf hoch festgelegt ist.
    - Verbose: schreibt die **LogText** -Nachricht in die Protokolldatei, wenn der Schweregrad des Protokolls auf ausführlich festgelegt ist.
    - Deaktiviert: Schreiben Sie nicht in die Protokolldatei.
- **LogText** : gibt den Text Inhalt an, der in das Protokoll geschrieben werden soll.
- **Tag** : Fügt dem Text ein Tag hinzu, um den Typ des Inhalts zu identifizieren, der im Protokoll geschrieben wird. Folgende Werte sind möglich: Fehler, Ablauf Verfolgung oder Warnung.

<!-- log severity is not defined in this document -->


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über den generischen Webdienst-Connector](microsoft-identity-manager-2016-ma-ws.md)
- [Installieren des Webdienst-Konfigurationstools](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Rest-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webdienst-MA-Konfiguration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
