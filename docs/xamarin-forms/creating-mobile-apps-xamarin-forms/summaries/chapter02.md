---
title: Zusammenfassung der Kapitel 2. Aufbau einer App
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 81bcc8e2f8627264820a859123e1be1a9f960a92
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Zusammenfassung der Kapitel 2. Aufbau einer App

In einer Xamarin.Forms-Anwendung werden Objekte, die Platz auf dem Bildschirm als bezeichnet *visuelle Elemente*, gekapselten durch die [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Klasse. Visuelle Elemente können in drei Kategorien, die für diese Klassen aufgeteilt werden:

- [Seite](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Layout](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [Ansicht](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

Ein `Page` Ableitung belegt den gesamten Bildschirm oder nahezu den gesamten Bildschirm. Häufig das untergeordnete Element einer Seite ist eine `Layout` Ableitung, untergeordneten visuellen Elemente zu organisieren. Die untergeordneten Elemente der `Layout` kann andere `Layout` Klassen oder `View` ableitungen (häufig aufgerufen *Elemente*), die vertraut sind Objekte, z. B. Text, Bitmaps, Schieberegler, Schaltflächen, Listenfeldern usw. sind.

In diesem Kapitel wird veranschaulicht, wie eine Anwendung zu erstellen, durch die Fokussierung auf die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), also die `View` Ableitung, in dem Text angezeigt.

## <a name="say-hello"></a>Begrüßen

Mit der Xamarin-Plattform installiert wird können Sie eine neue Xamarin.Forms-Projektmappe in Visual Studio oder Visual Studio für Mac erstellen. Die [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) Lösung verwendet eine Portable Klassenbibliothek für den gemeinsamen Code. Es zeigt eine Xamarin.Forms-Projektmappe in Visual Studio mit keine Änderungen erstellt. Die Lösung umfasst sechs Projekte (die letzte, von denen zwei nicht mit den aktuellen Xamarin.Forms-Projektmappenvorlagen erstellt werden):

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), eine Portable Klassenbibliothek (PCL) von anderen Projekten gemeinsam verwendet werden
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), ein Projekt für die Anwendung für Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), ein Projekt für die Anwendung für iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), ein Projekt für die Anwendung für die universelle Windows-Plattform (Windows 10 und Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), ein Projekt für die Anwendung für Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), ein Projekt für die Anwendung für Windows Phone 8.1

Sie können stellen diese Anwendung Projekte das Startup-Projekt, und klicken Sie dann erstellen und führen Sie das Programm auf einem Gerät oder den Simulator.

In vielen Programmen Xamarin.Forms wird nicht die Anwendungsprojekte ändern werden, werden. Diese verbleiben häufig denkbar Stubs nur um das Programm zu starten. Die meisten sich der Fokus werden der portablen Klassenbibliothek, die für alle Anwendungen sein.

## <a name="inside-the-files"></a>In den Dateien

