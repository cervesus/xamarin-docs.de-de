---
title: Xamarin.FormsImage Button
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
ms.openlocfilehash: 7d81c0ce4dc2a46a840a34cc9084c8f2388a0169
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137643"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.FormsImage Button

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Die ImageButton-Taste zeigt ein Bild an und antwortet auf eine Tap-oder Klick-Taste, die eine Anwendung anweist, eine bestimmte Aufgabe auszuführen._

`ImageButton`In der Ansicht werden die [`Button`](xref:Xamarin.Forms.Button) Ansicht und [`Image`](xref:Xamarin.Forms.Image) Ansicht kombiniert, um eine Schaltfläche zu erstellen, deren Inhalt ein Bild ist. Der Benutzer drückt den `ImageButton` mit einem Finger oder klickt mit der Maus darauf, um die Anwendung anzuweisen, eine bestimmte Aufgabe auszuführen. Anders als bei der `Button` Ansicht hat die `ImageButton` Ansicht jedoch kein Konzept für Text und Textdarstellung.

> [!NOTE]
> Obwohl die [`Button`](xref:Xamarin.Forms.Button) Sicht eine [`Image`](xref:Xamarin.Forms.Button.Image) Eigenschaft definiert, die es Ihnen ermöglicht, ein Bild auf dem anzuzeigen `Button` , sollte diese Eigenschaft verwendet werden, wenn ein kleines Symbol neben dem Text angezeigt wird `Button` .

Die Codebeispiele in dieser Anleitung stammen aus dem [formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery).

## <a name="setting-the-image-source"></a>Festlegen der Bildquelle

`ImageButton`definiert eine `Source` Eigenschaft, die auf das Bild festgelegt werden soll, das in der Schaltfläche angezeigt werden soll, wobei die Bildquelle entweder eine Datei, ein URI, eine Ressource oder ein Stream ist. Weitere Informationen zum Laden von Bildern aus unterschiedlichen Quellen finden Sie unter [Images in Xamarin.Forms ](images.md).

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

Die- `Source` Eigenschaft gibt das Bild an, das in der angezeigt wird `ImageButton` . In diesem Beispiel ist es auf eine lokale Datei festgelegt, die aus jedem Platt Form Projekt geladen wird, was zu den folgenden Screenshots führt:

