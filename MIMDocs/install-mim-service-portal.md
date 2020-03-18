---
title: Installieren des Microsoft Identity Manager-Diensts und -Portals | Microsoft-Dokumentation
description: Hier finden Sie die Schritte zum Konfigurieren und Installieren Erste Schritte zum Konfigurieren und Installieren des MIM-Diensts und -Portals für Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: e381bb418ce8215dafc369bf33782483a6e4de3e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042439"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Installieren von MIM 2016: MIM-Dienst und -Portal

> [!div class="step-by-step"]
> [« MIM-Synchronisierungsdienst](install-mim-sync.md)
> [Datenbanken synchronisieren »](install-mim-sync-ad-service.md)
 
> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Domänencontrollername: **mimservername**
> - Domänenname: **contoso**
> - Kennwort – <strong>Pass@word1</strong>
> - Name des Dienstkontos: **MIMService**

Falls Sie im letzten Schritt das MIM-Installationspaket nicht eingerichtet haben, kehren Sie zurück, und installieren Sie die Komponenten von Microsoft Identity Manager 2016, bevor Sie fortfahren:


## <a name="configure-mim-service-and-portal-for-installation"></a>Konfigurieren des MIM-Diensts und -Portals für die Installation

1. Führen Sie das **Installationsprogramm für den MIM-Dienst und das -Portal** aus dem entpackten Unterordner **Dienst und Portal** aus.

2. Klicken Sie im Begrüßungsbildschirm auf **Weiter**.

3. Lesen Sie die Microsoft-Software-Lizenzbedingungen, und klicken Sie, sofern Sie die Lizenzbedingungen akzeptieren, auf **Weiter**.

4. Klicken Sie im MIM-Bildschirm für das **Programm zur Verbesserung der Benutzerfreundlichkeit** auf **Weiter**.

5. Stellen Sie beim Auswählen der Komponentenfunktionen für diese Bereitstellung sicher, dass Sie die MIM-Dienst- (mit Ausnahme von MIM-Berichterstellung) und die MIM-Portalfunktionen einschließen. Sie können auch das MIM-Kennwortzurücksetzungsportal und den MIM-Benachrichtigungsdienst für Kennwortänderungen auswählen.

6. Wählen Sie auf der Seite **Konfigurieren der MIM-Datenbankverbindung** die Option **Neue Datenbank erstellen** aus.

    ![Bild: Konfigurieren der MIM-Datenbankverbindung](media/install-mim-service-portal/MIM_Install10.png)

7. Geben Sie bei **Configure mail server connection** (Konfigurieren der E-Mail-Server-Verbindung) den Namen Ihres Exchange-Servers als **Mailserver** an. Alternativ können Sie das **Office 365-Postfach** verwenden. Falls Sie keinen E-Mail-Server konfiguriert haben, verwenden Sie **localhost** als Namen für den E-Mail-Server, und deaktivieren Sie die oberen zwei Kontrollkästchen. Klicken Sie auf **Weiter**.
    >[!NOTE]
    >MIM 2016 SP2 und höher: Wenn Sie gruppenverwaltete Dienstkonten verwenden, müssen Sie das Kontrollkästchen **Anderen Benutzer für Exchange verwenden** auch aktivieren, wenn Sie nicht beabsichtigen, Exchange zu verwenden.
    
    >[!NOTE]
    >Wenn die Option **Exchange Online verwenden** ausgewählt ist, müssen Sie den Wert PollExchangeEnabled des Registrierungsschlüssels „HKLM\SYSTEM\CurrentControlSet\Services\FIMService“ nach der Installation auf 1 festlegen, damit der MIM-Dienst Genehmigungsantworten vom MIM Outlook-Add-On verarbeiten kann.

    ![Bild: Konfigurieren der E-Mail-Server-Verbindung](media/install-mim-service-portal/MIM_Install11.png)

8. Geben Sie an, dass Sie ein neues selbstsigniertes Zertifikat generieren möchten, oder wählen Sie das entsprechende Zertifikat.

