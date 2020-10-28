---
title: Profil Zustands Vorgänge erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 35246570b52285f916b74785c17ce55f2967fb7d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760650"
---
# <a name="get-profile-state-operations"></a>Profil Zustands Vorgänge erhalten
Ruft eine Liste der möglichen Vorgänge ab, die vom aktuellen Benutzer für das angegebene Profil ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{ID}/operations

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
---------|------------
id | Der Bezeichner (GUID) des Profils oder der Smartcard.

### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Keine.

## <a name="response"></a>Antwort
In diesem Abschnitt wird die-Antwort beschrieben.

### <a name="response-codes"></a>Antwort Codes

Code  |BESCHREIBUNG  
---------|---------
200 | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Antwort Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Bei Erfolg wird eine Liste der möglichen Vorgänge zurückgegeben, die vom Benutzer auf der Smartcard ausgeführt werden können. Die Liste kann eine beliebige Anzahl von folgenden Vorgängen enthalten: **ONLINEUPDATE** , **erneuern** , **Wiederherstellen, Wiederherstellen** **,** außer Kraft **setzen,** **widerrufen und aufheben** der **Blockierung** .

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum Erstellen von Profil Zustands Vorgängen für den aktuellen Benutzer.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
