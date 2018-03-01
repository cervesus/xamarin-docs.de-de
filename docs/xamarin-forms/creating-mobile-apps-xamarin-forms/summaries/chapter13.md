---
title: Zusammenfassung der Kapitel 13. Bitmaps
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2e511f2ebf75b065469a9051ee5797ac58c147f3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>Zusammenfassung der Kapitel 13. Bitmaps

Der Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element zeigt eine Bitmap. Alle Plattformen mit Xamarin.Forms unterstützt die Dateiformate JPEG, PNG, GIF und BMP.

Bitmaps in Xamarin.Forms stammen aus vier Stellen:

- Über das Internet entsprechend den Angaben von einer URL
- Eingebettete als Ressource in der gemeinsamen portablen Klassenbibliothek
- Als Ressource in der Plattform-Anwendungsprojekten eingebettet
- Von jedem beliebigen Standort mit einer .NET verwiesen werden kann `Stream` Objekts, einschließlich `MemoryStream`

Bitmapressourcen in der PCL sind plattformunabhängig, während Bitmapressourcen in den plattformprojekten plattformspezifischen sind.

Die Bitmap wird angegeben, indem die [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) Eigenschaft `Image` auf ein Objekt des Typs [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), eine abstrakte Klasse mit drei ableitungen:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) für den Zugriff auf eine Bitmap, über das Internet auf der Grundlage einer `Uri` -Objekts festgelegt, um seine [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) Eigenschaft
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) für den Zugriff auf eine Bitmap, die in einem Anwendungsprojekt Plattform gespeichert basierend auf einen Ordner und den Dateipfad festlegen, um seine [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) Eigenschaft
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) für das Laden einer Bitmap mit einem .NET `Stream` Objekt angegeben wird, wird durch Zurückgeben einer `Stream` aus einer `Func` legen Sie auf seine [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) Eigenschaft

Alternativ (und häufig) können Sie die folgenden statischen Methoden von der `ImageSource` Klasse, die alle die Rendite `ImageSource` Objekte:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) für den Zugriff auf eine Bitmap, über das Internet auf der Grundlage einer `Uri` Objekt
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) für den Zugriff auf eine Bitmap, die als eingebettete Ressource in der Anwendung PCL, gespeichert oder [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) oder [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) auf einer Bitmap in eine andere Assembly der Ereignisquelle
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) für den Zugriff auf eine Bitmap aus einem Plattform-Anwendungsprojekt
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) für das Laden einer Bitmap, die auf der Grundlage einer `Stream` Objekt

Es gibt keine Klasse Entsprechung der `Image.FromResource` Methoden. Die `UriImageSource` Klasse ist hilfreich, wenn Sie caching steuern müssen. Die `FileImageSource` -Klasse ist hilfreich in XAML. `StreamImageSource` eignet sich für das asynchrone Laden des `Stream` Objekte aufweist, wohingegen `ImageSource.FromStream` ist synchron.

## <a name="platform-independent-bitmaps"></a>Plattformunabhängige Bitmaps

Die [ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) Projekt lädt eine Bitmap, über das Web mit `ImageSource.FromUri`. Der `Image` -Elementgruppe ist der `Content` Eigenschaft der `ContentPage`, sodass es auf die Größe der Seite beschränkt ist. Unabhängig von der Bitmap-Größe, die eine eingeschränkte `Image` Element wird gestreckt, um die Größe des Containers, und die Bitmap wird angezeigt, in die maximale Größe innerhalb der `Image` Element beim Beibehalten des Seitenverhältnisses für die Bitmap. Bereiche der `Image` darüber hinaus die Bitmap mit Farbe werden kann [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

Die [ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) Beispiel ähnelt, aber einfach legt die `Source` -Eigenschaft auf die URL. Die Konvertierung erfolgt durch die [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) Klasse.

### <a name="fit-and-fill"></a>Anpassen und ausfüllen

Können Sie steuern, wie die Bitmap gestreckt wird, durch Festlegen der [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Eigenschaft von der `Image` auf einen der folgenden Elemente der der [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) Enumeration:

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): respektiert Seitenverhältnis (Standard)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): Bereich ausfüllt, Seitenverhältnis nicht berücksichtigt
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): füllt Bereich jedoch respektiert Seitenverhältnis erreicht, indem der Teil der Bitmap Zuschneiden

### <a name="embedded-resources"></a>Eingebettete Ressourcen

