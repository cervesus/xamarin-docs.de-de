---
title: Verwenden .NET Standardbibliotheken zum Freigeben von Code
description: Dieses Dokument beschreibt, wie .NET Standardbibliotheken Code freigeben. Es wird erläutert, eine .NET Standardbibliothek erstellen, bearbeiten die Einstellungen und ihn in einer Anwendung verwenden.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 82a89309e6462462471f42c3504d109ff0722917
ms.sourcegitcommit: 5db075bdd0b62d5d1d1567c267303a6a1888c8f2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806789"
---
# <a name="using-net-standard-libraries-to-share-code"></a>Verwenden .NET Standardbibliotheken zum Freigeben von Code

## <a name="net-standard"></a>.NET-Standard

Die .NET Standard-Bibliothek ist eine formale Spezifikation von .NET-APIs, die für alle .NET-Laufzeiten verfügbar sein sollen. Die Motivation hinter der Standard-Bibliothek ist das Herstellen einer umfassenderen Einheitlichkeit im .NET-Ökosystem.
Auch wenn [ECMA-335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) weiterhin für Einheitlichkeit im .NET-Laufzeitverhalten sorgt, gibt es keine ähnliche Spezifikation für die .NET-Basisklassenbibliotheken (BCL) für Implementierungen der .NET-Bibliothek.

Sie können es sich als eine vereinfachte nächste Generation der [Portable Class Library](https://msdn.microsoft.com/library/gg597391.aspx) vorstellen.
Es ist eine einzige Bibliothek mit einheitlicher API für alle .NET-Plattformen einschließlich .NET Core. Sie erstellen nur eine einzige .NET Standard-Bibliothek und verwenden diese in jeder Laufzeit, welche die .NET Standard-Plattform unterstützt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Dieser Abschnitt zeigt die Schritte zum Erstellen und Verwenden einer .NET Standard-Bibliothek mit Visual Studio für Mac. Eine vollständige Implementierung finden Sie im Beispielabschnitt der .NET Standard-Bibliothek.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard-Bibliothek

Der Projektmappe eine .NET Standard-Bibliothek hinzuzufügen ist ziemlich unkompliziert.

1. Klicken Sie im Dialogfeld "Neues Projekt hinzufügen" Wählen Sie die `.NET Core` Kategorie, und wählen Sie dann `Class Library(.NET Core)`.

  **Hinweis:** dieser Vorlage wird umbenannt werden, um `.NET Standard` in einer zukünftigen Version von Visual Studio für Mac.

  ![Erstellen einer Klassenbibliothek von .NET Core](net-standard-images/vsm01.png "erstellen eine neue Klassenbibliothek für .NET Core")

2. Das standardmäßige .NET-Steuerelementbibliothek-Projekt wird angezeigt, wie im Projektmappen-Explorer angezeigt. Der Knoten für die Abhängigkeiten hervor, dass die Bibliothek verwendet die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Abhängigkeiten von Knoten in der Projektmappe gibt .NET Standard an.](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Bearbeiten der Einstellungen für .NET-Standardbibliothek

Die Standardbibliothek .NET Einstellungen angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und auswählen können `Options` wie in diesem Screenshot gezeigt:

![Bearbeiten von .NET Standard Zielframeworks in Projektoptionen](net-standard-images/vsm03.png "bearbeiten Sie die Version für das Zielframework .NET Standard in Projektoptionen")

Sie können innerhalb Ihrer Version von ändern `netstandard` durch Ändern der `Target Framework` Dropdown-Wert.

**Darüber hinaus gilt:** können Sie bearbeiten die `.csproj` direkt zum Ändern dieses Werts.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

In diesem Abschnitt führt Sie durch die Schritte zum Erstellen und verwenden eine mithilfe von Visual Studio .NET-Standardbibliothek. Eine vollständige Implementierung finden Sie im Beispielabschnitt der .NET Standard-Bibliothek.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard-Bibliothek

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Der Projektmappe eine .NET Standard-Bibliothek hinzuzufügen ist ziemlich unkompliziert.

1. Klicken Sie im Dialogfeld "Neues Projekt hinzufügen" Wählen Sie die `.NET Standard` Kategorie, und wählen Sie dann `Class Library(.NET Standard)`.

  ![Erstellen eine neue .NET Standard Klassenbibliothek](net-standard-images/vs01.png "erstellen neue .NET Standard-Klassenbibliothek")

2. Das standardmäßige .NET-Steuerelementbibliothek-Projekt wird angezeigt, wie im Projektmappen-Explorer angezeigt. Der Knoten für die Abhängigkeiten hervor, dass die Bibliothek verwendet die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Im Projektordner NETStandard.Library](net-standard-images/vs02.png ".NET Standard Projekt in der Projektmappe")

#### <a name="editing-net-standard-library-settings"></a>Bearbeiten der Einstellungen für .NET-Standardbibliothek

Die Standardbibliothek .NET Einstellungen angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und auswählen können `Properties` wie in diesem Screenshot gezeigt:

![Bearbeiten von .NET standard Zielframeworks in den Projekteigenschaften](net-standard-images/vs03.png "verweisen auf eine .NET Standardbibliothek die gleiche Weise wie andere Projekte")

Sie können innerhalb Ihrer Version von ändern `netstandard` durch Ändern der `Target Framework` Dropdown-Wert.

**Darüber hinaus gilt:** können Sie bearbeiten die `.csproj` direkt zum Ändern dieses Werts.

#### <a name="using-net-standard-library"></a>Mithilfe der Standardbibliothek für .NET

Sobald eine Standardbibliothek des .NET erstellt wurde, können Sie einen Verweis darauf aus einem kompatiblen Anwendung oder Library-Projekt auf die gleiche Weise hinzufügen, Sie normalerweise fügen Sie Verweise hinzu. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie `Add Reference...` wechseln Sie zu der `Solution : Projects` Registerkarte wie gezeigt:

![Verweisen auf eine Standardbibliothek des .NET](net-standard-images/vs04.png "In Visual Studio mit der rechten Maustaste auf den Knoten \"Verweise\" und wählen Sie Verweis hinzufügen... und wechseln Sie dann auf der Registerkarte Projektmappenprojekte wie dargestellt")

-----

