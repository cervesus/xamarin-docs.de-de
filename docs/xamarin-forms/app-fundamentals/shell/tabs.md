---
title: Xamarin.Forms-Shell-Layout
description: Nach einem Flyout ist die nächste Navigationsebene in einer Shell-Anwendung die untere Registerkartenleiste. Alternativ kann das Navigationsmuster für eine Anwendung mit unteren Registerkarten beginnen und auf die Verwendung eines Flyouts verzichten. Wenn eine untere Registerkarte mehrere Seiten enthält, sind die Seiten in beiden Fällen über Register am oberen Rand navigierbar.
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2019
ms.openlocfilehash: cd3bfd9186c87594fc42702e2d62b33e68973db6
ms.sourcegitcommit: 10b4ccbfcf182be940899c00fc0fecae1e199c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252293"
---
# <a name="xamarinforms-shell-tabs"></a>Xamarin.Forms-Shell-Registerkarten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Wenn das Navigationsmuster für eine Anwendung einen Flyout beinhaltet, ist die nächste Navigationsebene in der Anwendung die untere Registerkartenleiste. Außerdem kann die untere Registerkartenleiste als oberste Ebene der Navigation betrachtet werden, wenn das Flyout geschlossen ist.

Alternativ kann das Navigationsmuster für eine Anwendung mit unteren Registerkarten beginnen und auf die Verwendung eines Flyouts verzichten. In diesem Szenario sollte das untergeordnete Objekt des `Shell`-Objekts ein `TabBar`-Objekt sein, das die untere Registerkartenleiste darstellt.

> [!NOTE]
> Der `TabBar`-Typ deaktiviert den Flyout.

Jedes `FlyoutItem`- oder `TabBar`-Objekt kann ein oder mehrere `Tab`-Objekte enthalten, wobei jedes `Tab`-Objekt eine Registerkarte auf der unteren Registerkartenleiste darstellt. Jedes `Tab`-Objekt kann ein oder mehrere `ShellContent`-Objekte enthalten, und jedes `ShellContent`-Objekt zeigt ein einzelnes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt an. Wenn mehr als ein `ShellContent`-Objekt in einem `Tab`-Objekt vorhanden ist, sind die `ContentPage`-Objekte über obere Registerkarten navigierbar.

Innerhalb jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekts kann zu zusätzlichen `ContentPage`-Objekten navigiert werden. Weitere Informationen zur Navigation finden Sie unter [Navigation in der Xamarin.Forms Shell](navigation.md).

## <a name="single-page-application"></a>Single-Page-Webanwendung

