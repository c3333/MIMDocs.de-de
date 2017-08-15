---
title: Genehmigen oder Ablehnen einer ausstehenden PAM-Anforderung | Microsoft-Dokumentation
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
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e5506f96a22d1918cff7d0c9b822babb0f6cfa9
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Genehmigen oder Ablehnen einer ausstehenden PAM-Anforderung
Dies wird von einem privilegierten Konto verwendet, um die Anforderung zur Rechteerweiterung für einer PAM-Rolle zu genehmigen, zu schließen oder abzulehnen.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({Genehmigungs-ID)/Approve <br/>/api/pamresources/pamrequeststoapprove({Genehmigungs-ID)/Reject

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
----------|-----------
approvalId | Der Bezeichner (GUID) des Genehmigungsobjekts in PAM, der wie folgt angegeben wird: `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
----------|--------------
v | (Optional) Die API-Version. Wenn diese nicht angegeben ist, wird die aktuelle (zuletzt veröffentlichte) Version der API verwendet. Weitere Informationen finden Sie unter [Versionsverwaltung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#versioning)


###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Kein

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
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

```       
