---
title: Installation des BHOLD-Modellgenerators | Microsoft-Dokumentation
description: Das BHOLD-Modell ermöglicht Ihnen das Strukturieren von Daten aus verschiedenen Quellen.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 90e7da2a1e39b802723ff0714bd0caccf9649440
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289134"
---
# <a name="bhold-model-generator-installation"></a>Installation des BHOLD-Modellgenerators

Mithilfe des BHOLD-Modellgeneratormoduls können Sie Daten aus autoritativen Quellen, die Informationen zu den Benutzern und der Organisation sowie Zugriffssteuerungslisten (ACLs) enthalten, in einem Modell strukturieren, das für die Verwaltung von BHOLD verwendet werden kann.

## <a name="bhold-model-generator-installation-requirements"></a>BHOLD-Modellgenerator – Installationsanforderungen 

Vor der Installation des BHOLD-Modellgeneratormoduls müssen Sie Folgendes installieren:

1. Das BHOLD Core-Modul auf dem Server, auf dem Sie das BHOLD-Modellgeneratormodul installieren möchten. Weitere Informationen zum Installieren des BHOLD Core-Moduls finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Der Microsoft OLE DB-Anbieter für Microsoft Jet muss installiert sein. Weitere Informationen finden Sie in [diesem Artikel](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Installieren Sie den BHOLD-Modellgenerator nicht in Ihrem Produktionsnetzwerk. Der BHOLD-Modellgenerator ist dafür konzipiert, offline in einer Stagingumgebung verwendet zu werden, um ein normalisiertes Rollenmodell zu erstellen, das Sie in das Rollenmodell Ihres Unternehmens importieren können. Das Ausführen des BHOLD-Modellgenerators in Ihrem Produktionsnetzwerk kann zum Verlust Ihres vorhandenen Rollenmodells führen.

## <a name="before-you-begin"></a>Vorbereitung

Bevor Sie das BHOLD-Modellgeneratormodul installieren, müssen Sie darauf vorbereitet sein, die Informationen bereitzustellen, die der Assistent für das Setup des BHOLD-Modellgenerators erfordert, um die Installation abzuschließen. Das folgende Arbeitsblatt unterstützt Sie beim Aufzeichnen dieser Informationen, sodass Sie diese bereitstellen können, wenn sie benötigt werden. Stellen Sie ebenfalls Folgendes sicher:

Microsoft Access Database Engine 2010 Redistributable

 

*Von \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Kontoeinstellungen**

| **Element**                                    | **Beschreibung**                                                                                                                                                                                                           | **Wert**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden eines Sicherheitsanbieters auf einer Domäne/einem Computer** | Bei der Auswahl dieser Option wird angegeben, dass die Active Directory Domain Services-Sicherheit den Zugriff auf BHOLD Core steuert.                                                                                                                | Aktivieren Sie das Kontrollkästchen. **Wichtig:** Die Installation schlägt fehl, wenn dieses Kontrollkästchen nicht aktiviert ist.                                                                                                                                                                                                                   |
| **Domäne**                                  | Gibt die Domäne an, die das Dienstkonto enthält, das Sie bei der Installation von BHOLD Core erstellt haben. Weitere Informationen finden Sie unter [BHOLD Core Installation (Installation von BHOLD Core)](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Der Domänenname wird automatisch vom Assistenten bereitgestellt. Ändern Sie den Namen nur, wenn dieser falsch ist. **Wichtig:** Geben Sie den Domänennamen an, indem Sie den kurzen NetBIOS-Namen verwenden, nicht den vollqualifizierten Domänennamen (FQDN). Wenn der FQDN der Domäne beispielsweise „fabrikam.com“ ist, geben Sie den Domänennamen als „FABRIKAM“ an. |
| **Benutzer**                                    | Gibt den Anmeldenamen des BHOLD Core-Dienstbenutzerkontos an.                                                                                                                                                          | Geben Sie den Benutzerkontonamen hier ein:                                                                                                                                                                                                                                                                                    |
| **Passwort**                                | Gibt das Kennwort des Dienstbenutzerkontos an.                                                                                                                                                                       | Geben Sie das Kennwort hier ein: **Wichtig:** Achten Sie darauf, dieses Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                  |

**Einstellungen für die Datenbanksicherung**

| Element                                        | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                  | Wert                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Verwenden der integrierten Sicherheit**                 | Gibt an, dass die Windows-Authentifizierung für den Zugriff auf die Datenbank verwendet wird.                                                                                                                                                                                                                                                                                                                                                        | Aktivieren Sie das Kontrollkästchen, wenn die Windows-Authentifizierung für das Verbinden mit SQL Server verwendet wird. Deaktivieren Sie das Kontrollkästchen, wenn die SQL Server-Authentifizierung verwendet wird. Wenn die SQL Server-Authentifizierung verwendet wird, muss die Datenbank erstellt werden, bevor das BHOLD Core-Setup ausgeführt wird. **Hinweis:** Wenn die Windows-Authentifizierung verwendet wird, müssen Sie mit dem Konto angemeldet sein, das die Serverrolle „Systemadministrator“ auf dem Datenbankserver besitzt. **Wichtig:** Verwenden Sie die SQL Server-Authentifizierung nur in Testumgebungen. Microsoft empfiehlt dringend, die Windows-Authentifizierung in Produktionsumgebungen zu verwenden. |
| **Datenbankbenutzer** und **Datenbankkennwort** | Gibt den Benutzernamen und das Kennwort eines Benutzers mit der Serverrolle „Systemadministrator“ auf dem Datenbankserver an. Diese Werte werden nur bereitgestellt, wenn die SQL Server-Authentifizierung verwendet wird.                                                                                                                                                                                                                                                  | Geben Sie den SQL Server-Benutzernamen hier ein: Geben Sie das SQL Server-Benutzerkennwort hier ein: </br></br> **Wichtig:** Achten Sie darauf, das Kennwort an einem verborgenen, sicheren Ort aufzubewahren.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Datenbankserver** und **Datenbankname**   | Gibt den NetBIOS-Namen des Datenbankservers und den Namen der Sicherungsdatenbank ein, die das Setup des BHOLD-Modellgenerators erstellt. Wenn Sie nicht die Standardinstanz des Datenbankservers verwenden, geben Sie die Instanz des Datenbankservers in der Form *\<Server\>*\\*\<Instanz\>* an.  Microsoft empfiehlt, dass Sie für die Benennung der Sicherungsdatenbank den Namen der BHOLD Core-Datenbank gefolgt von \_BACKUP verwenden, z.B. B1_BACKUP. | Geben Sie den Servernamen (oder Server- und Instanznamen) hier ein: </br> Geben Sie den Datenbanknamen hier ein:

## <a name="bhold-model-generator-setup"></a>Setup des BHOLD-Modellgenerators

Melden Sie sich zum Installieren des BHOLD-Modellgeneratormoduls als Mitglied der Gruppe „Domänenadministratoren“ an, laden Sie folgende Datei herunter, und führen Sie diese als Administrator auf dem Server aus, auf dem Sie das BHOLD Core-Modul installieren möchten:

- BholdModelGenerator *\<Version\>*\_Release.msi

Ersetzen Sie *\<Version\>* durch die Versionsnummer der Version des BHOLD-Modellgenerators, die Sie installieren.

Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie dann auf **Als Administrator ausführen**, um die Programmdatei als Administrator auszuführen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen der Eingabedateien finden Sie unter [Microsoft BHOLD Suite Technical Reference (Technische Referenz zu Microsoft BHOLD Suite)](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
- [BHOLD-Installationshandbuch](bhold-installation-guide.md)
- [BHOLD-Entwicklerreferenz](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-Versionsverlauf](../reference/version-bhold-history.md)