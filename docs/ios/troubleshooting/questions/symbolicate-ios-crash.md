---
title: Wo finde ich die dsym-Datei auf iOS-absturzprotokolle durch Symbole?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/09/2018
ms.openlocfilehash: 0b8f3aa736cba6e70fbf346766499c23a9bbe270
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114066"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Wo finde ich die dsym-Datei auf iOS-absturzprotokolle durch Symbole?

Beim Erstellen einer iOS-app mit Visual Studio für Mac oder Visual Studio 2017 wird die dsym-Datei, die erforderlich sind, um Absturzberichte zu symbolifizieren in der gleichen Verzeichnishierarchie Ihrer app-Projektdatei (.csproj) platziert werden. Der exakte Speicherort hängt von Buildeinstellungen Ihres Projekts:

- Wenn Sie gerätespezifische Builds aktiviert haben, können die dsym im folgenden Verzeichnis befinden:

    **&lt;Projektverzeichnis&gt;/bin /&lt;Plattform&gt;/&lt;Konfiguration&gt;/device-builds /&lt;Gerät&gt; - &lt; Version des Betriebssystems&gt;/**

    Zum Beispiel:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Wenn Sie keine gerätespezifische Builds aktiviert haben, können die dsym im folgenden Verzeichnis befinden:

    **&lt;Projektverzeichnis&gt;/bin /&lt;Plattform&gt;/&lt;Konfiguration&gt;/**

    Zum Beispiel:

    **TestApp/Bin/iPhone/Release /**

> [!NOTE]
> Im Rahmen des Buildprozesses kopiert Visual Studio 2017 die dsym-Datei, aus der Mac-buildhost zu Windows. Wenn Sie eine dsym-Datei auf dem Windows nicht angezeigt werden, werden Sie sicher, dass Sie die Buildeinstellungen so konfiguriert, Ihrer app konfiguriert haben [erstellen Sie eine IPA-Datei](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Siehe auch

- [Symbolicating iOS abstürzen Dateien (Xamarin.iOS)](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Die Enträtselung der iOS-Crash-Anwendungsprotokolle](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

