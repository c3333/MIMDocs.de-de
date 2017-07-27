---
title: PAM-Notfallwiederherstellung | Microsoft Docs
description: "Erfahren Sie, wie Sie Privileged Access Management für Hochverfügbarkeit und die Notfallwiederherstellung konfigurieren."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2fab9af837ed11b1f2f7f32c9ced6d79c8cc9d00
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment"></a>Hohe Verfügbarkeit und Überlegungen zur Notfallwiederherstellung für die geschützte Umgebung
Dieser Artikel beschreibt Überlegungen zur hohen Verfügbarkeit und Notfallwiederherstellung bei der Bereitstellung von Active Directory-Domänendiensten (AD DS, Active Directory Domain Services) und Microsoft Identity Manager 2016 (MIM) für Privileged Access Management (PAM).

Unternehmen konzentrieren sich auf Hochverfügbarkeit und Notfallwiederherstellung für Workloads in Windows Server, SQL Server und Active Directory. Aber die zuverlässige Verfügbarkeit der geschützten Umgebung für Privileged Access Management ist auch wichtig. Die geschützte Umgebung ist ein wichtiger Bestandteil der IT-Infrastruktur der Organisation, da Benutzer mit ihren Komponenten interagieren, um administrative Rollen zu übernehmen. Um weitere Informationen zur hohen Verfügbarkeit im Allgemeinen zu erhalten, können Sie das Whitepaper [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc) (Übersicht von Microsoft zur hohen Verfügbarkeit) herunterladen.

## <a name="high-availability-and-disaster-recovery-scenarios"></a>Szenarien zu Hochverfügbarkeit und Notfallwiederherstellung

Berücksichtigen Sie bei der Planung für hohe Verfügbarkeit und Wiederherstellung im Notfall folgende Fragen:

- Welche Funktionen können von einem Ausfall betroffen sein?
- Welche Funktionen sind unternehmenswichtig und/oder für den IT-Betrieb kritisch?
- Welche Risiken könnten zu einem Ausfall in diesen Systemen führen?

Der Umfang dieser Berücksichtigungen wirkt sich auf die Gesamtkosten für Bereitstellung und Betrieb aus, sodass Organisationen möglicherweise bestimmten Funktionen Priorität vor anderen einräumen und auch das Risiko von temporären Ausfällen bei Funktionen niedrigerer Priorität akzeptieren sollten. Die folgende Tabelle zeigt eine potenzielle Prioritätsrangfolge, die eine Organisation anwenden könnte:

| **Geschützte Gesamtstrukturfunktion** | **Relative Priorität bei der Wiederherstellung** | **Risikominderung, wenn die Funktion nicht verfügbar ist** |
| --------------------------- | --------------------- | -------------- |
| Einrichtung einer Vertrauensstellung         | Niedrig | Warten, bis die geschützte Umgebung wiederhergestellt ist |
| Benutzer- und Gruppenmigration   | Niedrig | Warten, bis die geschützte Umgebung wiederhergestellt ist |
| MIM-Administration          | Niedrig | Warten, bis die geschützte Umgebung wiederhergestellt ist |
| Aktivierung der privilegierten Rolle  | Mittel | Dedizierte Smartcard-gesicherte Konten zum manuellen Hinzufügen von Benutzern zu administrativen Gruppen |
| Ressourcenverwaltung         | Hoch | Dedizierte Smartcard-gesicherte Konten zum manuellen Hinzufügen von Benutzern zu administrativen Gruppen |
| Überwachung von Benutzern und Gruppen in einer vorhandenen Gesamtstruktur | Niedrig | Warten, bis die geschützte Umgebung wiederhergestellt ist |

Jetzt betrachten wir jede dieser Funktionen der geschützten Gesamtstruktur.

### <a name="trust-establishment"></a>Einrichtung einer Vertrauensstellung

