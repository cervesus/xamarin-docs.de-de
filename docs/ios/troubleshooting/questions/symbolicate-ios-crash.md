---
title: Wo finde ich die .dSYM Datei iOS Absturz Protokolle symbolicate?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33920654"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Wo finde ich die .dSYM Datei iOS Absturz Protokolle symbolicate?

Beim Erstellen einer iOS-app mit Visual Studio für Mac oder Visual Studio 2017 werden die .dSYM-Datei, die benötigt wurde, um Absturzberichte symbolicate in derselben Verzeichnishierarchie wie die app-Projektdatei (csproj) platziert. Der exakte Speicherort hängt von Buildeinstellungen des Projekts ab:

- Wenn Sie gerätespezifische Builds aktiviert haben die die .dSYM befinden sich im folgenden Verzeichnis:

    **&lt;Projektverzeichnis&gt;/bin/&lt;Plattform&gt;/&lt;Konfiguration&gt;/device-builds /&lt;Gerät&gt; - &lt; Version des Betriebssystems&gt;/**

    Zum Beispiel:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Wenn Sie gerätespezifische Builds nicht aktiviert haben, können die .dSYM im folgenden Verzeichnis gefunden:

    **&lt;Projektverzeichnis&gt;/bin/&lt;Plattform&gt;/&lt;Konfiguration&gt;/**

    Zum Beispiel:

    **TestApp/Bin/iPhone/Freigabe /**

> [!NOTE]
> Im Rahmen des Buildprozesses kopiert Visual Studio 2017 die .dSYM-Datei zwischen dem Mac-Build-Host und Windows. Wenn Sie eine Datei .dSYM unter Windows nicht angezeigt werden, werden Sie sicher, dass Sie die Buildeinstellungen zu Ihrer app konfiguriert haben [erstellen Sie eine IPA-Datei](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Siehe auch

- [Symbolicating iOS Crash-Dateien (Xamarin.iOS)](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS stürzt ab, Anwendungsprotokolle](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

