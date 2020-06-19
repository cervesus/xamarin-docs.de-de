---
title: Geräteausrichtung
description: In diesem Artikel wird erläutert, wie Sie Xamarin.Forms Anwendungen, die in hoch-und Querformat aussehen, hervorragend gestalten.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b1a47d4dcc92fca4d280708a2cbbe9374c17da8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573292"
---
# <a name="device-orientation"></a>Geräteausrichtung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

Es ist wichtig zu verstehen, wie Ihre Anwendung verwendet wird und wie die quer Ausrichtung integriert werden kann, um die Benutzerumgebung zu verbessern. Einzelne Layouts können so entworfen werden, dass Sie mehrere Ausrichtungen abdecken und den verfügbaren Platz optimal nutzen. Auf Anwendungsebene kann die Drehung deaktiviert oder aktiviert werden.

## <a name="controlling-orientation"></a>Steuern der Ausrichtung

Wenn Sie verwenden Xamarin.Forms , besteht die unterstützte Methode zum Steuern der Geräte Ausrichtung darin, die Einstellungen für die einzelnen Projekte zu verwenden.

### <a name="ios"></a>iOS

Unter IOS wird die Geräte Ausrichtung für Anwendungen konfiguriert, die die **Info. plist** -Datei verwenden. Verwenden Sie die oben in diesem Dokument beschriebenen IDE-Optionen, um auszuwählen, welche Anweisungen Sie anzeigen möchten:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Öffnen Sie in Visual Studio das IOS-Projekt, und öffnen Sie " **Info. plist**". Die Datei wird in einem Konfigurations Panel geöffnet, beginnend mit der Registerkarte "iPhone Deployment Info":

