---
title: Bindung aus einer Xamarin.Forms-ControlTemplate
description: Vorlagenbindungen können Binden von Inhaltssteuerelementen in einer Steuerelementvorlage auf Daten an der öffentlichen Eigenschaften aktivieren Eigenschaftswerte für Steuerelemente in der Steuerelementvorlage problemlos geändert werden. Dieser Artikel veranschaulicht, wie mit vorlagenbindungen Binden von Daten aus einer Steuerelementvorlage ausgeführt wird.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 99d798ce2c74da0cf7fa0d497128db628a12ead5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241580"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Bindung aus einer Xamarin.Forms-ControlTemplate

_Vorlagenbindungen können Binden von Inhaltssteuerelementen in einer Steuerelementvorlage auf Daten an der öffentlichen Eigenschaften aktivieren Eigenschaftswerte für Steuerelemente in der Steuerelementvorlage problemlos geändert werden. Dieser Artikel veranschaulicht, wie mit vorlagenbindungen Binden von Daten aus einer Steuerelementvorlage ausgeführt wird._

Ein [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) wird verwendet, um die Eigenschaft eines Steuerelements in einer Steuerelementvorlage auf eine bindbare Eigenschaft für das übergeordnete Element des Binden der *Ziel* anzeigen, die die Steuerelementvorlage besitzt. Z. B. anstelle eines definieren des angezeigten Elementtexts [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen innerhalb der [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), können Sie eine vorlagenbindung Binden der [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaft bindbare Eigenschaften, die die anzuzeigende Text zu definieren.

Ein [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) ähnelt dem einer vorhandenen [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/), außer dass die *Quelle* von einer `TemplateBinding` immer automatisch auf das übergeordnete Element des festgelegt die *Ziel* anzeigen, die die Steuerelementvorlage besitzt. Beachten Sie jedoch, dass die Verwendung einer `TemplateBinding` außerhalb von einem [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) wird nicht unterstützt.

## <a name="creating-a-templatebinding-in-xaml"></a>Erstellen eine TemplateBinding in XAML

In XAML eine [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) erstellt, wobei die [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

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

Statt set der [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaften für statischen Text, die Eigenschaften können vorlagenbindungen zum Binden an die bindungsfähigen Eigenschaften für das übergeordnete Element des der *Ziel* anzeigen, der Besitzer ist, die [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Beachten Sie jedoch, dass die vorlagenbindungen zu binden `Parent.HeaderText` und `Parent.FooterText`, anstatt `HeaderText` und `FooterText`. Grund dafür ist in diesem Beispiel die bindungsfähigen Eigenschaften definiert werden, auf der zweiten übergeordneten Ebene der dem *Ziel* zeigen an, anstatt das übergeordnete Element, wie im folgenden Codebeispiel wird veranschaulicht:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

Die *Quelle* der Vorlage Bindung immer automatisch auf das übergeordnete Element des festgelegt die *Ziel* anzeigen, das die Steuerelementvorlage besitzt, die hier ist die [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) -Instanz. Die Vorlage, die Bindung verwendet die [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) das übergeordnete Element des zurückzugebende Eigenschaft der `ContentView` -Instanz, die ist die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanz. Aus diesem Grund mithilfe einer [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) in der [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) zum Binden an `Parent.HeaderText` und `Parent.FooterText` sucht bindbaren auf definierten Eigenschaften der `ContentPage`, als Im folgenden Codebeispiel gezeigt:

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

Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

![](template-binding-images/teal-theme.png "Blaugrünen Steuerelementvorlage Vorlagenbindungen verwenden")

## <a name="creating-a-templatebinding-in-c35"></a>Erstellen eine TemplateBinding in C&#35;

In c# ist ein [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) wird erstellt, indem Sie mit der `TemplateBinding` -Konstruktor, wie im folgenden Codebeispiel wird veranschaulicht:

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

Statt set der [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaften für statischen Text, die Eigenschaften können vorlagenbindungen zum Binden an die bindungsfähigen Eigenschaften für das übergeordnete Element des der *Ziel* anzeigen, der Besitzer ist, die [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Die vorlagenbindung wird erstellt, mit der [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) -Methode angeben einer [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) Instanz als zweiten Parameter. Beachten Sie, die an die vorlagenbindungen binden `Parent.HeaderText` und `Parent.FooterText`, da die bindungsfähigen Eigenschaften auf der zweiten übergeordneten Ebene des definiert sind die *Ziel* zeigen an, anstatt das übergeordnete Element, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die bindungsfähigen Eigenschaften werden definiert, auf die `ContentPage`, wie zuvor beschrieben.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Binden einer BindableProperty an eine ViewModel-Eigenschaft

Wie bereits erwähnt, eine [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) Eigenschaft eines Steuerelements in einer Steuerelementvorlage bindet, auf eine bindbare Eigenschaft für das übergeordnete Element von der *Ziel* anzeigen, die die Steuerelementvorlage besitzt. Diese bindungsfähigen Eigenschaften können dagegen Eigenschaften in ViewModels gebunden werden.

Das folgende Codebeispiel definiert zwei Eigenschaften auf einem ViewModel:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

Die `HeaderText` und `FooterText` ViewModel-Eigenschaften zu, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt gebunden werden können:

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

Die `HeaderText` und `FooterText` bindbare Eigenschaften gebunden sind, um die `HomePageViewModel.HeaderText` und `HomePageViewModel.FooterText` Eigenschaften, die aufgrund der Einstellung der [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) mit einer Instanz von der `HomePageViewModel` Klasse. Insgesamt dadurch Steuerelementeigenschaften in der [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) an [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanzen auf der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), die wiederum Binden an ViewModel-Eigenschaften.

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

Weitere Informationen zum Binden von Daten in ViewModels finden Sie unter [aus Datenbindungen, MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel gezeigten mithilfe von vorlagenbindungen zum Binden von Daten aus einer Steuerelementvorlage ausführen. Vorlagenbindungen können Binden von Inhaltssteuerelementen in einer Steuerelementvorlage auf Daten an der öffentlichen Eigenschaften aktivieren Eigenschaftswerte für Steuerelemente in der Steuerelementvorlage problemlos geändert werden.



## <a name="related-links"></a>Verwandte Links

- [Data Binding-Grundlagen](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Aus Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Einfache Design mit Template Binding (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Einfache Design mit Template Binding und ViewModel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
