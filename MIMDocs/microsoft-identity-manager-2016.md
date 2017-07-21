---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "MIM enthält die Zugriffsverwaltungsfunktionen von FIM 2010 und unterstützt Sie dabei, Benutzer, Anmeldeinformationen, Richtlinien und Zugriffe in Ihrer Organisation zu verwalten."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ca5dafb78899e286aff6d2e767ad1509a6439e65
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 verwendet als Basis die Identitäts- und Zugriffsverwaltungsfunktionen von [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Wie sein Vorgänger unterstützt MIM Sie dabei, die Benutzer, die Anmeldeinformationen, die Richtlinien und den Zugriff in Ihrer Organisation zu verwalten.  Außerdem ist MIM 2016 um eine hybride Benutzeroberfläche, Funktionen für privilegierte Zugriffsverwaltung (Privileged Access Management) und Unterstützung für neue Plattformen erweitert.

Diese Version von Microsoft Identity Manager bietet neue Features wie das Privileged Identity Management und Unterstützung bei der Zertifikatsverwaltung für den REST-API-Zugriff. Die Zertifikatsverwaltung verfügt nun über die zusätzliche Unterstützung für Topologien mit mehreren Gesamtstrukturen, eine Windows-App für die Verwaltung virtueller Smartcards und des Zertifikatlebenslaufs, aktualisierte Ereignisse und Funktionen zur Problembehandlung. Self-Service-Szenarien umfassen jetzt die Kontenentsperrung sowie ein Gate für Azure MFA (mehrstufige Authentifizierung) zur Kennwortzurücksetzung.

## <a name="hybrid-experience"></a>Hybriderfahrung
Microsoft Identity Manager 2016 funktioniert zusammen mit Azure AD, damit Sie Kontrolle über Ihre vollständige Umgebung haben. Hybridberichterstellung in Azure AD präsentiert Ihre Cloud- und Ihre lokalen Daten an einem Ort. Darüber hinaus unterstützt das Portal zur Self Service-Kennwortzurücksetzung die Azure Multi-Factor Authentication (MFA).

## <a name="privileged-identity-management"></a>Privilegierte Identitätsverwaltung
Die privilegierte Identitätsverwaltung steuert und verwaltet den Administratorzugriff, indem temporärer, aufgabenbasierter Zugriff auf vertrauliche Ressourcen bereitgestellt wird. Dies bedeutet, Sie können Benutzern nur so viele Berechtigungen geben wie erforderlich. Dadurch die Wahrscheinlichkeit für einen Cyberangreifer verringert, vollen Administratorzugriff zu erhalten. Außerdem extrahiert und isoliert die privilegierte Identitätsverwaltung die Administratorkonten von vorhandenen Active Directory-Gesamtstrukturen.

MIM unterstützt eine lokale Privileged Identity Management-Lösung für die Verwaltung von Active Directory-Instanzen. Informationen finden Sie unter [Verwenden von Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Verwandte Themen
Microsoft Identity Manager ist nach wie vor eng mit dem Vorgänger Forefront Identity Manager verwandt. Wenn Sie weiterhin FIM verwenden oder zusätzliche Dokumentation zurate ziehen möchten, sehen Sie sich die [FIM 2010 R2-Dokumentationsroadmap](https://technet.microsoft.com/library/jj133885.aspx) an.
