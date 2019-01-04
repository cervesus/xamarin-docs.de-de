---
title: Xamarin.Forms-Shell
description: Dieser Artikel erklärt, wie die Xamarin.Forms-Shell verwendet wird, und wie sich die Komplexität von Xamarin.Forms-Anwendungen reduzieren lässt, indem sie die grundlegenden Funktionen der Benutzeroberfläche bereitstellt, die die meisten Anwendungen benötigen.
ms.prod: xamarin
ms.assetid: 1A674212-72DB-4AA4-B626-A4EC135AD1A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 422311c766584cbd27d0ab0c42adee042e9aac3e
ms.sourcegitcommit: 408b78dd6eded4696469e316af7922a5991f2211
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53246294"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms-Shell

![Vorschau](~/media/shared/preview.png)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/Microsoft/TailwindTraders-Mobile)

Xamarin.Forms-Shell ist ein Container für Anwendungen, der grundlegende UI-Funktionen bereitstellt, die die meisten Anwendungen benötigen, sodass Sie sich auf die Kernaufgaben der Anwendung konzentrieren können. Shell-Anwendungen werden mit den folgenden Funktionen bereitgestellt:

- Einer zentralen Stelle zur Beschreibung der visuellen Struktur einer Anwendung.
- Eine gemeinsame Benutzeroberfläche für die Navigation.
- Ein Navigationsdienst mit Deep Linking.
- Ein integrierter Suchhandler.

Diese Funktion reduziert die Komplexität von Anwendungen und steigert gleichzeitig die Produktivität der Entwickler. Darüber hinaus wurde die Shell unter Berücksichtigung der Renderinggeschwindigkeit und des Speicherverbrauchs geschrieben.

> [!IMPORTANT]
> Bestehende iOS- und Android-Anwendungen können die Shell übernehmen und sofort von Verbesserungen bei Navigation, Leistung und Erweiterbarkeit profitieren.

Shell bietet eine Benutzeroberfläche mit zahlreichen Optionen für die Navigation, basierend auf Flyouts und Registerkarten. Die oberste Ebene der Navigation in Shell-Anwendung ist ein Flyout:

![Flyout](shell-images/flyout.png "Flyout")

Die nächste Ebene der Navigation ist die untere Registerkartenleiste:

![Untere Registerkarten](shell-images/bottom-tabs-full.png "Untere Registerkarten")

Wenn der Flyout nicht geöffnet ist, wird die untere Registerkartenleiste als oberste Ebene der Navigation betrachtet.

Innerhalb jeder unteren Registerkarte ist die nächste Navigationsebene die obere Registerkartenleiste, wobei jede Registerkarte eine `ContentPage` ist:

![Obere Registerkarten](shell-images/top-tabs-full.png "Obere Registerkarten")

Innerhalb jeder `ContentPage` können zusätzliche `ContentPage`-Instanzen zum Navigationsstapel hinzugefügt bzw. daraus entfernt werden.

## <a name="bootstrapping-a-shell-application"></a>Bootstrapping für eine Shell-Anwendung

Der Bootstrap-Vorgang wird für eine Shell-Anwendung durchgeführt, indem die Eigenschaft `MainPage` der `App`-Klasse auf eine Instanz einer Shelldatei festgelegt wird:

```csharp
namespace TailwindTraders.Mobile
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            Forms.SetFlags("Shell_Experimental");
            MainPage = new TheShell();
        }
    }
}
```

Die `TheShell`-Klasse ist ein XAML-Datei, die die visuelle Struktur der Anwendung beschreibt.

> [!IMPORTANT]
> Shell ist derzeit experimentell und kann nur verwendet werden, indem `Forms.SetFlags("Shell_Experimental");` entweder zu Ihrer `App`-Klasse hinzugefügt wird, bevor Sie die `Shell`-Instanz erstellen, oder zu Ihrem Plattformprojekt, bevor Sie die `Forms.Init`-Methode aufrufen.

## <a name="shell-file-hierarchy"></a>Shelldateihierarchie

