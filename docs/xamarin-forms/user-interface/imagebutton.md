---
title: Xamarin.Forms ImageButton
description: ImageButton ein Bild anzeigt und reagiert auf ein tippen oder klicken Sie auf, die eine Anwendung, um eine bestimmte Aufgabe durchzuführen weiterleitet.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 7c6647a0299b5ece3caaaa1d322ec1a0efac3557
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305714"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Die ImageButton-Taste zeigt ein Bild an und antwortet auf eine Tap-oder Klick-Taste, die eine Anwendung anweist, eine bestimmte Aufgabe auszuführen._

In der `ImageButton` Ansicht werden die [`Button`](xref:Xamarin.Forms.Button) Ansicht und [`Image`](xref:Xamarin.Forms.Image) Ansicht kombiniert, um eine Schaltfläche zu erstellen, deren Inhalt ein Bild ist. Der Benutzer drückt den `ImageButton` mit einem Finger oder klickt mit der Maus darauf, um die Anwendung anzuweisen, eine bestimmte Aufgabe auszuführen. Anders als bei der `Button` Ansicht hat die `ImageButton` Ansicht jedoch kein Konzept für Text und Textdarstellung.

> [!NOTE]
> Während die [`Button`](xref:Xamarin.Forms.Button) Sicht eine [`Image`](xref:Xamarin.Forms.Button.Image) Eigenschaft definiert, die es Ihnen ermöglicht, ein Bild auf dem `Button`anzuzeigen, sollte diese Eigenschaft verwendet werden, wenn ein kleines Symbol neben dem `Button` Text angezeigt wird.

Die Codebeispiele in dieser Anleitung stammen aus dem [formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery).

## <a name="setting-the-image-source"></a>Die Bildquelle festlegen

`ImageButton` definiert eine `Source` Eigenschaft, die auf das Bild festgelegt werden soll, das in der Schaltfläche angezeigt werden soll, wobei die Bildquelle entweder eine Datei, ein URI, eine Ressource oder ein Stream ist. Weitere Informationen zum Laden von Bildern aus unterschiedlichen Quellen finden Sie unter [Bilder in xamarin. Forms](images.md).

Im folgenden Beispiel wird gezeigt, wie ein `ImageButton` in XAML instanziiert wird:

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

Die `Source`-Eigenschaft gibt das Bild an, das in der `ImageButton`angezeigt wird. In diesem Beispiel wird es in einer lokalen Datei festgelegt, die von jeder plattformprojekt, was in den folgenden Screenshots geladen werden:

[![Einfaches ImageButton](imagebutton-images/BasicImageButton.png "Einfaches ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "Einfaches ImageButton")

Standardmäßig ist die `ImageButton` rechteckig, aber Sie können mit der `CornerRadius`-Eigenschaft abgerundete Ecken vergeben. Weitere Informationen zur `ImageButton` Darstellung finden Sie unter [ImageButton](#imagebutton-appearance)-Darstellung.

> [!NOTE]
> Während ein `ImageButton` ein animiertes GIF laden kann, wird nur der erste Frame der gif angezeigt.

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

`ImageButton` definiert ein `Clicked` Ereignis, das ausgelöst wird, wenn der Benutzer mit einem Finger oder Mauszeiger auf die `ImageButton` tippt. Das Ereignis wird ausgelöst, wenn der Finger oder die Maustaste von der Oberfläche des `ImageButton`losgelassen wird. Für den `ImageButton` muss seine `IsEnabled`-Eigenschaft auf `true` festgelegt sein, um auf Abzweigungen zu reagieren.

Im folgenden Beispiel wird gezeigt, wie ein `ImageButton` in XAML instanziiert und das `Clicked` Ereignis behandelt wird:

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

Das `Clicked` Ereignis ist auf einen Ereignishandler mit dem Namen `OnImageButtonClicked` festgelegt, der sich in der Code Behind-Datei befindet:

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

Wenn die `ImageButton` getippt wird, wird die `OnImageButtonClicked`-Methode ausgeführt. Das `sender`-Argument ist der `ImageButton`, der für dieses Ereignis verantwortlich ist. Sie können dies verwenden, um auf das `ImageButton` Objekt zuzugreifen oder um zwischen mehreren `ImageButton` Objekten zu unterscheiden, die dasselbe `Clicked` Ereignis verwenden.

Dieser bestimmte `Clicked` Handler erhöht einen Leistungs Bewert und zeigt den Wert des Zählers in einem [`Label`](xref:Xamarin.Forms.Label)an:

[![Einfaches ImageButton-Klick](imagebutton-images/ImageButton.png "Einfaches ImageButton-Klick")](imagebutton-images/ImageButton-Large.png#lightbox "Einfaches ImageButton-Klick")

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

Manchmal befindet sich eine Anwendung in einem bestimmten Zustand, in dem ein bestimmter `ImageButton` Click kein gültiger Vorgang ist. In diesen Fällen sollte die `ImageButton` durch Festlegen der Eigenschaft `IsEnabled` auf `false`deaktiviert werden.

## <a name="using-the-command-interface"></a>Verwenden die Befehlsschnittstelle

Es ist möglich, dass eine Anwendung auf `ImageButton` TAPS antwortet, ohne das `Clicked`-Ereignis zu behandeln. Der `ImageButton` implementiert einen alternativen Benachrichtigungs Mechanismus, der als _Befehl_ _oder Befehls_ Schnittstelle bezeichnet wird. Dies besteht aus zwei Eigenschaften:

- `Command` vom Typ [`ICommand`](xref:System.Windows.Input.ICommand), eine Schnittstelle, die im [`System.Windows.Input`](xref:System.Windows.Input) Namespace definiert ist.
- `CommandParameter` Eigenschaft vom Typ [`Object`](xref:System.Object).

Dieser Ansatz eignet sich im Zusammenhang mit Datenbindung und insbesondere dann, wenn die Model-View-ViewModel (MVVM)-Architektur zu implementieren.

Weitere Informationen zum Verwenden der Befehlsschnittstelle finden Sie unter [Verwenden der Befehlsschnittstelle](button.md#using-the-command-interface) im [Schalt](button.md) Flächen Handbuch.

## <a name="pressing-and-releasing-the-imagebutton"></a>Drücken und Loslassen der ImageButton

Neben dem `Clicked`-Ereignis definiert `ImageButton` auch `Pressed`-und `Released`-Ereignisse. Das `Pressed` Ereignis tritt auf, wenn ein Finger auf einen `ImageButton`drückt, oder wenn eine Maustaste gedrückt wird, während der Zeiger auf die `ImageButton`positioniert ist. Das `Released` Ereignis tritt auf, wenn der Finger oder die Maustaste losgelassen wird. Im Allgemeinen wird das `Clicked` Ereignis auch zur gleichen Zeit ausgelöst wie das Ereignis `Released`, aber wenn der Finger oder Mauszeiger vor der Freigabe von der Oberfläche des `ImageButton` bewegt wird, tritt das `Clicked` Ereignis möglicherweise nicht auf.

Weitere Informationen zu diesen Ereignissen finden Sie im [Schalt](button.md) Flächen Handbuch unter [drücken und Freigeben der Schaltfläche](button.md#pressing-and-releasing-the-button) .

## <a name="imagebutton-appearance"></a>ImageButton Darstellung

Zusätzlich zu den Eigenschaften, die `ImageButton` von der [`View`](xref:Xamarin.Forms.View) -Klasse erbt, definiert `ImageButton` auch mehrere Eigenschaften, die sich auf ihre Darstellung auswirken:

- `Aspect` gibt an, wie das Bild so skaliert wird, dass es in den Anzeigebereich passt.
- `BorderColor` ist die Farbe eines Bereichs, der die `ImageButton`umgibt.
- `BorderWidth` ist die Breite des Rahmens.
- `CornerRadius` ist der Eckradius des `ImageButton`.

Die `Aspect`-Eigenschaft kann auf einen der Member der [`Aspect`](xref:Xamarin.Forms.Aspect) -Enumeration festgelegt werden:

- das Bild wird [`Fill`](xref:Xamarin.Forms.Aspect.Fill) gestreckt und die `ImageButton`vollständig aufgefüllt. Dies kann in das Bild verzerrt wird führen.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) schneidet das Bild ab, sodass es die `ImageButton` füllt, während das Seitenverhältnis beibehalten wird.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -Letterbox das Bild (falls erforderlich), damit das gesamte Bild in den `ImageButton`passt, wobei der obere bzw. untere Rand des Bilds in Abhängigkeit davon, ob das Bild breit oder hoch ist, Leerzeichen hinzugefügt werden. Dies ist der Standardwert der [`Aspect`](xref:Xamarin.Forms.Aspect) -Enumeration.

> [!NOTE]
> Die `ImageButton`-Klasse verfügt auch über [`Margin`](xref:Xamarin.Forms.View.Margin) -und `Padding`-Eigenschaften, die das Layoutverhalten der `ImageButton`steuern. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="imagebutton-visual-states"></a>Visuelle Zustände ImageButton

`ImageButton` verfügt über eine `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) , die zum Initiieren einer visuellen Änderung am `ImageButton` verwendet werden kann, wenn der Benutzer darauf klickt, sofern diese aktiviert ist.

Das folgende XAML-Beispiel zeigt, wie ein visueller Zustand für den `Pressed` Status definiert wird:

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

Die `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass die [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft beim Drücken der `ImageButton` von ihrem Standardwert von 1 in 0,8 geändert wird. Die `Normal` `VisualState` gibt an, dass die `Scale` Eigenschaft auf 1 festgelegt wird, wenn sich die `ImageButton` in einem normalen Zustand befindet. Der Gesamteffekt ist, dass die `ImageButton` gedrückt wird, dass Sie etwas kleiner ist, und wenn die `ImageButton` freigegeben wird, wird Sie auf Ihre Standardgröße umgestellt.

Weitere Informationen zu visuellen Zuständen finden Sie [unter xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [Formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
