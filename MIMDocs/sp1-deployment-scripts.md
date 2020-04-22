---
title: Bereitstellungsskripts für MIM2016 SP1 PAM
description: Diese Seite ist Teil einer Reihe von Artikeln über die Konfiguration des Privileged Identity Managers mithilfe von Skripts. Sie umfasst eine Liste mit Annahmen zur Umgebung.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: eeee6473f7471d4c961a4f4d3113d1af73ddaffe
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044411"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Bereitstellungsskripts für MIM2016 SP1 PAM

In diesem Servicepack werden eine Reihe von Bereitstellungsskripts für eine einfachere Bereitstellung von PAM eingeführt. Diese Skripts sind im Downloadcenter verfügbar. Bevor Sie die Skripts verwenden, müssen Sie sicherstellen, dass Sie die nachstehenden Anforderungen erfüllen:

1. Das Betriebssystem auf allen Servern ist mindestens Windows Server 2012 R2.
2. DNS muss so konfiguriert sein, dass die Namensauflösung zwischen Ihren Domänencontrollern und Komponentenservern aktiviert wird.
3. Binärdateien für die Installation müssen lokal auf den designierten Servern für die Installation von SQL, SharePoint und MIM verfügbar sein.
4. In der Umgebung werden auf drei dedizierten (physischen oder virtuellen) Computern CORPDC, PRIVDC und PAMSERVER unabhängig voneinander ausgeführt.
5. Sie benötigen eine dedizierte Arbeitsstation für die Überprüfungsoption.

>[!NOTE]
>Wenn bei der Skriptausführung ein Problem auftritt, müssen Sie u. U. in den Protokollen nachsehen. Alle Skriptprotokolle werden in „% AppData%\MIMPAMInstall“ gespeichert. Komprimieren Sie den Ordner als ZIP-Datei, und senden Sie ihn zusammen mit Details zu Vorgang und Fehler im Rahmen Ihres Supportfalls ein.

Sind Sie bereit, mit den Bereitstellungsskripts von PAM zu beginnen? Beginnen Sie mit dem Artikel [Configure PAM using scripts (Konfigurieren von PAM mithilfe von Skripts)](./pam/sp1-pam-configure-using-scripts.md).
