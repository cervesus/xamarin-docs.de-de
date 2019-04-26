---
title: Multi-Plattform-Library-Projekte mit NuGet (auch bekannt als Nugetizer 3000)
description: Dieses Dokument beschreibt, wie das Nugetizer 3000-Tool zum automatischen Erstellen von NuGet-Pakete zum Code plattformübergreifend gemeinsam verwenden.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 6d3f7b316e397705ecb9bd404007dcd9ef5aa183
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266792"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>NuGet-Bibliothek für mehrere Plattformen Projekte (Nugetizer 3000)

_Erstellen Sie automatisch NuGet-Pakete zum Teilen von Code über Plattformen hinweg "Nugetizer 3000"._

Es ist möglich, zum automatischen Erstellen von NuGet-Pakete zum Teilen von Code über Plattformen hinweg die _Nugetizer 3000_. Dies ermöglicht ist möglich, NuGet-Pakete erstellen, über vorhandene Bibliotheksprojekte oder durch Erstellen eines neuen **Multi-Plattform-Bibliotheksprojekt**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die Nugetizer 3000 ist im Lieferumfang von Visual Studio für Mac &ndash; suchen Sie nach der **Bibliothek > Mulitplatform Bibliothek** Projekttyp in der **Datei > Neu** Fenster:

[![](images/mulitplatform-library-sml.png "Erstellen Sie neues Fenster für Multi-Plattform-Bibliothek")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um die Nugetizer 3000 in Visual Studio verwenden zu können, wenden Sie [herunterladen, und führen Sie das VSIX-Installationsprogramm](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Erstellen von NuGet-Pakete

Nach der Konfiguration gibt jeden Build des Projekts ein vollständiges NuGet-Paket, die zum Freigeben von Code intern mit anderen apps verwendet oder in hochgeladen werden kann [NuGet.org](https://www.nuget.org).

Es gibt drei Szenarien für die Verwendung dieses Features:

- [Vorhandene Bibliotheksprojekte](existing-library.md)

  Erstellen Sie ein NuGet-Paket aus vorhandenen PCL (oder .NET Standard)-Projekte an.

- [Erstellen eines neuen Projekts für die Bibliothek für mehrere Plattformen](single-codebase.md)

  Erstellen Sie eine neue Bibliothek, die gemeinsamen Code über NuGet, die mit einer PCL oder .NET Standard nutzen.

- [Erstellen neuer Projekte der plattformspezifischen Bibliothek](platform-specific.md)

  Erstellen Sie eine neue Bibliothek und NuGet ab, der plattformspezifischen Code für iOS und Android und ein freigegebenes Projekt verwendet, um die gemeinsamen Code und den plattformspezifischen Projekten zur Unterstützung von iOS oder Android-spezifische Funktionen enthalten.

Finden Sie in der [Leitfaden zu Metadaten](metadata.md) ausführliche Informationen über die erforderliche und optionale Metadaten, die auf alle NuGet-Paket hinzugefügt werden muss.

## <a name="further-nuget-information"></a>Weitere Informationen für NuGet

Erfahren Sie mehr über [manuelle Erstellen von NuGet-Pakete für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) und [einschließen ein NuGet-Pakets in einer app](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Microsofts [NuGet-Dokumentation](https://docs.microsoft.com/nuget/) enthält ausführlichere Informationen zu den **NUPKG** Format und mit NuGet-Pakete in Visual Studio.

Die Informationen für NuGet-Paket-Projekte (auch als) NuGetizer 3000) steht auf der [NuGet GitHub-Repository](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

## <a name="related-links"></a>Verwandte Links

- [NuGetizer 3000 Anwendungsfälle](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Manuelles Erstellen von NuGet-Paketen für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [NuGet-Dokumentation](https://docs.microsoft.com/nuget/)
