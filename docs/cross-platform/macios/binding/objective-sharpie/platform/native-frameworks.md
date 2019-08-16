---
title: Binden von nativen Frameworks
description: In diesem Dokument wird beschrieben, wie Sie die Option "-Framework" von Target Sharpie verwenden, um eine Bindung an eine als Framework verteilte Bibliothek zu erstellen.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: cb6c39b2110161b3f839b8adc03701007f09cc4d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521885"
---
# <a name="binding-native-frameworks"></a>Binden von nativen Frameworks

Manchmal wird eine native Bibliothek als [Framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)verteilt. Ziel-Sharpie bietet eine praktische Funktion zum Binden ordnungsgemäß definierter Frame `-framework` Works mithilfe der-Option.

Beispielsweise ist die Bindung des [Adobe Creative SDK-Frameworks](https://creativesdk.adobe.com/downloads.html) für IOS unkompliziert:

```
$ sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

In einigen Fällen gibt ein Framework eine **Info. plist** -Datei an, die angibt, mit welchem SDK das Framework kompiliert werden soll. Wenn diese Informationen vorhanden sind und keine `-sdk` explizite Option übergeben wird, leitet der Ziel-Sharpie ihn aus der " **Info. plist** "-Datei `DTSDKName` des Frameworks (entweder dem `DTPlatformName` Schlüssel `DTPlatformVersion` oder einer Kombination der Schlüssel und) ab.

Die `-framework` -Option lässt nicht zu, dass explizite Header Dateien übermittelt werden. Die Header Datei des Headers wird anhand der Konvention basierend auf dem frameworknamen ausgewählt. Wenn ein Ober Schirm nicht gefunden werden kann, versucht der Ziel-Sharpie nicht, das Framework zu binden, und Sie müssen die Bindung manuell ausführen, indem Sie die richtigen zu pargenden Header Dateien und alle frameworgumente für clang bereitstellen (z. b. die `-F`frameworksuchpfad-Option).

Unter der Haube ist die `-framework` Angabe von nur eine Verknüpfung. Die folgenden Bindungs Argumente sind mit der `-framework` Kurzweile identisch.
Eine besondere Wichtigkeit ist `-F .` der frameworksuchpfad, der für clang bereitgestellt wird (Beachten Sie den Bereich und den Zeitraum, der als Teil des Befehls erforderlich ist).

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
