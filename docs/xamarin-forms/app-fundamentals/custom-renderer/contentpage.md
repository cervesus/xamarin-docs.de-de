---
title: Anpassen einer ContentPage verwendet wird
description: Eine ContentPage verwendet wird, ist ein visuelles Element, das eine einzelne Ansicht und nimmt den größten Teil der Bildschirm. Dieser Artikel veranschaulicht, wie einen benutzerdefinierten Renderer auf der Seite ContentPage verwendet und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0f4de4594e8abb8d0ee03690e5829193c51a3736
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-contentpage"></a>Anpassen einer ContentPage verwendet wird

_Eine ContentPage verwendet wird, ist ein visuelles Element, das eine einzelne Ansicht und nimmt den größten Teil der Bildschirm. Dieser Artikel veranschaulicht, wie einen benutzerdefinierten Renderer auf der Seite ContentPage verwendet und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben._

Jedes Xamarin.Forms-Steuerelement verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) einer Xamarin.Forms-Anwendung in iOS gerendert wird die `PageRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `UIViewController` Steuerelement. Auf der Android-Plattform die `PageRenderer` Klasse instanziiert einen `ViewGroup` Steuerelement. Auf die universelle Windows-Plattform (UWP), die `PageRenderer` Klasse instanziiert einen `FrameworkElement` Steuerelement. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) und das entsprechende systemeigene Steuerelemente, die ihn implementieren:

![](contentpage-images/contentpage-classes.png "Beziehung zwischen ContentPage verwendet Klasse und Implementieren von systemeigenen Steuerelementen")

Des Renderingprozesses kann Vorteil ausgeführt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Xamarin.Forms_Page) einer Xamarin.Forms-Seite.
1. [Nutzen](#Consuming_the_Xamarin.Forms_Page) die Seite Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Page_Renderer_on_each_Platform) der benutzerdefinierten Renderers für die Seite auf den verschiedenen Plattformen.

Jedes Element wird nun wiederum zum Implementieren der besprochen werden eine `CameraPage` einer live-Kamera sowie die Möglichkeit zum Erfassen eines Fotos bereitstellt.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Erstellen das Xamarin.Forms-Seite

Eine unveränderte [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) kann dem freigegebenen Xamarin.Forms-Projekt hinzugefügt werden, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Auf ähnliche Weise, die Code-Behind-Datei für die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) sollte auch unverändert bleiben, wie im folgenden Codebeispiel gezeigt:

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

Das folgende Codebeispiel zeigt, wie die Seite in c# erstellt werden kann:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Eine Instanz von der `CameraPage` wird verwendet, um die live-Kamera auf jeder Plattform anzuzeigen. Anpassung des Steuerelements durchgeführt werden in den benutzerdefinierten Renderer, sodass keine weitere Implementierungsdetails in erforderlich ist der `CameraPage` Klasse.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Nutzen die Seite "Xamarin.Forms"

Die leere `CameraPage` muss von der Anwendung Xamarin.Forms angezeigt werden. Dies tritt auf, wenn auf eine Schaltfläche auf der `MainPage` Instanz abgerufen, die wiederum führt die `OnTakePhotoButtonClicked` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Dieser Code einfach navigiert zu der `CameraPage`, auf welche benutzerdefinierten Renderer die Darstellung der Seite auf jeder Plattform angepasst wird.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Erstellen die Seite Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `PageRenderer` Klasse.
1. Überschreiben Sie die `OnElementChanged` -Methode, die die native Seite und Schreiben Logik zum Anpassen der Seite gerendert wird. Die `OnElementChanged` Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die Seite Renderer-Klasse, um anzugeben, dass es zum Rendern der Seite Xamarin.Forms verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Ist er optional einen Seiten-Renderer in jedem plattformprojekt bereitstellen. Wenn ein Renderer für Seiten nicht registriert ist, wird der Standardrenderer für die Seite verwendet werden.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehung zwischen ihnen:

![](contentpage-images/solution-structure.png "CameraPage benutzerdefinierten Renderer Projekt Zuständigkeiten")

Die `CameraPage` Clientplattform-spezifische Instanz gerendert wird `CameraPageRenderer` Klassen, die Ableitung der `PageRenderer` Klasse für diese Plattform. Dies führt in den einzelnen `CameraPage` Instanz mit einem live-Kamera Feed gerendert wird, wie in den folgenden Screenshots dargestellt:

![](contentpage-images/screenshots.png "CameraPage auf jeder Plattform")

Die `PageRenderer` -Klasse verfügbar macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn die Seite "Xamarin.Forms" erstellt wird, um das entsprechende systemeigene Steuerelement rendern. Diese Methode nimmt ein `ElementChangedEventArgs` Parameter, enthält `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, den Renderer *ist* angefügt sind, bzw. In der beispielanwendung der `OldElement` -Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `CameraPage` Instanz.

