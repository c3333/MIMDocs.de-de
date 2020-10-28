---
title: Workflow Handbuch zum webdienstconnector für die Rest-API | Microsoft-Dokumentation
description: In diesem Artikel wird das Bereitstellen eines REST-API-Beispiels erläutert.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e0c00972983d964a489d7c76e06e271bdf91b79e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760845"
---
# <a name="web-service-connector-workflow-guide-for-a-rest-api-sample"></a>Workflow Handbuch zum webdienstconnector für ein Rest-API-Beispiel

Dieser Artikel behandelt die Bereitstellung einer Beispiel-Rest-API, um das Webdienst-Konfigurationstool mit einer Rest-API-Webdaten Quelle zu durchlaufen.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen sind erforderlich, um das Beispiel zu verwenden:

- Das Webdienst-Konfigurations Tool ist installiert.
- Der Rest-Datenquellen-Beispiel Dienst wird bereitgestellt. Laden Sie das Beispiel herunter, und installieren Sie es von (siehe hier).

<!-- No link provided for "see here" -->
<!-- Should Note go with bullet point #2 -->

>[!NOTE]
>JSON-Daten müssen ein einzelnes-Objekt mit einer-Eigenschaft enthalten, die ein Array enthält.

<!-- Should JSON be exactly as-is or just a sample -->
<!-- Should JSON be part of Note content -->
```JSON
{

"EmployeeList":[

{"id":"1","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""},{"id":"2","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""}

]

}
```

## <a name="configure-rest-project-discovery-in-the-web-service-configuration-tool"></a>Konfigurieren der Rest-Projekt Ermittlung im Webdienst-Konfigurations Tool
In den folgenden Schritten wird gezeigt, wie Sie im Webdienst-Konfigurations Tool ein neues Projekt für die Datenquelle erstellen.

1. Öffnen Sie das Webdienst-Konfigurations Tool. Es wird ein leeres SOAP-Projekt geöffnet.

   ![Webdienst-Konfigurations Tool](media/microsoft-identity-manager-2016-ma-ws-restgeneric/web-service-configuration-tool.png)

2. Wählen Sie **Datei**  >  **Neues**  >  **Rest-Projekt** aus.

   ![Erstellen eines neuen Rest-Projekts](media/microsoft-identity-manager-2016-ma-ws-restgeneric/new-project.png)
   <!-- Image shows SOAP project selected, not REST project -->

3. Wählen Sie auf der linken Seite **Rest-Projekt** aus, und klicken Sie auf **Hinzufügen** .

   ![Rest-Projekt auswählen](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-project.png)

4. Geben Sie auf der nächsten Seite die folgenden Informationen ein:

   - Der Name des neuen Webdiensts
   - Adresse (Rest-API-URL-Pfad)
   - Namespace
   - Sicherheitsmodus (Authentifizierungstyp)

   ![REST-Dienst](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service.png)
    
   Der folgende Bildschirm zeigt Beispiele für diese Werte:
    
   ![Beispiel Werte für den Rest-Dienst](media/microsoft-identity-manager-2016-ma-ws-restgeneric/restsample.png)

   Legen Sie den **Sicherheitsmodus** auf _keine_ fest. Legen Sie die **Adresse** auf den JSON-Beispiel Server fest, der in Azure gehostet wird.

5. Klicken Sie auf **OK** . Das Rest-Projekt, das im Webdienst-Konfigurations Tool aufgeführt ist.

   ![Rest-Projekt im Webdienst-Konfigurations Tool](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-discovery.png)

6. Der nächste Schritt besteht darin, den Rest-API-Aufruf zu definieren und den Aufruf der Windows Communication Foundation (WCF)-Aufrufe zu übersetzen.

   1. Erweitern Sie das **Rest-Projekt** , und wählen Sie den Dienst _restsample_ aus.

   2. Wählen Sie **Hinzufügen** . Sie werden aufgefordert, zwei Werte hinzuzufügen:
   
      ![Werte für den Rest-Dienst eingeben](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service-highlights.png)
      
      1. Geben Sie den **Namen** ein. Dieser Schritt wird im Screenshot als 3 bezeichnet.
      2. Geben Sie die **Adresse** ein. Dieser Schritt wird im Screenshot als 4 bezeichnet.
      3. Klicken Sie auf **OK** . Der Beschreibung für den _Rest Sample_ -Dienst wird eine Rest-Ressource hinzugefügt.

