---
Titel: Abrufen einer Smartcard-Authentifizierungsantwort
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: e05ec898-06cd-4c17-a4f4-8f3545af0f14
---
# Abrufen einer Smartcard-Authentifizierungsantwort
Ruft die Antwort auf eine Base CSP-Authentifizierungsabfrage ab.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

###URL-Parameter
Parameter | Beschreibung
---------|------------
reqid | Erforderlich. Der Bezeichner der Anforderung (MIM CM-spezifisch).
scid | Erforderlich. Der Bezeichner der Smartcard (MIM CM-spezifisch). Dieser wird aus dem [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx)-Objekt abgerufen.
###Abfrageparameter
Parameter | Beschreibung
---------|------------
atr | Optional. Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.
cardid | Erforderlich. Die Karten-ID.
challenge | Erforderlich. Eine Base64-codierte Zeichenfolge, die die von der Smartcard ausgestellte Abfrage darstellt.
diversified | Erforderlich. Ein boolesches Flag, das angibt, ob der Smartcard-Administratorschlüssel diversifiziert wurde.


###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Keiner

##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird ein Byte-BLOB zurückgegeben, das die Abfrageantwort darstellt.

##Beispiel

###Anforderung
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###Antwort
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##Weitere Informationen

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse-Methode](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.executeoperations.getbasecspresponse(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
