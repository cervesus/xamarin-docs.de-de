---
title: Teil 1. Erste Schritte mit XAML
description: In einer Xamarin.Forms-Anwendung wird XAML hauptsächlich zum Definieren des visuellen Inhalts von einer Seite und zusammen mit einem Code-Behind-Datei verwendet.
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 855bbc61fb5e4e653dbd39ddf05fac3e2fb42d8c
ms.sourcegitcommit: 00deecefc17a98210bed12b4ef99ecca710275f1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67493356"
---
# <a name="part-1-getting-started-with-xaml"></a>Teil 1. Erste Schritte mit XAML

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_In einer Xamarin.Forms-Anwendung XAML wird hauptsächlich zum Definieren des visuellen Inhalts einer Seite, und kann mit einem C# Code-Behind-Datei._

Die Code-Behind-Datei bietet Unterstützung für das Markup. Tragen diese beiden Dateien zusammen, um eine neue Klassendefinition, die untergeordnete Ansichten und die Initialisierung einer Eigenschaft enthält. Klicken Sie in der XAML-Datei Klassen und Eigenschaften werden mit XML-Elementen und Attributen verwiesen, und Links zwischen Markup und Code erstellt werden.

## <a name="creating-the-solution"></a>Erstellen der Projektmappe

Um zu beginnen, Ihre erste XAML-Datei bearbeiten, verwenden Sie Visual Studio oder Visual Studio für Mac, um eine neue Xamarin.Forms-Projektmappe zu erstellen. (Wählen Sie die Registerkarte unten für Ihre Umgebung ein.)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Verwenden Sie in Windows, Visual Studio auswählen **Datei > Neu > Projekt** aus dem Menü. In der **neues Projekt** wählen Sie im Dialogfeld **Visual C# > plattformübergreifend** auf der linken Seite, und klicken Sie dann **Mobile App (Xamarin.Forms)** aus der Liste im mittleren Bereich.

![](get-started-with-xaml-images/win/newprojectdialog.w157.png "Dialogfeld \"Neues Projekt\"")

Wählen Sie einen Speicherort für die Projektmappe aus, geben sie einen Namen der **XamlSamples** (oder was auch immer Sie bevorzugen), und drücken Sie die **OK**.

Wählen Sie auf dem nächsten Bildschirm die **leere App** Vorlage und die **.NET Standard** Strategie für die gemeinsame Verwendung von Code:

![](get-started-with-xaml-images/win/newcrossplatformapp.png "Dialogfeld \"neue App\"")

Drücken Sie **OK**.

Vier Projekte in der Projektmappe erstellt werden: die **XamlSamples** .NET Standard-Bibliothek, **XamlSamples.Android**, **XamlSamples.iOS**, und die universelle Windows-Plattform Lösung **XamlSamples.UWP**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wählen Sie in Visual Studio für Mac **Datei > neue Projektmappe** aus dem Menü. In der **neues Projekt** wählen Sie im Dialogfeld **Multi-Plattform > App** auf der linken Seite und **leere Forms-App** (*nicht* **Forms-App** ) aus der Vorlagenliste:

![](get-started-with-xaml-images/mac/newprojectdialog1.png "Neue Projektdialogfeld 1")

Drücken Sie **Weiter**.

Im nächsten Dialogfeld, geben Sie dem Projekt einen Namen für **XamlSamples** (oder was auch immer Sie bevorzugen). Stellen Sie sicher, dass die **verwenden .NET Standard** Optionsfeld ist aktiviert:

![](get-started-with-xaml-images/mac/newprojectdialog2.png "Der Dialogfeld \"Neues Projekt\" 2")

Drücken Sie **Weiter**.

Im folgenden Dialogfeld können Sie einen Speicherort für das Projekt auswählen:

![](get-started-with-xaml-images/mac/newprojectdialog3.png "Der Dialogfeld \"Neues Projekt\" 3")

Drücken Sie **erstellen**

