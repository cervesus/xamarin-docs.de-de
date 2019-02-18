---
title: Xamarin.Forms-App-Klasse
description: Dieser Artikel erklärt Features der Standard-App-Klasse, darunter eine Eigenschaft zum Festlegen der Startseite der App und ein beständiges Wörterbuch zum Speichern einfacher Werte über Zustandsänderungen im Lebenszyklus hinweg.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: dbee8d3f55edee9c862b4c883b63b55f98972b48
ms.sourcegitcommit: 0c2745c1593eee3ecb40ab882e854a13ca72bc86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56078419"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms-App-Klasse

Die Basisklasse `Application` bietet folgende Features, die in der Standard-`App`-Unterklasse Ihres Projekts verfügbar gemacht werden:

* Eine `MainPage`-Eigenschaft, bei der die Startseite für die App festgelegt wird.
* Ein beständiges [`Properties`-Wörterbuch](#Properties_Dictionary), in dem einfache Werte über Zustandsänderungen im Lebenszyklus hinweg gespeichert werden.
* Eine statische `Current`-Eigenschaft, die einen Verweis auf das aktuelle Anwendungsobjekt enthält.

Es werden auch [Lifecycle methods (Lebenszyklusmethoden)](~/xamarin-forms/app-fundamentals/app-lifecycle.md) wie z.B. `OnStart`, `OnSleep` und `OnResume` sowie modale Navigationsereignisse verfügbar gemacht.

Je nachdem, welche Vorlage Sie ausgewählt haben, kann die `App`-Klasse auf eine von zwei Arten definiert werden:

* **C#** oder
* **XAML & C#**

Zum Erstellen einer **App**-Klasse mit XAML muss die Standard-**App**-Klasse durch eine XAML-**App**-Klasse und die dazugehörige CodeBehind-Datei ersetzt werden, wie im folgenden Codebeispiel gezeigt:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Das folgende Codebeispiel zeigt das zugehörige CodeBehind:

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

Das CodeBehind muss sowohl die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft festlegen als auch die `InitializeComponent`-Methode aufrufen, um die zugehörige XAML zu laden und zu analysieren.

## <a name="mainpage-property"></a>MainPage-Eigenschaft

Die `MainPage`-Eigenschaft der `Application`-Klasse legt die Stammseite der Anwendung fest.

Sie können zum Beispiel Logik in Ihrer `App`-Klasse erstellen, um unterschiedliche Seiten in Abhängigkeit davon anzuzeigen, ob der Benutzer angemeldet ist oder nicht.

Die `MainPage`-Eigenschaft sollte im `App`-Konstruktor festgelegt werden:

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

## <a name="properties-dictionary"></a>Eigenschaftenwörterbuch

Die `Application`-Unterklasse verfügt über ein statisches `Properties`-Wörterbuch, das zum Speichern von Daten verwendet werden kann, vor allem zur Verwendung in den Methoden `OnStart`, `OnSleep` und `OnResume`. Darauf kann von überall im Xamarin.Forms-Code mit `Application.Current.Properties` zugegriffen werden.

Das `Properties`-Wörterbuch verwendet einen `string`-Schlüssel und speichert einen `object`-Wert.

Sie könnten zum Beispiel eine beständige `"id"`-Eigenschaft an einer beliebigen Stelle im Code wie folgt festlegen (wenn ein Element ausgewählt wird in der Methode `OnDisappearing` einer Seite oder in der Methode `OnSleep`):

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

In den Methoden `OnStart` oder `OnResume` können Sie dann diesen Wert verwenden, um die Benutzeroberfläche auf gewisse Weise erneut zu erstellen. Das `Properties`-Wörterbuch speichert `object`e, sodass Sie dessen Wert vor der Verwendung umwandeln müssen.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Überprüfen Sie immer, ob der Schlüssel vorhanden ist, bevor Sie darauf zugreifen, um unerwartete Fehler zu vermeiden.

> [!NOTE]
> Das `Properties`-Wörterbuch kann nur primitive Typen für die Speicherung serialisieren. Der Versuch, andere Typen (wie z.B. `List<string>`) zu speichern, kann im Hintergrund fehlschlagen.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Persistenz

Das `Properties`-Wörterbuch wird automatisch auf dem Gerät gespeichert.
Daten, die dem Wörterbuch hinzugefügt werden, werden verfügbar, wenn die Anwendung aus dem Hintergrund zurückkehrt oder wenn sie neu gestartet wird.

Mit Xamarin.Forms 1.4 wurde die zusätzliche Methode `SavePropertiesAsync()` in der `Application`-Klasse eingeführt, die aufgerufen werden kann, um das `Properties`-Wörterbuch proaktiv beizubehalten. Dadurch können Sie Eigenschaften nach wichtigen Updates speichern anstatt zu riskieren, dass diese durch einen Absturz oder eine Löschung durch das Betriebssystem nicht serialisiert werden.

Verweise zur Verwendung des `Properties`-Wörterbuchs finden Sie in den Kapiteln [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf) und [20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) des Buchs **Creating Mobile Apps with Xamarin.Forms (Erstellen mobiler Apps mit Xamarin.Forms)** und in den zugehörigen [Beispielen](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Die Application-Klasse

Eine vollständige Implementierung der `Application`-Klasse wird unten zur Referenz gezeigt:

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

Diese Klasse wird dann in jedem plattformspezifischen Projekt instanziiert und an die `LoadApplication`-Methode übergeben, wo die `MainPage` geladen und dem Benutzer angezeigt wird.
Der Code für jede Plattform wird in den folgenden Abschnitten gezeigt. Die neuesten Vorlagen der Xamarin.Forms-Projektmappe enthalten bereits diesen gesamten Code, der für Ihre App vorkonfiguriert wurde.

### <a name="ios-project"></a>iOS-Projekt

Die iOS-`AppDelegate`-Klasse erbt von `FormsApplicationDelegate`. Sie sollte

* `LoadApplication` mit einer Instanz der `App`-Klasse aufrufen

* immer `base.FinishedLaunching (app, options);` zurückgeben

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

Die Android-`MainActivity`-Klasse erbt von `FormsAppCompatActivity`. In der `OnCreate`-Überschreibung wird die `LoadApplication`-Methode mit einer Instanz der `App`-Klasse aufgerufen.

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

### <a name="universal-windows-project-uwp-for-windows-10"></a>UWP-Projekt (Universelle Windows-Plattform) für Windows 10

Die Hauptseite im UWP-Projekt sollte von `WindowsPage` erben:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Die C#-CodeBehind-Konstruktion muss `LoadApplication` aufrufen, um eine Instanz der Xamarin.Forms-`App`-Klasse zu erstellen. Beachten Sie, dass es sich empfiehlt, explizit den Anwendungsnamespace zu verwenden, um die `App`-Klasse zu kennzeichnen, da UWP-Anwendungen auch über eine eigene, von Xamarin.Forms unabhängige `App`-Klasse verfügen.

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

Beachten Sie, dass `Forms.Init()` von **App.xaml.cs** im UWP-Projekt abgerufen werden muss.

Weitere Informationen finden Sie unter [Setup Windows Projects (Einrichten von Windows-Projekten)](~/xamarin-forms/platform/windows/installation/index.md). In diesem Artikel werden Schritte zum Hinzufügen eines UWP-Projekts zu einer vorhandenen Xamarin.Forms-Lösung erläutert, die nicht auf UWP abzielt.
