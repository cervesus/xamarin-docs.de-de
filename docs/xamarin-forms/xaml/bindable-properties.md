---
title: Bindbare Eigenschaften
description: In Xamarin.Forms wird die Funktionalität der common Language Runtime (CLR)-Eigenschaften von bindbare Eigenschaften verlängert. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, auf dem Wert der Eigenschaft wird von dem Eigenschaftensystem Xamarin.Forms nachverfolgt. Dieser Artikel bietet eine Einführung in die bindungsfähigen Eigenschaften und veranschaulicht das Erstellen und nutzen müssen.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7e1d3c82036ef703014ae548a6719937e89d22f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="bindable-properties"></a>Bindbare Eigenschaften

_In Xamarin.Forms wird die Funktionalität der common Language Runtime (CLR)-Eigenschaften von bindbare Eigenschaften verlängert. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, auf dem Wert der Eigenschaft wird von dem Eigenschaftensystem Xamarin.Forms nachverfolgt. Dieser Artikel bietet eine Einführung in die bindungsfähigen Eigenschaften und veranschaulicht das Erstellen und nutzen müssen._

## <a name="overview"></a>Übersicht

Bindbare Eigenschaften CLR-Eigenschaftenfunktionalität erweitern, indem eine Eigenschaft mit dem Sichern einer [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Typ, statt eine Eigenschaft mit einem Feld zu sichern. Dient der bindbare Eigenschaften zu einem Eigenschaftensystem bereitgestellt wird, die Datenbindung, Stile, Vorlagen unterstützt, und Werte, die über Parent-Child-Beziehungen festgelegt. Darüber hinaus bieten bindbare Eigenschaften Standardwerte, Überprüfung der Eigenschaftswerte und Rückrufe, die eigenschaftsänderungen zu überwachen.

Eigenschaften sollten als bindbare Eigenschaften zur Unterstützung von mindestens einer der folgenden Funktionen implementiert werden:

- Fungiert als gültige *Ziel* Eigenschaft für die Datenbindung.
- Festlegen der Eigenschaft über eine [Stil](~/xamarin-forms/user-interface/styles/index.md).
- Bereitstellen von einem Standardeigenschaftswert, die von der Standardwert für den Typ der Eigenschaft unterscheidet.
- Überprüfen den Wert der Eigenschaft.
- Überwachen von eigenschaftenänderungen.

Xamarin.Forms bindbare Eigenschaften zählen [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), und [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/). Jede bindbare Eigenschaft verfügt über ein entsprechendes `public static readonly` Eigenschaft vom Typ [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) , die in der gleichen Klasse verfügbar gemacht wird und das ist der Bezeichner der bindbare Eigenschaft. Z. B. der entsprechenden bindbare Eigenschaftenbezeichner für die `Label.Text` Eigenschaft [ `Label.TextProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Erstellen und nutzen eine bindbare Eigenschaft

Der Erstellungsprozess für eine bindbare Eigenschaft lautet wie folgt:

1. Erstellen einer [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanz mit einem der [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) methodenüberladungen.
1. Definieren von Eigenschaftenaccessoren für die [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanz.

Beachten Sie, dass alle [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanzen müssen im UI-Thread erstellt werden. Dies bedeutet, dass nur Code, der auf den UI-Thread ausgeführt wird abrufen kann, oder legen Sie den Wert, der eine bindbare Eigenschaft. Allerdings `BindableProperty` Instanzen von anderen Threads zugegriffen werden können, durch Marshalling an den UI-Thread mit der [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) Methode.

### <a name="creating-a-property"></a>Erstellen einer Eigenschaft

Zum Erstellen einer `BindableProperty` -Instanz, die enthaltende Klasse ableiten muss die [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Klasse. Allerdings die `BindableObject` Klasse ist in der Klassenhierarchie hoch, damit die meisten Klassen für Funktionen Unterstützung bindbare Benutzeroberflächeneigenschaften verwendet.

Eine bindbare Eigenschaft erstellt werden, indem Sie deklarieren eine `public static readonly` Eigenschaft vom Typ [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Die bindbare Eigenschaft sollte festgelegt werden, auf den zurückgegebenen Wert eines der [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) methodenüberladungen. Die Deklaration muss innerhalb eines Texts der [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) abgeleitete Klasse, aber außerhalb von Memberdefinitionen.

Mindestens ein Bezeichners muss angegeben werden beim Erstellen einer [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), zusammen mit den folgenden Parametern:

- Der Name des der [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/).
- Den Typ der Eigenschaft.
- Der Typ des besitzenden Objekts.
- Der Standardwert für die Eigenschaft. Dadurch wird sichergestellt, dass die Eigenschaft gibt immer einen bestimmten Standardwert zurück, wenn es nicht festgelegt ist, und es sich von der Standardwert für den Typ der Eigenschaft kann. Der Standardwert wird wiederhergestellt, wenn die [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) bindbare Eigenschaft Methode aufgerufen wird.

Der folgende Code zeigt ein Beispiel für eine bindbare Eigenschaft und einen Bezeichner und Werte für die vier erforderlichen Parameter an:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Dies erstellt eine [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanz mit dem Namen `EventName`, vom Typ `string`. Die Eigenschaft ist im Besitz der `EventToCommandBehavior` Klasse, und hat den Standardwert `null`. Die Namenskonvention für bindbare Eigenschaften ist, dass der Bezeichner bindbare Eigenschaft im angegebene Eigenschaftsname der entsprechen muss die `Create` Methode mit "Property" angefügt ist. Im obigen Beispiel daher die bindungsfähigen Eigenschafts-ID ist `EventNameProperty`.

Optional beim Erstellen einer [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanz, die folgenden Parameter können angegeben werden:

- Bindungsmodus. Dies wird verwendet, um die Richtung anzugeben, in der Eigenschaftswert ändert weitergegeben werden. Änderungen werden in der Bindung der Standardmodus von verteilt die *Quelle* auf die *Ziel*.
- Eine Validierung-Delegat, der aufgerufen wird, wenn der Eigenschaftswert festgelegt ist. Weitere Informationen finden Sie unter [Überprüfung Rückrufe](#validation).
- Eine Eigenschaft geändert, Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert hat. Weitere Informationen finden Sie unter [Erkennen von Eigenschaftenänderungen](#propertychanges).
- Eine Eigenschaft ändern der Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert wird. Dieser Delegat hat die gleiche Signatur wie der Delegat für die Eigenschaft geändert wurde.
- Ein zum Umwandeln-Wert-Delegat, der aufgerufen wird, wenn der Eigenschaftswert geändert hat. Weitere Informationen finden Sie unter [Coerce-Rückrufen](#coerce).
- Ein `Func` wird verwendet, um einen Standardwert für die Eigenschaft zu initialisieren. Weitere Informationen finden Sie unter [Erstellen eines Standardwerts mit Func](#defaultfunc).

### <a name="creating-accessors"></a>Erstellen von Accessoren

Eigenschaftenaccessoren sind erforderlich, um die Eigenschaftensyntax verwenden, um eine bindbare Eigenschaft zuzugreifen. Die `Get` Accessor sollte der Wert in der entsprechenden bindbare Eigenschaft zurückgeben. Dies kann durch Aufrufen der [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) Methode, bindbare Eigenschaft im Bezeichner für das Abrufen des Werts übergeben, und klicken Sie dann das Ergebnis in den erforderlichen Typ umwandeln. Die `Set` Accessor sollte den Wert der entsprechenden bindbare Eigenschaft festgelegt. Dies kann durch Aufrufen der [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) -Methode auf und übergibt die bindbare Eigenschaftenbezeichner für das der Wert und den festzulegenden Wert festgelegt.

Das folgende Codebeispiel zeigt die Zugriffsmethoden für den `EventName` bindbare Eigenschaft:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Nutzen eine bindbare Eigenschaft

Sobald eine bindbare Eigenschaft erstellt wurde, können sie aus XAML oder Code genutzt werden. In XAML wird dies erreicht, durch einen Namespace mit einem Präfix, mit der Namespacedeklaration, der angibt, die CLR-Namespace-Namen und optional ein Assemblyname deklarieren. Weitere Informationen finden Sie unter [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md).

Im folgenden Codebeispiel wird veranschaulicht, einen XAML-Namespace für einen benutzerdefinierten Typ, der eine bindbare Eigenschaft enthält, der innerhalb derselben Assembly wie den Code der Anwendung definiert ist, die den benutzerdefinierten Typ verweist:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Die Namespacedeklaration verwendet, der zum Einstellen der `EventName` bindbare Eigenschaft, wie im folgenden Beispiel der Verwendung von XAML-Code:

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

Beim Erstellen einer [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanz ist, stehen eine Reihe von optionalen Parametern, die festgelegt werden können, um erweiterte bindbare Eigenschaft Szenarien zu ermöglichen. In diesem Abschnitt wird erklärt, diese Szenarien.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Erkennen von Eigenschaftenänderungen

Ein `static` durch geänderte Eigenschaften ausgelöste Rückrufmethode beim eine bindbare Eigenschaft registriert werden kann, durch Angabe der `propertyChanged` -Parameter für die [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) Methode. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindbare Eigenschaft ändert.

Im folgenden Codebeispiel wird veranschaulicht wie die `EventName` bindbare Eigenschaft registriert die `OnEventNameChanged` Methode als eine geänderte Eigenschaften ausgelöste Rückrufmethode:

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

Der geänderte Eigenschaften ausgelöste Rückrufmethode die [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Parameter wird verwendet, um anzugeben, welche Instanz die besitzende Klasse gemeldet hat, eine Änderung, und die Werte der beiden `object` Parameter darstellen, die alten und neuen Werte der die bindbare Eigenschaft.

<a name="validation" />

### <a name="validation-callbacks"></a>Überprüfungsrückrufe

Ein `static` Rückrufmethode Überprüfung kann beim eine bindbare Eigenschaft registriert werden, durch Angabe der `validateValue` -Parameter für die [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) Methode. Wenn der Wert der bindbare Eigenschaft festgelegt ist, wird die angegebene Rückrufmethode aufgerufen werden.

Im folgenden Codebeispiel wird veranschaulicht wie die `Angle` bindbare Eigenschaft registriert die `IsValidValue` Methode als Validierungsmethode Rückruf:

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

Überprüfung Rückrufe werden mit einem Wert bereitgestellt und sollten zurückgeben `true` ist der Wert für die Eigenschaft ungültig andernfalls `false`. Eine Ausnahme wird ausgelöst, wenn ein Validierungsrückruf gibt `false`, die vom Entwickler behandelt werden sollen. Eine typische Verwendung einer Rückrufmethode für die Validierung ist die Werte für ganze Zahlen oder Double-Werte einschränken, wenn die bindbare Eigenschaft festgelegt ist. Z. B. die `IsValidValue` Methode überprüft, ob der Eigenschaftswert ist eine `double` innerhalb des Bereichs 0 bis 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Coerce-Rückrufen

Ein `static` Wert umzuwandeln Rückrufmethode mit eine bindbare Eigenschaft registriert werden kann, durch Angeben der `coerceValue` -Parameter für die [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) Methode. Die angegebene Rückrufmethode wird aufgerufen werden, wenn der Wert der bindbare Eigenschaft ändert.

Coerce-Wert, den Rückrufen verwendet werden, um eine erneute Auswertung für eine bindbare Eigenschaft zu erzwingen, wenn der Wert der Eigenschaft ändert. Beispielsweise kann ein Rückruf zum Wert verwendet werden, um sicherzustellen, dass der Wert eine bindbare Eigenschaft nicht größer als der Wert einer anderen bindbare Eigenschaft ist.

Im folgenden Codebeispiel wird veranschaulicht wie die `Angle` bindbare Eigenschaft registriert die `CoerceAngle` Methode als Wert Rückrufmethode zum umwandeln:

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

Die `CoerceAngle` Methode überprüft den Wert der die `MaximumAngle` -Eigenschaft, und wenn die `Angle` Eigenschaftswert ist größer als, es wandelt den Wert in der `MaximumAngle` Eigenschaftswert.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Erstellen einen Standardwert mit Func

Ein `Func` können initialisiert werden, den Standardwert eine bindbare Eigenschaft verwendet werden, wie im folgenden Codebeispiel gezeigt:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

Der `defaultValueCreator` Parameter auf festgelegt ist eine `Func` , aufruft der [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) Methode, um zurückzugeben eine `double` , der benannten Größe für die Schriftart, die auf darstellt eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) auf die systemeigene Plattform.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eine Einführung in die bindungsfähigen Eigenschaften bereitgestellt, und veranschaulicht, wie zum Erstellen und nutzen müssen. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, auf dem Wert der Eigenschaft wird von dem Eigenschaftensystem Xamarin.Forms nachverfolgt.


## <a name="related-links"></a>Verwandte Links

- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Ereignis zum Befehlsverhalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Validierungsrückruf (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce-Wert, Rückruf (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
