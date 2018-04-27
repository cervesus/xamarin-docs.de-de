---
title: Bilder
description: Bilder mit Xamarin.Forms plattformübergreifend gemeinsam genutzt werden, sie können speziell für jede Plattform geladen werden oder für die Anzeige heruntergeladen werden.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 5e8ad5ba3bdfa61ae1b2f4404016f204a8c1747c
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="images"></a>Bilder

_Bilder mit Xamarin.Forms plattformübergreifend gemeinsam genutzt werden, sie können speziell für jede Plattform geladen werden oder für die Anzeige heruntergeladen werden._

Images sind wichtiger Bestandteil der Navigation in der Anwendung, Nutzbarkeit und branding. Xamarin.Forms-Anwendungen müssen in der Lage, Freigeben von Bildern auf allen Plattformen, aber möglicherweise auch verschiedene Bilder auf jeder Plattform anzuzeigen.

Clientplattform-spezifische Bilder sind auch für Symbole und Begrüßungsbildschirme erforderlich; Diese müssen auf einem plattformbezogen konfiguriert werden.

Dieses Dokument erläutert die folgenden Themen:

- [ **Lokale Bilder** ](#Local_Images) – Anzeigen von Bildern mit der Anwendung, z. B. Auflösen von systemeigenen Lösungen wie iOS Retina, Android oder uwp-hoher DPI-Einstellung Versionen eines Bilds geliefert.
- [ **Eingebettete Bilder** ](#Embedded_Images) – Anzeigen von Bildern als eine Assemblyressource eingebettet.
- [ **Bilder heruntergeladen** ](#Downloading_Images) – herunterladen und Anzeigen von Bildern.
- [ **Symbole und Begrüßungsbildschirme** ](#Icons_and_splashscreens) -Clientplattform-spezifische Symbole und Startbildern.

## <a name="displaying-images"></a>Anzeigen von Bildern

Xamarin.Forms nutzt die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) können Sie Bilder auf einer Seite anzeigen. Er verfügt über zwei wichtige Eigenschaften:

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) -Eine [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) -Instanz, Datei, Uri oder Ressource, die das anzuzeigende Bild festlegt.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -Gewusst wie: Abbildgröße innerhalb der Grenzen, die sie innerhalb von (entweder Stretch, Zuschneiden oder Letterbox) angezeigt wird.

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) Instanzen können mithilfe der statischen Methoden für jeden Typ der Bildquelle abgerufen werden:

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -Erfordert ein Dateiname oder ein Dateipfad, der auf jeder Plattform aufgelöst werden kann.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -Eine Uri-Objekt, z. B. erfordert.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -Erfordert einen Ressourcenbezeichner in eine Bilddatei, eingebettet in der Anwendung oder die PCL, mit einem **Aktion: EmbeddedResource erstellen**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Ist ein Datenstrom, der Image-Daten bereitstellt.

Die [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Eigenschaft bestimmt, wie das Bild skaliert wird, um den Anzeigebereich anpassen:

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -Streckt das Bild, um den Anzeigebereich vollständig und genau zu füllen. Dies kann das Bild verzerrt werden führen.
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) -Schneidet das Bild, sodass sie den Anzeigebereich voll ist, während das Seitenverhältnis beibehalten (d. h. keine Verzerrung).
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Briefkästen das Bild (falls erforderlich), damit das gesamte Bild in den Anzeigebereich passt, mit Leerzeichen hinzugefügt, den oben/unten oder den Seiten, je nachdem, ob das Bild ist breit oder hoch.

Bilder können aus geladen werden, eine [lokale Datei](#Local_Images_in_Xaml), wird ein [eingebettete Ressource](#embedded_images), oder [heruntergeladen](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Lokale Bilder

Bilddateien können jede Anwendungsprojekt hinzugefügt und von Xamarin.Forms im freigegebenen Code verwiesen werden. Mithilfe ein einzelnes Images für alle apps *muss der gleiche Dateiname verwendet werden, auf jeder Plattform*, und es muss eine gültige Android Ressourcenname (ie. nur Kleinbuchstaben, Ziffern, Unterstriche und den Zeitraum sind zulässig).

- **iOS** : die bevorzugte Methode zum Verwalten und Bilder unterstützen, da iOS 9 ist die Verwendung **Asset-Katalog-Image-Sätze**, sollte die enthalten, alle Versionen eines Bilds, die zur Unterstützung von verschiedenen Geräten und Skalierungsfaktoren für erforderlich sind ein die Anwendung. Weitere Informationen finden Sie unter [Bilder hinzufügen, eine Asset-Katalog Image festgelegt](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -Platzieren von Bildern in der **Ressourcen und Ausgaben möglich** mit Verzeichnis **Buildvorgang: AndroidResource**. Hoher und niedriger DPI Versionen eines Bilds können auch angegeben werden (in entsprechend benannt **Ressourcen** Unterverzeichnisse, wie z. B. **zeichenbaren Ldpi**, **zeichenbaren Hdpi**, und **zeichenbaren Xhdpi**).
- **Universelle Windows-Plattform (UWP)** -Platzieren von Bildern in das Stammverzeichnis der Anwendung mit **Buildvorgang: Inhalt**.

> [!IMPORTANT]
> Vor iOS 9, Bilder in der Regel platziert wurden die **Ressourcen** Ordner mit **Buildvorgang: BundleResource**. Allerdings hat diese Methode für die Arbeit mit Bildern in einer iOS-app von Apple als veraltet markiert. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Diese Regeln für Dateibenennung und Platzierung einhalten kann die folgenden XAML-Code zum Laden und zeigt das Bild auf allen Plattformen:

```xaml
<Image Source="waterfront.jpg" />
```

Der entsprechende c#-Code lautet wie folgt:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von einer lokalen Image auf jeder Plattform:

[![Lokale "ImageSource"](images-images/local-sml.png "Beispielanwendung Anzeigen einer lokalen Bilds")](images-images/local.png#lightbox "Beispielanwendung Anzeigen einer lokalen Bilds")

Zum Erhöhen der Flexibilität der `Device.RuntimePlatform` Eigenschaft kann auf ein anderes Bild-Datei oder der Pfad für einige oder alle Plattformen verwendet werden, wie in diesem Codebeispiel gezeigt:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Verwendung der gleichen Bilddateinamen auf allen Plattformen muss der Name auf allen Plattformen gültig sein. Android Drawables haben benennungseinschränkungen – nur Kleinbuchstaben, Zahlen, Unterstriche und Zeitraum sind zulässig:, und für plattformübergreifende Kompatibilität Dies muss beachtet werden, auf allen Plattformen zu. Der Dateiname Beispiel **waterfront.png** folgt den Regeln jedoch Beispiele für ungültige Dateinamen enthalten "Water front.png", "WaterFront.png", "Water front.png" und "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Systemeigene Lösungen (Retina und hoher DPI-Einstellung)

IOS-, Android- und uwp-schließen die Unterstützung für anderes Bild Auflösungen, in denen das Betriebssystem das passende Image zur Laufzeit basierend auf dem Gerät Funktionen auswählt. Xamarin.Forms nutzt die systemeigener Plattformen APIs zum Laden von lokalen Bilder, sodass es alternative Lösungen automatisch unterstützt, wenn die Dateien ordnungsgemäß benannt und befindet sich im Projekt.

Die bevorzugte Methode zum Verwalten von Images, da iOS 9 darin besteht, die Bilder für jede Lösung erforderlich, um die entsprechenden Asset-Katalog Bildersatz ziehen. Weitere Informationen finden Sie unter [Bilder hinzufügen, eine Asset-Katalog Image festgelegt](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Vor iOS 9, Retina-Versionen des Image platziert werden konnte, in der **Ressourcen** Ordner - 2 und 3 Mal die Auflösung mit einem **@2x** oder **@3x**Suffixe für den Dateinamen vor der Dateierweiterung (z. b. **myimage@2x.png**). Allerdings hat diese Methode für die Arbeit mit Bildern in einer iOS-app von Apple als veraltet markiert. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Bilder Android alternativen Auflösung sollte [speziell benannte Verzeichnisse](http://developer.android.com/guide/practices/screens_support.html) im Android-Projekt, wie im folgenden Screenshot gezeigt:

[![Android mehrere Auflösung Bildspeicherort](images-images/xs-highdpisolution-sml.png "Android mehrere Auflösung Bildspeicherort")](images-images/xs-highdpisolution.png#lightbox "Android Bildspeicherort mehrere Auflösung")

Universelle Windows-Plattform Bilddateinamen [können mit dem Suffix werden `.scale-xxx` vor der Dateierweiterung](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), wobei `xxx` ist der Prozentsatz der Skalierung für die Ressource angewendet wurde, z. B. **myimage.scale 200.png**. Bilder können dann im Code oder in XAML ohne die Skalierung Modifizierer, z. B. direkt verwiesen werden **myimage.png**. Die Plattform wird die nächste geeignete Objektskalierung basierend auf der Anzeige des aktuellen DPI ausgewählt.

### <a name="additional-controls-that-display-images"></a>Weitere Steuerelemente, die Anzeige von Bildern

Einige Steuerelemente verfügen über Eigenschaften, die ein Bild, wie z. B. angezeigt:

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Eine Seite aus abgeleiteter Typ `Page` hat [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) und [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) Eigenschaften, die einen Verweis für die lokale Datei zugewiesen werden können. Unter bestimmten Umständen, z. B. wenn ein [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) anzeigt eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), das Symbol wird angezeigt, wenn von der Plattform unterstützt.

  > [!IMPORTANT]
  > Bei iOS kann die [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) Eigenschaft kann nicht aus einem Image in eine Asset-Katalog Bildersatz aufgefüllt werden. Stattdessen laden Symbolbilder für die `Page.Icon` Eigenschaft aus der **Ressourcen** Ordner im iOS-Projekt.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) – Verfügt über eine [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) -Eigenschaft, die auf einen lokalen Dateiverweis festgelegt werden kann.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) – Verfügt über eine [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) -Eigenschaft, die auf ein Bild festgelegt werden kann von einer lokalen Datei, die eine eingebettete Ressource oder einen URI abgerufen.

<a name="embedded_images" />

## <a name="embedded-images"></a>Eingebettete Bilder

Eingebettete Bilder sind auch im Lieferumfang einer Anwendung (z. B. lokale Bilder), aber anstatt eine Kopie des Abbilds in jede Anwendung Dateistruktur das Image-Datei in der Assembly als Ressource eingebettet ist. Diese Methode zum Verteilen von Abbildern eignet sich insbesondere zum Erstellen von Komponenten, wie das Bild mit dem Code gebündelt ist.

Um ein Bild in einem Projekt einbetten, mit der rechten Maustaste neue Elemente hinzugefügt werden, und wählen das Bild/s, die Sie hinzufügen möchten. Standardmäßig verfügt das Bild über **Buildvorgang: keine**; Dies muss festgelegt werden, um **Buildvorgang: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Buildaktion: EmbeddedResource")

Die **Buildvorgang** angezeigt und geändert werden können die **Eigenschaften** Fenster für eine Datei.

In diesem Beispiel ist die Ressourcen-ID **WorkingWithImages.beach.jpg**.
Die IDE hat diese Standardeinstellung generiert, durch die Aneinanderreihung der **Standard-Namespace** für dieses Projekt mit dem Dateinamen, verwenden Sie einen Punkt (.) zwischen den einzelnen Werten.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "Buildaktion: EmbeddedResource")

**Buildvorgang** auch angezeigt und geändert werden können die **Eigenschaften** Pad für eine Datei.
Dieser Block wird gezeigt, die **Ressourcen-ID** , auf die Ressource im Code verweisen verwendet wird. Im folgenden Screenshot der **Ressourcen-ID** ist **WorkingWithImages.beach.jpg**.
Die IDE hat diese Standardeinstellung generiert, durch die Aneinanderreihung der **Standard-Namespace** für dieses Projekt mit dem Dateinamen, verwenden Sie einen Punkt (.) zwischen den einzelnen Werten.
Diese ID kann bearbeitet werden, der **Eigenschaften** Pad, aber für diesen Beispielen den Wert **WorkingWithImages.beach.jpg** verwendet werden.

![](images-images/xs-embeddedproperties.png "EmbeddedResource Eigenschaften mit Leerstellen auffüllen")

-----

Wenn Sie eingebettete Bilder in Ordnern innerhalb des Projekts ablegen, werden die Ordnernamen durch Punkte (.) in den Ressourcen-ID auch getrennt Verschieben der **beach.jpg** Bild in einen Ordner namens **MyImages** würde eine Ressourcen-ID des **WorkingWithImages.MyImages.beach.jpg**

Übergibt der Code zum Laden eines eingebetteten Bilds der **Ressourcen-ID** auf die [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) Methode wie unten dargestellt:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg") };
```

Zurzeit besteht keine implizite Konvertierung für die Ressourcen-IDs. Sie müssen stattdessen verwenden [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) oder `new ResourceImageSource()` eingebettete Bilder zu laden.

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von ein eingebettetes Bild auf jeder Plattform:

[![ResourceImageSource](images-images/resource-sml.png "Beispielanwendung Anzeigen eines eingebetteten Bilds")](images-images/resource.png#lightbox "Beispielanwendung Anzeigen eines eingebetteten Bilds")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>Verwendung von XAML

Da es keine integrierte Typkonverter von ist `string` zu `ResourceImageSource`, diese Typen von Bildern können nicht systemintern durch XAML geladen werden. Stattdessen kann eine einfache benutzerdefinierte XAML-Markuperweiterung geschrieben werden, Laden von Bildern, die mit einem **Ressourcen-ID** in XAML angegeben:

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
   var imageSource = ImageSource.FromResource(Source);

   return imageSource;
 }
}
```

Verwenden Sie diese Erweiterung hinzufügen ein benutzerdefinierten `xmlns` in XAML, unter Verwendung des richtigen Namespace und Assembly für das Projekt. Die Bildquelle kann dann mithilfe dieser Syntax festgelegt werden: `{local:ImageResource WorkingWithImages.beach.jpg}`. Ein vollständiges Beispiel für die Verwendung von XAML-wird unten gezeigt:

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

### <a name="troubleshooting-embedded-images"></a>Problembehandlung bei eingebettete Bilder

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>Debuggen von Code

Da in einigen Fällen es schwierig ist zu verstehen, warum eine bestimmte Bildressource beansprucht wird nicht, kann die folgenden Debugcode vorübergehend hinzugefügt werden, zu einer Anwendung, um zu bestätigen, dass Ressourcen ordnungsgemäß konfiguriert sind. Es gibt alle bekannte Ressourcen, die in der angegebenen Assembly eingebettet der <span class="UIItem">Konsole</span> , um Probleme Laden der Ressource zu debuggen.

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

#### <a name="images-embedded-in-other-projects-dont-appear"></a>In anderen Projekten eingebetteten Bilder werden nicht angezeigt.

`Image.FromResource` Sucht nur nach Images in derselben Assembly wie die aufrufende Code `FromResource`. Anhand der Debugcode weiter oben aufgeführten können bestimmen, welche Assemblys durch ändern eine bestimmte Ressource enthalten die `typeof()` Anweisung, um eine `Type` bekannt, dass in den jeweiligen Assemblys.

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Herunterladen von Bildern

Bilder können für die Anzeige, automatisch heruntergeladen werden, wie in den folgenden XAML-Code dargestellt:

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

Der entsprechende c#-Code lautet wie folgt:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

Die [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Methode erfordert eine `Uri` -Objekt und gibt eine neue [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) , liest aus dem `Uri`.

Es gibt auch eine implizite Konvertierung für die URI-Zeichenfolgen, sodass das folgende Beispiel auch ausgeführt wird:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von remotebilds auf jeder Plattform:

[![Download von "ImageSource" erwartet](images-images/download-sml.png "Beispielanwendung ein heruntergeladenes Image anzeigen")](images-images/download.png#lightbox "Beispielanwendung ein heruntergeladenes Image anzeigen")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>Das heruntergeladene Image Zwischenspeichern

Ein [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) unterstützt auch das Zwischenspeichern von heruntergeladenen Bildern, über die folgenden Eigenschaften konfiguriert:

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) -Gibt an, ob das Zwischenspeichern aktiviert ist (`true` standardmäßig).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -Ein `TimeSpan` , der definiert, wie lange das Bild lokal gespeichert werden.

Caching ist standardmäßig aktiviert, und speichern Sie das Abbild wird für 24 Stunden lokal. Um das Zwischenspeichern für ein bestimmtes Abbild zu deaktivieren, instanziieren Sie die Bildquelle wie folgt:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Instanziieren Sie die Bildquelle wie folgt, um einen bestimmten Cache Zeitraum (z. B. 5 Tage) festgelegt:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Integriertes Zwischenspeichern erleichtert unterstützen Szenarien wie das Durchführen eines Bildlaufs Listen von Bildern, wo Sie festgelegt (ein Bild oder binden können) in jeder Zelle, und lassen die integrierten Cache kümmern erneut laden des Bilds, wenn die Zelle wieder einen Bildlauf angezeigt wird.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Symbole und Begrüßungsbildschirme

Obwohl sich nicht auf beziehen der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) anzeigen, Symbole und Begrüßungsbildschirme sind auch ein wichtiger Verwendungszweck von Bildern in Xamarin.Forms-Projekten.

Festlegen, Symbole und Begrüßungsbildschirme für apps mit Xamarin.Forms erfolgt in jeder der Anwendungsprojekte. Dies bedeutet, dass die Größe der Bilder für iOS, Android und uwp-ordnungsgemäß generiert. Diese Images sollte mit dem Namen und entsprechend der Anforderungen der einzelnen Plattformen gefunden werden.

## <a name="icons"></a>Symbole

Finden Sie unter der [iOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md), [Google Iconography](http://developer.android.com/design/style/iconography.html), und [Richtlinien für die Kachel und das Symbol Bestand](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) für Weitere Informationen zum Erstellen dieser Anwendungsressourcen.

## <a name="splashscreens"></a>Begrüßungsbildschirme

Nur IOS- und uwp-Apps erfordern eine Splashscreen (auch als ein Start-Bildschirm oder Default-Image).

Finden Sie in der Dokumentation für [iOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) und [Begrüßungsbildschirme](/windows/uwp/launch-resume/splash-screens/) im Windows Dev Center.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms bietet eine Vielzahl von Möglichkeiten, die eine plattformübergreifende Anwendung abfangen, indem für das gleiche Bild plattformübergreifend verwendet werden oder für plattformspezifische Images angegeben werden Bilder einschließt. Heruntergeladenen Abbilder werden auch automatisch zwischengespeichert, ein gängiges Szenario für die Codierung zu automatisieren.

Symbol und Splashscreen Anwendungsbilder Einrichtung sind und für Anwendungen, die nicht Xamarin.Forms - konfiguriert führen Sie die gleiche Anweisungen für plattformspezifische apps verwendet.


## <a name="related-links"></a>Verwandte Links

- [WorkingWithImages (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md)
- [Android Iconography](http://developer.android.com/design/style/iconography.html)
- [Richtlinien für die Kachel und das Symbol Bestand](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
