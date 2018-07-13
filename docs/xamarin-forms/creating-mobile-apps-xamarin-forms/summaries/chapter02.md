---
title: Zusammenfassung der Kapitel 2. Aufbau einer app
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 2. Aufbau einer app'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d1daceba29e45adf64947c89555cc4e75a850d32
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995276"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Zusammenfassung der Kapitel 2. Aufbau einer app

In einer Xamarin.Forms-Anwendung werden als Objekte, die Platz auf dem Bildschirm bezeichnet *visuelle Elemente*, die durch gekapselte der [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Klasse. Visuelle Elemente können in drei Kategorien, die für diese Klassen aufgeteilt werden:

- [Seite](xref:Xamarin.Forms.Page)
- [Layout](xref:Xamarin.Forms.Layout)
- [Ansicht](xref:Xamarin.Forms.View)

Ein `Page` Ableitung belegt den gesamten Bildschirm oder beinahe den gesamten Bildschirm. Das untergeordnete Element einer Seite ist häufig eine `Layout` Ableitung untergeordneten visuellen Elemente zu organisieren. Die untergeordneten Elemente der `Layout` kann sein, andere `Layout` Klassen oder `View` ableitungen (häufig bezeichnet *Elemente*), die vertraute Objekte wie z. B. Text, Bitmaps, Schieberegler, Schaltflächen, Listenfeldern usw. sind.

In diesem Kapitel wird veranschaulicht, wie Sie eine Anwendung erstellen, durch die Konzentration auf die [ `Label` ](xref:Xamarin.Forms.Label), d.h. die `View` Ableitung, die Text anzeigt.

## <a name="say-hello"></a>Grüßen

Mit der Xamarin-Plattform installiert wird können Sie eine neue Xamarin.Forms-Projektmappe in Visual Studio oder Visual Studio für Mac erstellen. Die [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) Lösung verwendet eine Portable Klassenbibliothek für die gemeinsamen Code. Es zeigt eine Xamarin.Forms-Projektmappe in Visual Studio erstellt werden, ohne Änderungen. Die Lösung besteht aus sechs Projekte (die letzte, von denen zwei mit der aktuellen Lösungsvorlagen in Xamarin.Forms nicht erstellt werden):

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), eine Portable Klassenbibliothek (PCL), die von anderen Projekten gemeinsam genutzt
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), ein Projekt für Android
- [**"Hello.IOS"**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), ein Anwendungsprojekt für iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), ein Anwendungsprojekt für die universelle Windows-Plattform (Windows 10 und Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), ein Projekt für Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), ein Projekt für Windows Phone 8.1

Sie können stellen diese Projekte für die Anwendung die Startup-Projekt, und klicken Sie dann erstellen und führen Sie das Programm auf einem Gerät oder Simulator.

In vielen Ihrer Xamarin.Forms-Programme wird nicht die Anwendungsprojekte ändern Sie sein. Diese verbleiben häufig winzige Stubs, um das Programm zu starten. Die meisten sich der Fokus werden der portablen Klassenbibliothek, die für alle Anwendungen sein.

## <a name="inside-the-files"></a>In den Dateien

Die visuellen Elemente angezeigt, indem die **Hello** Programm werden im Konstruktor des definiert die [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) Klasse. `App` die Xamarin.Forms-Klasse abgeleitet [ `Application` ](xref:Xamarin.Forms.Application).

Die **Verweise** Teil der **Hello** PCL-Projekt enthält die folgenden Xamarin.Forms-Assemblys:

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

Jedes der Anwendungsprojekte enthält einen Aufruf der statischen `Forms.Init` -Methode in der die `Xamarin.Forms` Namespace. Initialisiert die Xamarin.Forms-Bibliothek. Eine andere Version von `Forms.Init` für jede Plattform definiert ist. Die Aufrufe dieser Methode finden Sie in folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App` -Klasse, `OnLaunched` Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` -Klasse, `OnLaunched` Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` -Klasse, `OnLaunched` Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Darüber hinaus muss für jede Plattform Instanziieren der `App` Klasse die Position in der PCL. Dieser Fehler tritt in einem Aufruf von `LoadApplication` in folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Andernfalls sind diese Anwendungsprojekte Programme für den normalen "nichts".

## <a name="pcl-or-sap"></a>PCL oder SAP?

Es ist möglich, eine Xamarin.Forms-Projektmappe mit der allgemeine Code in eine Portable Klassenbibliothek (PCL) oder eine freigegebene Asset-Projekt (SAP) zu erstellen. Um eine SAP-Lösung zu erstellen, wählen Sie die Shared-Option in Visual Studio. Die [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) Lösung wird veranschaulicht, die SAP-Vorlage ohne Änderungen.

Die PCL Ansatz Bündel alle der allgemeinen code in einem Klassenbibliotheksprojekt, das auf die Anwendung Plattformprojekte verweist. Mit dem SAP-Ansatz wird der allgemeine Code effektiv in allen Projekten die Platform-Anwendung vorhanden ist, und von ihnen gemeinsam genutzt wird.

Die meisten Xamarin.Forms-Entwickler bevorzugen die PCL-Ansatz. In diesem Buch werden die meisten Lösungen PCL. Mit denen SAP enthält ein **Sap** Suffix in den Namen des Projekts.

Um alle Xamarin.Forms-Plattformen zu unterstützen, muss die Version von .NET von der PCL verwendet die folgenden Plattformen berücksichtigt werden:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (klassisch)

Dies wird als PC Profile 111 bezeichnet.

Mit dem SAP-Ansatz kann der Code im freigegebenen Projekt für die verschiedenen Plattformen unterschiedlichen Code ausführen, indem Sie mithilfe von C#-präprozessoranweisungen (`#if`, #`elif`, und `#endif`) mit diesen vordefinierten Bezeichner:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

