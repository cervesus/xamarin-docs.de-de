---
title: Beschleunigungsfunktionen
description: "Xamarin.Forms umfasst eine Beschleunigung-Klasse, die Ihnen die Angabe eine Übertragungsfunktion, die steuert, wie Animationen beschleunigen oder verlangsamen Sie, wie sie ausgeführt werden, kann. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet und wie benutzerdefinierte Beschleunigungsfunktionen erstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: a57fd6e45d744d0e527c811649ce5299ebcd34d5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="easing-functions"></a>Beschleunigungsfunktionen

_Xamarin.Forms umfasst eine Beschleunigung-Klasse, die Ihnen die Angabe eine Übertragungsfunktion, die steuert, wie Animationen beschleunigen oder verlangsamen Sie, wie sie ausgeführt werden, kann. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet und wie benutzerdefinierte Beschleunigungsfunktionen erstellen._


Die [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Klasse definiert eine Reihe von Beschleunigungsfunktionen, die von Animationen genutzt werden können:

- Die [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) Beschleunigungsfunktionen Funktion die Animation am Anfang abprallt.
- Die [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) Beschleunigungsfunktionen Funktion die Animation am Ende abprallt.
- Die [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) Funktion langsam Beschleunigungsfunktionen Animation beschleunigt.
- Die [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) Beschleunigungsfunktionen Funktion beschleunigt die Animation am Anfang und verlangsamt die Animation am Ende.
- Die [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) Funktion schnell Beschleunigungsfunktionen verlangsamt die Animation.
- Die [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) Konstante Geschwindigkeit Beschleunigungsfunktionen Funktion verwendet und ist die Standardeinstellung Beschleunigungsfunktionen Funktion.
- Die [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) Funktion reibungslos Beschleunigungsfunktionen Animation beschleunigt.
- Die [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) Funktion reibungslos Beschleunigungsfunktionen beschleunigt die Animation am Anfang und reibungslos verlangsamt die Animation am Ende.
- Die [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) Funktion reibungslos Beschleunigungsfunktionen verlangsamt die Animation.
- Die [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Beschleunigungsfunktionen Funktion bewirkt, dass die Animation sehr schnell gegen Ende zu beschleunigen.
- Die [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Beschleunigungsfunktionen Funktion bewirkt, dass die Animation schnell gegen Ende/Verlangsamung.

Die `In` und `Out` Suffixe angeben, ob der Effekt der Beschleunigungsfunktion gebotenen bemerkbar am Anfang der Animation, am Ende oder beides ist.

Darüber hinaus können benutzerdefinierte Beschleunigungsfunktionen erstellt werden. Weitere Informationen finden Sie unter [benutzerdefinierte einfachere Funktionen](#customeasing).

## <a name="consuming-an-easing-function"></a>Nutzen eine Beschleunigungsfunktion

Die Animation Erweiterungsmethoden in den [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse ermöglichen eine Beschleunigungsfunktion als der letzte Methodenparameter angegeben werden, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Durch die Angabe einer Beschleunigungsfunktion für eine Animation, auf die Geschwindigkeit der Animation wird nichtlineare und erzeugt die Auswirkung von Beschleunigungsfunktion bereitgestellt. Eine Beschleunigungsfunktion auslassen, für die Erstellung einer Animation führt dazu, dass die Animation den vorgegebenen [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) Beschleunigungsfunktionen-Funktion, die eine lineare Geschwindigkeit erzeugt.

Weitere Informationen zum Verwenden der Animation Erweiterungsmethoden in den [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse, finden Sie unter [einfachen Animationen](~/xamarin-forms/user-interface/animation/simple.md). Beschleunigungsfunktionen kann auch von genutzt werden die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Klasse. Weitere Informationen finden Sie unter [benutzerdefinierten Animationen](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Benutzerdefinierte Beschleunigungsfunktionen

Es gibt drei Hauptverfahren für einen benutzerdefinierten Beschleunigungsfunktion zu erstellen:

1. Erstellen Sie eine Methode, die akzeptiert eine `double` Argument und gibt eine `double` Ergebnis.
1. Erstellen Sie eine `Func<double, double>`.
1. Geben Sie die Beschleunigungsfunktion als Argument für die [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Konstruktor.

In allen drei Fällen sollte die benutzerdefinierte Beschleunigungsfunktion für ein Argument von 0 und 1 für ein Argument 1 0 zurückgeben. Allerdings kann einen beliebigen Wert zwischen die Argumentwerte 0 und 1 zurückgegeben werden. Jeder Ansatz wird jetzt der Reihe nach erläutert werden.

### <a name="custom-easing-method"></a>Benutzerdefinierte Beschleunigungsfunktionen-Methode

Einen benutzerdefinierten Beschleunigungsfunktion kann definiert werden, als eine Methode, die eine `double` Argument und gibt eine `double` zur Folge haben, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

Die `CustomEase` Methode schneidet ab, der eingehende Wert, der die Werte 0, 0,2, 0,4, 0,6, 0,8 und 1. Aus diesem Grund die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz wird in diskrete Sprünge statt reibungslos übersetzt.

### <a name="custom-easing-func"></a>Benutzerdefinierte Beschleunigungsfunktionen Func

Als kann auch einen benutzerdefinierten Beschleunigungsfunktion definiert eine `Func<double, double>`, wie im folgenden Codebeispiel gezeigt:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

Die `CustomEase` `Func` stellt eine Beschleunigungsfunktion, die beginnt, deaktiviert schnelle, verlangsamt und kehrt Kurs und kehrt dann Kurs erneut schnell gegen Ende beschleunigen. Aus diesem Grund dagegen die gesamte Verschiebung der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz wird nach unten, kehrt auch vorübergehend Kurs durchlaufen die Animation.

### <a name="custom-easing-constructor"></a>Benutzerdefinierte Beschleunigungsfunktionen-Konstruktor

Einen benutzerdefinierten Beschleunigungsfunktion kann auch definiert werden, als Argument für die [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Konstruktor, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Benutzerdefinierte Beschleunigungsfunktion wird angegeben, wie ein Lambda-Funktion-Argument, um die [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Konstruktor und verwendet die `Math.Cos` Methode, um eine langsame Drop-Effekt zu erstellen, die von gedämpft wird die `Math.Exp` Methode. Aus diesem Grund die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz ist, damit er angezeigt wird, Löschen zum endgültigen Verbleib wechselseitig übersetzt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet und wie benutzerdefinierte Beschleunigungsfunktionen erstellen. Xamarin.Forms enthält ein [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) -Klasse, die Ihnen die Möglichkeit geben Sie eine Übertragungsfunktion, die steuert, wie Animationen beschleunigen oder verlangsamen Sie, wie sie ausgeführt werden.



## <a name="related-links"></a>Verwandte Links

- [Übersicht über die asynchrone Unterstützung](~/cross-platform/platform/async.md)
- [Beschleunigungsfunktionen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Einfachere](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
