    ---
Titel: Abrufen von Generierungsoptionen für von Zertifikat anfordern
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
---
# Abrufen von Generierungsoptionen für Zertifikatanforderungen
Ruft die Parameter für die Generierung der clientseitigen Zertifikatanforderungen ab.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###URL-Parameter
Parameter | Beschreibung
--------|--------------
requestid| Erforderlich. Der GUID-Bezeichner der MIM CM-Anforderung, für die die Generierungsparameter der Zertifikatanforderung abgerufen werden sollen.

###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Kein


##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200 | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird eine Liste der „CertificateRequestGenerationOptions“-Objekte zurückgegeben. Jedes „CertificateRequestGenerationOptions“-Objekt entspricht einer einzelnen Zertifikatanforderung, die vom Client generiert werden muss und die folgenden Eigenschaften aufweisen soll:

Eigenschaft| Beschreibung
--------|-----------
Exportable | Ein Wert, der angibt, ob der für die Anforderung erstellte private Schlüssel exportiert werden kann.
FriendlyName | Der Anzeigename des eingeschriebenen Zertifikats.
HashAlgorithmName | Der Hashalgorithmus, der beim Erstellen der Signatur der Zertifikatanforderung verwendet wird.
KeyAlgorithmName | Der Algorithmus für öffentliche Schlüssel.
KeyProtectionLevel | Die Stufe des starken Schlüsselschutzes.
KeySize | Die Größe des privaten Schlüssels in Bits, der generiert werden soll.
KeyStorageProviderNames | Eine Liste der zulässigen Schlüsselspeicheranbieter, mit denen der private Schlüssel generiert werden kann. Falls der erste Schlüsselspeicheranbieter nicht zum Generieren der Zertifikatanforderung verwendet werden kann, wird so lange einer der anderen angegebenen Schlüsselspeicheranbieter verwendet, bis der Vorgang erfolgreich ausgeführt wurde.
KeyUsages | Der Vorgang, der vom privaten Schlüssel ausgeführt werden kann, der für diese Zertifikatanforderung erstellt wurde. Der Standardwert ist „Signatur“.
Antragsteller | Der Name des Antragstellers.

**Hinweis**: Weitere Informationen zu diesen Eigenschaften finden Sie in der Beschreibung der [Windows.Security.Cryptography.Certificates.CertificateRequestProperties-Klasse](https://msdn.microsoft.com/en-us/library/windows/apps/br212079.aspx) , aber beachten Sie, dass zwischen dieser Klasse und den „CertificateRequestGenerationOptions“-Objekten keine eindeutige Beziehung besteht.

##Beispiel

###Anforderung
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###Antwort
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
<!--HONumber=Mar16_HO1-->