Eine überschriebene Version von der `OnElementChanged` Methode in der `CameraPageRenderer` Klasse ist die Stelle, um die Seite "native" durchführen. Ein Verweis auf die Xamarin.Forms-Seite-Instanz, die gerendert wird abgerufen werden kann, durch die `Element` Eigenschaft.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: der Typname der Xamarin.Forms Seite gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

Die folgenden Abschnitte beschreiben die Implementierung der `CameraPageRenderer` benutzerdefinierten Renderers für jede Plattform.

### <a name="creating-the-page-renderer-on-ios"></a>Erstellen die Seiten-Renderer für iOS

Das folgende Codebeispiel zeigt die Seiten-Renderer für die iOS-Plattform:

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
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` -Methode instanziiert ein iOS `UIViewController` Steuerelement. Der live-Kamera Stream wird nur gerendert werden, vorausgesetzt, dass der Renderer ist nicht mit einem vorhandenen Xamarin.Forms-Element bereits angefügt und vorausgesetzt, dass eine Seiteninstanz vorhanden, vom benutzerdefinierten Renderer gerendert wird ist.

Die Seite wird dann durch eine Reihe von Methoden, mit denen angepasst die `AVCapture` -APIs zum Bereitstellen des livestreams von der Kamera und die Möglichkeit, ein Foto zu erfassen.

### <a name="creating-the-page-renderer-on-android"></a>Erstellen die Seiten-Renderer für Android

Das folgende Codebeispiel zeigt die Seiten-Renderer für die Android-Plattform:

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
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` -Methode instanziiert ein Android `ViewGroup` Steuerelement, das eine Gruppe von Ansichten ist. Der live-Kamera Stream wird nur gerendert werden, vorausgesetzt, dass der Renderer ist nicht mit einem vorhandenen Xamarin.Forms-Element bereits angefügt und vorausgesetzt, dass eine Seiteninstanz vorhanden, vom benutzerdefinierten Renderer gerendert wird ist.

Die Seite wird dann durch den Aufruf einer Reihe von Methoden, mit denen angepasst der `Camera` -API zum Bereitstellen des livestreams von der Kamera und die Möglichkeit, ein Foto vor dem Erfassen der `AddView` Methode wird aufgerufen, um die live-Kamera hinzufügen UI zum Streamen der `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>Erstellen die Seiten-Renderer für universelle Windows-Plattform

Im folgenden Codebeispiel wird veranschaulicht, die Seiten-Renderer für universelle Windows-Plattform:

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

Der Aufruf der Basisklasse `OnElementChanged` -Methode instanziiert einen `FrameworkElement` -Steuerelement, auf denen die Seite gerendert wird. Der live-Kamera Stream wird nur gerendert werden, vorausgesetzt, dass der Renderer ist nicht mit einem vorhandenen Xamarin.Forms-Element bereits angefügt und vorausgesetzt, dass eine Seiteninstanz vorhanden, vom benutzerdefinierten Renderer gerendert wird ist. Die Seite wird dann durch den Aufruf einer Reihe von Methoden, mit denen angepasst der `MediaCapture` -API zum Bereitstellen des livestreams von der Kamera und die Möglichkeit, ein Foto zu erfassen, bevor der angepasste Seite hinzugefügt wird die `Children` Auflistung für die Anzeige.

Wenn einen benutzerdefinierten Renderer implementieren, die abgeleitet `PageRenderer` für universelle Windows-Plattform, die `ArrangeOverride` Methode sollte auch um die Steuerelemente der Seite anordnen implementiert werden, da der Basis-Renderer was damit geschehen nicht bekannt ist. Andernfalls führt eine leere Seite. Aus diesem Grund in diesem Beispiel die `ArrangeOverride` Methodenaufrufe der `Arrange` Methode für die `Page` Instanz.

> [!NOTE]
> Es ist wichtig, beenden und löschen Sie die Objekte, die Zugriff auf die Kamera in einer uwp-Anwendung bereitstellen. Bei unterlassen kann mit anderen Anwendungen beeinträchtigen, die versuchen, die Kamera des Geräts zugreifen. Weitere Informationen finden Sie unter [anzeigen die Kameravorschau](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie erstellen ein benutzerdefiniertes Renderers für das [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Seite, und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben. Ein `ContentPage` ist ein visuelles Element, das eine einzelne Ansicht und nimmt den größten des Bildschirms.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererContentPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
