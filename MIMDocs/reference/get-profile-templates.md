---
title: Abrufen von Profilvorlagen | Microsoft-Dokumentation
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Abrufen von Profilvorlagen
Ruft eine Liste der Profilvorlagen ab, für die sich der angegebene Benutzer registrieren kann. Diese Methode gibt eine eingeschränkte Ansicht der Profilvorlage zurück. Die zurückgegebenen Profilvorlagendaten sollten ausreichend sein, damit der anfordernde Benutzer entscheiden kann, für welche Profilvorlage er sich ggf. registriert. Wenn keine Workflows und Berechtigungen angegeben sind, werden alle für den Benutzer sichtbaren Profilvorlagen zurückgegeben.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[Zielbenutzer\] 

###<a name="url-parameters"></a>URL-Parameter
Parameter| Beschreibung
--------|-------------
targetuser| (Optional) Gibt den Zielbenutzer an, für den Profilvorlagen zurückgegeben werden sollen. Wenn dieser nicht angegeben ist, wird die Identität des aktuellen Benutzers verwendet. <br/>**Hinweis**: Derzeit wird nur der aktuelle Benutzer unterstützt.

###<a name="request-headers"></a>Anforderungsheader
Allgemeine Anforderungsheader finden Sie im Thema „Dienst“.
###<a name="request-body"></a>Anforderungstext
Keiner

##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
204 | Kein Inhalt
500 | Interner Fehler

###<a name="response-headers"></a>Antwortheader
Allgemeine Antwortheader finden Sie im Thema „Dienst“.
###<a name="response-body"></a>Antworttext
Bei Erfolg wird eine Liste von „ProfileTemplateLimitedView“-Objekten mit den folgenden Eigenschaften zurückgegeben.

Eigenschaft| Typ| Beschreibung
--------|-----|--------
Name| string| Der Anzeigename der Profilvorlage.
Beschreibung| string| Die Beschreibung für die Profilvorlage.
Uuid| Guid| Der Bezeichner für die Profilvorlage.
IsSmartcardProfileTemplate| bool| Gibt an, ob es sich bei der Vorlage um eine Smartcard-Profilvorlage handelt.
IsVirtualSmartcardProfileTemplate| bool| Gibt an, ob für die Profilvorlage eine virtuelle Smartcard erforderlich ist.

##<a name="example"></a>Beispiel

###<a name="request"></a>Anforderung
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Antwort
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
