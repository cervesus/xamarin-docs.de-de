---
title: Bilder in Xamarin.Forms
description: Bilder können auf Plattformen mit Xamarin.Forms freigegeben werden, sie können speziell für jede Plattform geladen werden, oder für die Anzeige heruntergeladen werden.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 53179170afa1381a562699a39baaa716ecc6a5a6
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171195"
---
# <a name="images-in-xamarinforms"></a>Bilder in Xamarin.Forms

_Bilder können auf Plattformen mit Xamarin.Forms freigegeben werden, sie können speziell für jede Plattform geladen werden, oder für die Anzeige heruntergeladen werden._

Images sind ein entscheidender Punkt bei der Navigation in der Anwendung, Nutzbarkeit und branding. Xamarin.Forms-Anwendungen müssen in der Lage, Freigeben von Bildern auf allen Plattformen, aber möglicherweise auch verschiedene Bilder auf jeder Plattform anzuzeigen.

Clientplattform-spezifische Images sind auch für Symbole und Begrüßungsbildschirme erforderlich; für jede Plattform konfiguriert werden müssen.

## <a name="displaying-images"></a>Anzeigen von Bildern

Xamarin.Forms nutzt die [ `Image` ](xref:Xamarin.Forms.Image) Ansicht zum Anzeigen von Bildern auf einer Seite. Es werden zwei wichtige Eigenschaften hat:

- [`Source`](xref:Xamarin.Forms.Image.Source) -Eine [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) Instanz, entweder "Datei", "Uri" oder "Ressource, die das anzuzeigende Bild legt diese fest.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -Gewusst wie: die Imagegröße innerhalb der Grenzen, die sie innerhalb von (ob Stretch "," Zuschneiden "oder" Letterbox) angezeigt wird.

[`ImageSource`](xref:Xamarin.Forms.ImageSource) Instanzen können mithilfe der statischen Methoden für jeden Typ der Bildquelle abgerufen werden:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) – Erfordert einen Dateinamen oder ein Dateipfad, der auf jeder Plattform aufgelöst werden kann.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) – Ein Uri-Objekt, z. B. erfordert.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) – Erfordert eine Ressourcen-ID in eine Bilddatei eingebettet in der Anwendung oder .NET Standard Library-Projekt mit einem **Aktion: EmbeddedResource erstellen**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) – Erfordert einen Datenstrom, der Image-Daten bereitstellt.

Die [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) Eigenschaft bestimmt, wie das Bild skaliert wird, um den Anzeigebereich anpassen:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -Gestreckt wird, das Bild, um vollständig und genau den Anzeigebereich ausfüllen. Dies kann in das Bild verzerrt wird führen.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -Schneidet das Bild ab, so dass es sich um den Anzeigebereich ausfüllt, und gleichzeitig den Aspekt (d. h. ohne Verzerrung).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -Letterbox das Bild (falls erforderlich), damit das gesamte Bild in den Anzeigebereich passt, mit Leerzeichen hinzugefügt, die oben/unten oder den Seiten, je nachdem, ob das Bild ist breit oder hoch.

