---
title: Anpassungen des Microsoft Identity Manager 2016-Portals | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 144ce2344327f16f60269b4f505c30fd470e46da
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760812"
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016-Portal Anpassung

In Microsoft Identity Manager 2016 (MIM) können Sie ausgewählte Elemente der Kennwortportale einschließlich Bannerlogo, Zeichenfolgenressourcen und Cascading Stylesheets anpassen.

>[!WARNING]
>Löschen Sie den Browser Cache immer, wenn CSS-Anpassungen vorgenommen werden.

Die folgenden Elemente sind für die Anpassung der MIM 2016-Kenn Wort Registrierungs-und-Zurücksetzungs Portale verwendet:

- Ordner "Anpassungen": Dies ist der Ordner, der von MIM 2016 vor der Verwendung der Standardwerte überprüft wird. Jedes Portal, das angepasst werden soll, erfordert einen Ordner "Anpassungen". Anpassungen sollten nur in diesem Ordner durchgeführt werden, da der Setup Vorgang diesen Ordner nicht bei Upgrades, im Modus zum Ändern des Modus oder bei Installationen des Reparatur Modus überschreibt.

- Strings. Resources: Dies ist eine XML-basierte Datei, die es Ihnen ermöglicht, die Zeichen folgen zu ändern, die im Portal angezeigt werden. Diese Datei muss sich im Ordner „Customizations“ befinden.

- Style. CSS: Dies ist das Cascading Stylesheet, das von den Portalen zur Anpassung verwendet wird. Erstellen und ändern Sie dieses Stylesheet, um das Logo zu ändern. Sie können auch den gesamten Inhalt des Stylesheets durch ihre eigenen Anpassungen ersetzen.

Ausführliche Schritt-für-Schritt-Anleitungen zum Anpassen der Portale für die Kenn Wort Registrierung und zur Kenn Wort Zurücksetzung finden Sie unter Test Umgebungs Anleitung: demonstrieren der MIM 2016-Kenn Wort Registrierung und Zurücksetzen der Portal Anpassung.

>[!WARNING]
>Damit MIM angepasste Änderungen erkennt, müssen Sie IIS neu starten, indem Sie Ausführen `iisreset` .


## <a name="customizations-folder"></a>Ordner "Anpassungen"

