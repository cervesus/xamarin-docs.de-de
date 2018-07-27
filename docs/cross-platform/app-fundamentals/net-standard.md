---
title: Verwenden von .NET Standard-Bibliotheken zum Freigeben von Code
description: Dieses Dokument beschreibt, wie Sie .NET Standard-Bibliotheken zu verwenden, um die gemeinsamen Code nutzen. Es wird erläutert, .NET Standard-Bibliothek erstellen, die Einstellungen bearbeiten und es in einer Anwendung verwendet wird.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 65ba1915a2a968a14f0ce21bcada76e1b83531b0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270659"
---
# <a name="net-standard-library-code-sharing"></a>Freigeben von Code in .NET standard-Bibliothek

.NET standard-Bibliotheken haben eine einheitliche API für alle .NET-Plattformen einschließlich Xamarin und .NET Core. Erstellen Sie eine .NET Standard Library und verwenden Sie es aus einer beliebigen Laufzeit, die die .NET Standard-Plattform unterstützt. Finden Sie unter [in diesem Diagramm](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) für Details der unterstützten Plattformen.

Während .NET Standard-Versionen 1.0 bis 1.6 inkrementell größere Teilmengen von .NET Framework bereitstellen, bietet .NET Standard 2.0 das höchste Maß an Unterstützung für Xamarin-Anwendungen und für das Portieren vorhandenen portablen Klassenbibliotheken.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Dieser Abschnitt zeigt die Schritte zum Erstellen und Verwenden einer .NET Standard-Bibliothek mit Visual Studio für Mac.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard-Bibliothek

Der Projektmappe eine .NET Standard-Bibliothek hinzuzufügen ist ziemlich unkompliziert.

1. In der **neues Projekt hinzufügen** wählen Sie im Dialogfeld die **.NET Core** Kategorie, und wählen Sie dann **.NET Standardbibliothek**:

    ![Erstellen eine .NET Standard-Bibliothek](net-standard-images/vsm01-m157.png "erstellen eine neues .NET Standard-Bibliothek")

2. Wählen Sie auf dem nächsten Bildschirm das Zielframework - **.NET Standard 2.0** wird empfohlen:

    [![Wählen Sie .NET Standard 2.0](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. Geben Sie auf dem letzten Bildschirm, den Projektnamen, und klicken Sie auf **erstellen**.

4. Das .NET Standard-Bibliotheksprojekt wird im Projektmappen-Explorer wie dargestellt angezeigt. Der Knoten mit den Abhängigkeiten zeigt, dass die Bibliothek die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) verwendet.

    ![Knoten "Abhänigkeiten" in der Projektmappe mit .NET Standard hervorgehoben](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>Bearbeiten der Einstellungen für .NET Standard-Bibliothek

Die Einstellungen der .NET Standard-Bibliothek können angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt klicken und `Options` wählen, wie in diesem Screenshot gezeigt:

![Bearbeiten Sie die .NET Standard Zielframework in den Projektoptionen](net-standard-images/vsm03-m157.png "bearbeiten Sie die Version des .NET Standard Zielframeworks in den Projektoptionen")

Dort können Sie Ihre Version von `netstandard` durch Ändern des Dropdown-Werts `Target Framework` anpassen.

**Darüber hinaus gilt:** Sie können die `.csproj`-Datei zum Ändern des Werts direkt bearbeiten.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Dieser Abschnitt zeigt die Schritte zum Erstellen und Verwenden einer .NET Standard-Bibliothek mit Visual Studio.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard-Bibliothek

Der Projektmappe eine .NET Standard-Bibliothek hinzuzufügen ist ziemlich unkompliziert.

1. In der **neues Projekt** wählen Sie im Dialogfeld die **.NET Standard** Kategorie, und wählen Sie dann **-Klassenbibliothek ((.NET Standard)**.

    ![Erstellen einer neuen .NET Standard-Klassenbibliothek](net-standard-images/vs01-w157.png "Erstellen einer neuen .NET Standard-Klassenbibliothek")

2. Das .NET Standard-Bibliotheksprojekt wird im Projektmappen-Explorer wie dargestellt angezeigt. Der Knoten mit den Abhängigkeiten zeigt, dass die Bibliothek die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) verwendet.

    ![NETStandard.Library in der Projektmappe](net-standard-images/vs02-w157.png "NETStandard.Library in der Projektmappe")

### <a name="editing-net-standard-library-settings"></a>Bearbeiten der Einstellungen für .NET Standard-Bibliothek

Die Einstellungen für die .NET Standard-Bibliothek angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und auswählen können **Eigenschaften** wie im folgenden Screenshot gezeigt:

![Bearbeiten von .NET Standard-Zielframeworks in Projekteigenschaften](net-standard-images/vs03-w157.png "Bearbeiten Sie die Version des .NET Standard-Zielframeworks in den Projekteigenschaften")

**Darüber hinaus gilt:** können Sie bearbeiten die `.csproj` direkt so bearbeiten Sie die `TargetFramework` Element, und ändern, welche Version verwendet wird, als Ziel verwendet (z. b. `<TargetFramework>netstandard2.0</TargetFramework>`) angezeigt wird.

### <a name="using-a-net-standard-library-project"></a>Mithilfe eines .NET Standard Library-Projekts

Sobald eine .NET Standard-Bibliothek erstellt wurde, können Sie aus allen kompatiblen Anwendungen oder Bibliotheksprojekten einen Verweis auf die gleiche Weise hinzufügen, wie Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie **Verweis hinzufügen...**  wechseln Sie zu der **Projekte > Projektmappe** Registerkarte wie gezeigt:

![Verweisen auf eine .NET Standard-Bibliothek](net-standard-images/vs04.png "Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten \"Verweise\", wählen Sie \"Verweis hinzufügen...\", und wechseln Sie dann auf die Registerkarte der Projektmappen-Projekte wie dargestellt")

-----

## <a name="related-links"></a>Verwandte Links

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) – ausführliche Informationen und der Vergleich mit PCL.
