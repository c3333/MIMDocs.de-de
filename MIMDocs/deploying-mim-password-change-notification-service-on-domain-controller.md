---
title: Bereitstellen des Benachrichtigungsdiensts für Kennwortänderungen | Microsoft-Dokumentation
description: Hier finden Sie die Schritte zum Installieren und Konfigurieren des MIM-Benachrichtigungsdiensts für Kennwortänderungen auf Ihrem Domänencontroller.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f195a6db259bf0fefabcd05c8890ca65c9624314
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041963"
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>Bereitstellen des MIM-Benachrichtigungsdiensts für Kennwortänderungen auf einem Domänencontroller

## <a name="install-the-password-change-notification-service"></a>Installieren des Benachrichtigungsdiensts für Kennwortänderungen
Der Benachrichtigungsdienst für Kennwortänderungen (Password Change Notification Service; PCNS) ist ein Dienst, den Sie auf den Domänencontrollern installieren. Er ermöglicht MIM die Synchronisierung von Kennwörtern mit anderen Systemen, etwa dem Verzeichnisserver eines anderen Anbieters. Zur Kennwortsynchronisierung installieren Sie den Benachrichtigungsdienst für Kennwortänderungen auf jedem Domänencontoller.

1.  Melden Sie sich als Domänenadministrator bei einem Server unter Windows Server mit der Rolle eines Active Directory-Domänendiensts an.

2.  Kopieren Sie den Setupordner für den Benachrichtigungsdienst für Kennwortänderungen (Password Change Notification Service) auf den Computer.

3.  Suchen Sie nach der Datei *Password Change Notification Service.msi* , klicken Sie mit der rechten Maustaste darauf, und erstellen Sie eine Verknüpfung.

4.  Suchen Sie nach der Verknüpfung, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Eigenschaften**.

5.  Fügen Sie im Feld „Ziel“ die Präambel *msiexec.exe/i* vor dem Pfad zur MSI-Datei und das Suffix *SCHEMAONLY=TRUE* nach dem MSI-Pfad hinzu. Falls der Setupordner beispielsweise *C:\PCNS* ist, würde der auszuführende Befehl wie folgt aussehen (vollständig in einer Zeile):

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Speichern Sie die Änderungen an der Verknüpfung.

7.  Führen Sie die Verknüpfung aus, um die Installation des Benachrichtigungsdiensts für Kennwortänderungen im Schemaerweiterungsmodus zu starten. Wenn der folgende Bildschirm angezeigt wird, klicken Sie auf **Weiter**.

8.  Sie werden benachrichtigt, dass Setup jetzt das Active Directory-Schema für den Benachrichtigungsdienst für Kennwortänderungen aktualisiert. Klicken Sie auf **OK** , damit die Aktualisierung des Schemas vorgenommen wird.

9. Wenn der Schemaerweiterungsvorgang abgeschlossen ist und der folgende Bildschirm angezeigt wird, klicken Sie auf **Fertig stellen**.

10. Führen Sie die Datei *Password Change Notification Service.msi* erneut aus – dieses Mal direkt (es ist keine Ausführungszeichenfolge erforderlich).  Wenn der folgende Bildschirm angezeigt wird, klicken Sie auf **Weiter**.

11. Stimmen Sie dem Lizenzvertrag zu, und klicken Sie auf **Weiter**.

12. Klicken Sie, um die Installation zu starten.

13. Wenn der Bildschirm für erfolgreiches Abschließen der Installation angezeigt wird, klicken Sie auf **Fertig stellen**.

14. Starten Sie den Computer neu, damit die Konfigurationsänderungen wirksam werden, die Sie am MIM-Benachrichtigungsdienst für Kennwortänderungen vorgenommen haben. Sie können dazu in dem angezeigten Popupfenster auf **Ja** klicken, oder Sie können den Neustart später ausführen.

## <a name="configuring-the-password-change-notification-service"></a>Konfigurieren des Benachrichtigungsdiensts für Kennwortänderungen
Nachdem Sie sich beim Domänencontroller erneut als Domänenadministrator angemeldet haben, wechseln Sie in *C:\Programme\Microsoft Password Change Notification.* Führen Sie *pcnscfg.exe* aus.
