---
title: Verwenden von Azure Multi-Factor Authentication-Server-SDK zum Aktivieren von PAM oder in SSPR-Szenarios | Microsoft-Dokumentation
description: Richten Sie ein Azure Multi-Factor Authentication-Server-SDK als zweite Sicherheitsebene ein, wenn Benutzer Rollen in Privileged Access Management und der Self-Service-Kennwortzurücksetzung aktivieren.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: b92a217dd86d9e4de177ebec9ecec7c76222d7b1
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358277"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Use Azure Multi-Factor Authentication Server to activate PAM or SSPR (Verwenden eines Azure Multi-Factor Authentication-Servers zum Aktivieren von PAM oder SSPR)
Das folgende Dokument beschreibt, wie Sie einen Azure Multi-Factor Authentication-Server als zweite Sicherheitsebene einrichten, wenn Benutzer Rollen in Priviledge Access Management oder der Self-Service-Kennwortzurücksetzung aktivieren.

> [!IMPORTANT]
> Zur Ankündigung der Außerkraftsetzung des Azure MFA SDK (Multi-Factor Authentication Software Development Kit): Das Azure MFA SDK wird für Bestandskunden bis zur Ausmusterung am 14. November 2018 unterstützt. Neue Kunden und aktuelle Kunden können das SDK nicht mehr über das klassische Azure-Portal herunterladen. Zum Herunterzuladen müssen Sie sich an den Azure-Kundensupport wenden, um ein für Sie generiertes Paket mit MFA-Dienstanmeldeinformationen zu erhalten. <br> Das Microsoft-Entwicklungsteam arbeitet an Änderungen der MFA durch die Integration in das Multi-Factor Authentication-Server-SDK.

In diesem Artikel wird das Konfigurationsupdate und Schritte zum Aktivieren einer einfachen Umstellung vom Azure MFA-SDK zum Azure Multi-Factor Authentication-Server-SDK beschrieben, da dies bei Veröffentlichung in einem zukünftigen Hotfix enthalten sein wird. Ankündigungen finden Sie im [Versionsverlauf](/reference/version-history.md). 

## <a name="prerequisites"></a>Voraussetzungen

Um Azure Multi-Factor Authentication-Server mit MIM verwenden zu können, benötigen Sie Folgendes:

- Internetzugriff von jedem MIM-Dienst oder MFA-Server aus, der PAM und SSPR bereitstellt, um die Verbindung zum Azure MFA-Dienst herzustellen
- Ein Azure-Abonnement
- Azure MFA-SDK wird bereits zum Installieren verwendet.
- Azure Active Directory Premium-Lizenzen für Kandidatenbenutzer oder ein alternatives Verfahren zum Lizenzieren von Azure MFA
- Telefonnummern für alle Kandidatenbenutzer
- MIM-Hotfix 4.5 oder höher: Weitere Informationen zu Ankündigungen finden Sie unter [Versionsverlauf](/reference/version-history.md).

## <a name="azure-multi-factor-authentication-server-configuration"></a>Konfiguration von Azure Multi-Factor Authentication-Server 
> [!NOTE] 
> Für die Konfiguration benötigen Sie ein gültiges SSL-Zertifikat, das für das SDK installiert. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Schritt 1: Herunterladen von Multi-Factor Authentication-Server im Azure-Portal 
Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und laden Sie den Azure MFA-Server herunter.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Schritt 2: Generieren von Aktivierungsanmeldeinformationen
Verwenden Sie zum Generieren von Aktivierungsanmeldeinformationen den Link **Generieren von Aktivierungsanmeldeinformationen, mit denen die Verwendung initiiert werden kann**. Speichern Sie diese Informationen für später.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Schritt 3: Installieren von Azure Multi-Factor Authentication-Server
[Installieren](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) Sie den Server, nachdem Sie diesen heruntergeladen haben.  Ihre Aktivierungsanmeldeinformationen werden benötigt. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Schritt 4: Erstellen Ihrer IIS-Webanwendung, die das SDK hostet
1. Öffnen Sie den IIS-Manager. ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Erstellen Sie eine neue Website, rufen Sie „MIM MFASDK“ ab, und verknüpfen Sie es mit einem leeren Verzeichnis. ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Öffnen Sie die Multi-Factor Authentication-Konsole, und klicken Sie auf das Webdienst-SDK. ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG).
4. Wenn es unterstützt wird, klicken Sie in der Konfiguration auf „MIM MFASDK“ und „App-Pool“.

> [!NOTE] 
> Der Assistent fordert das Erstellen einer Administratorengruppe. Weitere Informationen finden Sie in der Azure-Dokumentation > MFA Azure Microsoft Azure Multi-Factor Authentication-Server.

5. Als nächstes muss das MIM-Dienstkonto importiert werden. Öffnen Sie die Multi-Factor Authentication-Konsole, und klicken Sie auf „Benutzer“. 1. Klicken Sie auf „Importieren aus Active Directory“. 2. Navigieren Sie zum Dienstkonto namens „contoso\mimservice“. 3. Klicken Sie auf „Importieren“ und „Schließen“. ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Bearbeiten Sie das MIM-Dienstkonto, um es in der Multi-Factor Authentication-Verwaltungskonsole zu aktivieren. ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Aktualisieren Sie die IIS-Authentifizierung auf der „MIM MFASDK“-Website. Zuerst müssen Sie die „Anonyme Authentifizierung“ deaktivieren und dann die „Windows-Authentifizierung“ aktivieren. ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Letzter Schritt: Fügen Sie „PhoneFactor Admins“ dem MIM-Dienstkonto hinzu. ![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Konfigurieren des MIM-Diensts für Azure Multi-Factor Authentication-Server 

### <a name="step-1-patch-server-to-452020"></a>Schritt 1: Aktualisieren Sie den Server auf Version 4.5.202.0.
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Schritt 2: Sichern Sie die unter „C:\Programme\Microsoft Forefront Identity Manager\2010\Service“ gespeicherte MfaSettings.xml-Datei, und öffnen Sie diese.

### <a name="step-3-update-the-following-lines"></a>Schritt 3: Aktualisieren Sie die folgenden Zeilen:
1. 3.1 Löschen/Entfernen Sie die folgenden Konfigurationeintragszeilen: <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. 3.2 Aktualisieren Sie die folgenden Zeilen, oder fügen Sie diese der MfaSettings.xml-Datei hinzu. <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Starten Sie den MIM-Dienst neu, und testen Sie die Funktion mit Azure Multi-Factor Authentication-Server.

> [!NOTE] 
> Ersetzen Sie zum Wiederherstellen Ihrer Einstellung MfaSettings.xml durch die Sicherungsdatei aus Schritt 2.


## <a name="next-steps"></a>Nächste Schritte

-    [Erste Schritte mit Azure Multi-Factor Authentication-Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Was ist Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Use Custom Multi-Factor Authentication API to activate PAM or SSPR (Verwenden einer benutzerdefinierten Multi-Factor Authentication-API zum Aktivieren von PAM oder SSPR)](Working-with-custommfaserver-for-mim.md)
- [MIM version release history (Versionsverlauf der MIM-Version)](./reference/version-history.md)
