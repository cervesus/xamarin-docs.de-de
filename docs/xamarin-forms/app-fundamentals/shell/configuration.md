---
title: Xamarin.Forms-Shell-Seitenkonfiguration
description: Die Shell-Klasse definiert angefügte Eigenschaften, die verwendet werden können, um die Darstellung von Seiten in Xamarin.Forms-Shell-Anwendungen zu konfigurieren. Dies schließt das Festlegen von Seitenfarben, das Deaktivieren der Navigationsleiste, das Deaktivieren der Registerkartenleiste und das Anzeigen von Ansichten in der Navigationsleiste ein.
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
ms.openlocfilehash: 186e9af867e89c0130822593bb0d7ab071ee0505
ms.sourcegitcommit: 10b4ccbfcf182be940899c00fc0fecae1e199c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252414"
---
# <a name="xamarinforms-shell-page-configuration"></a>Xamarin.Forms-Shell-Seitenkonfiguration

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Die `Shell`-Klasse definiert angefügte Eigenschaften, die verwendet werden können, um die Darstellung von Seiten in Xamarin.Forms-Shell-Anwendungen zu konfigurieren. Dies schließt das Festlegen von Seitenfarben, das Deaktivieren der Navigationsleiste, das Deaktivieren der Registerkartenleiste und das Anzeigen von Ansichten in der Navigationsleiste ein.

## <a name="set-page-colors"></a>Festlegen der Seitenfarben

Die `Shell`-Klasse definiert die folgenden angefügten Eigenschaften, mit denen Sie Seitenfarben in einer Shell-Anwendung festlegen können:

