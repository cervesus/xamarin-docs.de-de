---
title: Zusammenfassung der Kapitel 2. Aufbau einer app
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 2. Aufbau einer app'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: f900cb1532ba4415127c95b07e777881e1d74994
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724994"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Zusammenfassung der Kapitel 2. Aufbau einer app

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

In einer xamarin. Forms-Anwendung werden Objekte, die Speicherplatz auf dem Bildschirm belegen, als *visuelle Elemente*bezeichnet, die von der [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse gekapselt sind. Visuelle Elemente können in drei Kategorien, die für diese Klassen aufgeteilt werden:

- [Seite](xref:Xamarin.Forms.Page)
- [Layout](xref:Xamarin.Forms.Layout)
- [Ansicht](xref:Xamarin.Forms.View)

Eine `Page` Ableitung nimmt den gesamten Bildschirm oder fast den gesamten Bildschirm ein. Häufig ist das untergeordnete Element einer Seite eine `Layout` Ableitung, um untergeordnete visuelle Elemente zu organisieren. Die untergeordneten Elemente des `Layout` können andere `Layout` Klassen oder `View` Ableitungen (häufig als *Elemente*bezeichnet) sein, die vertraute Objekte sind, wie z. b. Text, Bitmaps, Schieberegler, Schaltflächen, Listenfelder usw.

In diesem Kapitel wird veranschaulicht, wie Sie eine Anwendung erstellen, indem Sie sich auf den [`Label`](xref:Xamarin.Forms.Label)konzentrieren, der `View` Ableitung, die Text anzeigt.

## <a name="say-hello"></a>Grüßen

Mit der Xamarin-Plattform installiert wird können Sie eine neue Xamarin.Forms-Projektmappe in Visual Studio oder Visual Studio für Mac erstellen. Die " [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) "-Lösung verwendet eine portable Klassenbibliothek für den allgemeinen Code.

> [!NOTE]
> Portable Klassenbibliotheken wurden von .NET Standard-Bibliotheken ersetzt. Der Beispielcode aus dem Buch wurde zur Verwendung von .NET standard-Bibliotheken konvertiert.

Dieses Beispiel zeigt eine Xamarin.Forms-Projektmappe in Visual Studio erstellt werden, ohne Änderungen. Die Lösung besteht aus vier Projekten:

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), eine portable Klassenbibliothek (PCL), die von den anderen Projekten gemeinsam genutzt wird
- [**Hello. Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), ein Anwendungsprojekt für Android
- [**Hello. IOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), ein Anwendungsprojekt für IOS
- [**Hello. UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), ein Anwendungsprojekt für die universelle Windows-Plattform (Windows 10 und Windows 10 Mobile)

> [!NOTE]
> Xamarin.Forms nicht mehr unterstützt wird, Windows 8.1, Windows Phone 8.1 oder Windows 10 Mobile Xamarin.Forms-Anwendungen werden auf dem Windows 10-Desktop ausgeführt.

Sie können stellen diese Projekte für die Anwendung die Startup-Projekt, und klicken Sie dann erstellen und führen Sie das Programm auf einem Gerät oder Simulator.

In vielen Ihrer Xamarin.Forms-Programme wird nicht die Anwendungsprojekte ändern Sie sein. Diese verbleiben häufig winzige Stubs, um das Programm zu starten. Die meisten sich der Fokus werden die Bibliothek, die für alle Anwendungen sein.

## <a name="inside-the-files"></a>In den Dateien

Die visuellen Elemente, die vom **Hello** -Programm angezeigt werden, werden im Konstruktor der [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) -Klasse definiert. `App` von der xamarin. Forms-Klasse [`Application`](xref:Xamarin.Forms.Application)abgeleitet.

> [!NOTE]
> Die Visual Studio-Lösungsvorlagen für Xamarin.Forms wird eine Seite mit einer XAML-Datei erstellen. XAML wird in diesem Buch erst in [Kapitel 7](chapter07.md)behandelt.

Der Abschnitt " **Verweise** " des Projekts " **Hello** PCL" enthält die folgenden xamarin. Forms-Assemblys:

- **Xamarin. Forms. Core**
- **Xamarin. Forms. XAML**
- **Xamarin. Forms. Platform**

Die **Referenz** Abschnitte der fünf Anwendungsprojekte enthalten zusätzliche Assemblys, die für die einzelnen Plattformen gelten:

- **Xamarin. Forms. Platform. Android**
- **Xamarin. Forms. Platform. IOS**
- **Xamarin. Forms. Platform. UWP**
- **Xamarin. Forms. Platform. WinRT**
- **Xamarin. Forms. Platform. WinRT. Tablet**
- **Xamarin. Forms. Platform. WinRT. Phone**

> [!NOTE]
> In den **Verweis** Abschnitten dieser Projekte werden die Assemblys nicht mehr aufgelistet. Stattdessen enthält die Projektdatei ein **packagereferenzierungspaket** , das auf das xamarin. Forms-nuget-Paket verweist. Der Abschnitt " **Verweise** " in Visual Studio listet anstelle der xamarin. Forms-Assemblys das **xamarin. Forms** -Paket auf.

