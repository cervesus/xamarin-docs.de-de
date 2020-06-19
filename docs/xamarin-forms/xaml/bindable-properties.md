---
title: Xamarin.FormsBindbare Eigenschaften
description: Dieser Artikel bietet eine Einführung in bindbare Eigenschaften und zeigt, wie Sie erstellt und genutzt werden.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 33b3763075b64ea8af615465825313a527d20db2
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138176"
---
# <a name="xamarinforms-bindable-properties"></a>Xamarin.FormsBindbare Eigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

Bindbare Eigenschaften erweitern die CLR-Eigenschafts Funktionalität durch das Sichern einer Eigenschaft mit einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Typ, anstatt eine Eigenschaft mit einem Feld zu unterstützen. Der Zweck der bindbaren Eigenschaften ist das Bereitstellen eines Eigenschaften Systems, das Daten Bindungen, Stile, Vorlagen und Werte unterstützt, die über Beziehungen zwischen übergeordneten und untergeordneten Elementen festgelegt werden. Außerdem können bindbare Eigenschaften Standardwerte, Validierung von Eigenschafts Werten und Rückrufe bereitstellen, die Eigenschaften Änderungen überwachen.

Eigenschaften sollten als bindbare Eigenschaften implementiert werden, um eine oder mehrere der folgenden Funktionen zu unterstützen:

- Das fungieren als gültige *Ziel* Eigenschaft für die Datenbindung.
- Festlegen der-Eigenschaft über einen [Stil](~/xamarin-forms/user-interface/styles/index.md).
- Bereitstellen eines Standard Eigenschafts Werts, der sich vom Standardwert für den Typ der Eigenschaft unterscheidet.
- Validieren des Werts der-Eigenschaft.
- Überwachen von Eigenschafts Änderungen.

