---
title: Abrufen von Smartcard-Daten | Microsoft-Dokumentation
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>Abrufen von Smartcard-Daten
Ruft eine Liste der Smartcardprofile für einen Benutzer mit einer Liste möglicher Vorgänge ab, die vom aktuellen Benutzer ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{Smartcard-UUID}


###<a name="url-parameters"></a>URL-Parameter
Eigenschaft| Beschreibung
---------|--------
smartcarduuid | (Optional) Die von MIM CM angegebene Smartcard-UUID. Dieser wird aus dem Feld „uuid“ im [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)-Objekt abgerufen.

###<a name="query-parameters"></a>Abfrageparameter
Eigenschaft| Beschreibung
---------|--------
cardid | (Optional) Die von MIM CM angegebene Smartcard-UUID. Dieser wird aus dem Feld „uuid“ im [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)-Objekt abgerufen.


###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Keiner

##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###<a name="response-headers"></a>Antwortheader
Informationen zu weit verbreiteten Antwortheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="response-body"></a>Antworttext
Bei Erfolg wird ein JSON-serialisiertes [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)-Objekt mit den folgenden Eigenschaften zurückgegeben:

Name | Beschreibung
-----|-----------
AssignedUserUuid | Der Bezeichner des Benutzers, dem die Smartcard zugewiesen ist.
Atr | Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard für die Karte, die derzeit initialisiert wird.
Kommentar | Der Kommentar, der die Smartcard beschreibt.
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

##<a name="example"></a>Beispiel

###<a name="request-1"></a>Anforderung 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>Antwort 1
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
###<a name="request-2"></a>Anforderung 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>Antwort 2
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
##<a name="see-also"></a>Siehe auch

-[Microsoft.Clm.Shared.Smartcards.Smartcard-Klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
