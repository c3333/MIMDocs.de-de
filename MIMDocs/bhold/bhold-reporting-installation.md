---
title: Installation der BHOLD-Berichterstellung | Microsoft-Dokumentation
description: "Das BHOLD-Berichterstellungsmodul ermöglicht Ihnen das Generieren von Berichten zu Rollen und Autorisierungsrichtlinien."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-reporting-installation"></a>Installation der BHOLD-Berichterstellung

Das BHOLD-Berichterstellungsmodul ermöglicht Ihnen das Generieren von Berichten zu Rollen und anderen Autorisierungsrichtlinien in BHOLD. Diese Berichte sind häufig für die Überwachung oder für das Demonstrieren der Konformität mit gesetzlichen Anforderungen nützlich. Außerdem erweitert dieses Modul die Möglichkeiten, die Autorisierung in Ihrer Organisation zu verwalten, indem Sie den Benutzern die Informationen übermitteln, die sie zum Analysieren der Mitgliedschaft ihrer Rollen benötigen. Diese Berichte können mit eingeschränkter Ansicht angezeigt werden, um zu gewährleisten, dass den Benutzern, die Berichte erstellen, nur die Informationen angezeigt werden, die für sie bestimmt sind.

## <a name="bhold-reporting-installation-requirements"></a>BHOLD-Berichterstellung – Installationsanforderungen

Vor der Installation des BHOLD-Berichterstellungsmoduls müssen Sie das BHOLD Core-Modul auf dem Server installieren, auf dem Sie das BHOLD-Berichterstellungsmodul installieren möchten. Weitere Informationen zum Installieren des BHOLD Core-Moduls finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Wenn Sie die BHOLD-Berichterstattung und den BHOLD-Nachweis installieren, müssen Sie die BHOLD-Berichterstattung vor dem BHOLD-Nachweis installieren.

## <a name="before-you-begin"></a>Vorbereitung

Bevor Sie mit der Installation des BHOLD-Berichterstellungsmoduls beginnen, müssen Sie darauf vorbereitet sein, die Informationen bereitzustellen, die der Assistent für das Setup des BHOLD-Berichterstellungsmoduls erfordert, um die Installation abzuschließen. Das folgende Arbeitsblatt unterstützt Sie beim Aufzeichnen dieser Informationen, sodass Sie diese bereitstellen können, wenn sie benötigt werden.

| **Element**                                    | **Beschreibung**                                                                                                                                                                                                           | **Wert**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden eines Sicherheitsanbieters auf einer Domäne/einem Computer** | Bei der Auswahl dieser Option wird angegeben, dass die Active Directory Domain Services-Sicherheit den Zugriff auf BHOLD Core steuert.                                                                                                                | Aktivieren Sie das Kontrollkästchen. </br>**Wichtig:** Die Installation schlägt fehl, wenn dieses Kontrollkästchen nicht aktiviert ist.                                                                                                                                                                                                                   |
| **Domäne**                                  | Gibt die Domäne an, die das Dienstkonto enthält, das Sie bei der Installation von BHOLD Core erstellt haben. Weitere Informationen finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Der Domänenname wird automatisch vom Assistenten bereitgestellt. Ändern Sie den Namen nur, wenn dieser falsch ist. **Wichtig:** Geben Sie den Domänennamen an, indem Sie den kurzen NetBIOS-Namen verwenden, nicht den vollqualifizierten Domänennamen (FQDN). Wenn der FQDN der Domäne beispielsweise „fabrikam.com“ ist, geben Sie den Domänennamen als „FABRIKAM“ an. |
| **Benutzer**                                    | Gibt den Anmeldenamen des BHOLD Core-Dienstbenutzerkontos an.                                                                                                                                                          | Geben Sie den Benutzerkontonamen hier ein:                                                                                                                                                                                                                                                                                    |
| **Passwort**                                | Gibt das Kennwort des Dienstbenutzerkontos an.                                                                                                                                                                       | Geben Sie das Kennwort hier ein: </br>**Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installation der BHOLD-Berichterstellung

Melden Sie sich zum Installieren des BHOLD-Berichterstellungsmoduls als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das BHOLD-Berichterstellungsmodul installieren möchten:

- BholdReporting*\<Version\>*\_Release.msi

Ersetzen Sie *\<Version\>* durch die Versionsnummer der Version des BHOLD-Berichterstellungsmoduls, die Sie installieren.

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

## <a name="next-steps"></a>Nächste Schritte

- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)