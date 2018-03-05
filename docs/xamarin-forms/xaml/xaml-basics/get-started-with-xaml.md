---
title: Teil 1. Erste Schritte mit XAML
description: "In einer Anwendung Xamarin.Forms ist XAML hauptsächlich den visuellen Inhalt einer Seite definieren. Eine XAML-Datei ist immer eine C#-Codedatei, die Code-Unterstützung für das Markup bietet zugeordnet. Tragen diese beiden Dateien zusammen, für die Klassendefinition einer neuen, die untergeordnete Ansichten und eigenschaftsinitialisierung enthält. In der XAML-Datei Klassen und Eigenschaften mit XML-Elementen und Attributen verwiesen, und Links zwischen Markup und Code hergestellt werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 8e02dbd8687fc10582874710db7ca6848f546751
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="part-1-getting-started-with-xaml"></a>Teil 1. Erste Schritte mit XAML

_In einer Anwendung Xamarin.Forms ist XAML hauptsächlich den visuellen Inhalt einer Seite definieren. Eine XAML-Datei ist immer eine C#-Codedatei, die Code-Unterstützung für das Markup bietet zugeordnet. Tragen diese beiden Dateien zusammen, für die Klassendefinition einer neuen, die untergeordnete Ansichten und eigenschaftsinitialisierung enthält. In der XAML-Datei Klassen und Eigenschaften mit XML-Elementen und Attributen verwiesen, und Links zwischen Markup und Code hergestellt werden._

## <a name="creating-the-solution"></a>Erstellen der Projektmappe

Um zu beginnen, Ihre erste Verwendung von XAML-Datei bearbeiten, verwenden Sie Visual Studio oder Visual Studio für Mac zum Erstellen einer neuen Xamarin.Forms-Projektmappe ein. (Wählen Sie die Registerkarte am oberen Rand dieser Seite für Ihre Umgebung ein.)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Verwenden Sie in Windows, Visual Studio, wählen Sie **Datei > Neu > Projekt** aus dem Menü. In der **neues Projekt** wählen Sie im Dialogfeld **Visual c# > plattformübergreifend** auf der linken Seite, und klicken Sie dann **Cross-Plattform-App (Xamarin.Forms oder systemeigen)** aus der Liste im mittleren Bereich. 

![](get-started-with-xaml-images/win/newprojectdialog.png "Dialogfeld "Neues Projekt"")

Wählen Sie einen Speicherort für die Projektmappe, geben sie einen Namen der **XamlSamples** (oder alle von Ihnen gewünschten gewünscht), und drücken Sie die **OK**.

Wählen Sie auf dem nächsten Bildschirm die **leere App** Vorlage, die **Xamarin.Forms** UI-Technologie und die **Portable Klassenbibliothek (PCL)** Codefreigabe Strategie:

![](get-started-with-xaml-images/win/newcrossplatformapp.png "Dialogfeld "neue App"")

Press **OK**. 

Vier Projekte in der Projektmappe erstellt werden: die **XamlSamples** portable Klassenbibliothek (PCL), **XamlSamples.Android**, **XamlSamples.iOS**, und die universelle Windows -Plattformlösung, **XamlSamples.UWP**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wählen Sie in Visual Studio für Mac **Datei > Neues Projektmappen** aus dem Menü. In der **neues Projekt** wählen Sie im Dialogfeld **mehrere Plattformen > App** auf der linken Seite und **leere Forms App** (*nicht* **Forms-App** ) aus der Vorlagenliste:

![](get-started-with-xaml-images/mac/newprojectdialog1.png "Der Dialogfeld "Neues Projekt" 1")

Drücken Sie **Weiter**.

Klicken Sie im Dialogfeld "Weiter" benennen Sie dem Projekt der **XamlSamples** (oder alle von Ihnen gewünschten gewünscht). Stellen Sie sicher, dass die **verwenden Portable Class Library** Optionsfeld ausgewählt ist, und dass **verwenden XAML für Schnittstelle Benutzerdateien** aktiviert ist:

