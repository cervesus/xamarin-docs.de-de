---
title: Xamarin.Forms in Xamarin Native-Projekten
description: Dieser Artikel beschreibt, wie ContentPage abgeleitete Seiten zu nutzen, die native Xamarin-Projekte direkt hinzugefügt werden, und zwischen ihnen zu navigieren.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 65bb3fa070c082fa6c6c489e326a870a80fb9502
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997511"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms in Xamarin Native-Projekten

_Native Formulare können Xamarin.Forms-ContentPage abgeleitete Seiten von systemeigenen Projekten für Xamarin.iOS, Xamarin.Android und universelle Windows-Plattform (UWP) genutzt werden. Systemeigene Projekte können Seiten ContentPage abgeleitete nutzen, die direkt auf das Projekt oder von einer .NET Standard-Bibliothek, eine .NET Standard-Bibliothek oder ein freigegebenes Projekt hinzugefügt werden. Dieser Artikel beschreibt, wie Seiten ContentPage abgeleitete nutzen, die direkt auf systemeigene Projekte hinzugefügt werden, und zwischen ihnen zu navigieren._

In der Regel eine Xamarin.Forms-Anwendung enthält eine oder mehrere Seiten, die von abgeleitet [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), und für diese Seiten werden alle Plattformen in einer .NET Standard Library-Projekt oder ein freigegebenes Projekt freigegeben. Native Formulare können jedoch `ContentPage`-abgeleitete Seiten direkt in systemeigenen Xamarin.iOS, Xamarin.Android und UWP-Anwendungen hinzugefügt werden. Im Vergleich zu, müssen das systemeigene Projekt nutzen `ContentPage`-abgeleitete Seiten aus einer .NET Standard Library-Projekt oder ein freigegebenes Projekt, den Vorteil, Hinzufügen von Seiten direkt in systemeigenen Projekten ist, dass die Seiten mit nativer Sichten erweitert werden können. Native Ansichten können dann den Namen in XAML mit `x:Name` und im Code-Behind verwiesen wird. Weitere Informationen zu systemeigenen Ansichten finden Sie unter [nativer Sichten](~/xamarin-forms/platform/native-views/index.md).

Der Prozess für die Nutzung einer Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite in einem systemeigenen Projekt lautet wie folgt:

1. Fügen Sie am systemeigenen Projekt das Xamarin.Forms-NuGet-Paket hinzu.
1. Hinzufügen der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite und alle Abhängigkeiten, um das systemeigene Projekt.
1. Rufen Sie die `Forms.Init`-Methode auf.
1. Erstellt eine Instanz von der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite abgeleitet, und konvertieren Sie ihn in den entsprechenden nativen Typ, der mit einem der folgenden Erweiterungsmethoden: `CreateViewController` für iOS, `CreateFragment` oder `CreateSupportFragment` für Android, oder `CreateFrameworkElement` für UWP.
1. Navigieren Sie mit Ihrer Darstellung in den systemeigenen Typ der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite mit der systemeigenen API-Navigation abgeleitet.

