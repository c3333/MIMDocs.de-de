---
title: 'Schritt 4: Konfigurieren von SharePoint'
description: Dies ist Schritt 4 der PAM-Konfiguration mithilfe von Skripts. In diesem Schritt konfigurieren Sie SharePoint so, dass es als Teil Ihrer PAM-Bereitstellung verwendet werden kann.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 17776b882b6a3f67313e2e41b424cbdaf22b6a44
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043799"
---
# <a name="step-4-configuring-sharepoint"></a>Schritt 4: Konfigurieren von SharePoint

> [!div class="step-by-step"]
> [« Schritt 3](sp1-step3-installing-configuring-sql.md)
> [Schritt 5 »](sp1-step5-configuring-pam.md)

SharePoint muss SharePoint Foundation 2013 mit SP1 sein.

Melden Sie sich auf Servern, die mit einer Domäne verknüpft sind, mit dem MIMAdmin-Konto an.

1. Führen Sie PowerShell als Administrator aus.
2.  .\PAMDeployment.ps1
3.  Wählen Sie Menüoption 4 (SharePoint-Setup)


Für Arbeitsgruppenserver

1. Führen Sie PowerShell als Administrator aus.
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Wählen Sie Menüoption 4 (SharePoint-Setup)

Der Computer wird während der Installation von SharePoint mehrmals neu gestartet. Stellen Sie bei jeder erneuten Ausführung des SharePoint-Setups sicher, dass die Anmeldung mit dem MIMAdmin-Konto erfolgt.
Wenn der Computer, auf dem SharePoint installiert wird, nicht mit dem Internet verbunden ist, um erforderliche Komponenten herunterzuladen, können diese auch unabhängig heruntergeladen und in einem lokalen Ordner abgelegt werden. **Dieser lokale Ordnerpfad muss in der Datei „PAMConfiguration.xml“ in <PrerequisitesBinaryLocation/> aktualisiert werden.** Die Links zum Herunterladen der Dateien finden Sie in Anhang 5.
Nach der Installation wird die GUI der SharePoint-Konfiguration geöffnet, und Sie werden durch die Schritte zum Abschließen der SharePoint-Installation geführt. Wählen Sie vollständige Serverinstallation aus, und führen Sie die restlichen Schritte auf der Benutzeroberfläche durch. Nach der Installation werden Sie aufgefordert, den Konfigurations-Assistenten auszuführen. Führen Sie folgende Schritte aus.

1. Wechseln Sie auf der Registerkarte **Verbindung mit einer Serverfarm herstellen** zu **Eine neue Serverfarm erstellen**.
2. Geben Sie den **SQL-Server** als Datenbankserver für die Konfigurationsdatenbank und das **SharePoint-Dienstkonto** als Datenbankzugriffskonto für SharePoint an.
3. Geben Sie ein Kennwort als Passphrase der Farmsicherheit an **(die Passphrase wird später nicht verwendet)** .
4. Übernehmen Sie die weiteren Standardeinstellungen des SharePoint-Konfigurations-Assistenten, um eine Einzelserverfarm zu erstellen.

Details finden Sie im Abschnitt **Konfigurieren von SharePoint** in [Schritt 3: Vorbereiten ein PAM-Servers](/microsoft-identity-manager/pam/step-3-prepare-pam-server). Wenn der Vorgang abgeschlossen ist, führen Sie das Skript „\PAMDeployment.ps1“ erneut aus und wählen Option 4 (SharePoint-Setup), um diesen Schritt abzuschließen.

> [!div class="step-by-step"]
> [« Schritt 3](sp1-step3-installing-configuring-sql.md)
> [Schritt 5 »](sp1-step5-configuring-pam.md)
