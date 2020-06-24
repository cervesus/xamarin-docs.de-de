---
title: Zusammenfassung von Kapitel 2. Aufbau einer App
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 2. Aufbau einer App'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 78da3ed91acea0c056074d712d368de70b251392
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136915"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Zusammenfassung von Kapitel 2. Aufbau einer App

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> In den Anmerkungen auf dieser Seite wird erläutert, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

In einer Xamarin.Forms-Anwendung werden Objekte, die Platz auf dem Bildschirm belegen, als *visuelle Elemente* bezeichnet, die in der [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Klasse gekapselt sind. Visuelle Elemente lassen sich in drei Kategorien unterteilen, die diesen Klassen entsprechen:

- [Seite](xref:Xamarin.Forms.Page)
- [Layout](xref:Xamarin.Forms.Layout)
- [Ansicht](xref:Xamarin.Forms.View)

Eine `Page`-Ableitung nimmt den ganzen Bildschirm oder fast den ganzen Bildschirm ein. Häufig ist das untergeordnete Element einer Seite eine `Layout`-Ableitung, um untergeordnete visuelle Elemente zu organisieren. Die untergeordneten Elemente des `Layout` können andere `Layout`-Klassen oder `View`-Ableitungen sein (häufig als *Elemente* bezeichnet), wobei es sich um vertraute Objekte handelt wie Text, Bitmaps, Schieberegler, Schaltflächen, Listenfelder usw.

In diesem Kapitel wird veranschaulicht, wie Sie eine Anwendung erstellen, indem Sie sich auf das [`Label`](xref:Xamarin.Forms.Label) konzentrieren, wobei es sich um die `View`-Ableitung handelt, mit der Text angezeigt wird.

## <a name="say-hello"></a>„Hallo“ sagen

Wenn Sie die Xamarin-Plattform installiert haben, können Sie in Visual Studio oder in Visual Studio für Mac eine neue Xamarin.Forms-Projektmappe erstellen. Die [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello)-Projektmappe verwendet eine portable Klassenbibliothek (Portable Class Library) für den allgemeinen Code.

> [!NOTE]
> Portable Klassenbibliotheken wurden durch .NET Standard-Bibliotheken ersetzt. Der gesamte Beispielcode innerhalb des Buchs wurde aktualisiert und verwendet jetzt die .NET Standard-Bibliotheken.

Dieses Beispiel veranschaulicht eine Xamarin.Forms-Projektmappe, die ohne Änderungen in Visual Studio erstellt wurde. Die Projektmappe besteht aus vier Projekten:

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), eine portable Klassenbibliothek (Portable Class Library, PCL), die von anderen Projekten gemeinsam genutzt wird.
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), ein Anwendungsprojekt für Android.
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), ein Anwendungsprojekt für iOS.
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), ein Anwendungsprojekt für die universelle Windows-Plattform (Windows 10 und Windows 10 Mobile).

> [!NOTE]
> Xamarin.Forms bietet keine Unterstützung mehr für Windows 8.1, Windows Phone 8.1 oder Windows 10 Mobile, aber Xamarin.Forms-Anwendungen können unter Windows 10 Desktop ausgeführt werden.

Sie können jedes dieser Anwendungsprojekte als Startprojekt festlegen und dann das Programm auf einem Gerät oder Simulator erstellen und ausführen.

In vielen Ihrer Xamarin.Forms-Programme werden Sie die Anwendungsprojekte nicht ändern. Diese bleiben häufig kleine Stubs, mit denen nur das Programm gestartet wird. Der größte Teil Ihres Schwerpunkts liegt auf der Bibliothek, die allen Anwendungen zugrunde liegt.

## <a name="inside-the-files"></a>Das Innere der Dateien

Die visuellen Elemente, die vom **Hello**-Programm angezeigt werden, werden im Konstruktor der [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs)-Klasse definiert. `App` wird von der Xamarin.Forms-Klasse [`Application`](xref:Xamarin.Forms.Application) abgeleitet.