Bilder können aus geladen werden, eine [lokale Datei](#Local_Images), [eingebettete Ressource](#embedded-images), oder [heruntergeladen](#Downloading_Images).

## <a name="local-images"></a>Lokalen Images

Bilddateien können jedes Anwendungsprojekt hinzugefügt und von Xamarin.Forms freigegebenen Code verwiesen werden. Dieses Verfahren zum Verteilen von Abbildern ist erforderlich, wenn Bilder plattformspezifisch, z. B. sind, bei Verwendung von unterschiedlichen Auflösungen auf verschiedenen Plattformen oder etwas unterschiedlichen Entwürfe.

Verwenden Sie ein einzelnes Abbild für alle apps *die gleiche Dateinamen muss auf allen Plattformen verwendet werden*, und er muss einen gültigen Ressourcennamen für Android (ie. nur Kleinbuchstaben, Ziffern, Unterstriche und den Zeitraum sind zulässig).

- **iOS** : die Möglichkeit zum Verwalten und unterstützen Bilder aus, da iOS 9 ist die Verwendung bevorzugter **Bildzusammenstellungen für Asset-Katalog**, die enthält alle Versionen der Bilddatei, die sind erforderlich, um Unterstützung für verschiedene Geräte und Skalierungsfaktoren für ein die Anwendung. Weitere Informationen finden Sie unter [Bilder hinzufügen, um eine Asset-Katalog-Image festzulegen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -Platzieren von Bildern in der **Ressourcen/drawable** -Verzeichnis mit **Buildvorgang: AndroidResource**. Hohe und niedrige-DPI-Versionen eines Bildes können auch angegeben werden (in entsprechend benannt **Ressourcen** Unterverzeichnisse, wie z. B. **drawable-Ldpi**, **drawable-Hdpi**, und **drawable-Xhdpi**).
- **Universelle Windows-Plattform (UWP)** -Platzieren von Bildern im Stammverzeichnis der Anwendung, mit **Buildvorgang: Inhalt**.

> [!IMPORTANT]
> Vor iOS 9, wurden in der Regel Bilder platziert die **Ressourcen** Ordner mit **Buildvorgang: BundleResource**. Allerdings ist diese Methode für die Arbeit mit Bildern in einer iOS-app von Apple veraltet. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Befolgen die folgenden Regeln für Dateibenennung und Platzierung kann der folgende XAML zum Laden und Anzeigen der Abbildung auf allen Plattformen:

```xaml
<Image Source="waterfront.jpg" />
```

Der entsprechende C#-Code lautet wie folgt aus:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Die folgenden Screenshots zeigen das Ergebnis ein lokales Image auf jeder Plattform anzuzeigen:

[![Lokale "ImageSource"](images-images/local-sml.png "Beispiel-Anwendung in ein lokales Image")](images-images/local.png#lightbox "Beispielanwendung, die ein lokales Image anzeigen")

Zur Erhöhung der Flexibilität der `Device.RuntimePlatform` Eigenschaft kann auf eine andere Bilddatei oder den Pfad für einige oder alle Plattformen verwendet werden, wie in diesem Codebeispiel wird dargestellt:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Verwendung der gleichen Dateiname des Bilds auf allen Plattformen muss der Name auf allen Plattformen gültig sein. Android zeichenbarer Ressourcen haben benennungseinschränkungen – nur Kleinbuchstaben, Zahlen, einem Unterstrich und Punkt sind zulässig – und für die plattformübergreifende Kompatibilität Dies muss folgen auf den anderen Plattformen zu. Der Dateiname Beispiel **waterfront.png** folgt den Regeln jedoch Beispiele für ungültige Dateinamen enthalten "Wasser front.png", "WaterFront.png", "Wasser-front.png" und "wåterfront.png".

### <a name="native-resolutions-retina-and-high-dpi"></a>Native Auflösung (Retina und High-DPI)

IOS-, Android- und UWP enthalten Unterstützung für verschiedene bildauflösungen, in denen das Betriebssystem das entsprechende Image zur Laufzeit basierend auf die Funktionen des Geräts auswählt. Xamarin.Forms verwendet der nativen Plattformen-APIs für das Laden von lokalen Images, damit es automatisch alternative Lösungen unterstützt, wenn die Dateien ordnungsgemäß mit dem Namen und dem Projekt ein.

Die bevorzugte Methode zum Verwalten von Images, da iOS 9 ist, das Bilder für jede Lösung erforderlich, um die Gruppe die entsprechenden Asset-Katalog von Bildern zu ziehen. Weitere Informationen finden Sie unter [Bilder hinzufügen, um eine Asset-Katalog-Image festzulegen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Vor iOS 9, Retina-Versionen des Images platziert werden konnte, der **Ressourcen** Ordner - 2 und 3 Mal die Auflösung mit einem **@2x** oder **@3x**Suffixe für den Dateinamen vor der Dateierweiterung (z. b. **myimage@2x.png**). Allerdings ist diese Methode für die Arbeit mit Bildern in einer iOS-app von Apple veraltet. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Bilder mit Android alternativen Auflösung platziert werden soll, im [speziell benannte Verzeichnisse](http://developer.android.com/guide/practices/screens_support.html) im Android-Projekt, wie im folgenden Screenshot gezeigt:

[![Android mehrere Auflösung Imagespeicherort](images-images/xs-highdpisolution-sml.png "Android mehrere Auflösung Imagespeicherort")](images-images/xs-highdpisolution.png#lightbox "Android mehrere Auflösung Imagespeicherort")

UWP-Bilddateinamen [können mit dem Suffix werden `.scale-xxx` vor der Dateierweiterung](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), wobei `xxx` ist der Prozentsatz der Skalierung auf das Medienobjekt angewendet wurde, z. B. **myimage.scale-200.png**. Images können im Code oder XAML, ohne den Scale-Modifizierer, z. B. nur dann verwiesen werden **myimage.png**. Die Plattform wird die nächste geeignete Objektskalierung basierend auf der Anzeige des aktuellen DPI ausgewählt.

### <a name="additional-controls-that-display-images"></a>Weitere Steuerelemente, die Bilder anzeigen

Einige Steuerelemente verfügen über Eigenschaften, die ein Bild, wie z. B. angezeigt:

- [`Page`](xref:Xamarin.Forms.Page) -Jede Seite von abgeleiteter Typ `Page` hat [ `Icon` ](xref:Xamarin.Forms.Page.Icon) und [ `BackgroundImage` ](xref:Xamarin.Forms.Page.BackgroundImage) Eigenschaften, die einen lokale Dateiverweis zugewiesen werden können. Unter bestimmten Umständen, z. B. wenn ein [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) zeigt eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), das Symbol wird angezeigt, wenn von der Plattform unterstützt.

  > [!IMPORTANT]
  > Unter iOS die [ `Page.Icon` ](xref:Xamarin.Forms.Page.Icon) Eigenschaft kann nicht aus einem Image in einem Satz für Asset-Katalog-Image nicht aufgefüllt werden. Stattdessen laden Symbolbilder für die `Page.Icon` Eigenschaft aus der **Ressourcen** Ordner im iOS-Projekt.

- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) – Verfügt über eine [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) -Eigenschaft, die auf einen lokalen Verweis festgelegt werden kann.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) – Verfügt über eine [ `ImageSource` ](xref:Xamarin.Forms.ImageCell.ImageSource) -Eigenschaft, die zu einem Bild festgelegt werden, kann von einer lokalen Datei, einer eingebetteten Ressource oder einen URI abgerufen.

## <a name="embedded-images"></a>Eingebettete Bilder

Eingebettete Bilder werden auch mit einer Anwendung (z. B. lokalen Images) geliefert, aber anstatt eine Kopie des Abbilds in jeder Anwendung Dateistruktur das Image-Datei in der Assembly als Ressource eingebettet ist. Dieses Verfahren zum Verteilen von Abbildern wird empfohlen, wenn die identische Images auf jeder Plattform verwendet werden, und eignet sich insbesondere für das Erstellen von Komponenten, wie das Bild mit dem Code im Paket enthalten ist.

Maustaste, um ein Bild in einem Projekt einbetten, um neue Elemente hinzufügen, und wählen Sie die Bild/s, die Sie hinzufügen möchten. Standardmäßig hat das Bild **Buildvorgang: keine**; Dies muss festgelegt werden, um **Buildvorgang: EmbeddedResource**.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](images-images/vs-buildaction.png "Legen Sie Buildaktion: EmbeddedResource")

Die **Buildvorgang** angezeigt und geändert werden können die **Eigenschaften** Fenster für eine Datei.

In diesem Beispiel ist die Ressourcen-ID **WorkingWithImages.beach.jpg**.
Die IDE hat diese Standardeinstellung generiert, durch die Verkettung der **Standard-Namespace** verwenden Sie für dieses Projekt mit dem Dateinamen, einen Punkt (.) zwischen den einzelnen Werten.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](images-images/xs-buildaction.png "Legen Sie Buildaktion: EmbeddedResource")

**Buildvorgang** auch angezeigt und geändert werden können die **Eigenschaften** Pad für eine Datei.
Dieses Feld zeigt die **Ressourcen-ID** , auf die Ressource im Code verwendet wird. Im folgenden Screenshot die **Ressourcen-ID** ist **WorkingWithImages.beach.jpg**.
Die IDE hat diese Standardeinstellung generiert, durch die Verkettung der **Standard-Namespace** verwenden Sie für dieses Projekt mit dem Dateinamen, einen Punkt (.) zwischen den einzelnen Werten.
Diese ID kann bearbeitet werden, der **Eigenschaften** Pad, aber diese Beispiele für den Wert **WorkingWithImages.beach.jpg** verwendet werden.

![](images-images/xs-embeddedproperties.png "EmbeddedResource Pad \"Eigenschaften\"")

-----

Wenn Sie eingebettete Bilder in Ordnern innerhalb Ihres Projekts ablegen, werden die Ordnernamen durch Punkte (.) in den Ressourcen-ID auch getrennt Verschieben der **beach.jpg** Image in einen Ordner namens **MyImages** würde eine Ressourcen-ID der **WorkingWithImages.MyImages.beach.jpg**

Übergibt der Code zum Laden eines eingebetteten Bilds einfach die **Ressourcen-ID** auf die [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*) Methode wie unten dargestellt:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(EmbeddedImages).GetTypeInfo().Assembly) };
```

> [!NOTE]
> Zum Anzeigen von eingebetteten Bildern im Releasemodus auf der universellen Windows-Plattform zu unterstützen, ist es erforderlich, verwenden Sie die Überladung von `ImageSource.FromResource` , angibt, dass die Assembly der Ereignisquelle in der für das Image gesucht werden soll.

Zurzeit besteht keine implizite Konvertierung für die Ressourcen-IDs. Sie müssen stattdessen [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*) oder `new ResourceImageSource()` eingebettete Bilder zu laden.

Die folgenden Screenshots zeigen das Ergebnis ein eingebettetes Bild auf jeder Plattform anzuzeigen:

[![ResourceImageSource](images-images/resource-sml.png "Beispielanwendung Anzeigen eines eingebetteten Bilds")](images-images/resource.png#lightbox "Beispielanwendung, die ein eingebettetes Bild anzeigen")

### <a name="using-xaml"></a>Mithilfe von XAML

Da es ist kein integrierter Typ-Konverter aus `string` zu `ResourceImageSource`, diese Arten von Images können nicht systemintern von XAML geladen werden. Stattdessen kann eine einfache benutzerdefinierte XAML-Markuperweiterung geschrieben werden, zum Laden von Bildern, die mit einem **Ressourcen-ID** in XAML angegeben:

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Zum Anzeigen von eingebetteten Bildern im Releasemodus auf der universellen Windows-Plattform zu unterstützen, ist es erforderlich, verwenden Sie die Überladung von `ImageSource.FromResource` , angibt, dass die Assembly der Ereignisquelle in der für das Image gesucht werden soll.

Verwenden Sie diese Erweiterung hinzufügen ein benutzerdefinierten `xmlns` , die XAML verwenden Sie die richtigen Werte für Namespace und Assembly für das Projekt. Die Bildquelle festgelegt werden, mit der folgenden Syntax: `{local:ImageResource WorkingWithImages.beach.jpg}`. Ein vollständiges Beispiel für XAML ist unten dargestellt:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshooting-embedded-images"></a>Problembehandlung bei Embedded-Images

#### <a name="debugging-code"></a>Debuggen von Code

Da es in einigen Fällen schwer ist zu verstehen, warum eine bestimmte imageressource nicht geladen werden ist, kann die folgenden Debuggen von Code vorübergehend hinzugefügt werden, zu einer Anwendung, um zu bestätigen, dass die Ressourcen korrekt konfiguriert sind. Alle bekannte Ressourcen, die in der angegebenen Assembly eingebettet erfolgt die Ausgabe der <span class="UIItem">Konsole</span> zum Debuggen von Problemen Laden der Ressource.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>In anderen Projekten eingebetteten Bilder

In der Standardeinstellung die `ImageSource.FromResource` Methode sucht nur für Bilder in der gleichen Assembly wie die aufrufende Code die `ImageSource.FromResource` Methode. Verwenden den Debuggen von Code über die Sie ermitteln kann, welche Assemblys eine bestimmte Ressource durch Ändern enthalten die `typeof()` Anweisung, um eine `Type` bekannt, dass in jeder Assembly sind.

Allerdings kann die Assembly der Ereignisquelle für ein eingebettetes Bild zu durchsuchenden angegeben werden, als Argument an die `ImageSource.FromResource` Methode:

```csharp
var imageSource = ImageSource.FromResource("filename.png", typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="downloading-images"></a>Herunterladen von Abbildern

Bilder können für die Anzeige, automatisch heruntergeladen werden, wie in den folgenden XAML dargestellt:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

Der entsprechende C#-Code lautet wie folgt aus:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

Die [ `ImageSource.FromUri` ](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) Methode erfordert eine `Uri` Objekt aus, und gibt ein neues [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource) , liest aus der `Uri`.

Es gibt auch eine implizite Konvertierung für URI-Zeichenfolgen, daher kann im folgenden Beispiel wird auch verwendet werden:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von remotebilds auf jeder Plattform:

[![Herunterladen von "ImageSource"](images-images/download-sml.png "Beispielanwendung ein heruntergeladenes Image anzeigen")](images-images/download.png#lightbox "Beispielanwendung ein heruntergeladenes Image anzeigen")

### <a name="downloaded-image-caching"></a>Heruntergeladene Bild Zwischenspeichern

Ein [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource) unterstützt auch die Zwischenspeicherung von heruntergeladenen Bilder, die über die folgenden Eigenschaften konfiguriert:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) -Gibt an, ob das Zwischenspeichern aktiviert ist (`true` standardmäßig).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) -Ein `TimeSpan` , die definiert, wie lange das Image lokal gespeichert werden.

