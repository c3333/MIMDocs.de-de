---
title: Arbeiten mit der Hybridberichterstellung in Azure mit Identity Manager 2016 | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie lokale Daten und Clouddaten als Hybridberichte in Azure kombinieren und wie Sie diese Berichte verwalten und anzeigen können."
keywords: 
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: e135cc5066220765d97568b3a1e1b984a876b2a2
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Arbeiten mit der Hybridberichterstellung im Identity Manager

Dieser Artikel erläutert, wie Sie lokale Daten und Clouddaten in Azure in Hybridberichte kombinieren und wie Sie diese Berichte verwalten und anzeigen können.

## <a name="available-hybrid-reports"></a>Verfügbare Hybridberichte
Dies sind die ersten drei Microsoft Identity Manager-Berichte, die in Azure Active Directory (Azure AD) verfügbar sind:

- **Aktivität zum Zurücksetzen des Kennworts**: Zeigt jede Durchführung einer Kennwortzurücksetzung mithilfe von SSPR (Self-Service Password Reset, Self-Service-Kennwortzurücksetzung) durch einen Benutzer an und stellt die für die Authentifizierung verwendeten Gates oder Methoden bereit.

- **Registrierung für die Kennwortzurücksetzung**: Zeigt jedes Vorkommen einer Benutzerregistrierung für SSPR sowie die zur Authentifizierung verwendete Methode an. Bei der Methode kann es sich z.B. um eine Mobiltelefonnummer oder eine Kombination aus Fragen und Antworten handeln.
   > [!NOTE]
   > Bei der *Registrierung für die Kennwortzurücksetzung* wird kein Unterschied zwischen SMS-Gate und MFA-Gate gemacht. Beide werden als Mobiltelefonmethoden betrachtet.

