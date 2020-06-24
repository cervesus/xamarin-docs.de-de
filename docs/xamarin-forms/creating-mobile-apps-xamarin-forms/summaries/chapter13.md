---
title: Zusammenfassung von Kapitel 13. Bitmaps
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 13. Bitmaps'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 43caf088ad6cb816f049e7862a287c17839c2170
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136772"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Zusammenfassung von Kapitel 13. Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> In den Anmerkungen auf dieser Seite wird erläutert, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

Vom Xamarin.Forms-[`Image`](xref:Xamarin.Forms.Image)-Element wird eine Bitmap angezeigt. Alle Xamarin.Forms-Plattformen unterstützen die Dateiformate JPEG, PNG, GIF und BMP.

Bitmaps in Xamarin.Forms stammen aus vier Quellen:

- Aus dem Internet, angegeben durch eine URL.
- Eingebettet als Ressource in der freigegebenen Bibliothek.
- Eingebettet als Ressource in den Plattformanwendungsprojekten.
- Von jedem Ort, auf den über ein `Stream`-Objekt von .NET verwiesen werden kann, einschließlich `MemoryStream`

Bitmapressourcen in der freigegebenen Bibliothek sind plattformunabhängig, während Bitmapressourcen in den Plattformprojekten plattformspezifisch sind.

> [!NOTE]
> Im Text dieses Buchs wird auf portable Klassenbibliotheken verwiesen, die durch .NET Standard-Bibliotheken ersetzt wurden. Der gesamte Beispielcode innerhalb des Buchs wurde aktualisiert und verwendet jetzt die .NET Standard-Bibliotheken.

Die Angabe der Bitmap erfolgt durch Festlegen der [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft von `Image` auf ein Objekt vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource), eine abstrakte Klasse mit drei Ableitungen:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) für den Zugriff auf eine Bitmap über das Internet, basierend auf einem auf seine [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri)-Eigenschaft festgelegtes `Uri`-Objekt.
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) für den Zugriff auf eine in einem Plattformanwendungsprojekt gespeicherte Bitmap, basierend auf einem auf seine [`File`](xref:Xamarin.Forms.FileImageSource.File)-Eigenschaft festgelegten Ordner- und Dateipfad.
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) zum Laden einer Bitmap mithilfe eines `Stream`-Objekts von .NET, das durch Zurückgeben eines `Stream` von einer `Func`, die auf seine [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream)-Eigenschaft festgelegt wurde, angegeben wird.

Alternativ (und etwas gängiger) können Sie die folgenden statischen Methoden der `ImageSource`-Klasse verwenden, die alle `ImageSource`-Objekte zurückgeben:

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) für den Zugriff auf eine Bitmap über das Internet, basierend auf einem `Uri`-Objekt.
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) für den Zugriff auf eine Bitmap, die als eingebettete Ressource in der Anwendungs-PCL gespeichert ist; [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) oder [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) für den Zugriff auf eine Bitmap in einer anderen Quellassembly.
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) für den Zugriff auf eine Bitmap aus einem Plattformanwendungsprojekt.
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) zum Laden einer Bitmap, basierend auf einem `Stream`-Objekt.

Es gibt keine Klassenentsprechung für die `Image.FromResource`-Methoden. Die `UriImageSource`-Klasse ist nützlich, wenn Sie das Zwischenspeichern kontrollieren müssen. Die `FileImageSource`-Klasse ist nützlich in XAML. `StreamImageSource` ist nützlich für das asynchrone Laden von `Stream`-Objekten, während `ImageSource.FromStream` synchron ist.

## <a name="platform-independent-bitmaps"></a>Plattformunabhängige Bitmaps

Das [**WebBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode)-Projekt lädt eine Bitmap über das Internet mithilfe von `ImageSource.FromUri`. Das `Image`-Element wird auf die `Content`-Eigenschaft von `ContentPage` festgelegt, sodass es auf die Größe der Seite eingeschränkt ist. Unabhängig von der Größe der Bitmap wird ein eingeschränktes `Image`-Element auf die Größe seines Containers gestreckt, und die Bitmap wird in ihrer maximalen Größe innerhalb des `Image`-Elements angezeigt, wobei das Seitenverhältnis der Bitmap beibehalten wird. Bereiche des `Image`, die über die Bitmap hinausgehen, können mit [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) eingefärbt werden.

