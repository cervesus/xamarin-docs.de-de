---
title: .NET-Standard
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: c70a1cb1aa05426ba6d54d8af3787f014883bfa1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="net-standard"></a>.NET-Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>Freigeben von Code über den .NET Standard-Bibliotheksprojekte

Die .NET Standard-Bibliothek ist eine formale Spezifikation von .NET-APIs, die für alle .NET-Laufzeiten verfügbar sein sollen. Die Motivation hinter der Standard-Bibliothek ist das Herstellen einer umfassenderen Einheitlichkeit im .NET-Ökosystem.
Auch wenn [ECMA-335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) weiterhin für Einheitlichkeit im .NET-Laufzeitverhalten sorgt, gibt es keine ähnliche Spezifikation für die .NET-Basisklassenbibliotheken (BCL) für Implementierungen der .NET-Bibliothek.

Sie können es sich als eine vereinfachte nächste Generation der [Portable Class Library](https://msdn.microsoft.com/library/gg597391.aspx) vorstellen.
Es ist eine einzige Bibliothek mit einheitlicher API für alle .NET-Plattformen einschließlich .NET Core. Sie erstellen nur eine einzige .NET Standard-Bibliothek und verwenden diese in jeder Laufzeit, welche die .NET Standard-Plattform unterstützt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Dieser Abschnitt zeigt die Schritte zum Erstellen und Verwenden einer .NET Standard-Bibliothek mit Visual Studio für Mac. Eine vollständige Implementierung finden Sie im Beispielabschnitt der .NET Standard-Bibliothek.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard-Bibliothek

Der Projektmappe eine .NET Standard-Bibliothek hinzuzufügen ist ziemlich unkompliziert.

1. Wählen Sie im Dialogfeld „Neues Projekt hinzufügen“ die Kategorie „.NET Core“ aus, und wählen Sie dann „Klassenbibliothek (.NET Core)“ aus.

  **Hinweis:** Diese Vorlage wird in einer zukünftigen Version von Visual Studio für Mac in „.NET Standard“ umbenannt werden.

  ![Erstellen einer neuen .NET Core-Klassenbibliothek](net-standard-images/vsm01.png "Erstellen einer neuen .NET Core-Klassenbibliothek")

2. Das .NET Standard-Bibliotheksprojekt wird im Projektmappen-Explorer wie dargestellt angezeigt. Der Knoten mit den Abhängigkeiten zeigt, dass die Bibliothek die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) verwendet.

  ![NETStandard.Library in der Projektmappe](net-standard-images/vsm02.png "NETStandard.Library in der Projektmappe")

#### <a name="editing-net-standard-library-settings"></a>Einstellungen einer .NET Standard-Bibliothek bearbeiten

Die Einstellungen der .NET Standard-Bibliothek können angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt klicken und „Optionen“ wählen, wie in diesem Screenshot gezeigt:

  ![Bearbeiten von .NET Standard Zielframeworks in Projektoptionen](net-standard-images/vsm03.png "Bearbeiten Sie die Version des .NET Standard Zielframeworks in den Projektoptionen")

Dort können Sie Ihre Version von „.NET Standard“ durch Ändern des Dropdown-Werts „Zielframework“ anpassen.

**Darüber hinaus gilt:** Sie können die `.csproj`-Datei zum Ändern dieses Werts direkt bearbeiten.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Dieser Abschnitt zeigt die Schritte zum Erstellen und Verwenden einer .NET Standard-Bibliothek mit Visual Studio. Eine vollständige Implementierung finden Sie im Beispielabschnitt der .NET Standard-Bibliothek.

### <a name="creating-a-net-standard-library"></a>Erstellen einer .NET Standard-Bibliothek

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Der Projektmappe eine .NET Standard-Bibliothek hinzuzufügen ist ziemlich unkompliziert.

1. Wählen Sie im Dialogfeld „Neues Projekt hinzufügen“ die Kategorie „.NET Standard“ aus, und wählen Sie dann „Klassenbibliothek (.NET Standard)“ aus.

  ![Erstellen einer neuen .NET Standard-Klassenbibliothek](net-standard-images/vs01.png "Erstellen einer neuen .NET Standard-Klassenbibliothek")

2. Das .NET Standard-Bibliotheksprojekt wird im Projektmappen-Explorer wie dargestellt angezeigt. Der Knoten mit den Abhängigkeiten zeigt, dass die Bibliothek die [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) verwendet.

  ![NETStandard.Library in der Projektmappe](net-standard-images/vs02.png "NETStandard.Library in der Projektmappe")

#### <a name="editing-net-standard-library-settings"></a>Einstellungen einer .NET Standard-Bibliothek bearbeiten

Die Einstellungen der .NET Standard-Bibliothek können angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt klicken und „Eigenschaften“ wählen, wie in diesem Screenshot gezeigt:

  ![Bearbeiten von .NET Standard-Zielframeworks in Projekteigenschaften](net-standard-images/vs03.png "Bearbeiten Sie die Version des .NET Standard-Zielframeworks in den Projekteigenschaften")

Dort können Sie Ihre Version von „.NET Standard“ durch Ändern des Dropdown-Werts „Zielframework“ anpassen.

**Darüber hinaus gilt:** Sie können die `.csproj`-Datei zum Ändern dieses Werts direkt bearbeiten.

#### <a name="using-net-standard-library"></a>Verwenden einer .NET Standard-Bibliothek

Sobald eine .NET Standard-Bibliothek erstellt wurde, können Sie aus allen kompatiblen Anwendungen oder Bibliotheksprojekten einen Verweis auf die gleiche Weise hinzufügen, wie Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten „Verweise“, und wählen Sie „Verweis hinzufügen...“ aus. Wechseln Sie anschließend wie hier gezeigt zur Registerkarte „Projekte: Projektmappe“:

  ![Verweisen auf eine .NET Standard-Bibliothek](net-standard-images/vs04.png "Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten \"Verweise\", wählen Sie \"Verweis hinzufügen...\", und wechseln Sie dann auf die Registerkarte der Projektmappen-Projekte wie dargestellt")

-----

