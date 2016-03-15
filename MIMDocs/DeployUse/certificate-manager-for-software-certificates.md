---
Titel: Zertifikat-Manager für Softwarezertifikate
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
MS.AssetId: fed5ada9-d80f-4825-aad7-4172ac5d71d3
Autor: Kgremban
---
# Zertifikat-Manager für Softwarezertifikate
Zum Registrieren und Erneuern von Softwarezertifikaten müssen Sie kein Administrator sein und es ist keine virtuelle Smartcard erforderlich. Beachten Sie, dass Sie im Verlauf dazu aufgefordert werden, einen Zertifikatvorgang zuzulassen. Das ist normal.

## Erstellen einer Softwarezertifikat-Profilvorlage im MIM 2016 Zertifikat-Manager

1.  Erstellen Sie eine Vorlage für das Zertifikat, das Sie für die virtuelle Smartcard anfordern werden. Öffnen Sie die MMC.

2.  Klicken Sie auf **Datei**, und klicken Sie dann auf **Snap-In hinzufügen/entfernen**.

3.  Klicken Sie in der Liste Verfügbare-Snap-ins auf **Zertifikatvorlagen**, und klicken Sie dann auf **Hinzufügen**.

4.  **Zertifikatvorlagen** wird in der MMC jetzt unter „Konsolenstamm“ angezeigt. Doppelklicken Sie auf diesen Eintrag, um alle verfügbaren Zertifikatvorlagen anzuzeigen.

5.  Mit der rechten Maustaste die **Benutzervorlage**, und klicken Sie auf **Doppelte Vorlage**.

6.  Auf der **Kompatibilität** Registerkarte unter Certification Authority wählen Sie Windows Server 2008 und Zertifikatempfänger wählen Sie Windows 8.1 / Windows Server 2012 R2.

    1.  Geben Sie auf der Registerkarte **Allgemein** im Feld „Anzeigename“ die Zeichenfolge **Archivierte Zertifikatvorlage**ein.

    2.  b.  Auf der Registerkarte **Anforderungsbehandlung** :

        1.  Legen Sie für **Zweck** „Signatur und Verschlüsselung“ fest.

        2.  Aktivieren Sie die Option **Vom Antragsteller zugelassene symmetrische Algorithmen einbeziehen**.

        3.  Wenn Sie den Schlüssel archivieren möchten, aktivieren Sie die Option **Privaten Schlüssel für die Verschlüsselung archivieren**.

        4.  Wählen Sie unter „Gehen Sie folgendermaßen vor...“ die Option **Benutzer zur Eingabe während der Registrierung auffordern**aus.

    3.  Auf der Registerkate **Kryptografie** :

        1.  Wählen Sie unter „Anbieterkategorie“ die Option **Schlüsselspeicheranbieter**aus.

        2.  Wählen Sie **Verwendung aller auf dem Computer des Antragstellers verfügbaren Anbieter für Anforderungen möglich**aus.

    4.  Fügen Sie auf der Registerkarte **Sicherheit** die Sicherheitsgruppe hinzu, der Sie den **Registrieren** -Zugriff zuweisen möchten. Wenn Sie beispielsweise allen Benutzern Zugriff gewähren möchten, wählen Sie die Gruppe **Authentifizierte Benutzer** und dann die Berechtigung **Registrieren** für sie aus.

    5.  Auf der Registerkarte **Antragstellername** :

        1.  Deaktivieren Sie **einschließen e-Mail-Name im Antragstellernamen**.

        2.  Deaktivieren Sie unter **Informationen im alternativen Antragstellernamen einbeziehen**die Option **E-Mail-Name**.

    6.  Klicken Sie auf **OK** , um die von Ihnen vorgenommenen Änderungen abzuschließen und die neue Vorlage zu erstellen. Ihre neue Vorlage sollte jetzt in der Liste der Zertifikatvorlagen angezeigt werden.

    7.  Wählen Sie **Datei**, klicken Sie dann auf **Snap-In hinzufügen/entfernen** das Zertifizierungsstellen-Snap-in zur MMC-Konsole hinzufügen. Wenn Sie gefragt werden, welchen Computer Sie verwalten möchten, wählen Sie **Lokaler Computer**aus.

    8.  Erweitern Sie im linken Bereich der MMC den Eintrag **Zertifizierungsstelle (lokal)**, und erweitern Sie dann Ihren Zertifikat-Manager in der Liste „Zertifizierungsstelle“.

    9. Mit der rechten Maustaste **Zertifikatvorlagen**, klicken Sie auf **Neu**, und klicken Sie dann auf **Zertifikatvorlage** Problem.

    10. Wählen Sie aus der Liste die neue Vorlage, die Sie soeben erstellt haben (**Archivierte Zertifikatvorlage**), und klicken Sie dann auf **OK**.

## Erstellen der Profilvorlage

1.  Melden Sie sich beim CM-Portal (Zertifikatverwaltung) als Benutzer mit Administratorrechten an.

2.  Wechseln Sie zu **Verwaltung &gt; Profilvorlagen verwalten** , und stellen Sie sicher, dass das Kontrollkästchen neben **MIM CM-Beispielsmartcard-Profilvorlage für Anmeldung** aktiviert ist, und klicken Sie dann auf **Ausgewählte Profilvorlage kopieren**.

3.  Geben Sie den Namen der Profilvorlage ein, und klicken Sie auf **OK**.

4.  Klicken Sie in der nächsten Anzeige auf **Neue Zertifikatvorlage hinzufügen** , und aktivieren Sie das Kontrollkästchen neben dem Namen der Zertifizierungsstelle.

5.  Aktivieren Sie das Kontrollkästchen neben dem Namen des archivierten Softwarezertifikats, und klicken Sie auf **Hinzufügen**.

6.  Entfernen Sie die Benutzervorlage, indem Sie das Kontrollkästchen neben diesem Eintrag aktivieren und dann auf **Ausgewählte Zertifikatvorlagen löschen** sowie auf **OK**klicken.

7.  Klicken Sie auf **Allgemeine Einstellungen ändern**.

8.  Aktivieren Sie die Kontrollkästchen auf der linken Seite von **Verschlüsselungsschlüssel auf dem Server generieren** , und klicken Sie auf **OK**. Klicken Sie im linken Bereich auf **Wiederherstellungsrichtlinie**.

9. Klicken Sie auf **Allgemeine Einstellungen ändern**.

10. Wenn Sie archivierte Zertifikate neu ausstellen möchten, aktivieren Sie die Kontrollkästchen auf der linken Seite von **Archivierte Zertifikate erneut ausstellen** , und klicken Sie auf **OK**.

11. Wenn Sie die Zertifikatsverwaltung mithilfe der virtuellen Smartcard verwenden, müssen Sie Datensammlungselemente deaktivieren, da diese nicht mit aktivierter Datensammlung funktioniert. Deaktivieren Sie die Datensammlung für jede Richtlinie, indem Sie im linken Bereich auf die Richtlinie klicken, dann das Kontrollkästchen neben **Beispieldatenelement** deaktivieren und schließlich auf **Datensammlungselemente löschen**klicken. Klicken Sie dann auf **OK**.

<!--HONumber=Mar16_HO1-->
