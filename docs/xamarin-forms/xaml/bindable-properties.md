---
title: Bindbare xamarin. Forms-Eigenschaften
description: Dieser Artikel bietet eine Einführung in bindbare Eigenschaften und veranschaulicht das Erstellen und deren Nutzung.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 56583aa0df676ae55e1b283f1a8e151b69a13d28
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635748"
---
# <a name="xamarinforms-bindable-properties"></a>Bindbare xamarin. Forms-Eigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

Bindbare Eigenschaften erweitern die CLR-Eigenschafts Funktionalität durch das unterstützen einer Eigenschaft mit einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Typs, anstatt eine Eigenschaft mit einem Feld zu unterstützen. Bindbare Eigenschaften zu einem Eigenschaftensystem bereitgestellt wird, die Datenbindung, Stile, Vorlagen unterstützt, dient, und Werte, die über übergeordnete und untergeordnete Beziehungen festgelegt. Darüber hinaus bieten bindbare Eigenschaften Standardwerte aufweisen, Eigenschaftswerte und die Rückrufe, die eigenschaftenänderungen zu überwachen.

Eigenschaften sollten als bindbare Eigenschaften zur Unterstützung von mindestens einer der folgenden Funktionen implementiert werden:

- Das fungieren als gültige *Ziel* Eigenschaft für die Datenbindung.
- Festlegen der-Eigenschaft über einen [Stil](~/xamarin-forms/user-interface/styles/index.md).
- Bereitstellen von Standardwert der Eigenschaft, die der Standardwert für den Typ der Eigenschaft unterscheidet.
- Überprüfen den Wert der Eigenschaft.
- Überwachung von eigenschaftenänderungen.

Beispiele für die bindbaren Eigenschaften von xamarin. Forms sind [`Label.Text`](xref:Xamarin.Forms.Label.Text), [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius)und [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation). Jede bindbare Eigenschaft verfügt über eine entsprechende `public static readonly`-Eigenschaft vom Typ [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) , die für die gleiche Klasse verfügbar gemacht wird und der Bezeichner der bindbaren Eigenschaft ist. Beispielsweise ist der entsprechende bindbare Eigenschaften Bezeichner für die `Label.Text`-Eigenschaft [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty).

## <a name="create-a-bindable-property"></a>Erstellen einer bindbaren Eigenschaft

Der Erstellungsprozess für eine bindbare Eigenschaft lautet wie folgt aus:

