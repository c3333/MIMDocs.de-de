---
title: "Schließen von PAM-Anforderungen | Microsoft-Dokumentation"
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>Schließen von PAM-Anforderungen
Dies wird von einem privilegierten Konto verwendet, um eine Anforderung zu schließen, die zur Rechteerweiterung für eine PAM-Rolle initiiert wurde.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
----------|-----------
requestId | Der Bezeichner (GUID) der zu schließenden PAM-Rolle, der wie folgt angegeben wird: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
----------|--------------
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
Keiner
##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

```       
