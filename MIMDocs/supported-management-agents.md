---
title: Unterstützte Connectors | Microsoft Docs
description: Verwenden Sie Connectors, um die Datenübertragung zwischen MIM und Ihren verknüpften Datenquellen zu verwalten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 023232b9ddb3cb0a299cbc14ab4c311b8c63fc47
ms.sourcegitcommit: fa30a8eb9c3a7f1ed6f8ce0f67362ca32751e00d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56667198"
---
# <a name="connect-to-your-directories"></a>Herstellen der Verbindung zu Ihren Verzeichnissen

Connectors verknüpfen bestimmte verbundene Datenquellen mit Microsoft Identity Manager SP1 (MIM). Ein Connector verschiebt Daten aus einer verbundenen Datenquelle nach MIM. Werden Daten in MIM geändert, kann der Connector die Daten auch in die verbundene Datenquelle exportieren, um diese mit MIM zu synchronisieren. In der Regel gibt es mindestens einen Connector für jedes verbundene Verzeichnis.

In Forefront Identity Manager wurden Connectors als Verwaltungs-Agents bezeichnet. Dieser Begriff wird in einigen Artikeln oder Teilen des Produkts noch verwendet, beachten Sie aber, dass beide Begriffe sich auf das gleiche Konzept beziehen.

Dieser Artikel behandelt die in MIM enthaltenen und unterstützte Connectors, aber mit dem Connector für Extensible Connectivity 2.0 können Sie sogar Verbindungen zu noch mehr Datenquellen herstellen. Einige Partner haben auf diese Weise ihre eigenen Connectors erstellt. Eine vollständige Liste finden Sie im Wiki [FIM 2010: Management Agents from Partners (Verwaltungs-Agents von Partnern)](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Unterstützte Connectors in MIM 2016 SP1

| Name | Unterstützte Versionen der verbundenen Datenquelle und technische Links |
| ---- | ----------------------------------------------- |
| Active Directory-Domänendienste | Active Directory in Windows Server 2012 und Windows Server 2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory-GAL (globale Adressliste) | Active Directory-GAL (globale Adressliste) – Exchange 2013, Exchange 2016 |
| Extensible Connectivity 2.0 | Jede aufruf- oder dateibasierte Datenquelle |
| FIM-Dienst | MIM-Dienst. Beachten Sie, dass die Versionen von MIM-Synchronisierungsdienst und MIM-Dienst identisch sind. |
| IBM DB2 Universal Database | IBM DB2, Versionen 9.5 oder 9.7; IBM DB2 OLEDB v9.5 FP5 oder v9.7 FP1 |
| IBM-Verzeichnisserver | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory, Versionen 8.7.3, 8.8.5 und 8.8.6 |
| Oracle-Datenbank | Oracle Database 10g oder 11g; 64-Bit-Client |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Verzeichnisserver für Oracle (zuvor Sun und Netscape) | Sun Directory Server 6.x, 7.x und Oracle 11 |
| [Windows PowerShell-Connector für FIM 2010 R2 – technische Referenz](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 oder höher |
| [Microsoft Azure Active Directory Connector](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (nicht für neue Bereitstellungen empfohlen) |
| [Generischer LDAP-Connector für FIM 2010 R2 – technische Referenz](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-Server (RFC 4510-konform)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Generischer SQL-Connector](./reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Der Connector wird mit allen 64-Bit-ODBC-Treibern unterstützt](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql.md) |
| [Connector für Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [SharePoint Services-Connector-UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013 oder 2016 mit Benutzerprofildienst-Anwendung (UPA) |
| [Connector für Webdienste](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 oder 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 und andere SOAP- und REST-APIs](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Attribut-Wert-Paar-Textdatei](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Attribut-Wert-Paar-Textdateien |
| [Textdatei mit Trennzeichen](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Textdateien mit Trennzeichen |
| [Directory Services Markup Language (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Textdatei mit fester Breite](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Textdateien mit fester Breite |
| [LDAP-Datenaustauschformat (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Microsoft Identity Manager-Connector für Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Verwandte Themen

[Verwaltungs-Agents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
