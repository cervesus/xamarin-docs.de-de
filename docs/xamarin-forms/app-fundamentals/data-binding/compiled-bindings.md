---
title: Xamarin.Forms kompiliert, Bindungen
description: In diesem Artikel wird erläutert, wie kompilierte Bindungen verwendet, um die Bindung datenleistung in Xamarin.Forms-Anwendungen zu verbessern.
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2018
ms.openlocfilehash: 0b350082c834076a1d69427644259087d64bf26a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111524"
---
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms kompiliert, Bindungen

_Kompilierte Bindungen werden schneller als bei klassischen Bindungen, optimieren die Bindung die modellleistung in Xamarin.Forms-Anwendungen aus diesem Grund aufgelöst._

Zwei Hauptproblemen wird über datenbindungen verfügen:

1. Es gibt keine Validierung während der Kompilierung von Bindungsausdrücken. Stattdessen werden die Bindungen zur Laufzeit aufgelöst. Aus diesem Grund sind keine ungültige Bindungen erst zur Laufzeit erkannt, wenn die Anwendung verhält sich nicht wie erwartet oder Fehlermeldungen angezeigt werden.
1. Sie sind nicht kosteneffizient. Bindungen werden zur Laufzeit mithilfe der allgemeinen objektüberprüfung (Reflektion) aufgelöst, und der Aufwand für die dies variiert je nach Plattform.

Kompilierte Bindungen Verbessern der datenleistung über die Bindung in Xamarin.Forms-Anwendungen durch das Auflösen der von Bindungsausdrücken zur Kompilierzeit statt der Runtime. Darüber hinaus kann diese Überprüfung während der Kompilierung von Bindungsausdrücken einen besseren Entwickler, die Benutzeroberfläche zur Problembehandlung, da ungültige Bindungen als Buildfehler gemeldet werden.

Der Prozess für die Verwendung von kompilierter Bindungen werden:

1. Aktivieren der XAML-Kompilierung. Weitere Informationen zu XAML-Kompilierung, finden Sie unter [XAML-Kompilierung](~/xamarin-forms/xaml/xamlc.md).
1. Legen Sie eine `x:DataType` Attribut eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) in den Typ des Objekts, die `VisualElement` und seine untergeordneten Elemente verbindet sich mit. Beachten Sie, dass dieses Attribut an jedem Ort in einer Hierarchie von Inhaltsansichten neu definiert werden kann.

> [!NOTE]
> Es wird empfohlen, legen Sie die `x:DataType` -Attributs auf der gleichen Ebene in der Hierarchie von Inhaltsansichten als die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) festgelegt ist.

Zum Zeitpunkt der XAML-Kompilierung werden alle Ausdrücke ungültige Bindung als Buildfehler gemeldet. Der XAML-Compiler meldet jedoch nur einen Buildfehler an, für den ersten Ausdruck für die ungültige Bindung, die es trifft. Gültige Bindung-Ausdrücke, die auf definierten die `VisualElement` oder seine untergeordneten Elemente werden kompilierte, unabhängig davon, ob die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) in XAML oder Code festgelegt ist. Kompilieren von einem Bindungsausdruck generiert kompilierten Code, der einen Wert aus einer Eigenschaft erhält, auf die *Quelle*, und legen Sie ihn für die Eigenschaft auf die *Ziel* , die im Markup angegeben ist. Je nach den Bindungsausdruck, der generierte Code möglicherweise Beachten Sie außerdem Änderungen am Wert von der *Quelle* -Eigenschaft und die Aktualisierung der *Ziel* -Eigenschaft, und kann Änderungen aus der *Ziel* zurück an die *Quelle*.

> [!IMPORTANT]
> Kompilierte Bindungen sind für alle Bindungsausdrücke, die definieren, derzeit deaktiviert die [ `Source` ](xref:Xamarin.Forms.Binding.Source) Eigenschaft. Grund hierfür ist die `Source` Eigenschaft ist immer festgelegt mit der `x:Reference` Markuperweiterung, die zum Zeitpunkt der Kompilierung nicht aufgelöst werden kann.

## <a name="using-compiled-bindings"></a>Verwenden von kompilierten Bindungen

Die **Farbauswahl kompiliert** Seite veranschaulicht die Verwendung von kompilierter Bindungen zwischen Xamarin.Forms-Ansichten und ViewModel-Eigenschaften:

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

