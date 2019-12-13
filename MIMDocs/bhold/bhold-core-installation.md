---
title: Installation von BHOLD Core | Microsoft-Dokumentation
description: Dokumentation zur Installation von BHOLD Core
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 48ff4a06233ba95288432c4cfe48e37b4d1449ab
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519153"
---
# <a name="bhold-core-installation"></a>Installation von BHOLD Core

Das BHOLD Core-Modul enthält die wichtigsten Funktionen von BHOLD Suite in Ihrer Umgebung. Das BHOLD Core-Modul muss auf einem Server in Ihrem lokalen Netzwerk installiert und konfiguriert werden, bevor Sie andere BHOLD Suite-Module installieren können.

## <a name="bhold-core-installation-requirements"></a>BHOLD Core – Installationsanforderungen

Das BHOLD Core-Modul bildet die Grundlage von Microsoft BHOLD Suite. Sie müssen das BHOLD Core-Modul installieren, bevor Sie andere Microsoft BHOLD Suite-Module installieren.

### <a name="bhold-core-hardware-requirements"></a>Hardwareanforderungen für BHOLD Core

Das BHOLD Core-Modul bildet die Grundlage von Microsoft BHOLD Suite. Sie müssen das BHOLD Core-Modul installieren, bevor Sie andere Microsoft BHOLD Suite-Module installieren.

|          |        |          |
|----------|--------|----------|
|**Komponente** |**Mindestens** | **Empfohlen** |
|Prozessor | 64-Bit-Prozessor | 64-Bit-Multicore-Prozessor |
| Arbeitsspeicher |3 GB | Mindestens 6 GB |
|Speicher| 30 GB verfügbar |Hängt von der Größe der Bereitstellung ab |
|Netzwerkadapter| 100 Mbit/s-Verbindung mit SQL und dem FIM-Server (Forefront Identity Manager) | 1 Gbit/s-Verbindung mit SQL und dem FIM-Server|

Diese Empfehlungen basieren auf typischen Implementierungen und berücksichtigen nicht die anderen Anwendungen, die auf dem Server ausgeführt werden. Abhängig von Ihrer bestimmten Umgebung müssen Sie möglicherweise Komponenten mit höherer Leistung verwenden.

### <a name="bhold-core-software-requirements"></a>Softwareanforderungen für BHOLD Core

Das BHOLD Core-Modul muss auf einem Computer installiert werden, der die folgenden Anforderungen erfüllt:

- Auf dem Server muss Windows Server 2012 R2 (64-Bit) oder Windows Server 2016 ausgeführt werden. 
- Der Server muss Mitglied einer Active Directory Domain Services-Domäne (AD DS) sein. In Testumgebungen kann der Server ein AD DS-Domänencontroller sein.
- BHOLD Core muss von einem Benutzer installiert werden, der mit einem Konto in derselben Domäne wie der Server angemeldet ist und der zur Gruppe „Domänenadministratoren“ in der Domäne und zur Gruppe „Administratoren“ auf dem Server gehört.
- Microsoft-Internetinformationsdienste (Internet Information Services, IIS) mit ASP.NET müssen auf dem Server installiert sein. IIS muss mit aktivierter Windows-Authentifizierung konfiguriert sein. Wenn BHOLD Core auf Windows Server 2012/2016 installiert ist, muss die IIS 6-Verwaltungskompatibilität installiert sein. Wenn BHOLD Core auf Windows Server 2012 installiert ist, müssen IIS 6-Skripttools installiert sein.
- Das .NET 3.5-Framework muss installiert sein.
  - Da BHOLD Core .NET 3.5 erfordert, kann es nicht auf Server Core installiert werden.
- Silverlight 4 ist für andere BHOLD-Module erforderlich. Deshalb wird empfohlen, dies vor der Installation von BHOLD Core zu installieren.
- Microsoft SQL Server 2014 oder Microsoft SQL Server 2016 müssen entweder auf dem BHOLD Core-Server oder auf einem anderen Server im lokalen Netzwerk installiert sein. 

Auf Windows-Clientcomputern müssen Microsoft Internet Explorer Version 6 oder höher und Microsoft Silverlight Version 4 oder höher ausgeführt werden.

## <a name="before-you-begin"></a>Vorbereitung

