---
title: Smartcard-Authentifizierungs Antwort erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ab320457c5d676cc381306e83f685fe288dc7ef9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760407"
---
# <a name="get-smart-card-authentication-response"></a>Smartcardauthentifizierungsantwort erhalten
Ruft die Antwort auf eine Kryptografiedienstanbieter-Authentifizierungs Aufforderung (CSP) ab.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
---------|------------
reqid | Erforderlich. Der Anforderungs Bezeichner, der spezifisch für Microsoft Identity Manager (MIM) Certificate Management (cm) ist.
scid | Erforderlich. Der Bezeichner der Smartcard, der für MIM cm spezifisch ist. Die scid wird aus dem [Microsoft. CLM. shared. Smartcards. Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) -Objekt abgerufen.

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
---------|------------
atr | Optional. Die ATR-Zeichenfolge (Antwort zum Zurücksetzen) der Smartcard.
cardid | Erforderlich. Die Smartcard-ID.
challenge | Erforderlich. Eine Base-64-codierte Zeichenfolge, die die von der Smartcard ausgegebene Herausforderung darstellt.
diversified | Erforderlich. Ein boolesches Flag, das anzeigt, ob der Smartcard-Administrator Schlüssel diversifiziert wurde.

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
Bei Erfolg wird ein Byte-BLOB zurückgegeben, das die Abfrageantwort darstellt.

## <a name="example"></a>Beispiel
In diesem Abschnitt finden Sie ein Beispiel für die Antwort auf eine Basis-CSP-Authentifizierungs Aufforderung.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       

## <a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Provision.Executeoperations. getbasecspresponse-Methode](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
