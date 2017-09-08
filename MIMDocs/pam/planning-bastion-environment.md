---
title: "Planen einer geschützten Umgebung | Microsoft Docs"
description: 
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/16/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 402c690b514dce62024f13014c1491433fbd8816
ms.sourcegitcommit: a0e206fd67245f02d94d5f6c9d606970117dd8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2017
---
# <a name="planning-a-bastion-environment"></a>Planen einer geschützten Umgebung

Durch das Hinzufügen einer geschützten Umgebung mit einer dedizierten administrativen Gesamtstruktur zu Active Directory sind Organisationen in der Lage, Administratorkonten, Arbeitsstationen und Gruppen problemlos zu verwalten. Dabei weist die Umgebung stärkere Sicherheitskontrollen auf als ihre vorhandene Produktionsumgebung.

Diese Architektur ermöglicht eine Reihe von Kontrollmechanismen, die in einer Architektur mit einer einzelnen Gesamtstruktur nicht möglich oder nicht einfach zu konfigurieren sind. Hierzu gehört die Bereitstellung von Konten als Standardbenutzer ohne erweiterte Berechtigungen in der administrativen Gesamtstruktur, die in der Produktionsumgebung über weitgehende Berechtigungen verfügen, wodurch eine umfassende technische Erzwingung der Governance möglich wird. Zudem ermöglicht diese Architektur die Verwendung der ausgewählten Authentifizierung von Vertrauensstellungen zum Einschränken von Anmeldungen (und Offenlegen der Anmeldeinformationen) auf autorisierte Hosts. In Situationen, in denen eine umfassendere Sicherung für die Produktionsgesamtstruktur ohne die Kosten und Komplexität einer vollständigen Wiederherstellung erwünscht ist, kann eine administrative Gesamtstruktur eine Umgebung bereitstellen, in der für das Produktionsumfeld eine höhere Sicherheitsstufe gilt.

Zusätzlich zur dedizierten administrativen Gesamtstruktur können auch weitere Verfahren verwendet werden. Dazu gehören das Einschränken der Situationen, in denen Administratorrechte verfügbar gemacht werden, das Beschränken der Rollenberechtigungen der Benutzer in dieser Gesamtstruktur und das Sicherstellen, dass administrative Aufgaben nicht auf Hosts für Standardbenutzeraktivitäten (z. B. E-Mail oder Browsen im Internet) durchgeführt werden.

## <a name="best-practice-considerations"></a>Überlegungen zu bewährten Methoden

Eine dedizierte administrative Gesamtstruktur ist eine Active Directory-Standardgesamtstruktur mit einer einzelnen Domäne, die der Active Directory-Verwaltung dient. Ein Vorteil bei administrativen Gesamtstrukturen und Domänen liegt darin, dass aufgrund ihrer begrenzten Anwendungsfälle mehr Sicherheitsmaßnahmen für diese angewendet werden können als für Produktionsgesamtstrukturen. Da diese Gesamtstruktur abgetrennt ist und den bestehenden Gesamtstrukturen der Organisation nicht vertraut, wirkt sich eine Sicherheitsgefährdung in einer anderen Gesamtstruktur nicht auf diese dedizierte Gesamtstruktur aus.

Beim Entwurf einer administrativen Gesamtstruktur ist Folgendes zu berücksichtigen:

### <a name="limited-scope"></a>Eingeschränkter Gültigkeitsbereich

Der Wert einer administrativen Gesamtstruktur liegt in der hohen Sicherheitsstufe und der verringerten Angriffsfläche. Die Gesamtstruktur kann zusätzliche Verwaltungsfunktionen und -anwendungen enthalten, jede Erweiterung des Bereichs vergrößert jedoch die Angriffsfläche der Gesamtstruktur und ihrer Ressourcen. Das Ziel ist es, die Funktionen der Gesamtstruktur zu begrenzen, um die Angriffsfläche möglichst gering zu halten.

