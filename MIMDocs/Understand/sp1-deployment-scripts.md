---
title: "Bereitstellungsskripts für MIM2016 SP1 PAM"
description: "Bereiten Sie die CORP-Domäne mit vorhandenen oder neuen Identitäten vor, die vom Privileged Identity Manager mithilfe von Skripts verwaltet werden sollen."
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 99b1ff9f622ddd357866b2a3f9f4cc8e0fc88005
ms.openlocfilehash: 82500ff42e24f5b155bfdd336566a2cd3d87fe7e


---

# Bereitstellungsskripts für MIM2016 SP1 PAM

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



<!--HONumber=Oct16_HO1-->


