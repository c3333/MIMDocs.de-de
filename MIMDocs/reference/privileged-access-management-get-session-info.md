---
Titel: Abrufen von PAM-Sitzungsinformationen
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: bc30e455-9a9c-413a-b8ca-9669286299cf
---
# Abrufen von PAM-Sitzungsinformationen
Dies wird von einem privilegierten Konto verwendet, um den Benutzernamen des Kontos abzurufen, das für die Sitzung angemeldet ist.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/api/session/sessioninfo

###Abfrageparameter
Parameter | Beschreibung
----------|--------------
v | Optional. Die API-Version. Wenn diese nicht angegeben ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#Versioning)

###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](privileged-access-management-rest-api-service-details.md#HttpHeaders) in *PAM REST-API-Dienstdetails*.
###Anforderungstext
Keiner

##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200 | OK
401 | Nicht autorisiert
403 | Verboten
408 | Anforderungstimeout   
500 | Interner Serverfehler.
503 | Dienst nicht verfügbar.

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](privileged-access-management-rest-api-service-details.md#HttpHeaders) in *PAM REST-API-Dienstdetails*.
###Antworttext
Eine erfolgreiche Antwort gibt ein PAM-Sitzungsobjekt mit den folgenden Eigenschaften zurück.

Eigenschaft| Beschreibung
--------|-------------
Benutzername| Der Benutzername des Kontos, das in dieser Sitzung angemeldet ist.

##Beispiel

###Anforderung
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
<!--HONumber=Mar16_HO1-->
