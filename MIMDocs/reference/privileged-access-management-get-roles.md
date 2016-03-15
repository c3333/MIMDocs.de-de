---
Titel: Abrufen von PAM-Rollen
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
---
# Abrufen von PAM-Rollen
Dies wird von einem privilegierten Konto verwendet, um die PAM-Rollen aufzulisten, für die das Konto geeignet ist.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/api/pamresources/pamroles

###Abfrageparameter
Parameter | Beschreibung
----------|--------------
$filter | Optional. Geben Sie eine der PAM-Rolleneigenschaften in einem Filterausdruck an, um eine gefilterte Liste von Antworten zurückzugeben. Weitere Informationen zu unterstützten Operatoren finden Sie unter [Filterung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#Filtering)
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
Eine erfolgreiche Antwort enthält eine Sammlung von mindestens einer PAM-Rolle, die jeweils die folgenden Eigenschaften aufweisen.

Eigenschaft | Beschreibung
--------|-------------
RoleID | Der eindeutige Bezeichner (GUID) der PAM-Rolle.
DisplayName | Der Anzeigename der PAM-Rolle im MIM-Dienst.
Beschreibung | Die Beschreibung der PAM-Rolle im MIM-Dienst.
TTL | Der maximale Ablauftimeout für die Zugriffsrechte der Rolle in Sekunden.
AvailableFrom | Die früheste Uhrzeit, zu der eine Anforderung aktiviert wird.
AvailableTo | Die späteste Uhrzeit, zu der eine Anforderung aktiviert wird.
MFAEnabled | Ein boolescher Wert, der angibt, ob Aktivierungsanforderungen für diese Rolle eine MFA-Abfrage erfordern.
ApprovalEnabled | Ein boolescher Wert, der angibt, ob Aktivierungsanforderungen für diese Rolle eine Genehmigung eines Rollenbesitzers erfordern.
AvailibitlyWindowEnabled | Ein boolescher Wert, der angibt, ob die Rolle nur während eines bestimmten Zeitintervalls aktiviert werden kann.

##Beispiel

###Anforderung
```
GET /api/pamresources/pamroles HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
<!--HONumber=Mar16_HO1-->
