---
Titel: Abrufen ausstehender PAM-Anforderungen
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 005dc8fd-d73e-4557-b485-5566f16537eb
---
# Abrufen ausstehender PAM-Anforderungen
Dies wird von einem privilegierten Konto verwendet, um eine Liste der ausstehenden Anforderungen zurückzugeben, für die eine Genehmigung erforderlich ist.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###Abfrageparameter
Parameter | Beschreibung
----------|--------------
$filter | Optional. Geben Sie eine der Eigenschaften für ausstehende PAM-Anforderungen in einem Filterausdruck an, um eine gefilterte Liste von Antworten zurückzugeben. Weitere Informationen zu unterstützten Operatoren finden Sie unter [Filterung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#Filtering)
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
Eine erfolgreiche Antwort enthält eine Liste der Genehmigungsobjekte der PAM-Anforderung mit den folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-------------
RoleName | Der Anzeigename der Rolle, für die die Genehmigung erforderlich ist.
Anforderungor | Der Benutzername des zu genehmigenden Requestor.
Justification | Die vom Benutzer bereitgestellte Ausrichtung.
AnforderungedTTL | Die angeforderte Ablaufzeit in Sekunden.
AnforderungedTime | Die angeforderte Zeit für die Rechteerweiterung.
CreationTime | Die Erstellungszeit der Anforderung.
FIMAnforderungID | Enthält ein einzelnes Element, „Wert“, mit dem eindeutigen Bezeichner (GUID) der PAM-Anforderung.
AnforderungorID | Enthält ein einzelnes Element, „Wert“, mit dem eindeutigen Bezeichner (GUID) für das Active Directory-Konto, das die PAM-Anforderung erstellt hat.
ApprovalObjectID | Enthält ein einzelnes Element, „Wert“, mit dem eindeutigen Bezeichner (GUID) für das Genehmigungsobjekt.

##Beispiel

###Anforderung
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
<!--HONumber=Mar16_HO1-->
