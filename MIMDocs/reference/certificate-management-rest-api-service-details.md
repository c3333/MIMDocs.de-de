---
title: CM REST-API-Dienstdetails | Microsoft-Dokumentation
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>CM REST-API-Dienstdetails
In den folgenden Abschnitten werden die Details der CM REST-API (Zertifikatsverwaltung) von Microsoft Identity Manager (MIM) besprochen.

## <a name="architecture"></a>Architektur 
Die MIM CM REST-API-Aufrufe werden vom Controller behandelt. In der folgenden Tabelle wird die vollständige Liste der Controller und Beispiele für den Kontext angezeigt, in dem sie verwendet werden können.

Controller| Beispielroute
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /api/v1.0/smartcards/{Smartcard-ID}/certificates <br/> /api/v1.0/profiles/{Profil-ID}/certificates
OperationsController| /api/v1.0/smartcards/{Smartcard-ID}/operations <br/> /api/v1.0/profiles/{Profil-ID}/operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| /api/v1.0/profiles/{ID} <br/> /api/v1.0/profiles <br/> /api/v1.0/requests/{Anforderungs-ID}/profiles/{ID}
ProfileTemplatesController| /api/v1.0/profiletemplates/{ID} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| /api/v1.0/requests/{ID} <br/> /api/v1.0/requests
SmartcardsController| /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID}/diversifiedkey <br/> /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID}/serverproposedpin <br/> /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID}/authenticationresponse <br/> /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID} <br/> /api/v1.0/smartcards/{ID} <br/> /api/v1.0/smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>

## <a name="http-request-and-response-headers"></a>HTTP-Anforderungs- und -Antwortheader

Die an die API gesendeten HTTP-Anforderungen sollten die folgenden Header enthalten (diese Liste ist nicht vollständig):

Header | Beschreibung
-------|------------
Autorisierung | Erforderlich. Der Inhalt hängt von der Authentifizierungsmethode ab, die konfiguriert werden kann und auf WIA (integrierte Windows-Authentifizierung) oder ADFS basiert.
Content-Type | Erforderlich, wenn die Anforderung Text enthält. Dieser muss „application/json“ sein.
Content-Length | Erforderlich, wenn die Anforderung Text enthält. 
Cookie | Das Sitzungscookie. Möglicherweise je nach Authentifizierungsmethode erforderlich.
<br/>
Die HTTP-Antworten enthalten die folgenden Header (diese Liste ist nicht vollständig):

Header | Beschreibung
-------|------------
Content-Type | Die API gibt immer „application/json“ zurück.
Content-Length | Die Länge des Anforderungstexts, falls vorhanden, in Bytes.


## <a name="api-versioning"></a>API-Versionsverwaltung 
Die aktuelle Version der CM REST-API ist 1.0. Die Version wird in dem Segment angegeben, das im URI direkt auf das Segment `/api` folgt: `/api/v1.0`. Die Versionsnummer kann sich in der Zukunft ändern, sollten wichtige Änderungen an der API-Schnittstelle auftreten.


## <a name="enabling-the-api"></a>Aktivieren der API 
Der Abschnitt `<ClmConfiguration>` der Datei „Web.config“ wurde durch einen neuen Schlüssel erweitert:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Dieser Schlüssel bestimmt, ob die CM REST-API für Clients zur Verfügung gestellt wird. Wenn der Schlüssel auf „False“ festgelegt ist, wird die Routenzuordnung für die API nicht beim Start der Anwendung ausgeführt. Dies bedeutet, dass alle nachfolgenden Anforderungen an API-Endpunkte den HTTP-Fehlercode 404 zurückgeben. Der Schlüssel ist standardmäßig auf „Deaktiviert“ festgelegt.
Denken Sie nach dem Ändern dieses Werts zu „True“ daran, dass Sie **iisreset** auf dem Server ausführen.

## <a name="enabling-tracing-and-logging"></a>Aktivieren der Ablaufverfolgung und Protokollierung 
Die MIM CM REST-API gibt für jede an sie gesendete HTTP-Anforderung Ablaufverfolgungsdaten aus. Sie können den Ausführlichkeitsgrad für die ausgegebenen Ablaufverfolgungsinformationen einstellen, indem Sie den folgenden Konfigurationswert festlegen:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Fehler- und Problembehandlung 
Wenn beim Verarbeiten einer Anforderung Ausnahmen auftreten, gibt die MIM CM REST-API einen HTTP-Statuscode an den Webclient zurück. Für allgemeine Fehler gibt die API einen entsprechenden HTTP-Statuscode und einen Fehlercode zurück. 

Nicht behandelte Ausnahmen werden in ein `HttpResponseException` mit HTTP-Statuscode 500 („Interner Fehler“) konvertiert und sowohl im Ereignisprotokoll als auch in der MIM CM-Ablaufverfolgungsdatei verfolgt. Jede nicht behandelte Ausnahme wird mit einer entsprechenden Korrelations-ID im Ereignisprotokoll eingetragen. Die Korrelations-ID wird auch an den Consumer der API in der Fehlermeldung gesendet. Ein Administrator kann das Ereignisprotokoll nach der entsprechenden Korrelations-ID und den Fehlerdetails durchsuchen, um den Fehler zu beheben.

Aufgrund von Sicherheitsbedenken werden Stapelüberwachungen zu Fehlern, die sich durch die Nutzung der MIM CM REST-API ergeben, nicht zurück an den Client gesendet.
