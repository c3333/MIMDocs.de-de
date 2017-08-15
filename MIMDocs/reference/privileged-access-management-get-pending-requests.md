---
title: Abrufen ausstehender PAM-Anforderungen | Microsoft-Dokumentation
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Abrufen ausstehender PAM-Anforderungen
Dies wird von einem privilegierten Konto verwendet, um eine Liste der ausstehenden Anforderungen zurückzugeben, für die eine Genehmigung erforderlich ist.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `http://api.contoso.com`.
##<a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
----------|--------------
$filter | (Optional) Geben Sie eine der Eigenschaften für ausstehende PAM-Anforderungen in einem Filterausdruck an, um eine gefilterte Liste von Antworten zurückzugeben. Weitere Informationen zu unterstützten Operatoren finden Sie unter [Filterung in PAM REST-API-Dienstdetails](privileged-access-management-rest-api-service-details.md#filtering)
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
Eine erfolgreiche Antwort enthält eine Liste der Genehmigungsobjekte der PAM-Anforderung mit den folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-------------
RoleName | Der Anzeigename der Rolle, für die die Genehmigung erforderlich ist.
Anforderungor | Der Benutzername des zu genehmigenden Requestor.
Justification | Die vom Benutzer bereitgestellte Ausrichtung.
RequestedTTL | Die angeforderte Ablaufzeit in Sekunden.
AnforderungedTime | Die angeforderte Zeit für die Rechteerweiterung.
CreationTime | Die Erstellungszeit der Anforderung.
FIMAnforderungID | Enthält ein einzelnes Element, „Wert“, mit dem eindeutigen Bezeichner (GUID) der PAM-Anforderung.
AnforderungorID | Enthält ein einzelnes Element, „Wert“, mit dem eindeutigen Bezeichner (GUID) für das Active Directory-Konto, das die PAM-Anforderung erstellt hat.
ApprovalObjectID | Enthält ein einzelnes Element, „Wert“, mit dem eindeutigen Bezeichner (GUID) für das Genehmigungsobjekt.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Antwort
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
