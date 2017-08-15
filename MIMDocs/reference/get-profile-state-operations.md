---
title: "Abrufen von Profilzustandsvorgängen | Microsoft-Dokumentation"
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>Abrufen von Profilzustandsvorgängen
Ruft eine Liste der möglichen Vorgänge ab, die vom aktuellen Benutzer für das angegebene Profil ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{ID}/operations

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner (GUID) des Profils oder der Smartcard.

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
Bei Erfolg wird eine Liste der möglichen Vorgänge zurückgegeben, die vom aktuellen Benutzer für die Smartcard ausgeführt werden können. Diese Liste kann eine beliebige Anzahl der folgenden Elemente enthalten: *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Retire*, *Revoke*und *Unblock*.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
