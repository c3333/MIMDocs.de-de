---
title: Microsoft Identity Manager 2016 – Empfohlene Vorgehensweisen | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/05/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 8987bc53af37b32b95b00c3df67d9581d4e47120
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518773"
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Microsoft Identity Manager 2016 – Empfohlene Vorgehensweisen

In diesem Thema werden empfohlene Vorgehensweisen für die Bereitstellung und den Betrieb von Microsoft Identity Manager 2016 (MIM) beschrieben.

## <a name="sql-setup"></a>SQL Setup
> [!NOTE]
> Die folgenden Empfehlungen für das Einrichten eines Servers mit SQL gehen von je einer SQL-Instanz aus, die nur für die FIMService-Datenbank bzw. die FIMSynchronizationService-Datenbank vorgesehen ist. Wenn Sie die FIMService-Datenbank in einer konsolidierten Umgebung ausführen, müssen Sie Anpassungen entsprechend Ihrer Konfiguration vornehmen.

Die Konfiguration des Structured Query Language-Servers (SQL) ist entscheidend für eine optimale Systemleistung. Für das Erreichen der optimalen Leistung von MIM in umfangreichen Implementierungen sind die empfohlenen Vorgehensweisen für einen Server mit SQL von großer Bedeutung. Weitere Informationen finden Sie in folgenden Themen zu empfohlenen Vorgehensweisen für SQL:

