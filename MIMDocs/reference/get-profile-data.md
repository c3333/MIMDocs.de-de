---
title: Profildaten erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62b4c336122376cd7da836b9ec6c6e09eac24729
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760446"
---
# <a name="get-profile-data"></a>Profildaten erhalten
Ruft eine Liste der Software Zertifikat Profile für einen Benutzer ab. Die Liste enthält die möglichen Vorgänge, die vom aktuellen Benutzer ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

>[!IMPORTANT]
>Der Server legt die PIN nur fest, wenn die Richtlinie für die Profil Vorlage angibt, dass Sie ausgeführt werden soll. Andernfalls sollte der Benutzer die PIN bereitstellen.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{ID} <br/>/CertificateManagement/api/v1.0/requests/{Anforderungs-ID}/profiles

### <a name="url-parameters"></a>URL-Parameter

Parameter | BESCHREIBUNG
---------|------------
id | Der Bezeichner (GUID) des zurückzugebenden Profils.
requestId | Der Bezeichner der Anforderung, für die die Profile zurückgegeben werden sollen.

### <a name="query-parameters"></a>Abfrageparameter

Parameter | BESCHREIBUNG
---------|------------
status | Optional. Gibt den Status der Profile an, für die Daten abgerufen werden sollen. Mögliche Status Typen sind "aktiv", "genehmigt", "abgebrochen", "abgeschlossen", "verweigert", "wird ausgeführt", "Fehler", "kein" und "Ausstehend". <br/>Wenn kein Status angegeben wird, werden alle Profile unabhängig vom Status zurückgegeben.

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
Bei Erfolg wird eine Liste von JSON-serialisierten Objekten vom Typ [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) mit den folgenden Eigenschaften zurückgegeben:

Eigenschaft | BESCHREIBUNG
---------|------------
AssignedUserUuid | Der Bezeichner des Benutzers, dem das Profil zugewiesen ist.
Kommentar | Der Kommentar, der das Profil beschreibt.
Flags | Die Flags, die das Profil beschreiben.
ParentProfileUuid | Der Bezeichner des alten Profils, das durch das Profil ersetzt wurde.
PrimaryProfileUuid | Der Bezeichner des primären Profils.
ProfileOperations | Die Liste der möglichen Vorgänge, die vom aktuellen Benutzer mit dem Profil ausgeführt werden können.
ProfileTemplateUuid | Der Bezeichner der Profilvorlage, die die Richtlinien und Einstellungen enthält, die das Profil bestimmen.
ProfileTemplateVersion | Die Version der Profilvorlage zum Zeitpunkt der Erstellung des Profils.
Status | Der Status des Profils.
Uuid | Der Bezeichner des Profils.


## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, mit dem die Profildaten für einen Benutzer angezeigt werden.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```

## <a name="see-also"></a>Weitere Informationen

- [Microsoft. CLM. shared. Profiles. profile-Klasse](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
