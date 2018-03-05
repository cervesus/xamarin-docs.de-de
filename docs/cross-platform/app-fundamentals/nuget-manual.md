---
title: "Manuelles Erstellen von NuGet-Pakete für Xamarin"
description: "Diese Seite enthält einige Tipps, wie die NuGet-Pakete erstellen, die auf die Xamarin-Plattform abzielen."
ms.topic: article
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 76f35819b00302f4a586643798afbd27416d3997
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Manuelles Erstellen von NuGet-Pakete für Xamarin

_Diese Seite enthält einige Tipps, wie die NuGet-Pakete erstellen, die auf die Xamarin-Plattform abzielen._

> [!NOTE]
> Xamarin Studio 6.2 (und Visual Studio für Mac) bietet die Möglichkeit, _automatisch_ NuGet-Pakete von PCL .NET Standard oder freigegebene Projekte generieren.
> Finden Sie in der [mehrere Plattformen-Bibliotheken für die Codefreigabe](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) Weitere-Handbuch.

## <a name="nuget-package-xamarin-profiles"></a>NuGet-Paket Xamarin Profile


Der NuGet-Website [mehrere .NET Framework-Versionen unterstützen und Profile](https://docs.nuget.org/create/enforced-package-conventions) erläutert, wie verschiedene Microsoft-Frameworks und Profile unterstützt jedoch keine der Framework-Zielnamen von Xamarin verwendet.

Die wichtigsten Xamarin Zielframeworks in Verwendung sind heute:

* **MonoAndroid** - Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [einheitliche API](~/cross-platform/macios/unified/index.md) (unterstützt 64-Bit)
* **Xamarin.Mac** -Xamarin.Mac des mobilen Profil, das Xamarin.iOS und Xamarin.Android-API-Oberfläche entspricht.

Es ist außerdem ein Ziel für das ältere iOS [klassische API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS klassische API

Ein **.nuspec** Datei, die das Ziel aller dieser etwa so aussehen:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Die oben genannten ignoriert alle portable Klassenbibliotheken.

Die meisten **.nuspec** Dateien angeben, die Versionsnummer des Zielframeworks, jedoch ist optional, wenn die Assembly mit allen Versionen von diesem Zielframework funktioniert. Wenn Ihr Ziel war also **Lib\MonoAndroid** Dies würde bedeuten, dass es mit einer Version von Xamarin.Android funktioniert.

Geben Sie die Version mit einer Reihe von Zahlen ohne Dezimalstellen oder Sie können ihn über Dezimalstellen angeben. Ohne Dezimaltrennzeichen NuGet nur übernimmt jede Zahl und schalten Sie ihn in eine Version durch Einfügen einer "." zwischen einzelnen Ziffer.

In der oben genannten "MonoAndroid10" bedeutet "Android 1.0". Dies bedeutet einfach des Projekts [Zielframework](~/android/app-fundamentals/android-api-levels.md) muss MonoAndroid Version 1.0 oder höher sein. Die Version wird angegeben, der `<TargetFrameworkVersion>` Element in der Projektdatei.

Um zu verdeutlichen:

- **MonoAndroid403** entspricht Android 4.0.3 und höher (d. h. API-Ebene 15)
- **Xamarin.iOS10** entspricht Xamarin.iOS 1.0 und höher
- **Xamarin.iOS1.0** entspricht auch Xamarin.iOS 1.0 und höher


## <a name="pcl-nugets-with-platform-dependencies"></a>PCL NuGets mit Platform-Abhängigkeiten

PCL-Profile sind beschränkt, in welche .NET Framework-APIs, die sie zugreifen können und sie können plattformspezifischen Code sicherlich nicht verfügbar. Diese Links 3rd Party werden verschiedene Ansätze zum Erstellen von NuGet-Pakete, die PCL und systemeigene APIs verwenden, damit die Kompatibilität für Xamarin und andere Plattformen beschrieben:

- [Gewusst wie: Portable Class Libraries Work für Sie vorzunehmen.](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Der Köder And Switch PCL Trick](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Erstellen einer NuGet PCL, arbeitet mit Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Externe [Liste der PCL-Profile mit ihren NuGet-Zielname](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) ist ebenfalls eine hilfreiche Referenz.

## <a name="examples"></a>Beispiele

Einige Open-Source-Beispiele, denen Sie verwenden können:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – Ihre app mit System.Net.Http schreiben, aber legen Sie diese Bibliothek im und geht erheblich schneller (Ansicht [Quelle](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat** ](https://www.nuget.org/packages/Splat/) – eine Bibliothek Dinge plattformübergreifende vornehmen, die sein soll (Ansicht [Quelle](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -eine plattformübergreifende Bibliothek für das Rendern von Vektorgrafiken auf .NET (Ansicht [Quelle](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).


## <a name="related-links"></a>Verwandte Links

- [Nugetizer 3000 automatisierte Erstellung von Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Aktualisieren von NuGets für iOS 64-bit](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Z. B. eine NuGet in Ihrem Projekt](/visualstudio/mac/nuget-walkthrough/index.md)
