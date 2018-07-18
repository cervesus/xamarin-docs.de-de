---
title: Angefügte Eigenschaften
description: Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und veranschaulicht das Erstellen und deren Nutzung.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 981e59fe3ba8c63d0f6c6a067ceb9f338a02da8f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997331"
---
# <a name="attached-properties"></a>Angefügte Eigenschaften

_Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft in einer Klasse definiert, aber mit anderen Objekten verbunden und in XAML als Attribut erkannt, die eine Klasse enthält, und ein Eigenschaftennamen an, die durch einen Punkt getrennt. Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und veranschaulicht das Erstellen und deren Nutzung._

## <a name="overview"></a>Übersicht

Angefügte Eigenschaften aktivieren, ein Objekt zuweisen ein Werts für eine Eigenschaft, die ihre eigene Klasse definieren, nicht an. Z. B. angefügte untergeordnete Elemente zu verwenden, können Eigenschaften, um darüber zu informieren jeweils übergeordneten Elements des wie diese werden in der Benutzeroberfläche angezeigt werden. Die [ `Grid` ](xref:Xamarin.Forms.Grid) Steuerelement ermöglicht es der Zeile und Spalte eines untergeordneten Elements durch Festlegen von angegeben, werden die `Grid.Row` und `Grid.Column` angefügte Eigenschaften. `Grid.Row` und `Grid.Column` angefügte Eigenschaften sind, da sie für Elemente festgelegt werden, die untergeordnete Elemente von einem `Grid`, anstatt auf die `Grid` selbst.

Bindbare Eigenschaften sollten als angefügte Eigenschaften in den folgenden Szenarien implementiert werden:

- Bei einen Einstellung Mechanismus verfügbar für Klassen aufweisen müssen Klasse definieren.
- Wenn die Klasse einen Dienst darstellt, der leicht in andere Klassen integriert werden muss.

