---
title: "Bereitstellungsskripts für MIM2016 SP1 PAM"
description: "Diese Seite ist Teil einer Reihe von Artikeln über die Konfiguration des Privileged Identity Managers mithilfe von Skripts. Sie umfasst eine Liste mit Annahmen zur Umgebung."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2017
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