Eine Shelldatei besteht aus drei hierarchischen Elementen:

- `ShellItem`. Ein oder mehrere Elemente im Flyout. Jedes `ShellItem` ist ein untergeordnetes Element einer `Shell`.
- `ShellSection`. Gruppierte Inhalt, über die unteren Registerkarten navigierbar. Jede `ShellSection` ist ein untergeordnetes Element eines `ShellItem`.
- `ShellContent`. Die `ContentPage`-Instanzen in Ihrer Anwendung, die über die oberen Registerkarten navigierbar sind. Jeder `ShellContent` ist ein untergeordnetes Element einer `ShellSection`.

Keines dieser Elemente repräsentiert eine Benutzeroberfläche, sondern die Organisation der visuellen Struktur der Anwendung. Shell wird diese Elemente übernehmen und die Benutzeroberfläche für die Navigation der Inhalte erstellen.

Die folgende XAML zeigt ein einfaches Beispiel für eine Shelldatei:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>
</Shell>
```

> [!NOTE]
> Jedes `ShellItem` kann auch eine `FlyoutIcon`-Eigenschaft auf eine `ImageSource` festlegen, die links neben dem Titel angezeigt wird.

Dieses XAML-Beispiel zeigt die `HomePage` an, da es das erste Element des in der Shell-Datei deklarierten Inhalts ist:

![Homepage](shell-images/homepage.png "Homepage")

Wenn Sie auf die Hamburger-Schaltfläche klicken, wird der Flyout angezeigt:

![Flyout offen](shell-images/flyout-initial.png "Flyout offen")

Die Anzahl der Elemente im Flyout kann erhöht werden, indem der Shelldatei weitere `ShellItem`-Instanzen hinzugefügt werden. Beachten Sie jedoch, dass `HomePage` während des Anwendungsstarts erstellt wird, und das Hinzufügen weiterer `ShellItem`-Instanzen mit diesem Ansatz führt dazu, dass diese Seiten auch beim Anwendungsstart erstellt werden. Dies kann vermieden werden, indem die `ShellContent.ContentTemplate`-Eigenschaft auf eine `DataTemplate` festgelegt wird:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    <ShellItem>
    <ShellContent Title="Profile"
                  ContentTemplate="{DataTemplate local:ProfilePage}" />
</Shell>
```

Bei diesem Ansatz wird `ProfilePage` nur erstellt, wenn der Benutzer dahin navigiert, und nicht beim Start der Anwendung. Beachten Sie außerdem, dass für `ProfilePage` die Objekte `ShellItem` und `ShellSection` weggelassen werden, wobei die `Title`-Eigenschaft in der `ShellContent`-Instanz definiert ist. Dieser kompakte Ansatz erfordert weniger Markup, da Shell die erforderlichen logischen Wrapper bereitstellt (ohne die visuelle Struktur um zusätzliche Ansichten zu erweitern).

Darüber hinaus kann die Darstellung jedes einzelne `ShellItem` angepasst werden, indem die `Shell.ItemTemplate`-Eigenschaft auf eine `DataTemplate` festgelegt wird:

```xaml
<Shell.ItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Title}" />
        </ContentView>
    </DataTemplate>
</Shell.ItemTemplate>          
```

Dieser Code positioniert den Text einfach für jedes `ShellItem` innerhalb des Flyouts neu. Beachten Sie, dass Shell die Eigenschaften `Title` (und `Icon`) für den `BindingContext` der `DataTemplate` zur Verfügung stellt.

## <a name="flyout"></a>Flyout

Der Flyout ist das Stammmenü für die Anwendung und besteht aus einem Flyout-Header und Menüpunkten:

![Mit Anmerkung versehener Flyout](shell-images/flyout-annotated.png "Mit Anmerkung versehener Flyout")

### <a name="flyout-behavior"></a>Flyout-Verhalten

