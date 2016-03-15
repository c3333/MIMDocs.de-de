---
Titel: Arbeiten mit Identity Manager-Hybridberichterstellung
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
MS.AssetId: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
Autor: Kgremban
---
# Arbeiten mit der Identity Manager-Hybridberichterstellung

## Verfügbare Hybridberichte
Die ersten drei Microsoft Identity Manager-Berichte, die in Azure AD verfügbar sind **kennwortzurücksetzungsaktivität**, **Zurücksetzen des Kennworts Registrierung** und **Self-Service-Aktivität gruppiert**.

-   Die Aktivität „Zurücksetzen des Kennworts“ zeigt jede Instanz an, wenn ein Benutzer mithilfe von SSPR die Kennwortzurücksetzung durchgeführt hat und die für die Authentifizierung verwendeten Gates oder **Methoden** bereitstellt.

-   Die Aktivität „Registrierung für Zurücksetzen des Kennworts“ wird jedes Mal angezeigt, wenn sich ein Benutzer für SSPR registriert und die **Methoden** zum Authentifizieren verwendet wurden, z. B. eine Mobiltelefonnummer oder Fragen und Antworten.
    Beachten Sie, dass bei der Registrierung für das Zurücksetzen des Kennworts kein Unterschied zwischen SMS-Gate und MFA-Gate gemacht wird. Beide werden als **Mobiltelefon**betrachtet.

-   Self-service-Gruppenaktivität zeigt jeden Versuch eines Benutzers selbst hinzufügen oder löschen selbst zu einer Gruppe oder eine Gruppe erstellen.

    ![](media/MIM-Hybrid-passwordreset.jpg)

> [!NOTE]
> Die Berichte stellen derzeit maximal die Daten für bis zu einem Monat dar.
>
> Wenn Sie Hybridberichte deinstallieren möchten, deinstallieren Sie den Agent „MIMreportingAgent.msi“.

## Voraussetzungen

1.  Installieren Sie Microsoft Identity Manager 2016 einschließlich des MIM-Diensts.

2.  Stellen Sie sicher, dass Sie über einen Azure AD Premium-Mandanten mit lizenziertem Administrator in Ihrem Verzeichnis verfügen.

3.  Stellen Sie sicher, dass Sie über eine ausgehende Internetverbindung vom Microsoft Identity Manager-Server zu Azure verfügen.

## Installieren der Microsoft Identity Manager-Berichterstellung in Azure AD
Nachdem der Agent für die Berichterstellung installiert wurde, werden die Daten der Microsoft Identity Manager-Aktivität von Microsoft Identity Manager in das Windows-Ereignisprotokoll exportiert. Der Agent für die Microsoft Identity Manager-Berichterstellung verarbeitet die Ereignisse und lädt sie in Azure hoch. Die Ereignisse werden in Azure hinsichtlich der erforderlichen Berichte analysiert, entschlüsselt und gefiltert.

1.  Installieren Sie Microsoft Identity Manager 2016.

2.  Laden Sie die Microsoft Identity Manager-Agents für die Berichterstellung herunter:

    1.  Melden Sie sich beim Azure AD-Verwaltungsportal an, und klicken Sie auf das Symbol für Active Directory.

    2.  Doppelklicken Sie auf das Verzeichnis, für das Sie als globaler Administrator angemeldet und ein Azure AD Premium-Abonnement besitzen.

    3.  Klicken Sie auf **Konfiguration** , und laden Sie den Agent für die Berichterstellung herunter.

3.  Installieren Sie den Microsoft Identity Manager-Agent für die Berichterstellung:

    1.  Erstellen Sie ein Verzeichnis auf dem Computer.

    2.  Extrahieren Sie die Dateien `MIMHybridReportingAgent.msi` und `tenant.cert` in diesem Verzeichnis.

    3.  Führen Sie das Installationsprogramm des Agents aus.

    4.  Stellen Sie sicher, dass der Agent-Dienst für die Microsoft Identity Manager-Berichterstellung ausgeführt wird.

    5.  Starten Sie den Microsoft Identity Manager-Dienst neu.

4.  Überprüfen Sie, ob die Microsoft Identity Manager-Berichterstellung in Azure funktioniert.

    Sie können die Berichtsdaten über das Self-Service-Portal zum Zurücksetzen von Kennwörtern von Microsoft Identity Manager erstellen, um das Kennwort eines Benutzers zurückzusetzen. Stellen Sie sicher, dass das Zurücksetzen des Kennworts erfolgreich abgeschlossen wurde, und überprüfen Sie dann, ob die Daten im Azure AD-Verwaltungsportal angezeigt werden.

## Anzeigen von Hybridberichten im Azure-Verwaltungsportal

1.  Melden Sie sich in Azure mit Ihrem globalen Administratorkonto für den Mandanten an.

2.  Klicken Sie auf das **Active Directory** -Symbol.

3.  Wählen Sie das Mandantenverzeichnis aus der Liste der verfügbaren Verzeichnisse für Ihr Abonnement aus.

4.  Klicken Sie auf **Berichte** und dann auf die **Aktivität „Zurücksetzen des Kennworts“**.

5.  Stellen Sie sicher, dass Sie **Identity Manager** im Dropdownmenü der Quelle auswählen.

> [!WARNING]
> Es kann einige Zeit dauern, bis Microsoft Identity Manager-Daten in Azure AD angezeigt werden.

## Beenden des Sendens von Microsoft Identity Manager-Ereignissen an Azure
Wenn Sie das Hochladen von Berichtsdaten von Microsoft Identity Manager in Azure Active Directory beenden möchten, sollten Sie den Agent für die Hybridberichterstellung deinstallieren. Deinstallieren Sie die Microsoft Identity Manager-Hybridberichterstellung mithilfe des Windows-Tools **Software** .

## Für die Microsoft Identity Manager-Berichterstellung in Azure AD verwendeten Windows-Ereignisse
Von Microsoft Identity Manager generierten Ereignisse werden im Windows-Ereignisprotokoll protokolliert und stehen in der Ereignisanzeige unter: Anwendung und Dienste Protokolle & Gt; **Identity Manager-Anforderungsprotokoll**. Jede Microsoft Identity Manager-Anforderung wird als Ereignis in das Windows-Ereignisprotokoll in der JSON-Struktur exportiert. Dieses kann in Ihr SIEM exportiert werden.

|Ereignistyp|ID|Ereignisdetails|
|--------------|------|-----------------|
|Informationen|4121|Die FIM-Ereignisdaten, die alle Anforderungsdaten einbeziehen.|
|Informationen|4137|Die Erweiterung des FIM-Ereignisses 4121, für den Fall, dass für ein einzelnes Ereignis zu viele Daten vorliegen. Der Header in diesem Ereignis liegt in der folgenden Form vor: `"Request: <GUID> , message <xxx> out of <xxx>`|
<!--HONumber=Mar16_HO1-->
