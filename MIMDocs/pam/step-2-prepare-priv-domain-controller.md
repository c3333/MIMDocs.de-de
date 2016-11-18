---
title: "Bereitstellen von PAM – Schritt 2: PRIV-Domänencontroller | Microsoft Docs"
description: "Bereiten Sie den PRIV-Domänencontroller vor, der die geschützte Umgebung bereitstellt, in der Privileged Access Management isoliert wird."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f84229908f31242b6d2f7636a7c67ca669de45b3


---

# <a name="step-2-prepare-the-first-priv-domain-controller"></a>Schritt 2: Vorbereiten des ersten PRIV-Domänencontrollers

>[!div class="step-by-step"]
[« Schritt 1](step-1-prepare-corp-domain.md)
[Schritt 3 »](step-3-prepare-pam-server.md)

In diesem Schritt erstellen Sie eine neue Domäne, die die geschützte Umgebung für die Administratorauthentifizierung bereitstellt.  Diese Gesamtstruktur benötigt mindestens einen Domänencontroller und mindestens einen Mitgliedsserver. Der Mitgliedsserver wird im nächsten Schritt konfiguriert.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>Erstellen eines neuen Privileged Access Management-Domänencontrollers

In diesem Abschnitt richten Sie einen virtuellen Computer ein, der als Domänencontroller für eine neue Gesamtstruktur fungiert.

### <a name="install-windows-server-2012-r2"></a>Installieren von Windows Server 2012 R2
Installieren Sie Windows Server 2012 R2 auf einem anderen neuen virtuellen Computer ohne installierte Software, um den Computer „PRIVDC“ zu erstellen.

1. Wählen Sie die benutzerdefinierte Installation von Windows Server (kein Upgrade). Geben Sie bei der Installation **Windows Server 2012 R2 Standard (Server mit grafischer Benutzeroberfläche) x64** an. _Wählen Sie nicht_ **Datacenter oder Server Core** aus.

2. Lesen und akzeptieren Sie die Lizenzbedingungen.

3. Da dieser Datenträger leer sein wird, wählen Sie **Benutzerdefiniert: Nur Windows installieren** aus, und verwenden Sie den nicht initialisierten Speicherplatz.

4. Nach der Installation der Betriebssystemversion melden Sie sich als neuer Administrator bei diesem neuen Computer an. Verwenden Sie die Systemsteuerung, um den Computernamen *PRIVDC* festzulegen und dem Computer im virtuellen Netzwerk eine statische IP-Adresse zuzuweisen. Konfigurieren Sie den DNS-Server dann so, dass der im vorherigen Schritt installierte Domänencontroller verwendet wird. Dies erfordert einen Neustart des Servers.

5. Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an. Konfigurieren Sie den Computer mithilfe der Systemsteuerung, um nach Updates zu suchen und ggf. alle erforderlichen Updates zu installieren. Dies erfordert möglicherweise einen Neustart des Servers.

### <a name="add-roles"></a>Hinzufügen von Rollen
Fügen Sie die Rollen „Active Directory-Domänendienste (AD DS)“ und „DNS-Server“ hinzu.

1. Starten Sie PowerShell als Administrator.

2. Geben Sie die folgenden Befehle ein, um eine Windows Server Active Directory-Installation vorzubereiten.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>Konfigurieren der Registrierungseinstellungen für die Migration des SID-Verlaufs

Starten Sie PowerShell, und geben Sie die folgenden Befehle ein, um die Quelldomäne so zu konfigurieren, dass RPC-Zugriff (Remote Procedure Call, Remoteprozeduraufruf) auf die SAM-Datenbank (Security Accounts Manager Sicherheitskontenverwaltung) zugelassen wird.

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>Erstellen einer neuen Privileged Access Management-Gesamtstruktur

Als Nächstes stufen Sie den Server zum Domänencontroller in einer neuen Gesamtstruktur höher.

In diesem Dokument wird der Name „priv.contoso.local“ als Domänenname der neuen Gesamtstruktur verwendet.  Der Name der Gesamtstruktur ist nicht wichtig, und er muss keinem Namen einer vorhandenen Gesamtstruktur in der Organisation untergeordnet werden. Sowohl der Domänen- als auch der NetBIOS-Name der neuen Gesamtstruktur muss jedoch gegenüber den anderen Domänen in der Organisation eindeutig sein.  

### <a name="create-a-domain-and-forest"></a>Erstellen einer Domäne und Gesamtstruktur

