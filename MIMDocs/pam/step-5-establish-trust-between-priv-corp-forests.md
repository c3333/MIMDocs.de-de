---
title: "Bereitstellen von PAM Schritt 5 – Forest link | Microsoft Identity Manager"
description: "Richten Sie eine Vertrauensstellung zwischen den Gesamtstrukturen von PRIV und CORP ein, sodass berechtigte Benutzer in PRIV weiterhin auf CORP-Ressourcen zugreifen können."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 3a7039f5d7c950cd0d4c8ab713a7beacc5c45526


---

# Schritt 5 – Einrichten einer Vertrauensstellung zwischen den Gesamtstrukturen PRIV und CORP

>[!div class="step-by-step"]
[« Schritt 4](step-4-install-mim-components-on-pam-server.md)
[Schritt 6 »](step-6-transition-group-to-pam.md)


Für jede CORP-Domäne, z. B. „contoso.local“, müssen die PRIV- und CONTOSO-Domänencontroller durch eine Vertrauensstellung gebunden werden. Dadurch können Benutzer in der PRIV-Domäne auf Ressourcen in der CORP-Domäne zugreifen.

## Verbinden jedes Domänencontrollers mit seinem Pendant

Vor dem Einrichten von Vertrauensstellungen muss jeder Domänencontroller für sein Pendant basierend auf der IP-Adresse des anderen Domänencontrollers/DNS-Servers für die DNS-Namensauflösung konfiguriert werden.

1.  Wenn die Domänencontroller oder der Server mit der MIM-Software als virtuelle Computer bereitgestellt werden, stellen Sie sicher, dass keine anderen DNS-Server Domain Name Services für diese Computer bereitstellen.
    - Wenn die virtuellen Computer über mehrere Netzwerkschnittstellen verfügen, einschließlich Netzwerkschnittstellen, die mit öffentlichen Netzwerken verbunden sind, müssen Sie diese Verbindungen möglicherweise vorübergehend deaktivieren oder die Windows-Netzwerkschnittstellen-Einstellungen überschreiben. Es ist wichtig, sicherzustellen, dass eine DHCP-bereitgestellte DNS-Serveradresse nicht von virtuellen Computern verwendet wird.

2.  Stellen Sie sicher, dass jeder vorhandene CORP-Domänencontroller Namen an die PRIV-Gesamtstruktur weiterleiten kann. Starten Sie PowerShell auf jedem Domänencontroller außerhalb der PRIV-Gesamtstruktur, z. B. CORPDC, und geben Sie den folgenden Befehl ein:

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    Überprüfen Sie, ob die Ausgabe einen Namenserverdatensatz für die PRIV-Domäne mit der richtigen IP-Adresse anzeigt.

3.  Wenn der Domänencontroller die PRIV-Domäne nicht weiterleiten kann, verwenden Sie den **DNS-Manager** (in **Start** > **Anwendungstools** > **DNS**), um die DNS-Namenweiterleitung für die PRIV-Domäne an die PRIVDC-IP-Adresse zu konfigurieren. Wenn dies eine übergeordnete Domäne ist (z. B. „contoso.local“), erweitern Sie die Knoten für diesen Domänencontroller und seine Domäne, z. B. **CORPDC** > **Forward-Lookupzonen** > **contoso.local**, und vergewissern Sie sich, dass ein Schlüssel namens **priv** als Namenservertyp (NS) vorhanden ist.

    ![Dateistruktur für Priv-Schlüssel – Screenshot](./media/PAM_GS_DNS_Manager.png)

## Einrichten einer Vertrauensstellung auf PAMSRV

Richten Sie auf PAMSRV eine unidirektionale Vertrauensstellung mit jeder Domäne wie CORPDC ein, damit die CORP-Domänencontroller der PRIV-Gesamtstruktur vertrauen.

1. Melden Sie sich bei PAMSRV als PRIV-Domänenadministrator (PRIV\Administrator) an.

