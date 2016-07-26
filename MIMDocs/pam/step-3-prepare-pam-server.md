---
title: "Bereitstellen von PAM Schritt 3 – PAM-Server | Microsoft Identity Manager"
description: "Bereiten Sie einen PAM-Server vor, der SQL und SharePoint für Ihre Bereitstellung von Privileged Access Management hostet."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 1a21399df9528f689b811400a660543853d88472


---

# Schritt 3: Vorbereiten eines PAM-Servers

>[!div class="step-by-step"]
[« Schritt 2](step-2-prepare-priv-domain-controller.md)
[Schritt 4 »](step-4-install-mim-components-on-pam-server.md)

## Installieren von Windows Server 2012 R2
Installieren Sie auf einem dritten virtuellen Computer Windows Server 2012 R2, genau gesagt Windows Server 2012 R2 Standard (Server mit grafischer Benutzeroberfläche) x64, um den Computer *PAMSRV* zu erstellen. Es sind mindestens 8 GB RAM erforderlich, weil SQL Server und SharePoint 2013 auf diesem Computer installiert werden.

1. Wählen Sie **Windows Server 2012 R2 Standard (Server mit grafischer Benutzeroberfläche) x64** aus.

    ![Auswahl von Windows Server Standard mit grafischer Benutzeroberfläche – Screenshot](media/PAM_GS_Select_WS2012.png)

2. Lesen und akzeptieren Sie die Lizenzbedingungen.

3.  Da dieser Datenträger leer sein wird, wählen Sie **Benutzerdefiniert: Nur Windows installieren** aus, und nutzen Sie den **nicht initialisierten Speicherplatz**.

4.  Melden Sie sich bei dem neuen Computer als Administrator an.  In der Systemsteuerung weisen Sie dem Computer eine statische IP-Adresse im virtuellen Netzwerk zu, konfigurieren die Netzwerkschnittstelle so, dass sie DNS-Abfragen an die IP-Adresse von PRIVDC sendet, und legen den Computernamen auf *PAMSRV* fest.  Dies erfordert einen Neustart des Servers.

5.  Ist über das virtuelle Netzwerk keine Internetverbindung möglich, fügen Sie dem Computer eine weitere Netzwerkschnittstelle hinzu, die eine Verbindung mit dem Internet ermöglicht.  Dies ist für die SharePoint-Installation erforderlich und kann deaktiviert werden, nachdem dieser Schritt abgeschlossen ist.

6.  Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an. Konfigurieren Sie den Computer mithilfe der Systemsteuerung, um nach Updates zu suchen und ggf. alle erforderlichen Updates zu installieren.  Dies erfordert möglicherweise einen Neustart des Servers.

7.  Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an, öffnen Sie die Systemsteuerung, und binden Sie PAMSRV in die Domäne PRIV (priv.contoso.local) ein.  Dazu müssen Sie den Benutzernamen und die Anmeldeinformationen eines Administrators der Domäne PRIV angeben, (PRIV\\Administrator). Nachdem die Willkommensnachricht angezeigt wird, schließen Sie das Dialogfeld, und starten Sie den Server neu.


### Hinzufügen der Rollen „Webserver (IIS)“ und „Anwendungsserver“
Fügen Sie die Rollen "Webserver (IIS)" und "Anwendungsserver", die .NET Framework 3.5-Features, das Active Directory-Modul für Windows PowerShell und weitere Features hinzu, die für SharePoint erforderlich sind.

1.  Melden Sie sich als PRIV-Domänenadministrator (PRIV\Administrator) an, und starten Sie PowerShell.

2.  Geben Sie die folgenden Befehle ein. Möglicherweise ist es notwendig, dass Sie einen anderen Speicherort für die Quelldateien für .NET Framework 3.5-Features angeben. Diese Funktionen sind in der Regel nicht vorhanden, wenn Windows Server installiert wird. Sie sind aber auf dem Betriebssystem-Installationsdatenträger im Ordner „Sources“ im parallelen Ordner (SxS) verfügbar, beispielsweise „D:\Sources\SxS“.\.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Konfigurieren der Serversicherheitsrichtlinie
Konfigurieren Sie die Serversicherheitsrichtlinien, damit die neu erstellten Konten als Dienste ausgeführt werden können.