![](get-started-with-xaml-images/mac/newprojectdialog2.png "Der Dialogfeld "Neues Projekt" 2")

Drücken Sie **Weiter**. 

Im folgenden Dialogfeld können Sie einen Speicherort für das Projekt auswählen:

![](get-started-with-xaml-images/mac/newprojectdialog3.png "Der Dialogfeld "Neues Projekt" 3")

Drücken Sie **erstellen**

In der Lösung werden drei Projekte erstellt: die **XamlSamples** portable Klassenbibliothek (PCL), **XamlSamples.Android**, und **XamlSamples.iOS**. 

-----

Nach dem Erstellen der **XamlSamples** Lösung möglicherweise Ihre Entwicklungsumgebung zu testen, indem Sie die verschiedenen Plattformprojekte als lösungsstartprojekt auswählen möchten, und erstellen und Bereitstellen die einfache Anwendung erstellt die Projektvorlage für Phone-Emulatoren oder echten Geräten.

Es sei denn, Sie plattformspezifischen Code, die gemeinsam verwendete schreiben müssen **XamlSamples** PCL-Projekt ist, in denen Sie nahezu alle Arbeitszeit Programmierung zur Verfügung steht. In diesen Artikeln werden außerhalb dieses Projekt nicht textbezogene.

### <a name="anatomy-of-a-xaml-file"></a>Aufbau einer XAML-Datei

Innerhalb der **XamlSamples** portable Klassenbibliothek sind ein Paar von Dateien mit den folgenden Namen:

- **App.XAML**, XAML-Datei ein, und
- **App.Xaml.cs**, C#- *CodeBehind* Datei die XAML-Datei zugeordnet.

Sie müssen auf den Pfeil neben klicken **App.xaml** zum Code-Behind-Datei finden Sie unter. 

Beide **App.xaml** und **App.xaml.cs** tragen zu einer Klasse mit dem Namen `App` abgeleitet, die `Application`. Die meisten anderen Klassen mit XAML-Dateien auf eine Klasse, die abgeleitet beitragen `ContentPage`; diese Dateien, die XAML verwenden, um den visuellen Inhalt eine gesamte Seite definieren. Dies ist "true", die anderen zwei Dateien in den **XamlSamples** Projekt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **"MainPage.xaml"**, XAML-Datei ein, und
- **"MainPage.Xaml.cs"**, die C#-Code-Behind-Datei.

Die **"MainPage.xaml"** Datei sieht wie folgt aus:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

- **XamlSamplesPage.xaml**, XAML-Datei ein, und
- **XamlSamplesPage.xaml.cs**, die C#-Code-Behind-Datei.

Die **XamlSamplesPage.xaml** Datei sieht wie folgt aus:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
             xmlns:local="clr-namespace:XamlSamples" 
             x:Class="XamlSamples.XamlSamplesPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

-----

Die beiden XML-Namespace ( `xmlns`) Deklarationen auf URIs, die erste scheinbar auf die Xamarin Website und das zweite auf Microsofts verweisen. Kümmern Sie nicht, welchem Punkt die URIs zu überprüfen. Es ist nichts vorhanden. Sie sind lediglich die URIs, die im Besitz von Xamarin und Microsoft, und im Grunde als Versionsbezeichner funktionieren.

Die erste XML-Namespacedeklaration bedeutet, Tags, die in der XAML-Datei ohne Präfix definiert z. B. auf Klassen in Xamarin.Forms, verweisen `ContentPage`. Die zweite Namespacedeklaration definiert ein Präfix des `x`. Hiermit wird für mehrere Elemente und Attribute, die in XAML systemintern sind selbst und die von anderen Implementierungen von XAML unterstützt werden. Diese Elemente und Attribute sind jedoch geringfügig je nach Jahr im URI eingebettet. Xamarin.Forms unterstützt 2009 XAML-Spezifikation, aber nicht alle Daten.

