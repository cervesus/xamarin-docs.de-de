---
title: "Geräteausrichtung"
description: "Verstehen Sie, wie zum Erstellen von Anwendungen, die großartig in Hochformat- und querformatausrichtung Ausrichtungen aussehen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: cb17c224fc6102d9e0dc25853c2222734299647a
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="device-orientation"></a>Geräteausrichtung

Es ist wichtig zu berücksichtigen, wie Ihre Anwendung verwendet werden soll und wie Querformat integriert werden kann, um die benutzerfreundlichkeit zu verbessern. Einzelne Layouts können entworfen werden, um mehrere Ausrichtungen aufzunehmen und am besten verwendet den verfügbaren Speicherplatz. Auf der Anwendungsebene kann Drehung deaktiviert oder aktiviert werden.

Dieser Artikel führt Sie durch das Erstellen von apps, die Ausrichtung von Gerätefunktionen nutzen und enthält die folgenden Abschnitte:

- **[Steuern der Ausrichtung](#Controlling_Orientation)**  &ndash; zu verstehen, wie zur Steuerung der Ausrichtung auf app-Ebene jede Plattform.
- **[Das reagieren auf Änderungen in Ausrichtung](#Reacting_to_Changes_in_Orientation)**  &ndash; erfahren Sie, wie Sie benachrichtigt werden, und darauf reagieren, werden Änderungen in der Ausrichtung.
- **[Dynamisches Layout](#Responsive_Layout)**  &ndash; erfahren Sie, wie Layouts erstellen, die automatisch über quer- und Hochformat Ausrichtungen arbeiten.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Steuern der Ausrichtung

Wenn Sie Xamarin.Forms verwenden, ist das unterstützte Verfahren zum Steuern des Geräts auf die Einstellungen für jedes einzelne Projekt verwenden.

> [!NOTE]
> Ab Xamarin.Forms 1.5.0, liegt ein Fehler wird verhindert, dass, versucht Renderer basierende benutzerdefinierte Ausrichtung zu Fehlern zu steuern. Finden Sie unter [in dieser Diskussion](https://forums.xamarin.com/discussion/46653/forcing-landscape-for-a-single-page-in-ios#latest)dieser Diskussion in die Xamarin-Foren für Weitere Informationen.

### <a name="ios"></a>iOS

Bei iOS kann geräteausrichtung konfiguriert ist, für Anwendungen, die **"Info.plist"** Datei. Diese Datei wird Ausrichtung-Einstellungen für iPhone & iPod sowie Einstellungen für iPad einschließen, wenn die app als Ziel enthält. Im folgenden sind die Anweisungen, die spezifisch für die IDE. Verwenden Sie die IDE-Optionen am oberen Rand dieses Dokuments an, welche Anweisungen auswählen, finden Sie unter möchten:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Visual Studio, öffnen Sie das iOS-Projekt, und öffnen Sie **"Info.plist"**. Die Datei wird in einem Konfigurationsfenster, beginnend mit der iPhone-Registerkarte "Bereitstellung-Info" zu öffnen:

![iPhone Deployment Info in Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Wählen Sie zum Konfigurieren der iPad-Ausrichtung der **iPad Bereitstellungsinformationen** Registerkarte im oberen linken Bereich des Bereichs, dann wählen Sie aus den verfügbaren Ausrichtungen:

![Unterstützte Geräte Ausrichtungen in Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Klicken Sie in Visual Studio für Mac öffnen Sie das iOS-Projekt, und öffnen Sie **"Info.plist"**. Klicken Sie unter der **Anwendung** Registerkarte Abschnitte werden für die Papierhöhe verfügbar sein:

![iPhone Bereitstellungsinformationen in Visual Studio für Mac](device-orientation-images/orientation-xam-ui.png)

Wenn Sie lieber so bearbeiten Sie die Werte, die über ein Schlüssel-Wert Editorschnittstelle, wählen Sie die **Quelle**> Registerkarte am unteren Rand des Bildschirms:

![Gerät Ausrichtungen in Visual Studio für Mac unterstützt.](device-orientation-images/orientation-xam-source.png)

-----


### <a name="android"></a>Android

Um die Ausrichtung auf Android-Geräten steuern möchten, öffnen Sie **MainActivity.cs** und legen Sie die Ausrichtung, die mit dem Attribut ergänzen die `MainActivity` Klasse:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android unterstützt mehrere Optionen zum Angeben der Ausrichtung:

- **Querformat** &ndash; erzwingt die Anwendung Ausrichtung auf Querformat, unabhängig von der Sensordaten.
- **Hochformat** &ndash; erzwingt die Anwendung Ausrichtung auf Hochformat, unabhängig von der Sensordaten.
- **Benutzer** &ndash; bewirkt, dass die Anwendung mit der Benutzer bevorzugte Ausrichtung präsentiert werden.
- **Hinter** &ndash; bewirkt, dass die Anwendung Ausrichtung auf die Ausrichtung des identisch der [Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/) vortransformierungsphase.
- **Temperatursensor** &ndash; bewirkt, dass die Anwendung die Ausrichtung durch die Sensor, der bestimmt, selbst wenn der Benutzer Automatisches Drehung deaktiviert hat.
- **SensorLandscape** &ndash; bewirkt, dass die Anwendung für die Verwendung von Querformat bei der Verwendung von Sensordaten so ändern Sie die Richtung der Bildschirm mit Internetzugriff ist, (, damit der Bildschirm als stehend angezeigt wird nicht).
- **SensorPortrait** &ndash; bewirkt, dass die Anwendung für die Verwendung von Hochformat bei der Verwendung von Sensordaten so ändern Sie die Richtung der Bildschirm mit Internetzugriff ist, (, damit der Bildschirm als stehend angezeigt wird nicht).
- **ReverseLandscape** &ndash; bewirkt, dass die Anwendung für die Verwendung von Querformat Gegenrichtung aus üblich, dass "stehend." angezeigt.
- **ReversePortrait** &ndash; bewirkt, dass die Anwendung für die Verwendung von Hochformat Gegenrichtung aus üblich, dass "stehend." angezeigt.
- **FullSensor** &ndash; bewirkt, dass die Anwendung Sensor-Daten, und wählen Sie die richtige Ausrichtung (aus den möglichen 4) basieren.
- **FullUser** &ndash; bewirkt, dass die Anwendung verwenden Sie die Einstellungen des Benutzers Ausrichtung. Wenn automatische Drehung aktiviert ist, können alle 4 Ausrichtungen verwendet werden.
- **UserLandscape** &ndash;  _\[nicht unterstützt\]_  bewirkt, dass die Anwendung für die Verwendung von Querformat, es sei denn, der Benutzer Automatisches Drehung aktiviert ist, wird in diesem Fall verwendet, die Temperatursensor Ausrichtung zu bestimmen. Diese Option wird die Kompilierung unterbrochen.
- **UserPortrait** &ndash;  _\[nicht unterstützt\]_  bewirkt, dass die Anwendung für die Verwendung von Hochformat, es sei denn, der Benutzer Automatisches Drehung aktiviert ist, wird in diesem Fall verwendet, die Temperatursensor Ausrichtung zu bestimmen. Diese Option wird die Kompilierung unterbrochen.
- **Gesperrt** &ndash;  _\[nicht unterstützt\]_  bewirkt, dass die Anwendung zur Verwendung der bildschirmausrichtung, was beim Starten, ist es ohne reagieren auf Änderungen in das Gerät den physikalischen Ausrichtung. Diese Option wird die Kompilierung unterbrochen.

Beachten Sie, dass die systemeigene Android-APIs bieten einen hohen steuern, wie die Ausrichtung verwaltet wird, einschließlich der Optionen, die des Benutzers explizit widersprechen Voreinstellungen ausgedrückt werden.

### <a name="windows-phone"></a>Windows Phone

Auf Windows Phone-RT, unterstützten Ausrichtungen festgelegt sind, der <span class="UIItem">"Package.appxmanifest"</span> Datei. Öffnen das Manifest wird ein Panel Konfiguration anzuzeigen, in denen unterstützten Ausrichtungen ausgewählt werden können:

![](device-orientation-images/vs-winrt-config.png ""Package.appxmanifest" Visual-Editor")

Unter Windows Phone 8 (Silverlight) unterstützten Ausrichtungen festgelegt sind, im Code in der <span class="UIItem">"MainPage.Xaml.cs"</span> Datei. In der Standard-Projektvorlage wird der Wert bereits mit der folgenden Zeile des Codes festgelegt:

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
```

Ersetzen Sie zum Angeben der Ausrichtung, die auf Windows Phone-Optionen, die durch den Code So aktivieren Sie die Ausrichtungen an, die Sie möchten:

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
SupportedOrientations = SupportedPageOrientation.Portrait; // portrait only
SupportedOrientations = SupportedPageOrientation.Landscape; // landscape only
```

Beachten Sie, dass Windows Phone-Landscape Ansichten in beiden unterstützt (wie von Hochformat dargestellt) Ausrichtungen von links nach rechts und rechts-nach-links. Es ist nicht möglich, um anzugeben, die verwendet wird.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Das reagieren auf Änderungen in der Ausrichtung

Xamarin.Forms bietet keine systemeigene Ereignisse für Ihre app Ausrichtung Änderungen im freigegebenen Code benachrichtigen. Allerdings die `SizeChanged` -Ereignis für die `Page` wird ausgelöst, wenn der Breite oder der Höhe des der `Page` Änderungen. Wenn die Breite des der `Page` ist größer als die Höhe des Geräts wird im Querformat. Weitere Informationen finden Sie unter [zeigen Sie ein Bild, das basierend auf Bildschirmausrichtung](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/).

> [!NOTE]
> Es wird eine vorhandene, kostenlose NuGet-Paket für den Empfang von Benachrichtigungen über Änderungen der Ausrichtung im freigegebenen Code. Finden Sie unter der [GitHub-Repository](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) für Weitere Informationen.

Alternativ ist es möglich, überschreiben die [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/) Methode auf eine `Page`, Einfügen von jedes Layout ändern Logik vorhanden. Die `OnSizeAllocated` Methode wird aufgerufen, wenn ein `Page` erhält eine neue Größe, die Whenver ausgeführt wird, wird das Gerät gedreht. Beachten Sie, dass die grundlegende Implementierung der `OnSizeAllocated` führt wichtige Layout-Funktionen, daher ist es wichtig, die grundlegende Implementierung in der Überschreibung aufrufen:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Dieses Schritts wird auf einer Seite nicht funktionsfähigen kommen.

Beachten Sie, dass die `OnSizeAllocated` Methode kann mehrfach aufgerufen, wenn ein Gerät gedreht wird. Ändern das Layout jedes Mal gehen zu Lasten der Ressourcen ist und zu Flimmern führen kann. In Betracht einer Instanzvariablen innerhalb der Seite zum Nachverfolgen, ob die Ausrichtung im Quer- oder Hochformat befindet, und nur neu gezeichnet werden, wenn eine Änderung:

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

Sobald eine Änderung in geräteausrichtung erkannt wurde, empfiehlt es sich beim Hinzufügen oder Entfernen von zusätzlichen Ansichten in der Benutzeroberfläche, auf die Änderung in den verfügbaren Speicherplatz zu reagieren. Betrachten Sie beispielsweise den integrierten Rechner auf jeder Plattform im Hochformat aus:

![](device-orientation-images/calculator-portrait.png "Rechneranwendung im Hochformat")

und Querformat:

![](device-orientation-images/calculator-landscape.png "Rechneranwendung im Querformat")

Beachten Sie, dass die apps an verfügbarem Speicherplatz nutzen durch Hinzufügen von zusätzlichen Funktionen im Querformat.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Dynamisches Layout

Es ist möglich, Design-Schnittstellen, die mithilfe der integrierten Layouts, damit sie ordnungsgemäß Übergang erfolgt, wenn das Gerät gedreht wird. Beim Entwerfen von Schnittstellen, die weiterhin ansprechend werden bei der Reaktion auf Änderungen in Ausrichtung betrachten Sie die folgenden allgemeinen Regeln:

- **Achten Sie darauf, Verhältnissen** &ndash; Änderungen in Ausrichtung können Probleme verursachen, wenn bestimmte Annahmen im Hinblick auf die Verhältnissen vorgenommen werden. Beispielsweise kann eine Sicht, die viel Speicherplatz in 1/3 des vertikalen Platz eines Bildschirms im Hochformat hätte nicht 1/3 des vertikalen Platz im Querformat ausreicht.
- **Achten Sie darauf, dass Sie mit absoluten Werten** &ndash; Absolute (Pixel)-Werte, die im Hochformat sinnvoll möglicherweise nicht sinnvoll im Querformat. Wenn Absolute Werte erforderlich sind, verwenden Sie geschachtelte Layouts, um ihre Auswirkungen zu isolieren. Beispielsweise wäre es sinnvoll, verwenden Absolute Werte in einer `TableView` `ItemTemplate` Wenn die Elementvorlage eine garantierte uniform Höhe hat.

Die oben genannten Regeln gelten auch beim Implementieren von Schnittstellen für mehrere Bildschirmgrößen und sind im Allgemeinen Best Practices betrachtet. Der Rest dieses Handbuchs werden spezifische Beispiele reaktionsfähig Layouts mit den einzelnen primären Layouts in Xamarin.Forms erläutert.

> [!NOTE]
> Aus Gründen der Übersichtlichkeit in den folgenden Abschnitten veranschaulichen, wie Sie reaktionsfähig verwenden nur eine Art von Layouts zu implementieren `Layout` zu einem Zeitpunkt. In der Praxis ist es oft einfacher zum Mischen von `Layout`s erzielen Sie eine gewünschte Layout verwenden, einfacher oder intuitivste `Layout` für jede Komponente.

### <a name="stacklayout"></a>StackLayout

Betrachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-stack-portrait.png "Foto-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-stack-landscape.png "Foto-Anwendung im Querformat")

Erfolgt durch den folgenden XAML-Code:

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

Einige C#-dient zum Ändern der Ausrichtung des `outerStack` basierend auf der Ausrichtung des Geräts:

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

- `outerStack` wird angepasst, um das Bild und Steuerelemente als horizontalen oder vertikalen Stapel je nach Ausrichtung, um den verfügbaren Platz optimal nutzen zu präsentieren.


### <a name="absolutelayout"></a>AbsoluteLayout

Betrachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-abs-portrait.png "Foto-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-abs-landscape.png "Foto-Anwendung im Querformat")

Erfolgt durch den folgenden XAML-Code:

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

- Aufgrund der Art die Seite angeordnet wurden, besteht keine Notwendigkeit für prozeduralen Code einführen Reaktionsfähigkeit zur Verfügung.
- Die `ScrollView` wird verwendet, um die Bezeichnung auch, wenn kleiner als die Summe der feste Höhe von Schaltflächen und das Bild der Höhe des Bildschirms ist sichtbar sein können.


### <a name="relativelayout"></a>RelativeLayout

Betrachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-rel-portrait.png "Foto-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-rel-landscape.png "Foto-Anwendung im Querformat")

Erfolgt durch den folgenden XAML-Code:

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

- Aufgrund der Art die Seite angeordnet wurden, besteht keine Notwendigkeit für prozeduralen Code einführen Reaktionsfähigkeit zur Verfügung.
- Die `ScrollView` wird verwendet, um die Bezeichnung auch, wenn kleiner als die Summe der feste Höhe von Schaltflächen und das Bild der Höhe des Bildschirms ist sichtbar sein können.

### <a name="grid"></a>Raster

Betrachten Sie die folgende Anwendung, die im Hochformat angezeigt:

![](device-orientation-images/photo-grid-portrait.png "Foto-Anwendung im Hochformat")

und Querformat:

![](device-orientation-images/photo-grid-landscape.png "Foto-Anwendung im Querformat")

Erfolgt durch den folgenden XAML-Code:

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

Zusammen mit dem folgenden prozeduralen Code so behandeln Sie Drehung Änderungen:

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

- Aufgrund der Art die Seite angeordnet, ist es eine Methode, um die Platzierung der Steuerelemente ändern.


## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Dynamisches Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Zeigen Sie ein Bild, das basierend auf Bildschirmausrichtung](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