Der Flyout ist über die Hamburger-Schaltfläche oder durch Streichen von der Seite des Bildschirms aus zugänglich. Allerdings kann dieses Verhalten geändert werden, indem die `Shell.FlyoutBehavior`-Eigenschaft auf eines der `FlyoutBehavior`-Enumerationsmember festgelegt wird:

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

Das Festlegen der `FlyoutBehavior`-Eigenschaft auf `Disabled` blendet den Flyout aus, was sehr hilfreich ist, wenn Sie nur ein `ShellItem` haben. Weitere gültige `FlyoutBehavior`-Werte sind `Flyout` (Standard) und `Locked`.

### <a name="flyout-header"></a>Flyout-Header

Der Flyout-Header ist der Inhalt, der optional oben im Flyout erscheint, wobei seine Darstellung durch einer `View` definiert wird, die über den Eigenschaftswert `Shell.FlyoutHeader` festgelegt werden kann:

```xaml
<Shell.FlyoutHeader>
    <local:FlyoutHeader />
</Shell.FlyoutHeader>
```

Alternativ kann die Darstellung des Flyout-Headers definiert werden, indem die `Shell.FlyoutHeaderTemplate`-Eigenschaft auf eine `DataTemplate` festgelegt wird:

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <StackLayout HorizontalOptions="Fill"
                     VerticalOptions="Fill"
                     BackgroundColor="White"
                     Padding="16">
            <Label FontSize="Medium"
                   Text="Smart Shopping">
                <Label.Margin>
                    <Thickness Left="8" />
                </Label.Margin>
            </Label>
            <Button Image="photo"
                    Text="By taking a photo">
                <Button.Margin>
                    <Thickness Top="16" />
                </Button.Margin>
            </Button>
            <Button Image="ia"
                    Text="By using AR">
                <Button.Margin>
                    <Thickness Top="8" />
                </Button.Margin>
            </Button>
        </StackLayout>
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

Dieses XAML führt zu folgendem Flyout-Header:

![Flyout-Header](shell-images/flyout-header.png "Flyout-Header")

Standardmäßig wird der Flyout-Header im Flyout fixiert, während der Inhalt unten scrollt, wenn genügend Elemente vorhanden sind. Allerdings kann dieses Verhalten geändert werden, indem die `Shell.FlyoutHeaderBehavior`-Eigenschaft auf eines der `FlyoutHeaderBehavior`-Enumerationsmember festgelegt wird:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

Wenn `FlyoutHeaderBehavior` auf `CollapseOnScroll` festgelegt wird, wird der Flyout beim Scrollen reduziert. Die andere gültigen `FlyoutHeaderBehavior`-Werte sind `Default`, `Fixed`, und `Scroll` (mit den Menüelementen scrollen).

## <a name="menu-items"></a>Menüelemente

Die Anzahl der Elemente im Flyout kann durch Hinzufügen weiterer `ShellItem`-Instanzen erhöht werden. Es könne jedoch auch `MenuItem`-Instanzen zum Flyout hinzugefügt werden. Dies ermöglicht Szenarien wie das Navigieren zu einer identischen Seite während der Datenübergabe durch die `MenuItem.CommandParameter`-Eigenschaft.

`MenuItem`-Instanzen sollten auch zur `Shell.MenuItems`-Sammlung hinzugefügt werden:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Views"
       x:Class="TailwindTraders.Shell"
       FlyoutHeaderBehavior="Fixed"
       Title="Tailwind Traders"
       ...>
    <Shell.MenuItems>
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="1"
                  Text="Holiday decorations" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="2"
                  Text="Appliances" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="3"
                  Text="Bathrooms" />
        ...
    </Shell.MenuItems>
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>    
</Shell>
```

> [!NOTE]
> Jedes `MenuItem` kann auch eine `Icon`-Eigenschaft auf eine `FileImageSource` festlegen, die links neben dem Text angezeigt wird.

Dieses XAML führt zu folgendem Flyout:

![Vollständiger Flyout](shell-images/flyout-full.png "vollständiger Flyout")

Darüber hinaus kann die Darstellung jedes `MenuItem` angepasst werden, indem die `Shell.MenuItemTemplate`-Eigenschaft auf eine `DataTemplate` festgelegt wird:

```xaml
<Shell.MenuItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Text}"
                   FontAttributes="Bold" />
        </ContentView>
    </DataTemplate>