Die `local` Namespacedeklaration können Sie andere Klassen zugreifen, indem Sie das PCL-Projekt.

Am Ende der ersten Tags die `x` -Präfix wird für ein Attribut mit dem Namen `Class`. Da die Verwendung dieses `x` Präfix ist praktisch universal XAML-Namespace in XAML-Attributen wie z. B. `Class` werden fast immer als bezeichnet `x:Class`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Die `x:Class` Attribut gibt an, einen vollqualifizierten Klassennamen von .NET: die `MainPage` -Klasse in der `XamlSamples` Namespace. Dies bedeutet, dass diese XAML-Datei eine neue Klasse mit dem Namen definiert `MainPage` in der `XamlSamples` Namespace, die abgeleitet `ContentPage`– das Tag in dem die `x:Class` Attribut angezeigt wird.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die `x:Class` Attribut gibt an, einen vollqualifizierten Klassennamen von .NET: die `XamlSamplesPage` -Klasse in der `XamlSamples` Namespace. Dies bedeutet, dass diese XAML-Datei eine neue Klasse mit dem Namen definiert `XamlSamplesPage` in der `XamlSamples` Namespace, die abgeleitet `ContentPage`– das Tag in dem die `x:Class` Attribut angezeigt wird.

-----

Die `x:Class` Attribut kann nur im Stammelement einer XAML-Datei zum Definieren einer abgeleiteten Klasse c# verwendet werden. Dies ist nur neue Klasse in der XAML-Datei definiert. Alle anderen Funktionen, die in der XAML-Datei angezeigt wird stattdessen einfach aus vorhandenen Klassen instanziiert und initialisiert.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Die **"MainPage.Xaml.cs"** Datei sieht wie folgt (abgesehen von nicht verwendeten `using` Direktiven):

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

Die `MainPage` Klasse abgeleitet `ContentPage`, aber beachten Sie die `partial` Klassendefinition. Dadurch ergibt sich, dass es eine andere partielle Klassendefinition für darf `MainPage`, aber es? Und was ist `InitializeComponent` Methode? 

Wenn Visual Studio das Projekt erstellt wurde, analysiert er die XAML-Datei eine C#-Codedatei generiert. Wenn Sie, in Suchen der **XamlSamples\XamlSamples\obj\Debug** Verzeichnis finden Sie eine Datei namens **XamlSamples.MainPage.xaml.g.cs**. Das "g" steht für generiert. Dies ist die andere partielle Klassendefinition `MainPage` , enthält die Definition der `InitializeComponent` Methode mit dem Namen aus der `MainPage` Konstruktor. Diese beiden partiellen `MainPage` Klassendefinitionen können dann zusammen kompiliert werden. Je nachdem, ob der XAML-Code kompiliert wird wird die Verwendung von XAML-Datei oder eine binäre Form eines XAML-Datei in die ausführbare Datei eingebettet.