Für das BHOLD Core-Modul ist ein Benutzerkonto erforderlich, das für das Authentifizieren und Autorisieren des BHOLD Core-Diensts für den Server und andere Netzwerkentitäten verwendet wird. In diesem Abschnitt wird erklärt, wie dieses Benutzerkonto erstellt und konfiguriert wird. Außerdem wird ein Arbeitsblatt für die Vorinstallation bereitgestellt, das Sie beim Sammeln der Informationen unterstützt, die Sie zum Abschließen des BHOLD Core-Setups benötigen.

>{!WICHTIG} Wenn Sie BHOLD Core installieren, können Sie eine bestehende SQL Server-Datenbank als BHOLD Core-Datenbank verwenden, oder Sie können dem Assistenten für das BHOLD Core-Setup gestatten, die BHOLD Core-Datenbank für Sie zu erstellen. Wenn Sie eine bestehende Datenbank verwenden, müssen Sie sich versichern, dass kein anderer Benutzer auf diese Datenbank zugreifen kann, während Sie BHOLD Core installieren. Überprüfen Sie vor der Installation von BHOLD Core, dass die Zugriffssteuerung der SQL Server-Datenbank keinen Zugriff durch einen anderen Benutzer als den gestattet, der BHOLD Core installiert.

## <a name="required-user-and-group"></a>Erforderliche Benutzer und Gruppen

Das BHOLD Core-Modul muss sich mit einem Benutzerkonto bei der Domäne anmelden können, das für diesen Zweck zur Verfügung steht und Mitglied bei zwei bestimmten Sicherheitsgruppen ist, einschließlich einer, die speziell für das BHOLD Core-Modul erstellt wurde. Die Mitgliedschaft in der Gruppe „Domänenadministratoren“ ist erforderlich, um dieses Verfahren auszuführen.
**Erstellen und Konfigurieren der Benutzer- und Sicherheitsgruppe für BHOLD Core**

1.  Klicken Sie auf dem Domänencontroller auf **Start**, zeigen Sie auf **Verwaltung**, und klicken Sie dann auf **Active Directory-Benutzer und -Computer**.

2.  Erweitern Sie in der Konsolenstruktur die Domäne, in der das Konto erstellt werden soll. Klicken Sie dann mit der rechten Maustaste auf **Benutzer**, zeigen Sie auf **Neu**, und klicken Sie dann auf **Gruppen**.

3.  Geben Sie im Dialogfeld **New Object – Group** (Neues Objekt – Gruppe) in **Gruppenname** den Namen der Gruppe ein (BHOLD-Standardwert: BHOLDApplicationGroup), und klicken Sie anschließend auf **OK**.

4.  Klicken Sie mit der rechten Maustaste auf **Benutzer**, zeigen Sie auf **Neu**, und klicken Sie dann auf **Benutzer**.

5.  Geben Sie unter **Vollständiger Name** einen Namen ein, mit dem Sie das Konto identifizieren können, z.B. „BHOLD Core-Dienstkonto“.

6.  Geben Sie unter **Benutzeranmeldename** den Benutzernamen des BHOLD Core-Dienstkontos (BHOLD-Standard: b1user) ein, und klicken Sie dann auf **Weiter**.

7.  Geben Sie unter **Kennwort** und **Kennwort bestätigen** das Kennwort für das Dienstkonto ein.

8.  Deaktivieren Sie **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**, wählen Sie **Benutzer kann Kennwort nicht ändern** und **Kennwort läuft nie ab** aus, und klicken Sie dann auf **Weiter** und **Fertig stellen**.

9.  Klicken Sie im Ergebnisbereich der Konsole mit der rechten Maustaste auf das Benutzerkonto, und klicken Sie dann auf **Zu einer Gruppe hinzufügen**.

10. Geben Sie im Dialogfeld **Gruppen auswählen** den Anzeigenamen der Gruppe ein, die Sie zuvor erstellt haben, geben Sie ein Semikolon (;) und dann „IIS_IUSRS“ ein.

11. Klicken Sie auf **Namen überprüfen** und dann auf **OK**.  

Das folgende Verfahren muss auf dem Computer ausgeführt werden, auf dem das BHOLD Core-Modul installiert werden soll. Zum Ausführen dieses Verfahrens müssen Sie als Mitglied der Gruppe „Domänenadministratoren“ angemeldet sein.

## <a name="bhold-core-installation-worksheet"></a>Arbeitsblatt für die Installation von BHOLD Core

