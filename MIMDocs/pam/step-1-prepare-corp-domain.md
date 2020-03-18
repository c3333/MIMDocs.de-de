---
title: 'Bereitstellen von PAM – Schritt 1: CORP-Domäne | Microsoft Docs'
description: Bereiten Sie die CORP-Domäne mit vorhandenen oder neuen Identitäten vor, die vom Privileged Identity Manager verwaltet werden sollen.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c21228dad923d80ab63c255c1184b7de04a0ff3d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043731"
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>Schritt 1: Vorbereiten des Hosts und der CORP-Domäne

> [!div class="step-by-step"]
> [Schritt 2 »](step-2-prepare-priv-domain-controller.md)

In diesem Schritt bereiten Sie das Hosten der geschützten Umgebung vor. Bei Bedarf erstellen Sie auch einen Domänencontroller und eine Mitgliedsarbeitsstation in einer neuen Domäne und Gesamtstruktur (der Gesamtstruktur *CORP*) mit Identitäten, die durch die geschützte Umgebung verwaltet werden. Die CORP-Gesamtstruktur simuliert eine vorhandene Gesamtstruktur, die zu verwaltende Ressourcen enthält. Dieses Dokument enthält eine Beispielressource, die geschützt werden soll: eine Dateifreigabe.

Wenn Sie bereits über eine Active Directory-Domäne mit einem Domänencontroller unter Windows Server 2012 R2 oder höher verfügen, in der Sie Domänenadministrator sind, können Sie stattdessen diese Domäne verwenden.  

## <a name="prepare-the-corp-domain-controller"></a>Vorbereiten des CORP-Domänencontrollers

Dieser Abschnitt erläutert, wie Sie einen Domänencontroller für eine CORP-Domäne einrichten. In der CORP-Domäne werden Administratoren von der geschützten Umgebung verwaltet. Der in diesem Beispiel verwendete DNS-Name (Domain Name System) der CORP-Domäne lautet *contoso.local*.

### <a name="install-windows-server"></a>Installieren von Windows Server

Installieren Sie auf einem virtuellen Computer Windows Server 2012 R2 oder Windows Server 2016 Technical Preview 4 oder höher, um einen Computer namens *CORPDC* zu erstellen.

1. Wählen Sie **Windows Server 2012 R2 Standard (Server mit grafischer Benutzeroberfläche) x64** oder **Windows Server 2016 Technical Preview (Server mit Desktopdarstellung)** aus.

2. Lesen und akzeptieren Sie die Lizenzbedingungen.

3. Da der Datenträger leer ist, wählen Sie **Benutzerdefiniert: Nur Windows installieren** aus, und verwenden Sie den nicht initialisierten Speicherplatz.

4. Melden Sie sich bei dem neuen Computer als Administrator an. Wechseln Sie zur Systemsteuerung. Legen Sie den Computernamen auf *CORPDC* fest, und weisen Sie ihm eine statische IP-Adresse im virtuellen Netzwerk zu. Starten Sie den Server neu.

5. Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator an. Wechseln Sie zur Systemsteuerung. Konfigurieren Sie den Computer für die Suche nach Updates, und installieren Sie alle erforderlichen Updates. Starten Sie den Server neu.

### <a name="add-roles-to-establish-a-domain-controller"></a>Hinzufügen von Rollen zum Einrichten eines Domänencontrollers

In diesem Abschnitt fügen Sie die Rollen „Active Directory-Domänendienste (AD DS)“, „DNS-Server“ und „Dateiserver“ (Teil des Abschnitts „Datei- und Speicherdienste“) hinzu und stufen diesen Server zu einem Domänencontroller der neuen Gesamtstruktur „contoso.local“ hoch.

