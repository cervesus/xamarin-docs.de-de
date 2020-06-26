---
title: Mehrfachbindungen in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie eine Sammlung von Binding-Objekten mithilfe der MultiBinding-Klasse an eine einzelne Bindungszieleigenschaft anfügen können.
ms.prod: xamarin
ms.assetid: E73AE622-664C-4A90-B5B2-BD47D0E7A1A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2020
ms.openlocfilehash: dfe6da8a76b447bf0c2a6c0a3bea9823e498d5e4
ms.sourcegitcommit: 8a18471b3d96f3f726b66f9bc50a829f1c122f29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988195"
---
# <a name="xamarinforms-multi-bindings"></a>Mehrfachbindungen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Mehrfachbindungen bieten die Möglichkeit, eine Sammlung von [`Binding`](xref:Xamarin.Forms.Binding)-Objekten an eine einzige Bindungszieleigenschaft anzufügen. Sie werden mit der `MultiBinding`-Klasse erstellt, die alle ihre `Binding`-Objekte auswertet und über eine von Ihrer Anwendung bereitgestellte `IMultiValueConverter`-Instanz einen Einzelwert zurückgibt. Darüber hinaus wertet `MultiBinding` alle seine `Binding`-Objekte neu aus, wenn sich beliebige der gebundenen Daten ändern.

Die `MultiBinding`-Klasse definiert die folgenden Eigenschaften:

- `Bindings` mit dem Typ `IList<BindingBase>`, die die Sammlung von [`Binding`](xref:Xamarin.Forms.Binding)-Objekten innerhalb der `MultiBinding`-Instanz darstellt.
- `Converter` mit dem Typ `IMultiValueConverter`, die den Konverter darstellt, der zum Konvertieren der Quellwerte in oder aus dem Zielwert verwendet wird.
- `ConverterParameter` mit dem Typ `object`, die einen optionalen Parameter darstellt, der an den `Converter` übergeben werden kann.

Die `Bindings`-Eigenschaft ist die Inhaltseigenschaft der `MultiBinding`-Klasse und muss daher nicht explizit in XAML festgelegt werden.

Darüber hinaus erbt die `MultiBinding`-Klasse die folgenden Eigenschaften von der `BindingBase`-Klasse:

- `FallbackValue` mit dem Typ `object`, die den Wert darstellt, der verwendet werden soll, wenn die Mehrfachbindung keinen Wert zurückgeben kann.
- `Mode` mit dem Typ [`BindingMode`](xref:Xamarin.Forms.BindingMode), die die Richtung des Datenflusses der Mehrfachbindung angibt.
- `StringFormat` mit dem Typ `string`, die angibt, wie das Mehrfachbindungsergebnis formatiert werden soll, wenn es als Zeichenfolge angezeigt wird.
- `TargetNullValue` mit dem Typ `object`, die den im Ziel verwendeten Wert darstellt, wenn der Wert der Quelle `null` ist.

`MultiBinding` muss `IMultiValueConverter` nutzen, um einen Wert für das Bindungsziel zu erzeugen, der auf dem Wert der Bindungen in der `Bindings`-Sammlung basiert. So kann [`Color`](xref:Xamarin.Forms.Color) beispielsweise aus roten, blauen und grünen Werten berechnet werden, die aus denselben oder anderen Bindungsquellobjekten stammen können. Wenn sich ein Wert aus dem Ziel zu den Quellen bewegt, wird der Zieleigenschaftswert in eine Gruppe von Werten übersetzt, die in die Bindungen zurückgegeben werden.

> [!IMPORTANT]
> Einzelne Bindungen in der `Bindings`-Sammlung können eigene Wertkonverter haben.

Der Wert der `Mode`-Eigenschaft bestimmt die Funktionalität von `MultiBinding` und wird als Bindungsmodus für alle Bindungen in der Sammlung genutzt, es sei denn, eine einzelne Bindung überschreibt die Eigenschaft. Wenn z. B. die `Mode`-Eigenschaft für ein `MultiBinding`-Objekt auf [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) festgelegt ist, werden alle Bindungen in der Sammlung als `TwoWay` betrachtet, es sei denn, Sie legen für eine der Bindungen explizit einen anderen `Mode`-Wert fest.

## <a name="define-a-imultivalueconverter"></a>Definieren der IMultiValueConverter-Schnittstelle

