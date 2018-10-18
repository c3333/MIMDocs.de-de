---
title: MIM-Dienst – Dynamische Protokollierung | Microsoft-Dokumentation
description: Aktivieren der dynamischen Protokollierung des MIM-Diensts ohne den Verwaltungsdienst erneut starten zu müssen
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332336"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Dynamische Protokollierung des MIM SP1-Diensts (4.4.1436.0)
In 4.4.1436.0 haben wir eine neue Protokollierungsfunktion eingeführt. Dadurch können Administratoren und Supporttechniker die Protokollierung aktivieren, ohne den Verwaltungsdienst neu starten zu müssen.

Nach der Installation sehen Sie die folgenden neuen Zeile in der Datei Microsoft.ResourceManagement.Service.exe.config, die wie folgt heißen:

*   Zeile 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Zeile 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Zeile 266: ``</system.diagnostics> ``

![Die hervorgehobenen Abschnitte zeigen die neuen Einträge der dynamischen Protokollierung.](media/mim-service-dynamic-logging/screen01.png)

Protokollierungsstufen der dynamischen Protokollierung finden Sie [hier](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3).

- Critical = Der Standardebenendienst schreibt nur kritische Ereignisse
- Aktualisieren von Zeile 8 (dynamicLogging mode="true" loggingLevel="Critical") mit bevorzugtem Protokollierwert

Die config-Datei der dynamischen Protokollierung befindet sich in Zeile 266: „Microsoft.ResourceManagement.Service.exe.config“

![Hervorgehobene Abschnitte zeigen die Zeilen mit den verschiedenen verfügbaren Protokollierungsbereichen](media/mim-service-dynamic-logging/screen02.png)

Standardmäßig finden Sie den Protokollierungsbereich unter C:\Programme\Microsoft Forefront Identity Manager\2010\Service. Das FIM Service-Konto benötigt Schreibberechtigungen für diesen Speicherort, um das dynamische Protokoll generieren zu können.

![Speicherort des Ordners der Protokollierungen](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Bei einem unerwarteten Fehler (Syntaxfehler in der config-Datei „Microsoft.ResourceManagement.Service.exe.config“ oder andere Fehler) werden entsprechende Fehlermeldung in die Datei „Microsoft.ResourceManagement.Service.exe_Emergency.log“ unter folgenden Pfad geschrieben: %TMP% oder %TEMP% oder %USERPROFILE% (der erste, der vorhanden ist).  
> 1. „%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log“
> 2. „%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log“
> 3. „%USERPROFILE%\Microsoft.ResourceManagement.Service.exe_Emergency.log“

Um die Nachverfolgung anzuzeigen, können Sie das [Service Trace Viewer-Tool](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) verwenden.

 ![Screenshot des Service Trace Viewer-Tools](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Updates: Build 4.5.x.x oder höher

Im Build 4.5.x.x wurde das Protokollierungsfeature überarbeitet, sodass der Standardprotokolliergrad **„Warnung“** lautet. Der Dienst schreibt Meldungen in zwei Dateien („00“- und „01“-Indizes werden vor der Erweiterung hinzugefügt). Die Dateien befinden sich im Verzeichnis „C:\Programme\Microsoft Forefront Identity Manager\2010\Service“. Wenn die Datei die maximale Größe überschreitet, beginnt der Dienst, in eine andere Datei zu schreiben. Wenn eine andere Datei vorhanden ist, wird sie überschrieben. Die standardmäßige maximale Größe der Datei ist 1 GB. Um die standardmäßige maximale Größe zu ändern, muss der Parameter **maxOutputFileSizeKB** mit dem Wert der maximalen Dateigröße in KB dem Listener hinzugefügt werden (siehe folgendes Beispiel) und der MIM-Dienst neu gestartet werden. Wenn der Dienst gestartet wird, fügt er der neueren Datei Protokolle hinzu (bei Überschreitung der Speicherplatzbegrenzung wird die älteste Datei überschreiben). 

> [!NOTE] Wie die Größe der Dienstüberprüfungsdatei vor dem Schreiben der Nachricht könnte die Größe der Datei über der maximale Größe für die Größe einer Nachricht liegen. Standardmäßig können die Protokolle ungefähr 6 GB groß sein (drei Listener mit zwei Dateien mit einer Größe von 1 GB).

> [!NOTE] Das Dienstkonto benötigt Berechtigungen zum Schreiben in das Verzeichnis „C:\Programme\Microsoft Forefront Identity Manager\2010\Service“. Falls das Dienstkonto nicht über solche Rechte verfügt, werden die Dateien nicht erstellt.

Beispiel zum Festlegen der maximalen Dateigröße auf 200 MB (200 * 1.024 KB) für SVCLOG-Dateien und 100 MB (100 * 1.024 KB) für TXT-Dateien

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
