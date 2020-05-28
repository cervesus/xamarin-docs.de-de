---
Title: "Beschleunigungsfunktionen in Xamarin.Forms " Description: " Xamarin.Forms enthält eine Beschleunigungs Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden.
ms. Prod: ms. assetid: ms. Technology: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="easing-functions-in-xamarinforms"></a>Beschleunigungsfunktionen inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin. Forms enthält eine Beschleunigungs Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden._

Die- [`Easing`](xref:Xamarin.Forms.Easing) Klasse definiert eine Reihe von Beschleunigungsfunktionen, die von Animationen genutzt werden können:

- Die Beschleunigungs [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) Funktion springt die Animation am Anfang.
- Die Beschleunigungs [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut) Funktion springt die Animation am Ende.
- Die Beschleunigungs [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn) Funktion beschleunigt die Animation langsam.
- Die Beschleunigungs [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut) Funktion beschleunigt die Animation am Anfang und verlangsamt die Animation am Ende.
- Die Beschleunigungs [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) Funktion verlangsamt die Animation schnell.
- Die Beschleunigungs [`Linear`](xref:Xamarin.Forms.Easing.Linear) Funktion verwendet eine konstante Geschwindigkeit und ist die standardmäßige Beschleunigungs Funktion.
- Die Beschleunigungs [`SinIn`](xref:Xamarin.Forms.Easing.SinIn) Funktion beschleunigt die Animation reibungslos.
- Die Beschleunigungs [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut) Funktion beschleunigt die Animation am Anfang und verlangsamt die Animation am Ende reibungslos.
- Die Beschleunigungs [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) Funktion verlangsamt die Animation nahtlos.
- Die [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) Beschleunigungs Funktion bewirkt, dass die Animation sehr schnell am Ende beschleunigt wird.
- Die Beschleunigungs [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) Funktion bewirkt, dass die Animation schnell zum Ende verlangsamt wird.

Die `In` `Out` Suffixe und geben an, ob die von der Beschleunigungs Funktion bereitgestellte Auswirkung am Anfang der Animation, am Ende oder an beiden fest erkennbar ist.

Außerdem können benutzerdefinierte Beschleunigungsfunktionen erstellt werden. Weitere Informationen finden Sie unter [benutzerdefinierte Beschleunigungsfunktionen](#customeasing).

## <a name="consuming-an-easing-function"></a>Nutzen einer Beschleunigungs Funktion

Die Animations Erweiterungs Methoden in der- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse ermöglichen, dass eine Beschleunigungs Funktion als abschließender Methoden Parameter angegeben wird, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Durch Angeben einer Beschleunigungs Funktion für eine Animation wird die Animationsgeschwindigkeit nicht linear und erzeugt die von der Beschleunigungs Funktion bereitgestellten Auswirkungen. Wenn beim Erstellen einer Animation eine Beschleunigungs Funktion weggelassen wird, verwendet die Animation die Standard Beschleunigungs [`Linear`](xref:Xamarin.Forms.Easing.Linear) Funktion, die eine lineare Geschwindigkeit erzeugt.

Weitere Informationen zum Verwenden der Animations Erweiterungs Methoden in der- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse finden Sie unter [einfache Animationen](~/xamarin-forms/user-interface/animation/simple.md). Beschleunigungsfunktionen können auch von der-Klasse genutzt werden [`Animation`](xref:Xamarin.Forms.Animation) . Weitere Informationen finden Sie unter [benutzerdefinierte Animationen](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Benutzerdefinierte Beschleunigungsfunktionen

Es gibt drei Hauptansätze für das Erstellen einer benutzerdefinierten Beschleunigungs Funktion:

1. Erstellen Sie eine Methode, die ein `double` -Argument annimmt und ein Ergebnis zurückgibt `double` .
1. Erstellen Sie eine `Func<double, double>`.
1. Geben Sie die Beschleunigungs Funktion als Argument für den [`Easing`](xref:Xamarin.Forms.Easing) Konstruktor an.

In allen drei Fällen sollte die benutzerdefinierte Beschleunigungs Funktion 0 für ein Argument von 0 und 1 für ein Argument von 1 zurückgeben. Allerdings kann jeder Wert zwischen den Argument Werten 0 und 1 zurückgegeben werden. Jeder Ansatz wird nun im Gegenzug behandelt.

### <a name="custom-easing-method"></a>Benutzerdefinierte Beschleunigungs Methode

Eine benutzerdefinierte Beschleunigungs Funktion kann als eine Methode definiert werden, die ein `double` -Argument annimmt und ein Ergebnis zurückgibt `double` , wie im folgenden Codebeispiel gezeigt:

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

Mit der- `CustomEase` Methode wird der eingehende Wert auf die Werte 0, 0,2, 0,4, 0,6, 0,8 und 1 abgeschnitten. Daher wird die [`Image`](xref:Xamarin.Forms.Image) Instanz in diskrete Sprünge und nicht reibungslos übersetzt.

### <a name="custom-easing-func"></a>Benutzerdefinierte Beschleunigungs Funktion

Eine benutzerdefinierte Beschleunigungs Funktion kann auch als definiert werden `Func<double, double>` , wie im folgenden Codebeispiel gezeigt:

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

`CustomEaseFunc`Stellt eine Beschleunigungs Funktion dar, die schnell startet, den Kurs verlangsamt und wiederherstellt und dann den Kurs wieder aufkehrt, um schnell zum Ende zu beschleunigen. Wenn die Gesamtverschiebung der [`Image`](xref:Xamarin.Forms.Image) Instanz abwärts ist, kehrt sie daher auch in der Mitte der Animation vorübergehend den Kurs um.

### <a name="custom-easing-constructor"></a>Benutzerdefinierter Beschleunigungs Konstruktor

Eine benutzerdefinierte Beschleunigungs Funktion kann auch als Argument für den [`Easing`](xref:Xamarin.Forms.Easing) Konstruktor definiert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Die benutzerdefinierte Beschleunigungs Funktion wird als Lambda-Funktions Argument für den [`Easing`](xref:Xamarin.Forms.Easing) Konstruktor angegeben und verwendet die- `Math.Cos` Methode, um einen langsamen Ablage Effekt zu erstellen, der von der-Methode gedämpft wird `Math.Exp` . Daher wird die- [`Image`](xref:Xamarin.Forms.Image) Instanz so übersetzt, dass Sie auf Ihre endgültige Ruhe Stelle fällt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie die vordefinierten Beschleunigungsfunktionen genutzt werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden. Xamarin.Formsenthält eine- [`Easing`](xref:Xamarin.Forms.Easing) Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden.

## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Beschleunigungsfunktionen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [Entlasten](xref:Xamarin.Forms.Easing)
- [Viewextensions](xref:Xamarin.Forms.ViewExtensions)
