---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 50e28291d72550264e3806c0911f59a57c6d8bf0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136330"
---
# <a name="customizing-a-contentpage"></a>Anpassen einer ContentPage

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)

_Eine ContentPage ist ein visuelles Element, das eine Ansicht anzeigt, die den Großteil des Bildschirms einnimmt. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für die ContentPage-Seite erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können._

Jedes Xamarin.Forms-Steuerelement verfügt über einen entsprechenden Renderer für jede Plattform, die eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekts durch eine Xamarin.Forms-App wird in iOS die `PageRenderer`-Klasse instanziiert, wodurch wiederum ein natives `UIViewController`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `PageRenderer`-Klasse ein `ViewGroup`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `PageRenderer`-Klasse ein `FrameworkElement`-Steuerelement. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Rendererbasisklassen und native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](contentpage-images/contentpage-classes.png "Relationship Between ContentPage Class and Implementing Native Controls")

Der Renderprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Xamarin.Forms_Page) Sie eine Xamarin.Forms-Seite.
1. [Nutzen](#Consuming_the_Xamarin.Forms_Page) Sie die Seite über Xamarin.Forms.
1. [Erstellen](#Creating_the_Page_Renderer_on_each_Platform) Sie einen benutzerdefinierten Renderer für die Seite auf jeder Plattform.

Jedes Element wird nun nacheinander besprochen, um ein `CameraPage`-Element zu implementieren, das ein Livekamerafeed und die Möglichkeit zur Aufnahme eines Fotos bietet.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Erstellen der Xamarin.Forms-Seite

Wenn [`ContentPage`](xref:Xamarin.Forms.ContentPage) unverändert bleibt, kann das Element dem freigegebenen Xamarin.Forms-Projekt hinzugefügt werden, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Ebenso sollte die CodeBehind-Datei für das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Element ebenfalls unverändert bleiben, wie im folgenden Codebeispiel gezeigt:

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

Das folgende Codebeispiel veranschaulicht, wie die Seite in C# erstellt werden kann:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Eine Instanz vom `CameraPage`-Element wird verwendet, um den Livekamerafeed auf jeder Plattform anzuzeigen. Die Anpassung des Steuerelements erfolgt im benutzerdefinierten Renderer, sodass keine zusätzliche Implementierung in der `CameraPage`-Klasse erforderlich ist.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Nutzung der Xamarin.Forms-Seite

Das leere `CameraPage`-Element muss von der Xamarin.Forms-Anwendung angezeigt werden. Dies geschieht, wenn Sie auf eine Schaltfläche der `MainPage`-Instanz tippen, die wiederum die `OnTakePhotoButtonClicked`-Methode ausführt, wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Dieser Code navigiert einfach zum `CameraPage`-Element, über das benutzerdefinierte Renderer das Erscheinungsbild der Seite für jede Plattform anpassen.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Erstellen des Seitenrenderers für jede Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `PageRenderer`-Klasse.
1. Überschreiben Sie die `OnElementChanged`-Methode, die die native Seite rendert, und schreiben Sie Logik, um die Seite anzupassen. Die `OnElementChanged`-Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Fügen Sie der Klasse des Seitenrenderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern der Xamarin.Forms-Seite verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Dies ist optional, um einen Seitenrenderer in jedem Plattformprojekt bereitzustellen. Wenn kein Seitenrenderer registriert wurde, wird der Standardrenderer für die Seite verwendet.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](contentpage-images/solution-structure.png "CameraPage Custom Renderer Project Responsibilities")

Die `CameraPage`-Instanz wird von plattformspezifischen `CameraPageRenderer`-Klassen gerendert, die alle von der `PageRenderer`-Klasse für diese Plattform abgeleitet werden. Wie in den folgenden Screenshots zu sehen ist, wird folglich jede `CameraPage`-Instanz mit einem Livekamerafeed gerendert:

![](contentpage-images/screenshots.png "CameraPage on each Platform")

Die `PageRenderer`-Klasse macht die `OnElementChanged`-Methode verfügbar, die bei der Erstellung der Xamarin.Forms-Seite aufgerufen wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert einen `ElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element dar, an das der Renderer angefügt *ist*. In der Beispielanwendung ist die `OldElement`-Eigenschaft `null`, und die `NewElement`-Eigenschaft enthält einen Verweis auf die `CameraPage`-Instanz.

Die native Seitenanpassung kann in einer überschriebenen Version der `OnElementChanged`-Methode in der `CameraPageRenderer`-Klasse ausgeführt werden. Ein Verweis auf die zu rendernde Instanz der Xamarin.Forms-Seite kann über die `Element`-Eigenschaft bezogen werden.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen der zu rendernden Xamarin.Forms-Seite und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird die Implementierung des benutzerdefinierten `CameraPageRenderer`-Renderers für die einzelnen Plattformen erläutert.

### <a name="creating-the-page-renderer-on-ios"></a>Erstellen des Seitenrenderers unter iOS

Im folgenden Codebeispiel wird der Seitenrenderer für die iOS-Plattform veranschaulicht:

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

Der Aufruf der `OnElementChanged`-Methode der Basisklasse instanziiert ein `UIViewController`-iOS-Steuerelement. Der Livekamerastream wird nur dann gerendert, wenn der Renderer nicht bereits an ein vorhandenes Xamarin.Forms-Element angehängt ist und wenn eine Seiteninstanz existiert, die vom benutzerdefinierten Renderer gerendert wird.

Die Seite wird dann durch eine Reihe von Methoden angepasst, die die `AVCapture`-APIs verwenden, um den Livestream von der Kamera und die Möglichkeit zur Aufnahme eines Fotos bereitzustellen.

### <a name="creating-the-page-renderer-on-android"></a>Erstellen des Seitenrenderers unter Android

Im folgenden Codebeispiel wird der Seitenrenderer für die Android-Plattform veranschaulicht:

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

Der Aufruf der `OnElementChanged`-Methode der Basisklasse instanziiert ein `ViewGroup`-Android-Steuerelement, bei dem es sich um eine Gruppe von Ansichten handelt. Der Livekamerastream wird nur dann gerendert, wenn der Renderer nicht bereits an ein vorhandenes Xamarin.Forms-Element angehängt ist und wenn eine Seiteninstanz existiert, die vom benutzerdefinierten Renderer gerendert wird.

Die Seite wird dann durch den Aufruf einer Reihe von Methoden angepasst, die die `Camera`-API verwenden, um den Livestream der Kamera und die Möglichkeit zur Aufnahme eines Fotos bereitzustellen. Dies geschieht, bevor die `AddView`-Methode aufgerufen wird, um die Benutzeroberfläche für den Livekamerastream zum `ViewGroup`-Element hinzuzufügen. Beachten Sie, dass es bei Android zudem erforderlich ist, die `OnLayout`-Methode zu überschreiben, um Measure- und Layoutvorgänge für die Ansicht auszuführen. Weitere Informationen finden Sie im [Beispiel zum ContentPage-Renderer](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage).

### <a name="creating-the-page-renderer-on-uwp"></a>Erstellen des Seitenrenderers auf der UWP

Im folgenden Codebeispiel wird der Seitenrenderer für die UWP veranschaulicht:

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

Der Aufruf der `OnElementChanged`-Methode der Basisklasse instanziiert ein `FrameworkElement`-Steuerelement, für das die Seite gerendert wird. Der Livekamerastream wird nur dann gerendert, wenn der Renderer nicht bereits an ein vorhandenes Xamarin.Forms-Element angehängt ist und wenn eine Seiteninstanz existiert, die vom benutzerdefinierten Renderer gerendert wird. Die Seite wird dann durch den Aufruf einer Reihe von Methoden angepasst, die die `MediaCapture`-API verwenden, um den Livestream der Kamera und die Möglichkeit zur Aufnahme eines Fotos bereitzustellen. Dies geschieht, bevor die angepasste Seite zur `Children`-Sammlung für die Anzeige hinzugefügt wird.

Bei der Implementierung eines benutzerdefinierten Renderers, der von `PageRenderer` auf der UWP stammt, sollte auch die `ArrangeOverride`-Methode implementiert werden. Diese ordnet die Seitensteuerelemente an, da der Basisrenderer über keine Informationen zu deren Anordnung verfügt. Andernfalls ist das Ergebnis eine leere Seite. Aus diesem Grund ruft in diesem Beispiel die `ArrangeOverride`-Methode die `Arrange`-Methode auf der `Page`-Instanz auf.

> [!NOTE]
> Es ist wichtig, die Objekte, die den Zugriff auf die Kamera ermöglichen, in einer UWP-Anwendung zu stoppen und zu löschen. Wenn dies nicht geschieht, kann dies andere Anwendungen beeinträchtigen, die versuchen, auf die Kamera des Geräts zuzugreifen. Weitere Informationen finden Sie unter [Display the camera preview (Anzeigen der Kameravorschau)](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen benutzerdefinierten Renderer für die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Seite erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können. Ein `ContentPage`-Element ist ein visuelles Element, das eine Ansicht anzeigt, die den Großteil des Bildschirms einnimmt.

## <a name="related-links"></a>Verwandte Links

- [CustomRendererContentPage (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)
