---
title: Informationen zu PAM-Sitzungen erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2529ac9665344403f798cd2bf708dffb8697876a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760740"
---
# <a name="get-pam-session-info"></a>Informationen zu PAM-Sitzungen erhalten
Dies wird von einem privilegierten Konto verwendet, um den Benutzernamen des Kontos abzurufen, das für die Sitzung angemeldet ist.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/api/session/sessioninfo

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
----------|--------------
v | Optional. Die API-Version. Wenn diese nicht enthalten ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM Rest-API-Dienst Details](privileged-access-management-rest-api-service-details.md#versioning).

### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Keine.

## <a name="response"></a>Antwort
In diesem Abschnitt wird die-Antwort beschrieben.

### <a name="response-codes"></a>Antwort Codes

Code  |BESCHREIBUNG  
---------|---------
200 | OK
401 | Nicht autorisiert
403 | Verboten
408 | Anforderungstimeout   
500 | Interner Serverfehler
503 | Dienst nicht verfügbar

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Eine erfolgreiche Antwort gibt ein PAM-Sitzungs Objekt mit den folgenden Eigenschaften zurück:

Eigenschaft | BESCHREIBUNG
--------|-------------
Username | Der Benutzername des Kontos, das in dieser Sitzung angemeldet ist.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, in dem die PAM-Sitzungsinformationen angezeigt werden.

### <a name="example-request"></a>Beispiel: Anforderung 

```
GET /api/session/sessioninfo/ HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
