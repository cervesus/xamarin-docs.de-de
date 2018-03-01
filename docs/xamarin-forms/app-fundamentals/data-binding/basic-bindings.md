---
title: Grundlegende Bindungen
description: "Ziele für die Datenbindung, Datenquellen und Bindungskontext"
ms.topic: article
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 500ad02d79cea79f59b1aca91b0312c9a9d6bac3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="basic-bindings"></a>Grundlegende Bindungen

Eine Xamarin.Forms-Datenbindung verknüpft ein Paar von Eigenschaften zwischen zwei Objekten, die mindestens, die eine der in der Regel ein Benutzeroberflächen-Objekt ist. Diese zwei Objekte heißen die *Ziel* und die *Quelle*:

- Die *Ziel* wird dem Objekt (und der Eigenschaft) auf dem Datenbindung festgelegt wurde.
- Die *Quelle* ist das Objekt (und die Eigenschaft) verwiesen wird, von dem Datenbindung.

Dieser Unterschied kann manchmal etwas verwirrend sein: im einfachsten Fall Daten fließen aus der Quelle zum Ziel, was bedeutet, dass der Wert der Zieleigenschaft aus dem Wert der Source-Eigenschaft festgelegt ist. Allerdings können in einigen Fällen auch vom Ziel zur Quelle oder in beide Richtungen Datenfluss. Um Verwirrung zu vermeiden, Bedenken Sie, das Ziel immer das Objekt auf dem Datenbindung, die festgelegt wird ist, auch wenn es Daten bereitstellt und nicht als empfangen von Daten.

## <a name="bindings-with-a-binding-context"></a>Bindungen mit einem Bindungskontext.

