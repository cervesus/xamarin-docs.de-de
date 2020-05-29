---
Title: "Teil 1. Bei den ersten Schritten mit XAML "Description:" in einer- Xamarin.Forms Anwendung wird XAML größtenteils zum Definieren des visuellen Inhalts einer Seite und zusammen mit einer Code Behind-Datei verwendet. "
ms. Prod: xamarin ms. assetid: 9073fa0e-bd5a-4492-8a93-54c466f 6edb9 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 09/30/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="part-1-getting-started-with-xaml"></a>Teil 1. Erste Schritte mit XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_In einer- Xamarin.Forms Anwendung wird XAML größtenteils zum Definieren des visuellen Inhalts einer Seite und zusammen mit einer c#-Code Behind-Datei verwendet._

Die Code-Behind-Datei bietet Code Unterstützung für das Markup. In kombinieren diese beiden Dateien zu einer neuen Klassendefinition, die untergeordnete Sichten und die Initialisierung von Eigenschaften enthält. In der XAML-Datei werden auf Klassen und Eigenschaften mit XML-Elementen und-Attributen verwiesen, und es werden Links zwischen Markup und Code erstellt.

## <a name="creating-the-solution"></a>Erstellen der Projektmappe

Um mit der Bearbeitung Ihrer ersten XAML-Datei zu beginnen, verwenden Sie Visual Studio oder Visual Studio für Mac, um eine neue Projekt Mappe zu erstellen Xamarin.Forms . (Wählen Sie die unten stehende Registerkarte aus, die Ihrer Umgebung entspricht.)

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Starten Sie in Windows Visual Studio 2019, und klicken Sie im Startfenster auf **Neues Projekt erstellen** , um ein neues Projekt zu erstellen:

![Neues projektmappenfenster](get-started-with-xaml-images/win/new-solution-2019.png)

Wählen Sie im Fenster **Neues Projekt erstellen** in der Dropdown-Dropdown-Dropdown-Datei den **Typ** **Mobil** aus, wählen **Sie die Vorlage** **Mobile App ( Xamarin.Forms )** aus, und klicken Sie auf die Schaltfläche

![Fenster "Neues Projekt"](get-started-with-xaml-images/win/new-project-2019.png)

Legen Sie im Fenster **Neues Projekt konfigurieren** den **Projektnamen** auf **xamlsamples** fest (oder was Sie bevorzugen), und klicken Sie auf die Schaltfläche **Erstellen** .

Klicken Sie im Dialogfeld **neue plattformübergreifende App** auf **leer**, und klicken Sie dann auf die Schaltfläche **OK** :

![Dialog Feld für neue APP](get-started-with-xaml-images/win/new-cross-platform-app.png)

In der Projekt Mappe werden vier Projekte erstellt: **XamlSamples** die xamlsamples .NET Standard Library, **xamlsamples. Android**, **xamlsamples. IOS**und die universelle Windows-Plattform Projekt Mappe **xamlsamples. UWP**.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Wählen Sie in Visual Studio für Mac im Menü **Datei > neue Projekt Mappe** aus. Wählen Sie im Dialogfeld **Neues Projekt** auf der linken Seite die Option **Multiplattform > App** und **leere Formular-App** (*keine* **Forms-App**) aus der Vorlagenliste aus:

![Dialog Feld "Neues Projekt"](get-started-with-xaml-images/mac/newprojectdialog1.png)

Klicken Sie auf **weiter**.

Benennen Sie im nächsten Dialogfeld das Projekt mit dem Namen " **xamlsamples** " (oder je nachdem, was Sie bevorzugen). Stellen Sie sicher, dass das Optionsfeld **use .NET Standard** ausgewählt ist:

![Dialog 2 für neues Projekt](get-started-with-xaml-images/mac/newprojectdialog2.png)

Klicken Sie auf **weiter**.

Im folgenden Dialogfeld können Sie einen Speicherort für das Projekt auswählen:

![Dialog 3 für neues Projekt](get-started-with-xaml-images/mac/newprojectdialog3.png)

