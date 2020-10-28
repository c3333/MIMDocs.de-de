---
title: Von der Smartcard vorgeschlagene PIN erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9d4f1444efa490f980e29440342844ac246d8a20
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760602"
---
# <a name="get-smart-card-proposed-pin"></a>Von der Smartcard vorgeschlagene PIN erhalten
Ruft die vom Server generierte PIN eines Benutzers ab.

>[!IMPORTANT]
>Der Server legt die PIN nur fest, wenn die Richtlinie für die Profil Vorlage angibt, dass Sie ausgeführt werden soll. Andernfalls sollte der Benutzer die PIN bereitstellen.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
---------|------------
id | Die Smartcard-ID, die spezifisch für Microsoft Identity Manager (MIM) Certificate Management (cm) ist. Die ID wird aus dem Objekt "mikrosft. CLM. shared. Smartcard" abgerufen.

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
---------|------------
atr | Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.
cardid | Die Smartcard-ID.
challenge | Eine Base-64-codierte Zeichenfolge, die die von der Smartcard ausgegebene Herausforderung darstellt.

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
Bei Erfolg wird eine Zeichenfolge zurückgegeben, die die vom Server vorgeschlagene Pin darstellt.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum erhalten der vom Server generierten Benutzer-PIN.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

... body coming soon
```       
