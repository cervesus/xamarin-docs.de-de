---
title: Xamarin.Forms-Basic-Bindungen
description: In diesem Artikel wird erläutert, wie Sie mit Xamarin.Forms Datenbindung, die einem Eigenschaftenpaar zwischen zwei Objekten mindestens verknüpft, von denen in der Regel ein Benutzeroberflächen-Objekt ist. Diese beiden Objekte heißen, dass das Ziel und Quelle.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b52f249b184d49731fd5decdb5877c70e29a3b84
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "38998071"
---
# <a name="xamarinforms-basic-bindings"></a>Xamarin.Forms-Basic-Bindungen

Eine Xamarin.Forms-Datenbindung verknüpft einem Eigenschaftenpaar zwischen zwei Objekten, die mindestens, die eine der in der Regel ein Benutzeroberflächen-Objekt ist. Diese beiden Objekte heißen die *Ziel* und *Quelle*:

- Die *Ziel* ist das Objekt (und Eigenschaft) auf der die Datenbindung festgelegt wurde.
- Die *Quelle* ist das Objekt (und Eigenschaft) verwiesen wird, durch die Datenbindung.

Diese Unterscheidung kann manchmal etwas verwirrend sein: im einfachsten Fall Datenflüsse aus der Quelle zum Ziel, was bedeutet, dass der Wert der Zieleigenschaft aus dem Wert der Source-Eigenschaft festgelegt ist. Jedoch können in einigen Fällen Daten auch vom Ziel zur Quelle oder in beide Richtungen fließen. Um Verwirrung zu vermeiden, Bedenken Sie, die das Ziel immer das Objekt für das die Datenbindung festgelegt ist ist, auch wenn es Daten bereitstellt anstatt empfangen von Daten.

## <a name="bindings-with-a-binding-context"></a>Bindungen, bei denen ein Bindungskontext.

Obwohl datenbindungen, die vollständig in XAML in der Regel angegeben sind, ist es hilfreich, die datenbindungen im Code finden Sie unter. Die **Basic-Code binden** Seite enthält eine XAML-Datei mit einem `Label` und `Slider`:

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

Ohne datenbindungen, legen Sie die `ValueChanged` Ereignis die `Slider` an einen Ereignishandler, der greift auf die `Value` Eigenschaft der `Slider` und legt diesen Wert auf die `Rotation` Eigenschaft der `Label`. Die Datenbindung automatisiert diesen Auftrag; der Ereignishandler und den Code in diesem sind nicht mehr erforderlich.

Sie können eine Bindung für eine Instanz einer Klasse, die von abgeleitet festlegen [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), wozu `Element`, `VisualElement`, `View`, und `View` ableitungen.  Die Bindung ist immer auf dem Zielobjekt festgelegt. Die Bindung verweist das Quellobjekt auf. Um die Datenbindung zu festzulegen, verwenden Sie die folgenden zwei Elemente der Zielklasse:

- Die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft gibt an, das Quellobjekt.
- Die [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) -Methode gibt die Zieleigenschaft und die Eigenschaft "Source".

In diesem Beispiel die `Label` ist das Bindungsziel, und die `Slider` wird von die Bindungsquelle. Änderungen in der `Slider` Quelle Auswirkungen auf die Rotation der `Label` Ziel. Daten werden aus der Quelle zum Ziel fließen.

Die `SetBinding` definierte Methode `BindableObject` hat ein Argument des Typs [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) aus dem die [ `Binding` ](xref:Xamarin.Forms.Binding) Klasse abgeleitet wird, aber es gibt andere `SetBinding` Methoden definiert durch die [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) Klasse. Der Code-Behind-Datei in die **Basic-Code binden** -Beispiel verwendet eine einfachere [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) Erweiterungsmethode, die von dieser Klasse.

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