Bevor Sie mit der Installation des BHOLD Core-Moduls beginnen, müssen Sie darauf vorbereitet sein, die Informationen bereitzustellen, die der Assistent für das BHOLD Core-Setup benötigt, um die Installation abzuschließen. Das folgende Arbeitsblatt unterstützt Sie beim Aufzeichnen dieser Informationen, sodass Sie diese bereitstellen können, wenn sie benötigt werden. Jeder Abschnitt entspricht einer Seite im Assistenten für das BHOLD Core-Setup.

### <a name="account-settings"></a>Konteneinstellungen

| **Element**                                    | **Beschreibung**                                                                                                                                                                                                                                                                                             | **Wert**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden eines Sicherheitsanbieters auf einer Domäne/einem Computer** | Bei der Auswahl dieser Option wird angegeben, dass die Active Directory Domain Services-Sicherheit den Zugriff auf BHOLD Core steuert.                                                                                                                                                                                                  | Aktivieren Sie das Kontrollkästchen. **Wichtig:** Die Installation schlägt fehl, wenn dieses Kontrollkästchen nicht aktiviert ist.                                                                 |
| **Domäne**                                  | Gibt die Domäne an, die den BHOLD-Server, das BHOLD-Dienstkonto und die BHOLD-Anwendungsgruppe enthält. **Wichtig:** Geben Sie den Domänennamen an, indem Sie den kurzen NetBIOS-Namen verwenden, nicht den vollqualifizierten Domänennamen (FQDN). Wenn der FQDN der Domäne beispielsweise „fabrikam.com“ ist, geben Sie den Domänennamen als „FABRIKAM“ an. | Geben Sie den Domänennamen hier ein:                                                                                                                                        |
| **Anwendungsgruppe**                       | Gibt den Namen der Sicherheitsgruppe an, die Sie zuvor in [Erforderliche Benutzer und Gruppen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug) erstellt haben.                                                                                                                                  | Geben Sie den Gruppennamen hier ein:                                                                                                                                         |
| **Dienstbenutzer**                            | Gibt den Anmeldenamen des Dienstbenutzerkontos an, das Sie zuvor in [Erforderliche Benutzer und Gruppen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug) erstellt haben.                                                                                                                      | Geben Sie den Benutzerkontonamen hier ein:                                                                                                                                  |
| **Passwort**                                | Gibt das Kennwort des BHOLD Core-Dienstbenutzerkontos an.                                                                                                                                                                                                                                              | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                |
| **IP/Port der Website**                         | Gibt die IP-Adresse und Portnummer der Website an, die auf dem Intranetserver erstellt werden soll. Ändern Sie den Standardwert (\*) nur, wenn Sie nicht dieselbe IP-Adresse wie die der Standardwebsite verwenden. Ändern Sie die Portnummer nur zu einem verfügbaren Port, wenn der Standardport (5151) bereits verwendet wird.             | Wenn von der Standardwebsite nicht die Standard-IP-Adresse verwendet wird, geben Sie diese IP-Adresse hier an:  Wenn die Standardportnummer bereits verwendet wird, geben Sie die Portnummer für die BHOLD-Website hier an: |

### <a name="database-settings"></a>Datenbankeinstellungen

| **Element**                                       | **Beschreibung**                                                                                                                                                                                                                                                           | **Wert**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden der integrierten Sicherheit**                    | Gibt an, dass die Windows-Authentifizierung für den Zugriff auf die Datenbank verwendet wird.                                                                                                                                                                                                     | Aktivieren Sie das Kontrollkästchen, wenn die Windows-Authentifizierung für das Verbinden mit SQL Server verwendet wird. Deaktivieren Sie das Kontrollkästchen, wenn die SQL Server-Authentifizierung verwendet wird. Wenn die SQL Server-Authentifizierung verwendet wird, muss die Datenbank erstellt werden, bevor das BHOLD Core-Setup ausgeführt wird. **Hinweis:** Wenn die Windows-Authentifizierung verwendet wird, müssen Sie mit einem Konto angemeldet sein, dem die Serverrolle „Systemadministrator“ auf dem Datenbankserver zugewiesen ist. |
| **Datenbankbenutzer** und  **Datenbankkennwort** | Gibt den Benutzernamen und das Kennwort eines Benutzers mit der Serverrolle „Systemadministrator“ auf dem Datenbankserver an. Diese Werte werden nur bereitgestellt, wenn die SQL Server-Authentifizierung verwendet wird.                                                                                               | Geben Sie hier den SQL Server-Benutzernamen an:  Geben Sie hier das SQL Server-Benutzerkennwort an: **Hinweis:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                                                  |
| **Datenbankserver** und  **Datenbankname**   | Gibt den NetBIOS-Namen des Datenbankservers und den Namen der Datenbank (Standard: b1) an, die das BHOLD Core-Setup erstellt. Wenn Sie nicht die Standardinstanz des Datenbankservers verwenden, geben Sie die Instanz des Datenbankservers in der Form *\<Server\>*\\*\<Instanz\>* an. | Geben Sie den Servernamen (oder Server- und Instanznamen) hier ein:  Geben Sie den Datenbanknamen hier ein:                                                                                                                                                                                                                                                                                                                   |
| **Festlegen von Einschränkungen für die Datenbankbenutzer**    | Veraltet.                                                                                                                                                                                                                                                                 | Ändern Sie den Standardwert nicht.                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD Core-Setup