Sie können eine Bitmap-Datei zu einer PCL oder in einen Ordner in der PCL hinzufügen. Geben Sie ihm eine **Buildvorgang** von **EmbeddedResource**. Die [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) Beispiel veranschaulicht, wie `ImageSource.FromResource` beim Laden der Datei. Der Ressourcenname, die an die Methode übergebenen besteht aus den Assemblynamen eingeben, gefolgt von einem Punkt, gefolgt vom Namen optionalen Ordner und einen Punkt, gefolgt von den Dateinamen.

Im Programm wird der `VerticalOptions` und `HorizontalOptions` Eigenschaften des der `Image` zu `LayoutOptions.Center`, wodurch die `Image` uneingeschränkten Element. Die `Image` und Bitmap für die haben die gleiche Größe:

- Auf IOS- und Android die `Image` ist die Größe der Bitmap in Pixel. Es gibt eine 1: 1-Zuordnung zwischen Bitmap und Bildschirmpixel.
- Auf den Windows-Runtime-Plattformen die `Image` ist die Größe in Pixel der Bitmap in geräteunabhängigen Einheiten. Auf den meisten Geräten belegt jede Bitmap Pixel mehrere Pixel.

Die [ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) Beispiel setzt eine `Image` in einem vertikalen `StackLayout` in XAML. Eine Markuperweiterung, die mit dem Namen [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) hilft Ihnen bei der die eingebettete Ressource in XAML verweisen. Diese Klasse lädt nur die Ressourcen aus der Assembly, in dem sie der betreffende Computer befindet, damit es in einer Bibliothek platziert werden kann.

### <a name="more-on-sizing"></a>Weitere Informationen zur größenanpassung

Es ist häufig wünschenswert, Größe Bitmaps konsistent auf allen Plattformen unterstützt.
Experimentieren mit **StackedBitmap**, Sie können festlegen, eine `WidthRequest` auf die `Image` Element in einem vertikalen `StackLayout` auf die Größe des Abfrageprotokolls zwischen den Plattformen, aber Sie möchten nur reduzieren die Größe, die auf diese Weise.

Sie können auch Festlegen der `HeightRequest` auf das Bild wird auf den Plattformen konsistent Größen, aber die eingeschränkte Breite der Bitmap wird die Flexibilität dieser Technik beschränkt. Für ein Bild in einem vertikalen `StackLayout`, `HeightRequest` sollte vermieden werden.

Die beste Herangehensweise ist, beginnen mit einer Bitmap, die länger als die Phone Breite in geräteunabhängigen Einheiten, und legen Sie `WidthRequest` eine gewünschte Breite in geräteunabhängigen Einheiten. Dies wird dargestellt, der [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) Beispiel.

Die [ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) zeigt Lewis Carrolls Kapitel 7 *Alice Adventures in Wunderland* mit den ursprünglichen Abbildungen von John Tenniel:

[![Dreifacher Screenshot mad Tee Partei](images/ch13fg16-small.png "Mad Hatters Tee Partei Buch Text")](images/ch13fg16-large.png "Mad Hatters Tee Partei Buch Text")

### <a name="browsing-and-waiting"></a>Durchsuchen und warten

Die [ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) Beispiel kann der Benutzer zum Durchsuchen von vorgefertigten Images, die auf der Xamarin-Website gespeichert. Er verwendet das .NET `WebRequest` Klasse zum Herunterladen einer JSON-Datei mit der Liste der Bitmaps.

