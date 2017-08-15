---
title: Erstellen von Anforderungen | Microsoft-Dokumentation
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Erstellen von Anforderungen
Erstellen Sie eine MIM CM-Anforderung.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###<a name="url-parameters"></a>URL-Parameter
Keiner

###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Der Anforderungstext enthält die folgenden Eigenschaften.

Eigenschaft | Beschreibung
---------|-----------
profiletemplateuuid | Erforderlich. Die GUID der Profilvorlage, für die der Benutzer die Anforderung erstellt.
datacollection | Erforderlich. Eine Sammlung von Name-Wert-Paaren, die die vom Registrierenden bereitzustellenden Daten darstellen. Die Sammlung der erforderlichen Daten, die bereitgestellt werden müssen, können über die Workflowrichtlinie der Profilvorlage abgerufen werden. Eine leere Sammlung kann angegeben werden.
target | Optional. Die GUID des Zielbenutzers, für den die Anforderung erstellt werden soll. Wenn diese nicht angegeben ist, wird standardmäßig der aktuelle Benutzer verwendet.
type | Erforderlich. Der Typ der zu erstellenden Anforderung. Verfügbare Anforderungstypen sind:„Enroll“, „Duplicate“, „OfflineUnblock“, „OnlineUpdate“, „Renew“, „Recover“, „RecoverOnBehalf“, „Reinstate“, „Retire“, „Revoke“, „TemporaryCards“ und „Unblock“.<br/>**Hinweis**: Nicht alle Typen von Anforderungen werden in allen Profilvorlagen unterstützt. Sie können z. B. keinen „unblock“-Vorgang für eine Softwareprofilvorlage angeben.
Kommentar | Erforderlich. Beliebige Kommentare, die vom Benutzer eingegeben werden können. Die Workflowrichtlinie definiert, ob dies erforderlich ist. Eine leere Zeichenfolge kann angegeben werden.
priority | (Optional) Die Priorität der Anforderung. Wenn diese nicht angegeben ist, wird die Standardanforderungspriorität gemäß den Einstellungen der Profilvorlage verwendet.


##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
201     | Erstellt
403 | Verboten
500 | Interner Fehler

###<a name="response-headers"></a>Antwortheader
Informationen zu weit verbreiteten Antwortheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="response-body"></a>Antworttext
Bei Erfolg wird der URI für die neu erstellte Anforderung zurückgegeben.
##<a name="example"></a>Beispiel

###<a name="request-1"></a>Anforderung 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Antwort 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Anforderung 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Antwort 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Anforderung 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>Siehe auch

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
