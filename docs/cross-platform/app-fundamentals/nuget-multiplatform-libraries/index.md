---
title: NuGet-Projekte (Nugetizer 3000)
description: "Erstellen Sie automatisch NuGet-Pakete zum Freigeben von Code plattformübergreifend mithilfe von \"Nugetizer 3000\"!"
ms.topic: article
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: 66bf9c215e3d30687fa8037220b8b35409ca285d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="nuget-projects-nugetizer-3000"></a>NuGet-Projekte (Nugetizer 3000)

_Erstellen Sie automatisch NuGet-Pakete zum Freigeben von Code plattformübergreifend mithilfe von "Nugetizer 3000"!_

Es ist möglich, zum automatischen Erstellen von NuGet-Pakete zum Freigeben von Code plattformübergreifend mithilfe der _Nugetizer 3000_. Dies ermöglicht die ist möglich, Erstellen von NuGet-Pakete aus vorhandenen Bibliotheksprojekte oder durch Erstellen eines neuen **Bibliotheksprojekt für mehrere Plattformen**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
Die Nugetizer 3000 ist im Lieferumfang von Visual Studio für Mac 6.2.
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
<a name="to-use-the-nugetizer-3000-in-visual-studio-please-download-and-run-the-vsix-installerhttpbitlynugetizer-2017"></a>Um die Nugetizer 3000 in Visual Studio verwenden, geben Sie [herunterzuladen, und führen Sie das VSIX-Installationsprogramm](http://bit.ly/nugetizer-2017).
-----



[ ![](images/mulitplatform-library-sml.png "Erstellen Sie neue Zeitfenster für die Bibliothek für mehrere Plattformen")](images/mulitplatform-library.png)

Nach der Konfiguration gibt jedem Build des Projekts ein vollständiges NuGet-Paket, der zum Freigeben von Code intern mit anderen apps verwendet oder in hochgeladen werden kann [NuGet.org](https://www.nuget.org).

Es gibt drei Szenarien zum Verwenden dieser Funktion:

- [Vorhandene Bibliotheksprojekte](existing-library.md)

  Erstellen Sie ein NuGet-Paket aus vorhandenen PCL (oder "Standard".NET)-Projekten.

- [Erstellen eines neuen Multiplattform-Bibliotheksprojekts](single-codebase.md)

  Erstellen Sie eine neue Bibliothek, die über NuGet, die mit einer PCL oder .NET Standard gemeinsamen Code nutzen.

- [Erstellen neuer plattformspezifischen Library-Projekte](platform-specific.md)

  Erstellt eine neue Bibliothek und NuGet, die plattformspezifischen Code für iOS und Android enthält, und verwendet ein freigegebenes Projekt plattformspezifischen Projekte, iOS oder Android-spezifische Funktionen unterstützen und die gemeinsamen Code enthalten.

Finden Sie in der [Metadaten Handbuch](metadata.md) ausführliche Informationen über die erforderliche und optionale Metadaten, die NuGet-Paket hinzugefügt werden müssen.


## <a name="further-nuget-information"></a>Weitere Informationen von NuGet

Erfahren Sie mehr über [manuell erstellen NuGets für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) und zum [enthalten ein NuGet-Paket in einer app](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Microsofts [NuGet-Dokumentation](https://docs.microsoft.com/nuget/) enthält ausführlichere Informationen zu den **.nupkg** Format und mithilfe von NuGet-Pakete in Visual Studio.

Der Entwurf Diskussion für NuGet-Paket-Projekte (auch als) NuGetizer 3000) wird über die [NuGet-GitHub-Repository](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>Verwandte Links

- [NuGetizer 3000-Anwendungsfälle](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Erstellen Sie manuell NuGet-Pakete für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet-Dokumentation](https://docs.microsoft.com/nuget/)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
