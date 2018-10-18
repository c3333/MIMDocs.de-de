---
title: Kennwortverwaltung mit Microsoft Identity Manager 2016 | Microsoft-Dokumentation
description: ''
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: def1be943b4f2f919a079e3fc4aa544af10463aa
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333393"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Kennwortverwaltung mit Microsoft Identity Manager 2016

Das Verwalten von Kennwörtern mehrerer Benutzerkonten gehört zu den komplexeren Aspekten der Verwaltung einer Unternehmensumgebung mit mehreren Datenquellen. Microsoft Identity Manager 2016 (MIM) bietet zwei Lösungen zum Verwalten von Kennwörtern:

-   Kennwortsynchronisierung: verwendet den Benachrichtigungsdienst für Kennwortänderungen (PCNS), um Kennwortänderungen von Active Directory (AD) zu erfassen und diese an andere verknüpfte Datenquellen zu verteilen.

-   Verwaltung von vom Benutzer ausgehenden Kennwortänderungen: verwendet Windows Management Instrumentation (WMI) über den webbasierten Helpdesk und Anwendungen zur Self-Service-Kennwortzurücksetzung.

Durch die Kennwortsynchronisierung und die Verwaltung von vom Benutzer ausgehenden Kennwortänderungen können Sie:

-   die Zahl der verschiedenen Kennwörter reduzieren, die sich ein Benutzer merken muss

-   Kennwörter für die verschiedenen Konten eines Benutzers auf das gleiche Kennwort festlegen oder ändern

-   Benutzern die Möglichkeit geben, ihr eigenes Kennwort in AD zu ändern und diese Änderung an andere Systeme zu melden

-   das Risiko ausschalten, dass ein zusätzlicher Kennwort- oder Anmeldeinformationsspeicher erstellt wird

-   Kennwörter datenquellenübergreifend mit AD als autoritativer Quelle synchronisieren

-   Kennwortverwaltungsvorgänge in Echtzeit ausführen, und zwar unabhängig von MIM-Vorgängen

## <a name="password-extensions"></a>Kennworterweiterungen

Verwaltungs-Agents für Verzeichnisserver unterstützen standardmäßig Kennwortänderungen und Festlegungsvorgänge. Sie können für dateibasierte und Datenbankverwaltungs-Agents und für Verwaltungs-Agents für Extensible Connectivity eine DLL der .NET-Kennworterweiterung erstellen, wenn diese Agents Kennwortänderungen und Festlegungsvorgänge nicht standardmäßig unterstützen.
Die DLL der .NET-Kenntworterweiterung wird immer dann aufgerufen, wenn ein Aufruf zur Kennwortänderung oder -festlegung für einen dieser Verwaltungs-Agents vorgenommen wird. Die Kennworterweiterungseinstellungen für diese Verwaltungs-Agents können in Synchronization Service Manager konfiguriert werden. Weitere Informationen zum Konfigurieren von Kennworterweiterungen finden Sie in der Referenz zum FIM-Entwickler.

