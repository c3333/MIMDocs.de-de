---
title: "Bereitstellungsskripts für MIM2016 SP1 PAM"
description: "Diese Seite ist Teil einer Reihe von Artikeln über die Konfiguration des Privileged Identity Managers mithilfe von Skripts. Sie umfasst eine Liste mit Annahmen zur Umgebung."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Bereitstellungsskripts für MIM2016 SP1 PAM

In diesem Servicepack werden eine Reihe von Bereitstellungsskripts für eine einfachere Bereitstellung von PAM eingeführt. Diese Skripts sind im Downloadcenter verfügbar. Bevor Sie die Skripts verwenden, müssen Sie sicherstellen, dass die folgenden Annahmen für Ihre Umgebung zutreffen.

Wichtige Annahmen:
1. Das Betriebssystem auf allen Computern ist mindestens Windows Server 2012 R2. Wenn Sie Windows Server 2016 Technical Preview 5 testen, muss der PRIV-Domänencontroller mit dem Build TP5 installiert sein.
2. DNS muss so konfiguriert sein, dass die Namensauflösung zwischen Ihren Domänencontrollern und Komponentenservern automatisch erfolgt.
3. Binärdateien für die Installation müssen lokal auf den designierten Servern für die Installation von SQL, SharePoint und MIM verfügbar sein.
4. In der Umgebung werden auf drei dedizierten (physischen oder virtuellen) Computern CORPDC, PRIVDC und PAMSERVER unabhängig voneinander ausgeführt.
5. Für die Überprüfungsoption wird davon ausgegangen, dass ein dedizierter Clientcomputer für diesen Schritt vorhanden ist.

>[!NOTE]
>Wenn bei der Skriptausführung ein Problem auftritt, müssen Sie u. U. in den Protokollen nachsehen. Alle Skriptprotokolle werden in „% AppData%\MIMPAMInstall“ gespeichert. Komprimieren Sie den Ordner als ZIP-Datei, und senden Sie ihn zusammen mit Details zu Vorgang und Fehler im Rahmen Ihres Supportfalls ein.

Sind Sie bereit, mit den Bereitstellungsskripts von PAM zu beginnen? Beginnen Sie mit dem Artikel [Configure PAM using scripts (Konfigurieren von PAM mithilfe von Skripts)](./pam/sp1-pam-configure-using-scripts.md).