Auf **Erstellen** klicken

In der Lösung werden drei Projekte erstellt: die **xamlsamples** .NET Standard Library, **xamlsamples. Android**und **xamlsamples. IOS**.

-----

Nachdem Sie die **xamlsamples** -Projekt Mappe erstellt haben, möchten Sie möglicherweise Ihre Entwicklungsumgebung testen, indem Sie die verschiedenen Platt Form Projekte als projektmappenstartprojekt auswählen und die einfache Anwendung, die von der Projektvorlage erstellt wurde, entweder auf Telefon Emulatoren oder echten Geräten erstellen und bereitstellen.

Wenn Sie keinen plattformspezifischen Code schreiben müssen, ist das freigegebene **xamlsamples** -.NET Standard Bibliotheksprojekt, in dem Sie praktisch die gesamte Programmierzeit ausgeben. Diese Artikel sind außerhalb des Projekts nicht vertraut.

### <a name="anatomy-of-a-xaml-file"></a>Anatomie einer XAML-Datei

In der **xamlsamples** -.NET Standard Bibliothek handelt es sich um ein paar von Dateien mit den folgenden Namen:

- **App. XAML**, die XAML-Datei immer
- **App.XAML.cs**, eine c# *-Code Behind-* Datei, die der XAML-Datei zugeordnet ist.

Sie müssen auf den Pfeil neben " **app. XAML** " klicken, um die Code Behind-Datei anzuzeigen.

Sowohl **app. XAML** als auch **app.XAML.cs** tragen zu einer Klasse mit dem Namen bei `App` , die von abgeleitet wird `Application` . Die meisten anderen Klassen mit XAML-Dateien tragen zu einer Klasse bei, die von abgeleitet wird `ContentPage` . diese Dateien verwenden XAML, um den visuellen Inhalt einer gesamten Seite zu definieren. Dies gilt für die anderen beiden Dateien im **xamlsamples** -Projekt:

- **MainPage. XAML**, die XAML-Datei immer
- **MainPage.XAML.cs**, die c#-Code Behind-Datei.

Die Datei " **MainPage. XAML** " sieht wie folgt aus (obwohl die Formatierung etwas unterschiedlich sein kann):

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

Die beiden Deklarationen von XML-Namespaces ( `xmlns` ) verweisen auf URIs, das erste scheinbar auf der xamarin-Website und das zweite auf den von Microsoft. Überprüfen Sie nicht, auf welche URIs diese URIs verweisen. Es gibt nichts. Sie sind einfach URIs im Besitz von xamarin und Microsoft und fungieren im Grunde als Versions Bezeichner.

Die erste XML-Namespace Deklaration bedeutet, dass Tags, die in der XAML-Datei ohne Präfix definiert Xamarin.Forms sind, z. b. auf Klassen in verweisen `ContentPage` . Die zweite Namespace Deklaration definiert ein Präfix von `x` . Diese wird für mehrere Elemente und Attribute verwendet, die in XAML selbst integriert sind und von anderen XAML-Implementierungen unterstützt werden. Diese Elemente und Attribute unterscheiden sich jedoch geringfügig von dem Jahr, das in den URI eingebettet ist. Xamarin.Formsunterstützt die 2009 XAML-Spezifikation, aber nicht alle.

Die `local` Namespace Deklaration ermöglicht den Zugriff auf andere Klassen aus dem .NET Standard Bibliotheksprojekt.

Am Ende dieses ersten Tags `x` wird das Präfix für ein Attribut mit dem Namen verwendet `Class` . Da die Verwendung dieses `x` Präfixes für den XAML-Namespace praktisch universell ist, werden XAML-Attribute wie `Class` fast immer als bezeichnet `x:Class` .

Das- `x:Class` Attribut gibt einen voll qualifizierten .net-Klassennamen an: die- `MainPage` Klasse im- `XamlSamples` Namespace. Dies bedeutet, dass diese XAML-Datei eine neue Klasse `MainPage` mit dem Namen in dem `XamlSamples` Namespace definiert, der von abgeleitet wird `ContentPage` – das Tag, in dem das `x:Class` Attribut angezeigt wird.

