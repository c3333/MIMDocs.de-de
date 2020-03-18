---
title: Upgrade von FIM 2010 R2 und MIM 2016 SP2 auf Microsoft Identity Manager 2016 Service Pack 2 | Microsoft-Dokumentation
description: Informieren Sie sich, wie Sie die FIM 2010 R2- oder MIM 2016 SP2-Komponenten aktualisieren und daraufhin die neuen MIM 2016-Komponenten installieren.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bfe604795d44cdea61fe0d10bc2943f9b8433784
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044145"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>MIM 2016 SP2-Upgrade von Forefront Identity oder Microsoft Identity Manager

Organisationen können von früheren Versionen von Microsoft Identity Manager oder Forefront Identity Manager auf Microsoft Identity Manager 2016 SP2 aktualisieren.  In jedem Abschnitt dieses Artikels wird ein unterstützter Upgradepfad behandelt.

Es sind mehrere Upgradeoptionen verfügbar. Wenn Sie bereits MIM 2016 ausführen und die zugrunde liegende Plattform (Windows Server, SQL, SharePoint, SCSM DW) nicht aktualisieren müssen oder MIM-Dienste mithilfe von gruppenverwalteten Dienstkonten ausführen und keine MIM-Sprachpakete verwenden, bietet ein direktes Upgrade oder eine Hotfix-Installation (.msp) die einfachste Option. Andernfalls wird die vollständige Installation empfohlen.