7. Wählen Sie im Feld **Ressourcen** die Rest-Ressource aus, die Sie soeben hinzugefügt haben. Fügen Sie die folgende Methode hinzu:

   ![Fügen Sie der Ressource eine Rest-Methode hinzu.](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-method.png)
   <!-- How does this dialog appear, by selecting Edit? -->

8. Wählen Sie die Rest-Methode aus. Beachten Sie, dass es möglich ist, mehrere Methoden in derselben Ressource zu erstellen und die während der Ausführung bestandenen Abfragen zu definieren.

9. Für die GetAll-Methode sind keine Abfragen erforderlich. Lassen Sie die Parameterwerte leer. Beim Exportieren oder Importieren der Rest-API müssen Sie die Beispiel Anforderung/oder Response abhängig von der Funktion definieren. Kopieren Sie die JSON-Rückgabe, und fügen Sie Sie beim Navigieren zu diesem Beispiel ein.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-samples.png)

10. Wählen Sie **Speichern** aus. Speichern Sie das Projekt in `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions` . 

>[!NOTE]
>Nachdem das Projekt gespeichert wurde, wird die wsconfig-Datei generiert. Die Konfigurationsdatei enthält mehrere Dateien, die zuvor in der Übersicht über den Webdienst definiert wurden.


## <a name="configure-object-types-in-the-web-service-configuration-tool"></a>Konfigurieren von Objekttypen im Webdienst-Konfigurations Tool
In den folgenden Schritten wird gezeigt, wie Sie Objekttypen für die Datenquelle im Webdienst-Konfigurations Tool konfigurieren.

1. Der nächste Schritt besteht darin, das-Connector-Speicher Schema zu definieren. Dies wird erreicht, indem der Objekttyp erstellt und deren Objekttypen definiert werden. Klicken Sie im linken Bereich auf **Objekttypen** , und klicken Sie auf **Hinzufügen** . Dies wird unter dem Bildschirm angezeigt. Fügen Sie einen neuen Objekttyp und einen Namen bereit. Klicken Sie auf die Schaltfläche **OK** .

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-types.png)

2. Das Hinzufügen eines Objekt Typs bietet den folgenden Bildschirm.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-type-employee.png)

3. Der Rechte Bereich, der dem Objekttyp entspricht, ermöglicht Ihnen die Verwaltung der Attribute und ihrer Eigenschaften für den ausgewählten Objekttyp. Wenn Sie auf die Schaltfläche Hinzufügen klicken, wird der folgende Bildschirm angezeigt.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employeeid-string.png)

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-id-string.png)

4. Nach dem Hinzufügen aller erforderlichen Attribute wird der Bildschirm unten angezeigt.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-object-type-02.png)

5. Objekttyp und Attribute, nachdem Sie erstellt wurden, stellt leere Workflows bereit, die für die in Microsoft Identity Manager (MIM) ausgeführten Vorgänge sorgen.


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

![Workflow-Designer](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-configuration-workflow.png)

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



## <a name="configure-a-full-import-workflow-in-the-web-service-configuration-tool"></a>Konfigurieren eines vollständigen Import Workflows im Webdienst-Konfigurations Tool
Die folgenden Schritte zeigen, wie Sie mithilfe des Webdienst-Konfigurationstools vollständige Import Workflows für die Rest-API konfigurieren.

>[!WARNING]
>In diesem Beispiel wird nur ein Workflow erstellt. Änderungen am Workflow, wie z. b. die Verwendung von benutzerdefinierter Logik in der API, sind möglicherweise erforderlich.