Jedes Anwendungsprojekt enthält einen aufzurufenden statischen `Forms.Init`-Methode im `Xamarin.Forms`-Namespace. Initialisiert die Xamarin.Forms-Bibliothek. Eine andere Version von `Forms.Init` wird für jede Plattform definiert. Die Aufrufe dieser Methode finden Sie in folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App`-Klasse, `OnLaunched`-Methode](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

Außerdem muss jede Plattform den Speicherort der `App` Klasse in der freigegebenen Bibliothek instanziieren. Dies erfolgt in einem `LoadApplication` in den folgenden Klassen:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

Andernfalls sind diese Anwendungsprojekte Programme für den normalen "nichts".

## <a name="pcl-or-sap"></a>PCL oder SAP?

Es ist möglich, eine Xamarin.Forms-Projektmappe mit der allgemeine Code in eine Portable Klassenbibliothek (PCL) oder eine freigegebene Asset-Projekt (SAP) zu erstellen. Um eine SAP-Lösung zu erstellen, wählen Sie die Shared-Option in Visual Studio. Die [**hellosap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) -Lösung veranschaulicht die SAP-Vorlage ohne Änderungen.

> [!NOTE]
> Portable Klassenbibliotheken wurde von .NET Standard-Bibliotheken ersetzt. Der Beispielcode aus dem Buch wurde zur Verwendung von .NET standard-Bibliotheken konvertiert. Andernfalls sind die PCL und .NET Standard-Bibliotheken vom Konzept her ähnlich.

Die Bibliothek Ansatz Bündel alle der allgemeinen code in einem Klassenbibliotheksprojekt, das auf die Anwendung Plattformprojekte verweist. Mit dem SAP-Ansatz wird der allgemeine Code effektiv in allen Projekten die Platform-Anwendung vorhanden ist, und von ihnen gemeinsam genutzt wird.

Xamarin.Forms-Entwickler, die meisten bevorzugen die Bibliothek-Ansatz. In diesem Buch verwenden die meisten Lösungen eine Bibliothek an. Solche, die SAP verwenden, enthalten im Projektnamen ein **SAP** -Suffix.

Mit dem SAP-Ansatz kann der Code im freigegebenen Projekt mithilfe C# von Präprozessordirektiven (`#if`, #`elif`und `#endif`) mit diesen vordefinierten bezeichlern unterschiedlichen Code für die verschiedenen Plattformen ausführen:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

In einer freigegebenen Bibliothek können Sie welche Plattform Sie zur Laufzeit ausführen auf bestimmen, wie Sie weiter unten in diesem Kapitel sehen werden.

## <a name="labels-for-text"></a>Bezeichnungen für text

Die [**Greetings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) -Lösung veranschaulicht, wie Sie dem C# **Greetings** -Projekt eine neue Datei hinzufügen. Diese Datei definiert eine Klasse mit dem Namen `GreetingsPage`, die von `ContentPage`abgeleitet ist. In diesem Buch enthalten die meisten Projekte eine einzelne `ContentPage` Ableitung, deren Name dem Namen des Projekts entspricht, dem das Suffix `Page` angehängt ist.

Der `GreetingsPage`-Konstruktor instanziiert eine [`Label`](xref:Xamarin.Forms.Label) Sicht, die die xamarin. Forms-Ansicht darstellt, in der Text angezeigt wird. Die [`Text`](xref:Xamarin.Forms.Label.Text) -Eigenschaft wird auf den Text festgelegt, der vom `Label`angezeigt wird. Mit diesem Programm wird der `Label` auf die `Content`-Eigenschaft von `ContentPage`festgelegt. Der Konstruktor der `App`-Klasse instanziiert `GreetingsPage` und legt ihn auf seine `MainPage`-Eigenschaft fest.

Der Text wird in der oberen linken Ecke der Seite angezeigt. Unter iOS bedeutet dies, dass sie die Seite der Statusleiste überlappt. Es gibt mehrere Lösungen für dieses Problem:

### <a name="solution-1-include-padding-on-the-page"></a>Lösung 1. Auffüllung, auf der Seite enthalten

