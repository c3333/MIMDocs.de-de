---
title: Konfigurieren von SQL Server für Microsoft Identity Manager 2016 SP2 | Microsoft-Dokumentation
description: Installieren von SQL Server 2016 oder 2017 als Vorbereitung für die Installation von MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4be699f123bf7d48b709ee8b8e91e2222cd492e2
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "73568025"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Einrichten eines Identitätsverwaltungsservers: SQL Server 2016 oder 2017

> [!div class="step-by-step"]
> [« Windows Server](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
 
> [!NOTE] 
> Der SQL Server 2017-Setupvorgang unterscheidet sich nicht von dem für SQL Server 2016.

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Name des Domänencontrollers: **corpdc**
> - Domänenname: **contoso**
> - Servername des MIM-Diensts: **corpservice**
> - Servername der MIM-Synchronisierung: **corpsync**
> - SQL-Servername: **corpsql**
> - Kennwort – <strong>Pass@word1</strong>

> [!IMPORTANT]
> MIM 2016 SP2 unterstützt SQL Always On-Verfügbarkeitsgruppenlistener (AoAG), bei denen die Option *RegisterAllProvidersIP* auf „0“ festgelegt ist. Das bedeutet, dass das subnetzübergreifende SQL Server Always On-Verfügbarkeitsgruppenfailover derzeit nicht unterstützt wird.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Installieren Sie **SQL Server 2016 Standard oder Enterprise Edition**.

1. Starten Sie **PowerShell** als Domänenadministrator.

2. Navigieren Sie in das Verzeichnis, in dem sich das SQL Server-Setupprogramm befindet.

3. Geben Sie die folgenden Befehle ein.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Weitere Informationen zu SQL- Bereitstellungskonten und -Diensten finden Sie [hier](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017).

> [!NOTE]
> SSMS ist nicht mehr in der SQL 2016 enthalten. Details zum Herunterladen finden Sie [hier](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

> [!div class="step-by-step"]  
> [« Windows Server](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
