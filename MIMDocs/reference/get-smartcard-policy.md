---
title: Smartcardrichtlinie erhalten | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e8e8b2ac7dcf8f72d4989d458272d78b8367d923
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760614"
---
# <a name="get-smart-card-policy"></a>Smartcardrichtlinie erhalten
Ruft die Profilvorlagenrichtlinie für den angegebenen Workflow ab. Diese Daten werden während der Erstellung der Anforderung verwendet. Die Workflowrichtlinie gibt an, welche Daten vom Client benötigt werden, um eine Anforderung zu erstellen. Die Daten können verschiedene Daten Sammlungs Elemente, Anforderungs Kommentare und eine einmal Kennwort-Richtlinie enthalten.

>[!NOTE]
>Die URLs in diesem Artikel beziehen sich auf den Hostnamen, der während der API-Bereitstellung ausgewählt wird, z `https://api.contoso.com` . b..

## <a name="request"></a>Anforderung

Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### <a name="url-parameters"></a>URL-Parameter

Parameter| BESCHREIBUNG
--------|-------------
id| Erforderlich. Die GUID, die der Profilvorlage entspricht, aus der die Richtlinie extrahiert werden soll.
type| Erforderlich. Der Typ der angeforderten Richtlinie. Folgende Werte sind möglich: "Enroll", "Duplicate", "offlineunblock", "ONLINEUPDATE", "Renew", "Recover", "Recover in Name", "Recover", "Recover", "Widerruf", "temporaryenroll" und "Unblock".

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
403 | Verboten
204 | Kein Inhalt
500 | Interner Fehler

### <a name="response-headers"></a>Antwortheader
Informationen zu allgemeinen Antwort Headern finden Sie unter [HTTP-Anforderungs-und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *cm Rest-API-Dienst Details* .

### <a name="response-body"></a>Antworttext
Gibt bei Erfolg ein Richtlinienobjekt auf Basis eines [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)-Objekts zurück. Das Richtlinien Objekt enthält mindestens die Eigenschaften in der folgenden Tabelle, kann jedoch abhängig von der angeforderten Richtlinie zusätzliche Eigenschaften enthalten. Eine Anforderung für eine Registrierungs Richtlinie gibt z. b. ein Registrierungs [Richtlinien](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) Objekt zurück. Weitere Informationen finden Sie in der Dokumentation zum Richtlinienobjekt, das dem Parameter „{type}“ in der Anforderung zugeordnet ist. Die Dokumentation für die verschiedenen Typen von Richtlinien Objekten finden Sie in der Dokumentation zum [Microsoft. CLM. shared. profile Templates-Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) .

Eigenschaft | BESCHREIBUNG
---------|------------
ApprovalsNeeded | Die Anzahl der Genehmigungen, die für Anforderungen von Forefront Identity Manager (FIM)-Zertifikat Verwaltung (FIM) für die Richtlinie erforderlich sind.
AuthorizedApprover | Die Sicherheitsbeschreibung für Benutzer, die berechtigt sind, FIM CM-Anforderungen für die Richtlinie zu genehmigen.
AuthorizedEnrollmentAgent | Die Sicherheitsbeschreibung für Benutzer, die für die Richtlinie als Enrollment Agent fungieren können.
AuthorizedInitiator | Die Sicherheitsbeschreibung für Benutzer, die FIM CM-Anforderungen für die Richtlinie initiieren können.
CollectComments | Ein boolescher Wert, der angibt, ob die Kommentarsammlung für FIM CM-Anforderungen für die Richtlinie aktiviert ist.
CollectRequestPriority | Ein boolescher Wert, der angibt, ob die Anforderungsprioritätssammlung für FIM CM-Anforderungen für die Richtlinie aktiviert ist.
DefaultRequestPriority | Die Standardpriorität für FIM CM-Anforderungen für die Richtlinie.
Dokumente | Die Richtliniendokumente, die für die Richtlinie konfiguriert sind.
Aktiviert | Ein boolescher Wert, der angibt, ob die Richtlinie aktiviert ist.
EnrollAgentRequired | Ein boolescher Wert, der angibt, ob für FIM CM-Anforderungen für die Richtlinie Enrollment Agents erforderlich sind.
OneTimePasswordPolicy | Die Verteilungsmethode für einmal Kennwörter für FIM cm-Anforderungen für die Richtlinie.
Personalisierung | Die Smartcard-Personalisierungsoptionen für die Richtlinie.
PolicyDataCollection | Die Datensammlungselemente, die der Richtlinie zugeordnet sind.
SelfServiceEnabled | Ein boolescher Wert, der angibt, ob die Self-Service-Initiierung von FIM CM-Anforderungen für die Richtlinie aktiviert ist.

## <a name="example"></a>Beispiel
Dieser Abschnitt enthält ein Beispiel, mit dem Sie die Profil Vorlagen Richtlinie für einen Workflow erhalten. 

### <a name="example-request-1"></a>Beispiel: Anforderung 1

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### <a name="example-response-1"></a>Beispiel: Antwort 1

```
HTTP/1.1 200 OK

... body coming soon
```       

### <a name="example-request-2"></a>Beispiel: Anforderung 2

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### <a name="example-response-2"></a>Beispiel: Antwort 2

```
HTTP/1.1 200 OK

... body coming soon
```       

## <a name="see-also"></a>Weitere Informationen

- [Microsoft. CLM. shared. profile Templates. profiletemplatepolicy-Klasse](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft. CLM. shared. profile Templates-Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