Während der Laufzeit code, in dem bestimmte Plattform Projekt Aufrufen einer `LoadApplication` -Methode, eine neue Instanz der an sie übergibt die `App` Klasse in der PCL. Die `App` -Klassenkonstruktor instanziiert `MainPage`. Der Konstruktor dieser Klasse ruft `InitializeComponent`, wodurch wiederum die `LoadFromXaml` -Methode, die die Verwendung von XAML-Datei (oder der kompilierten Binärdatei) aus der PCL extrahiert. `LoadFromXaml` Initialisiert die Objekte, die in der XAML-Datei definiert, verbindet diese zusammen in Parent-Child-Beziehungen, fügt der Ereignishandler im Code auf Ereignisse, die in der XAML-Datei festgelegt definiert und legt die sich ergebende Struktur von Objekten als Inhalt der Seite.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die **XamlSamplesPage.xaml.cs** Datei sieht wie folgt aus:

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class XamlSamplesPage : ContentPage
    {
        public XamlSamplesPage()
        {
            InitializeComponent();
        }
    }
}
```

Die `XamlSamplesPage` Klasse abgeleitet `ContentPage`, aber beachten Sie die `partial` Klassendefinition. Es wird vorgeschlagen, muss eine andere C#-Datei mit einer anderen partiellen Klassendefinition für `XamlSamplesPage`, aber es? Und was ist `InitializeComponent` Methode?

Wenn Visual Studio für Mac das Projekt erstellt wurde, analysiert er die XAML-Datei eine C#-Codedatei generiert. Wenn Sie, in Suchen der **XamlSamples\XamlSamples\obj\Debug** Verzeichnis finden Sie eine Datei namens **XamlSamples.XamlSamplesPage.xaml.g.cs**. Das "g" steht für generiert. Dies ist die andere partielle Klassendefinition `XamlSamplesPage` , enthält die Definition der `InitializeComponent` Methode mit dem Namen aus der `XamlSamplesPage` Konstruktor.  Diese beiden partiellen `XamlSamplesPage` Klassendefinitionen können dann zusammen kompiliert werden. Je nachdem, ob der XAML-Code kompiliert wird wird die Verwendung von XAML-Datei oder eine binäre Form eines XAML-Datei in die ausführbare Datei eingebettet.

Während der Laufzeit code, in dem bestimmte Plattform Projekt Aufrufen einer `LoadApplication` -Methode, eine neue Instanz der an sie übergibt die `App` Klasse in der PCL. Die `App` -Klassenkonstruktor instanziiert `XamlSamplesPage`. Der Konstruktor dieser Klasse ruft `InitializeComponent`, wodurch wiederum die `LoadFromXaml` -Methode, die die Verwendung von XAML-Datei (oder der kompilierten Binärdatei) aus der PCL extrahiert. `LoadFromXaml` Initialisiert die Objekte, die in der XAML-Datei definiert, verbindet diese zusammen in Parent-Child-Beziehungen, fügt der Ereignishandler im Code auf Ereignisse, die in der XAML-Datei festgelegt definiert und legt die sich ergebende Struktur von Objekten als Inhalt der Seite.

-----

Obwohl Sie normalerweise benötigen zu viel Zeit mit generierten Codedateien, werden manchmal Runtime-Ausnahmen ausgelöst für Code in die generierten Dateien, damit Sie mit diesen vertraut sein sollten.

Beim Kompilieren und dieses Programms ausführen die `Label` Element wird in der Mitte der Seite, wie nahe legt, der XAML-Code führt. Drei sind von links nach rechts die folgenden Plattformen iOS-, Android- und Windows 10 Mobile:

[![](get-started-with-xaml-images/xamlsamples.png "Standard Xamarin.Forms Anzeige")](get-started-with-xaml-images/xamlsamples-large.png "Xamarin.Forms standardmäßig anzeigen")

Für weitere interessante Visualisierungen, müssen Sie, lediglich weitere interessante XAML.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="preliminaries"></a>Vorbereitende Maßnahmen

Benennen Sie um Dateinamen in Visual Studio für Mac konsistent mit den Dateien, die von Visual Studio unter Windows ausgeführte erstellte machen, **XamlSamplesPage.xaml** auf **"MainPage.xaml"**, und  **XamlSamplesPage.xaml.cs** auf **"MainPage.Xaml.cs"**. Innerhalb der **XamlSamplesPage.xaml** Datei, ändern Sie `XamlSamplesPage` auf `MainPage`. Innerhalb der **XamlSamplesPage.xaml.cs** Datei, ändern Sie die zwei Vorkommen des `XamlSamplesPage` auf `MainPage`. Innerhalb der **App.xaml.cs** Datei, ändern Sie die Anweisung

```csharp
MainPage = new XamlSamplesPage();
```

in:

```csharp
MainPage = new MainPage();
```

-----

Test, der das Programm dennoch kompiliert und bereitgestellt werden, bevor Sie fortfahren.

## <a name="adding-new-xaml-pages"></a>Hinzufügen von neuen XAML-Seiten

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Andere XAML-basierte hinzuzufügende `ContentPage` Klassen zum Projekt, wählen Sie die **XamlSamples** PCL Projekt, und rufen die **Projekt > Neues Element hinzufügen** Menüelement. Am linken Rand der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C#-** und **Xamarin.Forms**. Wählen Sie aus der Liste **Inhaltsseite** (nicht **Inhaltsseite (c#)**, die eine Seite reinen erstellt oder **Inhaltsansicht**, eine Seite ist). Benennen Sie der Seite, z. B. **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.png "Dialogfeld "Neues Element" hinzufügen")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Andere XAML-basierte hinzuzufügende `ContentPage` Klassen zum Projekt, wählen Sie die **XamlSamples** PCL Projekt, und rufen die **Datei > neue Datei** Menüelement. Am linken Rand der **neue Datei** wählen Sie im Dialogfeld **Forms** auf der linken Seite und **Forms ContentPage verwendet Xaml** (nicht **Forms ContentPage verwendet**, welche erstellt eine Seite reinen oder **Inhaltsansicht**, eine Seite ist). Benennen Sie der Seite, z. B. **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "Dialogfeld "neue Datei"")

-----

Zwei Dateien werden dem Projekt hinzugefügt **HelloXamlPage.xaml** und der Code-Behind-Datei **HelloXamlPage.xaml.cs**. 

## <a name="setting-page-content"></a>Festlegen der Seiteninhalt

Bearbeiten der **HelloXamlPage.xaml** Datei, sodass nur Tags sind für `ContentPage` und `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