Es muss eine Gesamtstrukturvertrauensstellung zwischen den Domänen der vorhandenen Gesamtstruktur und der Gesamtstruktur der geschützten Umgebung vorhanden sein. Dies ist erforderlich, damit Benutzer, die sich bei der geschützten Umgebung authentifizieren, Ressourcen in den vorhandenen Gesamtstrukturen verwalten können. Zusätzliche Konfiguration kann z. B. erforderlich werden, um die Migration von Benutzern aus vorhandenen Domänen in früheren Versionen von Windows Server zuzulassen.

Die Einrichtung einer Vertrauensstellung erfordert sowohl, dass der vorhandene Gesamtstruktur-Domänencontroller online ist, als auch die MIM- und AD-Komponenten der geschützten Umgebung.  Wenn beliebige dieser Voraussetzungen während der Einrichtung der Vertrauensstellung ausfallen, kann der Administrator einen neuen Versuch starten, nachdem der Ausfall behandelt wurde.  Für den Fall, dass der vorhandene Gesamtstruktur-Domänencontroller oder die geschützte Umgebung nach einem Ausfall wiederhergestellt wurde, enthält MIM auch die PowerShell-Cmdlets `Test-PAMTrust` und `Test-PAMDomainConfiguration`, die verwendet werden können, um sicherzustellen, dass noch eine Vertrauensstellung gültig ist.

### <a name="user-and-group-migration"></a>Benutzer- und Gruppenmigration

Sobald die Vertrauensstellung eingerichtet ist, können in der geschützten Umgebung sowohl Schattengruppen als auch Benutzerkonten für Mitglieder dieser Gruppen und genehmigende Personen erstellt werden. Dies ermöglicht diesen Benutzern, privilegierte Rollen zu aktivieren und wieder effektive Gruppenmitgliedschaften zu erhalten.

Benutzer- und Gruppenmigration erfordert sowohl, dass der vorhandene Gesamtstruktur-Domänencontroller online ist, als auch die MIM- und AD-Komponenten der geschützten Umgebung.   Wenn die vorhandenen Gesamtstruktur-Domänencontroller nicht erreichbar sind, können der geschützten Umgebung keine zusätzlichen Benutzer und Gruppen hinzugefügt werden, aber vorhandene Benutzer und Gruppen sind nicht betroffen. Wenn beliebige dieser Komponenten während der Migration ausfallen, kann der Administrator einen neuen Versuch starten, nachdem der Ausfall behandelt wurde.

### <a name="mim-administration"></a>MIM-Administration
Nachdem Benutzer und Gruppen migriert wurden, können Administratoren durch Verknüpfen von Benutzern als Kandidaten für die Aktivierung in Rollen weiter Rollenzuweisungen in MIM konfigurieren.  Sie können auch die MIM-Richtlinien zur Genehmigung und Azure MFA konfigurieren.  

MIM-Administration erfordert, dass die MIM- und AD-Komponenten der geschützten Umgebung online sind.

### <a name="privileged-role-activation"></a>Aktivierung der privilegierten Rolle
Wenn ein Benutzer eine privilegierte Rolle aktivieren möchte, muss er sich bei der Domäne der geschützten Umgebung authentifizieren und eine Anforderung an MIM übermitteln.  MIM enthält SOAP- und REST-APIs sowie Benutzeroberflächen in PowerShell und auf einer Webseite.

Die Aktivierung der privilegierten Rolle erfordert, dass die MIM- und AD-Komponenten der geschützten Umgebung online sind.  Wenn MIM für die Verwendung von [Azure MFA zur Aktivierung](use-azure-mfa-for-activation.md) der ausgewählten Rolle konfiguriert ist, ist für den Kontakt mit dem Azure MFA-Dienst darüber hinaus Internetzugriff erforderlich ist.

### <a name="resource-management"></a>Ressourcenverwaltung.
Sobald ein Benutzer die Rolle erfolgreich aktiviert hat, kann der Domänencontroller ein Kerberos-Ticket für ihn generieren, dass von Domänencontrollern in den vorhandenen Domänen verwendet werden kann, und erkennt die neuen temporären Gruppenmitgliedschaften des Benutzers.

