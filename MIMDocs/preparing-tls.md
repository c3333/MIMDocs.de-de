---
title: Planen von Microsoft Identity Manager 2016 in der TLS 1.2-Umgebung | Microsoft-Dokumentation
description: Planen von Microsoft Identity Manager 2016 in der TLS 1.2-Umgebung
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e208e2f42c206ae50febefb6a8206dc3823e084
ms.sourcegitcommit: c9f5f960fd39745bf5b57161a2fd0238c88d035a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133531"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>Planen von MIM 2016 SP2 in den TLS 1.2- oder FIPS-Modus-Umgebungen


> [!IMPORTANT]
> Dieser Artikel gilt nur für MIM 2016 SP2.

Bei der Installation von MIM 2016 SP2 in der gesperrten Umgebung, in der alle Verschlüsselungsprotokolle außer TLS 1.2 deaktiviert sind, gelten die folgenden Anforderungen:
- Für das Einrichten einer sicheren TLS 1.2-Verbindung sind für die MIM-Komponenten die neuesten Updates für Windows Server und .NET Framework erforderlich, die die Installation der TLS 1.2-Unterstützung in .NET 3.5 Framework ermöglichen. Abhängig von der Serverkonfiguration müssen Sie *möglicherweise*[SystemDefaultTlsVersions für .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) aktivieren und/oder [alle SCHANNEL-Protokolle außer TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) in den Unterschlüsseln *Client* und *Server* der Registrierung deaktivieren.

## <a name="mim-synchronization-service-sql-ma"></a>MIM-Synchronisierungsdienst, SQL-Verwaltungs-Agent

- Für das Einrichten einer sicheren TLS 1.2-Verbindung mit SQL Server benötigen der MIM-Synchronisierungsdienst und der integrierte SQL-Verwaltungs-Agent [SQL Server Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) oder höher.

## <a name="mim-service"></a>MIM-Dienst
   >[!NOTE]
   >Bei der unbeaufsichtigten Installation von MIM 2016 SP2 tritt in Umgebungen ein Fehler auf, in denen nur TLS 1.2 verwendet wird. Installieren Sie den MIM-Dienst im interaktiven Modus, oder aktivieren Sie TLS 1.1, wenn Sie die Installation unbeaufsichtigt durchführen möchten. Erzwingen Sie TLS 1.2 nach Abschluss der unbeaufsichtigten Installation bei Bedarf.

- Selbstsignierte Zertifikate können in einer reinen TLS 1.2-Umgebung nicht vom MIM-Dienst verwendet werden. Wählen Sie ein Zertifikat mit starker Verschlüsselung, das von der vertrauenswürdigen Zertifizierungsstelle beim Installieren des MIM-Diensts ausgestellt wurde.
- Das Installationsprogramm des MIM-Diensts erfordert zusätzlich [OLE DB-Treiber für SQL Server Version 18.2](https://www.microsoft.com/download/details.aspx?id=56730) oder höher.

## <a name="fips-mode-considerations"></a>Überlegungen zum FIPS-Modus

Wenn Sie einen MIM-Dienst auf einem Server installieren, auf dem der FIPS-Modus aktiviert ist, müssen Sie die Überprüfung von FIPS-Richtlinien deaktivieren, damit Workflows des MIM-Diensts ausgeführt werden können. Fügen Sie daher wie unten dargestellt im Abschnitt *Runtime* der Datei *Microsoft.ResourceManagement.Service.exe.config* zwischen den Abschnitten *Runtime* und *assemblyBinding* das Element *enforceFIPSPolicy enabled=false* ein:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    
