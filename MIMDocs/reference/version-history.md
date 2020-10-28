---
title: Versionsverlauf von Identity Manager | Microsoft-Dokumentation
description: In diesem Artikel werden die verschiedenen Änderungen erläutert, die im Rahmen der Updates für MIM 2016 vorgenommen wurden.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fa4e5ade14c1a3df94b868d149472c1b42d1c59c
ms.sourcegitcommit: d6178a67014d66d37056c13d10328ae03e3cd781
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "92761037"
---
# <a name="identity-manager-version-release-history"></a>Versions Veröffentlichungs Verlauf von Identity Manager

Das Microsoft Identity Manager-Team veröffentlicht regelmäßig Updates. Dieser Artikel soll Ihnen dabei behilflich sein, die bereits veröffentlichten Versionen nachzuverfolgen. Anschließend können Sie feststellen, ob Sie bereits auf dem neuesten Service Pack und der Hotfix-Update Version sind oder ob Sie ein Upgrade durchführen müssen. 

>[!NOTE]
>In diesem Artikel werden nur Versionen der MIM-Synchronisierungs-, Dienst-und Portal-, Client-, cm-und PAM-Komponenten beschrieben  Sie sind nicht sicher, welche Komponenten Sie benötigen?  Weitere Informationen finden Sie unter [Microsoft Identity Manager Lizenzierung und Downloads](../microsoft-identity-manager-licensing.md).
>
>Den Versionsverlauf für Microsoft BHOLD Suite-Komponenten finden Sie im [Versionsverlauf der BHOLD-Module](version-bhold-history.md).
>
>Der Versionsverlauf für allgemeine LDAP-, generische SQL-, Webdienste-, PowerShell-, Graph-und Lotus Domino-Connectors finden Sie im [Versionsverlauf der Connector-Version](microsoft-identity-manager-2016-connector-version-history.md).  

