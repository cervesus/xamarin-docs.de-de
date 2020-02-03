---
title: Zusammenfassung der Kapitel 13. Bitmaps
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 13. Bitmaps'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e4746ed94a008d382ce15bb9cd7c52365d9ba574
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725528"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Zusammenfassung der Kapitel 13. Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Das xamarin. Forms [`Image`](xref:Xamarin.Forms.Image) -Element zeigt eine Bitmap an. Alle Xamarin.Forms-Plattformen unterstützen die JPEG, PNG, GIF und BMP-Dateiformate.

Bitmaps in Xamarin.Forms stammen aus vier Stellen:

- Über das Internet durch eine URL angegeben.
- Als Ressource in der freigegebenen Bibliothek eingebettet
- Als Ressource eingebettet ist, in die Plattform-Anwendungsprojekten
- Von überall aus, auf die von einem .net `Stream`-Objekt verwiesen werden kann, einschließlich `MemoryStream`

Bitmapressourcen in der freigegebenen Bibliothek sind plattformunabhängig, während Bitmapressourcen in die Plattformprojekte plattformspezifisch sind.

> [!NOTE]
> Der Text des Buchs stellt Verweise auf Portable Class Libraries, die von .NET Standard-Bibliotheken ersetzt wurden. Der Beispielcode aus dem Buch wurde zur Verwendung von .NET standard-Bibliotheken konvertiert.

Die Bitmap wird angegeben, indem die [`Source`](xref:Xamarin.Forms.Image.Source) -Eigenschaft von `Image` auf ein Objekt vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource), eine abstrakte Klasse mit drei Ableitungen, festgelegt wird:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) für den Zugriff auf eine Bitmap über das Web basierend auf einem `Uri` Objekt, das auf seine [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri) -Eigenschaft festgelegt ist.
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) für den Zugriff auf eine Bitmap, die in einem Platt Form Anwendungsprojekt gespeichert ist, basierend auf einem Ordner und einem Dateipfad, der auf seine [`File`](xref:Xamarin.Forms.FileImageSource.File)
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) zum Laden einer Bitmap mithilfe eines .net-`Stream` Objekts, das durch Zurückgeben eines `Stream` von einem `Func` auf seine [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream) -Eigenschaft festgelegt wurde.

Alternativ dazu können Sie die folgenden statischen Methoden der `ImageSource`-Klasse verwenden, die alle `ImageSource` Objekte zurückgeben:

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) für den Zugriff auf eine Bitmap über das Web basierend auf einem `Uri`-Objekt
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) für den Zugriff auf eine Bitmap, die als eingebettete Ressource in der Anwendung PCL gespeichert ist; [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) oder [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) , um auf eine Bitmap in einer anderen Quellassembly zuzugreifen.
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) für den Zugriff auf eine Bitmap aus einem Platt Form Anwendungsprojekt
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) zum Laden einer Bitmap basierend auf einem `Stream` Objekt

Es gibt keine Klassen Entsprechung der `Image.FromResource` Methoden. Die `UriImageSource`-Klasse ist nützlich, wenn Sie die Zwischenspeicherung steuern müssen. Die `FileImageSource`-Klasse ist in XAML nützlich. `StreamImageSource` ist nützlich für das asynchrone Laden von `Stream` Objekten, während `ImageSource.FromStream` synchron ist.

## <a name="platform-independent-bitmaps"></a>Plattformunabhängiger Bitmaps

Das [**webbitmapcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) -Projekt lädt mithilfe `ImageSource.FromUri`eine Bitmap über das Web. Das `Image`-Element wird auf die `Content`-Eigenschaft der `ContentPage`festgelegt, sodass es auf die Größe der Seite beschränkt ist. Unabhängig von der Größe der Bitmap wird ein eingeschränktes `Image` Element auf die Größe des Containers gestreckt, und die Bitmap wird in Ihrer maximalen Größe innerhalb des `Image` Elements angezeigt, während das Seitenverhältnis der Bitmap beibehalten wird. Bereiche der `Image` hinter der Bitmap können mit [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)gefärbt werden.

