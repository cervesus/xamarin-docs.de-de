---
title: Teil 1 – erstellen eine plattformübergreifende MonoGame
description: In dieser exemplarischen Vorgehensweise wird das Erstellen eines neuen Projekts für IOS- und Android mithilfe von MonoGame veranschaulicht. Das Ergebnis ist eine Visual Studio für Mac-Lösung mit einem Projekt mit freigegebenem plattformübergreifende-Code als auch ein Projekt für jede Plattform. Dieses Projekt wird einen leeren blauen Bildschirm bei der Ausführung angezeigt.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bd7990b94e678c205f9ce636f4eb0d28180fc6ec
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921988"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Teil 1 – erstellen eine plattformübergreifende MonoGame

_In dieser exemplarischen Vorgehensweise wird das Erstellen eines neuen Projekts für IOS- und Android mithilfe von MonoGame veranschaulicht. Das Ergebnis ist eine Visual Studio für Mac-Lösung mit einem Projekt mit freigegebenem plattformübergreifende-Code als auch ein Projekt für jede Plattform. Dieses Projekt wird einen leeren blauen Bildschirm bei der Ausführung angezeigt._

MonoGame ermöglicht die Entwicklung von plattformübergreifende Spiele mit großen Teil der Wiederverwendung von Code. In dieser exemplarischen Vorgehensweise konzentriert sich auf das Einrichten einer Lösung, die für IOS- und Android-Projekte als auch ein Projekt mit freigegebenem Code für plattformübergreifenden Code enthält.

Wenn wir fertig sind, wir haben ein Projekt, das zum Ausführen der Logik der spielaktualisierung die richtige Struktur hat und Spiel, zeichnen Logik zur 30 Frames pro Sekunde. Sie können für jedes Projekt MonoGame wie das Basis-Projekt verwendet werden. Unser Projekt wird bei der Ausführung sieht folgendermaßen aus:

![Leere blauen Bildschirm](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Hinzufügen von MonoGame zu Visual Studio für Mac

MonoGame kann als ein Add-in zu Visual Studio für Mac hinzugefügt werden Wählen Sie auf Mac **Visual Studio für Mac** > **Add-In-Manager...**  . Auf Windows-Tools aktivieren ** ** > **Add-In-Manager...**  . Wählen Sie die **Katalog** Registerkarte, erweitern Sie die **Game Development** Kategorie, und wählen **MonoGame-Add-in**, klicken Sie dann auf **installieren**:

![Visual Studio für Mac-erweiterungenkatalog MonoGame auswählen](part1-images/image2.png)

> [!IMPORTANT]
> **Hinweis**: Wenn die **Game Development** Abschnitt nicht im Add-In-Manager angezeigt wird, manuell herunterladen und installieren Sie die neueste Version von: http://www.monogame.net/downloads/. Sie müssen möglicherweise ein Neustart von Visual Studio für Mac für die Vorlagen angezeigt werden.

Nach Abschluss der Installation werden MonoGame Vorlagen werden in Visual Studio für Mac, wie wir im nächsten Abschnitt sehen werden.

## <a name="creating-a-new-solution"></a>Eine neue Projektmappe erstellen

In Visual Studio für Mac Select **Datei > Neues Projektmappen**. In der **neues Projekt** Dialogfeld klicken Sie auf **Sonstiges**, einen Bildlauf zu der **allgemeine** Abschnitt der ** Universal MonoGame Mobile-Anwendung ** aus, und klicken Sie auf Weiter.

![Dialogfeld "Neues Projekt" Erstellen einer Anwendung MonoGame](part1-images/image3.png)

Nennen Sie das Projekt WalkingGame, und klicken Sie auf erstellen:

![Dialogfeld "Neues Projekt" einen Namen und Speicherort auswählen](part1-images/image4.png)

Nun wird unsere Projekt genau wie alle anderen IOS- oder Android-Projekt ausführen. Ausführen des Projekts sollte einen Cornflower blauen Hintergrund anzeigen:

![Leere blauen app-Hintergrund](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android-Kompilierung und Fehlerbehebung

Die aktuelle Version MonoGames Vorlagen enthält einige Syntaxfehler in der Android `Activity1.cs` Datei. Um diese Probleme zu beheben, ersetzen die `OnCreate` -Funktion durch Folgendes:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise behandelt, wie eine plattformübergreifende MonoGame-Projekt mit Visual Studio für Mac erstellt Das Ergebnis dieses ist eine leere blauen Bildschirm. Dieses Projekt kann als Ausgangspunkt für IOS- und Android Spiel verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API-Dokumentation](http://www.monogame.net/documentation/?page=main)
