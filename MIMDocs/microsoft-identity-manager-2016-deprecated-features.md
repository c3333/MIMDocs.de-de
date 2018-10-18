---
title: Veraltete MIM-Features und Pläne für die Zukunft | Microsoft-Dokumentation
description: In diesem Artikel werden veraltete Features von MIM 2016 SP1 aufgelistet.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358433"
---
# <a name="deprecated-features"></a>Veraltete Features

In diesem Artikel werden veraltete Features von Microsoft Identity Manager 2016 SP1 beschrieben. Wenn das Feature in Microsoft Identity Manager noch vorhanden ist, wird es auch weiterhin unterstützt. Es wird nicht empfohlen, diese Features in neuen Bereitstellungen zu verwenden, da Sie möglicherweise in einem kommenden Release entfernt werden.  Für Entwickler wird vom Verwenden von veralteten Features in neuen Anwendungen oder Projektmappen abgeraten.

> [!NOTE]
> Features und Funktionen, die in MIM SP1 entfernt wurden, werden mit ** gekennzeichnet. <br>
> Weitere Informationen finden Sie im Artikel zum [Supportlebenszyklus für Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010).


## <a name="bhold"></a>BHOLD 

Es wird nicht empfohlen, neue Bereitstellungen von Microsoft BHOLD Suite-Komponenten vorzunehmen. Vorhandene BHOLD-Bereitstellungen werden weiterhin unterstützt. Azure AD bietet jetzt [Zugriffsüberprüfungen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), die einige der Nachweisfeatures von BHOLD ersetzen.

## <a name="certificate-management"></a>Zertifikatverwaltung 

| **Kategorie**                | **Veraltetes Feature**              | **Ersatz und Kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Verwaltungs-Agents | **FIM-Zertifikatverwaltung | Der FIM-Zertifikatverwaltungs-Agent wurde aus MIM 2016 entfernt.                                                             |

## <a name="service-and-portal"></a>Dienst und Portal

| **Kategorie**                | **Veraltetes Feature**              | **Ersatz und Kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmgesteuert Konfiguration | Schnittstelle für die Webdienstkonfiguration (ma-data und mv-data) | Die Möglichkeit der Konfiguration des FIM-Synchronisierungsdiensts über den FIM-Webdienst wird in der nächsten Version entfernt.
|

## <a name="synchronization-service"></a>Synchronisierungsdienst 

| **Kategorie**                | **Veraltetes Feature**              | **Ersatz und Kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmgesteuert Konfiguration | Schnittstelle für die Webdienstkonfiguration | Die Möglichkeit der Konfiguration des FIM-Synchronisierungsdiensts über den FIM-Dienst wird in der nächsten Version entfernt.                                                          |
| Verwaltungs-Agents           | Integrierte Verwaltungs-Agents                        | Folgende Verwaltungs-Agents wurden aus MIM 2016 entfernt: </br> 1. **Verwaltungs-Agent für die FIM-Zertifikatverwaltung </br>2. **Verwaltungs-Agent für Lotus Notes</br> 3. **Verwaltungs-Agent für SAP R/3 </br> Die Verwaltungs-Agents für Lotus Notes und SAP R/3 wurden durch neuere Versionen ersetzt. Weitere Informationen finden Sie unter [Connector – Versionsveröffentlichungsverlauf](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).                                                                                                                                                                                                                                              |
| Verwaltungs-Agents           | ECMA1                               | Das Erweiterungsframework ECMA1/XMA wurde durch ECMA 2.0 ersetzt. Vorhandene ECMA1-Verwaltungs-Agents müssen durch ECMA 2.0-Connectors ersetzt werden.                                                                                                                                          |
| Verwaltungs-Agents           | Ausführen von Connectors außerhalb des Prozesses      | Dieses Feature wird nicht ersetzt. Der Synchronisierungsdienst ruft den Connector immer im gleichen Prozess auf. Der Connector ist für das Starten und Verwalten des anderen Prozesses zuständig. |
| Verwaltungs-Agents           | Konfiguration des Anzeigenamens    | Dieses Feature wird nicht ersetzt. Diese Option wurde nur zum Bereitstellen eines alternativen Namens für eine Partition in der WMI-Schnittstelle verwendet.                                                                                                                                                                       |
| Ausführungsprofile                | Kombinierte Profile                   | Die kombinierten Profile „delta import/sync“, „full import/delta sync“ und „full import/sync“ werden entfernt. Es wird empfohlen, stattdessen Ausführungsprofile mit zwei Schritten zu verwenden. 

