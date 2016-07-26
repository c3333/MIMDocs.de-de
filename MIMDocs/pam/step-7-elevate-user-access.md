---
title: "Bereitstellen von PAM Schritt 7 – Benutzerzugriff | Microsoft Identity Manager"
description: "Als letzten Schritt gewähren Sie einem privilegierten Benutzer temporären Zugriff, um zu veranschaulichen, dass die Privileged Access Management-Bereitstellung erfolgreich war."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: b4b3f4c0605fabc7166e8ff8309078f80461301e


---

# Schritt 7 – Erhöhte Rechte für den Benutzerzugriff

>[!div class="step-by-step"]
[« Schritt 6 ](step-6-transition-group-to-pam.md)


Dieser Schritt veranschaulicht, dass ein Benutzer über MIM den Zugriff auf eine Rolle anfordern kann.

## Sicherstellen, dass Jen nicht auf privilegierte Ressourcen zugreifen kann
Ohne erhöhte Rechte kann Jen nicht auf die privilegierte Ressource in der CORP-Gesamtstruktur zugreifen.

1. Melden Sie sich bei CORPWKSTN ab, um alle zwischengespeicherten offenen Verbindungen zu entfernen.
2. Melden Sie sich bei CORPWKSTN als *CONTOSO\Jen* an, und wechseln Sie zur **Desktopansicht**.
3. Öffnen Sie eine DOS-Eingabeaufforderung.
4. Geben Sie den Befehl `dir \\corpwkstn\corpfs`. Die Fehlermeldung **Zugriff verweigert** sollte angezeigt werden.
5. Lassen Sie das Eingabeaufforderungsfenster geöffnet.

## Fordern Sie privilegierten Zugriff von MIM an.
1. Geben Sie auf CORPWKSTN – weiterhin als CONTOSO\Jen – den folgenden Befehl ein.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Geben Sie nach der entsprechenden Aufforderung das Kennwort für das Konto PRIV.Jen ein. Es wird ein neues Eingabeaufforderungsfenster angezeigt.
3. Wenn das PowerShell-Fenster angezeigt wird, geben Sie die folgenden Befehle ein.

    > [!NOTE]
    > Alle Schritte, die auf die Ausführung dieser Befehle folgen, sind zeitempfindlich.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Schließen Sie nach Abschluss dieser Vorgänge das PowerShell-Fenster.
5. Geben Sie im DOS-Befehlsfenster den folgenden Befehl ein:

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Geben Sie das Kennwort für das Konto PRIV.Jen ein. Es wird ein neues Eingabeaufforderungsfenster angezeigt.

## Überprüfen Sie die erhöhten Zugriffsrechte.
Geben Sie im neu geöffneten Fenster die folgenden Befehle ein.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Wenn bei dem Befehl „dir“ ein Fehler auftritt und die Fehlermeldung **Zugriff verweigert** angezeigt wird, sollten Sie die Vertrauensstellung erneut überprüfen.

## Aktivieren der privilegierten Rolle
Aktivieren Sie die Rechte durch die Anforderung des privilegierten Zugriffs über das PAM-Beispielportal.

1. Stellen Sie auf CORPWKSTN sicher, dass Sie als CORP\Jen angemeldet sind.
2. Geben Sie den folgenden Befehl im DOS-Befehlsfenster ein.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Geben Sie nach der entsprechenden Aufforderung das Kennwort für das Konto PRIV.Jen ein. Ein neues Webbrowserfenster wird angezeigt.
4. Navigieren Sie zu http://pamsrv.priv.contoso.local:8090, und stellen Sie sicher, dass eine Webseite des Beispielportals angezeigt wird.
5. Wählen Sie in Internet Explorer **Extras** > **Internetoptionen** aus, und klicken Sie dann auf die Registerkarte **Sicherheit**.
6. Klicken Sie auf die Zone **Lokales Intranet** > **Sites** > **Erweitert**, und fügen Sie anschließend die Website der Zone hinzu.
7. Schließen Sie das Dialogfeld **Internetoptionen** .
8. Klicken Sie auf der linken Registerkarte auf **Aktivieren**. Wählen Sie die **PAM-Rolle** aus, und klicken Sie dann auf **Aktivieren**.

> [!Note]
> Sie können in dieser Umgebung auch erfahren, wie Sie Anwendungen entwickeln, die die PAM-REST-API verwenden, wie in der [REST API-Referenz für Privileged Access Management ](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference.md) beschrieben.

## Zusammenfassung
Wenn Sie die Schritte in dieser exemplarischen Vorgehensweise abgeschlossen haben, haben Sie ein Privileged Access Management-Szenario veranschaulicht, in dem die Benutzerrechte für einen begrenzten Zeitraum erhöht werden, wodurch der Benutzer die Möglichkeit erhält, mit einem separaten privilegierten Konto auf geschützte Ressourcen zuzugreifen. Sobald die Sitzung mit erhöhten Rechten abgelaufen ist, kann das privilegierte Konto nicht länger auf die geschützte Ressource zugreifen. Die Entscheidung, welche Sicherheitsgruppen privilegierte Rollen darstellen, wird vom PAM-Administrator koordiniert. Sobald die Zugriffsrechte zu dem Privileged Access Management-System migriert sind, ist der Zugriff, der zuvor mit dem ursprünglichen Benutzerkonto ermöglicht wurde, jetzt nur über die Anmeldung mit einem speziellen privilegierten Konto möglich und wird auf Anfrage zur Verfügung gestellt. Daher sind Gruppenmitgliedschaften für sehr privilegierte Gruppen nur für einen begrenzten Zeitraum gültig.

>[!div class="step-by-step"]
[« Schritt 6 ](step-6-transition-group-to-pam.md)



<!--HONumber=Jul16_HO3-->


