---
title: Abrufen von PAM-Anforderungen | Microsoft-Dokumentation
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>Abrufen von PAM-Anforderungen
Dies wird von einem privilegierten Konto verwendet, um einen Verlauf der zuvor veröffentlichten PAM-Anforderungen zurückzugeben.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
----------|--------------
$filter | (Optional) Geben Sie eine der Eigenschaften für PAM-Anforderungen in einem Filterausdruck an, um eine gefilterte Liste von Antworten zurückzugeben. Weitere Informationen zu unterstützten Operatoren finden Sie unter [Filterung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#filtering)
v | (Optional) Die API-Version. Wenn diese nicht angegeben ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Keiner

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
Eine erfolgreiche Antwort enthält eine Liste der Objekte der PAM-Anforderung mit den folgenden Eigenschaften.

Eigenschaft | Beschreibung
--------|-------------
Anforderungs-ID | Der eindeutige Bezeichner (GUID) für die PAM-Anforderung.
CreatorID | Ein eindeutiger Bezeichner (GUID) für das Active Directory-Konto, das die PAM-Anforderung erstellt hat.
Justification | Der Grund für die Rechteerweiterung.
DisplayName | Der Anzeigename der PAM-Anforderung in MIM.
CreationTime | Die Erstellungszeit der Anforderung.
CreationMethode | Die Methode, die zum Erstellen der Anforderung verwendet wurde.
ExpirationTime | Die Ablaufzeit der Anforderung.
RoleID| Der eindeutige Bezeichner (GUID) der PAM-Rolle.
AnforderungedTTL | Der angeforderte Ablauftimeout in Sekunden.
RequestedTime | Der angeforderte Zeitpunkt für die Rechteerweiterung.
AnforderungedStatus | Der Status der Anforderung. Die folgenden Werte sind möglich: „Aktiv“, „Geschlossen“, „Wird geschlossen“, „Genehmigung ausstehend“ und „Abgelehnt“

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
