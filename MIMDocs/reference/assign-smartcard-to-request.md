---
Titel: Zuweisen einer Smartcard zu einer Anforderung
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
---
# Zuweisen einer Smartcard zu einer Anforderung
Bindet die angegebene Smartcard an die angegebene Anforderung. Nach der Bindung kann die Anforderung nur noch mit dieser Smartcard ausgeführt werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###URL-Parameter
Keine
###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Der Anforderungstext enthält die folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-----------
requestid | Die ID der Anforderung, an die die Smartcard gebunden werden soll.
cardid | Die Karten-ID der Smartcard.
atr | Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.


##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
201     | Erstellt
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird ein URI für das neu erstellte Smartcard-Objekt zurückgegeben.
##Beispiel

###Anforderung
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###Antwort
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##Weitere Informationen

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard-Methode (Zeichenfolge, Zeichenfolge, Anforderung)](https://msdn.microsoft.com/en-us/library/windows/desktop/bb456812(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