Beim Start sucht MIM im Ordner "Anpassungen" nach der Datei "Strings. Resources", bevor die Standardwerte verwendet werden. Erstellen Sie einen Ordner "Anpassungen" unter dem Verzeichnis für das Portal, das Sie anpassen möchten (d. h. das Kenn Wort Registrierungs Portal oder das Portal für die Kenn Wort Zurücksetzung). Wenn Sie beide Portale anpassen möchten, erstellen Sie einen Ordner "Anpassungen" unter jedem der folgenden Speicherorte:

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`

So erstellen Sie einen Ordner "Anpassungen" für das Kenn Wort Registrierungs Portal:

1. Navigieren Sie zum Ordner `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`.
   
2. Erstellen Sie einen Ordner mit dem Namen **Anpassungen** .

So erstellen Sie einen Ordner "Anpassungen" für das Portal für die Kenn Wort Zurücksetzung:

1. Navigieren Sie zum Ordner `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`.
   
2. Erstellen Sie einen Ordner mit dem Namen **Anpassungen** .


## <a name="custom-strings-in-the-stringsresources-file"></a>Benutzerdefinierte Zeichen folgen in der Datei "Strings. Resources"

Viele der Zeichenfolgen in der Benutzeroberfläche des Portals können Sie anpassen, indem Sie eine Strings.resources-Datei erstellen und diese Datei dem Ordner „Customizations“ hinzufügen. Erstellen Sie für jedes Portal, das Sie anpassen möchten, eine Strings. resources-Datei.

So erstellen Sie angepasste Zeichen folgen in der Datei "Strings. Resources":

1. Kopieren Sie den folgenden Code in Editor, und fügen Sie ihn in die Datei "Strings. Resources" ein. Speichern Sie die Datei im Ordner "Anpassungen" für das Portal.

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

2.  `<!-- Customizations begin here -->`Ändern Sie den Daten Namen im Abschnitt des Codes entsprechend den Zeichen folgen, die Sie anpassen möchten. Geben Sie den Wert für die Zeichenfolge zwischen den `<value></value>` Tags ein. Weitere Informationen zu den Zeichen folgen, die angepasst werden können, und ihren Standardwerten finden Sie in den nächsten Abschnitten.

>[!NOTE]
>Die Datei "Strings. Resources" ist sprachneutral. Um sprachspezifische, angepasste Zeichen folgen zu erstellen, müssen Sie das Sprachpaket installiert haben und die Datei im Zeichen folgen Format speichern `<language>-<culture>.resources` . Der Dateiname für die Kultur der englischen Sprache lautet beispielsweise **Strings. en-US. Resources** .


## <a name="portal-strings"></a>Portal Zeichenfolgen
In der folgenden Tabelle werden die Portal Zeichenfolgen angezeigt, die angepasst werden können:

<!-- The default values are actual UI strings and should not copy edited. -->

| Portal Zeichen folgen Name | Standardwert |
|---|---|
| AboutLinkText                                            | Info  |
| ButtonCancel                                             | Abbrechen |
| ButtonFinish                                             | Finish |
| ButtonNext                                               | Nächste   |
| ButtonOk                                                 | OK     |
| CancelFinishedMessage                                    | Ihre Sitzung ist nicht mehr aktiv. Sie können das Fenster schließen oder die Sitzung neu starten, indem Sie auf den folgenden Link klicken. |
| CancelFinishedTitle                                      | Die Sitzung ist beendet |
| ErrorDescription_3000                                    | Es ist ein Fehler aufgetreten. Versuchen Sie es erneut. Wenn das Problem weiterhin besteht, wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3000) |
| ErrorDescription_3001                                    | Stellen Sie sicher, dass Sie Ihren Benutzernamen korrekt eingeben. Wenn Sie Ihr Kennwort immer noch nicht zurücksetzen können, wenden Sie sich an Ihren Helpdesk. (Fehler 3001) |
| ErrorDescription_3002                                    | Die Sitzung ist beendet. Kehren Sie zur Homepage zurück, um den Vorgang zu wiederholen. (Fehler 3002) |
| ErrorDescription_3003                                    | Das aktuelle Benutzerkonto wird von Forefront Identity Manager nicht erkannt. Wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3003) |
| ErrorDescription_3004                                    | Sie sind nicht autorisiert, sich für die Kenn Wort Zurücksetzung zu registrieren. Wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3004) |
| ErrorDescription_3005                                    | Mindestens eine von Ihnen angegebene Antwort entspricht nicht den Antworten, die Sie bei der Kenn Wort Registrierung angegeben haben. Um Ihr Kennwort zurückzusetzen, müssen die von Ihnen bereitgestellten Antworten nun mit den Antworten identisch sein, die Sie bei der Registrierung angegeben haben. Sie können erneut von der Startseite aus starten, oder wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3005) |
| ErrorDescription_3006                                    | Das eingegebene Kennwort ist falsch. Sie müssen das richtige Kennwort eingeben, um sich für die Kenn Wort Zurücksetzung zu registrieren. (Fehler 3006) |
| ErrorDescription_3007                                    | Das Zurücksetzen Ihres Kennworts ist vorübergehend nicht zulässig. Versuchen Sie es später erneut, oder wenden Sie sich an den Helpdesk oder den Systemadministrator, um Hilfe zu erhalten. (Fehler 3007) |
| ErrorDescription_3008                                    | Es ist ein Fehler aufgetreten. Versuchen Sie es erneut. Wenn das Problem weiterhin besteht, wenden Sie sich an den Helpdesk oder den Systemadministrator. (Fehler 3008) |
| ErrorDescription_3009                                    | Die von Ihnen gemachte Eingabe enthält Text in einem Format, das nicht erlaubt ist. Versuchen Sie es erneut mit einer anderen Eingabe, oder wenden Sie sich an den Helpdesk bzw. den Systemadministrator. (Fehler 3009) |
| ErrorDescription_3010_Registration                       | Skripting ist im Browser nicht aktiviert. Aktivieren Sie Skripting, und kehren Sie zur Startseite der Kennwortregistrierung zurück, oder wenden Sie sich an den Helpdesk. |
| ErrorDescription_3010_Reset                              | Skripting ist im Browser nicht aktiviert. Aktivieren Sie Skripting, und kehren Sie zur Startseite der Kennwortzurücksetzung zurück, oder wenden Sie sich an den Helpdesk. |
| ErrorDescription_3011                                    | Von dieser Seite werden Cookies verwendet. Konfigurieren Sie den Browser für die Annahme von Cookies, und versuchen Sie es erneut, oder wenden Sie sich an den Helpdesk. |
| ErrorDescription_3012                                    | Die von Ihnen eingegebenen Daten entsprechen nicht dem Ihnen zugesendeten Sicherheitscode. Sie können versuchen, das Kennwort erneut zurückzusetzen, oder den Helpdesk um Unterstützung bitten.  |
| ErrorDescription_3013                                    | Der Sicherheitscode kann nicht gesendet werden. Bitten Sie den Helpdesk um Unterstützung. |
| ErrorMessageDomainUsernameFormat                         | Geben Sie den Benutzernamen im richtigen Format ein. |
| ErrorMessageDomainUsernameRequired                       | Geben Sie zum Fortfahren einen Benutzernamen ein. |
| ErrorMessagePasswordRequired                             | Geben Sie ein Kennwort ein. |
| ErrorMessagePasswordsDoNotMatch                          | Stellen Sie sicher, dass beide Kennwörter übereinstimmen. |
| ErrorPageDefaultHeading                                  | Anwendungsfehler <br/>**Hinweis** : auf die Überschrift folgt "=" und die Fehlermeldung.
| ErrorPageServerTime                                      | Server Zeit: {0:T} <br/>**Hinweis** : {0} ist die Zeit, zu der die Ausnahme abgefangen wurde. "'T" bewirkt, dass die eingeführte Zeit als "Long Time" (Stunde, Minute und Sekunde) formatiert wird. Die am/pm-Bezeichnung wird auch abhängig von der aktuellen Kultur angezeigt. |
| ErrorPageTitle                                           | Forefront Identity Management – Kennwortfehler | 
| ErrorTitle_3000                                          | Fehler |
| ErrorTitle_3001                                          | Zugriff verweigert |
| ErrorTitle_3002                                          | Die Sitzung ist beendet |
| ErrorTitle_3003                                          | Unbekannter Benutzer |
| ErrorTitle_3004                                          | Nicht autorisierter Benutzer |
| ErrorTitle_3005                                          | Antworten stimmen nicht überein |
| ErrorTitle_3006                                          | Falsches Kennwort |
| ErrorTitle_3007                                          | Zugriff vorübergehend verweigert |
| ErrorTitle_3008                                          | Kommunikationsfehler |
| ErrorTitle_3009                                          | Eingabe ist nicht zulässig |
| ErrorTitle_3010                                          | Fehler in der Browserkonfiguration |
| ErrorTitle_3011                                          | Fehler in der Browserkonfiguration |
| ErrorTitle_3012                                          | Fehler bei der Überprüfung |
| ErrorTitle_3013                                          | Der Sicherheitscode kann nicht gesendet werden |
| FinalizeRegistrationHeading1                             | Wenn Sie später einmal Ihr Kennwort zurücksetzen müssen: |
| FinalizeRegistrationSubHeading1                          | Besuchen Sie das Kennwortzurücksetzungsportal |
| FinalizeRegistrationSubHeading2                          | Bestätigen Sie Ihre Identität. |
| FinalizeRegistrationSubHeading3                          | Neues Kennwort auswählen |
| FinishingDescription                                     | Neues Kennwort auswählen |
| FinishingTitle                                           | Kennwort zurücksetzen: |
| GotoPortalPrefix                                         | Gehe zu |
| GotoPortalSuffix                                         | Startseite |
| LabelTroubleshootingLinkText                             | Details anzeigen |
| LoadingText                                              | Wird geladen... |
| NoScriptTagErrorMessage                                  | Skripting ist im Browser nicht aktiviert. Aktivieren Sie Skripting, und kehren Sie zur Startseite zurück, oder wenden Sie sich an den Helpdesk.  |
| PasswordResetOperationGeneralErrorMessage                | Fehler beim Zurücksetzen des Kennworts. |
| PasswordResetOperationPolicyViolationErrorMessage        | Das Kennwort entspricht nicht den Kennwortrichtlinien Ihrer Organisation. |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Fehler beim Zurücksetzen des Kennworts. Der Benutzer kann das Kennwort nicht ändern. |
| PrivacyStatement                                         | Datenschutzerklärung |
| RegistrationDescription                                  | Self-Service-Kennwortregistrierung |
| RegistrationMission                                      | Wenn Sie einmal Ihr Kennwort vergessen haben sollten, können Sie es selbst zurücksetzen, ohne sich an den Helpdesk wenden zu müssen. |
| RegistrationPageTitle                                    | Forefront Identity Management – Kennwortregistrierung |
| RegistrationSteps                                        | Klicken Sie auf „Weiter“, um den Registrierungsvorgang zu starten. |
| RegistrationSuccessDescription                           | Sie sind jetzt registriert |
| RegistrationSuccessTitle                                 | Abgeschlossen: |
| RegistrationWelcomeTitle                                 | Kennwortregistrierung: |
| ResetDescription                                         | Self-Service-Kennwortzurücksetzung |
| ResetEnterNamePrompt                                     | Geben Sie unten Ihren Benutzernamen ein |
| ResetEnterPassword                                       | Geben Sie ein neues Kennwort ein: |
| ResetExample1                                            | `contoso\mmeyers` |
| ResetExample2                                            | `mmeyers\@contoso.com` |
| ResetExamples                                            | Beispiele: |
| ResetPageTitle                                           | Forefront Identity Management – Kennwortzurücksetzung |
| ResetReenterPassword                                     | Geben Sie das Kennwort erneut ein: |
| ResetSuccessDescription                                  | Ihr Kennwort wurde zurückgesetzt. |
| ResetSuccessTitle                                        | Erfolg: |
| ResetUseNewPassword                                      | Sie können sich jetzt mit dem neuen Kennwort anmelden. |
| ResetUsernameTextFormat                                  | (Zurücksetzen des Kennworts für {0} ) <br/>**Hinweis** : {0} ist die Anmeldung des Benutzers. |
| ResetWelcomeTitle                                        | Kennwort zurücksetzen: |
| TroubleshootingEmailSubject                              | Details zum FIM-Anforderungsverarbeitungsfehler |
| TroubleshootingLabelAttributes                           | Attribute: |
| TroubleshootingLabelCloseButton                          | Schließen |
| TroubleshootingLabelCopyToClipboard                      | In Zwischenablage kopieren |
| TroubleshootingLabelCorrelationId                        | Korrelations-ID: |
| TroubleshootingLabelDetails                              | Details: |
| TroubleshootingLabelPostCopyClipboardMessage             | Die Informationen wurden in die Zwischenablage kopiert. |
| TroubleshootingLabelRequestId                            | Anforderungs-ID: |
| TroubleshootingLabelSendEmail                            | Informationen per E-Mail senden |
| TroubleshootingLabelSource                               | Ursache: |
| TroubleshootingLabelViewRequestDetails                   | Anforderungsdetails anzeigen |
| TroubleshootingLinkText                                  | Problembehandlungsinformationen |


## <a name="authentication-gate-strings"></a>Authentifizierungs Gate-Zeichen folgen
In der folgenden Tabelle sind die Authentifizierungs Gate-Zeichen folgen aufgeführt, die angepasst werden können:

<!-- The default values are actual UI strings and should not copy edited. -->

| Name der Authentifizierungs Gate-Zeichenfolge | Standardwert |
|---|---|
| OTPEmailRegistraionEmailTextboxLabel                  | E-Mail-Adresse: |
| OTPEmailRegistrationEmailRequiredErrorMessage         | Das Feld für die E-Mail-Adresse darf nicht frei gelassen werden. |
| OTPEmailRegistrationFooterReadOnly                    | Folgen Sie zum Aktualisieren Ihrer E-Mail-Adresse den Prozessen, die von Ihrer Organisation definiert sind, oder rufen Sie beim Helpdesk an. |
| OTPEmailRegistrationFooterReadWrite                   | Die E-Mail-Adresse wird von Ihrer Organisation in Forefront Identity Manager gespeichert. |
| OTPEmailRegistrationGateTitle                         | Bestätigung mit E-Mail-Adresse |
| OTPEmailRegistrationHeaderReadOnly                    | Wenn Sie das Kennwort zurücksetzen müssen, wird ein Prüfsicherheitscode an Ihre E-Mail-Adresse gesendet. Wenn die unten angegebene E-Mail-Adresse nicht korrekt ist, müssen Sie diese aktualisieren, damit Sie die Self-Service-Kennwortzurücksetzung verwenden können. |
| OTPEmailRegistrationHeaderReadWrite                   | Geben Sie unten Ihre E-Mail-Adresse ein. Wenn Sie das Kennwort zurücksetzen müssen, wird Ihnen eine E-Mail mit einem Prüfcode gesendet. |
| OTPEmailResetGateTitle                                | Überprüfen Ihrer Identität: E-Mail-Bestätigung |
| OTPEmailResetHeader                                   | Geben Sie unten Ihren Sicherheitscode ein. Ein Sicherheitscode wurde an die E-Mail-Adresse gesendet, die für diese Organisation registriert ist. |
| OTPRegularExpressionErrorMessage                      | Der angegebene Wert entspricht nicht dem erwarteten Format. |
| OTPResetOneTimePasswordRequiredErrorMessage           | Das Feld für den Sicherheitscode darf nicht frei gelassen werden. |
| OTPResetVerificationLabel                             | Sicherheitscode: |
| OTPSmsRegistrationFooterReadOnly                      | Folgen Sie zum Aktualisieren Ihrer Mobiltelefonnummer den Prozessen, die von Ihrer Organisation definiert sind, oder rufen Sie beim Helpdesk an. |
| OTPSmsRegistrationFooterReadWrite                     | Die Mobiltelefonnummer wurde von Ihrer Organisation in Forefront Identity Manager gespeichert. |
| OTPSmsRegistrationGateTitle                           | Mobiltelefonbestätigung |
| OTPSmsRegistrationHeaderReadOnly                      | Wenn Sie das Kennwort zurücksetzen müssen, wird ein Prüfsicherheitscode an Ihr Mobiltelefon gesendet. Wenn die unten angegebene Mobiltelefonnummer nicht korrekt ist, müssen Sie diese aktualisieren, damit Sie die Self-Service-Kennwortzurücksetzung verwenden können. |
| OTPSmsRegistrationHeaderReadWrite                     | Geben Sie unten Ihre Mobiltelefonnummer ein. Wenn Sie das Kennwort zurücksetzen müssen, wird ein Prüfcode an Ihr Mobiltelefon gesendet. |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Das Feld für die Mobiltelefonnummer darf nicht frei gelassen werden. |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobiltelefon: |
| OTPSmsResetGateTitle                                  | Überprüfen Ihrer Identität: Mobiltelefonbestätigung |
| OTPSmsResetHeader                                     | Geben Sie unten Ihren Sicherheitscode ein. Ein Sicherheitscode wurde an das Mobiltelefon gesendet, das für diese Organisation registriert ist. |
| PasswordGateDescriptionText                           | Geben Sie unten Ihr aktuelles Kennwort ein, und klicken Sie dann auf „Weiter“. |
| PasswordGateErrorMessagePasswordRequired              | Geben Sie Ihr aktuelles Kennwort ein. |
| PasswordGateGateTitle                                 | Ihr aktuelles Kennwort |
| PasswordGatePasswordLabelText                         | Password (Kennwort): |
| PasswordGateUsernameTextFormat                        | `<i>` (angemeldet als: `<b>{0}</b>` ) `</i>` |
| QAGateErrorNotEnoughQuestionsAnswered                 | Sie müssen mindestens Fragen beantworten {0} . |
| QAGateIncorrectAnswer                                 | Ihre Antworten sind nicht richtig. |
| QAGatePrivacyNotice                                   | Ihre Antworten werden von Ihrer Organisation in Forefront Identity Manager gespeichert. |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Sie müssen mindestens Fragen beantworten {0} , um sich zu registrieren. |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Eine oder mehrere Antworten stimmen nicht mit der Richtlinie überein. |
| QAGateRegistrationThisAnswerValidationFailed          | Diese Antwort stimmt nicht mit der Richtlinie überein. |
| QAGateRegistrationTitle                               | Antworten registrieren |
| QAGateResetNumberOfQuestionsExplanation_Format        | Sie müssen {0} die folgenden Fragen beantworten {1} . |
| QAGateResetTitle                                      | Identität bestätigen: Antworten angeben  |


## <a name="custom-logo-banners"></a>Banner für benutzerdefinierte Logos

Das Standardbanner auf den Seiten des Portals kann für Ihre Organisation angepasst werden.

So passen Sie das Logobanner an:

1. Erstellen Sie Ihre benutzerdefinierten Banner, und speichern Sie sie als PNG-Dateien. Die Dateien sollten den folgenden Empfehlungen entsprechen:

   - Größe: 490 x 50 Pixel.
   - Bittiefe: 32 Pixel.

2. Kopieren Sie die Dateien in jedem Portal, das Sie anpassen möchten, in den Ordner Customizations.

3. Erstellen Sie in jedem Ordner eine Datei „Style.css“. Zeigen Sie die Datei auf den Ordner Anpassungen für das Portal und das neue Logo. Sie können den Namen des Logos nach Bedarf ändern, z `/Customizations/contosologo.png` . b.. Das CSS sollte wie der folgende Code aussehen:

   `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4. Wenn Sie Internet Explorer 6,0 verwenden, müssen Sie ein alternatives nicht transparentes Logo angeben und "Style. CSS" den folgenden Code hinzufügen:

   `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

   Das CSS sollte wie der folgende Code aussehen:
   
   `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`


## <a name="custom-images-for-smartphones"></a>Benutzerdefinierte Images für Smartphones

Sie können das Logo Bild für Smartphones anpassen. 

So passen Sie ein Bild für ein Smartphone an:

1. Erstellen Sie Ihre Images, und speichern Sie Sie als PNG-Dateien. Die Dateien sollten den folgenden Empfehlungen entsprechen:

   - Größe: 190 x 50 Pixel.
   - Bittiefe: 32 Pixel.

2. Kopieren Sie die Dateien in jedem Portal, das Sie anpassen möchten, in den Ordner Customizations.

3. Erstellen Sie in jedem Ordner eine Datei „Style.css“. Zeigen Sie die Datei auf den Ordner Anpassungen für das Portal und das neue Logo. Sie können den Namen des Logos nach Bedarf ändern, z `/Customizations/contosologo.png` . b.. Das CSS sollte wie der folgende Code aussehen:

   ```css
   @media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="custom-style-sheets"></a>Benutzerdefinierte Stylesheets

