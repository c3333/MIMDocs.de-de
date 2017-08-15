---
title: "Microsoft Identity Manager 2016 – Portalanpassungen | Microsoft-Dokumentation"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016 – Portalanpassung


>[!Warning]
Achten Sie darauf, dass Sie den Browsercache löschen, nachdem alle CSS-Anpassungen vorgenommen wurden.

In Microsoft Identity Manager 2016 (MIM) können Sie ausgewählte Elemente der Kennwortportale einschließlich Bannerlogo, Zeichenfolgenressourcen und Cascading Stylesheets anpassen.

Zu diesem Zweck müssen je nach Anpassungsstufe einige Voraussetzungen erfüllt sein. Die folgende Liste enthält Elemente, die mit dem Anpassen der Portale zur Registrierung und Zurücksetzung von Kennwörtern in MIM 2016 zu tun haben.

-   Ordner „Customizations“: Diesen Ordner untersucht MIM 2016 vor der Verwendung der Standardwerte. Jedes Portal, das angepasst wird, erfordert einen Ordner „Customizations“. Anpassungen sollten nur in diesem Ordner vorgenommen werden, weil das Setup ihn bei Upgrades, Modusänderungs- oder Modusreparaturinstallationen nicht überschreibt.

-   Strings.resources: Diese XML-basierte Datei ermöglicht Ihnen, die im Portal angezeigten Zeichenfolgen zu ändern. Diese Datei muss sich im Ordner „Customizations“ befinden.

-   Style.css: Dieses Cascading Stylesheet benutzen die Portale zur Anpassung. Dieses Stylesheet muss erstellt und geändert werden, um das Logo zu ändern, oder kann vollständig durch Ihre eigenen Anpassungen ersetzt werden.

Detaillierte, schrittweise Anweisungen zum Anpassen der Portale zur Registrierung und Zurücksetzung von Kennwörtern finden Sie unter „Test Lab Guide: Demonstrating MIM 2016 Password Registration and Reset Portal Customization“ (Testumgebungsanleitung: Demonstration der Anpassung der Portale zur Registrierung und Zurücksetzung von Kennwörtern in MIM 2016).

>[! WARNING] Damit MIM Anpassungsänderungen erkennen kann, müssen Sie IIS durch Ausführung von „iisreset“ neu starten.


## <a name="creating-the-customizations-folder"></a>Erstellen des Ordners „Customizations“

Beim Starten sucht MIM im Ordner „Customizations“ nach der Strings.resources-Datei, bevor die Standardwerte verwendet werden. Sie müssen einen Ordner „Customizations“ unter dem Verzeichnis für das Portal erstellen, das Sie anpassen möchten (d.h. Kennwortregistrierungsportal oder Kennwortzurücksetzungsportal). Wenn Sie beide Portale anpassen möchten, müssen Sie einen Ordner „Customizations“ unter jedem der folgenden erstellen:

-   C:\\Programme\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal

-   C:\\Programme\\Microsoft Forefront Identity Manager\\2010\\Password Reset Portal

### <a name="to-create-the-customization-folder"></a>So erstellen Sie den Ordner „Customizations“

1.  Navigieren Sie zum Ordner „C:\\Programme\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal“.

2.  Erstellen Sie einen Ordner mit dem Namen „Customizations“.

3.  Navigieren Sie wieder eine Ebene zurück zum Ordner „Password Reset Portal“, und erstellen Sie einen Ordner mit dem Namen „Customizations“.

## <a name="customizing-strings"></a>Anpassen von Zeichenfolgen

Viele der Zeichenfolgen in der Benutzeroberfläche des Portals können Sie anpassen, indem Sie eine Strings.resources-Datei erstellen und diese Datei dem Ordner „Customizations“ hinzufügen. Sie müssen für jedes Portal, das Sie anpassen möchten, eine Strings.resources-Datei erstellen.

### <a name="to-customize-strings"></a>So passen Sie Zeichenfolgen an

1.  Kopieren Sie mit dem Editor den folgenden Code hinein, und speichern Sie ihn im Ordner „Customizations“ als „Strings.resources“.

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Ändern Sie unter dem Abschnitt `<!-- Customizations begin here -->` den Datennamen, sodass er den Zeichenfolgen entspricht, die Sie anpassen möchten, und geben Sie den Wert für diese Zeichenfolge zwischen den <value></value>-Tags ein. Die folgende Liste enthält die Zeichenfolgen, die angepasst werden können, und ihre Standardwerte.

