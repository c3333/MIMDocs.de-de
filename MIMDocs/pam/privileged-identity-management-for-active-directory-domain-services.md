---
title: "Was ist PAM für Active Directory-Domänendienste? | Microsoft Docs"
description: "Erfahren Sie mehr über Privileged Access Management und wie es Sie bei der Verwaltung und dem Schutz Ihrer Active Directory-Umgebung unterstützen kann."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experimental: true
experiment_id: kgremban_images
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 4b3779f33a8bbd2ad62c88343ef3611b17fc81a2

---

# <a name="privileged-access-management-for-active-directory-domain-services"></a>Privileged Access Management für Active Directory-Domänendienste
Die privilegierte Zugriffsverwaltung (PAM) ist eine Lösung, mit der Organisationen den privilegierten Zugriff innerhalb einer vorhandenen Active Directory-Umgebung einschränken können.

Mit Privileged Access Management lassen sich zwei Ziele erreichen:

- Wiederherstellen der Kontrolle über eine gefährdete Active Directory-Umgebung durch Verwalten einer separaten geschützten Umgebung, die bekanntermaßen nicht von Angriffen betroffen ist.  
- Isolieren der Verwendung von privilegierten Konten, um das Risiko zu reduzieren, dass diese Anmeldeinformationen gestohlen werden.

> [!NOTE]
> PAM ist eine Instanz von [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM), das mithilfe von Microsoft Identity Manager (MIM) implementiert wird.

## <a name="what-problems-does-pam-help-solve"></a>Welche Probleme lassen sich mit PAM lösen?
Viele Unternehmen machen sich heute Sorgen um den Zugriff auf Ressourcen innerhalb einer Active Directory-Umgebung. Besonders beunruhigend sind Nachrichten zu Sicherheitsrisiken, nicht autorisierten Hochstufungen von Berechtigungen und anderen Arten des unerlaubten Zugriffs, einschließlich Pass-the-Hash-, Pass-the-Ticket-, Spear-Phishing- und Kerberos-Angriffen.

Derzeit ist es für Angreifer zu einfach, sich Anmeldeinformationen von Domänenadministratorkonten zu verschaffen, und zu schwierig, diese Angriffe nachträglich zu erkennen. Ziel von PAM ist es, Zugriffschancen für böswillige Benutzer zu reduzieren und gleichzeitig die Kontrolle und Überwachung der Umgebung zu verbessern.

PAM erschwert es Angreifern, in ein Netzwerk einzudringen und Zugriff auf privilegierte Konten zu erhalten. PAM bietet mehr Schutz für privilegierte Gruppen, die den Zugriff auf die zur Domäne gehörenden Computer und die Anwendungen auf diesen Computern steuern. Ferner ermöglicht PAM eine bessere Überwachung, mehr Transparenz und differenzierte Steuerungsmöglichkeiten, sodass Organisationen erkennen können, welche Benutzer als privilegierte Administratoren fungieren und welche Aufgaben sie ausführen. PAM bietet Organisationen mehr Einblick in die Verwendung administrativer Konten in der Umgebung.

## <a name="how-is-pam-set-up"></a>Wie wird PAM eingerichtet?
PAM baut auf dem Prinzip der Just-in-Time-Verwaltung auf, die mit [Just Enough Administration (JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362) verwandt ist. JEA ist ein Windows PowerShell-Toolkit, das eine Reihe von Befehlen zum Ausführen privilegierter Aktivitäten und einen Endpunkt definiert, an dem Administratoren die Autorisierung zum Ausführen dieser Befehle erhalten. In JEA entscheidet ein Administrator, dass ein Benutzer mit einer bestimmten Berechtigung eine bestimmte Aufgabe ausführen darf. Jedes Mal, wenn ein berechtigter Benutzer diese Aufgabe ausführen muss, aktiviert er diese Berechtigung. Die Berechtigung läuft nach einem bestimmten Zeitraum ab, sodass ein böswilliger Benutzer den Zugriff nicht stehlen kann.

Für Einrichtung und Betrieb von PAM sind vier Schritte erforderlich.

![PAM-Schritte: Vorbereiten, Schützen, Betreiben, Überwachen – Diagramm](media/MIM_PIM_SetupProcess.png)

1.  **Vorbereiten**: Bestimmen Sie, welche Gruppen in Ihrer vorhandenen Gesamtstruktur maßgebliche Berechtigungen haben. Erstellen Sie diese Gruppen ohne Mitglieder in der geschützten Gesamtstruktur neu.

2.  **Schützen**: Richten Sie einen Lebenszyklus- und Authentifizierungsschutz ein, wie z. B. die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA), wenn Benutzer die Just-in-Time-Verwaltung anfordern. Die MFA verhindert programmgesteuerte Angriffe mittels Schadsoftware oder im Anschluss an den Diebstahl von Anmeldeinformationen.

