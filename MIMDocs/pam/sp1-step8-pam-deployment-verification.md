---
title: 'Schritt 8: Überprüfen der PAM-Bereitstellung'
description: Die skriptgesteuerte PAM-Bereitstellung umfasst Überprüfungsskripts, die ein PAM-Szenario ausführen können, um zu überprüfen, ob die PAM-Bereitstellung wie erwartet funktioniert.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d6b0327b39a76799b2943565dd0c3e00f55f745f
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379955"
---
# <a name="step-8-pam-deployment-verification"></a>Schritt 8: Überprüfen der PAM-Bereitstellung

> [!div class="step-by-step"]
> [« Schritt 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Nachtrag »](sp1-pam-deployment-addendum.md)

Das Bereitstellungspaket enthält Überprüfungsskripts, die ein PAM-Szenario ausführen können, um sicherzustellen, dass die PAM-Bereitstellung wie erwartet funktioniert.
Zum Verwenden der Bereitstellungsüberprüfung ändern Sie den Abschnitt „PAMDeploymentConfig.xml“ mit dem Namen <PamValidation/>.

>[!NOTE]
>Die Überprüfung erfordert einen Clientcomputer, der mit der CORP-Domäne verknüpft ist und auf dem die PAM-Clientkomponenten installiert sind. Skripts zum Installieren von Clients finden Sie im Nachtrag.

Der Name des Clientcomputers muss im <PAMValidationClient/>-Tag der Datei „PAMDeploymentConfig.xml“ aktualisiert werden. Die übrigen Daten im <PAMValidation/>-Knoten müssen nur bearbeitet werden, wenn sie zu Konflikten mit vorhandenen Benutzern/Gruppen führen, da bei dieser Überprüfung versucht wird, sie zu erstellen.
Führen Sie die folgenden Schritte aus, um die Überprüfung durchzuführen:

Schritt 1:

1. Melden Sie sich als Administrator der CORP-Domäne bei CORPDC an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Mit diesen Befehlen werden die erforderlichen Gruppen und Benutzer für die Überprüfung erstellt.

Schritt 2:

1. Melden Sie sich mit dem MIMAdmin-Konto auf dem PAM-Server an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Mit diesem Schritt werden die Benutzer und Gruppen in die PAM-Umgebung migriert.

Schritt 3:

1. Melden Sie sich als lokaler Administrator auf dem CORP-Client an.
2. Führen Sie PowerShell als Administrator aus.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


In diesem Schritt werden Sie zur Eingabe der CORPAdmin-Anmeldeinformationen aufgefordert. Sobald diese angegeben wurden, werden den Gruppen „Remotedesktopbenutzer“ und „Remoteverwaltungsbenutzer“ die erforderlichen Benutzer hinzugefügt.
Verwenden Sie auf dem CORP-Client den folgenden Befehl, um PowerShell als der PRIV-Benutzer zu öffnen, den Sie überprüfen. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Geben Sie im PowerShell-Fenster folgende Befehle ein:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Dadurch wird der Status der Anforderung angezeigt.
  Zunächst hat der Benutzer keinen Zugriff auf die Ressource. Nachdem der Benutzer der Rolle Just-In-Time hinzugefügt wurde, wird ihm Zugriff gewährt. Nachdem die Anforderungsdauer abgelaufen ist, hat der Benutzer keinen Zugriff mehr.
  Das Skript verwendet den Standardwert (11 Minuten) für die Zeit bis zum Ablauf der Anforderung.

> [!div class="step-by-step"]
> [« Schritt 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Nachtrag »](sp1-pam-deployment-addendum.md)
