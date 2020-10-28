---
title: Privileged Access Management Rest-API-Referenz | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8a318ae5f12fd49e2ee949d81aefd86a7221e120
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760722"
---
# <a name="privileged-access-management-rest-api-reference"></a>Privileged Access Management Rest-API-Referenz
Microsoft Identity Manager (MIM) 2016 fügt ein neues Szenario hinzu, dass als „privilegierte Zugriffsverwaltung“ (Privileged Access Management, PAM) bezeichnet wird. PAM weitet die Kontrolle eines Unternehmens über die Zugriffsrechte von sehr privilegierten Benutzerkonten, z. B. System- oder Dienstadministratoren, auf vertrauliche Ressourcen aus. PAM steuert den sehr privilegierten Kontenzugriff durch die Bereitstellung von Zugriffsrechten mit zeitlich begrenzter Gültigkeit (bedarfsorientiert, JIT), wenn die Zugriffsrechte erforderlich sind.

Ein Benutzer kann privilegierte Zugriffsrechte (Rechteerweiterung) auf eine von zwei möglichen Arten beim MIM-Dienst anfordern:

- Mithilfe der PAM Rest-API.
- Mithilfe des PAM PowerShell-New-PAMRequest-Cmdlets.

Die Themen in diesem Handbuch beschreiben die PAM REST-API. Weitere Informationen zum Verwenden des PowerShell-Cmdlets finden Sie in _der Test Umgebungs Anleitung: demonstrieren von privileged Access Management mithilfe Microsoft Identity Manager_ , die auf der Connect-Website verfügbar ist.

## <a name="pam-rest-api-resources-and-operations"></a>PAM Rest-API-Ressourcen und-Vorgänge
Die PAM Rest-API arbeitet mit den folgenden Ressourcen:
- **PAM-Rolle** : eine PAM-Rolle ordnet eine Sammlung von Benutzern eine Sammlung von Zugriffsrechten zu. Die Zugriffsrechte werden anhand von Sicherheitsgruppen definiert.  Jede PAM-Rolle verfügt über eine Liste von Benutzerkonten, die als Kandidaten bezeichnet werden, die berechtigt sind, die Rechte auf die PAM-Rolle zu erweitern. Für PAM-Rollen können die folgenden Vorgänge ausgeführt werden:

    - [Abrufen von PAM-Rollen](privileged-access-management-get-roles.md)

- **PAM-Anforderung** : ein Benutzer, der Rechte auf PAM-Rollen Zugriffsrechte herauf Stufen möchte, muss eine PAM-Anforderung senden und die Genehmigung für die Anforderung zur Erhöhung von Berechtigungen erhalten. Das PAM-Anforderungsobjekt verfolgt den Lebenszyklus dieser Anforderung im MIM-Dienst nach. Für PAM-Anforderungen können die folgenden Vorgänge ausgeführt werden:

    - [Erstellen von PAM-Anforderungen](privileged-access-management-create-request.md)
    - [Abrufen von PAM-Anforderungen](privileged-access-management-get-requests.md)
    - [Schließen von PAM-Anforderungen](privileged-access-management-close-request.md)

- **Ausstehende PAM-Anforderung** : wird zum genehmigen oder ablehnen von PAM-Anforderungen verwendet, die von Benutzern übermittelt wurden. Für ausstehende PAM-Anforderungen können die folgenden Vorgänge ausgeführt werden:

    - [Abrufen ausstehender PAM-Anforderungen](privileged-access-management-get-pending-requests.md)
    - [Genehmigen oder Ablehnen einer ausstehenden PAM-Anforderung](privileged-access-management-approve-reject-pending-request.md)

- **PAM-Sitzung** : bei der Verwendung der PAM-Rest-API verfügt der Client (z. b. ein Webbrowser) über eine Sitzung mit dem PAM Rest-API-Endpunkt. In dieser Sitzung wird der Client für den Rest-API-Endpunkt authentifiziert. Für PAM-Sitzungen können die folgenden Vorgänge ausgeführt werden:

     - [Abrufen von PAM-Sitzungsinformationen](privileged-access-management-get-session-info.md)

Ausführlichere Informationen zum Dienst finden Sie unter [PAM Rest-API-Dienst Details](privileged-access-management-rest-api-service-details.md).

## <a name="pam-sample-portal-on-github"></a>PAM-Beispiel Portal auf GitHub
Eine Möglichkeit, um zu erfahren, wie die PAM Rest-API verwendet wird, ist die Verwendung des PAM-Beispiel Portals, einer beispielweb Anwendung, die die API verwendet. Den Code für das PAM-Beispielportal finden Sie unter [PAM-Beispielrepository auf GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Informationen zum Bereitstellen des Beispielportals erhalten Sie in der PAM-Testumgebungsanleitung.
