---
title: Zusammenfassung der Kapitel 13. Bitmaps
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 13. Bitmaps'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 737e242e14778f38405845541b2ca30d27c3cf5a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334622"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Zusammenfassung der Kapitel 13. Bitmaps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE] 
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Die Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) Element wird in eine Bitmap. Alle Xamarin.Forms-Plattformen unterstützen die JPEG, PNG, GIF und BMP-Dateiformate.

Bitmaps in Xamarin.Forms stammen aus vier Stellen:

- Über das Internet durch eine URL angegeben.
- Als Ressource in der freigegebenen Bibliothek eingebettet
- Als Ressource eingebettet ist, in die Plattform-Anwendungsprojekten
- Von jedem beliebigen Standort, die von einer .NET verwiesen werden kann `Stream` Objekts, einschließlich `MemoryStream`

Bitmapressourcen in der freigegebenen Bibliothek sind plattformunabhängig, während Bitmapressourcen in die Plattformprojekte plattformspezifisch sind.

> [!NOTE] 
> Der Text des Buchs stellt Verweise auf Portable Class Libraries, die von .NET Standard-Bibliotheken ersetzt wurden. Der Beispielcode aus dem Buch wurde zur Verwendung von .NET standard-Bibliotheken konvertiert.

Die Bitmap wird angegeben, indem die [ `Source` ](xref:Xamarin.Forms.Image.Source) Eigenschaft `Image` auf ein Objekt des Typs [ `ImageSource` ](xref:Xamarin.Forms.ImageSource), eine abstrakte Klasse mit drei ableitungen:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) für den Zugriff auf eine Bitmap, über das Internet auf der Grundlage einer `Uri` -Objekts festgelegt, um die [ `Uri` ](xref:Xamarin.Forms.UriImageSource.Uri) Eigenschaft
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) für den Zugriff auf eine Bitmap, die in einem plattformprojekt gespeichert basierend auf einem Ordner und Pfad, legen Sie auf die [ `File` ](xref:Xamarin.Forms.FileImageSource.File) Eigenschaft
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) für das Laden einer Bitmap mit einer .NET-Konsolenanwendung `Stream` Objekt angegeben wird durch Zurückgeben einer `Stream` aus eine `Func` legen Sie auf die [ `Stream` ](xref:Xamarin.Forms.StreamImageSource.Stream) Eigenschaft

Alternativ (und häufiger) können Sie die folgenden statischen Methoden von der `ImageSource` Klasse, die alle zurückgeben `ImageSource` Objekte:

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) für den Zugriff auf eine Bitmap, über das Internet auf der Grundlage einer `Uri` Objekt
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) für den Zugriff auf eine Bitmap, die als eingebettete Ressource in der PCL-Anwendung gespeichert; [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) oder [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) auf einer Bitmap in einer anderen Assembly der Ereignisquelle
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) für den Zugriff auf eine Bitmap aus einem Projekt der Plattform-Anwendung
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) für das Laden einer Bitmap, die auf der Grundlage einer `Stream` Objekt

Es gibt keine Entsprechung Klasse von der `Image.FromResource` Methoden. Die `UriImageSource` Klasse ist hilfreich, wenn Sie caching steuern müssen. Die `FileImageSource` Klasse eignet sich für XAML. `StreamImageSource` eignet sich für das asynchrone Laden `Stream` Objekte aufweist, während `ImageSource.FromStream` ist synchron.

## <a name="platform-independent-bitmaps"></a>Plattformunabhängiger Bitmaps

Die [ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) Projekt lädt eine Bitmap, über das Web mit `ImageSource.FromUri`. Die `Image` -Elements auf festgelegt die `Content` Eigenschaft der `ContentPage`, sodass es auf die Größe der Seite begrenzt ist. Unabhängig von der die Bitmap-Größe, die eine eingeschränkte `Image` Element wird gestreckt, um die Größe des Containers, und die Bitmap wird angezeigt, in die maximale Größe innerhalb der `Image` Element unter Beibehaltung der Bitmap des Seitenverhältnisses. Bereiche der `Image` darüber hinaus die Bitmap mit gefärbt werden kann [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor).

Die [ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) Beispiel ist ähnlich, aber einfach legt die `Source` -Eigenschaft auf die URL. Die Konvertierung erfolgt durch die [ `ImageSourceConverter` ](xref:Xamarin.Forms.ImageSourceConverter) Klasse.

