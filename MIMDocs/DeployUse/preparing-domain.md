---
Titel: Vorbereiten der Domäne
MS.Custom: 
  - Identitätsmanagement
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology: 
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 50345fda-56d7-4b6e-a861-f49ff90a8376
Autor: Kgremban
---
# Vorbereiten der Domäne

## Aktualisieren der Domäne

1.  MIM erfordert, dass Active Directory bereits installiert ist. Stellen Sie sicher, dass Sie in Ihrer Umgebung über einen Domänencontroller für eine Domäne verfügen, die Sie verwalten können.

2.  Melden Sie sich beim Domänencontroller als Domänenadministrator an (*z. B. Contoso\Administrator*), und erstellen Sie die folgenden Benutzerkonten für MIM-Dienste.

    1.  Starten Sie PowerShell.

    2.  Geben Sie das folgende PowerShell-Skript, um die Domäne zu aktualisieren.

        > [!NOTE]
        > In allen folgenden Beispielen stellt **mimservername** den Namen des Domänencontrollers dar, während **domain** den Domänennamen und **Pass@word1** ein Beispielkennwort darstellen.

        ```
        import-module activedirectory
        $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
        New-ADUser –SamAccountName MIMMA –name MIMMA 
        Set-ADAccountPassword –identity MIMMA –NewPassword $sp
        Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMSync –name MIMSync 
        Set-ADAccountPassword –identity MIMSync –NewPassword $sp
        Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMService –name MIMService 
        Set-ADAccountPassword –identity MIMService –NewPassword $sp
        Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMSSPR –name MIMSSPR 
        Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
        Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName SharePoint –name SharePoint 
        Set-ADAccountPassword –identity SharePoint –NewPassword $sp
        Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName SqlServer –name SqlServer 
        Set-ADAccountPassword –identity SqlServer –NewPassword $sp
        Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName BackupAdmin –name BackupAdmin 
        Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
        Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
        ```

3.  Erstellen Sie für alle Gruppen Sicherheitsgruppen.

    1.  Führen Sie das folgende PowerShell-Skript aus:

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global       –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global         –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset 
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  Fügen Sie mithilfe von PowerShell SPNs hinzu, um die Kerberos-Authentifizierung für Dienstkonten zu aktivieren.

    ```
    setspn -S http/mimservername.domain.local Domain\SharePoint
    setspn -S http/mimservername Domain\SharePoint
    setspn -S FIMService/mimservername.domain.local Domain\MIMService
    setspn -S FIMSync/mimservername.domain.local Domain\MIMSync
    ```

<!--HONumber=Mar16_HO1-->
