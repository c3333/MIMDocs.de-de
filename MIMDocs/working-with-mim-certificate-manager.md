---
title: Bereitstellen der MIM-Zertifikat-Manager-Anwendung unter Windows | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Zertifikat-Manager-App bereitstellen, um Ihren Benutzern das Verwalten ihrer eigenen Zugriffsrechte zu ermöglichen.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/16/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8ff9edce6da865418e300095ff0827853a35d4eb
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358345"
---
# <a name="mim-certificate-manager-windows-store-application-deployment"></a>Bereitstellung der Windows Store-Anwendung des MIM-Zertifikat-Managers

Nachdem Sie MIM 2016 und den Zertifikat-Manager eingerichtet haben, können Sie die Windows Store-Anwendung des MIM-Zertifikat-Managers bereitstellen. Mithilfe der Windows Store-Anwendung können Benutzer ihre physischen Smartcards, virtuellen Smartcards sowie Softwarezertifikate verwalten. Mit folgenden Schritten stellen Sie die Anwendung für den MIM-Zertifikat-Manager (MIM Certificate Manager, MIM CM) bereit:

1. Erstellen Sie eine Zertifikatvorlage.

2. Erstellen Sie eine Profilvorlage.

3. Bereiten Sie die Anwendung vor.

4. Stellen Sie die Anwendung über SCCM oder Intune bereit.

## <a name="create-a-certificate-template"></a>Erstellen einer Zertifikatvorlage

Sie erstellen eine Zertifikatvorlage für die Zertifikat-Manager-Anwendung in derselben Weise, wie Sie dies normalerweise tun, mit der Ausnahme, dass Sie sicherstellen müssen, dass die Zertifikatvorlage in Version 3 oder höher vorliegt.

1. Melden Sie sich bei dem Server an, auf dem Active Directory-Zertifikatdienste (AD CS) ausgeführt werden (der Zertifikatserver).

2. Öffnen Sie die MMC.

3. Klicken Sie auf **Datei &gt; Snap-In hinzufügen/entfernen**.

4. Klicken Sie in der Liste „Verfügbare Snap-Ins“ auf **Zertifikatvorlagen**, und klicken Sie dann auf **Hinzufügen**.

5. In der MMC sehen Sie nun **Zertifikatvorlagen** unter **Konsolenstamm** . Doppelklicken Sie auf diesen Eintrag, um alle verfügbaren Zertifikatvorlagen anzuzeigen.

6. Klicken Sie mit der rechten Maustaste auf die Vorlage **Smartcard-Anmeldung**, und klicken Sie auf **Doppelte Vorlage**.

7. Wählen Sie auf der Registerkarte „Kompatibilität“ unter „Zertifizierungsstelle“ die Option „Windows Server 2008“ aus. Wählen Sie unter „Zertifikatempfänger“ die Option „Windows 8.1/Windows SErver 2012 R2“ aus. Die Version der Versionsvorlage wird festgelegt, wenn Sie zum ersten Mal die Zertifikatvorlage erstellen und speichern. Wenn Sie die Zertifikatvorlage nicht auf diese Weise erstellt haben, gibt es keine Möglichkeit, sie auf die richtige Version zu ändern.

   > [!NOTE]
   >  Dieser Schritt ist entscheidend, da hiermit sichergestellt wird, dass Sie eine Zertifikatvorlage in Version 3 (oder höher) haben. Nur Version 3 funktioniert mit der Zertifikat-Manager-Anwendung.

8. Geben Sie auf der Registerkarte **Allgemein** in das Feld **Anzeigename** den Namen ein, der in der Benutzeroberfläche der Anwendung angezeigt werden soll, etwa **Anmeldung über virtuelle Smartcard**.

9. Legen Sie auf der Registerkarte **Anforderungsverarbeitung** das Feld **Zweck** auf **Signatur und Verschlüsselung** fest, wählen Sie unter **Folgendes ausführen...** die Option **Benutzer zur Eingabe während der Registrierung auffordern**aus.

10. Wählen Sie auf der Registerkarte **Kryptografie** unter **Anbieterkategorie**die Option **Schlüsselspeicheranbieter und Verwendung aller auf dem Computer des Antragstellers verfügbaren Anbieter für Anforderungen möglich**aus.

    > [!NOTE]
    > Wenn Ihre Vorlage in Version 3 vorliegt, sehen Sie nur „Schlüsselspeicheranbieter“ als Option. Wird diese Option nicht angezeigt, haben Sie die Zertifikatvorlage wahrscheinlich nicht ordnungsgemäß mit der richtigen Version erstellen. Beginnen Sie erneut mit Schritt 5.

