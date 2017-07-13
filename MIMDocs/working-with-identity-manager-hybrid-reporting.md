---
title: "Arbeiten mit der Hybridberichterstellung in Azure über MIM 2016 | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie lokale Daten und Clouddaten als Hybridberichte in Azure kombinieren und wie Sie diese Berichte verwalten und anzeigen können."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# Arbeiten mit der Identity Manager-Hybridberichterstellung – öffentliche Vorschau (aktualisiert)
<a id="working-with-identity-manager-hybrid-reporting---public-preview-refresh" class="xliff"></a>

## Verfügbare Hybridberichte
<a id="available-hybrid-reports" class="xliff"></a>
Die ersten drei in Azure AD verfügbaren Microsoft Identity Manager-Berichte (MIM) sind die **Aktivität „Zurücksetzen des Kennworts“**, die **Registrierung für das Zurücksetzen des Kennworts** und die **Self-Service-Gruppenaktivität**.

-   Die Aktivität „Zurücksetzen des Kennworts“ zeigt jede Instanz an, wenn ein Benutzer mithilfe von SSPR die Kennwortzurücksetzung durchgeführt hat und die für die Authentifizierung verwendeten Gates oder **Methoden** bereitstellt.

-   Die Aktivität „Registrierung für Zurücksetzen des Kennworts“ wird jedes Mal angezeigt, wenn sich ein Benutzer für SSPR registriert und die **Methoden** zum Authentifizieren verwendet wurden, z. B. eine Mobiltelefonnummer oder Fragen und Antworten.
    Beachten Sie, dass bei der Registrierung für das Zurücksetzen des Kennworts kein Unterschied zwischen SMS-Gate und MFA-Gate gemacht wird. Beide werden als **Mobiltelefon**betrachtet.

