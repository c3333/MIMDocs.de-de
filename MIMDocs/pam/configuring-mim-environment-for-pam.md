---
title: Konfigurieren von MIM 2016 für Privileged Access Management | Microsoft-Dokumentation
description: Die Roadmap für die Installation und Konfiguration von MIM für Privileged Access Management.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ade3557c99dc2e0623000f47df0e9519178c0641
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334056"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Konfigurieren der MIM-Umgebung für Privileged Access Management

Das Einrichten der Umgebung für den gesamtstrukturübergreifenden Zugriff umfasst sieben Schritte, das Installieren und Konfigurieren von Active Directory und Microsoft Identity Manager sowie die Veranschaulichung einer bedarfsorientierten Zugriffsanforderung.

Diese Schritte sind so ausgelegt, dass Sie von Grund auf neu starten und eine Testumgebung erstellen können. Wenn Sie PAM auf eine vorhandene Umgebung anwenden, können Sie Ihre eigenen Domänencontroller oder Benutzerkonten verwenden, statt neue zu erstellen, die den Beispielen entsprechen.

1. Bereiten Sie den Server *CORPDC* als Domänencontroller und *CORPWKSTN* als Mitglied der Domäne/Arbeitsgruppe vor.

2. Bereiten Sie den Server *PRIVDC* als Domänencontroller vor.

3.  Bereiten Sie den Server *PAMSRV* in der Gesamtstruktur *PRIV* vor.

4.  Installieren Sie MIM-Komponenten auf *PAMSRV* und die Cmdlets auf einem Mitglied der Domäne/Arbeitsgruppe der Gesamtstruktur *CONTOSO* , und bereiten Sie diese dann für die privilegierte Zugriffsverwaltung vor.

5.  Richten Sie eine Vertrauensstellung zwischen den Gesamtstrukturen *PRIV* und *CONTOSO* ein.

6.  Vorbereiten von privilegierten Sicherheitsgruppen mit Zugriff auf geschützte Ressourcen und Mitgliedskonten für die bedarfsorientierte privilegierte Zugriffsverwaltung.

7.  Veranschaulichen Sie die Anforderung, den Erhalt und die Verwendung des privilegierten Zugriffs mit Rechteerweiterung auf eine geschützte Ressource.

> [!div class="step-by-step"]
> [Start »](step-1-prepare-corp-domain.md)
