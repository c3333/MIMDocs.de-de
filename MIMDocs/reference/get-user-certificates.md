---
title: Abrufen von Benutzerzertifikaten | Microsoft-Dokumentation
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
ms.assetid: 1b51b742-4a36-44e4-ae58-a93737d0dc27
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58976f90e33b8346b03b309824504ad75c0fb22
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-user-certificates"></a>Abrufen von Benutzerzertifikaten
Ruft die Liste der Zertifikate ab, die dem angegebenen Benutzer zugeordnet sind (zurückgezogene Zertifikate werden gefiltert).

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/certificates

###<a name="url-parameters"></a>URL-Parameter
Keiner

###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Keiner

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
Bei Erfolg wird eine Liste von JSON-serialisierten Objekten vom Typ [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate(v=vs.100).aspx) mit den folgenden Eigenschaften zurückgegeben:

Name | Beschreibung
-----|------------
ArchivedOnCa | Ein boolescher Wert, der angibt, ob das Zertifikat durch die Zertifizierungsstelle archiviert wird.
CertificateType | Der Typ des Zertifikats.
IsKeyHistory | Ein boolescher Wert, der angibt, ob es sich bei dem Zertifikat um ein wichtiges Verlaufszertifikat handelt.
Aussteller | Der Aussteller.
NotAfter | Datum und Uhrzeit, nach dem das Zertifikat nicht mehr gültig ist.
NotBefore | Datum und Uhrzeit des Zeitpunkts, ab dem das Zertifikat gültig wird.
AnforderungerName | Das Konto, über das das Zertifikat angefordert wurde.
SerialNumber | Die Seriennummer des Zertifikats.
Status | Der Status des Zertifikats.
TemplateCommonName | Der allgemeine Name für die Zertifikatsvorlage des Zertifikats.
Fingerabdruck | Der Fingerabdruck des Zertifikats.


##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/certificates HTTP/1.1
```
###<a name="response"></a>Antwort
```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000013C3A98764EC3233BE400000000013C",
        "Thumbprint":"09B41AC106F0C40168940DF4AB74D6DB39DE55EE",
        "NotAfter":"2016-07-13T09:10:31",
        "NotBefore":"2015-07-14T09:10:31",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000013B957A5513AF6CE2B200000000013B",
        "Thumbprint":"7F0248836E7B93FE2B0357F19269C7134B449376",
        "NotAfter":"2016-07-13T08:59:51",
        "NotBefore":"2015-07-14T08:59:51",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000013A68D58660B37FA7BB00000000013A",
        "Thumbprint":"67DDEE9CF2C417832305714FF4F64175CC2F8963",
        "NotAfter":"2016-07-13T08:56:31",
        "NotBefore":"2015-07-14T08:56:31",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":true,
        "CertificateType":1,
        "TemplateCommonName":"ArchivedCertificateTemplate",
        "SerialNumber":"27000001392838861E9DA725C9000000000139",
        "Thumbprint":"352650D413262E53389908E73BDF5DBAAEEA0D81",
        "NotAfter":"2016-07-13T08:08:50",
        "NotBefore":"2015-07-14T08:08:50",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"2700000137D1C6649BE84FAC2B000000000137",
        "Thumbprint":"9170C7E38126C6B87C5B4F8A50FC3CE261B59BBF",
        "NotAfter":"2016-07-13T07:14:43",
        "NotBefore":"2015-07-14T07:14:43",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":true,
        "CertificateType":1,
        "TemplateCommonName":"ArchivedCertificateTemplate",
        "SerialNumber":"270000012D12ED47DEF778AE5200000000012D",
        "Thumbprint":"E5CDBFA02072E0A19EFACAA5C009E2DBD4DA8CEC",
        "NotAfter":"2016-07-12T11:48:02",
        "NotBefore":"2015-07-13T11:48:02",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"CopyofUser",
        "SerialNumber":"270000012B1EFE271640C75D5800000000012B",
        "Thumbprint":"88C815E8A5FB40EE71907327061491B7F402EBA3",
        "NotAfter":"2016-07-12T11:47:09",
        "NotBefore":"2015-07-13T11:47:09",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000011D0E1C76C5EA0FA94500000000011D",
        "Thumbprint":"CCFC28DB6E5A48149ACD0E2D787F5E239E686809",
        "NotAfter":"2016-05-03T08:04:31",
        "NotBefore":"2015-05-04T08:04:31",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000011A5CC01932CACC661F00000000011A",
        "Thumbprint":"1190AFFBDA94333C5659BAAD63B566C10EC24620",
        "NotAfter":"2016-05-03T06:31:04",
        "NotBefore":"2015-05-04T06:31:04",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"2700000118E402E672C46BBD5C000000000118",
        "Thumbprint":"3D04D0F4FF932A7888A331A762BA46E51DAAE415",
        "NotAfter":"2016-04-27T16:35:03",
        "NotBefore":"2015-04-28T16:35:03",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000011685A0CE1F819B1917000000000116",
        "Thumbprint":"FF5FE72A35E9C17AB060805CC9BF84C0799BFBCA",
        "NotAfter":"2016-04-27T11:53:53",
        "NotBefore":"2015-04-28T11:53:53",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"2700000112A2339B46DC1E8B0C000000000112",
        "Thumbprint":"A3A4E733FE7CB19FC213C15378115101A752855F",
        "NotAfter":"2016-04-27T07:35:47",
        "NotBefore":"2015-04-28T07:35:47",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000011149EFD78B17F120CF000000000111",
        "Thumbprint":"4EFB6E9AB584186054005F0514B6D692F263629C",
        "NotAfter":"2016-04-27T07:34:26",
        "NotBefore":"2015-04-28T07:34:26",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000011070FD397CBEC70535000000000110",
        "Thumbprint":"5A584340294C88157F901366FF7E887FD7B05F1E",
        "NotAfter":"2016-04-27T07:32:48",
        "NotBefore":"2015-04-28T07:32:48",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"270000010F87F9FEE060CC024A00000000010F",
        "Thumbprint":"7E7E74AACF0581DD9C374E85C7A74BEFCF6608EF",
        "NotAfter":"2016-04-26T14:50:15",
        "NotBefore":"2015-04-27T14:50:15",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"KSPusercertificate",
        "SerialNumber":"27000000D9AABE8619E368DC760000000000D9",
        "Thumbprint":"4809C9291679527E1A943387144E1E7CC2863AA3",
        "NotAfter":"2016-01-20T09:21:13",
        "NotBefore":"2015-01-20T09:21:13",
        "Issuer":"WIN-TRUR24L4CFS.corp.cmteam.com\\corp-WIN-TRUR24L4CFS-CA",
        "RequesterName":"CORP\\Administrator",
        "Status":0
    }
]
```       
