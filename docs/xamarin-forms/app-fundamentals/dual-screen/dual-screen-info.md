---
title: 'Xamarin.Forms: DualScreenInfo'
description: In diesem Artikel wird erläutert, wie Sie mit DualScreenInfo in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: cae704b7d6300a9dc5eb456f0dec8989813c6581
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145614"
---
# <a name="xamarinforms-dualscreeninfo"></a>Xamarin.Forms: DualScreenInfo

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)

_Mit DualScreenInfo können Sie Änderungen an Layouts sowie an deren aktueller Beziehung zu den zwei Bildschirmen ermitteln und darüber informieren._

## <a name="properties"></a>Eigenschaften

- `SpanningBounds`: Wenn die Ansicht zwei Bildschirme umfasst, werden damit zwei Rechtecke zurückgegeben, die die Grenzen der einzelnen sichtbaren Bereiche angeben. Wenn sich das Fenster nicht über zwei Bildschirme erstreckt, wird ein leerer Array zurückgegeben.
- `HingeBounds`: Diese Eigenschaft gibt die Position des Scharniers auf dem Bildschirm an.
- `IsLandscape`: Diese Eigenschaft gibt an, ob das Querformat auf dem Gerät verwendet wird. Dies ist hilfreich, weil native Ausrichtungs-APIs die Ausrichtung nicht korrekt melden, wenn das Anwendungsfenster mehr als einen Bildschirm einnimmt.
- `PropertyChanged`: Diese Eigenschaft wird ausgelöst, wenn eine Eigenschaft geändert wird.
- `SpanMode`: Diese Eigenschaft gibt an, ob das Layout den hohen, breiten oder den Einzelseitenmodus ausweist.

## <a name="android-only-property"></a>Android-spezifische Eigenschaften

Diese Eigenschaft ist nur verfügbar, wenn Sie über ein Android-Plattformprojekt auf DualScreenInfo zugreifen.
Sie können diese Eigenschaft aus einem benutzerdefinierten Renderer verwenden.

- `GetHingeAngleAsync`: Diese Eigenschaft ruft den aktuellen Öffnungsgrad des Gerätescharniers ab. Im Simulator kann HingeAngle festgelegt werden, indem Sie den Drucksensor bearbeiten.

## <a name="android-custom-renderer-for-polling-hinge-angle"></a>Benutzerdefinierter Android-Renderer zum Abfragen des Scharniergrads

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

## <a name="access-dualscreeninfo-for-application-window"></a>Zugreifen auf DualScreenInfo für Anwendungsfenster

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
    // window is spanned across two screens and oriented as landscape
}
else if(currentWindow.SpanMode == TwoPaneViewMode.Wide)
{
    // window is spanned across two screens and oriented as portrait
}

// Detect if any of the properties on DualScreenInfo change.
// This is useful to detect if the app window gets spanned
// across two screens or put on only one  
currentWindow.PropertyChanged += OnDualScreenInfoChanged;
```

## <a name="apply-dualscreeninfo-to-your-own-layouts"></a>Anwenden von DualScreenInfo auf eigene Layouts

DualScreenInfo hat einen Konstruktor, der ein Layout nehmen kann, und in Bezug auf die zwei Bildschirme Informationen darüber zurückgibt.

```xaml
<Grid x:Name="grid" ColumnSpacing="0">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="{Binding Column1Width}"></ColumnDefinition>
        <ColumnDefinition Width="{Binding Column2Width}"></ColumnDefinition>
        <ColumnDefinition Width="{Binding Column3Width}"></ColumnDefinition>
    </Grid.ColumnDefinitions>

    <Label FontSize="Large" VerticalOptions="Center" HorizontalOptions="End" Text="I should be on the left side of the hinge"></Label>
    <Label FontSize="Large" VerticalOptions="Center" HorizontalOptions="Start" Grid.Column="2" Text="I should be on the right side of the hinge"></Label>
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

![](dual-screen-info-images/grid-on-two-screens.png "Positioning Grid on Two Screens")

## <a name="related-links"></a>Verwandte Links

- [DualScreen (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)