3.  **Betreiben**: Nachdem die Authentifizierungsvoraussetzungen erfüllt sind und eine Anforderung genehmigt wurde, wird ein Benutzerkonto temporär einer privilegierten Gruppe in der geschützten Gesamtstruktur hinzugefügt. Während eines vorher festgelegten Zeitraums besitzt der Administrator alle Rechte und Zugriffsberechtigungen, die dieser Gruppe zugewiesen sind. Nach Ablauf des Zeitraums wird das Konto aus der Gruppe entfernt.

4.  **Überwachen**: PAM bietet die Überwachung von, Benachrichtigungen zu und Berichte zu Anforderungen des privilegierten Zugriffs. Sie können den Verlauf privilegierter Zugriffe überprüfen und sehen, wer eine Aktivität ausgeführt hat. Sie können entscheiden, ob die Aktivität zulässig ist oder nicht, und nicht autorisierte Aktivitäten ermitteln, wie z. B. einen Versuch, einen Benutzer direkt einer privilegierten Gruppe in der ursprünglichen Gesamtstruktur hinzuzufügen. Dieser Schritt ist nicht nur wichtig für die Erkennung von Schadsoftware, sondern auch für die Überwachung auf "interne" Angreifer.

## <a name="how-does-pam-work"></a>Wie funktioniert PAM?
PAM basiert auf neuen Funktionen in den Active Directory-Domänendiensten, insbesondere für die Domänenauthentifizierung und -autorisierung, sowie neuen Funktionen in Microsoft Identity Manager. PAM trennt privilegierte Konten von einer vorhandenen Active Directory-Umgebung. Wenn ein privilegiertes Konto verwendet werden muss, muss es zunächst angefordert und dann genehmigt werden. Nach der Genehmigung erhält das privilegierte Konto Berechtigungen über eine fremde Prinzipalgruppe in einer neuen geschützten Gesamtstruktur anstatt in der aktuellen Gesamtstruktur des Benutzers oder der Anwendung. Der Einsatz einer geschützten Gesamtstruktur bietet der Organisation mehr Kontrolle, z. B. ob ein Benutzer Mitglied einer privilegierten Gruppe sein darf und wie der Benutzer sich authentifizieren muss.

Active Directory, der MIM-Dienst und andere Teile dieser Lösung können auch in einer Konfiguration mit hoher Verfügbarkeit bereitgestellt werden.

Das folgende Beispiel zeigt, wie PIM im Detail funktioniert.

![PIM-Verfahren und -Teilnehmer – Diagramm](media/MIM_PIM_howitworks.png)

Die geschützte Gesamtstruktur gibt zeitlich begrenzte Gruppenmitgliedschaften aus, die wiederum zeitlich begrenzte TGTs (Ticket-Granting Tickets) ausstellen. Kerberos-basierte Anwendungen oder Dienste können diese TGTs erkennen und durchsetzen, wenn sich die Anwendungen und Dienste in Gesamtstrukturen befinden, die der geschützten Gesamtstruktur vertrauen.

Täglich verwendete Benutzerkonten müssen nicht in eine neue Gesamtstruktur verschoben werden. Dasselbe gilt für Computer, Anwendungen und ihre Gruppen. Sie bleiben, wo sie derzeit sind, d. h. in einer vorhandenen Gesamtstruktur. Nehmen wir als Beispiel eine Organisation, die derzeit von Cybersicherheitsproblemen betroffen ist, jedoch nicht unmittelbar plant, ein Upgrade der Serverinfrastruktur auf die nächste Version von Windows Server vorzunehmen. Diese Organisation kann dennoch diese kombinierte Lösung durch den Einsatz von MIM und einer neuen geschützten Gesamtstruktur nutzen und den Zugriff auf vorhandenen Ressourcen besser steuern.

PAM bietet die folgenden Vorteile:

-   **Isolation bzw. Bestimmung des Geltungsbereichs von Berechtigungen**: Benutzer besitzen keine Berechtigungen in Konten, die auch für Aufgaben ohne Berechtigungen wie das Lesen von E-Mails oder den Internetzugriff verwendet werden. Benutzer müssen Berechtigungen anfordern. Anforderungen werden anhand von MIM-Richtlinien, die von einem PAM-Administrator festgelegt werden, genehmigt oder abgelehnt. Erst nachdem eine Anforderung genehmigt wurde, ist der privilegierte Zugriff verfügbar.

