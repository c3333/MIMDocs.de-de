---
Titel: Schritt 6 – Übergang eine Gruppe zur privilegierten Zugriffsverwaltung
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
MS.AssetId: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
---
# Schritt 6 – Übergang eine Gruppe zur privilegierten Zugriffsverwaltung
Die Erstellung des privilegierten Kontos in der Gesamtstruktur „PRIV“ erfolgt mithilfe mehrerer neuer PowerShell-Cmdlets.  Diese Cmdlets führen die Funktionen aus:

-   Erstellt eine neue Gruppe in der *PRIV* Gesamtstruktur mit dem gleichen *SID* (Security Identifier) einer Gruppe in der *CORP* Gesamtstruktur und als ein Objekt in der MIM-Dienst Datenbank entspricht der Gruppe in der *PRIV* Gesamtstruktur.

-   Für jedes Benutzerkonto erstellt die Cmdlets zwei Objekte in der MIM-Dienstdatenbank für den Benutzer in der *CORP* Gesamtstruktur und der neue Benutzer-Konto der *PRIV* Gesamtstruktur.

-   Erstellt eine *PAM Role* Objekt in der MIM-Datenbank.

    In dieser Vorabversionsvorschau müssen die Cmdlets einmal für jede Gruppe und einmal für jedes Mitglied einer Gruppe ausgeführt werden.  (Beachten Sie, dass die Migrations-Cmdlets nicht ändern oder ändern keine Benutzer oder Gruppen in der *CORP* Gesamtstruktur: das ist manuell vom PAM-Administrator später durchgeführt werden.)

    1.  Melden Sie sich bei *PAMSRV* als Domänenadministrator an, und überprüfen Sie, ob die Dienste ausgeführt werden.

        1.  Stellen Sie sicher, Sie sind angemeldet *PAMSRV* als *PRIV\Administrator*.

        2.  Öffnen Sie **Dienste**.

        3.  Überprüfen Sie, ob die **Forefront Identity Manager** -Dienst wird ausgeführt.  Wenn der Dienst nicht ausgeführt wird, rechtsklicken Sie auf den Dienst und wählen **start** zum Starten des Diensts.

    2.  Führen Sie die MIM PAM-Gruppen und Benutzer-Management-Cmdlets um aus der Gruppe "CorpAdmins" und dem Element, "jen", kopieren *CONTOSO* zu *PRIV* Domäne.

        1.  Starten Sie PowerShell, und führen Sie die folgenden Befehle, die Angabe der *CORP* Domänen-Admins (*CONTOSO\Administrator*) Kennwort bei entsprechender Aufforderung:

            ```
            Import-Module MIMPAM
            Import-Module ActiveDirectory
            $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
            $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca 
            $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen 
            $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
            Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
            Set-ADUser –identity priv.Jen –Enabled 1 
            Add-ADGroupMember "Protected Users" priv.Jen
            $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
            ```
            Referenz der **New-PAMGroup** Befehl verwendet die folgenden Parameter:

            -   Den Domänenname der Gesamtstruktur „CORP“ in NetBIOS-Form.

            -   Den Namen der Gruppe, die aus dieser Domäne kopiert wird.

            -   Den NetBIOS-Namen des Domänencontrollers der Gesamtstruktur „CORP“.

            -   Die Anmeldeinformationen eines Domänenadministrators in der Gesamtstruktur „CORP“.

        Als Nächstes führen Sie den Übergang für einen Benutzer für die bedarfsorientierte Rechteerweiterung durch, der derzeit Gruppenmitglied ist. Dann überprüfen Sie, ob die gesamtstrukturübergreifenden Zugriffsrechte oder das Administratorkonto des Benutzers wirksam sind.

    3.  Entfernen Sie auf *CORPDC*das Konto von Jen aus der Gruppe *CONTOSO CorpAdmins* , sofern dieses noch vorhanden ist.

        1.  Melden Sie sich bei *CORPDC* als *CONTOSO\Administrator*.

        2.  Starten Sie PowerShell, führen Sie den folgenden Befehl aus, und bestätigen Sie anschließend die Änderung.

            ```
            Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
            ```

<!--HONumber=Mar16_HO1-->
