---
title: Get-Anforderung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ad660c562b457890ea75d33325ada8d0f63feb8f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760638"
---
# <a name="get-request"></a>Get-Anforderung
Ruft eine oder mehrere angegebene Microsoft Identity Manager (MIM) Certificate Management (cm)-Anforderungen ab.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>URL-Parameter

Eigenschaft| BESCHREIBUNG
---------|--------
id| Optional. Die GUID einer abzurufenden Anforderung.

### <a name="query-parameters"></a>Abfrageparameter

Eigenschaft| BESCHREIBUNG
---------|--------
targetuser| Optional. Der Zielbenutzer der Anforderung. Wenn kein Ziel angegeben ist, werden alle Anforderungen unabhängig vom Ziel Benutzer zurückgegeben. <br/><br/>**Hinweis** : Derzeit wird nur der Wert "Current" unterstützt.
status| Optional. Gibt den Status der abzurufenden Anforderung an. Die möglichen Status Typen lauten "genehmigt", "abgebrochen", "abgeschlossen", "verweigert", "wird ausgeführt", "Fehler", "kein" und "Ausstehend". <br/>Wenn kein Status angegeben wird, werden alle Anforderungen unabhängig vom Status zurückgegeben.

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
Gibt bei Erfolg ein oder mehrere [Request](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)-Objekte mit den folgenden Eigenschaften zurück:

Name | Beschreibung
-----|------------
Kommentar | Der Kommentar, der der MIM CM-Anforderung zugeordnet ist.
Abgeschlossen | Der Zeitpunkt, zu dem die MIM CM-Anforderung abgeschlossen wurde.
DataCollection | Die Datensammlungselemente, die der MIM CM-Anforderung zugeordnet sind.
DataCollectionFlags | Die Optionen für die Datensammlung für die MIM CM-Anforderung.
Flags | Die Optionen, die der MIM CM-Anforderung zugeordnet sind.
IsDataCollectionComplete | Ein boolescher Wert, der angibt, ob die Datensammlung für die MIM CM-Anforderung abgeschlossen wurde.
IsEnrollmentAgent | Ein boolescher Wert, der angibt, ob ein Registrierungs-Agent erforderlich ist, um die MIM CM-Anforderung auszuführen.
IsSmartcard | Ein boolescher Wert, der angibt, ob die MIM CM-Anforderung eine Smartcard- oder Softwareprofilanforderung ist.
NewProfileUuid | Die Identität des neuen Softwareprofils, die durch die MIM CM-Anforderung erstellt wird.
NewSmartcardUuid | Die Identität der neuen Smartcard, die durch die MIM CM-Anforderung zugewiesen wird.
OldProfileUuid | Die Identität des Softwareprofils, für das die MIM CM-Anforderung erstellt wurde.
OldSmartcardUuid | Die Identität der Smartcard, für die die MIM CM-Anforderung erstellt wurde.
OriginatorUserUuid | Die Identität des Benutzers, der die MIM cm Anforderung ausgelöst hat.
Priorität | Die Priorität für die MIM CM-Anforderung.
ProfileTemplateUuid | Die Identität der Profilvorlage, für die die MIM CM-Anforderung erstellt wurde.
RequestType | Der Typ der MIM CM-Anforderung.
SecurityDescriptor | Die Sicherheitsbeschreibung für die MIM CM-Anforderung.
Status | Der Status der MIM CM-Anforderung.
Gesendet | Der Zeitpunkt, zu dem die MIM CM-Anforderung übermittelt wurde.
TargetUserUuid | Die Identität des Zielbenutzers für die MIM CM-Anforderung.
Uuid | Der Bezeichner für die MIM CM-Anforderung.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, um die angegebenen MIM cm Anforderungen zu erhalten.

### <a name="example-request-1"></a>Beispiel: Anforderung 1

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1
```

### <a name="example-response-1"></a>Beispiel: Antwort 1

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```

### <a name="example-request-2"></a>Beispiel: Anforderung 2

```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```

### <a name="example-response-2"></a>Beispiel: Antwort 2

```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```     

## <a name="see-also"></a>Weitere Informationen

- [Microsoft. CLM. Provision. findoperations. findrequest-Methode](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft. CLM. shared. requestberechtigungs-Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft. CLM. shared. Requests. Request-Klasse](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