In einer PCL können Sie welche Plattform Sie zur Laufzeit ausführen auf bestimmen, wie Sie weiter unten in diesem Kapitel sehen werden.

## <a name="labels-for-text"></a>Bezeichnungen für text

Die [ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) Lösung veranschaulicht, wie eine neue C#-Datei zum Hinzufügen der **Greetings** Projekt. Diese Datei definiert eine Klasse namens `GreetingsPage` abgeleitet, die `ContentPage`. In diesem Buch die meisten Projekte enthalten eine einzelne `ContentPage` Ableitung, deren Name der Name des Projekts mit dem Suffix ist `Page` angefügt.

Die `GreetingsPage` Konstruktor instanziiert ein [ `Label` ](xref:Xamarin.Forms.Label) anzuzeigen, in der die Xamarin.Forms-Ansicht ist, das Text anzeigt. Die [ `Text` ](xref:Xamarin.Forms.Label.Text) -Eigenschaftensatz auf den Text von der `Label`. Dieses Programm legt die `Label` auf die `Content` Eigenschaft `ContentPage`. Der Konstruktor, der die `App` Klasse instanziiert, klicken Sie dann `GreetingsPage` und legt es auf seine `MainPage` Eigenschaft.

Der Text wird in der oberen linken Ecke der Seite angezeigt. Unter iOS bedeutet dies, dass sie die Seite der Statusleiste überlappt. Es gibt mehrere Lösungen für dieses Problem:

### <a name="solution-1-include-padding-on-the-page"></a>Lösung 1. Auffüllung, auf der Seite enthalten

Legen Sie eine [ `Padding` ](xref:Xamarin.Forms.Page.Padding) Eigenschaft auf der Seite. `Padding` ist vom Typ [ `Thickness` ](xref:Xamarin.Forms.Thickness), eine Struktur mit den vier Eigenschaften:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` definiert einen Bereich innerhalb einer Seite, wenn der Inhalt ausgeschlossen ist. Dadurch wird die `Label` die iOS-Statusleiste überschreibungsänderungen vermeiden.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Lösung 2. Enthalten Sie Auffüllung nur für iOS (nur für SAP)

Festlegen Sie eine 'Abstand'-Eigenschaft wird nur unter iOS, die mithilfe eines von SAP mit einer C#-präprozessoranweisung. Dies wird veranschaulicht, der [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) Lösung.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Lösung 3. Enthalten Sie Auffüllung nur für iOS (PCL oder SAP)

In der Version von Xamarin.Forms verwendet werden, für das Buch eine `Padding` Eigenschaft, die spezifisch für iOS in einer PCL oder SAP kann ausgewählt werden, mithilfe der [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) oder [ `Device.OnPlatform<T>` ](xref:Xamarin.Forms.Device.OnPlatform*) statische Methode. Diese Methoden sind jetzt veraltet.

Die `Device.OnPlatform` Methoden werden verwendet, um plattformspezifischen Code auszuführen oder bestimmte Werte auswählen. Intern, stellen sie verwenden die [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) statische schreibgeschützte Eigenschaft, die Mitglied der zurückgibt. die [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) Enumeration:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) für Windows 8.1, Windows Phone 8.1 und alle UWP-Geräte.
- [`WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone), zuvor verwendet zum Identifizieren von Windows Phone 8.0 ist aber jetzt nicht mehr verwendeten
- [`Other`](xref:Xamarin.Forms.TargetPlatform.Other) wird nicht verwendet

