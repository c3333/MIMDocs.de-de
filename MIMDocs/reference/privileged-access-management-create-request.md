---
title: Erstellen von PAM-Anforderungen | Microsoft-Dokumentation
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Erstellen von PAM-Anforderungen
Dies wird von einem privilegierten Konto für die Rechteerweiterung auf eine PAM-Rolle verwendet.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
--------|-------------
Justification | (Optional) Der vom Benutzer angegebene Grund für die Anforderung zur Rechteerweiterung.
RoleId| Erforderlich. Der eindeutige Bezeichner (GUID) der PAM-Rolle für die Rechteerweiterung.
RequestedTTL| Erforderlich. Die angeforderte Ablaufzeit in Sekunden.
AnforderungedTime | Optional. Die für die Rechteerweiterung erforderliche Zeit.  
v | (Optional) Die API-Version. Wenn diese nicht angegeben ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#versioning)

**Hinweis**: Sie können die Parameter *Justification*, *RoleId*, *RequestedTTL*und *RequestedTime* im Anforderungstext als Eigenschaften anstatt als Abfrageparameter angeben. Der Parameter *v* kann nur als Abfrageparameter angegeben werden.

###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
(Optional) Wie bereits erwähnt, können die Parameter *Justification*, *RoleId*, *RequestedTTL*und *RequestedTime* im Anforderungstext als Eigenschaften angegeben werden, anstatt sie in der URL-Abfragezeichenfolge anzugeben.

##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
200 | OK
401 | Nicht autorisiert
403 | Verboten
408 | Anforderungstimeout   
500 | Interner Serverfehler.
503 | Dienst nicht verfügbar.

###<a name="response-headers"></a>Antwortheader
Informationen zu weit verbreiteten Antwortheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST-API-Dienstdetails*.
###<a name="response-body"></a>Antworttext
Eine erfolgreiche Antwort enthält ein PAM-Anforderungsobjekt mit den folgenden Eigenschaften.

Eigenschaft | Beschreibung
--------|-------------
Anforderungs-ID | Der eindeutige Bezeichner (GUID) für die PAM-Anforderung.
CreatorID | Der eindeutige Bezeichner (GUID) im MIM-Dienst für das Konto, mit dem die Anforderung erstellt wurde.
Justification | Der Grund für die Rechteerweiterung.
CreationTime | Die Erstellungszeit der Anforderung.
CreationMethode | Die Methode, die zum Erstellen der Anforderung verwendet wurde.
ExpirationTime | Die Ablaufzeit der Anforderung.
RoleID| Der eindeutige Bezeichner (GUID) der PAM-Rolle.
AnforderungedTTL | Der angeforderte Ablauftimeout in Sekunden.
RequestedTime | Der angeforderte Zeitpunkt für die Rechteerweiterung.
RequestStatus | Der Status der Anforderung. Die folgenden Werte sind möglich: „Verarbeitung“, „Aktiv“, „Geschlossen“, „Wird geschlossen“, „Abgelaufen“, „Genehmigung ausstehend“, „MFA ausstehend“ und „Abgelehnt“.

##<a name="example"></a>Beispiel

###<a name="request-1"></a>Anforderung 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Antwort 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Anforderung 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Antwort 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
