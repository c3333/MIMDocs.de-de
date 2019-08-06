---
title: Konfigurieren von Windows Server 2016 für MIM 2016 SP1 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die ersten Schritte und die Mindestanforderungen für die Vorbereitung von Windows Server 2016 für MIM 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7348507593426ba112feef9d68686ee493a6391d
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701411"
---
# <a name="set-up-an-identity-management-server-windows-server-2016"></a>Einrichten eines Identitätsverwaltungsservers: Windows Server 2016

> [!div class="step-by-step"]
> [« Vorbereiten einer Domäne](preparing-domain.md)
> [SQL Server 2016 »](prepare-server-sql2016.md)
> 
> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Name des Domänencontrollers: **corpdc**
> - Domänenname: **contoso**
> - Servername des MIM-Diensts: **corpservice**
> - Servername der MIM-Synchronisierung: **corpsync**
> - SQL-Servername: **corpsql**
> - Kennwort – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Hinzufügen von Windows Server 2016 zu Ihrer Domäne

Beginnen Sie mit einem Computer mit Windows Server 2016 mit mindestens 8-12 GB RAM. Geben Sie bei der Installation die Edition „Windows Server 2016 Standard/Datacenter (Server mit grafischer Benutzeroberfläche) x64“ an.

1. Melden Sie sich bei dem neuen Computer als Administrator an.

2. Geben Sie dem Computer mithilfe der Systemsteuerung eine statische IP-Adresse im Netzwerk. Konfigurieren Sie die Netzwerkschnittstelle so, dass DNS-Abfragen an die IP-Adresse des Domänencontrollers im vorherigen Schritt gesendet werden, und legen Sie den Computernamen auf **CORPSERVICE** fest.  Dieser Vorgang erfordert einen Neustart des Servers.

3. Öffnen Sie die Systemsteuerung, und fügen Sie den Computer der Domäne *contoso.com* hinzu, die Sie im letzten Schritt konfiguriert haben.  Dieser Vorgang beinhaltet das Angeben des Benutzernamens und der Anmeldeinformationen eines Domänenadministrators wie *Contoso\Administrator*.  Nachdem die Willkommensnachricht angezeigt wird, schließen Sie das Dialogfeld, und starten Sie den Server erneut neu.

4. Melden Sie sich beim Computer *CORPSERVICE* als Domänenkonto mit lokalen Administratorrechten an, z.B. als *Contoso\MIMINSTALL*.


5. Starten Sie ein PowerShell-Fenster als Administrator, und geben Sie den folgenden Befehl ein, um den Computer mit den Gruppenrichtlinieneinstellungen zu aktualisieren.

    ```
    gpupdate /force /target:computer
    ```

    Nach spätestens einer Minute wird der Vorgang mit der Meldung „Die Aktualisierung der Computerrichtlinie wurde erfolgreich abgeschlossen“ beendet.

6. Fügen Sie die Rollen **Webserver (IIS)** und **Anwendungsserver**, die **.NET Framework** 3.5-, 4.0- und 4.5-Features und das **Active Directory-Modul für Windows PowerShell** hinzu.

    ![Bild: PowerShell-Features](media/MIM-DeployWS2.png)

7. Geben Sie in PowerShell die folgenden Befehle ein: Möglicherweise ist es notwendig, dass Sie einen anderen Speicherort für die Quelldateien für **.NET Framework** 3.5-Features angeben. Diese Funktionen sind in der Regel nicht vorhanden, wenn Windows Server installiert wird. Sie sind aber auf dem Betriebssystem-Installationsdatenträger im Ordner „Sources“ im parallelen Ordner (SxS) verfügbar, beispielsweise „*D:\Sources\SxS\*“.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Konfigurieren der Serversicherheitsrichtlinie

Richten Sie die Serversicherheitsrichtlinien ein, damit die neu erstellten Konten als Dienste ausgeführt werden können.
> [!NOTE] 
> Je nach Konfiguration – Einzelserver (All-In-One) oder verteilter Server – müssen Sie sich beim Hinzufügen neuer Konten lediglich an der Rolle des Mitgliedgeräts (z.B. Synchronisierungsserver) orientieren. 

1. Starten Sie das Programm „Lokale Sicherheitsrichtlinie“.

2. Navigieren Sie zu **Lokale Richtlinien > Zuweisen von Benutzerrechten**.

3. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Anmelden als Dienst**, und wählen Sie dann **Eigenschaften** aus.

    ![Bild: Lokale Sicherheitsrichtlinien](media/MIM-DeployWS3.png)

4. Klicken Sie auf **Add User or Group** (Benutzer oder Gruppe hinzufügen), und geben Sie im Textfeld basierend auf der Rolle `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR` ein. Klicken Sie dann auf **Namen überprüfen** und anschließend auf **OK**.

5. Klicken Sie auf **OK**, um das Eigenschaftenfenster **Anmelden als Dienst** zu schließen.

6.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **Zugriff vom Netzwerk auf diesen Computer verweigern**, und wählen Sie **Eigenschaften** aus.

[!NOTE] Durch das Trennen der Rollenserver funktionieren einige Funktionen wie SSPR nicht mehr ordnungsgemäß.

7. Klicken Sie zunächst auf **Benutzer oder Gruppe hinzufügen**, geben Sie in das Textfeld `contoso\MIMSync; contoso\MIMService` ein, und klicken Sie auf **OK**.

8. Klicken Sie auf **OK**, um das Eigenschaftenfenster **Zugriff vom Netzwerk auf diesen Computer verweigern** zu schließen.

9. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Lokal anmelden verweigern**, und wählen Sie dann **Eigenschaften** aus.

10. Klicken Sie zunächst auf **Benutzer oder Gruppe hinzufügen**, geben Sie in das Textfeld `contoso\MIMSync; contoso\MIMService` ein, und klicken Sie auf **OK**.

11. Klicken Sie auf **OK**, um das Eigenschaftenfenster **Lokal anmelden verweigern** zu schließen.

12. Schließen Sie das Fenster „Lokale Sicherheitsrichtlinien“.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Ändern Sie falls notwendig den IIS Windows-Authentifizierungsmodus.

1.  Öffnen Sie ein PowerShell-Fenster.

2.  Beenden Sie IIS mit dem Befehl *iisreset/STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [« Vorbereiten einer Domäne](preparing-domain.md)
> [SQL Server 2016 »](prepare-server-sql2016.md)
