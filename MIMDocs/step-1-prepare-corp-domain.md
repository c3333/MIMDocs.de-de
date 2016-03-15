---
Titel: Schritt 1: Vorbereiten die CORP-Domäne
MS.Custom: Na
MS.Reviewer: Na
MS.Suite: Na
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 4b524ae7-6610-40a0-8127-de5a08988a8a
Autor: Kgremban
---
# Schritt 1 – Vorbereiten der Domäne „CORP“
In diesem Schritt erstellen Sie einen Domänencontroller und eine Mitgliedsworkstation (Mitglied der Domäne/Arbeitsgruppe) in einer neuen Domäne in einer neuen Gesamtstruktur. Mit dieser Gesamtstruktur wird eine vorhandene Gesamtstruktur simuliert, die zu verwaltende Ressourcen hat. Die Ressource, die für dieses Szenario geschützt werden muss, ist eine Dateifreigabe.

## Vorbereiten von vier virtuellen Computern

Bereiten Sie vier virtuelle Computer mit separaten Laufwerken vor, die in einem gemeinsamen virtuellen Netzwerk miteinander verbunden sind. Diese virtuellen Computer können von Windows 8.1, Windows Server 2012 R2 oder anderen Betriebssystemplattformen gehostet werden. Das Laufwerk, auf dem die Datenträgerimages der virtuellen Computer gespeichert werden, muss mindestens 100 GB freien Speicherplatz haben, damit es alle virtuellen Computer aufnehmen kann.

### Installieren von Windows Server 2012 R2 oder Windows Server 2016

Installieren Sie auf einem virtuellen Computer Windows Server 2012 R2 oder Windows Server 2016 Technical Preview 3, um einen Computer namens „CORPDC“ zu erstellen.

1. Geben Sie „Windows Server 2012 R2 Standard (Server mit Benutzeroberfläche) x64“ oder „Windows Server 2016 Technical Preview 3 (Server mit Desktopdarstellung)“ an.

2. Lesen und akzeptieren Sie die Lizenzbedingungen.

3. Da dieser Datenträger leer sein wird, wählen Sie „Benutzerdefiniert: Nur Windows installieren“ aus, und verwenden Sie den nicht initialisierten Speicherplatz.

4. Melden Sie sich bei dem neuen Computer als Administrator an. Legen Sie dessen Namen in der **Systemsteuerung**auf „CORPDC“ fest, und weisen Sie ihm eine statische IP-Adresse im virtuellen Netzwerk zu. Starten Sie den Server neu.

5. Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an. Konfigurieren Sie den Computer mithilfe der Systemsteuerung, um nach Updates zu suchen und ggf. alle erforderlichen Updates zu installieren. Starten Sie den Server neu.

### Rollen hinzufügen

Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an. Mithilfe von PowerShell fügen Sie die Rollen „Active Directory-Domänendienste“, „DNS-Server“ und „Dateiserver“ (Teil des Abschnitts „Datei- und Speicherdienste“) hinzu, und stufen Sie diesen Server gemäß der folgenden Beschreibung zu einem Domänencontroller der neuen Gesamtstruktur „contoso.local“ hoch:

1. Starten Sie PowerShell, während Sie als Administrator angemeldet sind.

2. Geben Sie die folgenden Befehle ein.

   `import-module ServerManager`

    `Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools`

    `Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork`

  Dies führt zu der Aufforderung, ein Administratorkennwort für den abgesicherten Modus zu verwenden. Beachten Sie, dass Fehlermeldungen für die DNS-Delegierung und Kryptografieeinstellungen angezeigt werden. Diese sind normal.

3. Nachdem die Erstellung der Gesamtstruktur abgeschlossen ist, wird der Server automatisch neu gestartet.

4. Melden Sie sich nach dem Neustart des Servers bei „CORPDC“ als Administrator der Domäne an. Dies ist üblicherweise der Benutzer *CONTOSO\\Administrator*, der das Kennwort hat, das bei der Installation von Windows auf *CORPDC*angegeben wurde.

### Erstellen von neuen Benutzern und Gruppen

Erstellen Sie neue Benutzer und Gruppen, einschließlich einer Gruppe namens „CorpAdmins“, eines Benutzers namens "Jen" sowie einer Gruppe, die für Überwachungszwecke von Active Directory selbst benötigt wird. Der Name der Gruppe muss der NetBIOS-Domänenname gefolgt von drei Dollarzeichen sein: *CONTOSO$$$*. Der Gruppenbereich muss *Lokal (in Domäne)*sein, und der Gruppentyp muss *Sicherheit*sein. Dies ist notwendig, damit Gruppen aktiviert werden können, die in einem späteren Schritt in der Gesamtstruktur PRIV erstellt werden.

1. Starten Sie PowerShell.

2. Geben Sie die folgenden Befehle ein.

   `import-module activedirectory`

    `New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins`

    `New-ADUser –SamAccountName Jen –name Jen`

    `Add-ADGroupMember –identity CorpAdmins –Members Jen`

    `$jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force`

    `Set-ADAccountPassword –identity Jen –NewPassword $jp`

    `Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"`

    `New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'`

### Konfigurieren der Überwachung

1. Wechseln Sie zu „Start“ > „Verwaltung“ (oder unter Windows Server 2016 zu „Start“ > „Windows-Verwaltungsprogramme“), und starten Sie die Gruppenrichtlinienverwaltung.

