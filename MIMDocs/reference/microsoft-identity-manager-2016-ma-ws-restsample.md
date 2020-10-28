---
title: App Service Beispiel für Web Service Connector-Rest-API | Microsoft-Dokumentation
description: Leitfaden für die Implementierung eines Beispiel-Rest-JSON-Servers in Azure
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/28/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: deb743fcbe4bdd155c1b0c4a31e24af0e8da8a4e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760836"
---
# <a name="web-service-connector-rest-api-app-service-sample"></a>App Service Beispiel für den Rest-API des webdienstconnector

Dieses Bereitstellungs Handbuch unterstützt Sie bei der Bereitstellung des Rest-JSON-Beispiel Servers in Azure. Sie können dieses Beispiel verwenden, um Sie bei der Konfiguration und dem Verständnis des webdienstconnector zu unterstützen.

- Für das Beispiel ist Microsoft Visual Studio 2017 erforderlich.
- Die NMP-Pakete (Native Module Path) im Beispiel verwenden diesen [JSON-Server](https://github.com/typicode/JSON-server) auf GitHub.
- Laden Sie den [Beispielcode](https://github.com/fimguy/SAMPLEREST) von GitHub herunter, und stellen Sie den Beispielcode für die Azure App Service bereit.

## <a name="deploy-the-json-server-sample"></a>Bereitstellen des JSON Server-Beispiels

1. Nachdem der Code heruntergeladen wurde, öffnen Sie die Projektmappendatei in [Visual Studio 2017](https://www.visualstudio.com/downloads/).

2. Stellen Sie die Projekt Mappe bereit, indem Sie das Projekt auswählen und dann mit der rechten Maustaste klicken und **veröffentlichen** auswählen:

    ![Veröffentlichen der Projektmappe](media/microsoft-identity-manager-2016-ma-ws-restsample/publish-project.png)

3. Wählen Sie die für die Bereitstellung zu verwendende App Service aus:

    ![Wählen Sie die APP Service](media/microsoft-identity-manager-2016-ma-ws-restsample/app-service.png)

4. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe:

    ![Wählen Sie eine Ressourcengruppe aus.](media/microsoft-identity-manager-2016-ma-ws-restsample/resource-group.png)

5. Verwenden Sie die vorhandenen Namen für den App Service, und klicken Sie auf **Erstellen** :

    ![Erstellen der App Service-Instanz](media/microsoft-identity-manager-2016-ma-ws-restsample/create.png)

    Der APP Service wird erstellt.

6. Wählen Sie **veröffentlichen** aus, um die APP Service zu veröffentlichen:

    ![Veröffentlichen des App Service](media/microsoft-identity-manager-2016-ma-ws-restsample/publish.png)

7. Nach der Veröffentlichung des App Service werden das Rest-API-Beispiel und die zugehörige Website in Ihrem Standardbrowser gestartet:

    ![Beispiel-Rest-API und-Website](media/microsoft-identity-manager-2016-ma-ws-restsample/sample-rest-api.png)

Nun können Sie die Bereitstellung konfigurieren, wie im [Rest-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-restgeneric.md)beschrieben.


## <a name="modify-the-sample"></a>Ändern des Beispiels

Um die JSON-Daten und das Rest-API-Beispiel zu ändern, nehmen Sie die Änderungen in der Datei **db.JSan** , und aktualisieren Sie dann die Bereitstellung:

![Aktualisieren Sie die db.JSfür die Datei.](media/microsoft-identity-manager-2016-ma-ws-restsample/db-json.png)


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über den generischen Webdienst-Connector](microsoft-identity-manager-2016-ma-ws.md)
- [Installieren des Webdienst-Konfigurationstools](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Rest-Bereitstellungs Handbuch](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Webdienst-MA-Konfiguration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
