---
title: Laden von XAML zur Laufzeit in Xamarin.Forms
description: XAML geladen, und zur Laufzeit mit der Erweiterungsmethoden LoadFromXaml analysiert werden kann.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: ce8ba32a1a6a1f69033615558c7ebf15d41e70fe
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2018
ms.locfileid: "53814098"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Laden von XAML zur Laufzeit in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)

Die [ `Xamarin.Forms.Xaml` ](xref:Xamarin.Forms.Xaml) Namespace enthält zwei [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Erweiterungsmethoden, die zum Laden verwendet werden können, und Analysieren von XAML zur Laufzeit.

## <a name="background"></a>Hintergrund

Wenn eine Xamarin.Forms-XAML-Klasse erstellt wird, die [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) indirekt aufgerufen. Dies tritt auf, die Code-Behind-Datei für eine XAML--Klasse ruft die `InitializeComponent` Methode aus seinem Konstruktor:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Wenn Visual Studio ein Projekt mit einer XAML-Datei erstellt wird, analysiert er die XAML-Datei zum Generieren einer C# -Codedatei (z. B. **MainPage.xaml.g.cs**), enthält die Definition der `InitializeComponent` Methode:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

Die `InitializeComponent` Methodenaufrufe der [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methode, um die XAML-Datei (oder die kompilierte Binärdatei) aus der .NET Standard-Bibliothek zu extrahieren. Nach dem Extrahieren initialisiert alle Objekte in der XAML-Datei definiert, verbindet diese alle zusammen in einer über-/ unterordnungsbeziehung, fügt der Ereignishandler definiert, die im Code, um Ereignisse, die in der XAML-Datei festgelegt und legt die resultierende Struktur der Objekte, die den Inhalt der Seite ".

## <a name="loading-xaml-at-runtime"></a>Laden von XAML zur Laufzeit

Die [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methoden sind `public`, und daher von Xamarin.Forms-Anwendungen beim Laden aufgerufen werden können, und analysieren Sie XAML zur Laufzeit. Dies ermöglicht Szenarien, z. B. eine Anwendung mit dem Herunterladen von XAML aus einem Webdienst, der die gewünschte Ansicht aus der XAML erstellt und in der Anwendung anzuzeigen.

> [!WARNING]
> Laden von XAML zur Laufzeit hat erhebliche Leistungseinbußen und in der Regel sollte vermieden werden kann.

Das folgende Codebeispiel zeigt eine einfache Verwendung:

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

In diesem Beispiel eine [ `Button` ](xref:Xamarin.Forms.Button) Instanz wird erstellt, mit dessen [ `Text` ](xref:Xamarin.Forms.Button.Text) Eigenschaftswert festgelegt wird, aus dem XAML definiert wird, der `string`. Die `Button` wird dann hinzugefügt, um eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , die in der XAML für die Seite definiert wurde.

> [!NOTE]
> Die [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Erweiterungsmethoden ermöglichen ein generisches Typargument angegeben werden. Es ist jedoch nur selten notwendig, geben Sie das Typargument, da er ist abgeleitet aus dem Typ der Instanz der arbeiten.

Die [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methode kann verwendet werden, um die Vergrößerung von jeder XAML, mit dem folgenden Beispiel wird jedoch eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) und dann navigieren:

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>Zugreifen auf Elemente

Laden von XAML zur Laufzeit mit der [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Methode lässt nicht zu stark typisierten Zugriff auf die XAML-Elemente, die Common Language Runtime zu verwendenden Objektnamen angegeben haben (mit `x:Name`). Jedoch diese XAML-Elemente können abgerufen werden mithilfe der [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) -Methode, und klicken Sie dann nach Bedarf zugegriffen:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

In diesem Beispiel ist der XAML für eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) vergrößert wird. Dieses XAML enthält eine [ `Label` ](xref:Xamarin.Forms.Label) mit dem Namen `monkeyName`, deren wird abgerufen, mit der [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) -Methode, bevor sie verfügt über [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft festgelegt ist.

## <a name="related-links"></a>Verwandte Links

- [LoadRuntimeXAML (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)