Gemäß dem [Ebenenmodell](tier-model-for-partitioning-administrative-privileges.md) der Partitionierung administrativer Berechtigungen sollten sich die Konten in einer administrativen Gesamtstruktur auf einer einzigen Ebene befinden, in der Regel entweder auf Ebene 0 oder Ebene 1. Wenn eine Gesamtstruktur sich auf Ebene 1 befindet, sollten Sie diese auf einen bestimmten Anwendungsbereich (z. B. Finanzanwendungen) oder eine Benutzercommunity (z. B. externe IT-Anbieter) beschränken.

### <a name="restricted-trust"></a>Eingeschränkte Vertrauensstellung

Die Produktionsgesamtstruktur *CORP* sollte der administrativen Gesamtstruktur *PRIV* vertrauen, aber nicht umgekehrt. Dabei kann es sich um eine Domänen- oder Gesamtstruktur-Vertrauensstellung handeln. Die administrative Gesamtstruktur muss den verwalteten Domänen und Gesamtstrukturen nicht vertrauen, um Active Directory verwalten zu können. Zusätzliche Anwendungen können jedoch eine bidirektionale Vertrauensstellung, Sicherheitsüberprüfungen und Tests erfordern.

Um sicherzustellen, dass Konten in der administrativen Gesamtstruktur nur die geeigneten Produktionshosts verwenden, sollte die ausgewählte Authentifizierung verwendet werden. Zur Verwaltung von Domänencontrollern und Delegierung von Rechten in Active Directory ist hierbei in der Regel die Berechtigung für die Anmeldung auf Domänencontrollern für die jeweiligen Administratorkonten der Ebene 0 in der administrativen Gesamtstruktur erforderlich. Weitere Informationen finden Sie unter [Konfigurieren der Einstellungen für ausgewählte Authentifizierung](http://technet.microsoft.com/library/cc816580.aspx).

## <a name="maintain-logical-separation"></a>Verwalten der logischen Trennung

Um sicherzustellen, dass die geschützte Umgebung nicht durch bestehende oder zukünftige Sicherheitsvorfälle im Active Directory der Organisation beeinträchtigt wird, sollten bei der Vorbereitung von Systemen für die geschützte Umgebung die folgenden Richtlinien berücksichtigt werden:

- Windows-Server sollten nicht in Domänen der vorhandenen Umgebung eingebunden werden oder Software oder verteilte Einstellungen von dieser nutzen.

- Die geschützte Umgebung muss eigene Active Directory-Domänendienste aufweisen, die Kerberos, LDAP, DNS und Zeitdienste für die geschützte Umgebung bereitstellen.

- MIM sollte keine SQL-Datenbankfarm in der vorhandenen Umgebung verwenden. SQL Server sollte auf dedizierten Servern in der geschützten Umgebung bereitgestellt werden.

- In der geschützten Umgebung ist Microsoft Identity Manager 2016 erforderlich. Im Speziellen müssen der MIM-Dienst und die PAM-Komponenten bereitgestellt werden.

- Sicherungssoftware und Medien für die geschützte Umgebung müssen getrennt von denen für Systeme in den bestehenden Gesamtstrukturen gehalten werden, damit Administratoren in der bestehenden Gesamtstruktur nicht in der Lage sind, eine Sicherung der geschützten Umgebung zu manipulieren.

- Benutzer, die die geschützte Umgebung verwalten, müssen sich von Arbeitsstationen aus anmelden, die nicht für Administratoren in der bestehenden Umgebung zugänglich sind, damit die Anmeldeinformationen für die geschützten Umgebung nicht weitergegeben werden können.

## <a name="ensure-availability-of-administration-services"></a>Sicherstellen der Verfügbarkeit von Verwaltungsdiensten

Während die Verwaltung von Anwendungen in die geschützte Umgebung überführt wird, muss eine ausreichende Verfügbarkeit für die Anforderungen dieser Anwendungen berücksichtigt werden. Die Methoden umfassen Folgendes:

- Bereitstellen von Active Directory-Domänendiensten auf mehreren Computern in der geschützten Umgebung. Es sind mindestens zwei erforderlich, um eine fortgesetzte Authentifizierung sicherzustellen, auch wenn ein Server aufgrund von geplanter Wartung vorübergehend neu gestartet wird. Möglicherweise sind zusätzliche Computer erforderlich, um eine höhere Auslastung zu verarbeiten oder um Ressourcen und Administratoren in mehreren geografischen Regionen zu verwalten.

- Vorbereiten von Notfallkonten („Break Glass-Konten“) in der dedizierten administrativen Gesamtstruktur und in der Gesamtstruktur für Ausnahmesituationen.

- Bereitstellen von SQL Server und dem MIM-Dienst auf mehreren Computern in der geschützten Umgebung.

- Verwalten einer Sicherungskopie von AD und SQL bei jeder Änderung von Benutzern oder Rollendefinitionen in der dedizierten Gesamtstruktur.

## <a name="configure-appropriate-active-directory-permissions"></a>Konfigurieren geeigneter Active Directory-Berechtigungen

Die administrative Gesamtstruktur sollte anhand der geringsten erforderlichen Rechte für die Active Directory-Verwaltung konfiguriert werden.

- Konten in der administrativen Gesamtstruktur, die für die Verwaltung der Produktionsumgebung verwendet werden, sollten keine Administratorberechtigungen für die administrative Gesamtstruktur oder die enthaltenen Domänen oder Arbeitsstationen erhalten.

- Administratorrechte für die administrative Gesamtstruktur selbst sollten über ein Offlineverfahren streng kontrolliert werden, um das Löschen von Überwachungsprotokollen durch Angreifer oder böswillige Mitarbeiter zu erschweren. Dies trägt auch dazu bei, dass Mitarbeiter mit Administratorkonten für die Produktion die Einschränkungen ihrer Konten nicht lockern und damit das Risiko für die Organisation steigern können.

- Die administrative Gesamtstruktur sollte die Microsoft Security Compliance Manager-Konfigurationen (SCM) für die Domäne befolgen, einschließlich sicherer Konfigurationen für Authentifizierungsprotokolle.

Sie müssen beim Erstellen der geschützten Umgebung vor der Installation von Microsoft Identity Manager die Konten identifizieren und erstellen, die für die Verwaltung dieser Umgebung verwendet werden. Dazu gehören:

- **Notfallkonten** sollten sich nur bei den Domänencontrollern in der geschützten Umgebung anmelden können.

- **„Red Card“-Administratoren** stellen andere Konten bereit und führen nicht geplante Wartungsaufgaben aus. Diesen Konten wird kein Zugriff auf bestehende Gesamtstrukturen oder Systeme außerhalb der geschützten Umgebung gewährt. Die Anmeldeinformationen, z. B. eine Smartcard, sollten physisch sicher aufbewahrt werden, und die Verwendung dieser Konten sollte protokolliert werden.

- **Dienstkonten** werden von Microsoft Identity Manager, SQL Server und anderer Software benötigt.

## <a name="harden-the-hosts"></a>Absichern der Hosts

Alle Hosts, einschließlich Domänencontrollern, Servern und Arbeitsstationen, die Mitglied der administrativen Gesamtstruktur sind, müssen mit den neuesten Betriebssystemen und Servicepacks ausgestattet sein und auf dem aktuellen Stand gehalten werden.

- Die Anwendungen für die Umsetzung der Verwaltung sollten auf Arbeitsstationen vorinstalliert werden, sodass Konten nicht der lokalen Administratorgruppe angehören müssen, um sie zu installieren. Die Wartung der Domänencontroller kann in der Regel mit RDP und Remote Server Administration Tools ausgeführt werden.

- Hosts in der administrativen Gesamtstruktur sollten automatisch mit Sicherheitsupdates aktualisiert werden. Dies birgt zwar das Risiko einer Unterbrechung des Domänencontroller-Wartungsbetriebs, bietet jedoch eine erhebliche Verringerung von Sicherheitsrisiken durch nicht behobene Schwachstellen.

### <a name="identify-administrative-hosts"></a>Identifizieren der administrativen Hosts

Das Risiko eines Systems oder einer Arbeitsstation sollte nach der Aktivität mit dem höchsten Risiko bewertet werden, die auf ihm ausgeführt wird, z. B. Browsen im Internet, Senden und Empfangen von E-Mails oder Verwenden anderer Anwendungen, die unbekannte oder nicht vertrauenswürdige Inhalte verarbeiten.

Zu den administrativen Hosts gehören die folgenden Computer:

- Ein Desktopcomputer, auf dem Anmeldeinformationen des Administrators eingegeben werden.

- Administrative „Wechselserver“, auf denen administrativen Sitzungen und Tools ausgeführt werden.

- Alle Hosts, auf denen administrative Aktionen durchgeführt werden, einschließlich solcher, bei denen Server und Anwendungen über einen Standard-Benutzerdesktop, auf dem ein RDP-Client ausgeführt wird, remote verwaltet werden.

- Server, auf denen Anwendungen gehostet werden, die verwaltet werden müssen und auf die nicht mit RDP im eingeschränkten Administratormodus oder mit Windows PowerShell-Remoting zugegriffen wird.

### <a name="deploy-dedicated-administrative-workstations"></a>Bereitstellen von dedizierten administrativen Arbeitsstationen

Möglicherweise sind separate abgesicherte Arbeitsstationen speziell für Benutzer mit sehr umfassenden Anmeldeinformationen erforderlich, auch wenn dies umständlich ist. Es ist wichtig, einen Host mit einer Sicherheitsstufe bereitzustellen, die der Berechtigungsstufe der Anmeldeinformationen entspricht oder höher als diese ist. Erwägen Sie die Einrichtung folgender Maßnahmen für zusätzlichen Schutz:

- **Überprüfung aller Medien im Build**, um das Risiko durch Malware zu verringern, die in einem Masterimage installiert ist oder während des Downloads oder der Speicherung in eine Installationsdateidatei gelangt.

- **Sicherheitsbaselines** sollten als Ausgangskonfiguration verwendet werden. Microsoft Security Compliance Manager (SCM) kann zum Konfigurieren von Baselines auf administrativen Hosts verwendet werden.

- **Sicherer Start** zur Abhilfe gegen Angreifer oder Malware, die versuchen, während des Startvorgangs nicht signierten Code zu laden.

- **Softwareeinschränkungen** , um sicherzustellen, dass nur autorisierte Verwaltungssoftware auf den administrativen Hosts ausgeführt wird. Kunden können für diesen Vorgang AppLocker mit einer Whitelist autorisierter Anwendungen verwenden, um zu verhindern, dass Malware oder nicht unterstützte Anwendungen ausgeführt werden.

- **Verschlüsselung ganzer Volumes** zur Verringerung des Risikos durch den Verlust physischer Computer, z. B. administrativer Laptops, die an Remotestandorten eingesetzt werden.

- **USB-Einschränkungen** zum Schutz vor physischer Infektion.

- **Netzwerkisolation** zum Schutz vor Netzwerkangriffen und unbeabsichtigten Administratoraktionen. Hostfirewalls sollten alle eingehenden Verbindungen, mit Ausnahme der ausdrücklich erforderlichen, und sämtliche nicht benötigten ausgehenden Internetzugriffe sperren.

- **Antimalware** zum Schutz vor bekannten Risiken und Malware.

- **Schutzmaßnahmen gegen Exploits** zum Verringern der Risiken durch unbekannte Gefahren und Exploits, einschließlich des Enhanced Mitigation Experience Toolkit (EMET).

- **Analyse der Angriffsfläche** zum Verhindern der Einführung neuer Angriffsvektoren auf Windows während der Installation von neuer Software. Tools wie Attack Surface Analyzer (ASA) tragen dazu bei, die Konfigurationseinstellungen auf einem Host auszuwerten und Angriffsvektoren zu identifizieren, die durch Software oder Konfigurationsänderungen eingeführt werden.

- Benutzern sollten auf ihren lokalen Computern keine **Administratorrechte** gewährt werden.

- Ausgehende RDP-Sitzungen sollten den **RestrictedAdmin-Modus** aufweisen, sofern die Rolle nichts anderes erfordert. Weitere Informationen finden Sie unter [Neues in den Remotedesktopdiensten unter Windows Server](https://technet.microsoft.com/library/dn283323.aspx).

Einige dieser Maßnahmen erscheinen möglicherweise extrem, doch haben viele Nachrichten in den letzten Jahren aufgezeigt, welche umfassenden Fähigkeiten erfahrene Angreifer einsetzen, um Ziele zu manipulieren.

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>Vorbereiten vorhandener Domänen zur Verwaltung durch die geschützte Umgebung

MIM verwendet PowerShell-Cmdlets, um Vertrauensstellungen zwischen den bestehenden AD-Domänen und der dedizierten administrativen Gesamtstruktur in der geschützten Umgebung einzurichten. Nach der Bereitstellung der geschützten Umgebung und bevor Benutzer oder Gruppen in JIT konvertiert wurden, aktualisieren die Cmdlets `New-PAMTrust` und `New-PAMDomainConfiguration` die Vertrauensstellungen zwischen den Domänen und erstellen für AD und MIM erforderliche Artefakte.

Bei Änderungen der bestehenden Active Directory-Topologie können mit den Cmdlets `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` und `Remove-PAMDomainConfiguration` die Vertrauensstellungen aktualisiert werden.

## <a name="establish-trust-for-each-forest"></a>Einrichten einer Vertrauensstellung für jede Gesamtstruktur

Das Cmdlet `New-PAMTrust` muss einmal für jede bestehende Gesamtstruktur ausgeführt werden. Er wird auf dem Computer des MIM-Diensts in der administrativen Domäne aufgerufen. Die Parameter für diesen Befehl sind der Domänenname der obersten Domäne in der bestehenden Gesamtstruktur und die Anmeldeinformationen eines Administrators dieser Domäne.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Nach dem Einrichten der Vertrauensstellung konfigurieren Sie jede Domäne für die Verwaltung über die geschützte Umgebung, wie im nächsten Abschnitt beschrieben.

## <a name="enable-management-of-each-domain"></a>Aktivieren der Verwaltung jeder Domäne

Es gibt sieben Anforderungen für die Aktivierung der Verwaltung für eine bestehende Domäne.

### <a name="1-a-security-group-on-the-local-domain"></a>1. Eine Sicherheitsgruppe in der lokalen Domäne

Es muss eine Gruppe in der vorhandenen Domäne geben, deren Name dem NetBIOS-Domänennamen gefolgt von drei Dollarzeichen entspricht, z. B. *CONTOSO$$$*. Der Gruppenbereich muss *Lokal (in Domäne)* sein, und der Gruppentyp muss *Sicherheit* sein. Dies ist erforderlich, damit in der dedizierten administrativen Gesamtstruktur erstellte Gruppen dieselbe Sicherheits-ID wie Gruppen in dieser Domäne aufweisen. Die Erstellung dieser Gruppe erfolgt mit dem folgenden PowerShell-Befehl – dieser wird von einem Administrator der bestehenden Domäne auf einer Arbeitsstation ausgeführt, die der bestehenden Domäne beigetreten ist:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2. Überwachung von Erfolg und Fehlern

Die Gruppenrichtlinieneinstellungen auf dem Domänencontroller für die Überwachung müssen sowohl Erfolgs- als auch Fehlerüberwachung für „Kontenverwaltung überwachen“ und „Verzeichniszugriff überwachen“ aufweisen. Sie können über die Gruppenrichtlinien-Verwaltungskonsole eingerichtet werden, die von einem Administrator der bestehenden Domäne auf einer Arbeitsstation ausgeführt wird, die der bestehenden Domäne beigetreten ist:

3. Wechseln Sie zu **Start** > **Verwaltung** > **Gruppenrichtlinienverwaltung**.

4. Navigieren Sie zu **Gesamtstruktur: contoso.local** > **Domänen** > **contoso.local** > **Domänencontroller** > **Standarddomänencontroller-Richtlinie**. Es wird eine informierende Meldung angezeigt.

    ![Standarddomänencontroller-Richtlinie – Screenshot](media/pam-group-policy-management.jpg)

5. Klicken Sie mit der rechten Maustaste auf **Standarddomänencontroller-Richtlinie**, und wählen Sie **Bearbeiten** aus. Ein neues Fenster wird angezeigt.

6. Navigieren Sie im Fenster „Gruppenrichtlinienverwaltungs-Editor“ unter „Standarddomänencontroller-Richtlinie“ zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Lokale Richtlinien** > **Überwachungsrichtlinie**.

    ![Gruppenrichtlinienverwaltungs-Editor – Screenshot](media/pam-group-policy-management-editor.jpg)

5. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Kontenverwaltung überwachen**, und wählen Sie den Befehl **Eigenschaften** aus. Wählen Sie **Diese Richtlinieneinstellungen definieren** aus, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler**, und klicken Sie anschließend auf **Übernehmen** und **OK**.

6. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Verzeichnisdienstzugriff überwachen**, und wählen Sie den Befehl **Eigenschaften** aus. Wählen Sie **Diese Richtlinieneinstellungen definieren** aus, aktivieren Sie das Kontrollkästchen **Erfolg** und das Kontrollkästchen **Fehler**, und klicken Sie anschließend auf **Übernehmen** und **OK**.

    ![Einstellungen der Erfolgs- und Fehlerrichtlinie – Screenshot](media/pam-group-policy-management-editor2.jpg)

7. Schließen Sie die Fenster „Gruppenrichtlinienverwaltungs-Editor“ und „Gruppenrichtlinienverwaltung“. Wenden Sie dann die Überwachungseinstellungen an, indem Sie ein PowerShell-Fenster starten und Folgendes eingeben:

    ```
    gpupdate /force /target:computer
    ```

Die Meldung „Die Aktualisierung der Computerrichtlinie wurde erfolgreich abgeschlossen.“ sollte nach einigen Minuten angezeigt werden.

### <a name="3-allow-connections-to-the-local-security-authority"></a>3. Zulassen von Verbindungen mit der lokalen Sicherheitsautorität

Die Domänencontroller müssen RPC über TCP/IP-Verbindungen für die lokale Sicherheitsautorität (Local Security Authority, LSA) aus der geschützten Umgebung zulassen. Auf älteren Versionen von Windows Server muss die TCP/IP-Unterstützung in LSA in der Registrierung aktiviert werden:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4. Erstellen der PAM-Domänenkonfiguration

Das Cmdlet `New-PAMDomainConfiguration` muss auf dem Computer des MIM-Diensts in der administrativen Domäne ausgeführt werden. Die Parameter für diesen Befehl sind der Domänenname der bestehenden Domäne und die Anmeldeinformationen eines Administrators dieser Domäne.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5. Gewähren von Leseberechtigungen für Konten

Die Konten in der geschützten Gesamtstruktur, über die Rollen eingerichtet werden (Administratoren, die die Cmdlets `New-PAMUser` und `New-PAMGroup` verwenden) sowie das vom MIM-Überwachungsdienst verwendete Konto benötigen Leseberechtigungen in der Domäne.

Die folgenden Schritte aktivieren den Lesezugriff für den Benutzer *PRIV\Administrator* für die Domäne *Contoso* im Domänencontroller *CORPDC*:

1. Stellen Sie sicher, dass Sie auf „CORPDC“ als Contoso-Domänenadministrator angemeldet sind (z. B. „Contoso\Administrator“).

2. Starten Sie Active Directory-Benutzer und -Computer.

3. Klicken Sie mit der rechten Maustaste auf die Domäne **contoso.local**, und wählen Sie **Objektverwaltung zuweisen**aus.

4. Klicken Sie auf der Registerkarte “Ausgewählte Benutzer und Gruppen“ auf **Hinzufügen**.

5. Klicken Sie im Popupfenster zur Auswahl von Benutzern, Computern oder Gruppen auf **Speicherorte**, und ändern Sie den Speicherort in *priv.contoso.local*. Geben Sie als Objektnamen *Domänen-Admins* ein, und klicken Sie dann auf **Namen überprüfen**. Wenn ein Popupfenster angezeigt wird, geben Sie den Benutzernamen *priv\administrator* sowie das Kennwort ein.

6. Geben Sie nach „Domänen-Admins“ die Zeichenfolge *; MIMMonitor* ein. Nachdem die Namen „Domänen-Admins“ und „MIMMonitor“ unterstrichen sind, klicken Sie auf **OK** und dann auf **Weiter**.

7. Wählen Sie in der Liste der allgemeinen Aufgaben **Liest alle Benutzerinformationen** aus, und klicken Sie auf **Weiter** und dann auf **Fertig stellen**.

18. Schließen Sie %%amp;quot;Active Directory-Benutzer und -Computer%%amp;quot;.

### <a name="6-a-break-glass-account"></a>6. Ein Notfallkonto

Wenn das Ziel des Projekts für Privileged Access Management darin besteht, die Anzahl der Konten mit Domänenadministrator-Berechtigungen zu verringern, die dauerhaft der Domäne zugewiesen sind, muss es in der Domäne ein *Notfallkonto* geben, falls später ein Problem mit der Vertrauensstellung auftritt. Konten für den Notfallzugriff auf die Produktionsgesamtstruktur sollten in jeder Domäne vorhanden sein und sich ausschließlich bei Domänencontrollern anmelden können. Für Organisationen mit mehreren Standorten sind aus Redundanzgründen möglicherweise zusätzliche Konten erforderlich.

### <a name="7-update-permissions-in-the-bastion-environment"></a>7. Aktualisieren von Berechtigungen in der geschützten Umgebung

Überprüfen Sie die Berechtigungen im *AdminSDHolder*-Objekt im Systemcontainer dieser Domäne. Das *AdminSDHolder* -Objekt enthält eine eindeutige Zugriffssteuerungsliste (ACL), mit der die Berechtigungen von Sicherheitsprinzipalen gesteuert werden, die Mitglieder der integrierten privilegierten Active Directory-Gruppen sind. Wenn Änderungen an den Standardberechtigungen vorgenommen wurden, die Auswirkungen auf Benutzer mit Administratorrechten in der Domäne hätten, werden diese Berechtigungen nicht für Benutzer übernommen, deren Konten sich in der geschützten Umgebung befinden.

## <a name="select-users-and-groups-for-inclusion"></a>Auswählen der einzuschließenden Benutzern und Gruppen

Der nächste Schritt besteht im Definieren der PAM-Rollen und im Zuordnen der Benutzer und Gruppen, auf die diese Zugriff haben sollten. Dabei handelt es sich in der Regel um eine Teilmenge der Benutzer und Gruppen für die Ebene, bei denen erkannt wurde, dass sie in der geschützten Umgebung verwaltet werden. Weitere Informationen finden Sie unter [Definieren von Rollen für Privileged Access Management](defining-roles-for-pam.md).