Das [**WebBitmapXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml)-Beispiel ist ähnlich, legt aber die `Source`-Eigenschaft einfach auf die URL fest. Die Konvertierung wird von der [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter)-Klasse durchgeführt.

### <a name="fit-and-fill"></a>Anpassung und Füllung

Sie können kontrollieren, wie die Bitmap gestreckt wird, indem Sie die [`Aspect`](xref:Xamarin.Forms.Image.Aspect)-Eigenschaft von `Image` auf einen der folgenden Member der [`Aspect`](xref:Xamarin.Forms.Aspect)-Enumeration festlegen:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): behält das Seitenverhältnis bei (Standard).
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): füllt den Bereich aus, ignoriert das Seitenverhältnis.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): füllt den Bereich aus, aber behält das Seitenverhältnis bei, was durch Zuschneiden eines Teils der Bitmap erreicht wird.

### <a name="embedded-resources"></a>Eingebettete Ressourcen

Sie können einer PCL oder einem Ordner in der PCL eine Bitmapdatei hinzufügen. Statten Sie sie mit dem **Buildvorgang** **EmbeddedResource** aus. Das [**ResourceBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode)-Beispiel veranschaulicht, wie Sie `ImageSource.FromResource` zum Laden der Datei verwenden. Der an die Methode übergebene Ressourcenname besteht aus dem Assemblynamen, gefolgt von einem Punkt, gefolgt vom optionalen Ordnernamen und einem Punkt, gefolgt vom Dateinamen.

Das Programm legt die Eigenschaften `VerticalOptions` und `HorizontalOptions` von `Image` auf `LayoutOptions.Center` fest, wodurch das `Image`-Element uneingeschränkt wird. Das `Image` und die Bitmap haben dieselbe Größe:

- Unter iOS und Android hat `Image` die Pixelgröße der Bitmap. Es findet eine 1:1-Zuordnung zwischen Bitmappixeln und Bildschirmpixeln statt.
- Bei der universellen Windows-Plattform hat `Image` die Pixelgröße der Bitmap in geräteunabhängigen Einheiten. Auf den meisten Geräten belegt jedes Bitmappixel mehrere Bildschirmpixel.

Im [**StackedBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap)-Beispiel wird ein `Image` in ein vertikales `StackLayout` in XAML gebracht. Eine Markuperweiterung namens [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) hilft dabei, auf die eingebettete Ressource in XAML zu verweisen. Diese Klasse lädt nur Ressourcen aus der Assembly, in der sie sich befindet, sodass sie nicht in einer Bibliothek abgelegt werden kann.

### <a name="more-on-sizing"></a>Weitere Informationen zur Größenanpassung

Häufig ist es wünschenswert, die Größe von Bitmaps konsistent über alle Plattformen hinweg anzupassen.
Durch Experimentieren mit **StackedBitmap** können Sie eine `WidthRequest` für das `Image`-Element in einem vertikalen `StackLayout` festlegen, um die Größe auf allen Plattformen konsistent zu halten, aber verringern können Sie die Größe nur mithilfe dieser Methode.

Sie können außerdem die `HeightRequest` festlegen, um die Bildgrößen auf den Plattformen konsistent zu halten, aber die eingeschränkte Breite der Bitmap schränkt die Vielseitigkeit dieser Methode ein. Bei einem Bild in einem vertikalen `StackLayout` sollte `HeightRequest` vermieden werden.

Der beste Ansatz besteht darin, mit einer Bitmap zu beginnen, die in geräteunabhängigen Einheiten breiter als das Telefon ist, und `WidthRequest` auf die gewünschte Breite in geräteunabhängigen Einheiten festzulegen. Dies wird im [**DeviceIndBitmapSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize)-Beispiel demonstriert.

Das Beispiel [**MadTeaParty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) zeigt Kapitel 7 von *Alice im Wunderland* von Lewis Carroll mit den Originalillustrationen von John Tenniel:

