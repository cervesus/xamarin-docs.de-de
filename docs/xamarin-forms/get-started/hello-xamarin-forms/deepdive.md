---
title: "Ausführliche Erläuterungen zu Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 3259e9b2bc9be52e8c19acce2dd031ad9046019b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-deep-dive"></a>Ausführliche Erläuterungen zu Xamarin.Forms

Im [Xamarin.Forms-Schnellstart](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md) wurde die Phoneword-Anwendung erstellt. In diesem Artikel wird dieser Vorgang noch einmal genauer betrachtet, um ein Verständnis für die Funktionsweise von Xamarin.Forms-Anwendungen zu erarbeiten.

Folgende Themen werden besprochen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Einführung in Visual Studio – Einführung in Visual Studio und das Erstellen einer neuen Xamarin.Forms-Anwendung

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

- Einführung in Visual Studio für Mac – Einführung in Visual Studio für Mac und das Erstellen einer neuen Xamarin.Forms-Anwendung

-----

- Aufbau einer Xamarin.Forms-Anwendung – Überblick über die wesentlichen Bestandteile einer Xamarin.Forms-Anwendung
- Architektur und Anwendungsgrundlagen – Wie die Anwendung auf jeder Plattform gestartet wird
- Benutzeroberfläche (UI) – Erstellen von Benutzeroberflächen in Xamarin.Forms
- Zusätzliche Konzepte in Phoneword – Kurze Beschreibungen der zusätzlichen Konzepte, die in der Phoneword-Anwendung verwendet werden
- Tests und Bereitstellung – Fertigstellen der Anwendung mit Ratschlägen zu Tests, zur Bereitstellung, zum Erstellen von Grafiken und vielem mehr

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Einführung in Visual Studio

Visual Studio ist eine leistungsstarke IDE von Microsoft. Sie enthält z.B. einen vollständig integrierten visuellen Designer, einen Text-Editor mit Refactoringtools, einen Assembly-Browser und Quellcodeintegration. Dieser Artikel konzentriert sich auf einige der grundlegenden Visual Studio-Funktionen mit dem Xamarin-Plug-In.

In Visual Studio wird Code in *Projektmappen* und *Projekte* organisiert. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Phoneword-Anwendung besteht wie im folgenden Screenshot gezeigt aus einer Projektmappe mit vier Projekten.

![](deepdive-images/vs/solution.png "Projektmappen-Explorer von Visual Studio")

Diese Projekte sind folgende:

- Phoneword: Dieses Projekt ist das .NET Standard-Klassenbibliotheksprojekt, das den gesamten freigegebenen Code und die gesamte freigegebene Benutzeroberfläche enthält.
- Phoneword.Android: Dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für die Android-Anwendung.
- Phoneword.iOS: Dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für die iOS-Anwendung.
- Phoneword.UWP: Dieses Projekt enthält für die universelle Windows-Plattform spezifischen Code (UWP) und ist der Einstiegspunkt für die UWP-Anwendung.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Einführung in Visual Studio für Mac

Visual Studio für Mac ist eine kostenlose Open-Source-IDE, die Visual Studio ähnelt. Sie enthält z.B. einen vollständig integrierten visuellen Designer, einen Text-Editor mit Refactoringtools, einen Assembly-Browser und Quellcodeintegration. Weitere Informationen zu Visual Studio für Mac finden Sie unter [Einführung in Visual Studio für Mac](/visualstudio/mac/).

Die Codeorganisation in Visual Studio für Mac baut auf Visual Studio auf und gliedert sich in *Projektmappen* und *Projekte*. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Phoneword-Anwendung besteht wie im folgenden Screenshot gezeigt aus einer Projektmappe mit drei Projekten.

![](deepdive-images/xs/solution.png "Visual Studio für Mac: Projektmappenbereich")

Diese Projekte sind folgende:

- Phoneword: Dieses Projekt ist das portable Klassenbibliotheksprojekt (Portable Class Library, PCL), das den gesamten freigegebenen Code und die gesamte freigegebene Benutzeroberfläche enthält.
- Phoneword.Droid: Dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für Android-Anwendungen.
- Phoneword.iOS: Dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für iOS-Anwendungen.

-----