> [!NOTE]
> Mit den Visual Studio-Projektmappenvorlagen für Xamarin.Forms wird eine Seite mit einer XAML-Datei erstellt. XAML wird in diesem Buch erst in [Kapitel 7](chapter07.md) behandelt.

Der Abschnitt **Verweise** des **Hello**-PCL-Projekts enthält die folgenden Xamarin.Forms-Assemblys:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

Die **Verweise**-Abschnitte der fünf Anwendungsprojekte enthalten zusätzliche Assemblys, die für die einzelnen Plattformen gelten:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

> [!NOTE]
> In den **Verweise**-Abschnitten dieser Projekte werden die Assemblys nicht mehr aufgeführt. Stattdessen enthält die Projektdatei **PackageReference**-Tags, die auf das Xamarin.Forms-NuGet-Paket verweisen. Im Abschnitt **Verweise** in Visual Studio wird das **Xamarin.Forms** -Paket anstelle der Xamarin.Forms-Assemblys aufgeführt.

Jedes der Anwendungsprojekte enthält einen Aufruf der statischen `Forms.Init`-Methode im `Xamarin.Forms`-Namespace. Dadurch wird die Xamarin.Forms-Bibliothek initialisiert. Für jede Plattform wird eine andere Version von `Forms.Init` definiert. Die Aufrufe dieser Methode finden Sie in den folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App`-Klasse, `OnLaunched`-Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

Zusätzlich muss jede Plattform den Speicherort der `App`-Klasse in der freigegebenen Bibliothek instanziieren. Dies erfolgt in einem Aufruf von `LoadApplication` in den folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

Andernfalls handelt es sich bei diesen Anwendungsprojekten um normale „Do Nothing“-Programme (Nichts ausführen).

## <a name="pcl-or-sap"></a>PCL oder SAP?

Es ist möglich, eine Xamarin.Forms-Projektmappe mit dem allgemeinen Code entweder in einer portierbaren Klassenbibliothek (Portable Class Library, PCL) oder in einem Projekt mit freigegebenen Anlagen (Shared Asset Project, SAP) zu erstellen. Wählen Sie zum Erstellen einer SAP-Projektmappe in Visual Studio die Option „Freigegeben“ aus. Die [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap)-Projektmappe veranschaulicht die SAP-Vorlage ohne Änderungen.

> [!NOTE]
> Portable Klassenbibliotheken wurden durch .NET Standard-Bibliotheken ersetzt. Der gesamte Beispielcode innerhalb des Buchs wurde aktualisiert und verwendet jetzt die .NET Standard-Bibliotheken. Andernfalls sind sich die PCL- und .NET Standard-Bibliotheken konzeptionell sehr ähnlich.

Der Bibliotheksansatz bündelt den gesamten allgemeinen Code in einem Bibliotheksprojekt, auf das von den Plattformanwendungsprojekten verwiesen wird. Beim SAP-Ansatz ist der allgemeine Codein in allen Plattformanwendungsprojekten tatsächlich vorhanden und wird von diesen gemeinsam genutzt.

Die meisten Xamarin.Forms-Entwickler bevorzugen den Bibliotheksansatz. In diesem Buch verwenden die meisten Projektmappen eine Bibliothek. Die, die SAP verwenden, enthalten ein **SAP**-Suffix im Projektnamen.

Beim SAP-Ansatz kann der Code im freigegebenen Projekt mithilfe von C#-Präprozessordirektiven (`#if`, #`elif` und `#endif`) mit diesen vordefinierten Bezeichnern unterschiedlichen Code für die verschiedenen Plattformen ausführen:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

In einer freigegebenen Bibliothek können Sie bestimmen, auf welcher Plattform Sie zur Laufzeit ausführen werden, wie Sie später in diesem Kapitel sehen werden.

## <a name="labels-for-text"></a>Bezeichnungen für Text

