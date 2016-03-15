---
Titel: Schließen PAM Request
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
---
# Schließen von PAM-Anforderungen
Dies wird von einem privilegierten Konto verwendet, um eine Anforderung zu schließen, die zur Rechteerweiterung für eine PAM-Rolle initiiert wurde.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###URL-Parameter
Parameter | Beschreibung
----------|-----------
requestId | Der Bezeichner (GUID) der zu schließenden PAM-Rolle, der wie folgt angegeben wird: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

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
Keiner
##Beispiel

###Anforderung
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###Antwort
```
HTTP/1.1 200 OK

```       
<!--HONumber=Mar16_HO1-->