> [!IMPORTANT]
> Lesen Sie vor der Installation von MIM 2016 SP2 die aktualisierten [Softwareanforderungen](prepare-server-ws2016.md#software-prerequisites).

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Upgrade von FIM 2010 R2 SP1 oder höheren FIM-Builds

> [!NOTE]
> Die minimale unterstützte Version von Forefront Identity Manager, die direkt auf MIM 2016 SP2 aktualisiert werden kann, ist FIM 2010 R2 SP1 (Build 4.1.3419.0). Das direkte Upgrade von früheren Versionen von FIM auf MIM 2016 SP2 wird nicht unterstützt. Wenn Sie FIM-Builds vor Version 4.1.3419.0 ausführen, müssen Sie vor dem Upgrade auf MIM 2016 SP2 auf FIM 2010 R2 SP1 aktualisieren.

1. **Option 1: Vollständige Installation mithilfe vorhandener Datenbanken**
    1. Erstellen Sie eine Sicherungskopie der FIMSynchronizationService- und FIMService-Datenbanken.
    1. Exportieren Sie alle RCDC-Objekte des FIM-Diensts und RCDC-Ressourcenzeichenfolgen, die Sie geändert haben.
    1. Exportieren Sie Verschlüsselungsschlüssel für den Synchronisierungsdienst.
    1. Erstellen Sie Sicherungskopien von miisserver.exe.config, dem Ordner „Erweiterungen“ des Synchronisierungsservers und Microsoft.ResourceManagement.Service.exe.config, da das MIM-Installationsprogramm möglicherweise benutzerdefinierte Änderungen an Konfigurationsdateien überschreibt.
    1. Deinstallieren Sie **alle** FIM-Komponenten einschließlich Sprachpakete (diese müssen zuerst deinstalliert werden).
    1. *Optional:* Verschieben Sie FIM-Datenbanken auf einen anderen SQL-Server. Es wird empfohlen, SQL-Aliase für MIM-Server zu erstellen und diese anstelle des tatsächlichen SQL Server-Namens zu verwenden, um die Migration der MIM-Datenbanken in Zukunft zu vereinfachen.
    1. Installieren Sie MIM 2016 SP2 gemäß dem [MIM-Bereitstellungsleitfaden](microsoft-identity-manager-deploy.md) von den ISO-Installationsmedien auf dem gleichen oder einem anderen Server, wählen Sie, wenn Sie dazu aufgefordert werden, die Option zum Verwenden vorhandener Datenbanken aus, und stellen Sie die zuvor exportierten Verschlüsselungsschlüssel des Synchronisierungsdiensts bereit.
    1. Überprüfen Sie miisserver.exe.config und Microsoft.ResourceManagement.Service.exe.config-Dateien auf fehlende .NET-Umleitungen oder benutzerdefinierte Abschnitte, die Sie hinzugefügt haben.
    1. Installieren Sie bei Bedarf MIM 2016 SP2-Sprachpakete.
    1. Übermitteln Sie bei Bedarf erneut Anpassungen an RCDC-Objekten und RCDC-Ressourcenzeichenfolgen und -lokalisierungen.
    1. Aktualisieren Sie FIM-Add-Ons und Kennwortzurücksetzungs-Clients, und geben Sie einen neuen Servernamen für den MIM-Dienst an, wenn der Servername des MIM-Diensts geändert wurde.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Upgrade von früheren MIM 2016-Builds
1. Erstellen Sie eine Sicherungskopie der FIMSynchronizationService- und FIMService-Datenbanken.
1. Exportieren Sie alle benutzerdefinierten FIM-Dienstlokalisierungen, die an RCDC-Objekten und RCDC-Ressourcenzeichenfolgen vorgenommen werden.
1. Exportieren Sie Verschlüsselungsschlüssel für den Synchronisierungsdienst.
1. Erstellen Sie Sicherungskopien von miisserver.exe.config, dem Ordner „Erweiterungen“ des Synchronisierungsservers und Microsoft.ResourceManagement.Service.exe.config, da das MIM-Installationsprogramm möglicherweise benutzerdefinierte Änderungen an Konfigurationsdateien überschreibt.
1. Deinstallieren Sie bei Bedarf MIM-Sprachpakete.
1. Beenden Sie MIM-Dienste.
1. **Option 1: Direktes Upgrade: Hotfixinstallation**
    1. Wenden Sie den [Hotfix](https://www.microsoft.com/download/details.aspx?id=100412) des Synchronisierungsdiensts für MIM 2016 SP2 an.
    1. Wenden Sie den [Hotfix](https://www.microsoft.com/download/details.aspx?id=100412) des MIM 2016 SP2-Diensts an.
    1. Überprüfen Sie miisserver.exe.config und Microsoft.ResourceManagement.Service.exe.config-Dateien auf fehlende .NET-Umleitungen oder benutzerdefinierte Abschnitte, die hinzugefügt werden müssen.
    1. Installieren Sie bei Bedarf MIM 2016 SP2-Sprachpakete.
    1. Übermitteln Sie bei Bedarf erneut Anpassungen an RCDC-Objekten und RCDC-Ressourcenzeichenfolgen und -lokalisierungen.
    1. Aktualisieren Sie MIM 2016-Add-Ons und Kennwortzurücksetzungs-Clients.
1. **Option 2: Vollständige Installation mithilfe vorhandener Datenbanken**
    1. Deinstallieren Sie **alle** MIM-Komponenten.
    1. *Optional:* Verschieben Sie FIM-Datenbanken auf einen anderen SQL-Server. Es wird empfohlen, SQL-Aliase für MIM-Server zu erstellen und diese anstelle des tatsächlichen SQL Server-Namens zu verwenden, um die Migration der MIM-Datenbanken in Zukunft zu vereinfachen.
    1. Installieren Sie MIM 2016 SP2 gemäß dem [MIM-Bereitstellungsleitfaden](microsoft-identity-manager-deploy.md) von den ISO-Installationsmedien auf dem gleichen oder einem anderen Server, wählen Sie, wenn Sie dazu aufgefordert werden, die Option zum Verwenden vorhandener Datenbanken aus, und stellen Sie die zuvor exportierten Verschlüsselungsschlüssel des Synchronisierungsdiensts bereit.
    1. Überprüfen Sie miisserver.exe.config und Microsoft.ResourceManagement.Service.exe.config-Dateien auf fehlende .NET-Umleitungen oder benutzerdefinierte Abschnitte, die hinzugefügt werden müssen.
    1. Installieren Sie bei Bedarf MIM 2016 SP2-Sprachpakete.
    1. Übermitteln Sie bei Bedarf erneut Anpassungen an RCDC-Objekten und RCDC-Ressourcenzeichenfolgen und -lokalisierungen.
    1. Aktualisieren Sie MIM 2016-Add-Ons und Kennwortzurücksetzungs-Clients, und geben Sie einen neuen Servernamen für den MIM-Dienst an, wenn der Servername des MIM-Diensts geändert wurde.

> [!NOTE]
> Sprachpaketupdates nach MIM 2016 SP2 werden als Hotfixes (MSP-Dateien) verteilt, sodass Sprachpakete nicht deinstalliert/neu installiert werden müssen.

Weitere Informationen zum Upgrade und den Vorgängen für die Datenbanksicherung finden Sie im Artikel [Upgrade auf FIM 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29), der für alle FIM- und MIM-Upgradevorgänge gilt.
