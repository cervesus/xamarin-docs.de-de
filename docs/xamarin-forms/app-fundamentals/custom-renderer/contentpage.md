---
title: Anpassen einer ContentPage
description: Eine ContentPage ist ein visuelles Element, das zeigt eine einzelnen Ansicht und der Großteil des Bildschirms einnimmt. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer auf der Seite ContentPage ermöglichen Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995741"
---
# <a name="customizing-a-contentpage"></a>Anpassen einer ContentPage

_Eine ContentPage ist ein visuelles Element, das zeigt eine einzelnen Ansicht und der Großteil des Bildschirms einnimmt. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer auf der Seite ContentPage ermöglichen Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben._

Alle Xamarin.Forms-Steuerelements verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) gerendert wird, von einer Xamarin.Forms-Anwendung unter iOS die `PageRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `UIViewController` Steuerelement. Auf der Android-Plattform die `PageRenderer` Klasse instanziiert ein `ViewGroup` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `PageRenderer` Klasse instanziiert ein `FrameworkElement` Steuerelement. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) und die entsprechenden systemeigenen Steuerelemente, die sie implementieren:

![](contentpage-images/contentpage-classes.png "Beziehung zwischen ContentPage-Klasse und Implementieren von systemeigenen Steuerelementen")

Das Rendern zu kann nutzen erstellt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Xamarin.Forms_Page) einer Xamarin.Forms-Startseite.
1. [Nutzen](#Consuming_the_Xamarin.Forms_Page) der Seite von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Page_Renderer_on_each_Platform) der benutzerdefinierte Renderer für die Seite auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer `CameraPage` , einer live-Kamera und die Möglichkeit, ein Foto aufzunehmen bietet.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Erstellen das Xamarin.Forms-Startseite

Eine unveränderte [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) können auf das freigegebene Xamarin.Forms-Projekt hinzugefügt werden, wie in den folgenden XAML-Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Auf ähnliche Weise die Code-Behind-Datei für die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) sollte auch bleiben unverändert, wie im folgenden Codebeispiel gezeigt:

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

Im folgenden Codebeispiel wird veranschaulicht, wie die Seite in c# erstellt werden kann:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Eine Instanz von der `CameraPage` wird verwendet, um die live-Kamera auf jeder Plattform anzuzeigen. Anpassung des Steuerelements durchgeführt werden in den benutzerdefinierten Renderer, sodass keine zusätzliche Implementierung erforderlich ist der `CameraPage` Klasse.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Nutzen die Xamarin.Forms-Startseite

Die leere `CameraPage` muss die Xamarin.Forms-Anwendung angezeigt werden. Dies geschieht, wenn eine Schaltfläche auf der `MainPage` Instanz tippen, das wiederum führt die `OnTakePhotoButtonClicked` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Dieser Code ist einfach navigiert die `CameraPage`, auf welche benutzerdefinierte Renderer die Darstellung der Seite auf jeder Plattform anpassen.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Erstellen die Seiten-Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `PageRenderer` Klasse.
1. Überschreiben der `OnElementChanged` -Methode, die native Seite und Write-Logik zum Anpassen der Seite rendert. Die `OnElementChanged` Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die Seite Renderer-Klasse, um anzugeben, dass er zum Rendern der Seite "Xamarin.Forms" verwendet wird. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Dies ist optional, um eine Seite-Renderer in jedem plattformprojekt bereitzustellen. Wenn ein Seite-Renderer nicht registriert ist, wird der Standard-Renderer für die Seite verwendet werden.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, zusammen mit der Beziehung zwischen ihnen:

![](contentpage-images/solution-structure.png "Zuständigkeiten von CameraPage benutzerdefinierten Renderer-Projekt")

Die `CameraPage` plattformspezifische Instanz gerendert wird `CameraPageRenderer` , alle abgeleiteten Klassen der `PageRenderer` Klasse für die betreffende Plattform. Dies führt in jeder `CameraPage` -Instanz mit einem live-Kamera-Feed, gerendert wird, wie in den folgenden Screenshots gezeigt:

![](contentpage-images/screenshots.png "CameraPage auf jeder Plattform")

Die `PageRenderer` -Klasse macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn die Xamarin.Forms-Seite erstellt wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert eine `ElementChangedEventArgs` Parameter mit `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, die den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, die den Renderer *ist* angefügt wird, bzw. In diesem Beispiel die `OldElement` Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `CameraPage` Instanz.

