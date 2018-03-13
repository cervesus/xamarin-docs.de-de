---
title: Implementieren eine Sicht
description: "Xamarin.Forms benutzerdefinierte Benutzeroberflächen-Steuerelemente müssen vom View-Klasse abgeleitet werden, die verwendet wird, um Layouts und Steuerelemente auf dem Bildschirm zu platzieren. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, die zum Anzeigen der Vorschau Videostreams von Kamera des Geräts verwendet wird."
ms.topic: article
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 30ee40272b5f7a6f5863dccf4dcae7431f6f536f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-a-view"></a>Implementieren eine Sicht

_Xamarin.Forms benutzerdefinierte Benutzeroberflächen-Steuerelemente müssen vom View-Klasse abgeleitet werden, die verwendet wird, um Layouts und Steuerelemente auf dem Bildschirm zu platzieren. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, die zum Anzeigen der Vorschau Videostreams von Kamera des Geräts verwendet wird._

Jede Ansicht Xamarin.Forms verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) einer Xamarin.Forms-Anwendung in iOS, gerendert wird die `ViewRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `UIView` Steuerelement. Auf der Android-Plattform die `ViewRenderer` Klasse instanziiert ein systemeigenes `View` Steuerelement. Auf Windows Phone und die universelle Windows-Plattform (UWP) die `ViewRenderer` Klasse instanziiert ein systemeigenes `FrameworkElement` Steuerelement. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) und das entsprechende systemeigene Steuerelemente, die ihn implementieren:

![](view-images/view-classes.png "Beziehung zwischen der Ansichtsklasse und die implementierenden Native Klassen")

Des Renderingprozesses kann verwendet werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Control) ein benutzerdefiniertes Steuerelement mit Xamarin.Forms.
1. [Nutzen](#Consuming_the_Custom_Control) das benutzerdefinierte Steuerelement aus Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das Steuerelement auf jeder Plattform.

Jedes Element wird nun wiederum zum Implementieren der besprochen werden eine `CameraPreview` Renderer, in der Vorschau Videostreams von Kamera des Geräts angezeigt. Tippen Sie auf den Videostream wird beendet, und starten Sie ihn.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Erstellen des benutzerdefinierten Steuerelements

Ein benutzerdefiniertes Steuerelement erstellt werden, indem Unterklassen der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die `CameraPreview` benutzerdefiniertes Steuerelement wird im Projekt portablen Klassenbibliothek (PCL) erstellt und definiert die API für das Steuerelement. Das benutzerdefinierte Steuerelement macht eine `Camera` -Eigenschaft, die zum Steuern von verwendet wird, ob von der Vorderseite oder rückseitige Kamera des Geräts der Videostream angezeigt werden soll. Wenn ein Wert nicht, für angegeben ist die `Camera` -Eigenschaft, wenn das Steuerelement erstellt wird, wird standardmäßig die rückseitige Kamera angeben.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen das benutzerdefinierte Steuerelement

Die `CameraPreview` benutzerdefiniertes Steuerelement im verwiesen werden kann in XAML PCL-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix für das benutzerdefinierte Steuerelement-Element. Im folgenden Codebeispiel wird veranschaulicht wie die `CameraPreview` benutzerdefiniertes Steuerelement genutzt werden kann, durch die entsprechende Verwendung von XAML-Seite:

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

Die `local` nichts Namespacepräfix kann benannt werden. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details des benutzerdefinierten Steuerelements übereinstimmen. Nach der Namespace deklariert ist, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

Im folgenden Codebeispiel wird veranschaulicht wie die `CameraPreview` benutzerdefiniertes Steuerelement genutzt werden kann, um eine C#-Seite:

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

Eine Instanz von der `CameraPreview` benutzerdefiniertes Steuerelement wird verwendet, um den Vorschau-Videostream von Kamera des Geräts angezeigt. Abgesehen von optional Angabe eines Werts für die `Camera` -Eigenschaft, die Anpassung des Steuerelements wird in den benutzerdefinierten Renderer durchgeführt werden.

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt plattformspezifischen Kamera Preview Steuerelemente erstellen hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `ViewRenderer<T1,T2>` -Klasse, die das benutzerdefinierte Steuerelement gerendert wird. Erste Typargument muss das benutzerdefinierte Steuerelement-Renderer, in diesem Fall ist `CameraPreview`. Das zweite Typargument sollten das systemeigene Steuerelement sein, das das benutzerdefinierte Steuerelement implementiert werden sollen.
1. Überschreiben Sie die `OnElementChanged` -Methode, die benutzerdefinierte Logik für Steuerelement und schreiben, um ihn anpassen rendert. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des benutzerdefinierten Steuerelements mit Xamarin.Forms verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms Elemente ist optional, geben Sie einen benutzerdefinierten Renderer in jedem plattformprojekt. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Basisklasse für das Steuerelement verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in jede plattformprojekt beim Rendern einer [Ansicht](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Element.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](view-images/solution-structure.png "CameraPreview benutzerdefinierten Renderer Projekt Zuständigkeiten")

Die `CameraPreview` plattformspezifischen Renderingklassen, die alle abgeleitet benutzerdefiniertes Steuerelement gerendert wird die `ViewRenderer` Klasse für jede Plattform. Dies führt in den einzelnen `CameraPreview` benutzerdefiniertes Steuerelement mit plattformspezifischen-Steuerelemente gerendert wird, wie in den folgenden Screenshots dargestellt:

![](view-images/screenshots.png "CameraPreview auf jeder Plattform")

Die `ViewRenderer` -Klasse verfügbar macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das benutzerdefinierte Steuerelement mit Xamarin.Forms erstellt wird, um das entsprechende systemeigene Steuerelement rendern. Diese Methode nimmt ein `ElementChangedEventArgs` Parameter, enthält `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, den Renderer *ist* angefügt sind, bzw. In der beispielanwendung der `OldElement` -Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `CameraPreview` Instanz.

