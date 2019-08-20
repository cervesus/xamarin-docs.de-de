---
title: Programmieren von urhusharp mitF#
description: In diesem Dokument wird beschrieben, wie eine einfache "Hello World urhusharp F# "-Anwendung mithilfe von in Visual Studio für Mac erstellt wird.
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: d87749bd74cf2c478e96284060fed7386d10b853
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69621003"
---
# <a name="programming-urhosharp-with-f"></a>Programmieren von urhusharp mit F\#

Urhusharp kann mit F# denselben Bibliotheken und Konzepten programmiert werden, die von C# Programmierern verwendet werden. Der Artikel [using urhusharp](~/graphics-games/urhosharp/using.md) enthält eine Übersicht über die urhusharp-Engine und sollte vor diesem Artikel gelesen werden.

Wie viele Bibliotheken, die aus der C++ Welt stammen, geben viele urhosharp-Funktionen boolesche Werte oder ganze Zahlen zurück, die auf Erfolg oder Fehler hinweisen. Verwenden `|> ignore` Sie, um diese Werte zu ignorieren.

Das [Beispielprogramm](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) ist ein "Hallo Welt" für urhusharp von F#.

## <a name="creating-an-empty-project"></a>Erstellen eines leeren Projekts

Es sind noch F# keine Vorlagen für urhusharp verfügbar. zum Erstellen eines eigenen urhusharp-Projekts können Sie also entweder mit dem [Beispiel](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) beginnen oder die folgenden Schritte ausführen:

1. Erstellen Sie in Visual Studio für Mac eine neueProjekt Mappe. Wählen Sie **IOS-> app > Einzelansicht** - **F#** App aus, und wählen Sie als Implementierungssprache aus. 
1. Löschen Sie die Datei " **Main. Storyboard** ". Öffnen Sie die Datei " **Info. plist** ", und löschen Sie im Bereich " **iPhone/iPod Deployment Info** " die `Main` Zeichenfolge in der Dropdown Liste **Hauptschnittstelle** .
1. Löschen Sie auch die Datei " **ViewController. FS** ".

## <a name="building-hello-world-in-urho"></a>Hallo Welt in Urho wird aufgebaut

Nun können Sie mit dem Definieren der Spielklassen beginnen. Sie müssen mindestens eine Unterklasse von `Urho.Application` definieren und deren `Start` -Methode überschreiben. Um diese Datei zu erstellen, klicken Sie mit der F# rechten Maustaste auf das Projekt, wählen Sie **neue Datei hinzufügen** aus F# , und fügen Sie dem Projekt eine leere Klasse hinzu. Die neue Datei wird am Ende der Liste der Dateien in Ihrem Projekt hinzugefügt. Sie müssen Sie jedoch ziehen, damit Sie angezeigt wird, *bevor* Sie in appdelegaten **. FS**verwendet wird.

1. Fügen Sie einen Verweis auf das Urho nuget-Paket hinzu.
1. Kopieren Sie aus einem vorhandenen Urho-Projekt die (großen) Verzeichnisse **CoreData/** und **Data/** in die **Ressourcen/** Verzeichnisse Ihres Projekts. Klicken Sie F# im Projekt mit der rechten Maustaste auf den Ordner **Ressourcen** , und fügen Sie alle diese Dateien dem Projekt mithilfe von **Hinzufügen/vorhandenem Ordner** hinzu.

Die Projektstruktur sollte nun wie folgt aussehen:

![](fsharp-images/solutionpane.png "Die Projektstruktur sollte nun wie folgt aussehen.")

Definieren Sie die neu erstellte Klasse als Untertyp von, `Urho.Application` und überschreiben `Start` Sie die-Methode:

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

Öffnen Sie die Datei appdelegat. FS, `FinishedLaunching` und ändern Sie die-Methode wie folgt:

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

`ApplicationOptions.Default` Stellt die Standardoptionen für eine Landscape-Modus-Anwendung bereit. Übergeben Sie `ApplicationOptions` diese an den Standardkonstruktor `Application` für die Unterklasse (Beachten Sie, dass `HelloWorld` die Zeile `inherit Application(o)` beim Definieren der-Klasse den Basisklassenkonstruktor aufruft).

Die `Run` -Methode `Application` des initiiert das Programm. Sie wird als zurückgeben eines `int`-Wert definiert, an den weiter `ignore`geleitet werden kann.

Das resultierende Programm sollte wie in diesem Screenshot aussehen:

![Screenshot des resultierenden Programms](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>Verwandte Links

- [Durchsuchen auf GitHub (Beispiel)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