-   Die Self-Service-Gruppenaktivität zeigt jeden Versuch einer Person an, sich selbst einer Gruppe hinzuzufügen oder sich aus einer Gruppe und einer Gruppenerstellung zu löschen.

    ![Bild: Azure-Hybridberichterstellung – Aktivität „Zurücksetzen des Kennworts“](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Die Berichte stellen derzeit maximal die Daten für bis zu einem Monat dar.</br>
> Der vorherige Hybrid-Agent muss deinstalliert werden.</br>
> Wenn Sie Hybridberichte deinstallieren möchten, deinstallieren Sie den Agent „MIMreportingAgent.msi“.

## Voraussetzungen
<a id="prerequisites" class="xliff"></a>

1.  Installieren Sie den Microsoft Identity Manager 2016 RTM oder den Service Pack 1 MIM-Dienst.

2.  Stellen Sie sicher, dass Sie über einen Azure AD Premium-Mandanten mit lizenziertem Administrator in Ihrem Verzeichnis verfügen.

3.  Stellen Sie sicher, dass Sie über eine ausgehende Internetverbindung vom Microsoft Identity Manager-Server zu Azure verfügen.

## Anforderungen
<a id="requirements" class="xliff"></a>
In der folgenden Tabelle werden die Anforderungen zum Verwenden der Microsoft Identity Manager-Hybridberichterstellung aufgelistet.

| Anforderung | Beschreibung |
| --- | --- |
| Azure AD Premium | Die Hybridberichterstellung ist eine Funktion von Azure AD Premium und erfordert Azure AD Premium. </br></br>Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium). </br>Informationen zum Starten einer 30-Tage-Testversion finden Sie im Abschnitt zum[Starten einer Testversion](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Sie müssen zum Ausführen der ersten Schritte ein globaler Administrator Ihres Azure AD-Systems sein. |Standardmäßig können nur globale Administratoren die Agents installieren und konfigurieren, die zum Einstieg, für den Portalzugriff und zum Ausführen von Vorgängen in Azure erforderlich sind. </br></br>**Wichtig:** Für die Installation der Agents muss ein Geschäfts- oder Schul-/Unikonto verwendet werden. Ein Microsoft-Konto kann nicht verwendet werden. Weitere Informationen finden Sie unter [Als Unternehmen für Azure registrieren](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization). |
| Der Microsoft Identity Manager-Hybrid-Agent ist auf jedem MIM-Zieldienstserver installiert. | Für die Hybridberichterstellung müssen die Agents auf den Zielservern installiert und konfiguriert sein, um die Daten zu empfangen und Überwachungs- und Analysefunktionen bereitzustellen. </br>|
| Ausgehende Verbindungen zu den Azure-Dienstendpunkten | Während der Installation und der Laufzeit benötigt der Agent Konnektivität mit den Azure-Dienstendpunkten. Wenn die ausgehende Konnektivität durch Firewalls blockiert wird, stellen Sie sicher, dass die folgenden Endpunkte der Zulassungsliste hinzugefügt werden: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net – Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Ausgehende Konnektivität basierend auf IP-Adresse | Informationen zu einer auf IP-Adressen basierenden Firewallfilterung finden Sie unter [Azure-IP-Bereiche](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| Die SSL-Inspektion für ausgehenden Datenverkehr wird gefiltert oder ist deaktiviert. | Bei der Agent-Registrierung oder bei Vorgängen zum Hochladen von Daten kann es zu Fehlern kommen, wenn auf der Netzwerkebene eine SSL-Inspektion oder -Terminierung für ausgehenden Datenverkehr erfolgt. |
| Firewallports auf dem Server, der den Agent ausführt |Der Agent erfordert, dass die folgenden Firewallports geöffnet sind, damit der Agent mit den Azure-Dienstendpunkten kommunizieren kann.</br></br><li>TCP-Port 443</li><li>TCP-Port 5671</li> |
| Bei aktivierter verstärkter Sicherheit für IE folgende Websites zulassen |Wenn die verstärkte Sicherheitskonfiguration für Internet Explorer auf dem Server aktiviert ist, auf dem der Agent installiert wird, müssen die folgenden Websites zugelassen werden.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Der Verbundserver für Ihre Organisation, dem Azure Active Directory vertraut. Beispiel: https://sts.contoso.com</li> |
</BR>

## Installieren des Microsoft Identity Manager-Berichterstellungs-Agents in Azure AD
<a id="install-microsoft-identity-manager-reporting-agent-in-azure-ad" class="xliff"></a>
Nachdem der Agent für die Berichterstellung installiert wurde, werden die Daten der Microsoft Identity Manager-Aktivität von MIM in das Windows-Ereignisprotokoll exportiert. Der MIM-Berichterstellungs-Agent verarbeitet die Ereignisse und lädt diese in Azure hoch. Die Ereignisse werden in Azure hinsichtlich der erforderlichen Berichte analysiert, entschlüsselt und gefiltert.

1.  Installieren Sie Microsoft Identity Manager 2016.

2.  Laden Sie die Microsoft Identity Manager-Agents für die Berichterstellung herunter:

    1.  Melden Sie sich beim Azure AD-Verwaltungsportal an, und klicken Sie auf das Symbol für Active Directory.

    2.  Doppelklicken Sie auf das Verzeichnis, für das Sie als globaler Administrator angemeldet und ein Azure AD Premium-Abonnement besitzen.

    3.  Klicken Sie auf **Konfiguration** , und laden Sie den Agent für die Berichterstellung herunter.

3.  Installieren Sie den Microsoft Identity Manager-Agent für die Berichterstellung:

    1.  Laden Sie [mimhreportingagentsetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) auf den MIM-Dienstserver herunter.
    2.  Ausführen von `MIMHReportingAgentSetup.exe` 
    3.  Führen Sie das Installationsprogramm des Agents aus.

    4.  Stellen Sie sicher, dass der Agent-Dienst für die MIM-Berichterstellung ausgeführt wird

    5.  Starten Sie den MIM-Dienst neu.

4.  Überprüfen Sie, ob die Microsoft Identity Manager-Berichterstellung in Azure funktioniert.

    Sie können die Berichtsdaten über das Self-Service-Portal zum Zurücksetzen von Kennwörtern von Microsoft Identity Manager erstellen, um das Kennwort eines Benutzers zurückzusetzen. Stellen Sie sicher, dass das Zurücksetzen des Kennworts erfolgreich abgeschlossen wurde, und überprüfen Sie dann, ob die Daten im Azure AD-Verwaltungsportal angezeigt werden.

## Anzeigen von Hybridberichten im Azure-Portal
<a id="view-hybrid-reports-in-the-azure-portal" class="xliff"></a>

1.  Melden Sie sich mit Ihrem globalen Administratorkonto für den Mandanten im [Azure-Portal](https://portal.azure.com/) an.

2.  Klicken Sie auf das **Azure Active Directory**-Symbol.

3.  Wählen Sie das Mandantenverzeichnis aus der Liste der verfügbaren Verzeichnisse für Ihr Abonnement aus.

4.  Klicken Sie auf **Audit Logs** (Überwachungsprotokolle).

5.  Achten Sie darauf, dass Sie **MIM Service** (MIM-Dienst) im Dropdownmenü „Category“ (Kategorie) auswählen.

> [!WARNING]
> Es kann einige Zeit dauern, bis Microsoft Identity Manager-Überwachungsdaten im Azure-Portal angezeigt werden.

## Beenden der Hybridberichterstellung
<a id="stop-creating-hybrid-reports" class="xliff"></a>
Falls Sie das Hochladen von Überwachungsberichtdaten von Microsoft Identity Manager in Azure Active Directory beenden möchten, deinstallieren Sie den Agent für die hybride Berichterstellung. Deinstallieren Sie die Microsoft Identity Manager-Hybridberichterstellung mithilfe des Windows-Tools **Programme hinzufügen oder entfernen**.

## Für die Hybridberichterstellung verwendete Windows-Ereignisse
<a id="windows-events-used-for-hybrid-reporting" class="xliff"></a>
Von Microsoft Identity Manager generierte Ereignisse werden im Windows-Ereignisprotokoll protokolliert und in der Ereignisanzeige unter **Anwendungs- und Dienstprotokolle-&gt; Identity Manager-Anforderungsprotokoll** angezeigt. Jede MIM-Anforderung wird als Ereignis in das Windows-Ereignisprotokoll in der JSON-Struktur exportiert. Dieses kann in Ihr SIEM exportiert werden.

|Ereignistyp|ID|Ereignisdetails|
|--------------|------|-----------------|
|Informationen|4121|Die MIM-Ereignisdaten, die alle Anforderungsdaten beinhalten.|
|Informationen|4137|Die Erweiterung des MIM-Ereignisses 4121, für den Fall, dass für ein einzelnes Ereignis zu viele Daten vorliegen. Der Header in diesem Ereignis liegt in der folgenden Form vor: `"Request: <GUID> , message <xxx> out of <xxx>`|
