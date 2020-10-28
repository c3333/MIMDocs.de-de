---
title: Zuweisen einer Smartcard zu einer Anforderung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a8acf3defc2087fc6ea08bf8063579058465fcc8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760506"
---
# <a name="assign-a-smart-card-to-a-request"></a>Zuweisen einer Smartcard zu einer Anforderung
Bindet die angegebene Smartcard an die angegebene Anforderung. Wenn eine Smartcard gebunden ist, kann die Anforderung nur mit der angegebenen Karte ausgeführt werden.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

### <a name="url-parameters"></a>URL-Parameter
Keine.

### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Der Anforderungs Text enthält die folgenden Eigenschaften:

Eigenschaft | BESCHREIBUNG
---------|-----------
requestid | Die ID der Anforderung, die an die Smartcard gebunden werden soll.
cardid | Die Karten-ID der Smartcard.
atr | Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.


## <a name="response"></a>Antwort
In diesem Abschnitt wird die-Antwort beschrieben.

### <a name="response-codes"></a>Antwort Codes

Code  |BESCHREIBUNG  
---------|---------
201 | Erstellt
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Antwort Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Bei Erfolg wird ein URI für das neu erstellte Smartcard-Objekt zurückgegeben.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum Binden einer Smartcard.

### <a name="example-request"></a>Beispiel: Anforderung

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## <a name="see-also"></a>Weitere Informationen

- [Microsoft. CLM. Provision. requestoperations. kreatesmartcard-Methode (Zeichenfolge, Zeichenfolge, Anforderung)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
