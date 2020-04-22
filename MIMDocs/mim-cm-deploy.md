---
title: Bereitstellen der Microsoft Identity Manager-Zertifikatverwaltung | Microsoft-Dokumentation
description: Installieren der Microsoft Identity Manager 2016-Zertifikatverwaltung
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 35fe08363b6964bf6d264ab1e60cd9751aa7b6aa
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043034"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Bereitstellen der Microsoft Identity Manager-Zertifikatverwaltung 2016 (MIM CIM)

Die Installation der Microsoft Identity Manager-Zertifikatverwaltung 2016 (MIM CM) umfasst eine Reihe von Schritten. Um Ihnen den Vorgang zu erleichtern, haben wir ihn in mehrere Schritte unterteilt. Es gibt Schritte, die Sie vor den eigentlichen Schritten in MIM CM durchführen müssen. Ohne diese vorausgehenden Maßnahmen schlägt die Installation fehl.

In folgendem Diagramm wird ein Beispiel für den Typ der Umgebung dargestellt, der verwendet werden kann. Die Systeme sind mit den jeweiligen Zahlen in der Liste unter dem Diagramm enthalten und sind erforderlich, um die Schritte in diesem Artikel wie beschrieben erfolgreich abzuschließen. Abschließend werden Windows 2016 Datacenter Server verwendet:

![Diagramm der Umgebung](media/mim-cm-deploy/image001.png)

1. CORPDC – Domänencontroller
2. CORPCM – MIM CM-Server
3. CORPCA – Zertifizierungsstelle
4. CORPCMR – MIM CM Rest-API Web – CM-Portal für Rest-API – Werden später verwendet
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10-Geräte, die in eine Domäne eingebunden sind

## <a name="deployment-overview"></a>Übersicht über die Bereitstellung