Die `Label` Objekt ist das Bindungsziel, ist das Objekt auf der diese Eigenschaft festgelegt wurde und auf dem die Methode aufgerufen wird. Die `BindingContext` Eigenschaft gibt an, die Bindungsquelle, d.h. die `Slider`.

Die `SetBinding` Methode für das Bindungsziel aufgerufen wird, aber gibt sowohl die Eigenschaft als auch die Source-Eigenschaft. Die Zieleigenschaft angegeben ist, als eine `BindableProperty` Objekt: `Label.RotationProperty`. Die Source-Eigenschaft, die als Zeichenfolge angegeben ist, und gibt an die `Value` Eigenschaft `Slider`.

Die `SetBinding` Methode stellt eine der wichtigsten Regeln datenbindungen:

*Die Zieleigenschaft muss durch eine bindbare Eigenschaft unterstützt werden.*

Diese Regel bedingt, dass das Zielobjekt eine Instanz einer Klasse sein muss, die von abgeleitet `BindableObject`. Finden Sie unter den [ **bindbare Eigenschaften** ](~/xamarin-forms/xaml/bindable-properties.md) Artikel, um eine Übersicht über die bindbare Objekte und bindbare Eigenschaften.

Es gibt keine solche Regel für die Source-Eigenschaft, die als Zeichenfolge angegeben wird. Intern wird Reflektion verwendet, auf die tatsächliche Eigenschaft zugreifen. In diesem speziellen Fall jedoch die `Value` Eigenschaft auch durch eine bindbare Eigenschaft unterstützt wird.

Der Code kann sein etwas vereinfacht: die `RotationProperty` bindbare Eigenschaft wird definiert, indem `VisualElement`, und von geerbt `Label` und `ContentPage` , also den Namen der Klasse ist nicht erforderlich, der `SetBinding` aufrufen:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Einschließlich der Klassenname ist jedoch ein guter Erinnerung des Zielobjekts.

Wie Sie bearbeiten die `Slider`, `Label` dreht entsprechend:

