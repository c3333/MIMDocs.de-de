---
Titel: Schritt 7 – Elevate-Benutzer Zugriff auf
MS.Custom: Na
MS.Reviewer: Na
MS.Suite: Na
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
Autor: Kgremban
---
# Schritt 7 – Erhöhte Rechte für den Benutzerzugriff
Dieser Schritt veranschaulicht, dass ein Benutzer über MIM den Zugriff auf eine Rolle anfordern kann.

## Sicherstellen, dass Jen nicht auf privilegierte Ressourcen zugreifen kann
Vergewissern Sie sich, dass Jen nicht auf die privilegierten Ressourcen in der Gesamtstruktur „CORP“ (mit ihrem Konto „CONTOSO\Jen“) zugreifen kann.

1. Melden Sie sich auf *CORPWKSTN*ab. (Dadurch werden alle zwischengespeicherten geöffneten Verbindungen entfernt.)
2. Melden Sie sich auf *CORPWKSTN* als *CONTOSO\Jen* an, und wechseln Sie zur **Desktopansicht** .
3. Öffnen Sie eine **DOS** -Eingabeaufforderung.
4. Geben Sie den Befehl `dir \\corpwkstn\corpfs`. Die Fehlermeldung „Zugriff verweigert“ sollte angezeigt werden.
5. Lassen Sie das Eingabeaufforderungsfenster geöffnet.

## Fordern Sie privilegierten Zugriff von MIM an.
1. Stellen Sie auf *CORPWKSTN*sicher, dass Sie als *CONTOSO\Jen* angemeldet sind und ein **DOS** -Befehlsfenster geöffnet ist.
2. Geben Sie folgenden Befehl ein:

    `runas /user:Priv.Jen@priv.contoso.local powershell`

3. Geben Sie nach der entsprechenden Aufforderung das Kennwort für das Konto *PRIV.Jen* ein. Es wird ein neues Eingabeaufforderungsfenster angezeigt.
4. Wenn das PowerShell-Fenster angezeigt wird, wechseln Sie zu diesem Fenster, und geben Sie die folgenden Befehle ein. **Hinweis**: Alle folgenden Interaktionen sind zeitkritisch.

```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
```
<br/>
5. Schließen Sie nach Abschluss dieser Vorgänge das neu geöffnete **PowerShell** -Fenster.
6. Geben Sie im DOS-Befehlsfenster den folgenden Befehl ein:

    `runas /user:Priv.Jen@priv.contoso.local powershell`

7. Geben Sie das Kennwort für das Konto *PRIV.Jen* ein. Es wird ein neues Eingabeaufforderungsfenster angezeigt.

## Überprüfen Sie die erhöhten Zugriffsrechte.
Geben Sie im neu geöffneten Fenster die folgenden Befehle ein.

```
    whoami /groups
    dir \\corpwkstn\corpfs
```
<br/>
Wenn bei dem Befehl „dir“ ein Fehler auftritt und die Fehlermeldung „Zugriff verweigert“ angezeigt wird, sollten Sie die Vertrauensstellung erneut überprüfen.

## (Optional) Aktivieren
Aktivieren Sie die Rechte durch die Anforderung des privilegierten Zugriffs über das PAM-Beispielportal.

1. Stellen Sie auf *CORPWKSTN*sicher, dass Sie als *CORP\Jen* angemeldet sind und ein DOS-Befehlsfenster geöffnet ist.
2. Geben Sie folgenden Befehl ein:

    `runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"`

3. Geben Sie nach der entsprechenden Aufforderung das Kennwort für das Konto *PRIV.Jen* ein. Ein neues Webbrowserfenster wird angezeigt.
4. Navigieren Sie zu *http://pamsrv.priv.contoso.local:8090* , und stellen Sie sicher, dass eine Webseite des Beispielportals angezeigt wird.
5. Wählen Sie im Internet Explorer **Tools** \ > **Internetoptionen** und klicken Sie auf die **Sicherheit** Registerkarte.
6. Klicken Sie auf die **Zone lokales Intranet** \ > **Sites** \ > **Erweitert** dann die Website zur Zone hinzufügen.
7. Schließen Sie das Dialogfeld **Internetoptionen** .
8. Klicken Sie auf der linken Registerkarte auf **Aktivieren**. Wählen Sie die *PAM-Rolle* aus, und klicken Sie dann auf **Aktivieren**.

**Hinweis**: In dieser Umgebung werden Sie auch erfahren, wie Clientanwendungen entwickeln können, die die PAM REST-API, gemäß der [Privileged Access Management REST-API-Referenz](reference/privileged-access-management-rest-api-reference.md) aktivieren.

## Zusammenfassung
Wenn Sie die Schritte in dieser testumgebungsanleitung abgeschlossen haben, werden Sie erlebt haben ein Verwaltungsszenario mit privilegierten Zugriff, in denen die Benutzer Berechtigungen für einen begrenzten Zeitraum erhöht werden und bietet dem Benutzer Zugriff auf geschützte Ressourcen mit einem separaten privilegierten Konto. Sobald die Sitzung mit erhöhten Rechten abgelaufen ist, kann das privilegierte Konto nicht länger auf die geschützte Ressource zugreifen. Die Entscheidung, welche Sicherheitsgruppen privilegierte Rollen darstellen, wird vom *PAM-Administrator*koordiniert. Sobald die Zugriffsrechte in das Verwaltungssystem für den privilegierten Zugriff migriert wurden, ist der Zugriff, der zuvor mit dem ursprünglichen Benutzerkonto ermöglicht wurde, jetzt nur über die Anmeldung mit einem speziellen privilegierten Konto möglich und wird auf Anfrage zur Verfügung gestellt. Daher sind Gruppenmitgliedschaften für sehr privilegierte Gruppen nur für einen begrenzten Zeitraum gültig.
<!--HONumber=Mar16_HO1-->
