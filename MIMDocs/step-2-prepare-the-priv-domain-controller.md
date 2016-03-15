---
Titel: Schritt 2: Vorbereiten des PRIV-Domänencontrollers
MS.Custom: Na
MS.Reviewer: Na
MS.Suite: Na
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 0e9993a0-b8ae-40e2-8228-040256adb7e2
Autor: Kgremban
---
# Schritt 2: Vorbereiten des PRIV-Domänencontrollers
## Erstellen einer neuen Privileged Access Management-Gesamtstruktur

In diesem Schritt erstellen Sie eine neue Domäne für eine neue privilegierte Zugriffsverwaltungsgesamtstruktur.

### Installieren von Windows Server 2012 R2
Installieren Sie auf einem anderen neuen virtuellen Computer ohne installierte Software Windows Server 2012 R2, um einen "PRIVDC".

1. Wählen Sie die benutzerdefinierte Installation von Windows Server (kein Upgrade). Geben Sie bei der Installation *Windows Server 2012 R2 Standard (Server mit grafischer Benutzeroberfläche) x64*an. Wählen Sie weder **Datacenter** noch *Server Core*aus.

2. Lesen und akzeptieren Sie die Lizenzbedingungen.

3. Da dieser Datenträger leer sein wird, wählen Sie *Benutzerdefiniert: Nur Windows installieren* aus, und nutzen Sie den nicht initialisierten Speicherplatz.

4. Nach der Installation der Betriebssystemversion melden Sie sich auf dem neuen Computer als neuer Administrator an. Verwenden Sie die Systemsteuerung, um den Computernamen „PRIVDC“ festzulegen. Weisen Sie ihm dann im virtuellen Netzwerk eine statische IP-Adresse zu. Anschließend konfigurieren Sie den DNS-Server, damit hierfür der im vorherigen Schritt installierte Domänencontroller verwendet wird. Dies erfordert einen Neustart des Servers.

5. Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an. Konfigurieren Sie den Computer mithilfe der Systemsteuerung, um nach Updates zu suchen und ggf. alle erforderlichen Updates zu installieren. Dies erfordert möglicherweise einen Neustart des Servers.

### Hinzufügen von Rollen
Fügen Sie die Rollen für die Active Directory-Domänendienste (AD DS) und den DNS-Server hinzu, und stufen Sie den Server zum Domänencontroller einer neuen Gesamtstruktur herauf.

**HINWEIS:** In diesem Dokument, wird der Name *priv.contoso.local* als Domänenname der neuen Gesamtstruktur verwendet.  Der Name der Gesamtstruktur ist nicht wichtig, und er muss keinem Namen einer vorhandenen Gesamtstruktur in der Organisation untergeordnet werden. Sowohl der Domänen- als auch der NetBIOS-Name der neuen Gesamtstruktur muss jedoch gegenüber den anderen Domänen in der Organisation eindeutig sein.

1. Starten Sie PowerShell, während Sie als Administrator angemeldet sind.
2. Geben Sie die folgenden Befehle ein.

   `import-module ServerManager`

    `Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools`

   `$ca= get-credential`

   `Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca`

3. Wenn das Popupfenster angezeigt wird, stellen Sie die Anmeldeinformationen für den Administrator der CORP-Gesamtstruktur bereit (z. B. den Benutzernamen „CONTOSO\\Administrator“ und das entsprechende Kennwort aus Schritt 1). Dies fordert dann im PowerShell-Fenster die Verwendung eines Administratorkennworts im abgesicherten Modus an. Geben Sie das neue Kennwort zweimal ein. Beachten Sie, dass Fehlermeldungen für die DNS-Delegierung und Kryptografieeinstellungen angezeigt werden. Diese sind normal.

Nachdem die Erstellung der Gesamtstruktur abgeschlossen ist, wird der Server automatisch neu gestartet.

