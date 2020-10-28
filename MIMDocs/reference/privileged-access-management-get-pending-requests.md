---
title: Ausstehende PAM-Anforderungen erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0ec4388ce5d5f748d9f779819d0a2d0bdec06791
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760773"
---
# <a name="get-pending-pam-requests"></a>Ausstehende PAM-Anforderungen erhalten
Dies wird von einem privilegierten Konto verwendet, um eine Liste der ausstehenden Anforderungen zurückzugeben, für die eine Genehmigung erforderlich ist.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
----------|--------------
$filter | Optional. Geben Sie eine der ausstehenden PAM-Anforderungs Eigenschaften in einem Filter Ausdruck an, um eine gefilterte Liste von Antworten zurückzugeben. Weitere Informationen zu unterstützten Operatoren finden Sie unter [Filterung in PAM Rest-API-Dienst Details](privileged-access-management-rest-api-service-details.md#filtering).
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
Eine erfolgreiche Antwort enthält eine Liste der Genehmigungs Objekte der PAM-Anforderung mit den folgenden Eigenschaften:

Eigenschaft | BESCHREIBUNG
---------|-------------
RoleName | Der Anzeigename der Rolle, für die die Genehmigung erforderlich ist.
Anforderer | Der Benutzername des zu genehmigenden Requestor.
Begründung | Die vom Benutzer bereitgestellte Ausrichtung.
RequestedTTL | Die angeforderte Ablaufzeit in Sekunden.
RequestedTime | Der angeforderte Zeitpunkt für die Rechteerweiterung.
CreationTime | Die Erstellungszeit der Anforderung.
FIMRequestID | Enthält ein einzelnes Element, "Wert", mit dem eindeutigen Bezeichner (GUID) der PAM-Anforderung.
RequestorID | Enthält ein einzelnes Element, "Wert", mit dem eindeutigen Bezeichner (GUID) für das Active Directory Konto, das die PAM-Anforderung erstellt hat.
ApprovalObjectID | Enthält ein einzelnes Element, "Wert", mit dem eindeutigen Bezeichner (GUID) für das Genehmigungs Objekt.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, mit dem die ausstehenden PAM-Anforderungen angezeigt werden.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