11. Fügen Sie auf der Registerkarte **Sicherheit** die Sicherheitsgruppe hinzu, für die Sie **Registrieren** -Zugriff bereitstellen möchten. Wenn Sie beispielsweise allen Benutzern Zugriff gewähren möchten, wählen Sie die Gruppe **Authentifizierte Benutzer** und dann die Berechtigung **Registrieren** für sie aus.

12. Klicken Sie auf **OK** , um die von Ihnen vorgenommenen Änderungen abzuschließen und die neue Vorlage zu erstellen. Sie sollten die neue Vorlage in der Liste der Zertifikatvorlagen sehen können.

13. die **Datei** aus, und klicken Sie auf **Snap-In hinzufügen/entfernen**, um das Zertifizierungsstelle-Snap-In in der MMC-Konsole hinzuzufügen. Wenn Sie gefragt werden, welchen Computer Sie verwalten möchten, wählen Sie **Lokaler Computer**aus.

14. Erweitern Sie im linken Bereich der MMC den Eintrag **Zertifizierungsstelle (lokal)** , und erweitern Sie dann Ihren Zertifikat-Manager in der Liste „Zertifizierungsstelle“.

15. Klicken Sie mit der rechten Maustaste auf **Zertifikatvorlagen**, und klicken Sie auf **Neu &gt; Auszustellende Zertifikatvorlage**.

16. Wählen Sie in der Liste die neue Vorlage aus, die Sie erstellt haben, und klicken Sie auf **OK**.

## <a name="create-a-profile-template"></a>Erstellen einer Profilvorlage

Wenn Sie eine Profilvorlage erstellen, müssen Sie „Virtuelle SmartCard erstellen/löschen“ und Entfernen der Datensammlung für diese festlegen. Die Zertifikat-Manager-Anwendung kann keine gesammelten Daten verarbeiten, daher muss dies wie folgt deaktiviert werden.

1.  Melden Sie sich beim CM-Portal (Zertifikatverwaltung) als Benutzer mit Administratorrechten an.

2.  Wechseln Sie zu „Verwaltung“ &gt; „Profilvorlagen verwalten“. Stellen Sie sicher, dass das Kontrollkästchen neben **MIM CM-Beispielsmartcard-Profilvorlage für Anmeldung** aktiviert ist, und klicken Sie dann auf „Ausgewählte Profilvorlage kopieren“.

3.  Geben Sie den Namen der Profilvorlage ein, und klicken Sie auf **OK**.

4.  Klicken Sie in der nächsten Anzeige auf **Neue Zertifikatvorlage hinzufügen** , und aktivieren Sie das Kontrollkästchen neben dem Namen der Zertifizierungsstelle.

5.  Aktivieren Sie das Kontrollkästchen neben dem Namen der Profilvorlage **Anmeldung** , und klicken Sie auf **Hinzufügen**.

6.  Entfernen Sie die SmartCardLogon-Vorlage, indem Sie das Kontrollkästchen neben diesem Eintrag aktivieren und dann auf **Ausgewählte Zertifikatvorlagen löschen** sowie auf **OK**klicken.

7.  Scrollen Sie bis nach ganz unten, und klicken Sie auf **Einstellungen ändern**.

8.  Aktivieren Sie die Kontrollkästchen neben **Virtuelle SmartCard erstellen/löschen** und **Administratorschlüssel diversifizieren**.

9. Wählen Sie unter **Benutzer-PIN-Richtlinie** die Option **Vom Benutzer angegeben**aus.

10. Klicken Sie im linken Bereich auf **Erneuerungsrichtlinie &gt; Allgemeine Einstellungen ändern**. Wählen Sie **Smartcard bei Erneuerung wiederverwenden** aus, und klicken Sie auf **OK**.

11. Sie müssen Datensammlungselemente für jede Richtlinie deaktivieren, indem Sie auf die Richtlinie im linken Bereich klicken. Anschließend müssen Sie das Feld neben **Beispieldatenelement** aktivieren und auf **Datensammlungselemente löschen** und dann auf **OK** klicken.

