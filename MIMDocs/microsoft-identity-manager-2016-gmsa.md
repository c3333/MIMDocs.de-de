---
title: Konvertierung von MIM-spezifischen Diensten in gMSA | Microsoft-Dokumentation
description: In diesem Thema werden die grundlegenden Schritte zum Konfigurieren von gMSA beschrieben.
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 49216a2d2077dd1be83f17719e996a20abb61cf8
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78289514"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konvertierung von MIM-spezifischen Diensten in gMSA

In diesem Handbuch werden die grundlegenden Schritte zum Konfigurieren von gMSA für unterstützte Dienste beschrieben. Der Prozess zum Konvertieren in gMSA ist einfach, sobald Sie Ihre Umgebung vorkonfigurieren.

Erforderliches Hotfix: [4.5.26.0 oder höher](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history)

Unterstützt:

-   MIM-Synchronisierungsdienst (FIMSynchronizationService)
-   MIM-Dienst (FIMService)
-   MIM-Kennwortregistrierung
-   MIM-Kennwortzurücksetzung
-   PAM-Überwachungsdienst (PamMonitoringService)
-   PAM-Komponentendienst (PrivilegeManagementComponentService)

Nicht unterstützt:

-   Das MIM-Portal wird nicht unterstützt. Es ist Teil der SharePoint-Umgebung, und Sie müssten die Bereitstellung im Farmmodus und das [Konfigurieren der automatischen Kennwortänderung in SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change) durchführen.
-   Alle Verwaltungs-Agents
-   Microsoft-Zertifikatverwaltung
-   BHOLD

<a name="general-information"></a>Allgemeine Informationen 
--------------------

Das Lesen dieser Informationen ist zum Durchführen der Einrichtung und zum Verständnis erforderlich.

