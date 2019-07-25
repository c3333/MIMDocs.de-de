---
title: Installation von BHOLD SP1 | Microsoft-Dokumentation
description: Dokumentation zur Installation von BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 05eb2afc0ddbf6104e27a5c24e121a55bd805292
ms.sourcegitcommit: 4c4bc7aa42cd5984c838abdd302490355ddcb4ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238903"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Installationshandbuch für Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) ist eine Sammlung von Anwendungen, die bei der Verwendung mit Microsoft Identity Manager 2016 SP1 (MIM) eine effektive Rollenverwaltung, Analyse und einen effektiven Nachweis zu MIM hinzufügt. Microsoft BHOLD Suite SP1 besteht aus folgenden Modulen:

- BHOLD Core
- Zugriffsverwaltungsconnector
- BHOLD FIM/MIM-Integration
- BHOLD-Modellgenerator
- BHOLD-Analyse
- BHOLD-Berichterstellung
- BHOLD-Nachweis


> [!NOTE]
> **Gilt für:** Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Inhalt dieses Dokuments

Dieses Dokument erläutert die Planung Ihrer BHOLD-Bereitstellung, um den Anforderungen Ihres Unternehmens zu entsprechen und die Installation der einzelnen BHOLD-Module. Die relevante Hardware, Infrastruktur, Softwareanforderungen, die Netzwerkkonfiguration für die Vorinstallation, die während der Setups erforderlichen Informationen und die Schritte vor der Installation werden für jedes Modul detailliert erläutert.

## <a name="pre-requisite-knowledge"></a>Vorwissen