Drei Projekte in der Projektmappe erstellt werden: die **XamlSamples** .NET Standard-Bibliothek, **XamlSamples.Android**, und **XamlSamples.iOS**.

-----

Nach dem Erstellen der **XamlSamples** Lösung möglicherweise möchten Sie Ihre Entwicklungsumgebung testen, indem Sie die verschiedenen Plattformprojekte als Startprojekt der Projektmappe auswählen, und erstellen und Bereitstellen von die einfache Anwendung erstellt die Projektvorlage auf echten Geräten oder Phone-Emulatoren.

Es sei denn, Sie zum Schreiben von plattformspezifischem Code, der gemeinsam verwendeten müssen **XamlSamples** .NET Standard Library-Projekt ist, in dem Sie praktisch alle der Programmierung Zeit zur Verfügung steht. In diesen Artikeln werden nicht außerhalb dieses Projekts müssten.

### <a name="anatomy-of-a-xaml-file"></a>Anatomie einer XAML-Datei

In der **XamlSamples** .NET Standard-Bibliothek sind ein Paar von Dateien mit den folgenden Namen:

- **"App.xaml"** , die XAML-Datei und
- **Datei "App.Xaml.cs"** , C# *CodeBehind* Datei, die der XAML-Datei zugeordnet.

Sie müssen auf den Pfeil neben klicken **"App.xaml"** zum Code-Behind-Datei finden Sie unter.

Beide **"App.xaml"** und **"App.Xaml.cs"** tragen zu einer Klasse namens `App` abgeleitet, die `Application`. Die meisten anderen Klassen mit XAML-Dateien auf eine abgeleitete Klasse beitragen `ContentPage`; diese Dateien, die XAML verwenden, um den visuellen Inhalt, der eine ganze Seite zu definieren. Dies gilt für die anderen beiden Dateien in die **XamlSamples** Projekt:

- **"MainPage.xaml"** , die XAML-Datei und
- **Datei "MainPage.Xaml.cs"** , C# Code-Behind-Datei.

Die **"MainPage.xaml"** Datei sieht folgendermaßen aus (obwohl die Formatierung ein wenig anders sein kann):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <StackLayout>
        <!-- Place new controls here -->
        <Label Text="Welcome to Xamarin Forms!"
               VerticalOptions="Center"
               HorizontalOptions="Center" />
    </StackLayout>

