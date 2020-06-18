---
title: Einfache Xamarin.Forms-Bindungen
description: In diesem Artikel wird beschrieben, wie Sie Xamarin.Forms-Datenbindungen verwenden können. Hierbei wird ein Eigenschaftenpaar zwischen zwei Objekten verknüpft, von denen mindestens eines ein Benutzeroberflächenobjekt ist. Diese beiden Objekte sind das Ziel und die Quelle.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: c0c6bc6e1005997548952aedc09cd83a451e7caa
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84133535"
---
# <a name="xamarinforms-basic-bindings"></a>Einfache Xamarin.Forms-Bindungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Bei einer Xamarin.Forms-Datenbindung wird ein Eigenschaftenpaar zwischen zwei Objekten verknüpft, von denen mindestens eines ein Benutzeroberflächenobjekt ist. Diese beiden Objekte sind das *Ziel* und die *Quelle*:

- Das *Ziel* ist das Objekt (und die Eigenschaft), auf dem die Datenbindung festgelegt wird.
- Die *Quelle* ist das Objekt (und die Eigenschaft), auf die von der Datenbindung verwiesen wird.

Dieser Unterschied kann mitunter verwirrend sein: Im einfachsten Fall fließen Daten von der Quelle zum Ziel. Das bedeutet, dass der Wert der Zieleigenschaft über den Wert der Quelleigenschaft festgelegt wird. In einigen Fällen können Daten jedoch auch vom Ziel zur Quelle fließen oder sogar in beide Richtungen. Merken Sie sich, dass das Ziel immer das Objekt ist, auf dem die Datenbindung festgelegt wird, auch wenn es der Ursprung der Daten ist und keine Daten empfängt.

## <a name="bindings-with-a-binding-context"></a>Bindungen mit Bindungskontext

Datenbindungen werden zwar normalerweise komplett in XAML angegeben, ist es dennoch empfehlenswert, sich Datenbindungen im Code anzusehen. Die Seite **Basic Code Binding** (Einfache Codebindung) enthält eine XAML-Datei mit einem `Label`- und einem `Slider`-Objekt:

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

Dieser `Slider` ist auf einen Bereich zwischen 0 und 360 festgelegt. Dieses Programm soll das `Label` drehen, indem es den `Slider` anpasst.

Ohne Datenbindungen würden Sie das `ValueChanged`-Ereignis des `Slider` auf einen Ereignishandler festlegen, der auf die `Value`-Eigenschaft des `Slider` zugreift und den Wert auf die `Rotation`-Eigenschaft des `Label` festlegt. Durch eine Datenbindung wird diese Aufgabe automatisiert. Der Ereignishandler und der darin enthaltene Code sind nicht mehr erforderlich.

Sie können eine Bindung auf einer Instanz einer beliebigen Klasse festlegen, die von [`BindableObject`](xref:Xamarin.Forms.BindableObject) abgeleitet wurde. Dazu zählen `Element`-, `VisualElement`-, `View`- und `View`-Derivate.  Die Bindung wird immer auf dem Zielobjekt festgelegt. Die Bindung verweist auf das Quellobjekt. Verwenden Sie die beiden folgenden Member der Zielklasse, um die Datenbindung herzustellen:

- Die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft gibt das Quellobjekt an.
- Mit der [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))-Methode wird die Ziel- und die Quelleigenschaft angegeben.

In diesem Beispiel ist das `Label` das Bindungsziel, und der `Slider` ist die Bindungsquelle. Änderungen der `Slider`-Quelle wirken sich auf die Drehung des `Label`-Ziels aus. Daten fließen von der Quelle zum Ziel.

Die von `BindableObject` definierte `SetBinding`-Methode weist ein Argument vom Typ [`BindingBase`](xref:Xamarin.Forms.BindingBase) auf, von dem die [`Binding`](xref:Xamarin.Forms.Binding)-Klasse abgeleitet wird. Es gibt aber auch andere `SetBinding`-Methoden, die von der [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions)-Klasse definiert werden. Die CodeBehind-Datei im Beispiel **Basic Code Binding** (Einfache Codebindung) verwendet eine einfachere [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*)-Erweiterungsmethode aus dieser Klasse.

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