[![Einfaches ImageButton](imagebutton-images/BasicImageButton.png "Einfaches ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "Einfaches ImageButton")

Standardmäßig ist der `ImageButton` rechteckig, aber Sie können ihm mithilfe der-Eigenschaft abgerundete Ecken übergeben `CornerRadius` . Weitere Informationen zur Darstellung `ImageButton` finden Sie unter [ImageButton](#imagebutton-appearance)-Darstellung.

> [!NOTE]
> Während ein `ImageButton` animiertes GIF laden kann, wird nur der erste Frame der gif angezeigt.

Im folgenden Beispiel wird gezeigt, wie eine Seite erstellt wird, die dem vorherigen XAML-Beispiel funktionell entspricht, jedoch vollständig in c#:

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

## <a name="handling-imagebutton-clicks"></a>Behandeln von ImageButton-Klicks

`ImageButton`definiert ein- `Clicked` Ereignis, das ausgelöst wird, wenn der Benutzer `ImageButton` mit einem Finger oder Mauszeiger auf das tippt. Das-Ereignis wird ausgelöst, wenn der Finger oder die Maustaste von der-Oberfläche der losgelassen wird `ImageButton` . Die-Eigenschaft muss für die auf `ImageButton` `IsEnabled` tippen festgelegt sein `true` .

Im folgenden Beispiel wird gezeigt, wie ein `ImageButton` in XAML instanziiert und das zugehörige-Ereignis behandelt wird `Clicked` :

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

Das- `Clicked` Ereignis wird auf einen Ereignishandler mit dem Namen festgelegt `OnImageButtonClicked` , der sich in der Code Behind-Datei befindet:

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

Wenn die `ImageButton` angetippt wird, wird die `OnImageButtonClicked`-Methode ausgeführt. Das- `sender` Argument ist der `ImageButton` Verantwortliche für dieses Ereignis. Sie können dies verwenden, um auf das `ImageButton` Objekt zuzugreifen, oder um zwischen mehreren Objekten zu unterscheiden, `ImageButton` die dasselbe `Clicked` Ereignis verwenden.

Dieser bestimmte `Clicked` Handler erhöht einen Leistungs Bewert und zeigt den Wert des Zählers in einem an [`Label`](xref:Xamarin.Forms.Label) :

[![Einfaches ImageButton-Klick](imagebutton-images/ImageButton.png "Einfaches ImageButton-Klick")](imagebutton-images/ImageButton-Large.png#lightbox "Einfaches ImageButton-Klick")

Im folgenden Beispiel wird gezeigt, wie eine Seite erstellt wird, die dem vorherigen XAML-Beispiel funktionell entspricht, jedoch vollständig in c#:

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

## <a name="disabling-the-imagebutton"></a>Deaktivieren von ImageButton

Manchmal befindet sich eine Anwendung in einem bestimmten Zustand, in dem ein bestimmter `ImageButton` Klick kein gültiger Vorgang ist. In diesen Fällen sollte der deaktiviert werden, indem die zugehörige- `ImageButton` Eigenschaft auf festgelegt wird `IsEnabled` `false` .

## <a name="using-the-command-interface"></a>Verwenden der Befehlsschnittstelle

Es ist möglich, dass eine Anwendung auf TAPS reagiert, `ImageButton` ohne das `Clicked` Ereignis zu behandeln. `ImageButton`Implementiert einen alternativen Benachrichtigungs Mechanismus, der als _Befehl_ oder Befehlsschnittstelle bezeichnet wird. _commanding_ Dies umfasst zwei Eigenschaften:

- `Command`vom Typ [`ICommand`](xref:System.Windows.Input.ICommand) , eine im-Namespace definierte-Schnittstelle [`System.Windows.Input`](xref:System.Windows.Input) .
- `CommandParameter`-Eigenschaft vom Typ [`Object`](xref:System.Object) .

Diese Vorgehensweise eignet sich für die Verbindung mit der Datenbindung und insbesondere bei der Implementierung der Model-View-ViewModel (MVVM)-Architektur.

Weitere Informationen zum Verwenden der Befehlsschnittstelle finden Sie unter [Verwenden der Befehlsschnittstelle](button.md#using-the-command-interface) im [Schalt](button.md) Flächen Handbuch.

## <a name="pressing-and-releasing-the-imagebutton"></a>Drücken und Freigeben von ImageButton

Neben dem `Clicked`-Ereignis definiert das `ImageButton`-Element auch die Ereignisse `Pressed` und `Released`. Das- `Pressed` Ereignis tritt auf, wenn ein Finger auf ein drückt `ImageButton` , oder wenn eine Maustaste gedrückt wird, während der Zeiger auf dem positioniert ist `ImageButton` . Das `Released` Ereignis tritt auf, wenn der Finger oder die Maustaste losgelassen wird. Im Allgemeinen wird das- `Clicked` Ereignis auch zur gleichen Zeit wie das- `Released` Ereignis ausgelöst, aber wenn sich der Finger oder der Mauszeiger vor der Freigabe von der-Oberfläche von entfernt `ImageButton` , wird das `Clicked` Ereignis möglicherweise nicht angezeigt.

Weitere Informationen zu diesen Ereignissen finden Sie im [Schalt](button.md) Flächen Handbuch unter [drücken und Freigeben der Schaltfläche](button.md#pressing-and-releasing-the-button) .

## <a name="imagebutton-appearance"></a>Darstellung von ImageButton

Zusätzlich zu den Eigenschaften, die `ImageButton` von der- [`View`](xref:Xamarin.Forms.View) Klasse erben, werden von `ImageButton` auch mehrere Eigenschaften definiert, die sich auf ihre Darstellung auswirken:

- `Aspect`Gibt an, wie das Bild so skaliert wird, dass es dem Anzeigebereich entspricht.
- `BorderColor`die Farbe eines Bereichs, der den umgibt `ImageButton` .
- `BorderWidth`die Breite des Rahmens.
- `CornerRadius`ist der Eckradius von `ImageButton` .

Die- `Aspect` Eigenschaft kann auf einen der Member der-Enumeration festgelegt werden [`Aspect`](xref:Xamarin.Forms.Aspect) :

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill)-streckt das Bild auf vollständig und gefüllt das `ImageButton` . Dies kann dazu führen, dass das Bild verzerrt wird.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)-schneidet das Bild ab, sodass es den füllt, `ImageButton` während das Seitenverhältnis beibehalten wird.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)-Letterbox das Bild (falls erforderlich), sodass das gesamte Bild in das-Feld passt `ImageButton` , wobei der obere bzw. untere Rand des Bilds in Abhängigkeit davon, ob das Bild breit oder hoch ist, Leerzeichen hinzugefügt werden. Dies ist der Standardwert der- [`Aspect`](xref:Xamarin.Forms.Aspect) Enumeration.

> [!NOTE]
> Die `ImageButton` -Klasse verfügt auch über [`Margin`](xref:Xamarin.Forms.View.Margin) -und- `Padding` Eigenschaften, die das Layoutverhalten von Steuern `ImageButton` . Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="imagebutton-visual-states"></a>Visuelle Status von ImageButton

`ImageButton`verfügt über ein `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) -Element, das zum Initiieren einer visuellen Änderung am verwendet werden kann `ImageButton` , wenn es vom Benutzer gedrückt wird, vorausgesetzt, dass es aktiviert ist.

Das folgende XAML-Beispiel zeigt, wie Sie einen visuellen Zustand für den `Pressed` Status definieren:

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

Der `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass die- `ImageButton` Eigenschaft beim Drücken [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) von von ihrem Standardwert von 1 in 0,8 geändert wird. Der `Normal` `VisualState` gibt an, dass die- `ImageButton` `Scale` Eigenschaft auf 1 festgelegt wird, wenn sich der in einem normalen Zustand befindet. Der Gesamteffekt besteht daher darin, dass beim `ImageButton` drücken von eine etwas geringere Größe erreicht wird, und wenn der freigegeben wird, wird `ImageButton` er auf seine Standardgröße umgestellt.

Weitere Informationen zu visuellen Zuständen finden Sie [unter Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [Formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
