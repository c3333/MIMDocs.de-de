---
title: Genehmigen oder ablehnen einer ausstehenden PAM-Anforderung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 743c28b343c374d3406600751b379e8a4695c2d0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760776"
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Genehmigen oder ablehnen einer ausstehenden PAM-Anforderung
Dies wird von einem privilegierten Konto verwendet, um die Anforderung zur Rechteerweiterung für einer PAM-Rolle zu genehmigen, zu schließen oder abzulehnen.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({Genehmigungs-ID)/Approve <br/>/api/pamresources/pamrequeststoapprove({Genehmigungs-ID)/Reject

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
----------|-----------
approvalId | Der Bezeichner (GUID) des Genehmigungs Objekts in PAM, angegeben als `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

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
Keine.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum Genehmigen einer Anforderung zur Erhöhung der Rechte auf eine PAM-Rolle.

### <a name="example-request"></a>Beispiel: Anforderung

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK
```       
