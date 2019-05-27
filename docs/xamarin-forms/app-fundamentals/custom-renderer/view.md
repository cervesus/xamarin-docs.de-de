---
title: Implementieren einer Ansicht
description: In diesem Artikel wird erläutert, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Xamarin.Forms-Steuerelement erstellen können, mit dem Sie eine Vorschau eines Videostreams von der Kamera des Geräts anzeigen können.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 88be56cae52e881792ec7a187ef7e158790e8a1b
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926593"
---
# <a name="implementing-a-view"></a>Implementieren einer Ansicht

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/View/)

_Benutzerdefinierte Xamarin.Forms-Benutzeroberflächen-Steuerelemente sollten von der View-Klasse abgeleitet werden, die verwendet wird, um Layout und Steuerelemente einer Anzeige hinzuzufügen. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Xamarin.Forms-Steuerelement erstellen können, mit dem Sie eine Vorschau eines Videostreams von der Kamera des Geräts anzeigen können._

Jede Xamarin.Forms-Ansicht verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`View`](xref:Xamarin.Forms.View)-Objekts durch eine Xamarin.Forms-Anwendung wird in iOS die `ViewRenderer`-Klasse instanziiert, wodurch wiederum ein natives `UIView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `ViewRenderer`-Klasse ein natives `View`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `ViewRenderer`-Klasse ein natives `FrameworkElement`-Steuerelement. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Renderer Base Classes and Native Controls (Rendererbasisklassen und native Steuerelemente)](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`View`](xref:Xamarin.Forms.View)-Objekt und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](view-images/view-classes.png "Beziehungen zwischen der View-Klasse und den nativen Klassen, die diese implementieren")

Der Renderingprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für eine [`View`](xref:Xamarin.Forms.View)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Der Prozess dafür sieht wie folgt aus:

1. [Erstellen](#Creating_the_Custom_Control) Sie ein benutzerdefiniertes Xamarin.Forms-Steuerelement.
1. [Nutzen](#Consuming_the_Custom_Control) Sie das benutzerdefinierte Steuerelement über Xamarin.Forms.
1. [Erstellen](#Creating_the_Custom_Renderer_on_each_Platform) Sie den benutzerdefinierten Renderer für das Steuerelement auf jeder Plattform.

Im Folgenden wird jedes Element zum Implementieren eines `CameraPreview`-Renderers einzeln erläutert, der einen Vorschauvideostream der Kamera des Geräts zeigt. Durch Tippen auf den Videostream wird er gestartet und gestoppt.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Erstellen des benutzerdefinierten Steuerelements

Sie können ein benutzerdefiniertes Steuerelement erstellen, indem Sie die [`View`](xref:Xamarin.Forms.View)-Klasse wie in folgendem Codebeispiel als Unterklasse verwenden:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

Das benutzerdefinierte Steuerelement `CameraPreview` wird im .NET Standard-Bibliotheksprojekt erstellt und definiert die API für das Steuerelement. Das benutzerdefinierte Steuerelement stellt eine `Camera`-Eigenschaft zur Verfügung, die verwendet wird, um zu steuern, ob der Videostream der vorderen oder hinteren Kamera des Geräts angezeigt werden soll. Wenn bei der Erstellung des Steuerelements kein Wert für die `Camera`-Eigenschaft angegeben ist, wird standardmäßig die hintere Kamera festgelegt.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen des benutzerdefinierten Steuerelements

Sie können auf das benutzerdefinierte Steuerelement `CameraPreview` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das benutzerdefinierte Steuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `CameraPreview` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf das benutzerdefinierte Steuerelement zu verweisen.

Im folgenden Codebeispiel wird veranschaulicht, wie das benutzerdefinierte Steuerelement `CameraPreview` von einer C#-Seite genutzt werden kann:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Eine Instanz des benutzerdefinierten Steuerelements `CameraPreview` wird zum Anzeigen des Vorschauvideostreams der Gerätekamera verwendet. Abgesehen von der optionalen Angabe eines Werts für die `Camera`-Eigenschaft, findet die Anpassung des Steuerelements im benutzerdefinierten Renderer statt.

Ein benutzerdefinierter Renderer kann nun zu jedem Anwendungsprojekt hinzugefügt werden, um plattformspezifische Steuerelemente für die Kameravorschau zu erstellen.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `ViewRenderer<T1,T2>`-Klasse, die das benutzerdefinierte Steuerelement rendert. Das erste Typargument sollte das benutzerdefinierte Steuerelement sein, für das der Renderer erstellt werden soll. In diesem Fall ist das `CameraPreview`. Das zweite Typargument sollte das native Steuerelement sein, das das benutzerdefinierte Steuerelement implementiert.
1. Überschreiben Sie die `OnElementChanged`-Methode, die das benutzerdefinierte Steuerelement rendert, und schreiben Sie Logik, um dieses anzupassen. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Bei den meisten Xamarin.Forms-Elementen ist das Angeben eines benutzerdefinierten Renderers in jedem Plattformprojekt optional. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet. Benutzerdefinierte Renderer sind jedoch in jedem Plattformprojekt erforderlich, wenn ein [View](xref:Xamarin.Forms.View)-Element gerendert wird.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](view-images/solution-structure.png "Projektzuständigkeiten beim benutzerdefinierten Steuerelement CameraPreview")

Das benutzerdefinierte Steuerelement `CameraPreview` wird von plattformspezifischen Rendererklassen gerendert, die alle von der `ViewRenderer`-Klasse für jede Plattform abgeleitet werden. Das führt dazu, dass jedes benutzerdefinierte `CameraPreview`-Steuerelement mit plattformspezifischen Steuerelementen gerendert wird. Dies wird in folgenden Screenshots veranschaulicht:

![](view-images/screenshots.png "CameraPreview auf jeder Plattform")

Die `ViewRenderer`-Klasse macht die `OnElementChanged`-Methode verfügbar, die bei der Erstellung des benutzerdefinierten Xamarin.Forms-Steuerelements aufgerufen wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert einen `ElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element dar, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element, an das der Renderer angefügt *ist*. In der Beispielanwendung weist die `OldElement`-Eigenschaft den Wert `null` auf, und die `NewElement`-Eigenschaft enthält einen Verweis auf die `CameraPreview`-Instanz.

