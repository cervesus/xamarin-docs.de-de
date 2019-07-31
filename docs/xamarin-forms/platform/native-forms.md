---
title: Xamarin.Forms in Xamarin Native-Projekten
description: Dieser Artikel beschreibt, wie ContentPage abgeleitete Seiten zu nutzen, die native Xamarin-Projekte direkt hinzugefügt werden, und zwischen ihnen zu navigieren.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/03/2019
ms.openlocfilehash: 88483f151852e882d6bac42a2d0c3fd0857060fb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653239"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms in Xamarin Native-Projekten

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

In der Regel eine Xamarin.Forms-Anwendung enthält eine oder mehrere Seiten, die von abgeleitet [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), und für diese Seiten werden alle Plattformen in einer .NET Standard Library-Projekt oder ein freigegebenes Projekt freigegeben. Native Formulare können jedoch `ContentPage`-abgeleitete Seiten direkt in systemeigenen Xamarin.iOS, Xamarin.Android und UWP-Anwendungen hinzugefügt werden. Im Vergleich zu, müssen das systemeigene Projekt nutzen `ContentPage`-abgeleitete Seiten aus einer .NET Standard Library-Projekt oder ein freigegebenes Projekt, den Vorteil, Hinzufügen von Seiten direkt in systemeigenen Projekten ist, dass die Seiten mit nativer Sichten erweitert werden können. Native Ansichten können dann den Namen in XAML mit `x:Name` und im Code-Behind verwiesen wird. Weitere Informationen zu systemeigenen Ansichten finden Sie unter [nativer Sichten](~/xamarin-forms/platform/native-views/index.md).

Der Prozess für die Nutzung einer Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite in einem systemeigenen Projekt lautet wie folgt:

1. Fügen Sie am systemeigenen Projekt das Xamarin.Forms-NuGet-Paket hinzu.
1. Hinzufügen der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite und alle Abhängigkeiten, um das systemeigene Projekt.
1. Rufen Sie die `Forms.Init`-Methode auf.
1. Erstellt eine Instanz von der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite abgeleitet, und konvertieren Sie ihn in den entsprechenden nativen Typ, der mit einem der folgenden Erweiterungsmethoden: `CreateViewController` für iOS, `CreateSupportFragment` für Android oder `CreateFrameworkElement` für UWP.
1. Navigieren Sie mit Ihrer Darstellung in den systemeigenen Typ der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite mit der systemeigenen API-Navigation abgeleitet.

