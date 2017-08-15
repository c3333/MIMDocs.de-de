---
title: Abrufen von Smartcard-Richtlinien | Microsoft-Dokumentation
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>Abrufen von Smartcard-Richtlinien

Ruft die Profilvorlagenrichtlinie für den angegebenen Workflow ab. Diese Daten werden während der Erstellung der Anforderung verwendet. Die Workflowrichtlinie gibt an, welche Daten vom Client benötigt werden, um eine Anforderung zu erstellen. Diese Daten können Folgendes enthalten: verschiedene Datensammlungselemente, Anforderungskommentare und eine Richtlinie für Einmalkennwörter.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##<a name="request"></a>Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{Typ}

###<a name="url-parameters"></a>URL-Parameter
Parameter| Beschreibung
--------|-------------
id| Erforderlich. Die GUID, die der Profilvorlage entspricht, aus der die Richtlinie extrahiert werden soll.
Typ| Erforderlich. Der Typ der angeforderten Richtlinie. Die folgenden Werte sind möglich: *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*, *Unblock*.

###<a name="request-headers"></a>Anforderungsheader
Informationen zu weit verbreiteten Anforderungsheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="request-body"></a>Anforderungstext
Keiner

##<a name="response"></a>Antwort
###<a name="response-codes"></a>Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
403 | Verboten
204 | Kein Inhalt
500 | Interner Fehler

###<a name="response-headers"></a>Antwortheader
Informationen zu weit verbreiteten Antwortheadern finden Sie unter [HTTP-Anforderungs- und Antwortheader](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST-API-Dienstdetails*.
###<a name="response-body"></a>Antworttext
Gibt bei Erfolg ein Richtlinienobjekt auf Basis eines [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)-Objekts zurück. Das Richtlinienobjekt enthält mindestens die Eigenschaften in der folgenden Tabelle. Es kann aber in Abhängigkeit von der angeforderten Richtlinie zusätzliche Eigenschaften enthalten. Eine Anforderung für eine Registrierungsrichtlinie gibt ein [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy)-Objekt zurück. Weitere Informationen finden Sie in der Dokumentation zum Richtlinienobjekt, das dem Parameter „{type}“ in der Anforderung zugeordnet ist. Die Dokumentation für die verschiedenen Typen von Richtlinienobjekten finden Sie in der [Microsoft.Clm.Shared.ProfileTemplates-Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates)-Dokumentation.

Eigenschaft | Beschreibung
---------|------------
ApprovalsNeeded | Die Anzahl der Genehmigungen, die für FIM CM-Anforderungen für die Richtlinie erforderlich sind.
AuthorizedApprover | Die Sicherheitsbeschreibung für Benutzer, die berechtigt sind, FIM CM-Anforderungen für die Richtlinie zu genehmigen.
AuthorizedEnrollmentAgent | Die Sicherheitsbeschreibung für Benutzer, die für die Richtlinie als Enrollment Agent fungieren können.
AuthorizedInitiator | Die Sicherheitsbeschreibung für Benutzer, die FIM CM-Anforderungen für die Richtlinie initiieren können.
CollectComments | Ein boolescher Wert, der angibt, ob die Kommentarsammlung für FIM CM-Anforderungen für die Richtlinie aktiviert ist.
CollectAnforderungPriority | Ein boolescher Wert, der angibt, ob die Anforderungsprioritätssammlung für FIM CM-Anforderungen für die Richtlinie aktiviert ist.
DefaultAnforderungPriority | Die Standardpriorität für FIM CM-Anforderungen für die Richtlinie.
Dokumente | Die Richtliniendokumente, die für die Richtlinie konfiguriert sind.
Aktiviert | Ein boolescher Wert, der angibt, ob die Richtlinie aktiviert ist.
EnrollAgentRequired | Ein boolescher Wert, der angibt, ob für FIM CM-Anforderungen für die Richtlinie Enrollment Agents erforderlich sind.
OneTimePasswordPolicy | Ermittelt, wie Einmalkennwörter für FIM CM-Anforderungen für die Richtlinie verteilt werden.
Personalization | Die Smartcard-Personalisierungsoptionen für die Richtlinie.
PolicyDataCollection | Die Datensammlungselemente, die der Richtlinie zugeordnet sind.
SelfServiceAktiviert | Ein boolescher Wert, der angibt, ob die Self-Service-Initiierung von FIM CM-Anforderungen für die Richtlinie aktiviert ist.

##<a name="example"></a>Beispiel

###<a name="request-1"></a>Anforderung 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Antwort 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Anforderung 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Antwort 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Weitere Informationen

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy-Klasse](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates-Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