Melden Sie sich zum Installieren des BHOLD Core-Moduls als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das BHOLD Core-Modul installieren möchten: 

- BholdCore *\<Version\>*\_Release.msi

Ersetzen Sie *\<Version\>* durch die Versionsnummer der BHOLD Core-Version, die Sie installieren.

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

### <a name="postinstallation-settings"></a>Einstellungen nach der Installation

Nach Abschluss des BHOLD Core-Setups müssen Sie die Windows-Firewall konfigurieren und die erweiterten Einstellungen im BHOLD Core-Anwendungspool in den Internetinformationsdiensten ändern, um die BHOLD Core-Konfiguration abzuschließen. Bei Bedarf sollten Sie auch die BHOLD-Systemattribute entsprechend Ihren Anforderungen ändern.

#### <a name="configuring-windows-firewall"></a>Konfigurieren der Windows-Firewall

Wenn Benutzer mithilfe eines Webbrowsers auf einem Remotecomputer auf BHOLD zugreifen, müssen Sie die Windows-Firewall auf dem BHOLD Core-Server so konfigurieren, dass sie eingehende Verbindungen mit dem Websiteport zulässt, den Sie bei der Installation von BHOLD Core angegeben haben.

Um dieses Verfahren ausführen zu können, müssen Sie Mitglied der Gruppe Administratoren auf dem lokalen Computer sein.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Zulassen eingehender Verbindungen mit der BHOLD-Website

1.  Klicken Sie auf **Start**, zeigen Sie auf **Verwaltung**, klicken Sie mit der rechten Maustaste auf **Windows Firewall with Advanced Settings** (Windows-Firewall mit erweiterten Einstellungen), und klicken Sie dann auf **Als Administrator ausführen**.

2.  Klicken Sie im linken Bereich auf **Eingehende Regeln**, und klicken Sie dann im rechten Bereich auf **Neue Regel**.

3.  Klicken Sie im Assistenten für neue eingehende Regeln auf **Port** und dann auf **Weiter**.

4.  Vergewissern Sie sich, dass **TCP** ausgewählt ist, und geben Sie in **Bestimmte lokale Ports** die Standardportnummer (5151) für BHOLD Core oder die Portnummer ein, die Sie bei der Installation von BHOLD Core angegeben haben, und klicken Sie dann auf **Weiter**.

5.  Vergewissern Sie sich, dass **Verbindung zulassen** ausgewählt ist, und klicken Sie dann auf **Weiter**.

6.  Deaktivieren Sie auf der Seite **Profil** die Kontrollkästchen für Standorte, von denen aus kein Zugriff auf die BHOLD-Website möglich sein soll, und klicken Sie dann auf **Weiter**.

7.  Geben Sie auf der Seite **Name** einen Namen für die Regel ein (z.B. „Eingehende Verbindungen mit BHOLD Core zulassen“), und klicken Sie dann auf **Fertig stellen**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Aktivieren von 32-Bit-Anwendungen für den BHOLD Core-Anwendungspool

Damit IIS ordnungsgemäß mit dem BHOLD Core-Modul funktioniert, müssen Sie den BHOLD Core-Anwendungspool dafür konfigurieren, 32-Bit-Anwendungen zu unterstützen. Sie müssen mit dem Konto angemeldet sein, das zum Installieren des BHOLD Core-Moduls verwendet wurde, um diesen Vorgang auszuführen.

**Aktivieren der Unterstützung für 32-Bit-Anwendungen für den BHOLD Core-Anwendungspool**

1.  Klicken Sie zum Öffnen des Internetinformationsdienste-Managers auf **Start**, zeigen Sie auf **Verwaltung**, und klicken Sie dann auf **Internetinformationsdienste-Manager (IIS)**.

