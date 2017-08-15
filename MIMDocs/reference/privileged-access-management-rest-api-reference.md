---
title: Referenz zur PAM REST-API | Microsoft-Dokumentation
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Referenz zur PAM REST-API (privilegierte Zugriffsverwaltung)
Microsoft Identity Manager (MIM) 2016 fügt ein neues Szenario hinzu, dass als „privilegierte Zugriffsverwaltung“ (Privileged Access Management, PAM) bezeichnet wird. PAM weitet die Kontrolle eines Unternehmens über die Zugriffsrechte von sehr privilegierten Benutzerkonten, z. B. System- oder Dienstadministratoren, auf vertrauliche Ressourcen aus. PAM steuert den sehr privilegierten Kontenzugriff durch die Bereitstellung von Zugriffsrechten mit zeitlich begrenzter Gültigkeit (bedarfsorientiert, JIT), wenn die Zugriffsrechte erforderlich sind.

Ein Benutzer kann privilegierte Zugriffsrechte (Rechteerweiterung) auf eine von zwei möglichen Arten beim MIM-Dienst anfordern:

- Mithilfe der PAM REST-API
- Mithilfe des PAM PowerShell-Cmdlets „New-PAMRequest“

Die Themen in diesem Handbuch beschreiben die PAM REST-API. Weitere Informationen zum Verwenden des PowerShell-Cmdlets finden Sie der Testumgebungsanleitung: „Veranschaulichen der privilegierten Zugriffsverwaltung mit Microsoft Identity Manager“, die auf der Connect-Website verfügbar ist.

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST-API-Ressourcen und -Vorgänge
Die PAM REST-API arbeitet mit den folgenden Ressourcen:
- **PAM-Rolle**: Eine PAM-Rolle ordnet eine Sammlung von Benutzern zu einer Sammlung von Zugriffsrechten zu. Die Zugriffsrechte werden anhand von Sicherheitsgruppen definiert.  Jede PAM-Rolle verfügt über eine Liste von Benutzerkonten, die als Kandidaten bezeichnet werden, die berechtigt sind, die Rechte auf die PAM-Rolle zu erweitern. Für PAM-Rollen können die folgenden Vorgänge ausgeführt werden:

    - [Abrufen von PAM-Rollen](privileged-access-management-get-roles.md)

- **PAM-Anforderung**: Ein Benutzer, der seine Rechte auf die Zugriffsrechte einer PAM-Rolle erweitern möchte, muss eine PAM-Anforderung senden und die Genehmigung für die Anforderung erhalten, damit die Rechte erweitert werden können. Das PAM-Anforderungsobjekt verfolgt den Lebenszyklus dieser Anforderung im MIM-Dienst nach. Für PAM-Anforderungen können die folgenden Vorgänge ausgeführt werden:

    - [Erstellen von PAM-Anforderungen](privileged-access-management-create-request.md)
    - [Abrufen von PAM-Anforderungen](privileged-access-management-get-requests.md)
    - [Schließen von PAM-Anforderungen](privileged-access-management-close-request.md)

- **Ausstehende PAM-Anforderung**: Diese wird zum Genehmigen oder Ablehnen von PAM-Anforderungen verwendet, die von Benutzern übermittelt wurden. Für ausstehende PAM-Anforderungen können die folgenden Vorgänge ausgeführt werden:

    - [Abrufen ausstehender PAM-Anforderungen](privileged-access-management-get-pending-requests.md)
    - [Genehmigen oder Ablehnen einer ausstehenden PAM-Anforderung](privileged-access-management-approve-reject-pending-request.md)

- **PAM-Sitzung**: Bei der Verwendung der PAM REST-API hat der Client (z. B. ein Webbrowser) eine Sitzung mit dem Endpunkt der PAM REST-API. In dieser Sitzung wird der Client für den REST-API-Endpunkt authentifiziert. Für PAM-Sitzungen können die folgenden Vorgänge ausgeführt werden:

     - [Abrufen von PAM-Sitzungsinformationen](privileged-access-management-get-session-info.md)

Weitere Informationen zum Dienst finden Sie unter [PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>PAM-Beispielportal auf GitHub
Eine Möglichkeit, um Informationen zur PAM REST-API zu erhalten, ist die Verwendung des PAM-Beispielportals, bei dem es sich um eine Beispielwebanwendung handelt, die diese API verwendet. Den Code für das PAM-Beispielportal finden Sie unter [PAM-Beispielrepository auf GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Informationen zum Bereitstellen des Beispielportals erhalten Sie in der PAM-Testumgebungsanleitung.