9. Geben Sie den zu verwendenden Dienstkontonamen an, z.B. *MIMService*, sowie das Dienstkontokennwort, z.B. <em>Pass@word1</em>, Ihre Dienstkontodomäne, z.B. *contoso*, und das Dienst-E-Mail-Konto, z.B. *contoso*.
    >[!NOTE]
    >MIM 2016 SP2 und höher: Wenn Sie gruppenverwaltete Dienstkonten verwenden, müssen Sie sicherstellen, dass sich das **$** -Zeichen am Ende des Dienstkontonamens befindet (z. B. MIMService$), und lassen Sie das Feld für das Dienstkontokennwort leer.

    ![Bild: Konfigurieren des MIM-Dienstkontos](media/install-mim-service-portal/MIM_Install12.png)

10. Beachten Sie, dass eine Warnung angezeigt werden kann, dass das Dienstkonto in seiner aktuellen Konfiguration nicht sicher ist.

11. Akzeptieren Sie die Standardeinstellungen für den Standort des Synchronisierungsservers, und geben Sie für das Konto „MIM-Verwaltungs-Agent“ *contoso\MIMMA* an.
    >[!NOTE]
    >MIM 2016 SP2 und höher: Wenn Sie das gruppenverwaltete Dienstkonto des MIM-Synchronisierungdiensts für die MIM-Synchronisierung verwenden und das Feature „Use MIM Sync account“ (MIM-Synchronisierungskonto verwenden) aktivieren möchten, müssen Sie den gMSA-Namen des MIM-Synchronisierungsdiensts als MIM-MA-Konto eingeben (z. B. *contoso\MIMSync$* ).

    ![Bild: Konfigurieren des MIM-Diensts und -Portals](media/install-mim-service-portal/MIM_Install13.png)

12. Geben Sie *CORPIDM* (den Namen dieses Computers) als MIM-Dienstserveradresse für das MIM-Portal an.

13. Geben Sie `http://mim.contoso.com` als URL für die SharePoint-Websitesammlung an.

14. Geben Sie `http://passwordregistration.contoso.com` als URL zur Kennwortregistrierung mit Port 80 an. Es ist empfehlenswert, später ein Update mit dem SSL-Zertifikat auf 443 durchzuführen.

15. Geben Sie `http://passwordreset.contoso.com` als URL zur Kennwortzurücksetzung mit Port 80 an. Es ist empfehlenswert, später ein Update mit dem SSL-Zertifikat auf 443 durchzuführen.

16. Aktivieren Sie das Kontrollkästchen, um die Ports 5725 und 5726 in der Firewall zu öffnen, sowie das Kontrollkästchen, um allen authentifizierten Benutzern den Zugriff auf das MIM-Portal zu gewähren.

## <a name="configure-mim-password-registration-portal"></a>Konfigurieren des MIM- Kennwort-Registrierungsportals

1. Legen Sie den Namen des Dienstkontos für die SSPR-Registrierung auf *contoso\MIMSSPR* und das zugehörige Kennwort auf <em>Pass@word1</em> fest.

2. Geben Sie *passwordregistration.contoso.com* als Hostnamen für die MIM-Kennwortregistrierung an, und legen Sie den Port auf **80** fest. Aktivieren Sie die Option **Port in der Firewall öffnen**.

   ![Geben Sie die Konfigurationsinformationen ein, die vom IIS-Image verwendet werden](media/install-mim-service-portal/MIM_Install14.png)

3. Eine Warnung wird angezeigt – lesen Sie sie, und klicken Sie auf **Weiter**.

4. Geben Sie im nächsten Konfigurationsbildschirm des MIM-Kennwortregistrierungsportals *mim.contoso.com* als Serveradresse für den MIM-Dienst für das Kennwort-Registrierungsportal an.

## <a name="configure-mim-password-reset-portal"></a>Konfigurieren des MIM-Kennwortzurücksetzungsportals

