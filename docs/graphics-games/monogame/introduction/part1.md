---
title: Teil 1 – Erstellen eines plattformübergreifenden monogame
description: In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie ein neues Projekt für IOS und Android mithilfe von monogame erstellen. Das Ergebnis ist eine Visual Studio für Mac Lösung mit einem plattformübergreifenden gemeinsam genutzten Code Projekt und einem Projekt für jede Plattform. Bei der Ausführung dieses Projekts wird ein leerer blauer Bildschirm angezeigt.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: d72c428bb4b8c88365180c5c3c50b107eed2b21d
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68978445"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Teil 1 – Erstellen eines plattformübergreifenden monogame

_In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie ein neues Projekt für IOS und Android mithilfe von monogame erstellen. Das Ergebnis ist eine Visual Studio für Mac Lösung mit einem plattformübergreifenden gemeinsam genutzten Code Projekt und einem Projekt für jede Plattform. Beim Ausführen dieses Projekts wird ein leerer blauer Bildschirm angezeigt._

Monogame ermöglicht die Entwicklung plattformübergreifender Spiele mit einem großen Teil der Wiederverwendung von Code. Diese exemplarische Vorgehensweise konzentriert sich auf das Einrichten einer Projekt Mappe, die Projekte für IOS und Android enthält, sowie ein frei gegebenes Code Projekt für plattformübergreifenden Code.

Nach Abschluss des Vorgangs verfügt das Projekt über die richtige Struktur zum Ausführen von Spiel Update Logik und Spiel Zeichnungs Logik bei 30 Frames pro Sekunde. Sie kann als Basisprojekt für ein beliebiges monogame-Projekt verwendet werden. Das Projekt sieht wie folgt aus, wenn es ausgeführt wird:

![Leerer blauer Bildschirm](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio"></a>Hinzufügen von monogame zu Visual Studio

> [!IMPORTANT]
> Monogame wird nicht standardmäßig in Visual Studio 2019 oder Visual Studio für Mac installiert.
>
> Sie sollten die neueste Version manuell herunterladen und installieren http://www.monogame.net/downloads/ dann das Installationsprogramm ausführen. Möglicherweise müssen Sie Visual Studio neu starten, damit die Vorlagen angezeigt werden.
>
> Der Abschnitt **Spielentwicklung** sollte dann im **Add-in-Manager**angezeigt werden.

Um das monogame-Add-in für Visual Studio für Mac zu aktivieren, wählen Sie **Visual Studio für Mac**  > **Add-in-Manager...** aus. Wählen Sie für Visual Studio 2019 unter Windows **Tools**  > **Add-in-Manager...** aus. Wählen Sie **die Registerkarte** Katalog aus, erweitern Sie die Kategorie **Game Development** , wählen Sie **monogame AddIn**aus, und klicken Sie dann auf **installieren...** :

![Visual Studio für Mac Extensions Gallery auswählen von monogame](part1-images/image2.png)

Nach der Installation werden monogame-Vorlagen in Visual Studio für Mac angezeigt, wie im nächsten Abschnitt erläutert wird.

## <a name="creating-a-new-solution"></a>Erstellen einer neuen Projekt Mappe

Wählen Sie in Visual Studio für Mac **Datei > neue Projekt Mappe**aus. Klicken Sie im Dialogfeld **Neues Projekt** auf **Verschiedenes**, Scrollen Sie zum Abschnitt **Allgemein** , wählen Sie die Option **universelle monogame-Anwendung** aus, und klicken Sie auf Weiter.

![Dialogfeld "Neues Projekt" Erstellen einer monogame-Anwendung](part1-images/image3.png)

Nennen Sie das Projekt walkinggame, und klicken Sie auf erstellen:

![Dialogfeld "Neues Projekt" mit Name und Speicherort](part1-images/image4.png)

Nun wird das Projekt wie jedes andere IOS-oder Android-Projekt ausgeführt. Das Projekt sollte mit dem blauen Kornblumen Hintergrund ausgeführt werden:

![Leerer blauer App-Hintergrund](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Beheben von Android-Kompilierungs Fehlern

Die aktuelle Version von monogame-Vorlagen enthält einige Syntax Fehler in der `Activity1.cs` Datei von Android. Um diese Probleme zu beheben, ersetzen Sie die `OnCreate`-Funktion durch Folgendes:

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

In dieser exemplarischen Vorgehensweise wird erläutert, wie Sie ein plattformübergreifendes monogame-Projekt mit Visual Studio für Mac erstellen. Das Ergebnis ist ein leerer blauer Bildschirm. Dieses Projekt kann als Ausgangspunkt für IOS-und Android-Spiele verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Monogame Android nuget](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [Monogame IOS nuget](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Monogame-API-Dokumentation](http://www.monogame.net/documentation/?page=main)
