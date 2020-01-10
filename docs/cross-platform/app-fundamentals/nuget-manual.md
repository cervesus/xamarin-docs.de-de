---
title: Manuelles Erstellen von nuget-Paketen für xamarin
description: Dieses Dokument enthält Tipps zum Erstellen von nuget-Paketen, die auf die xamarin-Plattform abzielen. Es beschreibt nuget-Paket xamarin-Profile, PCL-nuget mit Platt Form Abhängigkeiten und Links zu verschiedenen Open Source-Beispielen.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 16b8f303555bc2f45516c3c060c0d2482f9c4954
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728225"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Manuelles Erstellen von nuget-Paketen für xamarin

_Diese Seite enthält einige Tipps zum Erstellen von nuget-Paketen, die auf die xamarin-Plattform abzielen._

> [!NOTE]
> Xamarin Studio 6,2 (und Visual Studio für Mac) umfasst die Möglichkeit, nuget-Pakete _automatisch_ aus PCL, .NET Standard oder freigegebenen Projekten zu generieren. Weitere Informationen finden Sie im Leitfaden für [Multiplattform-Bibliotheken für Code Freigabe](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) .

## <a name="nuget-package-xamarin-profiles"></a>Xamarin-Profile für nuget-Pakete

In der nuget-Website, die [mehrere .NET Framework Versionen und Profile unterstützt](https://docs.nuget.org/create/enforced-package-conventions) , wird erläutert, wie verschiedene Microsoft-Frameworks und Profile unterstützt werden, aber nicht die von xamarin verwendeten zielframeworknamen.

Die wichtigsten xamarin-Ziel-Frameworks werden heute verwendet:

- **MonoAndroid** - Xamarin.Android
- **Xamarin. IOS** -xamarin. IOS- [Unified API](~/cross-platform/macios/unified/index.md) (unterstützt 64-Bit)
- Das Mobile Profil **xamarin. Mac** -xamarin. Mac entspricht der xamarin. IOS-und xamarin. Android-API-Oberfläche.

Es gibt auch ein Ziel für die ältere IOS- [Classic API](~/cross-platform/macios/unified/index.md):

- **MonoTouch** -IOS-Classic API

Eine **nuspec** -Datei, die auf all diese ausgerichtet ist, sieht in etwa wie folgt aus:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Im obigen Beispiel werden alle portablen Klassenbibliotheken ignoriert.

Die meisten **nuspec** -Dateien geben die Versionsnummer des Ziel Frameworks an, aber Sie ist optional, wenn die Assembly mit allen Versionen dieses Ziel Frameworks funktioniert. Wenn das Ziel **lib\monoandroid** ist, bedeutet dies, dass es mit jeder beliebigen Version von xamarin. Android funktioniert.

Sie können die Version mit einer Reihe von Zahlen ohne Dezimaltrennzeichen angeben, oder Sie können Sie mithilfe von Dezimaltrennzeichen angeben. Ohne den Dezimaltrennzeichen nimmt nuget einfach jede Zahl an und verwandelt sie in eine Version, indem zwischen den einzelnen Ziffern ein "." eingefügt wird.

Im obigen Abschnitt "MonoAndroid10" bedeutet "Android 1,0". Dies bedeutet lediglich, dass das [Ziel Framework](~/android/app-fundamentals/android-api-levels.md) des Projekts monoandroid, Version 1,0 oder höher, sein muss. Die Version wird im `<TargetFrameworkVersion>`-Element in der Projektdatei angegeben.

Zur Verdeutlichung:

- **MonoAndroid403** entspricht Android 4.0.3 und höher (IE-API-Ebene 15)
- **Xamarin. iOS10** entspricht xamarin. IOS 1,0 und höher
- **Xamarin. IOS 1.0** entspricht auch xamarin. IOS 1,0 und neuer.

## <a name="pcl-nugets-with-platform-dependencies"></a>PCL-nugets mit Platt Form Abhängigkeiten

PCL-Profile sind auf die .NET Framework-APIs beschränkt, auf die Sie zugreifen können, und Sie können sicherlich nicht auf plattformspezifischen Code zugreifen. Diese Links von Drittanbietern erörtern verschiedene Ansätze zum Erstellen von nuget-Paketen, die PCL und Native APIs verwenden, um Kompatibilität für xamarin und andere Plattformen bereitzustellen:

- [Vorgehensweise beim Arbeiten mit portablen Klassenbibliotheken](https://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Der Köder-und Switch-PCL-Trick](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Erstellen einer nuget-PCL, die mit xamarin. IOS funktioniert](https://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Diese externe [Liste von PCL-Profilen mit Ihrem nuget-Zielnamen](https://portablelibraryprofiles.stephencleary.com) ist auch eine hilfreiche Referenz.

## <a name="examples"></a>Beispiele

Einige Open Source-Beispiele, auf die Sie verweisen können:

- [**Modernhttpclient**](https://www.nuget.org/packages/modernhttpclient/) – schreiben Sie Ihre App mithilfe von System .net. http, aber löschen Sie diese Bibliothek in, und Sie wird drastisch schneller ( [Quelle](https://github.com/paulcbetts/ModernHttpClient)anzeigen).
- [**Splat**](https://www.nuget.org/packages/Splat/) – eine Bibliothek, um die plattformübergreifende Plattform zu gestalten, die ( [Quelle](https://github.com/paulcbetts/Splat)anzeigen) sein sollte.
- [**Ngraphics**](https://www.nuget.org/packages/NGraphics/) : eine plattformübergreifende Bibliothek zum Rendern von Vektorgrafiken in .net ( [Quelle](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)anzeigen).

## <a name="related-links"></a>Verwandte Themen

- [Nugetizer-3000 automatisierte nuget-Erstellung](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)       
- [Einschließen eines nuget-Projekts in Ihr Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