-   [Gruppenverwaltete Dienstkonten: Übersicht](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Schritt eins auf Ihrem Windows-Domänencontroller

1.  Erstellen Sie den Stammschlüssel der Schlüsselverteilungsdienste (Key Distribution Services, KDS), falls erforderlich (nur einmal pro Domäne). Der Stammschlüssel wird (zusammen mit anderen Informationen) vom KDS-Dienst auf Domänencontrollern zum Generieren von Kennwörtern verwendet.

    -   Add-KdsRootKey –EffectiveImmediately

    -   „– EffectiveImmediately“ bedeutet Warten bis zu \~10 Stunden / Notwendigkeit, an alle Domänencontroller zu replizieren. Dies dauert bei zwei Domänencontrollern etwa eine Stunde.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronisierungsdienst
-----------------------

1.  Erstellen Sie eine Gruppe namens „MIMSync_Servers“, und fügen Sie dieser Gruppe alle Synchronisierungsserver hinzu.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Führen Sie dann von Windows PowerShell den folgenden Befehl als Domänenadministrator aus, dessen Computerkonto bereits in die Domäne eingebunden ist.

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Abrufen von Details der GSMA (Group Managed Service Accounts) für die Synchronisierung:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Wenn der PCNS-Dienst ausgeführt wird, müssen Sie die Delegierung aktualisieren.

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Achten Sie als Nächstes bei den Synchronisierungsdiensten darauf, den Verschlüsselungsschlüssel zu sichern, da er bei der Änderungsmodusinstallation angefordert wird.

    -   Suchen Sie auf dem Server, auf dem der Synchronisierungsdienst installiert ist, das Tool zur Verwaltung des Synchronisierungsdienstschlüssels.

    -   In der Standardeinstellung ist **Keyset exportieren**  bereits ausgewählt.

    -   Klicken Sie auf **Weiter**.

    -   Jetzt werden Sie aufgefordert, die bereits vorhandenen Kontoinformationen für die Synchronisierung einzugeben.

    -   Geben Sie die FIM-Synchronisierungskontoinformationen ein, und überprüfen Sie sie.

        -   Kontoname – Kontoname des Synchronisierungsdienstkontos, das bei der Erstinstallation verwendet wird

        -   Kennwort – Kennwort des Synchronisierungsdienstkontos

        -   Domäne – Domäne, zu der das Synchronisierungsdienstkonto gehört

    -   Klicken Sie auf **Weiter**.

    -   Wenn Sie etwas falsch eingegeben haben, erhalten Sie die folgende Fehlermeldung.

    -   Nachdem Sie die Kontoinformationen erfolgreich eingegeben haben, wird Ihnen eine Option zum Ändern des Ziels (Exportdateispeicherort) des Verschlüsselungsschlüssels der Sicherung angezeigt.

        -   Standardmäßig ist der Exportspeicherort für die Datei „ **C:\\Windows\\system32**\\miiskeys-1.bin“.

4. Installieren Sie Microsoft Identity Manager SP1 Synchronization Service Build 4.4.1302.0. vom Volume License Download Center oder der MSDN Download Site. Nach Abschluss der Installation stellen Sie sicher, dass Sie das Keyset miiskeys.bin speichern.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installieren Sie das aktuelle [Hotfix 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) oder höher.

- Beenden Sie nach dem Patchen den FIM-Synchronisierungsdienst.
- Systemsteuerungsprogramme und Features von Microsoft Identity Manager
- Synchronisierungsdienst: „Ändern“ -\> „Weiter“ -\> „Konfigurieren“ –\> „Weiter“

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Löschen Sie den Kontonamen.
-  Geben Sie den Namen des Dienstkontos **MIMSyncGMSA** mit dem \$-Symbol wie im
- Screenshot ein. Lassen Sie das Kennwort leer.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- „Weiter“ –\> „Weiter“ –\> „Installieren“
- Stellen Sie das Keyset aus der gespeicherten BIN-Datei wieder her.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Die SQL-Berechtigung, die dem Konto hinzugefügt wird, wird für die Anmeldung erstellt. Aus diesem Grund müssen Sie zulassen, dass der Benutzer, der berechtigt ist, den Änderungsmodus anzuwenden, Konto und DBO der Synchronisierungsdienst-Datenbank hinzufügt.

## <a name="mim-service"></a>MIM-Dienst
-----------

>[!IMPORTANT]
>Der folgende Prozess muss durchgeführt werden, wenn zuerst die mit dem MIM-Dienst verknüpften Konten in gMSA-Konten konvertiert werden. Die im Anhang erwähnten PowerShell-Cmdlets können nur zum Ändern der Kontoinformationen verwendet werden, sobald die anfängliche Konfiguration abgeschlossen ist.*

1.  Erstellen Sie gruppenverwaltete Konten für MIM-Dienst, PAM-Rest-API, PAM-Überwachungsdienst, PAM-Komponentendienst, SSPR-Registrierungsportal und SSPR-Rücksetzungsportal.

    -   Stellen Sie sicher, dass Sie gMSA-Delegierung und SPN aktualisieren.
        -   Set-ADServiceAccount -Identity \<Konto\> -ServicePrincipalNames \@{Add="\<SPN\>"}
        -   Stellvertretung
            -   Set-ADServiceAccount -Identity \<gMSA-Konto\> -TrustedForDelegation \$true
        -   Eingeschränkte Delegierung
            -   \$delspns = 'http/mim', 'http/mim.contoso.com'
            -   New-ADServiceAccount -Name \<gMSA-Konto\> -DNSHostName \<gMSA-Konto\>.contoso.com -PrincipalsAllowedToRetrieveManagedPassword \<Gruppe\> -ServicePrincipalNames \$spns -OtherAttributes \@{'msDS-AllowedToDelegateTo'=\$delspns }

2.  Fügen Sie das Konto für den MIM-Dienst in den Synchronisierungsgruppen hinzu. Es ist für SSPR erforderlich.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **HINWEIS**:  Es ist ein bekanntes Problem, dass Dienste, die verwaltete Konten verwenden, nach dem Serverneustart hängen bleiben, weil der Microsoft-Schlüsselverteilungsdienst nach dem Windows-Neustart nicht neu gestartet wird. Der Dienst konnte nicht gestartet werden, und Windows konnte ebenfalls nicht neu gestartet werden. Das Problem ist mindestens unter Windows Server 2012 R2 reproduzierbar. Führen Sie zur Problemumgehung den Befehl 

-   **sc triggerinfo kdssvc start/networkon** aus,

    um den Microsoft-Schlüsselverteilungsdienst zu starten, wenn das Netzwerk aktiviert ist (in der Regel am Anfang des Startzyklus).

    Lesen Sie die Diskussion über ein ähnliches Problem: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>.

4.  Führen Sie die MSI-Datei des MIM-Diensts mit erhöhten Rechten aus, und wählen Sie die Änderungsoption.

5.  Aktivieren Sie auf „Hauptserver-Verbindungsseite konfigurieren“ das Kontrollkästchen „Anderes Konto für Exchange verwenden (für verwaltete Konten)“. Hier haben Sie eine Option zum Verwenden des alten Kontos, das über ein Postfach verfügt oder ein Cloudpostfach nutzt.
    >[!NOTE]
    >Wenn die Option **Exchange Online verwenden** ausgewählt ist, müssen Sie den Wert PollExchangeEnabled des Registrierungsschlüssels „HKLM\SYSTEM\CurrentControlSet\Services\FIMService“ nach der Installation auf 1 festlegen, damit der MIM-Dienst Genehmigungsantworten vom MIM Outlook-Add-On verarbeiten kann.
    
![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Geben Sie auf der Seite „MIM-Dienst-Konto konfigurieren“ das Dienstkonto mit dem \$-Symbol am Ende ein. Geben Sie auch das E-Mail-Kontokennwort für den Dienst ein. Das Dienstkontokennwort sollte deaktiviert werden.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Da die LogonUser-Funktion für verwaltete Konten nicht funktioniert, wird auf der nächsten Seite die Warnung „Überprüfen Sie, ob das Dienstkonto in seiner aktuellen Konfiguration sicher ist.“ angezeigt.

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Geben Sie auf der Seite „Privileged Access Management-REST-API konfigurieren“ den Kontonamen des Anwendungspools mit dem \$-Symbol am Ende ein, und lassen Sie das Feld „Kennwort“ leer.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Geben Sie auf der Seite „PAM-Komponentendienst konfigurieren“ den Dienstkontonamen mit dem \$-Symbol am Ende ein, und lassen Sie das Feld „Kennwort“ leer.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8.A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Geben Sie auf der Seite „Privileged Access Management-Überwachungsdienst konfigurieren“ den Dienstkontonamen mit dem \$-Symbol am Ende ein, und lassen Sie das Feld „Kennwort“ leer.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Geben Sie auf der Seite „MIM-Kennwort-Registrierungsportal konfigurieren“ den Kontonamen mit dem \$-Symbol am Ende ein, und lassen Sie das Feld „Kennwort“ leer.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Geben Sie auf der Seite „MIM-Kennwort-Rücksetzungsportal konfigurieren“ den Kontonamen mit dem \$-Symbol am Ende ein, und lassen Sie das Feld „Kennwort“ leer.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Schließen Sie die Installation ab.

Anmerkung:

-  Nach der Installation werden in der Registrierung zwei neue Schlüssel im Pfad
    - „HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service“ zum Speichern des verschlüsselten Exchange-Kennworts erstellt. Einer ist für
    - Exchange Online und der andere für die lokale Exchange-Instanz bestimmt (einer davon sollte
    - leer sein.)

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Um das Kennwort zu aktualisieren, [steht hier ein Skript zum Herunterladen bereit](microsoft-identity-manager-2016-gmsascript.md), damit der Kunde nicht den Änderungsmodus ausführen muss.

- Zum Verschlüsseln des Exchange-Kennworts erstellt das Installationsprogramm einen zusätzlichen Dienst und
    - führt ihn unter dem verwalteten Konto aus. Die folgenden Nachrichten werden während der Installation
    - dem Anwendungsereignisprotokoll hinzugefügt.

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