2. Navigieren Sie zu „Gesamtstruktur“: „contoso.local“, „Domänen“, „contoso.local“, „Domänencontroller“, „Standarddomänencontroller-Richtlinie“. Es wird eine informierende Meldung angezeigt.

3. Klicken Sie mit der rechten Maustaste auf „Standarddomänencontroller-Richtlinie“, und wählen Sie im Kontextmenü den Befehl „Bearbeiten“ aus. Ein neues Fenster wird angezeigt.

4. Navigieren Sie im Fenster „Gruppenrichtlinienverwaltungs-Editor“ unter „Standarddomänencontroller-Richtlinie“ zu „Computerkonfiguration“ > „Richtlinien“ > „Windows-Einstellungen“ > „Sicherheitseinstellungen“ > „Lokale Richtlinien“ > „Überwachungsrichtlinie“, und erweitern Sie diese.

5. Klicken Sie im Detailbereich mit der rechten Maustaste auf „Kontenverwaltung überwachen“, und wählen Sie im Kontextmenü die Option „Eigenschaften“ aus. Klicken Sie auf „Diese Richtlinieneinstellungen definieren“, aktivieren Sie das Kontrollkästchen „Erfolg“, dann das Kontrollkästchen „Fehler“, und klicken Sie anschließend auf „Übernehmen“ und „OK“.

6. Klicken Sie im Detailbereich mit der rechten Maustaste auf „Verzeichnisdienstzugriff überwachen“, und wählen Sie im Kontextmenü den Befehl „Eigenschaften“ aus. Klicken Sie auf „Diese Richtlinieneinstellungen definieren“, aktivieren Sie das Kontrollkästchen „Erfolg“, dann das Kontrollkästchen „Fehler“, und klicken Sie anschließend auf „Übernehmen“ und „OK“.

7. Schließen Sie die Fenster „Gruppenrichtlinienverwaltungs-Editor“ und „Gruppenrichtlinienverwaltung“. Wenden Sie dann die Überwachungseinstellungen an, indem Sie ein PowerShell-Fenster starten und Folgendes eingeben:

    `gpupdate /force /target:computer`

  Die Meldung „Die Aktualisierung der Computerrichtlinie wurde erfolgreich abgeschlossen.“ sollte nach einigen Minuten angezeigt werden.

### Konfigurieren der Registrierungseinstellungen
Konfigurieren Sie die Registrierungseinträge, die für die Migration des SID-Verlaufs benötigt werden, der zum Erstellen der Gruppe für privilegierte Zugriffsverwaltung verwendet wird. Starten Sie den Domänencontroller dann neu.

1. Starten Sie PowerShell.

2. Geben Sie die folgenden Befehle ein, die die Quelldomäne konfigurieren, um RPC-Zugriff (Remote Procedure Call, Remoteprozeduraufruf) auf die SAM-Datenbank (Security Accounts Manager, Sicherheitskonto-Manager) zuzulassen.

   `New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1`

   `Restart-Computer`

Dadurch wird CORPDC neu gestartet. Weitere Informationen zu dieser Registrierungseinstellung finden Sie in [diesem Knowledge Base-Artikel](http://support.microsoft.com/kb/322970).

### Installieren von Windows 8.1 oder Windows 10

Installieren Sie Windows Server 8.1Enterprise oder Windows 10 Enterprise auf einem anderen neuen virtuellen Computer ohne installierte Software, um einen Computer namens *CORPWKSTN*zu erstellen.

1. Verwenden Sie „Express-Einstellungen“ während der Installation.

2. Beachten Sie, dass die Installation möglicherweise keine Verbindung mit dem Internet herstellen kann. Klicken Sie auf „Lokales Konto erstellen“. Geben Sie einen anderen Benutzernamen an. Verwenden Sie weder „Administrator“ noch „Jen“.

3. Geben Sie in der Systemsteuerung diesem Computer eine statische IP-Adresse im virtuellen Netzwerk, und legen Sie den bevorzugten DNS-Server der Schnittstelle auf denjenigen des Servers „CORPDC“ fest.

4. Binden Sie in der Systemsteuerung den Computer „CORPWKSTN“ in die Domäne „contoso.local“ ein. Dies erfordert, dass die Anmeldeinformationen des Contoso-Domänenadministrators bereitgestellt werden. Nachdem dies abgeschlossen ist, starten Sie den Computer „CORPWKSTN“ neu.

5. Klicken Sie nach dem Neustart des Computers auf das Symbol**Benutzer wechseln**, und klicken Sie dann auf**Anderer Benutzer**. Vergewissern Sie sich, dass sich der Benutzer „CONTOSO\\Jen“ bei „CORPWKSTN“ anmelden kann.

6. Erstellen Sie auf „CORPWKSTN“ einen neuen Ordner namens „CorpFS“ für die Gruppe „CorpAdmins“, und geben Sie diesen Ordner frei.

7. Geben Sie im **Startmenü**den Namen „PowerShell“ ein, und wählen Sie „Als Administrator ausführen“ aus.

8. Nachdem das Fenster geöffnet ist, geben Sie die folgenden Befehle ein.

   `mkdir c:\corpfs`

   `New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins`

   `$acl = Get-Acl c:\corpfs`

   `$car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")`

   `$acl.SetAccessRule($car)`

   `Set-Acl c:\corpfs $acl`
<!--HONumber=Mar16_HO1-->
