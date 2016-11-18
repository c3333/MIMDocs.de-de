---
title: Ebenenmodell der PAM-Umgebung | Microsoft Docs
description: "Erfahren Sie mehr zum Ebenenmodell, das Ihr System auf Grundlage seiner Risikoanfälligkeit unterteilt."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 659a9d8ff53372300c0e52432116c7c295d59a4b


---

# <a name="tier-model-for-partitioning-administrative-privileges"></a>Ebenenmodell zur Partitionierung von Administratorrechten

Angesichts der heutigen Bedrohungslage lautet die Frage nicht, ob ein Angreifer sich Zugriff auf Ihre Systeme verschaffen kann, sondern wann. Das bedeutet, dass die interne Sicherheit genauso wichtig ist wie eine starke Verteidigung im Umkreis. Dieser Artikel beschreibt ein Sicherheitsmodell, das vor einer Erweiterung von Rechten schützt, indem Aktivitäten mit hohen Berechtigungen von Zonen mit hohen Risiken getrennt werden. Dieses Modell setzt bewährte Methoden und Sicherheitsprinzipien um und sorgt dennoch für ein angenehmes Benutzererlebnis.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Rechteerweiterung in Active Directory-Gesamtstrukturen

Benutzer, Dienste oder Anwendungskonten, denen dauerhaft Administratorrechte für Windows Server-Active Directory-Gesamtstrukturen gewährt werden, stellen ein großes Risiko für die Arbeit und die Geschäfte von Organisationen dar. Diese Konten sind häufig das Ziel von Angreifern, da ein Angreifer im Erfolgsfall die Berechtigung zum Herstellen einer Verbindung mit anderen Servern oder Anwendungen in der Domäne erhält.

Das Ebenenmodell erstellt verschiedene Abteilungen für Administratoren, je nachdem, welche Ressourcen ein Administrator verwaltet. Administratoren, die Benutzerarbeitsstationen steuern, werden von Administratoren getrennt, die Anwendungen steuern oder Unternehmensidentitäten verwalten. Informationen zu diesem Modell finden Sie unter [Securing privileged access reference material](http://aka.ms/tiermodel) (Referenzmaterial zur Sicherung des privilegierten Zugriffs).

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Einschränken der Offenlegung von Anmeldeinformationen mit Anmeldeeinschränkungen

Um das Risiko zu senken, dass Anmeldeinformationen für Administratorkonten gestohlen werden, müssen in der Regel administrative Verfahren neu gestaltet werden, um die Anfälligkeit für Angriffe zu verringern. Als erste Schritte sollten Organisationen Folgendes erledigen:

- Die Anzahl der Hosts, auf denen Administratoranmeldeinformationen verfügbar gemacht werden, verringern.
- Die Rollenberechtigungen auf die Mindestanforderungen beschränken.
- Sicherstellen, dass administrative Aufgaben nicht auf Hosts ausgeführt werden, die für Standardbenutzeraktivitäten (z. B. E-Mail und Browsen im Internet) verwendet werden.

Im nächsten Schritt müssen Anmeldebeschränkungen implementiert und Prozesse und Vorgehensweisen umgesetzt werden, die die Einhaltung der Anforderungen an das Ebenenmodell sicherstellen. Im Idealfall sollte die Offenlegung von Anmeldeinformationen auf die niedrigste Berechtigung für die Rolle auf der jeweiligen Ebene reduziert werden.

Anmeldebeschränkungen sollten erzwungen werden, um sicherzustellen, dass Konten mit hohen Berechtigungen keinen Zugriff auf weniger sichere Ressourcen erhalten. Beispiel:

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

Das nächste Dokument, [Planen einer geschützten Umgebung](planning-bastion-environment.md), beschreibt das Hinzufügen einer dedizierten administrativen Gesamtstruktur für Microsoft Identity Manager zum Einrichten administrativer Konten.



<!--HONumber=Nov16_HO2-->


