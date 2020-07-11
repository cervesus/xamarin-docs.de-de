---
title: Angefügte Eigenschaften
description: Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und zeigt, wie diese erstellt und genutzt werden.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1277b3cd875c1b4e05e45202a8e30ef2ff93972a
ms.sourcegitcommit: 898ba8e5140ae32a7df7e07c056aff65f6fe4260
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226793"
---
# <a name="attached-properties"></a>Angefügte Eigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)


Angefügte Eigenschaften ermöglichen einem Objekt das Zuweisen eines Werts für eine Eigenschaft, die von der eigenen Klasse nicht definiert wird. Beispielsweise können untergeordnete Elemente angefügte Eigenschaften verwenden, um das übergeordnete Element darüber zu informieren, wie Sie in der Benutzeroberfläche angezeigt werden sollen. Das [`Grid`](xref:Xamarin.Forms.Grid) -Steuerelement ermöglicht das Angeben der Zeile und der Spalte eines untergeordneten Elements durch Festlegen der `Grid.Row` `Grid.Column` angefügten Eigenschaften und. `Grid.Row`und `Grid.Column` sind angefügte Eigenschaften, da Sie auf Elemente festgelegt werden, die untergeordnete Elemente eines sind `Grid` , und nicht auf dem `Grid` selbst.

Bindbare Eigenschaften sollten in den folgenden Szenarien als angefügte Eigenschaften implementiert werden:

- Wenn ein Eigenschafts Einstellungs Mechanismus für andere Klassen als die definierende Klasse verfügbar sein muss.
- Wenn die-Klasse einen Dienst darstellt, der problemlos in andere Klassen integriert werden muss.

