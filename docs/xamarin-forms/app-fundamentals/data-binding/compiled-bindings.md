---
title: Kompilierte Xamarin.Forms-Bindungen
description: In diesem Artikel wird erläutert, wie Sie kompilierte Bindungen verwenden können, um die Leistung der Datenbindung in Xamarin.Forms-Anwendungen zu verbessern.
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2019
ms.openlocfilehash: 531d9719eb4bf5c23001ebe4260254e13f9989eb
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697161"
---
# <a name="xamarinforms-compiled-bindings"></a>Kompilierte Xamarin.Forms-Bindungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

_Kompilierte Bindungen werden schneller gelöst als klassische Bindungen, wodurch die Leistung der Datenbindung in Xamarin.Forms-Anwendungen verbessert wird._

Datenbindungen weisen zwei Hauptprobleme auf:

1. Es gibt keine Validierung von Bindungsausdrücken zur Kompilierzeit. Stattdessen werden Bindungen zur Laufzeit aufgelöst. Daher werden ungültige Bindungen erst zur Laufzeit ermittelt, wenn die Anwendung nicht erwartungsgemäß funktioniert oder Fehlermeldungen angezeigt werden.
1. Sie sind nicht kosteneffizient. Bindungen werden zur Laufzeit mithilfe Universeller Objektüberprüfung (Reflektion) aufgelöst. Der durch diese Methode entstehende Mehraufwand variiert von Plattform zu Plattform.

Kompilierte Bindungen verbessern die Datenbindungsleistung in Xamarin.Forms-Anwendungen, indem die Bindungsausdrücke zur Kompilierzeit anstatt zur Laufzeit aufgelöst werden. Darüber hinaus vereinfacht die Validierung von Bindungsausdrücken zur Kompilierzeit die Problembehandlung durch den Entwickler, da ungültige Bindungen als Buildfehler gemeldet werden.

Sie können kompilierte Bindungen wie folgt verwenden:

1. Aktivieren Sie die XAML-Kompilierung. Weitere Informationen zur XAML-Kompilierung finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).
1. Legen Sie ein `x:DataType`-Attribut für ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) auf den Typ des Objekts fest, an das `VisualElement` und dessen untergeordnete Elemente gebunden werden.

> [!NOTE]
> Es wird empfohlen, das `x:DataType`-Attribut auf der gleichen Ebene in der Ansichtshierarchie festzulegen, auf der die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft festgelegt ist. Dieses Attribut kann jedoch an jeder Position in einer Ansichtshierarchie neu definiert werden.

Wenn Sie kompilierte Bindungen verwenden möchten, muss das Attribut `x:DataType` auf ein Zeichenfolgenliteral oder einen Typ festgelegt sein, der die Markuperweiterung `x:Type` verwendet. Zur Kompilierzeit des XAML-Codes werden jegliche ungültige Bindungsausdrücke als Buildfehler gemeldet. Der XAML-Compiler meldet einen Buildfehler jedoch nur für den ungültigen Bindungsausdruck, der als erstes ermittelt wird. Alle gültigen Bindungsausdrücke, die im `VisualElement`-Element oder in dessen untergeordneten Elemente definiert sind, werden unabhängig davon, ob [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) in der XAML-Datei oder im Code festgelegt ist, kompiliert. Das Kompilieren eines Bindungsausdrucks generiert kompilierten Code, der einen Wert von einer Eigenschaft in der *Quelle* abruft und diesen für die Eigenschaft des *Ziels* festlegt, das im Markup angegeben ist. Je nach Bindungsausdruck wird außerdem der Wert der Eigenschaft *source* geändert und die Eigenschaft *target* aktualisiert. Möglicherweise werden Änderungen vom *Ziel* zurück an die *Quelle* übertragen.