In diesem Dokument wird davon ausgegangen, dass Sie grundlegende Kenntnisse der Installation von Software auf Servercomputern besitzen. Es wird ebenfalls davon ausgegangen, dass Sie grundlegende Kenntnisse zu Active Directory® Domain Services, Microsoft Identity Manager SP1 (MIM) und der Microsoft SQL Server 2012-Datenbanksoftware besitzen. Eine Beschreibung zur Einrichtung und Konfiguration von abhängigen Technologien wie AD DS und FIM ist nicht Gegenstand dieser Dokumentation. Weitere Informationen zu den Funktionen der Microsoft BHOLD-Module finden Sie im [Konzepthandbuch für Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Zielgruppe

Dieses Dokument richtet sich an IT-Planer, Systemarchitekten, Entscheidungsträger für Technologien, Berater, Infrastrukturplaner und IT-Personal, die Microsoft BHOLD Suite bereitstellen möchten.

## <a name="bhold-infrastructure-considerations"></a>Infrastrukturüberlegungen für BHOLD

In den meisten Fällen werden BHOLD und FIM in einer großen Infrastrukturumgebung verwendet. Sie können Ihre BHOLD- und FIM-Architektur Ihren Geschäftsanforderungen entsprechend anpassen. Die folgenden Abschnitte enthalten mögliche Lösungen für die Architektur. Diese Übersicht ist keine umfassende Liste aller möglichen Optionen, sondern stellt Vorschläge für die Bereitstellung von BHOLD in Ihrem Netzwerk bereit.
 
In diesem Abschnitt werden folgende Themen behandelt:

- Architektur mit einem Server
- Architektur mit zwei Servern
- Architektur mit zwei Ebenen
- SQL Server-Empfehlungen

### <a name="single-server-architecture"></a>Architektur mit einem Server

Für die Bereitstellung in kleinen Unternehmen oder für Entwicklungszwecke können Sie BHOLD und FIM wie in der folgenden Abbildung dargestellt auf demselben Server wie SQL Server und AD DS installieren.
 
![Architektur mit einem Server](media/bhold-installation-guide/single.png)

Wenn BHOLD Suite SP1 und das FIM-Portal zusammen auf einem einzelnen Server installiert sind, müssen Sie für BHOLD und FIM unterschiedliche Hostaliase (CNAME- oder A-Datensätze) in DNS erstellen. Dadurch können separate Dienstprinzipalnamen (Server Principal Names, SPNs) für die BHOLD- und FIM-Dienste erstellt werden. Weitere Informationen finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Weitere Informationen zum Installieren von FIM in einer Einzelserverkonfiguration finden Sie unter [Common Configuration for Getting Started Guides (Häufige Konfigurationen für Handbücher mit ersten Schritten)](https://technet.microsoft.com/library/ff575965.aspx) in der Microsoft TechNet-Bibliothek.

### <a name="dual-server-architecture"></a>Architektur mit zwei Servern

Das Installieren von BHOLD Core und FIM auf separaten Servern bietet mehr Leistung und Flexibilität für Organisationen von mittlerer Größe, die keine noch komplexere Bereitstellung (z.B. eine Architektur mit mehreren Ebenen) benötigen. Die folgende Abbildung veranschaulicht, dass BHOLD und FIM jeweils auf einem eigenen Server installiert sind. Auf dem FIM-Server wird ebenfalls SQL Server ausgeführt, um Datenbankdienste für BHOLD und FIM bereitzustellen. Der FIM-Synchronisierungsdienst, der auf dem FIM-Server ausgeführt wird, wechselt zwischen den FIM- und BHOLD-Datenbanken. Beachten Sie, dass das BHOLD FIM-Integrationsmodul auf demselben Server wie der FIM-Dienst und das FIM-Portal installiert sein muss, wenn Self-Service für Benutzer erforderlich ist. Das BHOLD FIM-Integrationsmodul erfordert, dass der FIM-Dienst und das BHOLD FIM-Integrationsmodul auf demselben Server installiert sind.

![Architektur mit zwei Servern](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> Die Berichterstellungsfunktion des BHOLD FIM-Integrationsmoduls erfordert, dass die BHOLD- und FIM-Datenbanken auf derselben Instanz von SQL Server installiert sind. Außerdem muss das BHOLD-Dienstkonto über die Zugriffsrechte für die FIM-Dienstdatenbank verfügen.

### <a name="two-tier-architecture"></a>Architektur mit zwei Ebenen

In den meisten Umgebungen, insbesondere in denen, für die Leistung wichtig ist, sollten Sie BHOLD Suite SP1, FIM und SQL Server auf separaten Servern ausführen (Architektur mit zwei Ebenen). Bei einer Architektur mit zwei Ebenen sind Arbeitsspeicher- und CPU-Ressourcen für jede Ebene fest zugeordnet. Die folgende Abbildung stellt einen möglichen Weg dar, um eine Architektur mit zwei Ebenen zu konfigurieren. Der FIM-Synchronisierungsdienst, der auf dem FIM-Server ausgeführt wird, wechselt zwischen den FIM- und BHOLD-Datenbanken. Beachten Sie, dass das BHOLD FIM-Integrationsmodul auf demselben Server wie der FIM-Dienst und das FIM-Portal installiert sein muss, wenn Self-Service für Benutzer erforderlich ist.

![Architektur mit zwei Ebenen](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server-Empfehlungen

Wenn Sie BHOLD für ein großes Unternehmen bereitstellen, wird dringend empfohlen, diese Richtlinien für das Einrichten der Microsoft SQL Server-Datenbank zu befolgen:

- Stellen Sie SQL Server auf einem anderen Server als die FIM- oder BHOLD-Dienste bereit.
- Isolieren Sie die Protokolldatei von der Datendatei auf Ebene des physischen Datenträgers.
- Wenn Sie RAID verwenden, um Speicherredundanz bereitzustellen, verwenden Sie RAID-Stufe 10 (1+0). Verwenden Sie nicht RAID-Stufe 5.
- Achten Sie darauf, die richtigen Einstellungen zu konfigurieren, wenn Sie mehr als 2 GB physischen Arbeitsspeicher für den Server verwenden, auf dem SQL Server ausgeführt wird.

Weitere Informationen zu den bewährten Methoden für SQL Server finden Sie unter [Storage Top 10 Best Practices (Top 10 der bewährten Speichermethoden)](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) in der Microsoft TechNet-Bibliothek.

### <a name="trusted-certificates-list-update"></a>Aktualisieren der Liste der vertrauenswürdigen Zertifikate

Windows kann so konfiguriert werden, dass Zertifikatketten vor dem Starten eines Diensts überprüft werden. Auf solchen Systemen kann ein Dienst nicht starten, wenn der ausführbare Code des Diensts mit einem Zertifikat signiert wurde, das sich nicht in der Liste vertrauenswürdiger Zertifikate (Trusted Certificate List, TCL) des Servers befindet. Die Microsoft BHOLD Suite SP1-Software verfügt über Code, der mit einer Zertifikatkette für die Codesignatur signiert wurde, die ihren Ursprung in einem Zertifikat der Microsoft-Stammzertifizierungsstelle 2010 hat.
Windows kann so konfiguriert werden, dass Stammzertifikate von Microsoft über eine Internetverbindung angerufen werden. Auf einem nicht verbundenen System enthält Windows Server jedoch nur die Zertifikate, die im Stammprogramm vorhanden waren, bevor Windows Server veröffentlicht wurde. In den Releases von Windows Server vor Windows Server 2010 enthalten diese Zertifikate nicht das Stammzertifikat, das für das Überprüfen der BHOLD Suite SP1-Zertifikatkette für die Codesignatur erforderlich ist. Wenn Sie ein oder mehrere Microsoft BHOLD Suite SP1-Module auf einem System installieren möchten, das nicht über eine aktuelle TCL verfügt, müssen Sie vor der Installation eines BHOLD Suite SP1-Moduls das Aktualisierungspaket für den Stamm herunterladen und installieren oder die Gruppenrichtlinie verwenden, um das Aktualisierungsprogramm für den Stamm zu installieren. Weitere Informationen finden Sie unter [Mitglieder des Microsoft-Programms für Stammzertifikate](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Erforderliche Schritte für die Installation von BHOLD Suite SP1 unter Windows Server 2012/2016 

![Installieren von BHOLD mithilfe von IIS](media/bhold-installation-guide/iis-install-bhold.png)

Wenn Sie BHOLD Suite SP1 unter Windows Server 2012 oder 2016 installieren, sind die BHOLD-Webseiten nicht verfügbar, bis Sie die Datei „applicationHost.config“ ändern, die sich in ```C:\Windows\System32\inetsrv\config``` befindet. Fügen Sie ```preCondition="bitness64``` im Abschnitt ```<globalModules>``` zu dem Eintrag hinzu, der ```<add name="SPNativeRequestModule"``` beginnt, sodass der Code folgendermaßen lautet:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Führen Sie nach dem Bearbeiten und Speichern der Datei den Befehl „iisreset“ aus, um den IIS-Server zurückzusetzen.


## <a name="upgrading-bhold-suite"></a>Aktualisieren von BHOLD Suite

Sie können eine vorhandene Installation von BHOLD Suite nicht aktualisieren. Stattdessen müssen Sie die vorhandene Installation von BHOLD Suite deinstallieren, bevor Sie BHOLD-Module aktualisieren können. Wenn Sie über ein vorhandenes BHOLD-Rollenmodell verfügen, können Sie die BHOLD-Datenbank aktualisieren und diese verwenden, wenn Sie das aktualisierte BHOLD Core-Modul installieren. Weitere Informationen finden Sie unter [Replacing BHOLD Suite with BHOLD Suite SP1 (Ersetzen von BHOLD Suite durch BHOLD Suite SP1)](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Nächste Schritte

- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)
