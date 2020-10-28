---
title: PAM Rest-API-Dienst Details | Microsoft-Dokumentation
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00a2f4d9c44747d50139655d368e42b11fbd388c
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2020
ms.locfileid: "92760989"
---
# <a name="pam-rest-api-service-details"></a>PAM Rest-API-Dienst Details
In den folgenden Abschnitten werden die Details der PAM REST-API (privilegierte Zugriffsverwaltung) von Microsoft Identity Manager (MIM) besprochen.

<h2 id="http-request-and-response-headers">HTTP-Anforderungsheader</h2>

HTTP-Anforderungen, die an die API gesendet werden, sollten die folgenden Header enthalten (diese Liste ist nicht vollständig):

Header | BESCHREIBUNG
-------|------------
Authorization | Erforderlich. Der Inhalt hängt von der Authentifizierungsmethode ab, die konfiguriert werden kann und auf WIA (integrierte Windows-Authentifizierung) oder ADFS basiert.
Content-Type | Erforderlich, wenn die Anforderung Text enthält. Muss auf `application/json` festgelegt sein.
Content-Length | Erforderlich, wenn die Anforderung Text enthält. 
Cookie | Das Sitzungscookie. Möglicherweise je nach Authentifizierungsmethode erforderlich.

## <a name="http-response-headers"></a>HTTP-Antwortheader

HTTP-Antworten sollten die folgenden Header enthalten (diese Liste ist nicht vollständig):

Header | BESCHREIBUNG
-------|------------
Content-Type | Die API gibt immer zurück `application/json` .
Content-Length | Die Länge des Anforderungstexts, falls vorhanden, in Bytes.

## <a name="versioning"></a>Versionsverwaltung 
Die aktuelle Version der API ist 1. Wie im folgenden Beispiel gezeigt, kann die API-Version über einen Abfrageparameter in der Anforderungs-URL angegeben werden: `http://localhost:8086/api/pamresources/pamrequests?v=1` Wenn die Version in der Anforderung nicht angegeben wird, wird die Anforderung für die zuletzt veröffentlichte Version der API ausgeführt. 

## <a name="security"></a>Sicherheit 
Der Zugriff auf die API erfordert die integrierte Windows-Authentifizierung (IWA). Dies muss vor der Installation von Microsoft Identity Manager (MIM) manuell in IIS konfiguriert werden.

HTTPS (TLS) wird unterstützt, muss aber manuell in IIS konfiguriert werden. Weitere Informationen finden Sie unter: **Implementieren von Secure Sockets Layer (SSL) für das FIM-Portal** in [Schritt 9: Durchführen der Aufgaben nach der Installation von FIM 2010 R2](https://technet.microsoft.com/library/hh322875.aspx) in der Test Umgebungs Anleitung für die Installation von FIM 2010 R2. 

Sie können ein neues SSL-Serverzertifikat generieren, indem Sie den folgenden Befehl über die Visual Studio-Eingabeaufforderung ausführen:

```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Der Befehl erstellt ein selbst signiertes Zertifikat, das zum Testen einer Webanwendung verwendet werden kann, die SSL auf einem Webserver verwendet, auf dem sich die URL befindet `test.cwap.com` . Die durch die Option definierte OID `-eku` identifiziert das Zertifikat als SSL-Serverzertifikat. Das Zertifikat wird **im Speicher gespeichert und steht auf Computer** Ebene zur Verfügung. Sie können das Zertifikat aus dem Snap-in "Zertifikate" in mmc.exe exportieren.

## <a name="cross-domain-access-cors"></a>Domänenübergreifender Zugriff (Cross Domain Access, CORS) 
CORS wird unterstützt, muss aber manuell in IIS konfiguriert werden. Fügen Sie die folgenden Elemente zur bereitgestellten API-Datei „web.config“ hinzu, um die API so zu konfigurieren, dass domänenübergreifende Aufrufe zulässig sind: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```

## <a name="error-handling"></a>Fehlerbehandlung 
Die API gibt HTTP-Fehlerantworten zurück, um Fehlerbedingungen anzuzeigen. Fehler sind OData-kompatibel. In der folgenden Tabelle sind die Fehlercodes aufgeführt, die an einen Client zurückgegeben werden können:

HTTP-Statuscode | BESCHREIBUNG
-----------------|------------
401 | Nicht autorisiert 
403 | Verboten 
408 | Anforderungstimeout   
500 | Interner Serverfehler 
503 | Dienst nicht verfügbar 

## <a name="filtering"></a>Filterung 
PAM REST-API-Anforderungen können Filter einbeziehen, um die Eigenschaften anzugeben, die in der Antwort enthalten sein sollen. Die Filtersyntax basiert auf OData-Ausdrücken.

Filter können beliebige Eigenschaften von PAM-Anfragen, PAM-Rollen oder ausstehenden PAM-Anforderungen angeben. Beispiel: *ExpirationTime* , *DisplayName* oder eine andere gültige Eigenschaft einer PAM-Anforderung, PAM-Rolle oder ausstehenden Anforderung.

Die API unterstützt in Filterausdrücken die folgenden Operatoren: *And* , *Equal* , *NotEqual* , *GreaterThan* , *LessThan* , *GreaterThenOrEqueal* und *LessThanOrEqual* . 

Die folgenden Beispielanforderungen beziehen Filter ein:

- Diese Anforderung gibt alle PAM-Anforderungen zwischen bestimmten Datumsangaben zurück: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime'2015-01-09T08:26:49.721Z' and ExpirationTime lt datetime'2015-02-10T08:26:49.722Z' `
 
- Diese Anforderung gibt die PAM-Role mit dem Anzeigenamen „SQL-Dateizugriff“ zurück: `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq 'SQL File Access' `
