---
Titel: Abrufen von Profildaten
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
---
# Abrufen von Profildaten
Ruft eine Liste der Softwarezertifikatprofile für einen Benutzer mit einer Liste möglicher Vorgänge ab, die vom aktuellen Benutzer ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

**Hinweis**: Der Server legt die PIN nur fest, wenn die Richtlinie der Profilvorlage angibt, dass dies erforderlich ist. Andernfalls muss sie vom Benutzer bereitgestellt werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles<br/>/ CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/API/v1.0/Requests/{RequestId}/Profiles

###URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner (GUID) des zurückzugebenden Profils.
requestId | Der Bezeichner der Anforderung, für die die Profile zurückgegeben werden sollen.

###Abfrageparameter
Parameter | Beschreibung
---------|------------
Status | Optional. Gibt den Status der Profile an, für die Daten abgerufen werden sollen. Mögliche Statustypen: „Aktiv“, „Genehmigt“, „Abgebrochen“, „Abgeschlossen“, „Verweigert“, „Ausführen“, „Fehler“, „Kein“ und „Ausstehend“. <br/>Wenn kein Status angegeben wird, werden alle Profile unabhängig vom Status zurückgegeben.
###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Keiner

##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200 | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird eine Liste von JSON-serialisierten Objekten [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.profiles.profile(v=vs.100%29.aspx) mit den folgenden Eigenschaften zurückgegeben:

Eigenschaft | Beschreibung
---------|------------
AssignedUserUuid | Der Bezeichner des Benutzers, dem das Profil zugewiesen ist.
Anmerkungen | Der Kommentar, der das Profil beschreibt.
Flags | Die Flags, die das Profil beschreiben.
ParentProfileUuid | Der Bezeichner des alten Profils, das durch das Profil ersetzt wurde.
PrimaryProfileUuid | Der Bezeichner des primären Profils.
ProfileOperations | Die Liste der möglichen Vorgänge, die vom aktuellen Benutzer mit dem Profil ausgeführt werden können.
ProfileTemplateUuid | Der Bezeichner der Profilvorlage, die die Richtlinien und Einstellungen enthält, die das Profil bestimmen.
ProfileTemplateVersion | Die Version der Profilvorlage zum Zeitpunkt der Erstellung des Profils.
Status | Der Status des Profils.
Uuid | Der Bezeichner des Profils.


##Beispiel

###Anforderung
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##Weitere Informationen

- [Microsoft.Clm.Shared.Profiles.Profile-Klasse](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.profiles.profile(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
