---
title: Übersicht über den generischen Webdienst-Connector | Microsoft-Dokumentation
description: Übersicht über die Konfiguration und die Anforderungen für den generischen webdienstconnector.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: b0d4cc9ac9b9ac080038d632df0c02c4c244e3d6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760800"
---
# <a name="overview-of-the-generic-web-service-connector"></a>Übersicht über den generischen Webdienst-Connector

Der Webdienst-Connector integriert Identitäten durch Webdienst Vorgänge mit Microsoft Identity Manager (MIM) 2016 SP1. Der Connector erfordert, dass die Webdienst-Projektdatei eine Verbindung mit der richtigen Datenquelle herstellt. Dieses Projekt kann aus dem [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=235883) heruntergeladen werden, und es wird die [Dokumentation](https://www.microsoft.com/en-us/download/details.aspx?id=29943) für die Verwendung des-Connector mit Oracle eBusiness, Oracle PeopleSoft und SAP heruntergeladen. Sie können Sie auch mithilfe des Webdienst-Konfigurationstools erstellen.

Wenn der MIM-Synchronisierungs Dienst den webdienstconnector aufruft, lädt er die konfigurierte Projektdatei ( **wsconfig** -Datei). Mit dieser Datei kann der Endpunkt der Datenquelle erkannt werden, der zum Herstellen einer Verbindung verwendet werden soll. Die Datei teilt dem Workflow außerdem mit, dass der Workflow ausgeführt werden soll, um einen MIM-Vorgang zu implementieren. Zum Ausführen der konfigurierten Workflows nutzt der Webdienst-Connector die Lauf Zeit-Engine von .NET 4 Workflow Foundation.

![Workflow](media/microsoft-identity-manager-2016-ma-ws/workflow.png)

## <a name="web-service-layers"></a>Webdienst Ebenen

Zwei Hauptebenen werden verwendet, um die Lösung für den webdienstverwaltungs-Agent (MA) zu implementieren: 

- Webdienst-Konfigurations Tool
- Lauf Zeit-Connector implementiert mit Workflow .NET 4,0

## <a name="supported-data-sources-for-web-service-discovery"></a>Unterstützte Datenquellen für die Webdienst Ermittlung

Das Webdienst-Konfigurations Tool implementiert die folgenden Funktionen:

- SOAP-Ermittlung: ermöglicht es dem Administrator, den WSDL-Pfad einzugeben, der vom Zielweb Dienst verfügbar gemacht wird. Bei der Ermittlung wird eine Baumstruktur der gehosteten Webdienste mit ihren inneren Endpunkten erstellt/Operations zusammen mit der Beschreibung des Vorgangs Meta Data. Es gibt keine Beschränkung für die Anzahl von Ermittlungs Vorgängen, die durchgeführt werden können (Schritt für Schritt). Die ermittelten Vorgänge werden später verwendet, um den Ablauf von Vorgängen zu konfigurieren, die die Vorgänge des Verbindungs Diensts für die Datenquelle implementieren (als Import/Export/Kennwort).

- Rest-Ermittlung: ermöglicht es dem Administrator, Rest-Dienst Details einzugeben, d. h. Dienst Endpunkt, Ressourcen Pfad, Methoden-und Parameter Details. Ein Benutzer kann eine unbegrenzte Anzahl von Rest-Diensten hinzufügen. Die Rest-Dienst Informationen werden in ```discovery.xml``` der Datei ```wsconfig``` Projekt gespeichert. Sie werden später vom Benutzer verwendet, um die Rest-Webdienst Aktivität im Workflow zu konfigurieren.

- Konfiguration des Connector-Speicherplatzes: ermöglicht dem Administrator, das Verbindungsraum Schema des-Verbindungs Bereichs zu konfigurieren. Die Schema Konfiguration enthält eine Auflistung von Objekttypen und Attributen für eine bestimmte Implementierung. Der Administrator kann die Objekttypen angeben, die vom Webdienst-MA unterstützt werden. Der Administrator kann hier auch die Attribute auswählen, die Teil des Connector-Bereichs Schemas sein werden.

- Vorgangs Fluss Konfiguration: Workflow-Designer-Benutzeroberfläche zum Konfigurieren der Implementierung von FIM-Vorgängen (Import/Export/Kennwort) pro Objekttyp durch verfügbar gemachte Webdienst-Vorgangs Funktionen wie:

    - Zuweisung von Parametern aus dem Connector-Bereich zu Webdienst Funktionen.
    - Zuweisung von Parametern von Webdienst Funktionen zum Connector-Bereich.

## <a name="resources-generated-by-the-web-service-configuration-tool"></a>Vom Webdienst-Konfigurations Tool generierte Ressourcen

Das Webdienst-Konfigurations Tool generiert die erforderlichen Ressourcen, die erforderlich sind, um einen voll funktionsfähigen Webdienst-MA zu konfigurieren. dazu gehören:

- Connector-Speicher Schema: eine Binärdatei, die die Schema Konfiguration einschließt. Die Datei wird von MIM über die- ```Get Schema``` Schnittstelle importiert, wenn die Ma mithilfe der FIM-Synchronisierungs-Benutzeroberfläche konfiguriert wird. Anschließend wird Sie in das ECMA2 Schema Format-Objekt konvertiert.

- Workflows: eine Reihe von Workflow Definitionen. Sie werden vom Webdienst-mA zur Laufzeit verwendet, um einen geeigneten Vorgang auszuführen.

- WCF-Konfigurationsdatei: eine Konfigurationsdatei, die durch den Ermittlungs Vorgang erstellt wird. Die Datei enthält die Bindungs-und Endpunkte Informationen, die erforderlich sind, um einen Webdienst Vorgang für die Datenquelle aufzurufen.

- Daten Vertragsassembly: da der webdienstconnector jetzt sowohl SOAP-als auch Rest-Dienst unterstützt, unterscheiden sich die für generierten Datenverträge in der generated.dll Datei.

- SOAP-Assembly: beim Ausführen der WSDL-Eingabe generiert das Webdienst-Konfigurationstool Daten Vertragstypen, bei denen es sich um Datenstrukturen handelt, die von den Webdienst Vorgängen für die Kommunikation mit dem Remote Dienst verwendet werden. Diese Vertragstypen werden auch verwendet, um Remote Datenquellen Entitäten für Objekttyp-Attribut Zuordnung verfügbar zu machen.

- Rest-Assembly: beim Beispiel für eine Anforderungs Antwort für den Rest-Webdienst generiert das Konfigurationstool Typen (Klassen), die im Workflow verwendet werden, um über die Aktivität "Webdienst Aufrufe" mit dem Webdienst zu kommunizieren. Jede Anforderung/Antwort wird in einem eigenen Namespace definiert. Der Namespace weist eine Syntax als \< Service Name auf \> . \< ResourceName \> . \< MethodName \> . [ Anforderung/Antwort]. Wenn Sie jede Anforderung/Antwort in einem separaten Namespace umbenennen, können Probleme aufgrund eines doppelten Typs (Klassen) reduziert werden.

![Workflow](media/microsoft-identity-manager-2016-ma-ws/workflow2.png)

### <a name="project-file-type"></a>Projekt Dateityp

Der Webdienst MA wird in komprimierter Datei (ZIP-Format) gespeichert, wobei der von der Benutzer-und der Dateierweiterung "wsconfig" angegebene Name angegeben ist. Die Dateierweiterung "wsconfig" wird registriert und vom Installationsprogramm mit dem Webdienst-Konfigurationstool verknüpft. Vorhandene MA-Projekte können geöffnet, geändert und gespeichert werden. Sie können im Ordner Erweiterungen des FIM-Synchronisierungs Dienstanbieter oder an einem anderen Speicherort gespeichert werden. Änderungen im Zusammenhang mit Objekttyp und Attributen müssen auf FIM-Seite synchronisiert werden.  Das-Konfigurationstool ist eine Anwendung mit mehreren Instanzen, die zum Erstellen und Ändern von Ma (s) entwickelt wurde.

## <a name="supported-security-modes"></a>Unterstützte Sicherheitsmodi

Die Rest-/SOAP-Webdienst Anwendung kann mithilfe eines Webservers wie IIS gesichert werden. Die Anwendung ermöglicht dem Benutzer, den Sicherheitsmodus auszuwählen, wie in der folgenden Abbildung dargestellt. Zu den Sicherheitsmodi zählen Basic, Digest, Certificate, Windows oder None.

![Sicherheitsmodi](media/microsoft-identity-manager-2016-ma-ws/security-mode.png)

## <a name="supported-data-types"></a>Unterstützte Datentypen

Die folgenden Datentypen werden unterstützt:

- SOAP (Legacy): der SOAP-Datentyp wird wie in diesem [MSDN-Artikel](https://msdn.microsoft.com/library/ms995800.aspx)beschrieben unterstützt. Die Unterstützung wird nur für den BAPI-Stapel (Business Application Programming Interface) bereitgestellt. Beispiel-SOAP-Vorlagen sind im [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495)verfügbar.
- Rest (nicht odata): ein HTTP-Protokoll basierter Connector/Web.

## <a name="next-steps"></a>Nächste Schritte 

- [Installieren des Webdienst-Konfigurationstools](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Rest-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webdienst-MA-Konfiguration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