## <a name="prepare-the-cm-app-for-deployment"></a>Vorbereiten der Zertifikat-Manager-Anwendung zur Bereitstellung

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um die App zu entpacken. Der Inhalt wird in einen neuen Unterordner namens „appx“ extrahiert, und eine Kopie wird erstellt, so dass Sie nicht die ursprüngliche Datei ändern.

    ```cmd
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2. Ändern Sie im Ordner „appx“ den Namen der Datei „CustomDataExample.xml“ in „Custom.data“.

3. Öffnen Sie die Datei „Custom.data“, und ändern Sie die Parameter nach Bedarf.


   |                     |                                                                                                                                                                                                          |
   |---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      MIMCM URL      |                                              Der FQDN des Portals, über das Sie CM (Zertifikat-Manager) konfiguriert haben. Beispiel: https://mimcmServerAddress/certificatemanagement                                              |
   |      ADFS URL       | Wenn Sie AD FS verwenden, fügen Sie Ihre AD FS-URL ein. Beispiel: <https://adfsServerSame/adfs> </br> Wenn AD FS nicht verwendet wird, konfigurieren Sie diese Einstellung mit einer leeren Zeichenfolge.  Beispiel: ```<ADFS URL=""/>``` |
   |     PrivacyUrl      |                                         Sie können eine URL zu einer Webseite einfügen, auf der erläutert wird, wie Sie die Benutzerdetails verarbeiten, die für die Zertifikatregistrierung gesammelt wurden.                                          |
   |     SupportMail     |                                                                           Sie können eine E-Mail-Adresse für Unterstützung bei Problemen einfügen.                                                                           |
   | LobComplianceEnable |                                                                     Sie können diesen Parameter auf „true“ oder „false“ festlegen. Er ist standardmäßig auf „true“ festgelegt.                                                                      |
   |  MinimumPinLength   |                                                                                        Er ist standardmäßig auf „6“ festgelegt.                                                                                         |
   |      NonAdmin       |           Sie können diesen Parameter auf „true“ oder „false“ festlegen. Er ist standardmäßig auf „false“ festgelegt. Ändern Sie diesen Parameter nur, wenn es Benutzern, die keine Administratoren auf ihren Computern sind, möglich sein soll, Zertifikate zu registrieren und zu erneuern.            |

   > [!IMPORTANT]
   > Es muss ein Wert für die AD FS-URL angegeben werden. Wenn kein Wert angegeben ist, gibt die moderne App beim ersten Gebrauch einen Fehler aus.
4. Speichern Sie die Datei, und beenden Sie den Editor.

5. Durch Signieren des Pakets wird eine Signaturdatei erstellt, sodass Sie die ursprüngliche Signaturdatei namens „AppxSignature.p7x“ löschen müssen.

6. Die Datei „AppxManifest.xml“ gibt den Antragstellernamen des Signaturzertifikats an. Öffnen Sie diese Datei, um sie zu bearbeiten.

7. Sie müssen ein Signaturzertifikat abrufen, bevor Sie mit diesem Abschnitt beginnen. Weitere Informationen finden Sie unten unter „Ermöglichen von SmartCard-Erneuerung für Nicht-Administratoren im MIM 2016-Zertifikat-Manager“, Schritt 1.

8. Ändern Sie im &lt;Identity&gt;-Element den Wert des Attributs „Publisher“ so, dass er mit dem Antragsteller identisch ist, der in Ihrem Signaturzertifikat aufgeführt ist, z. B. „CN=SUBJECT“.

9. Speichern Sie die Datei, und beenden Sie den Editor.

10. Führen Sie in der Eingabeaufforderung den folgenden Befehl aus, um die APPX-Datei erneut zu packen und sie zu signieren.

    ```cmd
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    Dabei ist App-Paketname (app package name) der Name, den Sie beim Erstellen der Kopie verwendet haben.

    ```cmd
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Dadurch wird die neue APPX-Datei bereitgestellt. Die APPX-Datei gibt einen Speicherort für das Signaturzertifikat und ein Kennwort für die PFX-Datei an.

11. So arbeiten Sie mit AD FS-Authentifizierung

    - Öffnen Sie die Anwendung für virtuelle Smartcards. Dies erleichtert Ihnen das Finden der Werte, die für den nächsten Schritt benötigt werden.

    - Öffnen Sie auf dem AD FS-Server Windows PowerShell, und führen Sie den Befehl `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`aus, um die Anwendung als Client auf dem AD FS-Server hinzuzufügen und den CM (Zertifikat-Manager) auf dem Server zu konfigurieren.

       Es folgt das Skript "ConfigureMIimCMClientAndRelyingParty.ps1":

      ```PowerShell
       # HELP

       <#
       .SYNOPSIS
                       Configure ADFS for CM client app and server.
       .DESCRIPTION
          What the Script does:
                                       a. Registers the MIM CM client app on the ADFS server.
                                       b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                       For parameter information, see 'get-help -detailed'
       .PARAMETER redirectUri
                       The redirectUri for CM client app. Will be added to ADFS server.
                       It can be found as follows:
                       1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                       2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
       .PARAMETER serverFqdn
                       Your deployed MIM CM server’s FQDN.
       .EXAMPLE
          .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
       #>

       # Parameter declaration
       [CmdletBinding()]
       Param(
         [Parameter(Mandatory=$True)]
          [string]$redirectUri,

          [Parameter(Mandatory=$True)]
          [string]$serverFqdn
       )

       Write-Host "Configuring ADFS Objects for OAuth.."

       #Configure SSO to get persistent sign on cookie
       Set-ADFSProperties -SsoLifetime 2880

       #Configure Authentication Policy
       #Intranet to use Kerberos
       #Extranet to use U/P

       #Create Client Objects

       Write-Host "Creating Client Objects..."

       $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

       if ($existingClient -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Modern App, removing"
           Remove-ADFSClient -TargetName "MIM CM Modern App"
           Write-Host "Client object removed"
       }

       Write-Host "Adding Client Object for MIM CM Modern App client"
       Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
       Write-Host "Client Object for MIM CM Modern App client Created"

       #Create Relying Parties
       Write-Host "Creating Relying Party Objects"

       $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
       if ($existingRp -ne $null)
       {
           Write-Host "Found existing instance of the MIM CM Service RP, removing"
           Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
           Write-Host "RP object Removed"
       }

       $authorizationRules =
       "@RuleTemplate = `"AllowAllAuthzRule`"
       => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

       $issuanceRules =
       "@RuleTemplate = `"LdapClaims`"
       @RuleName = `"Emit UPN and common name`"
       c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
       => issue(store = `"Active Directory`", types =
       (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
       `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
       `";userPrincipalName,cn;{0}`", param = c.Value);

       @RuleTemplate = `"PassThroughClaims`"
       @RuleName = `"Pass through Windows Account Name`"
       c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

       Write-Host "Creating RP Trust for MIM CM Service"
       Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
       Write-Host "RP Trust for MIM CM Service has been created"
       ```

    - Aktualisieren Sie die Werte von „redirectUri“ und „serverFQDN“.

    - Um den Umleitungs-URI (redirectUri) zu finden, öffnen Sie in der Anwendung für virtuelle Smartcards den Bereich für Anwendungseinstellungen, und klicken Sie auf **Einstellungen**. Der Umleitungs-URI sollte unter der Adressleiste des AD FS-Servers aufgelistet werden. Der URI wird nur angezeigt, wenn eine AD FS-Serveradresse konfiguriert ist.

    - Der FQDN des Servers (serverFQDN) ist nur der vollständige Computername des MIMCM-Servers.

    - Wenn Sie Hilfe zu dem Skript **ConfigureMIimCMClientAndRelyingParty.ps1** wünschen, führen Sie Folgendes aus: </br> 
      ```Powershell
      get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1
      ```

## <a name="deploy-the-app"></a>Bereitstellen der App

Wenn Sie die Zertifikat-Manager-App einrichten möchten, laden Sie aus dem Download Center die Datei „MIMDMModernApp_&lt;Version&gt;_AnyCPU_Test.zip“ herunter, und extrahieren Sie deren gesamten Inhalt. Die APPX-Datei ist das Installationsprogramm. Sie können diese Datei auf dieselbe Weise bereitstellen, auf die Sie normalerweise Windows Store-Apps bereitstellen. Das heißt, Sie können [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx) oder [Intune](https://technet.microsoft.com/library/dn613839.aspx) verwenden, um die App querzuladen, sodass Benutzer über das Unternehmensportal auf sie zugreifen müssen oder sie durch direktes Übertragen an ihre Computer erhalten.

## <a name="next-steps"></a>Nächste Schritte

- [Vorlagen für das Konfigurationsprofil](https://technet.microsoft.com/library/cc708656)
- [Verwalten von Smartcardanwendungen](https://technet.microsoft.com/library/cc708681)
