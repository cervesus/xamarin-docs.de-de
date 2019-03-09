---
title: Manuelles Erstellen von NuGet-Paketen für Xamarin
description: Dieses Dokument enthält Tipps zum Erstellen von NuGet-Pakete, die auf die Xamarin-Plattform abzielen. Es wird beschrieben, NuGet-Paket-Xamarin-Profilen, PCL-NuGet-Pakete mit plattformabhängigkeiten von sowie links zu verschiedenen Open-Source-Beispielen.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 4f4ca327479a7f4eb4a7dc7feafdd71291c1b7fe
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671688"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Manuelles Erstellen von NuGet-Paketen für Xamarin

_Diese Seite enthält einige Tipps, wie die NuGet-Pakete erstellen, die auf die Xamarin-Plattform abzielen._

> [!NOTE]
> Xamarin Studio 6.2 (und Visual Studio für Mac) bietet die Möglichkeit, _automatisch_ Generieren von NuGet-Pakete aus der PCL, .NET Standard- oder freigegebene Projekte. Finden Sie in der [Multi-Plattform-Bibliotheken für die Codefreigabe](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) Handbuch für die weitere Details.

## <a name="nuget-package-xamarin-profiles"></a>NuGet-Paket-Xamarin-Profil

Der NuGet-Website [unterstützen mehrerer .NET Framework-Versionen und Profile](https://docs.nuget.org/create/enforced-package-conventions) wird erläutert, wie andere Microsoft-Frameworks und -Profile unterstützen, enthält jedoch keine der Framework-Zielnamen, die von Xamarin verwendet.

Die wichtigsten Xamarin Zielframeworks verwendet sind heute:

* **MonoAndroid** - Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (unterstützt 64-Bit)
* **Xamarin.Mac** -mobile Xamarin.Mac-Profil, das entspricht, für die Xamarin.iOS und Xamarin.Android-API-Oberfläche.

Es ist außerdem ein Ziel für den älteren iOS [Classic API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS Classic API

Ein **NuSpec** Datei, die das Ziel all diese würde etwa folgendermaßen aussehen:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Die oben genannten ignoriert alle portablen Klassenbibliotheken.

Die meisten **NuSpec** Dateien geben Sie die Versionsnummer des Zielframeworks, aber es ist optional, wenn die Assembly mit allen Versionen des Zielframeworks funktioniert. Wenn Ihr Ziel wurde **Lib\MonoAndroid** würde dies bedeuten, es funktioniert mit jeder Version von Xamarin.Android.

Sie können die Version angeben, mit einer Reihe von Zahlen ohne Dezimalstellen oder können Sie ihn mit Dezimalstellen angeben. Ohne Dezimaltrennzeichen NuGet einfach jede Zahl aus und schalten Sie ihn in eine Version, durch Einfügen einer '.' zwischen einzelnen Ziffern.

In der obigen "MonoAndroid10" bedeutet "Android 1.0". Dies bedeutet lediglich, des Projekts [Zielframework](~/android/app-fundamentals/android-api-levels.md) MonoAndroid Version 1.0 oder höher sein muss. Die Version wird angegeben, der `<TargetFrameworkVersion>` Element in der Projektdatei.

Um zu verdeutlichen:

- **MonoAndroid403** entspricht Android 4.0.3 und höher (z.B. API-Ebene 15)
- **Xamarin.iOS10** entspricht Xamarin.iOS 1.0 und höher
- **Xamarin.iOS1.0** entspricht auch Xamarin.iOS 1.0 und höher

## <a name="pcl-nugets-with-platform-dependencies"></a>PCL-NuGet-Pakete mit Plattformabhängigkeiten

PCL-Profilen sind beschränkt, in welche .NET Framework-APIs, die sie zugreifen können und sie können plattformspezifischen Code sicherlich nicht verfügbar. Diese Links 3rd-Party erläutert verschiedene Ansätze zum Erstellen von NuGet-Pakete, die PCL und systemeigene APIs verwenden, um die Kompatibilität für Xamarin und andere Plattformen bereitzustellen:

- [Wie Sie Portable Class Libraries Work für Sie](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Der Trick dabei Lockvogel-PCL](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Erstellen eine NuGet PCL, die funktioniert mit Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Diese externen [Liste von PCL-Profilen mit ihren NuGet-Zielnamen](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) ist auch eine nützliche Referenz.

## <a name="examples"></a>Beispiele

Einige Open-Source-Beispiele, denen Sie verwenden können:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – schreiben Sie Ihre app mithilfe von System.Net.Http, aber löschen Sie diese Bibliothek in, und sie kehren erheblich schneller (Ansicht [Quelle](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat** ](https://www.nuget.org/packages/Splat/) – eine Bibliothek, die Dinge plattformübergreifende gestalten, die sein soll (Ansicht [Quelle](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -eine plattformübergreifende Bibliothek für das Rendern von Vektorgrafiken auf .NET (Ansicht [Quelle](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## <a name="related-links"></a>Verwandte Links

- [Nugetizer 3000 automatisiert die Erstellung von Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Aktualisieren von NuGet-Pakete für iOS-64-bit](https://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Einschließen ein NuGet in Ihr Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
