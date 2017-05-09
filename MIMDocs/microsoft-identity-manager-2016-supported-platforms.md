---
title: "Unterstützte Softwareplattformen | Microsoft Docs"
description: Suchen Sie die Produkte und Versionen, die mit allen Komponenten von MIM 2016 kompatibel sind
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f1faed3a09023e288a68a8950dae43725f19eb3e
ms.openlocfilehash: 33c84afa4d6fd2ed7bde33de39dd151f83f07fd4
ms.lasthandoff: 04/12/2017


---

# <a name="supported-platforms-for-mim-2016"></a>Unterstützte Plattformen für MIM 2016

In dieser Tabelle sind die unterstützten Plattformen und Versionen für die einzelnen Komponenten von Microsoft Identity Manager 2016 beschrieben. Die mit einem Sternchen (*) gekennzeichneten Versionen werden nur in MIM 2016 Servicepack 1 unterstützt.


| **MIM-Komponente** | **Plattform** | **Version** |
|-------------------|--------------|-------------|
| **MIM-Synchronisierung** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | MIM-Synchronisationsdatenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory für die Benutzerbereitstellung, PCNS und GAL-Synchronisierung (optional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange für die Bereitstellung von Postfächern und GAL-Synchronisierung (optional)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Entwicklungsumgebung (optional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Zusätzliches verbundenes System (optional) | Active Directory-Domänendienste<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 oder höher<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Andere Produkte von Drittanbietern |
| **MIM-Dienst** (ohne PAM-Szenario) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM-Dienstdatenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange für Bestätigungs- und Gruppenverwaltungs-E-Mails vom MIM-Dienst (optional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (nur Benachrichtigung) |
| **MIM-Dienst und -Portal** (nur für PAM-Szenario)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory für die geschützte Umgebung der PAM-Gesamtstruktur | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory für die vorhandene Gesamtstrukturen | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM-Dienstdatenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | E-Mail-Server für Bestätigungs- und Gruppenverwaltungs-E-Mails vom MIM-Dienst (optional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (nur Benachrichtigung) |
| | Browser | Alle gängigen Browser |
| **MIM-Dienstberichterstellung** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Data Warehouse | System Center 2012 – Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager (Mit 4.4.1459)<br/> [Kompatibilität der SQL Server-Version für System Center 2016](https://technet.microsoft.com/en-us/system-center-docs/system-requirements/sql-server-version-compatibility)
 |
| **MIM-Portale für Kennwortzurücksetzung und Registrierung** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Webbrowser | Alle gängigen Browser |
| **MIM-Add-Ins und -Erweiterungen** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-Integration (optional) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (unter Windows 10) * |
| | PowerShell-Cmdlets für PAM-Requestor (optional) | Windows 8.1<br/>Windows 10 |
| **MIM-Zertifikatverwaltung** (Server- und CA-Integration) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Zertifizierungsstelle | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM-Datenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM-Zertifikatverwaltung** (Anwendung) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM-Zertifikatverwaltung** (Client und Bulk Client) | Windows | Windows 7 |
| **MIM BHOLD-Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-Datenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | E-Mail-Server (optional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbrowser | Internet Explorer 7, 8, 9, 10 oder 11 mit Silverlight |

