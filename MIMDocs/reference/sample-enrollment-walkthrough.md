---
Titel: Beispielregistrierung
MS.Custom: 
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology: 
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 92b97803-9475-4b90-9a6c-430f107a167d
---
# Exemplarische Vorgehensweise bei der Beispielregistrierung
Dieses Thema beschreibt die Schritte, die erforderlich sind, um eine Self-Service-Registrierung einer virtuellen Smartcard auszuführen. Es zeigt eine automatisch genehmigte Anforderung mit einer vom Benutzer festgelegten PIN.
1.  Der Client authentifiziert den Benutzer und fordert dann eine Liste der Profilvorlagen an, für die sich der authentifizierte Benutzer registrieren kann:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates. 
    ```
    <br/>
2.  Der Client zeigt dem Benutzer die Ergebnisliste an. Der Benutzer wählt eine Profilvorlage für virtuelle Smartcards aus, die den Namen „Virtual Smartcard VPN“ und die UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*hat.

3.  Der Client ruft die Registrierungsrichtlinie der ausgewählten Profilvorlage ab, indem er die UUID verwendet, die im vorherigen Schritt zurückgegeben wurde:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policy/workflow/enroll
    ```
 <br/>   
4.  Der Client analysiert das Feld „DataCollection“ (Datensammlung) aus der zurückgegebenen Richtlinie und stellt fest, dass es ein einziges Datensammlungselement mit der Berechtigung „Anforderungsgrund" gibt. Der Client stellt außerdem fest, dass das Flag „collectComments“ auf *false*festgelegt ist, und fordert den Benutzer daher nicht zur Eingabe eines Kommentars auf.

5.  Nachdem der Benutzer den Grund für das Anfordern der Zertifikate eingegeben hat, erstellt der Client eine Anforderung: 

    ```
    POST /CertificateManagement/api/v1.0/requests 
    
    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  Der Server erstellt erfolgreich die Anforderung und gibt den URI der Anforderung an den Client zurück: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  Der Client ruft das Anforderungsobjekt ab, indem er den zurückgegebenen URI aufruft:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/> 
8.  Der Client vergewissert sich, dass die „state“-Eigenschaft in der Anforderung auf „approved“ festgelegt ist. Die Anforderungsausführung kann beginnen.

9.  Der Client wertet die Anforderung aus, um festzustellen, ob bereits eine Smartcard mit der Anforderung verknüpft ist. Dazu analysiert er den Inhalt des Parameters „newsmartcarduuid“. 

10. Da der Parameter nur eine leere GUID enthält, muss der Client entweder eine vorhandene Karte verwenden, die noch nicht von MIM CM verwendet wird, oder eine solche erstellen für den Fall, dass die Profilvorlage für virtuelle Smartcards konfiguriert wird. 

11. Da dem Client über die ursprüngliche Abfrage für registrierbare Profilvorlagen (Schritt 1) Letzteres angegeben wurde, muss der Client nun ein virtuelles Smartcard-Gerät erstellen. 

12. Der Client ruft die Smartcard-Richtlinie aus der Profilvorlage ab (über die UUID der Vorlage, die in Schritt 3 ausgewählt wurde): 

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards 
    ```
13. Die Smartcard-Richtlinie enthält in der Eigenschaft „DefaultAdminKeyHex“ den Standardadministratorschlüssel für die Karte. Wenn der Client die Smartcard erstellt, muss er den anfänglichen Administratorschlüssel der Smartcard auf diesen Schlüssel festlegen.  

14. Beim Erstellen des Smartcard-Geräts muss der Client den Schlüssel der Anforderung zuweisen:

    ```
    POST /CertificateManagement/api/v1.0/smartcards 
    
    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
        "atr":"3b8d0180fba000000397425446590301c8"
    }
    ```
<br/>
15. Der Server antwortet dem Client mit einem URI für das neu erstellte Smartcard-Objekt: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. Der Client ruft das Smartcard-Objekt ob:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Durch Auswerten des Werts von „diversifyadminkey“ aus der Smartcard-Richtlinie, die in Schritt 12 abgerufen wurde, weiß der Client, dass er den Administratorschlüssel diversifizieren muss.

18. Der Client Ruft den vorgeschlagene Administratorschlüssel ab:

    ``` 
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9 
    ```
<br/>
19. Der Client muss sich bei der Karte als Administrator authentifizieren, um den Administratorschlüssel festzulegen. Zu diesem Zweck fordert der Client eine Authentifizierungsaufforderung von der Smartcard an und sendet diese Aufforderung an den Server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&atr=AFDEE6&diversified=false
    ```
<br/>
20. Der Server sendet die Aufforderungsantwort im HTTP-Antworttext, und der Client verwendet die Antwort, um den Administratorschlüssel zu diversifizieren.

21. Durch Auswerten des „UserPinOption“-Felds der Smartcard-Richtlinie stellt der Client fest, dass der Benutzer seine gewünschte PIN bereitstellen muss. 

22. Nachdem der Benutzer die gewünschte PIN in einem Dialogfeld eingegeben hat, führt der Client eine Aufforderung-Antwort-Authentifizierung aus, wie in Schritt 19 umrissen, allerdings mit dem einen Unterschied, dass das Flag für Diversifizierung jetzt auf „true“ festgelegt sein muss:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&atr=AFDEE6&diversified=true 
    ```
<br/>
23. Der Client verwendet die Antwort, die er vom Server empfangen hat, um die gewünschte Benutzer-PIN festzulegen.

24. Der Client kann nun Zertifikatanforderungen generieren. Er fragt die Parameter für die Zertifikatgenerierung aus der Profilvorlage ab, um zu bestimmen, wie die Schlüssel/Anforderungen generiert werden sollen. 

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. Der Server antwortet mit einem einzelnen JSON-serialisierten CertificateRequestGenerationOptions-Objekt, das folgende Informationen enthält: Exportierbarkeit des Schlüssels, Anzeigename des Zertifikats, Hashalgorithmus, Schlüsselalgorithmus, Schlüsselgröße usw. Der Client verwendet diese Parameter, um eine Zertifikatanforderung zu generieren.

26. Die einzelne Zertifikatanforderung wird an den Server gesendet. Der Client könnte zusätzlich ein PFX-Kennwort angeben, das verwendet werden soll, um irgendwelche PFX-BLOBs zu entschlüsseln für den Fall, dass die Zertifikatvorlage ein Archivieren der Zertifikate in der Zertifizierungsstelle angibt, d. h., die Zertifizierungsstelle generiert das Schlüsselpaar und sendet es an den Client. Der Client könnte auch einige Kommentare hinzufügen.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata
    
    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Nach wenigen Sekunden antwortet der Server mit einem einzigen JSON-serialisierten Microsoft.Clm.Shared.Certificate-Objekt. Durch Auswerten des Flags „isPkcs7“ stellt der Client fest, dass diese Antwort kein PFX-BLOB ist. Der Client extrahiert das BLOB durch eine Base64-Decodierung der „pkcs7“-Zeichenfolge und installiert es. 

28. Der Client markiert die Anforderung als abgeschlossen.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5 
    
    {
        "status":"Completed"
    }
    ```
<!--HONumber=Mar16_HO1-->
