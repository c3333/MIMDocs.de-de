---
title: Installation des BHOLD-Zugriffsverwaltungsconnectors | Microsoft-Dokumentation
description: Das BHOLD-Connectormodul unterstützt die anfängliche und laufende Synchronisierung von Daten
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ae9cc0bb4c63089c6733c06b7b035b2b9566fdd0
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041862"
---
# <a name="access-management-connector-installation"></a>Installation des BHOLD-Zugriffsverwaltungsconnectors

Das Modul des BHOLD Suite-Zugriffsverwaltungsconnectors unterstützt die laufende und die Erstsynchronisierung von Daten in BHOLD. Der Zugriffsverwaltungsconnector arbeitet mit dem MIM-Synchronisierungsdienst (Microsoft Identity Manager), um Daten zwischen der BHOLD Core-Datenbank, der FIM 2010-Metaverse und den Zielanwendungen und -identitätsspeichern zu verschieben. Nach der Installation des Moduls des Zugriffsconnectors können Sie FIM-Verwaltungs-Agents erstellen, die den Datenfluss zwischen BHOLD und MIM steuern.

## <a name="access-management-connector-software-requirements"></a>Softwareanforderungen für den Zugriffsverwaltungsconnector

Bevor Sie das Modul des Zugriffsverwaltungsconnectors installieren, müssen Sie Microsoft .NET Framework 4 installieren. Weitere Informationen und Installationsanweisungen zu .NET Framework 4 finden Sie auf der [Microsoft .NET-Homepage](https://www.microsoft.com/net).
Sie müssen den Zugriffsverwaltungsconnector auf einem Computer installieren, auf dem der FIM-Synchronisierungsdienst von MIM ausgeführt wird.

## <a name="access-management-connector-setup"></a>Setup des Zugriffsverwaltungsconnectors

Melden Sie sich zum Installieren des Moduls des Zugriffsverwaltungsconnectors als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das Modul des Zugriffsverwaltungsconnectors installieren möchten:

- AccessManagementConnector.msi

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

## <a name="next-steps"></a>Nächste Schritte

- [BHOLD FIM integration installation (Installation der BHOLD FIM-Integration)](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Installieren Sie das BHOLD FIM-Integrationsmodul, um Benutzern den Self-Service für Rollen zu ermöglichen.
- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)