Weitere Informationen zu bindbaren Eigenschaften finden Sie unter [bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="create-an-attached-property"></a>Erstellen einer angefügten Eigenschaft

Der Prozess zum Erstellen einer angefügten Eigenschaft lautet wie folgt:

1. Erstellen Sie eine- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz mit einer der- [`CreateAttached`](xref:Xamarin.Forms.BindableProperty.CreateAttached*) Methoden Überladungen.
1. Stellen Sie `static` `Get` die Methoden *propertyName* und `Set` *propertyName* als Accessoren für die angefügte-Eigenschaft bereit.

### <a name="create-a-property"></a>Erstellen einer Eigenschaft

Beim Erstellen einer angefügten Eigenschaft für die Verwendung in anderen Typen muss die Klasse, in der die-Eigenschaft erstellt wird, nicht von abgeleitet werden [`BindableObject`](xref:Xamarin.Forms.BindableObject) . Die *Ziel* Eigenschaft für Accessoren sollte jedoch von oder abgeleitet werden [`BindableObject`](xref:Xamarin.Forms.BindableObject) .

Eine angefügte Eigenschaft kann erstellt werden, indem eine `public static readonly` Eigenschaft vom Typ deklariert wird [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Die bindbare Eigenschaft sollte auf den zurückgegebenen Wert von einem der [ `BindableProperty.CreateAttached` ] (Xref:) festgelegt werden Xamarin.Forms . Bindableproperty. kreateattached (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Bindableproperty. validatevaluedelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangeddelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangingdelegat, Xamarin.Forms . Bindableproperty. coercevaluedelegat, Xamarin.Forms . Bindableproperty. kreatedefaultvaluedelegat)-Methoden Überladungen. Die Deklaration sollte innerhalb des Texts der besitzenden Klasse, aber außerhalb von Element Definitionen liegen.

> [!IMPORTANT]
> Die Benennungs Konvention für angefügte Eigenschaften besteht darin, dass der Bezeichner der angefügten Eigenschaft mit dem in der Methode angegebenen Eigenschaftsnamen übereinstimmen muss `CreateAttached` , wobei "Property" angehängt wird.

Der folgende Code zeigt ein Beispiel für eine angefügte Eigenschaft:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Dadurch wird eine angefügte Eigenschaft `HasShadowProperty` mit dem Namen des Typs erstellt `bool` . Die-Eigenschaft ist im Besitz der `ShadowEffect` -Klasse, und hat den Standardwert `false` .

Weitere Informationen zum Erstellen von bindbaren Eigenschaften, einschließlich Parametern, die während der Erstellung angegeben werden können, finden Sie unter [Erstellen einer bindbare-Eigenschaft](~/xamarin-forms/xaml/bindable-properties.md#consume-a-bindable-property).

### <a name="create-accessors"></a>Erstellen von Accessoren

Statische `Get` *propertyName* - `Set` Methode und *propertyName* -Methode sind als Accessoren für die angefügte-Eigenschaft erforderlich. andernfalls kann das Eigenschaften System die angefügte-Eigenschaft nicht verwenden. Der `Get` *propertyName* -Accessor sollte der folgenden Signatur entsprechen:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

Der `Get` *propertyName* -Accessor sollte den Wert zurückgeben, der im entsprechenden `BindableProperty` Feld für die angefügte Eigenschaft enthalten ist. Dies kann durch Aufrufen von [ `GetValue` ] (Xref:) erreicht werden Xamarin.Forms . Bindableobject. GetValue ( Xamarin.Forms . Bindableproperty))-Methode, wobei der bindbare Eigenschafts Bezeichner übergeben wird, für den der Wert ausgegeben werden soll, und der resultierende Wert dann in den erforderlichen Typ umgewandelt wird.

Der `Set` *propertyName* -Accessor sollte der folgenden Signatur entsprechen:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

Der `Set` *propertyName* -Accessor sollte den Wert des entsprechenden `BindableProperty` Felds für die angefügte Eigenschaft festlegen. Dies kann durch Aufrufen von [ `SetValue` ] (Xref:) erreicht werden Xamarin.Forms . Bindableobject. SetValue ( Xamarin.Forms . Bindableproperty, System. Object))-Methode, wobei der bindbare Eigenschafts Bezeichner übergeben wird, für den der Wert festgelegt werden soll, und der festzulegende Wert.

Bei beiden Accessoren muss das *Ziel* Objekt von oder abgeleitet sein [`BindableObject`](xref:Xamarin.Forms.BindableObject) .

Das folgende Codebeispiel zeigt Accessoren für die `HasShadow` angefügte-Eigenschaft:

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

### <a name="consume-an-attached-property"></a>Verwenden einer angefügten Eigenschaft

Nachdem eine angefügte Eigenschaft erstellt wurde, kann Sie von XAML oder Code verwendet werden. In XAML wird dies erreicht, indem ein Namespace mit einem Präfix deklariert wird, wobei die Namespace Deklaration den CLR-Namespace Namen (Common Language Runtime) und optional einen Assemblynamen angibt. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Das folgende Codebeispiel veranschaulicht einen XAML-Namespace für einen benutzerdefinierten Typ, der eine angefügte-Eigenschaft enthält, die in derselben Assembly wie der Anwendungscode definiert ist, der auf den benutzerdefinierten Typ verweist:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Die Namespace Deklaration wird dann verwendet, wenn die angefügte-Eigenschaft für ein bestimmtes Steuerelement festgelegt wird, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consume-an-attached-property-with-a-style"></a>Verwenden einer angefügten Eigenschaft mit einem Stil

Angefügte Eigenschaften können auch einem Steuerelement über einen Stil hinzugefügt werden. Das folgende XAML-Codebeispiel zeigt einen *expliziten* Stil, der die `HasShadow` angefügte-Eigenschaft verwendet, die auf Steuerelemente angewendet werden kann [`Label`](xref:Xamarin.Forms.Label) :

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

Die [`Style`](xref:Xamarin.Forms.Style)-Klasse kann auf eine [`Label`](xref:Xamarin.Forms.Label)-Klasse angewendet werden, indem ihre [`Style`](xref:Xamarin.Forms.NavigableElement.Style)-Eigenschaft wie im folgenden Codebeispiel gezeigt mithilfe der Markuperweiterung `StaticResource` für die `Style`-Instanz festgelegt wird:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Weitere Informationen zu Formatvorlagen finden Sie unter [Styles (Formatvorlagen)](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

Beim Erstellen einer angefügten Eigenschaft gibt es eine Reihe optionaler Parameter, die festgelegt werden können, um erweiterte angefügte Eigenschafts Szenarios zu aktivieren. Dies umfasst das Erkennen von Eigenschafts Änderungen, das Überprüfen von Eigenschafts Werten und das Umwandeln von Eigenschafts Werten. Weitere Informationen finden Sie unter [Advanced Szenarios](~/xamarin-forms/xaml/bindable-properties.md#advanced-scenarios).

## <a name="related-links"></a>Verwandte Links

- [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Shadow Effect (Schatteneffekt (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
- [Bindableproperty-API](xref:Xamarin.Forms.BindableProperty)
- [Bindableobject-API](xref:Xamarin.Forms.BindableObject)