2.  Starten Sie PowerShell.

3.  Geben Sie die folgenden PowerShell-Befehle für jede vorhandene Gesamtstruktur ein. Geben Sie nach Aufforderung die Anmeldeinformationen für den CORP-Domänenadministrator (CONTOSO\Administrator) ein.

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Geben Sie die folgenden PowerShell-Befehle für jede Domäne in den vorhandenen Gesamtstrukturen ein. Geben Sie nach Aufforderung die Anmeldeinformationen für den CORP-Domänenadministrator (CONTOSO\Administrator) ein.

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## Erteilen von Lesezugriff auf Active Directory für Gesamtstrukturen

Aktivieren Sie für jede vorhandene Gesamtstruktur den Lesezugriff auf AD für PRIV-Administratoren und den Überwachungsdienst.

1.  Melden Sie sich bei dem vorhandenen CORP-Gesamtstrukturdomänencontroller (CORPDC) als Domänenadministrator für die Domäne der obersten Ebene in dieser Gesamtstruktur an (Contoso\Administrator).  
2.  Starten Sie **Active Directory-Benutzer und -Computer**.  
3.  Klicken Sie mit der rechten Maustaste auf die Domäne **contoso.local**, und wählen Sie **Objektverwaltung zuweisen** aus.  
4.  Klicken Sie auf der Registerkarte „Ausgewählte Benutzer und Gruppen“ auf **Hinzufügen**.  
5.  Klicken Sie im Fenster zur Auswahl von Benutzern, Computern oder Gruppen auf **Speicherorte** , und ändern Sie den Speicherort in *priv.contoso.local*.  Geben Sie für den Objektnamen *Domänen-Admins* ein, und klicken Sie dann auf **Namen überprüfen**. Wenn ein Popupfenster angezeigt wird, geben Sie den Benutzernamen *priv\administrator* sowie das Kennwort ein.  
6.  Fügen Sie hinter „Domänen-Admins“ die Zeichenfolge „*; MIMMonitor*“ hinzu. Nachdem die Namen **Domänen-Admins** und **MIMMonitor** unterstrichen sind, klicken Sie auf **OK** und dann auf **Weiter**.  
7.  Wählen Sie in der Liste der allgemeinen Aufgaben **Liest alle Benutzerinformationen** aus, und klicken Sie auf **Weiter** und dann auf **Fertig stellen**.  
8.  Schließen Sie %%amp;quot;Active Directory-Benutzer und -Computer%%amp;quot;.

9.  Öffnen Sie ein PowerShell-Fenster.  
10.  Verwenden Sie `netdom`, um sicherzustellen, dass der SID-Verlauf aktiviert und die SID-Filterung deaktiviert ist. Typ:  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    Die Ausgabe muss entweder **Der SID-Verlauf für diese Vertrauensstellung wird aktiviert** oder **Der SID-Verlauf ist für diese Vertrauensstellung bereits aktiviert** lauten.

    Die Ausgabe sollte auch **Für diese Vertrauensstellung ist keine SID-Filterung aktiviert** angeben. Weitere Informationen finden Sie unter [Disable SID filter quarantining](http://technet.microsoft.com/library/cc772816.aspx) (Deaktivieren der SID-Filterquarantäne).

## Starten der Überwachungs- und Komponentendienste

1.  Melden Sie sich bei PAMSRV als PRIV-Domänenadministrator (PRIV\Administrator) an.

2.  Starten Sie PowerShell.

3.  Geben Sie die folgenden PowerShell-Befehle ein.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

Im nächsten Schritt werden Sie eine Gruppe in PAM verschieben.

>[!div class="step-by-step"]
[« Schritt 4](step-4-install-mim-components-on-pam-server.md)
[Schritt 6 »](step-6-transition-group-to-pam.md)



<!--HONumber=Jul16_HO3-->