Xamarin.Forms muss initialisiert werden, durch den Aufruf der `Forms.Init` -Methode auf, bevor Sie ein systemeigenen Projekt erstellen, kann ein [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite abgeleitet. Entscheidung, wann in erster Linie dazu hängt, wenn es am einfachsten in den Anwendungsablauf wird – kann ausgeführt werden, beim Start der Anwendung, oder direkt vor der `ContentPage`-abgeleitete Seite erstellt wird. In diesem Artikel, und die zugehörigen Beispielanwendungen die `Forms.Init` Methode wird beim Start der Anwendung aufgerufen.

> [!NOTE]
> Die **NativeForms** beispiellösung für die Anwendung enthält keine Xamarin.Forms-Projekte. Stattdessen besteht sie von einem Xamarin.iOS-Projekt, eine Xamarin.Android-Projekt und ein UWP-Projekt. Jedes Projekt wird ein systemeigenes Projekt, das Native Formen verwendet wird, nutzen [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seiten abgeleitet. Es ist jedoch kein Grund, warum auf systemeigene Projekte nutzen konnte nicht `ContentPage`-Seiten von einer .NET Standard Library-Projekt oder ein freigegebenes Projekt abgeleitet.

Wenn Sie Native Formulare verwenden zu können, Funktionen wie z. B. Xamarin.Forms [ `DependencyService` ](xref:Xamarin.Forms.DependencyService), [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), und die Datenbindungs-Engine, alle noch arbeiten. Allerdings muss die Seitennavigation mithilfe der systemeigenen API-Navigation ausgeführt werden.

## <a name="ios"></a>iOS

Unter iOS die `FinishedLaunching` außer Kraft setzen, der `AppDelegate` Klasse ist normalerweise der Ort, an die Anwendung ausführen beim Start Aufgaben im Zusammenhang mit. Sie wird aufgerufen, nachdem die Anwendung gestartet hat, und in der Regel überschrieben werden, wird um das Hauptfenster zu konfigurieren und Domänencontroller anzeigen. Das folgende Codebeispiel zeigt die `AppDelegate` Klasse in der beispielanwendung:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static string FolderPath { get; private set; }

    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;
    UIViewController _noteEntryPage;

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

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

Die `FinishedLaunching` Methode führt die folgenden Aufgaben:

- Xamarin.Forms wird initialisiert, durch den Aufruf der `Forms.Init` Methode.
- Ein Verweis auf die `AppDelegate` Klasse befindet sich in der `static` `Instance` Feld. Dies ist einen Mechanismus für andere Klassen in definierte Methoden aufrufen, bieten die `AppDelegate` Klasse.
- Die `UIWindow`, d.h. der Hauptcontainer für Ansichten in native iOS-Anwendungen wird erstellt.
- Die `FolderPath` -Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die in XAML definiert, wird erstellt und in eine `UIViewController` mithilfe der `CreateViewController` -Erweiterungsmethode.
- Die `Title` Eigenschaft der `UIViewController` festgelegt ist, wird auf angezeigt werden die `UINavigationBar`.
- Ein `UINavigationController` wird zum Verwalten von hierarchischen Navigation erstellt. Die `UINavigationController` Klasse verwaltet einen Stapel von View-Controller, und die `UIViewController` übergebenen Konstruktor angezeigt ursprünglich bei der `UINavigationController` geladen wird.
- Die `UINavigationController` Instanz wird festgelegt, als der obersten Ebene `UIViewController` für die `UIWindow`, und die `UIWindow` ist als die wichtigsten Fenster für die Anwendung festgelegt und sichtbar gemacht wird.

Sobald die `FinishedLaunching` Methode ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `NotesPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer xamarin. IOS-Anwendung, die eine in der XAML verwende](native-forms-images/ios-notespage.png "xamarin. IOS-App definierte Benutzeroberfläche mit einer XAML-Benutzeroberfläche")](native-forms-images/ios-notespage-large.png#lightbox "Xamarin. IOS-App mit XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. **+** durch Tippen auf den [`Button`](xref:Xamarin.Forms.Button), führt dazu, dass der `NotesPage` folgende Ereignishandler im Code-Behind-Code ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

Die `static` `AppDelegate.Instance` Feld ermöglicht es dem `AppDelegate.NavigateToNoteEntryPage` -Methode aufgerufen werden soll, die im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    _noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    _noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(_noteEntryPage, true);
}
```

Die `NavigateToNoteEntryPage` -Methode konvertiert die Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, um eine `UIViewController` mit der `CreateViewController` Erweiterungsmethode, und legt die `Title` Eigenschaft der `UIViewController`. Die `UIViewController` klicken Sie dann zu leisten ist `UINavigationController` durch die `PushViewController` Methode. Aus diesem Grund die Benutzeroberfläche definiert, in der Xamarin.Forms `NoteEntryPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer xamarin. IOS-Anwendung, die eine in der XAML](native-forms-images/ios-noteentrypage.png "xamarin. IOS-App definierte Benutzeroberfläche mit einer XAML-Benutzeroberfläche") verwendet](native-forms-images/ios-noteentrypage-large.png#lightbox "Xamarin. IOS-App mit XAML-Benutzeroberfläche")

Bei der `NoteEntryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil angezeigt der `UIViewController` für die `NoteEntryPage` -Klasse aus der `UINavigationController`, Zurückgeben des Benutzers die `UIViewController` für die `NotesPage` Klasse.

> [!IMPORTANT]
> Durch das popping a `UIViewController` aus dem nativen IOS-Navigations Stapel werden von `UIViewController`nicht automatisch gelöscht. Es liegt in der Verantwortung des Entwicklers sicherzustellen, `UIViewController` dass alle, die nicht mehr benötigt `Dispose` werden, die-Methode `UIViewController` aufgerufen haben `Page` . andernfalls werden die und die angefügten verwaist und werden nicht von der Garbage Collector erfasst. Dies führt zu einem Speicher Verluste.

## <a name="android"></a>Android

Unter Android die `OnCreate` außer Kraft setzen, der `MainActivity` Klasse ist normalerweise der Ort, an die Anwendung ausführen beim Start Aufgaben im Zusammenhang mit. Das folgende Codebeispiel zeigt die `MainActivity` Klasse in der beispielanwendung:

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

Die `OnCreate` Methode führt die folgenden Aufgaben:

- Xamarin.Forms wird initialisiert, durch den Aufruf der `Forms.Init` Methode.
- Ein Verweis auf die `MainActivity` Klasse befindet sich in der `static` `Instance` Feld. Dies ist einen Mechanismus für andere Klassen in definierte Methoden aufrufen, bieten die `MainActivity` Klasse.
- Die `Activity` Inhalt aus einer layoutressource festgelegt ist. In diesem Beispiel, das Layout besteht aus einem `LinearLayout` , enthält eine `Toolbar`, und ein `FrameLayout` fungieren als Fragmentcontainer.
- Die `Toolbar` abgerufen wird, und legen Sie als der Aktionsleiste für die `Activity`, und der Titel der Aktion Leiste festgelegt ist.
- Die `FolderPath` -Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die in XAML definiert, wird erstellt und in eine `Fragment` mithilfe der `CreateSupportFragment` -Erweiterungsmethode.
- Die `SupportFragmentManager` Klasse erstellt und führt einen Commit für eine Transaktion, die ersetzt die `FrameLayout` -Instanz mit der `Fragment` für die `NotesPage` Klasse.

Weitere Informationen zu Fragmenten finden Sie unter [Fragmente](~/android/platform/fragments/index.md).

Sobald die `OnCreate` Methode ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `NotesPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer xamarin. Android-Anwendung, die eine in der XAML](native-forms-images/android-notespage.png "xamarin. Android-App definierte Benutzeroberfläche mit einer XAML-Benutzeroberfläche") verwendet](native-forms-images/android-notespage-large.png#lightbox "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. **+** durch Tippen auf den [`Button`](xref:Xamarin.Forms.Button), führt dazu, dass der `NotesPage` folgende Ereignishandler im Code-Behind-Code ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

Die `static` `MainActivity.Instance` Feld ermöglicht es dem `MainActivity.NavigateToNoteEntryyPage` -Methode aufgerufen werden soll, die im folgenden Codebeispiel gezeigt wird:

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

Der `NavigateToNoteEntryPage` -Methode konvertiert die Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite, um abgeleitete eine `Fragment` mit der `CreateSupportFragment` -Erweiterungsmethode, und fügt die `Fragment` dem Fragment Rückwärtsstapel. Aus diesem Grund die Benutzeroberfläche definiert, in der Xamarin.Forms `NoteEntryPage` wird angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer xamarin. Android-Anwendung, die eine in der XAML](native-forms-images/android-noteentrypage.png "xamarin. Android-App definierte Benutzeroberfläche mit einer XAML-Benutzeroberfläche") verwendet](native-forms-images/android-noteentrypage-large.png#lightbox "Xamarin. Android-App mit einer XAML-Benutzeroberfläche")

Bei der `NoteEntryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil angezeigt der `Fragment` für die `NoteEntryPage` aus dem BackStack Fragment, Zurückgeben von dem Benutzer die `Fragment` für die `NotesPage` Klasse.

### <a name="enable-back-navigation-support"></a>Unterstützung für die Back Navigation aktivieren

Die `SupportFragmentManager` -Klasse verfügt über eine `BackStackChanged` -Ereignis, das ausgelöst wird, wenn der Inhalt des Fragments BackStack geändert. Die `OnCreate` -Methode in der die `MainActivity` -Klasse enthält einen anonymes Ereignis-Handler für dieses Ereignis:

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

Dieser Ereignishandler zeigt eine Schaltfläche "zurück" in der Aktionsleiste, vorausgesetzt, es eine oder mehrere gibt `Fragment` Rückwärtsstapel für Instanzen auf das Fragment. Die Antwort auf die durch Tippen auf die Schaltfläche "zurück" erfolgt durch die `OnOptionsItemSelected` außer Kraft setzen:

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

Die `OnOptionsItemSelected` Außerkraftsetzung wird immer dann aufgerufen, wenn ein Element in das Menü "Optionen" ausgewählt ist. Diese Implementierung wird das aktuelle Fragment aus dem BackStack Fragment, vorausgesetzt, dass die Schaltfläche "zurück" ausgewählt wurde, und es eine oder mehrere gibt `Fragment` Rückwärtsstapel für Instanzen auf das Fragment.

### <a name="multiple-activities"></a>Mehrere Aktivitäten

Wenn eine Anwendung mehrere Aktivitäten besteht [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seiten in jede der Aktivitäten eingebettet werden können. In diesem Szenario die `Forms.Init` -Methode muss aufgerufen werden, nur in der `OnCreate` außer Kraft setzen, der das erste `Activity` , die eine Xamarin.Forms bettet `ContentPage`. Dies hat jedoch die folgende Auswirkungen:

- Der Wert des `Xamarin.Forms.Color.Accent` entnommen werden die `Activity` , aufgerufen der `Forms.Init` Methode.
- Der Wert des `Xamarin.Forms.Application.Current` zugeordnet wird der `Activity` , mit dem Namen der `Forms.Init` Methode.

### <a name="choose-a-file"></a>Datei auswählen

Beim Einbetten von einer [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die verwendet eine [ `WebView` ](xref:Xamarin.Forms.WebView) benötigt, die zur Unterstützung von HTML "Choose File" Schaltfläche, die `Activity` außer Kraft setzen müssen die `OnActivityResult` Methode:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Auf UWP, die Native `App` Klasse ist normalerweise der Ort, an die Anwendung ausführen beim Start Aufgaben im Zusammenhang mit. Xamarin.Forms ist in der Regel initialisiert in Xamarin.Forms UWP-Anwendungen in der `OnLaunched` außer Kraft setzen, in der systemeigenen `App` -Klasse, übergeben die `LaunchActivatedEventArgs` Argument für die `Forms.Init` Methode. Aus diesem Grund nativen UWP-Anwendungen, die eine Xamarin.Forms nutzen [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite kann am einfachsten Aufrufen der `Forms.Init` Methode aus der `App.OnLaunched` Methode.

Wird standardmäßig die Native `App` Klasse startet die `MainPage` Klasse als erste Seite der Anwendung. Das folgende Codebeispiel zeigt die `MainPage` Klasse in der beispielanwendung:

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

Die `MainPage` Konstruktor führt die folgenden Aufgaben:

- Zwischenspeichern auf der Seite aktiviert ist, damit ein neues `MainPage` wird nicht erstellt werden, wenn ein Benutzer zurück zur Seite navigiert.
- Ein Verweis auf die `MainPage` Klasse befindet sich in der `static` `Instance` Feld. Dies ist einen Mechanismus für andere Klassen in definierte Methoden aufrufen, bieten die `MainPage` Klasse.
- Die `FolderPath` -Eigenschaft wird mit einem Pfad auf dem Gerät initialisiert, auf dem Hinweis Daten gespeichert werden.
- Die `NotesPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die in XAML definiert, wird erstellt und in eine `FrameworkElement` mithilfe der `CreateFrameworkElement` Erweiterungsmethode, und legen Sie dann als Inhalt des der `MainPage` Klasse.

Sobald die `MainPage` Konstruktor ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `NotesPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer UWP-Anwendung, die eine mit xamarin. Forms-XAML](native-forms-images/uwp-notespage.png "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche") definierte Benutzeroberfläche verwendet](native-forms-images/uwp-notespage-large.png#lightbox "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche")

Die Interaktion mit der Benutzeroberfläche, z. b. **+** durch Tippen auf den [`Button`](xref:Xamarin.Forms.Button), führt dazu, dass der `NotesPage` folgende Ereignishandler im Code-Behind-Code ausgeführt wird:

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

Die `static` `MainPage.Instance` Feld ermöglicht es dem `MainPage.NavigateToNoteEntryPage` -Methode aufgerufen werden soll, die im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

Navigation im UWP erfolgt in der Regel mit der `Frame.Navigate` Methode, die akzeptiert eine `Page` Argument. Xamarin.Forms definiert eine `Frame.Navigate` -Erweiterungsmethode, die akzeptiert eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seiteninstanz abgeleitet. Aus diesem Grund, wenn die `NavigateToNoteEntryPage` Methode ausgeführt wird, die Benutzeroberfläche, die in der Xamarin.Forms definiert `NoteEntryPage` wird angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot einer UWP-Anwendung, die eine mit xamarin. Forms](native-forms-images/uwp-noteentrypage.png "-XAML-UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche") definierte Benutzeroberfläche verwendet](native-forms-images/uwp-noteentrypage-large.png#lightbox "UWP-App mit xamarin. Forms-XAML-Benutzeroberfläche")

Bei der `NoteEntryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil wird angezeigt. die `FrameworkElement` für die `NoteEntryPage` aus dem BackStack von in-app Zurückgeben des Benutzers die `FrameworkElement` für die `NotesPage` Klasse.

### <a name="enable-back-navigation-support"></a>Unterstützung für die Back Navigation aktivieren

Auf UWP müssen Anwendungen Navigationsverlauf zurück für alle Hardware- und Back-Schaltflächen, auf verschiedenen geräteausführungen aktivieren. Dies kann erreicht werden, indem Sie registrieren einen Ereignishandler für die `BackRequested` -Ereignis, das ausgeführt werden kann die `OnLaunched` -Methode in der systemeigenen `App` Klasse:

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

Wenn die Anwendung gestartet wird, die `GetForCurrentView` Methode ruft die `SystemNavigationManager` Objekt der aktuellen Ansicht zugeordnet ist, und registriert einen Ereignishandler für die `BackRequested` Ereignis. Die Anwendung empfängt dieses Ereignis nur, wenn es der Anwendung im Vordergrund ist und als Reaktion ruft der `OnBackRequested` -Ereignishandler:

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

Die `OnBackRequested` -Ereignishandler ruft die `GoBack` Methode im Bereich "Root" der Anwendung, und legt die `BackRequestedEventArgs.Handled` Eigenschaft `true` das Ereignis als behandelt markieren. Wenn das Ereignis nicht als behandelt markiert wird, kann es dazu führen, dass das Ereignis ignoriert wird.

Die Anwendung entscheidet, ob auf der Titelleiste eine Schaltfläche zurück angezeigt werden soll. Dies wird erreicht, indem die `AppViewBackButtonVisibility` Eigenschaft eines der `AppViewBackButtonVisibility` -Enumerationswerte fest:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

Die `OnNavigated` -Ereignishandler, die Ausführung als Antwort auf die `Navigated` das Auslösen von Ereignissen aktualisiert der Sichtbarkeit der Schaltfläche "zurück" in der Titelzeile aus, wenn die Seite navigiert wird. Dadurch wird sichergestellt, dass der Back-Titelleistenschaltfläche angezeigt wird, wenn der in-app-Back-Stapel nicht leer ist oder über die Titelleiste entfernt werden, wenn der in-app-Back-Stapel leer ist.

Weitere Informationen zur Unterstützung der Rückwärtsnavigation in UWP finden Sie unter [Navigationsverlauf und Abwärtskompatibilität Navigation für UWP-apps](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="related-links"></a>Verwandte Links

- [NativeForms (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [Native Ansichten](~/xamarin-forms/platform/native-views/index.md)
