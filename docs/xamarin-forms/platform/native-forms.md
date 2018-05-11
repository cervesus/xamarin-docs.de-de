---
title: Systemeigene Formulare
description: Systemeigene Forms ermöglichen Xamarin.Forms ContentPage verwendet abgeleitete Seiten von systemeigene universelle Windows-Plattform (UWP), Xamarin.iOS und Xamarin.Android Projekte verwendet wird. Systemeigene Projekte können Seiten ContentPage verwendet abgeleitete nutzen, die dem Projekt oder aus einer .NET Standard-Bibliothek, die Standardbibliothek .NET oder freigegebenes Projekt direkt hinzugefügt werden. In diesem Artikel wird erläutert, wie ContentPage verwendet abgeleitete Seiten verarbeitet werden, die direkt auf systemeigene Projekte hinzugefügt werden und wie zwischen ihnen zu navigieren.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: bb7aa9a7071f9ac7bef0dce5790a3fe74302cfb4
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="native-forms"></a>Systemeigene Formulare

_Systemeigene Forms ermöglichen Xamarin.Forms ContentPage verwendet abgeleitete Seiten von systemeigene universelle Windows-Plattform (UWP), Xamarin.iOS und Xamarin.Android Projekte verwendet wird. Systemeigene Projekte können Seiten ContentPage verwendet abgeleitete nutzen, die dem Projekt oder aus einer .NET Standard-Bibliothek, die Standardbibliothek .NET oder freigegebenes Projekt direkt hinzugefügt werden. In diesem Artikel wird erläutert, wie ContentPage verwendet abgeleitete Seiten verarbeitet werden, die direkt auf systemeigene Projekte hinzugefügt werden und wie zwischen ihnen zu navigieren._

In der Regel eine Xamarin.Forms-Anwendung besteht aus einer oder mehreren Seiten, die davon Herleiten [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), und diese Seiten werden in einer .NET Standard-Steuerelementbibliothek-Projekt oder ein freigegebenes Projekt alle Plattformen gemeinsam. Erlaubt jedoch die systemeigene Forms `ContentPage`-abgeleitete Seiten direkt systemeigene universelle Windows-Plattform, Xamarin.iOS und Xamarin.Android Anwendungen hinzugefügt werden. Im Vergleich zu mit dem systemeigenen Projekt nutzen `ContentPage`-abgeleitete Seiten aus einer .NET Standard-Steuerelementbibliothek-Projekt oder ein freigegebenes Projekt den Vorteil, dass beim Hinzufügen von Seiten direkt in systemeigenen Projekten ist, dass die Seiten mit systemeigenen Sichten erweitert werden können. Systemeigene Ansichten können dann den Namen in XAML mit `x:Name` und der Code-Behind verwiesen wird. Weitere Informationen zu systemeigenen Ansichten finden Sie unter [systemeigenen Ansichten](~/xamarin-forms/platform/native-views/index.md).

Der Prozess für die Nutzung einer Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleiteten Seite in einem systemeigenen Projekt lautet wie folgt:

1. Fügen Sie das Xamarin.Forms-NuGet-Paket am systemeigenen Projekt hinzu.
1. Hinzufügen der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seite und alle Abhängigkeiten, um das systemeigene Projekt abgeleitet.
1. Rufen Sie die `Forms.Init`-Methode auf.
1. Erstellen eine Instanz von der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seite abgeleitet und konvertieren Sie ihn in den entsprechenden systemeigenen Typ, der mit einer der folgenden Erweiterungsmethoden: `CreateViewController` für iOS, `CreateFragment` oder `CreateSupportFragment` für Android, oder `CreateFrameworkElement` für universelle Windows-Plattform.
1. Navigieren Sie zu den systemeigenen Typ-Darstellung des der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seite unter Verwendung der systemeigenen Navigation API abgeleitet.

