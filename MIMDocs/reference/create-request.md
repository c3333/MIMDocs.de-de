---
Titel: Request erstellen
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
---
# Erstellen von Anforderungen
Erstellen Sie eine MIM CM-Anforderung.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###URL-Parameter
Keiner

###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Der Anforderungstext enthält die folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-----------
profiletemplateuuid | Erforderlich. Die GUID der Profilvorlage, für die der Benutzer die Anforderung erstellt.
datacollection | Erforderlich. Eine Sammlung von Name-Wert-Paaren, die die vom Registrierenden bereitzustellenden Daten darstellen. Die Sammlung der erforderlichen Daten, die bereitgestellt werden müssen, können über die Workflowrichtlinie der Profilvorlage abgerufen werden. Eine leere Sammlung kann angegeben werden.
target | Optional. Die GUID des Zielbenutzers, für den die Anforderung erstellt werden soll. Wenn diese nicht angegeben ist, wird standardmäßig der aktuelle Benutzer verwendet.
Typ | Erforderlich. Der Typ der zu erstellenden Anforderung. Verfügbare Anforderungstypen: "Registrieren", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Erneuern", "Recover", "RecoverOnBehalf", "Reinstate", "Retire", "Revoke", "TemporaryCards" und "Zulassen".<br/>**Hinweis**: Nicht alle Typen von Anforderungen werden in allen Profilvorlagen unterstützt. Sie können z. B. keinen „unblock“-Vorgang für eine Softwareprofilvorlage angeben.
Kommentar | Erforderlich. Beliebige Kommentare, die vom Benutzer eingegeben werden können. Die Workflowrichtlinie definiert, ob dies erforderlich ist. Eine leere Zeichenfolge kann angegeben werden.
priority | Optional. Die Priorität der Anforderung. Wenn diese nicht angegeben ist, wird die Standardanforderungspriorität gemäß den Einstellungen der Profilvorlage verwendet.


##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
201     | Erstellt
403 | Verboten
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird der URI für die neu erstellte Anforderung zurückgegeben.
##Beispiel

###Anforderung 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###Antwort 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###Anforderung 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###Antwort 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###Anforderung 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##Weitere Informationen

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll-Methode](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock-Methode](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover-Methode](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire-Methode](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock-Methode](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