</Shell.MenuItemTemplate>
```

Dies führt zu `MenuItem`-Instanzen, bei denen der Text fett gerendert ist:

![Fett dargestellte Menüelemente](shell-images/menuitems-bold.png "Fett dargestellte Menüelemente")

## <a name="tabs"></a>Registerkarten

`ShellSection`-Instanzen werden als untere Registerkarten dargestellt, vorausgesetzt, dass es mehrere `ShellSection`-Instanzen in einem einzigen `ShellItem` gibt:

```xaml
<ShellItem Title="Bottom Tab Sample"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="AR" Icon="ia.png">
        <ShellContent ContentTemplate="{DataTemplate local:ARPage}"/>
    </ShellSection>
    <ShellSection Title="Photo" Icon="photo.png">
        <ShellContent ContentTemplate="{DataTemplate local:PhotoPage}"/>
    </ShellSection>
</ShellItem>
```

In diesem Beispiel werden die `ShellSection`-Instanzen als untere Registerkarten gerendert:

![Untere Registerkarten](shell-images/tabs-bottom.png "Untere Registerkarten")

`ShellContent`-Elemente werden als obere Registerkarten dargestellt, vorausgesetzt, dass es mehrere `ShellContent`-Instanzen in einem einzigen `ShellSection` gibt:

```xaml
<ShellItem Title="Store Home"
           Shell.TitleView="Store Home"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="Browse Product">
        <ShellContent Title="Featured"
                      ContentTemplate="{DataTemplate local:FeaturedPage}" />
        <ShellContent Title="On Sale"
                      ContentTemplate="{DataTemplate local:SalePage}" />
    </ShellSection>
</ShellItem>
```

In diesem Beispiel werden die `ShellContent`-Instanzen als obere Registerkarten gerendert:

![Obere Registerkarten](shell-images/tabs-top.png "Obere Registerkarten")

Registerkarten können mit XAML-Stilen oder durch die Bereitstellung eines benutzerdefinierten Renderers formatiert werden. Das folgende Beispiel zeigt beispielsweise einen XAML-Stil, der die Farbe der Registerkarte festlegt:

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.ShellTabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.ShellTabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.ShellTabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

## <a name="navigation"></a>Navigation

Shell bietet eine URI-basierte Navigation. URIs bieten ein verbessertes Navigationserlebnis, das die Navigation zu jeder Seite in der Anwendung ermöglicht, ohne einer festgelegten Navigationshierarchie folgen zu müssen. Darüber hinaus bietet es auch die Möglichkeit, rückwärts zu navigieren, ohne alle Seiten des Navigationsstapels aufrufen zu müssen.

Diese URI-basierte Navigation wird mit Routen durchgeführt. Dies sind URI-Segmente, die zur Navigation innerhalb der Anwendung verwendet werden. Die Shelldatei muss ein Routenschema, einen Routenhost und eine Route deklarieren:

```xaml
<Shell ...
       Route="tailwindtraders"
       RouteHost="www.microsoft.com"
       RouteScheme="app">
    ...
</Shell>
```

In Kombination bilden die Eigenschaftswerte `RouteScheme`, `RouteHost` und `Route` den `app://www.microsoft.com/tailwindtraders`-Stamm-URI.

Jedes Element der Shelldatei kann auch eine Routeneigenschaft definieren, die für die programmatische Navigation verwendet werden kann.

Im Shelldatei-Konstruktor oder an jedem anderen Ort, der vor dem Aufruf einer Route ausgeführt wird, können zusätzliche Routen explizit für alle Seiten registriert werden, die nicht durch ein Shellelement repräsentiert werden (z.B. `MenuItem`-Instanzen):

```csharp
Routing.RegisterRoute("productcategory", typeof(ProductCategoryPage));
```

### <a name="implementing-navigation"></a>Implementieren der Navigation