Sie können das Layout und den Stil der Kennwortportale mithilfe eines benutzerdefinierten Cascading Stylesheets (CSS) ändern.

So verwenden Sie ein benutzerdefiniertes CSS:

1. Erstellen Sie Ihre angepassten CSS-Dateien, und speichern Sie sie als „Style.css“.

2. Kopieren Sie die Dateien in jedem Portal, das Sie anpassen möchten, in den Ordner Customizations.

Der folgende Code ist ein einfaches Beispiel für eine Style. CSS-Datei:

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
>Damit MIM angepasste Änderungen erkennt, müssen Sie IIS neu starten, indem Sie Ausführen `iisreset` .

Der folgende Code ist ein ausführlichere Beispiel einer Datei "Style. CSS". Diese Datei enthält Informationen, die für ein Smartphone oder ein Apple iPad zum Anzeigen der Portale auf diesen Geräten spezifisch sind.

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


## <a name="common-customization-issues"></a>Häufige Anpassungsprobleme

In der folgenden Tabelle werden häufige Probleme aufgelistet, die beim Aktualisieren des FIM-Dienstanbieter-und MIM-Portals auftreten können.

| Problem | Lösung |
|---|---|
| Ich habe eine Zeichen folgen Anpassung vorgenommen, aber Sie wurde nicht in der Benutzeroberfläche widergespiegelt.                       | Zeichen folgen Anpassungen in Zeichen folgen. Ressourcen erfordern einen IIS-Neustart, indem Sie Ausführen `iisreset` . |
| Nachdem Sie eine Zeichenfolge vorgenommen haben, wird die Änderung meiner Zeichenfolge nicht angezeigt.          | Das Format "Strings. Resources" ist wahrscheinlich falsch formatiert und kann vom Portal ignoriert werden. Überprüfen Sie das Ereignisprotokoll unter **Windows**  >  **-Protokolle Anwendungs-und Dienst Protokolle**  >  **Forefront Identity Manager** . |
| Beim ersten Hinzufügen von "Style. CSS" habe ich meine Stiländerungen im Portal nicht angezeigt.     | Wenn Sie die Datei Style. CSS zum ersten Mal einführen, müssen Sie Ausführen `iisreset` . |
| Neue Stile werden in "Style. CSS" hinzugefügt oder geändert, Änderungen im Browser werden jedoch nicht angezeigt. | Löschen Sie den Browser Cache, und aktualisieren Sie die Seite. Überprüfen Sie die CSS-Syntax. |
| Ich habe den Inhalt des CSS-Ordners `<path_to_sspr_portal>\css\*.css` oder des Banner Logos direkt geändert `<path_to_sspr_portal>\images\fimlogo.png` . Ich habe diese Änderungen beim Upgrade verloren. | Es wird empfohlen, diese Dateien nicht direkt zu ändern. Verwenden Sie nur den Ordner "Anpassungen", um ein Banner Logo anzugeben, und machen Sie CSS-Stil Anpassungen nur in "Style. CSS". Der Ordner "Anpassungen" wird von größeren Upgrades absichtlich nicht überschrieben. Verwenden Sie Tools wie ilspy und Reflektor nicht zum Ändern von Zeichen folgen in den portassemblys. Verwenden Sie „strings.resources“, um Standardzeichenfolgen zu überschreiben. Die Assemblys werden beim Upgrade ersetzt.  |
| Das Banner Logo wird in den Portalen nicht angezeigt. Ich sehe weiterhin das FIM-Logo.                  | Der Bildname/-Pfad in "Style. CSS" ist ungültig, oder der Browser Cache wurde nicht gelöscht.  |
| Banner Logo sieht in Internet Explorer 6 hässlich aus.                                          | Geben Sie ein nicht transparentes Bild für Internet Explorer 6 mit einem entsprechenden Stil für das Bild in "Style. CSS" an. |