Ressourcenverwaltung erfordert, dass ein Domänencontroller für die Ressourcendomäne online ist, sowie einen Domänencontroller in der geschützten Umgebung.  Sobald ein Benutzer aktiviert ist, erfordert die Ausstellung seines Kerberos-Ticket nicht, dass MIM oder SQL in der geschützten Umgebung online ist.  (Beachten Sie, dass bei Windows Server 2012 R2 als Funktionsebene der geschützten Umgebung MIM online sein muss, um die Mitgliedschaft in der temporären Gruppe zu beenden.)

### <a name="monitoring-of-users-and-groups-in-the-existing-forest"></a>Überwachung von Benutzern und Gruppen in der vorhandenen Gesamtstruktur
MIM enthält auch einen PAM-Überwachungsdienst, der regelmäßig die Benutzer und Gruppen in den vorhandenen Domänen überprüft und MIM-Datenbank und AD entsprechend aktualisiert.  Dieser Dienst muss für die Rollenaktivierung oder während der Ressourcenverwaltung nicht online sein.

Die Überwachung erfordert sowohl, dass der vorhandene Gesamtstruktur-Domänencontroller online ist, als auch die MIM- und AD-Komponenten der geschützten Umgebung.  

## <a name="deployment-options"></a>Bereitstellungsoptionen
Die [Übersicht über die Umgebung](environment-overview.md) veranschaulicht eine einfache Topologie zum Erlernen der Technologie, die nicht für hohe Verfügbarkeit konzipiert ist. Dieser Abschnitt beschreibt, wie Sie die Topologie für die Bereitstellung hoher Verfügbarkeit ausweiten – sowohl für Organisationen mit einem einzigen Standort als auch solche mit mehreren Standorten.

### <a name="networking"></a>Netzwerk

Der Netzwerkverkehr zwischen den Computern in der geschützten Umgebung sollte von den vorhandenen Netzwerken isoliert werden, z. B. durch Verwendung eines anderen physischen oder virtuellen Netzwerks.  Je nach den Risiken für die geschützte Umgebung können auch unabhängige physische Verbindungen zwischen den Computern erforderlich sein.  Bestimmte Failovercluster-Technologien stellen zusätzliche Anforderungen an Netzwerkschnittstellen.

Die Computer, die die Active Directory-Domänendienste hosten, und diejenigen, die die MIM-Dienste in der geschützten Umgebung hosten, erfordern bidirektionale Konnektivität mit Ressourcen in der vorhandenen Gesamtstruktur für folgende Zwecke:
- Authentifizierung von Benutzern durch die PRIV-Gesamtstruktur-Domänencontroller
- Aktivierungsanforderung durch Benutzer
- Kerberos-Tickets für Benutzer, die von Ressourcen in einer vorhandenen Gesamtstruktur nutzbar sind
- MIM zum Überwachen der vorhandenen Domänen der Gesamtstruktur
- MIM zum Senden von E-Mail über E-Mail-Server in der vorhandenen Gesamtstruktur.

### <a name="minimal-high-availability-topologies"></a>Minimale Hochverfügbarkeitstopologien
Eine Organisation kann auswählen, welche Funktionen in ihrer geschützten Umgebung hohe Verfügbarkeit erfordern – mit den folgenden Einschränkungen:

- Hohe Verfügbarkeit für eine beliebige Funktion, die von der geschützten Umgebung bereitgestellt wird, erfordert mindestens zwei Domänencontroller.  
- Hohe Verfügbarkeit für Aktivierungsanforderungen erfordert mindestens zwei Computer, die den MIM-Dienst hosten, und außerdem hohe Verfügbarkeit für SQL Server.
- Hohe Verfügbarkeit für SQL Server mit Failoverclustern erfordert mindestens zwei Server, die SQL Server bereitstellen, und diese dürfen nicht mit dem Domänencontroller identisch sein.
- Der MIM-Dienst darf nicht auf dem Domänencontroller installiert werden, um die Angriffsfläche der einzelnen Server zu minimieren.