1.  Starten Sie das Programm **Lokale Sicherheitsrichtlinie** .   
2.  Navigieren Sie zu **Lokale Richtlinien** > **Zuweisen von Benutzerrechten**.  
3.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **Anmelden als Dienst**, und wählen Sie dann **Eigenschaften**aus.  
4.  Klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und geben Sie in „Benutzer- und Gruppennamen“ die Zeichenfolge *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer* ein. Klicken Sie auf **Namen überprüfen** und dann auf **OK**.  

5.  Klicken Sie auf **OK**, um das Dialogfeld „Eigenschaften“ zu schließen.  
6.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **Zugriff vom Netzwerk auf diesen Computer verweigern**, und wählen Sie **Eigenschaften** aus.  
7.  Klicken Sie auf **Benutzer oder Gruppe hinzufügen**, geben Sie in „Benutzer- und Gruppennamen“ die Zeichenfolge *priv\mimmonitor; priv\MIMService; priv\mimcomponent* ein, und klicken Sie auf **OK**.  
8.  Klicken Sie auf **OK**, um das Dialogfeld „Eigenschaften“ zu schließen.  

9. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Lokal anmelden verweigern**, und wählen Sie dann **Eigenschaften** aus.  
10. Klicken Sie auf **Benutzer oder Gruppe hinzufügen**, geben Sie in „Benutzer- und Gruppennamen“ die Zeichenfolge *priv\mimmonitor; priv\MIMService; priv\mimcomponent* ein, und klicken Sie auf **OK**.  
11. Klicken Sie auf **OK**, um das Dialogfeld „Eigenschaften“ zu schließen.  
12. Schließen Sie das Fenster „Lokale Sicherheitsrichtlinien“.  

13. Öffnen Sie die Systemsteuerung, und wechseln Sie zu **Benutzerkonten**.  
14. Klicken Sie auf **Anderen Benutzern Zugriff auf diesen Computer geben**.  
15. Klicken Sie auf **Hinzufügen**, geben Sie den Benutzer *MIMADMIN* in der Domäne *PRIV* ein, und klicken Sie auf dem nächsten Bildschirm des Assistenten auf **Diesen Benutzer als Administrator hinzufügen**.  
16. Klicken Sie auf **Hinzufügen**, geben Sie den Benutzer *SharePoint* in der Domäne *PRIV* ein, und klicken Sie auf dem nächsten Bildschirm des Assistenten auf **Diesen Benutzer als Administrator hinzufügen**.  
17. Schließen Sie die Systemsteuerung.  

### Ändern der IIS-Konfiguration
Es gibt zwei Möglichkeiten, die IIS-Konfiguration zu ändern, um es Anwendungen zu ermöglichen, die Windows-Authentifizierung zu nutzen. Stellen Sie sicher, dass Sie als MIMAdmin angemeldet sind, und führen Sie dann eine dieser Optionen durch.

