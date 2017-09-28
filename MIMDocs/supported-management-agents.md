---
title: "Unterstützte Connectors | Microsoft Docs"
description: "Verwenden Sie Connectors, um die Datenübertragung zwischen MIM und Ihren verknüpften Datenquellen zu verwalten."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/26/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 99e98f3f9cb5e68fde0e3018856bf613c082325d
ms.sourcegitcommit: ba4cd133f7b49752c5470c9fc46e7e302cc99b49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="connect-to-your-directories"></a>Herstellen der Verbindung zu Ihren Verzeichnissen

Connectors verknüpfen bestimmte verbundene Datenquellen mit Microsoft Identity Manager SP1 (MIM). Ein Connector verschiebt Daten aus einer verbundenen Datenquelle nach MIM. Werden Daten in MIM geändert, kann der Connector die Daten auch in die verbundene Datenquelle exportieren, um diese mit MIM zu synchronisieren. In der Regel gibt es mindestens einen Connector für jedes verbundene Verzeichnis.

In Forefront Identity Manager wurden Connectors als Verwaltungs-Agents bezeichnet. Dieser Begriff wird in einigen Artikeln oder Teilen des Produkts noch verwendet, beachten Sie aber, dass beide Begriffe sich auf das gleiche Konzept beziehen.

Dieser Artikel behandelt die in MIM enthaltenen und unterstützte Connectors, aber mit dem Connector für Extensible Connectivity 2.0 können Sie sogar Verbindungen zu noch mehr Datenquellen herstellen. Einige Partner haben auf diese Weise ihre eigenen Connectors erstellt. Eine vollständige Liste finden Sie im Wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Unterstützte Connectors in MIM 2016 SP1

| Name | Unterstützte Versionen der verbundenen Datenquelle |
| ---- | ----------------------------------------------- |
| Active Directory-Domänendienste | Active Directory 2012, 2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory-GAL (globale Adressliste) | Active Directory-GAL (globale Adressliste) – Exchange 2013, 2016 |
| Extensible Connectivity 2.0 | Jede aufruf- oder dateibasierte Datenquelle |
| FIM-Dienst | Der FIM-Dienstverwaltungs-Agent (Synchronisationsdienst) muss die gleiche Version wie der installierte „Forefront Identity Manager-Dienst (STS)“ aufweisen. |
| IBM DB2 Universal Database | IBM DB2, Versionen 9.5 oder 9.7; IBM DB2 OLEDB v9.5 FP5 oder v9.7 FP1 |
| IBM-Verzeichnisserver | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory, Versionen 8.7.3, 8.8.5 und 8.8.6 |
| Oracle-Datenbank | Oracle Database 10g oder 11g; 64-Bit-Client |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Verzeichnisserver für Oracle (zuvor Sun und Netscape) | Sun Directory Server 6.x, 7.x und Oracle 11 |
| [Windows PowerShell-Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 oder höher |
| [Microsoft Azure Active Directory Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Generischer LDAP-Connector für FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3-Server (RFC 4510-kompatibel) |
| [Connector für Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [SharePoint Services-Connector-UPA](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013 oder 2016 mit Benutzerprofildienst-Anwendung (UPA) |
| [Connector für Webdienste](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 oder 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| [Attribut-Wert-Paar-Textdatei](https://technet.microsoft.com/en-us/library/cc708644(v=ws.10).aspx) | Attribut-Wert-Paar-Textdateien |
| [Textdatei mit Trennzeichen](https://technet.microsoft.com/en-us/library/cc720612(v=ws.10).aspx) | Textdateien mit Trennzeichen |
| [Directory Services Markup Language (DSML)](https://technet.microsoft.com/en-us/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Textdatei mit fester Breite](https://technet.microsoft.com/en-us/library/cc720633(v=ws.10).aspx) | Textdateien mit fester Breite |
| [LDAP-Datenaustauschformat (LDIF)](https://technet.microsoft.com/en-us/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |

## <a name="related-topics"></a>Verwandte Themen

[Verwaltungs-Agents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