Caching ist standardmäßig aktiviert, und speichert das Bild lokal für 24 Stunden. Um das Zwischenspeichern für ein bestimmtes Bild zu deaktivieren, instanziieren Sie die Bildquelle wie folgt:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Instanziieren Sie die Bildquelle wie folgt, um einen bestimmten Cache Zeitraum (z. B. 5 Tage) festlegen:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Integriertes Zwischenspeichern erleichtert Szenarios wie die Liste der Images, in dem Sie festgelegt (ein Bild oder binden können) in jeder Zelle Bildlauf unterstützen und den integrierten Cache kümmert sich erneute Laden das Image, wenn die Zelle zurück in die Ansicht gescrollt wird.

## <a name="icons-and-splash-screens"></a>Symbole und Begrüßungsbildschirme

Während die nicht im Zusammenhang mit der [ `Image` ](xref:Xamarin.Forms.Image) anzeigen, Symbole und Begrüßungsbildschirme sind ebenfalls eine wichtige Aufgabe von Bildern in Xamarin.Forms-Projekte.

Festlegen, Symbole und Begrüßungsbildschirme für Xamarin.Forms-apps erfolgt in jedes der Anwendungsprojekte. Dies bedeutet, dass die Größe der Bilder für iOS, Android und UWP ordnungsgemäß generieren. Diese Images mit dem Namen und entsprechend den Anforderungen der einzelnen Plattformen gefunden.