### Erstellen von Benutzer- und Dienstkonten
Erstellen Sie die Benutzer- und Dienstkonten, die während der Einrichtung des MIM-Diensts und des Portals erforderlich sind, im Container „Benutzer“ der Domäne **priv.contoso.local** .

1. Melden Sie sich nach dem Neustart auf „PRIVDC“ als Domänenadministrator („PRIV\\Administrator“) an.

2. Geben Sie die folgenden Befehle ein.

    `import-module activedirectory`

    `$sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force`

    `New-ADUser –SamAccountName MIMMA –name MIMMA`

    `Set-ADAccountPassword –identity MIMMA –NewPassword $sp`

    `Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor`

    `Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp`

    `Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1`

    `New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent`

    `Set-ADAccountPassword –identity MIMComponent –NewPassword $sp`

   `Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1`

    `New-ADUser –SamAccountName MIMSync –name MIMSync`

    `Set-ADAccountPassword –identity MIMSync –NewPassword $sp`

    `Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName MIMService –name MIMService`

   `Set-ADAccountPassword –identity MIMService –NewPassword $sp`

    `Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName SharePoint –name SharePoint`

   `Set-ADAccountPassword –identity SharePoint –NewPassword $sp`

   `Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName SqlServer –name SqlServer`

   `Set-ADAccountPassword –identity SqlServer –NewPassword $sp`

   `Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName BackupAdmin –name BackupAdmin`

   `Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp`

   `Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1`

   `New-ADUser -SamAccountName MIMAdmin -name MIMAdmin`

   `Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp`

   `Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1`

   `Add-ADGroupMember "Domain Admins" SharePoint`

    `Add-ADGroupMember "Domain Admins" MIMService`

### Konfigurieren der Berechtigungen für die Überwachung und Anmeldung

1. Stellen Sie sicher, dass Sie als Domänenadministrator angemeldet sind (z. B. als „PRIV\\Administrator“).

2. Wechseln Sie zu **Start** » **Verwaltung** » **Gruppenrichtlinienverwaltung**.

3. Navigieren Sie zur Gesamtstruktur: *priv.contoso.local* » **Domänen** » *priv.contoso.local* » **Domänencontroller** » **Standarddomänencontroller-Richtlinie**. Beachten Sie, dass eine Warnmeldung angezeigt wird.

4. Klicken Sie mit der rechten Maustaste auf **Standarddomänencontroller-Richtlinie** , und wählen Sie dann im Kontextmenü den Befehl **Bearbeiten** aus.

5. Navigieren Sie in der Konsolenstruktur des **Gruppenrichtlinienverwaltungs-Editors** zu **Computerkonfiguration** » **Richtlinien** » **Windows-Einstellungen** » **Sicherheitseinstellungen** » **Lokale Richtlinien** » **Überwachungsrichtlinie**.

6. Klicken Sie im **Detailbereich** mit der rechten Maustaste auf **Kontenverwaltung überwachen** , und wählen Sie im Kontextmenü den Befehl **Eigenschaften** aus. Klicken Sie auf **Diese Richtlinieneinstellungen definieren**, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler** , und klicken Sie auf **Übernehmen** und dann auf **OK**.

7. Klicken Sie im **Detailbereich** mit der rechten Maustaste auf **Verzeichnisdienstzugriff überwachen** , und wählen Sie im Menü den Befehl **Eigenschaften** aus. Klicken Sie auf **Diese Richtlinieneinstellungen definieren**, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler** , und klicken Sie auf **Übernehmen** und dann auf **OK**.

8. navigieren Sie zu **Computerkonfiguration** » **Richtlinien** » **Windows-Einstellungen** » **Sicherheitseinstellungen** » **Kontorichtlinien** » **Kerberos-Richtlinie**.

9. Klicken Sie im **Detailbereich** mit der rechten Maustaste auf **Max. Gültigkeitsdauer des Benutzertickets** , und wählen Sie im Menü **Eigenschaften** aus. Klicken Sie auf **Diese Richtlinieneinstellungen definieren**, legen Sie die Anzahl der Stunden auf *1*fest, klicken Sie auf **Übernehmen** und dann auf **OK**. Beachten Sie, dass auch andere Einstellungen im Fenster geändert werden.