Der Stamm [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) instanziiert die `HslColorViewModel` und initialisiert die `Color` Eigenschaft im Eigenschaftenelement-Tags für die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft. Dieses Stammverzeichnis `StackLayout` definiert auch die `x:DataType` Attribut als der ViewModel-Typ, der angibt, dass alle Bindungsausdrücke im Stammverzeichnis `StackLayout` Hierarchie von Inhaltsansichten kompiliert wird. Dies kann durch Ändern der die Bindungsausdrücke auf eine nicht existierende "ViewModel"-Eigenschaft gebunden, was zu einem Buildfehler führt überprüft werden.

> [!IMPORTANT]
> Die `x:DataType` Attribut kann zu einem beliebigen Zeitpunkt in eine Hierarchie von Inhaltsansichten neu definiert werden.

Die [ `BoxView` ](xref:Xamarin.Forms.BoxView), [ `Label` ](xref:Xamarin.Forms.Label) Elemente und [ `Slider` ](xref:Xamarin.Forms.Slider) Ansichten erben den Bindungskontext aus der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Die Sichten befinden sich alle Bindungsziele, die Eigenschaften der Quelle in das "ViewModel" verweisen. Für die [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) -Eigenschaft, und die [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) -Eigenschaft, die datenbindungen werden `OneWay` – die Eigenschaften in der Ansicht werden von den Eigenschaften in "ViewModel" festgelegt. Allerdings die [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) -Eigenschaft verwendet einen `TwoWay` Bindung. Dadurch kann jede `Slider` festgelegt werden, aus dem ViewModel sowie für das "ViewModel" aus jedem festzulegende `Slider`.

Wenn die Anwendung zuerst ausgeführt wird, die [ `BoxView` ](xref:Xamarin.Forms.BoxView), [ `Label` ](xref:Xamarin.Forms.Label) Elemente und [ `Slider` ](xref:Xamarin.Forms.Slider) Elemente können aus "ViewModel" basierend auf der ursprüngliche `Color` -Eigenschaft festgelegt wird, wenn das "ViewModel" instanziiert wurde. Dies wird in den folgenden Screenshots gezeigt:

[![Kompiliert die Farbauswahl](compiled-bindings-images/compiledcolorselector-small.png "kompiliert Farbauswahl")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "kompiliert Farbauswahl")

Wenn der Schieberegler bearbeitet werden, die [ `BoxView` ](xref:Xamarin.Forms.BoxView) und [ `Label` ](xref:Xamarin.Forms.Label) Elemente werden entsprechend aktualisiert.

Weitere Informationen zu dieser Farbauswahl, finden Sie unter [ViewModels und Benachrichtigungen für Eigenschaftsänderungen](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications).

## <a name="using-compiled-bindings-in-a-datatemplate"></a>Verwenden von kompilierten Bindungen in eine DataTemplate-Element

Bindungen in eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) im Kontext des Objekts auf Vorlagen basierenden interpretiert werden. Aus diesem Grund, wenn mit kompiliert Bindungen in eine `DataTemplate`, `DataTemplate` muss zur Deklaration des Typs, der die Daten mithilfe der `x:DataType` Attribut.

Die **Farbenliste kompiliert** Seite veranschaulicht die Verwendung von kompilierter Bindungen in eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

Die [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ListView) -Eigenschaftensatz auf die statische `NamedColor.All` Eigenschaft. Die `NamedColor` Klasse verwendet .NET Reflection zum Aufzählen aller statischen öffentlichen Feldes in der [ `Color` ](xref:Xamarin.Forms.Color) Struktur, und sie mit ihren Namen in einer Sammlung zu speichern, die aus der statischen zugänglich ist `All` Eigenschaft. Aus diesem Grund die `ListView` gefüllt mit allen dem `NamedColor` Instanzen. Für jedes Element in der `ListView`, der Bindungskontext für das Element festgelegt ist, um eine `NamedColor` Objekt. Die [ `BoxView` ](xref:Xamarin.Forms.BoxView) und [ `Label` ](xref:Xamarin.Forms.Label) Elemente in der [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) gebunden sind, um `NamedColor` Eigenschaften.

Beachten Sie, dass die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) definiert die `x:DataType` Attribut der `NamedColor` Typ, der angibt, die von jedem Bindungsausdrücke in die `DataTemplate` Hierarchie von Inhaltsansichten kompiliert wird. Dies kann überprüft werden, durch Ändern der die Bindungsausdrücke zum Binden an eine nicht vorhandene `NamedColor` -Eigenschaft, die zu einem Buildfehler führt.

