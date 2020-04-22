---
title: Ebenenmodell der PAM-Umgebung | Microsoft Docs
description: Erfahren Sie mehr zum Ebenenmodell, das Ihr System auf Grundlage seiner Risikoanfälligkeit unterteilt.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4fb5689400d170adc19f15cbbc2d45915cb39fe3
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043595"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Ebenenmodell zur Partitionierung von Administratorrechten

Dieser Artikel beschreibt ein Sicherheitsmodell, das vor einer Erweiterung von Rechten schützt, indem Aktivitäten mit hohen Berechtigungen von Zonen mit hohen Risiken getrennt werden. Dieses Modell setzt bewährte Methoden und Sicherheitsprinzipien um und sorgt dennoch für ein angenehmes Benutzererlebnis.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Rechteerweiterung in Active Directory-Gesamtstrukturen

Benutzer, Dienste oder Anwendungskonten, denen dauerhaft Administratorrechte für Windows Server-Active Directory-Gesamtstrukturen gewährt werden, stellen ein großes Risiko für die Arbeit und die Geschäfte von Organisationen dar. Diese Konten sind häufig das Ziel von Angreifern, da ein Angreifer im Erfolgsfall Berechtigungen zum Herstellen einer Verbindung mit anderen Servern oder Anwendungen in der Domäne erhält.

Das Ebenenmodell erstellt verschiedene Abteilungen für Administratoren, je nachdem, welche Ressourcen ein Administrator verwaltet. Administratoren, die Benutzerarbeitsstationen steuern, werden von Administratoren getrennt, die Anwendungen steuern oder Unternehmensidentitäten verwalten. Informationen zu diesem Modell finden Sie unter [Securing privileged access reference material](https://aka.ms/tiermodel) (Referenzmaterial zur Sicherung des privilegierten Zugriffs).

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Einschränken der Offenlegung von Anmeldeinformationen mit Anmeldeeinschränkungen

Um das Risiko zu senken, dass Anmeldeinformationen für Administratorkonten gestohlen werden, müssen in der Regel administrative Verfahren neu gestaltet werden, um die Anfälligkeit für Angriffe zu verringern. Als erste Schritte sollten Organisationen Folgendes erledigen:

- Die Anzahl der Hosts, auf denen Administratoranmeldeinformationen verfügbar gemacht werden, verringern.
- Die Rollenberechtigungen auf die Mindestanforderungen beschränken.
- Sicherstellen, dass administrative Aufgaben nicht auf Hosts ausgeführt werden, die für Standardbenutzeraktivitäten (z. B. E-Mail und Browsen im Internet) verwendet werden.

Im nächsten Schritt müssen Anmeldebeschränkungen implementiert und Prozesse und Vorgehensweisen umgesetzt werden, die die Einhaltung der Anforderungen an das Ebenenmodell sicherstellen. Im Idealfall sollte die Offenlegung von Anmeldeinformationen auf die niedrigste Berechtigung für die Rolle auf der jeweiligen Ebene reduziert werden.

Anmeldebeschränkungen sollten erzwungen werden, um sicherzustellen, dass Konten mit hohen Berechtigungen keinen Zugriff auf weniger sichere Ressourcen erhalten. Beispiele:

- Domänenadministratoren (Ebene 0) können sich nicht bei Unternehmensservern (Ebene 1) und Standardbenutzer-Arbeitsstationen (Ebene 2) anmelden.
- Serveradministratoren (Ebene 1) können sich nicht bei Standardbenutzer-Arbeitsstationen (Ebene 2) anmelden.

>[!NOTE]
> Serveradministratoren sollten nicht Mitglied der Domänenadministratorgruppe sein. Personen, die für die Verwaltung sowohl von Domänencontrollern als auch von Unternehmensservern zuständig sind, benötigen separate Konten.

Anmeldebeschränkungen können durch Folgendes erzwungen werden:

- Einschränkungen für Anmeldungen per Gruppenrichtlinie, einschließlich der folgenden:
    - Zugriff vom Netzwerk auf diesen Computer verweigern
    - Anmelden als Batchauftrag verweigern
    - Anmelden als Dienst verweigern
    - Lokal anmelden verweigern
    - Anmelden über Remotedesktopeinstellungen verweigern  
- Authentifizierungsrichtlinien und Silos bei Verwendung von Windows Server 2012 oder höher
- Ausgewählte Authentifizierung, wenn das Konto Mitglied einer dedizierten administrativen Gesamtstruktur ist

## <a name="next-steps"></a>Nächste Schritte

- Das nächste Dokument, [Planen einer geschützten Umgebung](planning-bastion-environment.md), beschreibt das Hinzufügen einer dedizierten administrativen Gesamtstruktur für Microsoft Identity Manager zum Einrichten administrativer Konten.
- [Arbeitsstationen mit privilegiertem Zugriff](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) (Privileged Access Workstations, PAWs) stellen ein dediziertes Betriebssystem für sensible Aufgaben bereit, das vor Angriffen aus dem Internet und Bedrohungsvektoren geschützt ist.