10. Wählen Sie im Fenster **Gruppenrichtlinienverwaltung** die Option **Standarddomänenrichtlinie**aus, und klicken Sie mit der rechten Maustaste auf **Bearbeiten**. Das Fenster **Gruppenrichtlinienverwaltungs-Editor** wird angezeigt.

11. Erweitern Sie **Computerkonfiguration** » **Richtlinien** » **Windows-Einstellungen** » **Sicherheitseinstellungen** » **Lokale Richtlinien** , und wählen Sie dann **Zuweisen von Benutzerrechten**aus.

12. Klicken Sie im **Detailbereich** mit der rechten Maustaste auf **Anmelden als Batchauftrag verweigern**, und wählen Sie dann **Eigenschaften**aus.

13. Aktivieren Sie das Kontrollkästchen **Diese Richtlinieneinstellungen definieren** , klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und geben Sie dann in **Benutzer- und Gruppennamen** die Zeichenfolge *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* ein. Anschließend klicken Sie auf **OK**.

14. Klicken Sie auf **OK** , um das Eigenschaftenfenster **Anmelden als Batchauftrag verweigern** zu schließen.

15. Klicken Sie im **Detailbereich** mit der rechten Maustaste auf **Anmelden über Remotedesktopdienste verweigern**, und wählen Sie dann **Eigenschaften**aus.

16. Aktivieren Sie das Kontrollkästchen **Diese Richtlinieneinstellungen definieren** , klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und geben Sie dann in **Benutzer- und Gruppennamen** die Zeichenfolge *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* ein. Anschließend klicken Sie auf **OK**.

17. Klicken Sie auf **OK** , um das Eigenschaftenfenster **Anmelden über Remotedesktopdienste verweigern** zu schließen.

18. Schließen Sie die Fenster **Gruppenrichtlinienverwaltungs-Editor** und **Gruppenrichtlinienverwaltung** .

19. Starten Sie ein PowerShell-Fenster als Administrator, und geben Sie den folgenden Befehl ein, um den Domänencontroller mit den Gruppenrichtlinieneinstellungen zu aktualisieren.

    `gpupdate /force /target:computer`

  Nach einer Minute wird der Vorgang mit der Meldung beendet, dass die Aktualisierung der Computerrichtlinie erfolgreich abgeschlossen wurde.

### Konfigurieren Sie die für die Migration des SID-Verlaufs erforderlichen Registrierungseinstellungen.

Starten Sie PowerShell, und geben Sie die folgenden Befehle ein, die die Quelldomäne konfigurieren, um den RPC-Zugriff (Remoteprozeduraufruf) auf die SAM-Datenbank (Sicherheitskontenverwaltung) zuzulassen.


