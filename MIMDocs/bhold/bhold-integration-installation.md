---
title: Installation der BHOLD FIM/MIM-Integration | Microsoft-Dokumentation
description: Das BHOLD-Integrationsmodul fügt MIM und FIM eine Self-Service-Rollenverwaltung hinzu.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3005e06606ec4b3b6854003213c712770376b35d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042201"
---
# <a name="bhold-fimmim-integration-installation"></a>Installation der BHOLD FIM/MIM-Integration

Das BHOLD FIM-Integrationsmodul fügt Microsoft Identity Manager eine Self-Service-Rollenverwaltung hinzu. Dadurch können Benutzer zusätzliche Rollen anfordern. Außerdem kann erzwungen werden, wer diese Rollen übernehmen kann. Das BHOLD FIM-Integrationsmodul erweitert das FIM-Portal, um das Verwalten der Benutzerrollen als Teil der gesamten FIM-Verwaltung zu erleichtern. In diesem Thema wird beschrieben, wie Sie die Netzwerkinfrastruktur konfigurieren müssen, um das BHOLD FIM-Integrationsmodul installieren und verwenden zu können. Sie erfahren außerdem, wie Sie das BHOLD FIM-Integrationsmodul installieren können und welche Konfigurationen nach der Installation erforderlich sind.

## <a name="bhold-fim-integration-installation-requirements"></a>BHOLD FIM-Integration – Installationsanforderungen

Das BHOLD FIM-Integrationsmodul erweitert das FIM-Portal und den FIM-Dienst, damit Benutzer ihre eigenen Rollen im FIM-Portal verwalten können. Darum ist es wichtig, dass das BHOLD Core-Modul und die erforderlichen FIM-Funktionen installiert und konfiguriert sind, bevor Sie das BHOLD FIM-Integrationsmodul installieren.
Folgende Softwarekomponenten müssen auf dem Computer vorhanden sein, bevor Sie das BHOLD FIM-Integrationsmodul installieren können:

- Microsoft Identity Manager 2016 – Portal und Dienst
- Microsoft Silverlight 3 oder höher
- Internetinformationsdienste und ASP.NET
- Microsoft Silverlight-Tools