Die visuellen Elemente angezeigt, indem die **Hello** Programm sind in den Konstruktor des definiert die [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) Klasse. `App` leitet sich von der Klasse Xamarin.Forms [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

Die **Verweise** Teil der **Hello** PCL-Projekt enthält die folgenden Xamarin.Forms Assemblys:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

Die **Verweise** Abschnitte der fünf Anwendungsprojekte enthalten zusätzliche Assemblys, die für die einzelnen Plattformen gelten:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Jede die Anwendungsprojekte enthält einen Aufruf der statischen `Forms.Init` Methode in der `Xamarin.Forms` Namespace. Dadurch wird die Bibliothek Xamarin.Forms initialisiert. Eine andere Version von `Forms.Init` für jede Plattform definiert ist. Aufrufe dieser Methode finden Sie in folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Universelle Windows-Plattform: [ `App` -Klasse, `OnLaunched` Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` -Klasse, `OnLaunched` Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` -Klasse, `OnLaunched` Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Darüber hinaus muss für jede Plattform Instanziieren der `App` Speicherort in der PCL Klasse. Dieser Fehler tritt in einem Aufruf von `LoadApplication` in folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UNIVERSELLE WINDOWS-PLATTFORM: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Andernfalls sind diese Anwendungsprojekte normalen "nichts" Programme.

## <a name="pcl-or-sap"></a>PCL oder SAP?

Es ist möglich, erstellen eine Xamarin.Forms-Projektmappe mit der allgemeine Code in eine Portable Klassenbibliothek (PCL) oder eine freigegebene Asset-Projekt (SAP). Wählen Sie zum Erstellen von einer SAP-Lösung, die Shared-Option in Visual Studio. Die [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) Lösung wird veranschaulicht, die SAP-Vorlage mit keine Änderungen.

Die PCL Ansatz Bündel alle gemeinsamen code in eine Steuerelementbibliothek-Projekt verweist auf die Plattform-Anwendungsprojekte. Mit dem SAP-Ansatz der allgemeine Code effektiv ist in der Plattform-Anwendungsprojekte vorhanden und wird für sie freigegeben.

Die meisten Xamarin.Forms Entwickler bevorzugen das PCL-Ansatz. In diesem Handbuch werden die meisten Lösungen PCL. Solcher, bei denen SAP enthalten eine **Sap** Suffix in den Namen des Projekts.

Um die Xamarin.Forms-Plattformen unterstützen möchten, muss die Version von .NET die PCL, in den folgenden Plattformen berücksichtigen:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (klassisch)

Dies wird als PC Profil 111 bezeichnet.

Mit dem SAP-Ansatz kann der Code im freigegebenen Projekt anderen Code für die verschiedenen Plattformen ausgeführt werden mithilfe von C#-Präprozessordirektiven (`#if`, #`elif`, und `#endif`) mit diesen vordefinierte Bezeichner:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UNIVERSELLE WINDOWS-PLATTFORM: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

In einer PCL können Sie Sie zur Laufzeit ausführen Plattform bestimmen, wie Sie weiter unten in diesem Kapitel sehen.

## <a name="labels-for-text"></a>Bezeichnungen für text

Die [ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) Lösung veranschaulicht das Hinzufügen einer neuen C#-Datei, die **Greetings** Projekt. Diese Datei definiert eine Klasse namens `GreetingsPage` abgeleitet, die `ContentPage`. In diesem Buch die meisten Projekte enthalten eine einzelne `ContentPage` Ableitung, deren Name der Name des Projekts mit dem Suffix ist `Page` angefügt.

Die `GreetingsPage` Konstruktor instanziiert einen [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Sicht, die die Xamarin.Forms-Sicht handelt, die Text anzeigt. Die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaftensatz wird auf den Text angezeigt, indem die `Label`. Dieses Programm legt die `Label` auf die `Content` Eigenschaft `ContentPage`. Der Konstruktor, der die `App` Klasse instanziiert, klicken Sie dann `GreetingsPage` und legt es auf seine `MainPage` Eigenschaft.

Der Text wird in der oberen linken Ecke der Seite angezeigt. IOS bedeutet dies, dass es sich um die Seite Statusleiste überschneidet. Es gibt mehrere Lösungen für dieses Problem:

### <a name="solution-1-include-padding-on-the-page"></a>Lösung 1. Einschließen von Leerraum auf der Seite

Legen Sie eine [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) Eigenschaft auf der Seite. `Padding` ist vom Typ [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), eine Struktur mit vier Eigenschaften:

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` definiert einen Bereich innerhalb einer Seite, auf dem Inhalt ausgeschlossen ist. Dies ermöglicht die `Label` zu vermeiden, überschreiben die iOS-Statusleiste.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Lösung 2. Enthalten Sie Auffüllung nur für iOS (nur SAP)

Festlegen Sie Eigenschaft "Auffüllung" nur für iOS mit einer SAP mit einer C#-Präprozessordirektive. Dies wird dargestellt, der [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) Lösung.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Lösung 3. Enthalten Sie Auffüllung nur für iOS (PCL oder SAP)

In der Version von Xamarin.Forms für das Buch verwendet eine `Padding` Eigenschaft, die spezifisch für iOS in einer PCL oder SAP kann ausgewählt werden, mithilfe der [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) oder [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) statische Methode. Diese Methoden sind jetzt veraltet.

Die `Device.OnPlatform` Methoden werden verwendet, um plattformspezifischen Code ausführen oder plattformspezifische Werte auszuwählen. Intern, stellen sie nutzen die [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) statische schreibgeschützte Eigenschaft, die zurückgibt, ein Mitglied der [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) Enumeration:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) für Windows 8.1, Windows Phone 8.1 und alle uwp-Geräte.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), zuvor verwendet zum Identifizieren von Windows Phone 8.0 ist aber jetzt nicht verwendet.
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) wird nicht verwendet

Die `Device.OnPlatform` Methoden, die `Device.OS` -Eigenschaft, und die `TargetPlatform` Enumeration sind alle jetzt als veraltet markiert. Verwenden Sie stattdessen die [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) -Eigenschaft, und Vergleichen der `string` Rückgabewert mit den folgenden statischen Feldern:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), die Zeichenfolge "iOS" 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), die Zeichenfolge "Android"
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), die Zeichenfolge "UWP" verweist auf die Windows-Runtime-Plattform
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), die Zeichenfolge "Windows" für Windows-Runtime (Windows 8.1 und Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), die Zeichenfolge "WinPhone" für Windows Phone 8.0 

Die [ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) statische schreibgeschützte Eigenschaft verknüpft ist. Dies gibt ein Mitglied der [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), dem umfasst folgende Member:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) wird nicht verwendet

Für iOS und Android, den Unterschied zwischen `Tablet` und `Phone` Hochformat Breite 600 Einheiten. Für die Windows-Plattform `Desktop` gibt eine uwp-Anwendung unter Windows 10, `Tablet` ist ein Programm für Windows 8.1 und `Phone` gibt eine uwp-Anwendung, die unter Windows 10 oder Windows Phone 8.1-Anwendung an.

## <a name="solution-3a-set-margin-on-the-label"></a>Lösung 3a. Rand auf die Bezeichnung setzen.

Die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Eigenschaft wurde zu spät eingeführt, um in der Dokumentation enthalten sein, aber es ist auch vom Typ `Thickness` und Sie können festlegen, auf die `Label` um einen Bereich außerhalb der Ansicht zu definieren, die bei der Berechnung der enthalten ist das Anzeigen des Layouts.

Die `Padding` Eigenschaft ist nur verfügbar für [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) und [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) ableitungen. Die `Margin` Eigenschaft steht für alle [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ableitungen.

## <a name="solution-4-center-the-label-within-the-page"></a>Lösung 4. Zentrieren Sie die Bezeichnung auf der Seite

Können die `Label` innerhalb der `Page` (oder fügen Sie ihn in einer der anderen acht Stellen) durch Festlegen der [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften der `Label` Um einen Wert vom Typ [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). Die `LayoutOptions` Struktur definiert zwei Eigenschaften:

- Ein [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) Eigenschaft vom Typ [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), eine Enumeration mit vier Elementen: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), d. h. nach links oder je nach oben die Ausrichtung, [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), womit rechten oder unteren abhängig von der Ausrichtung und [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- Ein [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) Eigenschaft vom Typ `bool`.

Diese Eigenschaften werden in der Regel nicht direkt verwendet. Stattdessen werden die Kombinationen dieser beiden Eigenschaften von acht statischen schreibgeschützten Eigenschaften des Typs bereitgestellt `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` und `VerticalOptions` sind die wichtigsten Eigenschaften in Xamarin.Forms Layout und werden in ausführlicher erläutert [ **Chapter 4. Durchführen eines Bildlaufs im Stapel**](chapter04.md).

Hier ist das Ergebnis mit den `HorizontalOptions` und `VerticalOptions` Eigenschaften des `Label` festgelegt `LayoutOptions.Center`:

[![Dreifacher Screenshot der Anwendung Greetings](images/ch02fg05-small.png "horizontal und vertikal zentriert bezeichnen")](images/ch02fg05-large.png#lightbox "horizontal und vertikal zentriert Bezeichnung")

## <a name="solution-5-center-the-text-within-the-label"></a>Lösung 5. Der Text innerhalb der Bezeichnung zentrieren

Sie können auch zentrieren Text (oder fügen Sie ihn in anderen acht Positionen auf der Seite) durch Festlegen der [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) und [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) Eigenschaften von `Label` an ein Mitglied der [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) Enumeration:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), Bedeutung links oder oben (je nach Ausrichtung)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), d. h. rechten oder unteren (je nach Ausrichtung)

Diese beiden Eigenschaften sind nur von definiert `Label`, während die `HorizontalAlignment` und `VerticalAlignment` Eigenschaften werden definiert, indem `View` und geerbt, die von allen `View` ableitungen. Die visuellen Ergebnisse ähnlich erscheinen möglicherweise, doch sind sie sehr unterschiedlich, wie im nächste Kapitel veranschaulicht.



## <a name="related-links"></a>Verwandte Links

- [Chapter 2 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Chapter 2-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Chapter 2 f#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Erste Schritte mit Xamarin.Forms](~/xamarin-forms/get-started/index.md)
