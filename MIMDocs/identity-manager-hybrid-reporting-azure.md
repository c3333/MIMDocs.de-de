---
title: Was ist die Hybridberichterstellung in Azure AD? | Microsoft-Dokumentation
description: "Mit Hybridberichten zur Überwachung von Aktivitäten in Azure Active Directory können Sie überwachte Ereignisse sowohl lokal als auch in der Cloud anzeigen."
keywords: 
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Überwachungsberichte zur hybriden Identitätsverwaltung in Azure Active Directory
Mit Überwachungsberichten in Azure Active Directory (Azure AD) können Sie Identitätsverwaltungsaktivitäten lokal oder in der Cloud überwachen. Sparen Sie Zeit, und senken Sie die Gesamtkosten, indem Sie sämtliche Identitäts- und Zugriffsdaten in einem einzigen Bericht verwalten.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Was ist die hybride Berichterstellung in Azure Active Directory?
Die Hybridberichterstellung für die Überwachung unterstützt IT-Experten dabei, häufige Herausforderungen bei der Berichterstellung für die Identitätsverwaltung zu meistern, wie z.B. folgende:

* **Erfassen von Identitätsverwaltungsaktivitäten über verschiedene Systeme hinweg**. In Hybridberichten sehen Sie die Identitätsverwaltungsaktivitäten von Azure AD und Identity Manager.

* **Exportieren von Berichtdaten und Erstellen von benutzerdefinierten Berichten**. Zusätzlich zum Anzeigen der Berichte im Azure-Portal können Sie die Daten exportieren, um Ihre eigenen benutzerdefinierten Ansichten zu generieren.

* **Verringern der Kosten für die Infrastruktur des Berichterstellungssystems**. Dank der Hybridberichterstellung in der Cloud können Sie Kosten in Zusammenhang mit Ihrer lokalen Data Warehouse-Infrastruktur vermeiden.

## <a name="how-does-it-work"></a>Wie funktioniert es?

Um die lokalen Daten zu sammeln, installieren Sie zunächst einen Berichterstellungs-Agent auf dem Identity Manager 2016-Server. [Laden Sie den Identity Manager-Agent für die Berichterstellung herunter](https://www.microsoft.com/download/details.aspx?id=55112).

Die Hybridberichterstellung durchläuft folgenden Prozess:
1. Nachdem Sie den Berichterstellungs-Agent installiert haben, werden die Identity Manager-Aktivitätsdaten an das Windows-Ereignisprotokoll gesendet.
2. Der Berichterstellungs-Agent verarbeitet die Deltaereignisse alle 10 Minuten oder beim Neustart des Windows-Ereignisprotokolldiensts. Danach lädt der Agent die Ereignisse in das Azure-Portal hoch.
3. Das Azure-Portal verarbeitet die erhaltenen Daten innerhalb einer Stunde nach dem Empfang.
4. Die Aktivitätsdaten werden in Azure für einen Monat gespeichert.
5. Das Azure-Portal ruft die Überwachungsberichtdaten ab und zeigt sie im Fenster der Azure-Überwachungsberichterstellung an.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen:
- [Arbeiten mit der Identity Manager-Hybridberichterstellung](working-with-identity-manager-hybrid-reporting.md)
- [Überwachungsaktivitätsberichte im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Aufbewahrungsrichtlinien für die Berichterstellung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure-Protokollintegration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory-Berichterstellungs-API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
