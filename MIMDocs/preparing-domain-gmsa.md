---
title: Einrichten von gMSAs für Microsoft Identity Manager 2016 | Microsoft-Dokumentation
description: Einrichten gruppenverwalteter Dienstkonten (gMSAs, Group Managed Service Accounts) in einer Domäne für Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: a74f4074d9a0cf8378fd4972b7f51f723bd2f1c6
ms.sourcegitcommit: 80cdfd782cc6e2a4c4698decd54342f0e1460f5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75756255"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Konfigurieren einer Domäne für ein Szenario für gruppenverwaltete Dienstkonten

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)

> [!IMPORTANT]
> Dieser Artikel gilt nur für MIM 2016 SP2.

Microsoft Identity Manager (MIM) arbeitet mit Ihrer Active Directory-Domäne. Sie müssen Active Directory bereits installiert haben und sicherstellen, dass Sie in Ihrer Umgebung über einen Domänencontroller für eine Domäne verfügen, die Sie verwalten können.  In diesem Artikel wird beschrieben, wie Sie gruppenverwaltete Dienstkonten in dieser Domäne für die Verwendung durch MIM einrichten.

## <a name="overview"></a>Übersicht

Durch gruppenverwaltete Dienstkonten müssen Kennwörter für Dienstkonten nicht mehr regelmäßig geändert werden. Mit dem Release von MIM 2016 SP2 können für die folgenden MIM-Komponenten gMSA-Konten konfiguriert werden, die während des Installationsvorgangs verwendet werden:

-   MIM-Synchronisierungsdienst (FIMSynchronizationService)
-   MIM-Dienst (FIMService)
-   Anwendungspool für die Registrierungswebsite für die MIM-Kennwortregistrierung
-   Anwendungspool für die Website für die MIM-Kennwortzurücksetzung
-   Anwendungspool für die Website für PAM REST API
-   PAM-Überwachungsdienst (PamMonitoringService)
-   PAM-Komponentendienst (PrivilegeManagementComponentService)

Die folgenden MIM-Komponenten unterstützen nicht die Ausführung als gMSA-Konten:

