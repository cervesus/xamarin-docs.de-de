---
title: Bilder inXamarin.Forms
description: Images können über Plattformen hinweg gemeinsam genutzt werden Xamarin.Forms , Sie können speziell für jede Plattform geladen werden, oder Sie können zur Anzeige heruntergeladen werden.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7117bb809c43ab5edb67e8367840b17cd1d97ef9
ms.sourcegitcommit: c000c0ed15b7b2ef2a8f46a39171e11b6d9f8a5d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84980089"
---
# <a name="images-in-xamarinforms"></a>Bilder inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_Images können über Plattformen hinweg gemeinsam genutzt werden Xamarin.Forms , Sie können speziell für jede Plattform geladen werden, oder Sie können zur Anzeige heruntergeladen werden._

Bilder sind ein wichtiger Bestandteil der Anwendungs Navigation, Benutzerfreundlichkeit und Branding. Xamarin.FormsAnwendungen müssen in der Lage sein, Bilder auf allen Plattformen gemeinsam zu nutzen, aber auch möglicherweise unterschiedliche Images auf jeder Plattform anzuzeigen.

Für Symbole und Begrüßungs Bildschirme sind auch plattformspezifische Bilder erforderlich. Diese müssen Platt Form bezogen konfiguriert werden.

## <a name="display-images"></a>Anzeigen von Bildern

Xamarin.Formsverwendet die- [`Image`](xref:Xamarin.Forms.Image) Ansicht, um Bilder auf einer Seite anzuzeigen. Es gibt mehrere wichtige Eigenschaften:

- [`Source`](xref:Xamarin.Forms.Image.Source)-Eine- [`ImageSource`](xref:Xamarin.Forms.ImageSource) Instanz, entweder Datei, Uri oder Ressource, mit der das anzuzeigende Bild festgelegt wird.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect): Gibt an, wie die Größe des Bilds innerhalb der Grenzen, in denen es angezeigt wird

[`ImageSource`](xref:Xamarin.Forms.ImageSource)Instanzen können mithilfe statischer Methoden für jeden Typ von Bildquelle abgerufen werden:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)): Erfordert einen Dateinamen oder einen Dateipfad, der auf jeder Plattform aufgelöst werden kann.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))-Erfordert ein URI-Objekt, z. b.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*): Erfordert einen Ressourcen Bezeichner für eine Bilddatei, die in das Anwendungs-oder .NET Standard Bibliotheksprojekt eingebettet ist, mit einer **Buildaktion: EmbeddedResource**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))-Erfordert einen Stream, der Bilddaten bereitstellt.

Die- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) Eigenschaft bestimmt, wie das Bild so skaliert wird, dass es in den Anzeigebereich passt:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill)-Gestreckt das Bild vollständig und gefüllt den Anzeigebereich. Dies kann dazu führen, dass das Bild verzerrt wird.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)-Schneidet das Bild ab, sodass es den Anzeigebereich füllt und dabei den Aspekt bewahrt (d. h. keine Verzerrung).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)-Letterboxes das Bild (falls erforderlich), sodass das gesamte Bild in den Anzeigebereich passt, wobei je nachdem, ob das Bild breit oder hoch ist, leerer Leerraum zum oberen bzw. unteren Rand hinzugefügt wird.