## <a name="anatomy-of-a-xamarinforms-application"></a>Aufbau einer Xamarin.Forms-Anwendung

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Der folgende Screenshot zeigt den Inhalt des Phoneword .NET Standard-Bibliotheksprojekts in Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Inhalt des Phoneword-Projekts von .NET Standard")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält. Der **NuGet**-Knoten enthält das Xamarin.Forms-NuGet-Paket, das dem Projekt hinzugefügt wurde. Der **SDK**-Knoten enthält das `NETStandard.Library`-Metapaket, das auf alle NuGet-Pakete verweist, die .NET Standard definieren.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Der folgende Screenshot zeigt den Inhalt des Phoneword PCL-Projekts in Visual Studio für Mac:

![](deepdive-images/xs/pcl-project.png "Inhalt des Phoneword PCL-Projekts")

Das Projekt besteht aus drei Ordnern:

- **Verweise**: Enthält die Assemblys, die zum Erstellen und Ausführen der Anwendung erforderlich sind. Durch Erweitern des Ordners „.NET Portable-Teilmenge“ können Sie die Verweise auf .NET-Assemblys wie [System](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx), System.Core und [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx) einblenden. Durch Erweitern des Ordners **Von Paketen** lassen sich Verweise auf die Xamarin.Forms-Assemblys anzeigen.
- **Pakete**: Das Verzeichnis „Pakete“ enthält [NuGet](https://www.nuget.org)-Pakete, die die Verwendung von Drittanbieterbibliotheken in Ihrer Anwendung vereinfachen. Diese Pakete können auf die neuesten Releases aktualisiert werden, indem Sie mit der rechten Maustaste auf den Ordner klicken und die Updateoption im Popupmenü auswählen.
- **Eigenschaften**: Enthält **AssemblyInfo.cs**, eine Metadatendatei der .NET-Assembly. Es wird empfohlen, diese Datei mit grundlegenden Informationen zu Ihrer Anwendung zu füllen. Weitere Informationen zu dieser Datei finden Sie unter [AssemblyInfo-Klasse](http://msdn.microsoft.com/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx) auf MSDN.

-----

Das Projekt besteht aus mehreren Dateien:

- **App.xaml**: Das XAML-Markup für die `App`-Klasse, die ein Ressourcenverzeichnis für die Anwendung definiert
- **App.xaml.cs**: Das CodeBehind-Modell für die `App`-Klasse, das die erste Seite instanziiert, die von der Anwendung auf jeder Plattform angezeigt wird, und für die Behandlung von Ereignissen im App-Lebenszyklus zuständig ist
- **IDialer.cs**: Die `IDialer`-Schnittstelle, die angibt, dass die `Dial`-Methode von der implementierenden Klasse bereitgestellt werden muss.
- **MainPage.xaml**: Das XAML-Markup für die `MainPage`-Klasse, die die UI für die Seite definiert, die beim Start der Anwendung angezeigt wird
- **MainPage.xaml.cs**: Das CodeBehind-Modell für die `MainPage`-Klasse, das die Geschäftslogik enthält, die ausgeführt wird, wenn der Benutzer mit der Seite interagiert
- **packages.config**: (nur Visual Studio für Mac) Eine XML-Datei mit Informationen über die NuGet-Pakete, die vom Projekt verwendet werden, um erforderliche Pakete und ihre jeweiligen Versionen nachzuverfolgen. Sowohl Visual Studio für Mac als auch Visual Studio können so konfiguriert werden, dass fehlende NuGet-Pakete automatisch wiederhergestellt werden, wenn der Quellcode für andere Benutzer freigegeben wird. Der Inhalt dieser Datei wird vom NuGet-Paket-Manager gesteuert und sollte nicht manuell bearbeitet werden.
- **PhoneTranslator.cs**: Die Geschäftslogik, die eine Buchstabenwahl in eine Rufnummer konvertiert, die über **MainPage.xaml.cs** aufgerufen wird

Weitere Informationen zum Aufbau einer Xamarin.iOS-Anwendung finden Sie unter [Hello, iOS: Deep Dive (Hallo iOS: Ausführliche Informationen)](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy). Weitere Informationen zum Aufbau einer Xamarin.Android-Anwendung finden Sie unter [Hello, Android: Deep Dive (Hallo Android: Ausführliche Informationen)](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektur und Anwendungsgrundlagen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Eine Xamarin.Forms-Anwendung ist genauso aufgebaut wie eine traditionelle plattformübergreifende Anwendung. Freigegebener Code wird in der Regel in einer .NET Standard-Bibliothek platziert und von plattformspezifischen Anwendungen genutzt. Die folgende Abbildung bietet für die Phoneword-Anwendung einen Überblick über diese Beziehung:

![](deepdive-images/vs/architecture.png "Phoneword-Architektur")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Eine Xamarin.Forms-Anwendung ist genauso aufgebaut wie eine traditionelle plattformübergreifende Anwendung. Freigegebener Code wird in einer portablen Klassenbibliothek platziert, und plattformspezifische Anwendungen nutzen ihn. Die folgende Abbildung bietet für die Phoneword-Anwendung einen Überblick über diese Beziehung:

![](deepdive-images/xs/architecture.png "Phoneword-Architektur")

Weitere Informationen zu PCLs finden Sie unter [Introduction to Portable Class Libraries (Einführung in portable Klassenbibliotheken)](~/cross-platform/app-fundamentals/pcl.md).

-----

Um die Wiederverwendung von Startcode zu maximieren, haben Xamarin.Forms-Anwendungen eine winzige Klasse namens `App`, die die erste Seite instanziiert, die von der Anwendung wie im folgenden Codebeispiel gezeigt auf jeder Plattform angezeigt wird:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Phoneword
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new MainPage();
        }
        ...
    }
}
```

Dieser Code legt die `MainPage`-Eigenschaft der `App`-Klasse auf eine neue Instanz der [`MainPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/)-Klasse fest. Außerdem aktiviert das [`XamlCompilation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)-Attribut den XAML-Compiler, sodass XAML direkt in der Zwischensprache kompiliert wird. Weitere Informationen finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Starten der Anwendung auf jeder Plattform

### <a name="ios"></a>iOS

Um die Xamarin.Forms-Startseite in iOS zu starten, enthält das Phoneword.iOS-Projekt die `AppDelegate`-Klasse, die wie im folgenden Codebeispiel gezeigt von der `FormsApplicationDelegate`-Klasse erbt:

```csharp
namespace Phoneword.iOS
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init ();
            LoadApplication (new App ());
            return base.FinishedLaunching (app, options);
        }
    }
}
```

Die `FinishedLaunching`-Außerkraftsetzung initialisiert das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode. Dadurch wird die iOS-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor der Root View Controller durch den Aufruf auf die `LoadApplication`-Methode festgelegt wird.

### <a name="android"></a>Android

Für das Aufrufen der Xamarin.Forms-Startseite in Android enthält das Phoneword.Droid-Projekt Code, der eine `Activity` mit dem `MainLauncher`-Attribut erstellt, wobei die Aktivität wie im folgenden Codebeispiel gezeigt von der `FormsApplicationActivity`-Klasse erbt:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword",
              Icon = "@drawable/icon",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

Die `OnCreate`-Außerkraftsetzung initialisiert das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode. Dadurch wird die Android-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor die Xamarin.Forms-Anwendung geladen wird. Darüber hinaus speichert die `MainActivity`-Klasse einen Verweis auf sich selbst in der `Instance`-Eigenschaft. Die `Instance`-Eigenschaft ist als lokaler Kontext bekannt und darauf wird von der `PhoneDialer`-Klasse aus verwiesen.

## <a name="universal-windows-platform"></a>Universelle Windows-Plattform

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Anwendungen der universellen Windows-Plattform (Universal Windows Platform, UWP) wird die `Init`-Methode, die das Xamarin.Forms-Framework initialisiert, über die `App`-Klasse aufgerufen:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Dadurch wird die UWP-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen. Die Xamarin.Forms-Startseite wird wie im folgenden Codebeispiel veranschaulicht über die `MainPage`-Klasse aufgerufen:

```csharp
namespace Phoneword.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Phoneword.App());
        }
    }
}
```
Die Xamarin.Forms-Anwendung wird mit der `LoadApplication`-Methode geladen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Apps der universellen Windows-Plattform (UWP) können mit Xamarin.Forms erstellt werden, jedoch nur mit Visual Studio unter Windows.

-----

## <a name="user-interface"></a>Benutzeroberfläche

Es gibt vier Hauptsteuerelementgruppen, mit denen die Benutzeroberfläche einer Xamarin.Forms-Anwendung erstellt wird.

1. **Seiten**: Xamarin.Forms-Seiten stellen Bildschirme plattformübergreifender mobiler Anwendung dar. Die Phoneword-Anwendung verwendet die [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Klasse, um einen einzelnen Bildschirm anzuzeigen. Weitere Informationen zu Seiten finden Sie unter [Xamarin.Forms Pages (Xamarin.Forms-Seiten)](~/xamarin-forms/user-interface/controls/pages.md).
1. **Layouts**: Xamarin.Forms-Layouts sind Container, die zum Erstellen von Ansichten in Form von logischen Strukturen verwendet werden. Die Phoneword-Anwendung verwendet die [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)-Klasse, um Steuerelemente in einem horizontalen Stapel anzuordnen. Weitere Informationen zu Seiten finden Sie unter [Xamarin.Forms Layouts (Xamarin.Forms-Layouts)](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Ansichten**: Xamarin.Forms-Ansichten sind die Steuerelemente auf der Benutzeroberfläche, z.B. Bezeichnungen, Schaltflächen und Textfelder. Die Phoneword-Anwendung verwendet die Steuerelemente [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) und [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/). Weitere Informationen zu Ansichten finden Sie unter [Xamarin.Forms Views (Xamarin.Forms-Ansichten)](~/xamarin-forms/user-interface/controls/views.md).
1. **Zellen**: Xamarin.Forms Zellen sind spezielle Elemente für Listenelemente und beschreiben, wie jedes Element in einer Liste gezeichnet werden soll. Die Phoneword-Anwendung verwendet keinerlei Zellen. Weitere Informationen zu Zellen finden Sie unter [Xamarin.Forms Cells (Xamarin.Forms-Zellen)](~/xamarin-forms/user-interface/controls/cells.md).

Zur Laufzeit wird jedes Steuerelement seinem nativen Äquivalent zugeordnet. Dieses wird dann gerendert.

Wenn die Phoneword-Anwendung auf einer Plattform ausgeführt wird, zeigt sie einen einzigen Bildschirm an, der [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) in Xamarin.Forms entspricht. `Page` stellt unter Android eine *ViewGroup*, unter iOS einen *Ansichtscontroller* oder eine *Seite* auf der universellen Windows-Plattform dar. Die Phoneword-Anwendung instanziiert auch ein [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Objekt, das die `MainPage`-Klasse darstellt, deren XAML-Markup im folgenden Codebeispiel gezeigt wird:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                         x:Class="Phoneword.MainPage">
        ...
        <StackLayout>
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
</ContentPage>
```

