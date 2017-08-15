---
title: PAM REST-API-Dienstdetails | Microsoft-Dokumentation
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>PAM REST-API-Dienstdetails
In den folgenden Abschnitten werden die Details der PAM REST-API (privilegierte Zugriffsverwaltung) von Microsoft Identity Manager (MIM) besprochen.

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

## <a name="versioning"></a>Versionsverwaltung 
Die aktuelle Version der API ist 1. Wie im folgenden Beispiel gezeigt, kann die API-Version über einen Abfrageparameter in der Anforderungs-URL angegeben werden: `http://localhost:8086/api/pamresources/pamrequests?v=1` Wenn die Version in der Anforderung nicht angegeben wird, wird die Anforderung für die zuletzt veröffentlichte Version der API ausgeführt. 

## <a name="security"></a>Sicherheit 
Der Zugriff auf die API erfordert die integrierte Windows-Authentifizierung (IWA). Dies muss vor der Installation von Microsoft Identity Manager (MIM) manuell in IIS konfiguriert werden.

HTTPS (TLS) wird unterstützt, muss aber manuell in IIS konfiguriert werden. Weitere Informationen finden Sie unter: **Implementieren von SSL (Secure Sockets Layer) für das FIM-Portal** in [Schritt 9: Ausführen von Aufgaben nach der Installation von FIM 2010 R2](https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx) in der Testumgebungsanleitung „Installieren von FIM 2010 R2“. 

Sie können ein neues SSL-Serverzertifikat generieren, indem Sie den folgenden Befehl über die Visual Studio-Eingabeaufforderung ausführen:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Dieser Befehl erstellt ein selbstsigniertes Zertifikat, mit dem eine Webanwendung getestet werden kann, die SSL auf einem Webserver verwendet, dessen URL „test.cwap.com“ lautet. Die durch die Option „-eku“ definierte OID kennzeichnet das Zertifikat als SSL-Serverzertifikat. Das Zertifikat wird im persönlichen Speicher gespeichert und steht auf Computerebene zur Verfügung, daher können Sie es über das Snap-In „Zertifikate“ in mmc.exe exportieren.

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
<br/>

## <a name="error-handling"></a>Fehlerbehandlung 
Die API gibt HTTP-Fehlerantworten zurück, um Fehlerbedingungen anzuzeigen. Fehler sind OData-kompatibel. Die folgende Tabelle zeigt die Fehlercodes, die an einen Client zurückgegeben werden können.

HTTP-Statuscode | Beschreibung
-----------------|------------
401 | Nicht autorisiert 
403 | Verboten 
408 | Anforderungstimeout   
500 | Interner Serverfehler. 
503 | Dienst nicht verfügbar. 
<br/>

## <a name="filtering"></a>Filterung 
PAM REST-API-Anforderungen können Filter einbeziehen, um die Eigenschaften anzugeben, die in der Antwort enthalten sein sollen. Die Filtersyntax basiert auf OData-Ausdrücken.

Filter können beliebige Eigenschaften von PAM-Anfragen, PAM-Rollen oder ausstehenden PAM-Anforderungen angeben. Zum Beispiel: *ExpirationTime*, *DisplayName*oder eine andere gültige Eigenschaft einer PAM-Anforderung, PAM-Rolle oder ausstehenden Anforderung.

Die API unterstützt die folgenden Operatoren in Filterausdrücken: *Und*, *Gleich*, *Ungleich*, *Größer als*, *Kleiner als*, *Größer als oder gleich*und *Kleiner als oder gleich*. 

Die folgenden Beispielanforderungen beziehen Filter ein:

- Diese Anforderung gibt alle PAM-Anforderungen zwischen bestimmten Datumsangaben zurück: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Diese Anforderung gibt die PAM-Role mit dem Anzeigenamen „SQL-Dateizugriff“ zurück: `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
