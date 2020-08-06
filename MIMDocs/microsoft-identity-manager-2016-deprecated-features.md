---
title: Veraltete MIM-Features und Pläne für die Zukunft | Microsoft-Dokumentation
description: In diesem Artikel werden veraltete Features von MIM 2016 SP2 aufgelistet.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 7/28/2020
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: cb0159ec70e161dc47140712bd2ac039786e034d
ms.sourcegitcommit: 50cee02a7146445bd6fa361349099c7b294792d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87443544"
---
# <a name="deprecated-features"></a>Veraltete Features

In diesem Artikel werden veraltete Features von Microsoft Identity Manager 2016 SP2 beschrieben. Wenn das Feature in Microsoft Identity Manager noch vorhanden ist, wird es auch weiterhin unterstützt. Es wird nicht empfohlen, diese Features in neuen Bereitstellungen zu verwenden, da Sie möglicherweise in einem kommenden Hotfix oder Service Pack-Release entfernt werden.  Für Entwickler wird vom Verwenden von veralteten Features in neuen Anwendungen oder Projektmappen abgeraten.

> [!NOTE]
>
> Weitere Informationen zu den neuen Supportoptionen für MIM finden Sie unter [Supportoptionen für Azure AD Premium-Kunden](support-update-for-azure-active-directory-premium-customers.md).

## <a name="bhold"></a>BHOLD

Es wird nicht empfohlen, neue Bereitstellungen von Microsoft BHOLD Suite-Komponenten vorzunehmen. Vorhandene BHOLD-Bereitstellungen werden weiterhin unterstützt. Azure AD bietet nun [Zugriffsüberprüfungen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), die die Nachweisfeatures von BHOLD ersetzen, und die Berechtigungsverwaltung, die die Zugriffszugriffsfeatures ersetzen.

## <a name="service-and-portal"></a>Dienst und Portal

| **Kategorie**                | **Veraltetes Feature**              | **Kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmgesteuert Konfiguration der Synchronisierung | Schnittstelle für die Webdienstkonfiguration (ma-data und mv-data) | Die Möglichkeit der Konfiguration des MIM-Synchronisierungsdiensts über den MIM-Webdienst wird in einem zukünftigen Hotfix oder Service Pack möglicherweise entfernt.
|

## <a name="synchronization-service"></a>Synchronisierungsdienst 

Folgende Verwaltungs-Agents wurden aus MIM 2016 entfernt: </br> 1.  Verwaltungs-Agent für die FIM-Zertifikatverwaltung </br>2.  Verwaltungs-Agent für Lotus Notes</br> 3.  Verwaltungs-Agent für SAP R/3 </br> Die Verwaltungs-Agents für Lotus Notes und SAP R/3 wurden durch neuere Connectors ersetzt. Weitere Informationen finden Sie unter [Connector – Versionsveröffentlichungsverlauf](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).

Das Erweiterungsframework ECMA1/XMA wurde durch ECMA 2.0 ersetzt. Vorhandene ECMA1-Verwaltungs-Agents müssen durch ECMA 2.0-Connectors ersetzt werden.

| **Kategorie**                | **Veraltetes Feature**              | **Kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Verwaltungs-Agents           | Ausführen von Connectors außerhalb des Prozesses      | Der Synchronisierungsdienst ruft den Connector immer im gleichen Prozess auf. Der Connector ist für das Starten und Verwalten des anderen Prozesses zuständig. |
| Verwaltungs-Agents           | Konfiguration des Anzeigenamens    | Diese Option wurde nur zum Bereitstellen eines alternativen Namens für eine Partition in der WMI-Schnittstelle verwendet.                                                                                                                                                                       |
| Ausführungsprofile                | Kombinierte Profile                   | Die kombinierten Profile „delta import/sync“, „full import/delta sync“ und „full import/sync“ werden möglicherweise entfernt. Verwenden Sie stattdessen Ausführungsprofile mit zwei Schritten.

> [!NOTE]
> Kombinierte Ausführungsprofile sollten nur in Umgebungen verwendet werden, in denen viele Disconnectors die Leistung beeinträchtigen würden.

| **Kategorie**                | **Veraltetes Feature**              | **Kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Attributrangfolge | Multi-Master-/gleiche Rangfolge                       | Die gleiche Rangfolge wird möglicherweise entfernt. Es wird empfohlen, die Rangfolge manuell festzulegen. Sie können dieses Feature weiterhin verwenden, wenn in Ihrer Umgebung ein FIM-Dienstverwaltungs-Agent bereitgestellt wurde. Dieser Verwaltungs-Agent bietet keine manuelle Rangfolge, um „export-not-precedent“ für das deklarative Bereitstellen zu vermeiden. |
| Verknüpfungsregeln           | Verknüpfungen für den Objekttyp „any“                             | Alle Verknüpfungsregeln sollten explizit den Metaverse-Objekttyp definieren, mit dem sie eine Verknüpfung herzustellen versuchen.       |
| Attributflüsse      | Deaktivieren von „NULL zulassen“ bei exportierten Werten            | „NULL zulassen“ wird immer ausgewählt, stellen Sie also sicher, dass „NULL zulassen“ für Ihre aktuelle Umgebung aktiviert ist.  |
| Attributflüsse      | „Do not recall attributes” (Kein Attributrückruf)                            | Attribute werden immer zurückgerufen, da diese Methode sich bewährt hat.  |
| Regelerweiterung      | Ausführen der Regelerweiterung für Metaverse und Verwaltungs-Agents außerhalb eines Prozesses | Die Metaverse- und Attributflussregeln werden im gleichen Prozess wie die Synchronisierungs-Engine ausgeführt.       |
| Regelerweiterung      | Transaktionseigenschaften                                | Vermeiden Sie das Übergeben von Daten zwischen eingehenden, bereitstellenden und ausgehenden Synchronisierungen mit dieser Hilfsprogrammklasse.  |
| Regelerweiterung      | ExchangeUtils: Create55\*-Methoden                     | Die Methoden zum Erstellen von Objekten für Exchange 5.5-Server werden möglicherweise entfernt.        |
| Schnittstelle            | Mms_Metaverse                                        | Alle ClmUtils-Klassenmember werden möglicherweise in einem zukünftigen Hotfix oder Service Pack entfernt.   |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen:

- Microsoft Identity Manager ist nach wie vor eng mit dem Vorgänger Forefront Identity Manager verwandt. Wenn Sie weiterhin FIM verwenden oder zusätzliche Dokumentation zurate ziehen möchten, sehen Sie sich die [FIM 2010 R2-Dokumentationsroadmap](https://technet.microsoft.com/library/jj133885.aspx) an.
- [Überlegungen zur Topologie für die Bereitstellung von MIM](topology-considerations.md) In diesem Artikel werden mehrere Bereitstellungstopologien vorgestellt, die Sie implementieren können.
- [Kapazitätsplanungshandbuch](capacity-planning-guide.md) Sie können diese Anleitung zusammen mit Testumgebungen verwenden, um den entsprechenden Aufgabenbereich für Ihre Bereitstellung zu verstehen.

