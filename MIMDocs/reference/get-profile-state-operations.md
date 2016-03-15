---
Titel: Abrufen der Profil-Vorgänge
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: f62c827b-5229-4b13-ad37-4f62ad231d30
---
# Abrufen von Profilzustandsvorgängen
Ruft eine Liste der möglichen Vorgänge ab, die vom aktuellen Benutzer für das angegebene Profil ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles/{ID}/Operations <br/>/CertificateManagement/API/v1.0/Smartcards/{ID}/Operations

###URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner (GUID) des Profils oder der Smartcard.

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
Bei Erfolg wird eine Liste der möglichen Vorgänge zurückgegeben, die vom aktuellen Benutzer für die Smartcard ausgeführt werden können. Diese Liste kann eine beliebige Anzahl der folgenden Elemente enthalten: *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Retire*, *Revoke*und *Unblock*.

##Beispiel

###Anforderung
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
<!--HONumber=Mar16_HO1-->
