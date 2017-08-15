---
title: Abrufen einer per Smartcard vorgeschlagenen PIN | Microsoft-Dokumentation
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Abrufen einer per Smartcard vorgeschlagenen PIN
Ruft die vom Server generierte PIN eines Benutzers ab.

**Hinweis**: Der Server legt die PIN nur fest, wenn die Richtlinie der Profilvorlage angibt, dass dies erforderlich ist. Andernfalls muss sie vom Benutzer bereitgestellt werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner der Smartcard (MIM CM-spezifisch). Dieser wird aus dem „Microsft.Clm.Shared.Smartcard“-Objekt abgerufen.
###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
---------|------------
atr | Die ATR (Antwort zum Zurücksetzen) der Karte.
cardid | Die Karten-ID.
challenge | Eine Base64-codierte Zeichenfolge, die die von der Smartcard ausgestellte Abfrage darstellt.

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
Bei Erfolg wird eine Zeichenfolge zurückgegeben, die die vom Server vorgeschlagene PIN darstellt.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

... body coming soon
```       