Die `Device.OnPlatform` Methoden, die `Device.OS` -Eigenschaft, und die `TargetPlatform` Enumeration sind jetzt als veraltet markiert. Verwenden Sie stattdessen die [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) -Eigenschaft, und Vergleichen der `string` Rückgabewert mit den folgenden statischen Feldern:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), die Zeichenfolge "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android), die Zeichenfolge "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP), die Zeichenfolge "UWP", auf die Windows-Runtime-Plattform
- `Windows`, die Zeichenfolge "Windows" für die Windows-Runtime (Windows 8.1 und Windows Phone 8.1, veraltet)
- `WinPhone`, die Zeichenfolge "WinPhone" für Windows Phone 8.0 (veraltet)

Die [ `Device.Idiom` ](xref:Xamarin.Forms.Device.Idiom) statische schreibgeschützte Eigenschaft zugeordnet ist. Gibt zurück. ein Mitglied der [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom), die umfasst folgende Member:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) wird nicht verwendet

Für iOS und Android, den Grenzwert zwischen `Tablet` und `Phone` Hochformat Breite 600 Einheiten ist. Für die Windows-Plattform `Desktop` gibt eine UWP-Anwendung unter Windows 10, `Tablet` ist ein Windows 8.1-Programm, und `Phone` gibt an, die unter Windows 10 oder eine Windows Phone 8.1-Anwendung eine UWP-Anwendung.

## <a name="solution-3a-set-margin-on-the-label"></a>Lösung 3a: Rand für die Bezeichnung setzen.

Der [ `Margin` ](xref:Xamarin.Forms.View.Margin) Eigenschaft wurde zu spät eingeführt, um in das Buch aufgenommen werden, es ist aber auch vom Typ `Thickness` und Sie können sie festlegen, auf die `Label` um einen Bereich außerhalb der Ansicht definieren, bei der Berechnung des eingeschlossen ist, die Layout der Ansicht.

Die `Padding` Eigenschaft ist nur verfügbar für [ `Layout` ](xref:Xamarin.Forms.Layout) und [ `Page` ](xref:Xamarin.Forms.Page) ableitungen. Die `Margin` Eigenschaft ist verfügbar auf allen [ `View` ](xref:Xamarin.Forms.View) ableitungen.

## <a name="solution-4-center-the-label-within-the-page"></a>Lösung 4. Zentrieren Sie die Bezeichnung auf der Seite

Können Sie Zentrieren der `Label` innerhalb der `Page` (oder fügen Sie ihn in eine der anderen acht Stellen) durch Festlegen der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften der `Label` Um einen Wert vom Typ [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions). Die `LayoutOptions` Struktur definiert zwei Eigenschaften:

- Ein [ `Alignment` ](xref:Xamarin.Forms.LayoutOptions.Alignment) Eigenschaft vom Typ [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment), eine Enumeration mit vier Mitglieder: [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), d. h. linken oder oberen je die Ausrichtung [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), d.h. rechten oder unteren je nach Ausrichtung und [ `Fill` ](xref:Xamarin.Forms.LayoutAlignment.Fill).

- Ein [ `Expands` ](xref:Xamarin.Forms.LayoutOptions.Expands) Eigenschaft vom Typ `bool`.

Diese Eigenschaften werden in der Regel nicht direkt verwendet. Stattdessen werden die Kombinationen aus diesen beiden Eigenschaften von acht statische schreibgeschützte Eigenschaften des Typs bereitgestellt `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` und `VerticalOptions` sind die wichtigsten Eigenschaften in Xamarin.Forms-Layouts und werden ausführlicher in [ **Kapitel 4. Scrollen im Stapel**](chapter04.md).

Hier ist das Ergebnis mit der `HorizontalOptions` und `VerticalOptions` Eigenschaften `Label` auf festlegen `LayoutOptions.Center`:

[![Dreifacher Screenshot Greetings-Programms](images/ch02fg05-small.png "horizontal und vertikal ausgerichtet Bezeichnung")](images/ch02fg05-large.png#lightbox "horizontal und vertikal ausgerichtet Bezeichnung")

## <a name="solution-5-center-the-text-within-the-label"></a>Lösung 5. Der Text innerhalb der Bezeichnung zentrieren

Sie können den Text zentrieren (oder fügen Sie ihn in acht andere Speicherorte auf der Seite) durch Festlegen der [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) und [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) Eigenschaften `Label` auf einen Member der [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) Enumeration:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), Bedeutung linken oder oberen (je nach Ausrichtung)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), d. h. rechten oder unteren (je nach Ausrichtung)

Diese beiden Eigenschaften werden nur von definiert `Label`, während die `HorizontalAlignment` und `VerticalAlignment` Eigenschaften definiert werden, indem `View` und von allen geerbt `View` ableitungen. Die besten Ergebnisse scheinen ähnlich, aber sie sind sehr verschieden, wie im nächste Kapitel zeigt.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 2 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Kapitel 2-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Kapitel 2 F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Erste Schritte mit Xamarin.Forms](~/xamarin-forms/get-started/index.md)
