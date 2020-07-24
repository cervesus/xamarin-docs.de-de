---
title: Programmieren von UrhoSharp mit F#
description: 'In diesem Dokument wird beschrieben, wie eine einfache "Hello World urhusharp"-Anwendung mithilfe von F # in Visual Studio für Mac erstellt wird.'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: af9619ace957a47282cbf9fdefea4e81e7eace13
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86940009"
---
# <a name="programming-urhosharp-with-f"></a>Programmieren von urhusharp mit F\#

Urhusharp kann mit F # programmiert werden. dabei werden dieselben Bibliotheken und Konzepte verwendet, die auch für c#-Programmierer verwendet werden. Der Artikel [using urhusharp](~/graphics-games/urhosharp/using.md) enthält eine Übersicht über die urhusharp-Engine und sollte vor diesem Artikel gelesen werden.

Wie viele Bibliotheken, die in der C++-Welt entstanden sind, geben viele urhosharp-Funktionen boolesche Werte oder ganze Zahlen zurück, die auf Erfolg oder Fehler hinweisen. Verwenden Sie `|> ignore` , um diese Werte zu ignorieren.

Das [Beispielprogramm](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) ist eine "Hallo Welt" für urhusharp von F #.

## <a name="creating-an-empty-project"></a>Erstellen eines leeren Projekts

Es sind noch keine F #-Vorlagen für urhusharp verfügbar. zum Erstellen eines eigenen urhusharp-Projekts können Sie also entweder mit dem [Beispiel](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) beginnen oder die folgenden Schritte ausführen:

1. Erstellen Sie in Visual Studio für Mac **eine neue Projekt**Mappe. Wählen Sie **IOS-> App > Single View App** aus, und wählen Sie **F #** als Implementierungssprache aus. 
1. Löschen Sie die Datei " **Main. Storyboard** ". Öffnen Sie die Datei " **Info. plist** ", und löschen Sie im Bereich " **iPhone/iPod Deployment Info** " die `Main` Zeichenfolge in der Dropdown Liste **Hauptschnittstelle** .
1. Löschen Sie auch die Datei " **ViewController. FS** ".

## <a name="building-hello-world-in-urho"></a>Hallo Welt in Urho wird aufgebaut

Nun können Sie mit dem Definieren der Spielklassen beginnen. Sie müssen mindestens eine Unterklasse von definieren `Urho.Application` und deren-Methode überschreiben `Start` . Um diese Datei zu erstellen, klicken Sie mit der rechten Maustaste auf das F #-Projekt, wählen Sie **neue Datei hinzufügen** aus, und fügen Sie dem Projekt eine leere F #-Klasse hinzu. Die neue Datei wird am Ende der Liste der Dateien in Ihrem Projekt hinzugefügt. Sie müssen Sie jedoch ziehen, damit Sie angezeigt wird, *bevor* Sie in **appdelegaten. FS**verwendet wird.

1. Fügen Sie einen Verweis auf das Urho nuget-Paket hinzu.
1. Kopieren Sie aus einem vorhandenen Urho-Projekt die (großen) Verzeichnisse **CoreData/** und **Data/** in die **Ressourcen/** Verzeichnisse Ihres Projekts. Klicken Sie im F #-Projekt mit der rechten Maustaste auf den Ordner **Ressourcen** , und fügen Sie dem Projekt diese Dateien mit **Hinzufügen/vorhandenem Ordner** hinzu.

Die Projektstruktur sollte nun wie folgt aussehen:

![Die Projektstruktur sollte nun wie folgt aussehen.](fsharp-images/solutionpane.png)

Definieren Sie die neu erstellte Klasse als Untertyp von, `Urho.Application` und überschreiben Sie die- `Start` Methode:

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Der Code ist sehr unkompliziert. Es verwendet die `Urho.Gui.Text` -Klasse, um eine zentriert ausgerichtete Zeichenfolge mit einer bestimmten Schriftart und farbgröße anzuzeigen. 

Bevor dieser Code ausgeführt werden kann, muss jedoch urhusharp initialisiert werden. 

Öffnen Sie die Datei appdelegat. FS, und ändern Sie die- `FinishedLaunching` Methode wie folgt:

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default`Stellt die Standardoptionen für eine Landscape-Modus-Anwendung bereit. Übergeben Sie diese `ApplicationOptions` an den Standardkonstruktor für die `Application` Unterklasse (Beachten Sie, dass `HelloWorld` die Zeile beim Definieren der `inherit Application(o)` -Klasse den Basisklassenkonstruktor aufruft).

Die- `Run` Methode des `Application` initiiert das Programm. Sie wird als zurückgeben eines-Wert definiert `int` , an den weitergeleitet werden kann `ignore` .

Das resultierende Programm sollte wie in diesem Screenshot aussehen:

![Screenshot des resultierenden Programms](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>Verwandte Links

- [Durchsuchen auf GitHub (Beispiel)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
