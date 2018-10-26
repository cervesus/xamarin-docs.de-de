---
title: Bindung von einer Xamarin.Forms-ControlTemplate
description: Vorlagenbindungen können binden Steuerelemente in einer Steuerelementvorlage auf Daten an öffentliche Eigenschaften, aktivieren die Eigenschaftswerte für Steuerelemente in der Steuerelementvorlage problemlos geändert werden. In diesem Artikel veranschaulicht, wie mit vorlagenbindungen aus einer Steuerelementvorlage eine Datenbindung durchgeführt wird.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 13730dce5d4698085abe10cb93da5ba50b87ab01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106428"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Bindung von einer Xamarin.Forms-ControlTemplate

_Vorlagenbindungen können binden Steuerelemente in einer Steuerelementvorlage auf Daten an öffentliche Eigenschaften, aktivieren die Eigenschaftswerte für Steuerelemente in der Steuerelementvorlage problemlos geändert werden. In diesem Artikel veranschaulicht, wie mit vorlagenbindungen aus einer Steuerelementvorlage eine Datenbindung durchgeführt wird._

Ein [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) wird verwendet, um die Eigenschaft eines Steuerelements in einer Steuerelementvorlage auf eine bindbare Eigenschaft für das übergeordnete Element des binden die *Ziel* anzeigen, die die Vorlage des Steuerelements besitzt. Z. B. statt der definieren des Texts von [ `Label` ](xref:Xamarin.Forms.Label) Instanzen innerhalb der [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate), können Sie eine vorlagenbindung zum Binden der [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft bindbare Eigenschaften, die den anzuzeigenden Text definieren.

Ein [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) ist vergleichbar mit einer vorhandenen [ `Binding` ](xref:Xamarin.Forms.Binding), außer dass die *Quelle* von einer `TemplateBinding` immer automatisch festgelegt ist, das dem übergeordneten der *Ziel* anzeigen, die die Vorlage des Steuerelements besitzt. Beachten Sie jedoch, dass die Verwendung einer `TemplateBinding` außerhalb von einem [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) wird nicht unterstützt.

## <a name="creating-a-templatebinding-in-xaml"></a>Erstellen ein TemplateBinding-Element in XAML

In XAML eine [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) wurde mit der [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

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

Anstatt die [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaften in statischen Text, die Eigenschaften können vorlagenbindungen zum Binden an bindbare Eigenschaften auf das übergeordnete Element der *Ziel* anzeigen, der Besitzer ist, der [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Beachten Sie jedoch, dass die Bindung von vorlagenbindungen an `Parent.HeaderText` und `Parent.FooterText`, statt `HeaderText` und `FooterText`. Dies ist, da in diesem Beispiel, das die bindbaren Eigenschaften, auf die zweite übergeordnete definiert sind der *Ziel* anzuzeigen sein können, anstatt das übergeordnete Element, wie im folgenden Codebeispiel wird veranschaulicht:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

Der *Quelle* der Vorlage Bindung wird immer automatisch festgelegt, das übergeordnete Element des der *Ziel* anzeigen, das die Vorlage für ein Steuerelement besitzt, die hier ist der [ `ContentView` ](xref:Xamarin.Forms.ContentView) -Instanz. Die Vorlage, die Bindung verwendet die [ `Parent` ](xref:Xamarin.Forms.Element.Parent) das übergeordnete Element der zurückzugebende Eigenschaft der `ContentView` -Instanz, die ist die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Instanz. Daher wird die Verwendung einer [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) in die [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) zum Binden an `Parent.HeaderText` und `Parent.FooterText` sucht die bindbaren Eigenschaften, die auf definiert sind die `ContentPage`, als Im folgenden Codebeispiel veranschaulicht:

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

Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

![](template-binding-images/teal-theme.png "Blaugrünen Steuerelementvorlage, die mithilfe von Vorlagenbindungen")

## <a name="creating-a-templatebinding-in-c35"></a>Erstellen ein TemplateBinding-Element, in C&#35;

In c# eine [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) wird erstellt, indem Sie mit der `TemplateBinding` -Konstruktor, wie im folgenden Codebeispiel wird veranschaulicht:

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

Anstatt die [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaften in statischen Text, die Eigenschaften können vorlagenbindungen zum Binden an bindbare Eigenschaften auf das übergeordnete Element der *Ziel* anzeigen, der Besitzer ist, der [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Die vorlagenbindung wird erstellt, mit der [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) -Methode angeben einer [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) Instanz als zweiten Parameter. Beachten Sie, die an die vorlagenbindungen binden `Parent.HeaderText` und `Parent.FooterText`, da die bindbaren Eigenschaften, auf die zweite übergeordnete definiert sind der *Ziel* anzuzeigen sein können, anstatt das übergeordnete Element, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die bindbare Eigenschaften definiert die `ContentPage`, wie bereits kurz erwähnt.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Binden eine BindableProperty an eine ViewModel-Eigenschaft

Wie bereits erwähnt, ein [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) bindet die Eigenschaft eines Steuerelements in einer Steuerelementvorlage auf eine bindbare Eigenschaft für das übergeordnete Element des der *Ziel* anzeigen, die die Vorlage des Steuerelements besitzt. Im Gegenzug können diese bindbaren Eigenschaften auf Eigenschaften in ViewModels gebunden werden.

Das folgende Codebeispiel definiert zwei Eigenschaften auf ein "ViewModel" an:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

Die `HeaderText` und `FooterText` ViewModel-Eigenschaften zu, wie in den folgenden XAML-Codebeispiel gezeigt gebunden werden können:

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

Die `HeaderText` und `FooterText` bindbare Eigenschaften gebunden sind, um die `HomePageViewModel.HeaderText` und `HomePageViewModel.FooterText` Eigenschaften, die aufgrund der Einstellung der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) mit einer Instanz von der `HomePageViewModel` Klasse. Alles in allem dadurch Steuerelementeigenschaften in der [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) gebunden wird [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) auf Instanzen der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), die wiederum Binden an Eigenschaften von "ViewModel".

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

Sie auch eine Bindung an die ansichtsmodelleigenschaften direkt, damit Sie nicht deklarieren müssen `BindableProperty`s für `HeaderText` und `FooterText` auf die `ContentPage`, indem Sie die Steuerelementvorlage an Parent.BindingContext binden. _PropertyName_ z. B.:

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

Weitere Informationen zur Datenbindung in ViewModels finden Sie unter [von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht die Verwendung von vorlagenbindungen zum Ausführen von Datenbindung in einer Steuerelementvorlage. Vorlagenbindungen können binden Steuerelemente in einer Steuerelementvorlage auf Daten an öffentliche Eigenschaften, aktivieren die Eigenschaftswerte für Steuerelemente in der Steuerelementvorlage problemlos geändert werden.

## <a name="related-links"></a>Verwandte Links

- [Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Einfaches Design mit Vorlagenbindung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Einfaches Design mit Vorlagenbindung und "ViewModel" (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