Eine außer Kraft gesetzte Version von der `OnElementChanged` -Methode in der die `CameraPageRenderer` Klasse ist der Ort, an die systemeigene Seite anpassen. Ein Verweis auf die Instanz der Xamarin.Forms-Seite, die gerendert wird erhalten Sie über die `Element` Eigenschaft.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: der Typname der Xamarin.Forms-Startseite, die gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Die folgenden Abschnitte beschreiben die Implementierung der `CameraPageRenderer` benutzerdefinierter Renderer für jede Plattform.

### <a name="creating-the-page-renderer-on-ios"></a>Erstellen die Seiten-Renderer für iOS

Das folgende Codebeispiel zeigt die Seite-Renderer für die iOS-Plattform:

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` Methode instanziiert ein iOS `UIViewController` Steuerelement. Der live-Kamera-Stream wird nur gerendert werden, vorausgesetzt, dass der Renderer ist nicht mit einem vorhandenen Xamarin.Forms-Element bereits angefügt wurde und vorausgesetzt, dass eine Seiteninstanz vorhanden ist, wird, indem der benutzerdefinierte Renderer gerendert wird.

Die Seite wird dann durch eine Reihe von Methoden, mit denen angepasst der `AVCapture` APIs zum Bereitstellen des livestreams von der Kamera und die Möglichkeit, ein Foto aufzunehmen.

### <a name="creating-the-page-renderer-on-android"></a>Erstellen die Seiten-Renderer für Android

Das folgende Codebeispiel zeigt die Seite-Renderer für die Android-Plattform:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` Methode instanziiert eine Android `ViewGroup` -Steuerelement, das eine Gruppe von Sichten ist. Der live-Kamera-Stream wird nur gerendert werden, vorausgesetzt, dass der Renderer ist nicht mit einem vorhandenen Xamarin.Forms-Element bereits angefügt wurde und vorausgesetzt, dass eine Seiteninstanz vorhanden ist, wird, indem der benutzerdefinierte Renderer gerendert wird.

Die Seite wird dann durch den Aufruf einer Reihe von Methoden, mit denen angepasst der `Camera` -API zum Bereitstellen des livestreams von der Kamera und ein Foto, vor dem Aufzeichnen der `AddView` Methode wird aufgerufen, um die live-Kamera hinzuzufügen Benutzeroberfläche zum Streamen der `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>Erstellen die Seiten-Renderer für UWP

Das folgende Codebeispiel zeigt die Seiten-Renderer für UWP:

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

Der Aufruf der Basisklasse `OnElementChanged` Methode instanziiert ein `FrameworkElement` -Steuerelement, auf der Seite gerendert wird. Der live-Kamera-Stream wird nur gerendert werden, vorausgesetzt, dass der Renderer ist nicht mit einem vorhandenen Xamarin.Forms-Element bereits angefügt wurde und vorausgesetzt, dass eine Seiteninstanz vorhanden ist, wird, indem der benutzerdefinierte Renderer gerendert wird. Die Seite wird dann durch den Aufruf einer Reihe von Methoden, mit denen angepasst der `MediaCapture` -API zum Bereitstellen des livestreams von der Kamera und die Möglichkeit, ein Foto aufzunehmen, bevor die benutzerdefinierte Seite hinzugefügt wird die `Children` Auflistung für die Anzeige.

Wenn einen benutzerdefinierten Renderer zu implementieren, die von abgeleitet `PageRenderer` auf UWP, die `ArrangeOverride` Methode sollte auch um die Steuerelemente, anordnen implementiert werden, da die Basis-Renderer wissen nicht, was Sie mit ihnen möglichen Aktionen. Führt dazu, andernfalls eine leere Seite. Aus diesem Grund in diesem Beispiel die `ArrangeOverride` Methodenaufrufe der `Arrange` Methode für die `Page` Instanz.

> [!NOTE]
> Es ist wichtig, zum Stoppen und Verwerfen der Objekte, die Zugriff auf die Kamera in eine UWP-Anwendung bereitstellen. Bei unterlassen kann mit anderen Anwendungen beeinträchtigen, die versuchen, den Zugriff auf das Gerät die Kamera. Weitere Informationen finden Sie unter [zeigen die Kameravorschau](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde beschrieben, wie zum Erstellen eines benutzerdefinierten Renderers für das [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Seite ermöglichen Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben. Ein `ContentPage` ist ein visuelles Element, das zeigt eine einzelnen Ansicht und der Großteil des Bildschirms einnimmt.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererContentPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
