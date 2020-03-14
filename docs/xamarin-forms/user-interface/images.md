---
title: Bilder in Xamarin.Forms
description: Bilder können auf Plattformen mit Xamarin.Forms freigegeben werden, sie können speziell für jede Plattform geladen werden, oder für die Anzeige heruntergeladen werden.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 255d3f2f532e4899b1a890405af942a7ca2da8ea
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305708"
---
# <a name="images-in-xamarinforms"></a>Bilder in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_Bilder können plattformübergreifend mit xamarin. Forms freigegeben werden, Sie können speziell für jede Plattform geladen werden, oder Sie können zur Anzeige heruntergeladen werden._

Images sind ein entscheidender Punkt bei der Navigation in der Anwendung, Nutzbarkeit und branding. Xamarin.Forms-Anwendungen müssen in der Lage, Freigeben von Bildern auf allen Plattformen, aber möglicherweise auch verschiedene Bilder auf jeder Plattform anzuzeigen.

Clientplattform-spezifische Images sind auch für Symbole und Begrüßungsbildschirme erforderlich; für jede Plattform konfiguriert werden müssen.

## <a name="display-images"></a>Anzeigen von Bildern

Xamarin. Forms verwendet die [`Image`](xref:Xamarin.Forms.Image) Ansicht, um Bilder auf einer Seite anzuzeigen. Es werden zwei wichtige Eigenschaften hat:

- [`Source`](xref:Xamarin.Forms.Image.Source) -eine [`ImageSource`](xref:Xamarin.Forms.ImageSource) Instanz, entweder Datei, Uri oder Ressource, die das anzuzeigende Bild festlegt.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) : wie die Größe des Bilds innerhalb der Grenzen, in denen es angezeigt wird, angezeigt wird

[`ImageSource`](xref:Xamarin.Forms.ImageSource) Instanzen können mithilfe statischer Methoden für jeden Typ von Bildquelle abgerufen werden:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) : erfordert einen Dateinamen oder einen filePath, der auf jeder Plattform aufgelöst werden kann.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) : erfordert ein URI-Objekt, z. b.  `new Uri("http://server.com/image.jpg")`.
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) : erfordert einen Ressourcen Bezeichner für eine Bilddatei, die in die Anwendung oder das .NET Standard Bibliotheksprojekt eingebettet ist, mit einer **Buildaktion: EmbeddedResource**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) : erfordert einen Stream, der Bilddaten bereitstellt.

