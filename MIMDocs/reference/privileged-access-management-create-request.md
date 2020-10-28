---
title: Erstellen einer PAM-Anforderung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58155232b3f99f32df9cb6b4c2e344d0cbad278
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760764"
---
# <a name="create-pam-request"></a>Erstellen einer PAM-Anforderung
Dies wird von einem privilegierten Konto für die Rechteerweiterung auf eine PAM-Rolle verwendet.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
--------|-------------
Begründung | Optional. Der vom Benutzer angegebene Grund für die Anforderung zur Rechteerweiterung.
RoleId | Erforderlich. Der eindeutige Bezeichner (GUID) der PAM-Rolle für die Rechteerweiterung.
RequestedTTL | Erforderlich. Die angeforderte Ablaufzeit in Sekunden.
RequestedTime | Optional. Die für die Rechteerweiterung erforderliche Zeit.  
v | Optional. Die API-Version. Wenn diese nicht enthalten ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM Rest-API-Dienst Details](privileged-access-management-rest-api-service-details.md#versioning).

>[!NOTE]
>Sie können die Parameter **Justification** , **RoleId** , **RequestedTTL** und **RequestedTime** im Anforderungstext als Eigenschaften anstatt als Abfrageparameter angeben. Der **v** -Parameter kann nur als Abfrage Parameter angegeben werden.

### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Optional. Die Parameter " **Begründung** ", " **RoleID** ", " **requestedttl** " und " **requestedtime** " können als Eigenschaften eines Anforderungs Texts angegeben werden, anstatt Sie in der URL-Abfrage Zeichenfolge anzugeben.

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
Eine erfolgreiche Antwort enthält ein PAM-Anforderungs Objekt mit den folgenden Eigenschaften:

Eigenschaft | BESCHREIBUNG
--------|-------------
RequestID | Der eindeutige Bezeichner (GUID) für die PAM-Anforderung.
CreatorID | Der eindeutige Bezeichner (GUID) im MIM-Dienst für das Konto, mit dem die Anforderung erstellt wurde.
Begründung | Der Grund für die Rechteerweiterung.
CreationTime | Die Erstellungszeit der Anforderung.
CreationMethod | Die Methode, die zum Erstellen der Anforderung verwendet wurde.
ExpirationTime | Die Ablaufzeit der Anforderung.
RoleID| Der eindeutige Bezeichner (GUID) der PAM-Rolle.
RequestedTTL | Der angeforderte Ablauftimeout in Sekunden.
RequestedTime | Der angeforderte Zeitpunkt für die Rechteerweiterung.
RequestStatus | Der Status der Anforderung. Mögliche Werte sind "Verarbeitung", "aktiv", "geschlossen", "Schließen", "abgelaufen", "Zahlungsgenehmigung", "pdingmfa" und "abgelehnt".

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält Beispiele zum Erstellen einer PAM-Anforderung.

### <a name="example-request-1"></a>Beispiel: Anforderung 1

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### <a name="example-response-1"></a>Beispiel: Antwort 1

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

### <a name="example-request-2"></a>Beispiel: Anforderung 2

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### <a name="example-response-2"></a>Beispiel: Antwort 2

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