![iPhone-Bereitstellungs Informationen in Visual Studio](device-orientation-images/orientation-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Öffnen Sie in Visual Studio für Mac das IOS-Projekt, und öffnen Sie die Datei **Info. plist**. Auf der Registerkarte **Anwendung** stehen Abschnitte zum Festlegen der Ausrichtung zur Verfügung:

![iPhone-Bereitstellungs Informationen in Visual Studio für Mac](device-orientation-images/orientation-vsmac.png)

Wenn Sie die Werte mit einer Schnittstelle für den Schlüssel-Wert-Editor bearbeiten möchten, wählen Sie unten auf dem Bildschirm die Registerkarte **Quelle**> aus:

![Unterstützte Geräte Ausrichtungen in Visual Studio für Mac](device-orientation-images/orientation-source-vsmac.png)

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

Xamarin. Android unterstützt mehrere Optionen zum Angeben der Ausrichtung:

- **Quer** &ndash; Format erzwingt, dass die Anwendungs Ausrichtung unabhängig von den Sensordaten Querformat ist.
- Hochformat **Portrait** &ndash; erzwingt, dass die Anwendungs Ausrichtung unabhängig von den Sensordaten Hochformat ist.
- **Benutzer** &ndash; bewirkt, dass die Anwendung mithilfe der bevorzugten Ausrichtung des Benutzers angezeigt wird.
- **Hinter** &ndash; bewirkt, dass die Ausrichtung der Anwendung mit der Ausrichtung der dahinter liegenden [Aktivität](xref:Android.App.Activity) identisch ist.
- **Sensor** &ndash; bewirkt, dass die Ausrichtung der Anwendung vom Sensor bestimmt wird, auch wenn der Benutzer die automatische Drehung deaktiviert hat.
- **Sensorlandscape** &ndash; bewirkt, dass die Anwendung Querformat verwendet, während Sensordaten verwendet werden, um die Richtung zu ändern, mit der der Bildschirm angezeigt wird (sodass der Bildschirm nicht als aufwärts angezeigt wird).
- **Sensorportrait** &ndash; bewirkt, dass die Anwendung die Hochformat Ausrichtung verwendet, während Sensordaten verwendet werden, um die Richtung des Bildschirms zu ändern (sodass der Bildschirm nicht als aufwärts angezeigt wird).
- **Reverselandscape** &ndash; bewirkt, dass die Anwendung Querformat verwendet, wobei die entgegengesetzte Richtung von gewöhnlich aus angezeigt wird.
- **Reverum Hochformat** &ndash; bewirkt, dass die Anwendung die Hochformat Ausrichtung verwendet, wobei die entgegengesetzte Richtung von gewöhnlich aus angezeigt wird.
- **Fullsensor** &ndash; bewirkt, dass die Anwendung Sensordaten nutzt, um die richtige Ausrichtung (aus der möglichen 4) auszuwählen.
- **Fulluser** &ndash; bewirkt, dass die Anwendung die Ausrichtungs Einstellungen des Benutzers verwendet. Wenn die automatische Drehung aktiviert ist, können alle vier Ausrichtungen verwendet werden.
- **Userlandscape** &ndash; _ \[ Nicht unter \] stützt_ bewirkt, dass die Anwendung die quer Ausrichtung verwendet, es sei denn, für den Benutzer ist die automatische Rotation aktiviert. in diesem Fall wird der Sensor verwendet, um die Ausrichtung zu bestimmen. Diese Option wird die Kompilierung abbrechen.
- **Userportrait** &ndash; _ \[ Nicht unter \] stützt_ bewirkt, dass die Anwendung die Hochformat Ausrichtung verwendet, es sei denn, für den Benutzer ist die automatische Rotation aktiviert. in diesem Fall wird der Sensor zum Bestimmen der Ausrichtung verwendet. Diese Option wird die Kompilierung abbrechen.
- **Gesperrt** &ndash; _ \[ Nicht unter \] stützt_ bewirkt, dass die Anwendung die Bildschirm Ausrichtung verwendet, unabhängig davon, was beim Start erfolgt, ohne dass auf Änderungen der physischen Ausrichtung des Geräts reagiert wird. Diese Option wird die Kompilierung abbrechen.

Beachten Sie, dass die nativen Android-APIs eine große Kontrolle über die Verwaltung der Ausrichtung bieten, einschließlich Optionen, die den vom Benutzer angegebenen Einstellungen explizit widersprechen.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Beim universelle Windows-Plattform (UWP) werden unterstützte Ausrichtungen in der Datei " **Package. appxmanifest** " festgelegt. Wenn Sie das Manifest öffnen, wird ein Konfigurations Panel angezeigt, in dem unterstützte Ausrichtungen ausgewählt werden können.

## <a name="reacting-to-changes-in-orientation"></a>Reagieren auf Änderungen der Ausrichtung

Xamarin.Formsbietet keine nativen Ereignisse für die Benachrichtigung Ihrer APP mit Ausrichtungs Änderungen in frei gegebenem Code. Enthält jedoch [Xamarin.Essentials](~/essentials/index.md) eine [ `DeviceDisplay` ]-Klasse, die Benachrichtigungen über Orientierungsänderungen bereitstellt.

Zum Erkennen von Ausrichtungen ohne Xamarin.Essentials Überwachen Sie das- `SizeChanged` Ereignis des `Page` , das ausgelöst wird, wenn die Breite oder Höhe der `Page` Änderungen geändert wird. Wenn die Breite von `Page` größer als die Höhe ist, befindet sich das Gerät im Querformat. Weitere Informationen finden Sie unter [Anzeigen eines Bilds auf der Grundlage der Bildschirm Ausrichtung](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation).

Alternativ ist es möglich, die- [`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*) Methode für einen zu überschreiben `Page` , wobei jede beliebige layoutänderungslogik eingefügt werden kann. Die- `OnSizeAllocated` Methode wird immer dann aufgerufen, wenn eine `Page` neue Größe zugewiesen wird. Dies geschieht, wenn das Gerät gedreht wird. Beachten Sie, dass die Basis Implementierung von `OnSizeAllocated` wichtige Layoutfunktionen ausführt, daher ist es wichtig, die Basis Implementierung in der Überschreibung aufzurufen:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Wenn Sie diesen Schritt nicht ausführen, führt dies zu einer nicht funktionsfähigen Seite.

Beachten Sie, dass die- `OnSizeAllocated` Methode möglicherweise mehrmals aufgerufen wird, wenn ein Gerät gedreht wird. Wenn Sie das Layout jedes Mal ändern, ist dies eine Verschwendung von Ressourcen und kann zu einem Flimmern führen. Ziehen Sie in Erwägung, eine Instanzvariable auf der Seite zu verwenden, um zu verfolgen, ob sich die Ausrichtung im Querformat oder im Hochformat befindet, und zeichnen Sie nur bei einer Änderung

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

Nachdem eine Änderung der Geräte Orientierung erkannt wurde, können Sie der Benutzeroberfläche weitere Ansichten hinzufügen oder von ihr entfernen, um auf die Änderung des verfügbaren Speicherplatzes zu reagieren. Sehen Sie sich beispielsweise den integrierten Rechner auf jeder Plattform im Hochformat an:

![](device-orientation-images/calculator-portrait.png "Calculator Application in Portrait")

und Querformat:

![](device-orientation-images/calculator-landscape.png "Calculator Application in Landscape")

Beachten Sie, dass die apps den verfügbaren Speicherplatz nutzen, indem Sie mehr Funktionalität in Landscape hinzufügen.

## <a name="responsive-layout"></a>Reaktionsfähiges Layout

Es ist möglich, Schnittstellen mithilfe der integrierten Layouts zu entwerfen, damit Sie beim Drehen des Geräts ordnungsgemäß wechseln. Beachten Sie beim Entwerfen von Schnittstellen, die bei der Reaktion auf Änderungen bei der Ausrichtung weiterhin ansprechend sind, die folgenden allgemeinen Regeln:

- Achten **Sie auf die Verhältnisse** &ndash; . Änderungen an der Ausrichtung können Probleme verursachen, wenn bestimmte Annahmen hinsichtlich der Verhältnisse getroffen werden. Beispielsweise kann eine Sicht, die in 1/3 für den vertikalen Bereich eines Bildschirms im Hochformat viel Speicherplatz hätte, nicht in den Raum 1/3 des vertikalen Raums im Querformat passen.
- **Seien Sie vorsichtig mit absoluten Werten** &ndash; . absolute (Pixel-) Werte, die im Hochformat sinnvoll sind, sind im Querformat möglicherweise nicht sinnvoll. Wenn absolute Werte erforderlich sind, isolieren Sie Ihre Auswirkung mithilfe von "netsted Layouts". Beispielsweise wäre es sinnvoll, absolute Werte in einem zu verwenden, `TableView` `ItemTemplate` Wenn die Element Vorlage eine garantierte einheitliche Höhe aufweist.

Die oben genannten Regeln gelten auch für die Implementierung von Schnittstellen für mehrere Bildschirmgrößen und werden im Allgemeinen als bewährte Methoden betrachtet. Im weiteren Verlauf dieses Handbuchs werden bestimmte Beispiele für reaktionsfähige Layouts erläutert, die die einzelnen primären Layouts von verwenden Xamarin.Forms .

> [!NOTE]
> Aus Gründen der Übersichtlichkeit wird in den folgenden Abschnitten veranschaulicht, wie reaktionsfähige Layouts mit nur einem Typ von `Layout` gleichzeitig implementiert werden. In der Praxis ist es häufig einfacher, `Layout` s zu kombinieren, um ein gewünschtes Layout zu erzielen, indem die einfachere oder intuitivste `Layout` für jede Komponente verwendet wird.

### <a name="stacklayout"></a>StackLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt wird:

![](device-orientation-images/photo-stack-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-stack-landscape.png "Photo Application in Landscape")

Dies wird mit dem folgenden XAML erreicht:

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

Einige c#-Informationen werden verwendet, um die Ausrichtung von `outerStack` basierend auf der Ausrichtung des Geräts zu ändern:

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

- `outerStack`wird angepasst, um das Bild und die Steuerelemente je nach Ausrichtung als horizontalen oder vertikalen Stapel darzustellen, um den verfügbaren Platz optimal zu nutzen.

### <a name="absolutelayout"></a>AbsoluteLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt wird:

![](device-orientation-images/photo-abs-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-abs-landscape.png "Photo Application in Landscape")

Dies wird mit dem folgenden XAML erreicht:

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

- Aufgrund der Art und Weise, wie die Seite angelegt wurde, ist es nicht erforderlich, dass prozeduraler Code die Reaktionsfähigkeit einführt.
- Wird `ScrollView` verwendet, um zuzulassen, dass die Bezeichnung sichtbar ist, auch wenn die Höhe des Bildschirms kleiner ist als die Summe der Höhen der Schaltflächen und des Bilds.

### <a name="relativelayout"></a>RelativeLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt wird:

![](device-orientation-images/photo-rel-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-rel-landscape.png "Photo Application in Landscape")

Dies wird mit dem folgenden XAML erreicht:

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

- Aufgrund der Art und Weise, wie die Seite angelegt wurde, ist es nicht erforderlich, dass prozeduraler Code die Reaktionsfähigkeit einführt.
- Wird `ScrollView` verwendet, um zuzulassen, dass die Bezeichnung sichtbar ist, auch wenn die Höhe des Bildschirms kleiner ist als die Summe der Höhen der Schaltflächen und des Bilds.

### <a name="grid"></a>Raster

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt wird:

![](device-orientation-images/photo-grid-portrait.png "Photo Application in Portrait")

und Querformat:

![](device-orientation-images/photo-grid-landscape.png "Photo Application in Landscape")

Dies wird mit dem folgenden XAML erreicht:

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

Zusammen mit dem folgenden prozeduralen Code zum Behandeln von Rotations Änderungen:

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

- Aufgrund der Art und Weise, wie die Seite angelegt wurde, gibt es eine Methode, um die Raster Platzierung der Steuerelemente zu ändern.

## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Businesstumm Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [Reaktionsfähiges Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [Anzeigen eines Bilds auf der Grundlage der Bildschirm Ausrichtung](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