Die `IMultiValueConverter`-Schnittstelle ermöglicht das Anwenden benutzerdefinierter Logik auf eine `MultiBinding`. Um einen Konverter mit `MultiBinding` zu verknüpfen, erstellen Sie eine Klasse, die die `IValueConverter`-Schnittstelle implementiert, und implementieren Sie dann die Methoden `Convert` und `ConvertBack`:

```csharp
public class AllTrueMultiConverter : IMultiValueConverter
{
    public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
    {
        if (values == null || !targetType.IsAssignableFrom(typeof(bool)))
        {
            return false;
            // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
        }

        foreach (var value in values)
        {
            if (!(value is bool b))
            {
                return false;
                // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
            }
            else if (!b)
            {
                return false;
            }
        }
        return true;
    }

    public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
    {
        if (!(value is bool b) || targetTypes.Any(t => !t.IsAssignableFrom(typeof(bool))))
        {
            // Return null to indicate conversion back is not possible
            return null;
        }

        if (b)
        {
            return targetTypes.Select(t => (object)true).ToArray();
        }
        else
        {
            // Can't convert back from false because of ambiguity
            return null;
        }
    }
}
```

Die `Convert`-Methode konvertiert Quellwerte in einen Wert für das Bindungsziel. Xamarin.Forms ruft diese Methode auf, wenn Werte von Quellbindungen an das Bindungsziel weitergegeben werden. Diese Methode akzeptiert vier Argumente:

- `values` mit dem Typ `object[]` ist ein Array von Werten, die die Quellbindungen in `MultiBinding` erzeugen.
- `targetType` mit dem Typ `Type` ist der Typ der Bindungszieleigenschaft.
- `parameter` mit dem Typ `object` ist der zu verwendende Konverterparameter.
- `culture` mit dem Typ `CultureInfo` ist die im Konverter zu verwendende Kultur.

Die `Convert`-Methode gibt ein `object` zurück, das einen konvertierten Wert darstellt. Diese Methode muss Folgendes zurückgeben:

- `BindableProperty.UnsetValue`, um anzugeben, dass der Konverter keinen Wert produziert hat und dass die Bindung den `FallbackValue` nutzen wird.
- `Binding.DoNothing`, um Xamarin.Forms anzuweisen, keine Aktion auszuführen. Um Xamarin.Forms beispielsweise anzuweisen, keinen Wert an das Bindungsziel zu übertragen oder den `FallbackValue` nicht zu nutzen.
- `null`, um anzugeben, dass der Konverter die Konvertierung nicht ausführen kann und dass die Bindung `TargetNullValue` verwendet.

> [!IMPORTANT]
> Eine `MultiBinding`, die `BindableProperty.UnsetValue` von einer `Convert`.Methode erhält, muss ihre [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue)-Eigenschaft definieren. Ähnlich muss eine `MultiBinding`, die `null` von einer `Convert`-Methode erhält, ihre [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue)-Eigenschaft definieren.

Die `ConvertBack`-Methode konvertiert ein Bindungsziel in die Quellbindungswerte. Diese Methode akzeptiert vier Argumente:

- `value` mit dem Typ `object` ist der Wert, der vom Bindungsziel erzeugt wird.
- `targetTypes` mit dem Typ `Type[]` ist das Array von Typen, in die konvertiert werden soll. Die Arraylänge gibt die Anzahl und Typen von Werten an, die von der Methode zurückgegeben werden sollen.
- `parameter` mit dem Typ `object` ist der zu verwendende Konverterparameter.
- `culture` mit dem Typ `CultureInfo` ist die im Konverter zu verwendende Kultur.

Die `ConvertBack`-Methode gibt ein Array von Werten mit dem Typ `object[]` zurück, die aus dem Zielwert zurück in die Quellwerte konvertiert wurden. Diese Methode muss Folgendes zurückgeben:

- `BindableProperty.UnsetValue` an Position `i`, um anzuzeigen, dass der Konverter nicht in der Lage ist, einen Wert für die Quellbindung beim Index `i` bereitzustellen, und dass kein Wert dafür festgelegt werden soll.
- `Binding.DoNothing` an Position `i`, um anzugeben, dass kein Wert für die Quellbindung bei Index `i` festgelegt werden soll.
- `null`, um anzugeben, dass der Konverter die Konvertierung nicht durchführen kann oder die Konvertierung in diese Richtung nicht unterstützt.

## <a name="consume-a-imultivalueconverter"></a>Nutzen einer IMultiValueConverter-Schnittstelle

