---
title: Verwenden eines alternativen Multi-Factor Authentication-Anbieters über eine API zur Aktivierung von PAM oder in SSPR-Szenarios | Microsoft-Dokumentation
description: Richten Sie eine benutzerdefinierte MFA-API als zweite Sicherheitsebene ein, wenn Benutzer Rollen in Privileged Access Management aktivieren und die Self-Service-Kennwortzurücksetzung verwenden.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 9ce531fb3f6f9c831ecdb716f006f947611871e6
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256596"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Verwenden eines benutzerdefinierten Multi-Factor Authentication-Anbieters über eine API während der Aktivierung von PAM-Rollen oder in SSPR

Kunden von Azure AD Premium oder Azure MFA können Azure MFA in den zwei MIM-Szenarios Privileged Access Management-Rollenaktivierung (PAM) oder Self-Service-Kennwortzurücksetzung (SSPR) integrieren.

MIM-Kunden haben zwei weitere Optionen:

 - Verwenden eines benutzerdefinierten Einmalkennwortanbieters, der nur im MIM SSPR-Szenario anwendbar ist und in der Anleitung [Configure Self-Service Password Reset with OTP SMS Gate (Konfigurieren der Self-Service-Kennwortzurücksetzung mit OTP SMS-Gate)](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10)) dokumentiert ist.
 - Verwenden Sie einen benutzerdefinierten Telefonanbieter mit mehrstufiger Authentifizierung. Dies gilt für die in diesem Artikel beschriebenen Szenarios für MIM SSPR und PAM.

In diesem Artikel wird beschrieben, wie Sie MIM mit einem benutzerdefinierten Multi-Factor Authentication-Anbieter über eine API und ein vom Kunden entwickeltes SDK für die Integration verwenden.  

## <a name="prerequisites"></a>Voraussetzungen

Um eine benutzerdefinierte Multi-Factor Authentication-Anbieter-API mit MIM verwenden zu können, benötigen Sie Folgendes:

- Telefonnummern für alle Kandidatenbenutzer
- MIM-Hotfix 4.5.202.0 oder höher: siehe Ankündigungen im [Versionsverlauf](reference/version-history.md)
- MIM-Dienst für SSPR oder PAM konfiguriert

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Vorgehensweise mithilfe von Code für benutzerdefinierte mehrstufige Authentifizierung

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Schritt 1: Stellen Sie sicher, dass der MIM-Dienst die Version 4.5.202.0 oder höher aufweist.

Laden Sie den MIM-Hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) oder höher herunter, und installieren Sie diesen.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Schritt 2: Erstellen Sie eine DLL, die die IPhoneServiceProvider-Schnittstelle implementiert.

Die DLL muss eine Klasse enthalten, die drei Methoden implementiert:

- `InitiateCall`: Der MIM-Dienst ruft diese Methode auf. Der Dienst übergibt die Telefonnummer und Anforderung-ID als Parameter.  Die Methode muss für den `PhoneCallStatus`-Wert `Pending`, `Success` oder `Failed` zurückgeben.
- `GetCallStatus`: Wenn für einen früheren Aufruf von `initiateCall``Pending` zurückgegeben wurde, ruft der MIM-Dienst diese Methode auf. Diese Methode gibt für den `PhoneCallStatus`-Wert `Pending`, `Success` oder `Failed` zurück.
- `GetFailureMessage`: Wenn für einen früheren Aufruf von `InitiateCall` oder `GetCallStatus``Failed` zurückgegeben wurde, ruft der MIM-Dienst diese Methode auf. Diese Methode gibt eine diagnostische Meldung zurück.

Die Implementierungen dieser Methoden müssen threadsicher sein, und außerdem dürfen die Implementierungen von `GetCallStatus` und `GetFailureMessage` nicht davon ausgehen, dass diese vom selben Thread aufgerufen werden wie bei einem vorherigen Aufruf von `InitiateCall`.

Speichern Sie die DLL im `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`-Verzeichnis.

Beispielcode, der mithilfe von Visual Studio 2010 oder höher erstellt werden kann.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Schritt 3: Sichern Sie die unter „C:\Programme\Microsoft Forefront Identity Manager\2010\Service“ gespeicherte MfaSettings.xml-Datei.

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Schritt 4: Bearbeiten Sie die MfaSettings.xml-Datei.

Aktualisieren oder löschen Sie die folgenden Zeilen:

- Alle Konfigurationeintragszeilen entfernen/löschen 

- Aktualisieren Sie die folgenden Zeilen, oder fügen Sie diese mit Ihrem benutzerdefinierten Telefonanbieter der MfaSettings.xml-Datei hinzu. <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Schritt 5: Starten Sie den MIM-Dienst neu.

Verwenden Sie nach dem Neustart des Diensts SSPR und/oder PAM, um die Funktion mit dem benutzerdefinierten Identitätsanbieter zu überprüfen.

> [!NOTE] 
> Ersetzen Sie zum Wiederherstellen Ihrer Einstellung MfaSettings.xml durch die Sicherungsdatei aus Schritt 3.


## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Azure Multi-Factor Authentication-Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Was ist Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [MIM version release history (Versionsverlauf der MIM-Version)](./reference/version-history.md)
