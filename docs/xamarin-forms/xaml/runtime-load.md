---
Title: "Laden von XAML zur Laufzeit in Xamarin.Forms " Beschreibung: "XAML kann mit den loadfromxaml-Erweiterungs Methoden zur Laufzeit geladen und analysiert werden."
ms. Prod: xamarin ms. assetid: 25F 73bbf -2dd3-468e-a2d8-0897414f 0F 4a ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 12/12/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Laden von XAML zur Laufzeit inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

Der- [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml) Namespace enthält zwei [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Erweiterungs Methoden, die zum Laden und Analysieren von XAML zur Laufzeit verwendet werden können.

## <a name="background"></a>Hintergrund

Wenn eine Xamarin.Forms XAML-Klasse erstellt wird, [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) wird die-Methode indirekt aufgerufen. Dies liegt daran, dass die Code-Behind-Datei für eine XAML-Klasse die- `InitializeComponent` Methode von Ihrem Konstruktor aufruft:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Wenn Visual Studio ein Projekt erstellt, das eine XAML-Datei enthält, wird die XAML-Datei analysiert, um eine c#-Codedatei (z. b. **MainPage.XAML.g.cs**) zu generieren, die die Definition der `InitializeComponent` Methode enthält:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

Die- `InitializeComponent` Methode ruft die- [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methode auf, um die XAML-Datei (oder die kompilierte Binärdatei) aus der .NET Standard Bibliothek zu extrahieren. Nach dem Extrahieren werden alle Objekte initialisiert, die in der XAML-Datei definiert sind, die alle in über-/Unterordnungsbeziehungen miteinander verknüpft, im Code definierte Ereignishandler an in der XAML-Datei festgelegte Ereignisse anfügt und die resultierende Struktur von Objekten als Inhalt der Seite festgelegt.

## <a name="loading-xaml-at-runtime"></a>Laden von XAML zur Laufzeit

Die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methoden sind `public` und können daher von Anwendungen aufgerufen werden, Xamarin.Forms um Sie zu laden und XAML zur Laufzeit zu analysieren. Dies ermöglicht Szenarien wie z. b. eine Anwendung, die XAML von einem Webdienst herunterlädt, die erforderliche Ansicht aus dem XAML erstellt und in der Anwendung anzeigt.

> [!WARNING]
> Das Laden von XAML zur Laufzeit hat einen erheblichen Leistungs Aufwand und sollte im allgemeinen vermieden werden.

Das folgende Codebeispiel zeigt eine einfache Verwendung:

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

In diesem Beispiel wird eine- [`Button`](xref:Xamarin.Forms.Button) Instanz erstellt, deren- [`Text`](xref:Xamarin.Forms.Button.Text) Eigenschafts Wert aus der in definierten XAML festgelegt wird `string` . `Button`Wird dann zu einem hinzugefügt [`StackLayout`](xref:Xamarin.Forms.StackLayout) , das im XAML für die Seite definiert wurde.

> [!NOTE]
> Die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Erweiterungs Methoden ermöglichen das Angeben eines generischen Typarguments. Es ist jedoch nur selten notwendig, das Typargument anzugeben, da es vom Typ der Instanz abgeleitet wird, auf der es sich befindet.

Die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) -Methode kann verwendet werden, um beliebige XAML-Code aufzufüllen. das folgende Beispiel vergrößert ein [`ContentPage`](xref:Xamarin.Forms.ContentPage) und navigiert dann zu diesem:

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>Zugreifen auf Elemente

Das Laden von XAML zur Laufzeit mit der- [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methode ermöglicht keinen stark typisierten Zugriff auf die XAML-Elemente, die über angegebene Laufzeitobjekt Namen verfügen (mithilfe von `x:Name` ). Diese XAML-Elemente können jedoch mithilfe der [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) -Methode abgerufen und dann nach Bedarf aufgerufen werden:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

In diesem Beispiel wird der XAML-Code für eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) aufgeblasen. Dieser XAML-Code enthält einen [`Label`](xref:Xamarin.Forms.Label) `monkeyName` mit dem Namen, der mithilfe der-Methode abgerufen wird [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) , bevor seine- [`Text`](xref:Xamarin.Forms.Label.Text) Eigenschaft festgelegt wird.

## <a name="related-links"></a>Verwandte Links

- [Loadruntimexaml (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
