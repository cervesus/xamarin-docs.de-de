---
title: Geräteausrichtung
description: In diesem Artikel wird erläutert, wie Sie Layout Xamarin.Forms-Anwendungen, die im Hoch-und Querformat hervorragend aussehen.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: 7f0e1c27f7d6a62dc43ac447c4f796d685a6cd91
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241210"
---
# <a name="device-orientation"></a>Geräteausrichtung

Es ist wichtig zu berücksichtigen, wie Ihre Anwendung verwendet wird und wie Querformat integriert werden kann, um die benutzerfreundlichkeit zu verbessern. Einzelne Layouts können entworfen werden, um mehrere Ausrichtungen zu berücksichtigen und am besten verwenden Sie den verfügbaren Speicherplatz. Auf der Anwendungsebene kann Drehung deaktiviert oder aktiviert werden.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Steuern der Ausrichtung

Wenn Sie Xamarin.Forms verwenden, werden das unterstützte Verfahren zum Steuern des Geräts ab, die Einstellungen für jedes einzelne Projekt verwenden.

### <a name="ios"></a>iOS

Geräteausrichtung erfolgt unter iOS, für Anwendungen unter Verwendung der **"Info.plist"** Datei. Diese Datei enthält Einstellungen für die Ausrichtung für iPhone und iPod sowie Einstellungen für iPad, wenn die app als Ziel enthält. Im folgenden finden Anweisungen zu Ihrer IDE. Verwenden Sie die IDE-Optionen am oberen Rand dieses Dokuments, welche Anweisungen auswählen, finden Sie unter möchten:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Klicken Sie in Visual Studio, öffnen Sie das iOS-Projekt, und öffnen Sie **"Info.plist"**. Die Datei wird in einem Bereich Konfiguration angezeigt, beginnend mit der iPhone-Bereitstellungsinformationen-Registerkarte geöffnet:

