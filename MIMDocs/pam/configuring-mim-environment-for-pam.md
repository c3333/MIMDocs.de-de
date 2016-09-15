---
title: Bereitstellen und Konfigurieren von PAM | Microsoft Identity Manager
description: "Die Roadmap für die Installation und Konfiguration von MIM für Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 4b4953089cb676baae97988f380debbfefcd1083


---

# Konfigurieren der MIM-Umgebung für Privileged Access Management
Das Einrichten der Umgebung für den gesamtstrukturübergreifenden Zugriff umfasst sieben Schritte, das Installieren und Konfigurieren von Active Directory und Microsoft Identity Manager sowie die Veranschaulichung einer bedarfsorientierten Zugriffsanforderung.

Diese Schritte sind so ausgelegt, dass Sie von Grund auf neu starten und eine Testumgebung erstellen können. Wenn Sie PAM auf eine vorhandene Umgebung anwenden, können Sie Ihre eigenen Domänencontroller oder Benutzerkonten verwenden, statt neue zu erstellen, die den Beispielen entsprechen.

1.  Bereiten Sie den Server *CORPDC* als Domänencontroller und *CORPWKSTN* als Mitglied der Domäne/Arbeitsgruppe vor.

2.  Bereiten Sie den Server *PRIVDC* als Domänencontroller vor.

3.  Bereiten Sie den Server *PAMSRV* in der Gesamtstruktur *PRIV* vor.

4.  Installieren Sie MIM-Komponenten auf *PAMSRV* und die Cmdlets auf einem Mitglied der Domäne/Arbeitsgruppe der Gesamtstruktur *CONTOSO* , und bereiten Sie diese dann für die privilegierte Zugriffsverwaltung vor.

5.  Richten Sie eine Vertrauensstellung zwischen den Gesamtstrukturen *PRIV* und *CONTOSO* ein.

6.  Vorbereiten von privilegierten Sicherheitsgruppen mit Zugriff auf geschützte Ressourcen und Mitgliedskonten für die bedarfsorientierte privilegierte Zugriffsverwaltung.

7.  Veranschaulichen Sie die Anforderung, den Erhalt und die Verwendung des privilegierten Zugriffs mit Rechteerweiterung auf eine geschützte Ressource.

>[!div class="step-by-step"]
[Start »](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO3-->