-   MIM-Portal: Der Grund dafür ist, dass das MIM-Portal Teil der SharePoint-Umgebung ist. Stattdessen können Sie SharePoint im Farmmodus bereitstellen und [die automatische Kennwortänderung in SharePoint Server konfigurieren](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Alle Verwaltungs-Agents
-   Microsoft-Zertifikatverwaltung
-   BHOLD


Weitere Informationen zu gMSA finden Sie in diesen Artikeln:
-   [Gruppenverwaltete Dienstkonten: Übersicht](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Erstellen der Stammschlüssel für die Schlüsselverteilungsdienste (Key Distribution Services, KDS)](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Erstellen von Benutzerkonten und -gruppen

Alle Komponenten Ihrer MIM-Bereitstellung benötigen ihre eigenen Identitäten in der Domäne. Dazu gehören sowohl die MIM-Komponenten „Service“ und „Sync“ als auch SharePoint und SQL.


> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Name des Domänencontrollers: **dc**
> - Domänenname: **contoso**
> - Servername des MIM-Diensts: **mimservice**
> - Name des MIM-Synchronisierungsservers: **mimsync**
> - SQL Server-Name: **sql**
> - Kennwort – <strong>Pass@word1</strong>

1. Melden Sie sich auf dem Domänencontroller als Domänenadministrator an (*z.B. Contoso\Administrator*).

2. Erstellen Sie die folgenden Benutzerkonten für MIM-Dienste. Starten Sie PowerShell, und geben Sie das folgende PowerShell-Skript ein, um neue AD-Domänenbenutzer zu erstellen (nicht alle Konten sind obligatorisch, obwohl das Skript ausschließlich zu Informationszwecken bereitgestellt wird. Verwenden Sie deshalb am besten ein dediziertes *MIMAdmin*-Konto für den MIM- und SharePoint-Installationsvorgang).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Erstellen Sie für alle Gruppen Sicherheitsgruppen.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Fügen Sie SPNs hinzu, um die Kerberos-Authentifizierung für Dienstkonten zu aktivieren.

    ```PowerShell
    Set-ADServiceAccount -Identity svcMIMAppPool -ServicePrincipalNames @{Add="http/mim.contoso.com"}
    ```

5.  Stellen Sie sicher, dass Sie die folgenden „A“-DNS-Einträge für die richtige Namensauflösung registrieren (vorausgesetzt, dass der MIM-Dienst, das MIM-Portal sowie die Websites für die Kennwortzurücksetzung und Kennwortregistrierung auf demselben Computer gehostet werden).

    - mim.contoso.com: verweist auf die physische IP-Adresse des Servers des MIM-Diensts und des MIM-Portals
    - passwordreset.contoso.com: verweist auf die physische IP-Adresse des Servers des MIM-Diensts und des MIM-Portals
    - passwordregistration.contoso.com: verweist auf die physische IP-Adresse des Servers des MIM-Diensts und des MIM-Portals

## <a name="create-key-distribution-service-root-key"></a>Erstellen der Stammschlüssel für die Schlüsselverteilungsdienste (Key Distribution Services, KDS)

Stellen Sie sicher, dass Sie bei Ihrem Domänencontroller als Administrator angemeldet sind, um den Verteilungsdienst für den Gruppenschlüssel vorzubereiten.

Falls bereits ein Stammschlüssel für die Domäne vorhanden ist (verwenden Sie zur Überprüfung **Get-KdsRootKey**), fahren Sie mit dem nächsten Abschnitt fort.

6.  Erstellen Sie den Stammschlüssel der Schlüsselverteilungsdienste (Key Distribution Services, KDS), falls erforderlich (nur einmal pro Domäne). Der Stammschlüssel wird (zusammen mit anderen Informationen) vom KDS-Dienst auf Domänencontrollern zum Generieren von Kennwörtern verwendet. Geben Sie als Domänenadministrator die folgenden PowerShell-Befehle ein:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *–EffectiveImmediately* erfordert möglicherweise eine Verzögerung von bis zu \~10 Stunden, da alle Domänencontroller repliziert werden müssen. Dies dauert bei zwei Domänencontrollern etwa eine Stunde.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >In der Lab- oder Testumgebung können Sie die Verzögerung für die Replikation von 10 Stunden vermeiden, indem Sie stattdessen den folgenden Befehl ausführen:
    ><br/>
    >Add-KDSRootKey -EffectiveTime ((Get-Date).AddHours(-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>Erstellen des Dienstkontos zur MIM-Synchronisierung sowie den Gruppen- und Dienstprinzipal
-----------------------

Stellen Sie sicher, dass alle Computerkonten für Computer, auf denen die MIM-Software installiert werden soll, bereits mit der Domäne verknüpft sind.  Führen Sie anschließend die folgenden Schritte als Domänenadministrator in PowerShell aus.

7.  Erstellen Sie eine Gruppe namens *MIMSync_Servers*, und fügen Sie dieser Gruppe alle MIM-Synchronisierungsserver hinzu.
    Geben Sie Folgendes ein, um eine neue AD-Gruppe für MIM-Synchronisierungsserver zu erstellen. Fügen Sie anschließend zu dieser Gruppe die Active Directory-Computerkonten für den MIM-Synchronisierungsserver hinzu, z. B. *contoso\MIMSync$* .

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  Erstellen Sie das gruppenverwaltete Dienstkonto (gMSA) für den MIM-Synchronisierungsdienst. Geben Sie Folgendes in PowerShell ein.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Überprüfen Sie die Details des gMSA, der durch Ausführen des PowerShell-Befehls *Get-ADServiceAccount* erstellt wurde:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Wenn Sie den Benachrichtigungsdienst für Kennwortänderungen ausführen möchten, müssen Sie den Dienstprinzipalnamen durch Ausführen des folgenden PowerShell-Befehls registrieren:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Starten Sie Ihren MIM-Synchronisierungsserver neu, um das dem Server zugeordnete Kerberos-Token zu aktualisieren, da sich die „MIMSync_Server“-Gruppenmitgliedschaft geändert hat.

## <a name="create-mim-service-management-agent-service-account"></a>Erstellen eines Dienstkontos für den MIM-Dienstverwaltungs-Agent

11. Normalerweise erstellen Sie beim Installieren des MIM-Diensts ein neues Konto für den MIM-Dienstverwaltungs-Agent (MIM-MA-Konto).  Mit gMSA stehen Ihnen zwei Optionen zur Verfügung:

- Verwenden Sie das von der MIM-Synchronisierungsdienstgruppe verwaltete Dienstkonto, und erstellen Sie kein separates Konto.

    Sie können die Erstellung des Dienstkontos für den MIM-Dienstverwaltungs-Agent überspringen. Verwenden Sie in diesem Fall den Namen des gruppenverwalteten Dienstkontos für den MIM-Synchronisierungsdienst, z. B. *contoso\MIMSyncGMSAsvc$* , anstelle des MIM-MA-Kontos, wenn Sie den MIM-Dienst installieren. Aktivieren Sie im weiteren Verlauf der Konfiguration des MIM-Dienstverwaltungs-Agents die Option *'Use MIMSync Account'* (MIMSync-Konto verwenden).

    Aktivieren Sie nicht 'Deny Logon from Network' (Anmeldung über Netzwerk verweigern) für das gruppenverwaltete Dienstkonto des MIM-Synchronisierungsdiensts da das MIM-MA-Konto die Berechtigung 'Allow Network Logon' (Anmeldung über das Netzwerk zulassen) benötigt.

- Verwenden Sie ein reguläres Dienstkonto für das Dienstkonto des MIM-Dienstverwaltungs-Agent.

    Starten Sie PowerShell als Domänenadministrator, und geben Sie Folgendes ein, um einen neuen AD-Domänenbenutzer zu erstellen:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Aktivieren Sie nicht 'Deny Logon from Network' (Anmeldung über Netzwerk verweigern) für das MIM-MA-Konto, da dieses die Berechtigung 'Allow Network Logon' (Anmeldung über das Netzwerk zulassen) benötigt.

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>Erstellen von MIM-Dienstkonten, Gruppen und des Dienstprinzipals

Verwenden Sie weiterhin PowerShell als Domänenadministrator.
   
12. Erstellen Sie eine Gruppe namens *MIMService_Servers*, und fügen Sie dieser Gruppe alle MIM-Dienstserver hinzu.  Geben Sie den folgenden PowerShell-Befehl ein, um eine neue AD-Gruppe für die MIM-Dienstserver zu erstellen und um das Active Directory-Computerkonto (z. B. *contoso\MIMPortal$* ) des MIM-Dienstservers dieser Gruppe hinzuzufügen.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  Erstellen eines gruppenverwalteten Dienstkontos (gMSA) für den MIM-Dienst

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Registrieren Sie den Dienstprinzipalnamen, und aktivieren Sie die Kerberos-Delegation, indem Sie folgenden PowerShell-Befehl ausführen:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  Für Szenarios mit Self-Service-Kennwortzurücksetzung (SSPR) muss Ihr MIM-Dienstkonto mit dem MIM-Synchronisierungsdienst kommunizieren können. Deshalb muss das MIM-Dienstkonto entweder Mitglied der Gruppe MIMSyncAdministrators oder der Gruppe Kennwortzurücksetzung und Browsen für die MIM-Synchronisierung sein.

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Starten Sie Ihren MIM-Dienstserver neu, um ein dem Server zugeordnetes Kerberos-Token zu aktualisieren, da sich die Gruppenmitgliedschaft für „MIMService_Servers“ geändert hat.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Erstellen anderer MIM-Konten und -Gruppen (bei Bedarf)

Wenn Sie die Self-Service-Kennwortzurücksetzung für MIM konfigurieren, können Sie mit denselben Richtlinien wie oben für den MIM-Synchronisierungsserver und den MIM-Dienst weitere gMSA für folgenden Pools erstellen:

- Anwendungspool für die Website für die MIM-Kennwortzurücksetzung
- Anwendungspool für die Registrierungswebsite für die MIM-Kennwortregistrierung

Wenn Sie MIM-PAM konfigurieren, können Sie mit denselben Richtlinien wie oben für den MIM-Synchronisierungsserver und den MIM-Dienst weitere gMSA für folgenden Pools erstellen:

- Anwendungspool für die Website für MIM-PAM REST API
- MIM-PAM-Komponentendienst
- MIM-PAM-Überwachungsdienst

## <a name="specifying-a-gmsa-when-installing-mim"></a>Angeben eines gruppenverwalteten Dienstkontos (gMSA) bei der MIM-Installation

Als allgemeine Regel gilt, dass bei der Verwendung eines MIM-Installers zur Angabe, dass Sie ein gMSA anstelle eines regulären Kontos verwenden möchten, ein Dollarzeichen an den gMSA-Namen angefügt werden muss, also z. B. **contoso\MIMSyncGMSAsvc$** . Das Kennwortfeld bleibt in diesem Fall leer. Eine Ausnahme ist das *miisactivate.exe*-Tool, das den gMSA-Namen ohne Dollarzeichen akzeptiert.
<br/>

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)
