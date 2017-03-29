---
title: "Unterstützte Softwareplattformen | Microsoft Docs"
description: Suchen Sie die Produkte und Versionen, die mit allen Komponenten von MIM 2016 kompatibel sind
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 2f2ae09ab8bc28b83b66073985b3574517a220b7
ms.openlocfilehash: bed26316673d777f3d1934011668686b3e9def1d
ms.lasthandoff: 01/13/2017


---

# <a name="supported-platforms-for-mim-2016"></a>Unterstützte Plattformen für MIM 2016

In dieser Tabelle sind die unterstützten Plattformen und Versionen für die einzelnen Komponenten von Microsoft Identity Manager 2016 beschrieben. Die mit einem Sternchen (*) gekennzeichneten Versionen werden nur in MIM 2016 Servicepack 1 unterstützt.


| **MIM-Komponente** | **Plattform** | **Version** |
|-------------------|--------------|-------------|
| **MIM-Synchronisierung** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | MIM-Synchronisationsdatenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory für die Benutzerbereitstellung, PCNS und GAL-Synchronisierung (optional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange für die Bereitstellung von Postfächern und GAL-Synchronisierung (optional)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
| | Entwicklungsumgebung (optional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015  |
| | Zusätzliches verbundenes System (optional) | Active Directory-Domänendienste<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 oder höher<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Andere Produkte von Drittanbietern |
| **MIM-Dienst** (ohne PAM-Szenario) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM-Dienstdatenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange für Bestätigungs- und Gruppenverwaltungs-E-Mails vom MIM-Dienst (optional) | Exchange Server 2007 SP3 (mit installierter Exchange-Verwaltungskonsole)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Online* |
| **MIM-Dienst und -Portal** (nur für PAM-Szenario)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory für die geschützte Umgebung der PAM-Gesamtstruktur | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory für die vorhandene Gesamtstrukturen | Windows Server 2008 <br/> Windows Server 2008 R2 *<br/> Windows Server 2012* <br/> Windows Server 2012 R2 *<br/> Windows Server 2016* |
| | MIM-Dienstdatenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | E-Mail-Server für Bestätigungs- und Gruppenverwaltungs-E-Mails vom MIM-Dienst (optional) | Exchange Server 2007 SP3 (mit installierter Exchange-Verwaltungskonsole)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Online* |
| | Browser | Alle gängigen Browser |
| **MIM-Dienstberichterstellung** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Data Warehouse | System Center 2012 Service Manager SP1 |
| | Data Warehouse-Datenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **MIM-Portale für Kennwortzurücksetzung und Registrierung** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Webbrowser | Alle gängigen Browser |
| **MIM-Add-Ins und -Erweiterungen** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-Integration (optional) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (unter Windows 10) * |
| | PowerShell-Cmdlets für PAM-Requestor (optional) | Windows 8.1<br/>Windows 10 |
| **MIM-Zertifikatverwaltung** (Server- und CA-Integration) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
| | Zertifizierungsstelle | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
| | MIM CM-Datenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
| **MIM-Zertifikatverwaltung** (Anwendung) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM-Zertifikatverwaltung** (Client und Bulk Client) | Windows | Windows 7 |
| **MIM BHOLD-Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
| | BHOLD-Datenbank | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * |
| | E-Mail-Server (optional) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
| | Webbrowser | Internet Explorer 7, 8, 9, 10 oder 11 mit Silverlight |