> [!NOTE]  
> Wenn Sie bereits über eine Domäne verfügen, die Sie als CORP-Domäne verwenden können und diese Domäne Windows Server 2012 R2 oder höher als Domänencontroller verwendet, können Sie mit [Erstellen weiterer Benutzer und Gruppen zu Demonstrationszwecken](#create-additional-users-and-groups-for-demonstration-purposes) fortfahren.

1. Starten Sie PowerShell, während Sie als Administrator angemeldet sind.

2. Geben Sie die folgenden Befehle ein.

   ```PowoerShell
   import-module ServerManager

   Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

   Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
   ```

   Dies führt zu der Aufforderung, ein Administratorkennwort für den abgesicherten Modus zu verwenden. Beachten Sie, dass Fehlermeldungen für die DNS-Delegierung und Kryptografieeinstellungen angezeigt werden. Diese sind normal.

3. Wenn die Erstellung der Gesamtstruktur abgeschlossen ist, melden Sie sich ab. Der Server wird automatisch neu gestartet.

4. Nachdem der Server neu gestartet wurde, melden Sie sich als Administrator der Domäne bei CORPDC an. Dies ist üblicherweise der Benutzer „CONTOSO\\Administrator“ mit dem Kennwort, das während der Installation von Windows auf CORPDC angegeben wurde.

### <a name="create-a-group"></a>Erstellen einer Gruppe

Erstellen Sie eine Gruppe zur Überwachung durch Active Directory, wenn diese Gruppe nicht bereits vorhanden ist. Der Name der Gruppe muss der NetBIOS-Domänenname gefolgt von drei Dollarzeichen sein, z. B. *CONTOSO$$$* .

Melden Sie sich in jeder Domäne als Domänenadministrator bei einem Domänencontroller an, und führen Sie die folgenden Schritte aus:

1. Starten Sie PowerShell.

2. Geben Sie die folgenden Befehle ein, aber ersetzen Sie „CONTOSO“ durch den NetBIOS-Namen Ihrer Domäne.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
   ```

In einigen Fällen ist die Gruppe möglicherweise bereits vorhanden. Dies ist normal, wenn die Domäne auch in AD-Migrationsszenarien verwendet wurde.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Erstellen weiterer Benutzer und Gruppen zu Demonstrationszwecken

Wenn Sie eine neue CORP-Domäne erstellt haben, sollten Sie zur Demonstration des PAM-Szenarios weitere Benutzer und Gruppen erstellen. Die zu Demonstrationszwecken erstellten Benutzer und Gruppen dürfen keine Domänenadministratoren sein und nicht durch die adminSDHolder-Einstellungen in AD gesteuert werden.

> [!NOTE]
> Wenn Sie bereits über eine Domäne verfügen, die Sie als CORP-Domäne verwenden und diese Domäne einen Benutzer und eine Gruppe enthält, den bzw. die Sie zu Demonstrationszwecken verwenden können, können Sie mit dem Abschnitt [Konfigurieren der Überwachung](#configure-auditing) fortfahren.

Wir erstellen eine Sicherheitsgruppe namens *CorpAdmins* und einen Benutzer namens *Jen*. Sie können auch andere Namen verwenden.

1. Starten Sie PowerShell.

2. Geben Sie die folgenden Befehle ein. Ersetzen Sie das Kennwort „Pass@word1“ durch eine andere Kennwortzeichenfolge.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

   New-ADUser –SamAccountName Jen –name Jen

   Add-ADGroupMember –identity CorpAdmins –Members Jen

   $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   Set-ADAccountPassword –identity Jen –NewPassword $jp

   Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
   ```

### <a name="configure-auditing"></a>Konfigurieren der Überwachung

Sie müssen die Überwachung in vorhandenen Gesamtstrukturen aktivieren, um die PAM-Konfiguration in diesen Gesamtstrukturen einzurichten.  

Melden Sie sich in jeder Domäne als Domänenadministrator bei einem Domänencontroller an, und führen Sie die folgenden Schritte aus:

1. Wechseln Sie zu **Start** > **Verwaltung** (oder unter Windows Server 2016 zu **Windows-Verwaltungsprogramme**), und starten Sie die **Gruppenrichtlinienverwaltung**.

2. Navigieren Sie zur Domänencontrollerrichtlinie für diese Domäne.  Wenn Sie eine neue Domäne für „contoso.local“ erstellt haben, navigieren Sie zu **Gesamtstruktur: contoso.local** > **Domänen** > **contoso.local** > **Domänencontroller** > **Standarddomänencontroller-Richtlinie**. Es wird eine Informationsmeldung angezeigt.

3. Klicken Sie mit der rechten Maustaste auf **Standarddomänencontroller-Richtlinie**, und wählen Sie **Bearbeiten** aus. Ein neues Fenster wird angezeigt.

4. Navigieren Sie im Fenster „Gruppenrichtlinienverwaltungs-Editor“ unter „Standarddomänencontroller-Richtlinie“ zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Lokale Richtlinien** > **Überwachungsrichtlinie**.

5. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Kontenverwaltung überwachen**, und wählen Sie den Befehl **Eigenschaften** aus. Wählen Sie **Diese Richtlinieneinstellungen definieren** aus, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler**, und klicken Sie anschließend auf **Übernehmen** und **OK**.

6. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Verzeichnisdienstzugriff überwachen**, und wählen Sie den Befehl **Eigenschaften** aus. Wählen Sie **Diese Richtlinieneinstellungen definieren** aus, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler**, und klicken Sie anschließend auf **Übernehmen** und **OK**.

7. Schließen Sie die Fenster „Gruppenrichtlinienverwaltungs-Editor“ und „Gruppenrichtlinienverwaltung“.

8. Wenden Sie die Überwachungseinstellungen an, indem Sie ein PowerShell-Fenster starten und Folgendes eingeben:

   ```cmd
   gpupdate /force /target:computer
   ```

Nach wenigen Minuten sollte die Meldung **Die Aktualisierung der Computerrichtlinie wurde erfolgreich abgeschlossen** angezeigt werden.

### <a name="configure-registry-settings"></a>Konfigurieren der Registrierungseinstellungen

In diesem Abschnitt konfigurieren Sie die Registrierungseinträge für die Migration des SID-Verlaufs, der zum Erstellen der Gruppe für das Privileged Access Management verwendet wird.

1. Starten Sie PowerShell.

2. Geben Sie die folgenden Befehle ein, um die Quelldomäne so zu konfigurieren, dass RPC-Zugriff (Remote Procedure Call, Remoteprozeduraufruf) auf die SAM-Datenbank (Security Accounts Manager, Sicherheitskontenverwaltung) zugelassen wird.

   ```PowerShell
   New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

   Restart-Computer
   ```

Dadurch wird der Domänencontroller CORPDC neu gestartet. Weitere Informationen zu dieser Registrierungseinstellung finden Sie unter [How to troubleshoot inter-forest sIDHistory migration with ADMTv2](https://support.microsoft.com/kb/322970) (Problembehandlung der sIDHistory-Migration zwischen Gesamtstrukturen mit ADMTv2).

## <a name="prepare-a-corp-workstation-and-resource"></a>Vorbereiten einer CORP-Arbeitsstation und -Ressourcen

Wenn Sie noch nicht über einen in die Domäne eingebundenen Arbeitsstationcomputer verfügen, befolgen Sie diese Anweisungen, um einen solchen Computer zu erstellen.  

> [!NOTE]
> Wenn Sie bereits eine Arbeitsstation in die Domäne eingebunden haben, fahren Sie mit [Erstellen einer Ressource zu Demonstrationszwecken](#create-a-resource-for-demonstration-purposes) fort.

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Installieren von Windows 8.1 oder Windows 10 Enterprise als VM

Installieren Sie Windows Server 8.1 Enterprise oder Windows 10 Enterprise auf einem anderen neuen virtuellen Computer ohne installierte Software, um einen Computer namens *CORPWKSTN* zu erstellen.

1. Verwenden Sie „Express-Einstellungen“ während der Installation.

2. Beachten Sie, dass die Installation möglicherweise keine Verbindung mit dem Internet herstellen kann. Klicken Sie auf **Lokales Konto erstellen**. Geben Sie einen anderen Benutzernamen an. Verwenden Sie weder „Administrator“ noch „Jen“.

3. Geben Sie in der Systemsteuerung diesem Computer eine statische IP-Adresse im virtuellen Netzwerk, und legen Sie den bevorzugten DNS-Server der Schnittstelle auf denjenigen des Servers „CORPDC“ fest.

4. Binden Sie in der Systemsteuerung den Computer „CORPWKSTN“ in die Domäne „contoso.local“ ein. Sie müssen die Anmeldeinformationen des Contoso-Domänenadministrators angeben. Nachdem dies abgeschlossen ist, starten Sie den Computer „CORPWKSTN“ neu.

### <a name="create-a-resource-for-demonstration-purposes"></a>Erstellen einer Ressource zu Demonstrationszwecken

Sie benötigen eine Ressource, um die sicherheitsgruppenbasierte Zugriffskontrolle mit PAM zu demonstrieren.  Wenn Sie noch nicht über eine Ressource verfügen, können Sie zu Demonstrationszwecken einen Dateiordner verwenden.  Hierbei werden die AD-Objekte „Jen“ und „CorpAdmins“ verwendet, die Sie in der Domäne „contoso.local“ erstellt haben.

1. Stellen Sie eine Verbindung mit der Arbeitsstation CORPWKSTN her. Klicken Sie auf das Symbol **Benutzer wechseln** und dann auf **Anderer Benutzer**. Vergewissern Sie sich, dass sich der Benutzer „CONTOSO\\Jen“ bei „CORPWKSTN“ anmelden kann.

2. Erstellen Sie einen neuen Ordner namens *CorpFS*, und geben Sie ihn für die Gruppe *CorpAdmins* frei.

3. Öffnen Sie PowerShell als Administrator.

4. Geben Sie die folgenden Befehle ein.

   ```PowerShell
   mkdir c:\corpfs

   New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

   $acl = Get-Acl c:\corpfs

   $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

   $acl.SetAccessRule($car)

   Set-Acl c:\corpfs $acl
   ```

Im nächsten Schritt bereiten Sie den PRIV-Domänencontroller vor.

> [!div class="step-by-step"]
> [Schritt 2 »](step-2-prepare-priv-domain-controller.md)
