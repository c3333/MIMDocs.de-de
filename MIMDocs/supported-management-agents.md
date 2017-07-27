---
title: "Unterstützte Connectors | Microsoft Docs"
description: "Verwenden Sie Connectors, um die Datenübertragung zwischen MIM und Ihren Verzeichnissen zu verwalten."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="connect-to-your-directories"></a>Herstellen der Verbindung zu Ihren Verzeichnissen

Connectors verknüpfen bestimmte verbundene Datenquellen mit Microsoft Identity Manager (MIM). Ein Connector verschiebt Daten aus einer verbundenen Datenquelle nach MIM. Werden Daten in MIM geändert, kann der Connector die Daten auch in die verbundene Datenquelle exportieren, um diese mit MIM zu synchronisieren. In der Regel gibt es mindestens einen Connector für jedes verbundene Verzeichnis.

In Forefront Identity Manager wurden Connectors als Verwaltungs-Agents bezeichnet. Dieser Begriff wird in einigen Artikeln oder Teilen des Produkts noch verwendet, beachten Sie aber, dass beide Begriffe sich auf das gleiche Konzept beziehen.

Dieser Artikel behandelt die in MIM enthaltenen Connectors, aber mit dem Connector für Extensible Connectivity 2.0 können Sie sogar Verbindungen zu noch mehr Datenquellen herstellen. Einige Partner haben auf diese Weise ihre eigenen Connectors erstellt. Eine vollständige Liste finden Sie im Wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016"></a>Unterstützte Connectors im MIM 2016

| Name | Unterstützte Versionen der verbundenen Datenquelle |
| ---- | ----------------------------------------------- |
| Active Directory-Domänendienste | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory-GAL (globale Adressliste) | Active Directory-GAL (globale Adressliste) – Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Jede aufruf- oder dateibasierte Datenquelle |
| MIM-Dienst | Microsoft Docs 2016 |
| IBM DB2 Universal Database | IBM DB2, Versionen 9.1, 9.5 oder 9.7; IBM DB2 OLEDB v9.5 FP5 oder v9.7 FP1 |
| IBM-Verzeichnisserver | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory, Versionen 8.7.3, 8.8.5 und 8.8.6 |
| Oracle-Datenbank | Oracle Database 10g oder 11g; 64-Bit-Client |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Verzeichnisserver für Oracle (zuvor Sun und Netscape) | Sun Directory Server 6.x, 7.x und Oracle 11 |
| [Windows PowerShell-Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 oder höher |
| [Microsoft Azure Active Directory Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Generischer LDAP-Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3-Server (RFC 4510-kompatibel) |
| [Connector für Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.0.x oder v8.5.x |
| [SharePoint Services-Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013 oder 2016 mit Benutzerprofildienst-Anwendung (UPA) |
| [Connector für Webdienste](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 oder 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| Attribut-Wert-Paar-Textdatei | Attribut-Wert-Paar-Textdateien |
| Textdatei mit Trennzeichen | Textdateien mit Trennzeichen |
| Directory Services Markup Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Textdatei mit fester Breite | Textdateien mit fester Breite |
| LDAP Data Interchange Format (LDIF) | LDAP Data Interchange Format (LDIF) |

## <a name="related-topics"></a>Verwandte Themen

[Verwaltungs-Agents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
