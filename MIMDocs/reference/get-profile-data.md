---
title: Abrufen von Profildaten | Microsoft-Dokumentation
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Abrufen von Profildaten
Ruft eine Liste der Softwarezertifikatprofile für einen Benutzer mit einer Liste möglicher Vorgänge ab, die vom aktuellen Benutzer ausgeführt werden können. Für jeden der angegebenen Vorgänge kann dann eine Anforderung initiiert werden.

**Hinweis**: Der Server legt die PIN nur fest, wenn die Richtlinie der Profilvorlage angibt, dass dies erforderlich ist. Andernfalls muss sie vom Benutzer bereitgestellt werden.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{ID} <br/>/CertificateManagement/api/v1.0/requests/{Anforderungs-ID}/profiles

###<a name="url-parameters"></a>URL-Parameter
Parameter | Beschreibung
---------|------------
id | Der Bezeichner (GUID) des zurückzugebenden Profils.
requestId | Der Bezeichner der Anforderung, für die die Profile zurückgegeben werden sollen.

###<a name="query-parameters"></a>Abfrageparameter
Parameter | Beschreibung
---------|------------
Status | (Optional) Gibt den Status der Profile an, für die Daten abgerufen werden sollen. Mögliche Statustypen: „Aktiv“, „Genehmigt“, „Abgebrochen“, „Abgeschlossen“, „Verweigert“, „Ausführen“, „Fehler“, „Kein“ und „Ausstehend“. <br/>Falls kein Status angegeben wird, werden alle Profile unabhängig vom Status zurückgegeben.
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
Bei Erfolg wird eine Liste von JSON-serialisierten Objekten vom Typ [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) mit den folgenden Eigenschaften zurückgegeben:

Eigenschaft | Beschreibung
---------|------------
AssignedUserUuid | Der Bezeichner des Benutzers, dem das Profil zugewiesen ist.
Anmerkungen | Der Kommentar, der das Profil beschreibt.
Flags | Die Flags, die das Profil beschreiben.
ParentProfileUuid | Der Bezeichner des alten Profils, das durch das Profil ersetzt wurde.
PrimaryProfileUuid | Der Bezeichner des primären Profils.
ProfileOperations | Die Liste der möglichen Vorgänge, die vom aktuellen Benutzer mit dem Profil ausgeführt werden können.
ProfileTemplateUuid | Der Bezeichner der Profilvorlage, die die Richtlinien und Einstellungen enthält, die das Profil bestimmen.
ProfileTemplateVersion | Die Version der Profilvorlage zum Zeitpunkt der Erstellung des Profils.
Status | Der Status des Profils.
Uuid | Der Bezeichner des Profils.


##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Antwort
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
##<a name="see-also"></a>Siehe auch

- [Microsoft.Clm.Shared.Profiles.Profile Class](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