```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

### Konfigurieren der Weiterleitung von DNS-Namen auf „PRIVDC“

Konfigurieren Sie die Weiterleitung von DNS-Namen auf „PRIVDC“ mithilfe von PowerShell. Geben Sie *contoso.local* für die DNS-Domäne und die IP-Adresse für das virtuelle Netzwerk des Computers „CORPDC“ als IP-Adresse des Masterservers an.

1. Starten Sie PowerShell, und geben Sie den folgenden Befehl ein, wobei Sie die IP-Adresse von „10.1.1.31“ in die IP-Adresse für das virtuelle Netzwerk des Computers „CORPDC“ ändern.

   `Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31`

2. Fügen Sie mithilfe von PowerShell SPNs hinzu, damit die Kerberos-Authentifizierung von SharePoint und der PAM-REST-API sowie dem MIM-Dienst verwendet werden kann (SharePoint wird in Schritt 3 konfiguriert).

    `setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint`

    `setspn -S http/pamsrv PRIV\SharePoint`

   `setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService`

   `setspn -S FIMService/pamsrv PRIV\MIMService`

**HINWEIS:** Diese Anleitung beschreibt die ersten Schritte bei der Installation von MIM 2016-Serverkomponenten auf einem Einzelsystem zu Testzwecken.  Zusätzliche Kerberos-Konfiguration ist erforderlich, um Installation MIM 2016-Serverkomponenten auf mehreren Systemen für hohe Verfügbarkeit wie beschriebenen [in FIM 2010: Setup der Kerberos-Authentifizierung](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### Konfigurieren Sie die Delegierung.

1. Starten Sie **Active Directory-Benutzer und -Computer**.

2. Klicken Sie mit der rechten Maustaste auf die Domäne *priv.contoso.local* , und wählen Sie **Objektverwaltung zuweisen**aus.

3. Klicken Sie auf der Registerkarte **Ausgewählte Benutzer und Gruppen** auf **Hinzufügen**.

4. Geben Sie im Fenster **** zur Auswahl von Benutzern, Computern oder Gruppen die Zeichenfolge *mimcomponent; mimmonitor; mimservice* ein, und klicken Sie dann auf **Namen überprüfen**. Nachdem die Namen unterstrichen sind, klicken Sie auf **OK**und dann auf **Weiter**.

5. Wählen Sie in der Liste der allgemeinen Aufgaben die Optionen **Erstellt, entfernt und verwaltet Benutzerkonten** und **Ändert die Mitgliedschaft einer Gruppe**aus, und klicken Sie dann auf **Weiter** und **Fertig stellen**.

6. Klicken Sie mit der rechten Maustaste auf die Domäne *priv.contoso.local* , und wählen Sie **Objektverwaltung zuweisen**aus.

7. Klicken Sie auf der Registerkarte **Ausgewählte Benutzer und Gruppen** auf **Hinzufügen**.

8. Geben Sie im Popupfenster zur Auswahl **** von Benutzern, Computern oder Gruppen die Zeichenfolge *MIMAdmin* ein, und klicken Sie dann auf **Namen überprüfen**.

9. Nachdem die Namen unterstrichen sind, klicken Sie auf **OK**und dann auf **Weiter**.

10. Wählen Sie **Benutzerdefinierter Task**gilt für **Diesen Ordner**und **Allgemeine Berechtigungen**.  

11. Wählen Sie in der Liste der Berechtigungen die folgenden aus: **Lesen**, **Schreiben**, **Alle untergeordneten Objekte erstellen**, **Alle untergeordneten Objekte löschen**, **Alle Eigenschaften lesen**, **Alle Eigenschaften schreiben**und **SID-Verlauf migrieren**aus. Klicken Sie auf **Weiter** und dann auf **Fertig stellen**.

12. Klicken Sie erneut mit der rechten Maustaste auf die Domäne *priv.contoso.local* , und wählen Sie **Objektverwaltung zuweisen**aus.

13. Klicken Sie auf der Registerkarte **Ausgewählte Benutzer und Gruppen** auf **Hinzufügen**.

14. Geben Sie im Popupfenster zur Auswahl **** von Benutzern, Computern oder Gruppen die Zeichenfolge *MIMAdmin* ein, und klicken Sie dann auf **Namen überprüfen**.

15. Nachdem die Namen unterstrichen sind, klicken Sie auf **OK**und dann auf **Weiter**.

15. Wählen Sie **Benutzerdefinierter Task**gilt für **Diesen Ordner**aus, und klicken Sie dann auf **Nur Benutzerobjekte**.  

16. Wählen Sie in der Liste der Berechtigungen **Kennwort ändern** und **Kennwort zurücksetzen**aus. Klicken Sie anschließend auf **Weiter** und dann auf **Fertig stellen**.

17. Schließen Sie **Active Directory-Benutzer und -Computer**.

16. Starten Sie den Server *PRIVDC* neu, damit diese Änderung wirksam werden.
<!--HONumber=Mar16_HO1-->