## <a name="icons"></a>Symbole

Finden Sie unter den [iOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md), [Google Ikonographie](http://developer.android.com/design/style/iconography.html), und [Richtlinien für die Kachel "und" Symbol-Objekte](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) für Weitere Informationen zum Erstellen dieser Anwendungsressourcen.

## <a name="splash-screens"></a>Begrüßungsbildschirme

Nur iOS und UWP-Anwendungen einen Begrüßungsbildschirm (auch als ein Start-Bildschirm oder Standard-Image) ist erforderlich.

Lesen Sie die Dokumentation für [iOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) und [Begrüßungsbildschirmen](/windows/uwp/launch-resume/splash-screens/) im Windows Dev Center.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms bietet es sich um eine Reihe von Möglichkeiten zum Einschließen von Images in eine plattformübergreifende Anwendung, sodass für das gleiche Bild, der plattformübergreifend verwendet werden oder für plattformspezifische Images angegeben werden. Heruntergeladenen Bilder werden auch automatisch zwischengespeichert, ein gängiges Szenario für die Programmierung zu automatisieren.

Application-Symbol und Bilder des Begrüßungsbildschirms werden eingerichtet und konfiguriert wie für nicht-Xamarin.Forms-Anwendungen – befolgen Sie die gleiche Anweisungen, die für bestimmte apps verwendet.

## <a name="related-links"></a>Verwandte Links

- [WorkingWithImages (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md)
- [Android Ikonographie](http://developer.android.com/design/style/iconography.html)
- [Richtlinien für die Kachel "und" Symbol-Objekte](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
