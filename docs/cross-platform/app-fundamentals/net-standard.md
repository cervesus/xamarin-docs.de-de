---
title: .NET-Standard
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 20bbb6386de3fb83df22de5df6e9ee99e3913eaa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="net-standard"></a>.NET-Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>Freigeben von Code über den .NET Standard-Bibliotheksprojekte

Die .NET-Standardbibliothek ist eine formale Spezifikation von .NET-APIs, die für alle .NET-Laufzeiten verfügbar sein sollen. Die Motivation hinter der Standardbibliothek ist das Herstellen einer umfassenderen Einheitlichkeit im .NET-Ökosystem.
Auch wenn [ECMA-335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) weiterhin für Einheitlichkeit im .NET-Laufzeitverhalten sorgt, gibt es keine ähnliche Spezifikation für die .NET-Basisklassenbibliotheken (BCL) für Implementierungen der .NET-Bibliothek.

Sie können sozusagen eine vereinfachte nächste Generation von [Portable Class Library](https://msdn.microsoft.com/library/gg597391.aspx).
Es ist eine eine einheitliche API für alle .NET Plattformen einschließlich .NET Core Library. Sie gerade erstellen eine einzelne .NET Standardbibliothek und verwenden ihn aus jeder Laufzeit, die .NET Standard-Plattform unterstützt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Dieser Abschnitt führt Sie durch die Schritte zum Erstellen und Verwenden einer .NET Standardbibliothek, die mit Visual Studio für Mac Finden Sie im Beispielabschnitt von .NET Standard-Bibliothek für eine vollständige Implementierung.

### <a name="creating-a-net-standard-library"></a>Erstellen eine Standardbibliothek des .NET

Der Projektmappe eine Standardbibliothek des .NET hinzuzufügen ist ziemlich unkompliziert.

1. Klicken Sie im Dialogfeld "Neues Projekt hinzufügen" Wählen Sie die `.NET Core` Kategorie, und wählen Sie dann `Class Library(.NET Core)`.

  **Hinweis:** dieser Vorlage wird umbenannt werden, um `.NET Standard` in einer zukünftigen Version von Visual Studio für Mac.

  ![Erstellen einer Klassenbibliothek von .NET Core](net-standard-images/vsm01.png)

2. Das standardmäßige .NET-Steuerelementbibliothek-Projekt wird angezeigt, wie im Projektmappen-Explorer angezeigt. Der Knoten für die Abhängigkeiten hervor, dass die Bibliothek verwendet die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Abhängigkeiten von Knoten in der Projektmappe gibt .NET Standard an.](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Bearbeiten der Einstellungen für .NET-Standardbibliothek

Die Standardbibliothek .NET Einstellungen angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und auswählen können `Options` wie in diesem Screenshot gezeigt:

![Bearbeiten von .NET Standard Zielframeworks in Projektoptionen](net-standard-images/vsm03.png)

Sie können innerhalb Ihrer Version von ändern `netstandard` durch Ändern der `Target Framework` Dropdown-Wert.

**Darüber hinaus gilt:** können Sie bearbeiten die `.csproj` direkt zum Ändern dieses Werts.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

In diesem Abschnitt führt Sie durch die Schritte zum Erstellen und verwenden eine mithilfe von Visual Studio .NET-Standardbibliothek. Finden Sie im Beispielabschnitt von .NET Standard-Bibliothek für eine vollständige Implementierung.

### <a name="creating-a-net-standard-library"></a>Erstellen eine Standardbibliothek des .NET

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Der Projektmappe eine Standardbibliothek des .NET hinzuzufügen ist ziemlich unkompliziert.

1. Klicken Sie im Dialogfeld "Neues Projekt hinzufügen" Wählen Sie die `.NET Standard` Kategorie, und wählen Sie dann `Class Library(.NET Standard)`.

  ![](net-standard-images/vs01.png "Erstellen Sie neue .NET Standard-Klassenbibliothek")

2. Das standardmäßige .NET-Steuerelementbibliothek-Projekt wird angezeigt, wie im Projektmappen-Explorer angezeigt. Der Knoten für die Abhängigkeiten hervor, dass die Bibliothek verwendet die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![](net-standard-images/vs02.png ".NET standard Projekt in der Projektmappe")

#### <a name="editing-net-standard-library-settings"></a>Bearbeiten der Einstellungen für .NET-Standardbibliothek

Die Standardbibliothek .NET Einstellungen angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und auswählen können `Properties` wie in diesem Screenshot gezeigt:

![](net-standard-images/vs03.png "Verweisen auf eine .NET Standardbibliothek die gleiche Weise wie andere Projekte")

Sie können innerhalb Ihrer Version von ändern `netstandard` durch Ändern der `Target Framework` Dropdown-Wert.

**Darüber hinaus gilt:** können Sie bearbeiten die `.csproj` direkt zum Ändern dieses Werts.

#### <a name="using-net-standard-library"></a>Mithilfe der Standardbibliothek für .NET

Sobald eine Standardbibliothek des .NET erstellt wurde, können Sie einen Verweis darauf aus einem kompatiblen Anwendung oder Library-Projekt auf die gleiche Weise hinzufügen, Sie normalerweise fügen Sie Verweise hinzu. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie `Add Reference...` wechseln Sie zu der `Solution : Projects` Registerkarte wie gezeigt:

![](net-standard-images/vs04.png "In Visual Studio mit der rechten Maustaste auf den Knoten "Verweise" und wählen Sie Verweis hinzufügen... und wechseln Sie dann auf der Registerkarte Projektmappenprojekte wie dargestellt")

-----