</ContentPage>
```

Die beiden XML-Namespace (`xmlns`) Deklarationen URIs, die erste scheinbar auf Xamarins-Website und die zweite in von Microsoft finden Sie unter. Sie einfach keinen, welchem Punkt die URIs zu überprüfen. Es ist nichts vorhanden. Sie einfach die URIs, die im Besitz von Xamarin und Microsoft sind, und sie funktionieren im Wesentlichen als Versions-IDs.

Die erste XML-Namespacedeklaration bedeutet, dass es sich bei Tags, die in der XAML-Datei ohne Präfix definiert z. B. Klassen, die in Xamarin.Forms finden Sie `ContentPage`. Die zweite Namespacedeklaration definiert ein Präfix des `x`. Hiermit wird für mehrere Elemente und Attribute, die für XAML systemintern sind selbst und die von anderen Implementierungen von XAML unterstützt werden. Allerdings sind diese Elemente und Attribute variieren, je nachdem das Jahr im URI eingebetteten. Xamarin.Forms wird unterstützt, die XAML 2009-Spezifikation, aber nicht für alle.

Die `local` Namespacedeklaration ermöglicht es Ihnen, andere Klassen von .NET Standard Library-Projekt zuzugreifen.

Am Ende des ersten Tags die `x` Präfix wird verwendet, für ein Attribut mit dem Namen `Class`. Da die Verwendung dieses `x` Präfix ist praktisch universelle für den XAML-Namespace, XAML-Attribute wie z. B. `Class` werden fast immer als bezeichnet `x:Class`.

Die `x:Class` Attribut gibt an, einen vollqualifizierten Namen der .NET-Klasse: die `MainPage` -Klasse in der `XamlSamples` Namespace. Dies bedeutet, dass diese XAML-Datei eine neue Klasse namens definiert `MainPage` in die `XamlSamples` Namespace, die von abgeleitet `ContentPage`– das Tag in dem die `x:Class` Attribut angezeigt wird.

Die `x:Class` Attribut kann nur verwendet werden, in das Stammelement einer XAML-Datei, definieren Sie eine abgeleitete C# Klasse. Dies ist das nur neue Klasse, die in der XAML-Datei definiert. Alles andere, die in der XAML-Datei angezeigt wird. wird stattdessen einfach aus vorhandenen Klassen instanziiert und initialisiert.

Die **"MainPage.Xaml.cs"** Datei sieht folgendermaßen aus (abgesehen von nicht verwendeten `using` Direktiven):

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

Die `MainPage` Klasse leitet sich von `ContentPage`, aber beachten Sie, dass die `partial` Definition der Klasse. Dies deutet darauf hin, dass es eine andere partielle Klassendefinition für muss `MainPage`, wobei es jedoch? Und was ist `InitializeComponent` Methode?

Wenn Visual Studio das Projekt erstellt wurde, analysiert er die XAML-Datei zum Generieren einer C# Codedatei. Wenn Sie, in Suchen der **XamlSamples\XamlSamples\obj\Debug** Verzeichnis finden Sie eine Datei namens **XamlSamples.MainPage.xaml.g.cs**. Die 'g' steht für generiert. Dies ist der anderen partiellen Klassendefinition der `MainPage` , enthält die Definition der `InitializeComponent` Methode mit dem Namen aus der `MainPage` Konstruktor. Diese beiden partiellen `MainPage` Klassendefinitionen können dann zusammen kompiliert werden. Je nachdem, ob der XAML oder nicht kompiliert wird wird der XAML-Datei oder in eine binäre Form der XAML-Datei in die ausführbare Datei eingebettet.

Zur Laufzeit code, in den Aufrufen der speziellen Plattform-Projekt eine `LoadApplication` -Methode, übergibt Sie an eine neue Instanz der der `App` Klasse in der .NET Standard-Bibliothek. Die `App` Klassenkonstruktor instanziiert `MainPage`. Ruft der Konstruktor der Klasse `InitializeComponent`, wodurch wiederum die `LoadFromXaml` -Methode, die die XAML-Datei (oder die kompilierte Binärdatei) aus der .NET Standard-Bibliothek extrahiert. `LoadFromXaml` Initialisiert die Objekte, die in der XAML-Datei definiert, verbindet diese alle zusammen in einer über-/ unterordnungsbeziehung, fügt der Ereignishandler definiert, die im Code, um Ereignisse, die in der XAML-Datei festgelegt und legt die resultierende Struktur der Objekte als Inhalt der Seite.

Obwohl Sie normalerweise verbringen viel Zeit mit der generierten Codedateien nicht benötigen, werden manchmal Runtime-Ausnahmen ausgelöst für Code in die generierten Dateien, damit Sie mit ihnen vertraut sein sollten.

Beim Kompilieren und dieses Programms ausführen die `Label` Element in der Mitte der Seite wird angezeigt, wie die XAML bereits vermuten lässt:

[![](get-started-with-xaml-images/xamlsamples.png "Default Xamarin.Forms Anzeige")](get-started-with-xaml-images/xamlsamples-large.png#lightbox "Default Xamarin.Forms-Anzeige")

Für weitere interessante Visualisierungen, ist alles, was Sie benötigen weitere interessante XAML.

## <a name="adding-new-xaml-pages"></a>Hinzufügen von neuen XAML-Seiten

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Hinzufügen von anderen XAML-basierte `ContentPage` Klassen zu Ihrem Projekt, wählen Sie die **XamlSamples** .NET Standard-Bibliothek Projekt, und rufen Sie die **Projekt > Neues Element hinzufügen** Menüelement. Am linken Rand der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C#**  und **Xamarin.Forms**. Wählen Sie aus der Liste **Inhaltsseite** (nicht **Inhaltsseite (C#)** , eine Seite nur mit Code erstellt oder **Ansicht "Inhalt"** , dies ist keiner Seite). Geben Sie der Seite einen Namen, z. B. **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.w157.png "Dialogfeld \"Neues Element\" hinzufügen")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Hinzufügen von anderen XAML-basierte `ContentPage` Klassen zu Ihrem Projekt, wählen Sie die **XamlSamples** .NET Standard-Bibliothek Projekt, und rufen Sie die **Datei > neue Datei** Menüelement. Am linken Rand der **neue Datei** wählen Sie im Dialogfeld **Forms** auf der linken Seite und **Forms-ContentPage Xaml** (nicht **Formular-ContentPage**, erstellt eine Seite nur mit Code oder **Ansicht "Inhalt"** , dies ist keiner Seite). Geben Sie der Seite einen Namen, z. B. **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "Dialogfeld \"neue Datei\"")

-----

Zwei Dateien werden hinzugefügt, um dem Projekt **HelloXamlPage.xaml** und Code-Behind-Datei **HelloXamlPage.xaml.cs**.

## <a name="setting-page-content"></a>Inhalt der Seite festlegen

Bearbeiten der **HelloXamlPage.xaml** Datei, sodass nur Tags für sind `ContentPage` und `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>

    </ContentPage.Content>