Images können aus einer [lokalen Datei](#local-images), einer [eingebetteten Ressource](#embedded-images), [heruntergeladen](#download-images)oder aus einem Stream geladen werden. Außerdem können Schriftart Symbole in der Ansicht angezeigt werden, [`Image`](xref:Xamarin.Forms.Image) indem die Schriftart Symbol Daten in einem-Objekt angegeben werden `FontImageSource` . Weitere Informationen finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) im [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md) Handbuch.

## <a name="local-images"></a>Lokale Images

Bilddateien können zu jedem Anwendungsprojekt hinzugefügt und aus frei gegebenem Code referenziert werden Xamarin.Forms . Diese Vorgehensweise ist zum Verteilen plattformspezifischer Bilder erforderlich. Beispielsweise kann es vorkommen, dass auf verschiedenen Plattformen unterschiedliche Auflösungen oder geringfügig abweichende Designs verwendet werden.

Zur Verwendung eines einzelnen Bilds für alle apps *muss derselbe Dateiname auf jeder Plattform verwendet werden*, und es muss sich um einen gültigen Android-Ressourcennamen handeln (d. h. nur Kleinbuchstaben, Ziffern, Unterstriche und der Zeitraum sind zulässig).

- **IOS** : die bevorzugte Methode zum Verwalten und unterstützen von Images seit IOS 9 ist die Verwendung von **Asset Catalog-Image Sätzen**, die alle Versionen eines Abbilds enthalten, die zur Unterstützung verschiedener Geräte und Skalierungsfaktoren für eine Anwendung erforderlich sind. Weitere Informationen finden Sie unter [Hinzufügen von Bildern zu einem Asset Catalog-Image Satz](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** : Platzieren Sie Images im **Ressourcen/drawable-** Verzeichnis mit der **Buildaktion "androidresource**". High-und Low-dpi-Versionen eines Bilds können auch bereitgestellt werden (in entsprechend benannten **Ressourcen** Unterverzeichnissen wie **drawable-ldpi**, **drawable-hdpi**und **drawable-xhdpi**).
- **Universelle Windows-Plattform (UWP)** : Standardmäßig sollten Images im Stammverzeichnis der Anwendung mit der **Buildaktion "Content**" platziert werden. Alternativ können Bilder in einem anderen Verzeichnis platziert werden, das dann mit einem plattformspezifischen angegeben wird. Weitere Informationen finden Sie unter [Standard Image Verzeichnis unter Windows](~/xamarin-forms/platform/windows/default-image-directory.md).

> [!IMPORTANT]
> Vor IOS 9 wurden Images in der Regel im Ordner " **Resources** " mit der **Buildaktion "bundleresource**" platziert. Diese Methode zum Arbeiten mit Bildern in einer IOS-App wurde jedoch von Apple als veraltet markiert. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Die Einhaltung dieser Regeln für Datei Benennung und Platzierung ermöglicht dem folgenden XAML-Code das Laden und Anzeigen des Bilds auf allen Plattformen:

```xaml
<Image Source="waterfront.jpg" />
```

Der entsprechende c#-Code lautet wie folgt:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Die folgenden Screenshots zeigen das Ergebnis der Anzeige eines lokalen Bilds auf den einzelnen Plattformen:

[![Beispielanwendung, die ein lokales Image anzeigt](images-images/local-sml.png)](images-images/local.png#lightbox)

Um mehr Flexibilität `Device.RuntimePlatform` zu erhalten, kann die-Eigenschaft verwendet werden, um eine andere Bilddatei oder einen anderen Pfad für einige oder alle Plattformen auszuwählen, wie im folgenden Codebeispiel gezeigt:

```csharp
image.Source = Device.RuntimePlatform == Device.Android
                ? ImageSource.FromFile("waterfront.jpg")
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Um denselben Bilddateinamen auf allen Plattformen zu verwenden, muss der Name auf allen Plattformen gültig sein. Für Android-drawables gelten Benennungs Einschränkungen – nur Kleinbuchstaben, Ziffern, Unterstriche und Ziffern sind zulässig – und für die plattformübergreifende Kompatibilität muss dieser auf allen anderen Plattformen befolgt werden. Der Beispiel Dateiname **waterfront.png** folgt den Regeln, aber Beispiele für ungültige Dateinamen sind "Wasser front.png", "WaterFront.png", "water-front.png" und "wåterfront.png".

### <a name="native-resolutions-retina-and-high-dpi"></a>Native Auflösungen (Retina und High-dpi)

IOS, Android und UWP bieten Unterstützung für unterschiedliche Abbild Auflösungen, bei denen das Betriebssystem basierend auf den Funktionen des Geräts zur Laufzeit das geeignete Abbild auswählt. Xamarin.Formsverwendet die APIs der systemeigenen Plattform zum Laden von lokalen Images, sodass automatisch Alternative Auflösungen unterstützt werden, wenn die Dateien ordnungsgemäß benannt sind und sich im Projekt befinden.

Die bevorzugte Methode zum Verwalten von Abbildern seit IOS 9 ist das Ziehen von Bildern für jede erforderliche Auflösung, die für den passenden Asset Catalog-Image Satz erforderlich ist. Weitere Informationen finden Sie unter [Hinzufügen von Bildern zu einem Asset Catalog-Image Satz](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Vor IOS 9 konnten Retina-Versionen des Bilds im Ordner " **Resources** " platziert werden: zwei und dreifache der Auflösung mit einem- **@2x** oder-Suffix **@3x** für den Dateinamen vor der Dateierweiterung (z. b. **myimage@2x.png**). Diese Methode zum Arbeiten mit Bildern in einer IOS-App wurde jedoch von Apple als veraltet markiert. Weitere Informationen finden Sie unter [Bildgrößen und Dateinamen](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Images alternativer Android-Auflösung sollten in [speziell benannten Verzeichnissen](https://developer.android.com/guide/practices/screens_support.html) im Android-Projekt platziert werden, wie im folgenden Screenshot zu sehen:

[![Android-Image Speicherort mit mehreren Auflösungen](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

UWP-Bilddateinamen [können `.scale-xxx` vor der Dateierweiterung angehängt werden](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), wobei `xxx` der Prozentsatz der Skalierung ist, die auf das Medienobjekt angewendet wird, z. b. **myimage.scale-200.png**. Auf Bilder kann dann in Code oder XAML ohne den Skalierungsmodifizierer verwiesen werden, z. b. nur **myimage.png**. Die Plattform wählt die nächstgelegene geeignete Ressourcen Skala basierend auf dem aktuellen dpi-Wert der Anzeige aus.

### <a name="additional-controls-that-display-images"></a>Weitere Steuerelemente, die Bilder anzeigen

Einige Steuerelemente verfügen über Eigenschaften, die ein Bild anzeigen, z. b.:

- [`Button`](xref:Xamarin.Forms.Button)verfügt über eine- [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) Eigenschaft, die auf ein Bitmapbild festgelegt werden kann, das auf dem angezeigt werden soll `Button` . Weitere Informationen finden Sie unter [Verwenden von Bitmaps mit Schalt](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)Flächen.
- [`ImageButton`](xref:Xamarin.Forms.Button)verfügt über eine- [`Source`](xref:Xamarin.Forms.ImageButton.Source) Eigenschaft, die auf das Bild festgelegt werden kann, das im angezeigt werden soll `ImageButton` . Weitere Informationen finden Sie unter [Festlegen der Bildquelle](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source).
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)verfügt über eine- [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) Eigenschaft, die auf ein Bild festgelegt werden kann, das aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream geladen wird.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell)verfügt über eine- [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) Eigenschaft, die auf ein Bild festgelegt werden kann, das aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream abgerufen wurde.
- [`Page`](xref:Xamarin.Forms.Page). Jeder Seitentyp, der von abgeleitet wird, `Page` verfügt über [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) die Eigenschaften und, denen eine Datei, eine eingebettete Ressource, ein URI oder ein Stream zugewiesen werden kann. Unter bestimmten Umständen, z. b. Wenn ein eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) anzeigt [`ContentPage`](xref:Xamarin.Forms.ContentPage) , wird das Symbol angezeigt, wenn es von der Plattform unterstützt wird.

  > [!IMPORTANT]
  > Unter IOS kann die- [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) Eigenschaft nicht von einem Bild in einem Asset Catalog-Image Satz ausgefüllt werden. Laden Sie stattdessen Symbolbilder für die `Page.IconImageSource` Eigenschaft aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream.

## <a name="embedded-images"></a>Eingebettete Bilder

Eingebettete Bilder werden ebenfalls mit einer Anwendung ausgeliefert (z. b. lokale Images), aber statt eine Kopie des Bilds in der Dateistruktur jeder Anwendung zu verwenden, wird die Bilddatei als Ressource in die Assembly eingebettet. Diese Methode der Verteilung von Bildern wird empfohlen, wenn identische Bilder auf jeder Plattform verwendet werden, und ist besonders für das Erstellen von Komponenten geeignet, da das Bild mit dem Code gebündelt ist.

Wenn Sie ein Bild in ein Projekt einbetten möchten, klicken Sie mit der rechten Maustaste, um neue Elemente hinzuzufügen, und wählen Sie die hinzu zufügenden Images aus. Standardmäßig hat das Image eine **Buildaktion: None**; Dies muss auf **Buildaktion: EmbeddedResource**festgelegt werden.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Buildaktion auf eingebettete Ressource festlegen](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

Die **Buildaktion** kann im **Eigenschaften** Fenster für eine Datei angezeigt und geändert werden.

In diesem Beispiel ist die Ressourcen-ID **WorkingWithImages.beach.jpg**.
Die IDE hat diesen Standardwert generiert, indem der **Standard Namespace** für dieses Projekt mit dem Dateinamen verkettet wird, wobei ein Zeitraum (.) zwischen den einzelnen Werten verwendet wird.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![](images-images/xs-buildaction.png "Set Build Action: EmbeddedResource")

**Buildaktion** kann auch im eigenschaftenpad **Properties** für eine Datei angezeigt und geändert werden.
Dieser Pad zeigt die **Ressourcen-ID** , die verwendet wird, um im Code auf die Ressource zu verweisen. Im folgenden Screenshot ist die **Ressourcen-ID** **WorkingWithImages.beach.jpg**.
Die IDE hat diesen Standardwert generiert, indem der **Standard Namespace** für dieses Projekt mit dem Dateinamen verkettet wird, wobei ein Zeitraum (.) zwischen den einzelnen Werten verwendet wird.
Diese ID kann im **eigenschaftenpad** bearbeitet werden, aber in diesen Beispielen wird der Wert **WorkingWithImages.beach.jpg** verwendet.

[![Eigenschaften-Pad für eingebettete Ressourcen](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

Wenn Sie eingebettete Bilder in Ordner innerhalb Ihres Projekts platzieren, werden die Ordnernamen in der Ressourcen-ID auch durch Punkte (.) getrennt. Das Verschieben des **beach.jpg** Bilds in einen Ordner mit dem Namen **myImages** führt zu einer Ressourcen-ID **WorkingWithImages.MyImages.beach.jpg**

Der Code zum Laden eines eingebetteten Bilds übergibt einfach die **Ressourcen-ID** an die-Methode, [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) wie unten dargestellt:

```csharp
var embeddedImage = new Image {
      Source = ImageSource.FromResource(
        "WorkingWithImages.beach.jpg",
        typeof(EmbeddedImages).GetTypeInfo().Assembly
      ) };
```

> [!NOTE]
> Um das Anzeigen von eingebetteten Bildern im Releasemodus auf dem universelle Windows-Plattform zu unterstützen, ist es erforderlich, die-Überladung von zu verwenden, die `ImageSource.FromResource` die Quellassembly angibt, in der nach dem Bild gesucht werden soll.

Zurzeit gibt es keine implizite Konvertierung für Ressourcen Bezeichner. Stattdessen müssen Sie oder verwenden [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) `new ResourceImageSource()` , um eingebettete Bilder zu laden.

Die folgenden Screenshots zeigen das Ergebnis der Anzeige eines eingebetteten Bilds auf den einzelnen Plattformen:

[![Beispielanwendung, die ein eingebettetes Bild anzeigt](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

Da kein integrierter Typkonverter von in vorhanden ist `string` `ResourceImageSource` , können diese Typen von Bildern nicht System intern von XAML geladen werden. Stattdessen kann eine einfache benutzerdefinierte XAML-Markup Erweiterung zum Laden von Bildern mithilfe einer in XAML angegebenen **Ressourcen-ID** geschrieben werden:

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
> Um das Anzeigen von eingebetteten Bildern im Releasemodus auf dem universelle Windows-Plattform zu unterstützen, ist es erforderlich, die-Überladung von zu verwenden, die `ImageSource.FromResource` die Quellassembly angibt, in der nach dem Bild gesucht werden soll.

Um diese Erweiterung zu verwenden, fügen Sie dem XAML ein benutzerdefiniertes hinzu `xmlns` , indem Sie den korrekten Namespace und die entsprechenden assemblywerte für das Projekt Die Bildquelle kann dann mithilfe der folgenden Syntax festgelegt werden: `{local:ImageResource WorkingWithImages.beach.jpg}` . Unten ist ein vollständiges XAML-Beispiel dargestellt:

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

Da es manchmal schwierig ist zu verstehen, warum eine bestimmte Bildressource nicht geladen wird, kann der folgende Debugcode vorübergehend zu einer Anwendung hinzugefügt werden, um zu bestätigen, dass die Ressourcen ordnungsgemäß konfiguriert sind. Er gibt alle bekannten Ressourcen, die in der angegebenen Assembly eingebettet sind, in die **Konsole** aus, um Probleme beim Laden von Ressourcen zu beheben.

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

Standardmäßig sucht die- `ImageSource.FromResource` Methode nur nach Bildern in derselben Assembly wie der Code, der die-Methode aufgerufen hat `ImageSource.FromResource` . Mithilfe des obigen debugcodes können Sie feststellen, welche Assemblys eine bestimmte Ressource enthalten, indem Sie die- `typeof()` Anweisung in eine `Type` bekannte in jede Assembly ändern.

Die Quellassembly, die nach einem eingebetteten Image durchsucht wird, kann jedoch als Argument für die-Methode angegeben werden `ImageSource.FromResource` :

```csharp
var imageSource = ImageSource.FromResource("filename.png",
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>Images herunterladen

Bilder können für die Anzeige automatisch heruntergeladen werden, wie im folgenden XAML-Code dargestellt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://aka.ms/campus.jpg" />
    <Label Text="campus.jpg gets downloaded from microsoft.com" />
  </StackLayout>
</ContentPage>
```

Der entsprechende c#-Code lautet wie folgt:

```csharp
var webImage = new Image {
     Source = ImageSource.FromUri(
        new Uri("https://aka.ms/campus.jpg")
     ) };
```

Die [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) -Methode erfordert ein `Uri` -Objekt und gibt ein neues zurück [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) , das aus dem liest `Uri` .

Es gibt auch eine implizite Konvertierung für URI-Zeichen folgen. das folgende Beispiel funktioniert auch:

```csharp
webImage.Source = "https://aka.ms/campus.jpg";
```

Die folgenden Screenshots zeigen das Ergebnis der Anzeige eines Remote Bilds auf den einzelnen Plattformen:

[![Beispielanwendung, die ein heruntergeladenes Image anzeigt](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>Herunterladen von Bild Caching

Ein [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) unterstützt auch das Zwischenspeichern von heruntergeladenen Images, die über die folgenden Eigenschaften konfiguriert werden:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled)Gibt an, ob das Caching aktiviert ist ( `true` standardmäßig).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity)-Ein `TimeSpan` , der definiert, wie lange das Bild lokal gespeichert wird.

Caching ist standardmäßig aktiviert und speichert das Image für 24 Stunden lokal. Um das Zwischenspeichern für ein bestimmtes Image zu deaktivieren, instanziieren Sie die Image Quelle wie folgt:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("https://server.com/image") };
```

Zum Festlegen eines bestimmten Cache Zeitraums (z. b. 5 Tage) instanziieren Sie die Image Quelle wie folgt:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://aka.ms/campus.jpg"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Durch die integrierte Zwischenspeicherung ist es sehr einfach, Szenarien wie das Scrollen von Listen mit Bildern zu unterstützen. dabei können Sie ein Bild in jeder Zelle festlegen (oder binden) und dem integrierten Cache das erneute Laden des Bilds ermöglichen, wenn die Zelle wieder in die Ansicht verwandelt wird.

## <a name="animated-gifs"></a>Animierte GIFs

Xamarin.Formsbietet Unterstützung für die Anzeige von kleinen, animierten GIFs. Dies wird erreicht, indem die- [`Image.Source`](xref:Xamarin.Forms.Image.Source) Eigenschaft auf eine animierte GIF-Datei festgelegt wird:

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> Die animierte GIF-Unterstützung in Xamarin.Forms bietet zwar die Möglichkeit, Dateien herunterzuladen, aber es unterstützt nicht das Caching oder Streaming animierter Gifs.

Wenn eine animierte GIF geladen wird, wird Sie standardmäßig nicht wiedergegeben. Dies liegt daran, `IsAnimationPlaying` dass die-Eigenschaft, mit der gesteuert wird, ob ein animiertes GIF wiedergegeben oder angehalten wird, den Standardwert aufweist `false` . Diese Eigenschaft vom Typ `bool` wird durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass Sie das Ziel einer Datenbindung sein kann und formatiert wird.

Wenn ein animiertes GIF geladen wird, wird es daher erst wiedergegeben, wenn die- `IsAnimationPlaying` Eigenschaft auf festgelegt ist `true` . Die Wiedergabe kann dann durch Festlegen der- `IsAnimationPlaying` Eigenschaft auf beendet werden `false` . Beachten Sie, dass diese Eigenschaft keine Auswirkung hat, wenn eine nicht-GIF-Bildquelle angezeigt wird.

> [!NOTE]
> Unter Android erfordert die animierte GIF-Unterstützung, dass Ihre Anwendung schnelle Renderer verwendet, und funktioniert nicht, wenn Sie sich für die Verwendung der Legacy-Renderer entschieden haben.
> Bei UWP erfordert die animierte GIF-Unterstützung mindestens eine Version von Windows 10 Anniversary Update (Version 1607).

## <a name="icons-and-splash-screens"></a>Symbole und Begrüßungs Bildschirme

[`Image`](xref:Xamarin.Forms.Image)Anwendungs Symbole und Begrüßungs Bildschirme sind zwar nicht mit der Ansicht verknüpft, aber auch Anwendungs Symbole und Begrüßungs Bildschirme sind eine wichtige Verwendung von Bildern in Xamarin.Forms Projekten.

Das Festlegen von Symbolen und Begrüßungs Bildschirmen für Xamarin.Forms apps erfolgt in jedem Anwendungsprojekt. Dies bedeutet, dass Sie Images mit ordnungsgemäßer Größe für IOS, Android und UWP erstellen. Diese Images sollten entsprechend den Anforderungen der einzelnen Plattformen benannt werden.

## <a name="icons"></a>Symbole

Weitere Informationen zum Erstellen dieser Anwendungs Ressourcen finden Sie unter [IOS-arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md), [Google Iconography](https://developer.android.com/design/style/iconography.html)und [UWP-Richtlinien für Kachel-und Symbol](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) Ressourcen.

Außerdem können Schriftart Symbole in der Ansicht angezeigt werden, [`Image`](xref:Xamarin.Forms.Image) indem die Schriftart Symbol Daten in einem-Objekt angegeben werden `FontImageSource` . Weitere Informationen finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) im [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md) Handbuch.

## <a name="splash-screens"></a>Begrüßungsbildschirme

Nur IOS-und UWP-Anwendungen erfordern einen Begrüßungsbildschirm (auch als Startbildschirm oder Standardbild bezeichnet).

Weitere Informationen finden Sie in der Dokumentation zu [IOS arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) und Begrüßungs [Bildschirmen](/windows/uwp/launch-resume/splash-screens/) im Windows dev Center.

## <a name="related-links"></a>Verwandte Links

- [Workingwithimages (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [IOS-arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md)
- [Android-iconographie](https://developer.android.com/design/style/iconography.html)
- [App-Symbole und Logos](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