| In den Verwaltungs-Agents wird die Kennwortverwaltung standardmäßig für Folgendes unterstützt: | Durch das Verwenden einer Kennworterweiterung wird die Kennwortverwaltung auch in folgenden Verwaltungs-Agents unterstützt: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Attribut-Wert-Paar-Textdateien                                                                    |
| Active Directory Lightweight Directory Services (ADLDS)                   | Textdateien mit Trennzeichen                                                                               |
| IBM-Verzeichnisserver                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Textdateien mit fester Breite                                                                             |
| Verzeichnisserver für Sun und Netscape                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | LDAP Data Interchange Format (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle-Datenbank                                                                                    |

## <a name="password-synchronization"></a>Kennwortsynchronisierung


Die Kennwortsynchronisierung funktioniert mit dem Benachrichtigungsdienst für Kennwortänderungen (PCNS) in einer AD-Domäne. Dadurch ist es möglich, Kennwortänderungen, die von AD ausgehen, an andere verknüpfte Datenquellen zu verteilen. Dies ist möglich, da MIM als RPC-Server (Remote Procedure Call) ausgeführt wird, der auf Benachrichtigungen zu Kennwortänderungen von einem AD-Domänencontroller lauscht. Sobald die Anforderung für eine Kennwortänderung erfasst und authentifiziert wurde, wird diese von MIM verarbeitet und an die entsprechenden Verwaltungs-Agents verteilt.

> [!IMPORTANT]
> Die bidirektionale Kennwortsynchronisierung wird von MIM nicht unterstützt. Durch das Konfigurieren der bidirektionalen Kennwortsynchronisierung kann es zu einer Schleife kommen, die Serverressourcen beansprucht und möglicherweise AD und MIM beeinträchtigt.

Der PCNS wird auf jedem AD-Domänencontroller ausgeführt. Die Systeme, die die Kennwortbenachrichtigungen erhalten, werden als Ziele bezeichnet. Ihr MIM-Server muss als PCNS-Ziel in AD konfiguriert sein, bevor Kennwortbenachrichtigungen gesendet werden können. Die PCNS-Konfigurierung muss eine Einschlussgruppe definieren. Optional kann sie auch eine Ausschlussgruppe definieren. Diese Gruppen werden verwendet, um die Bewegung vertraulicher Kennwörter aus der Domäne einzuschränken. Wenn Sie z.B. die Kennwörter aller Benutzer, aber nicht die der Administratoren, versenden möchten, können Sie die Domänenbenutzer als Einschluss- und die Domänenadministratoren als Ausschlussgruppe festlegen. Weiter Informationen zur Konfigurierung des PCNS finden Sie unter [Using Password Synchronization (Verwenden der Kennwortsynchronisierung)](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx).

Dies sind die Komponenten, die in den Kennwortsynchronisierungsprozess involviert sind:

-   **Benachrichtigungsdienst für Kennwortänderungen („pcnssvc.exe“)**: Der PCNS wird auf einem Domänencontroller ausgeführt und erfasst Benachrichtigungen bezüglich Kennwortänderungen von einem lokalen Kennwortfilter. Diese fügt er einer Warteschlange für den Zielserver hinzu, der MIM ausführt, und stellt sie mit RPC bereit. Der Dienst verschlüsselt das Kennwort und stellt sicher, dass es so lange geschützt ist, bis es dem MIM-Zielserver bereitgestellt wurde.

-   **Dienstprinzipalname (Service Principal Name, SPN)**: Der SPN ist eine Eigenschaft des Kontoobjekts in AD, die vom Kerberos-Protokoll verwendet wird, um den PCNS und das Ziel zu authentifizieren. Der SPN stellt sicher, dass der PCNS den richtigen MIM-Server authentifiziert, und dass kein andere Dienst die Benachrichtigungen über Kennwortänderungen empfängt. Der SPN wird mit dem Tool „setspn.exe“ erstellt und zugewiesen. Weiter Informationen zur Konfigurierung des SPN finden Sie unter „Using Password Synchronization (Verwenden der Kennwortsynchronisierung)“.

-   **Filter für Benachrichtigungen zu Kennwortänderungen („pcnsflt.dll“)**: Der Kennwortfilter wird zum Empfangen von Nur-Text-Kennwörtern von AD verwendet. Dieser Filter wird von der lokalen Sicherheitsautorität (LSA) auf jedem Windows Server-Domänencontroller geladen, der an der Verteilung von Kennwörtern an einen MIM-Zielserver beteiligt ist. Sobald der Filter installiert und der Domänencontroller neu gestartet wurde, empfängt der Filter Benachrichtigungen zu Kennwortänderungen, die von diesem Domänencontroller ausgehen. Der Filter für Kennwortbenachrichtigungen wird gleichzeitig mit anderen Filtern auf dem Domänencontroller ausgeführt.

-   **Hilfsprogramm für die Konfigurierung des Benachrichtigungsdienstes für Kennwortänderungen („pcnscfg.exe“)**: Das Hilfsprogramm „pcnscfg.exe“ wird verwendet, um die Konfigurationsparameter des Benachrichtigungsdienstes für Kennwortänderungen, die in AD gespeichert sind, zu verwalten und zu warten. Zu diesen Konfigurationsparametern zählen z.B. Parameter zum Definieren der Zielserver, des Wiederholungsintervalls der Kennwortwarteschlange und Parameter zum Aktivieren und Deaktivieren der Zielserver. Sie werden bei der Authentifizierung und beim Senden von Kennwortbenachrichtigungen an den MIM-Zielserver verwendet.
    Die Dienstkonfiguration wird in AD gespeichert, sodass die Konfiguration immer nur auf einem Domänencontroller aktualisiert werden muss. AD repliziert die Änderung auf allen anderen Domänencontrollern.

-   **RPC-Server (Remote Procedure Call) auf dem Server, auf dem MIM ausgeführt wird**: Wenn die Kennwortsynchronisierung aktiviert ist, wird er RPC-Server auf dem Server, auf dem MIM ausgeführt wird, gestartet. So kann er Benachrichtigungen des Benachrichtigungsdienstes zu Kennwortänderungen empfangen. RPC wählt dynamisch einen zu verwendenden Portbereich. Wenn Sie möchten, dass MIM mit der AD-Gesamtstruktur durch eine Firewall hindurch kommuniziert, müssen Sie einen Portbereich öffnen.

-   **DLL der Kennworterweiterung**: Mit der DLL der Kennworterweiterung können Sie Vorgänge zum Festlegen und Ändern von Kennwörtern mithilfe einer Regelerweiterung für jeden dateibasierten oder Datenbankverwaltungs-Agent und Verwaltungs-Agent für Extensible Connectivity implementieren.
    Dafür müssen Sie ein verschlüsseltes Attribut mit dem Namen „export_password“ erstellen, das nur für den Export verwendet wird. Dieses Attribut darf im verknüpften Verzeichnis nicht vorhanden sein. Außerdem sollte es in einer Regelerweiterung zur Bereitstellung erreichbar bzw. festlegbar sein. Zusätzlich sollte es während des Export-Attributflusses verwendet werden können. Weiter Informationen zum Konfigurieren von Kennworterweiterungen finden Sie in der [Referenz zum FIM-Entwickler](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Vorbereiten der Kennwortsynchronisierung

Überprüfen Sie vor dem Einrichten der Kennwortsynchronisierung für Ihre MIM- und AD-Umgebung Folgendes:

-   Wurde MIM gemäß den Installationsanweisungen installiert?

-   Wurden Verwaltungs-Agents für die verknüpften Datenquellen, die für die Kennwortsynchronisierung verwaltet werden sollen, bereits erstellt, und wurden die Objekte erfolgreich verknüpft und synchronisiert?

Einrichten der Kennwortsynchronisierung:

-   Erweitern Sie das AD-Schema, um für die Installation und das Ausführen des PCNS notwendige Klassen und Attribute hinzuzufügen.

-   Installieren Sie den PCNS auf jedem Domänencontroller.

-   Konfigurieren Sie den SPN in AD für das MIM-Dienstkonto.

-   Konfigurieren Sie den PCNS so, dass er mit dem MIM-Zieldienst kommuniziert.

-   Konfigurieren Sie den Verwaltungs-Agent so, dass die verknüpften Datenquellen zur Kennwortsynchronisierung verwaltet werden.

-   Aktivieren Sie die Synchronisierung in MIM.

Weitere Informationen zum Einrichten der Kennwortsynchronisierung finden Sie unter „Using Password Synchronization“ (Verwenden der Kennwortsynchronisierung).

## <a name="password-synchronization-process"></a>Kennwortsynchronisierungsprozess

In folgendem Diagramm ist der Prozess der Anforderung einer Kennwortänderung von einem AD-Domänencontroller an andere verknüpfte Datenquellen dargestellt:

1.  Der Benutzer initiiert die Anforderung der Kennwortänderung mit der Tastenkombination STRG+ALT+ENTF. Die Anforderung der Kennwortänderung, einschließlich des neuen Kennworts, wird an den nächsten Domänencontroller gesendet.

2.  Der Domänencontroller erfasst die Anforderung der Kennwortänderung und benachrichtigt den Benachrichtigungsfilter zur Kennwortänderung („pcnsflt.dll“).

3.  Der Benachrichtigungsfilter übergibt die Anforderung an den PCNS.

4.  Der PCNS überprüft die Anforderung einer Kennwortänderung und authentifiziert anschließend den SPN mithilfe von Kerberos. Danach leitet er die Anforderung der Kennwortänderung in einem verschlüsselten RPC an den MIM-Zielserver weiter.

5.  MIM überprüft den Quelldomänencontroller und verwendet anschließend den Domänennamen, um den Verwaltungs-Agent zu suchen, der für diese Domäne zuständig ist. Außerdem verwendet MIM die Benutzerkontoinformationen in der Anforderung der Kennwortänderung, um das dazugehörige Objekt im Connectorbereich zu suchen.

6.  Mithilfe der Informationen der Verknüpfungstabelle bestimmt MIM die Verwaltungs-Agents, die die Kennwortänderung erhalten, und übermittelt die Kennwortänderung an diese.

## <a name="password-synchronization-security"></a>Sicherheit bei der Kennwortsynchronisierung

Folgende Sicherheitsbedenken wurden für die Kennwortsynchronisierung geäußert:

-   Authentifizierung der Kennwortquelle: Bei Erhalt der Anforderung einer Kennwortänderung wird die Authentifizierung mit Kerberos durch MIM vorgenommen. Dies ist auch der Fall beim Quelldomänencontroller. Dadurch wird sichergestellt, dass sowohl der Sender als auch der Empfänger zulässig sind. Bei Erhalt einer Anforderung der Kennwortänderung stellt MIM sicher, dass der Aufrufer über ein Konto im Domänencontroller-Container der Domäne verfügt, der er angehört.

-   Kennwortsynchronisierung mit einer Zieldatenquelle konnte aufgrund einer unsicheren Verbindung nicht durchgeführt werden: Wenn der Verwaltungs-Agent so konfiguriert wurde, dass er eine sichere Verbindung erfordert, aber keine sichere Verbindungen gefunden werden, schlägt die Synchronisierung fehl.
    Wenn der Verwaltungs-Agent so konfiguriert wurde, dass er auch unsichere Verbindungen zulässt, wird die Synchronisierung ganz normal durchgeführt. Das Zulassen unsicherer Verbindungen sollte nur dann aktiviert werden, wenn Sie sich über die Risiken im Klaren sind.

-   Sicheres Speichern von Kennwörtern: MIM speichert verschlüsselte Kennwörter nur vorübergehend. Alle in MIM während eines Vorgangs zur Kennwortänderungsbenachrichtigung erfassten Kennwörter werden verschlüsselt, sobald sie in den MIM-Prozess eintreten.
    Sobald sie erfolgreich an eine verknüpfte Zieldatenquelle geschickt wurden, werden sie entschlüsselt, und der Speicher, in dem das Kennwort gespeichert wird, wird bereinigt. Wenn der Vorgang nicht in die verknüpfte Zieldatenquelle schreiben kann, wird das verschlüsselte Kennwort so lange gespeichert, bis alle Wiederholungsversuche vorgenommen wurden, und wird anschließend aus dem Speicher gelöscht.

-   Sichere Kennwortwarteschlangen: In Kennwortwarteschlangen des PCNS gespeicherte Kennwörter werden verschlüsselt, bis sie gesendet werden.

## <a name="password-synchronization-error-recovery-scenarios"></a>Wiederherstellungsszenarios nach Fehlern bei der Kennwortsynchronisierung

Idealerweise werden Änderungen ohne Fehler synchronisiert, wenn ein Benutzer ein Kennwort ändert. Folgende Szenarios beschreiben die Wiederherstellung von MIM nach häufig auftretenden Synchronisierungsfehlern:

-   **Fehlgeschlagene Kennwortbenachrichtigung von AD an MIM**: Dies kann auftreten, wenn das Netzwerk offline ist, oder wenn der MIM-Server nicht verfügbar ist. Die Kennwortänderungsbenachrichtigung bleibt in einer lokalen Warteschlange des Domänencontrollers des PCNS. Der PCNS versucht so lange, die Benachrichtigung zu schicken, wie es in der Konfiguration seines Wiederholungsintervalls festgelegt ist.

-   **Fehlgeschlagene Kennwortsynchronisierung mit einer Zieldatenquelle**: Dies kann auch auftreten, wenn das Netzwerk offline ist, oder wenn die Datenquelle nicht verfügbar ist.
    Die Kennwortänderungsbenachrichtigung wird in die Warteschlange eingereiht und gemäß der Einstellung des Verwaltungs-Agents für Wiederholungsversuche und -intervalle erneut gesendet. Alle Kennwörter werden verschlüsselt, wenn sie für einen erneuten Versuch gespeichert werden. Sie werden gelöscht, sobald der Vorgang erfolgreich abgeschlossen oder die maximale Zahl an Versuchen überschritten wurde.

-   **Aktivieren eines betriebsbereiten Standbyservers, auf dem MIM aufgeführt wird, nachdem der Vorgang fehlgeschlagen ist**: Wenn der primäre Server ausfällt, auf dem MIM ausgeführt wird, können Sie einen betriebsbereiten Standbyserver für die Kennwortsynchronisierung festlegen und diesen aktivieren, ohne dass Kennwortänderungen verloren gehen. Weitere Informationen finden Sie unter [MIISactivate: Server Activation Tool (MIISActivate: Serveraktivierungstool)](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx).

Wenn der Vorgang fehlschlägt, kann dies derartig schwerwiegende Gründe haben, dass auch mehrere Versuche nicht zu einem erfolgreichen Abschluss führen. In solchen Fällen wird ein Fehlerereignis verzeichnet und der Vorgang wird abgebrochen. Folgende Ereignisse werden nicht erneut versucht:

| Ereignis | Schweregrad    | Beschreibung                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Informationen | Ein Festlegungsvorgang zur Kennwortsynchronisierung konnte nicht durchgeführt werden, weil der Zeitstempel abgelaufen ist.                                                                      |
| 6921  | Fehler       | Der Festlegungsvorgang zur Kennwortsynchronisierung konnte nicht verarbeitet werden, weil die Kennwortverwaltung im Zielverwaltungs-Agent nicht aktiviert ist.                                |
| 6922  | Fehler       | Der Festlegungsvorgang zur Kennwortsynchronisierung konnte nicht verarbeitet werden, weil die Kennwortverwaltung im Zielverwaltungs-Agent nicht konfiguriert ist.                             |
| 6923  | Warning     | Der Festlegungsvorgang zur Kennwortsynchronisierung konnte nicht verarbeitet werden, weil das Ziel-Connectorbereichsobjekt nicht im verknüpften Verzeichnis gefunden werden konnte.                  |
| 6927  | Fehler       | Der Festlegungsvorgang zur Kennwortsynchronisierung ist fehlgeschlagen, weil das Kennwort nicht den Kennwortrichtlinien des Zielsystems entspricht.                                      |
| 6928  | Fehler       | Der Festlegungsvorgang zur Kennwortsynchronisierung ist fehlgeschlagen, weil die Kennworterweiterung des Zielverwaltungs-Agents nicht so konfiguriert wurde, dass sie Kennwortfestlegungsvorgänge unterstützt. |

## <a name="user-based-password-change-management"></a>Verwaltung von vom Benutzer ausgehenden Kennwortänderungen

MIM stellt zwei Webanwendungen bereit, die Windows-Verwaltungsinstrumentation (WMI) für das Zurücksetzen von Kennwörtern verwenden. Genauso wie bei der Kennwortsynchronisierung aktivieren Sie die Kennwortverwaltung, wenn Sie den Verwaltungs-Agent im Management Agent Designer konfigurieren. Weitere Informationen zur Kennwortverwaltung und WMI finden Sie in der Referenz zum MIM-Entwickler.

MIM erstellt während der Installation zwei Sicherheitsgruppen, die Kennwortverwaltungsvorgänge ausdrücklich unterstützen:

-   FIMSyncBrowse: Mitglieder dieser Gruppe haben die Berechtigung, Informationen zu einem Benutzerkonto zu erfassen, wenn sie Suchvorgänge mit WMI-Abfragen durchführen.

-   FIMSyncPasswordSet: Mitglieder dieser Gruppe haben die Berechtigung, Vorgänge zur Kontensuche, Kennwortfestlegung und -änderungen mit den Kennwortverwaltungsschnittstellen mit WMI durchzuführen.
