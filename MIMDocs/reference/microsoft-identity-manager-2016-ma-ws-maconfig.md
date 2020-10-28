---
title: Konfigurationsoptionen für den webdienstconnector | Microsoft-Dokumentation
description: In diesem Artikel werden die Schritte beschrieben, die zum Installieren des Webdienst-Konfigurationstools erforderlich sind.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 3/27/2020
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.reviewer: markwahl-msft
ms.assetid: ''
ms.openlocfilehash: 34c83427b6dfb3084976aebf29c019d8228f8247
ms.sourcegitcommit: d21963c1fba6dc908bec5eaadc54e3395a8ef8c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "92761025"
---
# <a name="web-service-connector-configuration-options"></a>Konfigurationsoptionen für den Connector für Webdienste
In diesem Artikel werden die Schritte zum Konfigurieren eines neuen webdienstconnector oder zum vornehmen von Änderungen an einem vorhandenen webdienstconnector über die Benutzeroberfläche des Microsoft Identity Manager (MIM)-Synchronisierungs Diensts beschrieben.

>[!IMPORTANT]
>Laden Sie den [webdienstconnector](https://www.microsoft.com/download/details.aspx?id=51495) herunter, und installieren Sie ihn, bevor Sie die Schritte in diesem Artikel ausführen.

## <a name="configure-the-web-service-connector-in-the-synchronization-service"></a>Konfigurieren des webdienstconnector im Synchronisierungs Dienst

Mit dem Verwaltungs-Agent-Designer können Sie einen neuen webdienstconnector erstellen. Nachdem Sie den Connector erstellt haben, können Sie mehrere Lauf profile definieren, um verschiedene Aufgaben auszuführen. Wenn Sie einen vorhandenen Connector konfigurieren, können Sie eine Aufgabe ändern, indem Sie im Verwaltungs-Agent-Designer auf die entsprechende Seite klicken. Führen Sie die folgenden Schritte aus, um einen neuen Webdienst-Connector zu konfigurieren.

1. Öffnen Sie Microsoft Identity Manager 2016-Synchronisierungs Dienst. Klicken Sie **im Menü Extras** auf **Verwaltungs-Agents** .

2. Wählen Sie im Menü **Aktionen** die Option **Erstellen** aus. Der Verwaltungs-Agent-Designer wird geöffnet.

3. Wählen Sie im **Verwaltungs-Agent-Designer** unter Verwaltungs- **Agent für** die Option **Webdienst (Microsoft)** aus. Klicken Sie anschließend auf **Weiter** .

    ![Erstellen eines Verwaltungs-Agents](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma.png)

4. Wählen Sie auf dem Bildschirm **Konnektivität** das **Standardweb Service Connector-Projekt** aus. Geben Sie Werte für den **Host** und den **Port** an. Klicken Sie anschließend auf **Weiter** .

    ![Konnektivität für den Verwaltungs-Agent erstellen](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-connectivity.png)

5. Definieren Sie die **globalen Parameter** . Verwenden Sie die Anmelde Informationen, die vom Webdienst Administrator für die Verbindung mit dem Host beschafft wurden. Klicken Sie anschließend auf **Weiter** .

    ![Festlegen globaler Parameter für den Verwaltungs-Agent](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-global-parameters.png)

    - Wenn der Speicherort der Datenquelle die Sommerzeit beachtet und die Datenquelle so konfiguriert ist, dass Sie automatisch an die Sommerzeit angepasst wird, aktivieren Sie die Option **Datenquelle ist so konfiguriert, dass die Uhr für die Sommerzeit automatisch angepasst** wird.
    - Wenn Sie den Verbindungs Workflow Testen von diesem Connector aus initiieren möchten, aktivieren Sie die Option **Verbindung testen** .

6. Wählen Sie auf dem nächsten Bildschirm die Option **Standard** für **Verzeichnis Partitionen auswählen** aus. Klicken Sie anschließend auf **Weiter** .

    ![Erstellen von Partitionen für den Verwaltungs-Agent](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-partitions.png)

7. Wählen Sie auf der Seite **Objekttypen auswählen** den Objekttyp aus, mit dem Sie arbeiten möchten. Standardmäßig unterstützt der Webdienst-Connector zwei Objekttypen: **Mitarbeiter** und **Benutzer** . Klicken Sie anschließend auf **Weiter** .

    ![Objekttyp auswählen](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-object-types.png)

8. Wählen Sie auf der Seite **Attribute auswählen** alle obligatorischen Attribute für die ausgewählten Objekte und Attribute aus, die Sie bearbeiten müssen. Klicken Sie anschließend auf **Weiter** .

    ![Wählen Sie die Attribute für die Objekte aus.](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-attributes.png)

9. Geben Sie auf der Seite **Anker konfigurieren** die Anker Attribute an. Klicken Sie anschließend auf **Weiter** .

    ![Konfigurieren der Anker](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-anchors.png)

10. Geben Sie auf der Seite **Connector-Filter konfigurieren** den **Connector-Filter** an. Klicken Sie anschließend auf **Weiter** .

    ![Angeben des Connector-Filters](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-connector-filter.png)

11. Geben Sie auf der Seite **Join-und Projektions Regeln konfigurieren** die Join-und Projektions Regeln an. Sie können eine neue joinregel und Projektions Regel erstellen, indem Sie **neue joinregel** bzw. **neue Projektions Regel** auswählen. Klicken Sie anschließend auf **Weiter** .

    ![Angeben der Join-und Projektions Regeln](media/microsoft-identity-manager-2016-ma-ws-maconfig/join-projection.png)

12. Konfigurieren Sie auf der nächsten Seite den Attribut Fluss. Sie müssen den **mappentyp** und die **Fluss Richtung** für die Attribute für die ausgewählten Objekttypen angeben. Klicken Sie anschließend auf **Weiter** .

    ![Konfigurieren des Attribut Flusses](media/microsoft-identity-manager-2016-ma-ws-maconfig/attribute-flow.png)

13. Geben Sie den Typ der Aufhebung der Bereitstellung an, der auf die Objekte angewendet werden soll. Klicken Sie anschließend auf **Weiter** .

    ![Geben Sie den Typ der Aufhebung der Bereitstellung an.](media/microsoft-identity-manager-2016-ma-ws-maconfig/deprovisioning.png)

14. Im Fall eines Import Flusses ist die Seite **Erweiterungen konfigurieren** deaktiviert. Sie können Erweiterungen für Export Flows konfigurieren, indem Sie zuerst den **erweiterten** zuordungstyp auf der Seite **Attribut Fluss konfigurieren** auswählen.

    ![Erweiterungen konfigurieren](media/microsoft-identity-manager-2016-ma-ws-maconfig/extensions.png)

15. Klicken Sie auf **Fertig stellen** .

Ihr Connector ist nun konfiguriert:

![Connector-Konfiguration ist beendet](media/microsoft-identity-manager-2016-ma-ws-maconfig/sync-manager.png)

Nachdem ein Connector konfiguriert wurde, können Sie die Lauf Profile konfigurieren, indem Sie die Option " **Run Profiles konfigurieren** " auswählen.

## <a name="additional-steps"></a>Zusätzliche Schritte

Wenn die Zertifikat basierte Authentifizierung verwendet wird, ist eine zusätzliche Änderung erforderlich, nachdem das Webdienst-Konfigurations Tool eine wsconfig-Datei generiert hat, bevor diese Datei in ein webdienstconnector-Projekt im MIM-Synchronisierungs Dienst importiert werden kann.

So aktivieren Sie die Zertifikat basierte Authentifizierung:

- Konfigurieren des Projekts für die Verwendung der Standard Authentifizierung im Webdienst-Konfigurations Tool
- Erstellen Sie eine Kopie der Datei my_project. wsconfig, und benennen Sie Sie in my_project.zip
- Öffnen Sie dieses Archiv, und ändern Sie generated.config Datei, um die Standard Authentifizierung durch Zertifikat basierte Authentifizierung (ein Beispiel unten) zu ersetzen.
- Ersetzen Sie generated.config Datei in my_project.zip, und benennen Sie Sie in my_project_updated. wsconfig um.
- Wählen Sie beim Erstellen eines Verwaltungs-Agents auf dem MIM-Synchronisierungs Server my_project_updated. wsconfig.

Suchen Sie unten nach generated.config Beispieldatei mit Zertifikat basierter Authentifizierung:

```xml
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="SoapAuthenticationType" value="Certificate"/>
        </appSettings>
        <system.serviceModel>
            <bindings>
                <wsHttpBinding>
                    <binding name="binding">
                        <security mode="Transport">
                            <transport clientCredentialType="Certificate"/>
                        </security>
                    </binding>
                </wsHttpBinding>
            </bindings>
            <client>
                <endpoint address="https://myserver.local.net:8011/sap/bc/srt/scs/sap/zsapconnect?sap-client=800"
                    binding="wsHttpBinding" bindingConfiguration="binding"
                    contract="SAPCONNECTOR.ZSAPConnect" name="binding"/>
            </client>
            <behaviors>
                <endpointBehaviors>
                    <behavior name="endpointCredentialBehavior">
                        <clientCredentials>
                            <clientCertificate findValue="my.certificate.name.local.net"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName"/>
                        </clientCredentials>
                    </behavior>
                </endpointBehaviors>
            </behaviors>
        </system.serviceModel>
    </configuration>
```

## <a name="next-steps"></a>Nächste Schritte

- [Installieren des Webdienst-Konfigurationstools](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Rest-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webdienst-MA-Konfiguration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