Die [**Greetings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings)-Projektmappe veranschaulicht, wie Sie dem anderen **Greetings**-Projekt eine neue C#-Datei hinzufügen. In dieser Datei wird eine Klasse namens `GreetingsPage` definiert, die sich von `ContentPage` ableitet. In diesem Buch enthalten die meisten Projekte eine einzelne `ContentPage`-Ableitung, deren Name dem Namen des Projekts entspricht, mit angehängtem `Page`-Suffix.

Der `GreetingsPage`-Konstruktor instanziiert eine [`Label`](xref:Xamarin.Forms.Label)-Ansicht, bei der es sich um die Xamarin.Forms-Ansicht handelt, in der Text angezeigt wird. Der [`Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaft wird auf den Text festgelegt, der von `Label` angezeigt werden soll. Dieses Programm legt das `Label` auf die `Content`-Eigenschaft von `ContentPage` fest. Der Konstruktor der `App`-Klasse instanziiert dann `GreetingsPage` und legt sie auf ihre `MainPage`-Eigenschaft fest.

Der Text wird in der oberen linken Ecke der Seite angezeigt. Unter iOS bedeutet dies, dass er die Statusleiste der Seite überlappt. Für dieses Problem gibt es mehrere Lösungen:

### <a name="solution-1-include-padding-on-the-page"></a>Lösung 1. Aufnehmen von Innenabstand auf der Seite

Legen Sie eine [`Padding`](xref:Xamarin.Forms.Page.Padding)-Eigenschaft auf der Seite fest. `Padding` ist vom Typ [`Thickness`](xref:Xamarin.Forms.Thickness), einer Struktur mit vier Eigenschaften:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` definiert einen Bereich innerhalb einer Seite, in dem Inhalt ausgeschlossen wird. Hierdurch kann `Label` vermeiden, die iOS-Statusleiste zu überschreiben.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Lösung 2. Aufnehmen von Innenabstand nur für iOS (nur SAP)

Legen Sie eine „Padding“-Eigenschaft (Innenabstand) nur unter iOS fest, indem Sie ein SAP mit einer C#-Präprozessordirektive verwenden. Dies wird in der [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap)-Projektmappe veranschaulicht.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Lösung 3. Aufnehmen von Innenabstand nur für iOS (PCL oder SAP)

In der Version von Xamarin.Forms, die für dieses Buch verwendet wird, kann eine `Padding`-Eigenschaft, die in einer PCL oder einem SAP für iOS spezifisch ist, mithilfe der statischen [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))- oder [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*)-Methode ausgewählt werden. Diese Methoden sind jetzt veraltet.

Die `Device.OnPlatform`-Methoden werden verwendet, um plattformspezifischen Code auszuführen oder plattformspezifische Werte auszuwählen. Intern verwenden sie statische, schreibgeschützte [`Device.OS`](xref:Xamarin.Forms.Device.OS)-Eigenschaft, die einen Member der [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform)-Enumeration zurückgibt:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) für UWP-Geräte.

Die `Device.OnPlatform`-Methoden, die `Device.OS`-Eigenschaft und die `TargetPlatform`-Enumeration sind jetzt alle veraltet. Verwenden Sie stattdessen die [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform)-Eigenschaft, und vergleichen Sie den `string`-Rückgabewert mit den folgenden statischen Feldern:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), die Zeichenfolge „iOS“.
- [`Android`](xref:Xamarin.Forms.Device.Android), die Zeichenfolge „Android“.
- [`UWP`](xref:Xamarin.Forms.Device.UWP), die Zeichenfolge „UWP“, die sich auf die universelle Windows-Plattform bezieht.

