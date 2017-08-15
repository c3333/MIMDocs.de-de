---
title: Zuweisen einer Smartcard zu einer Anforderung | Microsoft-Dokumentation
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Zuweisen einer Smartcard zu einer Anforderung
Bindet die angegebene Smartcard an die angegebene Anforderung. Nach der Bindung kann die Anforderung nur noch mit dieser Smartcard ausgeführt werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###<a name="url-parameters"></a>URL-Parameter
Keine
###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Der Anforderungstext enthält die folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-----------
requestid | Die ID der Anforderung, an die die Smartcard gebunden werden soll.
cardid | Die Karten-ID der Smartcard.
atr | Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.


##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
201     | Erstellt
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###<a name="response-headers"></a>Antwortheader
Informationen zu weit verbreiteten Antwortheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="response-body"></a>Antworttext
Bei Erfolg wird ein URI für das neu erstellte Smartcard-Objekt zurückgegeben.
##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Antwort
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard-Methode (String, String, Anforderung)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
