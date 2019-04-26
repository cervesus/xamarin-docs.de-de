---
title: Teil 1 – Erstellen einer plattformübergreifenden MonoGame
description: Diese exemplarische Vorgehensweise veranschaulicht das Erstellen eines neuen Projekts für iOS und Android mithilfe von MonoGame. Das Ergebnis ist ein Visual Studio für Mac-Lösung mit einem gemeinsam genutzten Projektcode mit plattformübergreifenden als auch für ein Projekt für jede Plattform. Dieses Projekt zeigt einen blauen Bildschirm beim Ausführen.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 82b1408cafedf98a8619e8e039ba00b332f74516
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381845"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Teil 1 – Erstellen einer plattformübergreifenden MonoGame

_Diese exemplarische Vorgehensweise veranschaulicht das Erstellen eines neuen Projekts für iOS und Android mithilfe von MonoGame. Das Ergebnis ist ein Visual Studio für Mac-Lösung mit einem gemeinsam genutzten Projektcode mit plattformübergreifenden als auch für ein Projekt für jede Plattform. Dieses Projekt zeigt einen blauen Bildschirm beim Ausführen._

MonoGame ermöglicht die Entwicklung plattformübergreifender Spiele mit hohen Anteil der Wiederverwendung von Code. In dieser exemplarischen Vorgehensweise konzentriert sich auf das Einrichten einer Projektmappe enthält Projekte für iOS und Android als auch ein Projekt mit freigegebenem Code für die plattformübergreifenden Code.

Wenn wir fertig sind, wir haben ein Projekt, das zum Ausführen von spielaktualisierung Logik die richtige Struktur aufweist und zeichnen Logik zur 30 Frames pro Sekunde von Spielen. Sie können als Basis-Projekt für alle MonoGame-Projekts verwendet werden. Unser Projekt sieht folgendermaßen aus, wenn ausgeführt:

![Leere Bluescreen](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Hinzufügen von MonoGame zu Visual Studio für Mac

MonoGame kann als ein Add-in für Visual Studio für Mac hinzugefügt werden Wählen Sie auf Mac **Visual Studio für Mac** > **Add-In-Manager...**  . Auf Windows, wählen Sie ** Tools ** > **Add-In-Manager...**  . Wählen Sie die **Katalog** Registerkarte, erweitern Sie die **Game-Entwicklungsseite** Kategorie, und wählen **MonoGame-Add-in**, klicken Sie dann auf **installieren**:

![Visual Studio für Mac-erweiterungenkatalog MonoGame auswählen](part1-images/image2.png)

> [!IMPORTANT]
> **Hinweis:** Wenn die **Game-Entwicklungsseite** Abschnitt im Add-In-Manager nicht angezeigt wird, manuell herunterladen und Installieren der neuesten Version von hier aus: http://www.monogame.net/downloads/. Sie müssen möglicherweise Neustart von Visual Studio für Mac für die Vorlagen angezeigt werden.

Nach Abschluss der Installation werden MonoGame-Vorlagen in Visual Studio für Mac angezeigt, wie wir im nächsten Abschnitt sehen werden.

## <a name="creating-a-new-solution"></a>Erstellen einer neuen Projektmappe

In Visual Studio für Mac wählen **Datei > neue Projektmappe**. In der **neues Projekt** Dialogfeld klicken Sie auf **Sonstiges**, scrollen Sie zu der **allgemeine** wählen Sie im Abschnitt der ** universelle MonoGame-Mobile-Anwendung ** aus, und klicken Sie auf Weiter.

![Dialogfeld "Neues Projekt" Erstellen einer Anwendung MonoGame](part1-images/image3.png)

Nennen Sie das Projekt WalkingGame, und klicken Sie auf erstellen:

![Dialogfeld "Neues Projekt" auswählen, einen Namen und Speicherort](part1-images/image4.png)

Jetzt wird das Projekt genau wie alle anderen IOS- oder Android-Projekt ausgeführt. Ausführen des Projekts sollten Cornflower blauen Hintergrund angezeigt:

![Leere Blau-app-Hintergrund](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Beheben von Android-Kompilierungsfehler

Die aktuelle Version der MonoGame Vorlagen enthält einige Syntaxfehler in der Android `Activity1.cs` Datei. Um diese Probleme zu beheben, ersetzen die `OnCreate` -Funktion durch Folgendes:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erläutert, wie zum Erstellen einer plattformübergreifenden MonoGame-Projekts mit Visual Studio für Mac. Das Ergebnis davon ist es sich um einen blauen Bildschirm. Dieses Projekt kann als Ausgangspunkt für IOS- und Android-Spiel verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame-API-Dokumentation](http://www.monogame.net/documentation/?page=main)
