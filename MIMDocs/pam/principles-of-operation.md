---
title: Verstehen der PAM-Komponenten | Microsoft Docs
description: "Privileged Access Management verfügt über einige der Komponenten von MIM, besitzt aber auch ein paar eigene. Erfahren Sie, wie diese zusammenarbeiten."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 53fe79f251c3b18426f16b4007cda49e67d7b028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# Verstehen der Komponenten von PAM
<a id="understand-the-components-of-pam" class="xliff"></a>

Durch Privileged Access Management lässt sich der administrative Zugriff von täglich verwendeten Benutzerkonten trennen. Diese Lösung basiert auf parallelen Gesamtstrukturen:

- *CORP*: Ihre allgemeine Unternehmensgesamtstruktur, die eine oder mehrere Domänen umfasst. Sie verfügen möglicherweise über mehrere CORP-Gesamtstrukturen – zur Vereinfachung wird bei den Beispielen in diesen Artikeln jedoch nur eine einzige Gesamtstruktur in einer einzigen Domäne verwendet.  
- *PRIV*: Eine dedizierte Gesamtstruktur, die speziell für dieses PAM-Szenario erstellt wurde. Diese Gesamtstruktur enthält eine Domäne zur Aufnahme privilegierter Gruppen und Konten, die aus mindestens einer CORP-Domäne stammen.

Die für PAM konfigurierte MIM-Lösung umfasst die folgenden Komponenten:  

- **MIM-Dienst**: Implementiert Geschäftslogik zum Ausführen von Identitäts- und Zugriffsverwaltungsvorgängen, einschließlich der privilegierten Kontoverwaltung und Behandlung von Anforderungen zur Rechteerweiterung.   
- **MIM-Portal**: Ein auf SharePoint basierendes Portal, das von SharePoint 2013 gehostet wird und eine Benutzeroberfläche für die Administratorverwaltung und -konfiguration bereitstellt.
- **MIM-Dienstdatenbank**: Diese wird in SQL Server 2012 oder 2014 gespeichert und enthält Identitätsdaten und Metadaten, die für den MIM-Dienst erforderlich sind.
- **PAM-Überwachungsdienst** und **PAM-Komponentendienst**: Zwei Dienste, die den Lebenszyklus von privilegierten Konten verwalten und das PRIV AD hinsichtlich des Lebenszyklus der Gruppenmitgliedschaft unterstützen.
- **PowerShell-Cmdlets**: Dienen zum Auffüllen von MIM-Dienst und PRIV AD mit Benutzern und Gruppen, die den Benutzern und Gruppen in der CORP-Gesamtstruktur für PAM-Administratoren entsprechen, und mit Endbenutzern, die eine Just-in-Time-Verwendung von Berechtigungen für ein Administratorkonto anfordern.
- **PAM-REST-API und Beispielportal**: Diese sind für Entwickler konzipiert, die MIM in das PAM-Szenario mit benutzerdefinierten Clients für die Rechteerweiterung integrieren, ohne PowerShell oder SOAP verwenden zu müssen. Die Verwendung der REST-API wird anhand einer Beispielwebanwendung veranschaulicht.

Nach der Installation und Konfiguration ist jede durch das Migrationsverfahren in der Gesamtstruktur PRIV erstellte Gruppe eine Schattenkopie einer SIDHistory-basierten Sicherheitsgruppe (oder bei einem späteren Update mit Windows Server vNext einer Fremdprinzipalgruppe), die die SID einer Gruppe in der ursprünglichen Gesamtstruktur CORP spiegelt. Wenn der MIM-Dienst zudem Mitglieder zu dieser Gruppe in der Gesamtstruktur „PRIV“ hinzufügt, sind diese Mitgliedschaften zeitlich begrenzt.

Wenn ein Benutzer daher eine Rechteerweiterung mithilfe von PowerShell-Cmdlets anfordert und seine Anforderung genehmigt wird, fügt der MIM-Dienst sein Konto in der Gesamtstruktur „PRIV“ zu einer Gruppe in der Gesamtstruktur „PRIV“ hinzu. Wenn sich der Benutzer mit seinem privilegierten Konto anmeldet, enthält sein Kerberos-Token eine SID (Sicherheits-ID), die mit der SID der Gruppe in der Gesamtstruktur „CORP“ übereinstimmt. Da die Gesamtstruktur „CORP“ so konfiguriert ist, dass sie der Gesamtstruktur „PRIV“ vertraut, scheint das Konto mit Rechteerweiterung, das für den Zugriff auf eine Ressource verwendet wird, in der Gesamtstruktur „CORP“ für eine Ressource, die die Kerberos-Gruppenmitgliedschaft überprüft, ein Mitglied der Sicherheitsgruppen dieser Ressource zu sein. Dies wird über die gesamtstrukturübergreifende Kerberos-Authentifizierung bereitgestellt.

Darüber hinaus sind diese Mitgliedschaften zeitlich begrenzt, damit das Administratorkonto des Benutzers nach einem vorkonfigurierten Zeitintervall kein Mitglied der Gruppe in der Gesamtstruktur „PRIV“ mehr ist. Dieses Konto kann daher nicht mehr für den Zugriff auf weitere Ressourcen verwendet werden.
