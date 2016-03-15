---
Titel: Abrufen von Workflowrichtlinien
MS.Custom:
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology:
  - security
MS.tgt_pltfrm: Na
MS.topic: Referenz
MS.AssetId: be636205-c1f0-457c-982e-e17478cf0889
---
# Abrufen von Workflowrichtlinien
Ruft die Profilvorlagenrichtlinie für den angegebenen Workflow ab. Diese Daten werden während der Erstellung der Anforderung verwendet. Die Workflowrichtlinie gibt an, welche Daten vom Client benötigt werden, um eine Anforderung zu erstellen. Diese Daten können Folgendes enthalten: verschiedene Datensammlungselemente, Anforderungskommentare und eine Richtlinie für Einmalkennwörter.

**Hinweis**: In diesem Thema angezeigte URLs sind relativ zum Hostnamen, der während der API-Bereitstellung ausgewählt wurde. Beispiel: `https://api.contoso.com`.
##Anforderung


Methode  |Anforderungs-URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{Typ}

###URL-Parameter
Parameter| Beschreibung
--------|-------------
id| Erforderlich. Die GUID, die der Profilvorlage entspricht, aus der die Richtlinie extrahiert werden soll.
Typ| Erforderlich. Der Typ der angeforderten Richtlinie. Die folgenden Werte sind möglich: *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*, *Unblock*.

###Anforderungsheader
Informationen zu allgemeinen Anforderungsheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Anforderungstext
Keiner

##Antwort
###Antwortcodes
Code  |Beschreibung  
---------|---------
200     | OK
403 | Verboten
204 | Kein Inhalt
500 | Interner Fehler

###Antwortheader
Informationen zu allgemeinen Antwortheadern finden Sie unter [HTTP-Anforderung und Antwort-Header](certificate-management-rest-api-service-details.md#HttpHeaders) in *CM REST-API-Dienstdetails*.
###Antworttext
Bei Erfolg wird ein Richtlinienobjekt auf Basis eines [ProfileTemplatePolicy](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy(v=vs.100%29.aspx)-Objekts zurückgegeben. Das Richtlinienobjekt enthält mindestens die Eigenschaften in der folgenden Tabelle. Es kann aber in Abhängigkeit von der angeforderten Richtlinie zusätzliche Eigenschaften enthalten. Eine Registrierungsrichtlinie gibt z. B. ein [EnrollPolicy](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy(v=vs.100%29.aspx)-Objekt zurück. Weitere Informationen finden Sie in der Dokumentation zum Richtlinienobjekt, das dem Parameter „{type}“ in der Anforderung zugeordnet ist. Die Dokumentation für die verschiedenen Typen von Gruppenrichtlinienobjekten finden Sie unter der [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates(v=vs.100%29.aspx)-Dokumentation.

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

##Beispiel

###Anforderung 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###Antwort 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###Anforderung 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###Antwort 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##Weitere Informationen

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy-Klasse](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates-Namespace](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates(v=vs.100%29.aspx)
<!--HONumber=Mar16_HO1-->