Die [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -Eigenschaft bestimmt, wie das Bild so skaliert wird, dass es in den Anzeigebereich passt:

- das Bild wird [`Fill`](xref:Xamarin.Forms.Aspect.Fill) gestreckt, sodass der Anzeigebereich vollständig aufgefüllt wird. Dies kann in das Bild verzerrt wird führen.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -schneidet das Bild ab, sodass es den Anzeigebereich füllt und dabei den Aspekt bewahrt (d.h. keine Verzerrung).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -letterboxes das Bild (falls erforderlich), sodass das gesamte Bild in den Anzeigebereich passt, wobei der obere bzw. untere Rand des Bilds in Abhängigkeit davon, ob das Bild breit oder hoch ist, leerer Leerraum hinzugefügt wird.

Images können aus einer [lokalen Datei](#local-images), einer [eingebetteten Ressource](#embedded-images), [heruntergeladen](#download-images)oder aus einem Stream geladen werden. Außerdem können Schriftart Symbole in der [`Image`](xref:Xamarin.Forms.Image) Ansicht angezeigt werden, indem die Schriftart Symbol Daten in einem `FontImageSource`-Objekt angegeben werden. Weitere Informationen finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) im [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md) Handbuch.

## <a name="local-images"></a>Lokale Images

Bilddateien können jedes Anwendungsprojekt hinzugefügt und von Xamarin.Forms freigegebenen Code verwiesen werden. Diese Vorgehensweise ist zum Verteilen plattformspezifischer Bilder erforderlich. Beispielsweise kann es vorkommen, dass auf verschiedenen Plattformen unterschiedliche Auflösungen oder geringfügig abweichende Designs verwendet werden.

Um ein einzelnes Image für alle apps zu verwenden, *muss der gleiche Dateiname auf jeder Plattform verwendet werden*, und es muss sich um einen gültigen Android-Ressourcennamen handeln (d.h. nur Kleinbuchstaben, Ziffern, Unterstriche und der Zeitraum sind zulässig).

- **IOS** : die bevorzugte Methode zum Verwalten und unterstützen von Images seit IOS 9 ist die Verwendung von **Asset Catalog-Image Sätzen**, die alle Versionen eines Abbilds enthalten, die zur Unterstützung verschiedener Geräte und Skalierungsfaktoren für eine Anwendung erforderlich sind. Weitere Informationen finden Sie unter [Hinzufügen von Bildern zu einem Asset Catalog-Image Satz](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** : Platzieren Sie Images im **Ressourcen/drawable-** Verzeichnis mit der **Buildaktion "androidresource**". High-und Low-dpi-Versionen eines Bilds können auch bereitgestellt werden (in entsprechend benannten **Ressourcen** Unterverzeichnissen wie **drawable-ldpi**, **drawable-hdpi**und **drawable-xhdpi**).
- **Universelle Windows-Plattform (UWP)** : Platzieren Sie Images im Stammverzeichnis der Anwendung mit der **Buildaktion: Content**.

> [!IMPORTANT]
> Vor IOS 9 wurden Images in der Regel im Ordner " **Resources** " mit der **Buildaktion "bundleresource**" platziert. Allerdings ist diese Methode für die Arbeit mit Bildern in einer iOS-app von Apple veraltet. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Befolgen die folgenden Regeln für Dateibenennung und Platzierung kann der folgende XAML zum Laden und Anzeigen der Abbildung auf allen Plattformen:

```xaml
<Image Source="waterfront.jpg" />
```

Der entsprechende C#-Code lautet wie folgt aus:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Die folgenden Screenshots zeigen das Ergebnis ein lokales Image auf jeder Plattform anzuzeigen:

[![Beispielanwendung, die ein lokales Image anzeigt](images-images/local-sml.png)](images-images/local.png#lightbox)

Um mehr Flexibilität zu erhalten, kann die `Device.RuntimePlatform`-Eigenschaft verwendet werden, um eine andere Bilddatei oder einen anderen Pfad für einige oder alle Plattformen auszuwählen, wie im folgenden Codebeispiel gezeigt:

```csharp
image.Source = Device.RuntimePlatform == Device.Android 
                ? ImageSource.FromFile("waterfront.jpg") 
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Verwendung der gleichen Dateiname des Bilds auf allen Plattformen muss der Name auf allen Plattformen gültig sein. Android zeichenbarer Ressourcen haben benennungseinschränkungen – nur Kleinbuchstaben, Zahlen, einem Unterstrich und Punkt sind zulässig – und für die plattformübergreifende Kompatibilität Dies muss folgen auf den anderen Plattformen zu. Der Beispiel Dateiname **Waterfront. png** folgt den Regeln. Beispiele für ungültige Dateinamen sind z. b. "Water Front. png", "Waterfront. png", "Water-Front. png" und "wåterfront. png".

### <a name="native-resolutions-retina-and-high-dpi"></a>Native Auflösungen (Retina und High-dpi)

IOS-, Android- und UWP enthalten Unterstützung für verschiedene bildauflösungen, in denen das Betriebssystem das entsprechende Image zur Laufzeit basierend auf die Funktionen des Geräts auswählt. Xamarin.Forms verwendet der nativen Plattformen-APIs für das Laden von lokalen Images, damit es automatisch alternative Lösungen unterstützt, wenn die Dateien ordnungsgemäß mit dem Namen und dem Projekt ein.

Die bevorzugte Methode zum Verwalten von Images, da iOS 9 ist, das Bilder für jede Lösung erforderlich, um die Gruppe die entsprechenden Asset-Katalog von Bildern zu ziehen. Weitere Informationen finden Sie unter [Hinzufügen von Bildern zu einem Asset Catalog-Image Satz](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Vor IOS 9 konnten Retina-Versionen des Bilds im Ordner " **Resources** " platziert werden: zwei und dreifache der Auflösung mit einem **@2x** oder **@3x** Suffixe für den Dateinamen vor der Dateierweiterung (z. b. **myimage@2x.png** ). Allerdings ist diese Methode für die Arbeit mit Bildern in einer iOS-app von Apple veraltet. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Images alternativer Android-Auflösung sollten in [speziell benannten Verzeichnissen](https://developer.android.com/guide/practices/screens_support.html) im Android-Projekt platziert werden, wie im folgenden Screenshot zu sehen:

[Speicherort für die ![Android-Image mit mehreren Auflösungen](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

UWP-Bilddateinamen [können mit `.scale-xxx` vor der Dateierweiterung versehen werden](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), wobei `xxx` der Prozentsatz der Skalierung ist, die auf das Medienobjekt angewendet wird, z. b. **myImage. Scale-200. png**. Auf Bilder kann dann in Code oder XAML ohne den Skalierungsmodifizierer verwiesen werden, z. b. nur **myImage. png**. Die Plattform wird die nächste geeignete Objektskalierung basierend auf der Anzeige des aktuellen DPI ausgewählt.

### <a name="additional-controls-that-display-images"></a>Weitere Steuerelemente, die Bilder anzeigen

Einige Steuerelemente verfügen über Eigenschaften, die ein Bild, wie z. B. angezeigt:

- [`Button`](xref:Xamarin.Forms.Button) verfügt über eine [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) -Eigenschaft, die auf ein Bitmap-Bild festgelegt werden kann, das auf dem `Button`angezeigt werden soll. Weitere Informationen finden Sie unter [Verwenden von Bitmaps mit Schalt](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)Flächen.
- [`ImageButton`](xref:Xamarin.Forms.Button) verfügt über eine [`Source`](xref:Xamarin.Forms.ImageButton.Source) -Eigenschaft, die auf das Bild festgelegt werden kann, das im `ImageButton`angezeigt werden soll. Weitere Informationen finden Sie unter [Festlegen der Bildquelle](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source).
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) verfügt über eine [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) -Eigenschaft, die auf ein Bild festgelegt werden kann, das aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream geladen wird.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) verfügt über eine [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) -Eigenschaft, die auf ein Bild festgelegt werden kann, das aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream abgerufen wurde.
- [`Page`](xref:Xamarin.Forms.Page). Jeder Seitentyp, der von `Page` abgeleitet ist, verfügt über [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) -und [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) -Eigenschaften, denen eine Datei, eine eingebettete Ressource, ein URI oder ein Stream zugewiesen werden kann. Unter bestimmten Umständen, z. b. Wenn ein [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)anzeigt, wird das Symbol angezeigt, wenn es von der Plattform unterstützt wird.

  > [!IMPORTANT]
  > Unter IOS kann die [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) -Eigenschaft nicht aus einem Image in einem Asset Catalog-Image Satz aufgefüllt werden. Laden Sie stattdessen Symbolbilder für die `Page.IconImageSource`-Eigenschaft aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream.

## <a name="embedded-images"></a>Eingebettete Bilder

Eingebettete Bilder werden auch mit einer Anwendung (z. B. lokalen Images) geliefert, aber anstatt eine Kopie des Abbilds in jeder Anwendung Dateistruktur das Image-Datei in der Assembly als Ressource eingebettet ist. Dieses Verfahren zum Verteilen von Abbildern wird empfohlen, wenn die identische Images auf jeder Plattform verwendet werden, und eignet sich insbesondere für das Erstellen von Komponenten, wie das Bild mit dem Code im Paket enthalten ist.

Maustaste, um ein Bild in einem Projekt einbetten, um neue Elemente hinzufügen, und wählen Sie die Bild/s, die Sie hinzufügen möchten. Standardmäßig hat das Image eine **Buildaktion: None**; Dies muss auf **Buildaktion: EmbeddedResource**festgelegt werden.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Buildaktion auf eingebettete Ressource festlegen](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

Die **Buildaktion** kann im **Eigenschaften** Fenster für eine Datei angezeigt und geändert werden.

In diesem Beispiel ist die Ressourcen-ID **workingwithimages. Beach. jpg**.
Die IDE hat diesen Standardwert generiert, indem der **Standard Namespace** für dieses Projekt mit dem Dateinamen verkettet wird, wobei ein Zeitraum (.) zwischen den einzelnen Werten verwendet wird.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![](images-images/xs-buildaction.png "Set Build Action: EmbeddedResource")

**Buildaktion** kann auch im eigenschaftenpad **Properties** für eine Datei angezeigt und geändert werden.
Dieser Pad zeigt die **Ressourcen-ID** , die verwendet wird, um im Code auf die Ressource zu verweisen. Im folgenden Screenshot ist die **Ressourcen-ID** **workingwithimages. Beach. jpg**.
Die IDE hat diesen Standardwert generiert, indem der **Standard Namespace** für dieses Projekt mit dem Dateinamen verkettet wird, wobei ein Zeitraum (.) zwischen den einzelnen Werten verwendet wird.
Diese ID kann im **eigenschaftenpad** bearbeitet werden, aber für diese Beispiele wird der Wert **workingwithimages. Beach. jpg** verwendet.

[Eigenschaften-Pad für eingebettete Ressourcen ![](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

Wenn Sie eingebettete Bilder in Ordnern innerhalb Ihres Projekts ablegen, werden die Ordnernamen durch Punkte (.) in den Ressourcen-ID auch getrennt Wenn Sie das Image "Image **. jpg** " in einen Ordner mit dem Namen " **myImages** " verschieben, wird eine Ressourcen-ID von **workingwithimages. myImages. Beach. jpg** angezeigt.

Der Code zum Laden eines eingebetteten Bilds übergibt einfach die **Ressourcen-ID** an die [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) -Methode, wie unten dargestellt:

```csharp
var embeddedImage = new Image { 
      Source = ImageSource.FromResource(
        "WorkingWithImages.beach.jpg", 
        typeof(EmbeddedImages).GetTypeInfo().Assembly
      ) };
```

> [!NOTE]
> Um das Anzeigen eingebetteter Bilder im Releasemodus auf dem universelle Windows-Plattform zu unterstützen, ist es erforderlich, die-Überladung `ImageSource.FromResource` zu verwenden, die die Quellassembly angibt, in der nach dem Bild gesucht werden soll.

Zurzeit besteht keine implizite Konvertierung für die Ressourcen-IDs. Stattdessen müssen Sie [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) oder `new ResourceImageSource()` verwenden, um eingebettete Bilder zu laden.

Die folgenden Screenshots zeigen das Ergebnis ein eingebettetes Bild auf jeder Plattform anzuzeigen:

[![Beispielanwendung, die ein eingebettetes Bild anzeigt](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

Da kein integrierter Typkonverter von `string` zu `ResourceImageSource`vorhanden ist, können diese Typen von Bildern nicht nativ von XAML geladen werden. Stattdessen kann eine einfache benutzerdefinierte XAML-Markup Erweiterung zum Laden von Bildern mithilfe einer in XAML angegebenen **Ressourcen-ID** geschrieben werden:

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
> Um das Anzeigen eingebetteter Bilder im Releasemodus auf dem universelle Windows-Plattform zu unterstützen, ist es erforderlich, die-Überladung `ImageSource.FromResource` zu verwenden, die die Quellassembly angibt, in der nach dem Bild gesucht werden soll.

Um diese Erweiterung zu verwenden, fügen Sie der XAML ein benutzerdefiniertes `xmlns` hinzu, indem Sie den korrekten Namespace und die entsprechenden assemblywerte für das Projekt Die Bildquelle kann dann mithilfe der folgenden Syntax festgelegt werden: `{local:ImageResource WorkingWithImages.beach.jpg}`. Ein vollständiges Beispiel für XAML ist unten dargestellt:

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

### <a name="troubleshoot-embedded-images"></a>Problembehandlung für eingebettete Bilder

#### <a name="debug-code"></a>Debuggen von Code

Da es in einigen Fällen schwer ist zu verstehen, warum eine bestimmte imageressource nicht geladen werden ist, kann die folgenden Debuggen von Code vorübergehend hinzugefügt werden, zu einer Anwendung, um zu bestätigen, dass die Ressourcen korrekt konfiguriert sind. Er gibt alle bekannten Ressourcen, die in der angegebenen Assembly eingebettet sind, in die **Konsole** aus, um Probleme beim Laden von Ressourcen zu beheben.

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

#### <a name="images-embedded-in-other-projects"></a>In andere Projekte eingebettete Bilder

Standardmäßig sucht die `ImageSource.FromResource`-Methode nur nach Bildern in derselben Assembly wie der Code, der die `ImageSource.FromResource`-Methode aufrufen. Mithilfe des obigen debugcodes können Sie feststellen, welche Assemblys eine bestimmte Ressource enthalten, indem Sie die `typeof()`-Anweisung in eine `Type`, die in jeder Assembly bekannt ist, ändern.

Die Quellassembly, die nach einem eingebetteten Image durchsucht wird, kann jedoch als Argument für die `ImageSource.FromResource`-Methode angegeben werden:

```csharp
var imageSource = ImageSource.FromResource("filename.png", 
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>Images herunterladen

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
var webImage = new Image { 
     Source = ImageSource.FromUri(
        new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")
     ) };
```

Die [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) -Methode erfordert ein `Uri`-Objekt und gibt ein neues [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) zurück, das aus dem `Uri`liest.

Es gibt auch eine implizite Konvertierung für URI-Zeichenfolgen, daher kann im folgenden Beispiel wird auch verwendet werden:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Die folgenden Screenshots zeigen das Ergebnis zum Anzeigen von remotebilds auf jeder Plattform:

[![Beispielanwendung, die ein heruntergeladenes Image anzeigt](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>Herunterladen von Bild Caching

Ein [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) unterstützt auch das Zwischenspeichern von heruntergeladenen Images, die über die folgenden Eigenschaften konfiguriert werden:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) , ob Caching aktiviert ist (standardmäßig`true`).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) -ein `TimeSpan`, der definiert, wie lange das Image lokal gespeichert wird.

Caching ist standardmäßig aktiviert, und speichert das Bild lokal für 24 Stunden. Um das Zwischenspeichern für ein bestimmtes Bild zu deaktivieren, instanziieren Sie die Bildquelle wie folgt:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("http://server.com/image") };
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

## <a name="animated-gifs"></a>Animierte GIFs

Xamarin. Forms bietet Unterstützung für die Anzeige von kleinen, animierten GIFs. Dies wird erreicht, indem die [`Image.Source`](xref:Xamarin.Forms.Image.Source) -Eigenschaft auf eine animierte GIF-Datei festgelegt wird:

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> Während die animierte GIF-Unterstützung in xamarin. Forms die Möglichkeit bietet, Dateien herunterzuladen, unterstützt das Caching oder Streaming animierte GIFs nicht.

Wenn eine animierte GIF geladen wird, wird Sie standardmäßig nicht wiedergegeben. Dies liegt daran, dass die `IsAnimationPlaying`-Eigenschaft, mit der gesteuert wird, ob ein animiertes GIF wiedergegeben oder angehalten wird, über den Standardwert `false`verfügt. Diese Eigenschaft vom Typ "`bool`" wird durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel einer Datenbindung handeln und formatiert werden kann.

Wenn ein animiertes GIF geladen wird, wird es daher erst wiedergegeben, wenn die `IsAnimationPlaying`-Eigenschaft auf `true`festgelegt ist. Die Wiedergabe kann dann beendet werden, indem die `IsAnimationPlaying`-Eigenschaft auf `false`festgelegt wird. Beachten Sie, dass diese Eigenschaft keine Auswirkung hat, wenn eine nicht-GIF-Bildquelle angezeigt wird.

> [!NOTE]
> Unter Android erfordert die animierte GIF-Unterstützung, dass Ihre Anwendung schnelle Renderer verwendet, und funktioniert nicht, wenn Sie sich für die Verwendung der Legacy-Renderer entschieden haben.
> Bei UWP erfordert die animierte GIF-Unterstützung mindestens eine Version von Windows 10 Anniversary Update (Version 1607).

## <a name="icons-and-splash-screens"></a>Symbole und Begrüßungsbildschirme

Anwendungs Symbole und Begrüßungs Bildschirme sind zwar nicht mit der [`Image`](xref:Xamarin.Forms.Image) Ansicht verknüpft, aber auch Anwendungs Symbole und Begrüßungs Bildschirme sind eine wichtige Verwendung von Bildern in xamarin. Forms-Projekten.

Festlegen, Symbole und Begrüßungsbildschirme für Xamarin.Forms-apps erfolgt in jedes der Anwendungsprojekte. Dies bedeutet, dass die Größe der Bilder für iOS, Android und UWP ordnungsgemäß generieren. Diese Images mit dem Namen und entsprechend den Anforderungen der einzelnen Plattformen gefunden.

## <a name="icons"></a>Symbole

Weitere Informationen zum Erstellen dieser Anwendungs Ressourcen finden Sie unter [IOS-arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md), [Google Iconography](https://developer.android.com/design/style/iconography.html)und [UWP-Richtlinien für Kachel-und Symbol](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) Ressourcen.

Außerdem können Schriftart Symbole in der [`Image`](xref:Xamarin.Forms.Image) Ansicht angezeigt werden, indem die Schriftart Symbol Daten in einem `FontImageSource`-Objekt angegeben werden. Weitere Informationen finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) im [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md) Handbuch.

## <a name="splash-screens"></a>Begrüßungsbildschirme

Nur iOS und UWP-Anwendungen einen Begrüßungsbildschirm (auch als ein Start-Bildschirm oder Standard-Image) ist erforderlich.

Weitere Informationen finden Sie in der Dokumentation zu [IOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) und Begrüßungs [Bildschirmen](/windows/uwp/launch-resume/splash-screens/) im Windows dev Center.

## <a name="related-links"></a>Verwandte Links

- [Workingwithimages (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [IOS-arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md)
- [Android-iconographie](https://developer.android.com/design/style/iconography.html)
- [Richtlinien für Kachel-und Symbol Ressourcen](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
