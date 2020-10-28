---
title: Generieren von Optionen für die Generierung von Zertifikat Anforderungen | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8baab788c07dfb8c009857b45e38eb662f98199
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760482"
---
# <a name="get-certificate-request-generation-options"></a>Generieren von Optionen zur Generierung von Zertifikat Anforderungen
Ruft die Parameter für die Generierung der clientseitigen Zertifikatanforderungen ab.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

| Methode |                                       Anforderungs-URL                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions |

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
--------|--------------
requestid| Erforderlich. Der GUID-Bezeichner der MIM CM-Anforderung, für die die Generierungsparameter der Zertifikatanforderung abgerufen werden sollen.

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
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Bei Erfolg wird eine Liste von certificaterequestgenerationoptions-Objekten zurückgegeben. Jedes certificaterequestgenerationoptions-Objekt entspricht einer einzelnen Zertifikat Anforderung, die vom Client generiert werden muss. Jedes-Objekt verfügt über die folgenden Eigenschaften:

Eigenschaft| BESCHREIBUNG
--------|-----------
Exportable | Ein Wert, der angibt, ob der für die Anforderung erstellte private Schlüssel exportiert werden kann.
FriendlyName | Der Anzeigename des eingeschriebenen Zertifikats.
HashAlgorithmName | Der Hashalgorithmus, der beim Erstellen der Signatur der Zertifikatanforderung verwendet wird.
KeyAlgorithmName | Der Algorithmus für öffentliche Schlüssel.
KeyProtectionLevel | Die Stufe des starken Schlüsselschutzes.
KeySize | Die Größe des privaten Schlüssels in Bits, der generiert werden soll.
KeyStorageProviderNames | Eine Liste der zulässigen Schlüsselspeicheranbieter, mit denen der private Schlüssel generiert werden kann. Wenn der erste KSP nicht zum Generieren der Zertifikat Anforderung verwendet werden kann, kann jeder der angegebenen kSPS verwendet werden, bis der Vorgang erfolgreich ist.
KeyUsages | Der Vorgang, der vom privaten Schlüssel ausgeführt werden kann, der für diese Zertifikatanforderung erstellt wurde. Der Standardwert ist „Signatur“.
Subject | Der Name des Subjekts.

>[!NOTE]
>Weitere Informationen zu diesen Eigenschaften finden Sie in der Beschreibung der [Windows. Security. Cryptography. Certification. certificaterequestproperties-Klasse](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) . Beachten Sie, dass es zwischen dieser Klasse und den certificaterequestgenerationoptions-Objekten keine 1:1-Entsprechung gibt.
>

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, mit dem Sie die Generierungs Optionen für eine Zertifikat Anforderung erhalten.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```  
