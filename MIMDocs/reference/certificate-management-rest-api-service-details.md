---
title: Details zum Rest-API-Dienst für die Zertifikat Verwaltung | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a2dd84e121217772a8831653b2e4790436c32ec
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2020
ms.locfileid: "92761001"
---
# <a name="certificate-management-rest-api-service-details"></a>Rest-API-Dienst Details für die Zertifikat Verwaltung
In den folgenden Abschnitten werden die Details der Microsoft Identity Manager (MIM)-Rest-API für die Zertifikat Verwaltung (MIM) beschrieben.

## <a name="architecture"></a>Aufbau 
Die MIM CM REST-API-Aufrufe werden vom Controller behandelt. In der folgenden Tabelle wird die vollständige Liste der Controller und Beispiele für den Kontext angezeigt, in dem sie verwendet werden können.


|                  Controller                   |                                                                                                                                                           Beispielroute                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificateDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificateRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            CertificatesController             |                                                                                                                /api/v1.0/smartcards/{Smartcard-ID}/certificates <br/> /api/v1.0/profiles/{Profil-ID}/certificates                                                                                                                 |
|             OperationsController              |                                                                                                                  /api/v1.0/smartcards/{Smartcard-ID}/operations <br/> /api/v1.0/profiles/{Profil-ID}/operations                                                                                                                   |
|              PoliciesController               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              ProfilesController               |                                                                                                                                                     /api/v1.0/profiles/{ID}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{ID} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequestsController               |                                                                                                                                         /api/v1.0/requests/{ID} <br/> /api/v1.0/requests                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID}/diversifiedkey <br/> /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID}/serverproposedpin <br/> /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID}/authenticationresponse <br/> /api/v1.0/requests/{Anforderungs-ID}/smartcards/{ID} <br/> /api/v1.0/smartcards/{ID} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## <a name="http-request-and-response-headers"></a>HTTP-Anforderungs-und-Antwortheader
An die API gesendete HTTP-Anforderungen sollten die folgenden Header enthalten (diese Liste ist nicht vollständig):

Header | BESCHREIBUNG
-------|------------
Authorization | Erforderlich. Der Inhalt hängt von der Authentifizierungsmethode ab. Die-Methode ist konfigurierbar und kann auf der integrierten Windows-Authentifizierung (WIA) oder Active Directory-Verbunddienste (AD FS) (ADFS) basieren.
Content-Type | Erforderlich, wenn die Anforderung Text enthält. Muss `application/json`lauten.
Content-Length | Erforderlich, wenn die Anforderung Text enthält. 
Cookie | Das Sitzungscookie. Möglicherweise je nach Authentifizierungsmethode erforderlich.


HTTP-Antworten enthalten die folgenden Header (diese Liste ist nicht vollständig):

Header | BESCHREIBUNG
-------|------------
Content-Type | Die API gibt immer zurück `application/json` .
Content-Length | Die Länge des Anforderungstexts, falls vorhanden, in Bytes.


## <a name="api-versioning"></a>API-Versionsverwaltung 
Die aktuelle Version der CM REST-API ist 1.0. Die Version wird in dem Segment angegeben, das im URI direkt auf das Segment `/api` folgt: `/api/v1.0`. Die Versionsnummer ändert sich, wenn wichtige Änderungen an der API-Schnittstelle vorgenommen werden.


## <a name="enable-the-api"></a>Aktivieren der API 
Der Abschnitt `<ClmConfiguration>` der Datei „Web.config“ wurde durch einen neuen Schlüssel erweitert:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

Dieser Schlüssel bestimmt, ob die CM REST-API für Clients zur Verfügung gestellt wird. Wenn der Schlüssel auf „False“ festgelegt ist, wird die Routenzuordnung für die API nicht beim Start der Anwendung ausgeführt. Bei nachfolgenden Anforderungen an API-Endpunkte wird ein HTTP 404-Fehlercode zurückgegeben. Der Schlüssel ist standardmäßig auf „Deaktiviert“ festgelegt.

>[!NOTE]
>Wenn Sie diesen Wert in "true" ändern, denken Sie daran, **iisreset** auf dem Server auszuführen.

## <a name="enable-tracing-and-logging"></a>Aktivieren der Ablauf Verfolgung und Protokollierung 
Die MIM CM REST-API gibt für jede an sie gesendete HTTP-Anforderung Ablaufverfolgungsdaten aus. Sie können den Ausführlichkeitsgrad für die ausgegebenen Ablaufverfolgungsinformationen einstellen, indem Sie den folgenden Konfigurationswert festlegen:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## <a name="error-handling-and-troubleshooting"></a>Fehlerbehandlung und Problembehandlung 
Wenn beim Verarbeiten einer Anforderung Ausnahmen auftreten, gibt die MIM CM REST-API einen HTTP-Statuscode an den Webclient zurück. Für allgemeine Fehler gibt die API einen entsprechenden HTTP-Statuscode und einen Fehlercode zurück. 

Nicht behandelte Ausnahmen werden in ein `HttpResponseException` mit HTTP-Statuscode 500 („Interner Fehler“) konvertiert und sowohl im Ereignisprotokoll als auch in der MIM CM-Ablaufverfolgungsdatei verfolgt. Jede nicht behandelte Ausnahme wird mit einer entsprechenden Korrelations-ID im Ereignisprotokoll eingetragen. Die Korrelations-ID wird auch an den Consumer der API in der Fehlermeldung gesendet. Ein Administrator kann das Ereignisprotokoll nach der entsprechenden Korrelations-ID und den Fehlerdetails durchsuchen, um den Fehler zu beheben.

>[!NOTE]
>Stapel Überwachungen, die Fehlern entsprechen, die als Ergebnis der Verwendung der Rest-API MIM cm generiert werden, werden nicht an den Client zurückgesendet. Die Ablauf Verfolgungen werden aufgrund von Sicherheitsbedenken nicht zurück an den Client gesendet.
