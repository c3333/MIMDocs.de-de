---
title: Verwenden von Azure MFA zur Aktivierung von PAM | Microsoft Docs
description: Richten Sie Azure MFA als zweite Sicherheitsebene ein, wenn Benutzer Rollen in Privileged Access Management aktivieren.
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: dd77c0135bee40a90f3ea9fd5c1b2771cbc21793
ms.sourcegitcommit: c049dceaf02ab8b6008fe440daae4d07b752ca2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2017
---
# <a name="using-azure-mfa-for-activation"></a>Verwenden von Azure MFA zur Aktivierung

Beim Konfigurieren einer PAM-Rolle können Sie auswählen, wie Benutzer, die eine Aktivierung der Rolle anfordern, sich autorisieren müssen. Die PAM-Autorisierungsaktivität implementiert diese Wahlmöglichkeiten:

- Rolle Besitzergenehmigung
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Wenn keine Option aktiviert ist, werden Kandidatenbenutzer automatisch für ihre Rolle aktiviert.

Microsoft Azure Multi-Factor Authentication (MFA) ist ein Authentifizierungsdienst, der von Benutzern verlangt, ihre Anmeldeversuche über eine mobile App, einen Telefonanruf oder eine Textnachricht zu verifizieren. Der Dienst kann mit Microsoft Azure Active Directory sowie als Dienst für Cloud- und lokale Unternehmensanwendungen verwendet werden. Für das PAM-Szenario bietet Azure MFA einen zusätzlichen Authentifizierungsmechanismus. Azure MFA kann für die Autorisierung verwendet werden, unabhängig davon, wie ein Benutzer in der Windows PRIV-Domäne authentifiziert ist.

## <a name="prerequisites"></a>Voraussetzungen

Um Azure MFA mit MIM zu verwenden, benötigen Sie Folgendes:

- Internetzugriff von jedem MIM-Dienst aus, der PAM bereitstellt, um die Verbindung zum Azure MFA-Dienst herzustellen
- Ein Azure-Abonnement
- Azure Active Directory Premium-Lizenzen für Kandidatenbenutzer oder ein alternatives Verfahren zum Lizenzieren von Azure MFA
- Telefonnummern für alle Kandidatenbenutzer

## <a name="creating-an-azure-mfa-provider"></a>Erstellen eines Azure MFA-Anbieters

In diesem Abschnitt richten Sie Ihren Azure MFA-Anbieter in Microsoft Azure Active Directory ein.  Wenn Sie Azure MFA bereits verwenden, sei es eigenständig oder in Verbindung mit Azure Active Directory Premium konfiguriert, fahren Sie mit dem nächsten Abschnitt fort.

1.  Öffnen Sie einen Webbrowser, und melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Azure-Abonnementadministrator an.

2.  Klicken Sie in der unteren linken Ecke auf **Neu**.

3.  Klicken Sie auf **App-Dienste > Active Directory > Anbieter für mehrstufige Authentifizierung > Schnellerfassung**.

4.  Geben Sie im Feld **Name** den Wert **PAM**ein, und wählen Sie im Feld „Nutzungsmodell“ den Wert „Pro aktiviertem Benutzer“ aus. Wenn Sie bereits über ein Azure AD-Verzeichnis verfügen, wählen Sie es aus. Klicken Sie abschließend auf **Erstellen**.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Herunterladen der Azure MFA-Dienstanmeldeinformationen

Im nächsten Schritt generieren Sie eine Datei mit den Authentifizierungsdaten, die von PAM zum Kontaktieren von Azure MFA benötigt werden.

1. Öffnen Sie einen Webbrowser, und melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Azure-Abonnementadministrator an.

2.  Klicken Sie im Azure-Portalmenü auf **Active Directory** , und klicken Sie dann auf die Registerkarte **Anbieter für mehrstufige Authentifizierung** .

3.  Klicken Sie auf den Azure MFA-Anbieter, den Sie für PAM verwenden möchten, und klicken Sie dann auf **Verwalten**.

4.  Klicken Sie im neuen Fenster im linken Bereich unter **Konfigurieren**auf **Einstellungen**.

5.  Klicken Sie im Fenster **Azure Multi-Factor Authentication** unter **Downloads** auf **SDK**.

6.  Klicken Sie in der Spalte „ZIP“ auf den Link **Herunterladen** für die Datei mit der Sprache **SDK for ASP.net 2.0 C\#**.

