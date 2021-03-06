---
title: Funktionen für Dual-Screen-Geräte in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie mit der DualScreenInfo-Klasse in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: dd5eb074-f4cb-4ab4-b47d-76f862ac7cfa
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 760d03cb5667853ab7eea021e281ebd863dd8dc4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935225"
---
# <a name="xamarinforms-dualscreeninfo-helper-class"></a>Xamarin.Forms-Hilfsklasse „DualScreenInfo“

![Vorabrelease der API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Mithilfe der `DualScreenInfo`-Klasse können Sie z. B. bestimmen, in welchem Bereich Ihre Ansicht angezeigt wird, wie groß sie ist, welche Ausrichtung das Gerät hat und welchen Öffnungsgrad das Scharnier aufweist.

## <a name="configure-dualscreeninfo"></a>Konfigurieren von DualScreenInfo

Führen Sie die folgenden Anweisungen aus, um ein Dual-Screen-Layout in Ihrer App zu erstellen:

1. Befolgen Sie die Anweisungen zu den [ersten Schritten](index.md), um NuGet hinzuzufügen und die Android-Klasse `MainActivity` zu konfigurieren.
1. Fügen Sie `using Xamarin.Forms.DualScreen;` zu Ihrer Klassendatei hinzu.
1. Verwenden Sie die Klasse `DualScreenInfo.Current` in Ihrer App.

## <a name="properties"></a>Eigenschaften

- `SpanningBounds`: Wenn die Ansicht zwei Bildschirme umfasst, werden damit zwei Rechtecke zurückgegeben, die die Grenzen der einzelnen sichtbaren Bereiche angeben. Wenn sich das Fenster nicht über zwei Bildschirme erstreckt, wird ein leerer Array zurückgegeben.
- `HingeBounds`: Diese Eigenschaft gibt die Position des Scharniers auf dem Bildschirm an.
- `IsLandscape`: Diese Eigenschaft gibt an, ob das Querformat auf dem Gerät verwendet wird. Dies ist hilfreich, weil native Ausrichtungs-APIs die Ausrichtung nicht korrekt melden, wenn das Anwendungsfenster mehr als einen Bildschirm einnimmt.
- `SpanMode`: Diese Eigenschaft gibt an, ob das Layout den hohen, breiten oder den Einzelseitenmodus ausweist.

Darüber hinaus wird das Ereignis `PropertyChanged` ausgelöst, wenn sich beliebige Eigenschaften ändern. Das Ereignis `HingeAngleChanged` wird ausgelöst, wenn sich der Öffnungsgrads des Scharniers ändert.

## <a name="poll-hinge-angle-on-android-and-uwp"></a>Abrufen des Öffnungsgrads des Scharniers unter Android und UWP

Die folgende Methode steht zur Verfügung, wenn aus Projekten auf der Android- und UWP-Plattform auf `DualScreenInfo` zugegriffen wird:

- `GetHingeAngleAsync`: Diese Eigenschaft ruft den aktuellen Öffnungsgrad des Gerätescharniers ab. Im Simulator kann HingeAngle festgelegt werden, indem Sie den Drucksensor bearbeiten.

Diese Methode kann von benutzerdefinierten Renderern unter Android und UWP aufgerufen werden. Der folgende Code zeigt ein Beispiel eines benutzerdefinierten Android-Renderers:

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

![Positionieren eines Rasters auf zwei Bildschirmen](dual-screen-info-images/grid-on-two-screens.png)

## <a name="related-links"></a>Verwandte Links

- [DualScreen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)