Die kleinste Hochverfügbarkeitstopologie für alle Funktionen in einer geschützten Umgebung besteht aus mindestens vier Servern und freigegebenem Speicher. Zwei der Server müssen als Domänencontroller konfiguriert werden, die Active Directory-Domänendienste bereitstellen. Die anderen beiden Server können als Failovercluster konfiguriert werden, die SQL Server und den MIM-Dienst bereitstellen.

Darüber hinaus enthält eine typische Bereitstellung der geschützten Umgebung auch eine privilegierte Verwaltungsarbeitsstation für die Verwaltung dieser Server sowie eine Überwachungskomponente.

Das folgende Diagramm veranschaulicht eine mögliche Architektur:

![Topologie der geschützten Umgebung – Diagramm](media/bastion1.png)

Für jede dieser Funktionen können zusätzliche Server konfiguriert werden, um höhere Leistung unter Belastung oder geografische Redundanz bereitzustellen, wie unten beschrieben.

### <a name="deployments-supporting-multiple-sites"></a>Bereitstellungen, die mehrere Standorte unterstützen
Die Auswahl der richtigen Bereitstellungstopologie für Ressourcen, die an mehreren Standorten bereitgestellt werden, hängt von drei Faktoren ab:  
- Ziele und Risiken bei hoher Verfügbarkeit und Notfallwiederherstellung  
- Die Hardwarefunktionen zum Hosten der geschützten Umgebung  
- Das administrative Arbeitsmodell für jeden Standort.

Eine der einfachsten Methoden wäre das Hosten der geschützten Umgebung an einem bestimmten Standort.  Unter normalen Umständen würden Benutzer in der geschützten Umgebung dieses Standorts eine Verbindung mit der MIM-Bereitstellung herstellen und die Aktivierung anfordern, und die Aktivierungen hätten Auswirkungen auf Ressourcen an jedem Standort.  Falls die Netzwerkverbindung unterbrochen, oder der Standort, der die geschützte Umgebung hostet, nicht verfügbar ist, ist der Zugriff mittels Offlineanmeldeinformationen an einem anderen Standort möglich, um eine temporäre Verwaltung auszuführen, bis die Verbindung mit dem Netzwerk wieder hergestellt ist.  Dieser Ansatz eignet sich möglicherweise für Situationen, in denen erwartet wird, dass die lokale Verwaltung eines bestimmten Standorts, z. B. einer Zweigstelle, nur minimalen Aufwand erfordert und sich auf das Wiederherstellen der Verbindung dieses Standorts mit dem restlichen Netzwerk einer Organisation beschränkt.

![Einzelne geschützte Umgebung für standortübergreifende Topologie – Diagramm](media/bastion2.png)

Für standortübergreifende hohe Verfügbarkeit und Notfallwiederherstellung können die Komponenten der geschützten Umgebung auch unter Nutzung eines gemeinsamen PRIV-Verzeichnisses und einer allgemeinen SQL-Datenbank an jedem Standort bereitgestellt werden.  Sollte in dieser Topologie die Netzwerkverbindung getrennt werden, können die Benutzer an jedem Standort weiterhin unabhängig arbeiten.

![Mehrere geschützte Umgebungen für standortübergreifende Topologie – Diagramm](media/bastion3.png)

Eine Einschränkung bei diesem Bereitstellungsansatz ist, dass SQL Server einen Cluster benötigt, der beide Standorte umfasst, und dessen Bereitstellung könnte komplex sein. Erwägen Sie in diesem Fall als Alternative, nur das Active Directory (PRIV-Gesamtstruktur) der geschützten Umgebung zu replizieren.  Im Fall einer Unterbrechung des Netzwerks zwischen Standorten könnten Benutzer an Standort B, die ihre Rollen bereits zuvor aktiviert haben, weiterhin Ressourcen an Standort B verwalten.

![Replizierte geschützte Umgebung für standortübergreifende Topologie – Diagramm](media/bastion4.png)

