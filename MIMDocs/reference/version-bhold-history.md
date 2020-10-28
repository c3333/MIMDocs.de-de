---
title: BHOLD-Versionsverlauf in Identity Manager | Microsoft-Dokumentation
description: In diesem Artikel werden die verschiedenen Änderungen beschrieben, die im Rahmen von Updates für BHOLD innerhalb von MIM 2016 vorgenommen wurden.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1ff755aef697b92206745426e7820526db0666d3
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760965"
---
# <a name="bhold-modules-version-release-history"></a>Versions Veröffentlichungs Verlauf der BHOLD-Module

Das Microsoft Identity Manager Team veröffentlicht regelmäßig Updates, die im [Versions](version-history.md) Verlauf aufgelistet sind. Dieser Artikel unterstützt Sie dabei, die Versionen zu verfolgen, die für BHOLD, eine Unterkomponente des Microsoft Identity Manager, veröffentlicht wurden. Sie können dann entscheiden, ob Sie auf die neueste Version aktualisieren müssen oder nicht.

## <a name="version-60360"></a>Version 6.0.36.0

- Status: 7. September 2017
- [Download](https://www.microsoft.com/en-us/download/details.aspx?id=55950)

### <a name="enhancements"></a>Erweiterungen 
BHOLD-Version 6.0.36.0 umfasst die folgenden Erweiterungen:

- Platt Form Update x86 auf x64.

### <a name="fixed-issues"></a>Behobene Probleme
Die folgenden Probleme wurden in BHOLD-Version 6.0.36.0 behoben.

#### <a name="bhold-core"></a>BHOLD Core

- Das Installationsprogramm für x86 auf x64 wurde aktualisiert.
- Die Weboberfläche zeigt nicht alle Berechtigungen an.
- Korrekturen an der B1Common-Bibliothek.
- Der Benutzer kann OrgUnit keine Rolle zuweisen, wenn die Rolle nur über wenige Berechtigungen mit Kardinalität verfügt.
- Die Berechtigungs Kardinalität funktioniert nicht für max. Benutzer Zahlen.

#### <a name="access-management-connector"></a>Zugriffsverwaltungsconnector

- –

#### <a name="bhold-integration-module"></a>BHOLD-Integrationsmodul

- Das Installationsprogramm für x86 auf x64 wurde aktualisiert.
- Speicherort des layoutordners.

#### <a name="bhold-model-generator"></a>BHOLD-Modell Generator

- Modell Rollen aus übergeordneten Abteilungen werden nicht geerbt.

#### <a name="bhold-analytics"></a>BHOLD-Analyse

- –

#### <a name="bhold-attestation"></a>BHOLD-Nachweis

- –

#### <a name="bhold-reporting"></a>BHOLD-Berichterstellung

- Das benutzerdefinierte Attribut "Mitarbeiter-ID" ist nicht verfügbar.
- Große Daten können nicht ordnungsgemäß mit einem 3-Datei-Satz geladen werden.

## <a name="next-steps"></a>Nächste Schritte

- [BHOLD concepts guide (Leitfaden für BHOLD-Konzepte)](../bhold/bhold-concepts-guide.md)
- [BHOLD-Installationshandbuch](../bhold/bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](mim2016-bhold-developer-reference.md)
- [MIM-Versionsverlauf](version-history.md)

