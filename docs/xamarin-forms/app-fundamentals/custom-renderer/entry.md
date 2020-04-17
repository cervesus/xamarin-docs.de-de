---
title: Anpassen eines Eintrags
description: Durch das Xamarin.Forms-Steuerelement „Entry“ kann eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das Entry-Steuerelement erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/26/2018
ms.openlocfilehash: dccc47d8ee69686fe2ac7409f75284c64c99a2d4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "70772004"
---
# <a name="customizing-an-entry"></a>Anpassen eines Eintrags

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-entry)

_Durch das Xamarin.Forms-Steuerelement „Entry“ kann eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das Entry-Steuerelement erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können._

Jedes Xamarin.Forms-Steuerelement verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelements durch eine Xamarin.Forms-App wird in iOS die `EntryRenderer`-Klasse instanziiert, wodurch wiederum ein natives `UITextField`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `EntryRenderer`-Klasse ein `EditText`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `EntryRenderer`-Klasse ein `TextBox`-Steuerelement. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Renderer Base Classes and Native Controls (Rendererbasisklassen und native Steuerelemente)](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](entry-images/entry-classes.png "Relationship Between Entry Control and Implementing Native Controls")

Der Renderingprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für das [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Custom_Entry_Control) Sie ein benutzerdefiniertes Xamarin.Forms-Steuerelement.
1. [Nutzen](#Consuming_the_Custom_Control) Sie das benutzerdefinierte Steuerelement über Xamarin.Forms.
1. [Erstellen](#Creating_the_Custom_Renderer_on_each_Platform) Sie den benutzerdefinierten Renderer für das Steuerelement auf jeder Plattform.

Im Folgenden wird auf jeden dieser Punkte ausführlicher eingegangen, um zu veranschaulichen, wie Sie ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement implementieren können, das auf jeder Plattform eine andere Hintergrundfarbe aufweist.

> [!IMPORTANT]
> In diesem Artikel wird erläutert, wie Sie einen einfachen benutzerdefinierten Renderer erstellen. Es ist jedoch nicht erforderlich, einen benutzerdefinierten Renderer zu erstellen, um ein `Entry`-Steuerelement zu implementieren, das auf jeder Plattform eine andere Hintergrundfarbe aufweist. Dies erreichen Sie einfacher, indem Sie mithilfe der [`Device`](xref:Xamarin.Forms.Device)-Klasse oder der `OnPlatform`-Markuperweiterung plattformspezifische Werte angeben. Weitere Informationen finden Sie unter [Providing Platform-Specific Values (Angeben plattformspezifischer Werte)](~/xamarin-forms/platform/device.md#providing-platform-specific-values) und [OnPlatform Markup Extension (OnPlatform-Markuperweiterung)](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Erstellen des benutzerdefinierten Entry-Steuerelements

Ein benutzerdefiniertes [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement kann erstellt werden, indem Sie das `Entry`-Steuerelement wie in folgendem Codebeispiel als Unterklasse verwenden:

```csharp
public class MyEntry : Entry
{
}
```

Das `MyEntry`-Steuerelement wird im .NET-Standard-Bibliotheksprojekt erstellt. Es handelt sich dabei einfach um ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement. Die Anpassung des Steuerelements erfolgt im benutzerdefinierten Renderer, sodass keine zusätzliche Implementierung im `MyEntry`-Steuerelement erforderlich ist.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen des benutzerdefinierten Steuerelements

Sie können auf das Steuerelement `MyEntry` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das Steuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das Steuerelement `MyEntry` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf das benutzerdefinierte Steuerelement zu verweisen.

Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `MyEntry` von einer C#-Seite genutzt werden kann:

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Dieser Code instanziiert ein neues [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das ein [`Label`](xref:Xamarin.Forms.Label)- und `MyEntry`-Steuerelement anzeigt, die auf der Seite vertikal und horizontal zentriert sind.

Ein benutzerdefinierter Renderer kann nun zu jedem Anwendungsprojekt hinzugefügt werden, um die Darstellung des Steuerelements auf jeder Plattform anzupassen.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `EntryRenderer`-Klasse, die das native Steuerelement rendert.
1. Überschreiben Sie die `OnElementChanged`-Methode, die die native Seite rendert, und schreiben Sie Logik, um das Steuerelement anzupassen. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern des Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Optional kann ein benutzerdefinierter Renderer in jedem Plattformprojekt bereitgestellt werden. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](entry-images/solution-structure.png "MyEntry Custom Renderer Project Responsibilities")

Das Steuerelement `MyEntry` wird von plattformspezifischen `MyEntryRenderer`-Klassen gerendert, die alle von der `EntryRenderer`-Klasse für jede Plattform abgeleitet werden. Dies führt dazu, dass jedes `MyEntry`-Steuerelement mit einer plattformspezifischen Hintergrundfarbe gerendert wird. Dies wird in folgenden Screenshots veranschaulicht:

![](entry-images/screenshots.png "MyEntry Control on each Platform")

Die `EntryRenderer`-Klasse macht die `OnElementChanged`-Methode verfügbar, die bei der Erstellung des Xamarin.Forms-Steuerelements aufgerufen wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert einen `ElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element dar, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element, an das der Renderer angefügt *ist*. In der Beispielanwendung ist die `OldElement`-Eigenschaft `null`, und die `NewElement`-Eigenschaft enthält einen Verweis auf das `MyEntry`-Steuerelement.

Das native Steuerelement sollte in der überschriebenen Version der `OnElementChanged`-Methode in der `MyEntryRenderer`-Klasse angepasst werden. Über die `Control`-Eigenschaft können Sie auf einen typisierten Verweis auf das native Steuerelement zugreifen, das auf der Plattform verwendet wird. Darüber hinaus kann über die `Element`-Eigenschaft ein Verweis auf das Xamarin.Forms-Steuerelement abgerufen werden, das gerendert wird, obwohl diese in der Beispielanwendung nicht verwendet wird.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen des zu rendernden Xamarin.Forms-Steuerelements und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird die Implementierung jeder plattformspezifischen, benutzerdefinierten `MyEntryRenderer`-Rendererklasse erläutert.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

Der Aufruf der `OnElementChanged`-Methode der Basisklasse instanziiert ein iOS-`UITextField`-Steuerelement, wobei der `Control`-Eigenschaft des Renderers ein Verweis auf das Steuerelement zugewiesen wird. Die Hintergrundfarbe wird dann mithilfe der `UIColor.FromRGB`-Methode auf „hellviolett“ festgelegt.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

Der Aufruf der `OnElementChanged`-Methode der Basisklasse instanziiert ein Android-`EditText`-Steuerelement, wobei der `Control`-Eigenschaft des Renderers ein Verweis auf das Steuerelement zugewiesen wird. Die Hintergrundfarbe wird dann mithilfe der `Control.SetBackgroundColor`-Methode auf „hellgrün“ festgelegt.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen des benutzerdefinierten Renderers auf der UWP

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die UWP veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

Der Aufruf der `OnElementChanged`-Methode der Basisklasse instanziiert ein `TextBox`-Steuerelement, wobei der `Control`-Eigenschaft des Renderers ein Verweis auf das Steuerelement zugewiesen wird. Die Hintergrundfarbe wird dann durch Erstellen einer `SolidColorBrush`-Instanz auf „zyanblau“ festgelegt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das Xamarin.Forms-Steuerelement [`Entry`](xref:Xamarin.Forms.Entry) erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können. Benutzerdefinierte Renderer sind eine praktische Methode zum Anpassen der Darstellung von Xamarin.Forms-Steuerelementen. Sie können für geringfügige Formatierungsänderungen oder für umfangreiche, plattformspezifische Anpassungen des Layouts und Verhaltens verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [CustomRendererEntry (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-entry)