Das [**webbitmapxaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) -Beispiel ist ähnlich, jedoch wird die `Source`-Eigenschaft einfach auf die URL festgelegt. Die Konvertierung wird von der [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter) -Klasse durchgeführt.

### <a name="fit-and-fill"></a>Anpassen und Füllung

Sie können steuern, wie die Bitmap gestreckt wird, indem Sie die [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -Eigenschaft des `Image` auf einen der folgenden Member der [`Aspect`](xref:Xamarin.Forms.Aspect) -Enumeration festlegen:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): Respekt Seitenverhältnis (Standard)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): füllt den Bereich, berücksichtigt nicht das Seitenverhältnis
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): füllt den Bereich aus, berücksichtigt jedoch das Seitenverhältnis, erreicht durchzuschneiden eines Teils der Bitmap.

### <a name="embedded-resources"></a>Eingebettete Ressourcen

Sie können eine Bitmap-Datei in eine PCL oder in einen Ordner in der PCL hinzufügen. **Erstellen** Sie eine **Buildaktion von EmbeddedResource**. Das [**resourcebitmapcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) -Beispiel veranschaulicht, wie `ImageSource.FromResource` zum Laden der Datei verwendet wird. Der Ressourcenname, der an die Methode übergebene besteht aus den Assemblynamen, gefolgt von einem Punkt, gefolgt vom Namen der Ordner "optional" und einen Punkt, gefolgt von den Dateinamen.

Das Programm legt die Eigenschaften `VerticalOptions` und `HorizontalOptions` der `Image` auf `LayoutOptions.Center`fest, wodurch das `Image` Element nicht eingeschränkt wird. Die `Image` und die Bitmap haben dieselbe Größe:

- Unter IOS und Android ist die `Image` die Pixelgröße der Bitmap. Es gibt eine 1: 1-Zuordnung zwischen Bitmap und Bildschirmpixel.
- Auf universelle Windows-Plattform ist die `Image` die Pixelgröße der Bitmap in geräteunabhängigen Einheiten. Auf den meisten Geräten nimmt jedes Pixel der Bitmap mehrere Bildschirmpixel.

Das [**stackedbitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) -Beispiel versetzt eine `Image` in einem vertikalen `StackLayout` in XAML. Eine Markup Erweiterung mit dem Namen [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) unterstützt den Verweis auf die eingebettete Ressource in XAML. Diese Klasse lädt nur die Ressourcen aus der Assembly, in dem sie die befindet, damit es in einer Bibliothek platziert werden kann.

### <a name="more-on-sizing"></a>Weitere Informationen zur größenanpassung

Es ist häufig wünschenswert, Größe Bitmaps einheitlich für alle Plattformen.
Beim Experimentieren mit **stackedbitmap**können Sie eine `WidthRequest` für das `Image`-Element in einem vertikalen `StackLayout` festlegen, um die Größe zwischen den Plattformen konsistent zu machen, aber Sie können die Größe nur mithilfe dieser Technik verringern.

Sie können auch die `HeightRequest` festlegen, um die Bildgrößen auf den Plattformen konsistent zu machen, aber die eingeschränkte Breite der Bitmap schränkt die Flexibilität dieser Technik ein. Bei einem Bild in einem vertikalen `StackLayout`sollten `HeightRequest` vermieden werden.

Der beste Ansatz besteht darin, mit einer Bitmap zu beginnen, die in geräteunabhängigen Einheiten größer als die Telefon Breite ist, und `WidthRequest` in geräteunabhängigen Einheiten auf eine gewünschte Breite festzulegen. Dies wird im Beispiel "Debug- [**bitmapsize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) " veranschaulicht.

Die [**madteaparty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) zeigt Kapitel 7 der " *Alice's Adventures* " von Lewis Carroll in Wonderland mit den Original Abbildungen von John Tenniel an:

[![Dreifacher Screenshot der Mad-Tea-Party](images/ch13fg16-small.png "Mad Hatters Tea Party Book Text")](images/ch13fg16-large.png#lightbox "Mad Hatters Tea Party Book Text")

### <a name="browsing-and-waiting"></a>Durchsuchen, und warten

Das [**ImageBrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) -Beispiel ermöglicht dem Benutzer das Durchsuchen von Aktien Bildern, die auf der xamarin-Website gespeichert sind. Er verwendet die .net [`WebRequest`](xref:System.Net.WebRequest) -Klasse, um eine JSON-Datei mit der Liste der Bitmaps herunterzuladen.

> [!NOTE]
> Xamarin. Forms-Programme sollten [`HttpClient`](xref:System.Net.Http.HttpClient) anstelle von [`WebRequest`](xref:System.Net.WebRequest) für den Zugriff auf Dateien über das Internet verwenden.

Das Programm verwendet eine [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) , um anzugeben, dass etwas passiert. Beim Laden der einzelnen Bitmap wird die schreibgeschützte [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) -Eigenschaft `Image` `true`. Die `IsLoading`-Eigenschaft wird durch eine bindbare Eigenschaft gestützt, sodass ein `PropertyChanged` Ereignis ausgelöst wird, wenn sich die Eigenschaft ändert. Das Programm fügt einen Handler an dieses Ereignis an und verwendet die aktuelle Einstellung von `IsLoaded`, um die [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) -Eigenschaft der `ActivityIndicator`festzulegen.

## <a name="streaming-bitmaps"></a>Streaming von bitmaps

Die `ImageSource.FromStream`-Methode erstellt eine `ImageSource`, die auf einem .net-`Stream`basiert. Der Methode muss ein `Func` Objekt, das ein `Stream`-Objekt zurückgibt, übermittelt werden.

### <a name="accessing-the-streams"></a>Zugreifen auf die Datenströme

Das [**bitmapstreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) -Beispiel veranschaulicht, wie Sie die `ImaageSource.FromStream`-Methode verwenden, um eine Bitmap zu laden, die als eingebettete Ressource gespeichert ist, und um eine Bitmap über das Web zu laden.

### <a name="generating-bitmaps-at-run-time"></a>Erzeugen von Bitmaps zur Laufzeit

Alle xamarin. Forms-Plattformen unterstützen das nicht komprimierte BMP-Dateiformat, das einfach im Code konstruiert und dann in einem `MemoryStream`gespeichert werden kann. Diese Technik ermöglicht das algorithmisch Erstellen von Bitmaps zur Laufzeit, wie in der [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) -Klasse in der **xamrin. formsbook. Toolkit** -Bibliothek implementiert.

Das "Do it yourself"-Beispiel " [**diygradientbitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) " veranschaulicht die Verwendung von `BmpMaker` zum Erstellen einer Bitmap mit einem Farbverlaufs Bild.

## <a name="platform-specific-bitmaps"></a>Clientplattform-spezifische bitmaps

Alle Xamarin.Forms-Plattformen ermöglichen das Speichern von Bitmaps in den Assemblys der Plattform-Anwendung. Wenn Sie von einer xamarin. Forms-Anwendung abgerufen werden, sind diese Platt Form Bitmaps vom Typ [`FileImageSource`](xref:Xamarin.Forms.FileImageSource). Verwenden Sie diese für:

- die [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) -Eigenschaft von [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- die [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) -Eigenschaft von [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- die [`Image`](xref:Xamarin.Forms.Button) -Eigenschaft von `Button`

Die Testplattform-Assemblys enthalten bereits die Bitmaps für Symbole und Begrüßungsbildschirme:

- Im IOS-Projekt im Ordner " **Resources** "
- Im Android-Projekt in den Unterordnern des Ordners " **Resources** "
- In den Windows-Projekten im Ordner " **Assets** " (obwohl die Windows-Plattformen Bitmaps nicht auf diesen Ordner beschränken)

Das [**platformbitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) -Beispiel verwendet Code, um ein Symbol aus den Platt Form Anwendungsprojekten anzuzeigen.

### <a name="bitmap-resolutions"></a>Bitmap-Lösungen

Alle Plattformen ermöglichen die Speicherung mehrerer Versionen von Bitmapbildern Auflösung von verschiedenen Geräten. Zur Laufzeit wird die richtige Version basierend auf der Auflösung des Bildschirms für dieses Gerät geladen.

Diese Bitmaps unterscheiden sich unter iOS die durch das Suffix auf den Dateinamen:

- Kein Suffix für 160 DPI-Geräte (1-Pixel auf geräteunabhängige Einheit)
- Suffix "@2x" für 320 dpi-Geräte (2 Pixel bis zur Diu)
- Suffix "@3x" für 480 dpi-Geräte (3 Pixel zur Diu)

Eine Bitmap, als 1 Zoll Quadrat angezeigt werden soll, würden in drei Versionen vorhanden sein:

- "MeinBild.jpg" auf 160 Pixel im Quadrat
- MyImage@2x.jpg um 320 Pixel Quadrat
- MyImage@3x.jpg um 480 Pixel Quadrat

Das Programm würde in diese Bitmap als "MeinBild.jpg" verweisen, aber die richtige Version zur Laufzeit auf Grundlage der Auflösung des Bildschirms abgerufen. Wenn uneingeschränkt ist, wird die Bitmap immer auf 160 geräteunabhängige Einheiten gerendert.

Für Android werden Bitmaps in verschiedenen Unterordnern des Ordners " **Resources** " gespeichert:

- drawable-Ldpi (niedrige DPI) für 120 DPI-Geräte (0,75 Pixel, um die DIU)
- drawable-Mdpi (Mittel) für 160 DPI-Geräte (1-Pixel auf den DIU)
- drawable-Hdpi (hoch) für 240 DPI-Geräte (1,5 Pixel, um die DIU)
- drawable-Xhdpi (sehr hoch) für 320 DPI-Geräte (2 Pixel, um die DIU)
- drawable-Xxhdpi (zusätzliche sehr hoch) für 480 DPI-Geräte (3 Pixel, um die DIU)
- drawable-Xxxhdpi (drei zusätzliche Höchstwerte) für 640 DPI-Geräte (4 Pixel, um die DIU)

Für eine Bitmap mit einer quadratischen Zoll gerendert werden sollen die verschiedene Versionen der Bitmap hat den gleichen Namen, aber eine andere Größe, und in diesen Ordnern gespeichert werden:

- drawable-Ldpi / "MeinBild.jpg" auf 120 Pixel im Quadrat
- drawable-Mdpi / "MeinBild.jpg" auf 160 Pixel im Quadrat
- drawable-Hdpi / "MeinBild.jpg" auf 240 Pixel im Quadrat
- drawable-Xhdpi / "MeinBild.jpg" am 320 Pixel im Quadrat
- drawable-Xxhdpi / "MeinBild.jpg" auf 480 Pixel im Quadrat
- drawable-Xxxhdpi / "MeinBild.jpg" auf 640 Pixel im Quadrat

Die Bitmap wird immer auf 160 geräteunabhängige Einheiten gerendert. (Die Standardvorlage der Xamarin.Forms-Projektmappe enthält nur die Hdpi Xhdpi und Xxhdpi-Ordner.)

Das UWP-Projekt unterstützt ein Bitmap-Benennungsschema, das ein Skalierungsfaktor in Pixel pro geräteunabhängige Einheit als Prozentwert, z. B. besteht:

- MyImage.scale-200.jpg am 320 Pixel im Quadrat

Nur einige Prozentsätze sind gültig. Die Beispiel Programme für dieses Buch enthalten nur Bilder mit den Suffixen " **Scale-200** ", aber aktuelle xamarin. Forms-Lösungs Vorlagen umfassen " **Scale-100**", " **Scale-125**", " **Scale-150**" und " **Scale-400**".

Beim Hinzufügen von Bitmaps zu den Platt Form Projekten sollte die **Buildaktion** wie folgt lauten:

- iOS: **bundleresource**
- Android: **androidresource**
- UWP: **Inhalt**

Das [**imagetap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) -Beispiel erstellt zwei Schaltflächen ähnliche Objekte, die aus `Image` Elementen mit installiertem `TapGestureRecognizer` bestehen. Es richtet sich an, dass die Objekte einer-Zoll-quadratisch sein. Die `Source`-Eigenschaft von `Image` wird mithilfe `OnPlatform` und `On` Objekten festgelegt, um auf möglicherweise unterschiedliche Dateinamen auf den Plattformen zu verweisen. Die Bitmap-Images enthalten die Nummer ihrer Pixelgröße gibt, damit Sie sehen, welche Größe Bitmap abgerufen und gerendert wird.

### <a name="toolbars-and-their-icons"></a>Symbolleisten und die Symbole

Eine der wichtigsten Verwendungsmöglichkeiten von plattformspezifischen Bitmaps ist die xamarin. Forms-Symbolleiste, die erstellt wird, indem [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) Objekte der von `Page`definierten [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems) Auflistung hinzugefügt werden. `ToobarItem` von [`MenuItem`](xref:Xamarin.Forms.MenuItem) abgeleitet, von dem Sie einige Eigenschaften erbt.

Die wichtigsten `ToolbarItem` Eigenschaften sind:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) für Text, der abhängig von Plattform und `Order` angezeigt werden kann
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) vom Typ `FileImageSource` für das Image, das je nach Plattform und `Order` angezeigt werden kann
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) vom Typ [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder), eine Enumeration mit drei Membern, [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default), [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary)und [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Die Anzahl der `Primary` Elemente sollte auf drei oder vier beschränkt sein. Sie sollten eine `Text` Einstellung für alle Elemente einschließen. Für die meisten Plattformen benötigen nur die `Primary` Elemente eine `Icon`, Windows 8.1 erfordert jedoch eine `Icon` für alle Elemente. Die Symbole sollten 32 geräteunabhängige Einheiten Quadrat. Der `FileImageSource`-Typ gibt an, dass Sie plattformspezifisch sind.

Der `ToolbarItem` löst beim Tippen ein [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) Ereignis aus, ähnlich wie ein `Button`. `ToolbarItem` unterstützt auch [`Command`](xref:Xamarin.Forms.MenuItem.Command) und [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) Eigenschaften, die häufig in Verbindung mit MVVM verwendet werden. (Weitere Informationen finden Sie in [Kapitel 18, MVVM](chapter18.md)).

Sowohl für IOS als auch für Android ist es erforderlich, dass eine Seite, auf der eine Symbolleiste angezeigt wird, eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) oder eine Seite ist, zu der von `NavigationPage`einem Das [**toolbardemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) -Programm legt die `MainPage`-Eigenschaft seiner `App`-Klasse mit einem `ContentPage`-Argument auf den [`NavigationPage`-Konstruktor](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) fest und veranschaulicht die Konstruktion und den Ereignishandler einer Symbolleiste.

### <a name="button-images"></a>Schaltfläche "-images

Sie können auch plattformspezifische Bitmaps verwenden, um die [`Image`](xref:Xamarin.Forms.Button.Image) -Eigenschaft `Button` auf eine Bitmap mit 32 geräteunabhängigen Einheiten Square festzulegen, wie im [**Button Sample image**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) veranschaulicht.

> [!NOTE]
> Die Verwendung von Bildern auf Schaltflächen wurde verbessert. Siehe [Verwenden von Bitmaps mit Schalt](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)Flächen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 13 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Kapitel 13 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Working with Images (Arbeiten mit Bildern)](~/xamarin-forms/user-interface/images.md)
- [Verwenden von Bitmaps mit Schaltflächen](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