Eine `IMultiValueConverter`-Schnittstelle wird genutzt, indem sie in einem Ressourcenwörterbuch instanziiert und dann mit der Markuperweiterung `StaticResource` referenziert wird, um die `MultiBinding.Converter`-Eigenschaft festzulegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MultiBindingConverterPage"
             Title="MultiBinding Converter demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AllTrueConverter}">
                <Binding Path="Employee.IsOver16" />
                <Binding Path="Employee.HasPassedTest" />
                <Binding Path="Employee.IsSuspended"
                         Converter="{StaticResource InverterConverter}" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>    
```

In diesem Beispiel nutzt das `MultiBinding`-Objekt die `AllTrueMultiConverter`-Instanz, um die [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked)-Eigenschaft auf `true` festzulegen, vorausgesetzt, die drei [`Binding`](xref:Xamarin.Forms.Binding)-Objekte werden mit `true` ausgewertet. Andernfalls wird die `CheckBox.IsChecked`-Eigenschaft auf `false` festgelegt.

Standardmäßig verwendet die [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked)-Eigenschaft eine [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)-Bindung. Daher wird die `ConvertBack`-Methode der `AllTrueMultiConverter`-Instanz ausgeführt, wenn [`CheckBox`](xref:Xamarin.Forms.CheckBox) vom Benutzer deaktiviert wird, wodurch die Werte der Quellbindung auf den Wert der `CheckBox.IsChecked`-Eigenschaft festgelegt werden.

## <a name="format-strings"></a>Formatzeichenfolgen

`MultiBinding` kann jedes Mehrfachbindungsergebnis, das als Zeichenfolge angezeigt wird, mit der `StringFormat`-Eigenschaft formatieren. Diese Eigenschaft kann auf eine standardmäßige .NET-Formatierungszeichenfolge mit Platzhaltern festgelegt werden, die angibt, wie das Mehrfachbindungsergebnis formatiert werden soll:

```xaml
<Label>
    <Label.Text>
        <MultiBinding StringFormat="{}{0} {1} {2}">
            <Binding Path="Employee1.Forename" />
            <Binding Path="Employee1.MiddleName" />
            <Binding Path="Employee1.Surname" />
        </MultiBinding>
    </Label.Text>
</Label>
```

In diesem Beispiel kombiniert die `StringFormat`-Eigenschaft die drei gebundenen Werte zu einer einzelnen Zeichenfolge, die von [`Label`](xref:Xamarin.Forms.Label) angezeigt wird.

> [!IMPORTANT]
> Die Anzahl der Parameter in einem zusammengesetzten Zeichenfolgenformat darf die Anzahl der untergeordneten `Binding`-Objekte in `MultiBinding` nicht überschreiten.

Wenn die Eigenschaften `Converter` und `StringFormat` festgelegt werden, wird der Konverter zuerst auf den Datenwert und dann auf `StringFormat` angewendet.

Weitere Informationen zur Zeichenfolgenformatierung in Xamarin.Forms finden Sie unter [ Zeichenfolgenformatierung in Xamarin.Forms](string-formatting.md).

## <a name="provide-fallback-values"></a>Bereitstellen von Fallbackwerten

Datenbindungen können zuverlässiger gestaltet werden, indem Sie Fallbackwerte definieren, die verwendet werden, wenn die Bindung fehlschlägt. Dies kann erreicht werden, indem die Eigenschaften [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) und [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) für ein `MultiBinding`-Objekt definiert werden.

Eine `MultiBinding` nutzt ihren [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue), wenn die `Convert`-Methode einer `IMultiValueConverter`-Instanz `BindableProperty.UnsetValue` zurückgibt, was bedeutet, dass der Konverter keinen Wert erzeugt hat. Eine `MultiBinding` nutzt ihren [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue), wenn die `Convert`-Methode einer `IMultiValueConverter`-Instanz `null` zurückgibt, was bedeutet, dass der Konverter die Konvertierung nicht durchführen kann.

Weitere Informationen zu Fallbacks für Bindungen finden Sie unter [Fallbacks für Xamarin.Forms-Bindungen](binding-fallbacks.md).

## <a name="nest-multibinding-objects"></a>Schachteln von MultiBinding-Objekten

`MultiBinding`-Objekte können so geschachtelt werden, dass mehrere `MultiBinding`-Objekte ausgewertet werden, um einen Wert durch eine `IMultiValueConverter`-Instanz zurückzugeben:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.NestedMultiBindingPage"
             Title="Nested MultiBinding demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:AnyTrueMultiConverter x:Key="AnyTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AnyTrueConverter}">
                <MultiBinding Converter="{StaticResource AllTrueConverter}">
                    <Binding Path="Employee.IsOver16" />
                    <Binding Path="Employee.HasPassedTest" />
                    <Binding Path="Employee.IsSuspended" Converter="{StaticResource InverterConverter}" />                        
                </MultiBinding>
                <Binding Path="Employee.IsMonarch" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>
```

