---
Titel: Abrufen von Smartcard vorgeschlagenen PIN
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: ced93932-9912-4b32-9586-ada69b38a796
---
# Abrufen einer per Smartcard vorgeschlagenen PIN
Ruft die vom Server generierte PIN eines Benutzers ab.

**Hinweis**: Der Server legt die PIN nur fest, wenn die Richtlinie der Profilvorlage angibt, dass dies erforderlich ist. Andernfalls muss sie vom Benutzer bereitgestellt werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

###URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner der Smartcard (MIM CM-spezifisch). Dieser wird aus dem „Microsft.Clm.Shared.Smartcard“-Objekt abgerufen.
###Abfrageparameter
Parameter | Beschreibung
---------|------------
atr | Die ATR (Antwort zum Zurücksetzen) der Karte.
cardid | Die Karten-ID.
challenge | Eine Base64-codierte Zeichenfolge, die die von der Smartcard ausgestellte Abfrage darstellt.

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
Bei Erfolg wird eine Zeichenfolge zurückgegeben, die die vom Server vorgeschlagene PIN darstellt.

##Beispiel

###Anforderung
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

... body coming soon
```       
<!--HONumber=Mar16_HO1-->
