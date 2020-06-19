---
title: 'title: "Xamarin.Forms Shell-Seitenkonfiguration" description: "Die „Shell“-Klasse definiert angefügte Eigenschaften, die verwendet werden können, um die Darstellung von Seiten in Xamarin.Forms-Shell-Anwendungen zu konfigurieren.'
description: 'Dies schließt das Festlegen von Seitenfarben, das Deaktivieren der Navigationsleiste, das Deaktivieren der Registerkartenleiste und das Anzeigen von Ansichten in der Navigationsleiste ein." ms.prod: xamarin ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/29/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 102070fc478b42e9fbc0c7d0006197c81a49c9b8
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84137500"
---
# <a name="xamarinforms-shell-page-configuration"></a>Xamarin.Forms-Shell-Seitenkonfiguration

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Die `Shell`-Klasse definiert angefügte Eigenschaften, die verwendet werden können, um die Darstellung von Seiten in Xamarin.Forms-Shell-Anwendungen zu konfigurieren. Dies schließt das Festlegen von Seitenfarben und des Seitenpräsentationsmodus, das Deaktivieren der Navigationsleiste und der Registerkartenleiste sowie das Anzeigen von Ansichten auf der Navigationsleiste ein.

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

## <a name="set-page-presentation-mode"></a>Festlegen des Seitenpräsentationsmodus

Standardmäßig erfolgt eine kleine Navigationsanimation, wenn mit der `GoToAsync`-Methode zu einer Seite navigiert wird. Allerdings kann dieses Verhalten geändert werden, indem die angefügte `Shell.PresentationMode`-Eigenschaft für [`ContentPage`](xref:Xamarin.Forms.ContentPage) auf eines der `PresentationMode`-Enumerationsmember festgelegt wird:

- `NotAnimated` gibt an, dass die Seite ohne Navigationsanimation angezeigt wird.
- `Animated` gibt an, dass die Seite mit Navigationsanimation angezeigt wird. Dies ist der Standardwert der angefügten `Shell.PresentationMode`-Eigenschaft.
- `Modal` gibt an, dass die Seite als modale Seite angezeigt wird.
- `ModalAnimated` gibt an, dass die Seite als modale Seite mit Navigationsanimation angezeigt wird.
- `ModalNotAnimated` gibt an, dass die Seite als modale Seite ohne Navigationsanimation angezeigt wird.

> [!IMPORTANT]
> Der Typ `PresentationMode` ist eine Flagenumeration. Dies bedeutet, dass im Code eine Kombination von Enumerationsmembern angewendet werden kann. Für mehr Benutzerfreundlichkeit in XAML ist das `ModalAnimated`-Member jedoch eine Kombination der Member `Animated` und `Modal` und das `ModalNotAnimated`-Member eine Kombination der Member `NotAnimated` und `Modal`. Weitere Informationen zu Flagenumerationen finden Sie unter [Enumerationstypen als Bitflags](/dotnet/csharp/language-reference/builtin-types/enum#enumeration-types-as-bit-flags).

Im folgenden XAML-Beispiel wird die angefügte `Shell.PresentationMode`-Eigenschaft für eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) festgelegt:

```xaml
<ContentPage ...
             Shell.PresentationMode="Modal">
    ...             
</ContentPage>
```

In diesem Beispiel wird die [`ContentPage`](xref:Xamarin.Forms.ContentPage) so festgelegt, dass sie als modale Seite angezeigt wird, wenn mit der `GoToAsync`-Methode zur Seite navigiert wird.

## <a name="enable-navigation-bar-shadow"></a>Aktivieren des Navigationsleistenschattens

Die `Shell`-Klasse definiert die angefügte Eigenschaft `NavBarHasShadow` vom Typ `bool`, die steuert, ob die Navigationsleiste einen Schatten hat. Standardmäßig lautet der Wert der Eigenschaft `false` unter iOS und `true` unter Android.

Diese Eigenschaft kann zwar auf einem `Shell`-Objekt mit Unterklassen festgelegt werden, kann aber auch auf allen Seiten festgelegt werden, die den Schatten der Navigationsleiste aktivieren möchten. Im folgenden XAML-Code wird zum Beispiel das Aktivieren des Navigationsleistenschattens durch eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) gezeigt:

```xaml
<ContentPage ...
             Shell.NavBarHasShadow="true">
    ...
</ContentPage>
```

Dies führt dazu, dass der Schatten der Navigationsleiste aktiviert wird.

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

![Screenshot der Shell-Seite mit einer Titelansicht unter iOS und Android](configuration-images/titleview.png "Shell-Seite mit Titelansicht")

> [!IMPORTANT]
> Wenn die Navigationsleiste mit der angefügten `NavBarIsVisible`-Eigenschaft ausgeblendet wurde, wird die Titelansicht nicht angezeigt.

Viele Ansichten werden nicht in der Navigationsleiste angezeigt, es sei denn, die Größe der Ansicht wird mit den Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) angegeben oder der Ort der Ansicht wird mit den Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) angegeben.

Da die [`Layout`](xref:Xamarin.Forms.Layout)-Klasse von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet ist, kann die angefügte `TitleView`-Eigenschaft so festgelegt werden, dass sie eine Layoutklasse anzeigt, die mehrere Ansichten enthält. Entsprechend gilt, dass die angefügte `TitleView`-Eigenschaft so festgelegt werden kann, dass sie eine `ContentView` anzeigt, die eine einzelne Ansicht enthält, weil die [`ContentView`](xref:Xamarin.Forms.ContentView)-Klasse letztendlich von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet ist.

## <a name="page-visibility"></a>Seitensichtbarkeit

Shell beachtet die mit der [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)-Eigenschaft festgelegte Seitensichtbarkeit. Wenn die `IsVisible`-Eigenschaft einer Seite auf `false` festgelegt ist, wird sie daher in der Shell-Anwendung nicht angezeigt, und es ist nicht möglich, zu ihr zu navigieren.

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Spezifische Eigenschaften der Xamarin.Forms CSS-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