Das- `x:Class` Attribut kann nur im Root-Element einer XAML-Datei angezeigt werden, um eine abgeleitete c#-Klasse zu definieren. Dies ist die einzige neue Klasse, die in der XAML-Datei definiert ist. Alle anderen Elemente, die in der XAML-Datei angezeigt werden, werden stattdessen einfach aus vorhandenen Klassen instanziiert und initialisiert.

Die **MainPage.XAML.cs** -Datei sieht wie folgt aus (abgesehen von nicht verwendeten `using` Direktiven):

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

Die- `MainPage` Klasse wird von abgeleitet `ContentPage` , aber beachten Sie die `partial` Klassendefinition. Dies deutet darauf hin, dass eine andere partielle Klassendefinition für vorhanden sein sollte `MainPage` , aber wo ist Sie? Und was ist diese `InitializeComponent` Methode?

Wenn das Projekt in Visual Studio erstellt wird, wird die XAML-Datei analysiert, um eine c#-Codedatei zu generieren. Wenn Sie das Verzeichnis " **xamlsamples\xamlsamples\obj\debug** " betrachten, finden Sie eine Datei mit dem Namen **XamlSamples.MainPage.XAML.g.cs**. Das "g" steht für "generiert". Dies ist die andere partielle Klassendefinition von `MainPage` , die die Definition der Methode enthält, die `InitializeComponent` vom `MainPage` Konstruktor aufgerufen wird. Diese beiden partiellen `MainPage` Klassendefinitionen können dann zusammen kompiliert werden. Abhängig davon, ob der XAML-Code kompiliert wird, wird entweder die XAML-Datei oder eine binäre Form der XAML-Datei in die ausführbare Datei eingebettet.

Zur Laufzeit ruft der Code im bestimmten Platt Form Projekt eine- `LoadApplication` Methode auf und übergibt ihr eine neue Instanz der- `App` Klasse in der .NET Standard-Bibliothek. Der `App` Klassenkonstruktor instanziiert `MainPage` . Der Konstruktor dieser Klasse ruft auf `InitializeComponent` und ruft dann die-Methode auf, die `LoadFromXaml` die XAML-Datei (oder die kompilierte Binärdatei) aus der .NET Standard Bibliothek extrahiert. `LoadFromXaml`Initialisiert alle in der XAML-Datei definierten-Objekte, verbindet Sie alle in über-und untergeordneten Beziehungen, fügt Ereignishandler, die im Code definiert sind, an Ereignisse an, die in der XAML-Datei festgelegt sind, und legt die resultierende Struktur von Objekten als Inhalt der Seite fest.

Obwohl Sie normalerweise nicht viel Zeit mit generierten Code Dateien aufwenden müssen, werden manchmal Lauf Zeit Ausnahmen für Code in den generierten Dateien ausgelöst, sodass Sie mit Ihnen vertraut sein sollten.

Wenn Sie dieses Programm kompilieren und ausführen, wird das `Label` Element in der Mitte der Seite angezeigt, wie die XAML vorschlägt:

