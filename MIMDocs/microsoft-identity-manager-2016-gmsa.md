---
title: Konvertieren von spezifischen Microsoft Identity Manager-Diensten zu gMSA | Microsoft-Dokumentation
description: In diesem Artikel werden die Voraussetzungen und grundlegenden Schritte zum Konfigurieren eines über Gruppen verwalteten Dienstkontos (group Managed Service Account, gMSA) vorgestellt.
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 5985ded45a53a804728572404fb0db43e988ac1d
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176760"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Konvertieren von spezifischen Microsoft Identity Manager-Diensten zu über Gruppen verwalteten Dienstkonten

Dieser Artikel ist ein Leitfaden zum Konfigurieren unterstützter Microsoft Identity Manager-Dienste für die Verwendung von über Gruppen verwalteten Dienstkonten (group Managed Service Accounts, gMSA). Nachdem Sie Ihre Umgebung vorkonfiguriert haben, ist die Konvertierung zu einem gMSA ganz einfach.

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie folgenden erforderlichen Hotfix herunter, und installieren Sie ihn: [Microsoft Identity Manager 4.5.26.0 oder höher](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Unterstützte Dienste:

    -   Microsoft Identity Manager-Synchronisierungsdienst (FIMSynchronizationService)
    -   Microsoft Identity Manager-Dienst (FIMService)
    -   Microsoft Identity Manager-Kennwortregistrierung
    -   Microsoft Identity Manager-Kennwortzurücksetzung
    -   Privileged Access Management-Überwachungsdienst (PamMonitoringService)
    -   PAM-Komponentendienst (PrivilegeManagementComponentService)

    Nicht unterstützte Dienste:

    -   Das Microsoft Identity Manager-Portal wird nicht unterstützt. Das Portal gehört zur SharePoint-Umgebung, und Sie müssten es im Farmmodus bereitstellen und die [automatische Kennwortänderung in SharePointServer konfigurieren](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Alle Verwaltungs-Agents außer dem Microsoft Identity Manager-Dienstverwaltungs-Agent
    -   Microsoft-Zertifikatverwaltung
    -   BHOLD

- Hintergrundinformationen und allgemeine Referenzinformationen zum Einrichten Ihrer Umgebung finden Sie hier: 

    -   [Über Gruppen verwaltete Dienstkonten: Übersicht](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Bevor Sie beginnen, [erstellen Sie den Stammschlüssel des Schlüsselverteilungsdiensts](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) auf Ihrem Windows-Domänencontroller. Beachten Sie die folgenden Informationen:  

    - Stammschlüssel werden von den Schlüsselverteilungsdiensten (Key Distribution Services, KDS) verwendet, um Kennwörter und andere Informationen auf Domänencontrollern zu generieren.
    - Erstellen Sie nur einen Stammschlüssel pro Domäne, falls einer erforderlich ist.  
    - Schließen Sie `Add-KDSRootKey –EffectiveImmediately` ein. „–EffectiveImmediately“ bedeutet, dass es bis zu 10 Stunden dauern kann, den Stammschlüssel auf allen Domänencontrollern zu repliziert. Die Replikation auf zwei Domänencontrollern kann ca. 1 Stunde dauern. 
    ![Die Zeichenfolge „–EffectiveImmediately“](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Auf dem Active Directory-Domänencontroller auszuführende Aktionen

1.  Erstellen Sie eine Gruppe namens *MIMSync_Servers*, und fügen Sie dieser Gruppe alle Synchronisierungsserver hinzu.

    ![Erstellen einer Gruppe namens „MIMSync_Servers“](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Melden Sie sich als *Domänenadministrator* mit einem Konto bei Windows PowerShell an, das bereits in die Domäne eingebunden ist, und führen Sie dann folgenden Befehl aus: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![Der Befehl in PowerShell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Zeigen Sie Details des gMSA für die Synchronisierung an:  
     ![Details des gMSA für die Synchronisierung](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Wenn Sie den Benachrichtigungsdienst für Kennwortänderungen (Password Change Notification Service, PCNS) ausführen, aktualisieren Sie die Delegierung mithilfe des folgenden Befehls:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Auf dem Microsoft Identity Manager-Synchronisierungsserver auszuführende Aktionen

1. Sichern Sie im Synchronization Service Manager den Verschlüsselungsschlüssel. Dieser wird für den Änderungsmodus der Installation benötigt. Gehen Sie wie folgt vor:

    a. Suchen Sie auf dem Server, auf dem der Synchronization Service Manager installiert ist, das Tool zur Verwaltung des Synchronisierungsdienstschlüssels. Die Option **Keyset exportieren** ist standardmäßig bereits ausgewählt.

    b. Wählen Sie **Weiter** aus. 
    
    c. Geben Sie an der Eingabeaufforderung die Kontoinformationen für den Synchronisierungsdienst von Microsoft Identity Manager oder Forefront Identity Manager (FIM) ein, und überprüfen Sie diese:

    -   **Kontoname**: Der Name des Synchronisierungsdienstkontos, das bei der Erstinstallation verwendet wurde.  
    -   **Kennwort:** Das Kennwort des Synchronisierungsdienstkontos.  
    -   **Domäne:** Die Domäne, zu der das Synchronisierungsdienstkonto gehört.

    d. Wählen Sie **Weiter** aus.

    Wenn Sie die Kontoinformationen erfolgreich eingegeben haben, können Sie das Ziel bzw. den Speicherort der Exportdatei für den Verschlüsselungsschlüssel der Sicherung ändern. Standardmäßig lautet der Exportspeicherort für die Datei *C:\Windows\system32\miiskeys-1.bin*.

1. Installieren Sie Microsoft Identity Manager SP1 – zu finden im Volume Licensing Service Center oder auf der Downloadwebsite von MSDN. Wenn die Installation abgeschlossen ist, speichern Sie das Keyset *miiskeys.bin*.

   ![Das Fenster mit dem Installationsfortschritt des Microsoft Identity Manager-Synchronisierungsdiensts](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Installieren Sie [Hotfix 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) oder höher.

1. Nachdem das Patch installiert ist, beenden Sie den FIM-Synchronisierungsdienst wie folgt:

   a. Wählen Sie in der Systemsteuerung **Programme und Features** > **Microsoft Identity Manager** aus.  
   b. Wählen Sie auf der Seite **Synchronisierungsdienst** die Option **Ändern** > **Weiter** aus.  
   c. Wählen Sie im Fenster **Wartungsoptionen** die Option **Konfigurieren** aus.

   ![Das Fenster mit Wartungsoptionen](media/dc98c011bec13a33b229a0e792b78404.png)

   d. Löschen Sie im Fenster **Microsoft Identity Manager-Synchronisierungsdienst konfigurieren** den Standardwert im Feld **Dienstkonto**, und geben Sie **MIMSyncGMSA$** ein. Schließen Sie unbedingt das Dollarzeichen ($) ein, wie in der folgenden Abbildung gezeigt. Lassen Sie das Feld **Kennwort** leer.

   ![Das Fenster „Microsoft Identity Manager-Synchronisierungsdienst konfigurieren“](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Klicken Sie auf **Weiter** > **Weiter** > **Installieren**.  
   f. Stellen Sie das Keyset aus der Datei *miiskeys.bin* wieder her, die Sie zuvor gespeichert haben.

   ![Optionen zum Wiederherstellen der Keysetkonfiguration](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![Die Liste der Verwaltungs-Agents im Synchronization Service Manager](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Microsoft Identity Manager-Dienst

>[!IMPORTANT]
>Befolgen Sie die Anweisungen in diesem Abschnitt genau, wenn Sie Konten im Zusammenhang mit dem Microsoft Identity Manager-Dienst in gMSA-Konten konvertieren.

1. Erstellen Sie über Gruppen verwaltete Konten für den Microsoft Identity Manager-Dienst, die PAM-REST-API, den PAM-Überwachungsdienst, den PAM-Komponentendienst, das SSPR-Registrierungsportal (Self-Service-Kennwortzurücksetzung) und das SSPR-Rücksetzungsportal.

    -   Aktualisieren Sie die Delegierung und den Dienstprinzipalnamen (Service Principal Name, SPN) für das gMSA:

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Delegierung:

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Eingeschränkte Delegierung:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Fügen Sie ein Konto für den Microsoft Identity Manager-Dienst in Synchronisierungsgruppen hinzu. Dieser Schritt ist für SSPR erforderlich.

    ![Das Fenster „Active Directory-Benutzer und-Computer“](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > In Windows Server 2012 R2 tritt ein bekanntes Problem auf: Dienste, die ein verwaltetes Konto verwenden, reagieren nach dem Neustart des Servers nicht mehr, weil der Microsoft-Schlüsselverteilungsdienst nach dem Neustart von Windows nicht gestartet wird. Führen Sie als Problemumgehung den folgenden Befehl aus: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > Der Befehl startet den Microsoft-Schlüsselverteilungsdienst, wenn das Netzwerk aktiviert ist (in der Regel am Anfang des Startzyklus).
    >
    > Informationen zu einem ähnlichen Problem finden Sie unter [AD FS Windows 2012 R2: adfssrv hangs in starting mode](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS) (AD FS Windows 2012 R2: „adfssrv“ reagiert im Startmodus nicht).

1.  Führen Sie das MSI-Paket für den Microsoft Identity Manager-Dienst mit erhöhten Rechten aus, und wählen Sie **Ändern** aus.

1.  Aktivieren Sie im Fenster **E-Mail-Server-Verbindung konfigurieren** das Kontrollkästchen **Anderen Benutzer für Exchange verwenden (für verwaltete Konten)** . Jetzt wird Ihnen die Option angezeigt, das aktuelle Exchange-Konto oder das Cloudpostfach zu ändern.

    >[!NOTE]
    >Wenn Sie die Option **Exchange Online verwenden** auswählen, legen Sie den *PollExchangeEnabled*-Wert des Registrierungsschlüssels **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** nach der Installation auf **1** fest, damit der Microsoft Identity Manager-Dienst Genehmigungsantworten vom Outlook-Add-In für Microsoft Identity Manager verarbeiten kann.
    
    ![Das Fenster „E-Mail-Server-Verbindung konfigurieren“](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  Geben Sie im Fenster **MIM-Dienstkonto konfigurieren** im Feld **Dienstkontoname** den Namen des Dienstkontos ein. Schließen Sie unbedingt das Dollarzeichen ($) ein. Geben Sie auch im Feld **Kennwort des Dienst-E-Mail-Kontos** ein Kennwort ein. Das Feld **Kennwort für Dienstkonto** sollte nicht verfügbar sein.

    ![Das Fenster „MIM-Dienstkonto konfigurieren“](media/db0d543df6e1b0174a47135617c23fcb.png)

    Da die LogonUser-Funktion für verwaltete Konten nicht funktioniert, wird auf der nächsten Seite diese Warnung angezeigt: „Überprüfen Sie, ob das Dienstkonto in seiner aktuellen Konfiguration sicher ist“.

    ![Fenster „Sicherheitswarnung für Konto“](media/d350bc13751b2d0a884620db072ed019.png)

1.  Geben Sie im Fenster **Privileged Access Management-REST-API konfigurieren** im Feld **Kontoname für Anwendungspool** den Namen des Kontos ein. Schließen Sie unbedingt das Dollarzeichen ($) ein. Lassen Sie das Feld **Kennwort für Anwendungspoolkonto** leer.

    ![Das Fenster „Privileged Access Management-REST-API konfigurieren“](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  Geben Sie im Fenster **PAM-Komponentendienst konfigurieren** im Feld **Dienstkontoname** den Namen des Dienstkontos ein. Schließen Sie unbedingt das Dollarzeichen ($) ein. Lassen Sie das Feld **Kennwort für Dienstkonto** leer.

    ![Das Fenster „PAM-Komponentendienst konfigurieren“](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![Das Fenster „Sicherheitswarnung für Konto“](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  Geben Sie im Fenster **Privileged Access Management-Überwachungsdienst konfigurieren** im Feld **Dienstkontoname** den Namen des Dienstkontos ein. Schließen Sie unbedingt das Dollarzeichen ($) ein. Lassen Sie das Feld **Kennwort für Dienstkonto** leer.

    ![Das Fenster „Privileged Access Management-Überwachungsdienst konfigurieren“](media/d1e824248edf12a77fc9ffb011475164.png)

1.  Geben Sie im Fenster **MIM-Kennwortregistrierungsportal konfigurieren** im Feld **Kontoname** den Namen des Kontos ein. Schließen Sie unbedingt das Dollarzeichen ($) ein. Lassen Sie das Feld **Kennwort** leer.

    ![Das Fenster „MIM-Kennwortregistrierungsportal konfigurieren“](media/601e935cdfda298b61ae753a2a152996.png)

1.  Geben Sie im Fenster **MIM-Kennwortzurücksetzungsportal konfigurieren** im Feld **Kontoname** den Namen des Kontos ein. Schließen Sie unbedingt das Dollarzeichen ($) ein. Lassen Sie das Feld **Kennwort** leer.

    ![Das Fenster „MIM-Kennwortzurücksetzungsportal konfigurieren“](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Schließen Sie die Installation ab.

    > [!NOTE]
    > Während der Installation werden im Registrierungspfad **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Service** zwei neue Schlüssel zum Speichern des verschlüsselten Exchange-Kennworts erstellt. Ein Eintrag ist für *ExchangeOnline* gedacht, der andere für *ExchangeOnPremise*. Bei einem der Einträge sollte der Wert in der Spalte **Daten** leer sein.

    > ![Der Registrierungs-Editor](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Um das Kennwort für Ihre gespeicherten Konten zu aktualisieren, ohne den Änderungsmodus ausführen zu müssen, [laden Sie dieses PowerShell-Skript herunter](microsoft-identity-manager-2016-gmsascript.md).

Zum Verschlüsseln des Exchange-Kennworts erstellt das Installationsprogramm einen zusätzlichen Dienst und führt diesen im verwalteten Konto aus. Die folgenden Meldungen werden während der Installation zum Ereignisprotokoll **Anwendung** hinzugefügt:

![Das Fenster „Ereignisanzeige“](media/95b315454705cd4d939b55ac5ad910f5.jpg)
