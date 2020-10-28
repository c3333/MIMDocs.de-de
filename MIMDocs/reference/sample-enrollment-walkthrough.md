---
title: Exemplarische Vorgehensweise bei der Beispiel Registrierung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a560e68ea820211caa1dd0a40a0143f44d5f7fb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760377"
---
# <a name="sample-enrollment-walkthrough"></a>Exemplarische Vorgehensweise zur Beispiel Registrierung
In diesem Artikel werden die erforderlichen Schritte zum Durchführen einer Self-Service-Registrierung für eine virtuelle Smartcard beschrieben. Es zeigt eine automatisch genehmigte Anforderung mit einer vom Benutzer festgelegten PIN.

1. Der Client authentifiziert den Benutzer und fordert dann eine Liste der Profilvorlagen an, für die sich der authentifizierte Benutzer registrieren kann:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. Der Client zeigt dem Benutzer die Ergebnisliste an. Der Benutzer wählt eine VSC-Profil Vorlage (virtuelle Smartcard) mit dem Namen "virtuelles Smartcard-VPN" und "uuid" aus `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1` .

3. Der Client ruft die Registrierungsrichtlinie der ausgewählten Profilvorlage ab, indem er die UUID verwendet, die im vorherigen Schritt zurückgegeben wurde:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. Der Client analysiert das Feld " **DataCollection** " in der zurückgegebenen Richtlinie und weist darauf hin, dass ein einzelnes Daten Sammlungs Element mit der Bezeichnung "Anforderungs Grund" angezeigt wird. Der Client stellt außerdem fest, dass das " **collectcomments** "-Flag auf "false" festgelegt ist, sodass der Benutzer nicht zur Eingabe einer beliebigen aufgefordert wird.

5. Nachdem der Benutzer den Grund für das Anfordern der Zertifikate eingegeben hat, erstellt der Client eine Anforderung:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. Der Server erstellt die Anforderung und gibt den URI der Anforderung an den Client zurück:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5` .

7. Der Client ruft das Anforderungsobjekt ab, indem er den zurückgegebenen URI aufruft:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. Der Client überprüft, ob die **State** -Eigenschaft in der Anforderung auf "Approved" festgelegt ist. Die Anforderungsausführung kann beginnen.

9. Der Client prüft die Anforderung, um zu ermitteln, ob bereits eine Smartcard mit der Anforderung verknüpft ist, indem der Inhalt des Parameters " **newsmartcarduuid** " analysiert wird.

10. Da Sie nur eine leere GUID enthält, muss der Client entweder eine vorhandene Karte verwenden, die von MIM cm nicht bereits verwendet wird, oder eine vorhandene Karte erstellen, wenn die Profil Vorlage für virtuelle Smartcards konfiguriert wird.

11. Da dem Client über die ursprüngliche Abfrage für registrierbare Profilvorlagen (Schritt 1) Letzteres angegeben wurde, muss der Client nun ein virtuelles Smartcard-Gerät erstellen.

12. Der Client ruft die Smartcard-Richtlinie aus der Profil Vorlage ab. Es verwendet die UUID der Vorlage, die in Schritt 3 ausgewählt wurde:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13. Die Smartcard-Richtlinie enthält den standardmäßigen Administrator Schlüssel für die Karte in der **defaultadminkeyhex** -Eigenschaft. Wenn der Client die Smartcard erstellt, muss er den anfänglichen Administratorschlüssel der Smartcard auf diesen Schlüssel festlegen.  
14. Beim Erstellen des Smartcard-Geräts muss der Client den Schlüssel der Anforderung zuweisen:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. Der Server antwortet dem Client mit einem URI für das neu erstellte Smartcard-Objekt: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644` .

16. Der Client ruft das Smartcard-Objekt ab:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17. Durch Überprüfen des Werts des Flags " **diversifyadminkey** " in der Smartcard-Richtlinie, die in Schritt 12 abgerufen wurde, weiß der Client, dass er den Administrator Schlüssel diversifizieren muss.

18. Der Client Ruft den vorgeschlagene Administratorschlüssel ab:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19. Der Client muss sich bei der Karte als Administrator authentifizieren, um den Administratorschlüssel festzulegen. Zu diesem Zweck fordert der Client eine Authentifizierungsaufforderung von der Smartcard an und sendet diese Aufforderung an den Server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    Der Server sendet die Aufforderungsantwort im HTTP-Antworttext. Konvertieren Sie die hexadezimale Aufforderungszeichenfolge in base64, und übergeben Sie sie in der URL als Parameter. Die URL gibt eine andere Antwort zurück, die in das hexadezimale Format konvertiert werden muss.

20. Der Server sendet die Aufforderungsantwort im HTTP-Antworttext, und der Client verwendet die Antwort, um den Administratorschlüssel zu diversifizieren.

21. Der Client stellt fest, dass der Benutzer die gewünschte Pin bereitstellen muss, indem er das Feld **userpinoption** in der Smartcard-Richtlinie prüft.

22. Nachdem der Benutzer die gewünschte PIN in einem Dialogfeld eingegeben hat, führt der Client eine Abfrage Antwort-Authentifizierung aus, wie in Schritt 19 beschrieben. der einzige Unterschied besteht darin, dass das diversifizierte Flag jetzt auf "true" festgelegt werden muss:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23. Der Client verwendet die Antwort, die er vom Server empfangen hat, um die gewünschte Benutzer-PIN festzulegen.

24. Der Client kann nun Zertifikatanforderungen generieren. Er fragt die Parameter für die Zertifikatgenerierung aus der Profilvorlage ab, um zu bestimmen, wie die Schlüssel/Anforderungen generiert werden sollen.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25. Der Server antwortet mit einem einzelnen JSON-serialisierten **certificaterequestgenerationoptions** -Objekt. Das-Objekt enthält Parameter für die Exportierbarkeit des Schlüssels, den anzeigen amen des Zertifikats, den Hash Algorithmus, den Schlüssel Algorithmus, die Schlüsselgröße usw. Der Client verwendet diese Parameter, um eine Zertifikatanforderung zu generieren.

26. Die einzelne Zertifikatanforderung wird an den Server gesendet. Der Client könnte zusätzlich ein PFX-Kennwort angeben, das verwendet werden soll, um alle PFX-blobvorgänge zu entschlüsseln, wenn die Zertifikat Vorlage das Archivieren der Zertifikate auf der Zertifizierungsstelle angibt, d. h., die Zertifizierungsstelle generiert das Schlüsselpaar und sendet es an den Client. Der Client könnte auch einige Kommentare hinzufügen.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27. Nach wenigen Sekunden antwortet der Server mit einem einzelnen JSON-serialisierten **Microsoft. CLM. shared. Certificate** -Objekt. Durch das Überprüfen des **isPkcs7** -Flags erfährt der Client, dass diese Antwort kein PFX-BLOB ist. Der Client extrahiert das BLOB durch eine Base64-Decodierung der PKCS7-Zeichenfolge und installiert sie.

28. Der Client markiert die Anforderung als abgeschlossen.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