Obwohl datenbindungen vollständig in XAML in der Regel angegeben sind, ist es hilfreich, die datenbindungen im Code finden Sie unter. Die **grundlegenden Code binden** Seite enthält eine XAML-Datei mit einem `Label` und ein `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die `Slider` für einen Bereich von 0 bis 360 festgelegt ist. Der Zweck dieses Programm ist zum Drehen der `Label` durch Bearbeiten der `Slider`.

Ohne datenbindungen, legen Sie die `ValueChanged` -Ereignis der `Slider` mit einem Ereignishandler, die greift auf die `Value` Eigenschaft der `Slider` und legt diesen Wert auf die `Rotation` Eigenschaft der `Label`. Die Datenbindung automatisiert, das dem Auftrag; der Ereignishandler und den Code in ihr sind nicht mehr erforderlich. 

Sie können eine Bindung für eine Instanz einer Klasse, die abgeleitet festlegen [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), darunter `Element`, `VisualElement`, `View`, und `View` ableitungen.  Die Bindung ist für das Zielobjekt immer festgelegt werden. Die Bindung verweist auf das Quellobjekt. Um die Datenbindung festzulegen, verwenden Sie die folgenden beiden Elemente der Zielklasse:

- Die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Eigenschaft gibt an, das Quellobjekt.
- Die [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) -Methode gibt die Eigenschaft "Ziel" und die Source-Eigenschaft.

In diesem Beispiel wird die `Label` ist das Bindungsziel, und die `Slider` wird von die Bindungsquelle. Änderungen in der `Slider` Quelle Auswirkungen auf die Drehung der `Label` Ziel. Die Daten fließen aus der Quelle zum Ziel.

Die `SetBinding` definierte Methode `BindableObject` hat ein Argument des Typs [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) aus dem die [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) -Klasse abgeleitet ist, aber es gibt noch weitere `SetBinding` Methoden definiert durch die [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) Klasse. Der Code-Behind-Datei in die **grundlegenden Code binden** -Beispiel verwendet eine einfachere [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) Erweiterungsmethode, die von dieser Klasse.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

Die `Label` Objekt ist das Bindungsziel, damit ist das Objekt, auf die diese Eigenschaft festgelegt ist und auf dem die Methode aufgerufen wird. Die `BindingContext` Eigenschaft gibt an, die Bindungsquelle, also die `Slider`.

Die `SetBinding` Methode für das Bindungsziel aufgerufen wird, sondern gibt an, die Zieleigenschaft und die Source-Eigenschaft. Die Eigenschaft wird angegeben, als ein `BindableProperty` Objekt: `Label.RotationProperty`. Die Source-Eigenschaft als Zeichenfolge angegeben wird, und gibt an, die `Value` Eigenschaft `Slider`. 

Die `SetBinding` Methode stellt eine der wichtigsten Regeln von datenbindungen:

*Die Zieleigenschaft muss gesichert werden, indem eine bindbare Eigenschaft.*

Diese Regel bedingt, dass das Zielobjekt eine Instanz einer Klasse sein muss, die abgeleitet `BindableObject`. Finden Sie unter der [ **bindbare Eigenschaften** ](~/xamarin-forms/xaml/bindable-properties.md) Artikel für eine Übersicht über bindbare Objekte und bindbare Eigenschaften.

Es gibt keine solche Regel für die Source-Eigenschaft, die als Zeichenfolge angegeben wird. Intern wird Reflektion zum Zugriff auf die tatsächliche Eigenschaft verwendet. In diesem speziellen Fall die `Value` Eigenschaft auch durch eine bindbare Eigenschaft unterstützt wird.

Der Code in einem gewissen vereinfacht: die `RotationProperty` bindbare Eigenschaft wird definiert, indem `VisualElement`, und von geerbt `Label` und `ContentPage` auch der Name der Klasse ist nicht erforderlich der `SetBinding` aufrufen:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Der Name der Klasse einschließlich ist jedoch eine gute Erinnerung des Zielobjekts.

Wie Sie bearbeiten die `Slider`die `Label` dreht entsprechend:

[![Basice-Code binden](basic-bindings-images/basiccodebinding-small.png "grundlegenden Code Bindung")](basic-bindings-images/basiccodebinding-large.png "Basic-Code-Bindung")

Die **grundlegenden XAML-Bindung** Seite ist identisch mit **Standardbindung Code** mit dem Unterschied, dass es die gesamte Datenbindung in XAML definiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Wie Code, in die Datenbindung festgelegt ist, auf das Zielobjekt, also die `Label`. Es sind zwei XAML-Markuperweiterungen beteiligt. Dies sind sofort erkennbar, die durch die geschweifte Klammertrennzeichen:

- Die `x:Reference` Markuperweiterung ist erforderlich, um das Quellobjekt, verweisen die `Slider` mit dem Namen `slider`.
- Die `Binding` Markup Extension Links die `Rotation` Eigenschaft der `Label` auf die `Value` Eigenschaft der `Slider`. 

Finden Sie im Artikel [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md) für Weitere Informationen über Markuperweiterungen für XAML. Die `x:Reference` Markuperweiterung wird unterstützt, indem Sie die [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) Klasse. `Binding` wird unterstützt, indem Sie die [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) Klasse. Wie der XML-Namespacepräfixe anzugeben, `x:Reference` ist Teil der Spezifikation XAML 2009 während `Binding` ist Bestandteil des Xamarin.Forms. Beachten Sie, dass keine Anführungszeichen in der geschweiften Klammern angezeigt werden.

Es ist leicht vergessen der `x:Reference` Markuperweiterung, die beim Festlegen der `BindingContext`. Es ist üblich, versehentlich die Eigenschaft direkt auf den Namen der Bindungsquelle wie folgt festgelegt:

```xaml
BindingContext="slider"
```

Aber nicht geeignet ist. Legt das Markup der `BindingContext` Eigenschaft, um eine `string` Objekt, dessen Zeichen Rechtschreibprüfungs "Slider"!

Beachten Sie, die mit die Source-Eigenschaft angegeben ist die [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) Eigenschaft `BindingExtension`, entspricht dem der [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Eigenschaft von der [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Klasse.

Das Markup angezeigt, auf die **grundlegenden XAML-Bindung** Seite vereinfacht werden: XAML-Markuperweiterungen wie z. B. `x:Reference` und `Binding` können *Inhaltseigenschaft* Attribute definiert, die für Markuperweiterungen für XAML Markuperweiterungen bedeutet, dass es sich bei den Namen der Eigenschaft nicht angezeigt werden muss. Die `Name` Eigenschaft ist für die Inhaltseigenschaft des `x:Reference`, und die `Path` Eigenschaft ist für die Inhaltseigenschaft des `Binding`, was bedeutet, dass sie der Ausdrücke gelöscht werden können:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Bindungen, ohne einen Bindungskontext

Die `BindingContext` Eigenschaft ist eine wichtige Komponente von datenbindungen, aber es ist nicht immer erforderlich. Das Quellobjekt kann stattdessen angegeben werden, der `SetBinding` aufrufen oder die `Binding` Markuperweiterung.

Dies wird dargestellt, der **Alternative Code binden** Beispiel. Die XAML-Datei ähnelt der **grundlegenden Code binden** Beispiel, außer dass die `Slider` wird definiert, um zu steuern die `Scale` Eigenschaft der `Label`. Aus diesem Grund die `Slider` festgelegt ist, für einen Bereich von &ndash;2 bis 2:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei legt fest, die Bindung mit der [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) definierte Methode `BindableObject`. Das Argument ist ein [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) für die [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Klasse:

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

Die `Binding` Konstruktor verfügt über 6 Parameter, sodass der `source` Parameter ein benanntes Argument angegeben wird. Das Argument ist der `slider` Objekt. 

Dieses Programm ausführen, kann ein wenig überraschend sein:

[![Alternativer Code Bindung](basic-bindings-images/alternativecodebinding-small.png "alternativer Code Bindung")](basic-bindings-images/alternativecodebinding-large.png "alternativer Code Bindung")

Die iOS-Bildschirm auf der linken Seite zeigt wie sieht das beim ersten der Seite öffnen. Wobei ist die `Label`? 

Das Problem besteht darin, die die `Slider` hat einen Anfangswert von 0. Dies bewirkt, dass die `Scale` Eigenschaft von der `Label` auf 0 (null) und überschreiben den Standardwert 1 ebenfalls festgelegt werden. Dadurch wird die `Label` wird anfänglich nicht sichtbar. Wie zeigen die Screenshots Android und universelle Windows-Plattform (UWP), können Sie ändern die `Slider` vornehmen der `Label` erneut angezeigt, aber seine anfängliche verschwinden irritierend ist.

Erfahren Sie der [weiter Artikel](binding-mode.md) wie dieses Problem zu vermeiden, indem Sie initialisieren die `Slider` vom Standardwert von der `Scale` Eigenschaft.

Die **Alternative Verwendung von XAML-Bindung** Seite zeigt die entsprechende Bindung vollständig in XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Jetzt die `Binding` Markuperweiterung verfügt über zwei Eigenschaften festgelegt, `Source` und `Path`, getrennt durch ein Komma. Sie können in derselben Zeile angezeigt werden, wenn Sie es vorziehen:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

Die `Source` Eigenschaftensatz wird auf ein eingebettetes `x:Reference` Markuperweiterung, die die gleiche Syntax wie Einstellung andernfalls hat die `BindingContext`. Beachten Sie, dass keine Anführungszeichen in der geschweiften Klammern angezeigt werden, und die beiden Eigenschaften durch ein Komma getrennt werden müssen.

Die Content-Eigenschaft von der `Binding` Markuperweiterung `Path`, aber die `Path=` Teil der Markuperweiterung kann nur gelöscht werden, wenn es sich um die erste Eigenschaft im Ausdruck ist. Entfernen der `Path=` Teil, müssen Sie die beiden Eigenschaften austauschen:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Obwohl XAML-Markuperweiterungen in der Regel durch geschweifte Klammern getrennt sind, können sie auch als Objektelemente formuliert werden:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
``` 