[![Standard Xamarin.Forms Anzeige](get-started-with-xaml-images/xamlsamples.png)](get-started-with-xaml-images/xamlsamples-large.png#lightbox)

Für interessantere Visualisierungen benötigen Sie lediglich eine interessantere XAML-Datei.

## <a name="adding-new-xaml-pages"></a>Hinzufügen von neuen XAML-Seiten

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Wenn Sie Ihrem Projekt andere XAML-basierte Klassen hinzufügen möchten `ContentPage` , wählen Sie das Projekt **xamlsamples** .NET Standard Library aus, klicken Sie mit der rechten Maustaste, und wählen Sie **> neues Element hinzufügen...** aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Visual c#-Elemente > Xamarin.Forms > Inhaltsseite** (keine **Inhaltsseite (c#)** aus, die eine Code basierte Seite oder eine **Inhaltsansicht**erstellt, die keine Seite ist). Geben Sie der Seite einen Namen, z. b. **helloxamlpage**:

![Dialog Feld Neues Element hinzufügen](get-started-with-xaml-images/win/add-new-item-dialog-2019.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie Ihrem Projekt andere XAML-basierte Klassen hinzufügen möchten `ContentPage` , wählen Sie das Projekt **xamlsamples** .NET Standard Library aus, und rufen Sie die **Datei > Menü Element neue Datei** auf. Klicken Sie links neben dem Dialogfeld **neue Datei** auf der linken Seite auf **Formulare** und **Formular ContentPage XAML** (keine **Formular-ContentPage**, die eine Code basierte Seite oder eine **Inhaltsansicht**erstellt, die keine Seite ist). Geben Sie der Seite einen Namen, z. b. **helloxamlpage**:

![Dialogfeld „Neue Datei“](get-started-with-xaml-images/mac/newfiledialog.png)

-----

Dem Projekt werden zwei Dateien hinzugefügt: **helloxamlpage. XAML** und die Code-Behind-Datei **HelloXamlPage.XAML.cs**.

## <a name="setting-page-content"></a>Festlegen von Seiten Inhalt

Bearbeiten Sie die Datei " **helloxamlpage. XAML** ", sodass die einzigen Tags für `ContentPage` und sind `ContentPage.Content` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>

    </ContentPage.Content>
</ContentPage>
```

Die `ContentPage.Content` Tags sind Teil der eindeutigen Syntax von XAML. Zunächst erscheint dies möglicherweise als ungültiges XML, aber Sie sind zulässig. Der Zeitraum ist kein Sonderzeichen in XML.

Die `ContentPage.Content` Tags werden als *Eigenschaften Element* Tags bezeichnet. `Content`ist eine Eigenschaft von `ContentPage` und ist in der Regel auf eine einzelne Ansicht oder ein Layout mit untergeordneten Ansichten festgelegt. Normalerweise werden Eigenschaften zu Attributen in XAML, aber es wäre schwierig, ein `Content` Attribut auf ein komplexes Objekt festzulegen. Aus diesem Grund wird die-Eigenschaft als XML-Element ausgedrückt, das aus dem Klassennamen und dem Eigenschaftsnamen besteht, die durch einen Zeitraum getrennt sind. Nun kann die- `Content` Eigenschaft zwischen den Tags wie folgt festgelegt werden `ContentPage.Content` :

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

Beachten Sie auch, dass ein- `Title` Attribut für das Stammtag festgelegt wurde.

Zu diesem Zeitpunkt sollte die Beziehung zwischen Klassen, Eigenschaften und XML ersichtlich sein: eine Xamarin.Forms Klasse (z. b. `ContentPage` oder `Label` ) wird in der XAML-Datei als XML-Element angezeigt. Eigenschaften dieser Klasse – einschließlich `Title` on `ContentPage` und sieben Eigenschaften von `Label` – werden normalerweise als XML-Attribute angezeigt.

Viele Verknüpfungen sind vorhanden, um die Werte dieser Eigenschaften festzulegen. Einige Eigenschaften sind grundlegende Datentypen: die-Eigenschaft und die-Eigenschaft sind z. b. vom Typ, `Title` `Text` `String` `Rotation` ist `Double` vom Typ, und `IsVisible` ( `true` Standardmäßig wird hier nur zur Veranschaulichung festgelegt) ist vom Typ `Boolean` .

Die-Eigenschaft weist den `HorizontalTextAlignment` Typ auf `TextAlignment` , bei dem es sich um eine Enumeration handelt. Für eine Eigenschaft eines beliebigen Enumerationstyps müssen Sie lediglich einen Elementnamen angeben.

Für Eigenschaften komplexer Typen werden jedoch Konverter für die XAML-Verarbeitung verwendet. Dies sind Klassen in Xamarin.Forms , die von abgeleitet sind `TypeConverter` . Viele sind öffentliche Klassen, andere jedoch nicht. Für diese bestimmte XAML-Datei spielen einige dieser Klassen eine Rolle hinter den Kulissen:

- `LayoutOptionsConverter`für die `VerticalOptions` Eigenschaft
- `FontSizeConverter`für die `FontSize` Eigenschaft
- `ColorTypeConverter`für die `TextColor` Eigenschaft

Diese Konverter steuern die zulässige Syntax der Eigenschafts Einstellungen.

Der `ThicknessTypeConverter` kann eine, zwei oder vier Zahlen, die durch Kommas getrennt sind, verarbeiten. Wenn eine Zahl angegeben wird, gilt sie für alle vier Seiten. Bei zwei Zahlen sind die ersten Links und die Rechte Auffüll Zeichen, und der zweite Wert ist oben und unten. Vier Zahlen liegen in der Reihenfolge Links, oben, rechts und unten.

Der `LayoutOptionsConverter` kann die Namen der öffentlichen statischen Felder der- `LayoutOptions` Struktur in Werte vom Typ konvertieren `LayoutOptions` .

Der `FontSizeConverter` kann einen `NamedSize` Member oder eine numerische Schriftgröße verarbeiten.

Der `ColorTypeConverter` akzeptiert die Namen öffentlicher statischer Felder der `Color` Struktur oder Hexadezimal-RGB-Werte mit oder ohne Alphakanal, dem ein Nummern Zeichen (#) vorangestellt ist. Hier ist die Syntax ohne einen Alphakanal:

 `TextColor="#rrggbb"`

Bei jedem der kleinen Buchstaben handelt es sich um eine hexadezimale Ziffer. Es folgt ein Alphakanal:

 `TextColor="#aarrggbb">`

Beachten Sie für den Alphakanal, dass FF vollständig deckend und 00 vollständig transparent ist.

Mit zwei anderen Formaten können Sie nur eine einzelne hexadezimal Ziffer für jeden Kanal angeben:

 `TextColor="#rgb"` `TextColor="#argb"`

In diesen Fällen wird die Ziffer wiederholt, um den Wert zu bilden. #CF3 ist beispielsweise die RGB-Farbe CC-FF-33.

## <a name="page-navigation"></a>Seitennavigation

Wenn Sie das **xamlsamples** -Programm ausführen, `MainPage` wird das angezeigt. Um den neuen anzuzeigen `HelloXamlPage` , können Sie diesen entweder als neue Startseite in der **app.XAML.cs** -Datei festlegen, oder Sie können von zur neuen Seite navigieren `MainPage` .

Um die Navigation zu implementieren, ändern Sie zuerst Code im **app.XAML.cs** -Konstruktor, sodass ein- `NavigationPage` Objekt erstellt wird:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

Im **MainPage.XAML.cs** -Konstruktor können Sie eine einfache Erstellen `Button` und den-Ereignishandler verwenden, um zu zu navigieren `HelloXamlPage` :

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

Durch Festlegen der- `Content` Eigenschaft der Seite wird die Einstellung der- `Content` Eigenschaft in der XAML-Datei ersetzt. Wenn Sie die neue Version dieses Programms kompilieren und bereitstellen, wird auf dem Bildschirm eine Schaltfläche angezeigt. Das Drücken der IT navigiert zu `HelloXamlPage` . Dies ist die resultierende Seite auf iPhone, Android und UWP:

[![Bezeichnungs Text gedreht](get-started-with-xaml-images/helloxaml1.png)](get-started-with-xaml-images/helloxaml1-large.png#lightbox)

Sie können zurück zur `MainPage` Schaltfläche zurückkehren, indem Sie die Schaltfläche **zurück<** auf IOS verwenden. verwenden Sie dazu den Pfeil nach links oben auf der Seite oder am unteren Rand des Telefons unter Android, oder verwenden Sie den Pfeil nach links am oberen Rand der Seite unter Windows 10.

Sie können mit dem XAML experimentieren, um unterschiedliche Möglichkeiten zum renderenden zu haben `Label` . Wenn Sie Unicode-Zeichen in den Text einbetten müssen, können Sie die Standard-XML-Syntax verwenden. Wenn Sie z. b. den Gruß in Smarttags einfügen möchten, verwenden Sie Folgendes:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Dies sieht wie folgt aus:

[![Bezeichnungs Text mit Unicode-Zeichen gedreht](get-started-with-xaml-images/helloxaml2.png)](get-started-with-xaml-images/helloxaml2-large.png#lightbox)

## <a name="xaml-and-code-interactions"></a>XAML-und Code Interaktionen

Das **helloxamlpage** -Beispiel enthält nur ein einzelnes `Label` auf der Seite, aber dies ist sehr ungewöhnlich. Bei den meisten `ContentPage` Ableitungen wird die- `Content` Eigenschaft auf ein Layout einer bestimmten Art festgelegt, z `StackLayout` . b.. Die `Children` -Eigenschaft der `StackLayout` ist so definiert, dass Sie vom Typ ist `IList<View>` , aber tatsächlich ein Objekt vom Typ ist `ElementCollection<View>` . diese Auflistung kann mit mehreren Sichten oder anderen Layouts aufgefüllt werden. In XAML werden diese Beziehungen zwischen übergeordneten und untergeordneten Elementen mit der normalen XML-Hierarchie hergestellt. Im folgenden finden Sie eine XAML-Datei für eine neue Seite mit dem Namen **xamlpluscodepage**:

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

Diese XAML-Datei ist syntaktisch vollständig und sieht wie folgt aus:

[![Mehrere Steuerelemente auf einer Seite](get-started-with-xaml-images/xamlpluscode1.png)](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox)

Allerdings ist es wahrscheinlich, dass dieses Programm funktionell unzureichend ist. Vielleicht `Slider` soll das bewirken, dass den `Label` aktuellen Wert anzeigt, und das `Button` ist wahrscheinlich für die Ausführung von etwas innerhalb des Programms vorgesehen.

Wie Sie in [Teil 4 sehen werden. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md): die Aufgabe der Anzeige eines `Slider` Werts mithilfe eines `Label` kann vollständig in XAML mit einer Datenbindung behandelt werden. Es ist jedoch hilfreich, zuerst die Code Lösung anzuzeigen. Auch wenn Sie den Klick bearbeiten, `Button` erfordert dies definitiv Code. Dies bedeutet, dass die Code-Behind-Datei für `XamlPlusCodePage` Handler für das `ValueChanged` -Ereignis von `Slider` und das- `Clicked` Ereignis von enthalten muss `Button` . Fügen wir Sie hinzu:

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

In der XAML-Datei müssen die `Slider` -und- `Button` Tags Attribute für das `ValueChanged` -Ereignis und das-Ereignis einschließen, `Clicked` die auf diese Handler verweisen:

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

Beachten Sie, dass das Zuweisen eines Handlers zu einem Ereignis dieselbe Syntax aufweist wie das Zuweisen eines Werts zu einer Eigenschaft.

Wenn der Handler für das- `ValueChanged` Ereignis von das `Slider` verwendet, `Label` um den aktuellen Wert anzuzeigen, muss der Handler vom Code auf dieses Objekt verweisen. `Label`Benötigt einen Namen, der mit dem-Attribut angegeben wird `x:Name` .

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

Das `x` Präfix des `x:Name` Attributs gibt an, dass dieses Attribut in XAML intrinsisch ist.

Der Name, den Sie dem `x:Name` Attribut zuweisen, hat dieselben Regeln wie c#-Variablennamen. Er muss z. b. mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Nun `ValueChanged` kann der Ereignishandler das Festlegen `Label` , um den neuen `Slider` Wert anzuzeigen. Der neue Wert ist in den Ereignis Argumenten verfügbar:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Oder der Handler kann das Objekt, `Slider` das dieses Ereignis erzeugt, aus dem `sender` -Argument abrufen und die- `Value` Eigenschaft von diesem abrufen:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Wenn Sie das Programm zum ersten Mal ausführen, wird der `Label` Wert von nicht angezeigt, `Slider` da das `ValueChanged` Ereignis noch nicht ausgelöst wurde. Allerdings bewirkt jede Bearbeitung von `Slider` , dass der Wert angezeigt wird:

[![Schieberegler-Wert angezeigt](get-started-with-xaml-images/xamlpluscode2.png)](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox)

Jetzt für das `Button` . Wir simulieren eine Antwort auf ein- `Clicked` Ereignis, indem wir eine Warnung mit der `Text` der Schaltfläche anzeigen. Der Ereignishandler kann das `sender` -Argument auf sichere Weise in einen umwandeln `Button` und dann auf dessen Eigenschaften zugreifen:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Die-Methode wird als definiert `async` , da die `DisplayAlert` -Methode asynchron ist und dem- `await` Operator vorangestellt wird, der zurückgibt, wenn die-Methode abgeschlossen ist. Da diese Methode das Auslösen `Button` des-Ereignisses aus dem- `sender` Argument erhält, kann derselbe Handler für mehrere Schaltflächen verwendet werden.

Sie haben gesehen, dass ein in XAML definiertes Objekt ein Ereignis auslösen kann, das in der Code-Behind-Datei behandelt wird, und dass die Code-Behind-Datei auf ein Objekt zugreifen kann, das in XAML unter Verwendung des zugewiesenen namens mit dem-Attribut definiert ist `x:Name` . Dabei handelt es sich um die beiden grundlegenden Methoden zum Interagieren von Code und XAML.

Weitere Einblicke in die Funktionsweise von XAML finden Sie unter Untersuchen der neu generierten **XamlPlusCode.XAML.g.cs-Datei**, die jetzt alle Namen enthält, die einem beliebigen `x:Name` Attribut als privates Feld zugewiesen sind. Im folgenden finden Sie eine vereinfachte Version dieser Datei:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Die Deklaration dieses Felds ermöglicht die freie Verwendung der Variablen an beliebiger Stelle innerhalb der `XamlPlusCodePage` partiellen Klassendatei in ihrem Zuständigkeitsbereich. Zur Laufzeit wird das Feld zugewiesen, nachdem das XAML analysiert wurde. Dies bedeutet, dass das `valueLabel` Feld ist, `null` Wenn der `XamlPlusCodePage` Konstruktor beginnt, aber gültig ist, nachdem `InitializeComponent` aufgerufen wurde.

Nachdem `InitializeComponent` die Steuerung zurück an den Konstruktor zurückgegeben hat, wurden die visuellen Elemente der Seite so erstellt, als ob Sie im Code instanziiert und initialisiert worden wären. Die XAML-Datei spielt keine Rolle mehr in der Klasse. Sie können diese Objekte auf der Seite auf beliebige Weise ändern, z. b. durch Hinzufügen von Ansichten zu `StackLayout` oder durch Festlegen der- `Content` Eigenschaft der Seite auf etwas anderes. Sie können die Struktur durchlaufen, indem Sie die `Content` -Eigenschaft der Seite und die Elemente in den Auflistungen `Children` von Layouts untersuchen. Sie können Eigenschaften für Sichten festlegen, auf die auf diese Weise zugegriffen wird, oder Ihnen Ereignishandler dynamisch zuweisen.

Das ist kostenlos. Es ist Ihre Seite, und XAML ist nur ein Tool zum Erstellen des Inhalts.

## <a name="summary"></a>Zusammenfassung

Mit dieser Einführung haben Sie gesehen, wie eine XAML-Datei und eine Codedatei zu einer Klassendefinition beitragen und wie die XAML-und Code Dateien interagieren. XAML verfügt jedoch auch über eigene, einzigartige syntaktische Features, die eine sehr flexible Verwendung ermöglichen. Sie können diese in [Teil 2 untersuchen. Wichtige XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 2. Wichtige XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3: XAML-Markup Erweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Von der Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
