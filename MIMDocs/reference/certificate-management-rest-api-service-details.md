---
Titel: CM REST-API-Dienstdetails
MS.Custom: 
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology: 
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: 530047f1-e43b-4a69-9542-75bc1da57bf7
---
# CM REST-API-Dienstdetails
In den folgenden Abschnitten werden die Details der CM REST-API (Zertifikatsverwaltung) von Microsoft Identity Manager (MIM) besprochen.

## Inhalt dieses Themas

- [Architektur](#Architecture)
- [HTTP-Anforderungs- und -Antwortheader](#HttpHeaders)
- [API-Versionsverwaltung](#Versioning)
- [Aktivieren der API](#APIConfig)
- [Aktivieren der Ablaufverfolgung und Protokollierung](#TracingConfig)
- [Fehler- und Problembehandlung](#ErrorHandling)

<a name="Architecture"></a>
## Architektur 
Die MIM CM REST-API-Aufrufe werden vom Controller behandelt. In der folgenden Tabelle wird die vollständige Liste der Controller und Beispiele für den Kontext angezeigt, in dem sie verwendet werden können.

Controller| Beispielroute
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /API/v1.0/Smartcards/{smartcardid}/Certificates <br/> /API/v1.0/Profiles/{profileid}/Certificates
OperationsController| /API/v1.0/Smartcards/{smartcardid}/Operations <br/> /API/v1.0/Profiles/{profileid}/Operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /API/v1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| / api/v1.0/profiletemplates/{id} <br/> /API/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /API/v1.0/Requests
SmartcardsController| /API/v1.0/Requests/{RequestId}/Smartcards/{ID}/diversifiedkey <br/> /API/v1.0/Requests/{RequestId}/Smartcards/{ID}/serverproposedpin <br/> /API/v1.0/Requests/{RequestId}/Smartcards/{ID}/authenticationresponse <br/> / api/v1.0/requests/{requestid}/smartcards/{id} <br/> / api/v1.0/smartcards/{id} <br/> /API/v1.0/Smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>
<a name="HttpHeaders"></a>
## HTTP-Anforderungs- und -Antwortheader

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

<a name="Versioning"></a>
## API-Versionsverwaltung 
Die aktuelle Version der CM REST-API ist 1.0. Die Version wird in dem Segment angegeben, das im URI direkt auf das Segment `/api` folgt: `/api/v1.0`. Die Versionsnummer kann sich in der Zukunft ändern, sollten wichtige Änderungen an der API-Schnittstelle auftreten.

<a name="APIConfig"></a>
## Aktivieren der API 
Der Abschnitt `<ClmConfiguration>` der Datei „Web.config“ wurde durch einen neuen Schlüssel erweitert:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Dieser Schlüssel bestimmt, ob die CM REST-API für Clients zur Verfügung gestellt wird. Wenn der Schlüssel auf „False“ festgelegt ist, wird die Routenzuordnung für die API nicht beim Start der Anwendung ausgeführt. Dies bedeutet, dass alle nachfolgenden Anforderungen an API-Endpunkte den HTTP-Fehlercode 404 zurückgeben. Der Schlüssel ist standardmäßig auf „Deaktiviert“ festgelegt.
Denken Sie nach dem Ändern dieses Werts zu „True“ daran, dass Sie **iisreset** auf dem Server ausführen.

<a name="TracingConfig"></a>
## Aktivieren der Ablaufverfolgung und Protokollierung 
Die MIM CM REST-API gibt für jede an sie gesendete HTTP-Anforderung Ablaufverfolgungsdaten aus. Sie können den Ausführlichkeitsgrad für die ausgegebenen Ablaufverfolgungsinformationen einstellen, indem Sie den folgenden Konfigurationswert festlegen:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>
<a name="ErrorHandling"></a>
## Fehler- und Problembehandlung 
Wenn beim Verarbeiten einer Anforderung Ausnahmen auftreten, gibt die MIM CM REST-API einen HTTP-Statuscode an den Webclient zurück. Für allgemeine Fehler gibt die API einen entsprechenden HTTP-Statuscode und einen Fehlercode zurück. 

Nicht behandelte Ausnahmen werden in ein `HttpResponseException` mit HTTP-Statuscode 500 („Interner Fehler“) konvertiert und sowohl im Ereignisprotokoll als auch in der MIM CM-Ablaufverfolgungsdatei verfolgt. Jede nicht behandelte Ausnahme wird mit einer entsprechenden Korrelations-ID im Ereignisprotokoll eingetragen. Die Korrelations-ID wird auch an den Consumer der API in der Fehlermeldung gesendet. Ein Administrator kann das Ereignisprotokoll nach der entsprechenden Korrelations-ID und den Fehlerdetails durchsuchen, um den Fehler zu beheben.

Aufgrund von Sicherheitsbedenken werden Stapelüberwachungen zu Fehlern, die sich durch die Nutzung der MIM CM REST-API ergeben, nicht zurück an den Client gesendet.
<!--HONumber=Mar16_HO1-->
