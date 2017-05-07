---
title: Was ist hybride Berichterstellung? | Microsoft Docs
description: "Mit hybriden Aktivitätsüberwachungsberichte in Azure Active Directory können Sie sowohl lokale als auch überwachte Ereignisse der Cloud anzeigen."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Hybride Überwachungsberichte zur Identitätsverwaltung in Azure Active Directory
Mit Überwachungsberichten in Azure Active Directory (AD) können Sie einen einzigen Bericht anzeigen, um Identitätsverwaltungsaktivitäten zu überwachen, die entweder lokal oder in der Cloud erfolgen. Über diese Funktionalität können Sie sämtliche Ihrer Identitäts- und Zugriffsdaten an einem Ort verwalten, wodurch Zeit gespart wird und die Gesamtkosten verringert werden.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Was ist die hybride Berichterstellung in Azure Active Directory?
Die hybride Überwachungsberichterstellung hilft IT-Experten dabei, häufige Herausforderungen bei der Berichterstellung für die Identitätsverwaltung zu meistern.

1. **Sammeln von Identitätsverwaltungsaktivitäten über verschiedene Systeme hinweg.** In Hybridberichten sehen Sie die Identitätsverwaltungsaktivitäten von Azure AD und Identity Manager.

2. **Exportieren von Berichtsdaten und Erstellen von benutzerdefinierten Berichten.** Zusätzlich zum Anzeigen der Berichte im Azure-Portal können Sie die Daten exportieren, um Ihre eigenen benutzerdefinierten Ansichten zu generieren.

3. **Verringern der Kosten für die Infrastruktur des Berichterstellungssystems.** Hybridberichterstellung in die Cloud bedeutet, dass Sie keine lokale Data Warehouse-Infrastruktur für die Berichterstellung mehr benötigen.

## <a name="how-does-it-work"></a>Wie funktioniert es?

Um die lokalen Daten zu sammeln, installieren Sie zunächst einen Berichterstellungs-Agent auf dem Identity Manager 2016-Server. Sie können den Berichterstellungs-Agent [hier](https://www.microsoft.com/en-us/download/details.aspx?id=####/) im Downloadbereich auf der Microsoft-Webseite herunterladen.

Für die Hybridberichterstellung werden die folgenden Schritte ausgeführt:
1. Nachdem der Berichterstellungs-Agent installiert ist, werden die Identity Manager-Aktivitätsdaten an das Windows-Ereignisprotokoll gesendet.
2. Der Berichterstellungs-Agent verarbeitet die Ereignisse im Windows-Ereignisprotokoll und lädt diese ins Azure-Portal hoch.
3. Die Aktivitätsdaten werden in Azure für einen Monat gespeichert.
4. Wenn Sie einen Bericht anfordern, werden die Aktivitätsereignisse analysiert und für die gewünschten Berichte gefiltert.
5. Das Azure-Portal ruft die Berichtsdaten ab und stellt diese als Überwachung im Aktivitätsbericht dar.

## <a name="see-also"></a>Weitere Informationen:
- Lesen Sie weitere Details zum [Arbeiten mit der Identity Manager-Hybridberichterstellung](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting).
- [Hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs) erhalten Sie weitere Informationen zu Überwachungsaktivitäsberichten im Azure AD-Portal

