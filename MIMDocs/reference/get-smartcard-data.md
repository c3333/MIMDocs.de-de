---
title: Smartcardprofile erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0666a17abe63b0efbccd59aa0b9e0bb5daf80fe5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760623"
---
# <a name="get-smart-card-profiles"></a>Smartcardprofile erhalten
Ruft eine Liste der Smartcardprofile für einen Benutzer ab. Die Liste enthält die möglichen Vorgänge, die vom aktuellen Benutzer ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{Smartcard-UUID}


### <a name="url-parameters"></a>URL-Parameter

Eigenschaft| BESCHREIBUNG
---------|--------
smartcarduuid | Optional. Die Smartcard-UUID, die durch Microsoft Identity Manager (MIM) Certificate Management (cm) angegeben wird. Der Wert entspricht dem Feld "uuid" im [Microsoft. CLM. shared. Smartcards. Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) -Objekt.

### <a name="query-parameters"></a>Abfrageparameter

Eigenschaft| BESCHREIBUNG
---------|--------
cardid | Optional. Die von MIM cm angegebene Smartcard-UUID. Der Wert entspricht dem Feld "uuid" im [Microsoft. CLM. shared. Smartcards. Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) -Objekt.

### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Keine.

## <a name="response"></a>Antwort
In diesem Abschnitt wird die-Antwort beschrieben.

### <a name="response-codes"></a>Antwort Codes

Code  |BESCHREIBUNG  
---------|---------
200 | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Antwort Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Bei Erfolg wird ein JSON-serialisiertes [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)-Objekt mit den folgenden Eigenschaften zurückgegeben:

Name | BESCHREIBUNG
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

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum erhalten von smartcardprofilen für einen Benutzer.

### <a name="example-request-1"></a>Beispiel: Anforderung 1

```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1
```

### <a name="example-response-1"></a>Beispiel: Antwort 1

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

### <a name="example-request-2"></a>Beispiel: Anforderung 2

```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response-2"></a>Beispiel: Antwort 2

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

## <a name="see-also"></a>Siehe auch

- [Microsoft. CLM. shared. Smartcards. Smartcard-Klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