Wenn Sie PowerShell verwenden möchten, gehen Sie folgendermaßen vor:
1.  Klicken Sie mit der rechten Maustaste auf „PowerShell“, und wählen Sie **Als Administrator ausführen** aus.  
2.  Beenden Sie IIS, und verwenden Sie die folgenden Befehle, um die Hosteinstellungen der Anwendung zu entsperren:  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```  

Wenn Sie einen Text-Editor, beispielsweise den Editor von Windows verwenden möchten, gehen Sie folgendermaßen vor:   
1. Öffnen Sie die Datei **C:\Windows\System32\inetsrv\config\applicationHost.config**.   
2. Scrollen Sie zu Zeile 82 dieser Datei. Der Wert des Tags **overrideModeDefault** sollte **<section name="windowsAuthentication" overrideModeDefault="Deny" />** lauten.  
3. Ändern Sie den Wert von **overrideModeDefault** in *Zulassen*.  
4. Speichern Sie die Datei, und starten Sie IIS mit diesem PowerShell-Befehl neu: `iisreset /START`

## Installieren von SQL Server
Wenn SQL Server sich nicht bereits in der geschützten Umgebung befindet, installieren Sie entweder SQL Server 2012 (Service Pack 1 oder höher) oder SQL Server 2014. Die folgenden Schritte gelten für SQL 2014.

1. Stellen Sie sicher, dass Sie als MIMAdmin angemeldet sind.
2. Klicken Sie mit der rechten Maustaste auf „PowerShell“, und wählen Sie **Als Administrator ausführen** aus.   
3. Navigieren Sie zu dem Verzeichnis, in dem sich das SQL Server-Setupprogramm befindet.  
4. Geben Sie folgenden Befehl ein:  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Installieren von SharePoint Foundation 2013

Installieren Sie mit dem SharePoint Foundation 2013 mit SP1-Installationsprogramm die Softwarekomponenten, die Voraussetzungen für SharePoint sind, auf PAMSRV.

> [!NOTE]
> Dieses Installationsprogramm erfordert eine Internetverbindung, um die erforderlichen Komponenten herunterzuladen. Nach der Installation wird der Server neu gestartet.

1. Klicken Sie mit der rechten Maustaste auf „PowerShell“, und wählen Sie **Als Administrator ausführen** aus.  
2. Wechseln Sie in das Verzeichnis, in dem SharePoint entpackt wurde.  
3. Geben Sie den Befehl `.\prerequisiteinstaller.exe` ein.

Nachdem die erforderlichen Komponenten für SharePoint installiert wurden, installieren Sie SharePoint Foundation 2013 mit SP1.

1.  Klicken Sie mit der rechten Maustaste auf „PowerShell“, und wählen Sie **Als Administrator ausführen** aus.  
2.  Wechseln Sie in das Verzeichnis, in dem SharePoint entpackt wurde.  
3.  Geben Sie den Befehl `.\setup.exe` ein.  
4.  Wählen Sie den Typ für einen **vollständigen Server** aus.  
5.  Nachdem die Installation abgeschlossen ist, wählen Sie die Option zum Ausführen des Assistenten aus.  

### Konfigurieren von SharePoint
Führen Sie den Konfigurations-Assistenten für SharePoint-Produkte aus, um SharePoint zu konfigurieren.

1.  Wechseln Sie auf der Registerkarte „Verbindung mit einer Serverfarm herstellen“ zu **Eine neue Serverfarm erstellen**.  
2.  Geben Sie **PAMSRV** als Datenbankserver für die Konfigurationsdatenbank und **PRIV\\SharePoint** als das Datenbankzugriffskonto an, das für SharePoint verwendet werden soll.  
3.  Geben Sie ein Kennwort als Passphrase der Farmsicherheit an (die Passphrase wird später nicht in dieser exemplarischen Vorgehensweise verwendet).  
4.  Akzeptieren Sie die weiteren Standardeinstellungen des SharePoint-Konfigurations-Assistenten, um eine Einzelserverfarm zu erstellen.    
5.  Wenn der Konfigurations-Assistent die Konfigurationsaufgabe 10 von 10 abgeschlossen hat, klicken Sie auf **Fertig stellen**. Daraufhin wird ein Webbrowser geöffnet.  
6.  Authentifizieren Sie sich im Popupfenster von Internet Explorer als Domänenadministrator (PRIV\MIMAdmin), um fortzufahren.  
7.  Starten Sie den Assistenten innerhalb der Webanwendung, um die SharePoint-Farm zu konfigurieren.  
8.  Wählen Sie die Option zum Verwenden des vorhandenen verwalteten Kontos (PRIV\\SharePoint) aus, deaktivieren Sie alle optionalen Dienste, und klicken Sie auf **Weiter**.  
9. Wenn das Fenster zum Erstellen einer Websitesammlung angezeigt wird, klicken Sie auf **Überspringen** und dann auf **Fertig stellen**.  

## Erstellen einer SharePoint Foundation 2013-Webanwendung
Nachdem Sie die Assistenten abgeschlossen haben, verwenden Sie PowerShell, um eine SharePoint Foundation 2013-Webanwendung zu erstellen, die als Host für das MIM-Portal fungiert. Da diese exemplarische Vorgehensweise zu Demonstrationszwecken dient, wird SSL nicht aktiviert.

1.  Klicken Sie mit der rechten Maustaste auf „SharePoint 2013-Verwaltungsshell“, wählen Sie **Als Administrator ausführen** aus, und führen Sie das folgende PowerShell-Skript aus:

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Es wird eine Warnmeldung angezeigt, dass die klassische Windows-Authentifizierungsmethode verwendet wird, und es kann mehrere Minuten dauern, bis der letzte Befehl abgeschlossen ist.  Nach Abschluss gibt die Ausgabe die URL des neuen Portals an.

> [!NOTE]
> Lassen Sie das Fenster „SharePoint 2013-Verwaltungsshell“ offen, da Sie es im nächsten Schritt benötigen.

## Erstellen einer SharePoint-Websitesammlung
Erstellen Sie nun eine SharePoint-Websitesammlung, die dieser Webanwendung zugeordnet ist, um das MIM-Portal zu hosten.

1.  Starten Sie die **SharePoint 2013-Verwaltungsshell**, sofern sie nicht bereits geöffnet ist, und führen Sie das folgende PowerShell-Skript aus:

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Vergewissern Sie sich, dass die Variable **CompatibilityLevel** auf *14* festgelegt ist. Wenn *15* zurückgegeben wird, wurde die Websitesammlung nicht für die 2010-Umgebungsversion erstellt. Löschen Sie die Websitesammlung, und erstellen Sie diese neu.

2.  Führen Sie den folgenden PowerShell-Befehl über **SharePoint 2013-Verwaltungsshell** aus. Dies deaktiviert den serverseitigen SharePoint-Ansichtszustand und die SharePoint-Aufgabe **Integritätsanalyseauftrag (Stündlich, Microsoft SharePoint Foundation-Timer, Alle Server)**.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## Ändern der Updateeinstellungen

1. Öffnen Sie die Systemsteuerung, navigieren Sie zu **Windows Update**, und klicken Sie auf **Einstellungen ändern**.  
2. Ändern Sie die Einstellungen so, dass Sie Updates für Windows und andere Produkte über Windows Updates erhalten.  
3. Suchen Sie nach neuen Updates, und stellen Sie sicher, dass alle ausstehenden wichtigen Updates installiert sind, bevor Sie fortfahren.

## Festlegen der Website als lokales Intranet

1. Starten Sie Internet Explorer, und öffnen Sie im Webbrowser eine Registerkarte.
2. Navigieren Sie zu http://pamsrv.priv.contoso.local:82/, und melden Sie sich als „PRIV\MIMAdmin“ an.  Eine leere SharePoint-Website namens „MIM-Portal“ wird angezeigt.  
3. Öffnen Sie dann in Internet Explorer das Dialogfeld **Internetoptionen**, wechseln Sie zur Registerkarte **Sicherheit**, wählen Sie **Lokales Intranet** aus, und fügen Sie die URL `http://pamsrv.priv.contoso.local:82/` hinzu.

Sollte bei der Anmeldung ein Fehler auftreten, müssen die Kerberos-SPNs, die Sie zuvor im [Schritt 2](step-2-prepare-priv-domain-controller.md) erstellt haben, möglicherweise aktualisiert werden.

## Starten des SharePoint-Verwaltungsdiensts

Starten Sie in den Verwaltungstools unter **Dienste** den Dienst **SharePoint-Administration**, wenn dieser nicht bereits ausgeführt wird.

In Schritt 4 installieren Sie die MIM-Komponenten auf dem PAM-Server.

>[!div class="step-by-step"]
[« Schritt 2](step-2-prepare-priv-domain-controller.md)
[Schritt 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Jul16_HO3-->