1. Erstellen Sie eine [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Instanz mit einer der [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) -Methoden Überladungen.
1. Definieren Sie Eigenschaftenaccessoren für die [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz.

Alle [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanzen müssen im UI-Thread erstellt werden. Dies bedeutet, dass nur Code, der auf den UI-Thread ausgeführt wird, kann abrufen oder legen Sie den Wert, der eine bindbare Eigenschaft. Allerdings kann auf `BindableProperty` Instanzen von anderen Threads aus zugegriffen werden, indem ein Marshalling zum UI-Thread mit der [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) -Methode erfolgt.

### <a name="create-a-property"></a>Erstellen einer Eigenschaft

Um eine `BindableProperty` Instanz zu erstellen, muss die enthaltende Klasse von der [`BindableObject`](xref:Xamarin.Forms.BindableObject) Klasse abgeleitet werden. Allerdings ist die `BindableObject` Klasse in der Klassenhierarchie hoch, sodass die Mehrzahl der Klassen, die für die Funktionalität der Benutzeroberfläche verwendet werden, bindbare Eigenschaften unterstützen.

Eine bindbare Eigenschaft kann erstellt werden, indem Sie eine `public static readonly`-Eigenschaft des Typs [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)deklarieren. Die bindbare Eigenschaft sollte auf den zurückgegebenen Wert einer der [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Methoden Überladungen festgelegt werden. Die Deklaration sollte sich innerhalb des Texts [`BindableObject`](xref:Xamarin.Forms.BindableObject) abgeleiteter Klasse befinden, jedoch außerhalb von Element Definitionen.

Ein Bezeichner muss mindestens beim Erstellen einer [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)zusammen mit den folgenden Parametern angegeben werden:

- Der Name der [`BindableProperty`](xref:Xamarin.Forms.BindableProperty).
- Der Typ der Eigenschaft.
- Der Typ des besitzenden Objekts.
- Der Standardwert für die Eigenschaft. Dadurch wird sichergestellt, dass die Eigenschaft gibt immer einen bestimmten Standardwert zurück, wenn es nicht festgelegt ist, und wird der Standardwert für den Typ der Eigenschaft unterscheidet. Der Standardwert wird wieder hergestellt, wenn die [`ClearValue`](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) -Methode für die bindbare Eigenschaft aufgerufen wird.

Der folgende Code zeigt ein Beispiel für eine bindbare Eigenschaft und einen Bezeichner und Werte für die vier erforderliche Parameter:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Dadurch wird eine [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz mit dem Namen `EventName`vom Typ `string`erstellt. Die-Eigenschaft ist im Besitz der `EventToCommandBehavior`-Klasse und hat den Standardwert `null`. Die Benennungs Konvention für bindbare Eigenschaften besteht darin, dass der Bezeichner der bindbaren Eigenschaft mit dem Eigenschaftsnamen übereinstimmen muss, der in der `Create`-Methode angegeben wird, wobei "Property" angehängt wird. Daher wird im obigen Beispiel der Bezeichner der bindbaren Eigenschaft `EventNameProperty`.

Optional können beim Erstellen einer [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Instanz die folgenden Parameter angegeben werden:

- Der Bindungsmodus. Dies wird verwendet, um die Richtung anzugeben, in der Änderungen von Eigenschaftswerten weitergegeben. Im standardmäßigen Bindungs Modus werden Änderungen von der *Quelle* an das *Ziel*weitergegeben.
- Eine Überprüfung-Delegat, der aufgerufen wird, wenn der Eigenschaftswert festgelegt ist. Weitere Informationen finden Sie unter [Validierungs Rückrufe](#validation-callbacks).
- Eine Eigenschaft geändert, Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert wurde. Weitere Informationen finden Sie unter [Erkennen von Eigenschafts Änderungen](#detect-property-changes).
- Eine Eigenschaft ändern der Delegat, der aufgerufen wird, wenn der Eigenschaftswert ändert. Dieser Delegat hat die gleiche Signatur wie der Delegat für die Eigenschaft geändert wurde.
- Ein Coerce-Wert-Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert wurde. Weitere Informationen finden Sie unter [coerce Value-Rückrufe](#coerce-value-callbacks).
- Eine `Func`, die verwendet wird, um einen Standard Eigenschafts Wert zu initialisieren. Weitere Informationen finden Sie unter [Erstellen eines Standardwerts mit einem Func](#create-a-default-value-with-a-func).

### <a name="create-accessors"></a>Erstellen von Accessoren

Eigenschaftenaccessoren sind erforderlich, um die Syntax für Eigenschaften zu verwenden, um eine bindbare Eigenschaft zuzugreifen. Der `Get`-Accessor sollte den Wert zurückgeben, der in der entsprechenden bindbare-Eigenschaft enthalten ist. Dies kann erreicht werden, indem die [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) -Methode aufgerufen wird, wobei der bindbare Eigenschafts Bezeichner übergeben wird, für den der Wert abzurufen ist, und das Ergebnis dann in den erforderlichen Typ umgewandelt wird. Der `Set`-Accessor sollte den Wert der entsprechenden bindbaren Eigenschaft festlegen. Dies kann erreicht werden, indem Sie die [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) -Methode aufrufen und dabei den bindbaren Eigenschaften Bezeichner, für den der Wert festgelegt werden soll, und den festzulegenden Wert übergeben.

Das folgende Codebeispiel zeigt Accessoren für die `EventName` bindbare-Eigenschaft:

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>Verwenden einer bindbaren Eigenschaft

Sobald eine bindbare Eigenschaft erstellt wurde, können sie XAML oder Code genutzt werden. In XAML erfolgt dies durch einen Namespace mit einem Präfix deklariert, mit der Namespacedeklaration, der angibt, die CLR-Namespace-Namen und optional einen Assemblynamen. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Im folgenden Codebeispiel wird veranschaulicht, einen XAML-Namespace für einen benutzerdefinierten Typ, der eine bindbare Eigenschaft und enthält die in der gleichen Assembly wie der Anwendungscode definiert ist, die auf den benutzerdefinierten Typ verweisen:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Die Namespace Deklaration wird verwendet, wenn die `EventName` bindbare-Eigenschaft festgelegt wird, wie im folgenden XAML-Codebeispiel gezeigt:

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

Beim Erstellen einer [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanz gibt es eine Reihe optionaler Parameter, die festgelegt werden können, um erweiterte bindbare Eigenschafts Szenarios zu aktivieren. In diesem Abschnitt werden diese Szenarien.

### <a name="detect-property-changes"></a>Erkennen von Eigenschafts Änderungen

Eine `static` Eigenschaften geänderte Rückruf Methode kann mit einer bindbaren Eigenschaft registriert werden, indem der `propertyChanged` Parameter für die [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) -Methode angegeben wird. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindungsfähige Eigenschaft ändert.

Im folgenden Codebeispiel wird veranschaulicht, wie die `EventName` bindbare-Eigenschaft die `OnEventNameChanged`-Methode als Rückruf Methode für die Eigenschaften Änderung registriert:

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

In der Rückruf Methode für die Eigenschaften Änderung wird der [`BindableObject`](xref:Xamarin.Forms.BindableObject) -Parameter verwendet, um anzugeben, welche Instanz der besitzenden Klasse eine Änderung gemeldet hat, und die Werte der beiden `object` Parameter stellen die alten und neuen Werte der bindbaren Eigenschaft dar.

### <a name="validation-callbacks"></a>Validierungs Rückrufe

Eine `static` Validierungs Rückruf Methode kann mit einer bindbaren Eigenschaft registriert werden, indem der `validateValue` Parameter für die [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) -Methode angegeben wird. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindbare Eigenschaft festgelegt ist.

Das folgende Codebeispiel zeigt, wie die `Angle` bindbare-Eigenschaft die `IsValidValue`-Methode als Validierungs Rückruf Methode registriert:

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

Validierungs Rückrufe werden mit einem Wert bereitgestellt und sollten `true` zurückgeben, wenn der Wert für die Eigenschaft gültig ist, andernfalls `false`. Eine Ausnahme wird ausgelöst, wenn ein Validierungs Rückruf `false`zurückgibt, die vom Entwickler behandelt werden soll. Eine typische Verwendung einer Rückrufmethode für die Überprüfung wird die Werte für ganze Zahlen oder Double-Werte einschränken, wenn die bindbare Eigenschaft festgelegt ist. Beispielsweise prüft die `IsValidValue`-Methode, ob der Eigenschafts Wert ein `double` im Bereich von 0 bis 360 ist.

### <a name="coerce-value-callbacks"></a>Coerce-Wert Rückrufe

Eine `static` coerce-Wert-Rückruf Methode kann mit einer bindbaren Eigenschaft registriert werden, indem der `coerceValue` Parameter für die [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) -Methode angegeben wird. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindungsfähige Eigenschaft ändert.

> [!IMPORTANT]
> Der `BindableObject`-Typ verfügt über eine `CoerceValue`-Methode, die aufgerufen werden kann, um eine erneute Auswertung des Werts des `BindableProperty` Arguments zu erzwingen, indem der recoerce-Wert Rückruf aufgerufen wird.

Coerce-Wert, der Rückrufe verwendet werden, um eine erneute Auswertung der eine bindbare Eigenschaft zu erzwingen, wenn der Wert der Eigenschaft geändert. Beispielsweise kann ein Coerce-Wert-Rückruf verwendet werden, um sicherzustellen, dass der Wert eine bindbare Eigenschaft nicht größer als der Wert einer anderen bindbare Eigenschaft ist.

Im folgenden Codebeispiel wird veranschaulicht, wie die `Angle` bindbare-Eigenschaft die `CoerceAngle`-Methode als coerce-Wert-Rückruf Methode registriert:

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

Die `CoerceAngle`-Methode überprüft den Wert der `MaximumAngle`-Eigenschaft. wenn der `Angle`-Eigenschafts Wert größer als der Wert ist, wird der Wert in den `MaximumAngle`-Eigenschafts Wert umgewandelt. Wenn die `MaximumAngle`-Eigenschaft geändert wird, wird der coerce-Wert Rückruf für die `Angle`-Eigenschaft aufgerufen, indem die `CoerceValue`-Methode aufgerufen wird.

### <a name="create-a-default-value-with-a-func"></a>Erstellen eines Standardwerts mit einem Func

Eine `Func` kann verwendet werden, um den Standardwert einer bindbaren Eigenschaft zu initialisieren, wie im folgenden Codebeispiel gezeigt:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

Der `defaultValueCreator`-Parameter ist auf eine `Func` festgelegt, die die [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) -Methode aufruft, um eine `double` zurückzugeben, die die benannte Größe für die Schriftart darstellt, die in einer [`Label`](xref:Xamarin.Forms.Label) auf der nativen Plattform verwendet wird.

## <a name="related-links"></a>Verwandte Links

- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Verhalten von Ereignis zu Befehl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Validierungs Rückruf (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [Coerce-Wert Rückruf (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [Bindableproperty-API](xref:Xamarin.Forms.BindableProperty)
- [Bindableobject-API](xref:Xamarin.Forms.BindableObject)