Darüber hinaus müssen die Module von BHOLD Core und dem Zugriffsverwaltungsconnector bereits auf einem Server in der Umgebung bereitgestellt sein, und FIM muss mit einem oder mehreren Verwaltungs-Agents für BHOLD konfiguriert sein. Weitere Informationen zum Installieren und Konfigurieren des BHOLD Core-Moduls finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Weitere Informationen zum Installieren und Verwenden des Moduls des Zugriffsverwaltungsconnectors finden Sie unter [Installation des Zugriffsverwaltungsconnectors](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) und [Testlaboranleitung: BHOLD-Zugriffsverwaltungsconnector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> Der Name der FIM-Dienstdatenbank muss FIMService lauten. Das Setup für die BHOLD FIM-Integration schlägt fehl, wenn FIM nicht mit dem Standardnamen für die FIM-Dienstdatenbank installiert wurde.

## <a name="before-you-begin"></a>Vorbereitung

Bevor Sie mit der Installation des BHOLD FIM-Integrationsmoduls beginnen, müssen Sie ein BHOLD-Verzeichnis im Stammverzeichnis des Laufwerks C erstellen (C:\BHOLD).

Darüber hinaus müssen Sie darauf vorbereitet sein, die Informationen bereitzustellen, die der Assistent für das Setup der BHOLD FIM-Integration erfordert, um die Installation abzuschließen. Das folgende Arbeitsblatt unterstützt Sie beim Aufzeichnen dieser Informationen, sodass Sie diese bereitstellen können, wenn sie benötigt werden.

### <a name="bholdfim-account-settings"></a>Kontoeinstellungen für BHOLD FIM

| **Element**                            | **Beschreibung**                                                                                                                                                                                                               | **Wert**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden eines Sicherheitsanbieters auf einer Domäne** | Bei der Auswahl dieser Option wird angegeben, dass die Active Directory Domain Services-Sicherheit den Zugriff auf BHOLD Core steuert.                                                                                                                    | Aktivieren Sie das Kontrollkästchen. **Wichtig:** Die Installation schlägt fehl, wenn dieses Kontrollkästchen nicht aktiviert ist.                                                                                                                                                                                                                   |
| **Domäne**                          | Gibt die Domäne an, die das **Dienstkonto** enthält, das Sie bei der Installation von BHOLD Core erstellt haben. Weitere Informationen finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Der Domänenname wird automatisch vom Assistenten bereitgestellt. Ändern Sie den Namen nur, wenn dieser falsch ist. **Wichtig:** Geben Sie den Domänennamen an, indem Sie den kurzen NetBIOS-Namen verwenden, nicht den vollqualifizierten Domänennamen (FQDN). Wenn der FQDN der Domäne beispielsweise „fabrikam.com“ ist, geben Sie den Domänennamen als „FABRIKAM“ an. |
| **Benutzername**                        | Gibt den Anmeldenamen des BHOLD Core-Dienstbenutzerkontos an.                                                                                                                                                              | Geben Sie den Benutzerkontonamen hier ein:                                                                                                                                                                                                                                                                                    |
| **Kennwort**                        | Gibt das Kennwort des Dienstbenutzerkontos an.                                                                                                                                                                           | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Einstellungen für den FIM-Dienst

| **Element**            | **Beschreibung**                                                                                                                                                                                                                               | **Wert**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Benutzer**            | Gibt den Anmeldenamen eines Kontos mit Administratorrechten für FIM an. Microsoft empfiehlt dringend, dass Sie nicht das Konto verwenden, das dem Stammbenutzer in BHOLD Core zugeordnet ist (standardmäßig das Konto, das zum Installieren von BHOLD Core verwendet wurde). | Geben Sie den Benutzerkontonamen hier ein:                                                                   |
| **Kennwort**        | Gibt das Kennwort des FIM-Administratorbenutzerkontos an.                                                                                                                                                                                 | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren. |
| **FIM-Datenbank**    | Gibt den Namen der FIM-Dienstdatenbank an.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/Port der Website** | Gibt den Namen oder die IP-Adresse des FIM-Portalservers und den Websiteport an.                                                                                                                                                               | Geben Sie den Servernamen oder die Serveradresse und den Port hier ein:                                                     |

### <a name="bhold-core-connection"></a>Verbindung mit BHOLD Core

| **Element**               | **Beschreibung**                                                                                                                                                                                                                                                                                                                                                                               | **Wert**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domäne**             | Gibt den Namen der Domäne des Kontos an, das in **Benutzer** angegeben wurde. Geben Sie die Domäne im kurzen NetBIOS-Format an.                                                                                                                                                                                                                                                                   | Geben Sie den Domänennamen des Benutzerkontos hier ein:                                                            |
| **Benutzer**               | Gibt den Anmeldenamen des Kontos **eines BHOLD-Benutzers an, der Supervisor** für alle Benutzer und Rollen ist und über die Berechtigung verfügt, Benutzerrollen zu verknüpfen und aufzuheben. Microsoft empfiehlt dringend, dass Sie nicht das Konto verwenden, das dem Stammbenutzer in BHOLD Core zugeordnet ist (standardmäßig das Konto, das zum Installieren von BHOLD Core verwendet wurde). Bei diesem Konto darf es sich um dasselbe Konto handeln, das Sie zum Herstellen einer Verbindung mit FIM verwenden. | Geben Sie den Benutzerkontonamen hier ein:                                                                   |
| **Kennwort**           | Gibt das Kennwort des Benutzerkontos an, das in **Benutzer** angegeben ist.                                                                                                                                                                                                                                                                                                                             | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren. |
| **IP-Adresse/absolute Adresse** | Gibt die IP-Adresse des BHOLD Core-Websiteservers an. Verwenden Sie nicht den Servernamen.                                                                                                                                                                                                                                                                                                        | Geben Sie die IP-Adresse hier ein:                                                                          |
| **Portnummer**        | Gibt die Portnummer der BHOLD Core-Website an.                                                                                                                                                                                                                                                                                                                                          | Geben Sie die Portnummer hier ein:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Setup für die BHOLD FIM-Integration

Melden Sie sich zum Installieren des BHOLD FIM-Integrationsmoduls als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das BHOLD FIM-Integrationsmodul installieren möchten:

- BholdFIMIntegration<em>\<Version\></em>\_Release.msi

Ersetzen Sie *\<Version\>* mit der Versionsnummer der Version der BHOLD FIM-Integration, die Sie installieren.

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

![Ausführung von MSI](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Aufgaben nach der Installation

Nach der Installation der BHOLD FIM-Integration müssen Sie Microsoft SharePoint dafür konfigurieren, dem BHOLD-Dienstkonto die Berechtigungen des Websitebesitzers zu erteilen. Wenn das FIM-Portal für das Verwenden der SSL-Sicherheit (Secure Sockets Layer) konfiguriert ist, müssen Sie die Dateien ändern, die Verweise auf die Adressen der BHOLD-Seiten enthalten, die dem FIM-Portal hinzugefügt sind.

### <a name="configuring-sharepoint"></a>Konfigurieren von SharePoint

Für eine ordnungsgemäße Funktionsweise erfordert die BHOLD FIM-Integration, dass das BHOLD-Dienstkonto über die Berechtigungen des Websitemitglieds in Microsoft SharePoint verfügt.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Erteilen der Berechtigungen des Websitemitglieds für das BHOLD-Dienstkonto

1.  Melden Sie sich bei dem Server, auf dem die BHOLD FIM-Integration ausgeführt wird, mit Administratorrechten an.

2.  Klicken Sie auf **Starten** und dann auf **Internet Explorer**.

3.  Geben Sie in der Adressleiste „<https://localhost>“ ein, wenn SharePoint für die Verwendung von SSL-Sicherheit konfiguriert ist. Geben Sie andernfalls „<http://localhost>“ ein.

4.  Klicken Sie auf der linken Seite der Seite **Teamwebsite** auf **Personen und Gruppen**.

5.  Klicken Sie unter **Gruppen** auf **Team Site Members** (Mitglieder der Teamwebsite), und klicken Sie auf der Symbolleiste in der Mitte auf **Neu** und dann auf **Benutzer hinzufügen**.

6.  Geben Sie auf der Seite **Add Users: Team Site** (Benutzer hinzufügen: Teamwebsite) in **Benutzer/Gruppen** „BHOLDApplicationGroup“ ein, und klicken Sie dann auf die Schaltfläche „Namen überprüfen“ unter dem Feld **Benutzer/Gruppen**. Der Gruppenname sollte sich in den Domänennamen auflösen.

7.  Klicken Sie auf **Give users permissions directly** (Benutzerberechtigungen direkt erteilen), wählen Sie **Full Control – Has full control** (Vollzugriff – Verfügt über Vollzugriff) aus, und klicken Sie dann auf **OK**.

8.  Stellen Sie sicher, dass „BHOLDApplicationGroup“ in **Permissions: Team Site** (Berechtigungen: Teamwebsite) aufgeführt ist, und schließen Sie dann Internet Explorer.

![Ausführung von MSI](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurieren von BHOLD für die SSL-Unterstützung

Wenn das FIM-Portal für die Verwendung der SSL-Sicherheit konfiguriert ist, müssen Sie Dateien auf dem FIM-Server so ändern, dass Links zu BHOLD-Seiten geöffnet werden. Die Dateien befinden sich in folgendem Ordner: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

In der folgenden Tabelle werden die Dateien und die ursprünglichen und geänderten Versionen der Zeichenfolgen aufgelistet, die bearbeitet werden müssen.

| **Datei**                  | **Ursprüngliche Zeichenfolge**                                                                                                                   | **Geänderte Zeichenfolge**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>* \>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Hierbei gilt Folgendes:

-   *\<BHOLD_Server\>* gibt den Namen des BHOLD-Servers wie in der ursprünglichen Version der Datei an.

-   *\<FIM_Server\>* gibt den Namen des FIM-Servers wie in der ursprünglichen Version der Datei an.

-   *\<BHOLD_Server_FQDN\>* gibt den vollqualifizierten Domänennamen (FQDN) des BHOLD-Servers an.

-   *\<FIM_Port\>* gibt den Portnamen des FIM-Servers wie in der ursprünglichen Version der Datei an.

-   *\<FIM_Server_FQDN\>* gibt den FQDN des FIM-Servers an.

-   *\<FIM_SSL_Port\>* gibt einen anderen Port für die Verwendung mit SSL auf dem FIM-Server an.

### <a name="enable-approval-workflows-in-bhold-core"></a>Aktivieren der Genehmigungsworkflows in BHOLD Core

Wenn FIM und BHOLD für den Self-Service integriert sind, werden die Workflows für Genehmigungen im FIM-Dienst ausgeführt. Dies entspricht dem Workflowmodell für Anforderungen, die aus dem FIM-Portal stammen, z.B. wenn ein Benutzer eine Anforderung übermittelt, um einer Verteilerliste beizutreten. Es gibt jedoch entscheidende Unterschiede zwischen den BHOLD-Rollenworkflows und anderen Workflows, die im FIM-Dienst gehostet werden. Im Fall von BHOLD stammen Workflowparameter, die angeben, welche Benutzer eine Rollenanforderung genehmigen müssen, von BHOLD, statt in den Workflowdefinitionen in der FIM-Dienstdatenbank gespeichert zu werden. Diese Parameter werden von BHOLD für den FIM-Dienst bereitgestellt, wenn die erste Anforderung erfolgt ist. Ein Aktionsworkflow übermittelt die Ergebnisse dann an BHOLD Core.

BHOLD wählt eine genehmigende Person für eine Self-Service-Anforderung auf eine von drei Arten:

-   **Vorgesetzter als genehmigende Person: rollenbasierte Auswahl für eine Organisationseinheit (OrgUnit)** Wenn die Rolle über ein Attribut namens „Rollentyp“ verfügt, das auf „genehmigende Person“ oder „Eskalationsverantwortlicher“ festgelegt ist, und wenn diese Rolle mit einem oder mehreren Benutzern im Kontext einer Organisationseinheit verknüpft ist, müssen Anforderungen von Benutzern in dieser Organisationseinheit von einem der Benutzer genehmigt werden, die mit dem Rollentyp der genehmigenden Person oder des Eskalationsverantwortlichen verknüpft sind.

-   **Vorgesetzter als genehmigende Person: attributbasierte Auswahl für eine Organisationseinheit** Jede Organisationseinheit kann über ein oder mehrere Attribute verfügen, die die Aliase der Benutzer angeben, die Rollenzuweisungen für andere Benutzer in der Organisationseinheit genehmigen können. Die Namen dieser Attribute lauten approver1, approver2 usw. Wenn ein Benutzer in der Organisationseinheit eine Rollenzuweisung anfordert, leitet BHOLD die Anforderung über FIM zu den Benutzern weiter, die in der Organisationseinheit durch die Attribute für genehmigende Personen angegeben sind. Wenn diese Attribute für eine Organisationseinheit nicht festgelegt sind, überprüft BHOLD die übergeordnete Organisationseinheit bis hin zur Stammorganisationseinheit.

-   **Rollenmanager als genehmigende Person: attributbasierte Auswahl für eine Rolle** Eine Rolle kann über ein oder mehrere Attribute (deren Namen ebenfalls approver1 usw. lauten) verfügen, die die Aliase von Benutzern angeben, die die Zuweisung der Rolle genehmigen können. Wenn ein Benutzer die Zuweisung zu einer Rolle anfordert, für die das Attribut für die genehmigende Person festgelegt ist, leitet BHOLD die Anforderung an die Benutzer weiter, die von diesen Attributen angegeben werden.

Wenn keine genehmigende Person für eine Self-Service-Rollenanforderungen durch eine dieser Methoden angegeben ist, weist BHOLD diese Rollen standardmäßig automatisch ohne Genehmigung zu. Deshalb sollten Sie direkt nach der Installation der BHOLD FIM-Integration die Stammorganisationseinheit mit dem Alias einer genehmigenden Person konfigurieren, z.B. mit dem Stammkonto. Dadurch wird verhindert, dass Benutzern unbeabsichtigt Rollen erteilt werden, bevor umfassendere Genehmigungsrichtlinien implementiert werden können.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Konfigurieren einer genehmigenden Person für die Stammorganisationseinheit

1.  Melden Sie sich beim BHOLD Core-Server als Administrator an.

2.  Klicken Sie auf **Starten** und dann auf **Internet Explorer**.

3.  Geben Sie in der Internet Explorer-Adressleiste „<http://localhost:5151/bhold/core>“ ein, und drücken Sie dann die Eingabetaste.

4.  Klicken Sie auf der BHOLD Core-Homepage unter **Attribute def** (Attributdefinition) auf **Attributtypen**.

5.  Klicken Sie auf der Seite **Attributtypen** auf **Hinzufügen**.

6.  Geben Sie auf der Seite **Add attribute type** (Attributtyp hinzufügen) unter **Identität** „approver1“ ein, klicken Sie in der Liste **Datentyp** auf **Alphanumerisch**, geben Sie unter **Maximale Länge** „255“ ein, und klicken Sie unter **Liste der Werte** auf **Nein**. Geben Sie dann unter **Englisch** „Approver 1“ ein, klicken Sie auf **OK** und dann auf **Fertig**.

7.  Klicken Sie im linken Bereich unter **Attribute def** (Attributdefinition) auf **Attribute type sets** (Attributtypmengen).

8.  Klicken Sie auf der Seite **Attribute type sets** (Attributtypmengen) auf **Hinzufügen**.

9.  Geben Sie auf der Seite **Add attribute type set** (Attributtypmenge hinzufügen) unter **Beschreibung** „OrgUnit-Attribute“ ein, und klicken Sie auf **OK**.

10. Erweitern Sie **Attributtypen** auf der Seite **OrgUnit Attributes** (OrgUnit-Attribute), und klicken Sie dann auf **Ändern**.

11. Klicken Sie in der Liste **Attributtypen** auf **approver1** > **Hinzufügen**, und klicken Sie dann auf **Fertig**.

12. Klicken Sie im linken Bereich auf **Objekttypen**.

13. Klicken Sie auf der Seite **Objekttypen** auf **OrgUnit**.

14. Erweitern Sie **Attribute type sets** (Attributtypmengen) auf der Seite **Objekttyp/OrgUnit**, und klicken Sie dann auf **Ändern**.

15. Geben Sie auf der Seite **Link attribute type set/OrgUnit** (Attributtypmenge verknüpfen/OrgUnit) unter **Reihenfolge** „10“ ein, klicken Sie in der Liste **Attribute type set** (Attributtypmenge) auf **OrgUnit Attributes** (OrgUnit-Attribute) > **Hinzufügen**, und klicken Sie dann auf **Fertig**.

16. Klicken Sie im linken Bereich unter **Modell** auf **Organisationseinheiten**.

17. Klicken Sie auf der Seite **Organisationseinheiten** auf **Stamm**.

18. Klicken Sie auf der Seite **Organizational unit/root** (Organisationseinheit/Stamm) auf **Ändern**.

19. Geben Sie auf der Seite **Modify organizational unit attributes/root** (Attribute/Stamm der Organisationseinheit ändern) unter **Genehmigende Person** die Domäne und den Benutzernamen des Benutzers, der die Anforderungen für Rollenzuweisungen genehmigt, im Format *\<Domäne\>* \\ *\<Benutzer\>* ein. Hierbei entspricht *\<Domäne\>* dem kurzen NetBIOS-Domänennamen und *\<Benutzer\>* dem Anmeldenamen des Benutzers.
20. Klicken Sie auf **OK**.

> [!IMPORTANT]
> Der Domänen- und der Benutzername müssen mit dem Standardalias eines Benutzers in der BHOLD Core-Datenbank übereinstimmen.

Alternativ zum Angeben einer genehmigenden Person für Organisationseinheiten können Sie auch eine genehmigende Person für die vorgeschlagenen Rollen in der BHOLD Core-Datenbank angeben. Erstellen Sie dazu das Attribut „approver1“, fügen Sie dieses zu einer Attributtypmenge hinzu, die dem Objekttyp der Rolle zugeordnet ist, und ändern Sie dann jede vorgeschlagene Rolle, um die genehmigende Person anzugeben.

Für eine verbesserte Workflowsicherheit sollten Sie zusätzlich zu einer genehmigenden Person Modi und Benutzer für die Genehmigung hinzufügen, indem Sie folgende Attribute für Organisationseinheiten und Rollen erstellen und füllen:

- escalator<em>\<n\></em>

- owner<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- notification<em>\<n\></em>

*\<n\>* gibt hier ein optionales numerisches Suffix an, um mehrere Attribute vom gleichen Typ bereitzustellen.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Überprüfen der Genehmigungsworkflows, die im FIM-Dienst konfiguriert wurden

Die Installation der BHOLD FIM-Integration erstellt Sätze, Workflowdefinitionen und Verwaltungsrichtlinien (Management Policy Rules, MPRs) für den FIM-Dienst. Wenn Sie Ihre FIM-Bereitstellung dafür angepasst haben, die Administratoren oder Benutzer zu ändern, die Anforderungen senden können, sollten Sie sicherstellen, dass die MPRs auf die richtigen Benutzer verweisen.

> [!NOTE]
> Bevor Benutzer des FIM-Portals die von BHOLD bereitgestellten Self-Service-Funktionen verwenden können, müssen deren Benutzerkonten über den FIM-Synchronisierungsdienst mit der BHOLD-Datenbank synchronisiert werden. Insbesondere muss ein Benutzerdatensatz in der BHOLD Core-Datenbank und in der FIM-Dienstdatenbank für jeden Benutzer vorhanden sein, der Self-Service-Anforderungen senden kann oder als genehmigende Person oder Eskalationsverantwortlicher für Self-Service-Anforderungen angegeben ist.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Installieren des FIM-Portals und anderen FIM-Funktionen finden Sie unter [Planning and Architecture (Planung und Architektur)](https://technet.microsoft.com/library/ee808044.aspx) in der technischen Bibliothek für Microsoft Forefront.
- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)