Die statische, schreibgeschützte [`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom)-Eigenschaft ist damit verwandt. Diese gibt einen Member von [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) zurück, der diese Member enthält:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) wird nicht verwendet.

Für iOS und Android stellt der Trennungswert (Cutoff) zwischen `Tablet` und `Phone` eine Hochformatbreite von 600 Einheiten dar. Für die Windows-Plattform gibt `Desktop` eine UWP-Anwendung an, die unter Windows 10 ausgeführt wird, und `Phone` gibt eine UWP-Anwendung an, die unter einer Windows 10-Anwendung ausgeführt wird.

## <a name="solution-3a-set-margin-on-the-label"></a>Lösungs 3a. Festlegen eines Rands für die Bezeichnung

Die [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft wurde zu spät eingeführt, um in diesem Buch enthalten zu sein, aber Sie ist ebenfalls vom Typ `Thickness`, und Sie können sie für das `Label` festlegen, um einen Bereich außerhalb der Ansicht zu definieren, der in der Berechnung des Layouts der Ansicht einbezogen wird.

Die `Padding`-Eigenschaft ist nur für Ableitungen von [`Layout`](xref:Xamarin.Forms.Layout) und [`Page`](xref:Xamarin.Forms.Page) verfügbar. Die `Margin`-Eigenschaft ist für alle [`View`](xref:Xamarin.Forms.View)-Ableitungen verfügbar.

## <a name="solution-4-center-the-label-within-the-page"></a>Lösung 4. Zentrieren der Bezeichnung innerhalb der Seite

Sie können das `Label` innerhalb der `Page` zentrieren (oder es an einer von acht anderen Stellen platzieren), indem Sie die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) von `Label` auf einen Wert vom Typ [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) festlegen. Die `LayoutOptions`-Struktur definiert zwei Eigenschaften:

- Eine [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment)-Eigenschaft des Typs [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment), eine Enumeration mit vier Membern: [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), was in Abhängigkeit von der Ausrichtung links oder oben bedeutet, [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End), was in Abhängigkeit von der Ausrichtung rechts oder unten bedeutet, und [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill).

- Eine [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)-Eigenschaft vom Typ `bool`.

Im Allgemeinen werden diese Eigenschaften nicht direkt verwendet. Stattdessen werden Kombinationen dieser beiden Eigenschaften durch acht statische, schreibgeschützte Eigenschaften vom Typ `LayoutOptions` bereitgestellt:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` und `VerticalOptions` sind die wichtigsten Eigenschaften im Xamarin.Forms-Layout und werden ausführlicher in [**Kapitel 4: „Scrollen im Stapel“** ](chapter04.md), behandelt.

Im Folgenden sehen Sie das Ergebnis, wenn die Eigenschaften `HorizontalOptions` und `VerticalOptions` von `Label` beide auf `LayoutOptions.Center`festgelegt sind:

[![Dreifacher Screenshot des „Greetings“-Programms](images/ch02fg05-small.png "Horizontal und vertikal zentrierte Bezeichnung")](images/ch02fg05-large.png#lightbox "Horizontal und vertikal zentrierte Bezeichnung")

## <a name="solution-5-center-the-text-within-the-label"></a>Lösung 5. Zentrieren des Texts innerhalb der Bezeichnung

Sie können den Text auch zentrieren (oder an acht anderen Stellen auf der Seite platzieren), indem Sie die Eigenschaften [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) und [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) von `Label` auf einen Member der [`TextAlignment`](xref:Xamarin.Forms.TextAlignment)-Enumeration festlegen:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), was links oder oben bedeutet (abhängig von der Ausrichtung).
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), was rechts oder unten bedeutet (abhängig von der Ausrichtung).

Diese beiden Eigenschaften werden nur von `Label` definiert, während die Eigenschaften `HorizontalAlignment` und `VerticalAlignment` von `View` definiert und von allen `View`-Ableitungen geerbt werden. Die visuellen Ergebnisse sehen möglicherweise ähnlich aus, sind aber sehr unterschiedlich, wie im nächsten Kapitel veranschaulicht wird.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 2 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Kapitel 2 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Kapitel 2 – F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Erste Schritte mit Xamarin.Forms](~/get-started/index.yml)