## <a name="mim-version-462630"></a>MIM-Version 4.6.263.0
- Status: 7. August, 2020
- [Hotfix-Download](https://www.microsoft.com/download/details.aspx?id=101612)
- [KB-Artikel 4576473](https://support.microsoft.com/help/4576473)

Dieser Hotfix enthält Updates für die MIM cm, den MIM-Synchronisierungs-Manager und die PAM-Komponenten.


## <a name="mim-version-462580"></a>MIM-Version 4.6.258.0
- Status: 22. Mai 2020
- [Hotfix-Download](https://www.microsoft.com/download/details.aspx?id=101305)
- [KB-Artikel 4560335](https://support.microsoft.com/help/4560335)

Dieser Hotfix enthält Updates für den MIM-Dienst, das MIM-Portal und die PAM-Komponenten.


## <a name="mim-version-46340"></a>MIM-Version 4.6.34.0
* Status: MIM 2016 Service Pack 2 (SP2) vom Oktober, 2019
* Zugehörige BHOLD-Versionsnummer: 6.0.62.0
- [Hotfix-Download](https://www.microsoft.com/download/details.aspx?id=100412)
- [KB-Artikel 4512924](https://support.microsoft.com/help/4512924)
- Die ISO-Datei kann [aus dem VLSC](https://www.microsoft.com/Licensing/servicecenter/default.aspx) oder über [Visual Studio-Downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202) heruntergeladen werden.

> [!IMPORTANT]
>- .NET Framework 4,6 ist erforderlich.<br>
>- [Visual C++ 2013 Redistributable Package (vcredist_x64.exe oder vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784) ist erforderlich.<br>

>[!NOTE]
> Lesen Sie unbedingt das [MIM-Bereitstellungs Handbuch](../microsoft-identity-manager-deploy.md) für eine aktualisierte Liste der Voraussetzungen und Einschränkungen im Zusammenhang mit reinen TLS 1,2-Umgebungen, Unterstützung für Gruppen verwaltete Dienst Konten und [Upgradepfad von vorherigen MIM-und FIM-Versionen](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md).

Ein Service Pack 2 (SP2) Rollup-Paket (Build 4.6.34.0) ist für Microsoft Identity Manager (MIM) 2016 verfügbar. Dabei handelt es sich um ein kumulatives Update, das frühere MIM 2016 SP1-Updates 4.4.1302.0 durch Build 4.5.412.0 ersetzt.
### <a name="updates-in-mim-2016-service-pack-2"></a>Updates in MIM 2016 Service Pack 2

#### <a name="mim-client-addons"></a>MIM-Client-Addons
- Unterstützung für MIM Outlook-Add-on, das in Outlook geladen werden soll, für Microsoft 365 Click-to-Run-Version hinzugefügt.

#### <a name="service-and-portal"></a>Dienst und Portal
- Unterstützung für MIM-Dienst und-Portal zur Installation unter Windows Server 2019 hinzugefügt und SQL Server 2017, Exchange Server 2019, SharePoint 2019 System Center Service Manager Data Warehouse 2019 verwendet.
- Aktivieren der MIM-Dienst-und-Portal-Installation in nur TLS 1,2-Umgebungen.
- Die Installation für die Websites MIM-Dienst, Kenn Wort Zurücksetzung und Kenn Wort Registrierung, PAM-Überwachungsdienst und PAM-Komponenten Dienst für die Verwendung von Gruppen verwalteten Dienst Konten wurde aktiviert.
- Der Installationsparameter "keepsqljobs" wurde hinzugefügt.
- MIM-SQL Server-Agent Temporale Aufträge werden nicht mehr auf sekundären SQL Always-On Verfügbarkeits Gruppen Replikaten gestartet.
- Die virtuellen Attribute ' explitmember. Add ' und ' explizmember. Remove ' sind für benutzerdefinierte Objekttypen in RCDC-Formularen aktiviert, um mit Delta Änderungen zu arbeiten.

#### <a name="synchronization-service"></a>Synchronisierungsdienst
- Unterstützung für die Installation des MIM-Synchronisierungs Dienstanbieter unter Windows Server 2019 und die Verwendung von SQL Server 2017, Exchange Server 2019
- Aktiviert die Installation des MIM-Synchronisierungs Dienstanbieter in TLS 1,2-Umgebungen
- Ermöglicht die Installation des MIM-Synchronisierungs Dienstanbieter für die Verwendung eines Gruppen verwalteten Dienst Kontos.
- Die Option "mimsync-Konto verwenden" wurde für den MIM-Dienstverwaltungs-Agent hinzugefügt.
 
#### <a name="privilege-access-management"></a>Zugriffs Verwaltung für Berechtigungen 
- Das PowerShell-Cmdlet "Get-pamrequest" gibt eine zusätzliche Eigenschaft "fmrequestid" zurück.


## <a name="mim-version-454120"></a>MIM-Version 4.5.412.0
> [!IMPORTANT]
>- RCDC-Formular für Gruppen Objekte kann nicht wieder hergestellt werden, wenn der Attribut Wert "Display Name" nicht aufgefüllt ist. Wenn ein solcher Fehler auftritt, können Sie keine Gruppen bearbeiten/anzeigen.<br>

- Verfügbarkeits Datum: 10. Mai 2019
- [Hotfix-Download](https://www.microsoft.com/en-us/download/details.aspx?id=58213)
- [KB-Artikel 4489646](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

Dieser Hotfix enthält Updates für die MIM-Synchronisierung, den MIM-Dienst, das MIM-Portal, MIM cm und PAM-Komponenten.  Dabei handelt es sich um ein kumulatives Update, das frühere MIM 2016 SP1-Updates 4.4.1302.0 durch Build 4.5.286.0 ersetzt.

> [!IMPORTANT]
>- .NET Framework 4,6 ist auch für das Installationsprogramm erforderlich. <br>
>- [Visual C++ 2013 x64 Redistributable Packages (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) ist erforderlich.<br>

## <a name="mim-hybrid-reporting-agent-version-31410"></a>MIM-Hybrid Reporting-Agent-Version 3.1.41.0
- Verfügbarkeits Datum: 22. April 2019
- [Hybrid Reporting-Agent-Download](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

MIM-Hybrid Reporting-Agent-Version 3.1.41.0 enthält Updates für TLS 1,2 und eine Fehlerbehebung.  Der Agent für die Hybrid Berichterstellung kann mit MIM 2016 SP1, Version 4.4.1302.0 oder höher, und einem Azure AD Premium Abonnement verwendet werden.


## <a name="mim-version-452860"></a>MIM-Version 4.5.286.0
- Status: 19. November 2018
- [Hotfix-Download](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [KB-Artikel 4469694](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


Dieser Hotfix enthält Updates für den MIM-Dienst, das MIM-Portal und die PAM-Komponenten.  Für das Installationsprogramm ist .NET Framework 4,6 erforderlich.


## <a name="version-452020"></a>Version 4.5.202.0
- Status: 30. August 2018
- [KB-Version KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- .NET Framework 4,6 ist auch für das Installationsprogramm erforderlich. <br>
>- [Visual C++ 2013 x64 Redistributable Packages (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) ist erforderlich.<br>
>- Aktualisierte unterstützte Gebiets Schemas in neue ISO-Standards ([hier](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Bezeichnet neue Erweiterung 

#### <a name="mim-service"></a>MIM service (MIM-Dienst)
- Azure MFA-Server Integration-alternativer Multi-Factor Authentication Anbieter

#### <a name="privilege-access-management"></a>Zugriffs Verwaltung für Berechtigungen 
- Die PAM-Rest-API konnte nicht gestartet werden, da die Datei oder Assembly nicht geladen werden konnte.

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal
- Das Portal wird mit einer falschen Tabellenlänge angezeigt.
- Dialogfeld "Erweiterte Suche" im Portal, die Scrollleisten werden nicht ordnungsgemäß angezeigt.
- Fehler beim Überprüfen der Signatur des Sprachpakets mit starkem Namen.

#### <a name="certificate-management"></a>Zertifikatverwaltung
- Binden der Redirect-Anweisung für die Rest-API

## <a name="version-45260"></a>Version 4.5.26.0
- Status: 30. Juni 2018
- [KB-Release KB4073679](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- .NET Framework 4,6 ist auch für das Installationsprogramm erforderlich. <br>
>- [Visual C++ 2013 x64 Redistributable Packages (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) ist erforderlich.<br>
>- Aktualisierte unterstützte Gebiets Schemas in neue ISO-Standards ([hier](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Bezeichnet neue Erweiterung 

#### <a name="synchronization-service"></a>Synchronisierungs Dienst
- * Unterstützung für Gruppen verwaltete Dienst Konten
- * Visual Studio-Support (Visual Studio 2013, Visual Studio 2015, Visual Studio 2017)
- Updates für MIISACTIVATE.EXE, GMSA-Unterstützung hinzugefügt 
    - nicht-GMSA: Miisactivate.exe c:\configbu\ miiserver_01. bin "condeso\mimsyncservice" *
    - GMSA: Miisactivate.exe c:\configbu\ miiserver_01. bin "condeso\mimsyncservice"
- Updates für MIISKMU.exe, GMSA-Unterstützung hinzugefügt 
    - non-gMSA:MIISKMU.exe/e c:\configbu\ miiserver_02. bin "/u:" condeso\mimsyncservice "
    - gMSA:MIISKMU.exe/e c:\configbu\ miiserver_02. bin "/u:" condeso\mimsyncservice "*
- Aktualisierte Partitionsinformationen werden erwartungsgemäß gespeichert, wenn auf die Schaltflächen aktualisieren und OK geklickt wird.
- Wenn die Indizierung eines indizierbaren Zeichen folgen Attributs zu lang ist, wurde ein unerwarteter Fehler zurückgegeben. nun wird eine beschreibende Fehlermeldung zurückgegeben
- Erstellen eines Textdatei-Verwaltungs-Agents wenn der MIM-Synchronisierungs Dienst unter Windows Server 2016 installiert ist, sind einige Text Codierungsoptionen, einschließlich Unicode, nicht verfügbar.
- MIM-Dienst-mA wenn eine Export Fehlermeldung ein ungültiges Zeichen enthält, führt dies zu Beschädigungen in den Einträgen für den Lauf Verlauf. Dieser Build wurde aus der Fehlermeldung entfernt, bevor er im Verbindungsraum Objekt und im Lauf Verlauf gespeichert wird.

#### <a name="mim-service"></a>MIM service (MIM-Dienst)
- * Unterstützung für Gruppen verwaltete Dienst Konten
- * Verbesserte Sprachunterstützung in neuen definierten Standard
- * Powermautomation Export-FIMConfig PowerShell-Cmdlet das "-pamconfig"-Argument ist verfügbar, um das Exportieren der PAM-Konfigurationsobjekte zu erzwingen.
- * Powermautomation Export-FIMConfig PowerShell-Cmdlet der Parameter "-Request" wurde hinzugefügt.
- * Boolesche Attribute werden bei der Bindungs Erstellung immer auf NULL festgelegt, vorheriger boolescher Wert, bevor der Hotfix nicht aktualisiert wird.
> [!IMPORTANT]
>Dies kann eine Breaking Change sein, wenn Sie eine Konfigurations Migration Vorbilden. Die Konfiguration sollte ausgewertet und für die neue Funktion aktualisiert werden, da die Konfigurations Migration als neu gilt. 
    - Beim Erstellen eines neuen Objekts wurde die Initialisierung neuer MIM boolescher Attribute in "false" implementiert.
    - Beim Hinzufügen einer neuen booleschen Attribut Bindung zur Ressource wurde die Initialisierung neuer MIM boolescher Attribute in "false" implementiert.
- Programm zur Verbesserung der Benutzerfreundlichkeit Einstellung auf "false" beibehalten 
- Fehler beim Installieren des MIM-Dienstanbieter: der Wert NULL kann nicht in die Spalte ' Name ' eingefügt werden, wenn kein standardmäßiger Datenbankname verwendet wird.
- In hotfixfällen wird die Microsoft 365 Einstellung gelöscht, das verschlüsselte Kennwort für das Exchange Online-Postfach des MIM-Dienstanbieter wird nicht geändert.
- * Es gibt keine Beschränkung für die erstellte MIM-Dienst Protokolldatei, die aktualisierte Standardeinstellung für die Protokollierung und die implementierte zirkulärprotokol

#### <a name="privileged-access-management"></a>Privileged Access Management (Schützen von Windows und Microsoft Azure Active Directory mit Privileged Access Management) 
- * Unterstützung für Gruppen verwaltete Dienst Konten
- * Verbesserte Sprachunterstützung in neuen definierten Standard
- Objekte, die nicht verwaltete Ressourcen verwenden, werden nicht rechtzeitig gelöscht.  Diese Objekte werden ordnungsgemäß bereinigt.
- * New-pamrole PowerShell-Cmdlet "-disableautoapproveifowner" verweigert die selbst Genehmigung für die Rolle.
- * Get-PamRequest PowerShell-Cmdlet "-kreatedfrom" die Filterung der PAM-spezifischen Anforderung ermöglicht
- * PAM-Modul Ergänzungen
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- Die Warnung (Ausnahme: System. ObjectDisposedException: kein Zugriff auf ein frei gegebenes Objekt) wird nicht mehr im PAM-Ereignisprotokoll angezeigt.
- Set-PAMUser Cmdlet kann den privaccountname ohne den Löschvorgang ändern.
- New-PamRole jetzt überprüft, ob das Datum "verfügbar für" größer als das Datum "verfügbar ab" ist.
- Die "available from"-und "available for"-Werte werden vom Get-PAMRole PowerShell-Cmdlet zurückgegeben.
- Der Get-PamRequest Cmdlet-Filter ist nun ordnungsgemäß
- * Das Cmdlet "Set-pamgroup" kann jetzt das Active Directory Schatten Prinzipal Gruppen Objekt aktualisieren.
- Remove-PamUser PowerShell-Cmdlet mit einer unklaren Fehlermeldung fehlschlägt, wenn der Benutzer mit einer Rolle als Kandidat verknüpft ist. Nun wurde die Client seitige Validierung zum Cmdlet hinzugefügt, und die zurückgegebene Ausnahme wurde verdeutlicht.
- Der Änderungs Modus für PAM-Konten ist für die Konfiguration nicht verfügbar.
    - PAM-Rest-API-Konto
    - PAM-Komponenten Dienst Konto
    - PAM-Überwachungsdienst Konto

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal
- * Unterstützung für Gruppen verwaltete Dienst Konten
- * Verbesserte Sprachunterstützung in neuen definierten Standard
- Steuerelement für die Identitäts Auswahl: das Steuerelement scheint seine Breite dynamisch zu vergrößern, anstatt den Text zu umwickeln.
- Portal: Popup Dialogfelder werden bei der Anzeige in Internet Explorer (IE) 10 nicht ordnungsgemäß angezeigt.
- Kyrillische Symbole im Text der Titelleiste werden korrekt angezeigt.
- Bei Popup Fenstern wird die zusätzliche Bild Lauf Leiste nicht mehr angezeigt, wenn Sie in Internet Explorer angezeigt wird.
- Der Fehler "Workflow Definition importieren" löst ordnungsgemäß eine Ausnahme aus und stellt eine Ausnahme dar, sodass der Workflow Definition eine Synchronisierungs Regel Aktivität hinzugefügt werden kann.
- <httpRuntime enableVersionHeader="false" /> zu Standard web.config hinzugefügt
- Sonderzeichen in "Unterer Name" verhindern nicht mehr, dass Self-Service Kenn Wort Zurücksetzung das Kennwort des Benutzers im Active Directory zurücksetzt.
- Die Verbesserungen in den Sätzen sind in der Anzeige ordnungsgemäß lokalisiert.
- MIM-Add-in für Outlook enthält eine Kopie der fehlenden Outlook-Interop-Binärdateien.

#### <a name="certificate-management"></a>Zertifikatverwaltung
- Erneuern einer virtuellen Smartcard über die MIM cm moderne APP, der Benutzer erhält eine unzulässige Ausnahme
- * Verbesserte Sprachunterstützung in neuen definierten Standard
- Beim Versuch, die Smartcard-PIN zu ändern, wurde vom Pin-Hilfsprogramm "CLM ein Fehler aufgetreten.  Falsche Anzahl von Argumenten oder ungültige Eigenschaften Zuweisung. "
- Aktualisieren der MIM-Zertifizierungsstellen Module von 4.4.1302.0 auf einen Build, der höher als 4.4.1459 ist, schlägt das Setup fehl.
- Moderne App für Renew-, Registrierungs-und Ersetzungs Vorgänge enthält der Anforderungs Verlauf nicht alle Anforderungs Status Elemente, die aufgezeichnet werden.
- Das Online Update wird nicht abgeschlossen, und es wird die Ausnahme "der Datensatz wurde von einem anderen Benutzer aktualisiert oder gelöscht" zurückgegeben.  
- Der Link "Zertifikat herunterladen" im Zertifikat Verwaltungsportal, das Herunterladen des Zertifikats (CER-Datei) war zu groß.
- Der MIM-Zertifikat Verwaltungs-Massen Client funktioniert sowohl mit TLS 1,1 als auch mit TLS 1,2.  

 
## <a name="version-4417490"></a>Version 4.4.1749.0

- Status: 30. November 2017
- [Download](https://www.microsoft.com/download/details.aspx?id=56271)

### <a name="fixed-issues"></a>Behobene Probleme
Die folgenden Probleme wurden in MIM-Version 4.4.1749.0 behoben.

#### <a name="synchronization-service"></a>Synchronisierungs Dienst

- Die Kenn Wort Zurücksetzungs Routine schlägt fehl, wenn die Synchronisierungs Server Domäne keine Vertrauensstellung mit der Zieldomäne besitzt

#### <a name="mim-service"></a>MIM service (MIM-Dienst)

- Fehler beim Aktualisieren der Datenbank. Im datenbankupgradeprotokoll wird eine Ausnahme für die Verletzung der Fremdschlüssel Einschränkung aufgezeichnet.
- Beim Ausführen von Self-Service-Kenn Wort Zurücksetzungs Anforderungen wird der MIM-Dienst nach dem Zufallsprinzip beendet.
- Die festgelegte Berechnung kann die korrekte Mitgliedschaft nicht widerspiegeln. Eine Bindung für ein Attribut kann nicht gelöscht werden, wenn in einem dynamischen Satz oder Gruppen Filter darauf verwiesen wird.
- Der MIM-Dienst funktionierte nicht für das Anforderungs Genehmigungs Szenario mit Exchange Online, bei dem Genehmigungen über das MIM-Add-in für Outlook beantwortet werden.
- Das Attribut "msidmphonegatephonenumber" ohne Länder Code verwendet nicht den Wert "DefaultCountryCode" in MFASettings.xml.
- Gruppenmitgliedschaften können dynamisch aktualisiert werden, ohne dass Sie sich auf die FIM_TemporalEventsJob verlassen müssen.
- Synchronisierungs Regeln unterstützen nicht die Erstellung von Attribut Fluss Regeln für Attribute, deren Namen das Hash-oder Pfund-Symbol (#) enthalten.
  
#### <a name="privilege-access-management"></a>Zugriffs Verwaltung für Berechtigungen 

- New-PAMDomainConfiguration PowerShell-Cmdlet legt einen falschen Wert für die Domänen Vertrauensstellungs Konfiguration fest, was zu einem Fehler führt (dieser unbekannte Anforderungs Parameter kann nicht verarbeitet werden)

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal

- Auf dem Hauptbildschirm des Identitäts Verwaltungsportal wird eine Ausnahme angezeigt, und es wird auch eine Schaltfläche "Schließen" angezeigt.
- Im Fenster "Element löschen" sind falsch angezeigte SchaltflächenDieses Problem ist in Internet Explorer, Firefox und Chrome aufgetreten. 
- Die Such Schaltfläche überschneidet die Schaltfläche Ressourcen Auswahl in einem Genehmigungs Aktivitäts Fenster im Autorisierungs Workflow. Dieses Problem ist in Internet Explorer, Firefox und Chrome aufgetreten. 
- Im Popup Fenster Gruppen Eigenschaften überlappt der Schaltflächen Bereich die ListView-Navigations Steuerelemente auf dem Steuerelement Elemente löschen.Dieses Problem ist in Internet Explorer, Firefox und Chrome aufgetreten.
- Mehrere Benutzeroberflächen Elemente werden nicht ordnungsgemäß angezeigt. Die folgenden Elemente sind korrigiert:

    - Pfeile nach oben und nach unten in einigen Eigenschaften Blättern.
    - Leerer Bereich am unteren Rand einiger Seiten und Dialogfelder.
    - Fehlende Popup-Überlagerungen.

- Wenn Sie den Filter Generator in verschiedenen Bereichen des Produkts verwenden (z. b. Erweiterte Suche), würde der Filter-Generator "hängen", wenn auf die Schaltfläche "OK" im Dialogfeld "Wert auswählen" geklickt wird, ohne dass ein Objekt im Bereich Add-Anweisung ausgewählt wird.
- Das neue Attribut Fluss-Popup in einem Bearbeitungs Dialogfeld für Synchronisierungs Regeln funktionierte in Chrome nicht erwartungsgemäß.
- In einem Bildschirm für die Objekt Verwaltung (z. b. Verteiler Gruppen), wenn mehrere Objekte mithilfe des Kontrollkästchens ausgewählt werden und die Objekte lange anzeigen Amen aufweisen. Jetzt werden Dialog Größen vertikal, sodass das Steuerelement nicht über das Ende des Browser Bildschirms hinaus erweitert wird.
- In einem Objekt Verwaltungs-oder Listenbildschirm (z. b. Verteiler Gruppen) kann das Steuerelement ausgewählte Elemente den Bildschirm nach oben verschieben, sodass er sich direkt unter dem letzten in den Tabellen Listen aufgelisteten Objekt befindet.
- Der Filter Generator (z. b. die erweiterte Suche) im Safari-Browser ist nicht funktionsfähig.
- Portal Dialogfelder, in denen Attributwerte angezeigt werden, werden die kürzeren Wörter in der gesamten Zelle verteilt, wobei eine Menge Leerraum statt linksbündig ausgerichtet ist. 
- In einigen Browserversionen werden die ausgewählten Elemente nicht aktualisiert, wenn die Elementauswahl geändert wird.
- Dialog feldregister Karten und in Zwischenablage kopieren, wenn Sie mit der Tab-Taste zu navigieren.  
- Wenn Sie in Internet Explorer 10 eine Objekt Raster Anzeige anzeigen (z. b. Verteiler Gruppen), wird die Schaltfläche "Suchen Sie die Verteiler Gruppen, die Sie mit der obigen Suche verwenden" verwenden, und nicht in der Mitte des Dialog Felds angezeigt.  
- Nach der Installation eines Updates im MIM-Portal schlägt die Anzeige des Portals in Internet Explorer fehl.
- Wenn Sie die erweiterte Suche im Firefox-Browser verwenden, wird ein Fehler zurückgegeben, wenn Sie die EINGABETASTE für ein Attribut Wert Feld drücken.  

#### <a name="certificate-management"></a>Zertifikatverwaltung

- Ein Anforderungs Absender (Certificate Manager) kann eine Anforderung nicht verwerfen, die von einem Benutzer mit Ausführungs Berechtigung dupliziert oder vergessen wird.
- Wenn Sie versuchen, die virtuelle TPM-Smartcard aus der modernen APP zu erneuern, wird eine unzulässige Ausnahme zurückgegeben.
- Während einiger smartcardaktivitäten werden vorhandene Verbindungen mit der certifiingemanagement-Datenbank unerwartet geöffnet.  
- Wenn ein Update der MIM-Zertifikat Verwaltung (cm) vor dem Ausführen des Konfigurations-Assistenten für MIM cm ausgeführt wird, tritt bei der Aktualisierung ein Fehler auf, und es wird eine Ausnahme angezeigt, die nicht mit dem Problem zusammenhängt.
- Der Konfigurations-Assistent für MIM cm enthält falsche Produkt Versionsinformationen, und das Logo wird nicht ordnungsgemäß angezeigt.  
- Die exportierten Daten für einen MIM-Zertifikat Verwaltungsbericht unterscheiden sich von den Berichtsdaten.Die Spaltendaten stimmen nicht immer mit den Spaltenüberschriften identisch.

## <a name="version-4416420"></a>Version 4.4.1642.0

- Status: 29. August 2017
- [Download](https://www.microsoft.com/download/details.aspx?id=55794)

### <a name="fixed-issues"></a>Behobene Probleme
Die folgenden Probleme wurden in MIM-Version 4.4.1642.0 behoben.

#### <a name="synchronization-service"></a>Synchronisierungsdienst

- Die Kenn Wort Zurücksetzungs Routine schlägt fehl, wenn die Synchronisierungs Server Domäne keine Vertrauensstellung mit der Zieldomäne besitzt
- Der deklarierte Importfilter (Überprüfung des Distinguished Name) funktioniert nicht ordnungsgemäß, wenn ein Objekt aus einer Organisationseinheit verschoben wird, wo es nach einem anderen gefiltert werden sollte, weil es nicht aufgrund der alten Namens eines Phantom Objekts verwendet werden sollte.
- Der Verwaltungs-Agent-Designer reagiert auf der Seite "Partitionen und Hierarchien konfigurieren".
- Die Rangfolge wird nicht zum nächsten Objekt übertragen, wenn die vorherige mit der Rangfolge getrennt ist.
- Sun One Management Agent, der mithilfe von Paging nach untergeordneten Containern auf dem LDAP-Server sucht, verursacht einen Fehler, wenn der Server Paging nicht unterstützt.
- Der Metaverse-Objekttyp wird dynamisch durch Absturz verursacht.

#### <a name="mim-service"></a>MIM Service (MIM-Dienst)

- Die Word-Funktion gibt keine leere Zeichenfolge zurück, wenn die Zeichenfolge weniger als die Anzahl von Wörtern enthält.
- Die Benutzeroberfläche der Ansichts Elemente zeigt eine falsche Mitgliedschaft an, wenn die Kriterien Dereferenzierung enthalten und die Dereferenzierung unter einem anderen kriterienelement erfolgt. 
- Der Authz-Workflow verweigert die Anforderung mit der Fehlermeldung "der Workflow wurde im Zustands Beibehaltungs Speicher nicht gefunden".
- Der Workflow führt eine enumeringressourcenaktivität aus, um MIM abzufragen, und es kommt zeitweise zu einem Fehler.

#### <a name="privilege-access-management"></a>Zugriffs Verwaltung für Berechtigungen

- Get-PAMRequestToApprove Cmdlet gibt keine Kandidaten zurück, wenn die genehmigende Person nicht der zu genehmigenden Rolle entspricht.
- Verwandte MPRs-und PAM-Navigationsleisten Knoten werden auch dann aktiviert, wenn PAM nicht installiert ist.
- Der MIM-Dienst löst eine Ausnahme aus und protokolliert eine Warnung vom Domänen Konfigurations Synchronisierungs Modul für pam.

#### <a name="microsoft-identity-portal"></a>Microsoft Identity Portal

- Wenn Sie Firefox verwenden, um auf den Filter Generator im MIM-Portal zuzugreifen, können Sie die inneren Steuerelemente (Dropdown Felder, Textfelder usw.) nicht verwenden. Sie können erst ausgewählt werden, nachdem Sie mit der rechten Maustaste geklickt haben (d. h. Klicken Sie auf, um das Kontextmenü anzuzeigen)
- Die Portal Suche wird auf einigen Bildschirmauflösungen falsch gerendert. 
- Das Kalender Steuerelement in der erweiterten Suche ist abgeschnitten.
- Die Benutzeroberfläche des Filter-Generators wurde unterbrochen – im Bearbeitungsmodus (falsch platzierte Elemente).
- Das Popup Element enthielt eine Größe mit fester Größe, und Bearbeitungs Steuerelemente waren nicht ordnungsgemäß
- Zeichen folgen Ausschnitte für Norwegen und einige andere Sprachen im Hauptmenü.
- Die kopierte URL aus dem Popup funktioniert nicht.

#### <a name="certificate-management"></a>Zertifikatverwaltung

- MIM Self-Service-Kenn Wort Portale.

#### <a name="reporting"></a>Berichterstellung

- Import-FIMReportingSchemaDefinition Cmdlet für die Berichterstellung schlägt mit einem Fehler fehl.

#### <a name="client-add-in"></a>Client-Add-in

- Die Benutzeroberflächen Sprache prüft nicht die Gleichheit von Ländercodes.

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
Die folgenden Features und Verbesserungen wurden in MIM-Version 4.4.1642.0 hinzugefügt.

#### <a name="mim-service"></a>MIM Service (MIM-Dienst)

- Wiederholungsversuch in den längsten Anforderungs Verarbeitungsvorgängen (Überprüfungsphase) wurde hinzugefügt. Es ist nicht garantiert, dass die Anforderungs Verarbeitung abgeschlossen ist, aber die Anforderungen stabiler werden. Die Korrektur ist standardmäßig deaktiviert. Um die Korrektur zu aktivieren, fügen Sie alwaysonretryrequestprocessingtransaction = "true" im Abschnitt resourcemanagementservice der Konfigurationsdatei von "Datei"

#### <a name="certificate-management"></a>Zertifikatverwaltung

- Neuere Versionen von cm Server können mit einem älteren bulkclient (aber nicht älter als 4.4. xxxx. 0) arbeiten. 

#### <a name="mim-self-service-password-portals"></a>MIM-Self-Service-Kenn Wort Portale 

- Warnung für qagate anzeigen bei Verwendung wird eine Antwort mit Doppelbyte Zeichen bereitgestellt.
- Konfigurierbare Eigenschaft hinzufügen, um die Aktivierung/Deaktivierung der IME-Verwendung im sspr-Registrierungsformular zuzulassen 
- Sspr maskiert die Fragen zur Kenn Wort Registrierung auf dem Portal Client

## <a name="version-4415490"></a>Version 4.4.1549.0
 
* Status: vorheriger MIM 2016 SP1-Hotfixupdate vom 27. März 2017
* Zugehörige BHOLD-Versionsnummer: 6.0.36.0
* Weitere Informationen finden Sie unter [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## <a name="version-4413020"></a>Version 4.4.1302.0

* Status: MIM 2016 Service Pack 1 (SP1) vom 9. November 2016
* Zugehörige BHOLD-Versionsnummer: 5.0.3355.0

### <a name="updates-in-this-service-pack"></a>Aktualisierungen in diesem Servicepack


#### <a name="mim"></a>MIM

- **Browserübergreifende Kompatibilität des MIM-Portals für Endbenutzer-Self-Service:** In diesem Service Pack wird die Unterstützung der meisten gängigen Browser eingeführt. Benutzer können jetzt in Microsoft Edge, Chrome oder Safari auf das MIM-Portal zugreifen und Gruppen sowie Profile selbst verwalten.

- **Unterstützung des MIM-Dienstes für Exchange Online:** Der MIM-Dienst unterstützt bereits seit langem das Senden und Empfangen von E-Mails für Genehmigungen und Benachrichtigungen. Vor SP1 hat MIM nur Exchange Server oder SMTP unterstützt. Mit Service Pack 1 kann der MIM-Dienst Anforderungen sowie e-Mail-Benachrichtigungen über ein Microsoft 365 Exchange Online-Konto senden und empfangen.

- **Überprüfung des Bildformats beim Upload:** MIM kann jetzt das Dateiformat von Bildern überprüfen, die auf das Portal hochgeladen werden.

#### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Unterstützung der Funktionsebene „Windows Server 2016“ durch die PAM „PRIV“-Gesamtstruktur (geschützt) :** Der MIM PAM-Dienst kann in einer Umgebung mit Domänencontrollern konfiguriert werden, die auf der Active Directory Domain Services-Funktionsebene „Windows Server 2016“ ausgeführt werden. Bei Konfiguration ist das Kerberos-Ticket eines Benutzers auf die verbleibende Zeit seiner Rollenaktivierung zeitlich begrenzt.

    >[!Note]
    Wenn Sie die Gesamtstruktur-Funktionsebene „Windows Server 2012 R2“ in Ihrer CORP-Domäne beibehalten möchten, wird empfohlen, auf dem Domänencontroller der CORP-Domäne [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) und [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) zu installieren.

- **Heraufstufung von privilegierten Konten in exklusive Gruppen für die Gesamtstruktur „PRIV“ (geschützt):** Administratoren können jetzt den MIM-Dienst über Gruppen und Benutzer informieren, die ausschließlich in der Gesamtstruktur „PRIV“ vorhanden sind. Auf diese Weise können diese Gruppen und Benutzer in PAM-Rollen eingeschlossen werden.  Sie können dann für eine Rolle aktiviert werden und eine Gruppenmitgliedschaft in der Gesamtstruktur „PRIV“ zugewiesen bekommen.

- **PAM-Bereitstellungsskripts:** PAM-Bereitstellungsskripts ermöglichen Administratoren die Optimierung der Installation der PAM-Umgebung.

- **PAM-Cmdlets für die Konfiguration des Authentifizierungsrichtliniensilos:** Servicepack 1 führt neue Cmdlets ein, um die Sicherheit Ihrer geschützten Gesamtstruktur zu verbessern. Diese Cmdlets erstellen automatisch ein Authentifizierungsrichtliniensilo, das an eine Authentifizierungsrichtlinienvorlage gebunden ist.

    >[!Note]
    Diese Cmdlets werden automatisch als Teil der Bereitstellungsskripts ausgeführt.



### <a name="platform-support"></a>Plattformunterstützung 

Aktualisierte Informationen zur Plattformunterstützung finden Sie im Dokument [Unterstützte Plattformen für MIM 2016](../microsoft-identity-manager-2016-supported-platforms.md).  Zu den in diesem Service Pack unterstützten neuen Plattformen gehört SQL Server 2016, SharePoint 2016.


### <a name="issues-fixed-in-this-release"></a>In diesem Release behobene Probleme


#### <a name="pam"></a>PAM
- „New-PAMGroup“ hat in der PRIV-Gesamtstruktur keine MIM-Objekte für lokale Domänengruppen erstellt.
- „New-PAMDomainConfiguration“ ist mit einer „netdom“-Fehlermeldung fehlgeschlagen.
- PAM-Überwachungsdienst hat Warnungen für Gruppen in der PRIV-Gesamtstruktur protokolliert.


### <a name="how-to-upgrade"></a>Aktualisieren

Kunden, die ein Upgrade auf Microsoft Identity Manager 2016 Service Pack 1 durchführen, sollten die folgende Anleitung für alle Dienste in ihrer Bereitstellung beachten.

>[!Note]
>Kunden, die Forefront Identity Manager 2010 R2 SP1 oder eine frühere Version ausführen, müssen ihre Umgebung zunächst auf Microsoft Identity Manager 2016 (veröffentlicht im August 2015) aktualisieren und dann die folgenden Schritte ausführen.

Bevor Sie beginnen

Vor dem Upgrade des MIM-Dienstes und -Portals müssen Sie die MIM-Synchronisierung-Engine aktualisieren.
Sie müssen die MIM-Dienst- und MIM-Synchronisierungsdatenbanken sichern.

1. Deinstallieren Sie die Microsoft Identity Manager-Komponente, die Sie aktualisieren möchten.
2. Nachdem die Deinstallation abgeschlossen ist, öffnen Sie die Splash-Seite auf dem Installationsmedium „FIMSplash.htm“.
3. Wählen Sie die zu aktualisierende MIM-Komponente aus.
4. Setzen Sie die Installation fort und befolgen Sie die Aufforderungen.
   * Installation von MIM-Dienst und -Portal: Wenn Sie Exchange Online als E-Mail-Konto auswählen, geben Sie auf dem nächsten Bildschirm die E-Mail-Adresse und die Anmeldeinformationen des Exchange Online-Kontos ein.

## <a name="version-4322660"></a>Version 4.3.2266.0


* Status: MIM 2016 Hotfix vom 15. Juli 2016; Kunden müssen auf eine neuere Version aktualisieren, um weiterhin unterstützt zu werden.
* Weitere Informationen finden Sie unter [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## <a name="version-4321950"></a>Version 4.3.2195.0

* Status: MIM 2016 Hotfix vom 20. März 2016, Kunden müssen ein Upgrade auf eine neuere Version durchführen, um unterstützt zu werden.
* Zugehörige BHOLD-Versionsnummer: 5.0.3355.0
* Weitere Informationen finden Sie unter [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
    
## <a name="version-4320640"></a>Version 4.3.2064.0

* Status MIM 2016 Hotfix vom 11. Dezember 2015; Kunden müssen auf eine neuere Version aktualisieren, um weiterhin unterstützt zu werden.
* Zugehörige BHOLD-Versionsnummer: 5.0.3176.0
* Weitere Informationen finden Sie unter [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## <a name="version-4319350"></a>Version 4.3.1935.0

* Status MIM 2016 GA vom 6. August 2015; Kunden müssen auf eine neuere Version aktualisieren, um weiterhin unterstützt zu werden.
* Zugehörige BHOLD-Versionsnummer: 5.0.3079.0

