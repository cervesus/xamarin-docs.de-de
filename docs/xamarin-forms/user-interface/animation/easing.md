---
title: Beschleunigungsfunktionen in Xamarin.Forms
description: Xamarin.Forms umfasst eine Beschleunigung-Klasse, mit der Sie eine Übertragungsfunktion angeben, die steuert wie Animationen zu beschleunigen oder verlangsamen, wie sie ausgeführt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen genutzt, und wie Sie benutzerdefinierte Beschleunigungsfunktionen erstellen.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 211f56e0d9f96383670be1d60421d3ac28eabe46
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61345573"
---
# <a name="easing-functions-in-xamarinforms"></a>Beschleunigungsfunktionen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)

_Xamarin.Forms umfasst eine Beschleunigung-Klasse, mit der Sie eine Übertragungsfunktion angeben, die steuert wie Animationen zu beschleunigen oder verlangsamen, wie sie ausgeführt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen genutzt, und wie Sie benutzerdefinierte Beschleunigungsfunktionen erstellen._


Die [ `Easing` ](xref:Xamarin.Forms.Easing) Klasse definiert eine Reihe von Beschleunigungsfunktionen, die von Animationen genutzt werden können:

- Die [ `BounceIn` ](xref:Xamarin.Forms.Easing.BounceIn) Beschleunigungsfunktion springt die Animation am Anfang.
- Die [ `BounceOut` ](xref:Xamarin.Forms.Easing.BounceOut) Beschleunigungsfunktion springt die Animation am Ende.
- Die [ `CubicIn` ](xref:Xamarin.Forms.Easing.CubicIn) Beschleunigungsfunktion langsam beschleunigt die Animation.
- Die [ `CubicInOut` ](xref:Xamarin.Forms.Easing.CubicInOut) Beschleunigungsfunktion beschleunigt die Animation am Anfang und verlangsamt die Animation am Ende.
- Die [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut) Beschleunigungsfunktion schnell die Animation verlangsamt.
- Die [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) Beschleunigungsfunktion eine Konstante Geschwindigkeit verwendet und ist die Standardeinstellung Beschleunigungsfunktion.
- Die [ `SinIn` ](xref:Xamarin.Forms.Easing.SinIn) Beschleunigungsfunktion reibungslos beschleunigt die Animation.
- Die [ `SinInOut` ](xref:Xamarin.Forms.Easing.SinInOut) Beschleunigungsfunktion reibungslos beschleunigt die Animation am Anfang und reibungslos verlangsamt die Animation am Ende.
- Die [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut) Beschleunigungsfunktion reibungslos verlangsamt, wird die Animation.
- Die [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Beschleunigungsfunktion bewirkt, dass die Animation, die am Ende sehr schnell zu beschleunigen.
- Die [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) Beschleunigungsfunktion bewirkt, dass die Animation zum Ende schnell verlangsamen.

Die `In` und `Out` Suffixe zu angibt, ob die Auswirkungen, die der Beschleunigungsfunktion gebotenen merkliche am Anfang der Animation, auf das Ende oder beides handelt.

Darüber hinaus können benutzerdefinierte Beschleunigungsfunktionen erstellt werden. Weitere Informationen finden Sie unter [benutzerdefinierten Beschleunigungsfunktion Funktionen](#customeasing).

## <a name="consuming-an-easing-function"></a>Nutzen eine Beschleunigungsfunktion

Die Animation-Erweiterungsmethoden in den [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse kann eine Beschleunigungsfunktion als letzte Methodenparameter angegeben werden, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Durch Angabe einer Beschleunigungsfunktion für eine Animation, die Geschwindigkeit der Animation wird nicht linearen und erzeugt die Auswirkungen, die von der Beschleunigungsfunktion bereitgestellt. Eine Beschleunigungsfunktion weglassen, wenn Sie eine Animation erstellen bewirkt, dass die Animation den vorgegebenen [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) Beschleunigungsfunktion, die eine lineare und Winkelgeschwindigkeit erzeugt.

Weitere Informationen zur Verwendung der Animation-Erweiterungsmethoden in den [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse, finden Sie unter [einfache Animationen](~/xamarin-forms/user-interface/animation/simple.md). Beschleunigungsfunktionen kann auch von genutzt werden die [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse. Weitere Informationen finden Sie unter [benutzerdefinierte Animationen](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Benutzerdefinierte Easingfunctions

Es gibt drei Hauptmethoden der Erstellung einer benutzerdefinierten Beschleunigungsfunktion:

1. Erstellen Sie eine Methode, die akzeptiert eine `double` Argument und gibt eine `double` Ergebnis.
1. Erstellen Sie eine `Func<double, double>`.
1. Geben Sie die Beschleunigungsfunktion als Argument für die [ `Easing` ](xref:Xamarin.Forms.Easing) Konstruktor.

In allen drei Fällen sollte die benutzerdefinierte Beschleunigungsfunktion 0 für ein Argument von 0 und 1 für ein Argument 1 zurückgeben. Allerdings kann einen beliebigen Wert zwischen den Argumentwerten von 0 und 1 zurückgegeben werden. Jeder Ansatz werden jetzt im Gegenzug erläutert.

### <a name="custom-easing-method"></a>Benutzerdefinierte einfachere Methode

Eine benutzerdefinierte Beschleunigungsfunktion kann definiert werden, als einer Methode, eine `double` Argument und gibt eine `double` führen, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

Die `CustomEase` Methode schneidet den eingehenden Wert mit den Werten 0, 0,2, 0,4, 0,6, 0,8 und 1. Aus diesem Grund die [ `Image` ](xref:Xamarin.Forms.Image) Instanz wird in einzelne Sprünge statt reibungslos übersetzt.

### <a name="custom-easing-func"></a>Benutzerdefinierte Beschleunigungsfunktion Func

Eine benutzerdefinierte Beschleunigungsfunktion kann auch definiert werden, als eine `Func<double, double>`, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

Die `CustomEase` `Func` stellt eine Beschleunigungsfunktion, die Anfangs schnelle, verlangsamt und kehrt Kurs aus und kehrt dann erneut Kurs, um schnell zum Ende zu beschleunigen. Aus diesem Grund zwar die gesamte Verschiebung von der [ `Image` ](xref:Xamarin.Forms.Image) Instanz wird nach unten, und es auch vorübergehend umgekehrt, Kurs, die nach der Hälfte der Animation.

### <a name="custom-easing-constructor"></a>Benutzerdefinierte Beschleunigungsfunktion-Konstruktor

Eine benutzerdefinierte Beschleunigungsfunktion kann auch definiert werden, als Argument für die [ `Easing` ](xref:Xamarin.Forms.Easing) -Konstruktor, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Benutzerdefinierte Beschleunigungsfunktion angegeben ist, als Argument ein Lambda-Funktion der [ `Easing` ](xref:Xamarin.Forms.Easing) Konstruktor, und verwendet die `Math.Cos` Methode, um eine langsame &-Drop-Effekt zu erstellen, die von gedämpft wird die `Math.Exp` Methode. Aus diesem Grund die [ `Image` ](xref:Xamarin.Forms.Image) Instanz ist übersetzt, sodass er angezeigt wird, um den endgültigen Verbleib wechselseitig gelöscht.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, wie die vordefinierten Beschleunigungsfunktionen genutzt, und wie Sie benutzerdefinierte Beschleunigungsfunktionen erstellen. Xamarin.Forms umfasst einen [ `Easing` ](xref:Xamarin.Forms.Easing) -Klasse, die Ihnen die Möglichkeit zum Angeben einer Übertragungsfunktion, die steuert, wie Animationen beschleunigen oder verlangsamen, wie sie ausgeführt werden.



## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Beschleunigungsfunktionen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Einfachere](xref:Xamarin.Forms.Easing)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
