---
Titel: Aktualisieren des Smartcard-Status
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 598dace3-c6f2-447a-9301-c0b63ee38276
---
# Aktualisieren des Smartcard-Status
Aktualisiert den Status einer Smartcard.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
## Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### URL-Parameter
Parameter | Beschreibung
---------|------------
reqid | Erforderlich. Der Bezeichner der Anforderung (MIM CM-spezifisch).
scid | Erforderlich. Der Bezeichner der Smartcard (MIM CM-spezifisch). Dies ist die Eigenschaft „uuid“ im Objekt [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx).

### Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
### Anforderungstext
Der Anforderungstext enthält die folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-----------
Status | Der Status, den die Anforderung erhalten soll. Beispiel: „Zurückgezogen“.


## Antwort
### Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

### Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
### Antworttext
Bei Erfolg wird ein JSON-serialisierten [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) Objekt mit den folgenden Eigenschaften:

Name | Beschreibung
-----|-----------
AssignedUserUuid | Der Bezeichner des Benutzers, dem die Smartcard zugewiesen ist.
Atr | Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard für die Karte, die derzeit initialisiert wird.
Anmerkungen | Der Kommentar, der die Smartcard beschreibt.
Flags | Die Flags, die die Smartcard beschreiben.
Middleware | Die Middleware für die Smartcard.
ParentSmartcardUuid | Der Bezeichner der alten Smartcard, die von der Smartcard ersetzt wurde.
PermanentSmartcardUuid | Der Bezeichner für die permanente Smartcard, der mit der Smartcard zugewiesen wird.
PrimarySmartcardUuid | Der Bezeichner der primären Smartcard.
ProfileTemplateUuid | Der Bezeichner der Profilvorlage, die die Richtlinien und Einstellungen enthält, die die Smartcard bestimmen.
ProfileTemplateVersion | Die Version der Profilvorlage zum Zeitpunkt der Erstellung der Smartcard.
SerialNumber | Die Seriennummer der Smartcard.
Status | Der Status der Smartcard.
Uuid | Der Bezeichner für das Profil der Smartcard.

## Beispiel

### Anforderung
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### Antwort
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
## Siehe auch

- [Microsoft.Clm.Shared.Smartcards.Smartcard-Klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
<!--HONumber=Mar16_HO1-->