Xamarin.Forms muss initialisiert werden, durch Aufrufen der `Forms.Init` -Methode auf, bevor ein systemeigenes Projekt erstellen, kann eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seite abgeleitet. Wann in erster Linie dazu Auswahl hängt, wenn es am einfachsten in den Anwendungsablauf ist – konnte ausgeführt werden, beim Start der Anwendung oder direkt vor der `ContentPage`-abgeleiteten Seite erstellt wird. In diesem Artikel und die zugehörigen Beispielanwendungen die `Forms.Init` Methode wird beim Start der Anwendung aufgerufen.

> [!NOTE]
> Die **NativeForms** Anwendung errorhandling enthält keine Projekte Xamarin.Forms. Stattdessen besteht es von einem Xamarin.iOS-Projekt, ein Projekt Xamarin.Android und uwp-Projekt. Jedes Projekt ist ein systemeigenes Projekt, das systemeigene Forms verwendet werden, um nutzen [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seiten abgeleitet. Es ist jedoch kein Grund, warum die systemeigene Projekte nutzen konnte nicht `ContentPage`-Seiten von einer .NET Standard-Steuerelementbibliothek-Projekt oder ein freigegebenes Projekt abgeleitet.

Bei Verwendung von systemeigenen Forms Funktionen z. B. Xamarin.Forms [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), und das Datenbindungsmodul alle noch arbeiten.

## <a name="ios"></a>iOS

Bei iOS kann die `FinishedLaunching` außer Kraft setzen, der `AppDelegate` Klasse ist in der Regel die Position, an die Anwendung ausführen Start Aufgaben im Zusammenhang mit. Sie wird aufgerufen, nachdem die Anwendung gestartet wurde, und normalerweise überschrieben wird, um das Hauptfenster konfigurieren und Anzeigen von Controller. Das folgende Codebeispiel zeigt die `AppDelegate` Klasse in der beispielanwendung:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

Die `FinishedLaunching` Methode führt die folgenden Aufgaben:

- Xamarin.Forms wird initialisiert, durch Aufrufen der `Forms.Init` Methode.
- Ein Verweis auf die `AppDelegate` Klasse befindet sich in der `static` `Instance` Feld. Dadurch geben Sie einen Mechanismus für andere Klassen in definierte Methoden aufrufen, wird die `AppDelegate` Klasse.
- Die `UIWindow`, also der wichtigsten Container für Ansichten in systemeigenen iOS-Anwendungen wird erstellt.
- Die `PhonewordPage` Klasse, die eine Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seite, die in XAML definiert, wird erstellt und in konvertiert eine `UIViewController` mithilfe der `CreateViewController` Erweiterungsmethode.
- Die `Title` Eigenschaft von der `UIViewController` festgelegt ist, wird auf angezeigt werden die `UINavigationBar`.
- Ein `UINavigationController` wird erstellt, für die hierarchische Navigation verwalten. Die `UINavigationController` Klasse verwaltet einen Stapel von View-Controller und die `UIViewController` übergebenen Konstruktor die dienstdatenverschlüsselung anfänglich beim der `UINavigationController` geladen wird.
- Die `UINavigationController` Instanz ist als der obersten Ebene festgelegt `UIViewController` für die `UIWindow`, und die `UIWindow` ist als das Fenster für die Anwendung festgelegt und sichtbar gemacht.

Einmal die `FinishedLaunching` Methode ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `PhonewordPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

Interaktion mit der Benutzeroberfläche, z. B. durch Tippen auf eine [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), führt in den Ereignishandlern in der `PhonewordPage` Code-Behind ausführen. Z. B. wenn ein Benutzer tippt der **Anrufe** Schaltfläche den folgenden Ereignishandler ausgeführt wird:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

Die `static` `AppDelegate.Instance` Feld ermöglicht es dem `AppDelegate.NavigateToCallHistoryPage` -Methode aufgerufen werden soll, die im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

Die `NavigateToCallHistoryPage` -Methode konvertiert die Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seite, um abgeleitete eine `UIViewController` mit der `CreateViewController` Erweiterungsmethode, und legt die `Title` Eigenschaft der `UIViewController`. Die `UIViewController` dann leisten `UINavigationController` durch die `PushViewController` Methode. Aus diesem Grund die Benutzeroberfläche definiert, in der Xamarin.Forms `CallHistoryPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

Bei der `CallHistoryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil wird angezeigt. die `UIViewController` für die `CallHistoryPage` -Klasse aus der `UINavigationController`, Zurückgeben des Benutzers die `UIViewController` für die `PhonewordPage` Klasse.

## <a name="android"></a>Android

Auf Android-Geräten die `OnCreate` außer Kraft setzen, der `MainActivity` Klasse ist in der Regel die Position, an die Anwendung ausführen Aufgaben im Zusammenhang mit Start. Das folgende Codebeispiel zeigt die `MainActivity` Klasse in der beispielanwendung:

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

Die `OnCreate` Methode führt die folgenden Aufgaben:

- Xamarin.Forms wird initialisiert, durch Aufrufen der `Forms.Init` Methode.
- Ein Verweis auf die `MainActivity` Klasse befindet sich in der `static` `Instance` Feld. Dadurch geben Sie einen Mechanismus für andere Klassen in definierte Methoden aufrufen, wird die `MainActivity` Klasse.
- Die `Activity` Inhalt aus einer Ressource Layout festgelegt ist. In der beispielanwendung das Layout besteht aus einem `LinearLayout` , enthält eine `Toolbar`, und ein `FrameLayout` fungieren als Fragmentcontainer.
- Die `Toolbar` wird abgerufen, und legen Sie als der Aktionsleiste für die `Activity`, und der Titel der Aktion Leiste festgelegt ist.
- Die `PhonewordPage` Klasse, die eine Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seite, die in XAML definiert, wird erstellt und in konvertiert eine `Fragment` mithilfe der `CreateFragment` Erweiterungsmethode.
- Die `FragmentManager` Klasse erstellt und führt einen Commit für eine Transaktion, die ersetzt die `FrameLayout` -Instanz mit der `Fragment` für die `PhonewordPage` Klasse.

Weitere Informationen zu Fragmenten finden Sie unter [Fragmente](~/android/platform/fragments/index.md).

> [!NOTE]
> Zusätzlich zu den `CreateFragment` Erweiterungsmethode, Xamarin.Forms enthält auch eine `CreateSupportFragment` Methode. Die `CreateFragment` Methode erstellt eine `Android.App.Fragment` , die in Anwendungen, die API 11 als Ziel verwendet und größer sein kann. Die `CreateSupportFragment` Methode erstellt eine `Android.Support.V4.App.Fragment` , die in Anwendungen, die API-Versionen vor 11 als Ziel verwendet werden kann.

Einmal die `OnCreate` Methode ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `PhonewordPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

Interaktion mit der Benutzeroberfläche, z. B. durch Tippen auf eine [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), führt in den Ereignishandlern in der `PhonewordPage` Code-Behind ausführen. Z. B. wenn ein Benutzer tippt der **Anrufe** Schaltfläche den folgenden Ereignishandler ausgeführt wird:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

Die `static` `MainActivity.Instance` Feld ermöglicht es dem `MainActivity.NavigateToCallHistoryPage` -Methode aufgerufen werden soll, die im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

Die `NavigateToCallHistoryPage` -Methode konvertiert die Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seite, um abgeleitete eine `Fragment` mit der `CreateFragment` Erweiterungsmethode, und fügt die `Fragment` dem Fragment sichern Stapel. Aus diesem Grund die Benutzeroberfläche definiert, in der Xamarin.Forms `CallHistoryPage` angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

Bei der `CallHistoryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil wird angezeigt. die `Fragment` für die `CallHistoryPage` Zurückgeben von aus dem Fragment-Stapel zurück, des Benutzers die `Fragment` für die `PhonewordPage` Klasse.

### <a name="enabling-back-navigation-support"></a>Aktivieren der Unterstützung für die Navigation zurück

Die `FragmentManager` -Klasse verfügt über eine `BackStackChanged` Ereignis, das ausgelöst wird, wenn der Inhalt des Fragments Back Stapels ändert. Die `OnCreate` Methode in der `MainActivity` Klasse enthält einen anonymen Ereignishandler für dieses Ereignis:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Dieser Ereignishandler zeigt eine zurück-Schaltfläche auf der Aktionsleiste, vorausgesetzt, es ist mindestens ein `Fragment` Instanzen im Fragment sichern Stapel. Die Antwort auf die Schaltfläche "zurück" tippen erfolgt durch die `OnOptionsItemSelected` außer Kraft setzen:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

Die `OnOptionsItemSelected` Außerkraftsetzung wird aufgerufen, wenn ein Element im Menü "Optionen" ausgewählt ist. Diese Implementierung wird der aktuelle Nachrichtenfragmente aus dem Fragment-Stapel zurück, vorausgesetzt, dass die Schaltfläche "zurück" ausgewählt wurde, und es eine oder mehrere gibt `Fragment` Instanzen im Fragment sichern Stapel.

### <a name="multiple-activities"></a>Mehrere Aktivitäten

Wenn eine Anwendung mehrere Aktivitäten besteht [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seiten in jede der Aktivitäten eingebettet werden können. In diesem Szenario die `Forms.Init` Methode muss aufgerufen werden, nur in der `OnCreate` Überschreiben des ersten `Activity` , die eine Xamarin.Forms bettet `ContentPage`. Dies hat jedoch die folgenden Auswirkungen:

- Der Wert der `Xamarin.Forms.Color.Accent` entnommen werden die `Activity` , aufgerufen die `Forms.Init` Methode.
- Der Wert der `Xamarin.Forms.Application.Current` zugeordnet wird die `Activity` , aufgerufen die `Forms.Init` Methode.

### <a name="choosing-a-file"></a>Auswählen einer Datei

Beim Einbetten von eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seite, verwendet eine [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) benötigt, die zur Unterstützung von HTML "Choose File" Schaltfläche, die `Activity` überschreiben, müssen die `OnActivityResult` Methode:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Für universelle Windows-Plattform, die systemeigene `App` Klasse ist in der Regel die Position, an die Anwendung ausführen Aufgaben im Zusammenhang mit starten. Xamarin.Forms normalerweise initialisiert wird, in Xamarin.Forms uwp-Apps, in der `OnLaunched` außer Kraft setzen, in der systemeigenen `App` -Klasse, übergeben die `LaunchActivatedEventArgs` Argument an die `Forms.Init` Methode. Aus diesem Grund native uwp-Apps, die eine Xamarin.Forms nutzen [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleiteter kann am einfachsten Aufrufen der `Forms.Init` Methode aus der `App.OnLaunched` Methode.

Wird standardmäßig die systemeigene `App` -Klasse startet die `MainPage` Klasse als die erste Seite der Anwendung. Das folgende Codebeispiel zeigt die `MainPage` Klasse in der beispielanwendung:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

Die `MainPage` Konstruktor führt die folgenden Aufgaben:

- Zwischenspeicherung für die Seite aktiviert ist, damit ein neues `MainPage` wird nicht erstellt werden, wenn ein Benutzer wieder zur Seite navigiert.
- Ein Verweis auf die `MainPage` Klasse befindet sich in der `static` `Instance` Feld. Dadurch geben Sie einen Mechanismus für andere Klassen in definierte Methoden aufrufen, wird die `MainPage` Klasse.
- Die `PhonewordPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seite, die in XAML definiert, wird erstellt und in konvertiert eine `FrameworkElement` mithilfe der `CreateFrameworkElement` Erweiterungsmethode, und legen Sie anschließend als Inhalt der `MainPage` Klasse.

Einmal die `MainPage` Konstruktor ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `PhonewordPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/uwp-phonewordpage.png "Uwp-PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

Interaktion mit der Benutzeroberfläche, z. B. durch Tippen auf eine [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), führt in den Ereignishandlern in der `PhonewordPage` Code-Behind ausführen. Z. B. wenn ein Benutzer tippt der **Anrufe** Schaltfläche den folgenden Ereignishandler ausgeführt wird:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

Die `static` `MainPage.Instance` Feld ermöglicht es dem `MainPage.NavigateToCallHistoryPage` -Methode aufgerufen werden soll, die im folgenden Codebeispiel gezeigt wird:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Navigation in uwp-App erfolgt in der Regel mit der `Frame.Navigate` Methode, die akzeptiert eine `Page` Argument. Xamarin.Forms definiert eine `Frame.Navigate` Erweiterungsmethode, die akzeptiert eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Seiteninstanz abgeleitet. Aus diesem Grund, dass bei der `NavigateToCallHistoryPage` Methode ausgeführt wird, die Benutzeroberfläche in der Xamarin.Forms definierten `CallHistoryPage` angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/uwp-callhistorypage.png "Uwp-CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

Bei der `CallHistoryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil wird angezeigt. die `FrameworkElement` für die `CallHistoryPage` aus dem in-app-Back-Stapel Zurückgeben des Benutzers die `FrameworkElement` für die `PhonewordPage` Klasse.

### <a name="enabling-back-navigation-support"></a>Aktivieren der Unterstützung für die Navigation zurück

Auf UWP müssen Anwendungen Rückwärtsnavigation für alle Hardware- und softwareanforderungen Back Schaltflächen für Formfaktoren für verschiedene Geräte ermöglicht. Dies kann erreicht werden, indem Sie registrieren einen Ereignishandler für das `BackRequested` -Ereignis, das im erfolgen kann die `OnLaunched` Methode in der systemeigenen `App` Klasse:

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

Wenn die Anwendung gestartet wird, die `GetForCurrentView` Methode ruft die `SystemNavigationManager` Objekt der aktuellen Ansicht zugeordnet ist, und registriert einen Ereignishandler für das `BackRequested` Ereignis. Die Anwendung wird nur dieses Ereignis empfängt, wenn sie die Anwendung im Vordergrund ist und als Reaktion ruft der `OnBackRequested` Ereignishandler:

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

Die `OnBackRequested` Ereignishandleraufrufe die `GoBack` Methode im Rahmen der Stamm der Anwendung und legt die `BackRequestedEventArgs.Handled` Eigenschaft `true` das Ereignis als behandelt markiert werden. Das Ereignis als behandelt markiert werden, kann im System Weg von der Anwendung (auf die mobile Gerätefamilie) navigieren oder ignorieren das Ereignis (auf den desktop Gerätefamilie) kommen.

Die Anwendung basiert auf eine vom System bereitgestellte-zurück-Schaltfläche auf einem Telefon jedoch auswählt, ob eine zurück-Schaltfläche auf der Titelleiste auf desktop-Geräte angezeigt. Dies erfolgt durch Festlegen der `AppViewBackButtonVisibility` -Eigenschaft auf einen von der `AppViewBackButtonVisibility` Enumerationswerte:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

Die `OnNavigated` -Ereignishandler ausgeführt wird, als Antwort auf die `Navigated` das Auslösen von Ereignissen aktualisiert der Sichtbarkeit der Titelleiste, Schaltfläche "zurück", wenn Seite navigiert wird. Dadurch wird sichergestellt, dass Back Titelleistenschaltfläche sichtbar ist, wenn der in-app-Back-Stapel nicht leer ist, oder aus der Titelleiste entfernt werden, wenn in der app-Back-Stapel leer ist.

Weitere Informationen zur Unterstützung von Rückwärtsnavigation für universelle Windows-Plattform finden Sie unter [Navigationsverlauf und Abwärtskompatibilität Navigation für uwp-apps](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Zusammenfassung

Systemeigene Forms zulassen Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seiten von systemeigene universelle Windows-Plattform (UWP), Xamarin.iOS und Xamarin.Android Projekte verwendet wird. Systemeigene Projekte nutzen können `ContentPage`-abgeleitet von Seiten, die dem Projekt oder eine .NET Standard-Bibliotheksprojekt oder ein freigegebenes Projekt direkt hinzugefügt werden. In diesem Artikel wird erläutert, wie nutzen `ContentPage`-abgeleitet von Seiten, die systemeigene Projekte sowie zum Navigieren zwischen ihnen direkt hinzugefügt werden.


## <a name="related-links"></a>Verwandte Links

- [NativeForms (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Native Ansichten](~/xamarin-forms/platform/native-views/index.md)
