---
title: Abrufen einer Smartcard-Authentifizierungsantwort | Microsoft-Dokumentation
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>Abrufen einer Smartcard-Authentifizierungsantwort
Ruft die Antwort auf eine Base CSP-Authentifizierungsabfrage ab.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

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
challenge | Erforderlich. Eine Base64-codierte Zeichenfolge, die die von der Smartcard ausgestellte Abfrage darstellt.
diversified | Erforderlich. Ein boolesches Flag, das angibt, ob der Smartcard-Administratorschlüssel diversifiziert wurde.


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
Bei Erfolg wird ein Byte-BLOB zurückgegeben, das die Abfrageantwort darstellt.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse Method](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