1. Geben Sie in einem PowerShell-Fenster folgende Befehle ein, um die neue Domäne zu erstellen.  Dadurch wird eine DNS-Delegierung in einer übergeordneten Domäne (contoso.local) erstellt, die in einem vorherigen Schritt erstellt wurde.  Wenn Sie DNS später konfigurieren möchten, lassen Sie die `CreateDNSDelegation -DNSDelegationCredential $ca`-Parameter aus.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Wenn das Popupfenster angezeigt wird, stellen Sie die Anmeldeinformationen für den Administrator der CORP-Gesamtstruktur bereit (z. B. den Benutzernamen „CONTOSO\\Administrator“ und das entsprechende Kennwort aus Schritt 1).

3. Im PowerShell-Fenster werden Sie nach einem Administratorkennwort für den abgesicherten Modus gefragt. Geben Sie ein neues Kennwort ein (zweimal). Es werden Fehlermeldungen für die DNS-Delegierungs- und Kryptografieeinstellungen angezeigt. Diese sind normal.

Nachdem die Erstellung der Gesamtstruktur abgeschlossen ist, wird der Server automatisch neu gestartet.

### <a name="create-user-and-service-accounts"></a>Erstellen von Benutzer- und Dienstkonten
Erstellen Sie die Benutzer- und Dienstkonten für die Einrichtung von MIM-Dienst und -Portal. Diese Konten werden im Benutzercontainer der Domäne „priv.contoso.local“ gespeichert.

1. Melden Sie sich nach dem Neustart des Servers als Domänenadministrator („PRIV\\Administrator“) bei PRIVDC an.

2. Starten Sie PowerShell, und geben Sie die folgenden Befehle ein. Das Kennwort 'Pass@word1' ist nur ein Beispiel. Sie sollten für die Konten andere Kennwörter verwenden.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### <a name="configure-auditing-and-logon-rights"></a>Konfigurieren der Berechtigungen für die Überwachung und Anmeldung

Sie müssen die Überwachung einrichten, damit die PAM-Konfiguration in den Gesamtstrukturen eingerichtet werden kann.  

1. Stellen Sie sicher, dass Sie als Domänenadministrator (PRIV\\Administrator) angemeldet sind.

2. Wechseln Sie zu **Start** > **Verwaltung** > **Gruppenrichtlinienverwaltung**.

3. Navigieren Sie zu **Gesamtstruktur: priv.contoso.local** > **Domänen** > **priv.contoso.local** > **Domänencontroller** > **Standarddomänencontroller-Richtlinie**. Beachten Sie, dass eine Warnmeldung angezeigt wird.

4. Klicken Sie mit der rechten Maustaste auf **Standarddomänencontroller-Richtlinie**, und wählen Sie **Bearbeiten** aus.

5. Navigieren Sie in der Konsolenstruktur des Gruppenrichtlinienverwaltungs-Editors zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Lokale Richtlinien** > **Überwachungsrichtlinie**.

6. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Kontenverwaltung überwachen**, und wählen Sie **Eigenschaften** aus. Klicken Sie auf **Diese Richtlinieneinstellungen definieren**, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler** , und klicken Sie auf **Übernehmen** und dann auf **OK**.

7. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Verzeichnisdienstzugriff überwachen**, und wählen Sie **Eigenschaften** aus. Klicken Sie auf **Diese Richtlinieneinstellungen definieren**, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler** , und klicken Sie auf **Übernehmen** und dann auf **OK**.

8. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Kontorichtlinien** > **Kerberos-Richtlinie**.

9. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Max. Gültigkeitsdauer des Benutzertickets**, und wählen Sie **Eigenschaften** aus. Klicken Sie auf **Diese Richtlinieneinstellungen definieren**, legen Sie die Anzahl der Stunden auf *1*fest, klicken Sie auf **Übernehmen** und dann auf **OK**. Beachten Sie, dass auch andere Einstellungen im Fenster geändert werden.

10. Wählen Sie im Fenster „Gruppenrichtlinienverwaltung“ die Option **Standarddomänenrichtlinie** aus, und klicken Sie mit der rechten Maustaste auf **Bearbeiten**.

11. Erweitern Sie **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Lokale Richtlinien**, und wählen Sie dann **Zuweisen von Benutzerrechten** aus.

12. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Anmelden als Batchauftrag verweigern**, und wählen Sie dann **Eigenschaften** aus.

13. Aktivieren Sie das Kontrollkästchen **Diese Richtlinieneinstellungen definieren**, klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und geben Sie im Feld „Benutzer- und Gruppennamen“ die Zeichenfolge *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* ein. Klicken Sie dann auf **OK**.

14. Klicken Sie auf **OK**, um das Fenster zu schließen.

15. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Anmelden über Remotedesktopdienste verweigern**, und wählen Sie dann **Eigenschaften** aus.