>[!NOTE]
Die Datei **Strings.resources** ist sprachneutral. Um sprachspezifische Zeichenfolgen zu erstellen, müssen Sie das entsprechende Sprachpaket installieren und die Datei im Format „Strings. `<language>-<culture>.resources`“ speichern, z.B. „Strings.en-us.resources“.

Die folgende Liste enthält Portalzeichenfolgen, die angepasst werden kann.

<a name="portal-strings"></a>Portalzeichenfolgen
--------------

| Name                                                     | Standardwert                                                                                                                                                                                                                                                                                                                                         | Kommentar                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Informationen zu         |       |
| ButtonCancel                                             | Abbrechen                                                                                 |     |
| ButtonFinish                                             | Fertig stellen    |    |
| ButtonNext                                               | Weiter    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | Ihre Sitzung ist nicht mehr aktiv. Sie können das Fenster schließen oder die Sitzung neu starten, indem Sie auf den folgenden Link klicken.         |      |
| CancelFinishedTitle                                      | Die Sitzung ist beendet                                                              |      |
| ErrorDescription_3000                                    | Ein Fehler ist aufgetreten. Versuchen Sie es erneut. Wenn das Problem weiterhin besteht, wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3000)       |          |
| ErrorDescription_3001                                    | Stellen Sie sicher, dass Sie den Benutzernamen korrekt eingegeben haben. Wenn Sie Ihr Kennwort dann immer noch nicht zurücksetzen können, wenden Sie sich an den      |           |
|                                                          | Helpdesk, um Unterstützung zu erhalten. (Fehler 3001)   |                   |
| ErrorDescription_3002                                    | Die Sitzung ist beendet. Kehren Sie zur Homepage zurück, um den Vorgang zu wiederholen. (Fehler 3002)                                     |                |
| ErrorDescription_3003                                    | Das aktuelle Benutzerkonto wird von Forefront Identity Manager nicht erkannt. Wenden Sie sich an den Helpdesk oder an den Systemadministrator. (Fehler 3003)           |               |
| ErrorDescription_3004                                    | Sie sind nicht berechtigt, sich für die Kennwortzurücksetzung zu registrieren. Wenden Sie sich an den Helpdesk oder an den Systemadministrator. (Fehler 3004)     |              |
| ErrorDescription_3005                                    | Mindestens eine der angegebenen Antworten stimmt nicht mit den bei der Kennwortregistrierung angegebenen Antworten überein. Damit Sie Ihr Kennwort zurücksetzen können, müssen die hier angegebenen Antworten mit den bei der Registrierung angegebenen Antworten übereinstimmen. Sie können den Vorgang über die Homepage wiederholen, oder wenden Sie sich an den Helpdesk oder an den Systemadministrator. (Fehler 3005) |           |
| ErrorDescription_3006                                    | Das eingegebene Kennwort ist falsch. Sie müssen das richtige Kennwort eingeben, um sich für die Kennwortzurücksetzung registrieren zu können. (Fehler 3006)            |              |
| ErrorDescription_3007                                    | Die Zurücksetzung Ihres Kennworts wird vorübergehend verweigert. Wiederholen Sie den Vorgang zu einem späteren Zeitpunkt, oder wenden Sie sich an den Helpdesk oder an den Systemadministrator, um Unterstützung zu erhalten. (Fehler 3007)     |         |
| ErrorDescription_3008                                    | Ein Fehler ist aufgetreten. Versuchen Sie es erneut. Wenn das Problem weiterhin besteht, wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3008)        |          |
| ErrorDescription_3009                                    | Die von Ihnen gemachte Eingabe enthält Text in einem Format, das nicht erlaubt ist. Versuchen Sie es erneut mit einer anderen Eingabe, oder wenden Sie sich an den Helpdesk bzw. den Systemadministrator. (Fehler 3009)     |             |
| ErrorDescription_3010_Registration                       | Skripting ist im Browser nicht aktiviert. Aktivieren Sie Skripting, und kehren Sie zur Startseite der Kennwortregistrierung zurück, oder wenden Sie sich an den Helpdesk.      |               |
| ErrorDescription_3010_Reset                              | Skripting ist im Browser nicht aktiviert. Aktivieren Sie Skripting, und kehren Sie zur Startseite der Kennwortzurücksetzung zurück, oder wenden Sie sich an den Helpdesk.     |            |
| ErrorDescription_3011                                    | Von dieser Seite werden Cookies verwendet. Konfigurieren Sie den Browser für die Annahme von Cookies, und versuchen Sie es erneut, oder wenden Sie sich an den Helpdesk.          |                                           |
| ErrorDescription_3012                                    | Die von Ihnen eingegebenen Daten entsprechen nicht dem Ihnen zugesendeten Sicherheitscode. Sie können versuchen, das Kennwort erneut zurückzusetzen, oder den Helpdesk um Unterstützung bitten.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Der Sicherheitscode kann nicht gesendet werden. Bitten Sie den Helpdesk um Unterstützung.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Geben Sie den Benutzernamen im richtigen Format ein.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Geben Sie zum Fortfahren einen Benutzernamen ein.                                              |                         |
| ErrorMessagePasswordRequired                             | Geben Sie ein Kennwort ein.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Stellen Sie sicher, dass beide Kennwörter übereinstimmen.                                |           |
| ErrorPageDefaultHeading                                  | Anwendungsfehler                                            |               |
| ErrorPageServerTime                                      | Serverzeit: {0:T}                     | {0} ist der Zeitpunkt, zu dem die Ausnahme abgefangen wurde. 'T' bewirkt, dass die übergebene Zeit als „lange Uhrzeit“ formatiert wird. Es werden Stunde, Minute und Sekunde sowie (abhängig vom aktuellen Gebietsschema) möglicherweise die AM/PM-Bezeichnung angezeigt. |
| ErrorPageTitle                                           | Forefront Identity Management – Kennwortfehler                     |   |
| ErrorTitle_3000                                          | Fehler                                  |  |
| ErrorTitle_3001                                          | Zugriff verweigert“                          |  |
| ErrorTitle_3002                                          | Die Sitzung ist beendet                          |  |
| ErrorTitle_3003                                          | Unbekannter Benutzer                      |  |
| ErrorTitle_3004                                          | Nicht autorisierter Benutzer                      |  |
| ErrorTitle_3005                                          | Antworten stimmen nicht überein                    |  |
| ErrorTitle_3006                                          | Falsches Kennwort                     |  |  
| ErrorTitle_3007                                          | Zugriff vorübergehend verweigert              |  |
| ErrorTitle_3008                                          | Kommunikationsfehler                    |  |
| ErrorTitle_3009                                          | Eingabe ist nicht zulässig                       |  |
| ErrorTitle_3010                                          | Fehler in der Browserkonfiguration            |  |
| ErrorTitle_3011                                          | Fehler in der Browserkonfiguration            |  |
| ErrorTitle_3012                                          | Fehler bei der Überprüfung                    |  |
| ErrorTitle_3013                                          | Der Sicherheitscode kann nicht gesendet werden   |  |
| FinalizeRegistrationHeading1                             | Wenn Sie später einmal Ihr Kennwort zurücksetzen müssen:        |   |
| FinalizeRegistrationSubHeading1                          | Besuchen Sie das Kennwortzurücksetzungsportal   |   |
| FinalizeRegistrationSubHeading2                          | Bestätigen Sie Ihre Identität.   |   |
| FinalizeRegistrationSubHeading3                          | Neues Kennwort auswählen    |     |
| FinishingDescription                                     | Neues Kennwort auswählen        |    |
| FinishingTitle                                           | Kennwort zurücksetzen:      |     |
| GotoPortalPrefix                                         | Wechseln zu     |        |
| GotoPortalSuffix                                         | Homepage     |      |
| LabelTroubleshootingLinkText                             | Details anzeigen           |    |
| LoadingText                                              | Wird geladen...     |  |
| NoScriptTagErrorMessage                                  | Skripting ist im Browser nicht aktiviert. Aktivieren Sie Skripting, und kehren Sie zur Startseite zurück, oder wenden Sie sich an den Helpdesk.      |   |
| PasswordResetOperationGeneralErrorMessage                | Fehler beim Zurücksetzen des Kennworts.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | Das Kennwort entspricht nicht den Kennwortrichtlinien Ihrer Organisation.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Fehler beim Zurücksetzen des Kennworts. Der Benutzer kann das Kennwort nicht ändern.    |   |
| PrivacyStatement                                         | Datenschutzerklärung                                                      |    |
| RegistrationDescription                                  | Self-Service-Kennwortregistrierung     |    |
| RegistrationMission                                      | Wenn Sie einmal Ihr Kennwort vergessen haben sollten, können Sie es selbst zurücksetzen, ohne sich an den Helpdesk wenden zu müssen.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management – Kennwortregistrierung                                          |   |
| RegistrationSteps                                        | Klicken Sie auf „Weiter“, um den Registrierungsvorgang zu starten.   |      |
| RegistrationSuccessDescription                           | Sie sind jetzt registriert        |   |
| RegistrationSuccessTitle                                 | Abgeschlossen:   |    |
| RegistrationWelcomeTitle                                 | Kennwortregistrierung:    | |
| ResetDescription                                         | Self-Service-Kennwortzurücksetzung  |    |
| ResetEnterNamePrompt                                     | Geben Sie unten Ihren Benutzernamen ein     |     |
| ResetEnterPassword                                       | Geben Sie ein neues Kennwort ein:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Beispiele:  |    |
| ResetPageTitle                                           | Forefront Identity Management – Kennwortzurücksetzung       |     |
| ResetReenterPassword                                     | Geben Sie das Kennwort erneut ein:    | |
| ResetSuccessDescription                                  | Ihr Kennwort wurde zurückgesetzt.    |    |
| ResetSuccessTitle                                        | Erfolgreich:                                |    |
| ResetUseNewPassword                                      | Sie können sich jetzt mit dem neuen Kennwort anmelden.     |      |
| ResetUsernameTextFormat                                  | (Kennwort zurücksetzen für {0})       | {0} ist die Anmeldung des Benutzers             |
| ResetWelcomeTitle                                        | Kennwort zurücksetzen:     |      |
| TroubleshootingEmailSubject                              | Details zum FIM-Anforderungsverarbeitungsfehler     |       |
| TroubleshootingLabelAttributes                           | Attribute:    |    |
| TroubleshootingLabelCloseButton                          | Schließen       |    |
| TroubleshootingLabelCopyToClipboard                      | In Zwischenablage kopieren     |     |
| TroubleshootingLabelCorrelationId                        | Korrelations-ID:      |          |
| TroubleshootingLabelDetails                              | Details:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | Die Informationen wurden in die Zwischenablage kopiert.           |    |
| TroubleshootingLabelRequestId                            | Anforderungs-ID:                  |    |
| TroubleshootingLabelSendEmail                            | Informationen per E-Mail senden                            |    |
| TroubleshootingLabelSource                               | Grund:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Anforderungsdetails anzeigen        |              |                                                    
| TroubleshootingLinkText                                  | Problembehandlungsinformationen          |                  | |



