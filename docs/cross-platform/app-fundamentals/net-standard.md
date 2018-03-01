---
title: .NET-Standard
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 294d28c57978218986d62d1ee6579e8d283b8f72
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="net-standard"></a>.NET-Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>Freigeben von Code über den .NET Standard-Bibliotheksprojekte

Die .NET-Standardbibliothek ist eine formale Spezifikation von .NET-APIs, die für alle .NET-Laufzeiten verfügbar sein sollen. Die Motivation hinter der Standardbibliothek ist das Herstellen einer umfassenderen Einheitlichkeit im .NET-Ökosystem.
Auch wenn [ECMA-335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) weiterhin für Einheitlichkeit im .NET-Laufzeitverhalten sorgt, gibt es keine ähnliche Spezifikation für die .NET-Basisklassenbibliotheken (BCL) für Implementierungen der .NET-Bibliothek.

Sie können sozusagen eine vereinfachte nächste Generation von [Portable Class Library](https://msdn.microsoft.com/library/gg597391.aspx).
Es ist eine eine einheitliche API für alle .NET Plattformen einschließlich .NET Core Library. Sie gerade erstellen eine einzelne .NET Standardbibliothek und verwenden ihn aus jeder Laufzeit, die .NET Standard-Plattform unterstützt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="xamarin-studio"></a>Xamarin Studio

.NET standard Library-Projekte können in Xamarin Studio 6.2 erstellt werden, indem zunächst eine Portable Library-Projekts erstellt:

[ ![](net-standard-images/xs01-sml.png "Erstellen Sie eine neue portable Library-Projekts")](net-standard-images/xs01.png)

Nachdem das Projekt erstellt wurde, mit der rechten Maustaste, und öffnen Sie die **Projektoptionen** Fenster.
In der **allgemeine** Abschnitt das Projekt auf .NET Standard konvertiert und so festgelegt, dass eine bestimmte Version in der **Plattform** Dropdown-Liste:

[ ![](net-standard-images/xs02-sml.png "Im allgemeinen Optionen in standardmäßigen .NET konvertieren")](net-standard-images/xs02.png)

Sie können dann [erstellen Sie ein NuGet-Paket](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library.md) auf die Bibliothek mit anderen Entwicklern gemeinsam nutzen.

## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise für Visual Studio für Mac

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

## <a name="visual-studio-windows-walkthrough"></a>Exemplarische Vorgehensweise die Visual Studio (Windows)

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


## <a name="related-links"></a>Verwandte Links

- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#.NET_Standard_Support)