Beispiele für Xamarin.Forms bindbare Eigenschaften sind [`Label.Text`](xref:Xamarin.Forms.Label.Text) , [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) und [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) . Jede bindbare Eigenschaft verfügt über ein entsprechendes `public static readonly` Feld vom Typ [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , das für die gleiche Klasse verfügbar gemacht wird und der Bezeichner der bindbaren Eigenschaft ist. Beispielsweise ist der entsprechende bindbare Eigenschaften Bezeichner für die- `Label.Text` Eigenschaft [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty) .

## <a name="create-a-bindable-property"></a>Erstellen einer bindbaren Eigenschaft

Der Prozess zum Erstellen einer bindbaren Eigenschaft lautet wie folgt:

1. Erstellen Sie eine- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz mit einer der- [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) Methoden Überladungen.
1. Definieren Sie Eigenschaftenaccessoren für die- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz.

Alle [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanzen müssen im UI-Thread erstellt werden. Dies bedeutet, dass nur Code, der im UI-Thread ausgeführt wird, den Wert einer bindbaren Eigenschaft erhalten oder festlegen kann. Auf- `BindableProperty` Instanzen kann jedoch von anderen Threads aus zugegriffen werden, indem mit der-Methode ein Marshalling zum UI-Thread durchlaufen wird [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) .

### <a name="create-a-property"></a>Erstellen einer Eigenschaft

Zum Erstellen einer- `BindableProperty` Instanz muss die enthaltende Klasse von der-Klasse abgeleitet werden [`BindableObject`](xref:Xamarin.Forms.BindableObject) . Die `BindableObject` Klasse ist jedoch in der Klassenhierarchie hoch, sodass die Mehrzahl der Klassen, die für die Funktionalität der Benutzeroberfläche verwendet werden, bindbare Eigenschaften unterstützen.

Eine bindbare Eigenschaft kann erstellt werden, indem eine `public static readonly` Eigenschaft vom Typ deklariert wird [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Die bindbare Eigenschaft sollte auf den zurückgegebenen Wert von einem der [ `BindableProperty.Create` ] (Xref:) festgelegt werden Xamarin.Forms . Bindableproperty. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Bindableproperty. validatevaluedelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangeddelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangingdelegat, Xamarin.Forms . Bindableproperty. coercevaluedelegat, Xamarin.Forms . Bindableproperty. kreatedefaultvaluedelegat)-Methoden Überladungen. Die Deklaration sollte sich innerhalb des Texts der [`BindableObject`](xref:Xamarin.Forms.BindableObject) abgeleiteten Klasse befinden, jedoch außerhalb von Element Definitionen.

Es muss mindestens ein Bezeichner angegeben werden, wenn ein erstellt wird [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , zusammen mit den folgenden Parametern:

- Der Name des [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .
- Der Typ der Eigenschaft.
- Der Typ des besitzenden Objekts.
- Der Standardwert für die Eigenschaft. Dadurch wird sichergestellt, dass die-Eigenschaft immer einen bestimmten Standardwert zurückgibt, wenn Sie nicht festgelegt ist, und Sie kann sich vom Standardwert für den Typ der Eigenschaft unterscheiden. Der Standardwert wird wieder hergestellt, wenn [ `ClearValue` ] (Xref: Xamarin.Forms . Bindableobject. ClearValue ( Xamarin.Forms . Bindableproperty))-Methode wird für die bindbare Eigenschaft aufgerufen.

Der folgende Code zeigt ein Beispiel für eine bindbare Eigenschaft mit einem Bezeichner und Werten für die vier erforderlichen Parameter:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Dadurch wird eine- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz `EventName` mit dem Namen vom Typ erstellt `string` . Die-Eigenschaft ist im Besitz der `EventToCommandBehavior` -Klasse, und hat den Standardwert `null` . Die Benennungs Konvention für bindbare Eigenschaften besteht darin, dass der Bezeichner der bindbaren Eigenschaft mit dem in der Methode angegebenen Eigenschaftsnamen übereinstimmen muss `Create` , wobei "Property" angehängt wird. Im obigen Beispiel lautet der Bezeichner der bindbaren Eigenschaft daher `EventNameProperty` .

Wenn Sie eine- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz erstellen, können Sie optional die folgenden Parameter angeben:

- Der Bindungsmodus. Hiermit wird die Richtung angegeben, in der Eigenschafts Wertänderungen weitergegeben werden. Im standardmäßigen Bindungs Modus werden Änderungen von der *Quelle* an das *Ziel*weitergegeben.
- Ein Validierungs Delegat, der aufgerufen wird, wenn der-Eigenschafts Wert festgelegt wird. Weitere Informationen finden Sie unter [Validierungs Rückrufe](#validation-callbacks).
- Ein von der Eigenschaft geänderter Delegat, der aufgerufen wird, wenn der Eigenschafts Wert geändert wurde. Weitere Informationen finden Sie unter [Erkennen von Eigenschafts Änderungen](#detect-property-changes).
- Ein Eigenschaften Wechsel Delegat, der aufgerufen wird, wenn sich der Eigenschafts Wert ändert. Dieser Delegat hat dieselbe Signatur wie der Delegat, der geändert wurde.
- Ein coerce-Wert Delegat, der aufgerufen wird, wenn sich der-Eigenschafts Wert geändert hat. Weitere Informationen finden Sie unter [coerce Value-Rückrufe](#coerce-value-callbacks).
- Ein `Func` , der verwendet wird, um einen Standard Eigenschafts Wert zu initialisieren. Weitere Informationen finden Sie unter [Erstellen eines Standardwerts mit einem Func](#create-a-default-value-with-a-func).

### <a name="create-accessors"></a>Erstellen von Accessoren

Eigenschaftenaccessoren sind erforderlich, um die Eigenschaften Syntax für den Zugriff auf eine bindbare Eigenschaft zu verwenden. Der `Get` -Accessor sollte den Wert zurückgeben, der in der entsprechenden bindbare-Eigenschaft enthalten ist. Dies kann durch Aufrufen von [ `GetValue` ] (Xref:) erreicht werden Xamarin.Forms . Bindableobject. GetValue ( Xamarin.Forms . Bindableproperty))-Methode. übergeben Sie den bindbaren Eigenschafts Bezeichner, für den der Wert ausgegeben werden soll, und wandeln Sie das Ergebnis dann in den erforderlichen Typ um. Der- `Set` Accessor sollte den Wert der entsprechenden bindbaren Eigenschaft festlegen. Dies kann durch Aufrufen von [ `SetValue` ] (Xref:) erreicht werden Xamarin.Forms . Bindableobject. SetValue ( Xamarin.Forms . Bindableproperty, System. Object))-Methode, wobei der bindbare Eigenschafts Bezeichner übergeben wird, für den der Wert festgelegt werden soll, und der festzulegende Wert.

Das folgende Codebeispiel zeigt Accessoren für die `EventName` bindbare Eigenschaft:

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>Verwenden einer bindbaren Eigenschaft

Nachdem eine bindbare Eigenschaft erstellt wurde, kann Sie von XAML oder Code verwendet werden. In XAML wird dies erreicht, indem ein Namespace mit einem Präfix deklariert wird, wobei die Namespace Deklaration den Namen des CLR-Namespace und optional einen Assemblynamen angibt. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Das folgende Codebeispiel veranschaulicht einen XAML-Namespace für einen benutzerdefinierten Typ, der eine bindbare Eigenschaft enthält, die in derselben Assembly wie der Anwendungscode definiert ist, der auf den benutzerdefinierten Typ verweist:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Die Namespace Deklaration wird verwendet, wenn die `EventName` bindbare Eigenschaft festgelegt wird, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior
{
  EventName = "ItemSelected",
  ...
});
```

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

Wenn Sie eine- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz erstellen, gibt es eine Reihe optionaler Parameter, die festgelegt werden können, um erweiterte bindbare Eigenschafts Szenarios zu aktivieren. In diesem Abschnitt werden diese Szenarios erläutert.

### <a name="detect-property-changes"></a>Erkennen von Eigenschafts Änderungen

Eine `static` Eigenschaften geänderte Rückruf Methode kann mit einer bindbaren Eigenschaft registriert werden, indem der- `propertyChanged` Parameter für [ `BindableProperty.Create` ] (Xref:) angegeben wird Xamarin.Forms . Bindableproperty. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Bindableproperty. validatevaluedelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangeddelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangingdelegat, Xamarin.Forms . Bindableproperty. coercevaluedelegat, Xamarin.Forms . Bindableproperty. kreatedefaultvaluedelegat)-Methode. Die angegebene Rückruf Methode wird aufgerufen, wenn der Wert der bindbaren Eigenschaft geändert wird.

Im folgenden Codebeispiel wird veranschaulicht, wie die `EventName` bindbare Eigenschaft die- `OnEventNameChanged` Methode als Rückruf Methode für die Eigenschaften Änderung registriert:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

In der Rückruf Methode für die Eigenschaften Änderung wird der- [`BindableObject`](xref:Xamarin.Forms.BindableObject) Parameter verwendet, um anzugeben, welche Instanz der besitzenden Klasse eine Änderung gemeldet hat, und die Werte der beiden `object` Parameter stellen die alten und neuen Werte der bindbaren Eigenschaft dar.

### <a name="validation-callbacks"></a>Validierungs Rückrufe

Eine `static` Validierungs Rückruf Methode kann mit einer bindbaren Eigenschaft registriert werden, indem der- `validateValue` Parameter für [ `BindableProperty.Create` ] (Xref:) angegeben wird Xamarin.Forms . Bindableproperty. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Bindableproperty. validatevaluedelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangeddelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangingdelegat, Xamarin.Forms . Bindableproperty. coercevaluedelegat, Xamarin.Forms . Bindableproperty. kreatedefaultvaluedelegat)-Methode. Die angegebene Rückruf Methode wird aufgerufen, wenn der Wert der bindbaren Eigenschaft festgelegt wird.

Das folgende Codebeispiel zeigt, wie die `Angle` bindbare Eigenschaft die `IsValidValue` Methode als Validierungs Rückruf Methode registriert:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Validierungs Rückrufe werden mit einem-Wert bereitgestellt und sollten zurückgeben, `true` Wenn der Wert für die-Eigenschaft gültig ist, andernfalls `false` . Eine Ausnahme wird ausgelöst, wenn ein Validierungs Rückruf zurückgegeben wird `false` , der vom Entwickler behandelt werden soll. Eine typische Verwendung einer Validierungs Rückruf Methode ist das Einschränken der Werte von Ganzzahlen oder verdoppelt, wenn die bindbare Eigenschaft festgelegt ist. Beispielsweise wird von der- `IsValidValue` Methode überprüft, ob der-Eigenschafts Wert eine `double` im Bereich von 0 bis 360 ist.

### <a name="coerce-value-callbacks"></a>Coerce-Wert Rückrufe

Eine `static` coerce-Wert Rückruf Methode kann mit einer bindbaren Eigenschaft registriert werden, indem der- `coerceValue` Parameter für [ `BindableProperty.Create` ] (Xref:) angegeben wird Xamarin.Forms . Bindableproperty. Create (System. String, System. Type, System. Type, System. Object, Xamarin.Forms . BindingMode, Xamarin.Forms . Bindableproperty. validatevaluedelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangeddelegat, Xamarin.Forms . Bindableproperty. bindingpropertychangingdelegat, Xamarin.Forms . Bindableproperty. coercevaluedelegat, Xamarin.Forms . Bindableproperty. kreatedefaultvaluedelegat)-Methode. Die angegebene Rückruf Methode wird aufgerufen, wenn der Wert der bindbaren Eigenschaft geändert wird.

> [!IMPORTANT]
> Der- `BindableObject` Typ verfügt über eine- `CoerceValue` Methode, die aufgerufen werden kann, um eine erneute Auswertung des Werts seines Arguments zu erzwingen `BindableProperty` , indem der recoerce-Wert Rückruf aufgerufen wird.

Coerce-Wert Rückrufe werden verwendet, um eine erneute Auswertung einer bindbaren Eigenschaft zu erzwingen, wenn sich der Wert der Eigenschaft ändert. Beispielsweise kann ein coerce-Wert-Rückruf verwendet werden, um sicherzustellen, dass der Wert einer bindbaren Eigenschaft nicht größer als der Wert einer anderen bindbaren Eigenschaft ist.

Im folgenden Codebeispiel wird veranschaulicht, wie die `Angle` bindbare Eigenschaft die- `CoerceAngle` Methode als coerce-Wert-Rückruf Methode registriert:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0, propertyChanged: ForceCoerceValue);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle)
  {
    input = homePage.MaximumAngle;
  }
  return input;
}

static void ForceCoerceValue(BindableObject bindable, object oldValue, object newValue)
{
  bindable.CoerceValue(AngleProperty);
}
```

Die `CoerceAngle` -Methode überprüft den Wert der `MaximumAngle` -Eigenschaft, und wenn der- `Angle` Eigenschafts Wert größer als der Wert ist, wird der Wert in den- `MaximumAngle` Eigenschafts Wert umgewandelt. Wenn die- `MaximumAngle` Eigenschaft geändert wird, wird der Rückruf für den coerce-Wert für die-Eigenschaft aufgerufen, `Angle` indem die-Methode aufgerufen wird `CoerceValue` .

### <a name="create-a-default-value-with-a-func"></a>Erstellen eines Standardwerts mit einem Func

Ein `Func` kann verwendet werden, um den Standardwert einer bindbaren Eigenschaft zu initialisieren, wie im folgenden Codebeispiel gezeigt:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

Der- `defaultValueCreator` Parameter wird auf einen festgelegt `Func` , der [ `Device.GetNamedSize` ] (Xref:) aufruft Xamarin.Forms . Device. getnamedsize ( Xamarin.Forms . Namedsize, System. Type))-Methode, um einen zurückzugeben `double` , der die benannte Größe für die Schriftart darstellt, die auf einem [`Label`](xref:Xamarin.Forms.Label) auf der nativen Plattform verwendet wird.

## <a name="related-links"></a>Verwandte Links

- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Verhalten von Ereignis zu Befehl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Validierungs Rückruf (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [Coerce-Wert Rückruf (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [Bindableproperty-API](xref:Xamarin.Forms.BindableProperty)
- [Bindableobject-API](xref:Xamarin.Forms.BindableObject)