2.  Erweitern Sie in der Konsolenstruktur den Servernamen, und klicken Sie dann auf **Anwendungspool**.

3.  Klicken Sie in der Liste der **Anwendungspools** mit der rechten Maustaste auf **CoreAppPool**, und klicken Sie dann auf **Erweiterte Einstellungen**.

4.  Wählen Sie im Dialogfeld **Erweiterte Einstellungen** in der Liste **32-Bit-Anwendungen** **Wahr** aus, und klicken Sie dann auf **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Einrichten des Dienstprinzipalnamens für die BHOLD-Website

Wenn der Netzwerkname, der zum Herstellen einer Verbindung mit der BHOLD-Website verwendet wird, nicht der gleiche wie der Serverhostname ist, müssen Sie einen Dienstprinzipalnamen (Service Principal Name, SPN) für HTTP einrichten. Wenn Sie beispielsweise einen CNAME-Ressourceneintrag in DNS verwenden, um einen Alias für den Server anzugeben. Wenn Sie den Netzwerklastenausgleich verwenden, müssen Sie diese zusätzlichen Netzwerkadressen in Active Directory registrieren. Wenn Sie dies nicht tun, kann Internet Explorer das Kerberos-Protokoll beim Herstellen einer Verbindung mit der BHOLD-Website nicht verwenden.

> [!IMPORTANT]
> Wenn das BHOLD Core-Modul auf demselben Computer wie das FIM-Portal installiert ist, müssen Sie DNS-Ressourceneinträge (CNAME oder A) mit unterschiedlichen Hostnamen für die Server, die BHOLD Core ausführen und den Server, der das FIM-Portal ausführt, erstellen. Nur ein SPN kann für ein bestimmtes Servertyp/Serveralias-Paar eingerichtet werden. Darum erfordern BHOLD Core und das FIM-Portal separate SPNs, da sie in der Regel unter verschiedenen Konten ausgeführt werden. Der Befehl „setspn“ meldet einen Fehler, wenn ein SPN bereits unter einem anderen Konto eingerichtet wurde.

Zum Durchführen dieses Verfahrens ist mindestens die Mitgliedschaft in **Domänenadministratoren** oder eine entsprechende Berechtigung erforderlich.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Einrichten des SPN der BHOLD-Website

1.  Klicken Sie im Active Directory Domain Services-Domänencontroller auf **Start** > **Alle Programme** > **Zubehör**, klicken Sie mit der rechten Maustaste auf **Eingabeaufforderung**, und klicken Sie dann auf **Als Administrator ausführen**.

2.  Geben Sie an der Eingabeaufforderung folgenden Befehl ein, und drücken Sie dann die EINGABETASTE: setspn –S HTTP/ *\<networkalias\> \<domain\>* \\ *\<accountname\>*. Hierbei ist:

    -   *\<Netzwerkalias\>* die Adresse, die Clients verwenden, um eine Verbindung mit der BHOLD-Website herzustellen.

    -   *\<Domäne\>*\\*\<Kontoname\>* der Domänen- bzw. Benutzername des BHOLD Core-Dienstkontos, das Sie bei der Installation von BHOLD Core erstellt haben.

3.  Wiederholen Sie den vorherigen Schritt für alle anderen Namen, die Clients verwenden, um eine Verbindung mit der BHOLD-Website herzustellen, z.B. CNAME-Aliase, Namen, die einen vollqualifizierten Domänennamen enthalten oder Namen, die einen kurzen NetBIOS-Domänennamen enthalten.

#### <a name="setting-bhold-system-attributes"></a>Festlegen von BHOLD-Systemattributen

Öffnen Sie das BHOLD Core-Portal und zeigen Sie die Systemattribute an, um zu überprüfen, dass die Installation des BHOLD Core-Moduls erfolgreich war. Sie können bei Bedarf zusätzlich folgende BHOLD-Systemattribute ändern, um sicherzustellen, dass das BHOLD Core-Modul in Ihrer Umgebung ordnungsgemäß funktioniert.

