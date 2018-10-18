---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: MIM enthält die Zugriffsverwaltungsfunktionen von FIM 2010 und unterstützt Sie dabei, Benutzer, Anmeldeinformationen, Richtlinien und Zugriffe in Ihrer Organisation zu verwalten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: abbd661fa1bef13ad92b916f8485934390905bf4
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358311"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 verwendet als Basis die Identitäts- und Zugriffsverwaltungsfunktionen von [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Wie sein Vorgänger unterstützt MIM Sie dabei, die Benutzer, die Anmeldeinformationen, die Richtlinien und den Zugriff in Ihrer Organisation zu verwalten.  Außerdem ist MIM 2016 um eine hybride Benutzeroberfläche, Funktionen für privilegierte Zugriffsverwaltung (Privileged Access Management) und Unterstützung für neue Plattformen erweitert.

Neben den vorhandenen Identitätsverwaltungsfunktionen in [FIM](https://technet.microsoft.com/library/jj133868) bietet MIM 2016 neue Features und Verbesserungen wie etwa Folgende:

- Privilegierte Identitätsverwaltung
- Neue Funktionen bei der Zertifikatsverwaltung
  - [Referenz zur REST-API für die Zertifikatverwaltung](./reference/certificate-management-rest-api-reference.md)
  - Unterstützung für Topologien mit mehreren Gesamtstrukturen
  - [Eine Windows-App für die virtuelle Smartcard](working-with-mim-certificate-manager.md)
  - Aktualisierte Ereignisse und Problembehandlungsfunktionen 
- [Self-Service-Szenarios](working-with-self-service-password-reset.md) umfassen jetzt die Kontenentsperrung sowie ein Gate für Azure MFA (mehrstufige Authentifizierung) zur Kennwortzurücksetzung.

## <a name="hybrid-experience"></a>Hybriderfahrung

Microsoft Identity Manager 2016 funktioniert zusammen mit [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis), damit Sie Kontrolle über Ihre vollständige Umgebung haben. Hybridberichterstellung in Azure AD präsentiert Ihre Cloud- und Ihre lokalen Daten an einem Ort. Darüber hinaus unterstützt das [Portal zur Self Service-Kennwortzurücksetzung](working-with-self-service-password-reset.md) Azure Multi-Factor Authentication (MFA).

## <a name="privileged-identity-management"></a>Privilegierte Identitätsverwaltung

Die privilegierte Identitätsverwaltung steuert und verwaltet den Administratorzugriff, indem temporärer, aufgabenbasierter Zugriff auf vertrauliche Ressourcen bereitgestellt wird. Dies bedeutet, Sie können Benutzern nur so viele Berechtigungen geben wie erforderlich. Dadurch die Wahrscheinlichkeit für einen Cyberangreifer verringert, vollen Administratorzugriff zu erhalten. Außerdem extrahiert und isoliert die privilegierte Identitätsverwaltung die Administratorkonten von vorhandenen Active Directory-Gesamtstrukturen.

MIM unterstützt eine lokale Privileged Identity Management-Lösung für die Verwaltung von Active Directory-Instanzen. Informationen finden Sie unter [Verwenden von Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Verwandte Themen

- Microsoft Identity Manager ist nach wie vor eng mit dem Vorgänger Forefront Identity Manager verwandt. Wenn Sie weiterhin FIM verwenden oder zusätzliche Dokumentation zurate ziehen möchten, sehen Sie sich die [FIM 2010 R2-Dokumentationsroadmap](https://technet.microsoft.com/library/jj133885.aspx) an.
- [Überlegungen zur Topologie für die Bereitstellung von MIM](topology-considerations.md) In diesem Artikel werden mehrere Bereitstellungstopologien vorgestellt, die Sie implementieren können.
- [Kapazitätsplanungshandbuch](capacity-planning-guide.md) Sie können diese Anleitung zusammen mit Testumgebungen verwenden, um den entsprechenden Aufgabenbereich für Ihre Bereitstellung zu verstehen.
