---
title: Bereitstellen von MIM 2016 | Microsoft Docs
description: "Hier erhalten Sie die vollständige Liste der für die Bereitstellung von Microsoft Identity Manager 2016 notwendigen Schritte, von der Vorbereitung der Umgebung bis zur Konfiguration der Portale."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 09/07/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 7fdad09650af245157264778d2732ac4327f25b5


---

# <a name="deploy-mim-2016"></a>Bereitstellen MIM 2016
Die Artikel in diesem Abschnitt bieten Schritt-für-Schritt-Anleitungen für die Bereitstellung von Microsoft Identity Manager (MIM) 2016 für Endbenutzer-Self-Service-Szenarien. Es wird dabei von einer Bereitstellung auf einem neuen Server ausgegangen, auf dem zuvor weder FIM noch MIM bereitgestellt wurden.

> [!NOTE]
> Die in diesem Abschnitt beschriebene Bereitstellungstopologie ist nur für die ersten Schritte und das Kennenlernen von MIM vorgesehen.  Die [Anleitung zur Kapazitätsplanung](/microsoft-identity-manager/plan-design/capacity-planning-guide) bietet weitere Informationen zu Topologien für Produktionsbereitstellungen.  Es wird empfohlen, dass Sie diese Dokumentation vor der Bereitstellung von MIM für die Produktion lesen.

Das Szenario für die privilegierte Zugriffsverwaltung wird anders als andere MIM-Szenarien bereitgestellt, da es eine dedizierte geschützte Gesamtstrukturumgebung erfordert.  Weitere Informationen zum Bereitstellen von MIM für Privileged Access Management finden Sie unter [Konfigurieren der MIM-Umgebung für Privileged Access Management](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam).

Der Vorgang zum Bereitstellen von MIM 2016 ähnelt sehr dem des Vorgängers, FIM 2010 R2. Wenn Sie die Dokumentation für FIM zurate ziehen möchten, lesen Sie das [Bereitstellungshandbuch für Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Erstens: Vorbereiten einer Domäne
MIM funktioniert über Active Directory (AD). Führen Sie daher diese Schritte aus, um Ihre Active Directory-Domänencontroller zu konfigurieren.
- [Einrichten der Domäne](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>Nächster Schritt: Vorbereiten eines Servers für die Identitätsverwaltung
Sobald Ihre Domäne eingerichtet und konfiguriert ist, bereiten Sie den Server vor, der Ihre Corporate Identity verwaltet. Dies beinhaltet:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optional)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>Installieren Sie zu guter Letzt die Komponenten von Microsoft Identity Manager 2016.
Nachdem Sie die Domäne und den Server eingerichtet haben, können Sie die MIM-Komponenten installieren und konfigurieren, um sie mit Active Directory zu synchronisieren.
- [MIM-Synchronisierungsdienst](install-mim-sync.md)
- [MIM-Dienst und -Portal](install-mim-service-portal.md)
- [Synchronisieren von Active Directory und MIM-Dienstdatenbanken](install-mim-sync-ad-service.md)



<!--HONumber=Nov16_HO2-->