16. Aktivieren Sie das Kontrollkästchen **Diese Richtlinieneinstellungen definieren**, klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und geben Sie im Feld „Benutzer- und Gruppennamen“ die Zeichenfolge *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* ein. Klicken Sie dann auf **OK**.

17. Klicken Sie auf **OK**, um das Fenster zu schließen.

18. Schließen Sie die Fenster „Gruppenrichtlinienverwaltungs-Editor“ und „Gruppenrichtlinienverwaltung“.

19. Starten Sie ein PowerShell-Fenster als Administrator, und geben Sie den folgenden Befehl ein, um den Domänencontroller mit den Gruppenrichtlinieneinstellungen zu aktualisieren.

  ```
  gpupdate /force /target:computer
  ```

  Nach einer Minute wird der Vorgang mit der Meldung beendet, dass die Aktualisierung der Computerrichtlinie erfolgreich abgeschlossen wurde.


### <a name="configure-dns-name-forwarding-on-privdc"></a>Konfigurieren der Weiterleitung von DNS-Namen auf „PRIVDC“

Verwenden Sie PowerShell auf PRIVDC, um die DNS-Namensweiterleitung zu konfigurieren, sodass die PRIV-Domäne andere vorhandene Gesamtstrukturen erkennen kann.

1. Starten Sie PowerShell.

2. Geben Sie für jede Domäne auf oberster Ebene jeder vorhandenen Gesamtstruktur den folgenden Befehl ein, und geben Sie dabei die vorhandene DNS-Domäne (z. B. contoso.local) sowie die IP-Adresse des Masterservers dieser Domäne an.  

  Wenn Sie im vorherigen Schritt eine Domäne „contoso.local“ erstellt haben, geben Sie *10.1.1.31* als virtuelle IP-Netzwerkadresse des CORPDC-Computers an.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> Die anderen Gesamtstrukturen müssen DNS-Abfragen für die PRIV-Gesamtstruktur an diesen Domänencontroller weiterleiten können.  Wenn Sie über mehrere Active Directory-Gesamtstrukturen verfügen, müssen Sie jeder dieser Gesamtstrukturen auch eine bedingte DNS-Weiterleitung hinzufügen.

### <a name="configure-kerberos"></a>Konfigurieren von Kerberos

1. Fügen Sie mithilfe von PowerShell SPNs hinzu, sodass SharePoint, die PAM-REST-API und der MIM-Dienst die Kerberos-Authentifizierung verwenden können.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> Die nächsten Schritte in diesem Dokument beschreiben die Installation von MIM 2016-Serverkomponenten auf einem einzelnen Computer. Wenn Sie planen, zur Sicherstellung einer hohen Verfügbarkeit einen weiteren Server hinzuzufügen, müssen Sie zusätzliche Kerberos-Einstellungen konfigurieren, wie unter [FIM 2010: Kerberos Authentication Setup](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx) (FIM 2010: Einrichten der Kerberos-Authentifizierung) beschrieben.

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>Konfigurieren der Delegierung, um MIM-Dienstkonten Zugriff zu gewähren

Führen Sie die folgenden Schritte auf PRIVDC als Domänenadministrator aus.

1. Starten Sie **Active Directory-Benutzer und -Computer**.  
2. Klicken Sie mit der rechten Maustaste auf die Domäne **priv.contoso.local**, und wählen Sie **Objektverwaltung zuweisen** aus.  
3. Klicken Sie auf der Registerkarte „Ausgewählte Benutzer und Gruppen“ auf **Hinzufügen**.  
4. Geben Sie im Fenster zur Auswahl von Benutzern, Computern oder Gruppen die Zeichenfolge *mimcomponent; mimmonitor; mimservice* ein, und klicken Sie dann auf **Namen überprüfen**. Nachdem die Namen unterstrichen sind, klicken Sie auf **OK** und dann auf **Weiter**.  
5. Wählen Sie in der Liste der allgemeinen Aufgaben die Optionen **Erstellt, entfernt und verwaltet Benutzerkonten** und **Ändert die Mitgliedschaft einer Gruppe** aus, und klicken Sie dann auf **Weiter** und **Fertig stellen**.

