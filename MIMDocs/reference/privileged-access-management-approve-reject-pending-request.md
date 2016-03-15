---
Titel: genehmigen oder Ablehnen einer ausstehenden PAM Request
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 0632656f-ecf4-4090-85a8-216d5638140a
---
# Genehmigen oder Ablehnen einer ausstehenden PAM-Anforderung
Dies wird von einem privilegierten Konto verwendet, um die Anforderung zur Rechteerweiterung für einer PAM-Rolle zu genehmigen, zu schließen oder abzulehnen.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/API/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/API/pamresources/pamrequeststoapprove({approvalId)/Reject

###URL-Parameter
Parameter | Beschreibung
----------|-----------
approvalId | Der Bezeichner (GUID) des Genehmigungsobjekts in PAM, der wie folgt angegeben wird: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###Abfrageparameter
Parameter | Beschreibung
----------|--------------
v | Optional. Die API-Version. Wenn diese nicht angegeben ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#Versioning)


###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](privileged-access-management-rest-api-service-details.md#HttpHeaders) in *PAM REST-API-Dienstdetails*.
###Anforderungstext
Kein

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
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###Antwort
```
HTTP/1.1 200 OK

```       
<!--HONumber=Mar16_HO1-->
