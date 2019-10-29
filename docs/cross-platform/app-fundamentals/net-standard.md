---
title: Verwenden von .NET Standard-Bibliotheken zum Freigeben von Code
description: In diesem Dokument wird beschrieben, wie Sie .NET Standard-Bibliotheken verwenden, um Code freizugeben. Er erläutert das Erstellen einer .NET Standard Bibliothek, das Bearbeiten der Einstellungen und deren Verwendung in einer Anwendung.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 07/18/2018
ms.openlocfilehash: cae59053374f673a56d02e86cd59fb85f313c41b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016812"
---
# <a name="net-standard-library-code-sharing"></a>Code Freigabe für .NET Standard Bibliothek

.NET Standard Bibliotheken verfügen über eine einheitliche API für alle .net-Plattformen, einschließlich xamarin und .net Core. Erstellen Sie eine einzelne .NET Standard Bibliothek, und verwenden Sie Sie von jeder Laufzeit, die die .NET Standard Plattform unterstützt. Ausführliche Informationen zu den unterstützten Plattformen finden Sie in [diesem Diagramm](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) .

Während .NET Standard Versionen 1,0 bis 1,6 inkrementelle Teilmengen des .NET Framework bereitstellen, bietet .NET Standard 2,0 das beste Maß an Unterstützung für xamarin-Anwendungen und das Portieren vorhandener portabler Klassenbibliotheken.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

In diesem Abschnitt wird erläutert, wie Sie eine .NET Standard Bibliothek mithilfe von Visual Studio für Mac erstellen und verwenden.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard Bibliothek

Mit den folgenden Schritten können Sie Ihrer Projekt Mappe eine .NET Standard Bibliothek hinzufügen:

1. Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** die Kategorie **.net Core** aus, und wählen Sie dann **.NET Standard Bibliothek**aus:

    ![Erstellen einer .NET Standard Bibliothek](net-standard-images/vsm01-m157.png "Erstellen einer neuen .NET Standard Bibliothek")

2. Wählen Sie auf dem nächsten Bildschirm das Ziel Framework aus: **.NET Standard 2,0** wird empfohlen:

    [Wählen Sie![.NET Standard 2,0 aus.](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. Geben Sie auf dem letzten Bildschirm den Projektnamen ein, und klicken Sie auf **Erstellen**.

4. Das .NET Standard Bibliotheksprojekt wird angezeigt, wie in der Projektmappen-Explorer angezeigt. Der Knoten Abhängigkeiten gibt an, dass die Bibliothek die Datei [netstandard. Library](https://www.nuget.org/packages/NETStandard.Library/)verwendet.

    ![Der Knoten Abhängigkeiten in der Lösung gibt an .NET Standard](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>Bearbeiten von .NET Standard Bibliotheks Einstellungen

Die .NET Standard Bibliotheks Einstellungen können angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt klicken und `Options` auswählen, wie in diesem Screenshot gezeigt:

![.NET Standard Ziel Framework in den Projektoptionen bearbeiten](net-standard-images/vsm03-m157.png "Die Version der .NET Standard Ziel Framework in den Projektoptionen bearbeiten")

Innerhalb von können Sie Ihre Version von `netstandard` ändern, indem Sie den `Target Framework` Dropdown-Wert ändern.

**Außerdem** gilt Folgendes: Sie können den `.csproj` direkt bearbeiten, um diesen Wert zu ändern.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

In diesem Abschnitt wird erläutert, wie Sie eine .NET Standard Bibliothek mithilfe von Visual Studio erstellen und verwenden.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard Bibliothek

Das Hinzufügen einer .NET Standard-Bibliothek zu ihrer Projekt Mappe ist relativ unkompliziert.

1. Wählen Sie im Dialogfeld **Neues Projekt** die Kategorie **.NET Standard** aus, und klicken Sie dann auf **Klassenbibliothek (.NET Standard)** .

    ![Erstellen einer neuen .NET Standard-Klassenbibliothek](net-standard-images/vs01-w157.png "Erstellen einer neuen .NET Standard-Klassenbibliothek")

2. Das .NET Standard Bibliotheksprojekt wird angezeigt, wie in der Projektmappen-Explorer angezeigt. Der Knoten Abhängigkeiten gibt an, dass die Bibliothek die Datei [netstandard. Library](https://www.nuget.org/packages/NETStandard.Library/)verwendet.

    !["Netstandard. Library" im Projektordner](net-standard-images/vs02-w157.png "Projekt in Projekt Mappe .NET Standard")

### <a name="editing-net-standard-library-settings"></a>Bearbeiten von .NET Standard Bibliotheks Einstellungen

Die .NET Standard Bibliotheks Einstellungen können angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt klicken und **Eigenschaften** auswählen, wie in diesem Screenshot gezeigt:

![.NET Standard-Ziel Framework in den Projekteigenschaften bearbeiten](net-standard-images/vs03-w157.png "Verweisen auf eine .NET Standard Bibliothek auf dieselbe Weise wie andere Projekte")

**Außerdem** gilt Folgendes: Sie können die `.csproj` direkt bearbeiten, um das `TargetFramework` Element zu bearbeiten und die Zielversion zu ändern (z. b. `<TargetFramework>netstandard2.0</TargetFramework>`) angezeigt wird.

### <a name="using-a-net-standard-library-project"></a>Verwenden eines .NET Standard Bibliotheks Projekts

Nachdem eine .NET Standard Bibliothek erstellt wurde, können Sie aus einem beliebigen kompatiblen Anwendungs-oder Bibliotheksprojekt einen Verweis darauf hinzufügen, wie Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten Verweise, und wählen Sie **Verweis hinzufügen...** aus, und wechseln Sie dann zur Registerkarte **Projekte > Projekt** Mappe, wie gezeigt

![Verweisen auf eine .NET Standard Bibliothek](net-standard-images/vs04.png "Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten Verweise, und wählen Sie Verweis hinzufügen... aus. Wechseln Sie dann wie hier gezeigt zur Registerkarte Projektmappenprojekte.")

-----

## <a name="net-standard-and-xamarinforms-for-the-net-developer-video"></a>.NET Standard und xamarin. Forms für den .NET-Entwickler (Video)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/NET-Standard-and-XamarinForms-for-the-NET-Developer/player]

## <a name="related-links"></a>Verwandte Links

* [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) -ausführliche Informationen und einen Vergleich mit der PCL.
