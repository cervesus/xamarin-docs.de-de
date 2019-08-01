---
title: Bindung von einer Xamarin.Forms-Steuerelementvorlage
description: Durch Vorlagenbindungen können Steuerelemente in einer Steuerelementvorlage eine Datenbindung zu öffentlichen Eigenschaften herstellen, sodass Eigenschaftswerte von Steuerelementen in der Steuerelementvorlage leicht angepasst werden können. In diesem Artikel wird veranschaulicht, wie Sie mit Vorlagenbindungen Datenbindungen aus einer Steuerelementvorlage herstellen können.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 41e5bbc42ccde5cdd5223a7d2cb0a77da66e10c1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647003"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Bindung von einer Xamarin.Forms-Steuerelementvorlage

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simplethemewithtemplatebinding)

_Durch Vorlagenbindungen können Steuerelemente in einer Steuerelementvorlage eine Datenbindung zu öffentlichen Eigenschaften herstellen, sodass Eigenschaftswerte von Steuerelementen in der Steuerelementvorlage leicht angepasst werden können. In diesem Artikel wird veranschaulicht, wie Sie mit Vorlagenbindungen Datenbindungen aus einer Steuerelementvorlage herstellen können._

Eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Klasse wird verwendet, um eine Steuerelementeigenschaft in einer Steuerelementvorlage an eine bindbare Eigenschaft für das übergeordnete Element der *Ziel*-Anzeige zu binden, welches die Steuerelementvorlage besitzt. Anstatt einen durch [`Label`](xref:Xamarin.Forms.Label)-Instanzen innerhalb der [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse angezeigten Text zu definieren, könnten Sie eine Vorlagenbindung verwenden, um die [`Label.Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaft an bindbare Eigenschaften zu binden, die den anzuzeigenden Text definieren.

Eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Klasse ist vergleichbar mit einer vorhandenen [`Binding`](xref:Xamarin.Forms.Binding)-Klasse. Die *Quelle* einer `TemplateBinding`-Klasse wird jedoch automatisch auf das übergeordnete Element der *Ziel*-Anzeige festgelegt, welches die Steuerelementvorlage besitzt. Beachten Sie jedoch, dass die Verwendung einer `TemplateBinding`-Klasse außerhalb einer [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse nicht unterstützt wird.

## <a name="creating-a-templatebinding-in-xaml"></a>Erstellen von TemplateBinding-Elementen in XAML

In XAML wird eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Klasse mithilfe einer [`TemplateBinding`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension)-Markuperweiterung erstellt, wie in dem folgenden Codebeispiel gezeigt wird:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Anstatt die [`Label.Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaften auf statischen Text festzulegen, können die Eigenschaften Vorlagenbindungen verwenden zum Binden an bindbare Eigenschaften für das übergeordnete Element der *Ziel*-Anzeige, welches die [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse besitzt. Beachten Sie jedoch, dass die Vorlagenbindungen an `Parent.HeaderText` und `Parent.FooterText` anstelle von `HeaderText` und `FooterText` binden. Dies ist so, weil in diesem Beispiel die bindbaren Eigenschaften nicht für das übergeordnete, sondern für das über-übergeordnete Element der *Ziel*-Anzeige definiert sind, wie in dem folgenden Codebeispiel veranschaulicht wird:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

Die *Quelle* der Vorlagenbindung wird immer automatisch auf das übergeordnete Element der *Ziel*-Anzeige festgelegt, welches die Steuerelementvorlage besitzt, was hier der [`ContentView`](xref:Xamarin.Forms.ContentView)Instanz entspricht. Die Vorlagenbindung verwendet die [`Parent`](xref:Xamarin.Forms.Element.Parent)-Eigenschaft, um das übergeordnete Element der `ContentView`-Instanz zurückzugeben, was hier der [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanz entspricht. Deshalb werden die auf der `ContentPage` definierten bindbaren Eigenschaften gefunden, indem zum Binden an `Parent.HeaderText` und `Parent.FooterText` eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Klasse in der [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse verwendet wird, wie im folgenden Codebeispiel veranschaulicht wird:

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

![](template-binding-images/teal-theme.png "Blaugrüne Steuerelementvorlage mithilfe von Vorlagenbindungen")

## <a name="creating-a-templatebinding-in-c35"></a>Erstellen von TemplateBinding-Klassen in C&#35;

In C# wird eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Klasse erstellt, indem Sie den `TemplateBinding`-Konstruktor verwenden, wie im folgenden Codebeispiel veranschaulicht wird:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

Anstatt die [`Label.Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaften auf statischen Text festzulegen, können die Eigenschaften Vorlagenbindungen verwenden zum Binden an bindbare Eigenschaften für das übergeordnete Element der *Ziel*-Anzeige, welches die [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse besitzt. Die Vorlagenbindung wird mithilfe der [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))-Methode erstellt, wobei eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Instanz als der zweite Parameter angegeben wird. Beachten Sie, dass die Vorlagenbindungen an `Parent.HeaderText` und `Parent.FooterText` binden, weil die bindbaren Eigenschaften nicht für das übergeordnete, sondern für das über-übergeordnete Element der *Ziel*-Anzeige definiert sind, wie in dem folgenden Codebeispiel veranschaulicht wird:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

Wie bereits erwähnt, werden die bindbaren Eigenschaften auf der `ContentPage` definiert.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Binden einer bindbaren Eigenschaft an eine ViewModel-Eigenschaft

Wie bereits erwähnt, wird eine [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)-Klasse dazu verwendet, eine Steuerelementeigenschaft in einer Steuerelementvorlage an eine bindbare Eigenschaft für das übergeordnete Element der *Ziel*-Anzeige zu binden, welches die Steuerelementvorlage besitzt. Im Gegenzug können diese bindbaren Eigenschaften an Eigenschaften in ViewModels gebunden werden.

In dem folgenden Codebeispiel werden zwei Eigenschaften für ein ViewModel definiert:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

Die ViewModel-Eigenschaften `HeaderText` und `FooterText` können gebunden werden, wie in dem folgenden XAML-Codebeispiel zu sehen ist:

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

Die bindbaren Eigenschaften `HeaderText` und `FooterText` werden an die Eigenschaften `HomePageViewModel.HeaderText` und `HomePageViewModel.FooterText` gebunden, da die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Klasse auf eine Instanz der `HomePageViewModel`-Klasse festgelegt ist. Zusammenfassend führt dies zu Steuereigenschaften in der [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse, die an [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Instanzen auf der [`ContentPage`](xref:Xamarin.Forms.ContentPage) gebunden sind, und die wiederum an ViewModel-Eigenschaften binden.

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

Sie können an ViewModel-Eigenschaften auch direkt binden, so dass Sie `BindableProperty`-Klassen auf der `ContentPage` nicht als `HeaderText` und `FooterText` deklarieren müssen, indem Sie die Steuerelementvorlage an Parent.BindingContext binden._PropertyName_ z. B.:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.BindingContext.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.BindingContext.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Weitere Informationen zu Datenbindungen an ViewModels finden Sie unter [Von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie mit Vorlagenbindungen Datenbindungen aus einer Steuerelementvorlage herstellen können. Durch Vorlagenbindungen können Steuerelemente in einer Steuerelementvorlage eine Datenbindung zu öffentlichen Eigenschaften herstellen, sodass Eigenschaftswerte von Steuerelementen in der Steuerelementvorlage leicht angepasst werden können.

## <a name="related-links"></a>Verwandte Links

- [Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Einfaches Design mit Vorlagenbindung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simplethemewithtemplatebinding)
- [Einfaches Design mit Vorlagenbindung und "ViewModel" (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simplethemewithtemplatebindingandviewmodel)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
