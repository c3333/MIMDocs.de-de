---
title: Profil Vorlagen erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9f14853b3895dece1098270d35439d6c4999be2b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760422"
---
# <a name="get-profile-templates"></a>Profil Vorlagen erhalten
Ruft eine Liste der Profil Vorlagen ab, für die sich der angegebene Benutzer registrieren kann. Diese Methode gibt eine eingeschränkte Ansicht der Profilvorlage zurück. Die zurückgegebenen Profilvorlagendaten sollten ausreichend sein, damit der anfordernde Benutzer entscheiden kann, für welche Profilvorlage er sich ggf. registriert. Wenn kein Workflow und keine Berechtigung angegeben werden, werden alle für den Benutzer sichtbaren Profil Vorlagen zurückgegeben.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[Zielbenutzer\] 

### <a name="url-parameters"></a>URL-Parameter

Parameter| BESCHREIBUNG
--------|-------------
targetuser| Optional. Gibt den Zielbenutzer an, für den Profilvorlagen zurückgegeben werden sollen. Wenn kein Wert angegeben ist, wird die Identität des aktuellen Benutzers verwendet. <br/><br/>**Hinweis** : Derzeit wird nur der aktuelle Benutzer unterstützt.

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
500 | Interner Fehler

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Antwort Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Bei Erfolg wird eine Liste von profiletemplatelimitedview-Objekten mit den folgenden Eigenschaften zurückgegeben:

Eigenschaft| type| BESCHREIBUNG
--------|-----|--------
Name| Zeichenfolge| Der Anzeigename der Profilvorlage.
BESCHREIBUNG| Zeichenfolge| Die Beschreibung für die Profilvorlage.
Uuid| GUID| Der Bezeichner für die Profilvorlage.
IsSmartcardProfileTemplate| bool| Gibt an, ob es sich bei der Vorlage um eine Smartcard-Profilvorlage handelt.
IsVirtualSmartcardProfileTemplate| bool| Gibt an, ob für die Profilvorlage eine virtuelle Smartcard erforderlich ist.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, mit dem Sie die Liste der Profil Vorlagen für den angegebenen Benutzer erhalten.

### <a name="example-request"></a>Beispiel: Anforderung

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### <a name="example-response"></a>Beispiel: Antwort

```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]
```       