-   [Storage Top 10 Best Practices (Top 10 der bewährten Speichermethoden)](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Optimieren der Leistung von „tempdb“](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [SQL Server Best Practices Article (Bewährte Methoden für SQL Server)](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizing and Rebuilding Indexes (Neuorganisieren und Neuerstellen von Indizes)](http://go.microsoft.com/fwlink/?LinkID=188269)

### <a name="presize-data-and-log-files"></a>Voreinstellen der Größe von Daten- und Protokolldateien

Verlassen Sie sich nicht auf die automatische Vergrößerung. Verwalten Sie die Größe dieser Dateien stattdessen manuell. Sie können die automatische Vergrößerung aus Sicherheitsgründen aktiviert lassen, sollten die Vergrößerung der Datendateien jedoch proaktiv verwalten. Beispielgrößen der MIM-Datenbank finden Sie unter [FIM Capacity Planning Guide (Richtlinien zur FIM-Kapazitätsplanung)](http://go.microsoft.com/fwlink/?LinkID=185246).

### <a name="to-presize-sql-data-and-log-files"></a>So stellen Sie die Größe von SQL-Datendateien und -Protokolldateien ein

1.  Starten Sie SQL Server Management Studio.

2.  Navigieren Sie zur FIMService-Datenbank, klicken Sie mit der rechten Maustaste auf „FIMService“ und dann auf „Eigenschaften“.

3.  Vergrößern Sie auf der Seite „Dateien“ die Datenbankdateien wie erforderlich.

### <a name="isolate-log-from-data-files"></a>Trennen von Protokoll- und Datendateien

Befolgen Sie die empfohlenen Vorgehensweisen für SQL Server, um die Daten- und Transaktionsprotokolldateien für die Datenbanken auf separaten physischen Datenträgern zu speichern.

Erstellen zusätzlicher tempdb-Dateien

Für optimale Leistung wird empfohlen, dass Sie eine Datendatei pro CPU-Kern in der tempdb-Datei erstellen.

### <a name="to-create-additional-tempdb-files"></a>So erstellen Sie zusätzliche tempdb-Dateien

1.  Starten Sie SQL Server Management Studio.

2.  Navigieren Sie zur tempdb-Datenbank in Systemdatenbanken, klicken Sie mit der rechten Maustaste auf „tempdb“ und dann auf „Eigenschaften“.

3.  Erstellen Sie auf der Seite „Dateien“ eine Datendatei für jeden CPU-Kern. Achten Sie darauf, dass Sie die tempdb-Datendateien und die Protokolldateien auf verschiedenen Laufwerken und Spindeln speichern.

### <a name="ensure-adequate-space-for-log-files"></a>Genügend Speicherplatz für Protokolldateien sicherstellen

Es ist wichtig, die Datenträgeranforderungen des Wiederherstellungsmodells zu kennen. Der einfache Wiederherstellungsmodus kann beim ersten Laden des Systems geeignet sein, um die Speicherplatznutzung zu beschränken. Allerdings gehen so die Daten, die nach der letzten Sicherung erstellt wurden, verloren. Bei Verwendung des vollständigen Wiederherstellungsmodus müssen Sie die Datenträgerverwendung über Sicherungen verwalten, darunter häufige Sicherungen des Transaktionsprotokolls, um hohe Speicherplatznutzung zu verhindern. Weitere Informationen finden Sie unter [Übersicht über Wiederherstellungsmodelle](http://go.microsoft.com/fwlink/?LinkID=185370).

### <a name="limit-sql-server-memory"></a>Beschränken von SQL Server-Arbeitsspeicher

Je nachdem, über wie viel Arbeitsspeicher Sie auf dem Server mit SQL Server verfügen und ob Sie den Server mit SQL Server für andere Dienste (d.h. MIM 2016-Dienst und MIM 2016-Synchronisierungsdienst) freigeben, sollten Sie den Speicherverbrauch von SQL beschränken. Dies können Sie mit folgenden Schritten erreichen.

1. Starten Sie SQL Server Enterprise Manager.

2. Wählen Sie „Neue Abfrage“ aus.

3. Führen Sie die folgende Abfrage aus:

   ```SQL
   USE master

   EXEC sp_configure 'show advanced options', 1

   RECONFIGURE WITH OVERRIDE

   USE master

   EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
   WITH OVERRIDE
   ```

   In diesem Beispiel wird der SQL-Server so konfiguriert, dass er nicht mehr als 12 GB (Gigabyte) Arbeitsspeicher verwendet.

4. Überprüfen Sie die Einstellung unter Verwendung der folgenden Abfrage:

   ```SQL
   USE master

   EXEC sp_configure 'max server memory (MB)'--- verify the setting

   USE master

   EXEC sp_configure 'show advanced options', 0

   RECONFIGURE WITH OVERRIDE
   ```

### <a name="backup-and-recovery-configuration"></a>Konfigurieren von Sicherung und Wiederherstellung

Grundsätzlich sollten Sie mit Ihrem Datenbankadministrator zusammen am Entwurf einer Sicherungs- und Wiederherstellungsstrategie arbeiten. Einige Empfehlungen dazu lauten:
- Nehmen Sie Datenbanksicherungen gemäß der Sicherungsrichtlinie Ihrer Organisation vor. 
- Wenn keine inkrementellen Protokollsicherungen geplant sind, sollte für die Datenbank das einfache Wiederherstellungsmodell festgelegt werden. 
- Stellen Sie vor der Implementierung Ihrer Sicherungsstrategie sicher, dass Sie die Auswirkungen der unterschiedlichen Wiederherstellungsmodelle kennen. Informieren Sie sich über die Speicherplatzanforderungen für diese Modelle. Das vollständige Wiederherstellungsmodell erfordert regelmäßige Protokollsicherungen, um hohe Speicherplatznutzung zu vermeiden. 

Weitere Informationen finden Sie unter [Übersicht über Wiederherstellungsmodelle](http://go.microsoft.com/fwlink/?LinkID=185370) und [FIM 2010 Backup and Restore Guide (FIM 2010-Handbuch zur Sicherung und Wiederherstellung)](http://go.microsoft.com/fwlink/?LinkID=165864).

## <a name="create-a-backup-administrator-account-for-the-fim-service-after-installation"></a>Erstellen eines Sicherungsadministratorkontos für FIMService nach der Installation

Mitglieder der Gruppe von FIMService-Administratoren verfügen über eindeutige Berechtigungen, die für die MIM-Bereitstellung entscheidend sind. Wenn Sie sich als Mitglied der Administratorgruppe nicht anmelden können, ist die einzige Lösung das Zurücksetzen auf eine vorherige Sicherung des Systems. Zur Verbesserung dieser Situation sollten Sie der Gruppe von FIM-Administratoren als Teil der Konfiguration nach der Installation andere Benutzer hinzufügen.

## <a name="fim-service"></a>FIM-Dienst


### <a name="configuring-fim-service-service-exchange-mailbox"></a>Konfigurieren des Exchange-Postfachs des FIM-Diensts

Im Folgenden werden empfohlene Vorgehensweisen für die Konfiguration von Microsoft Exchange Server für das Dienstkonto des MIM 2016-Diensts beschrieben.

- Konfigurieren Sie das Dienstkonto so, dass es nur E-Mails von internen Adressen akzeptiert. Insbesondere sollte das Postfach des Dienstkontos nie E-Mails von externen SMTP-Servern empfangen können.

#### <a name="to-configure-the-service-account"></a>So konfigurieren Sie das Dienstkonto

1.  Wählen Sie in der Exchange-Verwaltungskonsole das **Dienstkonto des FIM-Diensts** aus.

2.  Wählen Sie „Eigenschaften“, „Nachrichtenflusseinstellungen“ und dann **Nachrichtenübermittlungseinschränkungen** aus.

3.  Aktivieren Sie das Kontrollkästchen **Authentifizierung aller Absender anfordern**.

Weitere Informationen finden Sie unter [Konfigurieren von Einschränkungen für die Nachrichtenübermittlung](http://go.microsoft.com/fwlink/?LinkID=183625).

-   Konfigurieren Sie das Dienstkonto so, dass es E-Mails mit mehr als 1 MB ablehnt. Befolgen Sie für ein Postfach oder einen E-Mail-aktivierten öffentlichen Ordner die empfohlenen Vorgehensweisen zum Thema [Configure Message Size Limits (Konfigurieren von Nachrichtengrößenbeschränkungen)](http://go.microsoft.com/fwlink/?LinkID=183626).

-   Konfigurieren Sie das Dienstkonto so, dass es über ein Speicherplatzkontingent von 5 GB für das Postfach verfügt. Befolgen Sie für optimale Ergebnisse die empfohlenen Vorgehensweisen unter [Configure Storage Quotas for a Mailbox (Konfigurieren von Speicherkontingenten für ein Postfach)](http://go.microsoft.com/fwlink/?LinkID=156929).

## <a name="mim-portal"></a>MIM-Portal


### <a name="disable-sharepoint-indexing"></a>Deaktivieren der SharePoint-Indizierung

Es wird empfohlen, dass Sie die Indizierung von Microsoft Office SharePoint® deaktivieren. Es sind keine Dokumente vorhanden, die indiziert werden müssen. Indizierung verursacht viele Einträge im Fehlerprotokoll und mögliche Leistungsprobleme in MIM. Führen Sie die Schritte unten aus, um die SharePoint-Indizierung zu deaktivieren:

1.  Klicken Sie auf dem Server, der das MIM 2016-Portal hostet, auf „Start“.

2.  Klicken Sie auf „Alle Programme“.

3.  Klicken Sie in der Liste „Alle Programme“ auf „Verwaltung“.

4.  Klicken Sie unter „Verwaltung“ auf „SharePoint-Zentraladministration“.

5.  Klicken Sie auf der Seite „Zentraladministration“ auf „Vorgänge“.

6.  Klicken Sie auf der Seite „Vorgänge“ unter „Globale Konfiguration“ auf „Zeitgeberauftragsdefinitionen“.

7.  Klicken Sie auf der Seite „Zeitgeberauftragsdefinitionen“ auf „Aktualisierung der SharePoint Services-Suche“.

8.  Klicken Sie auf der Seite „Zeitgeberauftrag bearbeiten“ auf „Deaktivieren“.

## <a name="mim-2016-initial-data-load"></a>MIM 2016 – Erster Datenladevorgang

Dieser Abschnitt enthält eine Reihe von Schritten, um die Leistung des ersten Datenladevorgangs aus einem externen System zu MIM zu erhöhen. Beachten Sie, dass einige dieser Schritte nur bei der ersten Auffüllung des Systems ausgeführt werden. Sie sollten nach dem Abschluss des Ladens zurückgesetzt werden. Dies ist ein einmaliger Vorgang und keine fortlaufende Synchronisierung.

> [!NOTE]
> Weitere Informationen zum Synchronisieren von Benutzern zwischen MIM und Active Directory-Domänendiensten (AD DS) finden Sie in der FIM-Dokumentation unter [How do I Synchronize Users from Active Directory to FIM (Synchronisieren von Benutzern aus Active Directory zu FIM)](http://go.microsoft.com/fwlink/?LinkID=188277).
> 
> [!IMPORTANT]
> Stellen Sie sicher, dass Sie die empfohlenen Vorgehensweisen aus dem Abschnitt zur Installation von SQL in dieser Anleitung angewendet haben. 

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>Schritt 1: Konfigurieren Sie SQL Server für den ersten Datenladevorgang.
Das erste Laden der Daten kann ein lang andauernder Vorgang sein. Wenn Sie am Anfang große Datenmengen laden möchten, können Sie die Zeit für das Auffüllen der Datenbank verkürzen, indem Sie die Volltextsuche vorübergehend deaktivieren und sie nach dem Abschluss des Exports auf dem MIM 2016-Verwaltungs-Agent (FIM-MA) wieder aktivieren.

So deaktivieren Sie die Volltextsuche vorübergehend

1.  Starten Sie SQL Server Management Studio.

2.  Wählen Sie „Neue Abfrage“ aus.

3.  Führen Sie die folgenden SQL-Anweisungen aus:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

> [!IMPORTANT]
> Wenn diese Vorgehensweisen nicht umgesetzt werden, kann dies dazu führen, dass nicht mehr genügend Speicherplatz vorhanden ist. Weitere Details zu diesem Thema finden Sie unter [Übersicht über Wiederherstellungsmodelle](http://go.microsoft.com/fwlink/?LinkID=185370). Weitere Informationen finden Sie unter [FIM 2010 Backup and Restore Guide (FIM 2010-Handbuch zur Sicherung und Wiederherstellung)](http://go.microsoft.com/fwlink/?LinkID=165864).

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>Schritt 2: Wenden Sie während des Ladevorgangs die erforderliche MIM-Minimalkonfiguration an.

Beim ersten Laden sollten Sie nur die Mindestkonfiguration anwenden, die erforderlich ist, um FIM für die Management-Richtlinienregeln (Management Policy Rules, MPRs) und Set-Definitionen zu konfigurieren. Erstellen Sie nach Abschluss des Datenladevorgangs die zusätzlichen Sets, die für die Bereitstellung erforderlich sind. Verwenden Sie die Einstellung zum Aktualisieren der Run-On-Richtlinie auf den Aktionsworkflows, um diese Richtlinien nachträglich auf die geladenen Daten anzuwenden.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>Schritt 3: Konfigurieren Sie den FIM-Dienst, und füllen Sie diesen mit externen Identitätsdaten auf.

An diesem Punkt sollten Sie die Richtlinien unter „How Do I Synchronize Users from Active Directory Domain Services to FIM (Synchronisieren von Benutzern aus Active Directory-Domänendiensten zu FIM)“ befolgen, um Ihr System mit Benutzern aus Active Directory zu konfigurieren und zu synchronisieren. Wenn Gruppeninformationen synchronisiert werden sollen, finden Sie dazu Informationen im Leitfaden [How Do I Synchronize Groups from Active Directory Domain Services to FIM (Synchronisieren von Gruppen aus Active Directory-Domänendiensten zu FIM)](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx).

#### <a name="synchronization-and-export-sequences"></a>Synchronisierung und Exportsequenzen

Führen Sie zum Optimieren der Leistung nach einem Synchronisierungsvorgang, der zu einer hohen Anzahl ausstehender Exportvorgänge in einem Connectorbereich führt, einen Export durch. Führen Sie dann auf dem Verwaltungs-Agent, der dem betroffenen Connectorbereich zugeordnet ist, einen bestätigenden Importvorgang durch. Wenn Sie zum Beispiel Synchronisierungsausführungsprofile auf mehreren Verwaltungs-Agents als Teil eines ersten Datenladevorgangs ausführen müssen, sollten sie nach jeder einzelnen Synchronisierung einen Export gefolgt von einem Deltaimport durchführen.
Führen Sie für jeden Quellverwaltungs-Agent, der Teil des Initialisierungszyklus ist, die folgenden Schritte aus:

1.  Vollständiger Import auf einem Quellverwaltungs-Agent.

2.  Vollständige Synchronisierung auf dem Quellverwaltungs-Agent.

3.  Führen Sie auf allen betroffenen Zielverwaltungs-Agents mit Stagingexport einen Export durch.

4.  Führen Sie auf allen betroffenen Zielverwaltungs-Agents mit Stagingexport einen Deltaimport durch.

### <a name="step-4-apply-your-full-mim-configuration"></a>Schritt 4: Wenden Sie die vollständige MIM-Konfiguration an.

Wenden Sie nach Abschluss des ersten Datenladevorgangs die vollständige MIM-Konfiguration für die Bereitstellung an.

Je nach Szenarios kann dies die Erstellung weiterer Sets, MPRs und Workflows umfassen. Verwenden Sie für alle Richtlinien, die Sie rückwirkend auf alle Objekte im System anwenden müssen, die Einstellung für die Ausführung für Richtlinienaktualisierung auf Aktionsworkflows, um diese Richtlinien rückwirkend auf die geladenen Daten anzuwenden.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>Schritt 5: Setzen Sie SQL auf die vorherigen Einstellungen zurück.

Denken Sie daran, die SQL-Einstellungen in die normalen Einstellungen zu ändern. Dies umfasst u. a.:

-   Aktivieren der Volltextsuche

-   Aktualisieren der Sicherungsrichtlinie nach der Organisationsrichtlinie

Wenn Sie den ersten Datenladevorgang abgeschlossen haben, müssen Sie die Volltextsuche wieder aktivieren. Führen Sie die folgenden SQL-Anweisungen aus, um die Volltextsuche wieder zu aktivieren:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Wenn Sie in den einfachen Wiederherstellungsmodus wechseln müssen, sollten Sie sicherstellen, dass Sie den Sicherungszeitplan gemäß der Sicherungsrichtlinie Ihrer Organisation neu konfigurieren. Zusätzliche Details zu FIM-Sicherungszeitplänen finden Sie im [FIM 2010 Backup and Restore Guide (FIM 2010-Handbuch zur Sicherung und Wiederherstellung)](http://go.microsoft.com/fwlink/?LinkID=165864).

## <a name="configuration-migration"></a>Migration der Konfiguration


### <a name="avoid-changing-display-names"></a>Vermeiden von Änderungen von Anzeigenamen

Für viele Objekttypen wie zum Beispiel MPRs verwendet das Skript „syncproduction.ps1“ den Anzeigenamen als einziges Ankerattribut zwischen zwei Systemen. Daher führt eine Änderung am Anzeigenamen einer vorhandenen MPR zur Löschung der vorhandenen MPR und der anschließenden Erstellung einer neuen MPR. Dies geschieht, da bei der Migration keine MPRs zusammengeführt werden können, deren Verknüpfungskriterien sich geändert haben. Um dieses Problem zu vermeiden, können Sie ein benutzerdefiniertes Attribut an alle Objekttypen für die Konfiguration binden und dieses Attribut als Verknüpfungskriterium verwenden. Dadurch können Sie Anzeigenamen ändern, ohne den Migrationsvorgang zu beeinträchtigen.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>Vermeiden von Änderungen des Inhalts von temporären Dateien

Während das Dateiformat und die Anwendungsprogrammierschnittstelle (API, Application Programming Interface) der Objekte niedriger Ebene öffentlich sind und von Entwicklern geändert werden können, sollten Sie die Inhalte der Zwischenformate während der Migration nicht ändern. Allerdings kann es erforderlich sein, ganze ImportObjects-Knoten aus „changes.xml“ zu entfernen oder mithilfe von Suchen und Ersetzen in „pilot.xml“ Versionsnummern oder DNS-Informationen von Pilotsystemen durch die von Produktionssystemen zu ersetzen.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>Überprüfen der Versionsnummer in „pilot.xml“ bei der Migration zwischen Versionen

Die Migration zwischen Versionsnummern wird zwar weder empfohlen noch unterstützt, Sie können dies aber häufig erreichen, indem Sie in „pilot.xml“ die Pilotversionsnummer durch die Produktionsversionsnummer ersetzen. Insbesondere die Objekte „WorkflowDefinition“ und

„ActivityInformationConfiguration“ erfordern, dass die Versionsnummer genau auf die Workflowaktivitäten in der Produktionsumgebung verweist. Wenn die Versionsnummer nicht ersetzt wird, identifiziert das Cmdlet „Compare-FIMConfig“ Unterschiede zwischen den Attributen der Extensible Object Markup Language (XOML) auf „WorkflowDefinition“-Objekten und migriert die Pilotversionsnummer. Der FIM-Dienst der Produktionsversion kann Workflowaktivitäten mit der falschen Versionsnummer möglicherweise nicht starten.

### <a name="avoid-cyclic-references"></a>Vermeiden zyklischer Verweise

Im Allgemeinen werden zyklische Verweise in einer MIM-Konfiguration nicht empfohlen. Zyklen treten jedoch manchmal auf, wenn Set A auf Set B verweist und Set B zudem auf Set A verweist. Um Probleme mit zyklischen Verweisen zu vermeiden, sollten Sie die Definition von Set A oder Set B so ändern, sodass nicht beide aufeinander verweisen. Starten Sie den Migrationsvorgang anschließend neu. Wenn zyklische Verweise vorhanden sind und das Cmdlet „Compare-FIMConfig“ deshalb zu einem Fehler führt, muss der Zyklus manuell unterbrochen werden. Da das Cmdlet „Compare-FIMConfig“ eine Liste der Änderungen nach Rangfolge ausgibt, dürfen unter den Verweisen von Konfigurationsobjekten keine Zyklen vorhanden sind.

## <a name="security"></a>Sicherheit

### <a name="mim-ma-account"></a>MIM-MA-Konto

Das MIM-MA-Konto wird nicht als Dienstkonto angesehen und sollte ein normales Benutzerkonto sein. Die Konten müssen zur lokalen Anmeldung fähig sein, damit das Dienstkonto des FIM-Synchronisierungsdiensts es annehmen kann.

So aktivieren Sie das MIM-MA-Konto für die lokale Anmeldung

1.  Klicken Sie auf „Start“, „Verwaltung“ und dann auf „Lokale Sicherheitsrichtlinie“.

2.  Öffnen Sie den Knoten „Lokale Richtlinien“, und klicken Sie dann auf „Zuweisen von Benutzerrechten“.

3.  Stellen Sie sicher, dass das FIM-MA-Konto in der Richtlinie „Lokale Anmeldung zulassen“ explizit angegeben wird, oder fügen Sie es einer der Gruppen hinzu, denen bereits Zugriff gewährt wurde.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>FIM-Synchronisierungsdienst und Konten von FIM-Diensten

Um die Server, auf denen die MIM-Serverkomponenten ausgeführt werden, auf sichere Weise zu konfigurieren, sollte die Dienstkonten beschränkt werden. Legen Sie mithilfe der vorherigen Vorgehensweise zur Aktivierung des MIM-MA-Kontos die folgenden Einschränkungen für den FIM-Synchronisierungsdienst und Konten von FIM-Diensten fest:

-   Anmelden als Batchauftrag verweigern

-   Lokal anmelden verweigern

-   Zugriff vom Netzwerk auf diesen Computer verweigern

Die Dienstkonten sollten kein Mitglied der lokalen Administratorgruppe sein.

Das Dienstkonto des FIM-Synchronisierungsdiensts sollte kein Mitglied der Sicherheitsgruppen sein, mit denen der Zugriff auf den FIM-Synchronisierungsdienst kontrolliert wird (diese Gruppen beginnen mit „FIMSync“ wie z.B. „FIMSyncAdmins“ usw.).

> [!IMPORTANT]
>  Wenn Sie dasselbe Konto für beide Dienstkonten verwenden und den FIM-Dienst und den FIM-Synchronisierungsdienst trennen, können Sie für diesen Computer nicht den Zugriff vom Netzwerk auf dem MMS-Synchronisierungsdienst-Server aus verweigern. Wenn der Zugriff verweigert wird, kann der FIM-Dienst keine Verbindung zum FIM-Synchronisierungsdienst herstellen, um die Konfiguration zu ändern und Kennwörter zu verwalten.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>Bei Kennwortzurücksetzungen an Kiosk-Computern Auslagerungsdatei des virtuellen Arbeitsspeichers löschen

Wenn die FIM-Kennwortzurücksetzung auf einer Arbeitsstation bereitgestellt wird, die als Kiosk verwendet werden soll, empfehlen wir, die Einstellung „Shutdown: Clear virtual memory pagefile local security policy“ (Herunterfahren: lokale Sicherheitsrichtlinie für Auslagerungsdatei des virtuellen Speichers löschen) zu aktivieren. So wird vermieden, dass vertrauliche Informationen aus dem Prozessspeicher nicht autorisierten Benutzern zur Verfügung gestellt werden.

### <a name="implementing-ssl-for-the-fim-portal"></a>Implementieren von SSL für das FIM-Portal

Es wird dringend empfohlen, dass Sie auf dem FIM-Portalserver Secure Sockets Layer (SSL) verwenden, um den Datenverkehr zwischen Clients und Server zu sichern.

So implementieren Sie SSL

1.  Öffnen Sie den IIS-Manager auf dem MIM-Portalserver.

2.  Klicken Sie auf den Namen des lokalen Computers.

3.  Klicken Sie auf „Serverzertifikate“.

4.  Klicken Sie auf „Zertifikatanforderung erstellen“.

5.  Geben Sie im Textfeld „Allgemeiner Name“ den Namen des Servers ein.

6.  Klicken Sie zweimal auf „Weiter“.

7.  Speichern Sie die Datei an einem beliebigen Ort. Sie müssen in den nachfolgenden Schritten auf diesen Speicherort zugreifen.

8.  Navigieren Sie zu https://servername/certsrv. Ersetzen Sie „servername“ durch den Namen des Servers, der Zertifikate ausstellt.

9.  Klicken Sie auf „Neues Zertifikat anfordern“.

10. Klicken Sie auf „Erweiterte Anforderung absenden“.

11. Klicken Sie auf „Zertifikatanforderung mit einer Base64-codierten PKCS7-Datei absenden“.

12. Fügen Sie den Inhalt der Datei ein, die Sie im vorherigen Schritt gespeichert haben.

13. Wählen Sie unter „Zertifikatvorlage“ „Webserver“ aus.

14. Klicken Sie auf Absenden.

15. Speichern Sie das Zertifikat auf Ihrem Desktop.

16. Klicken Sie im IIS-Manager auf „Zertifikatanforderung abschließen“.

17. Weisen Sie den IIS-Manager auf das Zertifikat hin, das Sie soeben auf dem Desktop gespeichert haben.

18. Geben Sie als Anzeigename den Namen des Servers ein.

19. Klicken Sie auf „Standorte“, und wählen Sie dann „SharePoint – 80“ aus.

20. Klicken Sie auf „Bindungen“ und dann auf „Hinzufügen“.

21. Wählen Sie „https“ aus.

22. Wählen Sie als Zertifikat das mit dem Namen des Servers aus (das Zertifikat, das Sie soeben importiert haben).

23. Klicken Sie auf OK.

24. Entfernen Sie die HTTP-Bindung.

25. Klicken Sie auf „SSL-Einstellungen“, und aktivieren Sie dann „SSL erforderlich“.

26. Speichern Sie die Einstellungen.

27. Klicken Sie auf „Start“, auf „Verwaltung“ und dann auf „SharePoint 3.0-Zentraladministration“.

28. Klicken Sie auf „Vorgänge“ und dann auf „Alternative Zugriffszuordnungen“.

29. Klicken Sie auf http://servername.

30. Ändern Sie http://servername in https://servername, und klicken Sie dann auf „OK“.

31. Klicken Sie auf „Start“ und dann auf „Ausführen“. Geben Sie „iisreset“ ein, und klicken Sie dann auf „OK“.

## <a name="performance"></a>Leistung

So erreichen Sie eine optimale Leistungskonfiguration:

-   Wenden Sie die empfohlenen Vorgehensweisen für SQL Setup wie im Abschnitt „SQL Setup“ dieses Artikels beschrieben an.

-   Deaktivieren Sie die SharePoint-Indizierung auf der MIM-Portalwebsite. Weitere Informationen finden Sie im Abschnitt „Deaktivieren der SharePoint-Indizierung“ in diesem Artikel.

## <a name="feature-specific-best-practices"></a>Featurespezifische bewährte Methoden 


### <a name="request-management"></a>Anforderungsverwaltung

Standardmäßig löscht MIM 2016 abgelaufene Systemobjekte einschließlich abgeschlossene Anforderungen mit zugehörigen Genehmigungen, Genehmigungsantworten und Workflowinstanzen im Abstand von 30 Tagen. Wenn Ihre Organisation einen längeren Anforderungsverlauf benötigt, sollten Sie Anfragen aus MIM exportieren und in einer zusätzlichen Datenbank speichern, um sie über dieses 30-Tage-Fenster hinaus zu erhalten. Das 30-Tage-Fenster für die Löschung von Anforderungen kann zwar konfiguriert werden, aber eine Erweiterung dieses Fensters kann durch die zusätzlichen Objekte im System die Leistung beeinträchtigen.

### <a name="management-policy-rules"></a>Verwaltungsrichtlinienregeln

#### <a name="use-the-appropriate-mpr-type"></a>Verwenden der passenden MPR-Art

MIM bietet zwei Arten von MPRs, eine für Anforderungen und eine für den Listenübergang:

- MPR für Anforderungen (RMPR, Request MPR)

  - Wird verwendet, um die Zugriffssteuerungsrichtlinie (Authentifizierung, Autorisierung und Aktion) für das Erstellen, Lesen, Aktualisieren oder Löschen (CRUD, Create, Read, Update, oder Delete) von Ressourcen zu definieren.
  - Wird angewendet, wenn ein CRUD-Vorgang für eine Zielressource in MIM ausgegeben wird.
  - Wird begrenzt durch die Auswahlkriterien, die in der Regel definiert sind und angeben, für welche CRUD-Anforderungen die Regel gilt.

- MPR für den Listenübergang (TMPR, Set Transition MPR)
  - Wird verwendet, um Richtlinien zu definieren, und zwar unabhängig davon, wie das Objekt in den aktuellen Status gelangt ist, der durch die Übergangsliste dargestellt wird. Sollte zum Modellieren von Berechtigungsrichtlinien verwendet werden.
  - Wird angewendet, wenn eine Ressource ein zugeordnetes Set betritt oder verlässt.
  - Ist begrenzt auf die Mitglieder des Sets.

>[HINWEIS] Weitere Informationen finden Sie unter [Designing Business Policy Rules (Entwerfen von Regeln für die Geschäftsrichtlinie)](http://go.microsoft.com/fwlink/?LinkID=183691).

#### <a name="only-enable-mprs-as-necessary"></a>Aktivieren von MPRs nur nach Bedarf

Verwenden Sie das Prinzip der geringsten Rechte, wenn Sie Ihre Konfiguration anwenden. MPRs kontrollieren die Zugriffsrichtlinie für die MIM-Bereitstellung. Aktivieren Sie nur die Funktionen, die von den meisten Benutzern verwendet werden. Nicht alle Benutzer verwenden zum Beispiel MIM für die Verwaltung von Gruppen, weshalb die entsprechenden MPRs für die Gruppenverwaltung deaktiviert werden sollten. Standardmäßig sind in MIM die meisten Nicht-Administratorberechtigungen deaktiviert.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>Duplizieren integrierter MPRs anstatt direkter Änderung
Wenn Sie die integrierten MPRs ändern müssen, sollten Sie eine neue MPR mit der erforderlichen Konfiguration erstellen und die integrierte MPR deaktivieren. Dadurch wird sichergestellt, dass zukünftige Änderungen an den integrierten MPRs im Rahmen des Upgradevorgangs nicht die Systemkonfiguration beeinträchtigen.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>Endbenutzerberechtigungen mit expliziten Attributlisten
Mit expliziten Attributlisten kann verhindert werden, dass versehentlich Berechtigungen an nicht berechtigte Benutzer erteilt werden, wenn Attribute zu Objekten hinzugefügt werden. Administratoren sollten explizit Zugriff auf neue Attribute gewähren müssen, anstatt zu versuchen, den Zugriff aufzuheben.

Der Datenzugriff sollte entsprechend den geschäftlichen Anforderungen der Benutzer begrenzt werden. Zum Beispiel sollten Mitglieder einer Gruppe keinen Zugriff auf das Filterattribut der Gruppe haben, in der sie Mitglied sind. Der Filter kann versehentlich Unternehmensdaten enthüllen, auf die der Benutzer normalerweise keinen Zugriff hätte.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>MPRs sollten effektiven Berechtigungen entsprechen
Vermeiden Sie es, Berechtigungen für Attribute zu erteilen, die der Benutzer nie verwenden kann. Sie sollten zum Beispiel keine Berechtigung zum Ändern von Kernressourcenattributen wie „objectType“ erteilen. Trotz der MPR wird vom System jeder Versuch verweigert, den Typ einer Ressource nach der Erstellung zu ändern.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>Trennung von Leseberechtigungen und Berechtigungen zum Ändern und Erstellen

Wenn Attribute in MPRs explizit aufgelistet werden, sind für das Erstellen und Ändern in der Regel andere Attribute als für den Lesezugriff erforderlich. Zum Beispiel kann der Lesezugriff über Systemattribute wie „Creator“ oder „objectId“ gewährt werden, während das Erstellen oder Ändern für Systemattribute nicht angegeben werden kann.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>Trennung von Berechtigungen zum Erstellen und Ändern

Für den Erstellungsvorgang muss der Benutzer „objectType“ als Teil des Vorgangs auswählen. Dies ist ein Kernsystemattribut, das nach einem Erstellungsvorgang nicht mehr geändert werden kann.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>Verwendung einer MPR für Anforderungen für alle Attribute mit gleichen erforderlichen Zugriffsberechtigungen

Für alle Attribute mit den gleichen erforderlichen Zugriffsberechtigungen, die sich voraussichtlich nicht ändern werden, können Sie aus Gründen der Effizienz eine einzige MPR für Anforderungen verwenden.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>Vermeiden von uneingeschränktem Zugriff selbst für ausgewählte Prinzipalgruppen

In MIM werden Berechtigungen als positive Assertion definiert. MIM unterstützt das Verweigern von Berechtigungen nicht. Wenn uneingeschränkter Zugriff auf eine Ressource gewährt wird, können deshalb nur sehr schwer Ausnahmen in den Berechtigungen gemacht werden. Deshalb wird empfohlen, nur die erforderlichen Berechtigungen zu gewähren.

#### <a name="use-tmprs-to-define-custom-entitlements"></a>Verwenden von TMPRs zum Definieren benutzerdefinierter Berechtigungen

Verwenden Sie MPRs für den Listenübergang (TMPRs) anstatt RMPRs, um benutzerdefinierte Berechtigungen zu definieren. TMPRs bieten ein zustandsbasiertes Modell zum Zuweisen oder Entfernen von Berechtigungen, die auf der Mitgliedschaft in den definierten Übergangslisten oder -rollen sowie den zugehörigen Workflowaktivitäten basieren. TMPRs sollten immer paarweise definiert werden, eine für jede Richtung des Ressourcenübergangs. Darüber hinaus sollte jede MPR für den Übergang getrennte Workflows für die Bereitstellung und Aufhebung von Aktivitäten enthalten.

> [!NOTE]
> Jeder Aufhebungsworkflow sollte sicherstellen, dass das Attribut „Für Richtlinienaktualisierung ausführen“ auf „TRUE“ festgelegt ist.

#### <a name="enable-the-set-transition-in-mpr-last"></a>TMPR für den Eingang zuletzt aktivieren

Wenn Sie ein TMPR-Paar erstellen, sollten Sie die TMPR für den Eingang zuletzt aktivieren. Durch diese Reihenfolge wird sichergestellt, dass keine Ressource mit der Berechtigung zurückbleibt, wenn sie dem Set hinzugefügt und aus dem Set entfernt wird, während die TMPR für den Eingang bereits aktiviert, die TMPR für den Ausgang aber noch deaktiviert ist.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>Workflows in der TMPR sollten Zielressource zuerst überprüfen

Bereitstellungsworkflows sollten zunächst überprüfen, ob bereits eine Bereitstellung an die Zielressource gemäß der Berechtigung erfolgt ist. Wenn dies der Fall ist, sollte der Workflow nichts unternehmen.

Aufhebungsworkflows sollten zunächst überprüfen, ob eine Bereitstellung an die Zielressource erfolgt ist. Wenn dies der Fall ist, sollte der Workflow die Bereitstellung an die Zielressource aufheben. Andernfalls sollte der Workflow nichts unternehmen.

#### <a name="select-run-on-policy-update-for-tmprs"></a>Auswählen von „Für Richtlinienaktualisierung ausführen“ für TMPRs

Dadurch wird sichergestellt, dass das richtige Bereitstellungsverhalten angewendet wird, wenn Aktualisierungen implementiert werden, und das Flag „Für Richtlinienaktualisierung ausführen“ auf den TMPRs zugeordneten Aktionsworkflows verwendet wird. Dadurch wird sichergestellt, dass durch Änderungen an den Richtliniendefinitionen die Aktionsworkflows auf neue Mitglieder der Übergangsliste angewendet werden.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>Vermeiden derselben Berechtigung für zwei verschiedene Übergangslisten

Wenn dieselbe Berechtigung zwei verschiedenen Übergangslisten zugeordnet wird, kann dies dazu führen, dass Berechtigungen unnötigerweise aufgehoben und erneut erteilt werden, wenn die Ressource von einem Set zu einem anderen verschoben wird. Es wird empfohlen sicherzustellen, dass in einem Set alle Ressourcen enthalten sind, die die zugeordnete Berechtigung erfordern. Dadurch wird eine 1: 1-Beziehung zwischen der Übergangsliste und der Berechtigung durch den Workflow sichergestellt.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>Richtige Reihenfolge beim Entfernen von Berechtigungen

Die Reihenfolge der Schritte beim Entfernen von Berechtigungen im System kann zu zwei verschiedenen Ergebnissen führen. Vergewissern Sie sich, welche Reihenfolge Sie für das gewünschte Ergebnis benötigen.

So entfernen Sie eine Berechtigung aus dem System (und heben sie für alle derzeit berechtigten Mitglieder auf)

1.  Deaktivieren Sie die TMPR für den Eingang. So werden keine weiteren Berechtigungen erteilt.

2.  Löschen Sie den Filter der Übergangsliste, oder ändern Sie ihn so, dass sie leer ist. Dadurch verlassen alle vorhandenen Mitglieder das Set, und die entsprechende Richtlinie wird angewendet, einschließlich des konfigurierten Aufhebungsworkflows, der der Berechtigung zugeordnet ist.

3.  Deaktivieren Sie die TMPR für den Ausgang.

So entfernen Sie eine Berechtigung ohne Auswirkung auf die derzeitigen Mitglieder (zum Beispiel MIM nicht mehr für die Verwaltung der Berechtigung verwenden):

1.  Deaktivieren Sie die TMPR für den Eingang. So werden keine weiteren Berechtigungen erteilt.

2.  Deaktivieren Sie die TMPR für den Ausgang.

3.  Löschen Sie den Filter der Übergangsliste, oder ändern Sie ihn so, dass sie leer ist. Da das Set nicht mehr an eine TMPR gebunden ist, werden keine Aufhebungsworkflows angewendet.

### <a name="sets"></a>Festlegungen

Wenn Sie die empfohlenen Vorgehensweisen für Sets anwenden, sollten Sie die Auswirkung der Optimierungen auf die zukünftige Verwaltbarkeit berücksichtigen. Bevor diese Empfehlungen angewendet werden, sollten entsprechende Tests für die erwartete Produktion durchgeführt werden, um das richtige Gleichgewicht zwischen Leistung und Verwaltbarkeit zu finden.

>[!NOTE]
> Alle folgenden Richtlinien gelten für dynamische Sets und dynamische Gruppen.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>Minimieren dynamischer Schachtelung

Dies bezieht sich auf den Filter eines Sets, das auf das Attribut „ComputedMember“ eines anderen Sets verweist. Häufig werden Sets geschachtelt, um das Duplizieren einer Mitgliedschaftsbedingung für mehrere Sets zu vermeiden. Dieser Ansatz kann zwar eine bessere Verwaltung der Sets ermöglichen, beeinträchtigt aber auch die Leistung. Sie können die Leistung optimieren, indem Sie die Mitgliedschaftsbedingungen eines geschachtelten Sets duplizieren, anstatt das Set selbst zu verschachteln.

Es kann jedoch vorkommen, dass Sie Sets verschachteln müssen, um eine funktionale Anforderung zu erfüllen. Dies sind die vorrangigen Situationen, in denen Sie Sets schachteln sollten. Wenn Sie zum Beispiel das Set aller Gruppen ohne Vollzeitmitarbeiter definieren, muss die Schachtelung der Sets wie folgt verwendet werden: `/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]`, wobei „X“ die ObjectID des Sets aller Vollzeitmitarbeiter ist.

#### <a name="minimize-the-use-of-negative-conditions"></a>Minimieren negativer Bedingungen

Negative Bedingungen sind die Mitgliedschaftsbedingungen, die die folgenden Operatoren oder Funktionen verwenden: `!=`, `not()`, `\<` ,`\<=`. Um die Leistung zu optimieren, können Sie wenn möglich die gewünschte Bedingung mit mehreren positiven Bedingungen anstatt als negative Bedingung ausdrücken.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>Minimieren von Mitgliedschaftsbedingungen, die auf mehrwertigen Verweisattributen basieren

Es sollten möglichst wenige Bedingungen verwendet werden, die auf mehrwertigen Verweisattributen basieren, da große Mengen solcher Sets die Leistung von Vorgängen auf den Attributen, die in der Mitgliedschaftsbedingung verwendet werden, beeinträchtigen können.

### <a name="password-reset"></a>Kennwortzurücksetzung

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>Bei Kennwortzurücksetzungen an Kiosk-Computern Auslagerungsdatei des virtuellen Arbeitsspeichers löschen

Wenn die MIM-Kennwortzurücksetzung auf einer Arbeitsstation bereitgestellt wird, die als Kiosk verwendet werden soll, empfehlen wir, die Einstellung „Shutdown: Clear virtual memory pagefile local security policy“ (Herunterfahren: lokale Sicherheitsrichtlinie für Auslagerungsdatei des virtuellen Speichers löschen) zu aktivieren. So wird vermieden, dass vertrauliche Informationen aus dem Prozessspeicher nicht autorisierten Benutzern zur Verfügung gestellt werden.

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>Registrierung für Kennwortzurücksetzung auf Anmeldecomputer

Wenn ein Benutzer versucht, sich über ein Webportal für eine Kennwortzurücksetzung zu registrieren, initiiert MIM immer eine Registrierung im Namen des angemeldeten Benutzers, unabhängig davon, wer auf der Website angemeldet ist. Benutzer sollten sich immer für eine Kennwortzurücksetzung registrieren, wenn sie an einem Computer angemeldet sind.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>AvoidPdcOnWan-Registrierungsschlüssel nicht auf „TRUE“ festlegen

Wenn Sie die MIM 2016-Kennwortzurücksetzung verwenden, sollten Sie den Registrierungsschlüssel „AvoidPdcOnWan“ nicht auf „TRUE“ festlegen.

Wenn dieser Registrierungsschlüssel auf „TRUE“ festgelegt ist, gelangt der Benutzer sehr wahrscheinlich durch das Kennwort-Gate, lässt sein Kennwort auf dem primären Domänencontroller (PDC, Primary Domain Controller) ändern und versucht, sich anzumelden. Aufgrund dieses Registrierungsschlüssels führt der lokale Domänencontroller die sekundäre Validierung mit dem PDC nicht durch, weshalb die Anmeldung verweigert wird. Wenn dem Benutzer die Anmeldung zu oft verweigert wird, wird er für die Domäne gesperrt und muss sich an den Support wenden.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>Kennwörter nicht im Klartext eingeben

Es ist möglich, Klartextkennwörter einzugeben, wenn das Servicelevel mit Diagnoseablaufverfolgung in Windows aktiviert wird.

Communication Foundation (WCF) aktiviert wurde. Diese Option ist standardmäßig nicht aktiviert, und es wird davon abgeraten, sie in Produktionsumgebungen zu aktivieren. Diese Kennwörter werden in einer verschlüsselten Simple Object Access-Protokollnachricht (SOAP, Simple Object Access Protocol) als Klartext angezeigt, wenn sich Benutzer für die Kennwortzurücksetzung registrieren. Weitere Informationen finden Sie unter [Konfigurieren der Nachrichtenprotokollierung](http://go.microsoft.com/fwlink/?LinkID=168572).

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>Keinen Autorisierungsworkflow bei Kennwortzurücksetzung verwenden

Sie sollten einer Kennwortzurücksetzung keinen Autorisierungsworkflow anhängen. Für die Kennwortzurücksetzung ist eine synchrone Antwort erforderlich, Autorisierungsworkflows mit Aktivitäten wie der Genehmigungsaktivität sind jedoch asynchron.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>Der Kennwortzurücksetzung nicht mehrere Aktionsaktivitäten zuordnen

Sie sollten einer Kennwortzurücksetzung keinen Workflow anhängen, der mehr als eine Aktionsaktivität enthält. Ein Beispielszenario wäre, wenn einer MPR zur Kennwortzurücksetzung eine zweite AD DS-Kennwortzurücksetzungsaktivität zugeordnet würde. Dieses Szenario wird nicht unterstützt.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>Fordern erneuter Registrierung beim Hinzufügen, Entfernen oder Ändern der Reihenfolge der Aktivitäten in einem vorhandenen Workflow

Wählen Sie beim Hinzufügen, Entfernen oder Ändern der Reihenfolge der Authentifizierungsaktivitäten in einem vorhandenen Workflow immer die Option aus, eine erneute Registrierung zu fordern. Wenn Benutzer versuchen, eine Authentifizierung für eine Kennwortzurücksetzung durchzuführen, nachdem eine Aktivität zu einem Workflow hinzugefügt oder daraus entfernt wurde, sie sich aber noch nicht erneut registriert haben, können unerwünschte Auswirkungen auftreten.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>Portalkonfiguration und Ressourcensteuerungs-Anzeigekonfiguration

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>Eventuell Datenschutz-Haftungsausschluss zu Benutzerprofilseite hinzufügen

In MIM werden möglicherweise einige Informationen zum Benutzerprofil standardmäßig für andere Benutzer angezeigt. Als freundliche Geste gegenüber Benutzern sollten Administratoren darüber nachdenken, der Benutzerprofilseite entsprechend den Richtlinien ihres Unternehmens einen benutzerdefinierten Text hinzuzufügen. Weitere Informationen über das Hinzufügen von benutzerdefiniertem Text zu einer MIM-Portalseite finden Sie unter [Introduction to Configuring and Customizing the FIM Portal (Einführung in das Konfigurieren und Anpassen des FIM-Portals)](http://go.microsoft.com/fwlink/?LinkID=165848).

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>Ressourcentypen „Person“ und „Gruppe“ nicht löschen

Auch wenn die Ressourcentypen „Person“ und „Gruppe“ nicht als Core-Ressourcentypen gekennzeichnet sind, sollten weder die Ressourcen selbst noch die ihnen zugewiesenen Attribute gelöscht werden. Die Benutzeroberfläche (UI) im MIM-Portal erfordert, dass die Ressourcentypen „Person“ und „Gruppe“ und deren Attribute vorhanden sind.

#### <a name="do-not-modify-the-core-attributes"></a>Nicht die Core-Attribute verändern

Es gibt 13 Core-Attribute, die allen Ressourcentypen zugewiesen werden. Sie sollten deren Beziehung zu einem Ressourcentyp in keiner Weise ändern. Die 13 Core-Attribute sind:

-   CreatedTime

-   Creator

-   DeletedTime

-   Beschreibung

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Gebietsschema

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Löschen Sie keine Schemaressource, die von Überwachungsanforderungen abhängt

Sie sollten Ihre Schemaressourcen nicht löschen, während dafür noch Überwachungsanforderungen vorhanden sind.

#### <a name="making-regular-expressions-case-insensitive"></a>Bei regulären Ausdrücken Groß-/Kleinschreibung nicht berücksichtigen

In MIM kann es hilfreich sein, bei einigen regulären Ausdrücken die Groß-/Kleinschreibung nicht zu berücksichtigen. Sie können die Groß-/Kleinschreibung innerhalb einer Gruppe ignorieren, indem Sie ?!: verwenden. Zum Beispiel können Sie für Employee Type (Mitarbeitertyp) Folgendes verwenden:

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>Berechnung des Mitgliedsattributs

Das Mitgliedsattribut, das an die Synchronisierungs-Engine bereitgestellt wird, wird eigentlich ComputedMembers zugeordnet. Es ist eine Kombination aus kriterienbasierten und manuell ausgewählten Mitgliedern. Auch wenn Sie alle drei Attribute (Filter, ExplicitMembers und ComputedMembers) hinzufügen, findet die dynamische Berechnung des Mitgliedsattributs nicht für andere Ressourcentypen als Gruppe und Set statt.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>Vorangestellte und nachfolgende Leerzeichen in Zeichenfolgen werden ignoriert

In MIM können Sie Zeichenfolgen mit vorangestellten und nachfolgenden Leerzeichen eingeben, diese Leerzeichen werden jedoch vom MIM-System ignoriert. Wenn Sie eine Zeichenfolge mit einem vorangestellten und nachfolgenden Leerzeichen übermitteln, werden diese Leerzeichen von der Synchronisierungs-Engine und Webdiensten ignoriert.

#### <a name="empty-strings-do-not-equal-null"></a>Leere Zeichenfolgen sind nicht gleich null

In dieser Version von MIM sind leere Zeichenfolgen nicht gleich null. Die Eingabe einer leeren Zeichenfolge wird als gültiger Wert betrachtet. Keine Eingabe wird als Null betrachtet.

### <a name="workflow-and-request-processing"></a>Workflow und Verarbeitung von Anforderungen

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>Keine Standardworkflows löschen, die mit MIM 2016 geliefert werden

Die folgenden Workflows werden mit MIM geliefert und sollten nicht gelöscht werden:

-   Ablaufworkflow

-   Workflow zur Filtervalidierung für Administratoren

-   Workflow zur Filtervalidierung für Nicht-Administratoren

-   Workflow zur Gruppenablaufbenachrichtigung

-   Workflow zur Gruppenvalidierung

-   Workflow zur Besitzergenehmigung

-   Aktionsworkflow zur Kennwortzurücksetzung

-   Authentifizierungsworkflow zur Kennwortzurücksetzung

-   Anforderervalidierung mit Autorisierung durch den Besitzer

-   Anforderervalidierung ohne Autorisierung durch den Besitzer

-   Für die Registrierung erforderlicher Systemworkflow

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>Nicht mehrere ApprovalActivities parallel ausführen

Sie sollten nicht zwei oder mehr ApprovalActivities parallel ausführen. Dies kann dazu führen, dass die Anforderung in der Autorisierungsphase hängenbleibt. Um mehrere Genehmigungen zu erhalten, können Sie entweder eine umfangreichere Liste von genehmigenden Personen in die Genehmigung einschließen oder die beiden Aktivitäten aufeinander folgend sequenzieren.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>Autorisierung sollte MIM-Ressourcendaten nicht verändern

Vermeiden Sie als Teil der Workflows in Autorisierungsworkflows Aktivitäten wie die Funktionsauswertung, die die MIM-Ressourcen verändern. Da für die Anforderung am Punkt der Autorisierung bei der Verarbeitung kein Commit ausgeführt wurde, können alle an den Identitätsinformationen vorgenommenen Änderungen angewendet werden, obwohl die Anforderung möglicherweise zurückgewiesen wird.

### <a name="understanding-fim-service-partitions"></a>Grundlegendes zu FIM-Dienstpartitionen

Das Ziel von MIM ist die Verarbeitung von Anforderungen, die von verschiedenen MIM-Clients wie z.B. dem FIM-Synchronisierungsdienst und den Self-Service-Komponenten gemäß den konfigurierten Geschäftsrichtlinien initiiert werden können. Standardmäßig gehört jede FIM-Dienstinstanz zu einer logischen Gruppe, die aus einer oder mehreren FIM-Dienstinstanzen besteht, was auch als FIM-Dienstpartition bezeichnet wird. Wenn Sie nur eine FIM-Dienstinstanz zur Verarbeitung aller Anforderungen bereitstellen lassen, kann es bei der Verarbeitung zu Verzögerungen kommen. Einige Vorgänge können sogar die standardmäßigen Timeoutwerte überschreiten, die für Self-Service-Vorgänge angemessen sind. FIM-Dienstpartitionen können Ihnen bei der Behebung dieses Problems helfen.

Weitere Informationen finden Sie unter [Understanding FIM Service Partitions (Grundlegendes zu FIM-Dienstpartitionen)](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx).

## <a name="next-steps"></a>Nächste Schritte
- [FIM – Handbuch zur Sicherung und Wiederherstellung](http://go.microsoft.com/fwlink/?LinkID=165864)
- [Synchronisieren von Benutzern aus Active Directory zu FIM](http://go.microsoft.com/fwlink/?LinkID=188277) 
- [Übersicht über das Sicherheitsmodell](http://go.microsoft.com/fwlink/?LinkID=185370).