![iPhone-Bereitstellungsinformationen in Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Wählen Sie zum Konfigurieren der iPad-Ausrichtung der **iPad-Bereitstellungsinformationen** Registerkarte oben links im Bereich, klicken Sie dann auswählen von die verfügbaren Ausrichtungen:

![Unterstützte Geräteausrichtungen in Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Klicken Sie in Visual Studio für Mac öffnen Sie das iOS-Projekt, und öffnen Sie **"Info.plist"**. Unter den **Anwendung** Registerkarte Abschnitte stehen Ausrichtung festgelegt:

![iPhone-Bereitstellungsinformationen in Visual Studio für Mac](device-orientation-images/orientation-xam-ui.png)

Wenn Sie lieber so bearbeiten Sie die Werte, die über eine Editorschnittstelle für Schlüssel-Wert-, wählen Sie die **Quelle**> Registerkarte am unteren Rand des Bildschirms:

![Geräteausrichtungen in Visual Studio für Mac unterstützt.](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Öffnen Sie zum Steuern der Ausrichtung auf Android **"mainactivity.cs"** und legen Sie die Orientierung mit dem Attribut versehen der `MainActivity` Klasse:

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

- **Querformat** &ndash; erzwingt, dass die Anwendung-Ausrichtung auf Querformat, unabhängig davon, Sensordaten sein.
- **Hochformat** &ndash; erzwingt, dass die Anwendung-Ausrichtung auf Hochformat, unabhängig von Daten von gerätesensoren zu werden.
- **Benutzer** &ndash; bewirkt, dass die Anwendung mithilfe der bevorzugte Ausrichtung des Benutzers angezeigt werden.
- **Hinter** &ndash; bewirkt Ausrichtung der Anwendung, mit der die Ausrichtung des identisch sein, dass die [Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/) dahinter.
- **Sensor** &ndash; bewirkt, dass die Anwendung die Ausrichtung vom Sensor, bestimmt werden soll, selbst wenn der Benutzer die automatische Drehung deaktiviert hat.
- **SensorLandscape** &ndash; bewirkt, dass die Anwendung zur Verwendung von Querformat bei der Verwendung von Daten von triebwerksensoren so ändern Sie die Richtung der Bildschirm befindet sich in einer (sodass der Bildschirm als auf dem Kopf stehend betrachtet wird).
- **SensorPortrait** &ndash; bewirkt, dass die Anwendung zur Verwendung von Hochformat bei der Verwendung von Daten von triebwerksensoren so ändern Sie die Richtung der Bildschirm befindet sich in einer (sodass der Bildschirm als auf dem Kopf stehend betrachtet wird).
- **ReverseLandscape** &ndash; bewirkt, dass die Anwendung zur Verwendung von Querformat, mit der entgegengesetzten Richtung üblich ist, um "auf dem Kopf stehend." angezeigt.
- **ReversePortrait** &ndash; bewirkt, dass die Anwendung zur Verwendung von Hochformat anzeigt, mit der entgegengesetzten Richtung üblich ist, um "auf dem Kopf stehend." angezeigt.
- **FullSensor** &ndash; bewirkt, dass die Anwendung abhängig von Sensordaten in die richtige Ausrichtung (nicht die möglichen 4) wählen.
- **FullUser** &ndash; bewirkt, dass die Anwendung verwenden Sie die Einstellungen des Benutzers Ausrichtung. Wenn die automatische Drehung aktiviert ist, können alle 4 Ausrichtungen verwendet werden.
- **UserLandscape** &ndash; _\[Nepodporuje\]_ bewirkt, dass die Anwendung zum Querformat, es sei denn, der Benutzer die automatische Drehung aktiviert ist, hat in diesem Fall verwendet der Sensor Ausrichtung zu bestimmen. Diese Option wird die Kompilierung unterbrochen.
- **UserPortrait** &ndash; _\[Nepodporuje\]_ bewirkt, dass die Anwendung zur Verwendung von Hochformat, es sei denn, der Benutzer die automatische Drehung aktiviert ist, hat in diesem Fall verwendet der Sensor Ausrichtung zu bestimmen. Diese Option wird die Kompilierung unterbrochen.
- **Gesperrt** &ndash; _\[Nepodporuje\]_ bewirkt, dass die Anwendung zur Verwendung der bildschirmausrichtung, was sie beim Start ist, ohne die Reaktion auf Änderungen an das Gerät den physikalischen die Ausrichtung. Diese Option wird die Kompilierung unterbrochen.

Beachten Sie, dass die systemeigene Android-APIs bieten einen hohen steuern, wie die Ausrichtung verwaltet wird, einschließlich der Optionen, die des Benutzers explizit zu widersprechen Voreinstellungen ausgedrückt werden.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Für die universelle Windows Plattform (UWP), in dem unterstützte Ausrichtungen festgelegt sind das **"Package.appxmanifest"** Datei. Öffnen das Manifest wird ein Konfigurationsfenster anzuzeigen, in denen unterstützte Ausrichtungen ausgewählt werden können.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Reagieren auf Änderungen im Ausrichtung

Xamarin.Forms bietet keine native Ereignisse für die Benachrichtigung von Ihrer app von Änderungen der bildschirmausrichtung in freigegebenem Code. Jedoch die `SizeChanged` Ereignis die `Page` wird ausgelöst, wenn entweder die Breite oder Höhe der `Page` Änderungen. Wenn die Breite des der `Page` ist größer als die Höhe des Geräts wird im Querformat. Weitere Informationen finden Sie unter [zeigen Sie ein Bild, das je nach Ausrichtung des Bildschirms](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation).

> [!NOTE]
> Es ist ein vorhandener, kostenlose NuGet-Paket für den Empfang von Benachrichtigungen über Änderungen der bildschirmausrichtung in freigegebenem Code. Finden Sie unter den [GitHub-Repository](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) für Weitere Informationen.

Alternativ ist es möglich, überschreiben die [ `OnSizeAllocated` ](xref:Xamarin.Forms.Page.OnSizeAllocated*) Methode für eine `Page`, Einfügen von Layouts ändern Logik vorhanden. Die `OnSizeAllocated` Methode wird aufgerufen, wenn eine `Page` wird eine neue Größe der Whenver eintritt, wird das Gerät gedreht, zugeordnet. Beachten Sie, dass die basisimplementierung für `OnSizeAllocated` führt wichtige Layout-Funktionen, daher ist es wichtig, überschreiben die basisimplementierung aufrufen:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Fehler bei diesem Schritt führt zu einer Seite nicht funktionsfähigen.

Beachten Sie, dass die `OnSizeAllocated` Methode kann mehrmals aufgerufen, wenn ein Gerät gedreht wird. Ändern das Layout jedes Mal, ist Vergeudung von Ressourcen und kann zu Flimmern führen. Erwägen Sie eine Instanzvariable innerhalb Ihrer Seite, um nachzuverfolgen, ob die Ausrichtung im Querformat oder Hochformat befindet, und nur neu zeichnen Sie, wenn eine Änderung vorliegt:

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

![](device-orientation-images/calculator-portrait.png "Rechner-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/calculator-landscape.png "Rechner-Anwendung im Querformat")

Beachten Sie, dass die apps von den verfügbaren Speicherplatz nutzen, indem weitere Funktionen im Querformat hinzu.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Dynamisches Layout

Es ist möglich, Design-Schnittstellen, die mithilfe von den integrierten Layouts, damit sie ordnungsgemäß übergehen, wenn das Gerät gedreht wird. Berücksichtigen Sie beim Entwerfen von Schnittstellen, die weiterhin attraktiv sein, die Reaktion auf Änderungen in Ausrichtung auf folgenden allgemeinen Regeln:

- **Achten Sie darauf, Verhältnissen** &ndash; Änderungen in Ausrichtung können Probleme verursachen, wenn bestimmte Annahmen im Hinblick auf die Verhältnisse vorgenommen werden. Z. B. eine Sicht, die ausreichend Speicherplatz in 1/3, der den vertikalen Platz eines Bildschirms im Hochformat hätte passt möglicherweise nicht in 1/3, der den vertikalen Platz im Querformat.
- **Achten Sie darauf, dass Sie mit absoluten Werten** &ndash; Absolutwerte (Pixel), die im Hochformat sinnvoll möglicherweise nicht sinnvoll im Querformat. Wenn Absolute Werte erforderlich sind, verwenden Sie geschachtelte Layouts, um deren Auswirkungen zu isolieren. Beispielsweise wäre es sinnvoll, den absoluten Werten in einer `TableView` `ItemTemplate` bei die Elementvorlage eine garantierte einheitliche Höhe hat.

Die oben genannten Regeln gelten auch beim Implementieren Schnittstellen für mehrere Bildschirmgrößen und sind im Allgemeinen Best Practice betrachtet. Im Rest dieses Leitfadens werden spezifische Beispiele von reaktionsfähigen Layouts mit den einzelnen primären Layouts in Xamarin.Forms erläutert.

> [!NOTE]
> Aus Gründen der Übersichtlichkeit in den folgenden Abschnitten veranschaulichen, wie reagiert, verwenden nur eine Art von Layouts zu implementieren `Layout` zu einem Zeitpunkt. In der Praxis ist es häufig einfacher zu mischen `Layout`s, um eine gewünschte Layout mit dem einfacher oder intuitivste erhalten `Layout` für jede Komponente.

### <a name="stacklayout"></a>StackLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-stack-portrait.png "Fotogalerie-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-stack-landscape.png "Fotogalerie-Anwendung im Querformat")

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

Einige C# -Code dient zum Ändern der Ausrichtung der `outerStack` je nach Ausrichtung des Geräts:

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

- `outerStack` wird angepasst, um das Image und die Steuerelemente als horizontalen oder vertikalen Stapel je nach Ausrichtung, den verfügbaren Platz optimal nutzen zu präsentieren.


### <a name="absolutelayout"></a>Von "AbsoluteLayout"

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-abs-portrait.png "Fotogalerie-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-abs-landscape.png "Fotogalerie-Anwendung im Querformat")

Die erfolgt mit den folgenden XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
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
- Die `ScrollView` wird verwendet, um die Bezeichnung auch, wenn kleiner als die Summe der feste Höhe der Schaltflächen und das Bild der Höhe des Bildschirms ist sichtbar sein können.


### <a name="relativelayout"></a>RelativeLayout

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-rel-portrait.png "Fotogalerie-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-rel-landscape.png "Fotogalerie-Anwendung im Querformat")

Die erfolgt mit den folgenden XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
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
- Die `ScrollView` wird verwendet, um die Bezeichnung auch, wenn kleiner als die Summe der feste Höhe der Schaltflächen und das Bild der Höhe des Bildschirms ist sichtbar sein können.

### <a name="grid"></a>Raster

Beachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-grid-portrait.png "Fotogalerie-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-grid-landscape.png "Fotogalerie-Anwendung im Querformat")

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


## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Dynamisches Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Zeigen Sie ein Bild, das je nach Ausrichtung des Bildschirms](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
