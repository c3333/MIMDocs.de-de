---
title: Smartcard-oder Profil Zertifikate erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e089caa78dd9a52517f2cae3fa42176225c73b5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760395"
---
# <a name="get-smart-card-or-profile-certificates"></a>Smartcard-oder Profil Zertifikate erhalten
Ruft eine Liste von Zertifikaten ab, die einer bestimmten Smartcard oder einem bestimmten Software Profil zugeordnet sind.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificateManagement/api/v1.0/smartcards/{id}/certificates

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
Bei Erfolg wird eine Liste von JSON-serialisierten Objekten vom Typ [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) mit den folgenden Eigenschaften zurückgegeben:

Name | BESCHREIBUNG
-----|------------
ArchivedOnCa | Ein boolescher Wert, der angibt, ob das Zertifikat durch die Zertifizierungsstelle archiviert wird.
CertificateType | Der Typ des Zertifikats.
IsKeyHistory | Ein boolescher Wert, der angibt, ob es sich bei dem Zertifikat um ein wichtiges Verlaufszertifikat handelt.
SerialNumber | Die Seriennummer des Zertifikats.
TemplateCommonName | Der allgemeine Name für die Zertifikatsvorlage des Zertifikats.
Fingerabdruck | Der Fingerabdruck des Zertifikats.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum erhalten der Zertifikate, die einer Smartcard oder einem Software Profil zugeordnet sind.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       

## <a name="see-also"></a>Weitere Informationen

- [Microsoft. CLM. Provision. findoperations. findzertifikats-Methode](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft. CLM. shared. Zertifikats. X509ClmCertificate-Klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
