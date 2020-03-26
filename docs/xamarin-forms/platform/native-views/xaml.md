---
title: Native Ansichten in XAML
description: Native Ansichten in iOS, Android und die universelle Windows-Plattform können direkt von Xamarin.Forms-XAML-Dateien verwiesen werden. Eigenschaften und Ereignishandler für native Ansichten festgelegt werden können, und sie können mit Xamarin.Forms-Ansichten interagieren. In diesem Artikel wird veranschaulicht, wie native Ansichten in Xamarin.Forms XAML-Dateien genutzt wird.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
ms.openlocfilehash: 6d3954f44cd769ed02535eb260b9952e81e67c98
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247625"
---
# <a name="native-views-in-xaml"></a>Native Ansichten in XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_Von xamarin. Forms-XAML-Dateien kann direkt auf native Ansichten von IOS, Android und den universelle Windows-Plattform verwiesen werden. Eigenschaften und Ereignishandler können für Native Ansichten festgelegt werden, und Sie können mit xamarin. Forms-Sichten interagieren. In diesem Artikel wird veranschaulicht, wie Native Sichten von xamarin. Forms-XAML-Dateien verwendet werden._

In diesem Artikel werden die folgenden Themen behandelt:

- Verwenden von [nativen Ansichten](#consuming) – der Prozess für die Verwendung einer systemeigenen Ansicht aus XAML.
- [Verwenden nativer Bindungen](#native_bindings) – Datenbindung an und von Eigenschaften nativer Sichten.
- [Übergeben von Argumenten an Native Sichten](#passing_arguments) – übergeben von Argumenten an Native Ansichts Konstruktoren und Aufrufen von systemeigenen View Factory-Methoden.
- [Verweisen auf native Sichten aus Code](#native_view_code) – Abrufen von in einer XAML-Datei deklarierten nativen Ansichts Instanzen aus der Code Behind-Datei.
- [Unterklassen](#subclassing) für systemeigene Sichten – Unterklassen für Native Sichten zum Definieren einer XAML-freundlichen API.  

<a name="overview" />

## <a name="overview"></a>Übersicht

So betten Sie einen einheitlichen Einblick in einer Xamarin.Forms-XAML-Datei ein:

1. Fügen Sie in der XAML-Datei eine `xmlns`-Namespace Deklaration für den Namespace hinzu, der die native Ansicht enthält.
1. Erstellen Sie eine Instanz der einheitlichen Ansicht, in der XAML-Datei.

> [!IMPORTANT]
> Das kompilierte XAML muss für alle XAML-Seiten deaktiviert werden, die native Sichten verwenden. Dies kann erreicht werden, indem Sie die Code-Behind-Klasse für die XAML-Seite mit dem `[XamlCompilation(XamlCompilationOptions.Skip)]`-Attribut versehen. Weitere Informationen zur XAML-Kompilierung finden Sie unter [XAML-Kompilierung in xamarin. Forms](~/xamarin-forms/xaml/xamlc.md).

Um auf eine einheitliche Ansicht aus einer CodeBehind-Datei verweisen, verwenden Sie eine freigegebene Asset Projekt (SAP) und umschließen Sie den plattformspezifischen Code mit bedingten Kompilierungsdirektiven. Weitere Informationen finden [Sie unter verweisen auf native Sichten aus Code](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Nutzen Native Ansichten

Im folgenden Codebeispiel wird die Verwendung von nativen Ansichten für jede Plattform zu einem xamarin. Forms- [`ContentPage`](xref:Xamarin.Forms.ContentPage)veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Wenn Sie die `clr-namespace` und `assembly` für einen nativen Ansichts Namespace angeben, muss auch eine `targetPlatform` angegeben werden. Dieser Wert sollte auf `iOS`, `Android`, `UWP`, `Windows` (entspricht `UWP`), `macOS`, `GTK`, `Tizen`oder `WPF`festgelegt werden. Zur Laufzeit ignoriert der XAML-Parser alle XML-Namespace Präfixe, die über eine `targetPlatform` verfügen, die nicht der Plattform entspricht, auf der die Anwendung ausgeführt wird.

Jede Namespacedeklaration kann verwendet werden, um auf einer beliebigen Klasse oder Struktur aus dem angegebenen Namespace zu verweisen. Beispielsweise kann die `ios`-Namespace Deklaration verwendet werden, um auf eine beliebige Klasse oder Struktur aus dem IOS `UIKit`-Namespace zu verweisen. Eigenschaften der systemeigenen Ansicht können über XAML festgelegt werden, aber die Eigenschaft und die Objekt-Typen müssen übereinstimmen. Beispielsweise wird die `UILabel.TextColor`-Eigenschaft auf `UIColor.Red` mithilfe der `x:Static` Markup Erweiterung und des `ios`-Namespace festgelegt.

Bindbare Eigenschaften und angefügte bindbare Eigenschaften können auch in nativen Ansichten mithilfe der `Class.BindableProperty="value"`-Syntax festgelegt werden. Jede native Ansicht wird in eine plattformspezifische `NativeViewWrapper` Instanz umschließt, die von der [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) -Klasse abgeleitet wird. Überträgt die Einstellung eine bindbare Eigenschaft oder die angefügten bindbare Eigenschaft in einer einheitlichen Ansicht den Wert der Eigenschaft an den Wrapper. Beispielsweise kann ein zentriertes horizontales Layout angegeben werden, indem `View.HorizontalOptions="Center"` in der systemeigenen Ansicht festgelegt wird.

> [!NOTE]
> Beachten Sie, dass Stile nicht mit nativen Ansichten verwendet werden können, da Stile nur auf Eigenschaften abzielen können, die von `BindableProperty` Objekten unterstützt werden.

Android-widgekonstruktoren erfordern im Allgemeinen das Android-`Context` Objekt als Argument, und dieses kann über eine statische Eigenschaft in der `MainActivity`-Klasse zur Verfügung gestellt werden. Wenn Sie ein Android-Widget in XAML erstellen, muss das `Context` Objekt daher im Allgemeinen mithilfe des `x:Arguments`-Attributs mit einer `x:Static` Markup Erweiterung an den Konstruktor des Widgets übergeben werden. Weitere Informationen finden Sie unter [übergeben von Argumenten an Native Ansichten](#passing_arguments).

> [!NOTE]
> Beachten Sie, dass das Benennen einer systemeigenen Ansicht mit `x:Name` weder in einem .NET Standard Bibliotheksprojekt noch in einem freigegebenen Ressourcen Projekt (SAP) möglich ist. Auf diese Weise wird eine Variable des systemeigenen Typs generiert, der einen Kompilierungsfehler verursacht. Systemeigene Sichten können jedoch in `ContentView` Instanzen umschließt und in der Code Behind-Datei abgerufen werden, vorausgesetzt, dass ein SAP verwendet wird. Weitere Informationen finden Sie unter [verweisen auf eine native Ansicht aus dem Code](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Systemeigenen Bindungen

Die Datenbindung wird verwendet, um eine Benutzeroberfläche mit der Datenquelle zu synchronisieren und vereinfacht, wie eine Xamarin.Forms-Anwendung zeigt an, und mit den Daten interagiert. Vorausgesetzt, dass das Quell Objekt die `INotifyPropertyChanged`-Schnittstelle implementiert, werden Änderungen im *Quell* Objekt vom Bindungs Framework automatisch an das *Ziel* Objekt übermittelt, und Änderungen im *Ziel* Objekt können optional an das *Quell* Objekt übermittelt werden.

Eigenschaften der systemeigenen Ansichten können auch die Datenbindung verwenden. Im folgenden Codebeispiel wird veranschaulicht, die Datenbindung mit den Eigenschaften des systemeigenen Ansichten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Die Seite enthält eine [`Entry`](xref:Xamarin.Forms.Entry) , deren [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft an die `NativeSwitchPageViewModel.IsSwitchOn`-Eigenschaft gebunden wird. Der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite wird auf eine neue Instanz der `NativeSwitchPageViewModel`-Klasse in der Code Behind-Datei festgelegt, wobei die ViewModel-Klasse die `INotifyPropertyChanged`-Schnittstelle implementiert.

Diese Seite enthält auch einen systemeigenen Switch für jede Plattform. Jeder Native Switch verwendet eine [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) Bindung, um den Wert der `NativeSwitchPageViewModel.IsSwitchOn`-Eigenschaft zu aktualisieren. Wenn der Switch deaktiviert ist, wird der `Entry` deaktiviert. wenn der Schalter aktiviert ist, wird der `Entry` aktiviert. Die folgenden Screenshots zeigen diese Funktion auf jeder Plattform:

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

Bidirektionale Bindungen werden automatisch unterstützt, vorausgesetzt, dass die systemeigene Eigenschaft `INotifyPropertyChanged`implementiert oder Key-Value-Beobachtungen (KVO) unter IOS unterstützt oder eine `DependencyProperty` auf UWP ist. Allerdings unterstützen viele native Sichten Benachrichtigung der Eigenschaftenänderung nicht. Für diese Ansichten können Sie einen [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) -Eigenschafts Wert als Teil des Bindungs Ausdrucks angeben. Diese Eigenschaft sollte auf den Namen eines Ereignisses in der einheitlichen Ansicht festgelegt werden, das signalisiert wird, wenn sich die Zieleigenschaft geändert hat. Wenn sich der Wert des systemeigenen Switchs ändert, wird der `Binding`-Klasse benachrichtigt, dass der Benutzer den Switch-Wert geändert hat, und der `NativeSwitchPageViewModel.IsSwitchOn`-Eigenschafts Wert wird aktualisiert.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Übergeben von Argumenten mit systemeigenen Ansichten

Konstruktorargumente können mithilfe des `x:Arguments`-Attributs mit einer `x:Static` Markup Erweiterung an Native Sichten übergeben werden. Außerdem können systemeigene View Factory-Methoden (`public static` Methoden, die Objekte oder Werte desselben Typs wie die Klasse oder Struktur zurückgeben, die die Methoden definiert) aufgerufen werden, indem der Name der Methode mithilfe des Attributs "`x:FactoryMethod`" und deren Argumente mithilfe des `x:Arguments` Attributs angegeben werden.

Im folgenden Codebeispiel wird veranschaulicht, beide Verfahren:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

Die [`UIFont.FromName`](xref:UIKit.UIFont.FromName*) Factory-Methode wird verwendet, um die [`UILabel.Font`](xref:UIKit.UILabel.Font) -Eigenschaft auf eine neue [`UIFont`](xref:UIKit.UIFont) unter IOS festzulegen. Der Name und die Größe der `UIFont` werden durch die Methodenargumente angegeben, die untergeordnete Elemente des `x:Arguments` Attributs sind.

Die [`Typeface.Create`](xref:Android.Graphics.Typeface.Create*) Factory-Methode wird verwendet, um die [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) -Eigenschaft auf eine neue [`Typeface`](xref:Android.Graphics.Typeface) unter Android festzulegen. Der Name und der Stil der `Typeface` Familie werden durch die Methodenargumente angegeben, die untergeordnete Elemente des `x:Arguments` Attributs sind.

Der [`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) -Konstruktor wird verwendet, um die [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) -Eigenschaft auf eine neue `FontFamily` auf dem universelle Windows-Plattform (UWP) festzulegen. Der `FontFamily` Name wird durch das Methoden Argument angegeben, das ein untergeordnetes Element des `x:Arguments` Attributs ist.

> [!NOTE]
> Argumente müssen die Typen, die erforderlich sind, von der Methode oder die Factorymethode übereinstimmen.

Die folgenden Screenshots zeigen das Ergebnis der Angabe der Factory Methoden- und konstruktorenaufrufe Argumente, um die Schriftart für andere native Ansichten festzulegen:

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

Weitere Informationen zum Übergeben von Argumenten in XAML finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Verweisen auf Native Ansichten aus Code

Obwohl es nicht möglich ist, eine systemeigene Ansicht mit dem `x:Name`-Attribut zu benennen, ist es möglich, eine in einer XAML-Datei deklarierte Native Ansichts Instanz aus der Code Behind-Datei in einem freigegebenen Zugriffs Projekt abzurufen, vorausgesetzt, die native Ansicht ist ein untergeordnetes Element eines [`ContentView`](xref:Xamarin.Forms.ContentView) , das einen `x:Name` Attribut Wert angibt. Klicken Sie dann, empfiehlt sich in Anweisungen für bedingte Kompilierung in der CodeBehind-Datei Folgendes:

1. Rufen Sie den [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) -Eigenschafts Wert ab, und wandeln Sie ihn in einen plattformspezifischen `NativeViewWrapper` Typ um.
1. Rufen Sie die `NativeViewWrapper.NativeElement`-Eigenschaft ab, und wandeln Sie Sie in den nativen Ansichtstyp

Die systemeigene API kann dann auf die systemeigene Ansicht für die gewünschten Vorgänge aufgerufen werden. Diese Vorgehensweise bietet auch den Vorteil, dass mehrere Native XAML-Ansichten für verschiedene Plattformen untergeordnete Elemente desselben [`ContentView`](xref:Xamarin.Forms.ContentView)sein können. Im folgenden Codebeispiel wird diese Technik veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

Im obigen Beispiel sind die nativen Ansichten für jede Plattform untergeordnete Elemente von [`ContentView`](xref:Xamarin.Forms.ContentView) -Steuerelementen, wobei der `x:Name`-Attribut Wert verwendet wird, um die `ContentView` im Code Behind abzurufen:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

Der Zugriff auf die [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) -Eigenschaft erfolgt, um die umschließende Native Sicht als plattformspezifische `NativeViewWrapper` Instanz abzurufen. Der Zugriff auf die `NativeViewWrapper.NativeElement`-Eigenschaft erfolgt dann, um die native Sicht als systemeigenen Typ abzurufen. Der systemeigene Ansicht-API wird dann aufgerufen, um die gewünschten Vorgänge auszuführen.

Die nativen IOS-und Android-Schaltflächen verwenden denselben `OnButtonTap` Ereignishandler, da jede systemeigene Schaltfläche einen `EventHandler` Delegaten als Reaktion auf ein Berührungs Ereignis verwendet. Allerdings verwendet die universelle Windows-Plattform (UWP) einen separaten `RoutedEventHandler`, der wiederum den `OnButtonTap` Ereignishandler in diesem Beispiel verwendet. Wenn auf eine systemeigene Schaltfläche geklickt wird, wird der `OnButtonTap` Ereignishandler ausgeführt, der das systemeigene Steuerelement skaliert und rotiert, das im [`ContentView`](xref:Xamarin.Forms.ContentView) mit dem Namen `contentViewTextParent`enthalten ist. Die folgenden Screenshots veranschaulichen dies zu auf jeder Plattform:

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Erstellen von Unterklassen für Native Ansichten

Viele IOS- und Android native-Ansichten eignen sich nicht in XAML instanziiert werden, da sie Methoden und Eigenschaften verwenden Sie zum Einrichten des Steuerelements. Die Lösung für dieses Problem besteht darin Unterklasse native Ansichten in Wrappern, die eine XAML-freundliche API definieren, Eigenschaften verwendet, um das Steuerelement einzurichten, und, plattformunabhängigen Ereignisse verwendet. Der umschlossenen nativen Ansichten und werden können in einem freigegebenen Asset Projekt (SAP) platziert und umgeben mit bedingten Kompilierungsdirektiven oder in plattformspezifischen Projekten platziert von XAML in eine .NET Standard Library-Projekt verwiesen.

Im folgenden Codebeispiel wird veranschaulicht, dass eine Xamarin.Forms-Startseite, die nutzt native Ansichten in Unterklassen unterteilt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Die Seite enthält eine [`Label`](xref:Xamarin.Forms.Label) , die die vom Benutzer gewählte Frucht von einem systemeigenen Steuerelement anzeigt. Der `Label` bindet an die `SubclassedNativeControlsPageViewModel.SelectedFruit`-Eigenschaft. Der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite wird auf eine neue Instanz der `SubclassedNativeControlsPageViewModel`-Klasse in der Code Behind-Datei festgelegt, wobei die ViewModel-Klasse die `INotifyPropertyChanged`-Schnittstelle implementiert.

Diese Seite enthält auch eine native Picker-Ansicht für jede Plattform. Jede native Ansicht zeigt die Auflistung von Früchten an, indem er die `ItemSource`-Eigenschaft an die `SubclassedNativeControlsPageViewModel.Fruits` Auflistung bindet. Dies ermöglicht dem Benutzer eine Frucht ist, wählen Sie, wie in den folgenden Screenshots gezeigt:

![](xaml-images/sub-classed.png "Subclassed Native Views")

Unter iOS und Android verwenden die nativen Sammlern Methoden zum Einrichten der Steuerelemente. Daher müssen diese Datumsauswahl in Unterklassen unterteilt werden zum Verfügbarmachen von Eigenschaften, um die XAML-freundliche zu. Beim universelle Windows-Plattform (UWP) ist die `ComboBox` bereits XAML-freundlich und erfordert daher keine Unterklassen.

### <a name="ios"></a>iOS

Die IOS-Implementierung Unterklassen der [`UIPickerView`](xref:UIKit.UIPickerView) Ansicht und macht Eigenschaften und ein Ereignis verfügbar, das problemlos von XAML verwendet werden kann:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

Die `MyUIPickerView`-Klasse macht `ItemsSource` und `SelectedItem` Eigenschaften sowie ein `SelectedItemChanged` Ereignis verfügbar. Ein [`UIPickerView`](xref:UIKit.UIPickerView) erfordert ein zugrunde liegendes [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) Datenmodell, auf das die `MyUIPickerView` Eigenschaften und das Ereignis zugreifen. Das `UIPickerViewModel`-Datenmodell wird von der `PickerModel`-Klasse bereitgestellt:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

Die `PickerModel`-Klasse stellt den zugrunde liegenden Speicher für die `MyUIPickerView`-Klasse über die-Eigenschaft `Items` bereit. Wenn sich das ausgewählte Element im `MyUIPickerView` ändert, wird die [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) -Methode ausgeführt, die den ausgewählten Index aktualisiert und das `ItemChanged` Ereignis auslöst. Dadurch wird sichergestellt, dass die `SelectedItem`-Eigenschaft immer das letzte Element zurückgibt, das vom Benutzer ausgewählt wurde. Außerdem überschreibt die `PickerModel`-Klasse Methoden, die zum Einrichten der `MyUIPickerView` Instanz verwendet werden.

### <a name="android"></a>Android

Die Android-Implementierung Unterklassen der [`Spinner`](xref:Android.Widget.Spinner) Ansicht und macht Eigenschaften und ein Ereignis verfügbar, das problemlos von XAML verwendet werden kann:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

Die `MySpinner`-Klasse macht `ItemsSource` und `SelectedObject` Eigenschaften sowie ein `ItemSelected` Ereignis verfügbar. Die Elemente, die von der `MySpinner`-Klasse angezeigt werden, werden vom [`Adapter`](xref:Android.Widget.Adapter) bereitgestellt, der der Sicht zugeordnet ist, und Elemente werden beim ersten Festlegen der `ItemsSource`-Eigenschaft in die `Adapter` aufgefüllt. Wenn sich das ausgewählte Element in der `MySpinner`-Klasse ändert, aktualisiert der `OnBindableSpinnerItemSelected`-Ereignishandler die `SelectedObject`-Eigenschaft.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie native Ansichten in Xamarin.Forms XAML-Dateien nutzen. Eigenschaften und Ereignishandler für native Ansichten festgelegt werden können, und sie können mit Xamarin.Forms-Ansichten interagieren.

## <a name="related-links"></a>Verwandte Links

- [Nativeswitch (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [Nativeviewinsidecontentview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [Subclassednativecontrols (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