Eine überschriebene Version von den `OnElementChanged` Methode in jeder Rendererklasse plattformspezifischen bietet die Möglichkeit zum vom systemeigenen Steuerelement Instanziierung sowie die Anpassung ausgeführt werden. Die `SetNativeControl` Methode sollte verwendet werden, um das systemeigene Steuerelement instanziiert und diese Methode wird auch des Steuerelementverweis auf die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

In einigen Fällen die `OnElementChanged` Methode kann mehrfach aufgerufen werden. Daher muss zum Verhindern von Speicherverlusten geachtet werden bei der Instanziierung eines neuen native Steuerelements. Im folgenden Codebeispiel ist der Ansatz zu sehen, der beim Instanziieren eines neuen nativen Steuerelements in einem benutzerdefinierten Renderer verwendet werden soll:

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

Ein neues natives Steuerelement sollte nur einmal instanziiert werden, wenn der Wert der Eigenschaft `Control` `null` lautet. Ein Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Ebenso sollten alle Ereignishandler, die abonniert wurden nur gekündigt werden beim ändert sich des Elements, dem der Renderer angefügt ist. Dieser Ansatz hilft ein benutzerdefinierten Renderers für leistungsstarke erstellen, das von Arbeitsspeicherverlusten nicht beeinträchtigt werden.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: der Typname, der das benutzerdefinierte Xamarin.Forms-Steuerelement gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

In den folgenden Abschnitten erläutern die Implementierung der einzelnen benutzerdefinierten Renderers Clientplattform-spezifische Klassen.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

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

Vorausgesetzt, dass die `Control` Eigenschaft ist `null`, `SetNativeControl` Methode wird aufgerufen, um ein neues instanziieren `UICameraPreview` Steuerelement und weisen Sie einen Verweis hinzu, um die `Control` Eigenschaft. Die `UICameraPreview` -Steuerelement ist ein benutzerdefiniertes plattformspezifischen-Steuerelement, das verwendet die `AVCapture` APIs den Zugriff auf die Vorschau-Stream, von der Kamera bereitstellen. Macht eine `Tapped` Ereignis, das vom übernommen wird die `OnCameraPreviewTapped` Methode zu beenden und Starten der Videovorschau, wenn es abgerufen werden. Die `Tapped` Ereignis ist für abonniert, wenn benutzerdefinierte Renderer auf ein neues Xamarin.Forms-Element angefügt und abbestellt nur, wenn das Element den Renderer Änderungen angefügt ist.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Das folgende Codebeispiel zeigt die benutzerdefinierten Renderers für das Android-Plattform:

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