Wenn die Anwendung zuerst ausgeführt wird, die [ `ListView` ](xref:Xamarin.Forms.ListView) mit aufgefüllt `NamedColor` Instanzen. Wenn ein Element in der `ListView` ausgewählt ist, die [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) -Eigenschaftensatz auf die Farbe des ausgewählten Elements in der `ListView`:

[![Kompilierte Farbenliste](compiled-bindings-images/compiledcolorlist-small.png "kompilierte Farbenliste]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

Auswählen von anderen Elementen in der [ `ListView` ](xref:Xamarin.Forms.BoxView) aktualisiert die Farbe der [ `BoxView` ](xref:Xamarin.Forms.BoxView).

## <a name="combining-compiled-bindings-with-classic-bindings"></a>Kombinieren von kompiliert Bindungen mit klassischen Bindungen

Bindungsausdrücke werden nur kompiliert, für die Hierarchie von Inhaltsansichten, die die `x:DataType` -Attribut auf definiert ist. Im Gegensatz dazu alle Ansichten in einer Hierarchie, die sich auf dem die `x:DataType` Attribut ist nicht definiert klassischen Bindungen verwenden. Es ist daher möglich, kompilierte Bindungen und klassischen Bindungen auf einer Seite zu kombinieren. Beispielsweise im vorherigen Abschnitt die Ansichten im der [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) kompilierten Bindungen, während die [ `BoxView` ](xref:Xamarin.Forms.BoxView) , festgelegt ist, der die Farbe, die im ausgewählten der [ `ListView` ](xref:Xamarin.Forms.ListView) nicht.

Eine sorgfältige Strukturierung von `x:DataType` Attribute daher zu einer Seite, die mithilfe von kompilierter und klassischen Bindungen führen können. Alternativ die `x:DataType` Attribut kann zu einem beliebigen Zeitpunkt in eine Hierarchie von Inhaltsansichten, neu definiert sein `null` mithilfe der `x:Null` Markuperweiterung. Dadurch wird angegeben, dass alle Bindungsausdrücke in der Hierarchie von Inhaltsansichten klassischen Bindungen. Die *gemischten Bindungen* Seite veranschaulicht diesen Ansatz:

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

Der Stamm [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) legt diese fest der `x:DataType` Attribut der `HslColorViewModel` Typ, der angibt, die von jedem Bindungsausdruck im Stammverzeichnis `StackLayout` Hierarchie von Inhaltsansichten kompiliert wird. Allerdings die innere `StackLayout` definiert die `x:DataType` Attribut `null` mit der `x:Null` Markup-Ausdruck. Aus diesem Grund die Bindungsausdrücke in der inneren `StackLayout` klassischen Bindungen verwenden. Nur die [ `BoxView` ](xref:Xamarin.Forms.BoxView), innerhalb des Stamms `StackLayout` Hierarchien, kompiliert verwendet Bindungen anzeigen.

Weitere Informationen zu den `x:Null` Markup-Ausdruck, finden Sie unter [X: Null Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#null).

## <a name="performance"></a>Leistung

Kompilierte Bindungen verbessern die Leistung mit der Leistung Vorteile variieren für die Datenbindung. Zeigt, dass Komponententests:

- Einer kompilierten Bindung, die Benachrichtigung für eigenschaftsänderungen verwendet (d. h. eine `OneWay`, `OneWayToSource`, oder `TwoWay` Bindung) ungefähr 8 Mal schneller als eine klassische Bindung aufgelöst wird.
- Einer kompilierten Bindung, die keine Benachrichtigung für eigenschaftsänderungen verwendet (d. h. eine `OneTime` Bindung) ungefähr 20-Mal schneller als eine klassische Bindung aufgelöst wird.
- Festlegen der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) änderungsbenachrichtigung in einer kompilierten Bindung, die Eigenschaft verwendet (d. h. eine `OneWay`, `OneWayToSource`, oder `TwoWay` Bindung) ist ungefähr 5-Mal schneller als das Festlegen der `BindingContext`auf einer klassischen Bindung.
- Festlegen der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) änderungsbenachrichtigung in einer kompilierten Bindung, die Eigenschaft nicht verwendet (d. h. eine `OneTime` Bindung) beträgt ca. 7-Mal schneller als die Einstellung der `BindingContext` auf einer klassischen Bindung.

Diese Leistungsunterschiede können auf mobilen Geräten, die abhängig von der Plattform, die verwendet wird, die Version des Betriebssystems verwendet wird und das Gerät, auf dem die Anwendung ausgeführt wird, vergrößert werden.

## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