![Herunterladen eines Multi-Factor Authentication SDK – Screenshot](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Kopieren Sie die resultierende ZIP-Datei auf alle Systeme, auf denen der MIM-Dienst installiert ist. 

>[!NOTE]
> Die ZIP-Datei enthält Schlüsselmaterial, das zur Authentifizierung beim Azure MFA-Dienst verwendet wird.

## <a name="configuring-the-mim-service-for-azure-mfa"></a>Konfigurieren des MIM-Diensts für Azure MFA

1.  Melden Sie sich bei dem Computer, auf dem der MIM-Dienst installiert ist, als Administrator oder als der Benutzer an, der MIM installiert hat.

2.  Erstellen Sie einen neuen Verzeichnisordner unterhalb des Verzeichnisses, in dem der MIM-Dienst installiert ist, z. B. ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Navigieren Sie in Windows Explorer zum ```pf\certs```-Ordner der ZIP-Datei, die Sie im vorherigen Abschnitt heruntergeladen haben. Kopieren Sie die Datei „```cert\_key.p12```“ in das neue Verzeichnis.

4.  Navigieren Sie in Windows Explorer zum ```pf```-Ordner der ZIP-Datei, und öffnen Sie die Datei „```pf\_auth.cs```“ in einem Text-Editor wie z B. WordPad.

5. Suchen Sie diese drei Parameter: ```LICENSE\_KEY```, ```GROUP\_KEY``` und ```CERT\_PASSWORD```.

![Kopieren der Werte aus der Datei „pf\_auth.cs“ – Screenshot](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Öffnen Sie in einem Editor die Datei **MfaSettings.xml**, die sich in ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service``` befindet.

7. Kopieren Sie die Werte aus den Parametern LICENSE\_KEY, GROUP\_KEY und CERT\_PASSWORD der Datei „pf\_auth.cs“ in die zugehörigen XML-Elemente in der Datei „MfaSettings.xml“.

8. Geben Sie im XML-Element **<CertFilePath>** den vollständigen Pfadnamen der zuvor extrahierten Datei „cert\_key.p12“ an.

9. Geben Sie im Element **<username>** einen beliebigen Benutzernamen ein.

10. Geben Sie im Element **<DefaultCountryCode>** die Landesvorwahl für Anrufe bei Ihren Benutzern an, z. B. „49“ für Deutschland. Dieser Wert wird für den Fall verwendet, dass Benutzer mit Telefonnummern registriert werden, die keine Landesvorwahl enthalten. Wenn die Telefonnummer eines Benutzers eine internationale Landesvorwahl aufweist, die sich von der für die Organisation konfigurierten unterscheidet, muss die betreffende Landesvorwahl in der registrierten Telefonnummer enthalten sein.

11. Speichern und überschreiben Sie die Datei **MfaSettings.xml** im MIM-Dienstordner ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> Stellen Sie am Ende des Vorgangs sicher, dass weder die Datei **MfaSettings.xml** noch alle Kopien von ihr oder die ZIP-Datei öffentlich lesbar sind.

## <a name="configure-pam-users-for-azure-mfa"></a>Konfigurieren von PAM-Benutzern für Azure MFA

Damit ein Benutzer eine Rolle aktivieren kann, für die Azure MFA erforderlich ist, muss die Telefonnummer des Benutzers in MIM gespeichert sein. Die Festlegung dieses Attributs erfolgt auf zwei Arten.

Beim ersten Verfahren kopiert der Befehl `New-PAMUser` ein Telefonnummerattribut aus dem Verzeichniseintrag des Benutzers in der CORP-Domäne in die MIM-Dienstdatenbank. Beachten Sie, dass es sich dabei um einen einmaligen Vorgang handelt.

Beim zweiten Verfahren aktualisiert der Befehl `Set-PAMUser` das Telefonnummerattribut in der MIM-Dienstdatenbank. Beispielsweise ersetzt der folgende Befehl die Telefonnummer eines vorhandenen PAM-Benutzers im MIM-Dienst. Der Verzeichniseintrag bleibt unverändert.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Konfigurieren von PAM-Rollen für Azure MFA

Sobald die Telefonnummern aller Kandidatenbenutzer für eine PAM-Rolle in der MIM-Dienstdatenbank gespeichert ist, kann die Rolle für die Anforderung von Azure MFA aktiviert werden. Dies erfolgt mithilfe der Befehle `New-PAMRole` oder `Set-PAMRole`. Beispiel:

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Azure MFA kann durch Festlegen des Parameters „-MFAEnabled 0“ im Befehl `Set-PAMRole` für eine Rolle deaktiviert werden.

## <a name="troubleshooting"></a>Problembehandlung

Die folgenden Ereignisse finden sich im Privileged Access Management-Ereignisprotokoll:

| ID  | Schweregrad | Generiert von | Beschreibung |
|-----|----------|--------------|-------------|
| 101 | Fehler       | MIM-Dienst            | Der Benutzer hat die Azure MFA nicht abgeschlossen (z. B. weil er nicht ans Telefon gegangen ist). |
| 103 | Informationen | MIM-Dienst            | Der Benutzer hat Azure MFA während der Aktivierung abgeschlossen.                       |
| 825 | Warning     | PAM-Überwachungsdienst | Die Telefonnummer wurde geändert.                                |

Wenn Sie weitere Informationen zu Fehlern bei Telefonanrufen (Ereignis 101) benötigen, können Sie auch einen Bericht von Azure MFA anzeigen oder herunterladen.

1.  Öffnen Sie einen Webbrowser, und melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als globaler Azure AD-Administrator an.

2.  Wählen Sie im Azure-Portalmenü **Active Directory** und dann die Registerkarte **Anbieter für mehrstufige Authentifizierung** aus.

3.  Wählen Sie den Azure MFA-Anbieter aus, den Sie für PAM verwenden, und klicken Sie dann auf **Verwalten**.

4.  Klicken Sie im neuen Fenster auf **Verwendung**und dann auf **Benutzerdetails**.

5.  Wählen Sie den Zeitraum aus, und aktivieren Sie das Kontrollkästchen neben dem **Namen** in der Spalte für weitere Berichte. Klicken Sie auf **Als CSV exportieren**.

6.  Wenn der MFA-Bericht generiert wurde, können Sie ihn im Portal anzeigen oder, wenn er sehr umfangreich ist, als CSV-Datei herunterladen. Die **SDK**-Werte in der Spalte **AUTH TYPE** zeigen Zeilen an, die als PAM-Aktivierungsanforderungen relevant sind: Dies sind Ereignisse, die von MIM oder anderer lokal ausgeführter Software stammen. Das Feld **USERNAME** stellt die GUID des Benutzerobjekts in der MIM-Dienstdatenbank dar. Wenn ein Aufruf nicht erfolgreich war, steht in der Spalte **AUTHD** der Wert**No**, und der Wert der Spalte **CALL RESULT** enthält die Details der Fehlerursache.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Erstellen Sie noch heute Ihr kostenloses Azure-Konto.](https://azure.microsoft.com/free/)