Die folgende Liste enthält Authentifizierungsgate-Zeichenfolgen, die angepasst werden können.

<a name="authentication-gate-strings"></a>Authentifizierungsgate-Zeichenfolgen
---------------------------

| Name                                          | Standardwert                                                                                                                                                                                                                | Kommentar |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | E-Mail-Adresse:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | Das Feld für die E-Mail-Adresse darf nicht frei gelassen werden.     |          |
| OTPEmailRegistrationFooterReadOnly            | Folgen Sie zum Aktualisieren Ihrer E-Mail-Adresse den Prozessen, die von Ihrer Organisation definiert sind, oder rufen Sie beim Helpdesk an.                   |          |
| OTPEmailRegistrationFooterReadWrite           | Die E-Mail-Adresse wird von Ihrer Organisation in Forefront Identity Manager gespeichert.                       |          |
| OTPEmailRegistrationGateTitle                 | Bestätigung mit E-Mail-Adresse   |          |
| OTPEmailRegistrationHeaderReadOnly            | Wenn Sie das Kennwort zurücksetzen müssen, wird ein Prüfsicherheitscode an Ihre E-Mail-Adresse gesendet. Wenn die unten angegebene E-Mail-Adresse nicht korrekt ist, müssen Sie diese aktualisieren, damit Sie die Self-Service-Kennwortzurücksetzung verwenden können. |          |
| OTPEmailRegistrationHeaderReadWrite           | Geben Sie unten Ihre E-Mail-Adresse ein. Wenn Sie das Kennwort zurücksetzen müssen, wird Ihnen eine E-Mail mit einem Prüfcode gesendet.                 |          |
| OTPEmailResetGateTitle                        | Überprüfen Ihrer Identität: E-Mail-Bestätigung         |          |
| OTPEmailResetHeader                           | Geben Sie unten Ihren Sicherheitscode ein. Ein Sicherheitscode wurde an die E-Mail-Adresse gesendet, die für diese Organisation registriert ist.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | Der angegebene Wert entspricht nicht dem erwarteten Format.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | Das Feld für den Sicherheitscode darf nicht frei gelassen werden.        |          |
| OTPResetVerificationLabel                     | Sicherheitscode:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Folgen Sie zum Aktualisieren Ihrer Mobiltelefonnummer den Prozessen, die von Ihrer Organisation definiert sind, oder rufen Sie beim Helpdesk an.   ||
| OTPSmsRegistrationFooterReadWrite                     | Die Mobiltelefonnummer wurde von Ihrer Organisation in Forefront Identity Manager gespeichert.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Mobiltelefonbestätigung                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Wenn Sie das Kennwort zurücksetzen müssen, wird ein Prüfsicherheitscode an Ihr Mobiltelefon gesendet. Wenn die unten angegebene Mobiltelefonnummer nicht korrekt ist, müssen Sie diese aktualisieren, damit Sie die Self-Service-Kennwortzurücksetzung verwenden können. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Geben Sie unten Ihre Mobiltelefonnummer ein. Wenn Sie das Kennwort zurücksetzen müssen, wird ein Prüfcode an Ihr Mobiltelefon gesendet.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Das Feld für die Mobiltelefonnummer darf nicht frei gelassen werden.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobiltelefon:                    |   |
| OTPSmsResetGateTitle                                  | Überprüfen Ihrer Identität: Mobiltelefonbestätigung         |   |
| OTPSmsResetHeader                                     | Geben Sie unten Ihren Sicherheitscode ein. Ein Sicherheitscode wurde an das Mobiltelefon gesendet, das für diese Organisation registriert ist.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Geben Sie unten Ihr aktuelles Kennwort ein, und klicken Sie dann auf „Weiter“.        |   |
| PasswordGateErrorMessagePasswordRequired              | Geben Sie Ihr aktuelles Kennwort ein.                  |   |
| PasswordGateGateTitle                                 | Ihr aktuelles Kennwort               |   |
| PasswordGatePasswordLabelText                         | Kennwort:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(angemeldet als: `<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Sie müssen mindestens {0} Fragen beantworten.        |   |
| QAGateIncorrectAnswer                                 | Ihre Antworten sind nicht richtig.       |   |
| QAGatePrivacyNotice                                   | Ihre Antworten werden von Ihrer Organisation in Forefront Identity Manager gespeichert.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Sie müssen zum Registrieren mindestens {0} Fragen beantworten.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Eine oder mehrere Antworten stimmen nicht mit der Richtlinie überein.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Diese Antwort stimmt nicht mit der Richtlinie überein.      |   |
| QAGateRegistrationTitle                               | Antworten registrieren         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | Sie müssen {0} der folgenden {1} Fragen beantworten.   |   |
| QAGateResetTitle                                      | Identität bestätigen: Antworten angeben                                  |   |



## <a name="customizing-the-logo-banner"></a>Anpassen des Logobanners

Das Standardbanner auf den Seiten des Portals kann für Ihre Organisation angepasst werden.
So passen Sie das Logobanner an:

1.  Erstellen Sie Ihre benutzerdefinierten Banner, und speichern Sie sie als PNG-Dateien. Die Dateien sollten den folgenden Empfehlungen entsprechen:

 - Größe: 490 x 50 Pixel.

 - Bittiefe: 32

2.  Kopieren Sie die Dateien in jedem Portal, das Sie anpassen möchten, in den Ordner \\Customizations.

3.  Erstellen Sie in jedem Ordner eine Datei „Style.css“. Sorgen Sie dafür, dass sie auf den Ordner „Customizations“ und das neue Logo weist. Ggf. können Sie den Namen des Logos (d.h. „/Customizations/contosologo.png“) ändern. Der Code sollte in etwa wie folgt aussehen:

   **Beispiel 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Wenn Sie Internet Explorer 6.0 verwenden, müssen Sie ein alternatives nicht transparentes Logo bereitstellen und „Style.css“ folgenden Code hinzufügen:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Beispiel 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Wenn Sie Internet Explorer 6.0 verwenden, müssen Sie ein alternatives nicht transparentes Logo bereitstellen und „Style.css“ folgenden Code hinzufügen:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Anpassen des Bildes für SmartPhones

Sie können das Bild mithilfe der folgenden Methode für SmartPhones anpassen. So passen Sie das Bild für SmartPhones an:

1.  Erstellen Sie Ihr Bild, und speichern Sie es als PNG-Dateien. Die Dateien sollten den folgenden Empfehlungen entsprechen:

    - Größe: 190 x 50 Pixel.
    - Bittiefe: 32

2.  Kopieren Sie die Dateien in jedem Portal, das Sie anpassen möchten, in den Ordner \\Customizations.

2.  Erstellen Sie in jedem Ordner eine Datei „Style.css“. Sorgen Sie dafür, dass sie auf den Ordner „Customizations“ und das neue Logo weist. Ggf. können Sie den Namen des Logos (d.h. „/Customizations/contosologo.png“) ändern. Der Code sollte in etwa wie folgt aussehen:

  **Beispiel 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Anpassen von Cascading Stylesheets

Sie können das Layout und den Stil der Kennwortportale mithilfe eines benutzerdefinierten Cascading Stylesheets (CSS) ändern. So verwenden Sie ein benutzerdefiniertes CSS:

1.  Erstellen Sie Ihre angepassten CSS-Dateien, und speichern Sie sie als „Style.css“.

2.  Kopieren Sie die Dateien in jedem Portal, das Sie anpassen möchten, in den Ordner \\Customizations.

Im Folgenden finden Sie ein einfaches Beispiel einer Datei „Style.css“:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Damit MIM Anpassungsänderungen erkennen kann, müssen Sie IIS durch Ausführung von „iisreset“ neu starten.                                                                                                                                                                                       

Im Folgenden finden Sie ein fortgeschrittenes Beispiel einer Datei „Style.css“. Diese Datei enthält Smartphone- und iPad-spezifische Informationen zur Anzeige der Portale auf diesen Geräten.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Allgemeine Probleme bei der Anpassung

Die folgende Tabelle ist eine Liste der bekannten Probleme, die beim Upgrade von FIM-Dienst und Portal auftreten können. Diese Tabelle enthält auch die Lösungen dieser Probleme.

|Problem |Lösung                                                                    |
|------|------------------------------|
| Ich habe eine Zeichenfolgenanpassung vorgenommen, aber sie wurde nicht in die Benutzeroberfläche übernommen.         | Zeichenfolgenanpassungen in „strings.resources“ erfordern immer „iisreset“.         |
| Nach einer Änderung in „strings.resources“ wird keine meiner Zeichenfolgen mehr angezeigt.     | Das Format von „strings.resources“ ist möglicherweise fehlerhaft und wird daher vom Portal ignoriert. Überprüfen Sie das Ereignisprotokoll unter „Windows-Protokolle – Anwendungs- und Dienstprotokolle – Forefront Identity Manager“.                        |
| Als ich zum ersten Mal „Style.css“ hinzufügte, wurden meine Änderungen an Formatvorlagen nicht im Portal angezeigt.            | Beim erstmaligen Einführen einer „Style.css“-Datei müssen Sie „iisreset“ ausführen.     |
| Neue Formatvorlagen werden in „Style.css“ hinzugefügt/geändert, aber Änderungen sind im Browser nicht sichtbar.      | Löschen Sie den Browsercache, und aktualisieren Sie die Seite. <br/> Überprüfen Sie die CSS-Syntax.    |
| Ich habe den Inhalt der CSS-Ordner in „path_to_sspr_portal\\css\\\*.css“ oder das Bannerlogo in „path_to_sspr_portal\\images\\fimlogo.png“ direkt geändert und diese Änderungen beim Upgrade verloren. | Prinzipiell sollten Sie diese Dateien nie ändern. Verwenden Sie den Ordner „Customizations“ nur als Mittel, um ein Bannerlogo und CSS-Formatvorlagenanpassungen in „Style.css“ bereitzustellen. Der Ordner „Customizations“ wird absichtlich nicht durch größere Upgrades überschrieben. <br/><br/>Versuchen Sie nicht, mit Tools wie ILSpy und Reflector Zeichenfolgen in den Portal-Assemblys zu ändern. Verwenden Sie „strings.resources“, um Standardzeichenfolgen zu überschreiben. Beim Upgrade werden die Assemblys ersetzt.  |
| Bannerlogo wird in den Portalen nicht angezeigt / ich sehe immer noch das FIM-Logo.     | Der Bildname/-pfad in „Style.css“ ist ungültig, oder der Browsercache wurde nicht gelöscht.          |
| Das Bannerlogo sieht in IE6 schlecht aus.       | Sie müssen ein nicht transparentes Bild für IE6 und eine spezielle begleitende Formatvorlage in „Style.css“ angeben.        |