Die `MainPage`-Klasse verwendet ein [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)-Steuerelement, um Steuerelemente auf dem Bildschirm unabhängig von seiner Größe automatisch anzuordnen. Alle untergeordneten Elemente werden nacheinander vertikal und in der Reihenfolge positioniert, in der sie hinzugefügt wurden. Das `StackLayout`-Steuerelement enthält ein [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)-Steuerelement zum Anzeigen von Text auf der Seite, ein [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)-Steuerelement zum Akzeptieren von Textbenutzereingaben und zwei [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)-Steuerelemente zum Ausführen von Code als Reaktion auf Fingereingabeereignisse.

Weitere Informationen zu XAML in Xamarin.Forms finden Sie unter [Xamarin.Forms XAML Basics (XAML-Grundlagen von Xamarin.Forms)](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Reagieren auf eine Benutzerinteraktion

Ein in XAML definiertes Objekt kann ein Ereignis auslösen, das in der CodeBehind-Datei behandelt wird. Das folgende Codebeispiel zeigt die `OnTranslate`-Methode in der CodeBehind-Datei für die `MainPage`-Klasse, die als Reaktion auf das [`Clicked`](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/)-Ereignis ausgeführt wird, das über die Schaltfläche *Übersetzen* ausgelöst wird.

```csharp
void OnTranslate(object sender, EventArgs e)
{
    translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
    if (!string.IsNullOrWhiteSpace (translatedNumber)) {
        callButton.IsEnabled = true;
        callButton.Text = "Call " + translatedNumber;
    } else {
        callButton.IsEnabled = false;
        callButton.Text = "Call";
    }
}
```

Die `OnTranslate`-Methode übersetzt die Buchstabenwahl in die entsprechende Telefonnummer und legt daraufhin die Eigenschaften auf der Anrufschaltfläche fest. Die CodeBehind-Datei für eine XAML-Klasse kann auf ein Objekt zugreifen, das mit dem Namen, der ihm mit dem `x:Name`-Attribut zugewiesen wurde, in XAML definiert wurde. So wie bei C#-Variablen muss der diesem Attribut zugewiesene Wert mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Die Verknüpfung der Schaltfläche „Übersetzen“ mit der `OnTranslate`-Methode geschieht im XAML-Markup für die `MainPage`-Klasse:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Zusätzliche in Phoneword eingeführte Konzepte

Mit der Phoneword-Anwendung für Xamarin.Forms wurden verschiedene neue Konzepte eingeführt, die den Rahmen dieses Artikels übersteigen. Dies sind z.B. folgende Konzepte:

- Aktivieren und Deaktivieren von Schaltflächen (buttons). Ein [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) kann durch Änderungen der [`IsEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/)-Eigenschaft ein- und ausgeschaltet werden. Im folgenden Codebeispiel wird beispielsweise `callButton` deaktiviert:

        callButton.IsEnabled = false;

- Anzeigen eines Warndialogfelds. Wenn der Benutzer die **Anrufschaltfläche** drückt, zeigt die Phoneword-Anwendung ein *Warndialogfeld* mit der Option an, einen Anruf zu tätigen oder abzubrechen. Mit der [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/)-Methode wird wie im folgenden Codebeispiel dargestellt das Dialogfeld erstellt:

        await this.DisplayAlert (
                "Dial a Number",
                "Would you like to call " + translatedNumber + "?",
                "Yes",
                "No");

- Zugreifen auf native Funktionen über die [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)-Klasse. Die Phoneword-Anwendung verwendet die `DependencyService`-Klasse, um die `IDialer`-Schnittstelle in plattformspezifische Telefonanwahlimplementierungen aufzulösen. Dies wird im folgenden Codebeispiel aus dem Phoneword-Projekt dargestellt:

        async void OnCall (object sender, EventArgs e)
        {
            ...
            var dialer = DependencyService.Get<IDialer> ();
            ...
        }

  Weitere Informationen zur [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)-Klasse finden Sie unter [Accessing Native Features via the DependencyService (Zugreifen auf native Funktionen über DependencyService)](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Tätigen eines Telefonanrufs mit einer URL. Die Phoneword-Anwendung verwendet `OpenURL`, um die Telefon-App des Systems zu starten. Die URL besteht wie im folgenden Codebeispiel aus dem iOS-Projekt gezeigt aus einem `tel:`-Präfix, gefolgt von der Rufnummer, die aufgerufen werden soll:

        return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));

- Optimieren des Plattformlayouts. Mit der [`Device`](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)-Klasse können Entwickler das Anwendungslayout und die Funktionen plattformbezogen anpassen. Dies wird im folgenden Codebeispiel gezeigt, in dem andere [`Padding`](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)-Werte für verschiedene Plattformen verwendet werden, um jede Seite korrekt anzuzeigen:

        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                     ...>
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            ...
        </ContentPage>

  Weitere Informationen zu Plattformanpassungen finden Sie unter [Device Class (Geräteklasse)](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Tests und Bereitstellung

Sowohl Visual Studio für Mac als auch Visual Studio stellen viele Optionen zum Testen und Bereitstellen einer Anwendung bereit. Das Debuggen von Anwendungen ist ein üblicher Vorgang im Lebenszyklus der Anwendungsentwicklung und trägt dazu bei, Codeprobleme zu diagnostizieren. Weitere Informationen finden Sie unter [Festlegen eines Haltepunkts](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [Step Through Code (Detailliertes Durchlaufen von Code)](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/) und [Output Information to the Log Window (Ausgabeinformationen an das Protokollfenster)](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

Simulatoren eigenen sich hervorragend für die Bereitstellung und das Testen einer Anwendung. Außerdem bieten sie nützliche Funktionen zum Testen von Anwendungen. Allerdings verwenden Benutzer die endgültige Anwendung nicht in einem Simulator. Daher sollte die Anwendungen frühzeitig und häufig auf echten Geräten getestet werden. Weitere Informationen zur Bereitstellung von iOS-Geräten finden Sie unter [Device Provisioning (Gerätebereitstellung)](~/ios/get-started/installation/device-provisioning/index.md). Weitere Informationen zur Bereitstellung von Android-Geräten finden Sie unter [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms erläutert. Zu den behandelten Themen zählen die Struktur einer Xamarin.Forms-Anwendung, die Architektur- und Anwendungsgrundlagen sowie die Benutzeroberfläche.

Im nächsten Abschnitt dieses Handbuchs wird die Anwendung auf mehrere Bildschirme erweitert, um Sie mit der erweiterten Xamarin.Forms-Architektur und den entsprechenden Konzepten vertraut zu machen.