Die einfachste Shell-Anwendung ist eine Single-Page-Webanwendung, die durch Hinzufügen eines einzelnen `Tab`-Objekts zu einem `TabBar`-Objekt erstellt werden kann. Im `Tab`-Objekt sollte ein `ShellContent`-Objekt auf ein [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt festgelegt werden:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Dieses Codebeispiel führt zu der folgenden Single-Page-Webanwendung:

[![Screenshot einer Shell-Einzelseiten-App unter iOS und Android](tabs-images/single-page-app.png "Shell-Einzelseiten-App")](tabs-images/single-page-app-large.png#lightbox "Shell-Einzelseiten-App")

> [!NOTE]
> Die Navigationsleiste kann bei Bedarf ausgeblendet werden, indem die angefügte Eigenschaft `Shell.NavBarIsVisible` für das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt auf `false` festgelegt wird.

Shell verfügt über implizite Konvertierungsoperatoren, die es ermöglichen, die visuelle Hierarchie der Shell zu vereinfachen, ohne zusätzliche Ansichten in die visuelle Struktur einzufügen. Dies ist möglich, weil ein `Shell`-Objekt mit Unterklassen immer nur `FlyoutItem`-Objekte oder ein `TabBar`-Objekt enthalten darf, die immer nur `Tab`-Objekte enthalten dürfen, die wiederum immer nur `ShellContent`-Objekte enthalten dürfen. Diese impliziten Konvertierungsoperatoren können verwendet werden, um die Objekte `TabBar`, `Tab` und `ShellContent` aus dem vorherigen Beispiel zu entfernen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

Diese implizite Konvertierung schließt das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt automatisch in einem `ShellContent`-Objekt ein, das in einem `Tab`-Objekt eingeschlossen ist, das wiederum in einem `FlyoutItem`-Objekt eingeschlossen ist. Ein Flyout ist in einer Single-Page-Webanwendung unnötig, und deshalb wird die `Shell.FlyoutBehavior`-Eigenschaft auf `Disabled` festgelegt.

> [!IMPORTANT]
> In einer Shell-Anwendung wird jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das ein untergeordnetes Element eines `ShellContent`-Objekts ist, während des Anwendungsstarts erstellt. Hinzufügen weiterer `ShellContent`-Objekte mit diesem Ansatz führt dazu, dass zusätzliche Seiten während des Anwendungsstarts erstellt werden, was zu einer schlechten Starterfahrung führen kann. Mit der Shell können Seiten jedoch auch nach Bedarf als Reaktion auf die Navigation erstellt werden. Weitere Informationen finden Sie unter [Effizientes Laden von Seiten](tabs.md#efficient-page-loading).

## <a name="bottom-tabs"></a>Untere Registerkarten

`Tab`-Objekte werden als untere Registerkarten dargestellt, vorausgesetzt, dass es mehrere `Tab`-Objekte in einem einzigen `TabBar`-Objekt gibt:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Registerkartentitel und -symbole werden für die einzelnen `Tab`-Objekte festgelegt und auf den unteren Registerkarten angezeigt:

[![Screenshot einer Shell-Two-Page-App mit unteren Registerkarten unter iOS und Android](tabs-images/two-page-app-bottom-tabs.png "Shell-Two-Page-App mit unteren Registerkarten")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "Shell-Two-Page-App mit unteren Registerkarten")

Alternativ können die impliziten Shell-Konvertierungsoperatoren verwendet werden, um die Objekte `ShellContent` und `Tab` aus dem vorherigen Beispiel zu entfernen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </TabBar>
</Shell>
```

Diese implizite Konvertierung schließt jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt automatisch in einem `ShellContent`-Objekt ein, die dann gemeinsam in einem `Tab`-Objekt eingeschlossen werden.

> [!IMPORTANT]
> In einer Shell-Anwendung wird jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das ein untergeordnetes Element eines `ShellContent`-Objekts ist, während des Anwendungsstarts erstellt. Hinzufügen weiterer `ShellContent`-Objekte mit diesem Ansatz führt dazu, dass zusätzliche Seiten während des Anwendungsstarts erstellt werden, was zu einer schlechten Starterfahrung führen kann. Mit der Shell können Seiten jedoch auch nach Bedarf als Reaktion auf die Navigation erstellt werden. Weitere Informationen finden Sie unter [Effizientes Laden von Seiten](tabs.md#efficient-page-loading).

### <a name="tab-class"></a>Tab-Klasse

Die `Tab`-Klasse umfasst die folgenden Eigenschaften, die die Darstellung und das Verhalten der Registerkarte steuern:

- `CurrentItem` vom Typ `ShellContent`: das ausgewählte Element.
- `FlyoutDisplayOptions` vom Typ `FlyoutDisplayOptions`: Definiert, wie das Element und seine untergeordneten Elemente im Flyout angezeigt werden. Der Standardwert ist `AsSingleItem`sein.
- `FlyoutIcon` vom Typ `ImageSource`: Definiert das Symbol, das im Flyout angezeigt wird.
- `Icon` vom Typ `ImageSource`: Definiert das Symbol, das in Teilen des Chroms angezeigt wird, die nicht das Flyout sind.
- `IsChecked` vom Typ `boolean`: Definiert, ob das Element im Flyout derzeit ausgewählt ist.
- `IsEnabled` vom Typ `boolean`: Definiert, ob das Element im Chrom ausgewählt werden kann.
- `IsTabStop` vom Typ `bool`: Gibt an, ob ein `Tab`-Objekt in der Tabstoppnavigation enthalten ist. Der Standardwert ist `true`, und wenn er `false` ist, wird das `Tab` von der Infrastruktur der Tabstoppnavigation ignoriert, unabhängig davon, ob ein `TabIndex` festgelegt ist.
- `Items` vom Typ `IList<ShellContent>`: Definiert alle Inhalte in einem `Tab`-Objekt.
- `TabIndex` vom Typ `int`: Gibt an, in welcher Reihenfolge die `Tab`-Objekte den Fokus erhalten, wenn der Benutzer durch Drücken der TAB-TASTE durch Elemente navigiert. Der Standardwert der Eigenschaft ist 0.
- `Title` vom Typ `string`: der Titel, der auf der Registerkarte in der Benutzeroberfläche angezeigt wird.

## <a name="shell-content"></a>Shell-Inhalt

Das untergeordnete Element jedes `Tab`-Objekts ist ein `ShellContent`-Objekt, dessen `Content`-Eigenschaft auf eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) festgelegt ist:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Innerhalb jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekts kann zu zusätzlichen `ContentPage`-Objekten navigiert werden. Weitere Informationen zur Navigation finden Sie unter [Navigation in der Xamarin.Forms Shell](navigation.md).

### <a name="shellcontent-class"></a>ShellContent-Klasse

Die `ShellContent`-Klasse umfasst die folgenden Eigenschaften, die die Darstellung und das Verhalten des Registerkarteninhalts steuern:

- `Content` vom Typ `object`: der Inhalt des `ShellContent`-Objekts.
- `ContentTemplate` vom Typ `DataTemplate`: die Vorlage, die zum dynamischen Vergrößern des Inhalts des `ShellContent`-Objekts verwendet wird.
- `FlyoutIcon` vom Typ `ImageSource`: Definiert das Symbol, das im Flyout angezeigt wird.
- `Icon` vom Typ `ImageSource`: Definiert das Symbol, das in Teilen des Chroms angezeigt wird, die nicht das Flyout sind.
- `IsChecked` vom Typ `boolean`: Definiert, ob das Element im Flyout derzeit ausgewählt ist.
- `IsEnabled` vom Typ `boolean`: Definiert, ob das Element im Chrom ausgewählt werden kann.
- `MenuItems` vom Typ `MenuItemCollection`: die Menüelemente, die im Flyout angezeigt werden, wenn dieses `ShellContent`-Objekt die angezeigte Seite ist.
- `Title` vom Typ `string`: der Titel, der in der Benutzeroberfläche angezeigt wird.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

## <a name="bottom-and-top-tabs"></a>Untere und obere Registerkarten

Wenn mehr als ein `ShellContent`-Objekt in einem `Tab`-Objekt vorhanden ist, wird der unteren Registerkarte eine obere Registerkartenleiste hinzugefügt, mit deren Hilfe durch die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekte navigiert werden kann:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Dies ergibt das in den folgenden Screenshots gezeigte Layout:

[![Screenshot einer Shell-Two-Page-App mit oberen und unteren Registerkarten unter iOS und Android](tabs-images/two-page-app-top-tabs.png "Shell-Two-Page-App mit oberen und unteren Registerkarten")](tabs-images/two-page-app-top-tabs-large.png#lightbox "Shell-Two-Page-App mit oberen und unteren Registerkarten")

Alternativ können die impliziten Shell-Konvertierungsoperatoren verwendet werden, um die `ShellContent`-Objekte und das zweite `Tab`-Objekt aus dem vorherigen Beispiel zu entfernen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </TabBar>
</Shell>
```

Diese implizite Konvertierung schließt `MonkeysPage` automatisch in einem `ShellContent`-Objekt ein, das dann in einem `Tab`-Objekt eingeschlossen wird. Darüber hinaus werden `CatsPage` und `DogsPage` implizit in `ShellContent`-Objekten eingeschlossen.

## <a name="efficient-page-loading"></a>Effizientes Laden von Seiten

In einer Shell-Anwendung wird jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt in einem `ShellContent`-Objekt während des Anwendungsstarts erstellt, was zu einem schlechten Starterlebnis führen kann. Shell erlaubt es jedoch auch, Seiten bei Bedarf als Reaktion auf die Navigation zu erstellen. Dies erreichen Sie indem Sie die `DataTemplate`-Markuperweiterung, verwenden, um jedes `ContentPage`-Objekt in ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Objekt zu konvertieren, und das Ergebnis anschließend als `ShellContent.ContentTemplate`-Eigenschaftswert festlegen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">    
    <TabBar>
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
    </TabBar>    
</Shell>
```

Mit dieser XAML wird `CatsPage` erstellt und angezeigt, da sie das erste Element des im `Shell`-Objekt mit Unterklassen deklarierten Inhalts ist. `CatsPage` und `MonkeysPage` können über die unteren Registerkarten aufgerufen werden, und diese Seiten werden nur erstellt, wenn der Benutzer zu ihnen navigiert. Der Vorteil dieses Ansatzes besteht darin, dass die schlechte Starterfahrung vermieden wird, da Seiten bei Bedarf als Reaktion auf die Navigation und nicht beim Starten der Anwendung erstellt werden.

## <a name="tab-appearance"></a>Registerkartendarstellung

Die `Shell`-Klasse definiert die folgenden angefügten Eigenschaften, die die Darstellung von Registerkarten steuern:

- `TabBarBackgroundColor` vom Typ `Color`: Definiert die Hintergrundfarbe für die Registerkartenleiste. Wenn die Eigenschaft nicht festgelegt ist, wird der `BackgroundColor`-Eigenschaftswert verwendet.
- `TabBarDisabledColor` vom Typ `Color`: Definiert die Farbe für „Deaktiviert“ für die Registerkartenleiste. Wenn die Eigenschaft nicht festgelegt ist, wird der `DisabledColor`-Eigenschaftswert verwendet.
- `TabBarForegroundColor` vom Typ `Color`: Definiert die Vordergrundfarbe für die Registerkartenleiste. Wenn die Eigenschaft nicht festgelegt ist, wird der `ForegroundColor`-Eigenschaftswert verwendet.
- `TabBarTitleColor` vom Typ `Color`: Definiert die Titelfarbe für die Registerkartenleiste. Wenn die Eigenschaft nicht festgelegt ist, wird der `TitleColor`-Eigenschaftswert verwendet.
- `TabBarUnselectedColor` vom Typ `Color`: Definiert die Farbe für „Nicht markierte“ für die Registerkartenleiste. Wenn die Eigenschaft nicht festgelegt ist, wird der `UnselectedColor`-Eigenschaftswert verwendet.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein und formatiert werden können.

Das folgende Beispiel zeigt ein XAML-Format, das unterschiedliche Farbeigenschaften für Registerkarten festlegt:

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

Darüber hinaus können Registerkarten auch mit Cascading Stylesheets (CSS) formatiert werden. Weitere Informationen finden Sie unter [Spezifische Eigenschaften der Xamarin.Forms-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Navigation in der Xamarin.Forms-Shell](navigation.md)
- [Spezifische Eigenschaften der Xamarin.Forms CSS-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
