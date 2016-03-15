---
Titel: Abbrechen, verwerfen oder Abschließen von Anforderungen
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: e29e0068-7602-455e-8a3a-690da9ca8eb5
---
# Abbrechen, Verwerfen oder Abschließen von Anforderungen
Markiert eine MIM CM-Anforderung als abgeschlossen, abgebrochen oder verworfen.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

###URL-Parameter
Eigenschaft| Beschreibung
---------|--------
id| Erforderlich. Die GUID der abzuschließenden Anforderung.


###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Der Anforderungstext enthält die folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-----------
Status | Der Status, den die Anforderung erhalten soll. Die folgenden Werte sind möglich: „Abgeschlossen“, „Abgebrochen“ oder „Verworfen“.


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
Gibt bei Erfolg ein [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.requests.request.aspx) -Objekt mit den folgenden Eigenschaften zurück, die die MIM CM-Anforderung beschreiben, die als abgeschlossen markiert wurde:

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

###Anforderung
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###Antwort
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##Weitere Informationen

- [Microsoft.Clm.Provision.ExecuteOperations.Complete-Methode](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Anforderungs.Anforderung](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.requests.request.aspx)
<!--HONumber=Mar16_HO1-->
