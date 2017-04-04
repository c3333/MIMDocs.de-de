---
title: "MIM-Dienst – Dynamische Protokollierung | Microsoft-Dokumentation"
description: "Aktivieren der dynamischen Protokollierung des MIM-Diensts ohne den Verwaltungsdienst erneut starten zu müssen"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 90a0f144b7674bbfaf13138dfd926dbfc3c74f28
ms.openlocfilehash: ddd707210d5cd6b618709a477d40e7771d73cfa1
ms.lasthandoff: 03/27/2017



---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Dynamische Protokollierung des MIM SP1-Diensts (4.4.1436.0)
In 4.4.1436.0 haben wir eine neue Protokollierungsfunktion eingeführt. Dadurch können Administratoren und Supporttechniker die Protokollierung aktivieren, ohne den Verwaltungsdienst neu starten zu müssen.

Nach der Installation sehen Sie die folgenden neuen Zeile in der Datei Microsoft.ResourceManagement.Service.exe.config, die wie folgt heißen:

*    Zeile 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*    Zeile 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*    Zeile 266: ``</system.diagnostics> ``

![Die hervorgehobenen Abschnitte zeigen die neuen Einträge der dynamischen Protokollierung.](/media/mim-service-dynamic-logging/screen01.png)

Protokollierungsstufen der dynamischen Protokollierung finden Sie [hier](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3).

- Critical = Der Standardebenendienst schreibt nur kritische Ereignisse
- Aktualisieren von Zeile 8 (dynamicLogging mode="true" loggingLevel="Critical") mit bevorzugtem Protokollierwert

Die config-Datei der dynamischen Protokollierung befindet sich in Zeile 266: „Microsoft.ResourceManagement.Service.exe.config“

![Hervorgehobene Abschnitte zeigen die Zeilen mit den verschiedenen verfügbaren Protokollierungsbereichen](/media/mim-service-dynamic-logging/screen02.png)

Standardmäßig finden Sie den Protokollierungsbereich unter **C:\Programme\Microsoft Forefront Identity Manager\2010\Service**. Das FIM Service-Konto benötigt Berechtigungen für diesen Speicherort, um das dynamische Protokoll generieren zu können.

![Speicherort des Ordners der Protokollierungen](/media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 Bei einem unerwarteten Fehler (Syntaxfehler in der config-Datei „Microsoft.ResourceManagement.Service.exe.config“ oder andere Fehler) werden entsprechende Fehlermeldung in die Datei „Microsoft.ResourceManagement.Service.exe_Emergency.log“ unter folgenden Pfad geschrieben: %TMP% oder %TEMP% oder %USERPROFILE% (der erste, der vorhanden ist).  
1. „%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log“
2. „%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log“
3. „%USERPROFILE%\Microsoft.ResourceManagement.Service.exe_Emergency.log“

Um die Nachverfolgung anzuzeigen, können Sie das [Service Trace Viewer-Tool](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) verwenden.

 ![Screenshot des Service Trace Viewer-Tools](/media/mim-service-dynamic-logging/screen04.png)

