---
title: Nuget-Bibliotheks Projekte für mehrere Plattformen (auch nugetizer 3000)
description: In diesem Dokument wird beschrieben, wie Sie das Tool nugetizer 3000 zum automatischen Erstellen von nuget-Paketen verwenden, um Code plattformübergreifend freizugeben.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: ac8e4b2eef0905ba50cb24b9eb035d50a05dd84a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932241"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>Nuget-Bibliotheken für mehrere Plattformen (nugetizer 3000)

_Automatisches Erstellen von nuget-Paketen für die plattformübergreifende Freigabe von Code mithilfe von "nugetizer 3000"!_

Es ist möglich, nuget-Pakete automatisch zu erstellen, um Code plattformübergreifend mit dem _nugetizer 3000_zu teilen. Dies ermöglicht das Erstellen von nuget-Paketen aus vorhandenen Bibliotheks Projekten oder durch Erstellen eines neuen **Bibliothek Projekts mit mehreren Plattformen**.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Der nugetizer 3000 ist in enthalten Visual Studio für Mac &ndash; Suchen nach der **Bibliothek > mulitplatform-Bibliotheks** Projekttyp in der **Datei > neues** Fenster:

[![Fenster "neue Bibliothek für mehrere Plattformen erstellen"](images/mulitplatform-library-sml.png)](images/mulitplatform-library.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Um nugetizer 3000 in Visual Studio zu verwenden, [Laden Sie das VSIX-Installationsprogramm herunter, und führen](https://bit.ly/nugetizer-2017)Sie es aus.

-----

## <a name="building-nuget-packages"></a>Nuget-Pakete werden aufgebaut.

Nach der Konfiguration gibt jeder Build des Projekts ein umfassendes nuget-Paket aus, das verwendet werden kann, um Code intern für andere apps freizugeben oder in [NuGet.org](https://www.nuget.org)hochzuladen.

Es gibt drei Szenarien für die Verwendung dieses Features:

- [Vorhandene Bibliotheks Projekte](existing-library.md)

  Erstellen Sie ein nuget-Paket aus vorhandenen PCL-Projekten (oder .NET Standard).

- [Erstellen eines neuen Bibliothek Projekts mit mehreren Plattformen](single-codebase.md)

  Erstellen Sie eine neue Bibliothek, um gemeinsamen Code über nuget zu teilen, indem Sie eine PCL oder eine .NET Standard verwenden.

- [Erstellen neuer plattformspezifischer Bibliotheks Projekte](platform-specific.md)

  Erstellen Sie eine neue Bibliothek und nuget mit Platt Form spezifischem Code für IOS und Android, und verwenden Sie ein frei gegebenes Projekt, das den gemeinsamen Code und plattformspezifische Projekte zur Unterstützung von IOS-oder Android-spezifischen Funktionen enthält.

Ausführliche Informationen zu den erforderlichen und optionalen Metadaten, die zu einem nuget-Paket hinzugefügt werden müssen, finden Sie im [metadatenhandbuch](metadata.md) .

## <a name="further-nuget-information"></a>Weitere nuget-Informationen

Weitere Informationen finden Sie unter [Manuelles Erstellen von nuget-Paketen für xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) und [einschließen eines nuget-Pakets in eine APP](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

In der [nuget-Dokumentation](https://docs.microsoft.com/nuget/) von Microsoft sind ausführlichere Informationen zum **nupkg** -Format und zum Verwenden von nuget-Paketen in Visual Studio enthalten.

Design Diskussion für nuget-Paket Projekte (auch bekannt als Nugetizer 3000) ist im [nuget-GitHub-Repository](https://github.com/NuGet/Home/wiki/NuGetizer-3000)verfügbar.

## <a name="related-links"></a>Ähnliche Themen

- [Nugetizer-3000 Anwendungsfälle](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Manuelles Erstellen von nuget-Paketen für xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Nuget-Dokumentation](https://docs.microsoft.com/nuget/)
