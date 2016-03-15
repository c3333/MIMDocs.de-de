---
Titel: PAM Request erstellen
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
---
# Erstellen von PAM-Anforderungen
Dies wird von einem privilegierten Konto für die Rechteerweiterung auf eine PAM-Rolle verwendet.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequests

###Abfrageparameter
Parameter | Beschreibung
--------|-------------
Justification | Optional. Der vom Benutzer angegebene Grund für die Anforderung zur Rechteerweiterung.
RoleId| Erforderlich. Der eindeutige Bezeichner (GUID) der PAM-Rolle für die Rechteerweiterung.
AnforderungedTTL| Erforderlich. Die angeforderte Ablaufzeit in Sekunden.
AnforderungedTime | Optional. Die für die Rechteerweiterung erforderliche Zeit.  
v | Optional. Die API-Version. Wenn diese nicht angegeben ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#Versioning)

**Hinweis**: Sie können die Parameter *Justification*, *RoleId*, *RequestedTTL*und *RequestedTime* im Anforderungstext als Eigenschaften anstatt als Abfrageparameter angeben. Der Parameter *v* kann nur als Abfrageparameter angegeben werden.

###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](privileged-access-management-rest-api-service-details.md#HttpHeaders) in *PAM REST-API-Dienstdetails*.
###Anforderungstext
Optional. Wie bereits erwähnt, können die Parameter *Justification*, *RoleId*, *RequestedTTL*und *RequestedTime* im Anforderungstext als Eigenschaften angegeben werden, anstatt sie in der URL-Abfragezeichenfolge anzugeben.

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
Eine erfolgreiche Antwort enthält ein PAM-Anforderungsobjekt mit den folgenden Eigenschaften.

Eigenschaft | Beschreibung
--------|-------------
Anforderungs-ID | Der eindeutige Bezeichner (GUID) für die PAM-Anforderung.
CreatorID | Der eindeutige Bezeichner (GUID) im MIM-Dienst für das Konto, mit dem die Anforderung erstellt wurde.
Justification | Der Grund für die Rechteerweiterung.
DisplayName | Der Anzeigename der PAM-Anforderung in MIM.
CreationTime | Die Erstellungszeit der Anforderung.
CreationMethode | Die Methode, die zum Erstellen der Anforderung verwendet wurde.
ExpirationTime | Die Ablaufzeit der Anforderung.
RoleID| Der eindeutige Bezeichner (GUID) der PAM-Rolle.
AnforderungedTTL | Der angeforderte Ablauftimeout in Sekunden.
AnforderungedTime | Der angeforderte Zeitpunkt für die Rechteerweiterung.
AnforderungedStatus | Der Status der Anforderung. Die folgenden Werte sind möglich: „Aktiv“, „Geschlossen“, „Wird geschlossen“, „Genehmigung ausstehend“ und „Abgelehnt“

##Beispiel

###Anforderung 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###Antwort 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###Anforderung 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###Antwort 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
<!--HONumber=Mar16_HO1-->
