---
title: Installation der BHOLD-Analyse | Microsoft-Dokumentation
description: "Das BHOLD-Analysemodul ermöglicht das regelbasierte Testen des Datenzugriffs."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 631e08667e5d1535d8f63cc297aad360080f8b20
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-analytics-installation"></a>Installation der BHOLD-Analyse

Das BHOLD-Analysemodul ermöglicht das regelbasierte Testen des Datenzugriffs, um sicherzustellen, dass Ihre Organisation den Datenzugriff effektiv steuern kann und die internen und externen Zugriffsanforderungen erfüllt. Die automatisierte Auswirkungsanalyse, die vom BHOLD-Analysemodul generiert wird, bietet Ihnen einen Überblick über die Anzahl von Benutzern, die vom Erzwingen einer vorgeschlagenen Regel betroffen wären. Dabei werden sowohl die Benutzer angezeigt, die die Regel erfüllen, als auch die, die die Regel verletzen. Das BHOLD-Analysemodul kann ebenfalls eine detaillierte Liste der Benutzer bereitstellen, die die Regel erfüllen oder verletzen.

## <a name="bhold-analytics-installation-requirements"></a>BHOLD-Analyse – Installationsanforderungen

Vor der Installation des BHOLD-Analysemoduls müssen Sie das BHOLD Core-Modul auf dem Server installieren, auf dem Sie das BHOLD-Analysemodul installieren möchten. Weitere Informationen zum Installieren des BHOLD Core-Moduls finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Vorbereitung

Bevor Sie mit der Installation des BHOLD-Analysemoduls beginnen, müssen Sie darauf vorbereitet sein, die Informationen bereitzustellen, die der Assistent für das Setup des BHOLD-Analysemoduls erfordert, um die Installation abzuschließen. Das folgende Arbeitsblatt unterstützt Sie beim Aufzeichnen dieser Informationen, sodass Sie diese bereitstellen können, wenn sie benötigt werden.

| **Element**                                    | **Beschreibung**                                                                                                                                                                                                           | **Wert**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden eines Sicherheitsanbieters auf einer Domäne/einem Computer** | Bei der Auswahl dieser Option wird angegeben, dass die Active Directory Domain Services-Sicherheit den Zugriff auf BHOLD Core steuert.                                                                                                                | Aktivieren Sie das Kontrollkästchen. **Wichtig:** Die Installation schlägt fehl, wenn dieses Kontrollkästchen nicht aktiviert ist.                                                                                                                                                                                                                   |
| **Domäne**                                  | Gibt die Domäne an, die das Dienstkonto enthält, das Sie bei der Installation von BHOLD Core erstellt haben. Weitere Informationen finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Der Domänenname wird automatisch vom Assistenten bereitgestellt. Ändern Sie den Namen nur, wenn dieser falsch ist. **Wichtig:** Geben Sie den Domänennamen an, indem Sie den kurzen NetBIOS-Namen verwenden, nicht den vollqualifizierten Domänennamen (FQDN). Wenn der FQDN der Domäne beispielsweise „fabrikam.com“ ist, geben Sie den Domänennamen als „FABRIKAM“ an. |
| **Benutzer**                                    | Gibt den Anmeldenamen des BHOLD Core-Dienstbenutzerkontos an.                                                                                                                                                          | Geben Sie den Benutzerkontonamen hier ein:                                                                                                                                                                                                                                                                                    |
| **Passwort**                                | Gibt das Kennwort des Dienstbenutzerkontos an.                                                                                                                                                                       | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, dieses Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Installation der BHOLD-Analyse

Melden Sie sich zum Installieren des BHOLD-Analysemoduls als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das BHOLD-Analysemodul installieren möchten:

- BholdAnalytics*\<Version\>*\_Release.msi

Ersetzen Sie *\<Version\>* durch die Versionsnummer der Version des BHOLD-Analysemoduls, die Sie installieren.

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

# <a name="next-steps"></a>Nächste Schritte

- [BHOLD core installation (Installation von BHOLD Core)](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)
- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)