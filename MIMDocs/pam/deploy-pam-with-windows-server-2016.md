---
title: Bereitstellen von MIM Privileged Access Management mit Windows Server 2016 | Microsoft-Dokumentation
description: Informationen zum Bereitstellen von Privileged Access Management mit Windows Server 2016
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: 8827a8b6d49672a7860c9265efac5f0881a2c018
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2017
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>Bereitstellen von MIM PAM mit Windows Server 2016


In diesem Szenario können Features von Windows Server 2016 als Domänencontroller für die Gesamtstruktur „PRIV“ durch MIM 2016 SP1 genutzt werden.  Beim Konfigurieren dieses Szenarios ist das Kerberos-Ticket eines Benutzers auf die verbleibende Dauer seiner Rollenaktivierungen zeitlich begrenzt. 

>[!Note]
Technical Preview-Versionen von Windows Server 2016 vor Technical Preview 5 können mit dieser MIM-Version nicht verwendet werden.

## <a name="preparation"></a>Vorbereitung

Für die Laborumgebung sind mindestens zwei virtuelle Computer erforderlich:

-   Ein virtueller Computer, der den PRIV-Domänencontroller unter Windows Server 2016 hostet

-   Ein virtueller Computer, der den MIM-Dienst unter Windows Server 2016 (empfohlen) oder Windows Server 2012 R2 hostet

>[!NOTE]
Wenn Sie in der Laborumgebung nicht bereits über eine CORP-Domäne verfügen, ist für diese Domäne ein zusätzlicher Domänencontroller erforderlich. Der CORP-Domänencontroller kann entweder Windows Server 2016 oder Windows Server 2012 R2 ausführen.


Führen Sie die Installation wie im [Leitfaden für erste Schritte](privileged-identity-management-for-active-directory-domain-services.md) beschrieben aus, **aber nehmen Sie die folgenden Anpassungen vor**:

-   Wenn Sie eine neue CORP-Domäne erstellen, können Sie beim Ausführen der Anweisungen in [Schritt 1: Vorbereiten des CORP-Domänencontrollers](step-1-prepare-corp-domain.md) die CORP-Domäne optional auf der Funktionsebene von Windows Server 2016 konfigurieren. **Wenn Sie diese Option wählen, nehmen Sie folgende Anpassungen vor**:

    -   Wenn Sie Windows Server 2016-Medien verwenden, heißt die Installationsoption „Windows Server 2016 (Server mit Desktopdarstellung)“.

    -   Sie können die Windows Server 2016-Funktionsebene für die CORP-Gesamtstruktur und -Domäne festlegen, indem Sie im Argument für den Befehl „Install-ADDSForest“ wie folgt „7“ als Domänen- und Gesamtstruktur-Versionsnummer angeben:
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   Der letzte Befehl (New-ADGroup -name 'CONTOSO\$\$\$' …)  **unter „Erstellen neuer Benutzer und Gruppen“ ist nicht erforderlich, wenn CORP- und PRIV-Domänencontroller sich auf der Windows Server 2016-Domänenfunktionsebene befinden**.

    -   Die unter „Konfigurieren der Überwachung“ (Punkt 8) und „Konfigurieren der Registrierungseinträge“ Punkt 10) beschrieben Änderungen **werden empfohlen, sind jedoch nicht erforderlich**, wenn CORP- und PRIV-Domänencontroller sich auf der Windows Server 2016-Domänenfunktionsebene befinden.