| **Attribut**                | **Beschreibung**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Legen Sie diesen Wert auf Y fest, wenn die BHOLD-Website auf einem gruppierten Webdienst ausgeführt wird, um sicherzustellen, dass die zuletzt angezeigten Elemente ordnungsgemäß funktionieren. Legen Sie diesen Wert auf N fest, wenn die BHOLD-Website auf einem eigenständigen IIS-Server ausgeführt wird.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Legen Sie diesen Wert auf Y fest, um sicherzustellen, dass Organisationseinheiten nur in Organisationseinheiten mit dem gleichen Organisationstyp wie die übergeordnete Organisationseinheit verschoben werden können. Dadurch wird beispielsweise verhindert, dass eine Organisationseinheit für ein Projekt in eine Organisationseinheit für eine Abteilung verschoben wird. Legen Sie diesen Wert auf N fest, um zuzulassen, dass eine Organisationseinheit in einer Organisationseinheit eines anderen Typs platziert wird. |
| **Tage zwischen der Ausführung von ABA**     | Legen Sie diesen Wert auf eine zweistellige ganze Zahl fest, um das Intervall (in Tagen) zwischen zwei Ausführungen der attributbasierten Autorisierung (ABA) festzulegen. Geben Sie beispielsweise 02 ein, um anzugeben, dass zwischen den Ausführungen von ABA zwei Tage liegen sollen.                                                                                                                     |
| **Startzeit der Ausführung von ABA**    | Legen Sie diesen Wert auf eine zweistellige ganze Zahl fest, um die Stunde des Tages anzugeben, zu der die attributbasierte Autorisierung ausgeführt wird. Um beispielsweise anzugeben, dass die Ausführung von ABA um 23:00 Uhr stattfindet, geben Sie 23 ein.                                                                                                             |
| **Systemkardinalität**       | Legen Sie diesen Wert auf N fest, wenn Sie keine Überprüfung der Systemkardinalität in BHOLD möchten. Der Standardwert ist Y.                                                                                                                                                                                                                             |
| **Protokollierung**                  | Legen Sie diesen Wert auf N fest, wenn Änderungen nicht protokolliert werden sollen. Der Standardwert ist Y.                                                                                                                                                                                                                                            |
| **Verarbeitung der Systemwarteschlange**   | Legen Sie diesen Wert auf N fest, wenn Sie keine Verarbeitung der Systemwarteschlange möchten. Ändern Sie diesen Wert nicht, wenn Sie nicht vom Produktsupport dazu aufgefordert werden.                                                                                                                                                                                           |

Zum Ausführen dieses Verfahrens müssen Sie als Mitglied der Gruppe „Domänenadministratoren“ angemeldet sein.

**Festlegen eines BHOLD-Systemattributs**

1.  Klicken Sie auf **Start** > **Alle Programme**, und klicken Sie dann auf **Internet Explorer**.

2.  Geben Sie http://<server>:<port>/bhold/core in das Adressfeld ein, wobei *\<server\>* der Name des Servers der BHOLD-Website und *\<port\>* die an die Website gebundene Portnummer ist.

3.  Klicken Sie auf **Startseite** > **Werte**, und klicken Sie dann auf **Ändern**.

4.  Suchen Sie den Namen des Attributs, das Sie ändern möchten, geben Sie den Wert in das Feld neben dem Attributnamen ein, und klicken Sie dann auf **OK**.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie BHOLD Core installiert und überprüft haben, dass die Installation erfolgreich war, können Sie zusätzliche Module installieren. An diesem Punkt ist die BHOLD-Datenbank praktisch leer. Sie enthält nur ein Benutzerkonto, das Stammkonto und eine Organisationseinheit, die Stammorganisationseinheit. Zum Hinzufügen weiterer Benutzer zur BHOLD-Datenbank können Sie abhängig von Ihren Anforderungen entweder das Modul des Zugriffsverwaltungsconnectors oder das BHOLD-Modellgeneratormodul installieren. Sie können das Modul des Zugriffsverwaltungsconnectors verwenden, um Benutzerdaten aus dem FIM-Synchronisierungsdienst zu importieren, oder Sie können den BHOLD-Modellgenerator verwenden, um Benutzerdaten aus einem Satz von strukturierten Dateien zu importieren. Weitere Informationen zum Verwenden des Moduls des Zugriffsverwaltungsconnectors finden Sie unter [Testlaboranleitung: BHOLD-Zugriffsverwaltungsconnector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Weitere Informationen zur Verwendung des BHOLD-Modellgeneratormoduls finden Sie unter:

- [Microsoft BHOLD Suite Concepts Guide (Konzepthandbuch für Microsoft BHOLD Suite)](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite Technical Reference (Technische Referenz zu Microsoft BHOLD Suite)](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