### <a name="fit-and-fill"></a>Anpassen und Füllung

Können Sie steuern, wie die Bitmap gestreckt wird, durch Festlegen der [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) Eigenschaft der `Image` auf einen der folgenden Elemente von der [ `Aspect` ](xref:Xamarin.Forms.Aspect) Enumeration:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): respektiert Seitenverhältnis (Standard)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): Bereich ausfüllt, nicht berücksichtigt, Seitenverhältnis beibehalten
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): Bereich ausfüllt, aber berücksichtigt Seitenverhältnis erreicht, indem der Teil der Bitmap Zuschneiden

### <a name="embedded-resources"></a>Eingebettete Ressourcen

Sie können eine Bitmap-Datei in eine PCL oder in einen Ordner in der PCL hinzufügen. Geben Sie ihm eine **Buildvorgang** von **EmbeddedResource**. Die [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) Beispiel veranschaulicht, wie `ImageSource.FromResource` beim Laden der Datei. Der Ressourcenname, der an die Methode übergebene besteht aus den Assemblynamen, gefolgt von einem Punkt, gefolgt vom Namen der Ordner "optional" und einen Punkt, gefolgt von den Dateinamen.

Im Programm wird die `VerticalOptions` und `HorizontalOptions` Eigenschaften der `Image` zu `LayoutOptions.Center`, wodurch die `Image` uneingeschränkt Element. Die `Image` und die Bitmap haben die gleiche Größe:

- Unter iOS und Android die `Image` ist die Größe in Pixel der Bitmap. Es gibt eine 1: 1-Zuordnung zwischen Bitmap und Bildschirmpixel.
- Für universelle Windows-Plattform die `Image` ist die Größe in Pixel der Bitmap in geräteunabhängigen Einheiten. Auf den meisten Geräten nimmt jedes Pixel der Bitmap mehrere Bildschirmpixel.

Die [ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) Beispiel setzt eine `Image` in einer vertikalen `StackLayout` in XAML. Eine Markuperweiterung, die mit dem Namen [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) können auf die eingebettete Ressource in XAML verweisen. Diese Klasse lädt nur die Ressourcen aus der Assembly, in dem sie die befindet, damit es in einer Bibliothek platziert werden kann.

### <a name="more-on-sizing"></a>Weitere Informationen zur größenanpassung

Es ist häufig wünschenswert, Größe Bitmaps einheitlich für alle Plattformen.
Experimentieren mit **StackedBitmap**, Sie können festlegen, eine `WidthRequest` auf die `Image` Element in einer vertikalen `StackLayout` damit die Größe von Plattformen, aber Sie können nur die Größe reduzieren mithilfe dieser Technik.

Sie können auch Festlegen der `HeightRequest` auf das Image auf den Plattformen konsistente Größen, jedoch die eingeschränkte Breite der Bitmap werden die Vielseitigkeit dieser Technik. Für ein Bild in einer vertikalen `StackLayout`, `HeightRequest` sollte vermieden werden.

Der beste Ansatz ist, beginnen mit einer Bitmap, die länger als die Phone Breite in geräteunabhängigen Einheiten, und legen Sie `WidthRequest` auf eine gewünschte Breite in geräteunabhängigen Einheiten. Dies wird veranschaulicht, der [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) Beispiel.

Die [ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) zeigt Kapitel 7 des Lewis Carroll *Abenteuer von Alice im Wunderland* mit den ursprünglichen Abbildungen von John Tenniel:

[![Dreifacher Screenshot mad Tee Partei](images/ch13fg16-small.png "Mad Hatters Tee Partei Buch Text")](images/ch13fg16-large.png#lightbox "Mad Hatters Tee Partei Buch Text")

### <a name="browsing-and-waiting"></a>Durchsuchen, und warten

Die [ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) Beispiel ermöglicht dem Benutzer, durchsuchen die vorhandene Images, die auf der Xamarin-Website gespeichert. Er verwendet den .NET [ `WebRequest` ](xref:System.Net.WebRequest) Klasse, um eine JSON-Datei mit der Liste von Bitmaps zu laden.

> [!NOTE]
> Xamarin.Forms-Programme verwenden sollten [ `HttpClient` ](xref:System.Net.Http.HttpClient) statt [ `WebRequest` ](xref:System.Net.WebRequest) für den Zugriff auf Dateien über das Internet. 

Das Programm verwendet eine [ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) um anzugeben, dass etwas passiert. Wie jede Bitmap lädt, die nur-Lese- [ `IsLoading` ](xref:Xamarin.Forms.Image.IsLoading) Eigenschaft `Image` ist `true`. Die `IsLoading` Eigenschaft wird durch eine bindbare Eigenschaft und gesichert, sodass eine `PropertyChanged` Ereignis wird ausgelöst, wenn diese Eigenschaft geändert wird. Die Anwendung fügt einen Handler an dieses Ereignis und verwendet die aktuelle Einstellung der `IsLoaded` Festlegen der [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Eigenschaft der `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Streaming von bitmaps

Die `ImageSource.FromStream` -Methode erstellt eine `ImageSource` basierend auf einer .NET `Stream`. Die Methode übergeben werden muss eine `Func` Objekt, das zurückgegeben eine `Stream` Objekt.

### <a name="accessing-the-streams"></a>Zugreifen auf die Datenströme

Die [ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) Beispiel veranschaulicht, wie die `ImaageSource.FromStream` Methode zum Laden einer Bitmap, die als eingebettete Ressource gespeichert, und klicken Sie zum Laden einer Bitmaps im Internet.

### <a name="generating-bitmaps-at-run-time"></a>Erzeugen von Bitmaps zur Laufzeit

Alle Xamarin.Forms-Plattformen unterstützen, die nicht komprimierte BMP-Dateiformat, handelt es sich einfach um im Code erstellen und speichern Sie einem `MemoryStream`. Bei dieser Technik können Bitmaps über einen Algorithmus zur Laufzeit zu erstellen, gemäß der Implementierung in der [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) -Klasse in der **Xamrin.FormsBook.Toolkit** Bibliothek.

Die "führen Sie es selbst" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) Beispiel veranschaulicht die Verwendung von `BmpMaker` eine Bitmap mit einer Grafik mit Farbverlauf zu erstellen.

## <a name="platform-specific-bitmaps"></a>Clientplattform-spezifische bitmaps

Alle Xamarin.Forms-Plattformen ermöglichen das Speichern von Bitmaps in den Assemblys der Plattform-Anwendung. Wenn von einer Xamarin.Forms-Anwendung abgerufen werden, sind diese Plattform Bitmaps vom Typ [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource). Verwenden Sie diese für:

- die [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) Eigenschaft [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- die [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) Eigenschaft [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- die [ `Image` ](xref:Xamarin.Forms.Button) Eigenschaft `Button`

Die Testplattform-Assemblys enthalten bereits die Bitmaps für Symbole und Begrüßungsbildschirme:

- In der iOS-Projekt in der **Ressourcen** Ordner
- In der Android-Projekt, in Unterordnern des der **Ressourcen** Ordner
- In der Windows-Projekten in der **Assets** Ordner (obwohl die Windows-Plattformen nicht Bitmaps auf diesen Ordner beschränken)

Die [ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) Beispiel wird Code verwendet, um ein Symbol aus den Platform-Anwendung-Projekten anzuzeigen.

### <a name="bitmap-resolutions"></a>Bitmap-Lösungen

Alle Plattformen ermöglichen die Speicherung mehrerer Versionen von Bitmapbildern Auflösung von verschiedenen Geräten. Zur Laufzeit wird die richtige Version basierend auf der Auflösung des Bildschirms für dieses Gerät geladen.

Diese Bitmaps unterscheiden sich unter iOS die durch das Suffix auf den Dateinamen:

- Kein Suffix für 160 DPI-Geräte (1-Pixel auf geräteunabhängige Einheit)
- "@2x'-Suffix für 320 DPI-Geräte (2 Pixel, um die DIU)
- "@3x'-Suffix für 480 DPI-Geräte (3 Pixel, um die DIU)

Eine Bitmap, als 1 Zoll Quadrat angezeigt werden soll, würden in drei Versionen vorhanden sein:

- "MeinBild.jpg" auf 160 Pixel im Quadrat
- MyImage@2x.jpg am 320 Pixel im Quadrat
- MyImage@3x.jpg auf 480 Pixel im Quadrat

Das Programm würde in diese Bitmap als "MeinBild.jpg" verweisen, aber die richtige Version zur Laufzeit auf Grundlage der Auflösung des Bildschirms abgerufen. Wenn uneingeschränkt ist, wird die Bitmap immer auf 160 geräteunabhängige Einheiten gerendert.

Für Android-Bitmaps werden gespeichert, in den verschiedenen Unterordnern von der **Ressourcen** Ordner:

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

Nur einige Prozentsätze sind gültig. Beispielprogramme dieses Buch enthalten nur Bilder mit **Scale-200** Suffixe, aber aktuelle Xamarin.Forms-Lösungsvorlagen enthalten **Skalierung: 100**, **Skalierung 125**, **Skalierung bis 150**, und **skalieren – 400**.

Beim Hinzufügen von Bitmaps für die Plattformprojekte, die **Buildvorgang** muss:

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **Inhalt**

Die [ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) Beispiel erstellt zwei schaltflächenähnliche-Objekten, die aus der `Image` Elemente mit einem `TapGestureRecognizer` installiert. Es richtet sich an, dass die Objekte einer-Zoll-quadratisch sein. Die `Source` Eigenschaft `Image` wird festgelegt, `OnPlatform` und `On` Objekte auf potenziell unterschiedliche Dateinamen auf den Plattformen zu verweisen. Die Bitmap-Images enthalten die Nummer ihrer Pixelgröße gibt, damit Sie sehen, welche Größe Bitmap abgerufen und gerendert wird.

### <a name="toolbars-and-their-icons"></a>Symbolleisten und die Symbole

Eine der hauptsächlichen Verwendungsformen von plattformspezifischen Bitmaps wird der Xamarin.Forms-Symbolleiste, die durch Hinzufügen von so konstruiert ist [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) Objekte die [ `ToolbarItems` ](xref:Xamarin.Forms.Page.ToolbarItems) Auflistung definiert von `Page`. `ToobarItem` leitet sich von [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) aus der sie einige Eigenschaften erbt.

Die wichtigste `ToolbarItem` Eigenschaften sind:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) für Text, der dargestellt wird, je nach Plattform und `Order`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) Der Typ `FileImageSource` für das Bild, das dargestellt wird, je nach Plattform und `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) Der Typ [ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder), eine Enumeration mit drei Membern [ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default), [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary), und [ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Die Anzahl der `Primary` Elemente müssen auf 3 oder 4 beschränkt sein. Sie sollten einschließen einer `Text` für alle Elemente festlegen. Für die meisten Plattformen, nur die `Primary` Elemente erfordern eine `Icon` Windows 8.1 erfordert jedoch eine `Icon` für alle Elemente. Die Symbole sollten 32 geräteunabhängige Einheiten Quadrat. Die `FileImageSource` Typ gibt an, dass sie plattformspezifische sind.

Die `ToolbarItem` ausgelöst wird eine [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) Ereignis aus, wenn tippen, ähnlich wie eine `Button`. `ToolbarItem` unterstützt auch [ `Command` ](xref:Xamarin.Forms.MenuItem.Command) und [ `CommandParameter` ](xref:Xamarin.Forms.MenuItem.CommandParameter) Eigenschaften, die häufig in Zusammenhang mit MVVM verwendet. (Finden Sie unter [Kapitel 18, MVVM](chapter18.md)).

IOS und Android erfordert, dass eine Seite, eine Symbolleiste angezeigt, werden. eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) oder auf einer Seite navigiert, indem eine `NavigationPage`. Die [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) Programm legt die `MainPage` Eigenschaft die `App` -Klasse der [ `NavigationPage` Konstruktor](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) mit einer `ContentPage` Argument und zeigt die Erstellung und dem angegebenen Ereignishandler einer Symbolleiste.

### <a name="button-images"></a>Schaltfläche "-images

Sie können auch plattformspezifische Bitmaps Festlegen der [ `Image` ](xref:Xamarin.Forms.Button.Image) Eigenschaft `Button` in eine Bitmap des Quadrats 32 geräteunabhängige Einheiten, wie die [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) Beispiel.

> [!NOTE]
> Die Verwendung von Bildern auf Schaltflächen wurde verbessert. Finden Sie unter [mithilfe von Bitmaps mit Schaltflächen](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).

## <a name="related-links"></a>Verwandte Links

- [Kapitel 13 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Kapitel 13-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Working with Images (Arbeiten mit Bildern)](~/xamarin-forms/user-interface/images.md)
- [Mithilfe von Bitmaps mit Schaltflächen](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
