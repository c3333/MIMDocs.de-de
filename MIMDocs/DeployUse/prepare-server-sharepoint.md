---
# required metadata

title: Einrichten eines Identitätsverwaltungsservers&#58; SharePoint | Microsoft Identity Manager
description: Installieren und konfigurieren Sie SharePoint Foundation, sodass es die MIM-Portalseite hosten kann. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Einrichten eines Identitätsverwaltungsservers: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> In allen folgenden Beispielen stellt **mimservername** den Namen Ihres Domänencontrollers dar, während **contoso** Ihren Domänennamen und **Pass@word1** ein Beispielkennwort darstellen.


## Installieren Sie **SharePoint Foundation 2013 mit SP1**

> [!NOTE]
> Damit das Installationsprogramm die erforderlichen Komponenten herunterladen kann, ist eine Internetverbindung erforderlich.

Der Server wird am Ende der Installation neu gestartet.

1.  Starten Sie **PowerShell** als Domänenadministrator.

    -   Wechseln Sie in das Verzeichnis, in dem SharePoint entpackt wurde.

    -   Geben Sie folgenden Befehl ein:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Nachdem die erforderlichen Komponenten für **SharePoint** installiert sind, installieren Sie **SharePoint Foundation 2013 mit SP1** , indem Sie den folgenden Befehl eingeben:

    ```
    .\setup.exe
    ```

3.  Wählen Sie den Typ für einen vollständigen Server aus.

4.  Nachdem die Installation abgeschlossen ist, führen Sie den Assistenten aus.

## Führen Sie den Assistenten aus, um SharePoint zu konfigurieren

Folgen Sie den im **Konfigurations-Assistenten für SharePoint-Produkte** erläuterten Schritten, um SharePoint für die Arbeit mit MIM zu konfigurieren.

1. Wechseln Sie auf die Registerkarte **Verbindung mit einer Serverfarm herstellen** , um eine neue Serverfarm zu erstellen.

2. Geben Sie diesen Server als Datenbankserver für die Konfigurationsdatenbank und *Contoso\SharePoint* als Datenbankzugriffskonto für SharePoint an.

3. Geben Sie ein Kennwort als Passphrase der Farmsicherheit an (die Passphrase wird später nicht in dieser Testumgebung verwendet).

4. Wenn der Konfigurations-Assistent die Konfigurationsaufgabe 10 von 10 abgeschlossen hat, klicken Sie auf „Fertig stellen“. Daraufhin wird ein Webbrowser geöffnet.

5. Authentifizieren Sie sich im Internet Explorer-Popup als *Contoso\Administrator* (oder mit dem entsprechenden Domänenadministratorkonto), um den Vorgang fortzusetzen.

6. Starten Sie den Assistenten (innerhalb der Webanwendung), um die SharePoint-Farm zu konfigurieren.

7. Wählen Sie die Option zum Verwenden des vorhandenen verwalteten Kontos (*Contoso\SharePoint*) aus, und klicken Sie auf **Weiter**.

8. Klicken Sie im Fenster zum **Erstellen einer Websitesammlung** auf **Überspringen**.  Klicken Sie dann auf **Fertig stellen**.

## Vorbereiten von SharePoint zum Hosten des MIM-Portals

1. Erstellen Sie eine **SharePoint Foundation 2013-Webanwendung**.

    > [!NOTE]
    > Zunächst wird SSL nicht konfiguriert. Achten Sie darauf, dass Sie SSL oder ähnliches konfigurieren, bevor Sie den Zugriff auf dieses Portal ermöglichen.

    1. Starten Sie  **SharePoint 2013-Verwaltungsshell** , und führen Sie das folgende PowerShell-Skript aus:

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
        -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
        ```

        2. Es wird eine Warnmeldung angezeigt, dass die klassische Windows-Authentifizierungsmethode verwendet wird, und es kann mehrere Minuten dauern, bis der letzte Befehl abgeschlossen ist.  Nach Abschluss gibt die Ausgabe die URL des neuen Portals an.  Belassen Sie das Fenster für die **SharePoint 2013-Verwaltungsshell** geöffnet, denn es wird in einer späteren Aufgabe benötigt.

2. Erstellen Sie eine **SharePoint-Websitesammlung**, die dieser Webanwendung zugeordnet ist.

    1. Starten Sie SharePoint 2013-Verwaltungsshell, und führen Sie das folgende PowerShell-Skript aus:

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://corpidm.contoso.local:82
        New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
        -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```

        2. Vergewissern Sie sich, dass die Variable *CompatibilityLevel* das Ergebnis „14“ hat.  (Weitere Informationen finden Sie unter [Installieren von FIM 2010 R2 in SharePoint Foundation 2013](http://technet.microsoft.com/library/jj863242.aspx).) Ist das Ergebnis gleich „15“, wurde die Websitesammlung nicht für die 2010-Umgebungsversion erstellt. Löschen Sie die Websitesammlung, und erstellen Sie diese neu.

3. Deaktivieren Sie den **serverseitigen SharePoint-Ansichtszustand** und die SharePoint-Aufgabe „Integritätsanalyseauftrag (Stündlich, Microsoft SharePoint Foundation-Timer, Alle Server)“, indem Sie die folgenden PowerShell-Befehle in der **SharePoint 2013-Verwaltungsshell** ausführen:

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

4. Öffnen Sie auf Ihrem Identitätsverwaltungsserver eine neue Registerkarte des Webbrowsers, navigieren Sie zu „http://localhost:82/“, und melden Sie sich als *contoso\Administrator* an.  Eine leere SharePoint-Website namens *MIM-Portal* wird angezeigt.

    ![Bild: MIM-Portal unter „http://localhost:82/“](media/MIM-DeploySP1.png)

5. Kopieren Sie die URL, und öffnen Sie in Internet Explorer das Dialogfeld **Internetoptionen**. Wechseln Sie zur Registerkarte **Sicherheit**, wählen Sie **Lokales Intranet** aus, und klicken Sie auf **Websites**.

    ![Bild: Internetoptionen](media/MIM-DeploySP2.png)

6. Klicken Sie im Fenster **Lokales Intranet** auf **Erweitert**, und fügen Sie die kopierte URL in das Textfeld **Diese Website zur Zone hinzufügen** ein. Klicken Sie auf **Hinzufügen**, und schließen Sie die Fenster.

7. Öffnen Sie das Programm **Verwaltungstools**, und navigieren Sie zu **Dienste**. Suchen Sie den SharePoint-Verwaltungsdienst, und starten Sie ihn, sofern dieser nicht bereits ausgeführt wird.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)


<!--HONumber=Apr16_HO2-->


