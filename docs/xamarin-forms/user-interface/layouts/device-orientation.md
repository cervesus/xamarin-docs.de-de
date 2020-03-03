---
title: Geräteausrichtung
description: In diesem Artikel wird erläutert, wie Sie Layout Xamarin.Forms-Anwendungen, die im Hoch-und Querformat hervorragend aussehen.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: f7526b6cecebadd30e95718b7e537026a6557adf
ms.sourcegitcommit: f43d5ecafd19cbc5cce39201916a83927a34617a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78214993"
---
# <a name="device-orientation"></a>Geräteausrichtung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

Es ist wichtig zu berücksichtigen, wie Ihre Anwendung verwendet wird und wie Querformat integriert werden kann, um die benutzerfreundlichkeit zu verbessern. Einzelne Layouts können entworfen werden, um mehrere Ausrichtungen zu berücksichtigen und am besten verwenden Sie den verfügbaren Speicherplatz. Auf der Anwendungsebene kann Drehung deaktiviert oder aktiviert werden.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Steuern der Ausrichtung

Wenn Sie Xamarin.Forms verwenden, werden das unterstützte Verfahren zum Steuern des Geräts ab, die Einstellungen für jedes einzelne Projekt verwenden.

### <a name="ios"></a>iOS

Unter IOS wird die Geräte Ausrichtung für Anwendungen konfiguriert, die die **Info. plist** -Datei verwenden. Diese Datei enthält Einstellungen für die Ausrichtung für iPhone und iPod sowie Einstellungen für iPad, wenn die app als Ziel enthält. Im folgenden finden Anweisungen zu Ihrer IDE. Verwenden Sie die IDE-Optionen am oberen Rand dieses Dokuments, welche Anweisungen auswählen, finden Sie unter möchten:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Öffnen Sie in Visual Studio das IOS-Projekt, und öffnen Sie " **Info. plist**". Die Datei wird in einem Bereich Konfiguration angezeigt, beginnend mit der iPhone-Bereitstellungsinformationen-Registerkarte geöffnet:

![iPhone-Bereitstellungsinformationen in Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Um die iPad-Ausrichtung zu konfigurieren, wählen Sie die Registerkarte **iPad-Bereitstellungs Informationen** oben links im Bereich aus, und wählen Sie dann eine der verfügbaren Ausrichtungen aus

![Unterstützte Geräteausrichtungen in Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Öffnen Sie in Visual Studio für Mac das IOS-Projekt, und öffnen Sie die Datei **Info. plist**. Auf der Registerkarte **Anwendung** stehen Abschnitte zum Festlegen der Ausrichtung zur Verfügung:

![iPhone-Bereitstellungsinformationen in Visual Studio für Mac](device-orientation-images/orientation-xam-ui.png)

Wenn Sie die Werte mit einer Schnittstelle für den Schlüssel-Wert-Editor bearbeiten möchten, wählen Sie unten auf dem Bildschirm die Registerkarte **Quelle**> aus:

![Geräteausrichtungen in Visual Studio für Mac unterstützt.](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Um die Ausrichtung auf Android zu steuern, öffnen Sie **MainActivity.cs** , und legen Sie die Ausrichtung mithilfe des Attributs fest, das die `MainActivity` Klasse schmückt:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android unterstützt mehrere Optionen zum Angeben der Ausrichtung:

- **Landscape** &ndash; erzwingt, dass die Anwendungs Ausrichtung unabhängig von den Sensordaten Querformat ist.
- Hoch &ndash; erzwingt, dass die Anwendungs Ausrichtung unabhängig von den Sensordaten Hochformat ist.
- **Benutzer** &ndash; bewirkt, dass die Anwendung mithilfe der bevorzugten Ausrichtung des Benutzers angezeigt wird.
- **Hinter** &ndash; bewirkt, dass die Ausrichtung der Anwendung mit der Ausrichtung der dahinter liegenden [Aktivität](xref:Android.App.Activity) identisch ist.
- **Sensor** &ndash; bewirkt, dass die Ausrichtung der Anwendung durch den Sensor bestimmt wird, auch wenn der Benutzer die automatische Drehung deaktiviert hat.
- **Sensorlandscape** &ndash; bewirkt, dass die Anwendung Querformat verwendet, während Sensordaten verwendet werden, um die Richtung zu ändern, mit der der Bildschirm angezeigt wird (sodass der Bildschirm nicht als aufwärts angezeigt wird).
- **Sensorhoch** &ndash; bewirkt, dass die Anwendung die Hochformat Ausrichtung verwendet, während Sensordaten die Richtung ändern, mit der der Bildschirm angezeigt wird (sodass der Bildschirm nicht als aufwärts angezeigt wird).
- **Reverselandscape** &ndash; bewirkt, dass die Anwendung Querformat verwendet, wobei die entgegengesetzte Richtung von gewöhnlich aus angezeigt wird.
- **Reversehoch** &ndash; bewirkt, dass die Anwendung die Hochformat-Ausrichtung verwendet, wobei die entgegengesetzte Richtung von gewöhnlich aus angezeigt wird.
- Die **voll Sensor** &ndash; bewirkt, dass die Anwendung Sensordaten nutzt, um die richtige Ausrichtung auszuwählen (aus der möglichen 4).
- **Fulluser** &ndash; bewirkt, dass die Anwendung die Ausrichtungs Einstellungen des Benutzers verwendet. Wenn die automatische Drehung aktiviert ist, können alle 4 Ausrichtungen verwendet werden.
- **Userlandscape** -&ndash; _\[nicht unterstützt\]_ bewirkt, dass die Anwendung die Querformat Ausrichtung verwendet, es sei denn, für den Benutzer ist die automatische Rotation aktiviert. in diesem Fall wird der Sensor zum Bestimmen der Ausrichtung verwendet. Diese Option wird die Kompilierung unterbrochen.
- **Userportrait** &ndash; _\[nicht unterstützt\]_ bewirkt, dass die Anwendung die Hochformat Ausrichtung verwendet, es sei denn, für den Benutzer ist die automatische Rotation aktiviert. in diesem Fall wird der Sensor verwendet, um die Ausrichtung zu bestimmen. Diese Option wird die Kompilierung unterbrochen.
- **Gesperrte** &ndash; _\[nicht unterstützt\]_ bewirkt, dass die Anwendung die Bildschirm Ausrichtung verwendet, unabhängig davon, was beim Start erfolgt, ohne dass auf Änderungen in der physischen Ausrichtung des Geräts reagiert wird. Diese Option wird die Kompilierung unterbrochen.

Beachten Sie, dass die systemeigene Android-APIs bieten einen hohen steuern, wie die Ausrichtung verwaltet wird, einschließlich der Optionen, die des Benutzers explizit zu widersprechen Voreinstellungen ausgedrückt werden.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Beim universelle Windows-Plattform (UWP) werden unterstützte Ausrichtungen in der Datei " **Package. appxmanifest** " festgelegt. Öffnen das Manifest wird ein Konfigurationsfenster anzuzeigen, in denen unterstützte Ausrichtungen ausgewählt werden können.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Reagieren auf Änderungen im Ausrichtung

Xamarin.Forms bietet keine native Ereignisse für die Benachrichtigung von Ihrer app von Änderungen der bildschirmausrichtung in freigegebenem Code. [Xamarin. Essentials](~/essentials/index.md) enthält jedoch eine [`DeviceDisplay`]-Klasse, die Benachrichtigungen über die Ausrichtung von Änderungen bereitstellt.

Zum Erkennen von Ausrichtungen ohne xamarin. Essentials überwachen Sie das `SizeChanged`-Ereignis des `Page`, das ausgelöst wird, wenn die Breite oder Höhe des `Page` geändert wird. Wenn die Breite des `Page` größer als die Höhe ist, befindet sich das Gerät im Querformat. Weitere Informationen finden Sie unter [Anzeigen eines Bilds auf der Grundlage der Bildschirm Ausrichtung](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation).

Es ist auch möglich, die [`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*) -Methode für eine `Page`zu überschreiben und dort jede beliebige layoutänderungslogik einzufügen. Die `OnSizeAllocated`-Methode wird immer dann aufgerufen, wenn einer `Page` eine neue Größe zugewiesen wird. Dies geschieht, wenn das Gerät gedreht wird. Beachten Sie, dass die Basis Implementierung von `OnSizeAllocated` wichtige Layoutfunktionen ausführt, daher ist es wichtig, die Basis Implementierung in der Überschreibung aufzurufen:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Fehler bei diesem Schritt führt zu einer Seite nicht funktionsfähigen.

Beachten Sie, dass die `OnSizeAllocated`-Methode möglicherweise mehrmals aufgerufen wird, wenn ein Gerät gedreht wird. Ändern das Layout jedes Mal, ist Vergeudung von Ressourcen und kann zu Flimmern führen. Erwägen Sie eine Instanzvariable innerhalb Ihrer Seite, um nachzuverfolgen, ob die Ausrichtung im Querformat oder Hochformat befindet, und nur neu zeichnen Sie, wenn eine Änderung vorliegt:

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

Sobald eine Änderung der geräteausrichtung erkannt wird, empfiehlt es sich beim Hinzufügen oder entfernen zusätzliche Ansichten in der Benutzeroberfläche, auf die Änderung in den verfügbaren Speicherplatz zu reagieren. Betrachten Sie z. B. des integrierte Rechners auf jeder Plattform im Hochformat aus:

![](device-orientation-images/calculator-portrait.png "Calculator Application in Portrait")

und Querformat:

![](device-orientation-images/calculator-landscape.png "Calculator Application in Landscape")

Beachten Sie, dass die apps von den verfügbaren Speicherplatz nutzen, indem weitere Funktionen im Querformat hinzu.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Dynamisches Layout

Es ist möglich, Design-Schnittstellen, die mithilfe von den integrierten Layouts, damit sie ordnungsgemäß übergehen, wenn das Gerät gedreht wird. Berücksichtigen Sie beim Entwerfen von Schnittstellen, die weiterhin attraktiv sein, die Reaktion auf Änderungen in Ausrichtung auf folgenden allgemeinen Regeln:

- **Achten Sie auf die Verhältnisse,** &ndash; Änderungen der Ausrichtung Probleme verursachen können, wenn bestimmte Annahmen hinsichtlich der Verhältnisse getroffen werden. Z. B. eine Sicht, die ausreichend Speicherplatz in 1/3, der den vertikalen Platz eines Bildschirms im Hochformat hätte passt möglicherweise nicht in 1/3, der den vertikalen Platz im Querformat.
- **Seien Sie vorsichtig mit absoluten Werten** &ndash; absoluten (Pixel-) Werten, die im Hochformat sinnvoll sind, im Querformat möglicherweise keinen Sinn machen. Wenn Absolute Werte erforderlich sind, verwenden Sie geschachtelte Layouts, um deren Auswirkungen zu isolieren. Beispielsweise wäre es sinnvoll, absolute Werte in einer `TableView` `ItemTemplate` zu verwenden, wenn die Element Vorlage eine garantierte einheitliche Höhe aufweist.

Die oben genannten Regeln gelten auch beim Implementieren Schnittstellen für mehrere Bildschirmgrößen und sind im Allgemeinen Best Practice betrachtet. Im Rest dieses Leitfadens werden spezifische Beispiele von reaktionsfähigen Layouts mit den einzelnen primären Layouts in Xamarin.Forms erläutert.

> [!NOTE]
> Aus Gründen der Übersichtlichkeit wird in den folgenden Abschnitten veranschaulicht, wie reaktionsfähige Layouts mit nur einer Art von `Layout` gleichzeitig implementiert werden. In der Praxis ist es häufig einfacher, `Layout`en zu kombinieren, um ein gewünschtes Layout zu erzielen, indem die einfachere oder intuitivste `Layout` für jede Komponente verwendet wird.

### <a name="stacklayout"></a>StackLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-stack-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-stack-landscape.png "Photo Application in Landscape")

Die erfolgt mit den folgenden XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Einige C# werden verwendet, um die Ausrichtung von `outerStack` basierend auf der Ausrichtung des Geräts zu ändern:

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

Beachten Sie Folgendes:

- `outerStack` wird so angepasst, dass das Bild und die Steuerelemente abhängig von der Ausrichtung als horizontaler oder vertikaler Stapel dargestellt werden, um den verfügbaren Platz optimal zu nutzen.

### <a name="absolutelayout"></a>Von "AbsoluteLayout"

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-abs-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-abs-landscape.png "Photo Application in Landscape")

Die erfolgt mit den folgenden XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Beachten Sie Folgendes:

- Aufgrund der Art und Weise, die die Seite angeordnet wurden, besteht keine Notwendigkeit für prozeduralen Code Reaktionsfähigkeit Einführung zur Verfügung.
- Die `ScrollView` wird verwendet, damit die Bezeichnung auch dann sichtbar ist, wenn die Höhe des Bildschirms kleiner ist als die Summe der Höhen der Schaltflächen und des Bilds.

### <a name="relativelayout"></a>RelativeLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-rel-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-rel-landscape.png "Photo Application in Landscape")

Die erfolgt mit den folgenden XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

Beachten Sie Folgendes:

- Aufgrund der Art und Weise, die die Seite angeordnet wurden, besteht keine Notwendigkeit für prozeduralen Code Reaktionsfähigkeit Einführung zur Verfügung.
- Die `ScrollView` wird verwendet, damit die Bezeichnung auch dann sichtbar ist, wenn die Höhe des Bildschirms kleiner ist als die Summe der Höhen der Schaltflächen und des Bilds.

### <a name="grid"></a>Raster

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-grid-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-grid-landscape.png "Photo Application in Landscape")

Die erfolgt mit den folgenden XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Zusammen mit den folgenden prozeduralen Code, Drehung, Änderungen zu behandeln:

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

Beachten Sie Folgendes:

- Aufgrund der Art und Weise, die die Seite angeordnet wurden, besteht eine Methode zum Ändern der Raster-Platzierung der Steuerelemente zur Verfügung.

## <a name="related-links"></a>Verwandte Themen

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Businesstumm Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [Reaktionsfähiges Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [Anzeigen eines Bilds auf der Grundlage der Bildschirm Ausrichtung](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
