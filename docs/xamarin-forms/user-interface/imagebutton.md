---
title: Xamarin.Forms ImageButton
description: ImageButton ein Bild anzeigt und reagiert auf ein tippen oder klicken Sie auf, die eine Anwendung, um eine bestimmte Aufgabe durchzuführen weiterleitet.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: f97cd3030b865b53b82845ff8941e3f0a10f0320
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169828"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)

_ImageButton ein Bild anzeigt und reagiert auf ein tippen oder klicken Sie auf, die eine Anwendung, um eine bestimmte Aufgabe durchzuführen weiterleitet._

Die `ImageButton` anzeigen kombiniert die [ `Button` ](xref:Xamarin.Forms.Button) anzeigen und [ `Image` ](xref:Xamarin.Forms.Image) Ansicht zum Erstellen einer Schaltfläche, deren Inhalt ist ein Image. Der Benutzer drückt die `ImageButton` mit einem Finger oder darauf klickt, mit der Maus auf das leiten von der Anwendung um eine bestimmte Aufgabe durchzuführen. Anders als bei der `Button` anzeigen, die `ImageButton` Ansicht hat kein Konzept für Text und Textdarstellung.

> [!NOTE]
> Während der [ `Button` ](xref:Xamarin.Forms.Button) Sicht definiert eine [ `Image` ](xref:Xamarin.Forms.Button.Image) Eigenschaft, die Ihnen ermöglicht, ein Bild angezeigt, auf die `Button`, diese Eigenschaft sollte verwendet werden, wenn ein kleines Symbol angezeigt neben der `Button` Text.

Die Codebeispiele in diesem Handbuch stammen aus der [FormsGallery Beispiel](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/).

## <a name="setting-the-image-source"></a>Die Bildquelle festlegen

`ImageButton` definiert eine `Source` -Eigenschaft, die auf das Bild festgelegt werden soll, um mit der Bildquelle wird entweder eine Datei, einem URI, eine Ressource oder einen Datenstrom in der Schaltfläche angezeigt werden soll. Weitere Informationen zum Laden von Bildern aus verschiedenen Quellen finden Sie unter [Bilder in Xamarin.Forms](images.md).

Das folgende Beispiel zeigt, wie Sie instanziieren ein `ImageButton` in XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die `Source` -Eigenschaft gibt das Bild, das in wird die `ImageButton`. In diesem Beispiel wird es in einer lokalen Datei festgelegt, die von jeder plattformprojekt, was in den folgenden Screenshots geladen werden:

[![Grundlegende ImageButton](imagebutton-images/BasicImageButton.png "grundlegende ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "grundlegende ImageButton")

In der Standardeinstellung die `ImageButton` rechteckig, aber Sie können die It abgerundete Ecken erteilen, mit der `CornerRadius` Eigenschaft. Weitere Informationen zu `ImageButton` Darstellung finden Sie unter [ImageButton Darstellung](#imagebutton-appearance).

Das folgende Beispiel zeigt, wie Sie eine Seite erstellen, der funktional dem im vorherigen XAML-Beispiel, jedoch vollständig in C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## <a name="handling-imagebutton-clicks"></a>Behandeln von ImageButton klickt

`ImageButton` definiert eine `Clicked` Ereignis, das ausgelöst wird, wenn der Benutzer tippt der `ImageButton` mit einem Finger oder der Maus-Zeiger. Das Ereignis wird ausgelöst, wenn der Finger oder der Maus über die Oberfläche des losgelassen wird, die `ImageButton`. Die `ImageButton` müssen die `IsEnabled` -Eigenschaftensatz auf `true` auf tippen zu reagieren.

Das folgende Beispiel zeigt, wie Sie instanziieren ein `ImageButton` in XAML und behandeln die `Clicked` Ereignis:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die `Clicked` Ereignis festgelegt ist, um einen Ereignishandler namens `OnImageButtonClicked` , befindet sich im Code-Behind-Datei:

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

Wenn die `ImageButton` getippt wird, die `OnImageButtonClicked` Methode ausgeführt wird. Die `sender` Argument ist der `ImageButton` für dieses Ereignis verantwortlich. Hiermit können Sie den Zugriff auf die `ImageButton` Objekt oder zur Unterscheidung zwischen mehreren `ImageButton` Objekte, die gemeinsame Verwendung desselben `Clicked` Ereignis.

Dieser spezielle `Clicked` Handler erhöht einen Zähler und zeigt den Wert dieses Indikators in einem [ `Label` ](xref:Xamarin.Forms.Label):

[![Grundlegende ImageButton auf](imagebutton-images/ImageButton.png "grundlegende ImageButton auf")](imagebutton-images/ImageButton-Large.png#lightbox "grundlegende ImageButton auf")

Das folgende Beispiel zeigt, wie Sie eine Seite erstellen, der funktional dem im vorherigen XAML-Beispiel, jedoch vollständig in C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## <a name="disabling-the-imagebutton"></a>Deaktivieren die ImageButton

Manchmal ist eine Anwendung einen bestimmten Zustand, bei einer bestimmten `ImageButton` auf ist kein gültiger Vorgang. In diesen Fällen die `ImageButton` sollte deaktiviert werden, indem die `IsEnabled` Eigenschaft `false`.

## <a name="using-the-command-interface"></a>Verwenden die Befehlsschnittstelle

Es ist möglich, dass eine Anwendung auf reagieren `ImageButton` Taps ohne Behandlung der `Clicked` Ereignis. Die `ImageButton` implementiert eine alternative Benachrichtigungsmechanismus, der Namen der _Befehl_ oder _Befehle_ Schnittstelle. Dies besteht aus zwei Eigenschaften:

- `Command` Der Typ [ `ICommand` ](xref:System.Windows.Input.ICommand), eine Schnittstelle definiert, der [ `System.Windows.Input` ](xref:System.Windows.Input) Namespace.
- `CommandParameter` Eigenschaft des Typs [ `Object` ](xref:System.Object).

Dieser Ansatz eignet sich im Zusammenhang mit Datenbindung und insbesondere dann, wenn die Model-View-ViewModel (MVVM)-Architektur zu implementieren.

Weitere Informationen zur Verwendung der Befehlsschnittstelle finden Sie unter [über die Befehlsschnittstelle](button.md#using-the-command-interface) in die [Schaltfläche](button.md) Guide.

## <a name="pressing-and-releasing-the-imagebutton"></a>Drücken und Loslassen der ImageButton

Neben der `Clicked` Ereignis `ImageButton` definiert auch `Pressed` und `Released` Ereignisse. Die `Pressed` Ereignis tritt auf, wenn ein Finger auf drückt eine `ImageButton`, oder eine Maustaste gedrückt wird mit dem Mauszeiger auf positioniert die `ImageButton`. Die `Released` Ereignis tritt auf, wenn die Finger oder der Maustaste losgelassen wird. Im Allgemeinen die `Clicked` Ereignis wird auch ausgelöst, zur gleichen Zeit wie die `Released` -Ereignis, aber die Folien Wenn des Finger oder der Maus Zeigers von der Oberfläche der `ImageButton` vor freigegeben wird, die `Clicked` Ereignis erfolgen möglicherweise keine.

Weitere Informationen zu diesen Ereignissen finden Sie unter [drücken und Loslassen der Taste](button.md#pressing-and-releasing-the-button) in die [Schaltfläche](button.md) Guide.

## <a name="imagebutton-appearance"></a>ImageButton Darstellung

Zusätzlich zu den Eigenschaften, die `ImageButton` erbt von der [ `View` ](xref:Xamarin.Forms.View) -Klasse, `ImageButton` definiert außerdem mehrere Eigenschaften, die die Darstellung auswirken:

- `Aspect` ist wie das Bild skaliert wird, um den Anzeigebereich anpassen.
- `BorderColor` entspricht der Farbe ein Bereich, umgibt die `ImageButton`.
- `BorderWidth` ist die Breite des Rahmens.
- `CornerRadius` ist der Eckradius der `ImageButton`.

Die `Aspect` Eigenschaft kann festgelegt werden, zu einem Mitglied der [ `Aspect` ](xref:Xamarin.Forms.Aspect) Enumeration:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -Geben Sie vollständig und genau das Bild gestreckt wird, die `ImageButton`. Dies kann in das Bild verzerrt wird führen.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -Schneidet das Bild ab, so dass sie voll ist die `ImageButton` Beibehaltung des Seitenverhältnisses.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -Letterbox das Image (falls erforderlich), damit das gesamte Bild in passt die `ImageButton`, mit Leerzeichen zu dem oben/unten oder Seiten abhängig davon, ob das Bild Breite oder Höhe hinzugefügt. Dies ist der Standardwert der [ `Aspect` ](xref:Xamarin.Forms.Aspect) Enumeration.

> [!NOTE]
> Die `ImageButton` -Klasse verfügt auch über [ `Margin` ](xref:Xamarin.Forms.View.Margin) und `Padding` Eigenschaften zum Steuern der Layoutverhalten der `ImageButton`. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="imagebutton-visual-states"></a>Visuelle Zustände ImageButton

`ImageButton` verfügt über eine `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, initiieren Sie eine visuelle Änderung an der `ImageButton` beim Klicken auf vom Benutzer aufgerufen werden, vorausgesetzt, dass es aktiviert ist.

Im folgende XAML-Beispiel zeigt, wie definieren Sie einen visuellen Zustand für die `Pressed` Zustand:

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

Die `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) gibt an, dass die `ImageButton` gedrückt wird, dessen [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft wird von seinem Standardwert von 1 auf 0,8 geändert werden. Die `Normal` `VisualState` gibt an, dass die `ImageButton` befindet sich in einem normalen Status, seine `Scale` Eigenschaft wird auf 1 festgelegt werden. Aus diesem Grund der Gesamteffekt besteht, die bei der `ImageButton` ist gedrückt, sie entsprechend neu skaliert etwas kleiner, und wenn die `ImageButton` wird veröffentlicht, wird es auf der Standardgröße angepasst.

Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [FormsGallery-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
