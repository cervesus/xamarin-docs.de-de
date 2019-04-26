---
title: Binden von nativen Frameworks
description: Dieses Dokument beschreibt, wie Sie mit der Ziel-Sharpie des - Framework-Option, um eine Bindung an eine Bibliothek erstellen, die als ein Framework verteilt.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ca103ee027597813be51e126aaa05f9aa969af35
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199127"
---
# <a name="binding-native-frameworks"></a>Binden von nativen Frameworks

Eine native Bibliothek wird manchmal als verteilt eine [Framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Objektive Sharpie verfügt über eine Funktion der Einfachheit halber für Frameworks über Bindung ordnungsgemäß definiert werden. die `-framework` Option.

Binden Sie z. B. die [Adobe Creative SDK-Framework](https://creativesdk.adobe.com/downloads.html) für iOS einfach ist:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

In einigen Fällen ein Framework geben einen **"Info.plist"** mit welcher SDK das Framework womit kompiliert werden soll. Wenn diese Informationen vorhanden ist und keine explizite `-sdk` Option übergeben, das Ziel Sharpie folgert er aus der Frameworks des **"Info.plist"** (entweder die `DTSDKName` Schlüssel oder eine Kombination der `DTPlatformName` und `DTPlatformVersion`Schlüssel).

Die `-framework` Option lässt keine explizite Headerdateien übergeben werden. Die Dach-Headerdatei wird gemäß der Konvention, die basierend auf den Namen des ausgewählt. Wenn ein Schirm-Header wurde nicht gefunden, Ziel Sharpie versucht nicht, um das Framework zu binden und Sie die Bindung manuell ausführen müssen, durch die Bereitstellung der richtigen Kategorie Header-Dateien analysiert werden, zusammen mit beliebigen Argumenten Framework für die Clang (z. B. die `-F`Framework Suchoption-Pfad).

Hinter den Kulissen angeben `-framework` ist nur eine Verknüpfung. Die folgenden Argumente für die Bindung sind identisch mit der `-framework` Kurzform, die oben genannten.
Von besonderer Bedeutung ist die `-F .` bereitgestellt, um die clang-Framework-Suchpfad (Beachten Sie den Speicherplatz und den Zeitraum, die als Teil des Befehls erforderlich sind).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen eine Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen Sie eine Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)

