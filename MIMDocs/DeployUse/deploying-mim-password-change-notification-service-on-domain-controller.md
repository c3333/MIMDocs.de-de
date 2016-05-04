---
Titel: Bereitstellen der MIM Kennwort Change Notification Service (PCNS) auf einem Domänencontroller
MS.Custom: 
  - Identitätsmanagement
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology: 
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 97edae12-6f86-4f9f-8620-a95a096e482a
Autor: Kgremban
---
# Bereitstellen des MIM-Benachrichtigungsdiensts für Kennwortänderungen auf einem Domänencontroller

## Installieren des Benachrichtigungsdiensts für Kennwortänderungen
Der Benachrichtigungsdienst für Kennwortänderungen ist ein Dienst, den Sie auf den Domänencontrollern installieren. Er ermöglicht MIM die Synchronisierung von Kennwörtern mit anderen Systemen, etwa dem Verzeichnisserver eines anderen Anbieters. Zur Kennwortsynchronisierung installieren Sie den Benachrichtigungsdienst für Kennwortänderungen auf jedem Domänencontoller.

1.  Melden Sie sich als Domänenadministrator bei einem Server unter Windows Server mit der Rolle eines Active Directory-Domänendiensts an.

2.  Kopieren Sie den Setupordner für den Benachrichtigungsdienst für Kennwortänderungen (Password Change Notification Service) auf den Computer.

3.  Suchen Sie nach der Datei *Password Change Notification Service.msi* , klicken Sie mit der rechten Maustaste darauf, und erstellen Sie eine Verknüpfung.

4.  Suchen Sie nach der Verknüpfung, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Eigenschaften**.

5.  Fügen Sie im Feld "Ziel" die Präambel *msiexec.exe/i* vor dem Pfad zur MSI-Datei, und das Suffix *SCHEMAONLY = TRUE* nach dem MSI-Pfad. Beispielsweise ist der Ordner "Setup" *C:\PCNS* der auszuführenden Befehl sieht wie folgt: (alles in einer Zeile).

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

## Konfigurieren des Benachrichtigungsdiensts für Kennwortänderungen
Sobald die Verbindung zum DC-Server als Domänenadministrator wiederhergestellt, gehen Sie zu *C:\Program Files\Microsoft Benachrichtigung.* Führen Sie *pcnscfg.exe*.



<!--HONumber=Mar16_HO1-->


