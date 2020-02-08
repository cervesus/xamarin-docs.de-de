---
title: Laden von XAML zur Laufzeit in xamarin. Forms
description: XAML kann mithilfe der loadfromxaml-Erweiterungs Methoden zur Laufzeit geladen und analysiert werden.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: 71f510cd37d4bed2a5a6077ed63f748ce9894725
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045070"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Laden von XAML zur Laufzeit in xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

Der [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml) -Namespace enthält zwei [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Erweiterungs Methoden, die zum Laden und Analysieren von XAML zur Laufzeit verwendet werden können.

## <a name="background"></a>Hintergrund

Wenn eine xamarin. Forms-XAML-Klasse erstellt wird, wird die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) -Methode indirekt aufgerufen. Dies liegt daran, dass die Code-Behind-Datei für eine XAML-Klasse die `InitializeComponent`-Methode von Ihrem Konstruktor aufruft:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Wenn Visual Studio ein Projekt erstellt, das eine XAML-Datei enthält, wird die XAML-Datei analysiert C# , um eine Codedatei (z. b. **MainPage.XAML.g.cs**) zu generieren, die die Definition der `InitializeComponent` Methode enthält:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

Die `InitializeComponent`-Methode ruft die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) -Methode auf, um die XAML-Datei (oder die kompilierte Binärdatei) aus der .NET Standard-Bibliothek zu extrahieren. Nach dem Extrahieren werden alle in der XAML-Datei definierten Objekte initialisiert, alle in über-/Unterordnungsbeziehungen miteinander verknüpft, im Code definierte Ereignishandler an in der XAML-Datei festgelegte Ereignisse angefügt und die resultierende Struktur von Objekten als Inhalt des s.

## <a name="loading-xaml-at-runtime"></a>Laden von XAML zur Laufzeit

Die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methoden sind `public`und können daher von xamarin. Forms-Anwendungen aufgerufen werden, um XAML zur Laufzeit zu laden und zu analysieren. Dies ermöglicht Szenarien wie z. b. eine Anwendung, die XAML von einem Webdienst herunterlädt, die erforderliche Ansicht aus dem XAML erstellt und in der Anwendung anzeigt.

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

In diesem Beispiel wird eine [`Button`](xref:Xamarin.Forms.Button) Instanz erstellt, deren [`Text`](xref:Xamarin.Forms.Button.Text) -Eigenschafts Wert aus der im `string`definierten XAML festgelegt wird. Der `Button` wird dann zu einer [`StackLayout`](xref:Xamarin.Forms.StackLayout) hinzugefügt, die im XAML für die Seite definiert wurde.

> [!NOTE]
> Mit den [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) -Erweiterungs Methoden kann ein generisches Typargument angegeben werden. Es ist jedoch nur selten notwendig, das Typargument anzugeben, da es vom Typ der Instanz abgeleitet wird, auf der es sich befindet.

Die [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) -Methode kann verwendet werden, um beliebige XAML-Code aufzufüllen. im folgenden Beispiel wird ein [`ContentPage`](xref:Xamarin.Forms.ContentPage) vergrößert und dann zu diesem navigiert:

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>Zugreifen auf Elemente

Das Laden von XAML zur Laufzeit mit der [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) -Methode ermöglicht keinen stark typisierten Zugriff auf die XAML-Elemente, die über die Namen der Lauf Zeit Objekte verfügen (mit `x:Name`). Diese XAML-Elemente können jedoch mit der [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) -Methode abgerufen und dann nach Bedarf aufgerufen werden:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

In diesem Beispiel wird der XAML-Code für eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) aufgeblasen. Dieser XAML-Code enthält eine [`Label`](xref:Xamarin.Forms.Label) mit dem Namen `monkeyName`, die mit der [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) -Methode abgerufen wird, bevor die zugehörige [`Text`](xref:Xamarin.Forms.Label.Text) -Eigenschaft festgelegt wird.

## <a name="related-links"></a>Verwandte Links

- [Loadruntimexaml (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
