---
title: Erste Schritte mit macOS
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: c129079aad14ac9e8aad6f73670ce9a43a36f222
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-macos"></a>Erste Schritte mit macOS


## <a name="what-you-will-need"></a>Sie benötigen

* Führen Sie die Anweisungen in unseren [Einstieg in Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) Handbuch.

* Einen .NET Assembly für die Verwendung mit **Embeddinator 4000**.

* Eine MacOS Kakao-Anwendung

Fahren Sie nach dem Ausführen der Anweisungen in unseren [Einstieg in Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) Handbuch. Wenn Sie eine .NET Framework-Assembly noch können Sie direkt in überspringen **Embeddinator-4000 mithilfe** Abschnitt.

## <a name="creating-a-net-assembly"></a>Erstellen eine .NET Framework-assembly

Um eine .NET Framework-Assembly zu erstellen, Sie öffnen müssen [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) und erstellen Sie ein neues **.NET Library-Projekts** durch praktische *Datei > neue Projektmappe > andere > .NET > Bibliothek*. Klicken Sie auf Weiter, und geben Sie *Weather* als die *Projektname*, und klicken Sie auf *erstellen*.

Befolgen Sie die folgenden Schritte aus:

1. Löschen der **MyClass.cs** Datei und die **Eigenschaften** Ordner.

2. Klicken Sie mit der rechten Maustaste auf *Weather Projekt > Hinzufügen > neuen Datei.*

3. Wählen Sie *leere Klasse* und **XAMWeatherFetcher** als Name, klicken Sie dann auf neu.

4. Ersetzen Sie den Inhalt des *XAMWeatherFetcher.cs* durch den folgenden Code:

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

Sie werden feststellen, dass `Using System.Json;` generiert einen Fehler, um dieses Problem zu beheben, wir die folgenden Aktionen ausführen müssen:

1. Klicken Sie mit der Doppelklicken auf die **Verweise** Ordner.

2. Klicken Sie auf die **Pakete** Registerkarte.

3. Überprüfen Sie **System.Json**.

4. Click **Ok**.

Nach der oben genannten Kriterien alle erfolgt erforderlich ist, ist Build unsere .NET-Assembly durch Klicken auf *Sie im Menü erstellen > Build All* oder ⌘ + b. Erstellen Sie erfolgreich, die Nachricht in der oberen Statusleiste angezeigt werden soll.

Jetzt mit der rechten Maustaste auf *Weather* Projektknoten und wählen Sie *im Finder anzeigen*. Wechseln Sie im Finder zu den *"bin" / Debug* Ordner; innerhalb es finden Sie **Weather.dll.**

## <a name="using-embeddinator-4000"></a>Verwenden von Embeddinator 4000

Wenn Sie Embeddinator-4000 mit unserem Pkg-Installer installiert und gestartet wurde, eine neue Terminalsitzung nach der Installation, Sie mithilfe muss der **Objcgen** Befehl (andernfalls können Sie die absoluten Pfad verwenden: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`); **Objcgen** ist das Tool an, die wir benötigen, um eine systemeigene Bibliothek aus einer .NET Framework-Assembly zu generieren.

Geöffneten Terminaldienste `cd` in den Ordner mit Weather.dll, und führen Sie **Objcgen** mit den Argumenten, die im folgenden gezeigt:

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

Alles, was Sie in platziert benötigen der **Ausgabe** Verzeichnis neben *Weather.dll*. Wenn Sie eine eigene Assembly .NET haben, ersetzen *Weather.dll* und führen Sie die gleichen oben Schritte.

## <a name="using-the-generated-output-in-an-xcode-project"></a>Mithilfe der generierten Ausgabe in einem Xcode-Projekt

Öffnen Sie Xcode, und erstellen Sie ein neues **MacOS Kakao Anwendung** und nennen Sie sie **MyWeather**. Klicken Sie mit der rechten Maustaste auf die *MyWeather Projektknoten*Option *Hinzufügen von Dateien zur "MyWeather"*, navigieren Sie zu der **Ausgabe** vom erstellte Verzeichnis *Embeddinator 4000* , und fügen Sie die folgenden Dateien:

* bindings.h
* embeddinator.h
* glib.h
* mono-support.h
* mono_embeddinator.h
* objc-support.h
* libWeather.dylib
* Weather.dll

Stellen Sie sicher, dass **Elemente kopieren, bei Bedarf** im Optionsbereich des Dialogfeld "Datei" aktiviert ist.

Nun wir sicherstellen, dass müssen **libWeather.dylib** und **Weather.dll** in der app-Bündel abrufen:

* Klicken Sie auf *MyWeather Projektknoten*.
* Wählen Sie *Build Phases* Registerkarte.
* Fügen Sie einen neuen *Dateien Kopierphase*.
* Auf *Ziel* wählen **Frameworks** und hinzufügen **libWeather.dylib**.
* Fügen Sie einen neuen *Dateien Kopierphase*.
* Auf *Ziel* wählen **ausführbare Dateien**, hinzufügen **Weather.dll** , und stellen Sie sicher, dass *Codesignatur Kopie* aktiviert ist.

Öffnen Sie jetzt **ViewController.m** und Ersetzen Sie den Inhalt mit:

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

Führen Sie schließlich die Xcode-Projekt, und etwa Folgendes angezeigt wird:

![MyWeather Beispiel ausgeführt wird](macos-images/weather-from-csharp-macos.png)

Eine umfassendere und ansprechendere Beispiel steht [hier](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
