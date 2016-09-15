---
title: "Bereitstellen von PAM Schritt 6 – Verschieben einer Gruppe | Microsoft Identity Manager"
description: Migrieren Sie eine Gruppe zur Gesamtstruktur PRIV, damit sie mit Privileged Access Management verwaltet werden kann.
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 603e5e28f0eee0f648ef7e00ef137f5a08b2ba34


---

# Schritt 6 – Übergang eine Gruppe zur privilegierten Zugriffsverwaltung

>[!div class="step-by-step"]
[«Schritt 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Schritt 7»](step-7-elevate-user-access.md)

Die Erstellung des privilegierten Kontos in der PRIV-Gesamtstruktur erfolgt mit PowerShell-Cmdlets. Diese Cmdlets führen die Funktionen aus:

- Erstellen Sie in der PRIV-Gesamtstruktur eine neue Gruppe mit der gleichen Sicherheits-ID (Security Identifier, SID) als Gruppe in der CORP-Gesamtstruktur.  
- Erstellen Sie ein Objekt in der MIM-Dienstdatenbank, das der Gruppe in der PRIV-Gesamtstruktur entspricht.  
- Erstellen Sie für jedes Benutzerkonto zwei Objekte in der MIM-Dienstdatenbank, die dem Benutzer in der CORP-Gesamtstruktur und dem neuen Benutzerkonto in der PRIV-Gesamtstruktur entsprechen.  
- Erstellt ein PAM-Rollenobjekt in der MIM-Dienstdatenbank.  

Die Cmdlets müssen einmal für jede Gruppe und einmal für jedes Mitglied einer Gruppe ausgeführt werden. Die Migrations-Cmdlets ändern keine Benutzer oder Gruppen in der CORP-Gesamtstruktur: dies führt der PAM-Administrator im Anschluss manuell durch.

1. Melden Sie sich bei PAMSRV, entweder direkt oder von einer PRIV-Arbeitsstation, als *PRIV\MIMAdmin* an.

2.  Starten Sie PowerShell, und geben Sie die folgenden Befehle ein.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Erstellen Sie zu Demonstrationszwecken in PRIV ein entsprechendes Benutzerkonto für ein Benutzerkonto in einer vorhandenen Gesamtstruktur.

    Geben Sie die folgenden Befehle in PowerShell ein.  Wenn Sie nicht zuvor den Namen *Jen* verwendet haben, um den Benutzer in „contoso.local“ zu erstellen, ändern Sie die Parameter des Befehls nach Bedarf. Das Kennwort „Pass@word1“ ist nur ein Beispiel und sollte durch einen eindeutigen Kennwortwert ersetzt werden.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Kopieren Sie zu Demonstrationszwecken eine Gruppe und deren Mitglied, Jen, von CONTOSO in die PRIV-Domäne.

    Führen Sie die folgenden Befehle aus, wobei Sie das Kennwort des CORP-Domänenadministrators (CONTOSO\Administrator) bei entsprechender Aufforderung angeben:

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Der Befehl **New-PAMGroup** übernimmt die beiden folgenden Parameter als Referenz:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Optional) Entfernen Sie auf CORPDC das Konto von Jen aus der Gruppe **CONTOSO CorpAdmins**, sofern dieses noch vorhanden ist.  Dies ist nur für Demonstrationszwecke erforderlich, um zu veranschaulichen, wie Berechtigungen mit Konten verknüpft werden können, die in der PRIV-Gesamtstruktur erstellt werden.

    1.  Melden Sie sich bei CORPDC als *CONTOSO\Administrator* an.

    2.  Starten Sie PowerShell, führen Sie den folgenden Befehl aus, und bestätigen Sie anschließend die Änderung.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Wenn Sie veranschaulichen möchten, dass das Administratorkonto des Benutzers über die Gesamtstruktur übergreifende Zugriffsrechte verfügt, fahren Sie mit dem nächsten Schritt fort.

>[!div class="step-by-step"]
[«Schritt 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Schritt 7»](step-7-elevate-user-access.md)



<!--HONumber=Jul16_HO3-->