- `BackgroundColor` vom Typ `Color`: Definiert die Hintergrundfarbe im Shell-Chrom. Die Farbe wird hinter dem Shell-Inhalt nicht ausgefüllt.
- `DisabledColor` vom Typ `Color`: Definiert die Farbe zum Schattieren von Text und deaktivierten Symbolen.
- `ForegroundColor` vom Typ `Color`: Definiert die Farbe zum Schattieren von Text und Symbolen.
- `TitleColor` vom Typ `Color`: Definiert die Farbe für den Titel der aktuellen Seite.
- `UnselectedColor` vom Typ `Color`: Definiert die Farbe für nicht ausgewählten Text und Symbole im Shell-Chrom.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein und unter Verwendung von XAML-Formatvorlagen formatiert werden können. Darüber hinaus können die Eigenschaften mithilfe von Cascading Stylesheets (CSS) festgelegt werden. Weitere Informationen finden Sie unter [Spezifische Eigenschaften der Xamarin.Forms-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

> [!NOTE]
> Es gibt auch Eigenschaften, mit denen Sie Registerkartenfarben definieren können. Weitere Informationen finden Sie unter [Darstellung von Registerkarten](tabs.md#tab-appearance).

Der folgende XAML-Code verdeutlicht das Festlegen der Farbeigenschaften in einer `Shell`-Klasse mit Unterklassen:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell"
       BackgroundColor="#455A64"
       ForegroundColor="White"
       TitleColor="White"
       DisabledColor="#B4FFFFFF"
       UnselectedColor="#95FFFFFF">

</Shell>
```

In diesem Beispiel werden die Farbwerte auf alle Seiten in der Shell-Anwendung angewendet, sofern sie nicht auf Seitenebene überschrieben werden.

Da die Farbeigenschaften angefügte Eigenschaften sind, können sie auch für einzelne Seiten festgelegt werden, um die Farben auf der entsprechenden Seite festzulegen:

```xaml
<ContentPage ...
             Shell.BackgroundColor="Gray"
             Shell.ForegroundColor="White"
             Shell.TitleColor="Blue"
             Shell.DisabledColor="#95FFFFFF"
             Shell.UnselectedColor="#B4FFFFFF">

</ContentPage>
```

Alternativ können die Farbeigenschaften mit einer XAML-Formatvorlage festgelegt werden:

```xaml
<Style x:Key="DomesticShell"
       TargetType="Element" >
    <Setter Property="Shell.BackgroundColor"
            Value="#039BE6" />
    <Setter Property="Shell.ForegroundColor"
            Value="White" />
    <Setter Property="Shell.TitleColor"
            Value="White" />
    <Setter Property="Shell.DisabledColor"
            Value="#B4FFFFFF" />
    <Setter Property="Shell.UnselectedColor"
            Value="#95FFFFFF" />
</Style>
```

Weitere Informationen zu XAML-Formatvorlagen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md).

## <a name="disable-the-navigation-bar"></a>Deaktivieren der Navigationsleiste

Die `Shell`-Klasse definiert die angefügte `NavBarIsVisible`-Eigenschaft vom Typ `bool` die steuert, ob die Navigationsleiste eingeblendet werden soll, wenn eine Seite angezeigt wird. Der Standardwert der Eigenschaft lautet `true`.

Diese Eigenschaft kann zwar auf ein `Shell`-Objekt mit Unterklassen festgelegt werden, wird aber typischerweise auf allen Seiten festgelegt, auf denen die Navigationsleiste ausgeblendet werden soll. Im folgenden XAML-Code wird zum Beispiel die Navigationsleiste von einem [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt deaktiviert:

```xaml
<ContentPage ...
             Shell.NavBarIsVisible="false">
    ...
</ContentPage>
```

Dadurch wird die Navigationsleiste unsichtbar, wenn die Seite angezeigt wird:

![Screenshot der Shell-Seite mit einer unsichtbaren Navigationsleiste unter iOS und Android](configuration-images/navigationbar-invisible.png "Shell-Seite mit unsichtbarer Navigationsleiste")

## <a name="disable-the-tab-bar"></a>Deaktivieren der Registerkartenleiste

Die `Shell`-Klasse definiert die angefügte `TabBarIsVisible`-Eigenschaft vom Typ `bool` die steuert, ob die Registerkartenleiste eingeblendet werden soll, wenn eine Seite angezeigt wird. Der Standardwert der Eigenschaft lautet `true`.

Diese Eigenschaft kann zwar auf einem `Shell`-Objekt mit Unterklassen festgelegt werden, wird aber typischerweise auf allen Seiten festgelegt, auf denen die Registerkartenleiste ausgeblendet werden soll. Im folgenden XAML-Code wird zum Beispiel die Registerkartenleiste von einem [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt deaktiviert:

```xaml
<ContentPage ...
             Shell.TabBarIsVisible="false">
    ...
</ContentPage>
```

Dadurch wird die Registerkartenleiste unsichtbar, wenn die Seite angezeigt wird:

![Screenshot der Shell-Seite mit einer unsichtbaren Registerkartenleiste unter iOS und Android](configuration-images/tabbar-invisible.png "Shell-Seite mit unsichtbarer Registerkartenleiste")

## <a name="display-views-in-the-navigation-bar"></a>Anzeigen von Ansichten in der Navigationsleiste

Die `Shell`-Klasse definiert die angefügte `TitleView`-Eigenschaft vom Typ `View`, mit der jedes Xamarin.Forms-[`View`](xref:Xamarin.Forms.View)-Objekt in der Navigationsleiste angezeigt werden kann.

Diese Eigenschaft kann zwar auf einem `Shell`-Objekt mit Unterklassen festgelegt werden, kann aber auch auf allen Seiten festgelegt werden, auf denen eine Ansicht in der Navigationsleiste angezeigt werden soll. Im folgenden XAML-Code wird zum Beispiel ein [`Image`](xref:Xamarin.Forms.Image)-Objekt in der Navigationsleiste einer [`ContentPage`](xref:Xamarin.Forms.ContentPage) angezeigt:

```xaml
<ContentPage ...>
    <Shell.TitleView>
        <Image Source="xamarin_logo.png"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Shell.TitleView>
    ...
</ContentPage>
```

Dies führt dazu, dass eine Bild in der Navigationsleiste auf der Seite angezeigt wird:

![Screenshot des Shell-Seite mit einer Titelansicht unter iOS und Android](configuration-images/titleview.png "Shell-Seite mit einer Titelansicht")

> [!IMPORTANT]
> Wenn die Navigationsleiste mit der angefügten `NavBarIsVisible`-Eigenschaft ausgeblendet wurde, wird die Titelansicht nicht angezeigt.

Viele Ansichten werden nicht in der Navigationsleiste angezeigt, es sei denn, die Größe der Ansicht wird mit den Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) angegeben oder der Ort der Ansicht wird mit den Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) angegeben.

Da die [`Layout`](xref:Xamarin.Forms.Layout)-Klasse von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet ist, kann die angefügte `TitleView`-Eigenschaft so festgelegt werden, dass sie eine Layoutklasse anzeigt, die mehrere Ansichten enthält. Entsprechend gilt, dass die angefügte `TitleView`-Eigenschaft so festgelegt werden kann, dass sie eine `ContentView` anzeigt, die eine einzelne Ansicht enthält, weil die [`ContentView`](xref:Xamarin.Forms.ContentView)-Klasse letztendlich von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet ist.

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Spezifische Eigenschaften der Xamarin.Forms CSS-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