Die Anwendung verwendet ein [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) um anzugeben, dass etwas passiert. Wie jede Bitmap lädt, die nur-Lese- [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) Eigenschaft `Image` ist `true`. Die `IsLoading` Eigenschaft durch eine bindbare Eigenschaft unterstützt wird daher ein `PropertyChanged` Ereignis wird ausgelöst, wenn diese Eigenschaft geändert wird. Die Anwendung fügt einen Handler für dieses Ereignis, und verwendet die aktuelle Einstellung der `IsLoaded` festzulegende der [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Eigenschaft von der `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Streaming von bitmaps

Die `ImageSource.FromStream` Methode erstellt ein `ImageSource` basierend auf einer .NET `Stream`. Die Methode übergeben werden muss eine `Func` zurückgegebene Objekt eine `Stream` Objekt.

### <a name="accessing-the-streams"></a>Zugreifen auf die Datenströme

Die [ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) Beispiel veranschaulicht, wie die `ImaageSource.FromStream` Methode zum Laden einer Bitmap, die als eingebettete Ressource gespeichert, und eine Bitmap über das Web zu laden.

### <a name="generating-bitmaps-at-run-time"></a>Generieren von Bitmaps zur Laufzeit

Alle Plattformen mit Xamarin.Forms unterstützen, die nicht komprimierte BMP-Dateiformat, das ist einfach, im Code erstellen und speichern Sie in einem `MemoryStream`. Bei dieser Technik können algorithmisch Bitmaps zur Laufzeit erstellen, wie im implementiert die [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) -Klasse in der **Xamrin.FormsBook.Toolkit** Bibliothek.

Die "führen Sie es selbst" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) Beispiel veranschaulicht die Verwendung von `BmpMaker` eine Bitmap mit einem Farbverlauf Image zu erstellen.

## <a name="platform-specific-bitmaps"></a>Clientplattform-spezifische bitmaps

Alle Plattformen mit Xamarin.Forms ermöglichen das Speichern von Bitmaps in Assemblys der Plattform-Anwendung. Wenn von einer Anwendung Xamarin.Forms abgerufen wird, sind diese Plattform Bitmaps vom Typ [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Verwenden Sie diese für:

- die [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) Eigenschaft [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- die [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) Eigenschaft [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Eigenschaft `Button`

Die Plattformassemblys enthalten bereits Bitmaps, Symbole und Begrüßungsbildschirme:

- In der iOS-Projekt in der **Ressourcen** Ordner
- In der Android-Projekts in Unterordnern des der **Ressourcen** Ordner
- In den Windows-Projekten in der **Bestand** Ordner (obwohl die Windows-Plattformen nicht Bitmaps in diesen Ordner beschränken)

Die [ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) Beispiel wird Code verwendet, um ein Symbol aus der Plattform-Anwendungsprojekten anzuzeigen.

### <a name="bitmap-resolutions"></a>Bitmap-Lösungen

Alle Plattformen ermöglichen die Speicherung mehrerer Versionen von Bitmapbildern bei der Auflösung von verschiedenen Geräten. Zur Laufzeit wird die richtige Version basierend auf dem geräteauflösung des Bildschirms geladen.

Diese Bitmaps werden bei iOS kann durch ein Suffix an den Dateinamen unterschieden:

- Ohne Suffix für 160 DPI-Geräte (1 Pixel der geräteunabhängige Einheit)
- "@2x" Suffix für 320 DPI-Geräte (2 Pixel, um die DIU)
- "@3x" Suffix für 480 DPI-Geräte (3 Pixel, um die DIU)

Eine Bitmap, die als einem Zoll Quadrat angezeigt werden sollen, würde in drei Versionen vorhanden sind:

- "MeinBild.jpg" am quadratische von 160 Pixeln
- MyImage@2x.jpg Bei 320 Pixel quadratische
- MyImage@3x.jpg am quadratische 480 Pixel

Das Programm würde auf diese Bitmap als "MeinBild.jpg" verweisen, aber die richtige Version zur Laufzeit basierend auf der Auflösung des Bildschirms abgerufen. Die Bitmap wird auf 160 geräteunabhängigen Einheiten immer uneingeschränkten, rendern.

Für Android, Bitmaps in verschiedenen Unterordnern gespeichert sind die **Ressourcen** Ordner:

- zeichenbaren-Ldpi (niedrige DPI) für 120 DPI-Geräte (0,75 Pixel, um die DIU)
- zeichenbaren-Mdpi (Mittel) für 160 DPI-Geräte (1 Pixel, um die DIU)
- zeichenbaren-Hdpi (hoch) für 240 DPI-Geräte (1,5 Pixel, um die DIU)
- zeichenbaren-Xhdpi (sehr hoch) für 320 DPI-Geräte (2 Pixel, um die DIU)
- zeichenbaren-Xxhdpi (zusätzliche sehr hoch) für 480 DPI-Geräte (3 Pixel, um die DIU)
- zeichenbaren-Xxxhdpi (drei zusätzliche Höchstwerte) für 640 DPI-Geräte (4 Pixel, um die DIU)

Für eine Bitmap, die an ein Quadrat Zoll gerendert werden soll die verschiedene Versionen der Bitmap müssen den gleichen Namen, aber eine andere Größe und in diesen Ordnern gespeichert werden:

- zeichenbaren-Ldpi / "MeinBild.jpg" von 120 Pixel quadratische
- zeichenbaren-Mdpi / "MeinBild.jpg" am quadratische von 160 Pixeln
- zeichenbaren-Hdpi / "MeinBild.jpg" von 240 Pixel quadratische
- zeichenbaren-Xhdpi / "MeinBild.jpg" von 320 Pixel quadratische
- zeichenbaren-Xxhdpi / "MeinBild.jpg" am quadratische 480 Pixel
- zeichenbaren-Xxxhdpi / "MeinBild.jpg" auf 640 Pixel quadratische

Die Bitmap wird immer um 160 geräteunabhängigen Einheiten gerendert. (Die Xamarin.Forms-Projektmappe Standardvorlage schließt nur die Hdpi, Xhdpi und Xxhdpi-Ordner.)

Die Windows-Runtime-Projekte unterstützen eine Bitmap Namensschema, aus denen ein Skalierungsfaktor in Pixel pro geräteunabhängige Einheit als Prozentsatz, z. B. besteht:

- MyImage.scale-200.jpg am quadratische 320 Pixel

Es sind nur einige Prozentsätze gültig. Beispielprogramme für dieses Buchs sind nur Bilder mit **Skalierung 200** Suffixe, aber die aktuellen Xamarin.Forms Lösungsvorlagen enthalten **Skalierung: 100**, **Skalierung 125**, **Skalierung 150**, und **Skalierung 400**. 

Beim Hinzufügen von Bitmaps für die Plattformprojekte, die **Buildvorgang** werden sollten:

- iOS: **BundleResource**
- Android: **AndroidResource**
- Windows-Runtime: **Inhalt**

Die [ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) Beispiel erstellt zwei Schaltfläche-ähnliche Objekte bestehend aus `Image` Elemente mit einem `TapGestureRecognizer` installiert. Es richtet sich an, dass die Objekte einem Zoll Quadrat sein. Die `Source` Eigenschaft `Image` festgelegt ist, mit `OnPlatform` und `On` -Objekten, die potenziell unterschiedliche Dateinamen für die Plattformen zu verweisen. Die Bitmapbilder sind Zahlen, deren Größe in Pixel, damit Sie sehen können, welche Größe Bitmap abgerufen und gerendert wird.

### <a name="toolbars-and-their-icons"></a>Symbolleisten und die Symbole

Keines der primäre Verwendungszwecke des plattformspezifischen Bitmaps handelt es sich um die Xamarin.Forms-Symbolleiste durch Hinzufügen von erstellt wird [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) -Objekte und die [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) -Auflistung definiert von `Page`. `ToobarItem` leitet sich von [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) aus dem sie einige Eigenschaften erbt.

Die wichtigste `ToolbarItem` Eigenschaften sind:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) für Text, die je nach Plattform angezeigt werden und `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) Der Typ `FileImageSource` für das Bild an, die je nach Plattform angezeigt werden und `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) Der Typ [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/), eine Enumeration mit drei Membern [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), und [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

Die Anzahl der `Primary` Elemente sollte auf drei oder vier beschränkt sein. Sie sollten berücksichtigen eine `Text` für alle Elemente festlegen. Für die meisten Plattformen, nur die `Primary` Elemente erfordern eine `Icon` Windows 8.1 erfordert jedoch eine `Icon` für alle Elemente. Die Symbole darf 32 geräteunabhängigen Einheiten Quadrat. Die `FileImageSource` Typ gibt an, dass diese Plattform spezifisch sind.

Die `ToolbarItem` ausgelöst wird eine [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) Ereignis aus, wenn getippt, ähnlich wie eine `Button`. `ToolbarItem` unterstützt auch [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) und [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) Eigenschaften, die häufig in Verbindung mit MVVM verwendet. (Siehe [Kapitel 18, MVVM](chapter18.md)).

IOS und Android erfordern, dass eine Seite, in dem eine Symbolleiste angezeigt werden, eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) oder eine Seite, zu dem navigiert ein `NavigationPage`. Der [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) Programmierung legt die `MainPage` Eigenschaft seine `App` Klasse der [ `NavigationPage` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) mit einer `ContentPage` Argument, und veranschaulicht die Erstellung und dem angegebenen Ereignishandler einer Symbolleiste.

### <a name="button-images"></a>Bilder

Sie können auch plattformspezifische Bitmaps Festlegen der [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) Eigenschaft `Button` in eine Bitmap 32 geräteunabhängigen Einheiten Quadrats, wie die [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) Beispiel.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 13 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Kapitel 13-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Working with Images (Arbeiten mit Bildern)](~/xamarin-forms/user-interface/images.md)
