---
title: Implementieren einer Ansicht
description: In diesem Artikel wird erläutert, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, mit dem das Gerät die Kamera einen Vorschau-Videodatenstrom anzuzeigen.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 8ee9926eb3b726673711141e7c75a68b607d02d3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994701"
---
# <a name="implementing-a-view"></a>Implementieren einer Ansicht

_Xamarin.Forms benutzerdefinierte Steuerelemente der Benutzeroberfläche sollte von der Ansichtsklasse abgeleitet werden, die verwendet wird, Layouts und Steuerelemente auf dem Bildschirm platziert. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, mit dem das Gerät die Kamera einen Vorschau-Videodatenstrom anzuzeigen._

Alle Xamarin.Forms-Ansicht ist eine begleitende-Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `View` ](xref:Xamarin.Forms.View) einer Xamarin.Forms-Anwendung unter iOS gerendert wird die `ViewRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `UIView` Steuerelement. Auf der Android-Plattform die `ViewRenderer` Klasse instanziiert ein systemeigenes `View` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `ViewRenderer` Klasse instanziiert ein systemeigenes `FrameworkElement` Steuerelement. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `View` ](xref:Xamarin.Forms.View) und die entsprechenden systemeigenen Steuerelemente, die sie implementieren:

![](view-images/view-classes.png "Beziehung zwischen der Ansichtsklasse und seine Implementierung systemeigenen Klassen")

Das Rendern zu kann verwendet werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `View` ](xref:Xamarin.Forms.View) auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Control) eines benutzerdefinierten Xamarin.Forms-Steuerelements.
1. [Nutzen](#Consuming_the_Custom_Control) das benutzerdefinierte Steuerelement von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das Steuerelement auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer `CameraPreview` Renderer, der einen Vorschau-Videodatenstrom des Geräts Kamera angezeigt. Tippen Sie auf den Videostream wird beendet, und starten Sie ihn.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Erstellen des benutzerdefinierten Steuerelements

Ein benutzerdefiniertes Steuerelement erstellt werden kann, indem Unterklassen der [ `View` ](xref:Xamarin.Forms.View) Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die `CameraPreview` benutzerdefiniertes Steuerelement wird in das portable Klassenbibliotheksprojekt (PCL) erstellt und definiert die API für das Steuerelement. Das benutzerdefinierte Steuerelement macht eine `Camera` -Eigenschaft, die zum Steuern der verwendet wird, ob der Videostream in die frontkamera oder die rückseitige Kamera des Geräts angezeigt werden soll. Wenn kein Wert, für angegeben wird die `Camera` -Eigenschaft, wenn das Steuerelement erstellt wird, wird standardmäßig die hintere Kamera angeben.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen das benutzerdefinierte Steuerelement

Die `CameraPreview` benutzerdefiniertes Steuerelement kann verwiesen werden in XAML im PCL-Projekt durch deklarieren einen Namespace für den Speicherort der und verwenden das Namespacepräfix für das benutzerdefinierte Steuerelement-Element. Das folgende Codebeispiel zeigt die `CameraPreview` benutzerdefiniertes Steuerelement kann von einer XAML-Seite verwendet werden:

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

Die `local` Namespacepräfix kann eine beliebige Bezeichnung. Allerdings die `clr-namespace` und `assembly` -Werte müssen die Details des benutzerdefinierten Steuerelements entsprechen. Sobald der Namespace deklariert wird, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

Das folgende Codebeispiel zeigt die `CameraPreview` benutzerdefiniertes Steuerelement durch einen C# -Code genutzt werden kann:

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

Eine Instanz von der `CameraPreview` benutzerdefiniertes Steuerelement zum Anzeigen der Preview-Videodatenstrom des Geräts Kamera verwendet werden. Abgesehen von optional Angabe eines Werts für die `Camera` -Eigenschaft, die Anpassung des Steuerelements wird in den benutzerdefinierten Renderer durchgeführt werden.

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt zur Erstellung von plattformspezifischen kamerasteuerelemente Vorschau hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `ViewRenderer<T1,T2>` -Klasse, die das benutzerdefinierte Steuerelement rendert. Das erste Argument des Typs muss das benutzerdefinierte Steuerelement, in diesem Fall der Renderer ist `CameraPreview`. Das zweite Typargument muss das native Steuerelement, das das benutzerdefinierte Steuerelement implementiert wird.
1. Überschreiben der `OnElementChanged` -Methode, die benutzerdefinierte Steuerelement und Schreiben-Logik, um es anzupassen rendert. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms-Elemente ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse des Steuerelements verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in dem jedes plattformprojekt beim Rendern einer [Ansicht](xref:Xamarin.Forms.View) Element.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](view-images/solution-structure.png "Zuständigkeiten von CameraPreview benutzerdefinierten Renderer-Projekt")

Die `CameraPreview` benutzerdefiniertes Steuerelement gerendert wird, durch das plattformspezifische, alle abgeleiteten Renderingklassen der `ViewRenderer` -Klasse für jede Plattform. Dies führt in jeder `CameraPreview` benutzerdefiniertes Steuerelement mit plattformspezifischen Steuerelementen gerendert wird, wie in den folgenden Screenshots gezeigt:

![](view-images/screenshots.png "CameraPreview auf jeder Plattform")

Die `ViewRenderer` -Klasse macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das benutzerdefinierte Steuerelement mit Xamarin.Forms erstellt wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert eine `ElementChangedEventArgs` Parameter mit `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, die den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, die den Renderer *ist* angefügt wird, bzw. In der beispielanwendung die `OldElement` Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `CameraPreview` Instanz.