Wenn jeder Standort eine separate Verwaltungsgrenze darstellt, ist es auch möglich, mehrere unabhängige geschützte Umgebungen bereitzustellen.  Jede geschützte Umgebung hätte zwar die gleiche Software, doch die Domänennamen wären unterschiedlich, und es gäbe keine Gemeinsamkeit zwischen den Verzeichnissen und Datenbanken der geschützten Umgebungen. Ein Benutzer, der Ressourcen an einem bestimmten Standort verwalten möchte, würde ein Benutzerkonto in der geschützten Umgebung dieses Standort aktivieren.

![Unabhängige geschützte Umgebungen für standortübergreifende Topologie – Diagramm](media/bastion5.png)

Schließlich sind komplexere Bereitstellungen möglich, da mehrere geschützte Umgebungen unabhängig konfiguriert werden können, um Ressourcen in einer bestimmten Domäne zu verwalten.

![Komplexe geschützte Umgebung für standortübergreifende Topologie – Diagramm](media/bastion6.png)

### <a name="hosted-bastion-environment"></a>Gehostete geschützte Umgebung
Einige Organisationen haben auch erwogen, die geschützte Umgebung separat von vorhandenen Standorten einzurichten. Die Software der geschützten Umgebung kann entweder in Netzwerken der Organisation oder bei einem externen Hostinganbieter auf einer Virtualisierungsplattform gehostet werden.  Beachten Sie bei der Evaluierung dieses Ansatzes Folgendes:

- Zum Schutz vor Angriffen aus den vorhandenen Domänen muss die Verwaltung der geschützten Umgebung von den Administratorkonten der vorhandenen Domäne isoliert werden.
- Die geschützte Umgebung erfordert TCP/IP-Konnektivität mit den Domänencontrollern in der vorhandenen Domäne.  Eine Liste der Ports finden Sie unter [How to configure a firewall for domains and trusts](https://support.microsoft.com/kb/179442) (Konfigurieren einer Firewall für Domänen und Vertrauensstellungen).
- Eine virtualisierte Bereitstellung von Active Directory-Domänendiensten setzt bestimmte Features bei der Virtualisierungsplattform voraus, siehe [Bereitstellung und Konfiguration virtualisierter Domänencontroller](https://technet.microsoft.com/library/jj574223.aspx).
- Eine Bereitstellung mit hoher Verfügbarkeit von SQL Server für den MIM-Dienst erfordert spezielle Speicherkonfiguration, wie unten im Abschnitt [SQL Server-Datenbankspeicher](#sql-server-database-storage) beschrieben.  Möglicherweise bieten derzeit nicht alle Hostinganbieter Windows Server-Hosting mit Datenträgerkonfigurationen an, die für SQL Server-Failovercluster geeignet sind.

## <a name="deployment-preparation-and-recovery-procedures"></a>Vorbereitung der Bereitstellung und Wiederherstellungsverfahren
Bei der Vorbereitung einer Bereitstellung mit hoher Verfügbarkeit oder für die Notfallwiederherstellung bereiten Bereitstellung der geschützten Umgebung muss berücksichtigt werden, wie Windows Server Active Directory, SQL Server und die Datenbank auf freigegebenem Speicher und der MIM-Dienst und seine PAM-Komponenten installiert werden.

### <a name="windows-server"></a>Windows Server
Windows Server enthält ein integriertes Feature für hohe Verfügbarkeit, sodass mehrere Computer als Failovercluster zusammenarbeiten können. Die geclusterten Server sind physisch über Kabel sowie durch Software verbunden. Wenn auf einem oder mehreren der Clusterknoten ein Fehler auftritt, werden seine Aufgaben sofort von anderen Knoten übernommen. Dieser Vorgang wird als Failover bezeichnet.   Weitere Informationen finden Sie unter [Failoverclustering: Übersicht](https://technet.microsoft.com/library/hh831579.aspx).

Stellen Sie sicher, dass das Betriebssystem und die Anwendungen in der geschützten Umgebung Updates zu Sicherheitsproblemen erhalten. Einige dieser Updates erfordern möglicherweise einen Neustart des Servers, darum koordinieren Sie die Zeiten, in denen Updates auf den Servern ausgeführt werden, um längere Ausfallzeiten zu vermeiden. Ein möglicher Ansatz ist die Verwendung [des clusterfähigen Aktualisierens](https://technet.microsoft.com/library/hh831694.aspx) für die Server in einem Windows Server-Failovercluster.

Die Server in der geschützten Umgebung werden einer Domäne hinzugefügt, und damit abhängig von den Domänendiensten. Stellen Sie sicher, dass sie nicht versehentlich mit einer Abhängigkeit von einem bestimmten Domänencontroller für Dienste wie DNS konfiguriert werden.

### <a name="bastion-environment-active-directory"></a>Active Directory für geschützte Umgebungen
Systemeigen umfassen Windows Server Active Directory-Domänendienste Unterstützung für hohe Verfügbarkeit und Notfallwiederherstellung.

#### <a name="preparation"></a>Vorbereitung
Eine normale Produktionsbereitstellung des Privileged Access Management umfasst mindestens zwei Domänencontroller in der geschützten Umgebung. Eine Anleitung zum Einrichten des ersten Domänencontrollers in der geschützten Umgebung finden Sie in Schritt 2 der Bereitstellungsartikel, [Prepare the PRIV domain controller](step-2-prepare-priv-domain-controller.md) (Vorbereiten des PRIV-Domänencontrollers).

Das Verfahren zum Hinzufügen eines zusätzlichen Domänencontrollers finden Sie unter [Installieren eines Windows Server 2012-Domänencontrollerreplikats in einer vorhandenen Domäne (Stufe 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Wenn der Domänencontroller auf einer Virtualisierungsplattform wie Hyper-V gehostet werden soll, beachten Sie die Warnungen in [Bereitstellung und Konfiguration virtualisierter Domänencontroller](https://technet.microsoft.com/library/jj574223.aspx).

#### <a name="recovery"></a>Wiederherstellung
Stellen Sie nach einem Ausfall sicher, dass mindestens ein Domänencontroller in der geschützten Umgebung verfügbar ist, bevor Sie die anderen Server neu starten.

Innerhalb einer Domäne verteilt Active Directory die Flexible Single Master Operation-Rollen (FSMO) auf die Domänencontroller, wie in [How Operations Masters Work](https://technet.microsoft.com/library/cc780487.aspx) (So funktioniert Operations Masters) beschrieben.  Wenn bei einem Domänencontroller ein Fehler aufgetreten ist, müssen möglicherweise eine oder mehrere der [Domänencontrollerrollen](https://technet.microsoft.com/library/cc786438.aspx), die dem Domänencontroller zugewiesen wurden, übertragen werden.

Nachdem Sie festgestellt haben, dass ein Domänencontroller nicht für die Produktion zurückgegeben wird, überprüfen Sie, ob diesem Domänencontroller Rollen zugewiesen wurden, und weisen Sie diese nach Bedarf neu zu. Anleitungen finden Sie in [View the Current Operations Master Role Holders](https://technet.microsoft.com/library/cc816893.aspx) (Anzeigen der aktuellen Operations Master-Rolleninhaber) und zugehörigen Artikeln.

Sie sollten auch die DNS-Einstellungen von Computern überprüfen, die der geschützten Umgebung hinzugefügt wurden, sowie die Domänencontroller in CORP-Domänen mit einer Vertrauensstellung zu diesem Domänencontroller, um sicherzustellen, dass sie nicht mit einer Abhängigkeit von der IP-Adresse dieses Domänencontrollercomputers hartcodiert sind.

### <a name="sql-server-database-storage"></a>SQL Server-Datenbankspeicher
Eine Bereitstellung mit hoher Verfügbarkeit erfordert SQL Server-Failovercluster, und SQL Server-Failovercluster-Instanzen reagieren auf Speicher, der für alle Knoten für Datenbank- und Protokollspeicherung freigegeben wird. Der freigegebene Speicher kann in Form von Windows Server Failover Clustering-Clusterdatenträgern, Datenträgern auf einem Storage Area Network (SAN) oder Dateifreigaben auf einem SMB-Server vorliegen.  Beachten Sie, dass diese der geschützten Umgebung zugewiesen werden müssen. Freigeben von Speicher für andere Workloads außerhalb der geschützten Umgebung wird nicht empfohlen, da die Integrität der geschützten Umgebung gefährdet werden könnte.

### <a name="sql-server"></a>SQL Server
MIM-Dienst erfordert eine SQL Server-Bereitstellung in der geschützten Umgebung.   Für eine hohe Verfügbarkeit kann SQL über eine Failoverclusterinstanz (FCI) bereitgestellt werden. Im Gegensatz zu eigenständigen Instanzen ist in FCIs die hohe Verfügbarkeit von SQL Server durch das Vorhandensein redundanter Knoten in der FCI geschützt. Bei einem Fehler oder geplanten Upgrade wird der Ressourcengruppenbesitz zu einem anderen Windows Server-Failovercluster-Knoten verschoben.

Wenn Sie nur Unterstützung für die Wiederherstellung im Notfall, aber keine hohe Verfügbarkeit benötigen, dann können Protokollversand, Transaktionsreplikation, Snapshotreplikation oder Datenbankspiegelung anstelle des Failoverclusterings verwendet werden.   

#### <a name="preparation"></a>Vorbereitung
Wenn Sie SQL Server in der geschützten Umgebung installieren, muss es von ggf. bereits in den CORP-Gesamtstrukturen vorhandenen SQL Server-Instanzen unabhängig sein.  Darüber hinaus sollte SQL Server auf einem dedizierten Server bereitgestellt werden, einem anderen als dem des Domänencontrollers.
Weitere Informationen finden Sie im SQL Server-Handbuch zu [AlwaysOn-Failoverclusterinstanzen (SQL Server)](https://msdn.microsoft.com/library/ms189134.aspx).

#### <a name="recovery"></a>Wiederherstellung
Wenn SQL Server mit Protokollversand für die Wiederherstellung im Notfall konfiguriert wurde, müssen Sie dafür sorgen, dass SQL Server während der Wiederherstellung aktualisiert wird.  Darüber hinaus ist das Neustarten jeder MIM-Dienstinstanz erforderlich.

Wenn bei SQL Server ein Fehler aufgetreten ist, oder die Verbindung zwischen SQL Server und MIM-Dienst verloren geht, sollten Sie nach der Wiederherstellung von SQL Server jeden MIM-Dienst neu starten.  Dadurch wird sichergestellt, dass der MIM-Dienst erneut eine Verbindung mit SQL Server herstellt.

### <a name="mim-service"></a>MIM-Dienst
Der MIM-Dienst ist erforderlich, um Aktivierungsanforderungen zu verarbeiten.  Damit ein Computer, der den MIM-Dienst hostet, zur Wartung heruntergefahren werden kann, während noch Aktivierungsanforderungen empfangen werden, können mehrere MIM-Dienst-Computer bereitgestellt werden.  Beachten Sie, dass der MIM-Dienst nicht an Kerberos-Vorgängen beteiligt ist, nachdem ein Benutzer einer Gruppe hinzugefügt wurde.  

#### <a name="preparation"></a>Vorbereitung
Sie sollten den MIM-Dienst auf mehreren Servern bereitstellen, die der PRIV-Domäne hinzugefügt werden.
Informationen zu hoher Verfügbarkeit finden Sie in den Windows Server-Dokumenten [Hardwareanforderungen und Speicheroptionen für Failovercluster](https://technet.microsoft.com/library/jj612869.aspx) und [Creating a Windows Server 2012 Failover Cluster](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx) (Erstellen eines Windows Server 2012-Failoverclusters).

Für die mehrere Server übergreifende Produktionsbereitstellung können Sie den Netzwerklastenausgleich (Network Load Balancing, NLB) zum Verteilen der Verarbeitungslast verwenden.  Sie sollten auch einen einzelnen Alias (z. B. einen A- oder CNAME-Datensatz) haben, sodass den Benutzern ein einziger gängiger Name angezeigt wird.

>[!IMPORTANT]
> Wenn Sie eine andere Lastenausgleichstechnologie als das NLB-Feature in Windows Server 2012 R2 verwenden, stellen Sie sicher, dass Ihre Lösung eine Sitzung auf dem gleichen Server und nicht auf einen beliebigen Server umleitet.

Bei einer MIM-Bereitstellung mit mehreren Servern hat jeder MIM-Dienst einen externen Hostnamen, einen Dienstnamen und einen Dienstpartitionsnamen.  Der Standardwert des Dienstnamens ist der Name des Computers, und der Standardwert des externen Hostnamens und Dienstpartitionsnamens wird während der Installation des MIM-Diensts auf dem Bildschirm konfiguriert, auf dem nach der Adresse des MIM-Dienstservers gefragt wird. Diese drei Namen werden als Attribute `externalHostName`, `serviceName` und `servicePartitionName` des `resourceManagementService`-Konfigurationsknotens in der Datei „%ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config“ gespeichert.  

Wenn ein MIM-Dienst eine Anforderung empfängt, wird der Dienstpartitionsname als Attribut dieser Anforderung gespeichert.   In der Folge dürfen nur andere MIM-Dienst-Installationen, die den gleichen Dienstpartitionsnamen tragen, mit dieser Anforderung interagieren.  Wenn das PAM-Szenario manuelle Genehmigungen oder andere langlebige Anforderungsverarbeitung umfasst, stellen Sie darum sicher, dass diese Konfigurationsdatei jedes MIM-Diensts das gleiche `servicePartitionName`-Attribut hat.

#### <a name="recovery"></a>Wiederherstellung
Stellen Sie nach einem Ausfall sicher, dass mindestens ein Active Directory-Domänencontroller und eine SQL Server-Instanz vor dem Neustart des MIM-Diensts in der geschützten Umgebung verfügbar ist.  

Eine Workflowinstanz kann nur von einem MIM-Dienstserver ausgeführt werden, der den gleichen Dienstpartitionsnamen und Dienstnamen wie der MIM-Dienstserver trägt, der sie gestartet hat.  Wenn bei einem bestimmten Computer beim Hosten eines MIM-Diensts, der Anforderungen verarbeitete, ein Fehler auftritt, und dieser Computer kehrt nicht wieder in den Dienst zurück, muss der MIM-Dienst auf einem neuen Computer installiert werden. Bearbeiten Sie im neuen MIM-Dienst nach der Installation die Datei *resourcemanagementservice.exe.config*, und legen Sie als Attribute `serviceName` und `servicePartitionName` der neuen MIM-Bereitstellung den Hostnamen und Dienstpartitionsnamen des Computers fest, bei dem der Fehler aufgetreten ist.

### <a name="mim-pam-components"></a>MIM-PAM-Komponenten
Das MIM-Dienst- und Portal-Installationsprogramm umfasst auch zusätzliche PAM-Komponenten, einschließlich PowerShell-Modulen und zwei Diensten.

#### <a name="preparation"></a>Vorbereitung
Die Privileged Access Management-Komponenten sollten auf jedem Computer in der geschützten Umgebung installiert werden, in der der MIM-Dienst installiert wird.  Sie können nicht später hinzugefügt werden.

#### <a name="recovery"></a>Wiederherstellung
Stellen Sie nach der Wiederherstellung nach einem Ausfall sicher, dass der MIM-Dienst auf mindestens einem Server ausgeführt wird.  Stellen Sie dann mithilfe von `net start "PAM Monitoring service"` sicher, dass der MIM-PAM-Überwachungsdienst auch auf diesem Server ausgeführt wird.

Wenn die Gesamtstrukturfunktionsebene der geschützten Umgebung Windows Server 2012 R2 ist, stellen Sie mit dem Befehl `net start "PAM Component service"` sicher, dass der MIM-PAM-Komponentendienst auch auf diesem Server ausgeführt wird.
