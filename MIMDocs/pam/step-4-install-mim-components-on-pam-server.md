---
title: "Bereitstellen von PAM – Schritt 4: Installieren von MIM | Microsoft Docs"
description: Installieren und konfigurieren Sie den MIM-Dienst und das MIM-Portal auf Ihrem Privileged Access Management-Server und den entsprechenden Arbeitsstationen.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3a1ec9db6da0a77f963dde76a3efe8d92f89078d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2017
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>Schritt 4 – Installieren von MIM-Komponenten auf PAM-Server und Arbeitsstation

>[!div class="step-by-step"]
[« Schritt 3](step-3-prepare-pam-server.md)
[Schritt 5 »](step-5-establish-trust-between-priv-corp-forests.md)


Melden Sie sich bei PAMSRV als PRIV\Administrator an, damit Sie den MIM-Dienst, das Portal und die Beispiel-Portalwebanwendung installieren können.

  > [!NOTE]
  > Sie müssen Domänenadministrator sein. Wenn Sie die folgenden Befehle nicht als Domänenadministrator ausführen, werden die Überprüfungen der Vertrauensstellung im nächsten Schritt nicht abgeschlossen.

Wenn Sie MIM heruntergeladen haben, entpacken Sie das MIM-Installationsarchiv in einem neuen Ordner.

##  <a name="run-the-service-and-portal-install-program"></a>Führen Sie das Installationsprogramm für den Dienst und das Portal aus.  

Befolgen Sie die Anweisungen des Installationsprogramms, und schließen Sie die Installation ab.

1.  Wenn Sie Komponentenfeatures auswählen, beziehen Sie MIM-Dienst (mit Privileged Access Management, aber nicht MIM-Berichterstellung) und MIM-Portal mit ein.  

    ![Benutzerdefiniertes Setup – Screenshot](./media/PAM_GS_MIM_2015_Service_Portal.png)

2.  Geben Sie beim Konfigurieren gemeinsamer Dienste und der MIM-Datenbankverbindung **Neue Datenbank** erstellen an.

    > [!NOTE]
    > Wenn Sie den MIM-Dienst mehrere Male für hohe Verfügbarkeit installieren, geben Sie **Vorhandene Datenbank verwenden** für alle nachfolgenden Installationen an.

3.  Legen Sie beim Konfigurieren einer E-Mail-Serververbindung für den E-Mail-Server den Hostnamen eines Exchange- oder SMTP-Servers für die CORP-Umgebung fest (verwenden Sie „corpdc.contoso.local“, wenn Sie keinen E-Mail-Server besitzen), und deaktivieren Sie die Kontrollkästchen **SSL verwenden** und **E-Mail-Server ist Exchange Server 2007 oder Exchange Server 2010**.

4.  Wählen Sie das Generieren eines neuen selbstsignierten Zertifikats.

5.  Legen Sie die folgenden Anmeldeinformationen fest:
    - Name des Dienstkontos: *MIMService*  
    - Dienstkontokennwort: *Pass@word1* (oder das Kennwort, das Sie in Schritt 2 erstellt haben)  
    - Dienstkontodomäne: *PRIV*  
    - Dienst-E-Mail-Konto: *MIMService@priv.contoso.local*  

6.  Akzeptieren Sie die Standardeinstellungen für den Hostnamen des Synchronisierungsservers, und geben Sie für das MIM-Verwaltungs-Agent-Konto *PRIV\MIMMA* an. Es wird eine Warnmeldung angezeigt, dass der MIM-Synchronisierungsdienst nicht vorhanden ist. Dies ist in Ordnung, da der MIM-Synchronisierungsdienst in diesem Szenario nicht verwendet wird.

7.  Geben Sie *PAMSRV* als Serveradresse für den MIM-Dienst an.

8.  Geben Sie *http://pamsrv.priv.contoso.local:82* als URL für die SharePoint-Websitesammlung an.

9. Lassen Sie die URL für das Registrierungsportal leer.

10. Aktivieren Sie das Kontrollkästchen, um die Ports 5725 und 5726 in der Firewall zu öffnen, sowie das Kontrollkästchen, um allen authentifizierten Benutzern den Zugriff auf die MIM-Portalwebsite zu gewähren.

