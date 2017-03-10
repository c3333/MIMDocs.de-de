---
title: Nachtrag
description: "Hierbei handelt es sich um den Nachtrag zu den Dokumenten zur skriptgesteuerten PAM-Bereitstellung. Der Nachtrag erläutert die Konfiguration der Domänen PRIV und CORP sowie die Einrichtung eines Clients zur Durchführung der Überprüfung und stellt Informationen zur Anforderung von Unterstützung bereit."
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
ms.openlocfilehash: f69fe68dc63323c0945a4902e34ea8153f938c02
ms.lasthandoff: 01/10/2017


---
# <a name="pam-deployment-scripts-addendum"></a>Nachtrag für Bereitstellungsskripts für PAM:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Nachtrag 1: Einrichten der PRIV-Domäne

Nach dem Entpacken der komprimierten Datei im Ordner „$env:SYSTEMDRIVE\PAM“ bearbeiten Sie die Datei „PAMDeploymentConfig.xml“, um Details zur PRIV-Gesamtstruktur hinzuzufügen. Aktualisieren Sie den DNS-Namen, den NetBIOS-Namen, den DC-Namen, den Datenbank-/Protokollpfad und den SYSVOL-Pfad. Aktualisieren Sie auch die Domänen- und den Gesamtstrukturmodus. Wenn Sie Windows Server Technical Preview 5 testen, legen Sie den Domänen- und Gesamtstrukturmodus auf „WinThreshold“ fest.

1. Melden Sie sich als Administrator auf dem DC der PRIV-Domäne an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wählen Sie Menüoption 9 (PRIV-Gesamtstruktureinrichtung)


Nach Abschluss des Vorgangs wird der DC automatisch neu gestartet. Das Administratorkennwort des Verzeichnisdienste-Wiederherstellungsmodus (Directory Services Restore Mode, DSRM) muss die folgenden Kriterien erfüllen:

  * Das Kennwort hat eine Länge von mindestens 15 Zeichen.
  * Das Kennwort enthält mindestens einen Kleinbuchstaben.
  * Das Kennwort enthält mindestens einen GROSSBUCHSTABEN.
  * Das Kennwort enthält mindestens eine Ziffer bzw. ein Sonderzeichen.

## <a name="addendum-2-setting-up-the-corp-domain"></a>Nachtrag 2: Einrichten der CORP-Domäne

Wenn Sie zunächst nur mit PAM beginnen und eine Testumgebung einrichten möchten, ermöglicht das Skript auch die Konfiguration einer CORP-Domäne. Nach dem Entpacken der komprimierten Datei im Ordner „$env:SYSTEMDRIVE\PAM“ bearbeiten Sie die Datei „PAMDeploymentConfig.xml“ und fügen die Details der Unternehmensgesamtstruktur hinzu. Aktualisieren Sie den DNS-Namen, den NetBIOS-Namen, den DC-Namen, den Datenbank-/Protokollpfad und den SYSVOL-Pfad. Die Funktionsebene sollte mindestens „Windows Server 2012 R2“.

1. Melden Sie sich als Administrator auf dem DC der CORP-Domäne an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wählen Sie Menüoption 10 (CORP-Gesamtstruktureinrichtung)

Der Domänencontroller wird nach Abschluss des Vorgangs automatisch neu gestartet.

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Nachtrag 3: Einrichten eines CORP-Clients für die Überprüfung

Der Speicherort für Binärdateien des Clients in der Konfigurationsdatei muss auf den Speicherort von „setup.exe“ verweisen.
Melden Sie sich als lokaler Administrator auf dem Client an, und führen Sie in einem PowerShell-Fenster mit erhöhten Rechten die folgenden Befehle aus:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Wählen Sie Menüoption 7 (MIM PAM-Clienteinrichtung)


Wenn der Computer nicht mit einer Domäne verknüpft ist, werden Sie zur Eingabe von CORP-Administratoranmeldeinformationen aufgefordert, um den Domänenbeitritt durchzuführen. Der Computer muss nach dem Domänenbeitritt neu gestartet werden. Melden Sie sich erneut als lokaler Administrator auf dem Client an, und führen Sie in einem PowerShell-Fenster mit erhöhten Rechten die folgenden Befehle aus:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Wählen Sie Menüoption 7 (MIM PAM-Clienteinrichtung)

Fahren Sie mit Schritt 8 (siehe oben) fort.

## <a name="addendum-4-if-something-goes-wrong"></a>Anhang 4: Beim Auftreten von Problemen

Alle Skriptprotokolle werden in „% AppData%\MIMPAMInstall“ gespeichert. Komprimieren Sie den Ordner als ZIP-Datei, und senden Sie ihn zusammen mit Details zum Vorgang und dem Fehler per E-Mail an [mim2016@microsoft.com](mailto:mim2016@microsoft.com).