1. Wählen Sie den vollständigen Import Workflow zum Konfigurieren aus. Die **Argumente** und **Importe** sind bereits definiert und spezifisch für die Aktivitäten. Weitere Informationen finden Sie auf den folgenden Bildschirmen.

   ![Vollständige Import-Workflow Argumente](media/microsoft-identity-manager-2016-ma-ws-restgeneric/arguments.png)

   ![Importierte Namespaces](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imported-name-spaces.png)

   Nach der Neukonfiguration der Aufrufe müssen Sie die Namen der Attribute ändern, die sich ändern, oder den Namespace zu Variablen hinzufügen, die auf die Rückgabe Struktur der API und Objekttypen verweisen, die auf den alten Namespace verweisen. Die Toolbox im rechten Bereich enthält alle benutzerdefinierten Workflow spezifischen Aktivitäten, die Sie für die Konfiguration benötigen. Weisen Sie die Werte den Variablen zu, die Sie für Ihre Logik verwenden möchten. Wechseln Sie zum unteren Abschnitt des zentralen Workflow-Designers, und deklarieren Sie die Variablen. Variablen werden im nächsten Schritt deklariert.

2. Fügen Sie eine Sequenz Aktivität hinzu. Ziehen Sie den **Sequence** -Aktivitäts Designer aus der **Toolbox** , und legen Sie ihn auf der Windows-Workflow-Designer Oberfläche ab. Weitere Informationen finden Sie in den folgenden Bildschirmen. Die [Sequence](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) -Aktivität enthält eine geordnete Auflistung von untergeordneten Aktivitäten, die in der angegebenen Reihenfolge ausgeführt werden.
   
    ![Sequence-Aktivität](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imports.png)

3. Suchen Sie zum Hinzufügen einer Variablen **Create Variable** . Geben Sie _wsresponse als_ **Name** ein, wählen Sie die Dropdown Liste **Variablentyp** aus, und wählen Sie dann **nach Typen suchen aus** . Ein Dialogfeld wird angezeigt. Wählen Sie **generierte**  >  **GetAll** -  >  **Antwort** aus. Lassen Sie den **Bereich** und die **Standard** Werte deaktiviert. Sie können diese Werte auch mithilfe der **Eigenschaften** Ansicht festlegen.

   ![Standardantwort](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list.png)

4. Ziehen Sie einen weiteren **Sequenz** Aktivitäts Designer aus der **Toolbox** innerhalb der bereits hinzugefügten Sequenz Aktivität.

5. Ziehen Sie eine **webservicecallactivity** , die unter Common angezeigt wird **.** Diese Aktivität wird verwendet, um den nach der Ermittlung verfügbaren Webdienst Vorgang aufzurufen. Dabei handelt es sich um eine benutzerdefinierte Aktivität, die in verschiedenen Vorgangs Szenarios üblich ist. 

    ![Dienstnamen Vorgang](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-operation-workflow.png)

   Um den Webdienst Vorgang zu verwenden, legen Sie die folgenden Eigenschaften fest:
   
      - **Dienst Name** : Geben Sie einen Namen für den Webdienst ein.
      - **Endpunkt Name** : Geben Sie einen Endpunkt Namen für den ausgewählten Dienst an.
      - **Vorgangs Name** : Geben Sie den jeweiligen Vorgang für den Dienst an.
      - **Argument** : Wählen Sie **Argumente** aus. Weisen Sie im nächsten Dialogfeld die Argument Werte zu, wie in der folgenden Abbildung dargestellt:
      
         ![Argumente zuweisen](media/microsoft-identity-manager-2016-ma-ws-restgeneric/get-all.png)

         >[!IMPORTANT]
         >Ändern Sie den **Namen** , die **Richtung** oder den **Typ** für ein Argument nicht mithilfe dieses Dialog Felds. Wenn einer dieser Werte geändert wird, wird die Aktivität ungültig. Legen Sie nur den **Wert** für das-Argument fest. Wie in dieser Abbildung gezeigt, wird der Wert *wsresponse* festgelegt.

6. Fügen Sie eine **foreach** -Aktivität direkt unterhalb von **webservicecallactivity hinzu.** Diese Aktivität wird zum Durchlaufen aller Attribute (sowohl Anker als auch nicht-Anker) des Objekt Typs verwendet. Beim Ziehen dieser Aktivität auf die Workflow-Designer Oberfläche werden automatisch alle Attributnamen für das Objekt aufgelistet. Legen Sie die erforderlichen Werte gemäß dem folgenden Bildschirm fest:

   ![Aktivität für Webdienst Aufrufe](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach.png)