-   Wenn Sie Windows Server 2012 R2 als Betriebssystem für CORPDC verwenden, müssen Sie die Hotfixes 2919442, 2919355, [und das Update 3155495](http://support.microsoft.com/kb/3156418) auf CORPDC installieren.

-   Folgen Sie den Anweisungen unter [Schritt 2: Vorbereiten des PRIV-Domänencontrollers](step-2-prepare-priv-domain-controller.md), aber nehmen Sie die folgenden Anpassungen vor:

    -   Führen Sie die Installation unter Verwendung von Windows Server 2016-Medien aus. Die Installationsoption heißt „Windows Server 2016 (Server mit Desktopdarstellung)“.

    -   In der Anleitung „Hinzufügen von Rollen“ (Punkt 4) **müssen Sie in der vierten Zeile der PowerShell-Befehle für die Domänen- und Gesamtstruktur-Versionsnummern den Wert 7 festlegen**, um zuzulassen, dass die weiter unten beschriebenen Windows Server AD-Features aktiviert werden können.

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   Wenn Sie die Überwachungs- und Anmelderechte konfigurieren, beachten Sie, dass die Gruppenrichtlinienverwaltung sich im Ordner mit den Windows-Verwaltungsprogrammen befindet.

    -   Das Konfigurieren der Registrierungseinträge für die Migration des SID-Verlaufs (Punkt 8) **ist nicht erforderlich, wenn sich die PRIV-Domäne auf der Windows Server 2016-Domänenfunktionsebene befindet**.

    -   Nach dem Konfigurieren der Delegierung und vor dem Neustart des Servers aktivieren Sie die Privileged Access Management-Features in Windows Server 2016 Active Directory, indem Sie als Administrator ein PowerShell-Fenster starten und die folgenden Befehle eingeben:

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   Nach dem Konfigurieren der Delegierung und vor dem Neustart des Servers autorisieren Sie die MIM-Administratoren und MIM-Dienstkonten zur Erstellung und Aktualisierung von Schattenprinzipalen.

     a. Starten Sie ein PowerShell-Fenster, und geben Sie „ADSIEdit“ ein.

     b. Klicken Sie im Menü „Aktion“ auf „Verbinden mit“. Ändern Sie für die Verbindungspunkteinstellung den Namenskontext von „Standardnamenskontext“ in „Konfiguration“, und klicken Sie auf „OK“.

     c. Nach dem Sie die Verbindung hergestellt haben, erweitern Sie auf der linken Seite des Fensters unter „ADSI-Editor“, den Knoten „Konfiguration“, sodass „CN=Configuration,DC=priv, ...“ angezeigt wird. Erweitern Sie „CN=Configuration“ und dann „CN=Services“.

     d. Klicken Sie mit der rechten Maustaste auf „CN=Shadow Principal Configuration“, und klicken Sie auf „Eigenschaften“. Wenn das Eigenschaftendialogfeld angezeigt wird, wechseln Sie auf die Registerkarte „Sicherheit“.

     e. Klicken Sie auf "Add" (Hinzufügen). Geben Sie die Konten „MIMService“ und alle anderen MIM-Administratoren an, die später „New-PAMGroup“ zum Erstellen von zusätzlichen PAM-Gruppen ausführen. Fügen Sie für jeden Benutzer in der Liste zulässiger Berechtigungen „Schreiben“, „Alle untergeordneten Objekte erstellen“ und „Alle untergeordneten Objekte löschen“ hinzu. Fügen Sie auf die Berechtigungen hinzu.

     f. Wechseln Sie zu „Erweiterte Sicherheitseinstellungen“. Klicken Sie in der Zeile, die den MIMService-Zugriff zulässt, auf „Bearbeiten“. Ändern Sie die Einstellung „Gilt für“ in „Dieses und alle untergeordneten Objekte“. Aktualisieren Sie diese Berechtigungseinstellung, und schließen Sie das Dialogfeld „Sicherheit“.

     g. Schließen Sie den ADSI-Editor.

 -   Nach dem Konfigurieren der Delegierung und vor dem Neustart des Servers autorisieren Sie die MIM-Administratoren zur Erstellung und Aktualisierung von Authentifizierungsrichtlinien.

     a.  Starten Sie eine erhöhte **Eingabeaufforderung**, und geben Sie die folgenden Befehle ein, wobei Sie „mimadmin“ in allen vier Zeilen durch den Namen Ihres MIM-Administratorkontos ersetzen:
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   Folgen Sie den Anweisungen in [Schritt 3: Vorbereiten eines PAM-Servers](step-3-prepare-pam-server.md), aber nehmen Sie die folgenden Anpassungen vor:

    -   Beachten Sie bei der Installation auf Windows Server 2016, dass die Rolle „ApplicationServer“ nicht verfügbar ist.

    -   Wenn Sie MIM auf Windows Server 2016 installieren, **kann SharePoint 2013 nicht installiert werden**.

-   Folgen Sie den Anweisungen in [Schritt 4: Installieren von MIM-Komponenten auf PAM-Server und -Arbeitsstation](step-4-install-mim-components-on-pam-server.md), aber nehmen Sie die folgenden Anpassungen vor:

    -   Der Benutzer, der den MIM-Dienst und die PAM-Komponenten installiert, **muss über Schreibzugriff für die PRIV-Domäne in AD verfügen**, da die MIM-Installation eine neue Active Directory-Organisationseinheit („PAM-Objekte“) erstellt.

    -   Wenn SharePoint nicht installiert ist, installieren Sie das MIM-Portal nicht.

-   Folgen Sie den Anweisungen in [Schritt 5: Herstellen der Vertrauensstellung](step-5-establish-trust-between-priv-corp-forests.md), aber nehmen Sie die folgenden Anpassungen vor:

    -   Beim Herstellen der unidirektionalen Vertrauensstellung führen Sie nur die ersten beiden PowerShell-Befehle („get-credential“ und „New-PAMTrust“) aus. **Führen Sie den Befehl „New-PAMDomainConfiguration“ nicht aus**.

    -   Nach dem Einrichten der Vertrauensstellung melden Sie sich bei PRIVDC als PRIV\\Administrator an, starten Sie PowerShell, und geben Sie die folgenden Befehle ein:
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   Punkt 5 (Überprüfung der Vertrauensstellung) **ist nicht erforderlich, wenn CORP- und PRIV-Domänen sich auf der Windows Server 2016-Domänenfunktionsebene befinden**.

## <a name="more-information"></a>Weitere Informationen

- [Privileged Access Management für Active Directory-Domänendienste](privileged-identity-management-for-active-directory-domain-services.md)
- [Konfigurieren der MIM-Umgebung für Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Konfigurieren von PAM mithilfe von Skripts](sp1-pam-configure-using-scripts.md)
