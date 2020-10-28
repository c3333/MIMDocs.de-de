---
title: Erhalten von PAM-Rollen | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f802dc22019f1ad97fc5cce8c9ed2e000349dc58
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760893"
---
# <a name="get-pam-roles"></a>Erhalten von PAM-Rollen
Dies wird von einem privilegierten Konto verwendet, um die PAM-Rollen aufzulisten, für die das Konto geeignet ist.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/api/pamresources/pamroles

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
----------|--------------
$filter | Optional. Geben Sie eine der PAM-Rollen Eigenschaften in einem Filter Ausdruck an, um eine gefilterte Liste von Antworten zurückzugeben. Weitere Informationen zu unterstützten Operatoren finden Sie unter [Filterung in PAM Rest-API-Dienst Details](privileged-access-management-rest-api-service-details.md#filtering).
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
Eine erfolgreiche Antwort enthält eine Sammlung von mindestens einer PAM-Rolle, die jeweils die folgenden Eigenschaften aufweist:

Eigenschaft | BESCHREIBUNG
--------|-------------
RoleID | Der eindeutige Bezeichner (GUID) der PAM-Rolle.
DisplayName | Der Anzeigename der PAM-Rolle im MIM-Dienst.
BESCHREIBUNG | Die Beschreibung der PAM-Rolle im MIM-Dienst.
TTL | Der maximale Ablauftimeout für die Zugriffsrechte der Rolle in Sekunden.
AvailableFrom | Die früheste Tageszeit, zu der eine Anforderung aktiviert wird.
AvailableTo | Die letzte Tageszeit, zu der eine Anforderung aktiviert wird.
MFAEnabled | Ein boolescher Wert, der angibt, ob Aktivierungsanforderungen für diese Rolle eine MFA-Abfrage erfordern.
ApprovalEnabled | Ein boolescher Wert, der angibt, ob Aktivierungsanforderungen für diese Rolle eine Genehmigung eines Rollenbesitzers erfordern.
Availabilitywindowenabled | Ein boolescher Wert, der angibt, ob die Rolle nur während eines bestimmten Zeitintervalls aktiviert werden kann.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum erhalten der PAM-Rollen.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /api/pamresources/pamroles HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
