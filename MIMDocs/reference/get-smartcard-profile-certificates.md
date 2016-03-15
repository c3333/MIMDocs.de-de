---
Titel: Abrufen von Smartcard- oder Profilzertifikaten
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: 206bde70-4af8-46fa-9c0b-f574745b0977
---
# Abrufen von Smartcard- oder Profilzertifikaten
Ruft eine Liste von Zertifikaten ab, die dem bestimmten Smartcard- oder Softwareprofil zugeordnet sind.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles/{ID}/Certificates <br/>/CertificateManagement/API/v1.0/Smartcards/{ID}/Certificates

###URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner (GUID) des Profils oder der Smartcard.

###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Keiner

##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
204 | Kein Inhalt
403 | Verboten
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird eine Liste von JSON-serialisierten Objekten [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.certificates.x509clmcertificate(v=vs.100%29.aspx) mit den folgenden Eigenschaften zurückgegeben:

Name | Beschreibung
-----|------------
ArchivedOnCa | Ein boolescher Wert, der angibt, ob das Zertifikat durch die Zertifizierungsstelle archiviert wird.
CertificateType | Der Typ des Zertifikats.
IsKeyHistory | Ein boolescher Wert, der angibt, ob es sich bei dem Zertifikat um ein wichtiges Verlaufszertifikat handelt.
SerialNumber | Die Seriennummer des Zertifikats.
TemplateCommonName | Der allgemeine Name für die Zertifikatsvorlage des Zertifikats.
Fingerabdruck | Der Fingerabdruck des Zertifikats.

##Beispiel

###Anforderung
```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```
###Antwort
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
##Weitere Informationen

- [Microsoft.Clm.Provision.FindOperations.FindCertificates-Methode](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.findoperations.findcertificates(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.Certificates.X509ClmCertificate-Klasse](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.certificates.x509clmcertificate(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