Das `Label` ist das Bindungsziel. Deshalb wird diese Eigenschaft dort festgelegt, und dort wird die Methode aufgerufen. Die `BindingContext`-Eigenschaft gibt die Bindungsquelle an, der `Slider`.

Die `SetBinding`-Methode wird auf dem Bindungsziel aufgerufen, gibt aber sowohl die Ziel- als auch die Quelleigenschaft an. Die Zieleigenschaft wird als `BindableProperty`-Objekt angegeben: `Label.RotationProperty`. Die Quelleigenschaft wird als Zeichenfolge angegeben und gibt die `Value`-Eigenschaft des `Slider` an.

Die `SetBinding`-Methode macht eine der wichtigsten Regeln der Datenbindung deutlich:

*Die Zieleigenschaft muss von einer bindbaren Eigenschaft unterstützt werden.*

Diese Regel impliziert, dass das Zielobjekt eine Instanz einer Klasse sein muss, die von `BindableObject` abgeleitet wird. Im Artikel zu [**bindbaren Eigenschaften**](~/xamarin-forms/xaml/bindable-properties.md) finden Sie eine Übersicht über bindbare Objekte und Eigenschaften.

Es gibt keine entsprechende Regel für die Quelleigenschaft, die als Zeichenfolge angegeben wird. Intern wird die Reflektion verwendet, um auf die tatsächliche Eigenschaft zuzugreifen. In diesem Fall wird die `Value`-Eigenschaft jedoch auch von einer bindbaren Eigenschaft unterstützt.

Der Code kann bis zu einem gewissen Grad vereinfacht werden: Die bindbare Eigenschaft `RotationProperty` wird von `VisualElement` definiert und von `Label` und `ContentPage` geerbt, weshalb der Klassenname im `SetBinding`-Aufruf nicht erforderlich ist:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Das Einbeziehen des Klassennamens ist jedoch eine gute Möglichkeit, das Zielobjekt anzugeben.

Wenn Sie `Slider` anpassen, rotiert `Label` dementsprechend:

[![Einfache Codebindung](basic-bindings-images/basiccodebinding-small.png "Einfache Codebindung")](basic-bindings-images/basiccodebinding-large.png#lightbox "Einfache Codebindung")

Die Seite **Basic XAML Binding** (Einfache XAML-Bindung) stimmt fast vollständig mit der Seite **Basic Code Binding** (Einfache Codebindung) überein. Der einzige Unterschied besteht darin, dass die gesamte Codebindung in XAML definiert wird:

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

Die Datenbindung wird genau wie im Code auf dem Zielobjekt festgelegt, in diesem Fall `Label`. Es wurden zwei XAML-Markuperweiterungen verwendet. Diese können leicht durch die geschweiften Klammern erkannt werden:

- Die Markuperweiterung `x:Reference` ist erforderlich, um auf das Quellobjekt zu verweisen, auf den `Slider` mit dem Namen `slider`.
- Die Markuperweiterung `Binding` verknüpft die `Rotation`-Eigenschaft des `Label` mit der `Value`-Eigenschaft des `Slider`.

Weitere Informationen zu XAML-Markuperweiterungen finden Sie unter [XAML Markup Extensions (XAML-Markuperweiterungen)](~/xamarin-forms/xaml/markup-extensions/index.md). Die Markuperweiterung `x:Reference` wird von der [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)-Klasse unterstützt. `Binding` wird von der [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension)-Klasse unterstützt. Wie in den XML-Namespacepräfixen angegeben, ist `x:Reference` Teil der XAML 2009-Spezifikation, und `Binding` ist Teil von Xamarin.Forms. Beachten Sie, dass innerhalb der geschweiften Klammern keine Anführungszeichen verwendet wurden.

Beim Festlegen des `x:Reference`-Objekts vergisst man schnell die Markuperweiterung `BindingContext`. Die Eigenschaft wird häufig fälschlicherweise direkt wie folgt auf den Namen der Bindungsquelle festgelegt:

```xaml
BindingContext="slider"
```

Das ist jedoch nicht korrekt. Dieses Markup legt die Eigenschaft `BindingContext` auf ein `string`-Objekt fest, dessen Zeichen „Slider“ ausschreiben.

Beachten Sie, dass die Quelleigenschaft mit der [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path)-Eigenschaft des `BindingExtension`-Objekts angegeben wird, was der [`Path`](xref:Xamarin.Forms.Binding.Path)-Eigenschaft der [`Binding`](xref:Xamarin.Forms.Binding)-Klasse entspricht.

Das Markup auf der Seite **Basic XAML Binding** (Einfache XAML-Bindung) kann vereinfacht werden: Für XAML-Markuperweiterungen wie `x:Reference` und `Binding` können *Inhaltseigenschaftsattribute* definiert werden. Für XAML-Markuperweiterungen bedeutet das, dass der Eigenschaftenname nicht verwendet werden muss. Die Eigenschaft `Name` ist die Inhaltseigenschaft von `x:Reference`, und die `Path`-Eigenschaft ist die Inhaltseigenschaft von `Binding`. Das bedeutet, dass sie nicht im Ausdruck verwendet werden müssen:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Bindungen ohne Bindungskontext

Die Eigenschaft `BindingContext` ist ein wichtiger Bestandteil von Datenbindungen. Sie ist jedoch nicht immer notwendig. Stattdessen kann das Quellobjekt im `SetBinding`-Aufruf oder der `Binding`-Markuperweiterung angegeben werden.

Dies wird im Beispiel **Alternative Code Binding** (Alternative Codebindung) veranschaulicht. Die XAML-Datei ähnelt dem Beispiel **Basic Code Binding** (Einfache Codebindung). Der einzige Unterschied besteht darin, dass der `Slider` so definiert wird, dass er die `Scale`-Eigenschaft des `Label` steuert. Aus diesem Grund wird dieser `Slider` auf einen Bereich zwischen &ndash;2 und 2 festgelegt:

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

Die CodeBehind-Datei stellt die Bindung mit der [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))-Methode her, die von `BindableObject` definiert wird. Das Argument ist ein [Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) für die [`Binding`](xref:Xamarin.Forms.Binding)-Klasse:

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

