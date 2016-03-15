---
Titel: Bereitstellen von MIM
MS.Custom:
  - Identitätsmanagement
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
Autor: Kgremban
---
# Bereitstellen von MIM
Die Artikel in diesem Abschnitt bieten eine schrittweise Anleitung für die Bereitstellung von Microsoft Identity Manager (MIM) 2016 für Endbenutzer Self-service-Szenarien auf einen neuen Server, der nicht mit FIM noch MIM bereitgestellt wurde.

> [!NOTE]
> Die in diesem Abschnitt beschriebene Bereitstellungstopologie ist nur für die ersten Schritte und das Kennenlernen von MIM vorgesehen.  Die [Kapazität Planungshandbuch](/MIM/PlanDesign/capacity-planning-guide.html) finden Sie weitere Informationen zu Topologien für produktionsbereitstellungen.  Es wird empfohlen, dass Sie diese Dokumentation vor der Bereitstellung von MIM für die Produktion lesen.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## [Domänen-setup](preparing-domain.md)
MIM funktioniert mit Active Directory (AD), führen Sie folgende Schritte zum Einrichten der AD-Domäne, falls Sie noch nicht.

## [Server-setup](preparing-corporate-identity-management-server.md)
Verwenden Sie diesen Artikel zum Bereitstellen Ihrer corporate Identity Management-Server vorbereiten. Dies schließt Windows Server 2012 R2, SQL Server und SharePoint.

## [Installieren Sie MIM 2016-Komponenten](microsoft-identity-manager-2016-install-server-components.md)
Nachdem die Domäne und den Server eingerichtet sind, hilft Ihnen dieser Artikel erste Schritte mit der MIM-Dienste, und konfigurieren sie eine Synchronisierung mit Active Directory.
<!--HONumber=Mar16_HO2-->
