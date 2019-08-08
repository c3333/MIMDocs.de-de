---
title: Verarbeitung von Daten in Microsoft Identity Manager | Microsoft-Dokumentation
description: Erfahren Sie mehr zur Verarbeitung von Daten in Microsoft Identity Manager zum Identifizieren von Daten und Erstellen von Berichten für Daten in der Umgebung sowie zum Durchführen von Aktionen in einem angegebenen System, das auf operativen Funktionen und Anforderungen basiert.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6f861c5b1984de70a91edcac89276402f289e355
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701497"
---
# <a name="microsoft-identity-manager-data-handling"></a>Verarbeitung von Daten in Microsoft Identity Manager 

Dieser Artikel bietet eine Anleitung, wie Organisationen Entscheidungen treffen können, die auf viele vernetzte Datenquellen angewendet werden können.  Dies kann durch Such-, Lösch-, Aktualisierungs- und Berichtsvorgänge erreicht werden.  Bevor Sie sich für einen Ansatz für das Löschen oder Aktualisieren entscheiden, ist es wichtig, das aktuelle Design und die Konfiguration Ihres Systems für die Identitätsverwaltung (MIM) zu kennen. 

Im Folgenden finden Sie einige Szenarios, und Kunden sollten folgende Fragen berücksichtigen und beantworten: 

- Welche Daten sind erforderlich, damit Ihre Identitätsverwaltung Sie bei Geschäftsprozessen unterstützt?
- Wo werden die aktuellen Daten in MIM gespeichert?
- Wie können Sie diese Daten im System verwenden?
- Geben Sie diese Daten für externe Datenquellen von Partnern frei (z.B. durch Exportieren)?
- Was ist die autorisierende Quelle für die Daten, und wie werden diese verarbeitet?
- Wie sieht Ihr Plan zur Datenaufbewahrung und Datenlöschung aus?
- Wissen Sie, welche Technologien Sie benötigen, um Daten zu verarbeiten und zu verwalten?

