---
title: "Angefügte Eigenschaften"
description: "Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in einer Klasse definiert, jedoch auf andere Objekte angefügt und erkennbaren in XAML als ein Attribut enthält, die eine Klasse, und ein Eigenschaftsnamen, die durch einen Punkt getrennt. Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und veranschaulicht das Erstellen und nutzen müssen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7112812c843ccbcd6c24ea028deae3c09851b03d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="attached-properties"></a>Angefügte Eigenschaften

_Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in einer Klasse definiert, jedoch auf andere Objekte angefügt und erkennbaren in XAML als ein Attribut enthält, die eine Klasse, und ein Eigenschaftsnamen, die durch einen Punkt getrennt. Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und veranschaulicht das Erstellen und nutzen müssen._

## <a name="overview"></a>Übersicht

Angefügte Eigenschaften aktiviert ein Objekt, das einen Wert für eine Eigenschaft zuzuweisen, die eine eigene Klasse definieren, nicht an. Beispielsweise angefügte untergeordnete Elemente verwenden, können Eigenschaften, informiert werden jeweils übergeordneten Elements dafür, wie sie sind in der Benutzeroberfläche dargestellt werden soll. Die [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Steuerelement ermöglicht es der Zeile und Spalte eines untergeordneten Elements durch Festlegen von angegeben werden die `Grid.Row` und `Grid.Column` angefügte Eigenschaften. `Grid.Row` und `Grid.Column` angefügte Eigenschaften sind, da für Elemente festgelegt werden, die untergeordnete Elemente von einem `Grid`, statt auf die `Grid` selbst.

Als angefügte Eigenschaften in den folgenden Szenarien sollte bindbare Eigenschaften implementiert werden:

- Wenn einer benötigt wird, einen Eigenschaft Einstellung Mechanismus verfügbar für Klassen außer ist das Definieren der Klasse.
- Wenn die Klasse einen Dienst darstellt, der einfach mit anderen Klassen integriert werden muss.

Weitere Informationen zu bindbare Eigenschaften finden Sie unter [bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Erstellen und nutzen eine angefügte Eigenschaft

Der Prozess zum Erstellen einer angefügten Eigenschaft lautet wie folgt:

1. Erstellen einer [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanz mit einem der [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) methodenüberladungen.
1. Geben Sie `static` `Get` *PropertyName* und `Set` *PropertyName* Methoden als Accessoren für die angefügte Eigenschaft.

### <a name="creating-a-property"></a>Erstellen einer Eigenschaft

Beim Erstellen einer angefügten Eigenschaft für die Verwendung auf anderen Typen der Klasse, in dem die Eigenschaft erstellt wird, keine Ableitung [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/). Allerdings die *Ziel* -Eigenschaft für Accessoren sein sollte, oder eine Ableitung von, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Eine angefügte Eigenschaft erstellt werden, indem Sie deklarieren eine `public static readonly` Eigenschaft vom Typ [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Die bindbare Eigenschaft sollte festgelegt werden, auf den zurückgegebenen Wert eines der [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) methodenüberladungen. Die Deklaration sollte innerhalb des Texts, der die besitzende Klasse, aber außerhalb von Memberdefinitionen sein.

Der folgende Code zeigt ein Beispiel für eine angefügte Eigenschaft:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Dies erstellt eine angefügte Eigenschaft mit dem Namen `HasShadow`, vom Typ `bool`. Die Eigenschaft ist im Besitz der `ShadowEffect` Klasse, und hat den Standardwert `false`. Die Namenskonvention für angefügte Eigenschaften ist, dass der Bezeichner der angefügten Eigenschaft im angegebene Eigenschaftsname der entsprechen muss die `CreateAttached` Methode mit "Property" angefügt ist. Im obigen Beispiel ist der Bezeichner der angefügten Eigenschaft daher `HasShadowProperty`.

Weitere Informationen zum Erstellen von bindbare Eigenschaften, einschließlich der Parameter, die während der Erstellung angegeben werden können finden Sie unter [erstellen und nutzen eine bindbare Eigenschaft](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Erstellen von Accessoren

Statische `Get` *PropertyName* und `Set` *PropertyName* Methoden als Accessoren für die angefügte Eigenschaft erforderlich sind, andernfalls im Eigenschaftensystem können nicht die angefügte Eigenschaft. Die `Get` *PropertyName* Accessor sollte die folgende Signatur entsprechen:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

Die `Get` *PropertyName* Accessor sollte der enthaltene Wert zurückgeben, in der entsprechenden `BindableProperty` Feld für die angefügte Eigenschaft. Dies kann durch Aufrufen der [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) Methode, bindbare Eigenschaft im Bezeichner für das Abrufen des Werts übergeben, und klicken Sie dann den resultierenden Wert an den erforderlichen Typ umwandeln.

Die `Set` *PropertyName* Accessor sollte die folgende Signatur entsprechen:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

Die `Set` *PropertyName* Accessor sollte legen Sie den Wert des entsprechenden `BindableProperty` Feld für die angefügte Eigenschaft. Dies kann durch Aufrufen der [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) -Methode auf und übergibt die bindbare Eigenschaftenbezeichner für das der Wert und den festzulegenden Wert festgelegt.

Für beide Accessoren der *Ziel* Objekt sein sollte, oder eine Ableitung von, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Das folgende Codebeispiel zeigt die Accessoren für die `HasShadow` -Eigenschaft:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>Nutzen eine angefügte Eigenschaft

Sobald eine angefügte Eigenschaft erstellt wurde, können sie aus XAML oder Code genutzt werden. In XAML erfolgt dies durch einen Namespace mit einem Präfix, mit der Namespacedeklaration, der angibt, die Common Language Runtime (CLR) Namespacenamen und optional ein Assemblyname deklarieren. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Im folgenden Codebeispiel wird veranschaulicht, einen XAML-Namespace für einen benutzerdefinierten Typ mit einer angefügten Eigenschaft, die in derselben Assembly wie den Code der Anwendung definiert ist, die den benutzerdefinierten Typ verweist:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Die Namespacedeklaration wird verwendet, wenn die angefügte Eigenschaft für ein bestimmtes Steuerelement festlegen, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Nutzen eine angefügte Eigenschaft mit einer Formatvorlage

Angefügte Eigenschaften können auch eine Formatvorlage auf ein Steuerelement hinzugefügt werden. Das folgende Beispiel zeigt für die Verwendung von XAML-Code ein *explizite* Format, das verwendet die `HasShadow` angefügten Eigenschaft, die auf angewendet werden kann [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelemente:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

Die [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) angewendet werden können, um eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) durch Festlegen seiner [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft, um die `Style` -Zielinstanz aus, die `StaticResource`Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

Wenn Sie eine angefügte Eigenschaft erstellen, stehen eine Reihe von optionalen Parametern, die festgelegt werden können, um erweiterte angefügte Eigenschaft Szenarien zu ermöglichen. Dies schließt das Erkennen von eigenschaftsänderungen, überprüfen die Eigenschaftswerte und Eigenschaftswerte umzuwandeln. Weitere Informationen finden Sie unter [erweiterte Szenarien der](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eine Einführung in die angefügte Eigenschaften bereitgestellt und veranschaulicht, wie zum Erstellen und nutzen müssen. Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in eine Klasse jedoch auf andere Objekte angefügt und erkennbaren in XAML definiert wird, als Attribute, die eine Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt enthalten.


## <a name="related-links"></a>Verwandte Links

- [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Schatten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
