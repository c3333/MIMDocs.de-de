---
title: Was ist hybride Berichterstellung? | Microsoft Docs
description: "Mit hybriden Aktivitätsüberwachungsberichte in Azure Active Directory können Sie sowohl lokale als auch überwachte Ereignisse der Cloud anzeigen."
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 678626e7c32659570de88d8178c16821cceaf7ee
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Überwachungsberichte für die Hybrididentitätsverwaltung in Azure Active Directory – öffentliche Vorschau (aktualisiert)
Mit Überwachungsberichten in Azure Active Directory (AD) können Sie einen einzigen Bericht anzeigen, um Identitätsverwaltungsaktivitäten zu überwachen, die entweder lokal oder in der Cloud erfolgen. Über diese Funktionalität können Sie sämtliche Ihrer Identitäts- und Zugriffsdaten an einem Ort verwalten, wodurch Zeit gespart wird und die Gesamtkosten verringert werden.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Was ist die hybride Berichterstellung in Azure Active Directory?
Die hybride Überwachungsberichterstellung hilft IT-Experten dabei, häufige Herausforderungen bei der Berichterstellung für die Identitätsverwaltung zu meistern.

1. **Sammeln von Identitätsverwaltungsaktivitäten über verschiedene Systeme hinweg.** In Hybridberichten sehen Sie die Identitätsverwaltungsaktivitäten von Azure AD und Identity Manager.

2. **Exportieren von Berichtsdaten und Erstellen von benutzerdefinierten Berichten.** Zusätzlich zum Anzeigen der Berichte im Azure-Portal können Sie die Daten exportieren, um Ihre eigenen benutzerdefinierten Ansichten zu generieren.

3. **Verringern der Kosten für die Infrastruktur des Berichterstellungssystems.** Durch eine Hybridberichterstellung in der Cloud wird keine lokale Data Warehouse-Infrastruktur für die Berichterstellung mehr benötigt.

## <a name="how-does-it-work"></a>Wie funktioniert es?

Um die lokalen Daten zu sammeln, installieren Sie zunächst einen Berichterstellungs-Agent auf dem Identity Manager 2016-Server. Sie können den Berichterstellungs-Agent [hier](https://www.microsoft.com/en-us/download/details.aspx?id=55112) im Downloadbereich auf der Microsoft-Webseite herunterladen.

Für die Hybridberichterstellung werden die folgenden Schritte ausgeführt:
1. Nachdem der Berichterstellungs-Agent installiert ist, werden die Identity Manager-Aktivitätsdaten an das Windows-Ereignisprotokoll gesendet.
2. Der Berichterstellungs-Agent verarbeitet alle 10 Minuten oder beim Dienstneustart die Deltaereignisse im Windows-Ereignisprotokoll und lädt diese in das Azure-Portal hoch.
3. Das Azure-Portal verarbeitet die abgerufenen Daten innerhalb 1 Stunde nach Datenempfang.
4. Die Aktivitätsdaten werden in Azure für einen Monat gespeichert.
5. Das Azure-Portal ruft die Überwachungsberichtsdaten ab und stellt diese als Überwachung auf dem Blatt für Azure-Überwachungsberichte dar.

## <a name="see-also"></a>Weitere Informationen:
- Lesen Sie weitere Details zum [Arbeiten mit der Identity Manager-Hybridberichterstellung](working-with-identity-manager-hybrid-reporting.md).
- [Hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs) erhalten Sie weitere Informationen zu Überwachungsaktivitätsberichten im Azure AD-Portal