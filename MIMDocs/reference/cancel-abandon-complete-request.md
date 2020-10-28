---
title: Abbrechen, verwerfen oder vervollständigen einer Anforderung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2016
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e971309963968ddd52ef44644fb9319738df6242
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760491"
---
# <a name="cancel-abandon-or-complete-a-request"></a>Abbrechen, verwerfen oder vervollständigen einer Anforderung
Markieren Sie eine Microsoft Identity Manager (MIM) Certificate Management (cm)-Anforderung als abgeschlossen, abgebrochen oder abgebrochen.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>URL-Parameter

Eigenschaft| BESCHREIBUNG
---------|--------
id| Erforderlich. Die GUID der abzuschließenden Anforderung.


### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Der Anforderungs Text enthält die folgenden Eigenschaften:

Eigenschaft | BESCHREIBUNG
---------|-----------
status | Der Status, den die Anforderung erhalten soll. Folgende Werte sind möglich: "abgeschlossen", "abgebrochen" oder "abgebrochen".


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
Bei Erfolg wird ein [Microsoft. CLM. shared. Requests. Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) -Objekt zurückgegeben. Das-Objekt beschreibt die MIM cm Anforderung, die als abgeschlossen markiert wurde. Das-Objekt enthält die folgenden Eigenschaften:

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
Dieser Abschnitt enthält ein Beispiel zum Markieren einer Anforderung als abgeschlossen.

### <a name="example-request"></a>Beispiel: Anforderung

```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    "status":"Completed"
}
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
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

## <a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Provision.Executeoperations. Complete-Methode](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft. CLM. shared. Requests. Request-Klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
