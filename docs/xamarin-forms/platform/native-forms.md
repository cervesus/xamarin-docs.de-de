---
title: Xamarin.Formsin nativen xamarin-Projekten
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9fb741a03d1c8dd2a8754120d0b46567d8889a0b
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84132274"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Formsin nativen xamarin-Projekten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

In der Regel Xamarin.Forms enthält eine-Anwendung eine oder mehrere Seiten, die von abgeleitet werden [`ContentPage`](xref:Xamarin.Forms.ContentPage) , und diese Seiten werden von allen Plattformen in einem .NET Standard Bibliotheksprojekt oder einem freigegebenen Projekt gemeinsam genutzt. Mithilfe von nativen Formularen können jedoch `ContentPage` von abgeleitete Seiten direkt zu nativen xamarin. IOS-, xamarin. Android-und UWP-Anwendungen hinzugefügt werden. Im Vergleich zu dem systemeigenen Projekt `ContentPage` , von dem abgeleitete Seiten aus einem .NET Standard Bibliotheksprojekt oder einem freigegebenen Projekt genutzt werden, besteht der Vorteil, dass Seiten direkt zu systemeigenen Projekten hinzugefügt werden können. Native Ansichten können dann in XAML mit `x:Name` dem Code-Behind benannt und darauf verwiesen werden. Weitere Informationen zu systemeigenen Sichten finden Sie unter [native Ansichten](~/xamarin-forms/platform/native-views/index.md).

Der Prozess für die Verwendung einer von Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteten Seite in einem nativen Projekt sieht wie folgt aus:

1. Fügen Sie dem systemeigenen Xamarin.Forms Projekt das nuget-Paket hinzu.
1. Fügen Sie die [`ContentPage`](xref:Xamarin.Forms.ContentPage) von abgeleitete Seite und alle Abhängigkeiten dem systemeigenen Projekt hinzu.
1. Rufen Sie die `Forms.Init` -Methode auf.
1. Erstellen Sie eine Instanz der von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteten Seite, und konvertieren Sie Sie mithilfe einer der folgenden Erweiterungs Methoden in den entsprechenden systemeigenen Typ: `CreateViewController` für IOS, `CreateSupportFragment` für Android oder `CreateFrameworkElement` für UWP.
1. Navigieren Sie mit der nativen Navigations-API zur systemeigenen Typdarstellung der von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteten Seite.

Xamarin.Formsmuss initialisiert werden, indem die `Forms.Init` -Methode aufgerufen wird, bevor ein natives Projekt eine von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seite erstellen kann. Die Entscheidung, wann dies zu tun ist, hängt in erster Linie davon ab, wann es am einfachsten in Ihrem Anwendungs Fluss ist – es kann beim Starten der Anwendung oder kurz vor dem Konstruieren der von `ContentPage` abgeleiteten Seite ausgeführt werden. In diesem Artikel und den begleitenden Beispielanwendungen wird die- `Forms.Init` Methode beim Anwendungsstart aufgerufen.

> [!NOTE]
> Die Beispiel Anwendungslösung **nativeforms** enthält keine Xamarin.Forms Projekte. Stattdessen besteht Sie aus einem xamarin. IOS-Projekt, einem xamarin. Android-Projekt und einem UWP-Projekt. Jedes Projekt ist ein System eigenes Projekt, das Native Formulare verwendet, um von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seiten zu verarbeiten. Es gibt jedoch keinen Grund, warum die systemeigenen Projekte keine `ContentPage` von abgeleiteten Seiten aus einem .NET Standard Bibliotheksprojekt oder einem freigegebenen Projekt verwenden können.

Bei der Verwendung von nativen Formularen Xamarin.Forms funktionieren Funktionen wie [`DependencyService`](xref:Xamarin.Forms.DependencyService) , [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) und die Daten Bindungs-Engine. Die Seitennavigation muss jedoch mithilfe der nativen Navigations-API ausgeführt werden.

## <a name="ios"></a>iOS

