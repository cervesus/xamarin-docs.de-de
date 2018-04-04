---
title: Zusammenfassung der Kapitel 8. Code und XAML-Code in harmonische
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ffd2d508e99508309ec07c6bc65c8d716427bdff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Zusammenfassung der Kapitel 8. Code und XAML-Code in harmonische

In diesem Kapitel werden XAML besser, und insbesondere wie Code und XAML interagieren.

## <a name="passing-arguments"></a>Übergeben von Argumenten

Im allgemeinen Fall muss eine Klasse, die in XAML instanziiert einen öffentlichen parameterlosen Konstruktor haben; Das resultierende Objekt wird durch Einstellungen der Eigenschaften initialisiert. Es gibt jedoch zwei weitere Methoden, Objekte instanziiert und initialisiert werden können.

Obwohl diese allgemeine Techniken sind, werden sie meistens im Zusammenhang mit MVVM Ansichtsmodelle verwendet.

### <a name="constructors-with-arguments"></a>Konstruktoren mit Argumenten

Die [ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) Beispiel veranschaulicht, wie die `x:Arguments` Tag Konstruktorargumente an. Diese Argumente müssen durch, der angibt, des Typs des Arguments Element-Tags getrennt werden. Für die grundlegenden Datentypen .NET stehen die folgenden Tags:

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

### <a name="can-i-call-methods-from-xaml"></a>Kann Methoden aus XAML werden aufgerufen?

Die [ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) Beispiel veranschaulicht, wie die `x:FactoryMethod` Element an eine Factorymethode, die aufgerufen wird, um ein Objekt zu erstellen. Solche eine Factorymethode muss öffentlich und statisch sein, und es muss ein Objekt des Typs in der sie definiert ist erstellen. (Z. B. die [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/))-Methode qualifiziert werden, da er öffentlich und für statische und einen Wert vom Typ gibt `Color`.) Sind die Argumente für die Factory-Methode angegebenen `x:Arguments` Tags.

## <a name="the-xname-attribute"></a>X: Name-Attribut

Die `x:Name` Attribut ermöglicht, dass ein Objekt instanziiert in XAML an ein Name zugewiesen werden. Die Benennungsregeln für diese entsprechen den C#-Variablennamen. Befolgen die Rückgabe von der `InitializeComponent` im Konstruktor aufrufen, die Code-Behind-Datei kann auf diese Namen auf die entsprechende Verwendung von XAML-Element verweisen. Tatsächlich werden die Namen von XAML-Parser in private Felder in der generierten partiellen Klasse konvertiert.

Die [ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) Beispiel veranschaulicht die Verwendung von `x:Name` ermöglichen die Code-Behind-Datei zu zwei `Label` Elemente definiert, die in XAML, die mit dem aktuellen Datum und Uhrzeit aktualisiert.

Der gleiche Name kann nicht für mehrere Elemente auf derselben Seite verwendet werden. Dies ist ein bestimmtes Problem, bei Verwendung von `OnPlatform` paralleler mit dem Namen Objekte für jede Plattform. Die [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) demonstriert eine bessere Möglichkeit, etwas wie zu tun.

## <a name="custom-xaml-based-views"></a>Benutzerdefinierte XAML-basierte Ansichten

Es gibt mehrere Möglichkeiten zur Vermeidung von Wiederholung von XAML-Markup. Ein gängiges Verfahren zum Erstellen einer neuen XAML-basierte-Klasse, die abgeleitet ist [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Dieses Verfahren wird veranschaulicht, der [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) Beispiel. Die `ColorView` Klasse abgeleitet `ContentView` zwar zum Anzeigen einer bestimmten Farbe und den Namen der `ColorViewListPage` Klasse abgeleitet `ContentPage` wie gewohnt und explizit erstellt 17 Instanzen von `ColorView`.

Zugreifen auf die `ColorView` Klasse in XAML erfordert eine andere XML-Namespacedeklaration, häufig mit dem Namen `local` für Klassen in derselben Assembly.

## <a name="events-and-handlers"></a>Ereignisse und Ereignishandler

Ereignisse können Ereignishandler in XAML zugewiesen werden, aber der eigentliche Ereignishandler implementiert werden muss, in der CodeBehind-Datei. Die [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) veranschaulicht, wie eine Zehnertastatur-Benutzeroberfläche in XAML erstellt und zum Implementieren der `Clicked` Handler in der CodeBehind-Datei.

## <a name="tap-gestures"></a>Tippen Sie auf Gesten

Alle `View` Objekt abrufen Fingereingabe und generieren Sie Ereignisse aus dieser Eingabe. Die `View` Klasse definiert ein [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) Auflistungseigenschaft, die eine oder mehrere Instanzen von Klassen enthalten kann, die davon Herleiten [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/).

Die [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) generiert [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) Ereignisse. Die [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) Programm veranschaulicht, wie die Verbindung `TapGestureRecognizer` Objekte auf vier `BoxView` Elemente eine Nachahmung Spiel zu erstellen:

[![Dreifacher Screenshot des Affe Tap](images/ch08fg07-small.png "Imitation Spiel")](images/ch08fg07-large.png#lightbox "Imitation Spiel")

Aber die **MonkeyTap** Programm tatsächlich Sound benötigt. (Siehe [des nächsten Kapitels](chapter09.md).)



## <a name="related-links"></a>Verwandte Links

- [Kapitel 8 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Kapitel 8-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Kapitel 8 f#-Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
