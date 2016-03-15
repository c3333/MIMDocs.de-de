---
Titel: Abrufen von Profilvorlagen
MS.Custom: 
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology: 
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
---
# Abrufen von Profilvorlagen
Ruft eine Liste der Profilvorlagen ab, für die sich der angegebene Benutzer registrieren kann. Diese Methode gibt eine eingeschränkte Ansicht der Profilvorlage zurück. Die zurückgegebenen Profilvorlagendaten sollten ausreichend sein, damit der anfordernde Benutzer entscheiden kann, für welche Profilvorlage er sich ggf. registriert. Wenn keine Workflows und Berechtigungen angegeben sind, werden alle für den Benutzer sichtbaren Profilvorlagen zurückgegeben.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###URL-Parameter
Parameter| Beschreibung
--------|-------------
targetuser| Optional. Gibt den Zielbenutzer an, für den Profilvorlagen zurückgegeben werden sollen. Wenn dieser nicht angegeben ist, wird die Identität des aktuellen Benutzers verwendet. <br/>**Hinweis**: Derzeit wird nur der aktuelle Benutzer unterstützt.

###Anforderungsheader
Allgemeine Anforderungsheader finden Sie im Thema „Dienst“.
###Anforderungstext
Keiner

##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
204 | Kein Inhalt
500 | Interner Fehler

###Antwortheader
Allgemeine Antwortheader finden Sie im Thema „Dienst“.
###Antworttext
Bei Erfolg wird eine Liste von „ProfileTemplateLimitedView“-Objekten mit den folgenden Eigenschaften zurückgegeben.

Eigenschaft| Typ| Beschreibung
--------|-----|--------
Name| string| Der Anzeigename der Profilvorlage.
Beschreibung| string| Die Beschreibung für die Profilvorlage.
Uuid| Guid| Der Bezeichner für die Profilvorlage.
IsSmartcardProfileTemplate| bool| Gibt an, ob es sich bei der Vorlage um eine Smartcard-Profilvorlage handelt.
IsVirtualSmartcardProfileTemplate| bool| Gibt an, ob für die Profilvorlage eine virtuelle Smartcard erforderlich ist.

##Beispiel

###Anforderung
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###Antwort
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
<!--HONumber=Mar16_HO1-->
