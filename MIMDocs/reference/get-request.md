---
Titel: Get-Anforderung
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: dcacf36c-0670-44d7-9f40-388667235271
---
# Abrufen von Anforderungen
Ruft mindestens eine angegebene MIM CM-Anforderung ab.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

###URL-Parameter
Eigenschaft| Beschreibung
---------|--------
id| Optional. Die GUID einer abzurufenden Anforderung.
###Abfrageparameter
Eigenschaft| Beschreibung
---------|--------
targetuser| Optional. Der Zielbenutzer der Anforderung. Wenn kein Ziel angegeben ist, werden alle Anforderungen unabhängig vom Zielbenutzer zurückgegeben. <br/> **Hinweis**: Derzeit wird nur „Aktuell“ unterstützt.
Status| Optional. Gibt den Status der abzurufenden Anforderung an. Mögliche Statustypen: *Genehmigt*, *Abgebrochen*, *Abgeschlossen*, *Verweigert*, *Ausführen*, *Fehler*, *Kein*und *Ausstehend*. <br/>Wenn kein Status angegeben wird, werden alle Anforderungen unabhängig vom Status zurückgegeben.

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
Bei Erfolg wird mindestens ein [Request](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.requests.request(v=vs.100%29.aspx)-Objekt mit den folgenden Eigenschaften zurückgegeben:

Name | Beschreibung
-----|------------
Anmerkungen | Der Kommentar, der der MIM CM-Anforderung zugeordnet ist.
Abgeschlossen | Der Zeitpunkt, zu dem die MIM CM-Anforderung abgeschlossen wurde.
DataCollection | Die Datensammlungselemente, die der MIM CM-Anforderung zugeordnet sind.
DataCollectionFlags | Die Optionen für die Datensammlung für die MIM CM-Anforderung.
Flags | Die Optionen, die der MIM CM-Anforderung zugeordnet sind.
IsDataCollectionComplete | Ein boolescher Wert, der angibt, ob die Datensammlung für die MIM CM-Anforderung abgeschlossen wurde.
IsEnrollmentAgent | Ein boolescher Wert, der angibt, ob ein Registrierungs-Agent erforderlich ist, um die MIM CM-Anforderung auszuführen.
IsSmartcard | Ein boolescher Wert, der angibt, ob die MIM CM-Anforderung eine Smartcard- oder Softwareprofilanforderung ist.
NewProfileUuid | Die Identität des neuen Softwareprofils, die durch die MIM CM-Anforderung erstellt wird.
NewSmartcardUuid | Die Identität der neuen Smartcard, die durch die MIM CM-Anforderung zugewiesen wird.
OldProfileUuid | Die Identität des Softwareprofils, für das die MIM CM-Anforderung erstellt wurde.
OldSmartcardUuid | Die Identität der Smartcard, für die die MIM CM-Anforderung erstellt wurde.
OriginatorUserUuid | Die Identität des Benutzers, von dem die MIM CM-Anforderung stammt.
Priorität | Die Priorität für die MIM CM-Anforderung.
ProfileTemplateUuid | Die Identität der Profilvorlage, für die die MIM CM-Anforderung erstellt wurde.
AnforderungType | Der Typ der MIM CM-Anforderung.
SecurityDescriptor | Die Sicherheitsbeschreibung für die MIM CM-Anforderung.
Status | Der Status der MIM CM-Anforderung.
Submitted | Der Zeitpunkt, zu dem die MIM CM-Anforderung übermittelt wurde.
TargetUserUuid | Die Identität des Zielbenutzers für die MIM CM-Anforderung.
Uuid | Der Bezeichner für die MIM CM-Anforderung.

##Beispiel

###Anforderung 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###Antwort 1
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}```       
###Request 2
```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.requestpermission(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.requests.request(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