Eine außer Kraft gesetzte Version von der `OnElementChanged` -Methode, in den einzelnen plattformspezifischen rendererklassen an, ist der Ort der nativen Steuerelements Instanziierung und Anpassung. Die `SetNativeControl` Methode sollte verwendet werden, um das native Steuerelement zu instanziieren und diese Methode weist auch steuerelementreferenz der `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

In einigen Fällen die `OnElementChanged` -Methode mehrere Male aufgerufen werden kann. Aus diesem Grund, um Speicherverluste zu verhindern, Sorgfalt bei der Instanziierung eines neuen nativen Steuerelements. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

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

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control` `null` lautet. Ein Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Ebenso sollten alle Ereignishandler, die abonniert wurden nur gekündigt werden Wenn ändert sich das Element, dem der Renderer angefügt ist. Dieser Ansatz hilft, einen leistungsfähigen benutzerdefinierten Renderer erstellen, der durch Speicherverluste beeinträchtigt nicht.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: den Typnamen des benutzerdefinierten Xamarin.Forms-Steuerelements gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Den folgenden Abschnitten werden die Implementierung der einzelnen plattformspezifischen benutzerdefinierten Renderer-Klasse.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die iOS-Plattform:

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

Vorausgesetzt, dass die `Control` -Eigenschaft ist `null`, `SetNativeControl` aufgerufen, um ein neues instanziieren `UICameraPreview` Steuerelement und weisen Sie einen Verweis darauf in der `Control` Eigenschaft. Die `UICameraPreview` Steuerelement ist ein benutzerdefiniertes plattformspezifische-Steuerelement, das verwendet die `AVCapture` APIs, um die Vorschau des Datenstroms von der Kamera zu bieten. Macht eine `Tapped` -Ereignis, das vom verarbeitet wird die `OnCameraPreviewTapped` Methode zum Beenden und starten die Videovorschau aus, wenn es angetippt wird. Die `Tapped` Ereignis ist abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, und abbestellt nur, wenn das Element der Renderer Änderungen zugeordnet ist.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die Android-Plattform:

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

Vorausgesetzt, dass die `Control` -Eigenschaft ist `null`, wird die `SetNativeControl` aufgerufen, um ein neues instanziieren `CameraPreview` steuern und weisen Sie einen Verweis, um die `Control` Eigenschaft. Die `CameraPreview` Steuerelement ist ein benutzerdefiniertes plattformspezifische-Steuerelement, das verwendet die `Camera` -API, um die Vorschau des Datenstroms von der Kamera zu bieten. Die `CameraPreview` Steuerelement dann konfiguriert ist, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Diese Konfiguration umfasst das Erstellen eines neuen systemeigenes `Camera` das Objekt, für den Zugriff auf eine bestimmte Hardware Kamera und registrieren einen Ereignishandler zum Verarbeiten der `Click` Ereignis. Dieser Handler wird wiederum beendet und die Videovorschau starten, wenn es angetippt wird. Die `Click` Ereignis ist abbestellt, wenn Änderungen der Xamarin.Forms-Element der Renderer angefügt ist.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen benutzerdefinierten Renderer auf UWP

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für UWP:

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

Vorausgesetzt, dass die `Control` -Eigenschaft ist `null`, ein neues `CaptureElement` instanziiert wird und die `SetupCamera` -Methode aufgerufen wird, verwendet der `MediaCapture` -API, um die Vorschau des Datenstroms von der Kamera zu bieten. Die `SetNativeControl` Methode wird aufgerufen, um weisen Sie einen Verweis auf die `CaptureElement` -Instanz, auf die `Control` Eigenschaft. Die `CaptureElement` -Steuerelement stellt einen `Tapped` -Ereignis, das vom verarbeitet wird die `OnCameraPreviewTapped` Methode zum Beenden und starten die Videovorschau aus, wenn es angetippt wird. Die `Tapped` Ereignis ist abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird, und abbestellt nur, wenn das Element der Renderer Änderungen zugeordnet ist.

> [!NOTE]
> Es ist wichtig, zum Stoppen und Verwerfen der Objekte, die Zugriff auf die Kamera in eine UWP-Anwendung bereitstellen. Bei unterlassen kann mit anderen Anwendungen beeinträchtigen, die versuchen, den Zugriff auf das Gerät die Kamera. Weitere Informationen finden Sie unter [zeigen die Kameravorschau](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellen, mit dem das Gerät die Kamera einen Vorschau-Videodatenstrom anzuzeigen. Xamarin.Forms benutzerdefinierte Steuerelemente der Benutzeroberfläche sollten leiten Sie von der [ `View` ](xref:Xamarin.Forms.View) -Klasse, die Layouts und Steuerelemente auf dem Bildschirm platziert.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
