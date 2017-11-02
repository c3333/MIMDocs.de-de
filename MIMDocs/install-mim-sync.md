---
title: Installieren des Microsoft Identity Manager-Synchronisierungsdiensts | Microsoft-Dokumentation
description: Erste Schritte mit MIM 2016-Komponenten durch Installation und Konfiguration von Synchronization Service
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 836279ecc7fce65912df4a1a34a9d48daf9d1151
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Installieren von MIM 2016: MIM Synchronization Service

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[MIM-Dienst und -Portal »](install-mim-service-portal.md)

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Domänencontrollername: **mimservername**
> - Domänenname: **contoso**
> - Kennwort – **Pass@word1**

Um den Microsoft Identity Manager 2016 zu installieren, richten Sie zuerst das Installationspaket ein:

1. Melden Sie sich beim Server, den Sie für die Identitätsverwaltung verwenden, als *contoso\Administrator* an.

2. Entpacken Sie das MIM-Installationspaket, oder binden Sie die MIM-Image-DVD ein.

## <a name="install-mim-2016-synchronization-service"></a>Installieren des MIM 2016-Synchronisierungsdiensts

1. Navigieren Sie im entpackten MIM-Installationsordner zum Ordner **Synchronisierungsdienst** .

2. Führen Sie das **MIM-Synchronisierungsdienst-Installationsprogramm**aus. Befolgen Sie die Anweisungen des Installationsprogramms, und schließen Sie die Installation ab.

3. Klicken Sie im Begrüßungsbildschirm auf **Weiter**.

    ![Begrüßungsbildschirm des MIM Installations-Assistenten](media/MIM-Install1.png)

4. Überprüfen Sie die Lizenzbedingungen, und klicken Sie auf **Weiter**, um den Bedingungen zuzustimmen.

5. Klicken sie auf dem Bildschirm **benutzerdefinierte Installation** auf **Weiter**.

    ![Bild: Benutzerdefinierte Installation](media/MIM-Install2.png)

6.  Wählen Sie im Konfigurationsbildschirm für die Synchronisierungsdienstdatenbank Folgendes aus:

    1.  Der SQL Server befindet sich auf: **Diesem Computer**.

    2.  Die SQL Server-Instanz ist: **Die Standardinstanz**.

    ![Bild: Datenbankverbindung](media/MIM-Install3.png)

7.  Konfigurieren Sie das Synchronisierungsdienstkonto gemäß dem zuvor von Ihnen erstellten Konto:

    1.  Dienstkonto: *MIMSync*

    2.  Kennwort: *Pass@word1*

    3.  Dienstkontodomäne oder Name des lokalen Computers: *contoso*

    ![Bild: Dienstkonto](media/MIM-Install4.png)

8.  Geben Sie dem Installationsprogramm für den MIM-Synchronisierungsdienst die relevanten Sicherheitsgruppen an:

    1. Administrator = *Contoso\MIMSyncAdmins*

    2. Operator = *Contoso\MIMSyncOperators*

    3. Verbindungszeichen = *Contoso\MIMSyncJoiners*

    4. Connector „Durchsuchen“ = *Contoso\MIMSyncBrowse*

    5. WMI-Kennwortverwaltung = *contoso\MIMSyncPasswordReset*

    ![Bild: Sicherheitsgruppen](media/MIM-Install5.png)

9. Aktivieren Sie im Fenster mit den Sicherheitseinstellungen **Enable Firewall rules for inbound RPC communications** (Firewallregeln für eingehende RPC-Kommunikation aktivieren), und klicken Sie auf **Weiter**.

10. Klicken Sie auf **Installieren**, um die Installation des MIM-Synchronisierungsdiensts zu starten.

    1. Möglicherweise wird eine Warnung bezüglich des MIM-Synchronisierungsdienstkontos angezeigt – klicken Sie dann auf **OK**.

    2. Der MIM-Synchronisierungsdienst wird installiert.

    3. Ein Hinweis zum Erstellen einer Sicherung des Verschlüsselungsschlüssels wird angezeigt – klicken Sie auf **OK**, und wählen Sie dann einen Ordner zum Speichern der Sicherung des Verschlüsselungsschlüssels aus.

        ![Bild: Hinweis zur Sicherung des Verschlüsselungsschlüsselsatzes für MIM Sync](media/MIM-Install7.png)

    4. Wenn das Installationsprogramm die Installation erfolgreich abgeschlossen hat, klicken Sie auf **Fertig stellen**.

    5. Sie müssen sich ab- und wieder anmelden, damit Änderungen an der Gruppenmitgliedschaft wirksam werden. Klicken Sie auf **Ja** , um sich abzumelden.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[MIM-Dienst und -Portal »](install-mim-service-portal.md)