1. Legen Sie den Namen des Dienstkontos für die SSPR-Registrierung auf *Contoso\MIMSSPR* und das zugehörige Kennwort auf <em>Pass@word1</em> fest.

2. Geben Sie *passwordreset.contoso.com* als Hostnamen für das MIM-Kennwortzurücksetzungsportal an, und legen Sie den Port auf **80** fest. Aktivieren Sie die Option **Port in der Firewall öffnen**.

   ![Geben Sie die Konfigurationsinformationen ein, die vom IIS-Image verwendet werden](media/install-mim-service-portal/MIM_Install15.png)

3. Eine Warnung wird angezeigt – lesen Sie sie, und klicken Sie auf **Weiter**.

4. Geben Sie im nächsten Konfigurationsbildschirm des MIM-Kennwortregistrierungsportals *mim.contoso.com* als Serveradresse für den MIM-Dienst für das Kennwortzurücksetzungsportal an.

## <a name="install-mim-service-and-portal"></a>Installieren des MIM-Diensts und -Portals

Wenn alle Vorinstallationsdefinitionen bereit sind, klicken Sie auf **Installieren**, um mit der Installation der ausgewählten **Dienst- und Portal**-Komponenten zu beginnen.

Nach Abschluss der Installation stellen Sie sicher, dass das MIM-Portal aktiv ist.

1. Starten Sie Internet Explorer, und stellen Sie eine Verbindung mit dem MIM-Portal unter *http://mim.contoso.com/identitymanagement* her. Beachten Sie, dass es beim ersten Besuch der Seite möglicherweise zu einer kurzen Verzögerung kommen kann.
    - Authentifizieren Sie sich in Internet Explorer bei Bedarf als *contoso\miminstall*.

2. Öffnen Sie in Internet Explorer die **Internetoptionen**, wechseln Sie zur Registerkarte **Sicherheit** , und fügen Sie die Website zur Zone **Lokales Intranet** hinzu, wenn sie noch nicht vorhanden ist.  Schließen Sie das Dialogfeld **Internetoptionen** .

3. Ermöglichen Sie Benutzern die Anzeige ihres eigenen Eintrags in MIM.

    1.  Klicken Sie unter Verwendung von Internet Explorer im **MIM-Portal**auf **Management-Richtlinienregeln**.

    2.  Suchen Sie nach der Verwaltungsrichtlinienregel **Benutzerverwaltung: Benutzer können eigene Attribute lesen.**

    3.  Wählen Sie diese Verwaltungsrichtlinienregel aus, und deaktivieren Sie **Richtlinie ist deaktiviert**.

    4.  Klicken Sie auf **OK** , und klicken Sie dann auf **Absenden**.

4.  Stellen Sie sicher, dass die Firewall eingehende Verbindungen an die TCP-Ports 5725 und 5726 zulässt.

    1.  Starten Sie **Verwaltung » Windows-Firewall** mit **Erweiterter Sicherheit**.

    2.  Klicken Sie auf **Eingehende Regeln**.

    3.  Stellen Sie sicher, dass die folgenden zwei Regeln angezeigt werden:

    -   Forefront Identity Manager-Dienst (STS).
    -   Forefront Identity Manager-Dienst (Webservice).

    4.  Schließen Sie den Assistenten ab, und schließen Sie die Anwendung **Windows-Firewall** .

    5.  Starten Sie **Systemsteuerung » Netzwerk und Internet » Netzwerkstatus und -aufgaben anzeigen**.

    6.  Stellen Sie sicher, dass ein aktives Netzwerk als "contoso.local" als Domänennetzwerk aufgeführt wird.

    7.  Schließen Sie die **Systemsteuerung**.

> [!NOTE]
> Optional: An diesem Punkt können Sie MIM-Add-Ins und -Erweiterungen installieren.
 
> [!div class="step-by-step"]  
> [« MIM-Synchronisierungsdienst](install-mim-sync.md)
> [Datenbanken synchronisieren »](install-mim-sync-ad-service.md)
