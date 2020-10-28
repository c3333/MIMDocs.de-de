---
title: Anforderung erstellen | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 51016951f9dec22e2a40b44a2b76a840192b518f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760671"
---
# <a name="create-request"></a>Anforderung erstellen
Erstellen Sie eine Anforderung für die Microsoft Identity Manager (MIM) Certificate Management (cm).

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

### <a name="url-parameters"></a>URL-Parameter
Keine.

### <a name="request-headers"></a>Anforderungsheader
Informationen zu allgemeinen Anforderungs Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="request-body"></a>Anforderungstext
Der Anforderungs Text enthält die folgenden Eigenschaften:

Eigenschaft | BESCHREIBUNG
---------|-----------
profiletemplateuuid | Erforderlich. Die GUID der Profilvorlage, für die der Benutzer die Anforderung erstellt.
datacollection | Erforderlich. Eine Sammlung von Name-Wert-Paaren, die die vom Registrierenden bereitzustellenden Daten darstellen. Die Sammlung der erforderlichen Daten, die bereitgestellt werden müssen, können über die Workflowrichtlinie der Profilvorlage abgerufen werden. Eine leere Sammlung kann angegeben werden.
target | Optional. Die GUID des Zielbenutzers, für den die Anforderung erstellt werden soll. Wenn nichts angegeben ist, wird das Ziel standardmäßig auf den aktuellen Benutzer festgelegt.
type | Erforderlich. Der Typ der zu erstellenden Anforderung. Zu den verfügbaren Anforderungs Typen zählen "Enroll", "Duplicate", "offlineunblock", "ONLINEUPDATE", "Renew", "Recover", "Recover in Name", "Recover", "Recover", "Widerruf", "temporarycards" und "Unblock".<br/><br/>**Hinweis** : nicht alle Anforderungs Typen werden für alle Profil Vorlagen unterstützt. Beispielsweise können Sie den Vorgang zum Entsperren einer Software Profil Vorlage nicht angeben.
comment | Erforderlich. Beliebige Kommentare, die vom Benutzer eingegeben werden können. Die Workflow Richtlinie definiert, ob die Comment-Eigenschaft erforderlich ist. Eine leere Zeichenfolge kann angegeben werden.
priority | Optional. Die Priorität der Anforderung. Wenn keine Angabe erfolgt, wird die standardmäßige Anforderungs Priorität, die durch die Profil Vorlagen Einstellungen festgelegt wird, verwendet.


## <a name="response"></a>Antwort
In diesem Abschnitt wird die-Antwort beschrieben.

### <a name="response-codes"></a>Antwort Codes

Code  |BESCHREIBUNG  
---------|---------
201 | Erstellt
403 | Verboten
500 | Interner Fehler

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Antwort Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Bei Erfolg wird der URI für die neu erstellte Anforderung zurückgegeben.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel zum Erstellen von Anforderungen zum Registrieren und Aufheben der Blockierung.

### <a name="example-request-1"></a>Beispiel: Anforderung 1

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### <a name="example-response-1"></a>Beispiel: Antwort 1

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### <a name="example-request-2"></a>Beispiel: Anforderung 2

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### <a name="example-response-2"></a>Beispiel: Antwort 2

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### <a name="example-request-3"></a>Beispiel: Anforderung 3

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

## <a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Provision.RequestOperations.Initiateenroll-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Initiateofflineunblock-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Initiaterecover-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Initiateretire-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Initiateunblock-Methode](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