</ContentPage>
```

Die `ContentPage.Content` Tags sind Teil der eindeutigen Syntax des XAML. Auf den ersten sie ungültige XML-Daten zu sein scheinen, aber zulässig. Der Zeitraum ist keine Sonderzeichen in XML.

Die `ContentPage.Content` Tags heißen *Property-Element* Tags. `Content` ist eine Eigenschaft des `ContentPage`, und in der Regel auf eine einzelne Ansicht oder ein Layout mit untergeordneten Ansichten festgelegt ist. Eigenschaften werden normalerweise Attribute in XAML, aber es wäre schwierig, legen Sie eine `Content` -Attribut auf ein komplexes Objekt. Aus diesem Grund wird die Eigenschaft ein XML-Element, bestehend aus den Klassennamen und den Namen der Eigenschaft durch einen Punkt getrennt angegeben. Jetzt die `Content` Eigenschaft kann festgelegt werden, zwischen den `ContentPage.Content` Tags wie folgt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

Beachten Sie auch, dass eine `Title` Attribut für das Stamm-Tag festgelegt wurde.

Zu diesem Zeitpunkt sollten die Beziehung zwischen Klassen, Eigenschaften und XML-offensichtlich sein: Eine Xamarin.Forms-Klasse (z. B. `ContentPage` oder `Label`) in der XAML-Datei als ein XML-Element angezeigt wird. Eigenschaften dieser Klasse – einschließlich `Title` auf `ContentPage` und sieben Eigenschaften der `Label`– in der Regel als XML-Attribute angezeigt werden.

Viele Verknüpfungen vorhanden sein, um die Werte dieser Eigenschaften festzulegen. Einige Eigenschaften sind die grundlegenden Datentypen: Z. B. die `Title` und `Text` Eigenschaften sind vom Typ `String`, `Rotation` ist vom Typ `Double`, und `IsVisible` (d.h. `true` standardmäßig und wird hier nur zur Veranschaulichung festgelegt) ist vom Typ `Boolean`.

Die `HorizontalTextAlignment` Eigenschaft ist vom Typ `TextAlignment`, eine Enumeration. Für die Eigenschaft ein Enumerationstyp Supply benötigen Sie lediglich einen Elementnamen.

Für Eigenschaften komplexer Typen werden jedoch Konverter verwendet für die Analyse der XAML. Hierbei handelt es sich um Klassen in Xamarin.Forms verwenden, die abgeleitet `TypeConverter`. Viele öffentliche Klassen sind, andere dagegen nicht. Für diese bestimmte XAML-Datei spielen einige dieser Klassen eine Rolle hinter den Kulissen:

-  `LayoutOptionsConverter` für die `VerticalOptions` Eigenschaft
-  `FontSizeConverter` für die `FontSize` Eigenschaft
-  `ColorTypeConverter` für die `TextColor` Eigenschaft

Diese Konverter gesteuert, die die zulässige Syntax für die Einstellungen der Eigenschaft.

Die `ThicknessTypeConverter` können eine, zwei oder vier Ziffern, die durch Kommas getrennt behandeln. Wenn eine Zahl angegeben wird, gilt es alle vier Seiten. Mit zwei Zahlen die erste ist, links und rechts aufgefüllt, und die zweite oben und unten. Vier Zahlen sind in der Reihenfolge von links, oben, rechts und unten.

Die `LayoutOptionsConverter` können die Namen der öffentlichen statischen Felder des konvertieren die `LayoutOptions` Struktur zu Werten vom Typ `LayoutOptions`.

Die `FontSizeConverter` verarbeiten kann eine `NamedSize` Member oder einen numerischen Schriftgrad.

Die `ColorTypeConverter` akzeptiert den Namen der öffentliche statische Felder von der `Color` Struktur oder hexadezimalen RGB-Werte, mit oder ohne einen Alphakanal, vorangestellt werden, durch ein Nummernzeichen (#). Hier ist die Syntax ohne einen alpha-Kanal:

 `TextColor="#rrggbb"`

Jede der kleinen Buchstaben ist eine hexadezimale Ziffer. Hier ist, wie ein Alphakanal enthalten ist:

 `TextColor="#aarrggbb">`

Bedenken Sie für den Alphakanal, dass FF vollständig deckend ist und 00 vollständig transparent ist.

Zwei andere Formate können Sie nur eine einzelne hexadezimale steht für jeden Kanal angeben:

 `TextColor="#rgb"` `TextColor="#argb"`

In diesen Fällen wird die Ziffer wiederholt, um den Wert zu bilden. #CF3 ist z. B. die RGB-Farbe CC-FF-33.

## <a name="page-navigation"></a>Seitennavigation

Beim Ausführen der **XamlSamples** -Programm die `MainPage` wird angezeigt. Um die neuen `HelloXamlPage` können Sie entweder, die als Start-neue Seite in der **"App.Xaml.cs"** aus, oder navigieren Sie zu der neuen Seite von `MainPage`.

Um die Navigation zu implementieren, ändern Sie zunächst Code in die **"App.Xaml.cs"** Konstruktor, damit eine `NavigationPage` Objekt wird erstellt:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

In der **"MainPage.Xaml.cs"** -Konstruktor, erstellen Sie eine einfache `Button` und verwenden Sie den Ereignishandler zu navigieren `HelloXamlPage`:

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

Festlegen der `Content` -Eigenschaft der Seite ersetzt die Einstellung der `Content` Eigenschaft in der XAML-Datei. Wenn Sie kompilieren und die neue Version dieses Programms bereitstellen, wird eine Schaltfläche auf dem Bildschirm angezeigt. Drücken sie navigiert zum `HelloXamlPage`. So sieht die resultierende Seite auf iPhone, Android und UWP aus:

[![](get-started-with-xaml-images/helloxaml1.png "Bezeichnungstext gedreht")](get-started-with-xaml-images/helloxaml1-large.png#lightbox "Bezeichnungstext gedreht")

Können Sie zurück zu navigieren `MainPage` mithilfe der **< wieder** auf iOS, verwenden den linken Pfeil am oberen Rand der Seite oder unten auf der das Telefon unter Android, oder verwenden den linken Pfeil am oberen Rand der Seite auf Windows 10 auf die Schaltfläche.

Gerne Experimentieren mit der XAML für verschiedene Möglichkeiten zum Rendern der `Label`. Wenn Sie alle Unicode-Zeichen in den Text einbetten möchten, können Sie die standard-XML-Syntax. Z. B. um die Begrüßung in typografische Anführungszeichen zu versetzen, verwenden Sie:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Hier ist, wie es aussieht:

[![](get-started-with-xaml-images/helloxaml2.png "Drehen von Text mit Unicode-Zeichen")](get-started-with-xaml-images/helloxaml2-large.png#lightbox "gedreht Bezeichnungstext mit Unicode-Zeichen")

## <a name="xaml-and-code-interactions"></a>XAML und Codeinteraktionen

Die **HelloXamlPage** Beispiel enthält nur eine einzige `Label` auf der Seite, aber dies ist sehr ungewöhnlich. Die meisten `ContentPage` ableitungen Satz der `Content` Eigenschaft, um ein Layout einiger zu sortieren, z. B. eine `StackLayout`. Die `Children` Eigenschaft der `StackLayout` Typ definiert ist `IList<View>` , aber es ist eigentlich ein Objekt des Typs `ElementCollection<View>`, und die Sammlung mit mehreren Ansichten oder anderer Layouts aufgefüllt werden kann. In XAML werden diese über-/ unterordnungsbeziehung mit normalen XML-Hierarchie eingerichtet. Hier ist eine XAML-Datei für eine neue Seite mit dem Namen **XamlPlusCodePage**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Dieser XAML-Datei, die syntaktisch abgeschlossen ist, und was er sieht wie folgt aus:

[![](get-started-with-xaml-images/xamlpluscode1.png "Mehrere Steuerelemente auf einer Seite")](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox "mehrere Steuerelemente auf einer Seite")

Allerdings werden Sie wahrscheinlich, berücksichtigen Sie dieses Programm funktional fehlerhaft sein. Vielleicht die `Slider` sollte dazu führen, dass die `Label` um den aktuellen Wert anzuzeigen und die `Button` wahrscheinlich etwas in das Programm tun soll.

Wie Sie sehen werden [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), den Auftrag zum Anzeigen von einer `Slider` -Wert mithilfe einer `Label` vollständig in XAML verarbeitet werden können, um mit einer Datenbindung. Es ist jedoch hilfreich, zunächst den Code-Lösung finden Sie unter. Behandeln Sie trotzdem die `Button` klicken Sie auf jeden Fall ist Code erforderlich. Dies bedeutet, dass der Code-Behind-Datei für `XamlPlusCodePage` darf Handler für die `ValueChanged` Ereignis die `Slider` und `Clicked` Ereignis die `Button`. Fügen Sie sie aus:

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

Diese Ereignishandler müssen nicht öffentlich sein.

In der XAML-Datei die `Slider` und `Button` Tags müssen Attribute für die `ValueChanged` und `Clicked` Ereignisse, die diese Handler verweisen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

Beachten Sie, dass ein Ereignis einen Ereignishandler zuweisen derselben Syntax wie eine Eigenschaft einen Wert zuweisen.

Wenn der Handler für die `ValueChanged` Ereignis die `Slider` verwenden die `Label` um den aktuellen Wert anzuzeigen, muss der Handler für dieses Objekt von Code zu verweisen. Die `Label` benötigt einen Namen, der Standardwert ist die `x:Name` Attribut.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

Die `x` Präfix der `x:Name` Attribut gibt an, dass dieses Attribut systeminterne XAML ist.

Der Name, die Sie zuweisen, die `x:Name` Attribut hat die gleichen Regeln wie C# Variablennamen. Beispielsweise müssen sie mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Jetzt die `ValueChanged` Ereignishandler kann festlegen, die `Label` Anzeigen der neuen `Slider` Wert. Der neue Wert wird aus den Ereignisargumenten zur Verfügung:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Oder der Handler konnte erhalten die `Slider` -Objekt, das generiert dieses Ereignis aus der `sender` Argument und zum Abrufen der `Value` Eigenschaft aus, die:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Beim ersten des Programms ausführen die `Label` nicht angezeigt, die `Slider` bewertet werden, da die `ValueChanged` Ereignis noch nicht noch ausgelöst. Aber keine Änderungen an der `Slider` bewirkt, dass den Wert, der angezeigt werden:

[![](get-started-with-xaml-images/xamlpluscode2.png "Der Wert des Schiebereglers angezeigt")](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox "Wert des Schiebereglers angezeigt")

Jetzt für die `Button`. Wir simulieren nun, eine Antwort auf eine `Clicked` Ereignis eine Warnung mit dem `Text` der Schaltfläche. Der Ereignishandler kann sichere Umwandlung der `sender` Argument für eine `Button` , und klicken Sie dann auf seine Eigenschaften zugreifen:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Die Methode definiert ist, als `async` da die `DisplayAlert` Methode ist asynchron und sollte das Präfix der `await` -Operator, der zurückgegeben wird, wenn die Methode abgeschlossen wird. Da diese Methode ruft die `Button` Auslösen des Ereignisses aus der `sender` Arguments, der gleiche Handler kann für mehrere Schaltflächen verwendet werden.

Sie haben gesehen, dass ein Objekt, das in XAML definiert ein Ereignis ausgelöst werden kann, die in der CodeBehind-Datei verarbeitet wird und die Code-Behind-Datei ein Objekt, das in XAML, die mit dem Namen zugewiesen, die mit definierten zugreifen kann die `x:Name` Attribut. Hierbei handelt es sich um die zwei grundlegenden Verfahren, die Code und XAML zu interagieren.

Einige zusätzliche Erkenntnisse darüber, wie XAML Works abgeleitet werden können, anhand der neu generierten **XamlPlusCode.xaml.g.cs Datei**, die nun umfasst einen beliebigen Namen, die einer `x:Name` -Attribut als ein privates Feld. Hier ist eine vereinfachte Version der Datei ein:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Die Deklaration dieses Felds kann die Variable an einer beliebigen Stelle in frei verwendet werden die `XamlPlusCodePage` partiellen Klassendatei in Ihren Zuständigkeitsbereich. Zur Laufzeit wird das Feld zugewiesen, nachdem das XAML analysiert wurde. Dies bedeutet, dass die `valueLabel` Feld `null` bei der `XamlPlusCodePage` Konstruktor beginnt jedoch gültig, nachdem `InitializeComponent` aufgerufen wird.

Nach dem `InitializeComponent` gibt wieder an den Konstruktor zu steuern, haben die visuellen Elemente der Seite erstellt wurden, als ob sie instanziiert und im Code initialisiert wurde. Die XAML-Datei spielt Rolle nicht mehr in der Klasse. Sie können diese Objekte auf der Seite in einer Weise, die Sie, b. z. durch Hinzufügen von Ansichten zu bearbeiten der `StackLayout`, oder einer Einstellung der `Content` -Eigenschaft der Seite etwas anderes vollständig. Sie können "durchlaufen die Struktur" anhand der `Content` Eigenschaft der Seite und die Elemente in der `Children` Auflistungen von Layouts. Sie können Eigenschaften festlegen, in Sichten, die auf diese Weise zugegriffen oder zuweisen Ereignishandler dynamisch.

Sie können. Es ist Ihre Seite aus, und XAML ist nur ein Tool, um seinen Inhalt zu erstellen.

## <a name="summary"></a>Zusammenfassung

Mit dieser Einführung haben Sie gelernt, wie eine XAML-Datei und Codedatei in eine Klassendefinition beitragen, und wie die Dateien für XAML und Code interagieren. XAML gibt jedoch auch eine eigene eindeutige syntaktischen Funktionen, die sie auf eine sehr flexible Weise verwendet werden können. Sie können beginnen, untersuchen diese in [Teil 2. Essential XAML Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus einer Datenbindung zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
