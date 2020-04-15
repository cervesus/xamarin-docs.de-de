---
title: Xamarin.Forms-Steuerelementvorlagen
description: Xamarin.Forms-Steuerelementvorlagen definieren die visuelle Struktur von ContentView abgeleiteter benutzerdefinierter Steuerelemente und von ContentPage abgeleiteter Seiten.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2020
ms.openlocfilehash: a73123b89cba932f2e2cb907645f6fe858cf6176
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303840"
---
# <a name="xamarinforms-control-templates"></a>Xamarin.Forms-Steuerelementvorlagen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplatedemos)

Mit Xamarin.Forms-Steuerelementvorlagen können Sie die visuelle Struktur von [`ContentView`](xref:Xamarin.Forms.ContentView) abgeleiteter benutzerdefinierter Steuerelemente und von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteter Seiten definieren. Steuerelementvorlagen trennen die Benutzeroberfläche (UI) für ein benutzerdefiniertes Steuerelement oder eine Seite von der Logik, die das Steuerelement oder die Seite implementiert. Zusätzliche Inhalte können auch an einer vordefinierten Stelle in benutzerdefinierten Steuerelementen mit Vorlagen oder Seiten mit Vorlagen eingefügt werden.

Beispielsweise können Sie eine Steuerelementvorlage erstellen, die die Benutzeroberfläche eines benutzerdefinierten Steuerelements neu definiert. Die Steuerelementvorlage kann anschließend von der erforderlichen Instanz des benutzerdefinierten Steuerelements genutzt werden. Alternativ können Sie eine benutzerdefinierte Steuerelementvorlage erstellen, die eine allgemeine Benutzeroberfläche definiert, die für mehrere Seiten einer Anwendung verwendet wird. Die Steuerelementvorlage kann anschließend von mehreren Seiten genutzt werden, wobei jede Seite ihren eindeutigen Inhalt anzeigt.

## <a name="create-a-controltemplate"></a>Erstellen einer ControlTemplate

Im folgenden Beispiel wird der Code für ein benutzerdefiniertes `CardView`-Steuerelement veranschaulicht:

```csharp
public class CardView : ContentView
{
    public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(nameof(CardTitle), typeof(string), typeof(CardView), string.Empty);
    public static readonly BindableProperty CardDescriptionProperty = BindableProperty.Create(nameof(CardDescription), typeof(string), typeof(CardView), string.Empty);
    // ...

    public string CardTitle
    {
        get => (string)GetValue(CardTitleProperty);
        set => SetValue(CardTitleProperty, value);
    }

    public string CardDescription
    {
        get => (string)GetValue(CardDescriptionProperty);
        set => SetValue(CardDescriptionProperty, value);
    }
    // ...
}
```

Die `CardView`-Klasse, die von der [`ContentView`](xref:Xamarin.Forms.ContentView)-Klasse abgeleitet wird, stellt ein benutzerdefiniertes Steuerelement dar, das Daten in einem Karten ähnlichen Layout anzeigt. Die Klasse enthält Eigenschaften, die von bindbaren Eigenschaften unterstützt werden, für die angezeigten Daten. Die `CardView`-Klasse definiert jedoch keine Benutzeroberfläche. Stattdessen wird die Benutzeroberfläche mithilfe einer Steuerelementvorlage definiert. Weitere Informationen zum Erstellen von `ContentView` abgeleiteter benutzerdefinierter Steuerelemente finden Sie unter [Xamarin.Forms-ContentView](~/xamarin-forms/user-interface/layouts/contentview.md).

Eine Steuerelementvorlage wird mit dem Typ [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) erstellt. Wenn Sie eine `ControlTemplate`-Klasse erstellen, kombinieren Sie [`View`](xref:Xamarin.Forms.View)-Objekte, um die Benutzeroberfläche für ein benutzerdefiniertes Steuerelement oder eine Seite zu erstellen. Eine `ControlTemplate`-Klasse darf nur ein `View` als Stammelement aufweisen. In der Regel enthält das Stammelement jedoch andere `View`-Objekte. Die visuelle Struktur wird durch die Kombination dieser Objekte bestimmt.

