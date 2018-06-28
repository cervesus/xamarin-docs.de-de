---
title: Kompilieren für verschiedene Geräte in Xamarin.iOS
description: In diesem Artikel werden verschiedene Optionen der Buildkonfiguration beschrieben, die zum Anpassen eines Xamarin.iOS-Builds für verschiedene Geräte verwendet werden können.
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 1b1bfab36646256eee706316c70004aef8399994
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784844"
---
# <a name="compiling-for-different-devices-in-xamarinios"></a>Kompilieren für verschiedene Geräte in Xamarin.iOS

Die Buildeigenschaften Ihrer ausführbaren Datei können Sie auf der Eigenschaftenseite **iOS-Build** des Projekts konfigurieren. Sie finden diese, indem Sie mit der rechten Maustaste auf den Projektnamen klicken und in Visual Studio für Mac zu **Optionen > iOS-Build** und in Visual Studio zu **Eigenschaften** navigieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


[![](compiling-for-different-devices-images/image1.png "Eigenschaftenseite „iOS-Build“ des Projekts")](compiling-for-different-devices-images/image1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](compiling-for-different-devices-images/image1a.png "Eigenschaftenseite „iOS-Build“ des Projekts")](compiling-for-different-devices-images/image1a.png#lightbox)

-----

Zusätzlich zu den Konfigurationsoptionen, die auf der Benutzeroberfläche verfügbar sind, können Sie auch Ihren eigenen Satz von Befehlszeilenoptionen an das [Xamarin.iOS-Build-Tool (mtouch)](~/ios/deploy-test/mtouch.md) übergeben.

[http://iossupportmatrix.com/](http://iossupportmatrix.com/) ist eine nützliche Ressource, mit der sich sicherstellen lässt, dass alle erforderlichen Geräte, Architekturen und iOS-Versionen eingeschlossen sind.

 <a name="SDK_Options" />


## <a name="sdk-options"></a>SDK-Optionen

Mit Visual Studio für Mac können Sie zwei wichtige Eigenschaften des SDK konfigurieren: die iOS SDK-Version, die für die Erstellung Ihrer Software verwendet wird, und das Bereitstellungsziel (bzw. die mindestens erforderliche iOS-Version).

Die iOS-Option **SDK-Version** ermöglicht Ihnen, verschiedene Versionen eines von Apple veröffentlichten SDK zu verwenden, wodurch Xamarin.iOS an die Compiler, Linker und Bibliotheken weitergeleitet wird, auf die es während Ihres Builds verweisen soll. 

Die Einstellung **Bereitstellungsziel** wird verwendet, um die mindestens erforderliche Version des Betriebssystems auszuwählen, unter der Ihre Anwendung ausgeführt wird. Diese wird in der Datei „Info.plist“ Ihres Projekts festgelegt. Sie sollten die Mindestversion auswählen, die alle APIs enthält, die Sie für die Ausführung Ihrer Anwendung benötigen.

Im Allgemeinen stellt die Xamarin.iOS-API alle Methoden zur Verfügung, die in der neuesten Version des SDK verfügbar sind. Bei Bedarf stellen wir praktische Eigenschaften zur Verfügung, mit denen Sie erkennen können, ob die Funktionalität zur Laufzeit verfügbar ist (z. B. funktionieren `UIDevice.UserInterfaceIdiom` und `UIDevice.IsMultitaskingSupported` immer in Xamarin.iOS. Wir erledigen alle Aufgaben im Hintergrund).

 <a name="Linking" />


## <a name="linking"></a>Verknüpfen

Sehen Sie sich unsere gesonderte Seite zum [Linker](~/ios/deploy-test/linker.md) an, um mehr darüber zu erfahren, wie der Linker Ihnen hilft, die Größe Ihrer ausführbaren Dateien zu reduzieren, und um herauszufinden, wie Sie diese effektiv nutzen können.

 <a name="Code_Generation_Engine" />


## <a name="code-generation-engine"></a>Codegenerierungsengine

Ab Xamarin.iOS 4.0 gibt es zwei Codegenerierungs-Back-Ends für Xamarin.iOS. Die reguläre Mono-Codegenerierungsengine und eine auf dem LLVM-Optimierungscompiler basierende Engine. Jede Engine hat Vor- und Nachteile.

Normalerweise verwenden Sie während des Entwicklungsprozesses wohl eher die Mono-Codegenerierungsengine, da sie es Ihnen ermöglicht, schnell zu iterieren. Für Releasebuilds und die AppStore-Bereitstellung sollten Sie auf die LLVM-Codegenerierungsengine umsteigen.

Die LLVM-Optimierungs-Back-End-Engine generiert sowohl schnelleren als auch strafferen Code als die Mono-Engine, jedoch auf Kosten langer Kompilierungszeiten.

Sie können diese in Visual Studio für Mac oder Visual Studio über „iOS-Buildoptionen“ aktivieren.

[![](compiling-for-different-devices-images/image2.png "Aktivieren von LLVM")](compiling-for-different-devices-images/image2.png#lightbox)

[![](compiling-for-different-devices-images/image2a.png "Aktivieren von LLVM")](compiling-for-different-devices-images/image2a.png#lightbox)

 <a name="ARMV7_and_ARMV7s_support" />


## <a name="architecture-support"></a>Architekturunterstützung

<a name="armv6-discontinued" />

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6 (Xamarin.iOS unterstützt ARMv6 ab v8.10 nicht mehr)

- iPhone (Original), 3G
- iPod 1., 2. Generation

### <a name="armv7"></a>ARMv7

- iPhone 3GS, 4, 4S
- iPad 1, 2, 3, Mini
- iPod 3., 4., 5. Generation

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

Wenn Sie nur auf den ARMv7s-Prozessor abzielen, wird der generierte Code etwas schneller sein. Er wird nicht mehr auf ARMv7- oder ARMv6-Systemen ausgeführt werden, es sei denn, Sie kompilieren eine FAT-Binärdatei, die mehrere ausführbare Dateien in Ihrem Paket enthält.

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64 (Xamarin.iOS unterstützt ARM64 seit v8.6)

- iPhone 5s
- iPhone SE
- iPhone 6, 6 Plus
- iPhone 6s, 6s Plus
- iPhone 7, 7 Plus
- iPhone 8, 8 Plus
- iPhone X
- iPad Air
- iPad Air 2
- iPad Mini 2, 3, 4
- iPad Pro (alle)

Beachten Sie, dass alle Builds, die an den App Store übermittelt werden, 64-Bit-Unterstützung aufweisen müssen. Dies ist eine von [Apple](https://developer.apple.com/news/?id=12172014b) gestellte Anforderung. Darüber hinaus unterstützt iOS 11 nur 64-Bit-Anwendungen.

 <a name="ARM_Thumb_Support" />


### <a name="arm-thumb-2-support"></a>ARM Thumb-2-Unterstützung

Thumb ist ein kompakterer Befehlssatz, der von ARM-Prozessoren verwendet wird. Indem Sie die Thumb-Unterstützung aktivieren, können Sie die Größe Ihrer ausführbaren Datei reduzieren, jedoch auf Kosten langsamerer Ausführungszeiten. Thumb wird unter ARMv7 und ARMv7s unterstützt.

 <a name="Conditional_framwork_useage" />


## <a name="conditional-framework-usage"></a>Bedingte Frameworknutzung

Wenn Ihr Projekt einige der Funktionen der neueren iOS-Versionen nutzen möchte, müssen Sie sich möglicherweise bedingt auf bestimmte neue Frameworks verlassen. Ein Paradebeispiel dafür ist der Wunsch, iAd unter iOS 4.0 oder höher zu verwenden, aber dennoch 3.x-Geräte zu unterstützen. Um dies zu erreichen, müssen Sie Xamarin.iOS wissen lassen, dass Sie für das iAd-Framework eine „schwache“ Verknüpfung benötigen. Schwache Bindungen sorgen dafür, dass das Framework nur bei Bedarf geladen wird, wenn zum ersten Mal eine Klasse aus dem Framework benötigt wird.

Gehen Sie dazu folgendermaßen vor:

-  Öffnen Sie **Projektoptionen**, und navigieren Sie zum Bereich **iOS-Build**.
-  Fügen Sie `'-gcc_flags "-weak_framework iAd"'` zu **Zusätzliche Optionen** für jede Konfiguration hinzu, für die Sie eine schwache Verknüpfung wünschen:


[![](compiling-for-different-devices-images/image3.png "Zusätzliche Optionen")](compiling-for-different-devices-images/image3.png#lightbox)


Zusätzlich dazu müssen Sie Ihre Verwendung der Typen davor schützen, unter älteren Versionen von iOS ausgeführt zu werden, in denen sie möglicherweise nicht vorhanden sind. Es gibt mehrere Methoden, um dies zu erreichen, von denen eine die Analyse von `UIDevice.CurrentDevice.SystemVersion` ist.



## <a name="related-links"></a>Verwandte Links

- [Linker](~/ios/deploy-test/linker.md)
- [Extern: iOS-Supportmatrix](http://iossupportmatrix.com/)
