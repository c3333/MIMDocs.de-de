---
title: Konfigurieren von SharePoint für Microsoft Identity Manager 2016 | Microsoft-Dokumentation
description: Installieren und konfigurieren Sie SharePoint Foundation, sodass es die MIM-Portalseite hosten kann.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5beee58388c12abbe8e3245ff610d3528b03786a
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690773"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Einrichten eines Identitätsverwaltungsservers: SharePoint

> [!div class="step-by-step"]
> [« SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server »](prepare-server-exchange.md)
> 
> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Name des Domänencontrollers: **corpdc**
> - Domänenname: **contoso**
> - Servername des MIM-Diensts: **corpservice**
> - Servername der MIM-Synchronisierung: **corpsync**
> - SQL-Servername: **corpsql**
> - Kennwort – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Installieren von **SharePoint 2016**

> [!NOTE]
> Das Installationsprogramm erfordert eine Internetverbindung, um die erforderlichen Komponenten herunterzuladen. Befindet sich der Computer in einem virtuellen Netzwerk, das keine Internetverbindung bietet, fügen Sie dem Computer eine weitere Netzwerkschnittstelle hinzu, die eine Verbindung mit dem Internet ermöglicht. Diese kann deaktiviert werden, nachdem die Installation abgeschlossen ist.

Führen Sie die folgenden Schritte aus, um SharePoint 2016 zu installieren. Nach Abschluss der Installation wird der Server neu gestartet.

1.  Starten Sie **PowerShell** als Domänenkonto mit lokalem Administrator auf **corpservice** und **sysadmin** auf dem SQL-Datenbankserver, den Sie für **contoso\miminstall** verwenden.

    -   Wechseln Sie in das Verzeichnis, in dem SharePoint entpackt wurde.

    -   Geben Sie folgenden Befehl ein:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Nachdem die erforderlichen Komponenten für **SharePoint** installiert sind, installieren Sie **SharePoint 2016**, indem Sie den folgenden Befehl eingeben:

    ```
    .\setup.exe
    ```

3.  Wählen Sie den Typ für einen vollständigen Server aus.

4.  Nachdem die Installation abgeschlossen ist, führen Sie den Assistenten aus.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Führen Sie den Assistenten aus, um SharePoint zu konfigurieren

Folgen Sie den im **Konfigurations-Assistenten für SharePoint-Produkte** erläuterten Schritten, um SharePoint für die Arbeit mit MIM zu konfigurieren.

1. Wechseln Sie auf die Registerkarte **Verbindung mit einer Serverfarm herstellen** , um eine neue Serverfarm zu erstellen.

2. Geben Sie diesen Server als Datenbankserver wie **corpsql** für die Konfigurationsdatenbank und *Contoso\SharePoint* als Datenbankzugriffskonto für SharePoint an.
3. Erstellen Sie ein Kennwort für die Passphrase der Farmsicherheit.

4. Der Konfigurations-Assistent empfiehlt die Auswahl des Typs [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) von **Front-end**.

5. Wenn der Konfigurations-Assistent die Konfigurationsaufgabe 10 von 10 abgeschlossen hat, klicken Sie auf „Fertig stellen“. Daraufhin wird ein Webbrowser geöffnet.

6. Wenn Sie dazu aufgefordert werden, authentifizieren Sie sich im Internet Explorer-Popup als *Contoso\miminstall* (oder mit dem entsprechenden Administratorkonto), um den Vorgang fortzusetzen.

7. Klicken Sie im Web-Assistenten (innerhalb der Web-App) auf **Cancel/Skip** (Abbrechen/Überspringen).


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Vorbereiten von SharePoint zum Hosten des MIM-Portals

> [!NOTE]
> Zunächst wird SSL nicht konfiguriert. Achten Sie darauf, dass Sie SSL oder ähnliches konfigurieren, bevor Sie den Zugriff auf dieses Portal ermöglichen.

1. Starten Sie die **SharePoint 2016-Verwaltungsshell**, und führen Sie das folgende PowerShell-Skript aus, um eine **SharePoint 2016-Webanwendung** zu erstellen.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Es wird eine Warnmeldung angezeigt, in der angegeben ist, dass die klassische Windows-Authentifizierungsmethode verwendet wird, und es kann mehrere Minuten dauern, bis der letzte Befehl abgeschlossen ist. Nach Abschluss gibt die Ausgabe die URL des neuen Portals an. Lassen Sie das Fenster **SharePoint 2016-Verwaltungsshell** geöffnet, um später darauf zurückzukommen.

2. Starten Sie „SharePoint 2016-Verwaltungsshell“, und führen Sie das folgende PowerShell-Skript aus, um eine **SharePoint-Websitesammlung** zu erstellen, die dieser Webanwendung zugeordnet ist.

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > Vergewissern Sie sich, dass die Variable *CompatibilityLevel* das Ergebnis „15“ hat. Wenn das Ergebnis von „15“ abweicht, wurde die Websitesammlung nicht für die korrekte Umgebungsversion erstellt. Löschen Sie die Websitesammlung, und erstellen Sie diese neu.

3. Deaktivieren Sie den **serverseitigen SharePoint-Ansichtszustand** und die SharePoint-Aufgabe „Integritätsanalyseauftrag (Stündlich, Microsoft SharePoint Foundation-Timer, Alle Server)“, indem Sie die folgenden PowerShell-Befehle in der **SharePoint 2016-Verwaltungsshell** ausführen:

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Öffnen Sie auf Ihrem Identitätsverwaltungsserver eine neue Registerkarte des Webbrowsers, navigieren Sie zu http://mim.contoso.com/, und melden Sie sich als *contoso\miminstall* an.  Eine leere SharePoint-Website namens *MIM-Portal* wird angezeigt.

    ![Bild: MIM-Portal unter http://mim.contoso.com/](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Kopieren Sie die URL, und öffnen Sie in Internet Explorer das Dialogfeld **Internetoptionen**. Wechseln Sie zur Registerkarte **Sicherheit**, wählen Sie **Lokales Intranet** aus, und klicken Sie auf **Websites**.

    ![Bild: Internetoptionen](media/MIM-DeploySP2.png)

6. Klicken Sie im Fenster **Lokales Intranet** auf **Erweitert**, und fügen Sie die kopierte URL in das Textfeld **Diese Website zur Zone hinzufügen** ein. Klicken Sie auf **Hinzufügen**, und schließen Sie die Fenster.

7. Öffnen Sie das Programm **Verwaltungstools**, und navigieren Sie zu **Dienste**. Suchen Sie den SharePoint-Verwaltungsdienst, und starten Sie ihn, sofern dieser nicht bereits ausgeführt wird.

> [!div class="step-by-step"]  
> [« SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server »](prepare-server-exchange.md)