Menüpunkte zeigen eine `Command`-Eigenschaft an, mit der die Navigation implementiert werden kann:

```csharp
public ICommand ProductTypeCommand { get; } = new Command<string>(NavigateToProductType);

static void NavigateToProductType(string typeId)
{
  (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"app:///tailwindtraders/productcategory?id={typeId}", true);
}
```

Um die Navigation aufzurufen, muss ein Verweis auf die Anwendung `Shell` über die `MainPage`-Eigenschaft der `Application`-Klasse abgerufen werden. Anschließend kann die Navigation durch Abrufen der `GoToAsync`-Methode aufgerufen werden, wobei ein gültiger URI als Argument übergeben wird. Die `GoToAsync`-Methode navigiert mit einem `ShellNavigationState`-Objekt, das aus einem `string` oder einer `Uri` erstellt wird.

### <a name="passing-data"></a>Übergeben von Daten

Daten können über Abfragezeichenfolgen-Parameter zwischen den Seiten übergeben werden. Shell legt die Abfragezeichenfolgen-Parameter auf `ContentPage` oder ViewModel fest, wenn Sie der Klasse Abfrageeigenschaftsattribute hinzufügen:

```csharp
[QueryProperty("TypeID", "id")]
public partial class ProductCategoryPage : ContentPage
{
    private string _typeId;

    public ProductCategoryPage()
    {
        InitializeComponent();

        BindingContext = new ProductCategoryViewModel();
    }

    public string TypeID
    {
        get => _typeId;
        set => MyLabel.Text = value;
    }
}
```

Das `QueryProperty`-Attribut legt fest, dass die `TypeID` die im `id`-Abfragezeichenfolgen-Parameter übergebenen Daten vom URI im `GoToAsync`-Methodenaufruf erhält.

### <a name="intercepting-navigation"></a>Abfangen der Navigation

Shell bietet die Möglichkeit, in das Navigationsrouting einzugreifen, bevor und nachdem es abgeschlossen ist. Dies kann durch die Registrierung von Ereignishandler für die Ereignisse `Navigating` und `Navigated` erreicht werden:

```xaml
<Shell ...
       Navigating="OnShellNavigating">
    ...
</Shell>
```

```csharp
void OnShellNavigating(object sender, ShellNavigatingEventArgs e)
{
  if (...)
  {
    e.Cancel(); // Cancel the navigation
  }
}
```

Die `ShellNavigatingEventArgs`-Klasse bietet folgende Eigenschaften:

| Eigenschaft | Typ | Beschreibung |
|---|---|---|
| Aktuell | `ShellNavigationState` | Der URI der aktuellen Seite. |
| Quelle | `ShellNavigationState` | Der URI, der darstellt, woher die Navigation stammt. |
| Target | `ShellNavigationSource`  | Der URI, der darstellt, wohin die Navigation führt. |
| CanCancel  | `bool` | Ein Wert, der angibt, ob ein Abbruch der Navigation möglich ist. |
| Cancelled  | `bool` | Ein Wert, der angibt, ob die Navigation abgebrochen wurde. |

Darüber hinaus bietet die `ShellNavigatingEventArgs`-Klasse eine `Cancel`-Methode.

Die `ShellNavigatedEventArgs`-Klasse bietet folgende Eigenschaften:

| Eigenschaft | Typ | Beschreibung |
|---|---|---|
| Aktuell | `ShellNavigationState` | Der URI der aktuellen Seite. |
| Vorheriges| `ShellNavigationState` | Der URI der vorherigen Seite. |
| Quelle  | `ShellNavigationSource` | Der URI, der darstellt, woher die Navigation stammt. |

Darüber hinaus bietet Shell eine überschreibbare `OnBackButtonPressed`-Methode, mit der eine Betätigung der Zurück-Schaltfläche abgefangen werden kann.

## <a name="related-links"></a>Verwandte Links

- [Tailwind-Händler (Beispiel)](https://github.com/Microsoft/TailwindTraders-Mobile)
