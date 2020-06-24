---
title: Supportupdate für Azure AD Premium-Kunden, die Microsoft Identity Manager verwenden | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Azure AD Premium-Kunden nach dem 21. Januar 2021 den Support beanspruchen können.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: ea16fea5d69aff58b862468d4bebfb05100d037a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2020
ms.locfileid: "84749216"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Supportupdate für Azure AD Premium-Kunden, die Microsoft Identity Manager verwenden

Gilt für: Azure AD Premium, Microsoft Identity Manager (MIM)

Azure AD Premium-Kunden steht ab Juni 2020 der Standardsupport zur Verfügung. Dieser wird ab Januar 2021 für bestimmte Komponenten von [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) oder höher fortgesetzt, die die Azure AD-Integration ermöglichen. Dadurch wird der bereits vorhandene Support für Microsoft Identity Manager ergänzt, der über die [Richtlinie für feste Lebenszyklen](https://docs.microsoft.com//lifecycle/policies/fixed) und die Pläne für den [Unternehmenssupport](https://support.microsoft.com/help/4341255) geboten wird.

Der Standardsupport ist unter anderem für folgende MIM-Komponenten verfügbar:
- MIM Synchronization Service und Password Change Notification Service (PCNS)
- MIM-Dienst und -Portal, Add-Ins und Erweiterungen, Data Warehouse-Supportskripts und -Sprachpakete
- MIM-Connectors

Diese MIM-Komponenten füllen Active Directory Domain Services und mit einer Erweiterung Azure AD bis Azure AD Connect mit den Benutzern und Gruppen auf, die von einem lokalen Personalsystem oder anderen Datensatzsystemen bereitgestellt werden. Dadurch wird sichergestellt, dass Kunden, die Azure AD Premium mit lokalen Systemen verwenden, während der Migration Ihrer Identitätsverwaltungsprozesse von lokalen Systemen zu Azure AD weiterhin Support beanspruchen können. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Öffnen einer Supportanfrage im Azure-Portal

Azure AD Premium-Kunden steht eine zusätzliche Supportoption für Microsoft Identity Manager zur Verfügung. Sie können Support für die oben genannten Komponenten von Microsoft Identity Manager 2016 Service Pack 2 oder höher (Hotfix oder Update) über das Azure-Portal beanspruchen.

Kunden können mithilfe der Anweisungen unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) eine Azure-Supportanfrage erstellen:
1. Wählen Sie *Problemtyp: Technisch* aus.
1. Wechseln Sie zur Ansicht *Alle Dienste*.
1. Klicken Sie in der Dienstliste unter Azure Active Directory auf *User Provisioning and Synchronization* (Benutzerbereitstellung und -synchronisierung).
1. Wählen Sie *Problemtyp: Microsoft Identity Manager (MIM)* aus.
1. Wählen Sie den *Problemuntertyp* *Connectors*, *Dienst und Portal* oder *Synchronisierungs-Engine* aus.

![MIM-Supportanfrage erstellen](media/azure-active-directory-new-support-request.png)

Die MIM-Komponenten werden als Problemtypen in *Azure Active Directory User Provisioning and Synchronization* (Benutzerbereitstellung und -synchronisierung in Azure Active Directory) im Azure-Portal aufgeführt.

Für Anfragen, die über das Azure-Portal erstellt werden, ist der Standardsupport für Azure AD Premium-Kunden verfügbar, die folgende Komponenten von Microsoft Identity Manager 2016 Service Pack 2 oder höher ([Hotfix oder Update](reference/version-history.md)) nutzen: Synchronization Service, Password Change Notification Service (PCNS), Connectors, Dienst und Portal, Add-Ins und Erweiterungen Data Warehouse-Supportskripts und -Sprachpakete

## <a name="other-support-options"></a>Weitere Supportoptionen

MIM 2016 SP2, Build 4.6.34.0, wurde im Oktober 2019 veröffentlicht. Kunden wird dringend empfohlen, weiterhin ein vollständig unterstütztes Service Pack zu nutzen, damit sie stets die aktuellste und sicherste Produktversion verwenden. Weitere Informationen finden Sie in der [Lebenszyklusrichtlinie für Service Packs](https://support.microsoft.com/help/17138).

Für Kunden, die noch einen älteren MIM-Build verwenden oder keinen Azure-Support bzw. kein Azure-Abonnement für eine Suite mit Azure Active Directory Premium besitzen, und für Probleme mit anderen, nicht aufgeführten MIM-Komponenten, ist der Support weiterhin verfügbar. Die Supportrichtlinie wird in der [Richtlinie für feste Lebenszyklen](https://docs.microsoft.com/lifecycle/policies/fixed) erläutert. Dort finden Sie auch die Datumsangaben zum [Supportlebenszyklus für Microsoft Identity Manager 2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

Neben dem Azure-Support gibt es weitere Supportoptionen, die Organisationen nutzen können. Wenn Sie beispielsweise ein Microsoft Professional-Supportangebot besitzen, können Sie eine [neue Supportanfrage erstellen](https://support.microsoft.com/supportforbusiness/productselection). So wählen Sie die entsprechende MIM-Komponente aus:
1. Wählen Sie *Sicherheit* als Produktfamilie aus.
1. Wählen Sie das Produkt *Identity Manager 2016* aus.
