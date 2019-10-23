---
title: Xamarin. Forms in nativen xamarin-Projekten
description: In diesem Artikel wird erläutert, wie von ContentPage abgeleitete Seiten verwendet werden, die direkt xamarin-Projekten hinzugefügt werden, und wie zwischen ihnen navigiert wird.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/19/2019
ms.openlocfilehash: 0c84b844455b8a792b8cbe2f4dac97097e5ebd97
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "69621056"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin. Forms in nativen xamarin-Projekten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

In der Regel enthält eine xamarin. Forms-Anwendung eine oder mehrere Seiten, die von [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitet werden, und diese Seiten werden von allen Plattformen in einem .NET Standard Bibliotheksprojekt oder einem freigegebenen Projekt gemeinsam genutzt. Mithilfe von systemeigenen Formularen können jedoch `ContentPage`-abgeleiteten Seiten direkt den nativen xamarin. IOS-, xamarin. Android-und UWP-Anwendungen hinzugefügt werden. Im Vergleich zu dem systemeigenen Projekt, das `ContentPage` von abgeleiteten Seiten aus einem .NET Standard Bibliotheksprojekt oder einem freigegebenen Projekt verwendet, besteht der Vorteil, dass Seiten direkt zu systemeigenen Projekten hinzugefügt werden können. Native Ansichten können dann in XAML mit `x:Name` benannt werden, und aus dem Code-Behind wird darauf verwiesen. Weitere Informationen zu systemeigenen Sichten finden Sie unter [native Ansichten](~/xamarin-forms/platform/native-views/index.md).

Der Prozess für die Verwendung einer xamarin. Forms- [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleiteten Seite in einem systemeigenen Projekt sieht wie folgt aus:

1. Fügen Sie das xamarin. Forms-nuget-Paket zum systemeigenen Projekt hinzu.
1. Fügen Sie dem systemeigenen Projekt die [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seite und alle Abhängigkeiten hinzu.
1. Rufen Sie die `Forms.Init`-Methode auf.
1. Erstellen Sie eine Instanz der [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleiteten Seite, und konvertieren Sie Sie mithilfe einer der folgenden Erweiterungs Methoden in den entsprechenden systemeigenen Typ: `CreateViewController` für IOS, `CreateSupportFragment` für Android oder `CreateFrameworkElement` für UWP.
1. Navigieren Sie mit der nativen Navigations-API zur systemeigenen Typdarstellung der [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleiteten Seite.

Xamarin. Forms muss initialisiert werden, indem die `Forms.Init`-Methode aufgerufen wird, bevor ein natives Projekt eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seite erstellen kann. Die Entscheidung, wann dies zu tun ist, hängt in erster Linie davon ab, wann es am einfachsten in Ihrem Anwendungs Fluss ist – es kann beim Starten der Anwendung oder kurz vor dem Konstruieren der `ContentPage` abgeleiteten Seite ausgeführt werden. In diesem Artikel und den begleitenden Beispielanwendungen wird die `Forms.Init`-Methode beim Anwendungsstart aufgerufen.

> [!NOTE]
> Die Beispiel Anwendungslösung **nativeforms** enthält keine xamarin. Forms-Projekte. Stattdessen besteht Sie aus einem xamarin. IOS-Projekt, einem xamarin. Android-Projekt und einem UWP-Projekt. Jedes Projekt ist ein System eigenes Projekt, das Native Formulare verwendet, um [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seiten zu verarbeiten. Es gibt jedoch keinen Grund, warum die systemeigenen Projekte keine `ContentPage` abgeleiteten Seiten aus einem .NET Standard Bibliotheksprojekt oder einem freigegebenen Projekt verwenden können.

Bei der Verwendung von nativen Formularen funktionieren xamarin. Forms-Funktionen wie [`DependencyService`](xref:Xamarin.Forms.DependencyService), [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)und die Daten Bindungs-Engine. Die Seitennavigation muss jedoch mithilfe der nativen Navigations-API ausgeführt werden.

## <a name="ios"></a>iOS

Unter IOS ist die `FinishedLaunching` Überschreibung in der `AppDelegate`-Klasse in der Regel der Ort zum Ausführen von Aufgaben im Zusammenhang mit dem Anwendungsstart. Sie wird aufgerufen, nachdem die Anwendung gestartet wurde, und wird in der Regel überschrieben, um das Hauptfenster und den Ansichts Controller zu konfigurieren. Das folgende Codebeispiel zeigt die `AppDelegate`-Klasse in der Beispielanwendung:

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

Die `FinishedLaunching`-Methode führt die folgenden Aufgaben aus:

- Xamarin. Forms wird initialisiert, indem die `Forms.Init`-Methode aufgerufen wird.
- Ein Verweis auf die `AppDelegate`-Klasse wird im Feld `static` `Instance` gespeichert. Dies dient zum Bereitstellen eines Mechanismus für andere Klassen zum aufzurufen von Methoden, die in der `AppDelegate` Klasse definiert sind.
- Der `UIWindow`, der der Haupt Container für Ansichten in systemeigenen IOS-Anwendungen ist, wird erstellt.
- Die `FolderPath`-Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage`-Klasse, bei der es sich um eine xamarin. Forms- [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seite handelt, die in XAML definiert ist, wird mithilfe der `CreateViewController`-Erweiterungsmethode erstellt und in eine `UIViewController` konvertiert.
- Die `Title`-Eigenschaft der `UIViewController` ist festgelegt, die auf dem `UINavigationBar` angezeigt wird.
- Zum Verwalten der hierarchischen Navigation wird ein `AppNavigationController` erstellt. Dabei handelt es sich um eine benutzerdefinierte Navigations Controller Klasse, die von `UINavigationController` abgeleitet ist. Das `AppNavigationController`-Objekt verwaltet einen Stapel von Ansichts Controllern, und die `UIViewController`, die an den Konstruktor übergeben werden, werden anfänglich angezeigt, wenn die `AppNavigationController` geladen wird.
- Das `AppNavigationController` Objekt wird als `UIViewController` der obersten Ebene für die `UIWindow` festgelegt, und der `UIWindow` wird als Schlüssel Fenster für die Anwendung festgelegt und sichtbar gemacht.

Nachdem die `FinishedLaunching`-Methode ausgeführt wurde, wird die in der xamarin. Forms-`NotesPage` Klasse definierte Benutzeroberfläche angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer xamarin. IOS-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/ios-notespage.png "Xamarin. IOS-App mit XAML-Benutzeroberfläche")](native-forms-images/ios-notespage-large.png#lightbox "Xamarin. IOS-App mit XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. durch Tippen auf die **+** [`Button`](xref:Xamarin.Forms.Button), führt zum folgenden Ereignishandler im `NotesPage` Code Behind, der ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

Im Feld `static` `AppDelegate.Instance` kann die `AppDelegate.NavigateToNoteEntryPage`-Methode aufgerufen werden. Dies wird im folgenden Codebeispiel gezeigt:

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

Die `NavigateToNoteEntryPage`-Methode konvertiert die [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete xamarin. Forms-Seite in eine `UIViewController` mit der `CreateViewController`-Erweiterungsmethode und legt die `Title`-Eigenschaft der `UIViewController` fest. Der `UIViewController` wird dann von der `PushViewController`-Methode auf `AppNavigationController`. Daher wird die Benutzeroberfläche, die in der xamarin. Forms-`NoteEntryPage` Klasse definiert ist, wie im folgenden Screenshot gezeigt angezeigt:

[![Screenshot einer xamarin. IOS-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/ios-noteentrypage.png "Xamarin. IOS-App mit XAML-Benutzeroberfläche")](native-forms-images/ios-noteentrypage-large.png#lightbox "Xamarin. IOS-App mit XAML-Benutzeroberfläche")

Wenn das `NoteEntryPage` angezeigt wird, werden die `UIViewController` für die `NoteEntryPage`-Klasse aus dem `AppNavigationController` in der rückwärts Navigation angezeigt, und der Benutzer wird an den `UIViewController` für die `NotesPage`-Klasse zurückgegeben. Wenn Sie jedoch eine `UIViewController` aus dem nativen IOS-Navigations Stapel löschen, werden die `UIViewController` und die angefügten `Page` Objekte nicht automatisch verworfen. Daher überschreibt die `AppNavigationController` Klasse die `PopViewController`-Methode, um die Ansichts Controller bei der rückwärts Navigation zu verwerfen:

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

Mit der `PopViewController` Überschreibung wird die `Dispose`-Methode für das `UIViewController` Objekt aufgerufen, das aus dem nativen IOS-Navigations Stapel entfernt wurde. Andernfalls führt dies dazu, dass die `UIViewController` und das angefügte `Page` Objekt verwaist sind.

> [!IMPORTANT]
> Verwaiste Objekte können nicht in die Garbage Collection aufgenommen werden, und dies führt zu einem Speicher Abfall.

## <a name="android"></a>Android

Unter Android ist die `OnCreate` Überschreibung in der `MainActivity`-Klasse in der Regel der Ort zum Ausführen von Aufgaben im Zusammenhang mit dem Anwendungsstart. Das folgende Codebeispiel zeigt die `MainActivity`-Klasse in der Beispielanwendung:

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

Die `OnCreate`-Methode führt die folgenden Aufgaben aus:

- Xamarin. Forms wird initialisiert, indem die `Forms.Init`-Methode aufgerufen wird.
- Ein Verweis auf die `MainActivity`-Klasse wird im Feld `static` `Instance` gespeichert. Dies dient zum Bereitstellen eines Mechanismus für andere Klassen zum aufzurufen von Methoden, die in der `MainActivity` Klasse definiert sind.
- Der `Activity` Inhalt wird aus einer layoutressource festgelegt. In der Beispielanwendung besteht das Layout aus einer `LinearLayout`, die einen `Toolbar` enthält, und einem `FrameLayout`, das als fragmentcontainer fungieren soll.
- Die `Toolbar` wird abgerufen und als Aktionsleiste für die `Activity` festgelegt, und der Titel der Aktionsleiste wird festgelegt.
- Die `FolderPath`-Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage`-Klasse, bei der es sich um eine xamarin. Forms- [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seite handelt, die in XAML definiert ist, wird mithilfe der `CreateSupportFragment`-Erweiterungsmethode erstellt und in eine `Fragment` konvertiert.
- Die `SupportFragmentManager`-Klasse erstellt und führt einen Commit für eine Transaktion aus, die die `FrameLayout` Instanz durch die `Fragment` für die `NotesPage`-Klasse ersetzt.

Weitere Informationen zu Fragmenten finden Sie unter [Fragmente](~/android/platform/fragments/index.md).

Nachdem die `OnCreate`-Methode ausgeführt wurde, wird die in der xamarin. Forms-`NotesPage` Klasse definierte Benutzeroberfläche angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer xamarin. Android-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/android-notespage.png "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")](native-forms-images/android-notespage-large.png#lightbox "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. durch Tippen auf die **+** [`Button`](xref:Xamarin.Forms.Button), führt zum folgenden Ereignishandler im `NotesPage` Code Behind, der ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

Im Feld `static` `MainActivity.Instance` kann die `MainActivity.NavigateToNoteEntryPage`-Methode aufgerufen werden. Dies wird im folgenden Codebeispiel gezeigt:

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

Die `NavigateToNoteEntryPage`-Methode konvertiert die [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete xamarin. Forms-Seite in eine `Fragment` mit der `CreateSupportFragment`-Erweiterungsmethode und fügt dem fragmentbackstapel den `Fragment` hinzu. Daher wird die Benutzeroberfläche, die im xamarin. Forms-`NoteEntryPage` definiert ist, wie im folgenden Screenshot gezeigt angezeigt:

[![Screenshot einer xamarin. Android-Anwendung, die eine in XAML definierte Benutzeroberfläche verwendet](native-forms-images/android-noteentrypage.png "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")](native-forms-images/android-noteentrypage-large.png#lightbox "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")

Wenn die `NoteEntryPage` angezeigt wird, wird beim Tippen auf den rückwärts Pfeil der `Fragment` für die `NoteEntryPage` aus dem fragmentbackstapel angezeigt, und der Benutzer wird an den `Fragment` für die `NotesPage`-Klasse zurückgegeben.

### <a name="enable-back-navigation-support"></a>Unterstützung für die Back Navigation aktivieren

Die `SupportFragmentManager`-Klasse verfügt über ein `BackStackChanged` Ereignis, das ausgelöst wird, wenn der Inhalt des fragmentbackstapels geändert wird. Die `OnCreate`-Methode in der `MainActivity`-Klasse enthält einen anonymen Ereignishandler für dieses Ereignis:

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

Dieser Ereignishandler zeigt eine Schaltfläche zurück auf der Aktionsleiste an, vorausgesetzt, dass es eine oder mehrere `Fragment` Instanzen auf dem fragmentbackstapel gibt. Die Antwort auf die Schaltfläche "zurück" wird durch die `OnOptionsItemSelected` Außerkraftsetzung behandelt:

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

Die `OnOptionsItemSelected` Überschreibung wird immer dann aufgerufen, wenn ein Element im Menü Optionen ausgewählt wird. Diese Implementierung löst das aktuelle Fragment aus dem fragmentbackstapel auf, vorausgesetzt, dass die Schaltfläche "zurück" ausgewählt wurde und mindestens eine `Fragment`-Instanz auf dem fragmentbackstapel vorhanden ist.

### <a name="multiple-activities"></a>Mehrere Aktivitäten

Wenn eine Anwendung aus mehreren Aktivitäten besteht, können [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seiten in jede der Aktivitäten eingebettet werden. In diesem Szenario muss die `Forms.Init`-Methode nur in der `OnCreate` Überschreibung des ersten `Activity` aufgerufen werden, das eine xamarin. Forms-`ContentPage` einbettet. Dies hat jedoch folgende Auswirkungen:

- Der Wert von `Xamarin.Forms.Color.Accent` wird aus dem `Activity` entnommen, der die `Forms.Init`-Methode aufgerufen hat.
- Der Wert von `Xamarin.Forms.Application.Current` wird mit dem `Activity` verknüpft, der die `Forms.Init`-Methode aufgerufen hat.

### <a name="choose-a-file"></a>Datei auswählen

Beim Einbetten einer [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleiteten Seite, die ein [`WebView`](xref:Xamarin.Forms.WebView) verwendet, das eine HTML-Schaltfläche "Datei auswählen" unterstützen muss, muss die `Activity` die `OnActivityResult`-Methode überschreiben:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Bei UWP ist die native `App`-Klasse in der Regel der Ort zum Ausführen von Aufgaben im Zusammenhang mit dem Anwendungsstart. Xamarin. Forms wird in der Regel in xamarin. Forms-UWP-Anwendungen initialisiert, in der `OnLaunched` Überschreibung in der systemeigenen `App`-Klasse, um das `LaunchActivatedEventArgs`-Argument an die `Forms.Init`-Methode zu übergeben. Aus diesem Grund können native UWP-Anwendungen, die eine xamarin. Forms- [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seite verwenden, die `Forms.Init`-Methode am einfachsten von der `App.OnLaunched`-Methode abrufen.

Standardmäßig wird die `MainPage`-Klasse von der systemeigenen `App`-Klasse als erste Seite der Anwendung gestartet. Das folgende Codebeispiel zeigt die `MainPage`-Klasse in der Beispielanwendung:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        this.Content = new Notes.UWP.Views.NotesPage().CreateFrameworkElement();
    }
    ...
}
```

Der `MainPage`-Konstruktor führt die folgenden Aufgaben aus:

- Das Caching ist für die Seite aktiviert, sodass ein neues `MainPage` nicht erstellt wird, wenn ein Benutzer zurück zur Seite wechselt.
- Ein Verweis auf die `MainPage`-Klasse wird im Feld `static` `Instance` gespeichert. Dies dient zum Bereitstellen eines Mechanismus für andere Klassen zum aufzurufen von Methoden, die in der `MainPage` Klasse definiert sind.
- Die `FolderPath`-Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage`-Klasse, bei der es sich um eine xamarin. Forms- [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seite handelt, die in XAML definiert ist, wird mithilfe der `CreateFrameworkElement`-Erweiterungsmethode erstellt und in eine `FrameworkElement` konvertiert und dann als Inhalt der `MainPage`-Klasse festgelegt.

Nachdem der `MainPage`-Konstruktor ausgeführt wurde, wird die Benutzeroberfläche, die in der xamarin. Forms-`NotesPage` Klasse definiert ist, wie im folgenden Screenshot gezeigt angezeigt:

[![Screenshot einer UWP-Anwendung, die eine mit xamarin. Forms XAML definierte Benutzeroberfläche verwendet](native-forms-images/uwp-notespage.png "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche")](native-forms-images/uwp-notespage-large.png#lightbox "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. durch Tippen auf die **+** [`Button`](xref:Xamarin.Forms.Button), führt zum folgenden Ereignishandler im `NotesPage` Code Behind, der ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

Im Feld `static` `MainPage.Instance` kann die `MainPage.NavigateToNoteEntryPage`-Methode aufgerufen werden. Dies wird im folgenden Codebeispiel gezeigt:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

Die Navigation in der UWP erfolgt in der Regel mit der `Frame.Navigate`-Methode, die ein `Page` Argument annimmt. Xamarin. Forms definiert eine `Frame.Navigate`-Erweiterungsmethode, die eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seiten Instanz annimmt. Wenn die `NavigateToNoteEntryPage`-Methode ausgeführt wird, wird daher die in der xamarin. Forms-`NoteEntryPage` definierte Benutzeroberfläche angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer UWP-Anwendung, die eine mit xamarin. Forms XAML definierte Benutzeroberfläche verwendet](native-forms-images/uwp-noteentrypage.png "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche")](native-forms-images/uwp-noteentrypage-large.png#lightbox "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche")

Wenn das `NoteEntryPage` angezeigt wird, wird beim Tippen auf den rückwärts Pfeil der `FrameworkElement` für die `NoteEntryPage` aus dem in-App-Back-Stack angezeigt, und der Benutzer wird an die `FrameworkElement` für die `NotesPage`-Klasse zurückgegeben.

### <a name="enable-back-navigation-support"></a>Unterstützung für die Back Navigation aktivieren

Bei UWP müssen Anwendungen die Navigation für alle Hardware-und Software-Back-Schaltflächen auf unterschiedlichen Geräte Formfaktoren aktivieren. Dies kann erreicht werden, indem ein Ereignishandler für das `BackRequested`-Ereignis registriert wird, das in der `OnLaunched`-Methode in der systemeigenen `App`-Klasse ausgeführt werden kann:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

Wenn die Anwendung gestartet wird, ruft die `GetForCurrentView`-Methode das `SystemNavigationManager`-Objekt ab, das der aktuellen Ansicht zugeordnet ist, und registriert dann einen Ereignishandler für das `BackRequested`-Ereignis. Die Anwendung empfängt dieses Ereignis nur, wenn es sich um die Vordergrund Anwendung handelt, und ruft als Antwort den `OnBackRequested` Ereignishandler auf:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

Der `OnBackRequested`-Ereignishandler ruft die `GoBack`-Methode für den Stamm Rahmen der Anwendung auf und legt die `BackRequestedEventArgs.Handled`-Eigenschaft auf `true` fest, um das Ereignis als behandelt zu markieren. Wenn das Ereignis nicht als behandelt markiert wird, kann es dazu führen, dass das Ereignis ignoriert wird.

Die Anwendung entscheidet, ob auf der Titelleiste eine Schaltfläche zurück angezeigt werden soll. Dies wird erreicht, indem die `AppViewBackButtonVisibility`-Eigenschaft auf einen der `AppViewBackButtonVisibility` Enumerationswerte festgelegt wird:

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