Xamarin.Forms muss initialisiert werden, durch den Aufruf der `Forms.Init` -Methode auf, bevor Sie ein systemeigenen Projekt erstellen, kann ein [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite abgeleitet. Entscheidung, wann in erster Linie dazu hängt, wenn es am einfachsten in den Anwendungsablauf wird – kann ausgeführt werden, beim Start der Anwendung, oder direkt vor der `ContentPage`-abgeleitete Seite erstellt wird. In diesem Artikel, und die zugehörigen Beispielanwendungen die `Forms.Init` Methode wird beim Start der Anwendung aufgerufen.

> [!NOTE]
> Die **NativeForms** beispiellösung für die Anwendung enthält keine Xamarin.Forms-Projekte. Stattdessen besteht sie von einem Xamarin.iOS-Projekt, eine Xamarin.Android-Projekt und ein UWP-Projekt. Jedes Projekt wird ein systemeigenes Projekt, das Native Formen verwendet wird, nutzen [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seiten abgeleitet. Es ist jedoch kein Grund, warum auf systemeigene Projekte nutzen konnte nicht `ContentPage`-Seiten von einer .NET Standard Library-Projekt oder ein freigegebenes Projekt abgeleitet.

Wenn Sie Native Formulare verwenden zu können, Funktionen wie z. B. Xamarin.Forms [ `DependencyService` ](xref:Xamarin.Forms.DependencyService), [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), und die Datenbindungs-Engine, alle noch arbeiten.

## <a name="ios"></a>iOS

Unter iOS die `FinishedLaunching` außer Kraft setzen, der `AppDelegate` Klasse ist normalerweise der Ort, an die Anwendung ausführen beim Start Aufgaben im Zusammenhang mit. Sie wird aufgerufen, nachdem die Anwendung gestartet hat, und in der Regel überschrieben werden, wird um das Hauptfenster zu konfigurieren und Domänencontroller anzeigen. Das folgende Codebeispiel zeigt die `AppDelegate` Klasse in der beispielanwendung:

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

- Xamarin.Forms wird initialisiert, durch den Aufruf der `Forms.Init` Methode.
- Ein Verweis auf die `AppDelegate` Klasse befindet sich in der `static` `Instance` Feld. Dies ist einen Mechanismus für andere Klassen in definierte Methoden aufrufen, bieten die `AppDelegate` Klasse.
- Die `UIWindow`, d.h. der Hauptcontainer für Ansichten in native iOS-Anwendungen wird erstellt.
- Die `PhonewordPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die in XAML definiert, wird erstellt und in eine `UIViewController` mithilfe der `CreateViewController` -Erweiterungsmethode.
- Die `Title` Eigenschaft der `UIViewController` festgelegt ist, wird auf angezeigt werden die `UINavigationBar`.
- Ein `UINavigationController` wird zum Verwalten von hierarchischen Navigation erstellt. Die `UINavigationController` Klasse verwaltet einen Stapel von View-Controller, und die `UIViewController` übergebenen Konstruktor angezeigt ursprünglich bei der `UINavigationController` geladen wird.
- Die `UINavigationController` Instanz wird festgelegt, als der obersten Ebene `UIViewController` für die `UIWindow`, und die `UIWindow` ist als die wichtigsten Fenster für die Anwendung festgelegt und sichtbar gemacht wird.

Sobald die `FinishedLaunching` Methode ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `PhonewordPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

Interaktion mit der Benutzeroberfläche, z. B. durch Tippen auf eine [ `Button` ](xref:Xamarin.Forms.Button), führt in den Ereignishandlern in die `PhonewordPage` Code-Behind ausgeführt. Wenn beispielsweise ein Benutzer tippt der **Anrufliste** Schaltfläche den folgenden Ereignishandler ausgeführt wird:

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

Die `NavigateToCallHistoryPage` -Methode konvertiert die Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, um eine `UIViewController` mit der `CreateViewController` Erweiterungsmethode, und legt die `Title` Eigenschaft der `UIViewController`. Die `UIViewController` klicken Sie dann zu leisten ist `UINavigationController` durch die `PushViewController` Methode. Aus diesem Grund die Benutzeroberfläche definiert, in der Xamarin.Forms `CallHistoryPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

Bei der `CallHistoryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil angezeigt der `UIViewController` für die `CallHistoryPage` -Klasse aus der `UINavigationController`, Zurückgeben des Benutzers die `UIViewController` für die `PhonewordPage` Klasse.

## <a name="android"></a>Android

Unter Android die `OnCreate` außer Kraft setzen, der `MainActivity` Klasse ist normalerweise der Ort, an die Anwendung ausführen beim Start Aufgaben im Zusammenhang mit. Das folgende Codebeispiel zeigt die `MainActivity` Klasse in der beispielanwendung:

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

- Xamarin.Forms wird initialisiert, durch den Aufruf der `Forms.Init` Methode.
- Ein Verweis auf die `MainActivity` Klasse befindet sich in der `static` `Instance` Feld. Dies ist einen Mechanismus für andere Klassen in definierte Methoden aufrufen, bieten die `MainActivity` Klasse.
- Die `Activity` Inhalt aus einer layoutressource festgelegt ist. In diesem Beispiel, das Layout besteht aus einem `LinearLayout` , enthält eine `Toolbar`, und ein `FrameLayout` fungieren als Fragmentcontainer.
- Die `Toolbar` abgerufen wird, und legen Sie als der Aktionsleiste für die `Activity`, und der Titel der Aktion Leiste festgelegt ist.
- Die `PhonewordPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die in XAML definiert, wird erstellt und in eine `Fragment` mithilfe der `CreateFragment` -Erweiterungsmethode.
- Die `FragmentManager` Klasse erstellt und führt einen Commit für eine Transaktion, die ersetzt die `FrameLayout` -Instanz mit der `Fragment` für die `PhonewordPage` Klasse.

Weitere Informationen zu Fragmenten finden Sie unter [Fragmente](~/android/platform/fragments/index.md).

> [!NOTE]
> Zusätzlich zu den `CreateFragment` -Erweiterungsmethode, Xamarin.Forms enthält auch eine `CreateSupportFragment` Methode. Die `CreateFragment` -Methode erstellt eine `Android.App.Fragment` , die in Anwendungen, die API-11 als Ziel verwendet und größer sein kann. Die `CreateSupportFragment` -Methode erstellt eine `Android.Support.V4.App.Fragment` , die in Anwendungen, die API-Versionen vor 11 als Ziel verwendet werden kann.

Sobald die `OnCreate` Methode ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `PhonewordPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

Interaktion mit der Benutzeroberfläche, z. B. durch Tippen auf eine [ `Button` ](xref:Xamarin.Forms.Button), führt in den Ereignishandlern in die `PhonewordPage` Code-Behind ausgeführt. Wenn beispielsweise ein Benutzer tippt der **Anrufliste** Schaltfläche den folgenden Ereignishandler ausgeführt wird:

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

Der `NavigateToCallHistoryPage` -Methode konvertiert die Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seite, um abgeleitete eine `Fragment` mit der `CreateFragment` -Erweiterungsmethode, und fügt die `Fragment` dem Fragment Rückwärtsstapel. Aus diesem Grund die Benutzeroberfläche definiert, in der Xamarin.Forms `CallHistoryPage` wird angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

Bei der `CallHistoryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil angezeigt der `Fragment` für die `CallHistoryPage` aus dem BackStack Fragment, Zurückgeben von dem Benutzer die `Fragment` für die `PhonewordPage` Klasse.

### <a name="enabling-back-navigation-support"></a>Aktivieren der Unterstützung für die Rückwärtsnavigation

Die `FragmentManager` -Klasse verfügt über eine `BackStackChanged` -Ereignis, das ausgelöst wird, wenn der Inhalt des Fragments BackStack geändert. Die `OnCreate` -Methode in der die `MainActivity` -Klasse enthält einen anonymes Ereignis-Handler für dieses Ereignis:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Dieser Ereignishandler zeigt eine Schaltfläche "zurück" in der Aktionsleiste, vorausgesetzt, es eine oder mehrere gibt `Fragment` Rückwärtsstapel für Instanzen auf das Fragment. Die Antwort auf die durch Tippen auf die Schaltfläche "zurück" erfolgt durch die `OnOptionsItemSelected` außer Kraft setzen:

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

Die `OnOptionsItemSelected` Außerkraftsetzung wird immer dann aufgerufen, wenn ein Element in das Menü "Optionen" ausgewählt ist. Diese Implementierung wird das aktuelle Fragment aus dem BackStack Fragment, vorausgesetzt, dass die Schaltfläche "zurück" ausgewählt wurde, und es eine oder mehrere gibt `Fragment` Rückwärtsstapel für Instanzen auf das Fragment.

### <a name="multiple-activities"></a>Mehrere Aktivitäten

Wenn eine Anwendung mehrere Aktivitäten besteht [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seiten in jede der Aktivitäten eingebettet werden können. In diesem Szenario die `Forms.Init` -Methode muss aufgerufen werden, nur in der `OnCreate` außer Kraft setzen, der das erste `Activity` , die eine Xamarin.Forms bettet `ContentPage`. Dies hat jedoch die folgende Auswirkungen:

- Der Wert des `Xamarin.Forms.Color.Accent` entnommen werden die `Activity` , aufgerufen der `Forms.Init` Methode.
- Der Wert des `Xamarin.Forms.Application.Current` zugeordnet wird der `Activity` , mit dem Namen der `Forms.Init` Methode.

### <a name="choosing-a-file"></a>Auswählen einer Datei

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

- Zwischenspeichern auf der Seite aktiviert ist, damit ein neues `MainPage` wird nicht erstellt werden, wenn ein Benutzer zurück zur Seite navigiert.
- Ein Verweis auf die `MainPage` Klasse befindet sich in der `static` `Instance` Feld. Dies ist einen Mechanismus für andere Klassen in definierte Methoden aufrufen, bieten die `MainPage` Klasse.
- Die `PhonewordPage` -Klasse, die eine Xamarin.Forms ist [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite, die in XAML definiert, wird erstellt und in eine `FrameworkElement` mithilfe der `CreateFrameworkElement` Erweiterungsmethode, und legen Sie dann als Inhalt des der `MainPage` Klasse.

Sobald die `MainPage` Konstruktor ausgeführt wurde, die Benutzeroberfläche definiert, in der Xamarin.Forms `PhonewordPage` -Klasse angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/uwp-phonewordpage.png "UWP-PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

Interaktion mit der Benutzeroberfläche, z. B. durch Tippen auf eine [ `Button` ](xref:Xamarin.Forms.Button), führt in den Ereignishandlern in die `PhonewordPage` Code-Behind ausgeführt. Wenn beispielsweise ein Benutzer tippt der **Anrufliste** Schaltfläche den folgenden Ereignishandler ausgeführt wird:

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

Navigation im UWP erfolgt in der Regel mit der `Frame.Navigate` Methode, die akzeptiert eine `Page` Argument. Xamarin.Forms definiert eine `Frame.Navigate` -Erweiterungsmethode, die akzeptiert eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seiteninstanz abgeleitet. Aus diesem Grund, wenn die `NavigateToCallHistoryPage` Methode ausgeführt wird, die Benutzeroberfläche, die in der Xamarin.Forms definiert `CallHistoryPage` wird angezeigt, wie im folgenden Screenshot gezeigt:

[![](native-forms-images/uwp-callhistorypage.png "UWP-CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

Bei der `CallHistoryPage` angezeigt wird, tippen Sie auf der Rückseite Pfeil wird angezeigt. die `FrameworkElement` für die `CallHistoryPage` aus dem BackStack von in-app Zurückgeben des Benutzers die `FrameworkElement` für die `PhonewordPage` Klasse.

### <a name="enabling-back-navigation-support"></a>Aktivieren der Unterstützung für die Rückwärtsnavigation

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

Die `OnBackRequested` -Ereignishandler ruft die `GoBack` Methode im Bereich "Root" der Anwendung, und legt die `BackRequestedEventArgs.Handled` Eigenschaft `true` das Ereignis als behandelt markieren. Fehler bei das Ereignis als behandelt markieren kann das System verlassen zu müssen die Anwendung (auf die mobile-Gerätefamilie) oder ignorieren das Ereignis (auf der desktop-Gerät-Familie) führen.

Die Anwendung basiert auf eine vom System bereitgestellte-zurück-Schaltfläche auf einem Smartphone, aber Sie entscheiden, ob eine zurück-Schaltfläche auf der Titelleiste auf desktop-Geräte angezeigt. Dies wird erreicht, indem die `AppViewBackButtonVisibility` Eigenschaft eines der `AppViewBackButtonVisibility` -Enumerationswerte fest:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

Die `OnNavigated` -Ereignishandler, die Ausführung als Antwort auf die `Navigated` das Auslösen von Ereignissen aktualisiert der Sichtbarkeit der Schaltfläche "zurück" in der Titelzeile aus, wenn die Seite navigiert wird. Dadurch wird sichergestellt, dass der Back-Titelleistenschaltfläche angezeigt wird, wenn der in-app-Back-Stapel nicht leer ist oder über die Titelleiste entfernt werden, wenn der in-app-Back-Stapel leer ist.

Weitere Informationen zur Unterstützung der Rückwärtsnavigation in UWP finden Sie unter [Navigationsverlauf und Abwärtskompatibilität Navigation für UWP-apps](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Zusammenfassung

Native Formulare können Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Seiten zur Nutzung durch systemeigene Projekte für Xamarin.iOS, Xamarin.Android und universelle Windows-Plattform (UWP) abgeleitet. Systemeigene Projekte können nutzen `ContentPage`-abgeleitet, die direkt auf das Projekt oder eine .NET Standard-Bibliotheksprojekts oder ein freigegebenes Projekt hinzugefügt werden. In diesem Artikel wurde erläutert, wie nutzen `ContentPage`-abgeleitet, die direkt auf systemeigene Projekte sowie die Navigation zwischen ihnen hinzugefügt werden.


## <a name="related-links"></a>Verwandte Links

- [NativeForms (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Native Ansichten](~/xamarin-forms/platform/native-views/index.md)
