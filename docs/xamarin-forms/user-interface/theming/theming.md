---
Title: "Design a Xamarin.Forms Application" Description: "Design" kann in Anwendungen implementiert werden Xamarin.Forms , indem für jedes Design ein ResourceDictionary erstellt und anschließend die Ressourcen mit der DynamicResource-Markup Erweiterung geladen werden.
ms. Prod: xamarin ms. assetid: B7B17F66-4E37-4B50-9A57-351B62BE4FED ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 08/07/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="theme-a-xamarinforms-application"></a>Design einer- Xamarin.Forms Anwendung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Xamarin.FormsAnwendungen können mithilfe der Markup Erweiterung dynamisch auf Formatänderungen zur Laufzeit reagieren `DynamicResource` . Diese Markup Erweiterung ähnelt der `StaticResource` Markup Erweiterung, da beide eine Wörterbuch Taste verwenden, um einen Wert aus einer abzurufen [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Obwohl die `StaticResource` Markup Erweiterung eine einzelne Wörterbuchsuche ausführt, `DynamicResource` behält die Markup Erweiterung einen Link zum Wörterbuch Schlüssel bei. Wenn der Wert, der dem Schlüssel zugeordnet ist, ersetzt wird, wird die Änderung daher auf das angewendet [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Dies ermöglicht die Implementierung von Lauf Zeit Designs in Xamarin.Forms Anwendungen.

Der Prozess zum Implementieren von Lauf Zeit Designs in einer- Xamarin.Forms Anwendung lautet wie folgt:

1. Definieren Sie die Ressourcen für jedes Design in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .
1. Nutzen Sie Design Ressourcen in der Anwendung mit der `DynamicResource` Markup Erweiterung.
1. Legen Sie ein Standarddesign in der **app. XAML** -Datei der Anwendung fest.
1. Fügen Sie Code hinzu, um ein Design zur Laufzeit zu laden.

> [!IMPORTANT]
> Verwenden `StaticResource` Sie die Markup Erweiterung, wenn Sie das App-Design zur Laufzeit nicht ändern müssen.

Die folgenden Screenshots zeigen Design Seiten, bei denen die IOS-Anwendung ein helles Design und die Android-Anwendung mit einem dunklen Design verwendet:

[![Screenshot der Hauptseite einer APP mit einem Thema unter IOS und Android](theming-images/main-page-both-themes.png "Hauptseite der App "Thema"")](theming-images/main-page-both-themes-large.png#lightbox "Hauptseite der App "Thema"") 
 [ ![Screenshot der Detailseite einer Designs-app unter IOS und Android](theming-images/detail-page-both-themes.png "Detail Seite der APP mit Design")](theming-images/detail-page-both-themes-large.png#lightbox "Detail Seite der APP mit Design")

> [!NOTE]
> Wenn Sie ein Design zur Laufzeit ändern, sind XAML-Stile erforderlich, und es ist derzeit nicht möglich, CSS zu verwenden.

## <a name="define-themes"></a>Definieren von Designs

Ein Design wird als Auflistung von Ressourcen Objekten definiert, die in einem gespeichert sind [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .

Das folgende Beispiel zeigt die `LightTheme` aus der Beispielanwendung:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.LightTheme">
    <Color x:Key="PageBackgroundColor">White</Color>
    <Color x:Key="NavigationBarColor">WhiteSmoke</Color>
    <Color x:Key="PrimaryColor">WhiteSmoke</Color>
    <Color x:Key="SecondaryColor">Black</Color>
    <Color x:Key="PrimaryTextColor">Black</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">Gray</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

Das folgende Beispiel zeigt die `DarkTheme` aus der Beispielanwendung:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.DarkTheme">
    <Color x:Key="PageBackgroundColor">Black</Color>
    <Color x:Key="NavigationBarColor">Teal</Color>
    <Color x:Key="PrimaryColor">Teal</Color>
    <Color x:Key="SecondaryColor">White</Color>
    <Color x:Key="PrimaryTextColor">White</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">WhiteSmoke</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

Jede [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) enthält [`Color`](xref:Xamarin.Forms.Color) Ressourcen, die ihre jeweiligen Designs definieren, wobei jeweils `ResourceDictionary` identische Schlüsselwerte verwendet werden. Weitere Informationen zu Ressourcen Wörterbüchern finden Sie unter [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

> [!IMPORTANT]
> Eine Code Behind-Datei ist für jeden erforderlich `ResourceDictionary` , der die- `InitializeComponent` Methode aufruft. Dies ist erforderlich, damit ein CLR-Objekt, das das ausgewählte Design darstellt, zur Laufzeit erstellt werden kann.

## <a name="set-a-default-theme"></a>Festlegen eines Standarddesigns

Eine Anwendung erfordert ein Standarddesign, sodass Steuerelemente Werte für die Ressourcen aufweisen, die Sie nutzen. Ein Standarddesign kann festgelegt werden, indem das Design [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) in die Anwendungsebene zusammengeführt wird `ResourceDictionary` , die in **app. XAML**definiert ist:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>
        <ResourceDictionary Source="Themes/LightTheme.xaml" />
    </Application.Resources>
</Application>
```

Weitere Informationen zum Zusammenführen von Ressourcen Wörterbüchern finden Sie unter [zusammengeführte Ressourcen](~/xamarin-forms/xaml/resource-dictionaries.md#merged-resource-dictionaries)Verzeichnisse.

## <a name="consume-theme-resources"></a>Verbrauchen von Design Ressourcen

Wenn eine Anwendung eine Ressource nutzen möchte, die in einer gespeichert ist, die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ein Design darstellt, sollte Sie dies mit der `DynamicResource` Markup Erweiterung tun. Dadurch wird sichergestellt, dass die Werte aus dem neuen Design angewendet werden, wenn zur Laufzeit ein anderes Design ausgewählt wird.

Das folgende Beispiel zeigt drei Stile aus der Beispielanwendung, die auf-Objekte angewendet werden können [`Label`](xref:Xamarin.Forms.Label) :

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>

        <Style x:Key="LargeLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource SecondaryTextColor}" />
            <Setter Property="FontSize"
                    Value="30" />
        </Style>

        <Style x:Key="MediumLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource PrimaryTextColor}" />
            <Setter Property="FontSize"
                    Value="25" />
        </Style>

        <Style x:Key="SmallLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource TertiaryTextColor}" />
            <Setter Property="FontSize"
                    Value="15" />
        </Style>

    </Application.Resources>
</Application>
```

Diese Stile werden im Ressourcen Wörterbuch auf Anwendungsebene definiert, sodass Sie von mehreren Seiten genutzt werden können. Jeder Stil nutzt Design Ressourcen mit der `DynamicResource` Markup Erweiterung.

Diese Stile werden dann von Seiten genutzt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ThemingDemo"
             x:Class="ThemingDemo.UserSummaryPage"
             Title="User Summary"
             BackgroundColor="{DynamicResource PageBackgroundColor}">
    ...
    <ScrollView>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="200" />
                <RowDefinition Height="120" />
                <RowDefinition Height="70" />
            </Grid.RowDefinitions>
            <Grid BackgroundColor="{DynamicResource PrimaryColor}">
                <Label Text="Face-Palm Monkey"
                       VerticalOptions="Center"
                       Margin="15"
                       Style="{StaticResource MediumLabelStyle}" />
                ...
            </Grid>
            <StackLayout Grid.Row="1"
                         Margin="10">
                <Label Text="This monkey reacts appropriately to ridiculous assertions and actions."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Cynical but not unfriendly."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Seven varieties of grimaces."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Doesn't laugh at your jokes."
                       Style="{StaticResource SmallLabelStyle}" />
            </StackLayout>
            ...
        </Grid>
    </ScrollView>
</ContentPage>
```

Wenn eine Design Ressource direkt genutzt wird, sollte Sie mit der `DynamicResource` Markup Erweiterung genutzt werden. Wenn jedoch ein Stil verwendet wird, der die `DynamicResource` Markup Erweiterung verwendet, sollte er mit der `StaticResource` Markup Erweiterung verwendet werden.

Weitere Informationen zum Formatieren finden Sie unter Formatieren von [ Xamarin.Forms apps mit XAML-Stilen](~/xamarin-forms/user-interface/styles/xaml/index.md). Weitere Informationen zur `DynamicResource` Markup Erweiterung finden Sie unter [dynamische Stile in Xamarin.Forms ](~/xamarin-forms/user-interface/styles/xaml/dynamic.md).

## <a name="load-a-theme-at-runtime"></a>Ein Design zur Laufzeit laden

Wenn ein Design zur Laufzeit ausgewählt wird, sollte die Anwendung Folgendes ausführen:

1. Entfernen Sie das aktuelle Design aus der Anwendung. Dies wird erreicht, indem die [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) -Eigenschaft der auf Anwendungsebene gelöscht wird [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .
2. Ausgewähltes Designladen. Dies wird erreicht, indem der-Eigenschaft der auf Anwendungsebene eine Instanz des ausgewählten Designs hinzugefügt wird `MergedDictionaries` `ResourceDictionary` .

Alle [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekte, die Eigenschaften mit der `DynamicResource` Markup Erweiterung festlegen, wenden dann die neuen Designwerte an. Dies liegt daran, dass die `DynamicResource` Markup Erweiterung einen Link zu Wörterbuch Schlüsseln beibehält. Wenn also die den Schlüsseln zugeordneten Werte ersetzt werden, werden die Änderungen auf die `VisualElement` Objekte angewendet.

In der Beispielanwendung wird ein Design über eine modale Seite ausgewählt, die eine enthält [`Picker`](xref:Xamarin.Forms.Picker) . Der folgende Code zeigt die- `OnPickerSelectionChanged` Methode, die ausgeführt wird, wenn sich das ausgewählte Design ändert:

```csharp
void OnPickerSelectionChanged(object sender, EventArgs e)
{
    Picker picker = sender as Picker;
    Theme theme = (Theme)picker.SelectedItem;

    ICollection<ResourceDictionary> mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    if (mergedDictionaries != null)
    {
        mergedDictionaries.Clear();

        switch (theme)
        {
            case Theme.Dark:
                mergedDictionaries.Add(new DarkTheme());
                break;
            case Theme.Light:
            default:
                mergedDictionaries.Add(new LightTheme());
                break;
        }
    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Design (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Reagieren auf Systemdesignänderungen](system-theme-changes.md)
- [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile inXamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
