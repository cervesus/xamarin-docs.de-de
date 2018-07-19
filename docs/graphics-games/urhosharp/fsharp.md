---
title: Programmieren von UrhoSharp mit f#
description: In diesem Dokument wird beschrieben, wie zum Erstellen einer einfachen Hello World von UrhoSharp-Anwendung mit f# in Visual Studio für Mac.
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: a4e1a31a2591c799a153e1333e4a4a4a0719a107
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111198"
---
# <a name="programming-urhosharp-with-f"></a>Programmieren von UrhoSharp mit f#

Von UrhoSharp kann mit f# mit dem gleichen Bibliotheken und Konzepte, die ein c#-Programmierer, die so programmiert werden. Die [Verwenden von UrhoSharp](~/graphics-games/urhosharp/using.md) Artikel bietet einen Überblick über die von UrhoSharp-Engine und gelesen werden sollen, bevor Sie in diesem Artikel.

Geben Sie z.B. viele Bibliotheken, die in der C++-Welt stammen, viele von UrhoSharp-Funktionen zurück, boolesche Werte oder ganze Zahlen enthält, der angibt, Erfolg oder Fehler. Verwenden Sie `|> ignore` diese Werte ignoriert werden sollen.

Die [Beispielprogramm](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) von UrhoSharp von f# ist eine "Hello World".

## <a name="creating-an-empty-project"></a>Erstellen ein leeres Projekt

Es sind keine F#-Vorlagen für von UrhoSharp noch verfügbar ist, also zum Erstellen Ihres eigenen Projekts verwenden von UrhoSharp können Sie entweder beginnen Sie mit der [Beispiel](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) oder gehen Sie folgendermaßen vor:

1. In Visual Studio für Mac, erstellen Sie ein neues **Lösung**. Wählen Sie **iOS > App > Einzelansicht-App** , und wählen Sie **f#** als Implementierungssprache für die. 
1. Löschen der **"Main.Storyboard"** Datei. Öffnen der **"Info.plist"** Datei und klicken Sie in der **iPhone / iPod Bereitstellungsinformationen** Bereich Löschen der `Main` in eine Zeichenfolge der **Hauptschnittstelle** Dropdownliste.
1. Löschen der **ViewController.fs** -Datei.

## <a name="building-hello-world-in-urho"></a>Erstellen von Hallo Welt in Urho

Sie können nun damit beginnen, des Spiels Klassen definieren. Zumindest müssen Sie definieren eine Unterklasse von `Urho.Application` und überschreiben seine `Start` Methode. Um diese Datei zu erstellen, mit der rechten Maustaste auf das F#-Projekt, wählen Sie **neue Datei hinzufügen...**  , und fügen Sie dem Projekt eine leere F#-Klasse hinzu. Die neue Datei wird am Ende der Liste der Dateien in Ihrem Projekt hinzugefügt werden, jedoch müssen Sie sie ziehen, sodass es angezeigt wird *vor* wird verwendet in **AppDelegate.fs**.

1. Fügen Sie einen Verweis auf das Urho NuGet-Paket hinzu.
1. Aus einem vorhandenen Urho-Projekt, kopieren Sie die Verzeichnisse (groß) **CoreData /** und **Daten /** des Projekts **Ressourcen /** Verzeichnis. F#-Projekts, mit der Maustaste auf die **Ressourcen** Ordner und die Verwendung **hinzu, oder fügen Sie vorhandene Ordner** alle diese Dateien zu Ihrem Projekt hinzufügen.

Die Projektstruktur sollte jetzt aussehen:

![](fsharp-images/solutionpane.png "Die Projektstruktur sollte nun wie aussehen.")

Definieren Sie die neu erstellte Klasse als ein Untertyp von `Urho.Application` und überschreiben seine `Start` Methode:

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

Der Code ist sehr einfach. Er verwendet den `Urho.Gui.Text` Klasse, um eine Center ausgerichtete Zeichenfolge mit einer bestimmten Größe Schriftart und Farbe anzuzeigen. 

Bevor Sie diesen Code ausführen kann, muss jedoch von UrhoSharp initialisiert werden. 

Öffnen Sie die AppDelegate.fs-Datei, und Ändern der `FinishedLaunching` -Methode wie folgt:

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

Die `ApplicationOptions.Default` bietet die Standardoptionen für eine Anwendung im Querformat. Übergeben Sie diese `ApplicationOptions` des Standardkonstruktors für Ihre `Application` Unterklasse (Beachten Sie, dass beim Definieren der `HelloWorld` Klasse, die Zeile `inherit Application(o)` ruft der Konstruktor der Basisklasse). 

Die `Run` Methode Ihrer `Application` initiiert das Programm. Es wird definiert als Rückgabewert einer `int`, die weitergeleitet werden kann, um `ignore`. 

Das entsprechende Programm aussehen sollte:

![](fsharp-images/helloworldfsharp.png "Wie soll das entsprechende Programm aussehen.")








## <a name="related-links"></a>Verwandte Links

- [Navigieren Sie auf GitHub an (Beispiel)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
