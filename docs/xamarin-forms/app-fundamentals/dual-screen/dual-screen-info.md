---
title: Funktionen für Dual-Screen-Geräte in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie mit der DualScreenInfo-Klasse in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 59575823a9ec29847a60209e846ba244e51ca0c0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138930"
---
# <a name="xamarinforms-dual-screen-device-capabilities"></a>Funktionen für Dual-Screen-Geräte in Xamarin.Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Mithilfe der `DualScreenInfo`-Klasse können Sie z. B. bestimmen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist.

## <a name="properties"></a>Eigenschaften

- `SpanningBounds`: Wenn die Ansicht zwei Bildschirme umfasst, werden damit zwei Rechtecke zurückgegeben, die die Grenzen der einzelnen sichtbaren Bereiche angeben. Wenn sich das Fenster nicht über zwei Bildschirme erstreckt, wird ein leerer Array zurückgegeben.
- `HingeBounds`: Diese Eigenschaft gibt die Position des Scharniers auf dem Bildschirm an.
- `IsLandscape`: Diese Eigenschaft gibt an, ob das Querformat auf dem Gerät verwendet wird. Dies ist hilfreich, weil native Ausrichtungs-APIs die Ausrichtung nicht korrekt melden, wenn das Anwendungsfenster mehr als einen Bildschirm einnimmt.
- `SpanMode`: Diese Eigenschaft gibt an, ob das Layout den hohen, breiten oder den Einzelseitenmodus ausweist.

Außerdem wird ein `PropertyChanged`-Ereignis ausgelöst, wenn eine Eigenschaft geändert wird.

## <a name="poll-hinge-angle-on-android"></a>Abrufen des Öffnungsgrads des Scharniers auf Android-Geräten

Die folgende Eigenschaft ist verfügbar, wenn Sie über ein Android-Plattformprojekt auf `DualScreenInfo` zugreifen:

- `GetHingeAngleAsync`: Diese Eigenschaft ruft den aktuellen Öffnungsgrad des Gerätescharniers ab. Im Simulator kann HingeAngle festgelegt werden, indem Sie den Drucksensor bearbeiten.

Diese Eigenschaft kann von einem benutzerdefinierten Android-Renderer verwendet werden:

```csharp
public class HingeAngleLabelRenderer : Xamarin.Forms.Platform.Android.FastRenderers.LabelRenderer
{
    System.Timers.Timer _hingeTimer;
    public HingeAngleLabelRenderer(Context context) : base(context)
    {
    }

    async void OnTimerElapsed(object sender, System.Timers.ElapsedEventArgs e)
    {
        if (_hingeTimer == null)
            return;

        _hingeTimer.Stop();
        var hingeAngle = await DualScreenInfo.Current.GetHingeAngleAsync();

        Device.BeginInvokeOnMainThread(() =>
        {
            if (_hingeTimer != null)
                Element.Text = hingeAngle.ToString();
        });

        if (_hingeTimer != null)
            _hingeTimer.Start();
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
    {
        base.OnElementChanged(e);

        if (_hingeTimer == null)
        {
            _hingeTimer = new System.Timers.Timer(100);
            _hingeTimer.Elapsed += OnTimerElapsed;
            _hingeTimer.Start();
        }
    }

    protected override void Dispose(bool disposing)
    {
        if (_hingeTimer != null)
        {
            _hingeTimer.Elapsed -= OnTimerElapsed;
            _hingeTimer.Stop();
            _hingeTimer = null;
        }

        base.Dispose(disposing);
    }
}
```

## <a name="access-dualscreeninfo-in-your-application-window"></a>Zugreifen auf DualScreenInfo in einem Anwendungsfenster

Im folgenden Code sehen Sie, wie Sie für Ihr Anwendungsfenster auf `DualScreenInfo` zugreifen:

```csharp
DualScreenInfo currentWindow = DualScreenInfo.Current;

// Retrieve absolute position of the hinge on the screen
var hingeBounds = currentWindow.HingeBounds;

// check if app window is spanned across two screens
if(currentWindow.SpanMode == TwoPaneViewMode.SinglePane)
{
    // window is only on one screen
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Tall)
{
    // window is spanned across two screens and oriented top-bottom
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented side-by-side
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-layouts"></a>Anwenden von DualScreenInfo auf Layouts

Die `DualScreenInfo`-Klasse hat einen Konstruktor, der ein Layout annehmen kann, und in Bezug auf die zwei Bildschirme Informationen darüber zurückgibt:

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}" />
        <ColumnDefinition Width="{Binding Column2Width}" />
        <ColumnDefinition Width="{Binding Column3Width}" />
    </Grid.ColumnDefinitions>
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="End"
           Text="I should be on the left side of the hinge" />
    <Label FontSize="Large"
           VerticalOptions="Center"
           HorizontalOptions="Start"
           Grid.Column="2"
           Text="I should be on the right side of the hinge" />
</Grid>
```

```csharp
public partial class GridUsingDualScreenInfo : ContentPage
{
    public DualScreenInfo DualScreenInfo { get; }
    public double Column1Width { get; set; }
    public double Column2Width { get; set; }
    public double Column3Width { get; set; }

    public GridUsingDualScreenInfo()
    {
        InitializeComponent();
        DualScreenInfo = new DualScreenInfo(grid);
        BindingContext = this;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        DualScreenInfo.PropertyChanged += OnInfoPropertyChanged;
        UpdateColumns();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        DualScreenInfo.PropertyChanged -= OnInfoPropertyChanged;
    }

    void UpdateColumns()
    {
        // Check if grid is on two screens
        if (DualScreenInfo.SpanningBounds.Length > 0)
        {
            // set the width of the first column to the width of the layout
            // that's on the left screen
            Column1Width = DualScreenInfo.SpanningBounds[0].Width;

            // set the middle column to the width of the hinge
            Column2Width = DualScreenInfo.HingeBounds.Width;

            // set the width of the third column to the width of the layout
            // that's on the right screen
            Column3Width = DualScreenInfo.SpanningBounds[1].Width;
        }
        else
        {
            Column1Width = 100;
            Column2Width = 0;
            Column3Width = 100;
        }

        OnPropertyChanged(nameof(Column1Width));
        OnPropertyChanged(nameof(Column2Width));
        OnPropertyChanged(nameof(Column3Width));

    }

    void OnInfoPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
    {
        UpdateColumns();
    }
}
```

Der folgende Screenshot zeigt das Layout, das sich ergibt:

![](dual-screen-info-images/grid-on-two-screens.png "Positioning Grid on Two Screens")

## <a name="related-links"></a>Verwandte Links

- [DualScreen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)
