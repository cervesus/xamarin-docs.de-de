---
Title: "explizite Stile in Xamarin.Forms " Description: "ein expliziter Stil ist ein Wert, der selektiv auf Steuerelemente angewendet wird, indem seine Stileigenschaften festgelegt werden. In diesem Artikel wird erläutert, wie explizite Stile in einer-Anwendung verwendet werden Xamarin.Forms . "
ms. Prod: xamarin ms. assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 02/17/2016 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="explicit-styles-in-xamarinforms"></a>Explizite Stile inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Ein explizites Format ist ein Element, das selektiv durch Festlegen der Stileigenschaften auf Steuerelemente angewendet wird._

## <a name="create-an-explicit-style-in-xaml"></a>Erstellen eines expliziten Stils in XAML

Um einen [`Style`](xref:Xamarin.Forms.Style) auf Seitenebene zu deklarieren, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) muss der Seite ein hinzugefügt werden, und anschließend kann eine oder mehrere `Style` Deklarationen in den eingefügt werden `ResourceDictionary` . Ein `Style` wird *explizit* durch die Deklaration eines `x:Key` Attributs erstellt, das ihm einen beschreibenden Schlüssel in der übergibt `ResourceDictionary` . *Explizite* Stile müssen dann durch Festlegen ihrer Eigenschaften auf bestimmte visuelle Elemente angewendet werden [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

Das folgende Codebeispiel zeigt *explizite* Stile, die in XAML in einer Seite deklariert `ResourceDictionary` und auf die Instanzen der Seite angewendet werden [`Label`](xref:Xamarin.Forms.Label) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Definiert drei *explizite* Stile, die auf die Instanzen der Seite angewendet werden [`Label`](xref:Xamarin.Forms.Label) . Jede `Style` wird zum Anzeigen von Text in einer anderen Farbe verwendet, während gleichzeitig die Optionen Schriftart Größe und horizontal und vertikal Layout festgelegt werden. Jeder `Style` wird auf einen anderen angewendet, `Label` indem seine [`Style`](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften mithilfe der `StaticResource` Markup Erweiterung festgelegt werden. Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![Beispiel für explizite Stile](explicit-images/explicit-styles.png)](explicit-images/explicit-styles-large.png#lightbox)

Außerdem wird auf das endgültige [`Label`](xref:Xamarin.Forms.Label) eine [`Style`](xref:Xamarin.Forms.Style) angewendet, aber auch die- [`TextColor`](xref:Xamarin.Forms.Label.TextColor) Eigenschaft auf einen anderen Wert überschrieben `Color` .

### <a name="create-an-explicit-style-at-the-control-level"></a>Erstellen eines expliziten Stils auf der Steuerelement Ebene

Zusätzlich zum Erstellen *expliziter* Stile auf Seitenebene können Sie auch auf der Steuerelement Ebene erstellt werden, wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In diesem Beispiel werden die *expliziten* - [`Style`](xref:Xamarin.Forms.Style) Instanzen der-Auflistung des-Steuer Elements zugewiesen [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Die Stile können dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden.

Weitere Informationen zum Erstellen von Stilen in einer Anwendung [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) finden Sie unter [globale Stile](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-explicit-style-in-c35"></a>Erstellen eines expliziten Stils in C-&#35;

[`Style`](xref:Xamarin.Forms.Style)der Auflistung einer Seite in c# können-Instanzen hinzugefügt werden [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) , indem ein neuer erstellt [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) und dann die-Instanzen hinzugefügt werden `Style` `ResourceDictionary` , wie im folgenden Codebeispiel gezeigt:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Der-Konstruktor definiert drei *explizite* Stile, die auf die-Instanzen der Seite angewendet werden [`Label`](xref:Xamarin.Forms.Label) . Jeder *explizit* [`Style`](xref:Xamarin.Forms.Style) wird [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) mithilfe der-Methode hinzugefügt [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) , wobei eine `key` Zeichenfolge angegeben wird, die auf die `Style` Instanz verweist. Jede `Style` wird `Label` durch Festlegen ihrer Eigenschaften auf ein anderes angewendet [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

Es gibt jedoch keinen Vorteil bei der Verwendung von [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Stattdessen [`Style`](xref:Xamarin.Forms.Style) können-Instanzen direkt den [`Style`](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften der erforderlichen visuellen Elemente zugewiesen werden, und Sie `ResourceDictionary` können entfernt werden, wie im folgenden Codebeispiel gezeigt:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Der-Konstruktor definiert drei *explizite* Stile, die auf die-Instanzen der Seite angewendet werden [`Label`](xref:Xamarin.Forms.Label) . Jede `Style` wird zum Anzeigen von Text in einer anderen Farbe verwendet, während gleichzeitig die Optionen Schriftart Größe und horizontal und vertikal Layout festgelegt werden. Jeder `Style` wird `Label` durch Festlegen seiner Eigenschaften auf einen anderen angewendet [`Style`](xref:Xamarin.Forms.NavigableElement.Style) . Außerdem wird auf das endgültige `Label` eine `Style` angewendet, aber auch die- `TextColor` Eigenschaft auf einen anderen Wert überschrieben `Color` .

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Grundlegende Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [style](xref:Xamarin.Forms.Style)
- [Trend](xref:Xamarin.Forms.Setter)