Die `ContentPage.Content` Tags sind Teil der eindeutig von XAML-Syntax. Zunächst erscheinen ungültige XML-Datei handeln, aber sie sind zulässig. Der Zeitraum ist nicht in XML ein Sonderzeichen.

Die `ContentPage.Content` Tags heißen *Eigenschaftselement* Tags. `Content` ist eine Eigenschaft des `ContentPage`, und in der Regel auf eine einzige Ansicht oder einem Layout mit untergeordneten Ansichten festgelegt ist. Normalerweise werden Eigenschaften, Attribute in XAML, aber wäre es schwierig, legen Sie eine `Content` -Attribut auf ein komplexes Objekt. Aus diesem Grund wird die Eigenschaft als XML-Element besteht aus den Klassennamen und den Eigenschaftennamen durch einen Punkt getrennt angegeben werden. Jetzt die `Content` Eigenschaft kann festgelegt werden, zwischen den `ContentPage.Content` Tags wie folgt:

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

Beachten Sie auch, dass ein `Title` Attribut für Stammtag festgelegt wurde.

Zu diesem Zeitpunkt sollten die Beziehung zwischen Klassen, Eigenschaften und XML-offensichtlich sein: ein Xamarin.Forms-Klasse (z. B. `ContentPage` oder `Label`) wird in der XAML-Datei als XML-Element angezeigt. Die Eigenschaften dieser Klasse – einschließlich `Title` auf `ContentPage` und sieben Eigenschaften der `Label`– in der Regel als XML-Attribute angezeigt werden.

Viele Tastenkombinationen vorhanden sein, um die Werte dieser Eigenschaften festzulegen. Einige Eigenschaften sind die grundlegenden Datentypen: Z. B. die `Title` und `Text` Eigenschaften sind vom Typ `String`, `Rotation` ist vom Typ `Double`, und `IsVisible` (also `true` standardmäßig festgelegt und ist hier nur Abbildung) ist vom Typ `Boolean`.

Die `HorizontalTextAlignment` -Eigenschaft ist vom Typ `TextAlignment`, eine Enumeration. Für eine Eigenschaft eines beliebigen Enumerationstyps benötigen Sie geben lediglich einen Elementnamen.

Für Eigenschaften komplexer Typen werden jedoch Typkonverter verwendet für die XAML-Analyse. Hierbei handelt es sich um Klassen in Xamarin.Forms, die davon Herleiten `TypeConverter`. Viele der öffentlichen Klassen sind, andere dagegen nicht. Für diese Verwendung von XAML-Datei spielen einige dieser Klassen eine Rolle im Hintergrund:

-  `LayoutOptionsConverter` für die `VerticalOptions` Eigenschaft
-  `FontSizeConverter` für die `FontSize` Eigenschaft
-  `ColorTypeConverter` für die `TextColor` Eigenschaft

Dieser Konverter gesteuert, die die zulässige Syntax der eigenschaftseinstellungen.

Die `ThicknessTypeConverter` eine, zwei oder vier Ziffern, die durch Kommas getrennt behandeln können. Wenn eine Zahl angegeben wird, gilt sie allen vier Seiten. Mit zwei Zahlen die erste ist die linken und rechten Abstand, und das zweite ist, oberen und unteren Rand. Vier Ziffern werden in der Reihenfolge von links, oben, rechts und unten.

Die `LayoutOptionsConverter` können die Namen der öffentlichen statischen Felder der konvertieren die `LayoutOptions` Struktur zu Werten vom Typ `LayoutOptions`.

Die `FontSizeConverter` verarbeiten kann eine `NamedSize` Member oder einen numerischen Schriftgrad.

Die `ColorTypeConverter` akzeptiert den Namen der öffentlichen statischen Felder der der `Color` Struktur oder hexadezimalen RGB-Werte, mit oder ohne einen alpha-Kanal vorangestellt ein Nummernzeichen (#). So sieht die Syntax ohne einen alpha-Kanal aus:

 `TextColor="#rrggbb"`

Jede der kleinen Buchstaben ist eine hexadezimale Ziffer. So sieht wie ein alpha-Kanal enthalten ist:

 `TextColor="#aarrggbb">`

Sollten Sie für den Alphakanal bedenken, dass FF vollständig deckend und 00 vollständig transparent ist.

Zwei andere Formate können Sie nur eine hexadezimale Ziffer für jeden Kanal angeben:

 `TextColor="#rgb"` `TextColor="#argb"`

In diesen Fällen wird die Ziffer wiederholt, um den Wert zu bilden. #CF3 ist beispielsweise der RGB-Farbe CC-FF-33.

## <a name="page-navigation"></a>Seitennavigation

Beim Ausführen der **XamlSamples** Programm, das `MainPage` wird angezeigt. Um die neue finden Sie unter `HelloXamlPage` können Sie festlegen, die als neue Start-Seite in der **App.xaml.cs** -Datei ein, oder navigieren Sie zu der neuen Seite aus `MainPage`.

Ändern Sie zum Implementieren der Navigation zuerst Code in der **App.xaml.cs** Konstruktor, damit ein `NavigationPage` Objekt wird erstellt:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

In der **"MainPage.Xaml.cs"** Konstruktor, erstellen Sie eine einfache `Button` und verwenden Sie den Ereignishandler zu navigieren `HelloXamlPage`:

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

Festlegen der `Content` Eigenschaft von der Seite "ersetzt die Einstellung von der `Content` Eigenschaft in der XAML-Datei. Beim Kompilieren und die neue Version dieses Programms bereitstellen, wird eine Schaltfläche auf dem Bildschirm angezeigt. Drücken sie navigiert zum `HelloXamlPage`. Hier ist die resultierende Seite auf iPhone, Android und Windows 10 Mobile Geräte:

[ ![](get-started-with-xaml-images/helloxaml1.png "Bezeichnungstext gedreht")](get-started-with-xaml-images/helloxaml1-large.png "Bezeichnungstext gedreht")

Navigieren Sie Sie, zurück zu `MainPage` mithilfe der **< zurück** Schaltfläche iOS, verwenden Sie den linken Pfeil am oberen Rand der Seite oder am unteren Rand des Telefons auf Android-Geräten oder der nach-links-Taste am unteren Rand der Seite auf Windows 10 Mobile.

Gerne zum Experimentieren mit der XAML-Code für verschiedene Möglichkeiten zum Rendern der `Label`. Wenn keine Unicode-Zeichen in den Text eingebettet werden sollen, können Sie die standard-XML-Syntax. Angenommen, um die Grußformel in typografische einzufügen, zu verwenden:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Hier ist, wie es aussieht:

[ ![](get-started-with-xaml-images/helloxaml2.png "Drehen von Text mit Unicode-Zeichen")](get-started-with-xaml-images/helloxaml2-large.png "gedreht Bezeichnungstext mit Unicode-Zeichen")

## <a name="xaml-and-code-interactions"></a>XAML und Codeinteraktionen

Die **HelloXamlPage** Beispiel enthält nur einen einzigen `Label` auf der Seite ", aber dies ist sehr ungewöhnlich. Die meisten `ContentPage` ableitungen Satz der `Content` Eigenschaft zu einem Layout einiger zu sortieren, z. B. eine `StackLayout`. Die `Children` Eigenschaft von der `StackLayout` Datentyp definiert ist `IList<View>` aber tatsächlich auf ein Objekt des Typs `ElementCollection<View>`, und dass die Auflistung mit mehreren Sichten oder andere Tastaturlayouts stimmen aufgefüllt werden kann. In XAML werden diese über-/ unterordnungsbeziehungen mit normalen XML-Hierarchie eingerichtet. Hier ist eine XAML-Datei für eine neue Seite mit dem Namen **XamlPlusCodePage**:

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

Diese Verwendung von XAML-Datei ist syntaktisch abgeschlossen, und was er sieht folgendermaßen aus:

[ ![](get-started-with-xaml-images/xamlpluscode1.png "Mehrere Steuerelemente auf einer Seite")](get-started-with-xaml-images/xamlpluscode1-large.png "mehrere Steuerelemente auf einer Seite")

Allerdings werden Sie wahrscheinlich Installation dieses Programms funktional ungenügend werden berücksichtigt. Vielleicht die `Slider` soll dazu führen, dass die `Label` zur Anzeige des aktuellen Werts und die `Button` richtet sich wahrscheinlich eine Aktion innerhalb des Programms.

Wie Sie sehen werden [Teil 4. Data Binding-Grundlagen](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), den Auftrag zum Anzeigen von einer `Slider` -Wert mit einer `Label` können mit einer Datenbindung vollständig in XAML verarbeitet werden. Es ist jedoch hilfreich, zunächst den Code-Lösung finden Sie unter. Allerdings behandeln die `Button` auf definitiv Code erfordert. Dies bedeutet, dass der Code-Behind-Datei für `XamlPlusCodePage` darf Handler für das `ValueChanged` -Ereignis für die `Slider` und die `Clicked` -Ereignis für die `Button`. Fügen Sie sie aus:

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

Wieder in der XAML-Datei die `Slider` und `Button` Tags müssen Attribute für die `ValueChanged` und `Clicked` Ereignisse, die diese Handler zu verweisen:

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

Beachten Sie, dass ein Ereignis einen Ereignishandler zuweisen derselben Syntax wie das Zuweisen eines Werts zu einer Eigenschaft.

Wenn der Handler für das `ValueChanged` -Ereignis für die `Slider` wird die `Label` um den aktuellen Wert anzuzeigen, muss der Handler aus Code auf das Objekt verweisen. Die `Label` benötigt einen Namen, der Standardwert ist die `x:Name` Attribut.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

Die `x` Präfix der `x:Name` Attribut gibt an, dass dieses Attribut in XAML intrinsischen handelt.

Der Name, die Sie zum Zuweisen der `x:Name` Attribut hat die gleichen Regeln wie c# Variablennamen. Er muss z. B. mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Jetzt die `ValueChanged` Ereignishandler festlegen, kann die `Label` Anzeigen der neuen `Slider` Wert. Der neue Wert wird über die Ereignisargumente verfügbar:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Oder der Handler konnte erhalten die `Slider` -Objekt, das generiert wird, dass Sie dieses Ereignis aus der `sender` Argument und erhalten die `Value` Eigenschaft aus, die:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Beim Ausführen des Programms die `Label` wird nicht angezeigt, die `Slider` bewertet werden, da die `ValueChanged` Ereignis wurde nicht noch ausgelöst. Aber keine Änderungen an der `Slider` bewirkt, dass den Wert, der angezeigt werden:

[ ![](get-started-with-xaml-images/xamlpluscode2.png "Schiebereglerwert angezeigt")](get-started-with-xaml-images/xamlpluscode2-large.png "angezeigten Schiebereglerwert")

Jetzt für die `Button`. Wir simulieren Sie eine Antwort auf eine `Clicked` Ereignis über eine Warnung mit dem `Text` der Schaltfläche. Der Ereignishandler kann problemlos umgewandelt. die `sender` Argument an einen `Button` , und klicken Sie dann auf seine Eigenschaften zugreifen:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Die Methode definiert ist, als `async` da die `DisplayAlert` Methode ist asynchron und eingeleitet werden soll, mit der `await` -Operator, der zurückgegeben wird, wenn die Methode abgeschlossen hat. Da diese Methode ruft die `Button` Auslösen des Ereignisses aus der `sender` Argument der gleiche Handler kann für mehrere Schaltflächen verwendet werden.

Sie haben gesehen, dass ein Objekt, das in XAML definiert ein Ereignis ausgelöst werden kann, die in der Code-Behind-Datei behandelt wird, und die Code-Behind-Datei ein Objekt, das in XAML, die mit dem Namen mit zugewiesenen definierten zugreifen kann die `x:Name` Attribut. Dies sind die zwei grundlegenden Methoden, die Code und XAML zu interagieren.

Einige zusätzliche Erkenntnisse wie für die Verwendung von XAML-Works abgeleitet werden können, durch Untersuchen der neu generierten **XamlPlusCode.xaml.g.cs Datei**, die nun umfasst einen beliebigen Namen zugewiesen, `x:Name` Attribut als ein privates Feld. Hier ist eine vereinfachte Version der Datei ein:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Die Deklaration dieses Felds kann die Variable an einer beliebigen Stelle in beliebig verwendet werden die `XamlPlusCodePage` partiellen Klassendatei in Ihre Zuständigkeit. Zur Laufzeit wird das Feld zugewiesen, nachdem der XAML-Code analysiert wurde. Dies bedeutet, dass die `valueLabel` Feld `null` bei der `XamlPlusCodePage` Konstruktor beginnt jedoch gültig, nachdem `InitializeComponent` aufgerufen wird.

Nach dem `InitializeComponent` gibt wieder an den Konstruktor zu steuern, als ob sie instanziiert und initialisiert, die im Code wurde die visuellen Elemente der Seite erstellt wurden. Die XAML-Datei spielt Rolle nicht mehr in der Klasse. Können Sie diese Objekte auf der Seite in keiner Weise, die Sie, b. z. durch Hinzufügen von Ansichten zum Ändern der `StackLayout`, oder die Einstellung der `Content` -Eigenschaft der Seite ganz vollständig. Sie können "die Struktur durchlaufen" durch Untersuchen der `Content` Eigenschaft und die Elemente in der `Children` Auflistungen von Layouts. Sie können Eigenschaften für Sichten, die auf diese Weise zugegriffen oder zuweisen Ereignishandler dynamisch.

Gerne. Es wird die Seite, und XAML ist nur ein Tool, mit dessen Inhalt erstellen.

## <a name="summary"></a>Zusammenfassung

Mit dieser Einführung haben Sie gesehen, wie ein XAML-Datei und die Codedatei für die Klassendefinition einer beitragen, und wie die Dateien XAML und generierter Code interagieren. XAML gibt jedoch auch einen eigenen eindeutigen syntaktischen Funktionen, mit die sie eine sehr flexible Weise verwendet werden können. Können Sie anfangen, untersuchen diese in [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus dem Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
