---
title: Installieren des Microsoft Identity Manager-Synchronisierungsdiensts | Microsoft-Dokumentation
description: Erste Schritte mit MIM 2016-Komponenten durch Installation und Konfiguration von Synchronization Service
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fba7eb3caea1f00c37f00f3fd2bf67dfe3f12871
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701272"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Installieren von MIM 2016: MIM Synchronization Service

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [MIM-Dienst und -Portal »](install-mim-service-portal.md)
> 
> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Name des Domänencontrollers: **corpdc**
> - Domänenname: **contoso**
> - Servername des MIM-Diensts: **corpservice**
> - Servername der MIM-Synchronisierung: **corpsync**
> - SQL-Servername: **corpsql**
> - Kennwort – <strong>Pass@word1</strong>

Um den Microsoft Identity Manager 2016 zu installieren, richten Sie zuerst das Installationspaket ein:

1. Melden Sie sich als *contoso\miminstall* beim Server an, den Sie für den Synchronisierungsserver **corpsync** der Identitätsverwaltung verwenden.

2. Entpacken Sie das MIM-Installationspaket, oder binden Sie die MIM-Image-DVD ein.  Wenn Sie nicht über diese DVD verfügen, lesen Sie [Microsoft Identity Manager – Lizenzierung und Downloads](microsoft-identity-manager-licensing.md).

## <a name="install-mim-2016-sp1-synchronization-service"></a>Installieren von MIM 2016 SP1: Synchronisierungsdienst

1. Navigieren Sie im entpackten MIM-Installationsordner zum Ordner **Synchronisierungsdienst** .

2. Führen Sie das **MIM-Synchronisierungsdienst-Installationsprogramm**aus. Befolgen Sie die Anweisungen des Installationsprogramms, und schließen Sie die Installation ab.

3. Klicken Sie im Begrüßungsbildschirm auf **Weiter**.

    ![Begrüßungsbildschirm des MIM Installations-Assistenten](media/install-mim-sync/MIM_Install1.png)

4. Überprüfen Sie die Lizenzbedingungen, und klicken Sie auf **Weiter**, um den Bedingungen zuzustimmen.

5. Klicken sie auf dem Bildschirm **benutzerdefinierte Installation** auf **Weiter**.

    ![Bild: Benutzerdefinierte Installation](media/install-mim-sync/MIM_Install2.png)

6. Wählen Sie im Konfigurationsbildschirm für die Synchronisierungsdienstdatenbank Folgendes aus:

   1.  Der SQL Server befindet sich auf: **Einem Remotecomputer** namens **corpsql.contoso.com**.

   2.  Die SQL Server-Instanz ist: **Die Standardinstanz**

   ![Bild: Datenbankverbindung](media/install-mim-sync/MIM_Install3.png)

7. Konfigurieren Sie das Synchronisierungsdienstkonto gemäß dem zuvor von Ihnen erstellten Konto:

   1. Dienstkonto: *MIMSync*

   2. Kennwort: <em>Pass@word1</em>

   3. Dienstkontodomäne oder Name des lokalen Computers: *contoso*

   ![Bild: Dienstkonto](media/install-mim-sync/MIM_Install4.png)

8. Geben Sie dem Installationsprogramm für den MIM-Synchronisierungsdienst die relevanten Sicherheitsgruppen an:

   1. Administrator = *Contoso\MIMSyncAdmins*

   2. Operator = *Contoso\MIMSyncOperators*

   3. Verbindungszeichen = *Contoso\MIMSyncJoiners*

   4. Connector „Durchsuchen“ = *Contoso\MIMSyncBrowse*

   5. WMI-Kennwortverwaltung = *contoso\MIMSyncPasswordReset*

   ![Bild: Sicherheitsgruppen](media/install-mim-sync/MIM_Install5.png)

9. Aktivieren Sie im Fenster mit den Sicherheitseinstellungen **Enable Firewall rules for inbound RPC communications** (Firewallregeln für eingehende RPC-Kommunikation aktivieren), und klicken Sie auf **Weiter**.

10. Klicken Sie auf **Installieren**, um die Installation des MIM-Synchronisierungsdiensts zu starten.

    1. Möglicherweise wird eine Warnung bezüglich des MIM-Synchronisierungsdienstkontos angezeigt – klicken Sie dann auf **OK**.

    2. Der MIM-Synchronisierungsdienst wird installiert.

    3. Ein Hinweis zum Erstellen einer Sicherung des Verschlüsselungsschlüssels wird angezeigt – klicken Sie auf **OK**, und wählen Sie dann einen Ordner zum Speichern der Sicherung des Verschlüsselungsschlüssels aus.

        ![Bild: Hinweis zur Sicherung des Verschlüsselungsschlüsselsatzes für MIM Sync](media/MIM-Install7.png)

    4. Wenn das Installationsprogramm die Installation erfolgreich abgeschlossen hat, klicken Sie auf **Fertig stellen**.

    5. Sie müssen sich ab- und wieder anmelden, damit Änderungen an der Gruppenmitgliedschaft wirksam werden. Klicken Sie auf **Ja** , um sich abzumelden.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [MIM-Dienst und -Portal »](install-mim-service-portal.md)