Wenn Sie eine aktuelle MIM-Umgebung verstehen möchten, können Sie folgendes Tool verwenden, um Ihre MIM-Umgebung zu dokumentieren oder dies aus Ihren Entwurfsdokumenten für die Implementierung abzuleiten.
- [Exportieren der aktuellen Konfiguration mit dem MIM-Dokumentierer](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Suchen und Ermitteln von personenbezogenen Daten
Das Suchen von Daten innerhalb von MIM hängt von der Konfiguration und dem Setup ab. Die meisten Umgebungen sind miteinander verbunden, aus Gründen der Übersichtlichkeit wurden diese jedoch von einer Komponente auf hoher Ebene separiert.

### <a name="synchronization-service"></a>Synchronisierungsdienst

Alle benutzerbezogenen Daten in MIM werden von Active Directory- und HR-Datenquellen abgeleitet. Bei der Suche nach personenbezogenen Daten sollten Sie zuerst in Azure AD oder in verknüpften Datenquellen nachsehen. 

Wenn Sie nicht sicher sind, welche Autoritätsquelle vorliegt, können Sie diesen Benutzer über die Synchronization Service Manager-Konsole von MIM nachverfolgen. Klicken Sie auf die Metaverse-Suchleiste, um die identifizierbaren personenbezogenen Daten anzuzeigen, die in der Datenbank gespeichert sind. Benutzer können nach einem bestimmten Benutzer oder einem bestimmten Attribut suchen.

- Durchführen einer Überprüfung von oder einer Suche nach Benutzerobjektdaten
    - Öffnen Sie den Client des Synchronisierungsdiensts.
        - Wenn Sie den Metaverse-Designer verwenden, können Sie die Importe und die Rangfolge des Attributflows sehen.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Wenn Sie die Metaverse-Suche verwenden, können Sie jedes Objekt und Attribut innerhalb der Datenbank suchen ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Wenn Sie ein Objekt gesucht haben, können Sie auf dieses klicken, um die Seite „Benutzerprofil“ zu öffnen. In den Objektdetails finden Sie umfassende Informationen zum Objekt, seinen Attributen, zur letzten Änderung, der Autoritätsquelle und zu zugehörigen verknüpften Datenquellen, die vom untenstehenden Beispiel für die Konfiguration des Verwaltungs-Agents abgeleitet wurden.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Dienst und Portal/PAM
Wenn Sie eine Instanz des Diensts und Portals oder von PAM installiert haben, ist es wichtig, nach Benutzern suchen zu können. 

Wenn Sie das Portal installiert haben, können Sie die Benutzeroberfläche verwenden, um jedes Attribut und jede Abfrage nach einem bestimmten Benutzer zu durchsuchen.

Wenn Sie nur den Dienstserver (ohne die Benutzeroberfläche des Portals) installiert haben, können Sie eine Suchsyntax ausführen, die auf [FIMAutomation PSSnapin] basiert. Ein Beispiel finden Sie [hier](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM kann die gleiche Suchsyntax wie oben verwenden, oder Sie können das [MIMPAM-Modul](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) verwenden, insbesondere das Cmdlet „get-pamuser“, um innerhalb der PAM-Umgebung nach dem Benutzer zu suchen.

Weitere Berichterstellungsoptionen für die Suche in verfügbaren Daten befinden sich im Dienst und im Portal.
- [Hybridberichterstellung](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Berichterstellung mit SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Der BHOLD Core-Dienst verfügt über eine Benutzeroberfläche, die die Suche nach Benutzern oder Attributen ermöglicht. 

![BHOLD-Suche](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Wenn Sie BHOLD mit dem [Zugriffsverwaltungsconnector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) für den Synchronisierungsdienst synchronisieren, können Sie die verknüpften Benutzerobjekte und die Attribute, die Sie an BHOLD-Core senden, anzeigen.

Sie können ebenfalls das BHOLD-Berichterstellungsmodul laden.

- [BHOLD-Berichterstellung](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Zertifikatverwaltung
Die Dienstsuche der Zertifikatverwaltung ist in die Benutzeroberfläche integriert. Der Administrator startet diese und wählt „Find user and view or manage their information“ (Benutzer oder Ansicht finden und Informationen verwalten) aus.  

![Suche der Zertifikatverwaltung](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportieren von personenbezogenen Daten
Da die zu Entitäten gehörenden Daten in MIM von mehreren Quellen abgeleitet werden, werden die meisten Daten in der Datenbank des Synchronisierungsdiensts gespeichert. Deshalb sollten Sie objektbezogene Daten aus der MIM-Synchronisierung exportieren oder den Besitzer dieser Daten bestimmen.

### <a name="synchronization-service"></a>Synchronisierungsdienst
Synchronisierungsdienste für das Exportieren von Daten wählen einfach die Daten aus der Benutzeroberfläche der Suche aus, kopieren diese und fügen sie in eine CSV-Datei oder in das bevorzugte Format ein. Eine weitere Möglichkeit zum Exportieren dieser Daten ist das Erstellen eines dateibasierten Verwaltungs-Agents, um aktuelle Daten eines bestimmten Benutzers zu löschen. Ein Beispiel für die Verwendung eines dateibasierten Verwaltungs-Agents finden Sie [hier](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Dienst und Portal/PAM
Im Dienst und Portal können Sie zusammen mit PAM die Daten exportieren, eine Suchsyntax basierend auf [FIMAutomation PSSnapin] ausführen und diese über die Pipeline an die [CSV-Datei](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) übergeben. Ein Beispiel dafür finden Sie [hier](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM kann die gleiche Suchsyntax wie oben verwenden, oder Sie können das [MIMPAM-Modul](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) verwenden, insbesondere das Cmdlet „get-pamuser“, um innerhalb der PAM-Umgebung nach dem Benutzer zu suchen und diesen über die Pipeline an eine CSV-Datei zu übergeben.

- [Beispiel für das Abfragen des MIM-Diensts mithilfe von PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
BHOLD-Daten können mithilfe des BHOLD-Berichterstellungsmoduls in das bevorzugte Format exportiert werden.

### <a name="certificate-management"></a>Zertifikatverwaltung
Die Daten der Zertifikatverwaltung, die mit den personenbezogenen Daten in Zusammenhang stehen, sind mit Active Directory verbunden. Ein Administrator kann diese Daten mithilfe von Active Directory PowerShell exportieren.

## <a name="updating-personal-data"></a>Aktualisieren von personenbezogenen Daten

Personenbezogene Daten zu Benutzern oder Objekten in MIM-Lösungen werden üblicherweise vom Benutzerobjekt in den verbundenen Datenquellen Ihrer Organisation abgeleitet. Dies liegt daran, dass alle Änderungen, die Sie in der HR-Quelle oder in einem anderen autorisierenden Datensatzsystem (z.B. AD) am Benutzerprofil vornehmen, anschließend im MIM-Synchronisierungsdienst angezeigt werden.

### <a name="synchronization-service"></a>Synchronisierungsdienst

Administratoren müssen zu den [hier](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)) definierten Synchronisierungsvorgängen oder -administratoren gehören, um Verwaltungsvorgänge durchzuführen.

Ein Update der Daten wird durchgeführt, indem Regeln von der Autoritätsquelle definiert werden. Die Verwaltungskonsole unterstützt Sie beim Identifizieren der Autoritätsquelle, um diese an der Quelle zu aktualisieren. Eine weitere Möglichkeit besteht im Erstellen einer Synchronisierungsregel oder einer Regelerweiterung, um das Update der Daten zu steuern, wenn eine Quelle wie eine HR-Datenquelle beibehalten werden soll. Dies sind die verfügbaren unterstützten Optionen.

Im Folgenden finden Sie weitere Informationen zu den verschiedenen Methoden, um Attribute zu aktualisieren. 

- [Using Rules Extensions (Verwenden von Regelerweiterungen)](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Grundlegendes zur Datensynchronisierung mit externen Systemen](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Dienst und Portal/PAM

Der Dienst und das Portal zum Einfügen von PAM-Daten können mithilfe der FIMAutomation- oder PAM-Cmdlets aktualisiert werden. Wenn Sie über das Portal verfügen, können Sie ein direktes Update durchführen, indem Sie das Objekt suchen und bearbeiten. Beachten Sie, dass je nach Konfiguration ein einfaches Update über das Portal nicht bedeutet, dass diese Änderungen beibehalten werden. Dies liegt daran, dass die Autoritätsquelle stark von der Gesamtkonfiguration abhängig ist.

### <a name="bhold"></a>BHOLD

Benutzer können direkt über die BHOLD Core-Benutzeroberfläche oder über den Connector der Zugriffsverwaltung aktualisiert werden.

### <a name="certificate-management"></a>Zertifikatverwaltung

Die Benutzer im Zertifikatverwaltungsdienst werden aus Active Directory übernommen. Verwenden Sie Active Directory, um Objektdetails zu bearbeiten und zu aktualisieren.

## <a name="deleting-personal-data"></a>Löschen von personenbezogenen Daten

>[!Note] 
> Dieser Artikel enthält Anleitungen zum Löschen von personenbezogenen Daten aus Microsoft Identity Manager und kann verwendet werden, um Ihren Pflichten gemäß der DSGVO nachzukommen. Allgemeine Informationen zur DSGVO finden Sie im [Abschnitt „DSGVO“ im Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Die Daten in MIM werden synchronisiert und stets über die verbundene Datenquelle aktualisiert. Wenn ein Objekt im Ziel gelöscht wird, können die Daten des Objekts in MIM für eine Sicherheitsüberprüfung beibehalten werden. Das Löschen von Objekten wird nach den Regeln oder Regelerweiterungen (z.B. durch Code) pro verbundener Datenquelle oder nach den Objektlöschregeln konfiguriert.

### <a name="synchronization-service"></a>Synchronisierungsdienst
Im Synchronisierungsdienst stehen viele Möglichkeiten zur Verfügung, um Daten abhängig vom Geschäftsprozess zu verarbeiten oder zu löschen. In den folgenden Artikeln finden Sie weitere Informationen zu den Optionen für das Löschen und Aktualisieren von Attributen: 

- [Grundlegendes zur Aufhebung der Bereitstellung](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Using Rules Extensions (Verwenden von Regelerweiterungen)](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Bewährte Methoden für MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Dienst und Portal/PAM

Für den Dienst und das Portal wird empfohlen, dass Sie die Standardkonfiguration von 30 Tagen für die Aufbewahrung von Systemressourcen beibehalten. Dadurch wird dem Dienst vermittelt, wann die Löschung durchgeführt wird. Es werden nicht nur Anforderungsdaten gelöscht, sondern auch Objekte, die aus dem System gelöscht werden müssen. Wenn der Prozess durchgeführt wird, werden alle mit diesem Objekt verknüpften Daten gelöscht. Dazu zählen alle SSPR-Registrierungsdaten. Dies wird in die obige Konfiguration für die Löschung von Objekten einbezogen. Die GUID der Objekte wird in einer Tabelle gespeichert. Für das Reduzieren der Gesamtgröße der Tabelle im Build 4.4.1459 wurde ein Prozess namens „FIM_DeleteExpiredSystemObjectsJob“ hinzugefügt. Weitere Informationen zu diesem Prozess finden Sie [hier](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

BHOLD kann wie die meisten mit dem Synchronisierungsdienst verbundenen Systeme dafür konfiguriert werden, eine Löschung durchzuführen, sobald ein Quellobjekt wie HR entfernt wurde. Dies wird im Verwaltungs-Agent konfiguriert und von den Objektlöschregeln wie in den Features des Synchronisierungsdiensts beschrieben gesteuert.

Eine weitere Möglichkeit ist das direkte Entfernen des Benutzerobjekts aus der BHOLD Core-Benutzeroberfläche. Dies funktioniert je nach Setup gut, beachten Sie jedoch, dass die Bereitstellungslogik diesen Benutzer erneut erstellen könnte, wenn er nicht aus der Quelle gelöscht wird.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Zertifikatverwaltung
Löschen Sie einen Benutzer aus Active Directory, um ihn aus der Zertifikatverwaltung zu entfernen.

Die Zertifikatverwaltung speicher nur die Benutzer-ID des Profils aus Zertifikatdiensten mit der Domäne „sAMAccountName“. Sobald der Benutzer aus Active Directory gelöscht wurde, wird der Benutzercache nur für Zertifikate beibehalten, in denen dieser registriert ist. Es wird nicht empfohlen, Elemente aus der Datenbank zu löschen, da dies die Ausführung der Umgebung beeinträchtigen kann.

## <a name="opt-out-of-telemetry"></a>Deaktivieren der Telemetrie
In vorherigen Builds wurden von FIM/MIM anonymisierte Telemetriedaten zu jeder Bereitstellung gesammelt und über HTTPS an die Microsoft-Server übertragen. Diese Daten wurden von Microsoft früher zur Verbesserung von zukünftigen FIM- und MIM-Versionen verwendet.

>[!Note] 
> In späteren Releases von 4.5.x.x oder höher ist die Datensammlung deaktiviert.

Wenn Sie die Datensammlung in vorherigen Versionen deaktivieren möchten, führen Sie „Modus ändern“ aus, und wählen Sie in folgender Aufforderung die entsprechende Option aus:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

Alternativ können Sie die Registrierung bearbeiten und folgenden Wert auf 0 festlegen: (Komponente)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010.

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Nächste Schritte 
- [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Abschnitt „DSGVO“ im Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 Archive: Ramp Up – Implementing Forefront Identity Manager 2010 (FIM 2010-Archiv: Einstieg in die Implementierung von Forefront Identity Manager 2010)](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