In diesem Beispiel nutzt das `MultiBinding`-Objekt seine `AnyTrueMultiConverter`-Instanz, um die [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked)-Eigenschaft auf `true` festzulegen. Voraussetzung dafür ist, dass alle [`Binding`](xref:Xamarin.Forms.Binding)-Objekte im inneren `MultiBinding`-Objekt mit `true` ausgewertet werden, oder dass das `Binding`-Objekt im äußeren `MultiBinding`-Objekt mit `true` ausgewertet wird. Andernfalls wird die `CheckBox.IsChecked`-Eigenschaft auf `false` festgelegt.

## <a name="use-a-relativesource-binding-in-a-multibinding"></a>Verwenden einer RelativeSource-Bindung in einer MultiBinding

`MultiBinding`-Objekte unterstützen relative Bindungen, die die Möglichkeit bieten, die Quellbindung relativ zur Position des Bindungsziels festzulegen:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:DataBindingDemos">
    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />

        <ControlTemplate x:Key="CardViewExpanderControlTemplate">
            <Expander BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                      IsExpanded="{Binding IsExpanded, Source={RelativeSource TemplatedParent}}"
                      BackgroundColor="{Binding CardColor}">
                <Expander.IsVisible>
                    <MultiBinding Converter="{StaticResource AllTrueConverter}">
                        <Binding Path="IsExpanded" />
                        <Binding Path="IsEnabled" />
                    </MultiBinding>
                </Expander.IsVisible>
                <Expander.Header>
                    <Grid>
                        <!-- XAML that defines Expander header goes here -->
                    </Grid>
                </Expander.Header>
                <Grid>
                    <!-- XAML that defines Expander content goes here -->
                </Grid>
            </Expander>
        </ControlTemplate>
    </ContentPage.Resources>

    <StackLayout>
        <controls:CardViewExpander BorderColor="DarkGray"
                                   CardTitle="John Doe"
                                   CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                                   IconBackgroundColor="SlateGray"
                                   IconImageSource="user.png"
                                   ControlTemplate="{StaticResource CardViewExpanderControlTemplate}"
                                   IsEnabled="True"
                                   IsExpanded="True" />
    </StackLayout>
</ContentPage>
```

Bei diesem Beispiel wird der Modus `TemplatedParent` für eine relative Bindung verwendet, um eine Steuerelementvorlage an die Instanz eines Runtimeobjekts zu binden, auf die die Vorlage angewendet wird. `Expander`, das Stammelement von [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate), hat sein `BindingContext` auf die Instanz des Runtimeobjekts festgelegt, auf die die Vorlage angewendet wird. Daher lösen `Expander` und seine untergeordneten Elemente ihre Bindungsausdrücke und [`Binding`](xref:Xamarin.Forms.Binding)-Objekte in Bezug auf die Eigenschaften des `CardViewExpander`-Objekts auf. `MultiBinding` nutzt die `AllTrueMultiConverter`-Instanz, um die `Expander.IsVisible`-Eigenschaft auf `true` festzulegen, vorausgesetzt, die beiden [`Binding`](xref:Xamarin.Forms.Binding)-Objekte werden mit `true` ausgewertet. Andernfalls wird die `Expander.IsVisible`-Eigenschaft auf `false` festgelegt.

Weitere Informationen zu relativen Bindungen finden Sie unter [Relative Bindungen in Xamarin.Forms](relative-bindings.md). Weitere Informationen zu Steuerelementvorlagen finden Sie unter [Xamarin.Forms-Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Zeichenfolgenformatierung in Xamarin.Forms](string-formatting.md)
- [Fallbacks für Xamarin.Forms-Bindungen](binding-fallbacks.md)
- [Relative Bindungen in Xamarin.Forms](relative-bindings.md)
- [Xamarin.Forms-Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md)
