---
title: "Abrufen von Generierungsoptionen für Zertifikatsanforderungen | Microsoft-Dokumentation"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Abrufen von Generierungsoptionen für Zertifikatanforderungen

Ruft die Parameter für die Generierung der clientseitigen Zertifikatanforderungen ab.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
--------|--------------
requestid| Erforderlich. Der GUID-Bezeichner der MIM CM-Anforderung, für die die Generierungsparameter der Zertifikatanforderung abgerufen werden sollen.

###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Kein


##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
200 | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###<a name="response-headers"></a>Antwortheader
Informationen zu weit verbreiteten Antwortheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="response-body"></a>Antworttext
Bei Erfolg wird eine Liste der „CertificateRequestGenerationOptions“-Objekte zurückgegeben. Jedes „CertificateRequestGenerationOptions“-Objekt entspricht einer einzelnen Zertifikatanforderung, die vom Client generiert werden muss und die folgenden Eigenschaften aufweisen soll:

Eigenschaft| Beschreibung
--------|-----------
Exportable | Ein Wert, der angibt, ob der für die Anforderung erstellte private Schlüssel exportiert werden kann.
FriendlyName | Der Anzeigename des eingeschriebenen Zertifikats.
HashAlgorithmName | Der Hashalgorithmus, der beim Erstellen der Signatur der Zertifikatanforderung verwendet wird.
KeyAlgorithmName | Der Algorithmus für öffentliche Schlüssel.
KeyProtectionLevel | Die Stufe des starken Schlüsselschutzes.
KeySize | Die Größe des privaten Schlüssels in Bits, der generiert werden soll.
KeyStorageProviderNames | Eine Liste der zulässigen Schlüsselspeicheranbieter, mit denen der private Schlüssel generiert werden kann. Falls der erste Schlüsselspeicheranbieter nicht zum Generieren der Zertifikatanforderung verwendet werden kann, wird so lange einer der anderen angegebenen Schlüsselspeicheranbieter verwendet, bis der Vorgang erfolgreich ausgeführt wurde.
KeyUsages | Der Vorgang, der vom privaten Schlüssel ausgeführt werden kann, der für diese Zertifikatanforderung erstellt wurde. Der Standardwert ist „Signatur“.
Antragsteller | Der Name des Antragstellers.

**Hinweis**: Weitere Informationen zu diesen Eigenschaften finden Sie in der Beschreibung der [Windows.Security.Cryptography.Certificates.CertificateRequestProperties-Klasse](https://msdn.microsoft.com/library/windows/apps/br212079.aspx), aber beachten Sie, dass zwischen dieser Klasse und den CertificateRequestGenerationOptions-Objekten keine eindeutige Beziehung besteht.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Antwort
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