Legen Sie eine [`Padding`](xref:Xamarin.Forms.Page.Padding) -Eigenschaft auf der Seite fest. `Padding` ist vom Typ [`Thickness`](xref:Xamarin.Forms.Thickness), eine Struktur mit vier Eigenschaften:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` definiert einen Bereich innerhalb einer Seite, in der Inhalt ausgeschlossen wird. Dadurch kann der `Label` das Überschreiben der IOS-Statusleiste vermeiden.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Lösung 2. Enthalten Sie Auffüllung nur für iOS (nur für SAP)

Festlegen Sie eine 'Abstand'-Eigenschaft wird nur unter iOS, die mithilfe eines von SAP mit einer C#-präprozessoranweisung. Dies wird in der [**greetingssap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) -Lösung veranschaulicht.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Lösung 3. Enthalten Sie Auffüllung nur für iOS (PCL oder SAP)

In der Version von xamarin. Forms, die für das Buch verwendet wird, kann eine `Padding` Eigenschaft, die für IOS in einer PCL oder SAP spezifisch ist, mithilfe der Methode [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) oder [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) static ausgewählt werden. Diese Methoden sind jetzt veraltet.

Die `Device.OnPlatform`-Methoden werden verwendet, um plattformspezifischen Code auszuführen oder plattformspezifische Werte auszuwählen. Intern verwenden Sie die [`Device.OS`](xref:Xamarin.Forms.Device.OS) statische schreibgeschützte Eigenschaft, die einen Member der [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform) Enumeration zurückgibt:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) für UWP-Geräte.

Die `Device.OnPlatform`-Methoden, die `Device.OS`-Eigenschaft und die `TargetPlatform`-Enumeration sind nun veraltet. Verwenden Sie stattdessen die [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) -Eigenschaft, und vergleichen Sie den `string` Rückgabewert mit den folgenden statischen Feldern:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), die Zeichenfolge "IOS"
- [`Android`](xref:Xamarin.Forms.Device.Android)die Zeichenfolge "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP), die Zeichenfolge "UWP", die auf den universelle Windows-Plattform verweist.

Die [`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom) statische schreibgeschützte Eigenschaft ist verknüpft. Dadurch wird ein Member des [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom)zurückgegeben, der über diese Member verfügt:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) wird nicht verwendet.

Für IOS und Android ist das Umstellungs zwischen `Tablet` und `Phone` eine Breite von 600 Einheiten. Für die Windows-Plattform gibt `Desktop` eine UWP-Anwendung an, die unter Windows 10 ausgeführt wird, und `Phone` eine UWP-Anwendung an, die unter Windows 10-Anwendung ausgeführt wird.

## <a name="solution-3a-set-margin-on-the-label"></a>Lösung 3a: Rand für die Bezeichnung setzen.

Die [`Margin`](xref:Xamarin.Forms.View.Margin) -Eigenschaft wurde zu spät eingeführt, um im Buch enthalten zu sein, aber Sie ist auch vom Typ `Thickness`. Sie können Sie auf dem `Label` festlegen, um einen Bereich außerhalb der Ansicht zu definieren, der in der Berechnung des Layouts der Ansicht enthalten ist.

Die `Padding`-Eigenschaft ist nur für [`Layout`](xref:Xamarin.Forms.Layout) -und [`Page`](xref:Xamarin.Forms.Page) -Ableitungen verfügbar. Die `Margin`-Eigenschaft ist für alle [`View`](xref:Xamarin.Forms.View) Ableitungen verfügbar.

## <a name="solution-4-center-the-label-within-the-page"></a>Lösung 4. Zentrieren Sie die Bezeichnung auf der Seite

Sie können den `Label` innerhalb des `Page` zentrieren (oder ihn an einer von acht anderen Stellen platzieren), indem Sie die [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften des `Label` auf einen Wert vom Typ [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)festlegen. Die `LayoutOptions`-Struktur definiert zwei Eigenschaften:

- Eine [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) -Eigenschaft vom Typ " [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment)", eine Enumeration mit vier Membern: [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), d. h. "Left" oder "Top", abhängig von der Ausrichtung, [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End), was in Abhängigkeit von der Ausrichtung rechts oder unten bedeutet, und [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill).

- Eine [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) -Eigenschaft vom Typ `bool`.

Diese Eigenschaften werden in der Regel nicht direkt verwendet. Stattdessen werden Kombinationen dieser beiden Eigenschaften durch acht statische, schreibgeschützte Eigenschaften vom Typ "`LayoutOptions`" bereitgestellt:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` und `VerticalOptions` sind die wichtigsten Eigenschaften im xamarin. Forms-Layout und werden ausführlicher in [**Kapitel 4 erläutert. Scrollen des Stapels**](chapter04.md).

Im folgenden ist das Ergebnis aufgeführt, bei dem die `HorizontalOptions`-und `VerticalOptions` Eigenschaften von `Label` beide auf `LayoutOptions.Center`festgelegt sind:

[![Dreifacher Screenshot des Greetings-Programms](images/ch02fg05-small.png "Horizontal und vertikal zentrierte Bezeichnung")](images/ch02fg05-large.png#lightbox "Horizontal und vertikal zentrierte Bezeichnung")

## <a name="solution-5-center-the-text-within-the-label"></a>Lösung 5. Der Text innerhalb der Bezeichnung zentrieren

Sie können den Text auch zentrieren (oder an acht anderen Positionen auf der Seite platzieren), indem Sie die [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) -und [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) Eigenschaften von `Label` auf einen Member der [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) -Enumeration festlegen:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), d.h. Links oder oben (abhängig von der Ausrichtung)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), Bedeutung von rechts oder unten (abhängig von der Ausrichtung)

Diese beiden Eigenschaften werden nur durch `Label`definiert, während die Eigenschaften `HorizontalAlignment` und `VerticalAlignment` durch `View` definiert und von allen `View` Ableitungen geerbt werden. Die besten Ergebnisse scheinen ähnlich, aber sie sind sehr verschieden, wie im nächste Kapitel zeigt.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 2 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Kapitel 2 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Kapitel 2 F# Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Einstieg in xamarin. Forms](~/get-started/index.yml)