Weitere Informationen über bindbare Eigenschaften finden Sie unter [bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Erstellen und nutzen eine angefügte Eigenschaft

Der Prozess zum Erstellen einer angefügten Eigenschaft lautet wie folgt aus:

1. Erstellen Sie eine [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanz mit einem der [ `CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached*) Überladungen der Methode.
1. Geben Sie `static` `Get` *PropertyName* und `Set` *PropertyName* Methoden als Accessoren für die angefügte Eigenschaft.

### <a name="creating-a-property"></a>Erstellen einer Eigenschaft

Wenn Sie eine angefügte Eigenschaft für die Verwendung auf anderen Typen zu erstellen, die Klasse, in dem die Eigenschaft wird erstellt, keine abgeleitet [ `BindableObject` ](xref:Xamarin.Forms.BindableObject). Allerdings die *Ziel* Eigenschaft für Accessoren sein sollte, oder abgeleitet, [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Eine angefügte Eigenschaft erstellt werden, indem Sie deklarieren eine `public static readonly` Eigenschaft vom Typ [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Die bindbare Eigenschaft muss festgelegt werden, auf den zurückgegebenen Wert eines der [ `BindableProperty.CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Überladungen der Methode. Die Deklaration sollte innerhalb des Texts von der besitzenden Klasse, jedoch außerhalb von Memberdefinitionen sein.

Der folgende Code zeigt ein Beispiel für eine angefügte Eigenschaft:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Dies erstellt eine angefügte Eigenschaft mit dem Namen `HasShadow`, des Typs `bool`. Die Eigenschaft ist im Besitz der `ShadowEffect` Klasse, und hat den Standardwert des `false`. Die Namenskonvention für angefügte Eigenschaften ist, dass der Bezeichner der angefügten Eigenschaft den Namen der Eigenschaft im angegebenen entsprechen, muss die `CreateAttached` Methode mit "Property" angefügt. Im obigen Beispiel ist der Bezeichner der angefügten Eigenschaft aus diesem Grund `HasShadowProperty`.

Weitere Informationen zum Erstellen von bindbare Eigenschaften, einschließlich der Parameter, die während der Erstellung angegeben werden können, finden Sie unter [erstellen und nutzen eine bindbare Eigenschaft](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Erstellen von Accessoren

Statische `Get` *PropertyName* und `Set` *PropertyName* Methoden als Accessoren für die angefügte Eigenschaft erforderlich sind, andernfalls ist das Eigenschaftensystem kann nicht verwendet das angefügte Eigenschaft. Die `Get` *PropertyName* Accessor sollte die folgende Signatur entsprechen:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

Die `Get` *PropertyName* Accessor sollte der enthaltene Wert zurückgeben, in der entsprechenden `BindableProperty` Feld für die angefügte Eigenschaft. Dies kann erreicht werden, durch den Aufruf der [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) Methode, übergebe die ID für die bindbare Eigenschaft auf dem den Wert abgerufen, und klicken Sie dann umwandeln den resultierenden Wert an den erforderlichen Typ.

Die `Set` *PropertyName* Accessor sollte die folgende Signatur entsprechen:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

Die `Set` *PropertyName* Accessor sollte legen Sie den Wert des entsprechenden `BindableProperty` Feld für die angefügte Eigenschaft. Dies kann erreicht werden, durch den Aufruf der [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) Methode und übergebe die ID für die bindbare Eigenschaft auf dem der Wert und den festzulegenden Wert festgelegt.

Für beide Accessoren die *Ziel* Objekt sein sollte, oder abgeleitet, [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Im folgenden Codebeispiel wird veranschaulicht, Accessoren für die `HasShadow` angefügte Eigenschaft:

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

Sobald eine angefügte Eigenschaft erstellt wurde, können sie XAML oder Code genutzt werden. In XAML erfolgt dies durch einen Namespace mit einem Präfix, mit der Namespacedeklaration, der angibt, der Namespacename des Common Language Runtime (CLR) und optional einen Assemblynamen deklarieren. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Im folgenden Codebeispiel wird veranschaulicht, einen XAML-Namespace für einen benutzerdefinierten Typ mit einer angefügten Eigenschaft, die in der gleichen Assembly wie der Anwendungscode definiert ist, die auf den benutzerdefinierten Typ verweisen:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Die Namespacedeklaration wird verwendet, wenn die angefügte Eigenschaft für ein bestimmtes Steuerelement festlegen, wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Nutzen eine angefügte Eigenschaft mit einem Stil

Angefügte Eigenschaften können auch durch einen Stil auf ein Steuerelement hinzugefügt werden. Das folgende XAML-Code-Beispiel zeigt eine *explizite* Stil, der verwendet die `HasShadow` angefügten Eigenschaft, die auf angewendet werden kann [ `Label` ](xref:Xamarin.Forms.Label) Steuerelemente:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

Die [ `Style` ](xref:Xamarin.Forms.Style) angewendet werden können, um eine [ `Label` ](xref:Xamarin.Forms.Label) durch Festlegen der [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaft, um die `Style` -Instanz unter Verwendung der `StaticResource`Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

Wenn Sie eine angefügte Eigenschaft zu erstellen, gibt es eine Reihe von optionalen Parametern, die festgelegt werden können, um erweiterte angefügte Eigenschaft Szenarien zu ermöglichen. Dies schließt das Erkennen von eigenschaftenänderungen, der Eigenschaftswerte Überprüfung und Koersion Eigenschaftswerte. Weitere Informationen finden Sie unter [erweiterte Szenarien der](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine Einführung in angefügte Eigenschaften und veranschaulicht, wie das Erstellen und nutzen diese. Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in einer Klasse, aber mit anderen Objekten verbunden und erkennbaren in XAML definiert wird, als Attribute, die eine Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt enthalten.


## <a name="related-links"></a>Verwandte Links

- [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Schatteneffekt (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
