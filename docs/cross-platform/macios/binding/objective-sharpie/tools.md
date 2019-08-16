---
title: Ziel-Sharpie-Tools & Befehle
description: Dieses Dokument enthält eine Übersicht über die Tools, die im Ziel-Sharpie enthalten sind, und die Befehlszeilenargumente, die mit Ihnen verwendet werden.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: ddfe0f99991808214a6006c9504d267179adf2ab
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521856"
---
# <a name="objective-sharpie-tools--commands"></a>Ziel-Sharpie-Tools & Befehle

_Übersicht über die Tools, die im Ziel-Sharpie enthalten sind, und die Befehlszeilenargumente, um Sie zu verwenden._

Nachdem das Ziel "Sharpie" erfolgreich [installiert](~/cross-platform/macios/binding/objective-sharpie/get-started.md)wurde, öffnen Sie ein Terminal, und machen Sie sich mit den *Befehlen* vertraut, die der Sharpie bietet:

```
$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation
```

Ziel-Sharpie stellt die folgenden Tools bereit:

|Tool|Beschreibung|
|--- |--- |
|**xcode**|Enthält Informationen zur aktuellen Xcode-Installation sowie zu den verfügbaren Versionen von IOS-und Mac-SDOs. Diese Informationen werden später verwendet, wenn wir unsere Bindungen generieren.|
|**Pfund**|Sucht nach, konfiguriert, installiert (in einem lokalen Verzeichnis) und bindet Ziel-C- [cocoapod-](https://cocoapods.org/) Bibliotheken, die im Repository für Master Spezifikationen verfügbar sind. Dieses Tool wertet die installierte cocoapod aus, um automatisch die korrekte Eingabe abzuleiten, die an `bind` das Tool weiter unten übergeben werden soll. Neu in 3,0!|
|**bind**|Analysiert die Header Dateien (`*.h`) in der Ziel-C-Bibliothek in die anfänglichen [ApiDefinition.cs-und StructsAndEnums.cs-](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) Dateien.|
|**update**|Prüft, ob neuere Versionen von Ziel-Sharpie vorhanden sind, und lädt das Installationsprogramm herunter, sofern ein solches verfügbar ist.|
|**verify-docs**|Zeigt ausführliche Informationen über `[Verify]` Attribute an.|
|**docs**|Navigiert zu diesem Dokument in Ihrem Standard Webbrowser.|

Um Hilfe zu einem bestimmten Ziel-Sharpie-Tool zu erhalten, geben Sie den Namen des `-help` Tools und die Option ein. Beispielsweise `sharpie xcode -help` gibt die folgende Ausgabe zurück:

```
$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.
```

Bevor wir den Bindungsprozess starten können, müssen wir den folgenden Befehl in das Terminal `sharpie xcode -sdks`eingeben, um Informationen zu unseren aktuell installierten sdden zu erhalten. Die Ausgabe kann sich je nach Version (n) von Xcode, die Sie installiert haben, unterscheiden. Der Ziel-Sharpie sucht in einem `Xcode*.app` der `/Applications` folgenden Verzeichnisse nach SDI-Daten:

```
$ sharpie xcode -sdks
sdk: appletvos9.0    arch: arm64
sdk: iphoneos9.1     arch: arm64   armv7
sdk: iphoneos9.0     arch: arm64   armv7
sdk: iphoneos8.4     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: macosx10.10     arch: x86_64  i386
sdk: watchos2.0      arch: armv7
```

In der obigen Ansicht sehen wir, dass das SDK auf `iphoneos9.1` dem Computer installiert ist `arm64` und die Architektur unterstützt wird. Dieser Wert wird für alle Beispiele in diesem Abschnitt verwendet. Nachdem diese Informationen vorhanden sind, können wir eine Ziel-C-Bibliotheks Header Datei in den ursprünglichen `ApiDefinition.cs` und `StructsAndEnums.cs` für das Bindungs Projekt analysieren.