Der Konstruktor `Binding` verfügt über sechs Parameter, weshalb der Parameter `source` mit einem benannten Argument angegeben wird. Das Argument ist das `slider`-Objekt.

Wenn Sie dieses Programm ausführen, passiert etwas Unerwartetes:

[![Alternative Codebindung](basic-bindings-images/alternativecodebinding-small.png "Alternative Codebindung")](basic-bindings-images/alternativecodebinding-large.png#lightbox "Alternative Codebindung")

Wenn die Seite angezeigt wird, sieht sie zunächst wie der iOS-Bildschirm links aus. Wo ist das `Label`?

Dieses Problem wird ausgelöst, weil der `Slider` einen Anfangswert von 0 (null) hat. Dadurch wird die `Scale`-Eigenschaft des `Label` auch auf 0 (null) festgelegt, also der Standardwert von 1 überschrieben. Deshalb ist das `Label` zunächst nicht sichtbar. Wie Sie auf dem Android-Screenshot sehen können, können Sie den `Slider` anpassen, damit das `Label` wieder angezeigt wird. Dass es aber zunächst nicht angezeigt wurde, ist verwirrend.

Im [nächsten Artikel](binding-mode.md) erfahren Sie, wie Sie dieses Problem umgehen, indem Sie den `Slider` über den Standardwert der `Scale`-Eigenschaft initialisieren.

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Klasse definiert auch die Eigenschaften [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) und [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY), die das `VisualElement` horizontal und vertikal skalieren können.

Die Seite **Alternative XAML Binding** (Alternative XAML-Bindung) zeigt die entsprechende Bindung in XAML:

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

Hier verfügt die Markuperweiterung `Binding` über zwei festgelegte Eigenschaften, `Source` und `Path`, die durch ein Komma getrennt sind. Sie können auch in eine Zeile geschrieben werden, wenn Sie dies bevorzugen:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

Die Eigenschaft `Source` wird auf eine eingebettete `x:Reference`-Markuperweiterung festgelegt. Dabei wird die gleiche Syntax wie beim Festlegen des `BindingContext` verwendet. Beachten Sie, dass innerhalb der geschweiften Klammern keine Anführungszeichen verwendet werden und dass die beiden Eigenschaften durch ein Komma getrennt werden müssen.

Die Inhaltseigenschaft der Markuperweiterung `Binding` ist `Path`, aber `Path=`-Teil der Markuperweiterung kann nur weg gelassen werden, wenn sie die erste Eigenschaft im Ausdruck ist. Tauschen Sie die beiden Eigenschaften, um den `Path=`-Teil weglassen zu können:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Obwohl XAML-Markuperweiterungen normalerweise durch geschweifte Klammern getrennt werden, können sie auch als Objektelemente ausgedrückt werden:

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

Jetzt sind die Eigenschaften `Source` und `Path` normale XAML-Attribute: Die Werte befinden sich in Anführungszeichen, und die Attribute sind nicht durch Kommas getrennt. Die Markuperweiterung `x:Reference` kann auch ein Objektelement werden:

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

Diese Syntax ist unüblich, aber manchmal nötig, wenn komplexe Objekte involviert sind.

In den besprochenen Beispielen wurde die `BindingContext`-Eigenschaft und die `Source`-Eigenschaft von `Binding` auf eine `x:Reference`-Markuperweiterung festgelegt, um auf eine andere Ansicht auf der Seite zu verweisen. Die beiden Eigenschaften weisen den Typ `Object` auf, und sie können auf ein beliebiges Objekt festgelegt werden, das Eigenschaften enthält, die für Bindungsquellen geeignet sind.

In den nächsten Artikeln lernen Sie, dass Sie die Eigenschaften `BindingContext` oder `Source` auf eine `x:Static`-Markuperweiterung festlegen können, um auf die Werte einer statischen Eigenschaft oder eines statischen Felds zu verweisen. Sie können die Eigenschaften auch auf die `StaticResource`-Markuperweiterung festlegen, um auf ein Objekt, das in einem Ressourcenverzeichnis gespeichert ist, oder direkt auf ein Objekt zu verweisen, das meist (aber nicht immer) eine ViewModel-Instanz ist.

Die `BindingContext`-Eigenschaft kann auch auf ein `Binding`-Objekt festgelegt werden, sodass die Eigenschaften `Source` und `Path` von `Binding` den Bindungskontext definieren.

## <a name="binding-context-inheritance"></a>Vererbung von Bindungskontexten

In diesem Artikel haben Sie gelernt, dass Sie das Quellobjekt mit der Eigenschaft `BindingContext` oder der `Source`-Eigenschaft des `Binding`-Objekts angeben können. Wenn beide Eigenschaften festgelegt sind, hat die `Source`-Eigenschaft des `Binding`-Objekts Vorrang vor der `BindingContext`-Eigenschaft.

Die `BindingContext`-Eigenschaft hat ein sehr wichtiges Merkmal:

*Das Festlegen der Eigenschaft `BindingContext` wird über die visuelle Struktur vererbt.*

Das kann für das Vereinfachen von Bindungsausdrücken sehr praktisch sein. In einigen Fällen &mdash; besonders in MVVM-Szenarios (Model View ViewModel) &mdash; ist dies wesentlich.

Das Beispiel **Binding Context Inheritance** (Bindungskontextvererbung) veranschaulicht die Vererbung des Bindungskontexts auf einfache Weise:

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

Die `BindingContext`-Eigenschaft des `StackLayout`-Objekts wird auf das Objekt `slider` festgelegt. Dieser Bindungskontext wird sowohl vom `Label` als auch von `BoxView` geerbt. Deren `Rotation`-Eigenschaften werden auf die `Value`-Eigenschaft des `Slider` festgelegt:

[![Vererbung von Bindungskontexten](basic-bindings-images/bindingcontextinheritance-small.png "Vererbung von Bindungskontexten")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "Vererbung von Bindungskontexten")

Im [nächsten Artikel](binding-mode.md) erfahren Sie, wie sich der *Bindungsmodus* auf den Datenfluss zwischen Ziel- und Quellobjekten auswirken kann.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Kapitel zu Datenbindung aus dem Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Data-Binding/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
