---
title: Zusammenfassung der Kapitel 8. Code und XAML in Harmonie
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 8. Code und XAML in Harmonie'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 75f153499edb6d979f9a0269a1439eaf8ca53878
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334247"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Zusammenfassung der Kapitel 8. Code und XAML in Harmonie

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

In diesem Kapitel untersucht XAML besser und insbesondere wie Code und XAML zu interagieren.

## <a name="passing-arguments"></a>Übergeben von Argumenten

Im Allgemeinen muss eine Klasse, die in XAML instanziiert einen öffentlichen parameterlosen Konstruktor verfügen; Das resultierende Objekt wird durch Einstellungen der Eigenschaften initialisiert. Es gibt jedoch zwei andere Möglichkeiten, wie Objekte instanziiert und initialisiert werden können.

Obwohl diese allgemeine Techniken sind, werden sie vor allem im Zusammenhang mit MVVM-Ansichtsmodelle verwendet.

### <a name="constructors-with-arguments"></a>Konstruktoren mit Argumenten

Die [ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) Beispiel veranschaulicht, wie die `x:Arguments` Tag Konstruktorargumente an. Diese Argumente müssen vom Element-Tags, der angibt, der des Typs des Arguments begrenzt werden. Für die grundlegende .NET Datentypen stehen die folgenden Tags:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>Kann ich die Methoden aus XAML aufrufen?

Die [ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) Beispiel veranschaulicht, wie die `x:FactoryMethod` Element an eine Factorymethode, die aufgerufen wird, um ein Objekt zu erstellen. Solche eine Factorymethode muss öffentlich und statisch sein, und es muss erstellen Sie ein Objekt des Typs in der sie definiert ist. (Z. B. die [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) Methode qualifiziert werden, da es öffentlich und statisch und einen Wert vom Typ gibt `Color`.) Die Argumente an die Factorymethode werden im angegeben `x:Arguments` Tags.

## <a name="the-xname-attribute"></a>X: Name-Attribut

Die `x:Name` Attribut ermöglicht, ein Objekt instanziiert, die in XAML ein Name angegeben werden. Die Regeln für diese Namen sind identisch mit dem Namen von C#-Variablen. Befolgen die Rückgabe der `InitializeComponent` im Konstruktor aufrufen, die Code-Behind-Datei kann auf diese Namen auf das entsprechende XAML-Element verweisen. Tatsächlich werden die Namen der XAML-Parser in privaten Feldern in der generierten partiellen Klasse konvertiert.

Die [ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) Beispiel veranschaulicht die Verwendung von `x:Name` können die Code-Behind-Datei zu zwei `Label` Elemente definiert, die in XAML, die mit dem aktuellen Datum und Uhrzeit aktualisiert.

Der gleiche Namen kann nicht für mehrere Elemente auf derselben Seite verwendet werden. Dies ist ein bestimmtes Problem, bei Verwendung von `OnPlatform` parallel mit dem Namen Objekte für jede Plattform. Die [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) Beispiel wird eine bessere Möglichkeit, etwas zu tun, veranschaulicht.

## <a name="custom-xaml-based-views"></a>Benutzerdefinierte XAML-basierten Ansichten

Es gibt mehrere Möglichkeiten zur Vermeidung von Wiederholungen des Markups in XAML. Ein gängiges Verfahren ist die Erstellung eine neue XAML-basierten Klasse, die von abgeleitet [ `ContentView` ](xref:Xamarin.Forms.ContentView). Dieses Verfahren wird veranschaulicht, der [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) Beispiel. Die `ColorView` Klasse leitet sich von `ContentView` zwar zum Anzeigen einer bestimmten Farbe und den Namen der `ColorViewListPage` Klasse leitet sich von `ContentPage` wie gewohnt und explizit erstellt 17 Instanzen von `ColorView`.

Zugreifen auf die `ColorView` Klasse in XAML erfordert eine andere XML-Namespacedeklaration, häufig mit dem Namen `local` für Klassen in derselben Assembly.

## <a name="events-and-handlers"></a>Ereignisse und Handler

Ereignisse können von Ereignishandlern in XAML zugewiesen werden, aber der eigentliche Ereignishandler implementiert werden muss, in der CodeBehind-Datei. Die [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) wird veranschaulicht, wie Sie eine Zehnertastatur-Benutzeroberfläche in XAML zu erstellen und zum Implementieren der `Clicked` Handler in der CodeBehind-Datei.

## <a name="tap-gestures"></a>Tippen Sie auf Gesten

Alle `View` Objekt abrufen toucheingaben und Generieren von Ereignissen von der betreffenden Eingabe. Die `View` -Klasse definiert eine [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) Auflistungseigenschaft, die eine oder mehrere Instanzen von Klassen enthalten kann, die von abgeleitet [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer).

Die [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) generiert [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) Ereignisse. Die [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) Programm veranschaulicht das Anfügen `TapGestureRecognizer` Objekte auf vier `BoxView` Elementen, die eine Nachahmung spieleindruck zu erzielen:

[![Dreifacher Screenshot des Monkey Tap](images/ch08fg07-small.png "Nachahmung Spiel")](images/ch08fg07-large.png#lightbox "Nachahmung-Spiel")

Aber die **MonkeyTap** Programm wirklich Sound benötigt. (Finden Sie unter [im nächsten Kapitel](chapter09.md).)

## <a name="related-links"></a>Verwandte Links

- [Kapitel 8 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Kapitel 8-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Kapitel 8 F# Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
