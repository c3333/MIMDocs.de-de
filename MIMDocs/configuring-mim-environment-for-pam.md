---
Titel: Konfigurieren der MIM-Umgebung für die privilegierte Zugriffsverwaltung
MS.Custom: 
  - Identitätsmanagement
  - MIM
MS.Prod: Identität-Manager-2015
MS.Reviewer: Na
MS.Suite: Na
MS.Technology: 
  - security
MS.tgt_pltfrm: Na
MS.topic: Artikel
MS.AssetId: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
Autor: Kgremban
---
# Konfigurieren der MIM-Umgebung für die privilegierte Zugriffsverwaltung
Das Einrichten der Umgebung für den gesamtstrukturübergreifenden Zugriff umfasst sieben Schritte, das Installieren und Konfigurieren von Active Directory und Microsoft Identity Manager sowie die Veranschaulichung einer bedarfsorientierten Zugriffsanforderung.

1.  Bereiten Sie den Server *CORPDC* als Domänencontroller und *CORPWKSTN* als Mitglied der Domäne/Arbeitsgruppe vor.

2.  Bereiten Sie den Server *PRIVDC* als Domänencontroller vor.

3.  Bereiten Sie den Server *PAMSRV* in der Gesamtstruktur *PRIV* vor.

4.  Installieren Sie MIM-Komponenten auf *PAMSRV* und die Cmdlets auf einem Mitglied der Domäne/Arbeitsgruppe der Gesamtstruktur *CONTOSO* , und bereiten Sie diese dann für die privilegierte Zugriffsverwaltung vor.

5.  Richten Sie eine Vertrauensstellung zwischen den Gesamtstrukturen *PRIV* und *CONTOSO* ein.

6.  Vorbereiten von privilegierten Sicherheitsgruppen mit Zugriff auf geschützte Ressourcen und Mitgliedskonten für die bedarfsorientierte privilegierte Zugriffsverwaltung.

7.  Veranschaulichen Sie die Anforderung, den Erhalt und die Verwendung des privilegierten Zugriffs mit Rechteerweiterung auf eine geschützte Ressource.

In den folgenden Abschnitten finden Sie Details zum Ausführen dieser Aufgaben.

<!--HONumber=Mar16_HO1-->