Unter IOS ist die `FinishedLaunching` außer Kraft Setzung in der-Klasse in der `AppDelegate` Regel der Ort zum Ausführen von Aufgaben im Zusammenhang mit Anwendungsstart. Sie wird aufgerufen, nachdem die Anwendung gestartet wurde, und wird in der Regel überschrieben, um das Hauptfenster und den Ansichts Controller zu konfigurieren. Das folgende Codebeispiel zeigt die- `AppDelegate` Klasse in der Beispielanwendung:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;
    UIWindow _window;
    AppNavigationController _navigation;

    public static string FolderPath { get; private set; }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
        UIViewController mainPage = new NotesPage().CreateViewController();
        mainPage.Title = "Notes";

        _navigation = new AppNavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    // ...
}
```

Die Methode `FinishedLaunching` führt die folgenden Aufgaben aus:

- Xamarin.Formswird durch Aufrufen der-Methode initialisiert `Forms.Init` .
- Ein Verweis auf die- `AppDelegate` Klasse wird im- `static` `Instance` Feld gespeichert. Dies dient zum Bereitstellen eines Mechanismus für andere Klassen, um Methoden aufzurufen, die in der-Klasse definiert sind `AppDelegate` .
- Der `UIWindow` , der der Haupt Container für Ansichten in systemeigenen IOS-Anwendungen ist, wird erstellt.
- Die- `FolderPath` Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage` -Klasse, bei der es sich um eine Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) in XAML definierte, von abgeleitete Seite handelt, wird `UIViewController` mithilfe der-Erweiterungsmethode erstellt und in eine konvertiert `CreateViewController` .
- Die- `Title` Eigenschaft der `UIViewController` wird festgelegt, die auf dem angezeigt wird `UINavigationBar` .
- `AppNavigationController`Zum Verwalten der hierarchischen Navigation wird ein erstellt. Dabei handelt es sich um eine benutzerdefinierte Navigations Controller Klasse, die von abgeleitet wird `UINavigationController` . Das `AppNavigationController` -Objekt verwaltet einen Stapel von Ansichts Controllern, und der an `UIViewController` den Konstruktor eingegebene wird anfänglich angezeigt, wenn der `AppNavigationController` geladen wird.
- Das `AppNavigationController` -Objekt wird als die oberste Ebene `UIViewController` für den festgelegt `UIWindow` , und die `UIWindow` wird als Schlüssel Fenster für die Anwendung festgelegt und sichtbar gemacht.

Nachdem die `FinishedLaunching` Methode ausgeführt wurde, wird die in der Klasse definierte Benutzeroberfläche Xamarin.Forms `NotesPage` angezeigt, wie im folgenden Screenshot zu sehen:

[![Screenshot einer xamarin. IOS-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/ios-notespage.png "Xamarin. IOS-App mit XAML-Benutzeroberfläche")](native-forms-images/ios-notespage-large.png#lightbox "Xamarin. IOS-App mit XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. durch Tippen auf den **+** [`Button`](xref:Xamarin.Forms.Button) , führt dazu, dass der folgende Ereignishandler im `NotesPage` Code-Behind-Code ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

Das- `static` `AppDelegate.Instance` Feld ermöglicht das Aufrufen der- `AppDelegate.NavigateToNoteEntryPage` Methode, was im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    var noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

Die `NavigateToNoteEntryPage` -Methode konvertiert die Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) von abgeleitete Seite `UIViewController` mit der- `CreateViewController` Erweiterungsmethode in ein und legt die- `Title` Eigenschaft von fest `UIViewController` . Der `UIViewController` wird dann von der-Methode auf den abgelegt `AppNavigationController` `PushViewController` . Daher wird die in der-Klasse definierte Benutzeroberfläche Xamarin.Forms `NoteEntryPage` angezeigt, wie im folgenden Screenshot zu sehen:

[![Screenshot einer xamarin. IOS-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/ios-noteentrypage.png "Xamarin. IOS-App mit XAML-Benutzeroberfläche")](native-forms-images/ios-noteentrypage-large.png#lightbox "Xamarin. IOS-App mit XAML-Benutzeroberfläche")

Wenn das `NoteEntryPage` angezeigt wird, wird die rückwärts Navigation `UIViewController` für die- `NoteEntryPage` Klasse aus dem Pop `AppNavigationController` , wobei der Benutzer an den `UIViewController` für die-Klasse zurück `NotesPage` gegeben wird. Das Löschen eines `UIViewController` aus dem nativen IOS-Navigations Stapel entfernt jedoch nicht automatisch das `UIViewController` angefügte-Objekt und das angefügte- `Page` Objekt. Daher `AppNavigationController` überschreibt die-Klasse die- `PopViewController` Methode, um die Ansichts Controller bei der rückwärts Navigation zu verwerfen:

```csharp
public class AppNavigationController : UINavigationController
{
    //...
    public override UIViewController PopViewController(bool animated)
    {
        UIViewController topView = TopViewController;
        if (topView != null)
        {
            // Dispose of ViewController on back navigation.
            topView.Dispose();
        }
        return base.PopViewController(animated);
    }
}
```

Mit der-Überschreibung wird `PopViewController` die- `Dispose` Methode für das `UIViewController` Objekt aufgerufen, das aus dem nativen IOS-Navigations Stapel entfernt wurde. Wenn dies nicht der Fall ist, führt dies dazu `UIViewController` , dass das angefügte `Page` Objekt verwaist ist.

> [!IMPORTANT]
> Verwaiste Objekte können nicht in die Garbage Collection aufgenommen werden, und dies führt zu einem Speicher Abfall.

## <a name="android"></a>Android

Unter Android ist die `OnCreate` außer Kraft Setzung in der-Klasse in der `MainActivity` Regel der Ort zum Ausführen von Aufgaben im Zusammenhang mit Anwendungsstart. Das folgende Codebeispiel zeigt die- `MainActivity` Klasse in der Beispielanwendung:

```csharp
public class MainActivity : AppCompatActivity
{
    public static string FolderPath { get; private set; }

    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Notes";

        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        Android.Support.V4.App.Fragment mainPage = new NotesPage().CreateSupportFragment(this);
        SupportFragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

Die Methode `OnCreate` führt die folgenden Aufgaben aus:

- Xamarin.Formswird durch Aufrufen der-Methode initialisiert `Forms.Init` .
- Ein Verweis auf die- `MainActivity` Klasse wird im- `static` `Instance` Feld gespeichert. Dies dient zum Bereitstellen eines Mechanismus für andere Klassen, um Methoden aufzurufen, die in der-Klasse definiert sind `MainActivity` .
- Der `Activity` Inhalt wird aus einer layoutressource festgelegt. In der Beispielanwendung besteht das Layout aus einer, die `LinearLayout` eine enthält `Toolbar` , und einer, die `FrameLayout` als fragmentcontainer fungieren soll.
- Der `Toolbar` wird abgerufen und als Aktionsleiste für die festgelegt `Activity` , und der Titel der Aktionsleiste wird festgelegt.
- Die- `FolderPath` Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage` -Klasse, bei der es sich um eine Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) in XAML definierte, von abgeleitete Seite handelt, wird `Fragment` mithilfe der-Erweiterungsmethode erstellt und in eine konvertiert `CreateSupportFragment` .
- Die `SupportFragmentManager` -Klasse erstellt und führt einen Commit für eine Transaktion aus, die die `FrameLayout` Instanz durch den `Fragment` für die- `NotesPage` Klasse ersetzt.

Weitere Informationen zu Fragmenten finden Sie unter [Fragmente](~/android/platform/fragments/index.md).

Nachdem die `OnCreate` Methode ausgeführt wurde, wird die in der Klasse definierte Benutzeroberfläche Xamarin.Forms `NotesPage` angezeigt, wie im folgenden Screenshot zu sehen:

[![Screenshot einer xamarin. Android-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/android-notespage.png "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")](native-forms-images/android-notespage-large.png#lightbox "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. durch Tippen auf den **+** [`Button`](xref:Xamarin.Forms.Button) , führt dazu, dass der folgende Ereignishandler im `NotesPage` Code-Behind-Code ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

Das- `static` `MainActivity.Instance` Feld ermöglicht das Aufrufen der- `MainActivity.NavigateToNoteEntryPage` Methode, was im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    Android.Support.V4.App.Fragment noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateSupportFragment(this);
    SupportFragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, noteEntryPage)
        .Commit();
}
```

Die `NavigateToNoteEntryPage` -Methode konvertiert die von Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seite `Fragment` mit der- `CreateSupportFragment` Erweiterungsmethode in einen und fügt den dem `Fragment` fragmentbackstapel hinzu. Daher wird die in der definierte Benutzeroberfläche Xamarin.Forms `NoteEntryPage` angezeigt, wie im folgenden Screenshot zu sehen:

[![Screenshot einer xamarin. Android-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/android-noteentrypage.png "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")](native-forms-images/android-noteentrypage-large.png#lightbox "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")

Wenn das-Symbol `NoteEntryPage` angezeigt wird, wird beim Tippen auf den rückwärts Pfeil der `Fragment` für die `NoteEntryPage` aus dem fragmentbackstapel angezeigt, und der Benutzer wird für die-Klasse zurückgegeben `Fragment` `NotesPage` .

### <a name="enable-back-navigation-support"></a>Unterstützung für die Back Navigation aktivieren

Die- `SupportFragmentManager` Klasse hat ein- `BackStackChanged` Ereignis, das ausgelöst wird, wenn der Inhalt des fragmentbackstapels geändert wird. Die- `OnCreate` Methode in der- `MainActivity` Klasse enthält einen anonymen Ereignishandler für dieses Ereignis:

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

Dieser Ereignishandler zeigt eine Schaltfläche zurück auf der Aktionsleiste an, vorausgesetzt, dass es eine oder mehrere `Fragment` Instanzen auf dem fragmentbackstapel gibt. Die Antwort für das Tippen auf die Schaltfläche "zurück" wird durch die Überschreibung behandelt `OnOptionsItemSelected` :

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && SupportFragmentManager.BackStackEntryCount > 0)
    {
        SupportFragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

Die `OnOptionsItemSelected` außer Kraft setzung wird immer dann aufgerufen, wenn ein Element im Menü Optionen ausgewählt wird. Diese Implementierung löst das aktuelle Fragment aus dem fragmentbackstapel auf, vorausgesetzt, dass die Schaltfläche "zurück" ausgewählt wurde und eine oder mehrere `Fragment` Instanzen auf dem fragmentbackstapel vorhanden sind.

### <a name="multiple-activities"></a>Mehrere Aktivitäten

Wenn eine Anwendung aus mehreren Aktivitäten besteht, [`ContentPage`](xref:Xamarin.Forms.ContentPage) können von abgeleitete Seiten in jede der Aktivitäten eingebettet werden. In diesem Szenario muss die- `Forms.Init` Methode nur in der Überschreibung des ersten aufgerufen werden, der `OnCreate` `Activity` einen bettet Xamarin.Forms `ContentPage` . Dies hat jedoch folgende Auswirkungen:

- Der Wert von `Xamarin.Forms.Color.Accent` wird von der- `Activity` Methode übernommen, die die-Methode aufgerufen hat `Forms.Init` .
- Der Wert von `Xamarin.Forms.Application.Current` wird dem zugeordnet, der `Activity` die-Methode aufgerufen hat `Forms.Init` .

### <a name="choose-a-file"></a>Datei auswählen

Beim [`ContentPage`](xref:Xamarin.Forms.ContentPage) Einbetten einer von abgeleiteten Seite, die einen verwendet, [`WebView`](xref:Xamarin.Forms.WebView) der eine HTML-Schaltfläche "Datei auswählen" unterstützen muss, muss die- `Activity` Methode überschreiben `OnActivityResult` :

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Bei UWP ist die native `App` Klasse in der Regel der Ort zum Ausführen von Aufgaben im Zusammenhang mit dem Anwendungsstart. Xamarin.Formswird in der Regel in Xamarin.Forms UWP-Anwendungen in der- `OnLaunched` Überschreibung in der systemeigenen Klasse initialisiert `App` , um das- `LaunchActivatedEventArgs` Argument an die-Methode zu übergeben `Forms.Init` . Aus diesem Grund können native UWP-Anwendungen, die eine von Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seite verwenden, die-Methode am einfachsten `Forms.Init` von der- `App.OnLaunched` Methode abrufen.

Standardmäßig wird die Klasse von der systemeigenen `App` Klasse `MainPage` als erste Seite der Anwendung gestartet. Das folgende Codebeispiel zeigt die- `MainPage` Klasse in der Beispielanwendung:

```csharp
public sealed partial class MainPage : Page
{
    NotesPage notesPage;
    NoteEntryPage noteEntryPage;
    
