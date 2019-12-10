---
title: Unterstützte Softwareplattformen | Microsoft Docs
description: Suchen Sie die Produkte und Versionen, die mit allen Komponenten von MIM 2016 kompatibel sind
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: f69566c9d6abc6b7c54cc875a958b66a112c3111
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "73329356"
---
# <a name="supported-platforms-for-mim-2016"></a>Unterstützte Plattformen für MIM 2016

In dieser Tabelle sind die unterstützten Plattformen und Versionen für die einzelnen Komponenten von Microsoft Identity Manager 2016 beschrieben. Die mit einem Sternchen (*) gekennzeichneten Versionen werden nur in MIM 2016 Servicepack 1 unterstützt. Die mit zwei Sternchen (**) gekennzeichneten Versionen werden nur in MIM 2016 Servicepack 2 oder einem späteren Hotfix unterstützt. Die mit „NR“ markierten Versionen (Not Recommended, nicht empfohlen) werden unterstützt, jedoch nicht empfohlen, wenn eine neue Bereitstellung dieser Plattform für MIM gestartet wird.


| **MIM-Komponente** | **Plattform** | **Version** |
|-------------------|--------------|--------------|
| **MIM-Synchronisierung** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 ** |
| | Active Directory-Funktionsebene für die Benutzerbereitstellung, PCNS und GAL-Synchronisierung | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM-Synchronisationsdatenbank | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/>SQL Server 2016 SP2 *<br/>SQL Server 2017 ** |
| | Active Directory für die Benutzerbereitstellung, PCNS und GAL-Synchronisierung (optional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Exchange für die Bereitstellung von Postfächern und GAL-Synchronisierung (optional)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Exchange Server 2019 ** |
| | Entwicklungsumgebung (optional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Zusätzliches verbundenes System (optional) | Active Directory-Domänendienste<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 oder höher<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> SharePoint Server 2019 ** <br/> Andere Produkte von Drittanbietern |
| **MIM-Dienst und -Portal** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| |PAM-Szenario:  Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |PAM-Szenario: Active Directory für die geschützte Umgebung der PAM-Gesamtstruktur | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |PAM-Szenario: Active Directory für vorhandene PAM-Szenario-Gesamtstrukturen (CORP) | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM-Dienstdatenbank | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 *<br/> SharePoint 2019 ** |
| | E-Mail-Server für Bestätigungs- und Gruppenverwaltungs-E-Mails vom MIM-Dienst (optional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Server 2019 ** <br/> Exchange Online * (nur Benachrichtigung vor Build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Alle gängigen unterstützten Browser * (auf mobile Geräte begrenzt)|
| **MIM-Dienstberichterstellung** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Data Warehouse | System Center 2012 – Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager (Mit 4.4.1459)<br/> System Center 2019 Service Manager ** |
| **MIM-Portale für Kennwortzurücksetzung und Registrierung** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Webbrowser | Alle gängigen unterstützten Browser |
| **MIM-Add-Ins und -Erweiterungen** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-Integration (optional) | Outlook 2010 (unter Windows, mit Ausnahme von Klick-und-Los)<br/>Outlook 2013 (unter Windows, mit Ausnahme von Klick-und-Los) <br/> Outlook 2016 (unter Windows 10, mit Ausnahme von Klick-und-Los) *<br/>Office 365 Outlook (unter Windows 10, einschließlich Klick-und-Los) ** |
| | PowerShell-Cmdlets für PAM-Requestor (optional) | Windows 8.1<br/>Windows 10 |
| **MIM-Zertifikatverwaltung** (Server- und CA-Integration) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Zertifizierungsstelle | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | MIM CM-Datenbank | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** |
| **MIM-Zertifikatverwaltung** (Anwendung) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM-Zertifikatverwaltung** (Sammelclient) | Windows | Windows 7 |
| **MIM-Zertifikatverwaltung** (Auf ActiveX basierende Clientsmartcard) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD-Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-Datenbank | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | E-Mail-Server (optional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbrowser | Von Internet Explorer unterstützte Browser mit Silverlight |