- **Self-Service-Gruppenaktivität**: Zeigt jeden Versuch einer Person an, sich selbst zu einer Gruppe oder Gruppenerstellung hinzuzufügen oder daraus zu löschen.

    ![Bild: Azure-Hybridberichterstellung – Aktivität „Zurücksetzen des Kennworts“](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Die Berichte stellen derzeit die Daten für bis zu einem Monat Aktivität dar.
> * Der vorherige Hybrid-Bericht-Agent muss deinstalliert werden.
> * Um Hybridberichte zu deinstallieren, deinstallieren Sie den Agent „MIMreportingAgent.msi“.

## <a name="prerequisites"></a>Voraussetzungen

* Identity Manager-Dienst in Identity Manager 2016 SP1, empfohlener Build: [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager)

* Ein Azure AD Premium-Mandant mit lizenziertem Administrator in Ihrem Verzeichnis.

* Ausgehende Internetkonnektivität vom Identity Manager-Server zu Azure.

## <a name="requirements"></a>Anforderungen
Die Anforderungen zum Verwenden der Identity Manager-Hybridberichterstellung sind in der folgenden Tabelle aufgelistet:

| Anforderungen | Beschreibung |
| --- | --- |
| Azure AD Premium | Die Hybridberichterstellung ist ein Feature von Azure AD Premium und erfordert Azure AD Premium. </br>Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Erhalten Sie eine [kostenlose 30-Tage-Testversion von Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Sie müssen globaler Administrator Ihres Azure AD-Verzeichnisses sein |Standardmäßig können nur globale Administratoren die Agents installieren und konfigurieren, die erforderlich sind, um mit der Verwendung des Features zu beginnen, auf das Portal zuzugreifen und Vorgänge in Azure auszuführen. </br>**Wichtig**: Für die Installation der Agents muss ein Geschäfts-, Schul- oder Unikonto verwendet werden. Ein Microsoft-Konto kann nicht verwendet werden. Weitere Informationen finden Sie unter [Als Unternehmen für Azure registrieren](https://docs.microsoft.com/azure/active-directory/sign-up-organization). |
| Der Identity Manager-Hybrid-Agent ist auf jedem Identity Manager-Zieldienstserver installiert | Für die Hybridberichterstellung müssen die Agents auf den Zielservern installiert und konfiguriert sein, um die Daten zu empfangen und Überwachungs- und Analysefunktionen bereitzustellen.  </br>|
| Ausgehende Verbindungen zu den Azure-Dienstendpunkten | Während der Installation und der Laufzeit benötigt der Agent Konnektivität mit den Azure-Dienstendpunkten. Wenn die ausgehende Konnektivität durch Firewalls blockiert wird, stellen Sie sicher, dass die folgenden Endpunkte der Zulassungsliste hinzugefügt werden:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net – Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|Ausgehende Konnektivität basierend auf IP-Adressen | Informationen zu einer auf IP-Adressen basierenden Firewallfilterung finden Sie unter [Azure-IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653).|
| Die SSL-Inspektion für ausgehenden Datenverkehr wird gefiltert oder ist deaktiviert | Bei der Agent-Registrierung oder bei Vorgängen zum Hochladen von Daten kann es zu Fehlern kommen, wenn auf der Netzwerkebene eine SSL-Inspektion oder -Terminierung für ausgehenden Datenverkehr erfolgt. |
| Firewallports auf dem Server, der den Agent ausführt | Damit der Agent mit den Azure-Dienstendpunkten kommunizieren kann, müssen die folgenden Firewallports geöffnet sein:<ul><li>TCP-Port 443</li><li>TCP-Port 5671</li></ul> |
| Bei aktivierter verstärkter Sicherheit in Internet Explorer müssen bestimmte Websites zugelassen sein |Wenn die verstärkte Sicherheitskonfiguration für Internet Explorer aktiviert ist, müssen auf dem Server, auf dem der Agent installiert ist, die folgenden Websites zugelassen sein:<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Der Verbundserver für Ihre Organisation, dem Azure Active Directory vertraut. Zum Beispiel: https://sts.contoso.com).</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Installieren des Identity Manager-Berichterstellungs-Agents in Azure AD
Nachdem der Agent für die Berichterstellung installiert wurde, werden die Daten der Identity Manager-Aktivität von Identity Manager in das Windows-Ereignisprotokoll exportiert. Der Identity Manager-Berichterstellungs-Agent verarbeitet die Ereignisse und lädt sie in Azure hoch. Die Ereignisse werden in Azure hinsichtlich der erforderlichen Berichte analysiert, entschlüsselt und gefiltert.

1.  Installieren Sie Identity Manager 2016.

2.  Laden Sie den Identity Manager-Berichterstellungs-Agent herunter, und führen Sie folgende Schritte aus:

    ein. Melden Sie sich beim Azure AD-Verwaltungsportal an, und wählen Sie **Active Directory** aus.

    b. Doppelklicken Sie auf das Verzeichnis, für das Sie globaler Administrator sind und ein Azure AD Premium-Abonnement besitzen.

    c. Wählen Sie **Konfiguration** aus, und laden Sie den Berichterstellungs-Agent herunter.

3.  Installieren Sie den Berichterstellungs-Agent, indem Sie folgende Schritte ausführen:

    ein.  Laden Sie die Datei [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) auf den Identity Manager-Dienstserver herunter.

    b.  Führen Sie `MIMHReportingAgentSetup.exe` aus. 

    c.  Führen Sie das Installationsprogramm des Agents aus.

    d.  Stellen Sie sicher, dass der Agent-Dienst für die Identity Manager-Berichterstellung ausgeführt wird.

    e.  Starten Sie den Identity Manager-Dienst neu.

4.  Überprüfen Sie, ob der Identity Manager-Berichterstellungs-Agent in Azure funktioniert.

    Sie können Berichtsdaten erstellen, indem Sie das Self-Service-Portal von Identity Manager für die Kennwortzurücksetzung verwenden, um das Kennwort eines Benutzers zurückzusetzen. Stellen Sie sicher, dass die Kennwortzurücksetzung erfolgreich abgeschlossen wurde, und überprüfen Sie dann, ob die Daten im Azure AD-Verwaltungsportal angezeigt werden.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Anzeigen von Hybridberichten im Azure-Portal

1.  Melden Sie sich mit Ihrem globalen Administratorkonto für den Mandanten beim [Azure-Portal](https://portal.azure.com/) an.

2.  Wählen Sie **Azure Active Directory** aus.

3.  Wählen Sie aus der Liste der verfügbaren Verzeichnisse für Ihr Abonnement das Mandantenverzeichnis aus.

4.  Wählen Sie **Überwachungsprotokolle** aus.

5.  Vergewissern Sie sich, dass in der Dropdownliste **Kategorie** der **MIM-Dienst** ausgewählt ist.

> [!IMPORTANT]
> Es kann einige Zeit dauern, bis Identity Manager-Überwachungsdaten im Azure-Portal angezeigt werden.

## <a name="stop-creating-hybrid-reports"></a>Beenden der Hybridberichterstellung
Falls Sie das Hochladen von Überwachungsberichtdaten von Identity Manager nach Azure Active Directory beenden möchten, deinstallieren Sie den Agent für die Hybridberichterstellung. Deinstallieren Sie die Identity Manager-Hybridberichterstellung mithilfe des Windows-Tools „Programme hinzufügen oder entfernen“.

## <a name="windows-events-used-for-hybrid-reporting"></a>Für die Hybridberichterstellung verwendete Windows-Ereignisse
Vom Identity Manager generierte Ereignisse werden im Windows-Ereignisprotokoll gespeichert. Sie können die Ereignisse in der **Ereignisanzeige** anzeigen, indem Sie **Anwendungs- und Dienstprotokolle** > **Identity Manager-Anforderungsprotokoll** auswählen. Jede Identity Manager-Anforderung wird als Ereignis in das Windows-Ereignisprotokoll in der JSON-Struktur exportiert. Sie können das Ergebnis in Ihr SIEM-System (Security Information and Event Management) exportieren.

|Ereignistyp|ID|Ereignisdetails|
|--------------|------|-----------------|
|Informationen|4121|Die Identity Manager-Ereignisdaten, die alle Anforderungsdaten beinhalten.|
|Informationen|4137|Die Erweiterung des Identity Manager-Ereignisses 4121, für den Fall, dass für ein einzelnes Ereignis zu viele Daten vorliegen. Der Header in diesem Ereignis wird in folgendem Format angezeigt: `"Request: <GUID> , message <xxx> out of <xxx>`.|
