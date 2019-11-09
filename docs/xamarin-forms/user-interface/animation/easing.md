---
title: Beschleunigungsfunktionen in xamarin. Forms
description: Xamarin. Forms enthält eine Beschleunigungs Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 56ea31d1e1be8bbad4a27dd7ffd844aa03f75bbb
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842832"
---
# <a name="easing-functions-in-xamarinforms"></a>Beschleunigungsfunktionen in xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)

_Xamarin. Forms enthält eine Beschleunigungs Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden._

Die [`Easing`](xref:Xamarin.Forms.Easing) -Klasse definiert eine Reihe von Beschleunigungsfunktionen, die von Animationen genutzt werden können:

- Die [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) Beschleunigungs Funktion springt die Animation am Anfang.
- Die [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut) Beschleunigungs Funktion springt die Animation am Ende.
- Die [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn) Beschleunigungs Funktion beschleunigt die Animation langsam.
- Die [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut) Beschleunigungs Funktion beschleunigt die Animation am Anfang und verlangsamt die Animation am Ende.
- Die [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) Beschleunigungs Funktion verlangsamt die Animation schnell.
- Die [`Linear`](xref:Xamarin.Forms.Easing.Linear) Beschleunigungs Funktion verwendet eine konstante Geschwindigkeit und ist die standardmäßige Beschleunigungs Funktion.
- Die [`SinIn`](xref:Xamarin.Forms.Easing.SinIn) Beschleunigungs Funktion beschleunigt die Animation reibungslos.
- Mit der [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut) Beschleunigungs Funktion wird die Animation am Anfang reibungslos beschleunigt, und die Animation wird nahtlos verlangsamt.
- Die [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) Beschleunigungs Funktion verlangsamt die Animation nahtlos.
- Die [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) Beschleunigungs Funktion bewirkt, dass die Animation sehr schnell am Ende beschleunigt wird.
- Die [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) Beschleunigungs Funktion bewirkt, dass die Animation schnell zum Ende verlangsamt.

Die `In`-und `Out` Suffixe geben an, ob die von der Beschleunigungs Funktion bereitgestellte Auswirkung am Anfang der Animation, am Ende oder an beiden fest erkennbar ist.

Außerdem können benutzerdefinierte Beschleunigungsfunktionen erstellt werden. Weitere Informationen finden Sie unter [benutzerdefinierte Beschleunigungsfunktionen](#customeasing).

## <a name="consuming-an-easing-function"></a>Nutzen einer Beschleunigungs Funktion

Die Animations Erweiterungs Methoden in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse ermöglichen, dass eine Beschleunigungs Funktion als abschließender Methoden Parameter angegeben wird, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Durch Angeben einer Beschleunigungs Funktion für eine Animation wird die Animationsgeschwindigkeit nicht linear und erzeugt die von der Beschleunigungs Funktion bereitgestellten Auswirkungen. Wenn beim Erstellen einer Animation eine Beschleunigungs Funktion weggelassen wird, verwendet die Animation die standardmäßige [`Linear`](xref:Xamarin.Forms.Easing.Linear) Beschleunigungs Funktion, die eine lineare Geschwindigkeit erzeugt.

Weitere Informationen zum Verwenden der Animations Erweiterungs Methoden in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse finden Sie unter [einfache Animationen](~/xamarin-forms/user-interface/animation/simple.md). Beschleunigungsfunktionen können auch von der [`Animation`](xref:Xamarin.Forms.Animation) -Klasse genutzt werden. Weitere Informationen finden Sie unter [benutzerdefinierte Animationen](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Benutzerdefinierte Beschleunigungsfunktionen

Es gibt drei Hauptansätze für das Erstellen einer benutzerdefinierten Beschleunigungs Funktion:

1. Erstellen Sie eine Methode, die ein `double` Argument annimmt und ein `double` Ergebnis zurückgibt.
1. Erstellen Sie eine `Func<double, double>`.
1. Geben Sie die Beschleunigungs Funktion als Argument für den [`Easing`](xref:Xamarin.Forms.Easing) -Konstruktor an.

In allen drei Fällen sollte die benutzerdefinierte Beschleunigungs Funktion 0 für ein Argument von 0 und 1 für ein Argument von 1 zurückgeben. Allerdings kann jeder Wert zwischen den Argument Werten 0 und 1 zurückgegeben werden. Jeder Ansatz wird nun im Gegenzug behandelt.

### <a name="custom-easing-method"></a>Benutzerdefinierte Beschleunigungs Methode

Eine benutzerdefinierte Beschleunigungs Funktion kann als eine Methode definiert werden, die ein `double` Argument annimmt und ein `double` Ergebnis zurückgibt, wie im folgenden Codebeispiel gezeigt:

```csharp
double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}

await image.TranslateTo(0, 200, 2000, (Easing)CustomEase);
```

Mit der `CustomEase`-Methode wird der eingehende Wert auf die Werte 0, 0,2, 0,4, 0,6, 0,8 und 1 abgeschnitten. Aus diesem Grund wird die [`Image`](xref:Xamarin.Forms.Image) Instanz in diskrete Sprünge und nicht reibungslos übersetzt.

### <a name="custom-easing-func"></a>Benutzerdefinierte Beschleunigungs Funktion

Eine benutzerdefinierte Beschleunigungs Funktion kann auch als `Func<double, double>`definiert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
Func<double, double> CustomEaseFunc = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEaseFunc);
```

Der `CustomEaseFunc` stellt eine Beschleunigungs Funktion dar, die schnell startet, den Kurs verlangsamt und wiederherstellt und dann den Kurs wieder aufkehrt, um schnell zum Ende zu beschleunigen. Wenn die Gesamtbewegung der [`Image`](xref:Xamarin.Forms.Image) Instanz abwärts ist, kehrt sie daher auch in der Mitte der Animation vorübergehend den Kurs um.

### <a name="custom-easing-constructor"></a>Benutzerdefinierter Beschleunigungs Konstruktor

Eine benutzerdefinierte Beschleunigungs Funktion kann auch als Argument für den [`Easing`](xref:Xamarin.Forms.Easing) -Konstruktor definiert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Die benutzerdefinierte Beschleunigungs Funktion wird als Lambda-Funktions Argument für den [`Easing`](xref:Xamarin.Forms.Easing) -Konstruktor angegeben und verwendet die `Math.Cos`-Methode, um einen langsamen Ablage Effekt zu erstellen, der von der `Math.Exp`-Methode gedämpft wird. Daher wird die [`Image`](xref:Xamarin.Forms.Image) Instanz so übersetzt, dass Sie auf Ihre endgültige Ruhe Stelle fällt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie die vordefinierten Beschleunigungsfunktionen genutzt werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden. Xamarin. Forms enthält eine [`Easing`](xref:Xamarin.Forms.Easing) -Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden.

## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Beschleunigungsfunktionen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-easing)
- [Entlasten](xref:Xamarin.Forms.Easing)
- [Viewextensions](xref:Xamarin.Forms.ViewExtensions)