Jetzt die `Source` und `Path` Eigenschaften sind reguläre Verwendung von XAML-Attributen: die Werte in Anführungszeichen angezeigt werden und die Attribute werden nicht durch ein Komma getrennt. Die `x:Reference` Markuperweiterung kann auch einem Objektelement werden:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Diese Syntax ist häufig nicht, aber manchmal ist es erforderlich, wenn komplexe Objekte daran beteiligt sind.

Legen Sie die bisher gezeigten Beispiele der `BindingContext` Eigenschaft und die `Source` Eigenschaft `Binding` auf eine `x:Reference` Markuperweiterung auf einer anderen Ansicht auf der Seite verweisen. Diese beiden Eigenschaften sind vom Typ `Object`, und sie können auf jedes Objekt, das Eigenschaften enthält, die zum Binden von Datenquellen geeignet sind festgelegt werden. 

In den Artikeln im voraus, werden Sie feststellen, dass Sie festlegen können die `BindingContext` oder `Source` Eigenschaft, um eine `x:Static` Markuperweiterung auf den Wert des eine statische Eigenschaft oder ein Feld verweisen oder eine `StaticResource` Markuperweiterung auf ein Objekt in verweisen eine Ressourcenverzeichnis oder direkt auf ein Objekt, das in der Regel (aber nicht immer) einer Instanz von einem ViewModel. 

Die `BindingContext` Eigenschaft kann auch festgelegt werden, um eine `Binding` Objekt, damit die `Source` und `Path` Eigenschaften des `Binding` definieren, den Bindungskontext.

## <a name="binding-context-inheritance"></a>Binden von Kontext Vererbung

In diesem Artikel haben Sie gesehen, dass Sie mit der Quelle angeben, können die `BindingContext` Eigenschaft oder die `Source` Eigenschaft von der `Binding` Objekt. Wenn beide festgelegt sind, die `Source` Eigenschaft von der `Binding` hat Vorrang vor den `BindingContext`.

Die `BindingContext` Eigenschaft verfügt über ein besonders wichtiges Merkmal:

*Die Einstellung von der `BindingContext` Eigenschaft über die visuelle Struktur geerbt wird.*

Wie Sie sehen werden, kann dies sehr nützlich für die Vereinfachung Bindungsausdrücke und in einigen Fällen werden &mdash; besonders in Szenarien mit Model-View-ViewModel (MVVM) &mdash; ist es sehr wichtig.

Die **Bindung Kontext Vererbung** Beispiel ist eine einfache Veranschaulichung der Vererbung von den Bindungskontext:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">
            
            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider" 
                Maximum="360" />
        
    </StackLayout>
</ContentPage>
```

Die `BindingContext` Eigenschaft der `StackLayout` festgelegt ist, um die `slider` Objekt. Diesem Bindungskontext von sowohl geerbt wird die `Label` und die `BoxView`, die beide von wofür ihre `Rotation` Eigenschaften festlegen, um die `Value` Eigenschaft der `Slider`: 

[![Binden von Kontext Vererbung](basic-bindings-images/bindingcontextinheritance-small.png "binden Kontext Vererbung")](basic-bindings-images/bindingcontextinheritance-large.png "binden Kontext Vererbung")

In der [weiter Artikel](binding-mode.md), sehen Sie wie die *Bindungsmodus* können den Fluss der Daten zwischen Quell-und ändern.


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
