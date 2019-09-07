---
title: Wo finde ich die DSYM-Datei zum Ersetzen der iOS-Absturzprotokolle durch Symbole?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: cc681e94187530c5442ad4fb12557017b92b2bb4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769220"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Wo finde ich die DSYM-Datei zum Ersetzen der iOS-Absturzprotokolle durch Symbole?

Beim Entwickeln einer IOS-App mit Visual Studio für Mac oder Visual Studio 2017 wird die dsym-Datei, die zum symbolisieren von Absturzberichten benötigt wird, in derselben Verzeichnishierarchie platziert wie die Projektdatei Ihrer APP (. csproj). Der genaue Speicherort hängt von den Buildeinstellungen des Projekts ab:

- Wenn Sie gerätespezifische Builds aktiviert haben, finden Sie die dsym-Datei im folgenden Verzeichnis:

    **&lt;Projektverzeichnis&gt;/bin/&lt;Platt&gt;FormKonfiguration/Device-Builds/&gt;Gerät/&lt;&lt;&gt; - &lt; Betriebssystemversion&gt;/**

    Beispiel:
  
    **TestApp/bin/iPhone/Release/Device-Builds/iPhone 8.4-11.3.1/**

- Wenn Sie gerätespezifische Builds nicht aktiviert haben, finden Sie die dsym-Datei im folgenden Verzeichnis:

    **&lt;Projektverzeichnis&gt;/bin/&lt;Platt&gt;FormKonfiguration/&lt;&gt;/**

    Beispiel:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> Im Rahmen des Buildprozesses wird die dsym-Datei von Visual Studio 2017 vom Mac-buildhost auf Windows kopiert. Wenn eine dsym-Datei unter Windows nicht angezeigt wird, stellen Sie sicher, dass Sie die Buildeinstellungen Ihrer APP so konfiguriert haben, dass [eine IPA-Datei erstellt](~/ios/deploy-test/app-distribution/ipa-support.md)wird.

## <a name="see-also"></a>Siehe auch

- [Symbolizieren von IOS-Absturz Dateien (xamarin. IOS)](https://www.jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Entmystifizierung von Absturz Protokollen für IOS-Anwendungen](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