-   **Hochstufung und Nachweis**: Es gibt neue Herausforderungen an die Authentifizierung und Autorisierung, die zum Verwalten des Lebenszyklus getrennter Administratorkonten bewältigt werden müssen. Der Benutzer kann die Hochstufung eines Administratorkontos anfordern, woraufhin diese Anforderung MIM-Workflows durchläuft.

-   **Zusätzliche Protokollierung**: Neben den integrierten MIM-Workflows gibt es eine zusätzliche Protokollierung für PAM, die die Anforderung, ihre Autorisierung sowie nach der Genehmigung aufgetretene Ereignisse identifiziert.

-   **Anpassbarer Workflow**: Die MIM-Workflows können für unterschiedliche Szenarien konfiguriert werden. Basierend auf den Parametern des anfordernden Benutzers bzw. der angeforderten Rollen können mehrere Workflows verwendet werden.

## <a name="how-do-users-request-privileged-access"></a>Wie können Benutzer einen privilegierten Zugriff anfordern?
Es gibt verschiedene Möglichkeiten für Benutzer, eine Anforderung zu übermitteln, z. B. die folgenden:  
- Die Webdienste-API für die MIM-Dienste  
- Ein REST-Endpunkt  
- Windows PowerShell (`New-PAMRequest`)

Details zu [Privileged Access Management-Cmdlets](https://technet.microsoft.com/library/mt604080.aspx).

## <a name="what-workflows-and-monitoring-options-are-available"></a>Welche Workflows und Überwachungsoptionen stehen zur Verfügung?
Lassen Sie uns als Beispiel annehmen, dass ein Benutzer Mitglied einer administrativen Gruppe war, bevor PIM eingerichtet wurde. Als Teil der Einrichtung von PIM werden der Benutzer aus der administrativen Gruppe entfernt und eine Richtlinie in MIM erstellt. Die Richtlinie gibt an, dass wenn dieser Benutzer Administratorrechte anfordert und mittels MFA authentifiziert wird, die Anforderung genehmigt und ein gesondertes Konto für den Benutzer der privilegierten Gruppe in der geschützten Gesamtstruktur hinzugefügt wird.

Bei Genehmigung der Anforderung kommuniziert der Aktionsworkflow direkt mit der geschützten Active Directory-Gesamtstruktur, um einen Benutzer einer Gruppe hinzuzufügen. Wenn die Benutzerin Jen beispielsweise die Verwaltung der Datenbank der Personalabteilung anfordert, wird das Administratorkonto für Jen binnen Sekunden der privilegierten Gruppe in der Gesamtstruktur hinzugefügt. Ihre Mitgliedschaft im Administratorkonto läuft nach einem festgelegten Zeitraum ab. Unter Windows Server Technical Preview ist diese Gruppenmitgliedschaft in Active Directory einer Frist zugeordnet. Bei Windows Server 2012 R2 in der geschützten Gesamtstruktur wird diese Frist von MIM erzwungen.

> [!NOTE]
> Wenn Sie einer Gruppe ein neues Mitglied hinzufügen, muss die Änderung auf andere Domänencontroller (DCs) in der geschützten Gesamtstruktur repliziert werden. Die Replikationslatenz kann sich bei Benutzern auf den Ressourcenzugriff auswirken. Weitere Informationen zur Replikationslatenz finden Sie im Artikel [How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx) (Funktionsweise der Active Directory-Replikationstopologie).
>
> Ein abgelaufener Link dagegen wird von der Sicherheitskontenverwaltung (Security Accounts Manager, SAM) in Echtzeit ausgewertet. Obwohl das Hinzufügen eines Gruppenmitglieds vom Domänencontroller, der die Zugriffsanforderung empfängt, repliziert werden muss, wird das Entfernen eines Gruppenmitglieds sofort auf einem beliebigen Domänencontroller ausgewertet.

Dieser Workflow ist speziell für diese Administratorkonten vorgesehen. Administratoren (oder sogar Skripts), die nur gelegentlich Zugriff auf privilegierte Gruppen benötigen, können genau diesen Zugriff anfordern. MIM protokolliert die Anforderung und die Änderungen in Active Directory. Sie können die Daten in der Ereignisanzeige anzeigen oder an Lösungen für die Unternehmensüberwachung wie System Center 2012 - Operations Manager-Überwachungssammeldienste oder an Drittanbietertools senden.



<!--HONumber=Nov16_HO2-->