7. In einigen Fällen müssen Sie möglicherweise die generated.dll in der wsconfig-Datei öffnen. Kopieren Sie diese wsconfig-Datei, und benennen Sie Sie mit der ZIP-Erweiterung um. Öffnen und extrahieren Sie die generated.dll mit Ihrem bevorzugten .net-reflektortool.

   ![Konfigurationsdatei](media/microsoft-identity-manager-2016-ma-ws-restgeneric/config-files.png)

8. Identifizieren Sie den öffentlichen Namespace für die Mitarbeiter _Liste_ :

    ![Mitarbeiter Listen Code](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list-code.png)

    Fügen Sie dann diese Rückgabe dem Workflow **foreach** hinzu:

    ![Hinzufügen der Mitarbeiterliste zum foreach-Workflow](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach-employee-list.png)

9. Ziehen Sie eine Aktivität " **kreatecsentrychangescope** " in **foreach** -Text. Diese Aktivität wird verwendet, um beim Abrufen von Daten aus der Ziel Datenquelle eine Instanz des csentrychange-Objekts in der Workflow Domäne für jeden einzelnen Datensatz zu erstellen. Durchziehen dieser Aktivität wird der folgende Bildschirm angezeigt. Die Aktivitäten von " **kreateanchorattribute** " werden automatisch geerbt. Aktualisieren Sie den **DN** -Wert auf Ihren bevorzugten Domänen Namen.

    ![Bereich für Änderungs Bereich für CS-Eintrag erstellen](media/microsoft-identity-manager-2016-ma-ws-restgeneric/createcsentry.png)

    >[!NOTE]
    >Anker Werte und Objektnamen variieren je nach dem verfügbar gemachten Webdienst. Die Abbildung zeigt ein Beispiel.

10. Ziehen Sie eine Aktivität " **kreateattributechange** " unterhalb der Aktivität " **kreateanchorattribute** ". Die Anzahl der zu ziehenden Aktivitäten ist gleich der Anzahl der nicht Anker Attribute. Weitere Informationen finden Sie in der folgenden Abbildung.

    ![Anker erstellen](media/microsoft-identity-manager-2016-ma-ws-restgeneric/create-anchor-attribute.png)

    >[!NOTE]
    >Um diese Aktivität zu verwenden, wählen Sie die entsprechenden Felder aus der Dropdown Liste aus, und weisen Sie die Werte zu. Legen Sie für mehrwertige Attribute mehrere Aktivitäten vom Typ " **kreatevaluechangeactivity** " innerhalb einer Aktivität " **kreateattributechangeactivity** " ab.

11. Speichern Sie dieses Projekt am Speicherort `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` . Konfigurieren Sie dann den Verwaltungs-Agent wie in [Webdienst-MA-Konfiguration](microsoft-identity-manager-2016-ma-ws-maconfig.md)beschrieben.

    ![Speichern Sie das Rest-Projekt.](media/microsoft-identity-manager-2016-ma-ws-restgeneric/sample-rest.png)
    
    Standardprojekte sollten heruntergeladen und am Speicherort `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` auf dem Zielsystem gespeichert werden. Die Projekte werden dann im Assistenten für Webdienst-Connector angezeigt.
    
    Wenn Sie die ausführbare Datei ausführen, werden Sie aufgefordert, den Speicherort für die Installation anzugeben. Geben Sie den Speicherort ein.
    
    >[!IMPORTANT]
    >Die Projektdatei kann an einem beliebigen Speicherort gespeichert und geöffnet werden (mit den entsprechenden Zugriffsberechtigungen Ihres Executor). Nur Projektdateien, die im Ordner gespeichert werden, `Synchronization Service\Extension` können im Assistenten für webdienstconnector ausgewählt werden, auf den über die Benutzeroberfläche der MIM-Synchronisierung zugegriffen wird.
    
    Der Benutzer, der das Webdienst-Konfigurationstool ausführen muss, benötigt die folgenden Berechtigungen:
    
       - Vollzugriff auf den Synchronisierungs Dienst-Erweiterungs Ordner.
       - Lesezugriff auf den Registrierungsschlüssel `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` , über den sich der Pfad des Erweiterungs Ordners befindet.


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über den generischen Webdienst-Connector](microsoft-identity-manager-2016-ma-ws.md)
- [Installieren des Webdienst-Konfigurationstools](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Rest-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webdienst-MA-Konfiguration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