6. Klicken Sie erneut mit der rechten Maustaste auf die Domäne **priv.contoso.local**, und wählen Sie **Objektverwaltung zuweisen** aus.  
7. Klicken Sie auf der Registerkarte „Ausgewählte Benutzer und Gruppen“ auf **Hinzufügen**.  
8. Geben Sie im Fenster zur Auswahl von Benutzern, Computern oder Gruppen die Zeichenfolge *MIMAdmin* ein, und klicken Sie dann auf **Namen überprüfen**. Nachdem die Namen unterstrichen sind, klicken Sie auf **OK** und dann auf **Weiter**.  
9. Wählen Sie **Benutzerdefinierter Task**gilt für **Diesen Ordner**und **Allgemeine Berechtigungen**.    
10. Wählen Sie in der Liste mit den Berechtigungen Folgendes aus:  
  - **Lesen**  
  - **Schreiben**  
  - **Alle untergeordneten Objekte erstellen**  
  - **Alle untergeordneten Objekte löschen**  
  - **Alle Eigenschaften lesen**  
  - **Alle Eigenschaften schreiben**  
  - **SID-Verlauf migrieren**  
  Klicken Sie auf **Weiter** und dann auf **Fertig stellen**.

11. Klicken Sie erneut mit der rechten Maustaste auf die Domäne **priv.contoso.local**, und wählen Sie **Objektverwaltung zuweisen** aus.  
12. Klicken Sie auf der Registerkarte „Ausgewählte Benutzer und Gruppen“ auf **Hinzufügen**.  
13. Geben Sie im Fenster zur Auswahl von Benutzern, Computern oder Gruppen die Zeichenfolge *MIMAdmin* ein, und klicken Sie dann auf **Namen überprüfen**. Nachdem die Namen unterstrichen sind, klicken Sie auf **OK**und dann auf **Weiter**.  
14. Wählen Sie **Benutzerdefinierter Task**gilt für **Diesen Ordner**aus, und klicken Sie dann auf **Nur Benutzerobjekte**.    
15. Wählen Sie in der Liste der Berechtigungen **Kennwort ändern** und **Kennwort zurücksetzen**aus. Klicken Sie anschließend auf **Weiter** und dann auf **Fertig stellen**.  
16. Schließen Sie %%amp;quot;Active Directory-Benutzer und -Computer%%amp;quot;.

17. Öffnen Sie eine Eingabeaufforderung.  
18. Überprüfen Sie die Zugriffssteuerungsliste im Objekt „AdminSDHolder“ in den PRIV-Domänen. Wenn Ihre Domäne beispielsweise „priv.contoso.local“ lautet, geben Sie folgenden Befehl ein:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19. Aktualisieren Sie die Zugriffssteuerungsliste nach Bedarf, um sicherzustellen, dass der MIM-Dienst und der MIM-Komponentendienst Mitgliedschaften von Gruppen aktualisieren können, die von dieser Zugriffssteuerungsliste geschützt werden.  Geben Sie den folgenden Befehl ein:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. Starten Sie den PRIVDC-Server neu, damit diese Änderung wirksam werden.

## <a name="prepare-a-priv-workstation"></a>Vorbereiten einer PRIV-Arbeitsstation

Wenn Sie nicht bereits über einen Arbeitsstationcomputer verfügen, der in die PRIV-Domäne eingebunden werden kann, um Wartungsmaßnahmen für PRIV-Ressourcen (wie z. B. MIM) auszuführen, befolgen Sie diese Anweisungen zur Vorbereitung einer Arbeitsstation.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Installieren von Windows 8.1 oder Windows 10 Enterprise

Installieren Sie Windows Server 8.1 Enterprise oder Windows 10 Enterprise auf einem anderen neuen virtuellen Computer ohne installierte Software, um einen Computer namens *PRIVWKSTN* zu erstellen.

1. Verwenden Sie „Express-Einstellungen“ während der Installation.

2. Beachten Sie, dass die Installation möglicherweise keine Verbindung mit dem Internet herstellen kann. Klicken Sie auf **Lokales Konto erstellen**. Geben Sie einen anderen Benutzernamen an. Verwenden Sie weder „Administrator“ noch „Jen“.

3. Weisen Sie in der Systemsteuerung diesem Computer eine statische IP-Adresse im virtuellen Netzwerk zu, und legen Sie den bevorzugten DNS-Server der Schnittstelle auf denjenigen des PRIVDC-Servers fest.

4. Binden Sie in der Systemsteuerung den Computer „PRIVWKSTN“ in die Domäne „priv.contoso.local“ ein. Dies erfordert die Bereitstellung der Anmeldeinformationen des PRIV-Domänenadministrators. Nachdem dies abgeschlossen ist, starten Sie den Computer „PRIVWKSTN“ neu.

Weitere Details finden Sie unter [Securing privileged access workstations](https://technet.microsoft.com/en-us/library/mt634654.aspx) (Sichern von Arbeitsstationen mit privilegiertem Zugriff).

Im nächsten Schritt bereiten Sie einen PAM-Server vor.

>[!div class="step-by-step"]
[« Schritt 1](step-1-prepare-corp-domain.md)
[Schritt 3 »](step-3-prepare-pam-server.md)



<!--HONumber=Nov16_HO2-->


