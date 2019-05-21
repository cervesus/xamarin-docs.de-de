---
title: Bindbare Eigenschaften
description: Dieser Artikel bietet eine Einführung in bindbare Eigenschaften und veranschaulicht das Erstellen und deren Nutzung.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
ms.openlocfilehash: d3e1c8a2b17ba41e3c9d212948f42023c15205ac
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926236"
---
# <a name="bindable-properties"></a>Bindbare Eigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)

_In Xamarin.Forms wird die Funktionalität der common Language Runtime (CLR)-Eigenschaften von bindbare Eigenschaften erweitert. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, in denen der Wert der Eigenschaft wird vom Eigenschaftensystem Xamarin.Forms nachverfolgt. Dieser Artikel bietet eine Einführung in bindbare Eigenschaften und veranschaulicht das Erstellen und deren Nutzung._

## <a name="overview"></a>Übersicht

Bindbare Eigenschaften, die CLR-Eigenschaft-Funktionalität erweitern, indem Sie eine Eigenschaft mit einem [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Typ, anstatt eine Eigenschaft mit einem Feld. Bindbare Eigenschaften zu einem Eigenschaftensystem bereitgestellt wird, die Datenbindung, Stile, Vorlagen unterstützt, dient, und Werte, die über übergeordnete und untergeordnete Beziehungen festgelegt. Darüber hinaus bieten bindbare Eigenschaften Standardwerte aufweisen, Eigenschaftswerte und die Rückrufe, die eigenschaftenänderungen zu überwachen.

Eigenschaften sollten als bindbare Eigenschaften zur Unterstützung von mindestens einer der folgenden Funktionen implementiert werden:

- Fungiert als gültiger *Ziel* -Eigenschaft für die Datenbindung.
- Festlegen der Eigenschaft durch eine [Stil](~/xamarin-forms/user-interface/styles/index.md).
- Bereitstellen von Standardwert der Eigenschaft, die der Standardwert für den Typ der Eigenschaft unterscheidet.
- Überprüfen den Wert der Eigenschaft.
- Überwachung von eigenschaftenänderungen.

Beispiele für Xamarin.Forms bindbare Eigenschaften [ `Label.Text` ](xref:Xamarin.Forms.Label.Text), [ `Button.BorderRadius` ](xref:Xamarin.Forms.Button.BorderRadius), und [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation). Jede bindbare Eigenschaft verfügt über eine entsprechende `public static readonly` Eigenschaft vom Typ [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) , für die gleiche Klasse verfügbar gemacht wird und das ist der Bezeichner der bindbare Eigenschaft. Z. B. der entsprechenden bindbare Eigenschaftsbezeichner für die `Label.Text` Eigenschaft [ `Label.TextProperty` ](xref:Xamarin.Forms.Label.TextProperty).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Erstellen und nutzen eine bindbare Eigenschaft

Der Erstellungsprozess für eine bindbare Eigenschaft lautet wie folgt aus:

1. Erstellen Sie eine [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanz mit einem der [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create*) Überladungen der Methode.
1. Definieren von Eigenschaftenaccessoren für die [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanz.

Beachten Sie, dass alle [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanzen müssen im UI-Thread erstellt werden. Dies bedeutet, dass nur Code, der auf den UI-Thread ausgeführt wird, kann abrufen oder legen Sie den Wert, der eine bindbare Eigenschaft. Allerdings `BindableProperty` Instanzen von anderen Threads zugegriffen werden können, durch Marshalling an den UI-Thread mit der [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) Methode.

### <a name="creating-a-property"></a>Erstellen einer Eigenschaft

Zum Erstellen einer `BindableProperty` Instanz die enthaltende Klasse abgeleitet muss die [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Klasse. Allerdings die `BindableObject` Klasse ist in der Klassenhierarchie, hoch, damit die Mehrzahl der Klassen für die Funktionalität Unterstützung bindbare Eigenschaften der Benutzeroberfläche verwendet.

Eine bindbare Eigenschaft erstellt werden, indem Sie deklarieren eine `public static readonly` Eigenschaft vom Typ [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Die bindbare Eigenschaft muss festgelegt werden, auf den zurückgegebenen Wert eines der [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Überladungen der Methode. Die Deklaration muss innerhalb des Texts der [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) abgeleitete Klasse sein, jedoch außerhalb von Memberdefinitionen.

Mindestens ein Bezeichner muss angegeben werden beim Erstellen einer [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty), zusammen mit den folgenden Parametern:

- Der Name des der [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty).
- Den Typ der Eigenschaft.
- Der Typ des besitzenden Objekts.
- Der Standardwert für die Eigenschaft. Dadurch wird sichergestellt, dass die Eigenschaft gibt immer einen bestimmten Standardwert zurück, wenn es nicht festgelegt ist, und wird der Standardwert für den Typ der Eigenschaft unterscheidet. Der Standardwert wird wiederhergestellt, wenn die [ `ClearValue` ](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) Methode wird aufgerufen, für die bindbare Eigenschaft.

Der folgende Code zeigt ein Beispiel für eine bindbare Eigenschaft und einen Bezeichner und Werte für die vier erforderliche Parameter:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Dies erstellt eine [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanz mit dem Namen `EventName`, des Typs `string`. Die Eigenschaft ist im Besitz der `EventToCommandBehavior` Klasse, und hat den Standardwert des `null`. Die Namenskonvention für bindbare Eigenschaften ist, dass der Bezeichner für die bindbare Eigenschaft den Namen der Eigenschaft im angegebenen entsprechen muss die `Create` Methode mit "Property" angefügt. Im obigen Beispiel ist der Bezeichner für die bindbare aus diesem Grund `EventNameProperty`.

Optional beim Erstellen einer [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Instanz, die folgenden Parameter können angegeben werden:

- Der Bindungsmodus. Dies wird verwendet, um die Richtung anzugeben, in der Änderungen von Eigenschaftswerten weitergegeben. In der Standardmodus für die Bindung, werden Änderungen von übertragen die *Quelle* auf die *Ziel*.
- Eine Überprüfung-Delegat, der aufgerufen wird, wenn der Eigenschaftswert festgelegt ist. Weitere Informationen finden Sie unter [Überprüfungsrückrufe](#validation).
- Eine Eigenschaft geändert, Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert wurde. Weitere Informationen finden Sie unter [Erkennen von Eigenschaftenänderungen](#propertychanges).
- Eine Eigenschaft ändern der Delegat, der aufgerufen wird, wenn der Eigenschaftswert ändert. Dieser Delegat hat die gleiche Signatur wie der Delegat für die Eigenschaft geändert wurde.
- Ein Coerce-Wert-Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert wurde. Weitere Informationen finden Sie unter [Coerce-Wert-Rückrufe](#coerce).
- Ein `Func` , mit dem Standardwert der Eigenschaft zu initialisieren. Weitere Informationen finden Sie unter [erstellen einen Default-Wert mit einer Funktion](#defaultfunc).

### <a name="creating-accessors"></a>Erstellen von Accessoren

Eigenschaftenaccessoren sind erforderlich, um die Syntax für Eigenschaften zu verwenden, um eine bindbare Eigenschaft zuzugreifen. Die `Get` Accessor sollte den Wert, der enthalten ist in der entsprechenden bindbare Eigenschaft zurückgeben. Dies kann erreicht werden, durch den Aufruf der [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) Methode, übergebe die ID für die bindbare Eigenschaft auf dem den Wert abgerufen, und klicken Sie dann wandelt das Ergebnis in den erforderlichen Typ. Die `Set` Accessor sollte den Wert der entsprechenden bindbare Eigenschaft festgelegt. Dies kann erreicht werden, durch den Aufruf der [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) Methode und übergebe die ID für die bindbare Eigenschaft auf dem der Wert und den festzulegenden Wert festgelegt.

Im folgenden Codebeispiel wird veranschaulicht, Accessoren für die `EventName` bindbare Eigenschaft:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Nutzen eine bindbare Eigenschaft

Sobald eine bindbare Eigenschaft erstellt wurde, können sie XAML oder Code genutzt werden. In XAML erfolgt dies durch einen Namespace mit einem Präfix deklariert, mit der Namespacedeklaration, der angibt, die CLR-Namespace-Namen und optional einen Assemblynamen. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Im folgenden Codebeispiel wird veranschaulicht, einen XAML-Namespace für einen benutzerdefinierten Typ, der eine bindbare Eigenschaft und enthält die in der gleichen Assembly wie der Anwendungscode definiert ist, die auf den benutzerdefinierten Typ verweisen:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Die Namespacedeklaration wird verwendet, beim Festlegen der `EventName` bindbare Eigenschaft an, wie im folgenden XAML-Codebeispiel veranschaulicht:

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
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

Beim Erstellen einer [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanz ist, es gibt eine Reihe von optionalen Parametern, die festgelegt werden können, um erweiterte bindbare Eigenschaft Szenarien zu ermöglichen. In diesem Abschnitt werden diese Szenarien.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Erkennen von Eigenschaftenänderungen

Ein `static` durch geänderte Eigenschaften ausgelöste Callback-Methode mit der eine bindbare Eigenschaft registriert werden kann, durch Angabe der `propertyChanged` -Parameter für die [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Methode. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindungsfähige Eigenschaft ändert.

Das folgende Codebeispiel zeigt die `EventName` bindbare Eigenschaft registriert die `OnEventNameChanged` Methode als eine durch geänderte Eigenschaften ausgelöste Callback-Methode:

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

In der durch geänderte Eigenschaften ausgelöste Rückrufmethode die [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Parameter verwendet, um anzugeben, welche Instanz des der besitzenden Klasse gemeldet hat, eine Änderung, und die Werte der beiden `object` Parameter darstellen, die alten und neuen Werte der die bindbare Eigenschaft.

<a name="validation" />

### <a name="validation-callbacks"></a>Überprüfungsrückrufe

Ein `static` Validation Callback-Methode mit der eine bindbare Eigenschaft registriert werden kann, durch Angabe der `validateValue` -Parameter für die [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Methode. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindbare Eigenschaft festgelegt ist.

Das folgende Codebeispiel zeigt wie die `Angle` bindbare Eigenschaft registriert die `IsValidValue` -Methode, wie eine Validierungsmethode für den Rückruf:

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

Überprüfungsrückrufe mit einem Wert bereitgestellt werden und sollte zurückgeben `true` Wenn der Wert gültig für die Eigenschaft andernfalls `false`. Eine Ausnahme wird ausgelöst, wenn eine Überprüfung des Rückrufs `false`, die vom Entwickler behandelt werden sollen. Eine typische Verwendung einer Rückrufmethode für die Überprüfung wird die Werte für ganze Zahlen oder Double-Werte einschränken, wenn die bindbare Eigenschaft festgelegt ist. Z. B. die `IsValidValue` Methode überprüft, ob der Eigenschaftswert ist eine `double` innerhalb des Bereichs 0 bis 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Coerce-Wert-Rückrufe

Ein `static` coerce-Wert Callback-Methode mit der eine bindbare Eigenschaft registriert werden kann, durch Angabe der `coerceValue` -Parameter für die [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Methode. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindungsfähige Eigenschaft ändert.

Coerce-Wert, der Rückrufe verwendet werden, um eine erneute Auswertung der eine bindbare Eigenschaft zu erzwingen, wenn der Wert der Eigenschaft geändert. Beispielsweise kann ein Coerce-Wert-Rückruf verwendet werden, um sicherzustellen, dass der Wert eine bindbare Eigenschaft nicht größer als der Wert einer anderen bindbare Eigenschaft ist.

Das folgende Codebeispiel zeigt die `Angle` bindbare Eigenschaft registriert die `CoerceAngle` -Methode, wie eine Rückrufmethode der Coerce-Wert:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

Die `CoerceAngle` Methode überprüft den Wert des der `MaximumAngle` -Eigenschaft, und, wenn die `Angle` -Eigenschaftswert ist größer als, es wandelt den Wert in der `MaximumAngle` -Eigenschaftswert.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Erstellen einen Standardwert mit einer Funktion

Ein `Func` kann zum Initialisieren des Standardwert, der eine bindbare Eigenschaft und verwendet werden, wie im folgenden Codebeispiel gezeigt:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

Die `defaultValueCreator` Parameter auf festgelegt ist eine `Func` aufruft, die die [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) -Methode zur Rückgabe eine `double` , das darstellt, der benannten Größe für die Schriftart, die verwendet wird eine [ `Label` ](xref:Xamarin.Forms.Label) auf der nativen Plattform.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine Einführung in bindbare Eigenschaften und veranschaulicht, wie das Erstellen und nutzen diese. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, in denen der Wert der Eigenschaft wird vom Eigenschaftensystem Xamarin.Forms nachverfolgt.


## <a name="related-links"></a>Verwandte Links

- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Ereignis zum Verhalten des Befehls (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Behaviors/EventToCommandBehavior/)
- [Validation Callback (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce-Wert-Rückruf (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
