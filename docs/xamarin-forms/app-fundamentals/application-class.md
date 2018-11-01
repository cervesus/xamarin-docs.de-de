---
title: Xamarin.Forms-App-Klasse
description: Dieser Artikel beschreibt die Funktionen der Standard-App-Klasse, die enthält eine Eigenschaft, auf die Startseite für die app festzulegen, und ein persistenten Wörterbuch zum Speichern von einfache Werte in Änderungen am Ansichtszustand des Lebenszyklus.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 9acd1b8f25696267578f5cc269eb1b0c738be571
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675093"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms-App-Klasse

Die `Application` Basisklasse bietet die folgenden Funktionen, die in Ihrem Standard-Projekte verfügbar gemacht werden `App` Unterklasse:

* Ein `MainPage` Eigenschaft, die an die Startseite für die app festgelegt ist.
* Ein beständiges [ `Properties` Wörterbuch](#Properties_Dictionary) , einfache Werte über Änderungen im abonnementlebenszyklus Zustand zu speichern.
* Eine statische `Current` -Eigenschaft, die einen Verweis auf das aktuelle Anwendungsobjekt enthält.

Macht auch [Lebenszyklusmethoden](~/xamarin-forms/app-fundamentals/app-lifecycle.md) wie z. B. `OnStart`, `OnSleep`, und `OnResume` sowie modalen Navigation-Ereignisse.

Je nachdem, welche Vorlage Sie ausgewählt haben, die `App` Klasse kann auf zwei Arten definiert werden:

* **C#-**, oder
* **XAML UND C#**

Zum Erstellen einer **App** -Klasse unter Verwendung von XAML, der Standardwert **App** Klasse ersetzt werden muss, mit einer XAML **App** -Klasse und zugeordnete Code-Behind, wie im folgenden Codebeispiel gezeigt:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Im folgenden Codebeispiel wird veranschaulicht, der zugehörigen Code-Behind:

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

Sowie die Einstellung der [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) -Eigenschaft, Code-Behind muss auch aufrufen, die `InitializeComponent` Methode zum Laden und analysieren die zugehörigen XAML.

## <a name="mainpage-property"></a>Eigenschaft "MainPage"

Die `MainPage` Eigenschaft für die `Application` Klasse legt die Stammseite der Anwendung fest.

Sie können z. B. erstellen, Logik in Ihrer `App` zum Anzeigen einer anderen Seite abhängig davon, ob der Benutzer oder nicht angemeldet ist.

Die `MainPage` Eigenschaft sollte festgelegt werden, der `App` -Konstruktor

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Wörterbuch "Properties"

Die `Application` Unterklasse weist eine statische `Properties` Wörterbuch zum Speichern von Daten, insbesondere verwendet werden kann, für die Verwendung in der `OnStart`, `OnSleep`, und `OnResume` Methoden. Dies kann an einer beliebigen Stelle in Ihrer Xamarin.Forms-Code mithilfe von zugegriffen werden `Application.Current.Properties`.

Die `Properties` Wörterbuch verwendet eine `string` Schlüssel und speichert eine `object` Wert.

Beispielsweise können Sie einen permanenten festlegen `"id"` Eigenschaft an eine beliebige Stelle im Code (wenn ein Element ausgewählt ist, in einer Seite `OnDisappearing` -Methode, oder in der `OnSleep` Methode) wie folgt aus:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

In der `OnStart` oder `OnResume` Methoden, Sie können diesen Wert dann verwenden, um die benutzererfahrung auf irgendeine Weise erneut zu erstellen. Die `Properties` Wörterbuch speichern `object`s, daher Sie den Wert umgewandelt, bevor Sie ihn verwenden müssen.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Überprüfen Sie immer auf das Vorhandensein des Schlüssels, bevor Sie den Zugriff auf, um zu verhindern, dass unerwartete Fehler.

> [!NOTE]
> Die `Properties` Wörterbuch kann nur primitive Typen für die Speicherung serialisiert. Es wird versucht, andere Typen speichern (z. B. `List<string>`) kann fehlschlagen, im Hintergrund.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Persistenz

Die `Properties` Wörterbuch wird automatisch auf dem Gerät gespeichert.
Daten, die dem Wörterbuch hinzugefügt werden verfügbar, oder die Anwendung im Hintergrund gibt auch nach einem Neustart.

Xamarin.Forms 1.4 eine weitere Methode eingeführt, auf die `Application` -Klasse – `SavePropertiesAsync()` -proaktiv beibehalten aufgerufen werden kann die `Properties` Wörterbuch. Dies ist es Ihnen ermöglichen, Eigenschaften nach wichtigen Updates zu speichern, anstatt Sie zu riskieren sie nicht abrufen serialisierter sich aufgrund eines Absturzes oder vom Betriebssystem beendet wird.

Finden Sie Verweise auf die Verwendung der `Properties` Wörterbuch in der **Erstellen mobiler Apps mit Xamarin.Forms** book-Kapitel [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), und [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf), und der zugeordneten [Beispiele](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Die Application-Klasse

Eine vollständige `Application` Implementierung wird unten als Referenz angezeigt:

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

Diese Klasse ist dann in den einzelnen plattformspezifischen Projekten instanziiert und übergeben die `LoadApplication` Methode, wofür die `MainPage` geladen und dem Benutzer angezeigt.
Der Code für jede Plattform wird in den folgenden Abschnitten dargestellt. Die neuesten Vorlagen der Xamarin.Forms-Projektmappe enthalten noch der gesamte Code, vorkonfiguriert für Ihre app.


### <a name="ios-project"></a>iOS-Projekts

IOS `AppDelegate` Klasse erbt von `FormsApplicationDelegate`. Es sollten:

* Rufen Sie `LoadApplication` mit einer Instanz von der `App` Klasse.

* Jederzeit `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Android-Projekt

Die Android `MainActivity` erbt `FormsAppCompatActivity`. In der `OnCreate` außer Kraft setzen der `LoadApplication` Methode wird aufgerufen, mit einer Instanz von der `App` Klasse.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Universelle Windows-Projekt (UWP) für Windows 10

Finden Sie unter [Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md) für Informationen zur UWP-Unterstützung in Xamarin.Forms.

Die Hauptseite im UWP-Projekt sollte erben `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Die C# CodeBehind-Konstruktion muss Aufrufen `LoadApplication` zum Erstellen einer Instanz von Ihrer Xamarin.Forms `App`. Beachten Sie, dass es empfiehlt sich, den anwendungsnamespace explizit zu verwenden, um zu kennzeichnen die `App` da UWP-Anwendungen auch ihre eigenen haben `App` Klasse, die nicht mit Xamarin.Forms.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Beachten Sie, dass `Forms.Init()` muss aufgerufen werden, **"App.Xaml.cs"** der Nähe von Zeile 63.