- Basisbetriebssysteminstallation

    Das Lab besteht aus Windows 2016 Datacenter-Servern.

    >[!NOTE]
    >Weitere Informationen zu den unterstützten Plattformen für MIM 2016 finden Sie im Artikel [Unterstützte Plattformen für MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Schritte vor der Bereitstellung

    - [Erweitern des Schemas](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Erstellen von Dienstkonten

    - [Erstellen von Zertifikatvorlagen](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurieren von Kerberos

    - Datenbankschritte

        - SQL-Konfigurationsanforderungen

        - Datenbankberechtigungen

2. Bereitstellung

## <a name="pre-deployment-steps"></a>Schritte vor der Bereitstellung

Der MIM CM-Konfigurationsassistent erfordert Informationen, die bereitgestellt werden, damit er erfolgreich abgeschlossen werden kann.

![Diagramm](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Erweitern des Schemas

Der Prozess der Erweiterung des Schemas ist unkompliziert, aber er muss mit Vorsicht durchgeführt werden, da er nicht rückgängig gemacht werden kann.

>[!NOTE]
>Für diesen Schritt muss das verwendete Konto über Schemaadministratorrechte verfügen.

1. Navigieren Sie zum Speicherort der MIM-Medien und anschließend zum Ordner \\Certificate Management\\x64.

2. Kopieren Sie den Schemaordner nach „CORPDC“, und navigieren Sie anschließend dort hin.

    ![Diagramm](media/mim-cm-deploy/image005.png)

3. Führen Sie das Skript „resourceForestModifySchema.vbs“ in einem Ein-Struktur-Szenario aus. Führen Sie für das Szenario mit einer Ressourcengesamtstruktur folgende Skripts aus:
   - DomainA: Benutzer wurden gefunden (userForestModifySchema.vbs)
   - ResourceForestB: Speicherort der CM-Installation (resourceForestModifySchema.vbs)

     >[!NOTE]
     >Schemaänderungen sind ein unidirektionaler Vorgang und erfordern eine Wiederherstellung der Gesamtstruktur, damit ein Rollback ausgeführt werden kann. Stellen Sie daher sicher, dass Sie die nötigen Sicherungen vorgenommen haben. Weitere Informationen zu Änderungen des Schemas, die durch diesen Vorgang vorgenommen werden, finden Sie im Artikel [Forefront Identity Manager 2010 Certificate Management Schema Changes (Schemaänderungen von Forefront Identity Manager 2010 Certificate Management)](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx).

     ![Diagramm](media/mim-cm-deploy/image007.png)

4. Wenn Sie das Skript ausführen, sollten eine Erfolgsmeldung erhalten, sobald das Skript abgeschlossen wurde.

    ![Erfolgsmeldung](media/mim-cm-deploy/image009.png)

Das Schema in AD wurde erweitert, so dass es jetzt MIM CM unterstützt.

### <a name="creating-service-accounts-and-groups"></a>Erstellen von Dienstkonten und -gruppen

In der folgenden Tabelle werden die Konten und Berechtigungen, die für MIM CM erforderlich sind, zusammengefasst. Sie können MIM CM erlauben, automatisch die folgenden Konten zu erstellen, oder Sie können diese selbst vor der Installation erstellen. Die tatsächlichen Kontonamen können geändert werden. Wenn Sie die Konten tatsächlich manuell erstellen, sollten Sie die Benutzerkonten so benennen, dass man sie leicht ihrer Funktion zuordnen kann.

Benutzer:

![Diagramm](media/mim-cm-deploy/image010.png)

![Diagramm](media/mim-cm-deploy/image012.png)

| **Rolle**                   | **Anmeldename des Benutzers** |
|----------------------------|---------------------|
| MIM CM-Agent               | MIMCMAgent          |
| MIM CM-Key Recovery Agent  | MIMCMKRAgent        |
| MIM CM-Authorization Agent | MIMCMAuthAgent      |
| MIM CM-CA Manager Agent    | MIMCMManagerAgent   |
| MIM CM-Web Pool Agent      | MIMCMWebAgent       |
| MIM CM-Enrollment Agent    | MIMCMEnrollAgent    |
| MIM CM-Updatedienst      | MIMCMService        |
| MIM-Installationskonto        | MIMINSTALL          |
| Helpdesk-Agent            | CMHelpdesk1-2       |
| CM-Manager                 | CMManager1-2        |
| Abonnentenbenutzer            | CMUser1-2           |

Gruppen:

| **Rolle**               | **Gruppe**         |
|------------------------|-------------------|
| CM-Helpdeskmember    | MIMCM-Helpdesk    |
| CM-Managermember     | MIMCM-Managers    |
| CM-Abonnentenmember | MIMCM-Subscribers |

PowerShell: Agent-Konten:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Aktualisieren der lokalen Serverrichtlinie von **CORPCM** für Agentkonten 

| **Anmeldename des Benutzers** | **Beschreibung und Berechtigungen**   |
|------|---------------------|
| MIMCMAgent          | Stellt die folgenden Dienste bereit: </br>– Ruft verschlüsselte private Schlüssel aus der Zertifizierungsstelle ab </br>– Schützt Informationen über die Smartcard-PIN in der FIM CM-Datenbank </br>– Schützt die Kommunikation zwischen FIM CM und der Zertifizierungsstelle </br></br> Dieses Benutzerkonto erfordert die folgenden Einstellungen für die Zugriffssteuerung:</br>-    Die Benutzerberechtigung **Lokal anmelden zulassen**</br>-    Die Benutzerberechtigung **Ausstellen und Verwalten von Zertifikaten** </br>– Lese- und Schreibberechtigung im temporären Systemordner an folgendem Speicherort: %WINDIR%\\Temp.</br>– Eine digitale Signatur und ein Verschlüsselungszertifikat, die im Benutzerspeicher ausgestellt und installiert wurden
|MIMCMKRAgent        | Stellt archivierte private Schlüssel aus der Zertifizierungsstelle wieder her. Dieses Benutzerkonto erfordert die folgenden Einstellungen für die Zugriffssteuerung:</br> -    Die Benutzerberechtigung **Lokal anmelden zulassen**</br>– Mitgliedschaft in der lokalen **Administratorgruppe**. </br>– Registrierungsberechtigung für die Zertifikatvorlage **KeyRecoveryAgent**. </br>– Das Zertifikat des Key Recovery Agents wurde im Benutzerspeicher ausgestellt und installiert. Das Zertifikat muss der Liste des Key Recovery Agents in der Zertifizierungsstelle hinzugefügt werden. </br>– Lese- und Schreibberechtigung im temporären Systemordner an folgendem Speicherort: ```%WINDIR%\\Temp.```.                                                                                                                     |
| MIMCMAuthAgent      | Bestimmt Benutzerrechte und -berechtigungen für Benutzer und Gruppen. Dieses Benutzerkonto erfordert die folgenden Einstellungen für die Zugriffssteuerung: </br>– Mitgliedschaft in der Domänengruppe „Prä-Windows 2000 kompatibler Zugriff“ </br> – Verfügt über die Benutzerberechtigung **Generieren von Sicherheitsüberwachungen**             |
| MIMCMManagerAgent   | Führt die CA-Verwaltungsaktivitäten durch </br> Dieser Benutzer muss über die Berechtigung „Zertifizierungsstelle verwalten“ verfügen.        |
| MIMCMWebAgent       | Stellt die Identität für den IIS-Anwendungspool bereit FIM CM wird in einer Microsoft Win32®-Anwendungsprogrammierschnittstelle ausgeführt, die diese Anmeldeinformationen des Benutzers verwendet. </br> Dieses Benutzerkonto erfordert die folgenden Einstellungen für die Zugriffssteuerung:</br> – Mitgliedschaft in der lokalen Gruppe **IIS_WPG, Windows 2016 = IIS_IUSRS** </br>– Mitgliedschaft in der lokalen **Administratorgruppe**.</br>– Verfügt über die Benutzerberechtigung **Generieren von Sicherheitsüberwachungen** </br>– Verfügt über die Benutzerberechtigung **Einsetzen als Teil des Betriebssystems** </br>– Verfügt über die Benutzerberechtigung **Ersetzen eines Tokens auf Prozessebene**.</br>– Zugewiesen als Identität des IIS-Anwendungspools **CLMAppPool** </br>– Verfügt über die Leseberechtigung für den Registrierungsschlüssel **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser**. </br>– Dieses Konto muss auch für die Delegierung vertrauenswürdig sein.|
| MIMCMEnrollAgent    | Führt die Registrierung für einen Benutzer aus. Dieses Benutzerkonto erfordert die folgenden Einstellungen für die Zugriffssteuerung:</br>– Das Zertifikat des Registrierungs-Agents wurde im Benutzerspeicher ausgestellt und installiert.</br>-    Die Benutzerberechtigung **Lokal anmelden zulassen** </br>– Registrieren der Berechtigung für die Zertifikatvorlage **Registrierungs-Agent** (oder die benutzerdefinierte Vorlage, sofern eine verwendet wird).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Erstellen von Zertifikatvorlagen für die MIM CM-Dienstkonten

Drei der Dienstkonten, die von MIM CM verwendet werden, erfordern ein Zertifikat. Der Konfigurationsassistent erfordert, dass Sie den Namen der Zertifikatvorlagen bereitstellen, der zum Anfordern von Zertifikaten für diese verwendet werden soll.

Die Dienstkonten, die Zertifikate erfordern, sind:

- MIMCMAgent: Dieses Konto erfordert ein Benutzerzertifikat.

- MIMCMEnrollAgent: Dieses Konto erfordert ein Zertifikat des Registrierungs-Agents.

- MIMCMKRAgent: Dieses Konto erfordert das Zertifikat **Key Recovery Agent**.

In AD stehen Vorlagen zur Verfügung. Sie müssen allerdings Ihre eigenen Versionen erstellen, die in MIM CM funktionieren, da Sie Änderungen an den Grundvorlagen vornehmen müssen.

Alle drei oben aufgeführten Konten verfügen über erweiterte Rechte innerhalb Ihrer Organisation und sollten mit Vorsicht verwendet werden.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Erstellen der MIM CM-Signaturzertifikatvorlage

1. Öffnen Sie unter **Verwaltung** **Zertifizierungsstelle**.

2. Erweitern Sie **Contoso-CorpCA** in der Konsole **Zertifizierungsstelle** in der Konsolenstruktur, und klicken Sie anschließend auf **Zertifikatvorlagen**.

3. Klicken Sie mit der rechten Maustaste auf **Zertifikatvorlagen**, und klicken Sie dann auf **Verwalten**.

4. Klicken Sie mit der rechten Maustaste in der **Zertifikatvorlagenkonsole** im Bereich **Details** auf **Benutzer**, und klicken Sie anschließend auf **Doppelte Vorlage**.

5. Klicken Sie im Dialogfeld **Doppelte Vorlage** auf **Windows Server 2003 Enterprise** und anschließend auf **OK**.

    ![Anzeigen der daraus entstehenden Änderungen](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM funktioniert nicht mit Zertifikaten, die auf Zertifikatvorlagen der Version 3 basieren. Sie müssen eine Windows Server® 2003 Enterprise-Zertifikatvorlage (Version 2) erstellen. Weitere Informationen finden Sie unter [FAQ for FIM 2010 to support SHA2, KSP/CNG and v3 certificate templates for issuing user and agent certificates and MIM 2016 upgrade (FAQ für FIM 2010 zur Unterstützung von SHA2-, KSP/CNG- und v3-Zertifikatvorlagen zum Veröffentlichen von Benutzer- und Agent-Zertifikaten und zum Upgraden von MIM 2016)](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade).

6. Geben Sie im Dialogfeld **Eigenschaften der neuen Vorlage** auf der Registerkarte **Allgemein** im Feld **Vorlagenanzeigename** **MIM CM signieren** ein. Ändern Sie den **Gültigkeitszeitraum** in **2 Jahre**, und deaktivieren Sie dann das Kontrollkästchen **Zertifikat in Active Directory veröffentlichen**.

7. Stellen Sie auf der Registerkarte **Anforderungsverarbeitung** sicher, dass das Kontrollkästchen **Exportieren von privatem Schlüssel zulassen** aktiviert ist, und klicken Sie anschließend auf die Registerkarte **Kryptografie**.

8. Deaktivieren Sie im Dialogfeld **Cryptography Selection** (Kryptografieauswahl) den **Microsoft Enhanced Cryptographic Provider v1.0**, aktivieren Sie den **Microsoft Enhanced RSA und AES Cryptographic Provider**, und klicken Sie anschließend auf **OK**.

9. Deaktivieren Sie auf der Registerkarte **Antragstellername** die Kontrollkästchen **E-Mail-Name im Antragstellernamen** und **E-Mail-Name**.

10. Stellen Sie auf der Registerkarte **Erweiterungen** in der Liste **Erweiterungen in dieser Vorlage** sicher, dass **Anwendungsrichtlinien** ausgewählt ist, und klicken Sie anschließend auf **Bearbeiten**.

11. Klicken Sie im Dialogfeld **Anwendungsrichtlinienerweiterung bearbeiten** auf die Anwendungsrichtlinien **Verschlüsselndes Dateisystem** und **Sichere E-Mail**. Klicken Sie auf **Entfernen** und dann auf **OK**.

12. Führen Sie auf der Registerkarte **Sicherheit** die folgenden Schritte aus:

    - Entfernen Sie den **Administrator**.

    - Entfernen Sie die **Domänenadministratoren**.

    - Entfernen Sie die **Domänenbenutzer**.

    - Weisen Sie den **Organisationsadministratoren** nur **Lese-** und **Schreibberechtigungen** zu.

    - Fügen Sie**MIMCMAgent** hinzu.

    - Weisen Sie **MIMCMAgent** **Lese-** und **Registrierungsberechtigungen** zu.

13. Klicken Sie im Dialogfeld **Eigenschaften der neuen Vorlage** auf **OK**.

14. Lassen Sie die **Zertifikatvorlagenkonsole** geöffnet.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Erstellen der Zertifikatvorlage des MIM CM-Enrollment Agents

1. Klicken Sie mit der rechten Maustaste in der **Zertifikatvorlagenkonsole** im Bereich **Details** auf **Enrollment Agent**, und klicken Sie anschließend auf **Doppelte Vorlage**.

2. Klicken Sie im Dialogfeld **Doppelte Vorlage** auf **Windows Server 2003 Enterprise** und anschließend auf **OK**.

3. Geben Sie im Dialogfeld **Eigenschaften der neuen Vorlage** auf der Registerkarte **Allgemein** im Feld **Vorlagenanzeigename** **MIM CM-Enrollment Agent** ein. Stellen Sie sicher, dass der **Gültigkeitszeitraum** **2 Jahre** beträgt.

4. Aktivieren Sie auf der Registerkarte **Anforderungsverarbeitung** die Option **Exportieren von privatem Schlüssel zulassen**, und klicken Sie anschließend auf die Registerkarte **Krytpografiedienstanbieter oder Kryptografie**.

5. Deaktivieren Sie im Dialogfeld **Kryptografiedienstanbieter auswählen** den **Microsoft Base Cryptographic Provider v1.0** und den **Microsoft Enhanced Cryptographic Provider v1.0**, aktivieren Sie den **Microsoft Enhanced RSA und AES Cryptographic Provider**, und klicken Sie anschließend auf **OK**.

6. Führen Sie auf der Registerkarte **Sicherheit** Folgendes aus:

    - Entfernen Sie den **Administrator**.

    - Entfernen Sie die **Domänenadministratoren**.

    - Weisen Sie den **Organisationsadministratoren** nur **Lese-** und **Schreibberechtigungen** zu.

    - Fügen Sie **MIMCMEnrollAgent** hinzu.

    - Weisen Sie **MIMCMEnrollAgent** **Lese-** und **Registrierungsberechtigungen** zu.

7. Klicken Sie im Dialogfeld **Eigenschaften der neuen Vorlage** auf **OK**.

8. Lassen Sie die **Zertifikatvorlagenkonsole** geöffnet.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Erstellen der Zertifikatvorlage des MIM CM-Key Recovery Agents

1. Klicken Sie mit der rechten Maustaste in der Konsole **Zertifikatvorlagen** im Bereich **Details** auf **Key Recovery Agent**, und klicken Sie anschließend auf **Doppelte Vorlage**.

2. Klicken Sie im Dialogfeld **Doppelte Vorlage** auf **Windows Server 2003 Enterprise** und anschließend auf **OK**.

3. Geben Sie im Dialogfeld **Eigenschaften der neuen Vorlage** auf der Registerkarte **Allgemein** im Feld **Vorlagenanzeigename** **MIM CM-Key Recovery Agent** ein. Stellen Sie sicher, dass der **Gültigkeitszeitraum** auf der Registerkarte **Kryptografie** **2 Jahre** beträgt.

4. Deaktivieren Sie im Dialogfeld **Providers Selection** (Anbieterauswahl) den **Microsoft Enhanced Cryptographic Provider v1.0**, aktivieren Sie den **Microsoft Enhanced RSA und AES Cryptographic Provider**, und klicken Sie anschließend auf **OK**.

5. Stellen Sie auf der Registerkarte **Ausstellungsvoraussetzungen** sicher, dass die **Genehmigung von Zertifikatverwaltung der Zertifizierungsstelle** **deaktiviert** ist.

6. Führen Sie auf der Registerkarte **Sicherheit** Folgendes aus:

    - Entfernen Sie den **Administrator**.

    - Entfernen Sie die **Domänenadministratoren**.

    - Weisen Sie den **Organisationsadministratoren** nur **Lese-** und **Schreibberechtigungen** zu.

    - Fügen Sie **MIMCMKRAgent** hinzu.

    - Weisen Sie **KRAgent** **Lese-** und **Registrierungsberechtigungen** zu.

7. Klicken Sie im Dialogfeld **Eigenschaften der neuen Vorlage** auf **OK**.

8. Schließen Sie die **Zertifikatvorlagenkonsole**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Veröffentlichen der erforderlichen Zertifikatvorlagen in der Zertifizierungsstelle

1. Stellen Sie die Konsole **Zertifizierungsstelle** wieder her.

2. Klicken Sie in der Konsole **Zertifizierungsstelle** in der Konsolenstruktur mit der rechten Maustaste auf **Zertifikatvorlagen**, zeigen Sie auf **Neu** und klicken Sie anschließend auf **Auszustellende Zertifikatvorlage**.

3. Klicken Sie im Dialogfeld **Aktivieren von Zertifikatvorlagen** auf **MIM CM-Enrollment Agent**, **MIM CM-Key Recovery Agent** und **MIM CM Signing** (MIM CM signieren). Klicken Sie auf **OK**.

4. Klicken Sie in der Konsolenstruktur auf **Zertifikatvorlagen**.

5. Überprüfen Sie, ob die drei neuen Vorlagen im Bereich **Details** angezeigt werden, und schließen Sie die **Zertifizierungsstelle**.

    ![MIM CM-Signierung](media/mim-cm-deploy/image016.png)

6. Schließen Sie alle geöffneten Fenster, und melden Sie sich ab.

### <a name="iis-configuration"></a>IIS-Konfiguration

Um die Website für CM zu hosten, installieren und konfigurieren Sie IIS.

#### <a name="install-and-configure-iis"></a>Installieren und Konfigurieren von IIS

1. Melden Sie sich bei **CORLog in** als **MIMINSTALL** an.

    >[!IMPORTANT]
    >Das MIM-Installationskonto sollte ein lokaler Administrator sein.

2. Öffnen Sie PowerShell als Administrator, und führen Sie den folgenden Befehl aus.

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Eine Website mit dem Namen „Default Web Site“ (Standardwebsite) wird standardmäßig mit IIS 7 installiert. Wenn diese Website umbenannt oder entfernt wurde, muss eine Website mit dem Namen „Standardwebsite“ vorhanden sein, bevor MIM CM installiert werden kann.

#### <a name="configuring-kerberos"></a>Konfigurieren von Kerberos

Das Konto „MIMCMWebAgent“ führt das MIM CM-Portal aus. In IIS und höher ist die Authentifizierung für den Kernelmodus standardmäßig aktiviert. Sie deaktivieren die Kerberos-Kernelmodusauthentifizierung und konfigurieren stattdessen SPNs für das Konto MIMCMWebAgent. Einige Befehle erfordern erweiterte Berechtigungen auf dem Server von Active Directory und CORPCM.

![Diagramm](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Aktualisieren von IIS auf CORPCM**

![Diagramm](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Sie müssen einen DNS-A-Eintrag für „cm.contoso.com“ hinzufügen und auf „CORPCM IP“ zeigen.

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Erfordern von SSL im MIM CM-Portal

Es wird dringend empfohlen, dass Sie SSL im MIM CM-Portal erforderlich machen. Wenn Sie dies nicht tun, wird der Assistent Sie darauf hinweisen.

1. Registrieren Sie sich im Webzertifikat für **cm.contoso.com**, das der Standardwebsite zugewiesen ist.

2. Öffnen Sie den **IIS-Manager**, und navigieren Sie zur **Zertifikatverwaltung**.

3. Doppelklicken Sie in der Featureansicht auf die SSL-Einstellungen.

4. Klicken Sie auf der Seite SSL-Einstellungen auf die Option **SSL erforderlich**.

5. Klicken Sie im Aktionsbereich auf **Anwenden**.

### <a name="database-configuration-corpsql-for-mim-cm"></a>Datenbankkonfiguration **CORPSQL** für MIM CM

1. Stellen Sie sicher, dass eine Verbindung zum CORPSQL01-Server vorhanden ist.

2. Stellen Sie sicher, dass Sie als SQL-Datenbankadministrator angemeldet sind.

3. Führen Sie das folgende T-SQL-Skript aus, um dem Konto CONTOSO\\MIMINSTALL die Erstellung der Datenbank zu erlauben, wenn Sie zum Konfigurationsschritt übergehen.

    >[!NOTE]
    >Wir müssen zu SQL zurückkehren, sobald Sie für das Modul „Beenden & Richtlinie“ bereit sind.

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Fehlermeldung des MIM CM-Konfigurationassistenten](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Bereitstellen der Microsoft Identity Manager 2016-Zertifikatverwaltung

1. Stellen Sie sicher, dass Sie mit dem CORPCM-Server verbunden sind und dass das **MIMINSTALL**-Konto ein Mitglied der Gruppe **Lokale Administratoren** ist.

2. Stellen Sie sicher, dass Sie als Contoso\\MIMINSTALL angemeldet sind.

3. Legen Sie Microsoft Identity Manager SP1 ISO ein.

4. **Öffnen** Sie das Verzeichnis **Certificate Management\\x64**.

5. Klicken Sie im Fenster **x64** mit der rechten Maustaste auf **Setup**, und klicken Sie anschließend auf **Als Administrator ausführen**.

6. Klicken Sie auf der Seite „Welcome to the Microsoft Identity Manager Certificate Management Setup Wizard“ (Willkommen beim Setupassistenten der Microsoft Identity Manager-Zertifikatverwaltung) auf **Weiter**.

7. Lesen Sie auf der Seite „Benutzerlizenzvertrag“ den Vertrag, aktivieren Sie das **Kontrollkästchen** „Ich stimme den Lizenzbedingungen zu“, und klicken Sie anschließend auf „Weiter“.

8. Vergewissern Sie sich auf der Seite „Benutzerdefinierte Installation“, dass das **MIM CM-Portal** und die **MIM CM-Aktualisierungsdienstkomponenten** zur Installation festgelegt sind, und **klicken Sie auf „Weiter“** .

9. Stellen Sie auf der Seite „Virtueller Webordner“ sicher, dass der Name des virtuellen Ordners **CertificateManagement** ist, und **klicken Sie auf „Weiter“** .

10. Klicken Sie auf der Seite „Microsoft Identity Manager-Zertifikatverwaltung installieren“ auf **Installieren**.

11. Klicken Sie auf der Seite „Setupassistent der Microsoft Identity Manager-Zertifikatverwaltung **abgeschlossen**“ auf **Fertig stellen**.

![MIM CM-Assistent abgeschlossen](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Konfigurationsassistent der Microsoft Identity Manager 2016-Zertifikatverwaltung

Fügen Sie MIMINSTALL für den Konfigurationsassistenten vor der Registrierung in CORPCM der Gruppe **Domänenadministratoren, Schemaadministratoren und lokale Administratoren** hinzu. Dies kann später entfernt werden, sobald die Konfiguration abgeschlossen ist.

![Fehlermeldung](media/mim-cm-deploy/image028.png)

1. Klicken Sie im Menü **Start** auf **Konfigurations-Assistent für die Zertifikatverwaltung**. Führen Sie ihn als **Administrator** aus.

2. Klicken Sie auf der Seite **Willkommen** auf **Weiter**.

3. Stellen Sie auf der Seite **Zertifizierungsstellenkonfiguration** sicher, dass **Contoso-CORPCA-CA** die ausgewählte ZS und **CORPCA.CONTOSO.COM** der ausgewählte Server ist, und klicken Sie anschließend auf **Weiter**.

4. Geben Sie auf der Seite **Set up the Microsoft® SQL Server® Database** (Installieren der Microsoft® SQL Server®-Datenbank) im Feld **SQL Server-Name** **CORPSQL1** ein, aktivieren Sie das Kontrollkästchen **Meine Anmeldeinformationen zum Erstellen der Datenbank verwenden**, und klicken Sie anschließend auf **Weiter**.

5. Akzeptieren Sie auf der Seite **Datenbankeinstellungen** den Standarddatenbanknamen von **FIMCertificateManagement**, stellen Sie sicher, dass **Integrierte SQL-Authentifizierung** ausgewählt ist, und klicken Sie anschließend auf **Weiter**.

6. Akzeptieren Sie auf der Seite **Einrichten von Active Directory** den bereitgestellten Standardnamen für den Dienstverbindungspunkt, und klicken Sie auf **Weiter**.

7. Bestätigen Sie auf der Seite **Authentifizierungsmethode**, dass **Integrierte Windows-Authentifizierung** ausgewählt ist, und klicken Sie auf **Weiter**.

8. Deaktivieren Sie auf der Seite **Agents – FIM CM** das Kontrollkästchen **FIM CM-Standardeinstellungen verwenden**, und klicken Sie anschließend auf **Benutzerdefinierte Konten**.

9. Geben Sie auf jeder Registerkarte im Dialogfeld **Agents – FIM CM** mit mehreren Registerkarten folgende Informationen ein:

   - Benutzername: **Update**

   - Kennwort: **Pass\@word1**

   - Kennwort bestätigen: **Pass\@word1**

   - Vorhandenen Benutzer verwenden: **Aktiviert**

     >[!NOTE]
     >Diese Konten haben Sie zuvor erstellt. Stellen Sie sicher, dass die Prozeduren in Schritt 8 für alle sechs Agentkontoregisterkarten wiederholt werden.

     ![MIM CM-Konten](media/mim-cm-deploy/image030.png)

10. Wenn alle Agentkontoinformationen abgeschlossen sind, klicken Sie auf **OK**.

11. Klicken Sie auf der Seite **Agents – MIM CM** auf **Weiter**.

12. Aktivieren Sie auf der Seite **Serverzertifikate einrichten** die folgenden Zertifikatvorlagen:

    - Zertifikatvorlage, die für das Zertifikat des Key Recovery Agents verwendet werden soll: **MIMCMKeyRecoveryAgent**.

    - Zertifikatvorlage, die für das Zertifikat des FIM CM-Agents verwendet werden soll: **MIMCMSigning**.

    - Zertifikatvorlage, die für das Zertifikat des Enrollment Agents verwendet werden soll: **FIMCMEnrollmentAgent**.

13. Klicken Sie auf der Seite **Serverzertifikate einrichten** auf **Weiter**.

14. Klicken Sie auf der Seite **E-Mail-Server, Dokumentdruck einrichten** im Feld **Geben Sie den Namen des SMTP-Servers an, der zum Senden von E-Mail-Registrierungsbenachrichtigungen verwendet werden soll** auf **Weiter**.

15. Klicken Sie auf der Seite **Bereit für die Konfiguration** auf **Konfigurieren**.

16. Klicken Sie in der Warnmeldung **Konfigurationsassistent – Microsoft Forefront Identity Manager 2010 R2** auf **OK**, um zu bestätigen, dass SSL nicht auf dem virtuellen IIS-Verzeichnis aktiviert ist.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Klicken Sie nicht auf „Fertig stellen“, bevor die Ausführung des Konfigurationsassistenten abgeschlossen ist. Die Protokollierung für den Assistenten können Sie hier finden: **%programfiles%\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log**

17. Klicken Sie auf **Fertig stellen**.

    ![MIM CM-Assistent abgeschlossen](media/mim-cm-deploy/image033.png)

18. Schließen Sie alle geöffneten Fenster.

19. Fügen Sie `https://cm.contoso.com/certificatemanagement` der lokalen Intranetzone in Ihrem Browser hinzu.

20. Besuchen Sie die folgende Website vom Server CORPCM: `https://cm.contoso.com/certificatemanagement`  

    ![Diagramm](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Überprüfen des CNG-Schlüsselisolationsdienstes

1. Öffnen Sie in der **Verwaltung** die **Dienste**.

2. Doppelklicken Sie im Bereich **Details** auf **CNG-Schlüsselisolation**.

3. Ändern Sie auf der Registerkarte **Allgemein** die Einstellung von **Starttyp** in **Automatisch**.

4. Starten Sie auf der Registerkarte **Allgemein** den Dienst, wenn er sich nicht im gestarteten Status befindet.

5. Klicken Sie auf der Registerkarte **Allgemein** auf **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Installieren und Konfigurieren der ZS-Module:

In diesem Schritt installieren und konfigurieren Sie die FIM CM-Zertifizierungsstellenmodule in der Zertifizierungsstelle.

1. Konfigurieren Sie FIM CM, um Benutzerberechtigungen nur für Verwaltungsvorgänge zu überprüfen.

2. Machen Sie im Fenster **C:\\Programme\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web** eine Kopie von **web.config**, und nennen Sie diese Kopie **web.1.config**.

3. Klicken Sie im Fenster **Web** mit der rechten Maustaste **Web.config**, und klicken Sie anschließend auf **Öffnen**.

    >[!Note]
    >Die Datei „Web.config“ wird im Editor geöffnet.

4. Wenn die Datei geöffnet wird, drücken Sie STRG + F.

5. Geben Sie im Dialogfeld **Suchen und Ersetzen** im Feld **Suchen nach** **UseUser** ein, und klicken Sie anschließend drei Mal auf **Weitersuchen**.

6. Schließen Sie das Dialogfeld **Suchen und Ersetzen**.

7. Sie sollten sich in der Zeile **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>** befinden. Ändern Sie die Zeile in **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>** .

8. Speichern Sie alle Änderungen, und schließen Sie die Datei.

9. Erstellen Sie ein Konto für die ZS-Computer auf dem SQL Server \<ohne Skript\>.

10. Stellen Sie sicher, dass eine Verbindung zum **CORPSQL01**-Server vorhanden ist.

11. Stellen Sie sicher, dass Sie als **Datenbankadministrator** angemeldet sind.

12. Starten Sie **SQL Server Management Studio** über das Menü **Start**.

13. Geben Sie im Dialogfeld **Mit Server verbinden** im Feld **Servername** **CORPSQL01** ein, und klicken Sie anschließend auf **Verbinden**.

14. Erweitern Sie in der Konsolenstruktur **Sicherheit**, und klicken Sie auf **Anmeldungen**.

15. Klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie dann auf **Neue Anmeldung**.

16. Geben Sie auf der Registerkarte **Allgemein** im Feld **Anmeldename** **contoso\\CORPCA\$** ein. Klicken Sie auf **Windows-Authentifizierung**. Die Standarddatenbank ist **FIMCertificateManagement**.

17. Klicken Sie im linken Bereich auf **Benutzerzuordnung**. Klicken Sie im rechten Bereich auf das Kontrollkästchen in der Spalte **Zuordnung** neben **FIMCertificateManagement**. Aktivieren Sie in der Liste **Mitgliedschaft in Datenbankrolle für: FIMCertificateManagement** die Rolle **clmApp**.

18. Klicken Sie auf **OK**.

19. Beenden Sie **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Installieren der FIM CM-Zertifizierungsstellenmodule in der Zertifizierungsstelle

1. Stellen Sie sicher, dass eine Verbindung zum **CORPCA**-Server vorhanden ist.

2. Klicken Sie mit der rechten Maustaste in den Fenstern **x64** auf **Setup.exe**, und klicken Sie anschließend auf **Als Administrator ausführen**.

3. Klicken Sie auf der Seite **Welcome to the Microsoft Identity Manager Certificate Management Setup Wizard** (Willkommen beim Setupassistenten der Microsoft Identity Manager-Zertifikatverwaltung) auf **Weiter**.

4. Lesen Sie den Vertrag auf der Seite **Benutzerlizenzvertrag**. Aktivieren Sie das Kontrollkästchen **Ich stimme den Bedingungen des Lizenzvertrags zu**, und klicken Sie auf **Weiter**.

5. Klicken Sie auf der Seite **Benutzerdefinierte Installation** auf **MIM CM-Portal** und anschließend auf **Dieses Feature steht nicht zur Verfügung**.

6. Klicken Sie auf der Seite **Benutzerdefinierte Installation** auf **MIM CM-Aktualisierungsdienst** und anschließend auf **Dieses Feature steht nicht zur Verfügung**.

    >[!Note]
    >Dadurch bleiben die ZS-Dateien von MIM CM die einzige Funktion, die für die Installation aktiviert ist.

7. Klicken sie auf der Seite **Benutzerdefinierte Installation** auf **Weiter**.

8. Klicken Sie auf der Seite **Install Microsoft Identity Manager Certificate Management** (Microsoft Identity Manager-Zertifikatverwaltung installieren) auf **Installieren**.

9. Klicken Sie auf der Seite **Completed the Microsoft Identity Manager Certificate Management Setup Wizard** (Setupassistent der Microsoft Identity Manager-Zertifikatverwaltung abgeschlossen) auf **Fertig stellen**.

10. Schließen Sie alle geöffneten Fenster.

### <a name="configure-the-mim-cm-exit-module"></a>Konfigurieren des MIM CM-Beendigungsmoduls

1. Öffnen Sie unter **Verwaltung** **Zertifizierungsstelle**.

2. Klicken Sie in der Konsolenstruktur auf **contoso-CORPCA-CA**, und klicken Sie anschließend auf **Eigenschaften**.

3. Klicken Sie auf der Registerkarte **Beendigungsmodul** auf **FIM CM-Beendigungsmodul** und dann auf **Eigenschaften**.

4. Geben Sie im Feld **Verbindungszeichenfolge für FIM CM-Datenbank angeben** **Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01** ein. Lassen Sie das Kontrollkästchen **Verbindungszeichenfolge verschlüsseln** aktiviert, und klicken Sie anschließend auf **OK**.
5. Klicken Sie im Meldungsfeld **Microsoft FIM-Zertifikatverwaltung** auf **OK**.

6. Klicken Sie im Dialogfeld **Eigenschaften von contoso-CORPCA-CA** auf **OK**.

7. Klicken Sie mit der rechten Maustaste auf **contoso-CORPCA-CA** *,* zeigen Sie auf **Alle Tasks**, und klicken Sie anschließend auf **Dienst beenden**. Warten Sie, bis die Active Directory-Zertifikatdienste beendet werden.

8. Klicken Sie mit der rechten Maustaste auf **contoso-CORPCA-CA** *,* zeigen Sie auf **Alle Tasks**, und klicken Sie anschließend auf **Dienst starten**.

9. Minimieren Sie die Konsole **Zertifizierungsstelle**.

10. Öffnen Sie in der **Verwaltung** die **Ereignisanzeige**.

11. Erweitern Sie in der Konsolenstruktur **Anwendungs- und Dienstprotokolle**, und klicken Sie anschließend auf **FIM-Zertifikatverwaltung**.

12. Überprüfen Sie in der Liste der Ereignisse, ob die neuesten Ereignisse seit dem letzten Neustart der Zertifikatdienste *keine***Warnungs-** oder **Fehlerereignisse** einschließen.

    >[!NOTE] 
    >Das letzte Ereignis sollte angeben, dass das Beendigungsmodul mithilfe der Einstellungen aus `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit` geladen wird.

13. Minimieren Sie die **Ereignisanzeige**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Kopieren Sie den MIMCMAgent-Zertifikatfingerabdruck in die Windows®-Zwischenablage.

1. Stellen Sie die Konsole **Zertifizierungsstelle** wieder her.

2. Erweitern Sie in der Konsolenstruktur **contoso-CORPCA-CA**, und klicken Sie anschließend auf **Ausgestellte Zertifikate**.

3. Doppelklicken Sie im Bereich **Details** auf das Zertifikat, das **CONTOSO\\MIMCMAgent** in der Spalte **Name des Auftraggebers** aufweist, und **FIM CM Signing** (FIM CM signieren) in der Spalte **Zertifikatvorlage**.

4. Wählen Sie auf der Registerkarte **Details** das Feld **Fingerabdruck** aus.

5. Wählen Sie den Fingerabdruck aus, und drücken Sie auf STRG + C.

    >[!NOTE]
    >Schließen Sie **kein** führendes Leerzeichen in der Liste der Fingerabdruckzeichen ein.

6. Klicken Sie im Dialogfeld **Zertifikat** auf **OK**.

7. Geben Sie im Menü **Start** im Feld **Programme/Dateien durchsuchen** **Editor** ein, und drücken Sie dann die EINGABETASTE.

8. Klicken Sie im **Editor** im Menü **Bearbeiten** auf **Einfügen**.

9. Klicken Sie im Menü **Bearbeiten** auf **Ersetzen**.

10. Geben Sie im Feld **Suchen nach** ein Leerzeichen ein, und klicken Sie anschließend auf **Alle ersetzen**.

    >[!Note]
    >Dies entfernt alle Leerzeichen zwischen den Zeichen im Fingerabdruck.

11. Klicken Sie im Dialogfeld **Ersetzen** auf **Abbrechen**.

12. Wählen Sie die konvertierte Zeichenfolge *thumbprintstring* aus, und drücken Sie STRG + C.

13. Schließen Sie den **Editor**, ohne die Änderungen zu speichern.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurieren des FIM CM-Richtlinienmoduls

1. Stellen Sie die Konsole **Zertifizierungsstelle** wieder her.

2. Klicken Sie mit der rechten Maustaste auf **contoso-CORPCA-CA**, und klicken Sie anschließend auf **Eigenschaften**.

3. Klicken Sie im Dialogfeld **Eigenschaften von contoso-CORPCA-CA** auf der Registerkarte **Richtlinienmodul** auf **Eigenschaften**.

    - Stellen Sie auf der Registerkarte **Allgemein** sicher, dass **Nicht-FIM CM-Anforderungen zur Verarbeitung an das Standardrichtlinienmodul übergeben** ausgewählt ist.

    - Klicken Sie auf der Registerkarte **Signaturzertifikate** auf **Hinzufügen**.

    - Klicken Sie im Dialogfeld „Zertifikat“ mit der rechten Maustaste auf das Feld **Geben Sie den hexadezimal-codierten Zertifikathash an**, und klicken Sie anschließend auf **Einfügen**.

    - Klicken Sie im Dialogfeld **Zertifikat** auf **OK**.

        >[!Note]
        >Wenn die Schaltfläche **OK** nicht aktiviert ist, haben Sie versehentlich ein ausgeblendetes Zeichen in der Fingerabdruckzeichenfolge eingeschlossen, als Sie den Fingerabdruck aus dem clmAgent-Zertifikat kopiert haben. Wiederholen Sie ab **Aufgabe 4: Kopieren Sie den MIMCMAgent-Zertifikatfingerabdruck in die Windows-Zwischenablage** alle Schritte in dieser Übung.

4. Stellen Sie im Dialogfeld **Konfigurationseigenschaften** sicher, dass der Fingerabdruck in der Liste **Valid Signing Certificates** (Gültige Signaturzertifikate) erscheint, und klicken Sie auf **OK**.

5. Klicken Sie im Meldungsfeld **FIM-Zertifikatverwaltung** auf **OK**.

6. Klicken Sie im Dialogfeld **Eigenschaften von contoso-CORPCA-CA** auf **OK**.

7. Klicken Sie mit der rechten Maustaste auf **contoso-CORPCA-CA** *,* zeigen Sie auf **Alle Tasks**, und klicken Sie anschließend auf **Dienst beenden**.

8. Warten Sie, bis die Active Directory-Zertifikatdienste beendet werden.

9. Klicken Sie mit der rechten Maustaste auf **contoso-CORPCA-CA** *,* zeigen Sie auf **Alle Tasks**, und klicken Sie anschließend auf **Dienst starten**.

10. Schließen Sie die Konsole **Zertifizierungsstelle**.

11. Schließen Sie alle geöffneten Fenster und melden Sie sich anschließend ab.

**Letzter Schritt in der Bereitstellung**: Wir möchten sicherstellen, dass CONTOSO\\MIMCM-Manager Vorlagen bereitstellen und erstellen und das System konfigurieren können, ohne dass sie Schema- und Domänenadministratoren sind. Das nächste Skript fügt die Berechtigungen mithilfe von dsacls der ACL der Zertifikatvorlagen hinzu. Führen Sie mit dem Konto, das über umfassende Berechtigung zum Ändern der Sicherheit verfügt, Lese- und Schreibberechtigungen für jede vorhandene Zertifikatvorlage in der Gesamtstruktur aus.

Erste Schritte: **Konfigurieren des Dienstverbindungspunkts und von Zielgruppenberechtigungen & Delegieren der Profilvorlagenverwaltung**

1. Konfigurieren Sie die Berechtigungen auf dem Dienstverbindungspunkt (SCP).

2. Konfigurieren Sie die delegierte Profilvorlagenverwaltung.

3. Konfigurieren Sie die Berechtigungen auf dem Dienstverbindungspunkt (SCP). **\<ohne Skript\>**

4.   Stellen Sie sicher, dass eine Verbindung zum **CORPDC**-Server vorhanden ist.

5. Melden Sie sich als **contoso\\corpadmin** an.

6. Öffnen Sie in der **Verwaltung** die **Active Directory-Benutzer und-Computer**.

7. Stellen Sie unter **Active Directory-Benutzer und-Computer** im Menü **Ansicht** sicher, dass **Erweiterte Features** aktiviert sind.

8. Erweitern Sie in der Konsolenstruktur **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager**, und klicken Sie anschließend auf **CORPCM**.

9. Klicken Sie mit der rechten Maustaste auf **CORPCM**, und klicken Sie auf **Eigenschaften**.

10. Fügen Sie im Dialogfeld **CORPCM-Eigenschaften** auf der Registerkarte **Sicherheit** die folgenden Gruppen mit den entsprechenden Berechtigungen hinzu:

    | Group          | Berechtigungen      |
    |----------------|------------------|
    | mimcm-Manager | Siehe </br> FIM CM Audit</br> FIM CM-Enrollment Agent</br> FIM CM Request Enroll</br> FIM CM Request Recover</br> FIM CM Request Renew</br> FIM CM Request Revoke </br> FIM CM Request Unblock Smart Card |
    | mimcm-HelpDesk | Siehe</br> FIM CM-Enrollment Agent</br> FIM CM Request Revoke</br> FIM CM Request Unblock Smart Card |

11. Klicken Sie im Dialogfeld **CORPDC-Eigenschaften** auf **OK**.

12. Lassen Sie **Active Directory-Benutzer und-Computer** geöffnet.

**Konfigurieren von Berechtigungen für die untergeordneten Benutzerobjekte**

1. Stellen Sie sicher, dass Sie sich noch in der Konsole **Active Directory-Benutzer und -Computer** befinden.

2. Klicken Sie in der Konsolenstruktur mit der rechten Maustaste auf **Contoso.com**, und klicken Sie anschließend auf **Eigenschaften**.

3. Klicken Sie auf der Registerkarte **Sicherheit** auf **Erweitert**.

4. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für Contoso** auf **Hinzufügen**.

5. Geben Sie im Dialogfeld **Select User, Computer, Service Account, or Group** (Benutzer, Computer, Dienstkonto oder Gruppe auswählen) im Feld **Namen des auszuwählenden Objekts eingeben** **mimcm-Managers** ein, und klicken Sie anschließend auf **OK**.

6. Klicken Sie im Dialogfeld **Berechtigungseintrag für Contoso** in der Liste **Übernehmen für** auf **Untergeordnete Benutzerobjekte**, und aktivieren Sie anschließend das Kontrollkästchen **Zulassen** für die folgenden **Berechtigungen**:

    - **Alle Eigenschaften lesen**

    - **Berechtigungen lesen**

    - **FIM CM Audit**

    - **FIM CM-Enrollment Agent**

    - **FIM CM Request Enroll**

    - **FIM CM Request Recover**

    - **FIM CM Request Renew**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

7. Klicken Sie im Dialogfeld **Berechtigungseintrag für Contoso** auf **OK**.

8. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für Contoso** auf **Hinzufügen**.

9. Geben Sie im Dialogfeld **Select User, Computer, Service Account, or Group** (Benutzer, Computer, Dienstkonto oder Gruppe auswählen) im Feld **Namen des auszuwählenden Objekts eingeben** **mimcm-HelpDesk** ein, und klicken Sie anschließend auf **OK**.

10. Klicken Sie im Dialogfeld **Permission Entry for Contoso** (Berechtigungseintrag für Contoso) in der Liste **Anwenden auf** auf **Descendant User objects** (Untergeordnete Benutzerobjekte), und aktivieren Sie anschließend das Kontrollkästchen **Zulassen** für die folgenden **Berechtigungen**:

    - **Alle Eigenschaften lesen**

    - **Berechtigungen lesen**

    - **FIM CM-Enrollment Agent**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

11. Klicken Sie im Dialogfeld **Berechtigungseintrag für Contoso** auf **OK**.

12. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für Contoso** auf **OK**.

13. Klicken Sie im Dialogfeld **contoso.com-Eigenschaften** auf **OK**.

14. Lassen Sie **Active Directory-Benutzer und-Computer** geöffnet.

**Konfigurieren von Berechtigungen für die untergeordneten Benutzerobjekte \<ohne Skript\>**

1. Stellen Sie sicher, dass Sie sich noch in der Konsole **Active Directory-Benutzer und -Computer** befinden.

2. Klicken Sie in der Konsolenstruktur mit der rechten Maustaste auf **Contoso.com**, und klicken Sie anschließend auf **Eigenschaften**.

3. Klicken Sie auf der Registerkarte **Sicherheit** auf **Erweitert**.

4. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für Contoso** auf **Hinzufügen**.

5. Geben Sie im Dialogfeld **Select User, Computer, Service Account, or Group** (Benutzer, Computer, Dienstkonto oder Gruppe auswählen) im Feld **Namen des auszuwählenden Objekts eingeben** **mimcm-Managers** ein, und klicken Sie anschließend auf **OK**.

6. Klicken Sie im Dialogfeld **Permission Entry for CONTOSO** in der Liste **Anwenden auf** auf **Descendant User objects**, und aktivieren Sie anschließend das Kontrollkästchen **Zulassen** für die folgenden **Berechtigungen**:

    - **Alle Eigenschaften lesen**

    - **Berechtigungen lesen**

    - **FIM CM Audit**

    - **FIM CM-Enrollment Agent**

    - **FIM CM Request Enroll**

    - **FIM CM Request Recover**

    - **FIM CM Request Renew**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

7. Klicken Sie im Dialogfeld **Berechtigungseintrag für CONTOSO** auf **OK**.

8. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für CONTOSO** auf **Hinzufügen**.

9. Geben Sie im Dialogfeld **Select User, Computer, Service Account, or Group** (Benutzer, Computer, Dienstkonto oder Gruppe auswählen) im Feld **Namen des auszuwählenden Objekts eingeben** **mimcm-HelpDesk** ein, und klicken Sie anschließend auf **OK**.

10. Klicken Sie im Dialogfeld **Permission Entry for CONTOSO** in der Liste **Anwenden auf** auf **Descendant User objects**, und aktivieren Sie anschließend das Kontrollkästchen **Zulassen** für die folgenden **Berechtigungen**:

    - **Alle Eigenschaften lesen**

    - **Berechtigungen lesen**

    - **FIM CM-Enrollment Agent**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

11. Klicken Sie im Dialogfeld **Berechtigungseintrag für contoso** auf **OK**.

12. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für Contoso** auf **OK**.

13. Klicken Sie im Dialogfeld **contoso.com-Eigenschaften** auf **OK**.

14. Lassen Sie **Active Directory-Benutzer und-Computer** geöffnet.

Weitere Schritte: **Delegieren von Verwaltungsberechtigungen für Zertifikatvorlagen \<script\>**

- Container für das Delegieren der Berechtigungen für den Zertifikatvorlagen

- Container für das Delegieren der Berechtigungen für den OID

- Delegieren der Berechtigungen in den vorhandenen Zertifikatvorlagen

Definieren der Berechtigungen im Zertifikatvorlagencontainer

1. Stellen Sie die Konsole **Active Directory-Standorte und -Dienste** wieder her.

2. Erweitern Sie in der Konsolenstruktur **Dienste** und **Public Key Services**, und klicken Sie anschließend auf **Zertifikatvorlagen**.

3. Klicken Sie in der Konsolenstruktur mit der rechten Maustaste auf **Zertifikatvorlagen**, und klicken Sie auf **Objektverwaltung zuweisen**.

4. Klicken Sie im Assistenten zum **Zuweisen der Objektverwaltung** auf **Weiter**.

5. Klicken Sie auf der Seite **Benutzer oder Gruppen** auf **Hinzufügen**.

6. Geben Sie im Dialogfeld **Select Users, Computers, or Groups** (Benutzer, Computer oder Gruppen auswählen) im Feld **Enter the object names to select** (Geben Sie die Namen der auszuwählenden Objekte ein) **mimcm-Managers** ein, und klicken Sie anschließend auf **OK**.

7. Klicken Sie auf der Seite **Benutzer oder Gruppen** auf **Weiter**.

8. Klicken Sie auf der Seite **Zuzuweisende Aufgaben** auf **Benutzerdefinierte Aufgaben zum Zuweisen erstellen**, und klicken Sie auf **Weiter**.

9.  Stellen Sie auf der Seite **Active Directory-Objekttyp** sicher, dass **Diesem Ordner, bestehenden Objekten in diesem Ordner und neuen Objekten in diesem Ordner** ausgewählt ist, und klicken Sie anschließend auf **Weiter**.

10. Aktivieren Sie auf der Seite **Berechtigungen** in der Liste **Berechtigungen** das Kontrollkästchen **Vollzugriff**, und klicken Sie auf **Weiter**.

11. Klicken Sie auf der Seite **Fertigstellen des Assistenten** auf **Fertig stellen**.

Definieren der Berechtigungen für den OID-Container:

1. Klicken Sie in der Konsolenstruktur mit der rechten Maustaste auf **OID**, und klicken Sie anschließend auf **Eigenschaften**.

2. Klicken Sie im Dialogfeld **OID-Eigenschaften** auf der Registerkarte **Sicherheit** auf **Erweitert**.

3. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für OID** auf **Hinzufügen**.

4. Geben Sie im Dialogfeld **Select User, Computer, Service Account, or Group** (Benutzer, Computer, Dienstkonto oder Gruppe auswählen) im Feld **Namen des auszuwählenden Objekts eingeben** **mimcm-Managers** ein, und klicken Sie anschließend auf **OK**.

5. Stellen Sie im Dialogfeld **Berechtigungseintrag für OID** sicher, dass die Berechtigungen für **Dieses und alle untergeordneten Objekte** gelten. Klicken Sie auf **Vollzugriff** und anschließend auf **OK**.

6. Klicken Sie im Dialogfeld **Erweiterte Sicherheitseinstellungen für OID** auf **OK**.

7. Klicken Sie im Dialogfeld **OID-Eigenschaften** auf **OK**.

8. Schließen Sie **Active Directory-Standorte und -Dienste**.

**Skripts: Berechtigungen für den OID, Profilvorlage- & Zertifikatvorlagen-Container**

![Diagramm](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Skripts: Delegieren der Berechtigungen in den vorhandenen Zertifikatvorlagen**  

![Diagramm](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