> [!IMPORTANT]
> Kompilierte Bindungen sind derzeit für alle Bindungsausdrücke deaktiviert, die die [`Source`](xref:Xamarin.Forms.Binding.Source)-Eigenschaft definieren. Der Grund dafür ist, dass die `Source`-Eigenschaft immer mit der Markuperweiterung `x:Reference` festgelegt wird, die zur Kompilierzeit nicht aufgelöst werden kann.

## <a name="use-compiled-bindings"></a>Verwenden kompilierter Bindungen

Die Seite **Kompilierte Farbauswahl** veranschaulicht die Verwendung von kompilierten Bindungen zwischen Xamarin.Forms-Ansichten und ViewModel-Eigenschaften:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorSelectorPage"
             Title="Compiled Color Selector">
    ...
    <StackLayout x:DataType="local:HslColorViewModel">
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>
        <BoxView Color="{Binding Color}"
                 ... />
        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />
            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>    
</ContentPage>
```

Der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Stamm instanziiert das `HslColorViewModel`-Objekt und initialisiert die `Color`-Eigenschaft innerhalb der Eigenschaftselementtags für die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft. Dieser `StackLayout`-Stamm definiert außerdem das `x:DataType`-Attribut als ViewModel-Typ, der angibt, dass alle Bindungsausdrücke in der Ansichtshierarchie des `StackLayout`-Stamms kompiliert werden. Dies können Sie überprüfen, indem Sie einen der Bindungsausdrücke so ändern, dass er an eine nicht vorhandene ViewModel-Eigenschaft gebunden wird. Dies führt zu einem Buildfehler. In diesem Beispiel wird das `x:DataType`-Attribut auf ein Zeichenfolgenliteral festgelegt. Sie können es jedoch auch auf einen Typ mit der Markuperweiterung `x:Type` festlegen. Weitere Informationen zur Markuperweiterung `x:Type` finden Sie unter [x:Type-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#type).

> [!IMPORTANT]
> Das `x:DataType`-Attribut kann jederzeit in einer Ansichtshierarchie neu definiert werden.

Die [`BoxView`](xref:Xamarin.Forms.BoxView)- und [`Label`](xref:Xamarin.Forms.Label)-Elemente sowie die [`Slider`](xref:Xamarin.Forms.Slider)-Ansichten erben den Bindungskontext von [`StackLayout`](xref:Xamarin.Forms.StackLayout). Alle diese Ansichten sind Bindungsziele, die auf Quelleneigenschaften in der ViewModel-Klasse verweisen. Für die Eigenschaften [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) und [`Label.Text`](xref:Xamarin.Forms.Label.Text) sind die Datenbindungen `OneWay`. Die Eigenschaften in der Ansicht werden über die Eigenschaften in der ViewModel-Klasse festgelegt. Die Eigenschaft [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) verwendet jedoch eine `TwoWay`-Bindung. Dadurch kann jedes `Slider`-Element über ViewModel und die ViewModel-Klasse über jedes `Slider`-Element festgelegt werden.

Bei der ersten Ausführung der Anwendung werden alle [`BoxView`](xref:Xamarin.Forms.BoxView)-, [`Label`](xref:Xamarin.Forms.Label)- und [`Slider`](xref:Xamarin.Forms.Slider)-Elemente über die ViewModel-Klasse basierend auf der ursprünglichen `Color`-Eigenschaft festgelegt, die bei der Instanziierung von ViewModel festgelegt wurde. Dies wird im folgenden Screenshot veranschaulicht:

[![Kompilierte Farbauswahl](compiled-bindings-images/compiledcolorselector-small.png "Kompilierte Farbauswahl")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "Kompilierte Farbauswahl")

Wenn die Schieberegler bewegt werden, werden die Elemente [`BoxView`](xref:Xamarin.Forms.BoxView) und [`Label`](xref:Xamarin.Forms.Label) entsprechend aktualisiert.

Weitere Informationen zu dieser Farbauswahl finden Sie unter [ViewModels and Property-Change Notifications (Benachrichtigungen für Änderungen an ViewModels und Eigenschaften)](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications).

## <a name="use-compiled-bindings-in-a-datatemplate"></a>Verwenden von kompilierten Bindungen in einer DataTemplate-Klasse

Bindungen in einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse werden abhängig vom Kontext des Objekts interpretiert, auf dem die Vorlage basiert. Daher muss die `DataTemplate`-Klasse den Typ ihres Datenobjekts mithilfe des `x:DataType`-Attributs deklarieren, wenn kompilierte Bindungen in einer `DataTemplate`-Klasse verwendet werden.

Die Seite **Compiled Color List** (Kompilierte Farbliste) veranschaulicht die Verwendung kompilierter Bindungen in einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorListPage"
             Title="Compiled Color List">
    <Grid>
        ...
        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  ... >
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:NamedColor">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     ... />
                            <Label Text="{Binding FriendlyName}"
                                   ... />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <!-- The BoxView doesn't use compiled bindings -->
        <BoxView Color="{Binding Source={x:Reference colorListView}, Path=SelectedItem.Color}"
                 ... />
    </Grid>
</ContentPage>
```

