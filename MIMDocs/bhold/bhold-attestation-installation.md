---
title: Installation des BHOLD-Nachweises | Microsoft-Dokumentation
description: Durch BHOLD-Nachweismodule können Sie Prüfer festlegen und Überprüfungen durchführen.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519166"
---
# <a name="bhold-attestation-installation"></a>Installation des BHOLD-Nachweises

Durch das BHOLD-Nachweismodul können Sie Prüfer festlegen und wiederkehrende Überprüfungen der Beziehungen zwischen Benutzern und anwendungsspezifischen Berechtigungen und Konten durchführen.

## <a name="bhold-attestation-installation-requirements"></a>BHOLD-Nachweis – Installationsanforderungen

Vor der Installation des BHOLD-Nachweismoduls müssen Sie das BHOLD Core-Modul auf dem Server installieren, auf dem Sie das BHOLD-Nachweismodul installieren möchten. Weitere Informationen zum Installieren des BHOLD Core-Moduls finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Da die Kontakte des BHOLD-Nachweismoduls E-Mail-Nachrichten an Benutzer senden, muss Ihre Umgebung über einen SMTP-E-Mail-Server (Simple Mail Transfer Protocol) verfügen, z.B. Microsoft Exchange Server.

> [!IMPORTANT]
> Wenn Sie die BHOLD-Berichterstattung und den BHOLD-Nachweis installieren, müssen Sie die BHOLD-Berichterstattung vor dem BHOLD-Nachweis installieren.

## <a name="before-you-begin"></a>Vorbereitung

Bevor Sie mit der Installation des BHOLD-Nachweismoduls beginnen, müssen Sie darauf vorbereitet sein, die Informationen bereitzustellen, die der Assistent für das Setup des BHOLD-Nachweises erfordert, um die Installation abzuschließen. Das folgende Arbeitsblatt unterstützt Sie beim Aufzeichnen dieser Informationen, sodass Sie diese bereitstellen können, wenn sie benötigt werden.

| **Element**                                    | **Beschreibung**                                                                                                                                                                                                           | **Wert**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden eines Sicherheitsanbieters auf einer Domäne/einem Computer** | Bei der Auswahl dieser Option wird angegeben, dass die Active Directory Domain Services-Sicherheit den Zugriff auf BHOLD Core steuert.                                                                                                                | Aktivieren Sie das Kontrollkästchen. **Wichtig:** Die Installation schlägt fehl, wenn dieses Kontrollkästchen nicht aktiviert ist.                                                                                                                                                                                                                   |
| **Domäne**                                  | Gibt die Domäne an, die das Dienstkonto enthält, das Sie bei der Installation von BHOLD Core erstellt haben. Weitere Informationen finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Der Domänenname wird automatisch vom Assistenten bereitgestellt. Ändern Sie den Namen nur, wenn dieser falsch ist. **Wichtig:** Geben Sie den Domänennamen an, indem Sie den kurzen NetBIOS-Namen verwenden, nicht den vollqualifizierten Domänennamen (FQDN). Wenn der FQDN der Domäne beispielsweise „fabrikam.com“ ist, geben Sie den Domänennamen als „FABRIKAM“ an. |
| **Benutzer**                                    | Gibt den Anmeldenamen des BHOLD Core-Dienstbenutzerkontos an.                                                                                                                                                          | Geben Sie den Benutzerkontonamen hier ein:                                                                                                                                                                                                                                                                                    |
| **Passwort**                                | Gibt das Kennwort des Dienstbenutzerkontos an.                                                                                                                                                                       | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD-Nachweisinstallation

Melden Sie sich zum Installieren des BHOLD-Nachweismoduls als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das BHOLD-Nachweismodul installieren möchten:

- BholdAttestation<em>\<Version\></em>\_Release.msi

Ersetzen Sie *\<Version\>* durch die Versionsnummer der BHOLD-Nachweisversion, die Sie installieren.

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

## <a name="next-steps"></a>Nächste Schritte

- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)
