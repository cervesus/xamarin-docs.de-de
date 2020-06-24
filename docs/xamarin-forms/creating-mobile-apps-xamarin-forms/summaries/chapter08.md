---
title: Zusammenfassung von Kapitel 8. Verknüpfung von Code und XAML
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 8. Verknüpfung von Code und XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 90db8b4f11095a2a56c82c3f563844efbcf7e2b1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136824"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Zusammenfassung von Kapitel 8. Verknüpfung von Code und XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

In diesem Kapitel wird XAML ausführlicher behandelt, insbesondere wie Code und XAML miteinander interagieren.

## <a name="passing-arguments"></a>Übergeben von Argumenten

Im Allgemeinen muss eine Klasse, die in XAML instanziiert wird, über einen öffentlichen parameterlosen Konstruktor verfügen. Das resultierende Objekt wird durch Eigenschaftseinstellungen initialisiert. Es gibt jedoch zwei weitere Möglichkeiten, wie Objekte instanziiert und initialisiert werden können.

Obwohl es sich hierbei um allgemeine Methoden handelt, werden sie größtenteils in Verbindung mit MVVM-Ansichtsmodellen verwendet.

### <a name="constructors-with-arguments"></a>Konstruktoren mit Argumenten

Das [**ParameteredConstructorDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo)-Beispiel veranschaulicht, wie das `x:Arguments`-Tag verwendet wird, um Konstruktorargumente anzugeben. Diese Argumente müssen durch Elementtags begrenzt werden, die den Typ des Arguments angeben. Für die grundlegenden .NET-Datentypen sind folgende Tags verfügbar:

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

### <a name="can-i-call-methods-from-xaml"></a>Kann ich Methoden aus XAML aufrufen?

Das [**FactoryMethodDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo)-Beispiel veranschaulicht, wie das `x:FactoryMethod`-Element verwendet wird, um eine Factorymethode anzugeben, die aufgerufen wird, um ein Objekt zu erstellen. Eine solche Factorymethode muss öffentlich und statisch sein, und sie muss ein Objekt des Typs erstellen, in dem sie definiert ist. (Beispielsweise ist die [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double))-Methode qualifiziert, weil sie öffentlich und statisch ist und einen Wert vom Typ `Color` zurückgibt.) Die Argumente für die Factorymethode werden innerhalb von `x:Arguments`-Tags angegeben.

## <a name="the-xname-attribute"></a>Das x:Name-Attribut

Das `x:Name`-Attribut ermöglicht die Vergabe eines Namens an ein in XAML instanziiertes Objekt. Die Regeln für diese Namen sind dieselben wie für Variablennamen in C#. Im Anschluss an die Rückgabe des `InitializeComponent`-Aufrufs in den Konstruktor kann die CodeBehind-Datei auf diese Namen verweisen, um auf das entsprechende XAML-Element zuzugreifen. Die Namen werden vom XAML-Parser tatsächlich in private Felder in der generierten partiellen Klasse konvertiert.

Das [**XamlClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock)-Beispiel veranschaulicht die Verwendung von `x:Name`, damit die CodeBehind-Datei zwei in XAML definierte `Label`-Elemente mit dem aktuellen Datum und der aktuellen Uhrzeit aktualisiert.

Derselbe Name kann nicht für mehrere Elemente auf derselben Seite verwendet werden. Dies ist ein besonderes Problem, wenn Sie `OnPlatform` zum Erstellen paralleler benannter Objekte für jede Plattform verwenden. Das [**PlatformSpecificLabele**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels)-Beispiel veranschaulicht eine bessere Möglichkeit, etwas Ähnliches zu tun.

## <a name="custom-xaml-based-views"></a>Benutzerdefinierte, XAML-basierte Ansichten

Es gibt verschiedene Möglichkeiten, um die Wiederholung von Markup in XAML zu vermeiden. Eine gängige Methode ist das Erstellen einer neuen XAML-basierten Klasse, die von [`ContentView`](xref:Xamarin.Forms.ContentView) abgeleitet wird. Dieses Verfahren wird im [**ColorViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList)-Beispiel veranschaulicht. Die `ColorView`-Klasse wird von `ContentView` abgeleitet, um eine bestimmte Farbe und ihren Namen anzuzeigen, während die `ColorViewListPage`-Klasse wie gewohnt von `ContentPage` abgeleitet wird und explizit 17 Instanzen von `ColorView` erstellt.

Der Zugriff auf die `ColorView`-Klasse in XAML erfordert eine andere XML-Namespacedeklaration, die für Klassen in derselben Assembly gängigerweise `local` heißt.

## <a name="events-and-handlers"></a>Ereignisse und Handler

Ereignisse können Ereignishandlern in XAML zugewiesen werden, aber der Ereignishandler selbst muss in der CodeBehind-Datei implementiert werden. Das [**XamlKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad)-Beispiel veranschaulicht, wie eine Bildschirmtastatur-Benutzeroberfläche in XAML erstellt wird und wie die `Clicked`-Handler in der CodeBehind-Datei implementiert werden.

## <a name="tap-gestures"></a>Tippgesten

Jedes `View`-Objekt kann Toucheingaben abrufen und Ereignisse aus dieser Eingabe generieren. Die `View`-Klasse definiert eine [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers)-Sammlungseigenschaft, die eine oder mehrere Instanzen von Klassen enthalten kann, die von [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) abgeleitet werden.

[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) generiert [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped)-Ereignisse. Das [**MonkeyTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap)-Programm veranschaulicht, wie `TapGestureRecognizer`-Objekte an vier `BoxView`-Elemente angefügt werden, um ein Imitationsspiel zu erstellen:

[![Dreifacher Screenshot von MonkeyTap](images/ch08fg07-small.png "Imitationsspiel")](images/ch08fg07-large.png#lightbox "Imitationsspiel")

Aber das **MonkeyTap**-Programm benötigt wirklich Sound. (Siehe [im nächsten Kapitel](chapter09.md).)

## <a name="related-links"></a>Verwandte Links

- [Kapitel 8 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Kapitel 8 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Kapitel 8 – F#-Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
