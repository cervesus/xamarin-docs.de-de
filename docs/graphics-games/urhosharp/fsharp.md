---
title: UrhoSharp der Programmierung mit f#
description: 'Vorgehensweise: erstellen eine einfache UrhoSharp-Anwendung mit f# in Visual Studio für Mac'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 1fff90056f39660695c1e6d9ed307fad575b5127
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="programming-urhosharp-with-f"></a>UrhoSharp der Programmierung mit f#

_Vorgehensweise: erstellen eine einfache UrhoSharp-Anwendung mit f# in Visual Studio für Mac_

Mit f#-mit dem gleichen Bibliotheken und Konzepte von C#-Programmierer kann UrhoSharp programmiert werden. Die [UrhoSharp verwenden](~/graphics-games/urhosharp/using.md) Artikel vermittelt einen Überblick über das Modul UrhoSharp und gelesen werden sollen, bevor Sie in diesem Artikel.

Wie viele Bibliotheken, die in der C++-Welt stammen, zurück viele UrhoSharp Funktionen boolesche Werte oder ganze Zahlen, wodurch der Erfolg oder Fehler. Verwenden Sie `|> ignore` , diese Werte ignoriert werden sollen.

Die [Beispielprogramm](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) UrhoSharp von f# ist eine "Hello World".

## <a name="creating-an-empty-project"></a>Erstellen ein leeres Projekt

Es sind keine f#-Vorlagen für UrhoSharp noch zur Verfügung, daher Erstellen Ihres eigenen Projekts UrhoSharp können Sie entweder beginnen Sie mit der [Beispiel](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) oder gehen Sie folgendermaßen vor:

1. In Visual Studio für Mac, erstellen Sie ein neues **Lösung**. Wählen Sie **iOS > App > einzelne Ansicht App** , und wählen Sie **f#** als Implementierungssprache für die. 
1. Löschen der **Main.storyboard** Datei. Öffnen der **"Info.plist"** Datei und klicken Sie in der **iPhone / iPod Bereitstellungsinformationen** Bereich Löschen der `Main` Zeichenfolge in der **Hauptbenutzeroberfläche** Dropdownliste.
1. Löschen der **ViewController.fs** ebenfalls.

## <a name="building-hello-world-in-urho"></a>Erstellen von Hello World in Urho

Sie können nun zum Starten des Spiels Klassen definieren. Zumindest müssen Sie eine Unterklasse von definieren `Urho.Application` und überschreiben die `Start` Methode. Um diese Datei zu erstellen, mit der rechten Maustaste auf das f#-Projekt, wählen Sie **neue Datei hinzufügen...**  und dem Projekt eine leere F#-Klasse hinzufügen. Die neue Datei wird am Ende der Liste der Dateien in Ihrem Projekt hinzugefügt werden, aber müssen, damit er angezeigt wird ziehen *vor* verwendet **AppDelegate.fs**.

1. Fügen Sie einen Verweis auf das Urho NuGet-Paket.
1. Kopieren Sie aus einem vorhandenen Urho-Projekt (groß) Verzeichnisse **CoreData /** und **Daten /** in Ihr Projekts **Ressourcen /** Verzeichnis. F#-Projekts, mit der Maustaste auf die **Ressourcen** Ordner und Verwendung **hinzufügen oder vorhandene Ordner hinzufügen** all diese Dateien zum Projekt hinzufügen.

Die Projektstruktur sollte jetzt wie aussehen:

![](fsharp-images/solutionpane.png "Die Projektstruktur müsste jetzt wie aussehen.")

Definieren Sie die neu erstellten Klasse als Untertyp `Urho.Application` und überschreiben die `Start` Methode:

```csharp
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

Der Code ist sehr einfach. Er verwendet die `Urho.Gui.Text` Klasse, um eine Center ausgerichtete Zeichenfolge mit einer bestimmten Schriftart und Farbe Größe anzuzeigen. 

Bevor Sie diesen Code ausführen kann, muss jedoch UrhoSharp initialisiert werden. 

Öffnen Sie die Datei AppDelegate.fs und ändern die `FinishedLaunching` Methode wie folgt:

```csharp
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

Die `ApplicationOptions.Default` bietet die Standardoptionen für eine Anwendung im Querformat. Übergeben Sie diese `ApplicationOptions` Standardkonstruktor für Ihre `Application` Unterklasse (Beachten Sie, dass beim Definieren der `HelloWorld` Klasse, die Zeile `inherit Application(o)` Ruft den Konstruktor der Basisklasse). 

Die `Run` Methode Ihrer `Application` initiiert das Programm. Als Rückgabe definiert ein `int`, dem können werden über die Pipeline an `ignore`. 

Das resultierende Programm aussehen sollte:

![](fsharp-images/helloworldfsharp.png "Das resultierende Programm sollte ähneln.")








## <a name="related-links"></a>Verwandte Links

- [Navigieren Sie auf GitHub (Beispiel)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
