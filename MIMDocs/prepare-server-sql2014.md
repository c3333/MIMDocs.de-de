---
title: "Konfigurieren von SQL Server für Microsoft Identity Manager 2016 | Microsoft-Dokumentation"
description: "Installieren von SQL Server 2014 als Vorbereitung für die Installation von MIM 2016."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 105d2320ed5a0d610e8e6c5f459366680e3f8a77
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="set-up-an-identity-management-server-sql-server-2014"></a>Einrichten eines Identitätsverwaltungsservers: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Domänencontrollername: **mimservername**
> - Domänenname: **contoso**
> - Kennwort – **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>Installieren von **SQL Server 2014 Standard Edition**

1. Starten Sie **PowerShell** als Domänenadministrator.

2. Navigieren Sie in das Verzeichnis, in dem sich das SQL Server-Setupprogramm befindet.

3. Geben Sie die folgenden Befehle ein.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)
