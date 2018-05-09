---
title: Einrichten einer Domäne für Microsoft Identity Manager 2016 | Microsoft-Dokumentation
description: Einrichten eines Active Directory-Domänencontrollers vor der Installation von MIM 2016
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/26/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ff8d8a6f66212b006e2c17186dc299a5bcf3f68b
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-a-domain"></a>Einrichten einer Domäne

>[!div class="step-by-step"]
[Windows Server 2016 »](prepare-server-ws2016.md)

Microsoft Identity Manager (MIM) arbeitet mit Ihrer Active Directory-Domäne. Sie müssen Active Directory bereits installiert haben und sicherstellen, dass Sie in Ihrer Umgebung über einen Domänencontroller für eine Domäne verfügen, die Sie verwalten können.

Dieser Artikel führt Sie durch die Schritte, in denen Sie Ihre Domäne für eine Verwendung von MIM vorbereiten.

## <a name="create-user-accounts-and-groups"></a>Erstellen von Benutzerkonten und -gruppen

Alle Komponenten Ihrer MIM-Bereitstellung benötigen ihre eigenen Identitäten in der Domäne. Dazu gehören sowohl die MIM-Komponenten „Service“ und „Sync“ als auch SharePoint und SQL.

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Name des Domänencontrollers: **corpdc**
> - Domänenname: **contoso**
> - Servername des MIM-Diensts: **corpservice**
> - Servername der MIM-Synchronisierung: **corpsync**
> - SQL-Servername: **corpsql**
> - Kennwort – **Pass@word1**

1. Melden Sie sich auf dem Domänencontroller als Domänenadministrator an (*z.B. Contoso\Administrator*).

2. Erstellen Sie die folgenden Benutzerkonten für MIM-Dienste. Starten Sie PowerShell, und geben Sie das folgende PowerShell-Skript ein, um die Domäne zu aktualisieren.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMMA
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
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
    New-ADUser –SamAccountName MIMpool –name BackupAdmin
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Erstellen Sie für alle Gruppen Sicherheitsgruppen.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  Fügen Sie SPNs hinzu, um die Kerberos-Authentifizierung für Dienstkonten zu aktivieren.

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  Während des Setups müssen Sie die folgenden A-Datensätze von DNS für die richtige Namensauflösung hinzufügen.

- mim.contoso.com als Punkt auf physische IP-Adresse von corpservice
- passwordreset.contoso.com als Punkt auf physische IP-Adresse von corpservice
- passwordregistration.contoso.com als Punkt auf physische IP-Adresse von corpservice

>[!div class="step-by-step"]
[Windows Server 2016 »](prepare-server-ws2016.md)