Zwar kann eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse inline definiert werden, jedoch besteht der übliche Ansatz zum Deklarieren einer [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse darin, sie als Ressource in einem Ressourcenverzeichnis zu deklarieren. Da es sich bei Steuerelementvorlagen um Ressourcen handelt, unterliegen sie denselben Bereichsregeln, die für alle Ressourcen gelten. Wenn Sie beispielsweise eine Steuerelementvorlage im Stammelement der XAML-Anwendungsdefinitionsdatei deklarieren, kann die Vorlage überall in Ihrer Anwendung verwendet werden. Wenn Sie die Vorlage in einer Seite definieren, kann die Steuerelementvorlage nur auf dieser Seite verwendet werden. Weitere Informationen zu Ressourcen finden Sie unter [Xamarin.Forms-Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md).

Im folgenden XAML-Beispiel eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse für `CardView`-Objekte veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
      <ControlTemplate x:Key="CardViewControlTemplate">
          <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                 BackgroundColor="{Binding CardColor}"
                 BorderColor="{Binding BorderColor}"
                 CornerRadius="5"
                 HasShadow="True"
                 Padding="8"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">
              <Grid>
                  <Grid.RowDefinitions>
                      <RowDefinition Height="75" />
                      <RowDefinition Height="4" />
                      <RowDefinition Height="Auto" />
                  </Grid.RowDefinitions>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="75" />
                      <ColumnDefinition Width="200" />
                  </Grid.ColumnDefinitions>
                  <Frame IsClippedToBounds="True"
                         BorderColor="{Binding BorderColor}"
                         BackgroundColor="{Binding IconBackgroundColor}"
                         CornerRadius="38"
                         HeightRequest="60"
                         WidthRequest="60"
                         HorizontalOptions="Center"
                         VerticalOptions="Center">
                      <Image Source="{Binding IconImageSource}"
                             Margin="-20"
                             WidthRequest="100"
                             HeightRequest="100"
                             Aspect="AspectFill" />
                  </Frame>
                  <Label Grid.Column="1"
                         Text="{Binding CardTitle}"
                         FontAttributes="Bold"
                         FontSize="Large"
                         VerticalTextAlignment="Center"
                         HorizontalTextAlignment="Start" />
                  <BoxView Grid.Row="1"
                           Grid.ColumnSpan="2"
                           BackgroundColor="{Binding BorderColor}"
                           HeightRequest="2"
                           HorizontalOptions="Fill" />
                  <Label Grid.Row="2"
                         Grid.ColumnSpan="2"
                         Text="{Binding CardDescription}"
                         VerticalTextAlignment="Start"
                         VerticalOptions="Fill"
                         HorizontalOptions="Fill" />
              </Grid>
          </Frame>
      </ControlTemplate>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Wenn eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse als Ressource deklariert wird, muss sie einen mit dem `x:Key`-Attribut festgelegten Schlüssel enthalten, damit sie im Ressourcenverzeichnis identifiziert werden kann. In diesem Beispiel handelt es sich beim Stammelement von `CardViewControlTemplate` um ein [`Frame`](xref:Xamarin.Forms.Frame)-Objekt. Das `Frame`-Objekt nutzt die Markuperweiterung `RelativeSource`, um `BindingContext` auf die Instanz des Laufzeitobjekts festzulegen, auf die die Vorlage angewendet wird. Diese wird auch als *übergeordnetes Element mit Vorlagen* bezeichnet. Das `Frame`-Objekt enthält eine Kombination aus [`Grid`](xref:Xamarin.Forms.Grid)-, `Frame`-, [`Image`](xref:Xamarin.Forms.Image)-, [`Label`](xref:Xamarin.Forms.Label)- und [`BoxView`](xref:Xamarin.Forms.BoxView)-Objekten, die die visuelle Struktur eines `CardView`-Objekts definieren. Die Bindungsausdrücke dieser Objekte werden mit `CardView`-Eigenschaften aufgelöst, weil sie `BindingContext` vom `Frame`-Stammelement erben. Weitere Informationen zur Markuperweiterung `RelativeSource` finden Sie unter [Relative Bindungen in Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

## <a name="consume-a-controltemplate"></a>Verwenden von ControlTemplate

Ein [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekt kann auf ein von [`ContentView`](xref:Xamarin.Forms.ContentView)abgeleitetes benutzerdefiniertes Steuerelement angewendet werden, indem das Steuerelementvorlagenobjekt als [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)-Eigenschaft festgelegt wird. Entsprechend kann ein [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekt auf eine von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seite angewendet werden, indem das Steuerelementvorlagenobjekt als [`ControlTemplate`](xref:Xamarin.Forms.TemplatedPage.ControlTemplate)-Eigenschaft festgelegt wird. Wenn zur Laufzeit ein [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekt angewendet wird, werden alle in `ControlTemplate` definierten Steuerelemente zur visuellen Struktur des benutzerdefinierten Steuerelements mit Vorlagen oder der Seite mit Vorlagen hinzugefügt.

Im folgenden Beispiel wird veranschaulicht, wie `CardViewControlTemplate` zur [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)-Eigenschaft aller `CardView`-Objekte zugewiesen wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <StackLayout Margin="30">
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel werden die Steuerelemente in `CardViewControlTemplate` Teil der visuellen Struktur aller `CardView`-Objekte. Da das Stammobjekt [`Frame`](xref:Xamarin.Forms.Frame) für die Steuerelementvorlage das übergeordnete Element mit Vorlagen als `BindingContext` festlegt, werden die Bindungsausdrücke von `Frame` und dessen untergeordnete Elemente anhand der Eigenschaften der einzelnen `CardView`-Objekte aufgelöst.

In den folgenden Screenshots werden die `CardViewControlTemplate`-Elemente veranschaulicht, die auf die drei `CardView`-Objekte angewendet wurden:

[![Screenshot: CardView-Objekte mit Vorlagen unter iOS und Android](control-template-images/relativesource-controltemplate.png "CardView-Objekte mit Vorlagen")](control-template-images/relativesource-controltemplate-large.png#lightbox "CardView-Objekte mit Vorlagen")

> [!IMPORTANT]
> Der Zeitpunkt, zu dem ein [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) auf eine Steuerelementinstanz angewendet wird, kann ermittelt werden, indem die `OnApplyTemplate`-Methode im benutzerdefinierten Steuerelement mit Vorlagen oder in der Seite mit Vorlagen überschrieben wird. Weitere Informationen finden Sie unter [Abrufen eines benannten Elements aus einer Vorlage](#get-a-named-element-from-a-template).

## <a name="pass-parameters-with-templatebinding"></a>Übergeben von Parametern mit TemplateBinding

Die Markuperweiterung `TemplateBinding` bindet eine Eigenschaft eines in einem [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekt enthaltenen Elements an eine vom benutzerdefinierten Steuerelement mit Vorlagen oder der Seite mit Vorlagen definierte öffentliche Eigenschaft. Wenn Sie die Markuperweiterung `TemplateBinding` verwenden, können Eigenschaften des Steuerelements als Parameter der Vorlage fungieren. Wenn eine Eigenschaft für ein benutzerdefiniertes Steuerelement mit Vorlagen oder eine Seite mit Vorlagen festgelegt wird, wird daher dieser Wert an das Element übergeben, das `TemplateBinding` enthält.

> [!IMPORTANT]
> Die Markuperweiterung `TemplateBinding` kann alternativ zum Erstellen eines [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekts verwendet werden, das die Markuperweiterung `RelativeSource` verwendet, um die `BindingContext`-Eigenschaft des Stammelements in der Vorlage auf das übergeordnete Element mit Vorlagen festzulegen. Die Markuperweiterung `TemplateBinding` entfernt die `RelativeSource`-Bindung und ersetzt `Binding`-Ausdrücke durch `TemplateBinding`-Ausdrücke.

Die Markuperweiterung `TemplateBinding` definiert die folgenden Eigenschaften:

- Die Eigenschaft `Path` vom Typ `string`, die den Pfad zur Eigenschaft definiert.
- Die Eigenschaft `Mode` vom Typ `BindingMode`, die die Richtung für die Weitergabe von Änderungen zwischen der *Quelle* und dem *Ziel* definiert.
- Die Eigenschaft `Converter` vom Typ `IValueConverter`, die den Bindungswertkonverter definiert.
- Die Eigenschaft `ConverterParameter` vom Typ `object`, die den Parameter für den Bindungswertkonverter definiert.
- Die Eigenschaft `StringFormat` vom Typ `string`, die das Zeichenfolgenformat für die Bindung definiert.

Die `ContentProperty`-Eigenschaft für die Markuperweiterung `TemplateBinding` ist `Path`. Daher kann der Abschnitt „Path=“ der Markuperweiterung ausgelassen werden, wenn der Pfad das erste Element im Ausdruck `TemplateBinding` ist. Weitere Informationen zur Verwendung dieser Eigenschaften in einem Bindungsausdruck finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

> [!WARNING]
> Die Markuperweiterung `TemplateBinding` sollte nur in [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) verwendet werden. Wenn Sie jedoch versuchen, einen `TemplateBinding`-Ausdruck außerhalb von `ControlTemplate` zu verwenden, führt dies weder zu einem Buildfehler noch zu einer Ausnahme.

Im folgenden XAML-Beispiel wird ein [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) für `CardView`-Objekte veranschaulicht, das die Markuperweiterung `TemplateBinding` verwendet:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BackgroundColor="{TemplateBinding CardColor}"
                   BorderColor="{TemplateBinding BorderColor}"
                   CornerRadius="5"
                   HasShadow="True"
                   Padding="8"
                   HorizontalOptions="Center"
                   VerticalOptions="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="75" />
                        <RowDefinition Height="4" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="75" />
                        <ColumnDefinition Width="200" />
                    </Grid.ColumnDefinitions>
                    <Frame IsClippedToBounds="True"
                           BorderColor="{TemplateBinding BorderColor}"
                           BackgroundColor="{TemplateBinding IconBackgroundColor}"
                           CornerRadius="38"
                           HeightRequest="60"
                           WidthRequest="60"
                           HorizontalOptions="Center"
                           VerticalOptions="Center">
                        <Image Source="{TemplateBinding IconImageSource}"
                               Margin="-20"
                               WidthRequest="100"
                               HeightRequest="100"
                               Aspect="AspectFill" />
                    </Frame>
                    <Label Grid.Column="1"
                           Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold"
                           FontSize="Large"
                           VerticalTextAlignment="Center"
                           HorizontalTextAlignment="Start" />
                    <BoxView Grid.Row="1"
                             Grid.ColumnSpan="2"
                             BackgroundColor="{TemplateBinding BorderColor}"
                             HeightRequest="2"
                             HorizontalOptions="Fill" />
                    <Label Grid.Row="2"
                           Grid.ColumnSpan="2"
                           Text="{TemplateBinding CardDescription}"
                           VerticalTextAlignment="Start"
                           VerticalOptions="Fill"
                           HorizontalOptions="Fill" />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    ...
</ContentPage>
```

In diesem Beispiel löst die Markuperweiterung `TemplateBinding` Bindungsausdrücke für die Eigenschaften der `CardView`-Objekte auf. In den folgenden Screenshots werden die `CardViewControlTemplate`-Elemente veranschaulicht, die auf die drei `CardView`-Objekte angewendet wurden:

[![Screenshot: CardView-Objekte mit Vorlagen unter iOS und Android](control-template-images/templatebinding-controltemplate.png "CardView-Objekte mit Vorlagen")](control-template-images/templatebinding-controltemplate-large.png#lightbox "CardView-Objekte mit Vorlagen")

> [!IMPORTANT]
> Die Verwendung der Markuperweiterung `TemplateBinding` gleicht dem Festlegen des übergeordneten Elements mit Vorlagen für die `BindingContext`-Eigenschaft des Stammelements in der Vorlage mithilfe der Markuperweiterung `RelativeSource` und dem anschließenden Auflösen von Bindungen von untergeordneten Objekten mit der Markuperweiterung `Binding`. Durch die Markuperweiterung `TemplateBinding` wird ein `Binding` erstellt, dessen `Source` `RelativeBindingSource.TemplatedParent` ist.

## <a name="apply-a-controltemplate-with-a-style"></a>Anwenden von ControlTemplate mit einer Formatvorlage

Steuerelementvorlagen können auch mit Formatvorlagen angewendet werden. Hierzu wird eine *implizite* oder *explizite* Formatvorlage erstellt, die das [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekt verwendet.

Im folgenden XAML-Beispiel wird eine *implizite* Formatvorlage veranschaulicht, die `CardViewControlTemplate` verwendet:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            ...
        </ControlTemplate>

        <Style TargetType="controls:CardView">
            <Setter Property="ControlTemplate"
                    Value="{StaticResource CardViewControlTemplate}" />
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="30">
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"/>
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die *implizite* [`Style`](xref:Xamarin.Forms.Style)-Klasse automatisch auf alle `CardView`-Objekte angewendet, und die Eigenschaft [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) aller `CardView`-Elemente wird auf `CardViewControlTemplate` festgelegt.

Weitere Informationen zu Formatvorlagen finden Sie unter [Xamarin.Forms-Formatvorlagen](~/xamarin-forms/user-interface/styles/index.md).

## <a name="redefine-a-controls-ui"></a>Neudefinieren der Benutzeroberfläche eines Steuerelements

Wenn ein [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Objekt instanziiert und der `ControlTemplate`-Eigenschaft eines von [`ContentView`](xref:Xamarin.Forms.ContentView) abgeleiteten benutzerdefinierten Steuerelements oder einer von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteten Seite zugewiesen wird, wird die vom benutzerdefinierten Steuerelement oder der Seite definierte visuelle Struktur durch die in `ControlTemplate` definierte visuelle Struktur ersetzt.

Das benutzerdefinierte Steuerelement `CardViewUI` definiert die Benutzeroberfläche beispielsweise mithilfe des folgenden XAML-Codes:

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ControlTemplateDemos.Controls.CardViewUI"
             x:Name="this">
    <Frame BindingContext="{x:Reference this}"
           BackgroundColor="{Binding CardColor}"
           BorderColor="{Binding BorderColor}"
           CornerRadius="5"
           HasShadow="True"
           Padding="8"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="75" />
                <RowDefinition Height="4" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="75" />
                <ColumnDefinition Width="200" />
            </Grid.ColumnDefinitions>
            <Frame IsClippedToBounds="True"
                   BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Gray'}"
                   CornerRadius="38"
                   HeightRequest="60"
                   WidthRequest="60"
                   HorizontalOptions="Center"
                   VerticalOptions="Center">
                <Image Source="{Binding IconImageSource}"
                       Margin="-20"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill" />
            </Frame>
            <Label Grid.Column="1"
                   Text="{Binding CardTitle, FallbackValue='Card title'}"
                   FontAttributes="Bold"
                   FontSize="Large"
                   VerticalTextAlignment="Center"
                   HorizontalTextAlignment="Start" />
            <BoxView Grid.Row="1"
                     Grid.ColumnSpan="2"
                     BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     HeightRequest="2"
                     HorizontalOptions="Fill" />
            <Label Grid.Row="2"
                   Grid.ColumnSpan="2"
                   Text="{Binding CardDescription, FallbackValue='Card description'}"
                   VerticalTextAlignment="Start"
                   VerticalOptions="Fill"
                   HorizontalOptions="Fill" />
        </Grid>
    </Frame>
</ContentView>
```

Die Steuerelemente, aus der sich diese Benutzeroberfläche zusammensetzt, können jedoch ersetzt werden, indem eine neue visuelle Struktur in einer [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse definiert wird, die dann der [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)-Eigenschaft eines `CardViewUI`-Objekts zugewiesen wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="{TemplateBinding IconImageSource}"
                        BackgroundColor="{TemplateBinding IconBackgroundColor}"
                        WidthRequest="100"
                        HeightRequest="100"
                        Aspect="AspectFill"
                        HorizontalOptions="Center"
                        VerticalOptions="Center" />
                <StackLayout Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout Margin="30">
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="John Doe"
                             CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="Jane Doe"
                             CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="Xamarin Monkey"
                             CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die visuelle Struktur des `CardViewUI`-Objekts in einer [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse neu definiert, die eine kompaktere visuelle Struktur bereitstellt, die sich für eine komprimierte Liste eignet:

[![Screenshot: CardViewUI-Objekte mit Vorlagen unter iOS und Android](control-template-images/redefine-controltemplate.png "CardViewUI-Objekte mit Vorlagen")](control-template-images/redefine-controltemplate-large.png#lightbox "CardViewUI-Objekte mit Vorlagen")

## <a name="substitute-content-into-a-contentpresenter"></a>Ersetzen von Inhalten in ContentPresenter

Eine [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)-Klasse kann in einer Steuerelementvorlage platziert werden, um zu markieren, wo Inhalt vom benutzerdefinierten Steuerelement mit Vorlagen oder der Seite mit Vorlagen angezeigt werden soll. Das benutzerdefinierte Steuerelement oder die Seite, die die Steuerelementvorlage nutzen, definieren dann den Inhalt, der von `ContentPresenter` angezeigt werden soll. Das folgende Diagramm veranschaulicht eine `ControlTemplate`-Klasse für eine Seite mit einer Reihe von Steuerelementen. Zusätzlich abgebildet ist eine `ContentPresenter`-Klasse, hier dargestellt durch ein blaues Rechteck:

![Steuerelementvorlage für ein ContentPage-Element](control-template-images/control-template.png "Steuerelementvorlage für ein ContentPage-Element")

Im folgenden XAML-Code wird eine Steuerelementvorlage namens `TealTemplate` veranschaulicht, die eine [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)-Klasse in der visuellen Struktur enthält:

```xaml
<ControlTemplate x:Key="TealTemplate">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="0.1*" />
            <RowDefinition Height="0.8*" />
            <RowDefinition Height="0.1*" />
        </Grid.RowDefinitions>
        <BoxView Color="Teal" />
        <Label Margin="20,0,0,0"
               Text="{TemplateBinding HeaderText}"
               TextColor="White"
               FontSize="Title"
               VerticalOptions="Center" />
        <ContentPresenter Grid.Row="1" />
        <BoxView Grid.Row="2"
                 Color="Teal" />
        <Label x:Name="changeThemeLabel"
               Grid.Row="2"
               Margin="20,0,0,0"
               Text="Change Theme"
               TextColor="White"
               HorizontalOptions="Start"
               VerticalOptions="Center">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnChangeThemeLabelTapped" />
            </Label.GestureRecognizers>
        </Label>
        <controls:HyperlinkLabel Grid.Row="2"
                                 Margin="0,0,20,0"
                                 Text="Help"
                                 TextColor="White"
                                 Url="https://docs.microsoft.com/xamarin/xamarin-forms/"
                                 HorizontalOptions="End"
                                 VerticalOptions="Center" />
    </Grid>
</ControlTemplate>
```

Im folgenden Beispiel wird veranschaulicht, wie `TealTemplate` zur [`ControlTemplate`](xref:Xamarin.Forms.TemplatedPage.ControlTemplate)-Eigenschaft einer von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteten Seite zugewiesen wird:

```xaml
<controls:HeaderFooterPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"                           
                           ControlTemplate="{StaticResource TealTemplate}"
                           HeaderText="MyApp"
                           ...>
    <StackLayout Margin="10">
        <Entry Placeholder="Enter username" />
        <Entry Placeholder="Enter password"
               IsPassword="True" />
        <Button Text="Login" />
    </StackLayout>
</controls:HeaderFooterPage>
```

Wenn `TealTemplate` zur Laufzeit auf die Seite angewendet wird, wird der Seiteninhalt in das [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)-Element eingefügt, das in der Steuerelementvorlage definiert wird:

[![Screenshot: Seitenobjekt mit Vorlagen unter iOS und Android](control-template-images/tealtemplate-contentpage.png "ContentPage-Element mit Vorlagen")](control-template-images/tealtemplate-contentpage-large.png#lightbox "ContentPage-Element mit Vorlagen")

## <a name="get-a-named-element-from-a-template"></a>Abrufen eines benannten Elements aus einer Vorlage

Benannte Elemente innerhalb einer Steuerelementvorlage können aus dem benutzerdefinierten Steuerelement mit Vorlagen oder der Seite mit Vorlagen abgerufen werden. Hierzu dient die `GetTemplateChild`-Methode, die das benannte Element in der instanziierten visuellen [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Struktur zurückgibt, wenn es gefunden wird. Andernfalls wird `null`zurückgegeben.

Nachdem eine Steuerelementvorlage instanziiert wurde, wird die `OnApplyTemplate`-Methode der Vorlage aufgerufen. Die `GetTemplateChild`-Methode sollte daher über eine `OnApplyTemplate`-Außerkraftsetzung im Steuerelement mit Vorlagen oder der Seite mit Vorlagen aufgerufen werden.

> [!IMPORTANT]
> Die `GetTemplateChild`-Methode sollte nur nach dem Aufruf der `OnApplyTemplate`-Methode aufgerufen werden.

Im folgenden XAML-Code wird eine Steuerelementvorlage namens `TealTemplate` veranschaulicht, die auf von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleitete Seiten angewendet werden kann:

```xaml
<ControlTemplate x:Key="TealTemplate">
    <Grid>
        ...
        <Label x:Name="changeThemeLabel"
               Grid.Row="2"
               Margin="20,0,0,0"
               Text="Change Theme"
               TextColor="White"
               HorizontalOptions="Start"
               VerticalOptions="Center">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnChangeThemeLabelTapped" />
            </Label.GestureRecognizers>
        </Label>
        ...
    </Grid>
</ControlTemplate>
```

Im folgenden Beispiel wird das [`Label`](xref:Xamarin.Forms.Label)-Element benannt, das im Code für die Seite mit Vorlagen abgerufen werden kann. Hierzu wird die `GetTemplateChild`-Methode der `OnApplyTemplate`-Außerkraftsetzung für die Seite mit Vorlagen aufgerufen:

```csharp
public partial class AccessTemplateElementPage : HeaderFooterPage
{
    Label themeLabel;

    public AccessTemplateElementPage()
    {
        InitializeComponent();
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        themeLabel = (Label)GetTemplateChild("changeThemeLabel");
        themeLabel.Text = OriginalTemplate ? "Aqua Theme" : "Teal Theme";
    }
}
```

In diesem Beispiel wurde das [`Label`](xref:Xamarin.Forms.Label)-Objekt namens `changeThemeLabel` abgerufen, sobald `ControlTemplate` instanziiert wurde. `changeThemeLabel` kann anschließend von der `AccessTemplateElementPage`-Klasse aufgerufen und bearbeitet werden. In den folgenden Screenshots wird gezeigt, dass der von `Label` angezeigte Text geändert wurde:

[![Screenshot: Seitenobjekt mit Vorlagen unter iOS und Android](control-template-images/get-named-element.png "ContentPage-Element mit Vorlagen")](control-template-images/get-named-element-large.png#lightbox "ContentPage-Element mit Vorlagen")

## <a name="bind-to-a-viewmodel"></a>Einbindung in ViewModel

Eine [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse kann eine Datenbindung mit einem ViewModel-Element auch dann herstellen, wenn die `ControlTemplate`-Klasse in das übergeordnete Element mit Vorlagen eingebunden ist (die Laufzeitobjektinstanz, auf die die Vorlage angewendet wird).

Im folgenden XAML-Beispiel wird eine Seite veranschaulicht, die ein ViewModel-Element namens `PeopleViewModel` verwendet:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ControlTemplateDemos"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <ContentPage.BindingContext>
        <local:PeopleViewModel />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <DataTemplate x:Key="PersonTemplate">
            <controls:CardView BorderColor="DarkGray"
                               CardTitle="{Binding Name}"
                               CardDescription="{Binding Description}"
                               ControlTemplate="{StaticResource CardViewControlTemplate}" />
        </DataTemplate>
    </ContentPage.Resources>

    <StackLayout Margin="10"
                 BindableLayout.ItemsSource="{Binding People}"
                 BindableLayout.ItemTemplate="{StaticResource PersonTemplate}" />
</ContentPage>
```

In diesem Beispiel wird eine `PeopleViewModel`-Instanz für die `BindingContext`-Eigenschaft der Seite festgelegt. Dieses ViewModel-Element stellt eine `People`-Sammlung und eine `ICommand`-Schnittstelle namens `DeletePersonCommand` zur Verfügung. Die [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse der Seite verwendet ein bindbares Layouts, um eine Datenbindung mit der `People`-Sammlung herzustellen. Die `ItemTemplate`-Eigenschaft des bindbaren Layouts wird auf die Ressource `PersonTemplate` festgelegt. Diese [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse legt fest, dass alle Elemente in der `People`-Sammlung mithilfe eines `CardView`-Objekts angezeigt werden. Die visuelle Struktur des `CardView`-Objekts wird mithilfe einer [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse namens `CardViewControlTemplate` definiert:

```xaml
<ControlTemplate x:Key="CardViewControlTemplate">
    <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
           BackgroundColor="{Binding CardColor}"
           BorderColor="{Binding BorderColor}"
           CornerRadius="5"
           HasShadow="True"
           Padding="8"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="75" />
                <RowDefinition Height="4" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Label Text="{Binding CardTitle}"
                   FontAttributes="Bold"
                   FontSize="Large"
                   VerticalTextAlignment="Center"
                   HorizontalTextAlignment="Start" />
            <BoxView Grid.Row="1"
                     BackgroundColor="{Binding BorderColor}"
                     HeightRequest="2"
                     HorizontalOptions="Fill" />
            <Label Grid.Row="2"
                   Text="{Binding CardDescription}"
                   VerticalTextAlignment="Start"
                   VerticalOptions="Fill"
                   HorizontalOptions="Fill" />
            <Button Text="Delete"
                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeletePersonCommand}"
                    CommandParameter="{Binding CardTitle}"
                    HorizontalOptions="End" />
        </Grid>
    </Frame>
</ControlTemplate>
```

In diesem Beispiel handelt es sich beim Stammelement von [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) um ein [`Frame`](xref:Xamarin.Forms.Frame)-Objekt. Das Objekt `Frame` verwendet die Markuperweiterung `RelativeSource`, um das übergeordnete Element mit Vorlagen für `BindingContext` festzulegen. Die Bindungsausdrücke des `Frame`-Objekts und dessen untergeordneten Elemente werden mit `CardView`-Eigenschaften aufgelöst, weil sie `BindingContext` vom `Frame`-Stammelement erben. In den folgenden Screenshots wird die Seite veranschaulicht, die die `People`-Sammlung anzeigt, die aus drei Elementen besteht:

[![Screenshot: CardView-Objekte mit Vorlagen unter iOS und Android](control-template-images/viewmodel-controltemplate.png "CardView-Objekte mit Vorlagen")](control-template-images/viewmodel-controltemplate-large.png#lightbox "CardView-Objekte mit Vorlagen")

Während die Objekte in [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) Eigenschaften nur an das übergeordnete Element mit Vorlagen binden, wird die [`Button`](xref:Xamarin.Forms.Button)-Klasse in der Steuerelementvorlage sowohl an das übergeordnete Element mit Vorlagen als auch an die `DeletePersonCommand`-Eigenschaft im ViewModel-Element gebunden. Dies liegt daran, dass Eigenschaft `Button.Command` ihre Bindungsquelle als Bindungskontext des Vorgängers neu definiert, dessen Bindungskontexttyp `PeopleViewModel` ist. Dabei handelt es sich um die [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse. Der Abschnitt `Path` der Bindungsausdrücke kann dann die `DeletePersonCommand`-Eigenschaft auflösen. Die `Button.CommandParameter`-Eigenschaft ändert jedoch nicht ihre Bindungsquelle, stattdessen erbt sie sie vom übergeordneten Element in der [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)-Klasse. Daher wird die `CommandParameter`-Eigenschaft an die `CardTitle`-Eigenschaft von `CardView` gebunden.

Der Gesamteffekt der [`Button`](xref:Xamarin.Forms.Button)-Bindungen besteht darin, dass `DeletePersonCommand` in der `PeopleViewModel`-Klasse ausgeführt wird, wenn auf das `Button`-Element getippt wird, wodurch der Wert der `CardName`-Eigenschaft an `DeletePersonCommand` übergeben wird. Dies führt wiederum dazu, dass das angegebene `CardView`-Element aus dem bindbaren Layout entfernt wird:

[![Screenshot: CardView-Objekte mit Vorlagen unter iOS und Android](control-template-images/viewmodel-itemdeleted.png "CardView-Objekte mit Vorlagen")](control-template-images/viewmodel-itemdeleted-large.png#lightbox "CardView-Objekte mit Vorlagen")

Weitere Informationen zu relativen Bindungen finden Sie unter [Relative Bindungen in Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

## <a name="related-links"></a>Verwandte Links

- [Beispiel für ControlTemplateDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplatedemos)
- [Xamarin.Forms-ContentView](~/xamarin-forms/user-interface/layouts/contentview.md)
- [Relative Bindungen in Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)
- [Ressourcenverzeichnisse in Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms-Formatvorlagen](~/xamarin-forms/user-interface/styles/index.md)