Für die Eigenschaft [`ListView.ItemsSource`](xref:Xamarin.Forms.ListView) wird die statische Eigenschaft `NamedColor.All` festgelegt. Die `NamedColor`-Klasse nutzt die .NET-Reflektion, um alle statischen öffentlichen Felder in der [`Color`](xref:Xamarin.Forms.Color)-Struktur aufzuführen und sie mitsamt ihrer Namen in einer Sammlung zu speichern, auf die über die statische Eigenschaft `All` zugegriffen werden kann. Aus diesem Grund wird `ListView` mit allen Instanzen von `NamedColor` aufgefüllt. Der Bindungskontext aller Elemente in der `ListView`-Klasse wird auf ein `NamedColor`-Objekt festgelegt. Die Elemente [`BoxView`](xref:Xamarin.Forms.BoxView) und [`Label`](xref:Xamarin.Forms.Label) in der [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Klasse werden an `NamedColor`-Eigenschaften gebunden.

Beachten Sie, dass [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) den Typ `NamedColor` für das `x:DataType`-Attribut definiert, d.h., dass alle Bindungsausdrücke in der Ansichtshierarchie `DataTemplate` kompiliert werden. Dies können Sie überprüfen, indem Sie einen der Bindungsausdrücke so ändern, dass eine nicht vorhandene `NamedColor`-Eigenschaft gebunden wird, was zu einem Buildfehler führt.  In diesem Beispiel wird das `x:DataType`-Attribut auf ein Zeichenfolgenliteral festgelegt. Sie können es jedoch auch auf einen Typ mit der Markuperweiterung `x:Type` festlegen. Weitere Informationen zur Markuperweiterung `x:Type` finden Sie unter [x:Type-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#type).

Wenn die Anwendung zum ersten Mal ausgeführt wird, wird die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse mit `NamedColor`-Instanzen aufgefüllt. Wenn ein Element in der `ListView`-Klasse ausgewählt wird, wird die Farbe des ausgewählten Elements für die [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color)-Eigenschaft in der `ListView`-Klasse festgelegt:

[![Kompilierte Farbliste](compiled-bindings-images/compiledcolorlist-small.png "Kompilierte Farbliste]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

Wenn andere Elemente in der [`ListView`](xref:Xamarin.Forms.BoxView)-Klasse ausgewählt werden, wird die Farbe der [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse aktualisiert.

## <a name="combine-compiled-bindings-with-classic-bindings"></a>Kombinieren von kompilierten und klassischen Bindungen

Bindungsausdrücke werden nur für die Ansichtshierarchie kompiliert, für die das Attribut `x:DataType` definiert ist. Im Gegensatz dazu, verwenden alle Ansichten in einer Hierarchie, für die das Attribut `x:DataType` nicht definiert ist, klassische Bindungen. Daher ist es möglich, kompilierte Bindungen und klassische Bindungen auf einer Seite zu kombinieren. Zum Beispiel verwenden die Ansichten in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse im vorherigen Abschnitt kompilierte Bindungen, während die [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse, die auf die in [`ListView`](xref:Xamarin.Forms.ListView) ausgewählte Farbe festgelegt ist, klassische Bindungen verwendet.

Mithilfe sorgfältiger Strukturierung von `x:DataType`-Attributen kann deshalb eine Seite mit kompilierten und klassischen Bindungen erstellt werden. Alternativ kann das Attribut `x:DataType` jederzeit und an beliebiger Position in einer Ansichtshierarchie mithilfe der Markuperweiterung `x:Null` als `null` definiert werden. Dadurch wird impliziert, dass alle Bindungsausdrücke in der Ansichtshierarchie klassische Bindungen nutzen. Die Seite *Mixed Bindings* (Gemischte Bindungen) veranschaulicht diese Vorgehensweise:

```xaml
<StackLayout x:DataType="local:HslColorViewModel">
    <StackLayout.BindingContext>
        <local:HslColorViewModel Color="Sienna" />
    </StackLayout.BindingContext>
    <BoxView Color="{Binding Color}"
             VerticalOptions="FillAndExpand" />
    <StackLayout x:DataType="{x:Null}"
                 Margin="10, 0">
        <Label Text="{Binding Name}" />
        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</StackLayout>   
```

Der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Stamm legt den Typ `HslColorViewModel` für das Attribut `x:DataType` fest, d.h., dass alle Bindungsausdrücke im `StackLayout`-Stamm der Ansichtshierarchie kompiliert werden. Das Attribut `x:DataType` wird jedoch von der inneren `StackLayout`-Klasse mithilfe des Markupausdrucks `x:Null` mit dem Wert `null` neu definiert. Daher verwenden Bindungsausdrücke in der inneren `StackLayout`-Klasse klassische Bindungen. Nur die [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse im `StackLayout`-Stamm der Ansichtshierarchie verwendet kompilierte Bindungen.

Weitere Informationen über den Markupausdruck `x:Null` finden Sie unter [x:Null Markup Extension (x:Null-Markuperweiterung)](~/xamarin-forms/xaml/markup-extensions/consuming.md#null).

## <a name="performance"></a>Leistung

Kompilierte Bindungen verbessern die Leistung der Datenbindung mit variierenden Leistungsvorteilen. Unittests zeigen folgende Ergebnisse:

- Eine kompilierte Bindung, die eine Eigenschaftsänderungsbenachrichtigung verwendet (z.B. eine `OneWay`-, `OneWayToSource`- oder `TwoWay`-Bindung), wird ungefähr achtmal so schnell wie eine klassische Bindung aufgelöst.
- Eine kompilierte Bindung, die keine Eigenschaftsänderungsbenachrichtigung verwendet (z.B. eine `OneTime`-Bindung) wird ungefähr 20-mal so schnell wie eine klassische Bindung aufgelöst.
- Das Festlegen der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft für eine kompilierte Bindung, die eine Eigenschaftsänderungsbenachrichtigung verwendet (z.B. eine `OneWay`-, `OneWayToSource`- oder `TwoWay`-Bindung), wird ungefähr fünfmal so schnell wie eine auf `BindingContext` festgelegte klassische Bindung aufgelöst.
- Das Festlegen der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft für eine kompilierte Bindung, die keine Eigenschaftsänderungsbenachrichtigung verwendet (z.B. eine `OneTime`-Bindung), wird ungefähr siebenmal so schnell wie eine auf `BindingContext` festgelegte klassische Bindung aufgelöst.

Diese Leistungsunterschiede können bei mobilen Geräten abhängig von der verwendeten Plattform, der Version des verwendeten Betriebssystems und dem Gerät, auf dem die Anwendung ausgeführt wird, gesteigert werden.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
