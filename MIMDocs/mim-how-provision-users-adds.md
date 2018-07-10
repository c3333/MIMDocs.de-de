---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: Durchgehen des Einstellungsprozesses von Benutzern in AD DS mit Microsoft Identity Manager 2016
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: a12a8436d70b3ae866df0f615e10a3d76f791168
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290100"
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Bereitstellen von Benutzern in AD DS

Gilt für: Microsoft Identity Manager 2016 SP1 (MIM)

Eine grundlegende Anforderung für ein Identitätsverwaltungssystem ist die Fähigkeit zum Bereitstellen von Ressourcen an ein externes System.

Dieses Handbuch führt Sie durch die wichtigsten Bausteine, die während der Bereitstellung von Active Directory®-Domänendiensten (AD DS) für Benutzer von Microsoft® Identity Manager (MIM) 2016 beteiligt sind, und beschreibt, wie Sie überprüfen können, ob Ihr Szenario wie erwartet funktioniert. Außerdem werden Vorschläge für die Verwaltung von Active Directory-Benutzern mit MIM 2016 gemacht und zusätzliche Quellen für Informationen aufgeführt.

## <a name="before-you-begin"></a>Vorbereitungen


In diesem Abschnitt finden Sie Informationen zum Umfang dieses Dokuments. Im Allgemeinen richten sich die "Wie kann ich..."-Handbücher an Leser, die bereits Erfahrung mit dem Synchronisierungsprozess von Objekten mit MIM haben, wie es in den zugehörigen [Handbüchern mit ersten Schritten](http://go.microsoft.com/FWLink/p/?LinkId=190486) beschrieben ist.

### <a name="audience"></a>Zielgruppe


Dieses Handbuch ist für IT-Experten (information technology) gedacht, die bereits ein grundlegendes Verständnis der Funktionsweise des MIM-Synchronisierungsprozesses haben und an praktischer Erfahrung und konzeptionellen Informationen zu spezifischen Szenarien interessiert sind.

### <a name="prerequisite-knowledge"></a>Vorwissen


In diesem Dokument wird davon ausgegangen, dass Sie Zugriff auf eine ausgeführte Instanz von MIM und Erfahrung in der Konfiguration von einfachen Synchronisierungsszenarios haben, wie in den folgenden Dokumenten beschrieben:

-   [Einführung in die eingehende Synchronisierung](http://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Einführung in die ausgehende Synchronisierung](http://go.microsoft.com/FWLink/p/?LinkId=189653)

Der Inhalt in diesem Dokument ist als Erweiterung für die einführenden Dokumente gedacht.

### <a name="scope"></a>Bereich


Das in diesem Dokument beschriebene Szenario wurde vereinfacht, um die Anforderungen an eine grundlegende Testumgebung zu erfüllen. Der Fokus ist es, Ihnen einen Überblick über die erläuterten Konzepte und Technologien zu geben.

Dieses Dokument unterstützt Sie bei der Entwicklung einer Lösung, die die Verwaltung von Gruppen in AD DS mithilfe von MIM beinhaltet.

### <a name="time-requirements"></a>Zeitaufwand


Die Verfahren in diesem Dokument nehmen zwischen 90 und 120 Minuten in Anspruch.

Bei dieser Zeitschätzung wird davon ausgegangen, dass die Testumgebung bereits konfiguriert ist. Sie beinhaltet nicht den Zeitaufwand für die Einrichtung der Testumgebung.

### <a name="getting-support"></a>Unterstützung erhalten


Wenn Sie Fragen zum Inhalt dieses Dokuments oder allgemeines Feedback haben, über das Sie diskutieren möchten, senden Sie uns gerne eine Nachricht im [Forefront Identity Manager 2010-Forum](http://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Szenariobeschreibung


Fabrikam, ein fiktives Unternehmen, plant MIM zu verwenden, um die Benutzerkonten des Unternehmens in AD DS mithilfe von MIM zu verwalten. Als Teil dieses Prozesses muss Fabrikam Benutzer in AD DS bereitstellen. Fabrikam hat eine grundlegende Testumgebung installiert, die aus MIM und AD DS besteht, um die ersten Tests zu starten.
In dieser Testumgebung testet Fabrikam ein Szenario mit einem Benutzer, der im MIM-Portal manuell erstellt wurde. Dieses Szenario soll den Benutzer als einen aktivierten Benutzer mit einem vordefinierten Kennwort in AD DS bereitstellen.

## <a name="scenario-design"></a>Szenarioentwurf


Sie benötigen drei Komponenten, um dieses Handbuch verwenden zu können:

-   Active Directory-Domänencontroller

-   einen Computer mit dem FIM-Synchronisierungsdienst
-   einen Computer mit dem FIM-Portal

Die folgende Abbildung beschreibt die erforderliche Umgebung.

![Erforderliche Umgebung](media/how-provision-users-adds/image001.png)


Sie können alle Komponenten auf einem Computer ausführen.

> [!NOTE]
> Weitere Informationen zum Einrichten von MIM finden Sie unter [FIM Installation Guide (FIM-Installationshandbuch)](http://go.microsoft.com/FWLink/p/?LinkId=165845).

## <a name="scenario-components-list"></a>Liste der Szenariokomponenten


Die folgende Tabelle enthält die Komponenten, die Teil des Szenarios in diesem Handbuch sind.

| ![Organisationseinheit](media/how-provision-users-adds/image005.jpg)   | Organisationseinheit                | MIM-Objekte: Organisationseinheit (OU), die für die bereitgestellten Benutzer als Ziel verwendet wird                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Benutzerkonten](media/how-provision-users-adds/image006.jpg)   | Benutzerkonten                      | &#183; **ADMA**: Active Directory-Benutzerkonto mit ausreichenden Rechten zur Verbindung mit AD DS<br/> &#183; **FIMMA**: Active Directory-Benutzerkonto mit ausreichenden Rechten zur Verbindung mit MIM.
                                                                 |
| ![Verwaltungs-Agents und Ausführungsprofile](media/how-provision-users-adds/image007.jpg)  | Verwaltungs-Agents und Ausführungsprofile | &#183; **Fabrikam ADMA** : Verwaltungs-Agent, der Daten mit AD DS austauscht <br/> &#183; Fabrikam FIMMA: Verwaltungs-Agent, der Daten mit MIM austauscht                                                                                 |
| ![Synchronisierungsregeln](media/how-provision-users-adds/image008.jpg)  | Synchronisierungsregeln              | Ausgehende Synchronisierungsregel der Fabrikam-Gruppe: Ausgehende Regel, die Benutzern AD DS bereitstellt.                                     |
| ![Festlegungen](media/how-provision-users-adds/image009.jpg)   | Festlegungen                               | Alle Vertragsnehmer: Mit der dynamischen Mitgliedschaft für alle Objekte mit dem Attributwert EmployeeType des Vertragsnehmers festgelegt.                                |
| ![Workflows](media/how-provision-users-adds/image010.jpg)  | Workflows                          | AD-Bereitstellungsworkflow: Workflow, um den MIM-Benutzer im Bereich der ausgehenden Synchronisierungsregel von AD einzubinden                                |
| ![Management-Richtlinienregeln](media/how-provision-users-adds/image011.jpg)   | Management-Richtlinienregeln            | AD-Bereitstellung der Verwaltungsrichtlinienregel: Verwaltungsrichtlinienregel (MPR), die ausgelöst wird, wenn eine Ressource Member der Gruppe „Alle Vertragsnehmer“ wird. |
| ![MIM-Benutzer](media/how-provision-users-adds/image012.jpg) | MIM-Benutzer                          | Britta Simon: MIM-Benutzer, die Sie in AD DS bereitstellen                                                                                             |



## <a name="scenario-steps"></a>Szenarioschritte


Das in diesem Handbuch beschriebene Szenario besteht aus den folgenden Bausteinen.

![Szenarioschritte](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Konfigurieren der externen Systeme


In diesem Abschnitt finden Sie eine Anleitung für die Ressourcen, die Sie erstellen müssen und sich außerhalb Ihrer MIM-Umgebung befinden.

### <a name="step-1-create-the-ou"></a>Schritt 1: Erstellen Sie die Organisationseinheit


Sie benötigen die Organisationseinheit als Container für den bereitgestellten Beispielbenutzer. Weitere Informationen zum Erstellen von Organisationseinheiten finden Sie unter [Erstellen einer neuen Organisationseinheit](http://go.microsoft.com/FWLink/p/?LinkId=189655).

Erstellen Sie eine Organisationseinheit mit dem Namen „MIMObjects“ in Ihrer AD DS.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Schritt 2: Erstellen der Active Directory-Benutzerkonten

Für das Szenario in diesem Handbuch benötigen Sie zwei Active Directory-Benutzerkonten:

- **ADMA**: wird vom Active Directory-Verwaltungs-Agent verwendet

- **FIMMA**: wird vom FIM-Dienstverwaltungs-Agent verwendet

In beiden Fällen ist es ausreichend, normale Benutzerkonten zu erstellen. Weitere Informationen zu den spezifischen Anforderungen der beiden Konten finden Sie weiter unten in diesem Dokument. Weitere Informationen zum Erstellen von Benutzern finden Sie unter [Erstellen eines neuen Benutzerkontos](http://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Konfigurieren des FIM-Synchronisierungsdiensts


Für die Konfigurationsschritte in diesem Abschnitt müssen Sie den FIM-Synchronisierungsdienst-Manager starten.

### <a name="creating-the-management-agents"></a>Erstellen des Verwaltungs-Agents

Für das Szenario in diesem Handbuch müssen Sie zwei Verwaltung-Agents erstellen:

-   **Fabrikam ADMA**: Verwaltungs-Agent für AD DS

-   **Fabrikam FIMMA**: Verwaltungs-Agent für FIM-Dienstverwaltungs-Agent

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Schritt 3: Erstellen Sie den Fabrikam ADMA-Verwaltungs-Agent

Wenn Sie einen Verwaltungs-Agent für AD DS konfigurieren, müssen Sie ein Konto angeben, die vom Verwaltungs-Agent im Datenaustausch mit AD DS verwendet wird. Sie sollten ein normales Benutzerkonto verwenden. Zum Importieren von Daten aus AD DS muss das Konto jedoch über das Recht zum Abrufen von Änderungen aus dem DirSync-Steuerelement verfügen. Wenn Ihr Verwaltungs-Agent Daten in AD DS exportieren soll, müssen Sie dem Konto ausreichende Rechte für die Zielorganisationseinheiten gewähren. Weitere Informationen zu diesem Thema finden Sie unter [Configuring the ADMA Account (Konfigurieren des ADMA-Kontos)](http://go.microsoft.com/FWLink/p/?LinkId=189657).

Zum Erstellen eines Benutzers in AD DS müssen Sie den definierten Namen des Objekts auslaufen lassen. Darüber hinaus empfiehlt es sich, den Vornamen, Nachnamen und Anzeigenamen durchlaufen zu lassen, um sicherzustellen, dass Ihre Objekte erkannt werden.

In AD DS verwenden Benutzer immer noch häufig das Attribut SAMAccountName, um sich im Verzeichnisdienst anzumelden. Wenn Sie keinen Wert für dieses Attribut angeben, generiert der Verzeichnisdienst einen zufälligen Wert. Diese zufälligen Werte sind jedoch nicht benutzerfreundlich, daher ist eine benutzerfreundliche Version dieses Attributs in der Regel Teil des Exports in AD DS. Damit sich ein Benutzer in AD DS anmelden kann, müssen Sie auch ein Kennwort mit dem Attribut „unicodePwd“ in der Exportlogik erstellen.

> [!Note]
> Stellen Sie sicher, dass der Wert, den Sie als „unicodePwd“ angeben, den Kennwortrichtlinien des Ziel-AD DS entspricht.

Wenn Sie ein Kennwort für AD DS-Konten festlegen, müssen Sie auch ein Konto als ein aktiviertes Konto erstellen. Dies erreichen Sie, indem das Attribut „userAccountControl“ festlegen. Weitere Informationen zum Attribut „userAccountControl“ finden Sie unter [Using FIM to Enable or Disable Accounts in Active Directory (Aktivieren oder Deaktivieren von Konten in Active Directory mithilfe von FIM)](http://go.microsoft.com/FWLink/p/?LinkId=189658).

Die folgende Tabelle enthält die wichtigsten Einstellungen für bestimme Szenarios, die Sie konfigurieren müssen.

| Designer-Seite des Verwaltungs-Agents                          | Konfiguration                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Verwaltungs-Agent erstellen                                 | 1. **Verwaltungs-Agent für:** AD DS  <br/> 2.  **Name:** Fabrikam ADMA |
| Mit Active Directory-Gesamtstruktur verbinden                      | 1. **Verzeichnispartitionen auswählen:** „DC=Fabrikam,DC=com“   <br/>   2. Klicken Sie auf **Container**, um das Dialogfeld **Container auswählen** zu öffnen, und stellen Sie sicher, dass **MIMObjects** die einzige ausgewählte Organisationseinheit ist.        |
| Objekttypen auswählen                                     | Wählen Sie neben den bereits ausgewählten Objekttypen den **Benutzer** aus. |
| Attribute auswählen                                       | 1. Klicken Sie auf **Alle anzeigen.** <br/>   2. Wählen Sie die folgenden Attribute aus: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Weitere Informationen finden Sie unter den folgenden Themen in der Hilfe:
- Erstellen eines Verwaltungs-Agents
- Mit Active Directory-Gesamtstruktur verbinden
- Verwaltungs-Agent für Active Directory verwenden
- Verzeichnispartitionen konfigurieren

> [!Note]
> Stellen Sie sicher, dass Sie eine Attribut-Importflussregel für das Attribut „ExpectedRulesList“ konfiguriert haben.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Schritt 4: Erstellen Sie den Fabrikam FIMMA-Verwaltungs-Agent

Wenn Sie einen FIM-Dienstverwaltungs-Agent konfigurieren, müssen Sie ein Konto angeben, das vom Verwaltungs-Agent im Datenaustausch mit dem FIM-Dienst verwendet wird.

Sie sollten ein normales Benutzerkonto verwenden. Bei dem Konto muss es sich um das gleiche Konto handeln wie das, das während der Installation von MIM angegeben wurde. Ein Skript, das Sie verwenden können, um den Namen des FIMMA-Kontos zu bestimmen, das Sie während des Setups angegeben haben, und um zu prüfen, ob dieses Konto noch gültig ist, finden Sie unter: Verwenden von Windows PowerShell, um einen [FIM MA Account Configuration Quick Test (Konfigurationsschnelltest des FIMMA-Kontos)](http://go.microsoft.com/FWLink/p/?LinkId=189659) durchzuführen.

Die folgende Tabelle enthält die wichtigsten Einstellungen für bestimmte Szenarios, die Sie konfigurieren müssen. Erstellen Sie den Verwaltungs-Agent anhand der Informationen in der folgenden Tabelle.  

| Designer-Seite des Verwaltungs-Agents | Konfiguration |
|------------|------------------------------------|
| Verwaltungs-Agent erstellen | 1. **Verwaltungs-Agent für:** FIM-Dienstverwaltungs-Agent <br/> 2. **Name** Fabrikam FIMMA |
| Mit Datenbank verbinden     | Verwenden Sie die folgenden Einstellungen: <br/> &#183; **Server:** localhost <br/> &#183; **Datenbank:** FIMService <br/> &#183; **FIM-Dienstbasisadresse:** http://localhost:5725 <br/> <br/> Bereitstellen der Informationen zum Konto, das Sie für diesen Verwaltungs-Agent erstellt haben |
| Objekttypen auswählen                                     | Wählen Sie neben den bereits ausgewählten Objekttypen die **Person** aus.   |
| Objekttypzuordnungen konfigurieren                          | Fügen Sie zusätzlich zu den bereits vorhandenen Objekttypzuordnungen eine Zuordnung für die Person **Objekttyp der Datenquelle** zur Person des Objekttyps **Metaverse** hinzu. |
| Attributfluss konfigurieren                                | Fügen Sie zusätzlich zu den bereits vorhandenen Attributflusszuordnungen die folgenden Attributflusszuordnungen hinzu: <br/><br/> ![Attributfluss](media/how-provision-users-adds/image018.jpg) |




Weitere Informationen finden Sie unter den folgenden Themen in der Hilfe:
-   Erstellen eines Verwaltungs-Agents

-   Mit Active Directory-Datenbank verbinden

-   Verwaltungs-Agent für Active Directory verwenden

-   Verzeichnispartitionen konfigurieren

> [!NOTE]
>  Stellen Sie sicher, dass Sie eine Attribut-Importflussregel für das Attribut „ExpectedRulesList“ konfiguriert haben.

### <a name="step-5-create-the-run-profiles"></a>Schritt 5: Erstellen Sie die Ausführungsprofile

Die folgende Tabelle enthält die Ausführungsprofile, die Sie für das Szenario in diesem Handbuch erstellen müssen.

| Verwaltungs-Agent  | Ausführungsprofil     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1. Vollständiger Import  <br/> 2. Vollständige Synchronisierung <br/> 3. Deltaimport <br/> 4. Deltasynchronisierung <br/> 5. Exportieren                                                                    |
| Fabrikam FIMMA   | 1. Vollständiger Import <br> 2. Vollständige Synchronisierung <br/> 3. Deltaimport <br/> 4. Deltasynchronisierung <br/> 5. Exportieren|                                                                                                                                                                                   

Erstellen Sie Ausführungsprofile für jeden Verwaltungs-Agent gemäß der vorherigen Tabelle.


> [!Note]
> Weitere Informationen finden Sie unter der Hilfe zum Erstellen eines Ausführungsprofils des Verwaltungs-Agents in MIM.                                                                                                                  
> 
> 
> [!Important]
>  Stellen Sie sicher, dass die Bereitstellung in Ihrer Umgebung aktiviert ist. Sie können dies tun, indem Sie das Skript mithilfe von Windows PowerShell zur Aktivierung der Bereitstellung ausführen („http://go.microsoft.com/FWLink/p/?LinkId=189660)“).


## <a name="configuring-the-fim-service"></a>Konfigurieren des FIM-Diensts


Für das Szenario in diesem Handbuch müssen Sie eine Bereitstellungsrichtlinie konfigurieren, wie in der folgenden Abbildung dargestellt.

![Bereitstellungsrichtlinie](media/how-provision-users-adds/image019.png)

Das Ziel dieser Bereitstellungsrichtlinie ist es, Gruppen im Bereich der ausgehenden Synchronisierungsregel von AD-Benutzern einzubinden. Durch das Einbinden der Ressource in den Bereich der Synchronisierungsregel ermöglichen Sie der Synchronisierungs-Engine, Ihre Ressource in AD DS gemäß Ihrer Konfiguration bereitzustellen.

Navigieren Sie im Windows Internet Explorer® zu „http://localhost/identitymanagement“, um den FIM-Dienst zu konfigurieren. Navigieren Sie zum Erstellen der Bereitstellungsrichtlinie auf der Seite des MIM-Portals zu den verknüpften Seiten aus dem Abschnitt „Verwaltung“. Sie sollten das Skript in [Using Windows PowerShell to document your provisioning policy configuration (Mithilfe von Windows PowerShell Ihre Bereitstellungsrichtlinienkonfiguration dokumentieren)](http://go.microsoft.com/FWLink/p/?LinkId=189661) ausführen, um die Konfiguration zu überprüfen.

### <a name="step-6-create-the-synchronization-rule"></a>Schritt 6: Erstellen Sie die Synchronisierungsregel

In den folgenden Tabellen wird die Konfiguration der erforderlichen Fabrikam-Synchronisierungsregel für die Bereitstellung gezeigt. Erstellen Sie die Synchronisierungsregel anhand der Daten in den folgenden Tabellen.

| Konfiguration der Synchronisierungsregel                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Name                                                                                                       | Ausgehende Active Directory-Benutzersynchronisierungsregel                         |                                                          
| Beschreibung                                                                                               |                                                                             |                                                           
| Vorrang                                                                                                | 2                                                                           |                                                           
| Datenflussrichtung   | Ausgehend             |       
| Abhängigkeit       |         |                                         


| Bereich |                                                                             |                                                           
|--------|-------|
| Metaverseressourcentyp | person |                                                         
| Externes System                   |Fabrikam ADMA                                                               |                                                       
| Externer Systemressourcentyp                                                                              | user      



| Relationship ||
|------------|---------|
| Ressource im externen System erstellen                                                                         | True                                                                        |                                                           
| Aufheben der Bereitstellung aktivieren                                                                                      | False                                                                       |                                                           

| Beziehungskriterien                                                                                      | |
|------------|----------|
| ILM-Attribut     | Datenquellenattribut                                                       |
| Datenquellenattribut         | sAMAccountName    |

| Erste ausgehende Attributflüsse        | |                                                             |
|-------------------|---------------------- |---------------|
| NULL-Werte zulassen                 | Ziel                                                                 | Quelle                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **Konstante:** 512                                         |
| false                                                                     | unicodePwd                    | Konstante: P\@\$\$W0rd                                    |

| Permanente ausgehende Attributflüsse  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| NULL-Werte zulassen                                                                                                | Ziel                                                                 | Quelle                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



> [!NOTE]
>  Wichtig Stellen Sie sicher, dass Sie „Nur erster Fluss“ für den Attributfluss ausgewählt haben, der den definierten Namen als Ziel hat.                                                                          

### <a name="step-7-create-the-workflow"></a>Schritt 7: Erstellen des Workflows

Das Ziel des AD-Bereitstellungsworkflow ist das Hinzufügen der Fabrikam-Synchronisierungsregel für die Bereitstellung an eine Ressource. Die folgenden Tabellen zeigen die Konfiguration.  Erstellen Sie ein Workflow anhand der Daten in den folgenden Tabellen.

| Workflowkonfiguration               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Active Directory-Bereitstellungsworkflow für die Benutzer                     |
| Beschreibung                          |                                                                 |
| Workflowtyp                        | Aktion                                                          |
| Ausführen der Richtlinienaktualisierung                 | False                                                           |

| Synchronisierungsregel                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Ausgehende Active Directory-Benutzersynchronisierungsregel             |
| Aktion                               | Hinzufügen                                                             |




### <a name="step-8-create-the-mpr"></a>Schritt 8: Erstellen Sie die MPR

Die erforderliche MPR ist vom Typ „Listenübergang“ und wird ausgelöst, wenn eine Ressource Mitglied der Gruppe „Alle Vertragsnehmer“ wird. Die folgenden Tabellen zeigen die Konfiguration.  Erstellen Sie eine MPR anhand der Daten in den folgenden Tabellen.

| MPR-Konfiguration                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Name                                 | Verwaltungsrichtlinienregel der AD-Benutzerbereitstellung                 |
| Beschreibung                          |                                                             |
| Typ                                 | Listenübergang                                              |
| Erteilen von Berechtigungen                   | False                                                       |
| Deaktiviert                             | False                                                       |

| Übergangsdefinition                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Übergangstyp                      | Übergang in                                               |
| Übergangsliste                       | Alle Vertragsnehmer                                             |

| Richtlinienworkflows                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Typ                                 | Aktion                                                      |
| Anzeigename                         | Active Directory-Bereitstellungsworkflow für die Benutzer                 |




## <a name="initializing-your-environment"></a>Ihre Umgebung initialisieren


Die Ziele der Initialisierungsphase lauten wie folgt:

-   Binden Sie die Synchronisierungsregel in die Metaverse ein.

-   Binden Sie die Active Directory-Struktur in den Active Directory-Connectorbereich ein.

### <a name="step-9-run-the-run-profiles"></a>Schritt 9: Führen Sie die Ausführungsprofile aus

Die folgende Tabelle führt die Ausführungsprofile auf, die Teil der Initialisierungsphase sind.  Führen Sie die Ausführungsprofile gemäß der folgenden Tabelle aus.

| Ausführen                                                                                                           | Verwaltungs-Agent                                      | Ausführungsprofil          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | Vollständiger Import          |
| 2                                                                                                             |                                                       | Vollständige Synchronisierung |
| 3                                                                                                             |                                                       | Exportieren               |
| 4                                                                                                             |                                                       | Deltaimport         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | Vollständiger Import          |
| 6                                                                                                             |                                                       | Vollständige Synchronisierung |



> [!NOTE]
> Sie sollten sicherstellen, dass Ihre ausgehende Synchronisierungsregel erfolgreich in die Metaverse projiziert wurde.

## <a name="testing-the-configuration"></a>Konfiguration testen


Ziel dieses Abschnitts ist der Test der tatsächlichen Konfiguration. So testen Sie die Konfiguration:

1.  Erstellen Sie einen Beispielbenutzer im FIM-Portal.

2.  Überprüfen Sie die Bereitstellungsvoraussetzungen des Beispielbenutzers.

3.  Stellen Sie den Benutzer in AD DS bereit.

4.  Überprüfen Sie, ob der Benutzer in AD DS vorhanden ist.

### <a name="step-10-create-a-sample-user-in-mim"></a>Schritt 10: Erstellen Sie einen Beispielbenutzer in MIM


Die folgende Tabelle listet die Eigenschaften des Beispielbenutzers auf. Erstellen Sie einen Beispielbenutzer gemäß den Daten in der folgenden Tabelle.

| Attribut                              | Wert                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Vorname                             | Britta                                                         |
| Nachname                              | Simon                                                          |
| Anzeigename                           | Britta Simon                                                   |
| Kontoname                           | BSimon                                                         |
| Domain                                 | Fabrikam                                                       |
| Mitarbeitertyp                          | Vertragsnehmer                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Die Bereitstellungsvoraussetzungen des Beispielbenutzers überprüfen


Wenn Sie den Beispielbenutzer in AD DS bereitstellen möchten, müssen zwei erforderliche Voraussetzungen berücksichtigt werden:

1.  Der Benutzer muss ein Mitglied der Gruppe „Alle Vertragsnehmer“ sein.

2.  Der festgelegte Benutzer muss sich im Bereich der ausgehenden Synchronisierungsregel befinden.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Schritt 11: Stellen Sie sicher, dass der Benutzer ein Mitglied von „Alle Vertragsnehmer“ ist

Öffnen Sie die Gruppe, und klicken Sie dann auf „Mitglieder anzeigen“, um zu überprüfen, ob der Benutzer ein Mitglied der Gruppe „Alle Vertragsnehmer“ ist.

![Überprüfen Sie, ob der Benutzer ein Mitglied aller Vertragsnehmer ist](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Schritt 12: Überprüfen Sie, ob sich der festgelegte Benutzer im Bereich der ausgehenden Synchronisierungsregel befindet

Öffnen Sie die Eigenschaftenseite des Benutzers, und überprüfen Sie das Attribut der Erwartungsregelliste auf der Registerkarte „Bereitstellung“, um zu überprüfen, ob sich der Benutzer im Bereich der Synchronisierungsregel befindet. Das Attribut der Erwartungsregelliste sollte den AD-Benutzer auflisten.

Ausgehende Synchronisierungsregel Der folgende Screenshot zeigt ein Beispiel des Attributs der Erwartungsregelliste.

![Status der Synchronisierungsregel](media/how-provision-users-adds/image023.jpg)

An diesem Punkt im Prozess ist der Status der Synchronisierungsregel „Ausstehend“. Dies bedeutet, dass die Synchronisierungsregel noch nicht auf den Benutzer angewendet wurde.



### <a name="step-13-synchronize-the-sample-group"></a>Schritt 13: Synchronisieren Sie die Beispielgruppe


Bevor Sie den ersten Synchronisierungszyklus für ein Testobjekt beginnen, sollten Sie den erwarteten Zustand des Objekts nach jedem Ausführungsprofil nachverfolgen, das Sie in einem Testplan ausführen. Ihr Testplan sollte neben dem allgemeinen Zustand des Objekts (Erstellt, Aktualisiert oder Gelöscht) auch die Attributwerte umfassen, die Sie erwarten.
Verwenden Sie den Testplan, um die Erwartungen des Testplans zu überprüfen. Wenn ein Schritt nicht die erwarteten Ergebnisse zurückgibt, fahren Sie nicht mit dem nächsten Schritt fort, bis Sie die Diskrepanz zwischen dem erwarteten und dem tatsächlichen Ergebnis behoben haben.

Sie können die Synchronisierungsstatistiken als ersten Indikator verwenden, um Ihre Erwartungen zu überprüfen. Wenn Sie z.B. erwarten, dass neue Objekte in einem Connectorbereich bereitgestellt werden, aber die Importstatistiken geben keine „Hinzufügungen“ zurück, gibt es offensichtlich etwas in Ihrer Umgebung, das nicht wie erwartet funktioniert.

![Synchronisierungsstatistiken](media/how-provision-users-adds/image024.jpg)

Während die Synchronisierungsstatistiken Ihnen erste Hinweise darauf geben können, ob das Szenario wie erwartet funktioniert, sollten Sie den Connectorbereich der Suche und die Suchfunktion der Metaverse für den Synchronisierungsdienst-Manager verwenden, um die erwarteten Attributwerte zu überprüfen.

So synchronisieren Sie die Benutzer in AD DS:

1.  Importieren Sie den Benutzer in den FIM-MA-Connectorbereich.

2.  Projizieren Sie den Benutzer in die Metaverse.

3.  Stellen Sie den Benutzer im Connectorbereich von Active Directory bereit.

4.  Exportieren Sie Statusinformationen in FIM.

5.  Exportieren Sie den Benutzer in AD DS.

6.  Bestätigen Sie die Erstellung des Benutzers.

Um diese Aufgaben durchzuführen, führen Sie die folgenden Ausführungsprofile aus.

| Verwaltungs-Agent | Ausführungsprofil  |
|------------------|--------------|
| Fabrikam FIMMA   | 1. Deltaimport <br/> 2. Deltasynchronisierung <br/> 3. Exportieren <br/> 4. Deltaimport |
| Fabrikam FIMMA   | 1. Exportieren <br/> 2. Deltaimport       |


Nach dem Import aus der FIM-Dienstdatenbank sind Britta Simon und das Objekt „ExpectedRuleEntry“, das Britta mit der ausgehenden Synchronisierungsregel von AD-Benutzern verbindet, im Connectorbereich des Fabrikam FIMMA aufgeführt. Wenn Sie Brittas Eigenschaften im Connectorbereich neben den Attributwerten, die Sie im FIM-Portal konfiguriert haben, überprüfen, finden Sie auch einen gültigen Verweis auf das Objekt des Erwartungsregeleintrags. Der folgende Screenshot zeigt ein Beispiel für diesen Vorgang.

![Connector-Space-Objekteigenschaften](media/how-provision-users-adds/image025.jpg)

Das Ziel der auf dem Fabrikam FIMMA ausgeführten Deltasynchronisierung ist es, mehrere Vorgänge auszuführen:

-   Projektion: Das neue Benutzerobjekt und das verbundene Objekt des Erwartungsregeleintrags werden in die Metaverse projiziert.

-   Bereitstellung: Das neu geplante Objekt „Britta Simon“ wird im Connectorbereich vom Fabrikam ADMA bereitgestellt.

-   Attribut-Exportflüsse: Attribut-Exportflüsse treten in beiden Verwaltungs-Agents auf. Für den Fabrikam ADMA wird das neu bereitgestellte Objekt „Britta Simon“ mit neuen Attributwerten aufgefüllt. Auf dem Fabrikam FIMMA werden das vorhandene Objekt „Britta Simon“ und das verbundene Objekt „ExpectedRuleEntry“ mit Attributwerten aktualisiert, die ein Ergebnis der Projektion sind.

![Synchronisierungsstatistiken](media/how-provision-users-adds/image026.jpg)

Wie bereits von den Synchronisierungsstatistiken angegeben, gab es eine Bereitstellungsaktivität im Connectorbereich vom Fabrikam ADMA. Wenn Sie die Metaverse-Objekteigenschaften von Britta Simon überprüfen, sehen Sie, dass diese Aktivität ein Ergebnis des Attributs „ExpectedRulesList“ ist, das mit einem gültigen Verweis aufgefüllt wurde.

![Metaverse-Objekteigenschaften](media/how-provision-users-adds/image027.jpg)

Während des folgenden Exports auf dem Fabrikam FIMMA wird der Status der Synchronisierungsregel von Britta Simon von „Ausstehend“ auf „Angewendet“ aktualisiert. Dies gibt an, dass die ausgehende Synchronisierungsregel für das Objekt im Metaverse aktiviert wurde.

![Angewendete Synchronisierungsregel](media/how-provision-users-adds/image028.jpg)

Da ein neues Objekt im Bereich des Connectorbereichs vom ADMA bereitgestellt wurde, sollten Sie über einen ausstehenden Export für Hinzufügungen auf diesem Verwaltungs-Agent verfügen. Indem Sie ein Skript für diesen Zweck verwenden, wird ein gemeldeter, ausstehender Export für Hinzufügungen für den Fabrikam ADMA angezeigt. Weitere Informationen zur Verwendung des Skripts finden Sie unter [Using Windows PowerShell to Display the Export State of a Management Agent (Anzeigen des Exportstatus eines Verwaltungs-Agents mithilfe von Windows PowerShell)](http://go.microsoft.com/FWLink/p/?LinkId=189664).

![Ausstehende Exporte für den Verwaltungs-Agent](media/how-provision-users-adds/image029.jpg)

In FIM benötigt jede Exportausführung einen Deltaimport für den Abschluss des Exportvorgangs. Der Deltaimport, den Sie nach einer vorherigen Exportausführung durchführen, ist als bestätigender Import bekannt. Bestätigende Importe sind erforderlich, um den FIM-Synchronisierungsdienst zu aktivieren, sodass während der aufeinander folgenden Synchronisierungsausführung entsprechende Updateanforderungen vorgenommen werden.


Führen Sie die Ausführungsprofile gemäß den Anweisungen in diesem Abschnitt aus.

> [!IMPORTANT]
> Jede Ausführung von Ausführungsprofilen muss ohne Fehler erfolgen.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Schritt 14: Überprüfen Sie die bereitgestellten Benutzer in AD DS

Öffnen Sie die Organisationseinheit „FIMObjects“, um sicherzustellen, dass Ihr Beispielbenutzer in AD DS bereitgestellt wurde. Britta Simon sollte sich in der Organisationseinheit „FIMObjects“ befinden.

![Überprüfen, ob der Benutzer in der Organisationseinheit FIMObjects ist](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>Zusammenfassung
=======

Das Ziel dieses Dokuments ist es, Ihnen die wichtigsten Bausteine für die Synchronisierung eines Benutzers in MIM mit AD DS vorzustellen. Während der ersten Testphase sollten Sie zunächst mit einem Minimum an Attributen beginnen, die zum Durchführen einer Aufgabe nötig sind, und weitere Attribute für Ihr Szenario hinzufügen, wenn die allgemeinen Schritte erwartungsgemäß funktionieren. Wenn Sie die Komplexität auf einem Mindestlevel halten, vereinfacht dies die Problembehandlung.

Wenn Sie Ihre Konfiguration testen, ist es sehr wahrscheinlich, dass Sie neue Testobjekte löschen und neu erstellen. Bei Objekten mit einem

aufgefüllten Attribut ExpectedRulesList können dadurch verwaiste ERE-Objekte entstehen.
Eine Beschreibung, wie Sie diese Objekte aus Ihrer Testumgebung entfernen können, finden Sie unter [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment (Eine Methode zum Entfernen von verwaisten ExpectedRuleEntry-Objekten aus Ihrer Umgebung)](http://go.microsoft.com/FWLink/p/?LinkId=189667).

In einem typischen Szenario mit Synchronisierung, die AD DS als Synchronisierungsziel enthält, ist MIM nicht autoritativ für alle Attribute eines Objekts. Wenn Sie z.B. Benutzerobjekte in AD DS mithilfe von FIM verwalten, müssen zumindest die Domäne und die Attribute „objectSID“ durch den Verwaltungs-Agent von AD DS bereitgestellt werden.
Die Attribute des Kontonamens, die Domänenattribute und die Attribute „objectSID“ sind erforderlich, damit sich ein Benutzer im FIM-Portal anmelden kann. Es ist eine weitere eingehende Synchronisierungsregel für den AD DS-Connectorbereich erforderlich, um diese Attribute aus AD DS zu füllen. Beim Verwalten von Objekten mit mehreren Quellen für Attributwerte müssen Sie sicherstellen, dass Sie die Attributflussrangfolge ordnungsgemäß konfigurieren. Wenn die Attributflussrangfolge nicht ordnungsgemäß konfiguriert ist, verhindert die Synchronisierungs-Engine das Auffüllen der Attributwerte. Weitere Informationen zur Attributflussrangfolge finden Sie im Artikel [About Attribute Flow Precedence (Informationen zur Attributflussrangfolge)](http://go.microsoft.com/FWLink/p/?LinkId=189675).

<a name="see-also"></a>Weitere Informationen
=========

<a name="other-resources"></a>Weitere Ressourcen
---------------

[Using FIM to Enable or Disable Accounts in Active Directory (Aktivieren oder Deaktivieren von Konten in Active Directory mithilfe von FIM)](http://go.microsoft.com/FWLink/p/?LinkId=189670)

[About Reference Attributes (Informationen zu Verweisattributen)](http://go.microsoft.com/FWLink/p/?LinkId=189671)

[How Can I Manage My FIM MA Account (Wie kann ich mein FIMMA-Konto verwalten)](http://go.microsoft.com/FWLink/p/?LinkId=189672)

[Detecting Nonauthoritative Accounts – Part 1: Envisioning (Erkennen von nicht autoritativen Konten. Teil 1: Entwurf)](http://go.microsoft.com/FWLink/p/?LinkId=189673)

[The Poor Man’s Version of a Connector Detection Mechanism (Ein Connector-Erkennungsmechanismus für Arme)](http://go.microsoft.com/FWLink/p/?LinkId=189674)

[Configuring the ADMA Account (Konfigurieren des ADMA-Kontos)](http://go.microsoft.com/FWLink/p/?LinkId=189657)

[A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment (Eine Methode zum Entfernen von verwaisten ExpectedRuleEntry-Objekten aus Ihrer Umgebung)](http://go.microsoft.com/FWLink/p/?LinkId=189667)

[About Attribute Flow Precedence (Informationen zur Attributflussrangfolge)](http://go.microsoft.com/FWLink/p/?LinkId=189675)

[About Exports (Informationen zu Exporten)](http://go.microsoft.com/FWLink/p/?LinkId=189676)
