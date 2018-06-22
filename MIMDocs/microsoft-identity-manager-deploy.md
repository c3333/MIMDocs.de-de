---
title: Erforderliche Schritte für die Bereitstellung von Microsoft Identity Manager 2016 | Microsoft-Dokumentation
description: Hier erhalten Sie die vollständige Liste der für die Bereitstellung von Microsoft Identity Manager 2016 notwendigen Schritte, von der Vorbereitung der Umgebung bis zur Konfiguration der Portale.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3460436682054acf5e9e1b186c3fa39faaa40a43
ms.sourcegitcommit: 8316fa41f06f137dba0739a8700910116b5575d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33079005"
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Bereitstellen von Microsoft Identity Manager 2016 SP1
Die Artikel in diesem Abschnitt bieten Schritt-für-Schritt-Anleitungen für die Bereitstellung von Microsoft Identity Manager (MIM) 2016 für Endbenutzer-Self-Service-Szenarien. Es wird dabei von einer Bereitstellung auf einem neuen Server ausgegangen, auf dem zuvor weder FIM noch MIM bereitgestellt wurden.

> [!NOTE]
> Die in diesem Abschnitt beschriebene Bereitstellungstopologie ist nur für die ersten Schritte und das Kennenlernen von MIM vorgesehen.  Die [Anleitung zur Kapazitätsplanung](capacity-planning-guide.md) bietet weitere Informationen zu Topologien für Produktionsbereitstellungen.  Es wird empfohlen, dass Sie diese Dokumentation vor der Bereitstellung von MIM für die Produktion lesen.

Das Szenario für die privilegierte Zugriffsverwaltung wird anders als andere MIM-Szenarien bereitgestellt, da es eine dedizierte geschützte Gesamtstrukturumgebung erfordert.  Weitere Informationen zum Bereitstellen von MIM für Privileged Access Management finden Sie unter [Konfigurieren der MIM-Umgebung für Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

Der Vorgang zum Bereitstellen von MIM ähnelt sehr dem des Vorgängers, FIM 2010 R2. Wenn Sie die Dokumentation für FIM zurate ziehen möchten, lesen Sie das [Bereitstellungshandbuch für Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Erstens: Vorbereiten einer Domäne
MIM funktioniert über Active Directory (AD). Führen Sie daher diese Schritte aus, um Ihre Active Directory-Domänencontroller zu konfigurieren.
- [Einrichten der Domäne](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>Nächster Schritt: Vorbereiten von Servern für die Identitätsverwaltung
Sobald Ihre Domäne eingerichtet und konfiguriert ist, bereiten Sie den Server vor, der Ihre Corporate Identity verwaltet. Dies beinhaltet:
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optional)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>Installieren Sie zum Schluss die Komponenten von Microsoft Identity Manager 2016 SP1.
Nachdem Sie die Domäne und den Server eingerichtet haben, können Sie die MIM-Komponenten installieren und konfigurieren, um sie mit Active Directory zu synchronisieren.
- [MIM-Synchronisierungsdienst](install-mim-sync.md)
- [MIM-Dienst und -Portal](install-mim-service-portal.md)
- [Synchronisieren von Active Directory und MIM-Dienstdatenbanken](install-mim-sync-ad-service.md)
- [Unterstützte Plattformen für MIM 2016](microsoft-identity-manager-2016-supported-platforms.md)
