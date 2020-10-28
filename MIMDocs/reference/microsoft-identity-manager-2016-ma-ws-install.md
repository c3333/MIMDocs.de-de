---
title: MIM Installieren des Webdienst-Codiertools | Microsoft-Dokumentation
description: In diesem Artikel werden die Schritte zum Installieren des Webdienst-Konfigurationstools beschrieben.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4b9d2463ea30839c2ea4e2a3427d057c925183e8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760539"
---
# <a name="install-the-web-service-configuration-tool"></a>Installieren des Webdienst-Konfigurationstools

Der webdienstconnector und die Standardprojekte sind im [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495)verfügbar.

Die **MSI des webdienstconnector** stellt zwei Funktionen bereit:

- Web Service Connector Runtime: installiert den kernconnector, die Connector-Abhängigkeiten und den paketconnector.
- Webdienst-Konfigurationstool: installiert das Webdienst-Konfigurationstool.

![Connector-Optionen des Installations-Assistenten](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

Das-Konfigurationstool kann installiert werden, ohne dass der Synchronisierungs Dienst installiert ist. Dies ermöglicht die Konfiguration auf einem separaten Computer.

## <a name="default-projects"></a>Standardprojekte

Weitere Standardprojekte werden mit dem webdienstconnector ausgeliefert. Diese sind als selbst extrahierte exe-Dateien verfügbar. Sie können das webdienstconnector-Projekt entsprechend Ihren Anforderungen herunterladen.

Nach Abschluss der Installation werden die unterschiedlichen Komponenten mit den zugehörigen Binärdateien im Unterordner des Ordners auf Ihrem System installiert.

| Inhalte | Standort |
|---|---|
| Laufzeit des webdienstconnector           | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010- \\ Synchronisierungs &nbsp; Dienst \\ Erweiterungen |
| Webdienstconnector-Projekt           | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010- \\ Synchronisierungs &nbsp; Dienst \\ Erweiterungen |
| Paketierten Connector                      | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010- \\ Synchronisierungs &nbsp; Dienst \\ uiShell \\ XMLs \\ packagedmas |
| Webdienst-Konfigurationstool          | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010- \\ Synchronisierungs &nbsp; Dienst- \\ uiShell- \\ &nbsp; Webdienst &nbsp; Konfiguration <br/>**Hinweis** : Dies ist der Standard Speicherort für die Installation. Dieser Speicherort kann während der Installation geändert werden. |
| Webdienst-Projektdatei                | Der Benutzer kann einen beliebigen Zielordner auswählen, in den die Datei extrahiert werden soll, aber die extrahierte Projektdatei (. Wsconfig) ist nur für die FIM-Synchronisierungs Benutzeroberfläche sichtbar, wenn die Projektdatei in den FIM- **Erweiterungs** Ordner extrahiert wird. Die extrahierte Projektdatei ist für das Webdienst-Konfigurationstool an einem beliebigen Speicherort sichtbar. |


## <a name="additional-permissions"></a>Zusätzliche Berechtigungen

Die Projektdatei kann an einem beliebigen Speicherort gespeichert und geöffnet werden (mit den entsprechenden Zugriffsberechtigungen Ihres Executor). Allerdings können nur Projektdateien, die im Ordner gespeichert werden, `Synchronization Service\Extension` im Assistenten für den webdienstconnector ausgewählt werden, auf den über die FIM-Synchronisierungs-Benutzeroberfläche zugegriffen wird.

Der Benutzer, der das Webdienst-Konfigurations Tool ausführen muss, benötigt die folgenden Berechtigungen:

- Lese-/Schreibberechtigungen für den Synchronisierungs Dienst-Erweiterungs Ordner.
- Lesezugriff auf den Registrierungsschlüssel **HKLM \\ System \\ CurrentControlSet \\ Services \\ fimsynchronizationservice- \\ Parameter** .
