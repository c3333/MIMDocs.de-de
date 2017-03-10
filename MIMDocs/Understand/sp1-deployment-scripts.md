---
title: "Bereitstellungsskripts für MIM2016 SP1 PAM"
description: "Diese Seite ist Teil einer Reihe von Artikeln über die Konfiguration des Privileged Identity Managers mithilfe von Skripts. Sie umfasst eine Liste mit Annahmen zur Umgebung."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 10d06ae573e378797467ab1eb91e977d59b821d1
ms.lasthandoff: 01/10/2017


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
>Wenn bei der Skriptausführung ein Problem auftritt, müssen Sie u. U. in den Protokollen nachsehen. Alle Skriptprotokolle werden in „% AppData%\MIMPAMInstall“ gespeichert. Komprimieren Sie den Ordner als ZIP-Datei, und senden Sie ihn zusammen mit Details zum Vorgang und dem Fehler per E-Mail an mim2016@microsoft.com.

Sind Sie bereit, mit den Bereitstellungsskripts von PAM zu beginnen? Beginnen Sie mit dem Artikel [Configure PAM using scripts (Konfigurieren von PAM mithilfe von Skripts)](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts).