[![Dreifacher Screenshot der verrückten Teegesellschaft](images/ch13fg16-small.png "Buchtext der verrückten Teegesellschaft beim Hutmacher")](images/ch13fg16-large.png#lightbox "Buchtext der verrückten Teegesellschaft beim Hutmacher")

### <a name="browsing-and-waiting"></a>Durchsuchen und Warten

Das [**ImageBrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser)-Beispiel gestattet dem Benutzer das Durchsuchen von Archivbildern, die auf der Xamarin-Website gespeichert sind. Es verwendet die [`WebRequest`](xref:System.Net.WebRequest)-Klasse von .NET zum Herunterladen einer JSON-Datei mit der Liste der Bitmaps.

> [!NOTE]
> Für Xamarin.Forms-Programme sollte [`HttpClient`](xref:System.Net.Http.HttpClient) anstelle von [`WebRequest`](xref:System.Net.WebRequest) für den Zugriff auf Dateien über das Internet verwendet werden.

Das Programm verwendet einen [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator), um anzuzeigen, das etwas durchgeführt wird. Bei jedem Laden einer Bitmap hat die schreibgeschützte Eigenschaft [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) von `Image` den Wert `true`. Die `IsLoading`-Eigenschaft wird von einer bindbaren Eigenschaft unterstützt, sodass ein `PropertyChanged`-Ereignis ausgelöst wird, wenn sich diese Eigenschaft ändert. Das Programm fügt einen Handler an dieses Ereignis an und verwendet die aktuelle Einstellung von `IsLoaded`, um die [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)-Eigenschaft des `ActivityIndicator` festzulegen.

## <a name="streaming-bitmaps"></a>Streaming von Bitmaps

Die `ImageSource.FromStream`-Methode erstellt auf Grundlage eines `Stream` von .NET eine `ImageSource`. An die Methode muss ein `Func`-Objekt übergeben werden, das ein `Stream`-Objekt zurückgibt.

### <a name="accessing-the-streams"></a>Zugreifen auf die Streams

Das [**BitmapStreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams)-Beispiel veranschaulicht, wie Sie die `ImaageSource.FromStream`-Methode verwenden, um eine Bitmap zu laden, die als eingebettete Ressource gespeichert ist, und um eine Bitmap über das Internet zu laden.

### <a name="generating-bitmaps-at-run-time"></a>Generieren von Bitmaps zur Laufzeit

Alle Xamarin.Forms-Plattformen unterstützen das unkomprimierte BMP-Dateiformat, das sich in Code einfach erstellen und dann in `MemoryStream` speichern lässt. Diese Methode ermöglicht das algorithmische Erstellen von Bitmaps zur Laufzeit, wie in der [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)-Klasse in der **Xamrin.FormsBook.Toolkit**-Bibliothek implementiert.

Das „Do-it-yourself“-Beispiel [**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) veranschaulicht die Verwendung von `BmpMaker` zum Erstellen einer Bitmap mit einem Farbverlaufsbild.

## <a name="platform-specific-bitmaps"></a>Plattformspezifische Bitmaps

Alle Xamarin.Forms-Plattformen erlauben das Speichern von Bitmaps in den Plattformanwendungsassemblys. Wenn sie von einer Xamarin.Forms-Anwendung abgerufen werden, sind diese Plattformbitmaps vom Typ [`FileImageSource`](xref:Xamarin.Forms.FileImageSource). Sie verwenden sie für Folgendes:

- die [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)-Eigenschaft von [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- die [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)-Eigenschaft von [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- die [`Image`](xref:Xamarin.Forms.Button)-Eigenschaft von `Button`

Die Plattformassemblys enthalten bereits Bitmaps für Symbole und Begrüßungsbildschirme:

- im iOS-Projekt im Ordner **Resources** (Ressourcen).
- im Android-Projekt in den Unterordnern des Ordners **Resources** (Ressourcen).
- in den Windows-Projekten im Ordner **Assets** (Ressourcen; obgleich die Windows-Plattformen Bitmaps nicht nur auf diesen Ordner beschränken).

Das [**PlatformBitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps)-Beispiel verwendet Code zum Anzeigen eines Symbols aus den Plattformanwendungsprojekten.

### <a name="bitmap-resolutions"></a>Bitmapauflösungen

Alle Plattformen gestatten das Speichern mehrerer Versionen von Bitmapbildern für unterschiedliche Geräteauflösungen. Zur Laufzeit wird die richtige Version geladen, basierend auf der Geräteauflösung des Bildschirms.

Unter iOS werden diese Bitmaps durch ein Suffix am Dateinamen unterschieden:

- Kein Suffix bei 160-DPI-Geräten (1 Pixel pro geräteunabhängige Einheit)
- Suffix „@2x“ bei 320-DPI-Geräten (2 Pixel pro geräteunabhängige Einheit)
- Suffix „@3x“ bei 480-DPI-Geräten (3 Pixel pro geräteunabhängige Einheit)

Eine für die Anzeige als Quadrat mit Seitenlänge 2,54 cm gedachte Bitmap gäbe es in drei Versionen:

- „MyImage.jpg“ mit 160 Pixel im Quadrat
- „MyImage@2x.jpg“ mit 320 Pixel im Quadrat
- „MyImage@3x.jpg“ mit 480 Pixel im Quadrat

Das Programm würde auf diese Bitmap als „MyImage.jpg“ verweisen, doch die richtige Version wird zur Laufzeit abgerufen, basierend auf der Auflösung des Bildschirms. Wenn die Bitmap uneingeschränkt ist, wird sie immer mit 160 geräteunabhängigen Einheiten gerendert.

Unter Android werden Bitmaps in verschiedenen Unterordnern des Ordners **Ressourcen** gespeichert:

- „drawable-ldpi“ (niedrige DPI) bei 120-DPI-Geräten (0,75 Pixel pro geräteunabhängige Einheit)
- „drawable-mdpi“ (mittel) bei 160-DPI-Geräten (1 Pixel pro geräteunabhängige Einheit)
- „drawable-hdpi“ (hoch) bei 240-DPI-Geräten (1,5 Pixel pro geräteunabhängige Einheit)
- „drawable-xhdpi“ (sehr hoch) bei 320-DPI-Geräten (2 Pixel pro geräteunabhängige Einheit)
- „drawable-xxhdpi“ (besonders hoch) bei 480-DPI-Geräten (3 Pixel pro geräteunabhängige Einheit)
- „drawable-xxxhdpi“ (extrem hoch) bei 640-DPI-Geräten (4 Pixel pro geräteunabhängige Einheit)

Bei einer für das Rendern als Quadrat mit Seitenlänge 2,54 cm gedachten Bitmap besitzen die verschiedenen Versionen der Bitmap denselben Namen, aber eine andere Größe, und werden in diesen Ordnern gespeichert:

- „drawable-ldpi/MyImage.jpg“ mit 120 Pixel im Quadrat
- „drawable-mdpi/MyImage.jpg“ mit 160 Pixel im Quadrat
- „drawable-hdpi/MyImage.jpg“ mit 240 Pixel im Quadrat
- „drawable-xhdpi/MyImage.jpg“ mit 320 Pixel im Quadrat
- „drawable-xxhdpi/MyImage.jpg“ mit 480 Pixel im Quadrat
- „drawable-xxxhdpi/MyImage.jpg“ mit 640 Pixel im Quadrat

Die Bitmap wird immer mit 160 geräteunabhängigen Einheiten gerendert. (Die standardmäßige Xamarin.Forms-Projektmappenvorlage umfasst nur die Ordner „hdpi“, „xhdpi“ und „xxhdpi“.)

Das UWP-Projekt unterstützt ein Benennungsschema für Bitmaps, das aus einem Skalierungsfaktor in Pixel pro geräteunabhängiger Einheit als Prozentsatz besteht, z. B.:

- „MyImage.scale-200.jpg“ mit 320 Pixel im Quadrat

Nur manche Prozentsätze sind gültig. Die Beispielprogramme für dieses Buch enthalten nur Bilder mit **scale-200**-Suffixen, aber aktuelle Xamarin.Forms-Projektmappenvorlagen enthalten **scale-100**, **scale-125**, **scale-150** und **scale-400**.

Beim Hinzufügen von Bitmaps zu den Plattformprojekten sollte der **Erstellungsvorgang** der folgende sein:

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **Inhalt**

Im [**ImageTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap)-Beispiel werden zwei schaltflächenähnliche Objekte erstellt, die aus `Image`-Elementen mit installiertem `TapGestureRecognizer` bestehen. Die Objekte sollen die Größe eines Quadrats mit Seitenlänge 2,54 cm haben. Die `Source`-Eigenschaft von `Image` wird mithilfe der Objekte `OnPlatform` und `On` so festgelegt, dass sie potenziell auf unterschiedliche Dateinamen auf den Plattformen verweist. Die Bitmapbilder enthalten Nummern, die ihre Pixelgröße anzeigen, sodass Sie erkennen können, welche Bitmapgröße abgerufen und gerendert wird.

### <a name="toolbars-and-their-icons"></a>Symbolleisten und ihre Symbole

Einer der primären Verwendungszwecke plattformspezifischer Bitmaps ist die Xamarin.Forms-Symbolleiste, die erstellt wird, indem [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)-Objekte der von `Page` definierten [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems)-Sammlung hinzugefügt werden. `ToobarItem` wird von [`MenuItem`](xref:Xamarin.Forms.MenuItem) abgeleitet, von dem es einige Eigenschaften erbt.

Die wichtigsten `ToolbarItem`-Eigenschaften sind:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) für Text, der abhängig von Plattform und `Order` möglicherweise angezeigt wird.
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) vom Typ `FileImageSource` für das Bild, das abhängig von Plattform und `Order` möglicherweise angezeigt wird.
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) vom Typ [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder), eine Enumeration mit drei Zahlen, [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default), [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary) und [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Die Anzahl der `Primary`-Elemente sollte auf drei oder vier beschränkt werden. Sie sollten eine `Text`-Einstellung für alle Elemente einschließen. Bei den meisten Plattformen erfordern nur die `Primary`-Elemente ein `Icon`, doch Windows 8.1 erfordert für alle Elemente ein `Icon`. Die Symbole sollten 32 geräteunabhängige Einheiten im Quadrat sein. Der `FileImageSource`-Typ gibt an, dass sie plattformspezifisch sind.

Das `ToolbarItem` löst ein [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked)-Ereignis aus, wenn darauf getippt wird, ganz ähnlich wie eine `Button`. `ToolbarItem` unterstützt auch die Eigenschaften [`Command`](xref:Xamarin.Forms.MenuItem.Command) und [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter), die häufig in Verbindung mit MVVM verwendet werden. (Siehe [Kapitel 18, „MVVM“](chapter18.md).)

Sowohl iOS als auch Android erfordern, dass eine Seite, auf der eine Symbolleiste angezeigt wird, eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ist oder eine Seite, auf die man von einer `NavigationPage` weitergeleitet wurde. Das [**ToolbarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo)-Programm legt die `MainPage`-Eigenschaft seiner `App`-Klasse auf den [`NavigationPage`-Konstructor](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) mit einem `ContentPage`-Argument fest und veranschaulicht den Konstruktions- und Ereignishandler einer Symbolleiste.

### <a name="button-images"></a>Schaltflächenbilder

Sie können auch plattformspezifische Bitmaps verwenden, um die [`Image`](xref:Xamarin.Forms.Button.Image)-Eigenschaft von `Button` auf eine Bitmap mit 32 geräteunabhängigen Einheiten im Quadrat festzulegen, wie im [**ButtonImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage)-Beispiel veranschaulicht.

> [!NOTE]
> Die Verwendung von Bildern auf Schaltflächen wurde verbessert. Siehe [Verwenden von Bitmaps mit Schaltflächen](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).

## <a name="related-links"></a>Verwandte Links

- [Kapitel 13 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Kapitel 13 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Working with Images (Arbeiten mit Bildern)](~/xamarin-forms/user-interface/images.md)
- [Verwenden von Bitmaps mit Schaltflächen](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