[![Basice-Code binden](basic-bindings-images/basiccodebinding-small.png "grundlegenden Code Bindung")](basic-bindings-images/basiccodebinding-large.png#lightbox "Basic-Code-Bindung")

Die **grundlegende Xaml-Datenbindung** Seite ist identisch mit **Standardbindung Code** mit dem Unterschied, dass sie die gesamte Datenbindung in XAML definiert:

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

Ebenso wie Code, in die Datenbindung festgelegt ist, auf der Zielobjekt, das ist die `Label`. Es sind zwei XAML-Markuperweiterungen beteiligt. Dies sind sofort erkennbar, die die geschweifte Klammern eingeschlossen:

- Die `x:Reference` Markuperweiterung ist erforderlich, um das Quellobjekt, verweisen die `Slider` mit dem Namen `slider`.
- Die `Binding` Markup Extension Links die `Rotation` Eigenschaft der `Label` auf die `Value` Eigenschaft der `Slider`.

Finden Sie im Artikel [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md) für Weitere Informationen über Markuperweiterungen für XAML. Die `x:Reference` Markuperweiterung wird unterstützt, indem die [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) Klasse. `Binding` wird von unterstützt die [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) Klasse. Da das XML-Namespacepräfixe anzugeben, `x:Reference` ist Teil der Spezifikation XAML 2009, während `Binding` ist Teil von Xamarin.Forms. Beachten Sie, dass keine Anführungszeichen innerhalb der geschweiften Klammern angezeigt werden.

Es wird leicht vergessen der `x:Reference` Markuperweiterung, die beim Festlegen der `BindingContext`. Es ist üblich, versehentlich die Eigenschaft direkt auf den Namen der Bindungsquelle wie folgt festgelegt:

```xaml
BindingContext="slider"
```

Aber das ist nicht richtig. Dieses Markup wird der `BindingContext` Eigenschaft, um eine `string` Objekt, dessen Zeichen Rechtschreibprüfung "Slider"!

Beachten Sie, die die Source-Eigenschaft angegeben wird, mit der [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) Eigenschaft `BindingExtension`, entspricht dem der [ `Path` ](xref:Xamarin.Forms.Binding.Path) Eigenschaft der [ `Binding` ](xref:Xamarin.Forms.Binding) Klasse.

Das Markup angezeigt, auf die **grundlegende XAML-Bindung** Seite vereinfacht werden: XAML-Markuperweiterungen wie z. B. `x:Reference` und `Binding` können *Inhaltseigenschaft* Attribute definiert, die für XAML Markuperweiterungen bedeutet, dass der Eigenschaftenname nicht angezeigt werden. Die `Name` -Eigenschaft ist die Content-Eigenschaft des `x:Reference`, und die `Path` -Eigenschaft ist die Content-Eigenschaft des `Binding`, was bedeutet, dass sie aus den Ausdrücken lässt:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Bindungen, ohne einen Bindungskontext

Die `BindingContext` -Eigenschaft ist eine wichtige Komponente der datenbindungen, aber es ist nicht immer erforderlich. Das Quellobjekt kann stattdessen angegeben werden, der `SetBinding` aufrufen oder die `Binding` Markuperweiterung.

Dies wird veranschaulicht, der **Alternative Code binden** Beispiel. Die XAML-Datei ähnelt der **Basic-Code binden** Beispiel, außer dass die `Slider` definiert ist, um zu steuern die `Scale` Eigenschaft der `Label`. Aus diesem Grund die `Slider` festgelegt ist, für einen Bereich von &ndash;2 bis 2:

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

Die Code-Behind-Datei wird die Bindung mit der [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) definierte Methode `BindableObject`. Das Argument ist ein [Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) für die [ `Binding` ](xref:Xamarin.Forms.Binding) Klasse:

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

Die `Binding` Konstruktor verfügt über 6-Parameter, sodass die `source` Parameter mit der ein benanntes Argument angegeben ist. Das Argument ist der `slider` Objekt.

Dieses Programm kann ein wenig überraschend sein:

[![Alternativer Code Bindung](basic-bindings-images/alternativecodebinding-small.png "alternativen Code Bindung")](basic-bindings-images/alternativecodebinding-large.png#lightbox "alternativen Code-Bindung")

IOS-Bildschirms auf der linken Seite zeigt wie der Bildschirm beim ersten der Seite öffnen. Wo ist die `Label`?

Das Problem besteht darin, die die `Slider` hat einen Anfangswert von 0. Dies bewirkt, dass die `Scale` Eigenschaft der `Label` auch auf 0 (null) und überschreiben den Standardwert von 1 festgelegt werden. Dadurch wird die `Label` anfänglich unsichtbar wird. Zeigen Sie die Screenshots für Android- und universelle Windows-Plattform (UWP), können Sie ändern die `Slider` vornehmen der `Label` erneut angezeigt, aber die erste verschwinden ist verwirrend.

Erfahren Sie der [nächsten Artikel](binding-mode.md) wie dieses Problem zu vermeiden, indem Sie initialisieren den `Slider` vom Standardwert von der `Scale` Eigenschaft.

> [!NOTE]
> Die [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) -Klasse definiert außerdem [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) und [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) Eigenschaften, die skaliert werden können, die `VisualElement` anders als in der horizontaler bzw. vertikaler Richtung.

Die **Alternative XAML-Bindung** Seite zeigt die entsprechende Bindung vollständig in XAML:

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

Jetzt die `Binding` Markuperweiterung verfügt über zwei Eigenschaften festgelegt, `Source` und `Path`und durch ein Komma getrennt wird. Sie können in der gleichen Zeile angezeigt werden, wenn Sie möchten:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

Die `Source` -Eigenschaftensatz auf ein eingebettetes `x:Reference` Markuperweiterung, die andernfalls die gleiche Syntax wie das Festlegen der `BindingContext`. Beachten Sie, dass keine Anführungszeichen innerhalb der geschweiften Klammern angezeigt werden und dass die beiden Eigenschaften, die durch ein Komma getrennt werden müssen.

Der Content-Eigenschaft der `Binding` Markuperweiterung ist `Path`, aber die `Path=` Teil der Markuperweiterung kann nur behoben werden, wenn es sich um die erste Eigenschaft im Ausdruck ist. Zum Entfernen der `Path=` Teil, müssen Sie die beiden Eigenschaften austauschen:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML-Markuperweiterungen in der Regel durch geschweifte Klammern getrennt sind, können sie auch als Object-Elemente ausgedrückt werden:

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

Jetzt die `Source` und `Path` Eigenschaften sind reguläre XAML-Attribute: die Werte in Anführungszeichen angezeigt werden und die Attribute werden nicht durch ein Komma getrennt. Die `x:Reference` Markuperweiterung kann auch ein Objektelement werden:

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

Legen Sie die Beispiele, die bisher gezeigten der `BindingContext` Eigenschaft und die `Source` Eigenschaft `Binding` auf eine `x:Reference` Markuperweiterung zur Verwendung einer anderen Ansicht auf der Seite verweisen. Diese beiden Eigenschaften sind vom Typ `Object`, und sie können festgelegt werden, um jedes Objekt, das Eigenschaften enthält, die zum Binden von Datenquellen geeignet sind.

In den Artikeln fort, erfahren Sie, dass Sie festlegen können, die `BindingContext` oder `Source` Eigenschaft, um ein `x:Static` Markup Extension, um den Wert von einer statischen Eigenschaft oder ein Feld verweisen oder eine `StaticResource` Markuperweiterung verweisen auf ein Objekt, das in gespeicherten ein Ressourcenverzeichnis oder direkt auf ein Objekt, das sich in der Regel (aber nicht immer) eine Instanz eines "ViewModels".

Die `BindingContext` Eigenschaft kann auch festgelegt werden, um eine `Binding` Objekt, damit die `Source` und `Path` Eigenschaften `Binding` definieren, den Bindungskontext.

## <a name="binding-context-inheritance"></a>Binden die Kontext-Vererbung

In diesem Artikel haben Sie erfahren, dass Sie das Datenquellenobjekt mithilfe angeben, können die `BindingContext` Eigenschaft oder das `Source` Eigenschaft der `Binding` Objekt. Wenn beide festgelegt sind, die `Source` Eigenschaft der `Binding` hat Vorrang vor den `BindingContext`.

Die `BindingContext` Eigenschaft verfügt über ein sehr wichtiges Merkmal:

*Die Einstellung der `BindingContext` Eigenschaft wird von der visuellen Struktur geerbt.*

Wie Sie sehen werden, dies kann sehr nützlich sein für die Vereinfachung Bindungsausdrücke und in einigen Fällen &mdash; insbesondere in Szenarien mit Model-View-ViewModel (MVVM) &mdash; , ist es wichtig.

Die **Bindung Kontext Vererbung** Beispiel ist eine einfache Veranschaulichung der Vererbung von der Bindungskontext:

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

Die `BindingContext` Eigenschaft der `StackLayout` nastaven NA hodnotu der `slider` Objekt. Dieser Bindungskontext geerbt wird, sowohl die `Label` und die `BoxView`, beide von denen ihre `Rotation` Eigenschaften festgelegt, um die `Value` Eigenschaft der `Slider`:

[![Binden von Kontext Vererbung](basic-bindings-images/bindingcontextinheritance-small.png "Bindung Kontext Vererbung")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "Bindung Kontext Vererbung")

In der [nächsten Artikel](binding-mode.md), sehen Sie die *Bindungsmodus* können ändern, den Datenfluss zwischen Quelle und Ziel-Objekten.


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
