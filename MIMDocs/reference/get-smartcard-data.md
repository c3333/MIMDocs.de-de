---
Titel: Abrufen von Smartcard-Daten
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
---
# Abrufen von Smartcard-Daten
Ruft eine Liste der Smartcardprofile für einen Benutzer mit einer Liste möglicher Vorgänge ab, die vom aktuellen Benutzer ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> / CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###URL-Parameter
Eigenschaft| Beschreibung
---------|--------
smartcarduuid | Optional. Die von MIM CM angegebene Smartcard-UUID. Dies ist das Feld „uuid“ im Objekt [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx).

###Abfrageparameter
Eigenschaft| Beschreibung
---------|--------
cardid | Optional. Die von MIM CM angegebene Smartcard-UUID. Dies ist das Feld „uuid“ im Objekt [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx).


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
Bei Erfolg wird ein JSON-serialisiertes [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx)-Objekt mit den folgenden Eigenschaften zurückgegeben:

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

##Beispiel

###Anforderung 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###Antwort 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###Anforderung 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###Antwort 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
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
##Weitere Informationen

-[Microsoft.Clm.Shared.Smartcards.Smartcard-Klasse](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