Vorausgesetzt, dass die `Control` Eigenschaft ist `null`, `SetNativeControl` Methode wird aufgerufen, um ein neues instanziieren `CameraPreview` steuern und weisen Sie einen Verweis hinzu, um die `Control` Eigenschaft. Die `CameraPreview` -Steuerelement ist ein benutzerdefiniertes plattformspezifischen-Steuerelement, das verwendet die `Camera` -API, um die Vorschau-Stream, von der Kamera bereitstellen. Die `CameraPreview` Steuerelement ist konfiguriert, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Diese Konfiguration umfasst das Erstellen eines neuen systemeigenes `Camera` Objekt, das eine bestimmte Hardware Kamera zugreifen, und registrieren Sie einen Ereignishandler zum Verarbeiten der `Click` Ereignis. Wiederum dieser Handler beendet und der Videovorschau gestartet, wenn es abgerufen werden. Die `Click` Ereignis ist abbestellt, wenn das Xamarin.Forms-Element der Renderer Änderungen angefügt ist.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Erstellen von benutzerdefinierten Renderers für Windows Phone und universelle Windows-Plattform

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für Windows Phone-und uwp:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        MediaCapture mediaCapture;
        CaptureElement captureElement;
        CameraOptions cameraOptions;
        Application app;
        bool isPreviewing = false;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                ...
                captureElement = new CaptureElement ();
                captureElement.Stretch = Stretch.UniformToFill;

                InitializeAsync ();
                SetNativeControl (captureElement);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped (object sender, TappedRoutedEventArgs e)
        {
            if (isPreviewing) {
                await StopPreviewAsync ();
            } else {
                await StartPreviewAsync ();
            }
        }
        ...
    }
}
```

Vorausgesetzt, dass die `Control` Eigenschaft ist `null`, ein neues `CaptureElement` instanziiert wird und die `InitializeAsync` -Methode aufgerufen wird, verwendet die `MediaCapture` -API, um die Vorschau-Stream, von der Kamera bereitstellen. Die `SetNativeControl` Methode wird aufgerufen, um einen Verweis auf weisen die `CaptureElement` -Instanz, auf die `Control` Eigenschaft. Die `CaptureElement` -Steuerelement stellt eine `Tapped` Ereignis, das vom übernommen wird die `OnCameraPreviewTapped` Methode zu beenden und Starten der Videovorschau, wenn es abgerufen werden. Die `Tapped` Ereignis ist für abonniert, wenn benutzerdefinierte Renderer auf ein neues Xamarin.Forms-Element angefügt und abbestellt nur, wenn das Element den Renderer Änderungen angefügt ist.

> [!NOTE]
> Es ist wichtig, beenden und löschen Sie die Objekte, die Zugriff auf die Kamera in einem Windows Phone oder einer uwp-Anwendung bereitstellen. Bei unterlassen kann mit anderen Anwendungen beeinträchtigen, die versuchen, die Kamera des Geräts zugreifen. Weitere Informationen finden Sie unter und [Schnellstart: Aufzeichnen von Videos über die MediaCapture-API](https://msdn.microsoft.com/library/windows/apps/xaml/dn642092.aspx) für Windows-Runtime-Anwendungen und [anzeigen die Kameravorschau](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access) für uwp-Apps.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat gezeigt, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, die zum Anzeigen der Vorschau Videostreams von Kamera des Geräts verwendet wird. Xamarin.Forms benutzerdefinierte Benutzeroberflächen-Steuerelemente sollten Ableiten der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Klasse, die verwendet wird, um Layouts und Steuerelemente auf dem Bildschirm zu platzieren.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