    public static MainPage Instance;
    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        notesPage = new Notes.UWP.Views.NotesPage();
        this.Content = notesPage.CreateFrameworkElement();
        // ...        
    } 
    // ...
}
```

Der- `MainPage` Konstruktor führt die folgenden Aufgaben aus:

- Das Caching ist für die Seite aktiviert, sodass ein neues `MainPage` nicht erstellt wird, wenn ein Benutzer zurück zur Seite wechselt.
- Ein Verweis auf die- `MainPage` Klasse wird im- `static` `Instance` Feld gespeichert. Dies dient zum Bereitstellen eines Mechanismus für andere Klassen, um Methoden aufzurufen, die in der-Klasse definiert sind `MainPage` .
- Die- `FolderPath` Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage` -Klasse, bei der es sich um eine Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) in XAML definierte, von abgeleitete Seite handelt, wird mithilfe der-Erweiterungsmethode erstellt und in eine konvertiert `FrameworkElement` `CreateFrameworkElement` und dann als Inhalt der-Klasse festgelegt `MainPage` .

Nachdem der `MainPage` Konstruktor ausgeführt wurde, wird die Benutzeroberfläche, die in der Klasse definiert ist Xamarin.Forms `NotesPage` , wie im folgenden Screenshot gezeigt angezeigt:

[![Screenshot einer UWP-Anwendung, die eine mit XAML definierte Benutzeroberfläche verwendet Xamarin.Forms](native-forms-images/uwp-notespage.png "UWP-App mit a [! Schel. No-Loc (xamarin. Forms)] XAML-Benutzeroberfläche")](native-forms-images/uwp-notespage-large.png#lightbox "UWP-App mit a [! Schel. No-Loc (xamarin. Forms)] XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. durch Tippen auf den **+** [`Button`](xref:Xamarin.Forms.Button) , führt dazu, dass der folgende Ereignishandler im `NotesPage` Code-Behind-Code ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

Das- `static` `MainPage.Instance` Feld ermöglicht das Aufrufen der- `MainPage.NavigateToNoteEntryPage` Methode, was im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    noteEntryPage = new Notes.UWP.Views.NoteEntryPage
    {
        BindingContext = note
    };
    this.Frame.Navigate(noteEntryPage);
}
```

Die Navigation in der UWP erfolgt in der Regel mit der- `Frame.Navigate` Methode, die ein- `Page` Argument annimmt. Xamarin.Formsdefiniert eine `Frame.Navigate` Erweiterungsmethode, die eine von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seiten Instanz annimmt. Wenn die- `NavigateToNoteEntryPage` Methode ausgeführt wird, wird daher die in der definierte Benutzeroberfläche Xamarin.Forms `NoteEntryPage` angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer UWP-Anwendung, die eine mit XAML definierte Benutzeroberfläche verwendet Xamarin.Forms](native-forms-images/uwp-noteentrypage.png "UWP-App mit a [! Schel. No-Loc (xamarin. Forms)] XAML-Benutzeroberfläche")](native-forms-images/uwp-noteentrypage-large.png#lightbox "UWP-App mit a [! Schel. No-Loc (xamarin. Forms)] XAML-Benutzeroberfläche")

Wenn der `NoteEntryPage` angezeigt wird, wird beim Tippen auf den rückwärts Pfeil der `FrameworkElement` für die `NoteEntryPage` aus dem in-App-Back-Stapel angezeigt, und der Benutzer wird für die-Klasse zurückgegeben `FrameworkElement` `NotesPage` .

### <a name="enable-page-resizing-support"></a>Aktivieren der Unterstützung für die Seiten Änderung

Wenn die Größe des UWP-Anwendungsfensters geändert wird, Xamarin.Forms sollte auch die Größe des Inhalts angepasst werden. Dies erfolgt durch das Registrieren eines Ereignis Handlers für das- `Loaded` Ereignis im- `MainPage` Konstruktor:

```csharp
public MainPage()
{
    // ...
    this.Loaded += OnMainPageLoaded;
    // ...
}
```

Das `Loaded` Ereignis wird ausgelöst, wenn die Seite angelegt, gerendert und bereit für die Interaktion ist, und führt die- `OnMainPageLoaded` Methode als Antwort aus:

```csharp
void OnMainPageLoaded(object sender, RoutedEventArgs e)
{
    this.Frame.SizeChanged += (o, args) =>
    {
        if (noteEntryPage != null)
            noteEntryPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
        else
            notesPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
    };
}
```

Die- `OnMainPageLoaded` Methode registriert einen anonymen Ereignishandler für das- `Frame.SizeChanged` Ereignis, das ausgelöst wird, wenn sich die-Eigenschaft `ActualHeight` oder die-Eigenschaft `ActualWidth` in der ändert `Frame` . Als Antwort wird der Xamarin.Forms Inhalt für die aktive Seite durch Aufrufen der-Methode angepasst `Layout` .

### <a name="enable-back-navigation-support"></a>Unterstützung für die Back Navigation aktivieren

Bei UWP müssen Anwendungen die Navigation für alle Hardware-und Software-Back-Schaltflächen auf unterschiedlichen Geräte Formfaktoren aktivieren. Dies kann erreicht werden, indem ein Ereignishandler für das-Ereignis registriert wird `BackRequested` , das im Konstruktor ausgeführt werden kann `MainPage` :

```csharp
public MainPage()
{
    // ...
    SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
}
```

Wenn die Anwendung gestartet wird, ruft die- `GetForCurrentView` Methode das- `SystemNavigationManager` Objekt ab, das der aktuellen Ansicht zugeordnet ist, und registriert dann einen Ereignishandler für das- `BackRequested` Ereignis. Die Anwendung empfängt dieses Ereignis nur, wenn es sich um die Vordergrund Anwendung handelt, und ruft als Antwort den `OnBackRequested` Ereignishandler auf:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
        noteEntryPage = null;
    }
}
```

Der `OnBackRequested` -Ereignishandler ruft die `GoBack` -Methode für den Stamm Rahmen der Anwendung auf und legt die-Eigenschaft auf fest `BackRequestedEventArgs.Handled` `true` , um das Ereignis als behandelt zu markieren. Wenn das Ereignis nicht als behandelt markiert wird, kann es dazu führen, dass das Ereignis ignoriert wird.

Die Anwendung entscheidet, ob auf der Titelleiste eine Schaltfläche zurück angezeigt werden soll. Dies wird erreicht, indem die- `AppViewBackButtonVisibility` Eigenschaft auf einen der- `AppViewBackButtonVisibility` Enumerationswerte in der-Klasse festgelegt wird `App` :

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

Der `OnNavigated` Ereignishandler, der als Reaktion auf das Auslösen des `Navigated` Ereignisses ausgeführt wird, aktualisiert die Sichtbarkeit der Titelleiste zurück, wenn die Seitennavigation erfolgt. Dadurch wird sichergestellt, dass die Schaltfläche "zurück" der Titelleiste angezeigt wird, wenn der in-App-BackStack nicht leer ist oder aus der Titelleiste entfernt wird, wenn der in-App-BackStack leer ist.

Weitere Informationen zur Unterstützung der Back Navigation bei UWP finden Sie unter [Navigationsverlauf und rückwärts Navigation für UWP-apps](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="related-links"></a>Verwandte Links

- [Nativeforms (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [Native Ansichten](~/xamarin-forms/platform/native-views/index.md)
