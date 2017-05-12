---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft-Dokumentation
description: "Erfahren Sie, wie MIM 2016 eine sicherere und einfachere Erfahrung bei der Identitätsverwaltung in der Cloud und lokal bietet."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 69d44af5eaef3665f3a55ea91f48d3658cd5e65c
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>Neuheiten in Microsoft Identity Manager 2016 Service Pack 1 #

Wir freuen uns, im Rahmen des normalen Veröffentlichungszyklus für die Wartung und Aktualisierung von Microsoft Identity Manager die Einführung von [Microsoft Identity Manager (MIM) 2016 Servicepack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212) bekanntgeben zu können. In diesem Dokument werden die Aktualisierungen, Verbesserungen, Funktionen und Änderungen in dieser Version erläutert.

Wenn bei einer Produktionsbereitstellung von MIM SP1 Probleme auftreten, wenden Sie sich an den Microsoft-Kundensupport.

Wir möchten auch von Ihnen hören! Wenn Sie Feedback, Kommentare oder Anliegen für das Produktteam haben, senden Sie uns eine E-Mail an [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## <a name="updates-in-this-service-pack"></a>Aktualisierungen in diesem Servicepack #

### <a name="mim"></a>MIM

- **Browserübergreifende Kompatibilität des MIM-Portals für Endbenutzer-Self-Service:** In diesem Service Pack wird die Unterstützung der meisten gängigen Browser eingeführt. Benutzer können jetzt in Edge, Chrome oder Safari auf das MIM-Portal zugreifen und Gruppen sowie Profile selbst verwalten.

- **Unterstützung des MIM-Dienstes für Exchange Online:** Der MIM-Dienst unterstützt bereits seit langem das Senden und Empfangen von E-Mails für Genehmigungen und Benachrichtigungen. Vor SP1 hat MIM nur Exchange Server oder SMTP unterstützt. Mit Service Pack 1 kann der MIM-Dienst Anforderungen sowie E-Mail-Benachrichtigungen mit einem Office 365 Exchange-Onlinekonto senden und empfangen.

- **Überprüfung des Bildformats beim Upload:** MIM kann jetzt das Dateiformat von Bildern überprüfen, die auf das Portal hochgeladen werden.

### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Unterstützung der Funktionsebene „Windows Server 2016“ durch die PAM „PRIV“-Gesamtstruktur (geschützt) :** Der MIM PAM-Dienst kann in einer Umgebung mit Domänencontrollern konfiguriert werden, die auf der Active Directory Domain Services-Funktionsebene „Windows Server 2016“ ausgeführt werden. Bei Konfiguration ist das Kerberos-Ticket eines Benutzers auf die verbleibende Zeit seiner Rollenaktivierung zeitlich begrenzt.

    >[!Note]
    Wenn Sie die Gesamtstruktur-Funktionsebene „Windows Server 2012 R2“ in Ihrer CORP-Domäne beibehalten möchten, wird empfohlen, auf dem Domänencontroller der CORP-Domäne [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) und [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) zu installieren.

- **Heraufstufung von privilegierten Konten in exklusive Gruppen für die Gesamtstruktur „PRIV“ (geschützt):** Administratoren können jetzt den MIM-Dienst über Gruppen und Benutzer informieren, die ausschließlich in der Gesamtstruktur „PRIV“ vorhanden sind. Auf diese Weise können diese Gruppen und Benutzer in PAM-Rollen eingeschlossen werden.  Sie können dann für eine Rolle aktiviert werden und eine Gruppenmitgliedschaft in der Gesamtstruktur „PRIV“ zugewiesen bekommen.

- **PAM-Bereitstellungsskripts:** PAM-Bereitstellungsskripts ermöglichen Administratoren die Optimierung der Installation der PAM-Umgebung.

- **PAM-Cmdlets für die Konfiguration des Authentifizierungsrichtliniensilos:** Servicepack 1 führt neue Cmdlets ein, um die Sicherheit Ihrer geschützten Gesamtstruktur zu verbessern. Diese Cmdlets erstellen automatisch ein Authentifizierungsrichtliniensilo, das an eine Authentifizierungsrichtlinienvorlage gebunden ist.

    >[!Note]
    Diese Cmdlets werden automatisch als Teil der Bereitstellungsskripts ausgeführt.


## <a name="platform-support"></a>Plattformunterstützung
Aktualisierte Informationen zur Plattformunterstützung finden Sie im Dokument [Unterstützte Plattformen für MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).  Zu den neuen Plattformen, die in diesem Servicepack unterstützt werden, gehören SQL Server 2016 und SharePoint 2016.

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>In dieser Version behobene Probleme aus der allgemein verfügbaren Version von MIM 2016

### <a name="pam"></a>PAM
- „New-PAMGroup“ hat in der PRIV-Gesamtstruktur keine MIM-Objekte für lokale Domänengruppen erstellt.
- „New-PAMDomainConfiguration“ ist mit einer „netdom“-Fehlermeldung fehlgeschlagen.
- PAM-Überwachungsdienst hat Warnungen für Gruppen in der PRIV-Gesamtstruktur protokolliert.

## <a name="how-to-upgrade-to-service-pack-1"></a>Upgrade auf Service Pack 1

Kunden, die ein Upgrade auf Microsoft Identity Manager 2016 Service Pack 1 durchführen, sollten die folgende Anleitung für alle Dienste in ihrer Bereitstellung beachten.

>[!Note]
>Kunden, die Forefront Identity Manager 2010 R2 SP1 oder eine frühere Version ausführen, müssen ihre Umgebung zunächst auf Microsoft Identity Manager 2016 (veröffentlicht im August 2015) aktualisieren und dann die folgenden Schritte ausführen.

Vorbereitung

Vor dem Upgrade des MIM-Dienstes und -Portals müssen Sie das MIM-Synchronisierung-Modul aktualisieren.
Sie müssen die MIM-Dienst- und MIM-Synchronisierungsdatenbanken sichern.

  1. Deinstallieren Sie die Microsoft Identity Manager-Komponente, die Sie aktualisieren möchten.
  2. Nachdem die Deinstallation abgeschlossen ist, öffnen Sie die Splash-Seite auf dem Installationsmedium „FIMSplash.htm“.
  3. Wählen Sie die zu aktualisierende MIM-Komponente aus.
  4. Setzen Sie die Installation fort und befolgen Sie die Aufforderungen.
    * Installation von MIM-Dienst und -Portal: Wenn Sie Exchange Online als E-Mail-Konto auswählen, geben Sie auf dem nächsten Bildschirm die E-Mail-Adresse und die Anmeldeinformationen des Exchange Online-Kontos ein.

