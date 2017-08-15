---
title: "Abrufen eines per Smartcard diversifizierten Administratorschlüssels | Microsoft-Dokumentation"
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
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 57a8481b2a4f976717b07061e96ccb041164a5a6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-diversified-admin-key"></a>Abrufen eines per Smartcard diversifizierten Admininistratorschlüssels
Ruft den diversifizierten Administratorschlüssel für die angegebene Smartcard ab.

**Hinweis**: Der Administratorschlüssel muss nur diversifiziert werden, wenn dies in der Richtlinie der Profilvorlage angegeben ist.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
---------|------------
reqid | Erforderlich. Der Bezeichner der Anforderung (MIM CM-spezifisch).
scid | Erforderlich. Der Bezeichner der Smartcard (MIM CM-spezifisch). Dieser wird aus dem [Microsft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)-Objekt abgerufen.
###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
---------|------------
atr | Optional. Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.
cardid | Erforderlich. Die Karten-ID.

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
Bei Erfolg wird ein Byte-BLOB zurückgegeben, das den diversifizierten Administratorschlüssel darstellt.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##<a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard-Methode (String, String, Anforderung)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