> [!NOTE]
> Kombinierte Ausführungsprofile sollten nur in Umgebungen verwendet werden, in denen viele Disconnectors die Leistung beeinträchtigen würden.


| **Kategorie**                | **Veraltetes Feature**              | **Ersatz und Kommentar**           |
|--------|-------|---|    
| Attributvorrang | Multi-Master-/gleiche Rangfolge                       | Die gleiche Rangfolge wird entfernt. Dieses Feature wird nicht ersetzt. Es wird empfohlen, die Rangfolge manuell festzulegen. Sie können dieses Feature weiterhin verwenden, wenn in Ihrer Umgebung ein FIM-Dienstverwaltungs-Agent bereitgestellt wurde. Dieser Verwaltungs-Agent bietet keine manuelle Rangfolge, um „export-not-precedent“ für das deklarative Bereitstellen zu vermeiden. |
| Verknüpfungsregeln           | Verknüpfungen für den Objekttyp „any“                             | Dieses Feature wird nicht ersetzt. Alle Verknüpfungsregeln sollten explizit den Metaverse-Objekttyp definieren, mit dem sie eine Verknüpfung herzustellen versuchen.       |
| Attributflüsse      | Deaktivieren von „NULL zulassen“ bei exportierten Werten            | Dieses Feature wird nicht ersetzt. „NULL zulassen“ ist immer ausgewählt. Stellen Sie sicher, dass „NULL zulassen“ für Ihre aktuelle Umgebung aktiviert ist.  |
| Attributflüsse      | „Do not recall attributes” (Kein Attributrückruf)                            | Dieses Feature wird nicht ersetzt. Attribute werden immer zurückgerufen, da diese Methode sich bewährt hat.  |
| Regelerweiterung      | Ausführen der Regelerweiterung für Metaverse und Verwaltungs-Agents außerhalb eines Prozesses | Dieses Feature wird nicht ersetzt. Die Metaverse- und Attributflussregeln werden im gleichen Prozess wie die Synchronisierungs-Engine ausgeführt.       |
| Regelerweiterung      | Transaktionseigenschaften                                | Dieses Feature wird nicht ersetzt. Vermeiden Sie das Übergeben von Daten zwischen eingehenden, bereitstellenden und ausgehenden Synchronisierungen mit dieser Hilfsprogrammklasse.  |
| Regelerweiterung      | ExchangeUtils: Create55\*-Methoden                     | Die Methoden zum Erstellen von Objekten für Exchange 5.5-Server werden entfernt.        |
| Schnittstelle            | Mms_Metaverse                                        | Alle ClmUtils-Klassenmember werden in einer kommenden Version entfernt.   |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen:

- Microsoft Identity Manager ist nach wie vor eng mit dem Vorgänger Forefront Identity Manager verwandt. Wenn Sie weiterhin FIM verwenden oder zusätzliche Dokumentation zurate ziehen möchten, sehen Sie sich die [FIM 2010 R2-Dokumentationsroadmap](https://technet.microsoft.com/library/jj133885.aspx) an.
- [Überlegungen zur Topologie für die Bereitstellung von MIM](topology-considerations.md) In diesem Artikel werden mehrere Bereitstellungstopologien vorgestellt, die Sie implementieren können.
- [Kapazitätsplanungshandbuch](capacity-planning-guide.md) Sie können diese Anleitung zusammen mit Testumgebungen verwenden, um den entsprechenden Aufgabenbereich für Ihre Bereitstellung zu verstehen.