11. Lassen Sie den PAM REST-API-Hostnamen leer, und geben Sie *8086* als Portnummer an.

  ![Bindungsinformationen für die PAM REST-API – Screenshot](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Konfigurieren Sie das MIM PAM REST-API-Konto so, dass das auch von SharePoint verwendete Konto verwendet wird (da sich das MIM-Portal ebenfalls auf diesem Server befindet):
    - Anwendungspoolkonto-Name: *SharePoint*  
    - Anwendungspoolkonto-Kennwort: *Pass@word1* (oder das Kennwort, das Sie in Schritt 2 erstellt haben)  
    - Anwendungspoolkonto-Domäne: *PRIV*  

    ![Anmeldeinformationen für Anwendungspoolkonto – Screenshot](./media/PAM_GS_Configure_Component_Service.png)

    Es kann eine Warnung angezeigt werden, dass das Dienstkonto in seiner aktuellen Konfiguration nicht sicher ist. Das ist in Ordnung.

13. Konfigurieren Sie den MIM PAM-Komponentendienst:
    - Name des Dienstkontos: *MIMComponent*
    - Dienstkontokennwort: *Pass@word1* (oder das Kennwort, das Sie in Schritt 2 erstellt haben)  
    - Dienstkontodomäne: *PRIV*

  ![Anmeldeinformationen für PAM-Komponentendienst – Screenshot](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Konfigurieren Sie den PAM-Überwachungsdienst:
    - Name des Dienstkontos: *MIMMonitor*  
    - Dienstkontokennwort: *Pass@word1* (oder das Kennwort, das Sie in Schritt 2 erstellt haben)  
    - Dienstkontodomäne: *PRIV*  

  ![Anmeldeinformationen für PAM-Überwachungsdienst– Screenshot](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Lassen Sie auf der Seite zur Eingabe von Informationen für MIM-Kennwortportale die Kontrollkästchen deaktiviert, und fahren Sie fort. Klicken Sie auf **Weiter**, um die Installation fortzusetzen.

Nach Abschluss der Installation wird der Server neu gestartet. Überprüfen Sie dann, ob das MIM-Portal aktiv ist, und ermöglichen Sie es Benutzern, ihre eigene Objektressource in MIM anzuzeigen.

## <a name="set-up-mim-portal-management-policy-rules"></a>Einrichten von Verwaltungsrichtlinienregeln für das MIM-Portal

1. Melden Sie sich nach dem Neustart von PAMSRV als „PRIV\Administrator“ an.

2. Starten Sie Internet Explorer, und stellen Sie über http://pamsrv.priv.contoso.local:82/identitymanagement eine Verbindung mit dem MIM-Portal her. Es tritt möglicherweise eine kurze Verzögerung auf, wenn die Seite das erste Mal gefunden wird.

3. Melden Sie sich in Internet Explorer ggf. als „PRIV\Administrator“ an.

4. Öffnen Sie in Internet Explorer die **Internetoptionen**, wechseln Sie zur Registerkarte **Sicherheit**, und fügen Sie die Website der Zone **Lokales Intranet** hinzu, wenn sie noch nicht vorhanden ist. Schließen Sie das Dialogfeld „Internetoptionen“.

5. Klicken Sie in Internet Explorer auf **Verwaltungsrichtlinienregeln**, um das MIM-Portal anzuzeigen.

6. Suchen Sie nach der Verwaltungsrichtlinienregel **Benutzerverwaltung: Benutzer können eigene Attribute lesen**.

7. Wählen Sie diese Verwaltungsrichtlinienregel aus, deaktivieren Sie **Richtlinie ist deaktiviert**, klicken Sie auf **OK** und dann auf **Absenden**.

## <a name="verify-the-firewall-connections"></a>Überprüfen der Firewallverbindungen

Die Firewall sollte eingehende Verbindungen an den TCP-Ports 5725, 5726, 8086 und 8090 zulassen.

1.  Starten Sie **Windows-Firewall mit erweiterter Sicherheit** (befindet sich in „Verwaltung“).  
2.  Klicken Sie auf **Eingehende Regeln**.  
3.  Stellen Sie sicher, dass diese zwei Regeln aufgeführt sind:  
    - Forefront Identity Manager-Dienst (STS)
    - Forefront Identity Manager-Dienst (Webservice)  
4.  Klicken Sie auf **Neue Regel** > **Port** > **TCP**, und geben Sie die lokalen Ports *8086* und *8090* ein. Klicken Sie im Assistenten, um die Standardwerte zu übernehmen, benennen Sie die Regel, und klicken Sie dann auf **Fertig stellen**.  
5.  Nachdem Sie den Assistenten abgeschlossen haben, schließen Sie die Anwendung „Windows-Firewall“.

6.  Starten Sie die **Systemsteuerung**.  
7.  Wählen Sie unter „Netzwerk und Internet“ **Netzwerkstatus und -aufgaben anzeigen**.  
8.  Stellen Sie sicher, dass das aktive Netzwerk „priv.contoso.local“ und ein Domänennetzwerk aufgeführt wird.  
9. Schließen Sie die **Systemsteuerung**.

## <a name="set-up-the-sample-web-application"></a>Einrichten der Beispielwebanwendung

In diesem Abschnitt installieren und konfigurieren Sie die Beispielwebanwendung für die MIM PAM REST-API.

1.  Laden Sie aus dem Beispielwebanwendungs-Archiv die [Identitätsverwaltungsbeispiele](https://github.com/Azure/identity-management-samples) als ZIP-Datei herunter.

2. Entpacken Sie die Inhalte des Ordners **identity-management-samples-master\Privileged-Access-Management-Portal\src** in den neuen Ordner **C:\Programme\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3.  Erstellen Sie in IIS eine neue Website namens „MIM Privileged Access Management Example Portal“ mit dem physischen Pfad „C:\Programme\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal“ und dem Port 8090.  Dies kann mithilfe der folgenden PowerShell-Befehle erreicht werden:

  ```
  New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
  ```

4.  Richten Sie die Beispielwebanwendung ein, damit Benutzer zur MIM PAM REST-API umgeleitet werden können. Bearbeiten Sie die Datei **C:\Programme\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config** in einem Text-Editor wie dem Editor von Windows. Fügen Sie im Abschnitt **<system.webServer>** die folgenden Zeilen hinzu:

  ```
  <httpProtocol>
    <customHeaders>
      <add name="Access-Control-Allow-Credentials" value="true"  />
      <add name="Access-Control-Allow-Headers" value="content-type" />
      <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
    </customHeaders>
  </httpProtocol>
  ```

5.  Konfigurieren Sie die Beispielwebanwendung. Bearbeiten Sie die Datei **C:\Programme\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js** in einem Text-Editor wie dem Editor von Windows. Legen Sie für **pamRespApiUrl** den Wert *http://pamsrv.priv.contoso.local:8086/api/pamresources/* fest.

6.  Starten Sie IIS mit dem folgenden Befehl neu, damit diese Änderungen wirksam werden.

  ```
  iisreset
  ```

7.  (Optional) Überprüfen Sie, ob sich der Benutzer für die REST-API authentifizieren kann. Öffnen Sie als Administrator einen Webbrowser auf PAMSRV.  Navigieren Sie zur Website-URL http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, authentifizieren Sie sich, falls erforderlich, und stellen Sie dann sicher, dass ein Download erfolgt.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>Installieren der MIM PAM-Requestor-Cmdlets

Installieren Sie die MIM PAM-Requestor-Cmdlets auf der Arbeitsstation, die Sie in Schritt 1 konfiguriert haben.

1.  Melden Sie sich bei „CORPWKSTN“ als Administrator an.

2.  Laden Sie die **Add-Ins und Erweiterungen** auf den Computer „CORPWKSTN“ herunter, sofern diese noch nicht vorhanden sind.

3.  Entpacken Sie den Ordner mit den **Add-Ins und Erweiterungen** aus dem Archiv in einen neuen Ordner.

4.  Führen Sie das Installationsprogramm **setup.exe**aus.

5.  Geben Sie für das benutzerdefinierte Setup an, dass der **PAM-Client** installiert werden soll. Das **MIM-Add-In für Outlook** oder die **MIM-Kennwort- und -Authentifizierungserweiterungen** sollen jedoch nicht installiert werden.

6.  Geben Sie für die PAM-Serveradresse als Hostnamen des PRIV-MIM-Servers *pamsrv.priv.contoso.local* an.

Nach Abschluss der Installation starten Sie „CORPWKSTN“ neu, um die Registrierung des neuen PowerShell-Moduls abzuschließen.

Im nächsten Schritt richten Sie eine Vertrauensstellung zwischen den Gesamtstrukturen PRIV und CORP ein.

>[!div class="step-by-step"]
[« Schritt 3](step-3-prepare-pam-server.md)
[Schritt 5 »](step-5-establish-trust-between-priv-corp-forests.md)
