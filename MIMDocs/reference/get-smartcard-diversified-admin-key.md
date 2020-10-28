---
title: Smartcard-diversifizierten Administrator Schlüssel erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6668ac823607436c2472a076f7c5ea2d9c727b04
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760398"
---
# <a name="get-smart-card-diversified-admin-key"></a>Smartcard-diversifizierten Administrator Schlüssel erhalten
Ruft den diversifizierten Administrator Schlüssel für die angegebene Smartcard ab.

>[!IMPORTANT]
>Der Administrator Schlüssel sollte nur dann diversifiziert werden, wenn die Richtlinie für die Profil Vorlage angibt, dass er lauten soll.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
---------|------------
reqid | Erforderlich. Der Anforderungs Bezeichner, der spezifisch für Microsoft Identity Manager (MIM) Certificate Management (cm) ist.
scid | Erforderlich. Der Bezeichner der Smartcard, der für MIM cm spezifisch ist. Die scid wird aus dem [Microsoft. CLM. shared. Smartcards. Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) -Objekt abgerufen.

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
---------|------------
atr | Optional. Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.
cardid | Erforderlich. Die Karten-ID.

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
Bei Erfolg wird ein Byte-BLOB zurückgegeben, das den diversifizierten Administratorschlüssel darstellt.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, mit dem Sie den diversifizierten Administrator Schlüssel für eine Smartcard erhalten.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## <a name="see-also"></a>Weitere Informationen

- [Microsoft. CLM. Provision. requestoperations. kreatesmartcard-Methode (Zeichenfolge, Zeichenfolge, Anforderung)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