Das native Steuerelement sollte bei der überschriebenen Version der `OnElementChanged`-Methode in jeder plattformspezifischen Rendererklasse instanziiert und angepasst werden. Mit der `SetNativeControl`-Methode sollte das native Steuerelement instanziiert und die Steuerelementreferenz der Eigenschaft `Control` zugewiesen werden. Zusätzlich kann ein Verweis auf das zu rendernde Xamarin.Forms-Steuerelement über die `Element`-Eigenschaft abgerufen werden.

In einigen Fällen kann die `OnElementChanged`-Methode mehrmals aufgerufen werden. Daher müssen Sie bei der Instanziierung eines nativen Steuerelements sorgfältig vorgehen, um Arbeitsspeicherverluste zu vermeiden. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control` `null` lautet. Ein Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Mit diesem Ansatz kann ein leistungsstarker benutzerdefinierter Renderer erstellt werden, der nicht durch Speicherverluste beeinträchtigt wird.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen des zu rendernden benutzerdefinierten Xamarin.Forms-Steuerelements und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird die Implementierung jeder plattformspezifischen, benutzerdefinierten Rendererklasse erläutert.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Sofern die `Control`-Eigenschaft den Wert `null` aufweist, wird die `SetNativeControl`-Methode aufgerufen, um ein neues `UICameraPreview`-Steuerelement zu instanziieren und der `Control`-Eigenschaft einen Verweis auf dieses Steuerelement zuzuweisen. Das `UICameraPreview`-Steuerelement ist ein plattformspezifisches, benutzerdefiniertes Steuerelement, das die `AVCapture`-APIs nutzt, um den Vorschauvideostream der Kamera bereitzustellen. Es stellt ein `Tapped`-Ereignis zur Verfügung, das von der `OnCameraPreviewTapped`-Methode verarbeitet wird, um die Videovorschau zu stoppen und zu starten, wenn sie angetippt wird. Das `Tapped`-Ereignis wird abonniert, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, und das Abonnement wird nur gekündigt, wenn das Element, an das der Renderer angefügt ist, geändert wird.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Sofern die `Control`-Eigenschaft den Wert `null` aufweist, wird die `SetNativeControl`-Methode aufgerufen, um ein neues `CameraPreview`-Steuerelement zu instanziieren und der `Control`-Eigenschaft einen Verweis auf dieses Steuerelement zuzuweisen. Das `CameraPreview`-Steuerelement ist ein plattformspezifisches, benutzerdefiniertes Steuerelement, das die `Camera`-API nutzt, um den Vorschauvideostream der Kamera bereitzustellen. Das `CameraPreview`-Steuerelement wird dann konfiguriert, vorausgesetzt der benutzerdefinierte Renderer wird an ein neues Xamarin.Forms-Element angefügt. Zu dieser Konfiguration gehört das Erstellen eines neuen nativen `Camera`-Objekts, um auf eine bestimmte Hardwarekamera zuzugreifen, und das Registrieren eines Ereignishandlers, um das `Click`-Ereignis zu verarbeiten. Dieser Handler stoppt und startet die Videovorschau wiederum, wenn darauf getippt wird. Das Abonnement des `Click`-Ereignisses wird gekündigt, wenn das Xamarin.Forms-Element, dem der Renderer angefügt ist, geändert wird.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen des benutzerdefinierten Renderers auf der UWP

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die UWP veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

Sofern die `Control`-Eigenschaft den Wert `null` aufweist, wird eine neue `CaptureElement`-Klasse instanziiert und die `SetupCamera`-Methode wird aufgerufen, die die `MediaCapture`-API verwendet, um den Vorschauvideostream der Kamera bereitzustellen. Anschließend wird die `SetNativeControl`-Methode aufgerufen, um der `Control`-Eigenschaft einen Verweis auf die `CaptureElement`-Instanz zuzuweisen. Das `CaptureElement`-Steuerelement stellt ein `Tapped`-Ereignis zur Verfügung, das von der `OnCameraPreviewTapped`-Methode verarbeitet wird, um die Videovorschau zu stoppen und zu starten, wenn sie angetippt wird. Das `Tapped`-Ereignis wird abonniert, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, und das Abonnement wird nur gekündigt, wenn das Element, an das der Renderer angefügt ist, geändert wird.

> [!NOTE]
> Es ist wichtig, die Objekte, die den Zugriff auf die Kamera ermöglichen, in einer UWP-Anwendung zu stoppen und zu löschen. Wenn dies nicht geschieht, kann dies andere Anwendungen beeinträchtigen, die versuchen, auf die Kamera des Geräts zuzugreifen. Weitere Informationen finden Sie unter [Display the camera preview (Anzeigen der Kameravorschau)](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Xamarin.Forms-Steuerelement erstellen können, mit dem Sie eine Vorschau eines Videostreams von der Kamera des Geräts anzeigen können. Benutzerdefinierte Xamarin.Forms-Benutzeroberflächen-Steuerelemente sollten von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet werden, die verwendet wird, um Layout und Steuerelemente einer Anzeige hinzuzufügen.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererView (CustomRendererView (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/View/)
