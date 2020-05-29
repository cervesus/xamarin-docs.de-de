---
title: ''
description: Native Ansichten von IOS, Android und der universelle Windows-Plattform können direkt von Xamarin.Forms XAML-Dateien referenziert werden. Eigenschaften und Ereignishandler können für Native Ansichten festgelegt werden, und Sie können mit Xamarin.Forms Sichten interagieren. In diesem Artikel wird veranschaulicht, wie Native Sichten von Xamarin.Forms XAML-Dateien genutzt werden.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 67994371d3042100503eb3a7c3bc7d117b1c590c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139580"
---
# <a name="native-views-in-xaml"></a>Native Ansichten in XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_Native Ansichten von IOS, Android und der universelle Windows-Plattform können direkt von Xamarin.Forms XAML-Dateien referenziert werden. Eigenschaften und Ereignishandler können für Native Ansichten festgelegt werden, und Sie können mit Xamarin.Forms Sichten interagieren. In diesem Artikel wird veranschaulicht, wie Native Sichten von Xamarin.Forms XAML-Dateien genutzt werden._

In diesem Artikel werden die folgenden Themen behandelt:

- Verwenden von [nativen Ansichten](#consuming) – der Prozess für die Verwendung einer systemeigenen Ansicht aus XAML.
- [Verwenden nativer Bindungen](#native_bindings) – Datenbindung an und von Eigenschaften nativer Sichten.
- [Übergeben von Argumenten an Native Sichten](#passing_arguments) – übergeben von Argumenten an Native Ansichts Konstruktoren und Aufrufen von systemeigenen View Factory-Methoden.
- [Verweisen auf native Sichten aus Code](#native_view_code) – Abrufen von in einer XAML-Datei deklarierten nativen Ansichts Instanzen aus der Code Behind-Datei.
- [Unterklassen](#subclassing) für systemeigene Sichten – Unterklassen für Native Sichten zum Definieren einer XAML-freundlichen API.  

<a name="overview" />

## <a name="overview"></a>Übersicht

So Betten Sie eine native Ansicht in eine Xamarin.Forms XAML-Datei ein:

1. Fügen Sie eine `xmlns` Namespace Deklaration in der XAML-Datei für den Namespace hinzu, der die native Ansicht enthält.
1. Erstellen Sie eine Instanz der systemeigenen Ansicht in der XAML-Datei.

> [!IMPORTANT]
> Das kompilierte XAML muss für alle XAML-Seiten deaktiviert werden, die native Sichten verwenden. Dies kann erreicht werden, indem Sie die Code-Behind-Klasse für die XAML-Seite mit dem- `[XamlCompilation(XamlCompilationOptions.Skip)]` Attribut versehen. Weitere Informationen zur XAML-Kompilierung finden Sie unter [XAML Xamarin.Forms -Kompilierung in ](~/xamarin-forms/xaml/xamlc.md).

Um auf eine native Ansicht aus einer Code Behind-Datei zu verweisen, müssen Sie ein frei gegebenes Asset-Projekt (SAP) verwenden und den plattformspezifischen Code mit bedingten Kompilierungs Direktiven einbinden. Weitere Informationen finden [Sie unter verweisen auf native Sichten aus Code](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Verwenden von nativen Ansichten

Im folgenden Codebeispiel wird die Verwendung von nativen Ansichten für jede Plattform zu einem veranschaulicht Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) :

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

Wenn Sie `clr-namespace` und `assembly` für einen nativen Ansichts Namespace angeben, `targetPlatform` muss auch ein angegeben werden. Dies sollte auf `iOS` , `Android` , `UWP` , `Windows` (entspricht `UWP` ), `macOS` ,, `GTK` `Tizen` oder `WPF` festgelegt werden. Zur Laufzeit ignoriert der XAML-Parser alle XML-Namespace Präfixe, die über einen verfügen `targetPlatform` , der nicht der Plattform entspricht, auf der die Anwendung ausgeführt wird.

Jede Namespace-Deklaration kann verwendet werden, um auf eine beliebige Klasse oder Struktur aus dem angegebenen Namespace zu verweisen. Beispielsweise kann die- `ios` Namespace Deklaration verwendet werden, um auf eine beliebige Klasse oder Struktur aus dem IOS- `UIKit` Namespace zu verweisen. Eigenschaften der systemeigenen Ansicht können mithilfe von XAML festgelegt werden, aber die Eigenschafts-und Objekttypen müssen mit identisch sein. Beispielsweise wird die `UILabel.TextColor` `UIColor.Red` -Eigenschaft mithilfe der `x:Static` -Markup Erweiterung und des- `ios` Namespace auf festgelegt.

Bindbare Eigenschaften und angefügte bindbare Eigenschaften können auch in nativen Ansichten mithilfe der-Syntax festgelegt werden `Class.BindableProperty="value"` . Jede native Ansicht ist in einer plattformspezifischen Instanz umschließt `NativeViewWrapper` , die von der-Klasse abgeleitet wird [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) . Wenn Sie eine bindbare Eigenschaft oder eine angefügte bindbare Eigenschaft für eine native Ansicht festlegen, wird der Eigenschafts Wert auf den Wrapper übertragen. Beispielsweise kann ein zentriertes horizontales Layout durch Festlegen von `View.HorizontalOptions="Center"` in der systemeigenen Ansicht angegeben werden.

> [!NOTE]
> Beachten Sie, dass Stile nicht mit nativen Ansichten verwendet werden können, da Stile nur auf Eigenschaften abzielen können, die von-Objekten unterstützt werden `BindableProperty` .

Android-widgekonstruktoren erfordern im Allgemeinen das Android `Context` -Objekt als Argument, das über eine statische Eigenschaft in der-Klasse verfügbar gemacht werden kann `MainActivity` . Wenn Sie ein Android-Widget in XAML erstellen, `Context` muss das Objekt daher im Allgemeinen an den Konstruktor des Widgets übergeben werden, indem das- `x:Arguments` Attribut mit einer `x:Static` Markup Erweiterung verwendet wird. Weitere Informationen finden Sie unter [übergeben von Argumenten an Native Ansichten](#passing_arguments).

> [!NOTE]
> Beachten Sie, dass das Benennen einer systemeigenen Ansicht mit weder `x:Name` in einem .NET Standard Bibliotheksprojekt noch in einem freigegebenen Ressourcen Projekt (SAP) möglich ist. Dadurch wird eine Variable des systemeigenen Typs generiert, was zu einem Kompilierungsfehler führt. Native Sichten können jedoch in Instanzen umschließt `ContentView` und in der Code Behind-Datei abgerufen werden, vorausgesetzt, dass ein SAP verwendet wird. Weitere Informationen finden Sie unter [verweisen auf eine native Ansicht aus dem Code](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Native Bindungen

Die Datenbindung wird verwendet, um eine Benutzeroberfläche mit der zugehörigen Datenquelle zu synchronisieren, und vereinfacht Xamarin.Forms die Anzeige und Interaktion einer Anwendung mit Ihren Daten. Vorausgesetzt, dass das Quell Objekt die- `INotifyPropertyChanged` Schnittstelle implementiert, werden Änderungen im *Quell* Objekt automatisch vom Bindungs Framework an das *Ziel* Objekt übermittelt, und Änderungen im *Ziel* Objekt können optional an das *Quell* Objekt übermittelt werden.

Eigenschaften von systemeigenen Sichten können auch die Datenbindung verwenden. Im folgenden Codebeispiel wird die Datenbindung mithilfe von Eigenschaften nativer Sichten veranschaulicht:

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

Die Seite enthält eine [`Entry`](xref:Xamarin.Forms.Entry) , deren- [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) Eigenschaft an die-Eigenschaft gebunden ist `NativeSwitchPageViewModel.IsSwitchOn` . Die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite wird auf eine neue Instanz der `NativeSwitchPageViewModel` -Klasse in der Code-Behind-Datei festgelegt, wobei die ViewModel-Klasse die- `INotifyPropertyChanged` Schnittstelle implementiert.

Die Seite enthält auch einen nativen Switch für jede Plattform. Jeder Native Switch verwendet eine- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) Bindung, um den Wert der-Eigenschaft zu aktualisieren `NativeSwitchPageViewModel.IsSwitchOn` . Wenn der Switch deaktiviert ist, `Entry` ist daher deaktiviert, und wenn der Schalter aktiviert ist, `Entry` wird aktiviert. Die folgenden Screenshots zeigen diese Funktionalität auf den einzelnen Plattformen:

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

Bidirektionale Bindungen werden automatisch unterstützt, vorausgesetzt, dass die systemeigene Eigenschaft implementiert `INotifyPropertyChanged` oder die Schlüssel-Wert-Beobachtung unter IOS unterstützt, oder ob ein `DependencyProperty` on UWP ist. Viele Native Sichten unterstützen jedoch keine Benachrichtigung über Eigenschafts Änderungen. Für diese Ansichten können Sie einen [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) Eigenschafts Wert als Teil des Bindungs Ausdrucks angeben. Diese Eigenschaft sollte auf den Namen eines Ereignisses in der nativen Ansicht festgelegt werden, das signalisiert, wenn sich die Ziel Eigenschaft geändert hat. Wenn sich der Wert des systemeigenen Switchs ändert, wird die- `Binding` Klasse benachrichtigt, dass der Benutzer den Switch-Wert geändert hat und der- `NativeSwitchPageViewModel.IsSwitchOn` Eigenschafts Wert aktualisiert wird.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Übergeben von Argumenten an Native Sichten

Konstruktorargumente können mithilfe des- `x:Arguments` Attributs mit einer Markup Erweiterung an Native Sichten übergeben werden `x:Static` . Außerdem können systemeigene View Factory-Methoden ( `public static` Methoden, die Objekte oder Werte desselben Typs wie die Klasse oder Struktur zurückgeben, die die Methoden definiert) aufgerufen werden, indem der Methodenname mit dem- `x:FactoryMethod` Attribut und den Argumenten mithilfe des- `x:Arguments` Attributs angegeben wird.

Im folgenden Codebeispiel werden beide Methoden veranschaulicht:

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

Die [`UIFont.FromName`](xref:UIKit.UIFont.FromName*) Factory-Methode wird verwendet, um die- [`UILabel.Font`](xref:UIKit.UILabel.Font) Eigenschaft auf eine neue [`UIFont`](xref:UIKit.UIFont) auf IOS festzulegen. Der `UIFont` Name und die Größe werden durch die Methodenargumente angegeben, die untergeordnete Elemente des `x:Arguments` Attributs sind.

Die [`Typeface.Create`](xref:Android.Graphics.Typeface.Create*) Factory-Methode wird verwendet, um die- [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) Eigenschaft auf einen neuen [`Typeface`](xref:Android.Graphics.Typeface) unter Android festzulegen. Der `Typeface` Familienname und-Stil werden durch die Methodenargumente angegeben, die untergeordnete Elemente des `x:Arguments` Attributs sind.

Der- [`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor wird verwendet, um die- [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) Eigenschaft auf einen neuen `FontFamily` auf dem universelle Windows-Plattform (UWP) festzulegen. Der `FontFamily` Name wird durch das Methoden Argument angegeben, das ein untergeordnetes Element des- `x:Arguments` Attributs ist.

> [!NOTE]
> Argumente müssen den Typen entsprechen, die für den Konstruktor oder die Factorymethode erforderlich sind.

Die folgenden Screenshots zeigen das Ergebnis der Angabe der Factorymethode und Konstruktorargumente zum Festlegen der Schriftart für verschiedene Native Sichten:

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

Weitere Informationen zum Übergeben von Argumenten in XAML finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Verweisen auf native Sichten aus Code

Obwohl es nicht möglich ist, eine native Sicht mit dem- `x:Name` Attribut zu benennen, ist es möglich, eine in einer XAML-Datei deklarierte Native Ansichts Instanz aus der Code Behind-Datei in einem freigegebenen Zugriffs Projekt abzurufen, vorausgesetzt, die native Ansicht ist ein untergeordnetes Element von, [`ContentView`](xref:Xamarin.Forms.ContentView) das einen `x:Name` Attribut Wert angibt. In den Direktiven für bedingte Kompilierung in der Code-Behind-Datei sollten Sie die folgenden Schritte ausführen:

1. Rufen [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) Sie den Eigenschafts Wert ab, und wandeln Sie ihn in einen plattformspezifischen `NativeViewWrapper` Typ um.
1. Rufen `NativeViewWrapper.NativeElement` Sie die-Eigenschaft ab, und wandeln Sie Sie in den nativen Ansichtstyp

Die Native API kann dann in der nativen Ansicht aufgerufen werden, um die gewünschten Vorgänge auszuführen. Diese Vorgehensweise bietet auch den Vorteil, dass mehrere systemeigene XAML-Ansichten für verschiedene Plattformen untergeordnete Elemente des gleichen sind [`ContentView`](xref:Xamarin.Forms.ContentView) . Dieses Verfahren wird im folgenden Codebeispiel veranschaulicht:

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

Im obigen Beispiel sind die nativen Ansichten für jede Plattform untergeordnete Elemente von Steuer [`ContentView`](xref:Xamarin.Forms.ContentView) Elementen, wobei der- `x:Name` Attribut Wert verwendet wird, um den `ContentView` im Code Behind abzurufen:

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

Der [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) Zugriff auf die-Eigenschaft erfolgt, um die umschließende Native Sicht als plattformspezifische `NativeViewWrapper` Instanz abzurufen. `NativeViewWrapper.NativeElement`Dann wird auf die-Eigenschaft zugegriffen, um die native Sicht als ihren systemeigenen Typ abzurufen. Die API der systemeigenen Ansicht wird dann aufgerufen, um die gewünschten Vorgänge auszuführen.

Die nativen IOS-und Android-Schaltflächen verwenden denselben `OnButtonTap` Ereignishandler, da jede systemeigene Schaltfläche einen Delegaten als `EventHandler` Reaktion auf ein Berührungs Ereignis verwendet. Allerdings verwendet die universelle Windows-Plattform (UWP) einen separaten `RoutedEventHandler` , der wiederum den `OnButtonTap` Ereignishandler in diesem Beispiel verwendet. Wenn auf eine systemeigene Schaltfläche geklickt wird, wird daher der `OnButtonTap` Ereignishandler ausgeführt, der das systemeigene Steuerelement skaliert und rotiert, das im benannten enthalten ist [`ContentView`](xref:Xamarin.Forms.ContentView) `contentViewTextParent` . Die folgenden Screenshots veranschaulichen dies auf den einzelnen Plattformen:

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Unterklassen für Native Sichten

Viele Native IOS-und Android-Ansichten können nicht in XAML instanziiert werden, da Sie anstelle von Eigenschaften Methoden verwenden, um das Steuerelement einzurichten. Die Lösung für dieses Problem besteht in der Unterklasse von systemeigenen Sichten in Wrapper, die eine XAML-freundliche API definieren, die Eigenschaften zum Einrichten des Steuer Elements verwendet und plattformunabhängige Ereignisse verwendet. Die umschließenden systemeigenen Sichten können dann in ein frei gegebenes Asset-Projekt (SAP) eingefügt und mit bedingten Kompilierungs Direktiven eingeschlossen oder in plattformspezifischen Projekten platziert werden, auf die in einem .NET Standard Bibliotheksprojekt in XAML verwiesen wird.

Im folgenden Codebeispiel wird eine Xamarin.Forms Seite veranschaulicht, die untergeordnete Native Sichten nutzt:

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

Die Seite enthält ein-Element [`Label`](xref:Xamarin.Forms.Label) , das die vom Benutzer gewählte Frucht von einem systemeigenen Steuerelement anzeigt. Der `Label` bindet an die- `SubclassedNativeControlsPageViewModel.SelectedFruit` Eigenschaft. Die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite wird auf eine neue Instanz der `SubclassedNativeControlsPageViewModel` -Klasse in der Code-Behind-Datei festgelegt, wobei die ViewModel-Klasse die- `INotifyPropertyChanged` Schnittstelle implementiert.

Die Seite enthält auch eine native Auswahl Ansicht für jede Plattform. Jede native Ansicht zeigt die Auflistung von Früchten an, indem die zugehörige- `ItemSource` Eigenschaft an die Auflistung gebunden wird `SubclassedNativeControlsPageViewModel.Fruits` . Dies ermöglicht es dem Benutzer, eine Frucht auszuwählen, wie in den folgenden Screenshots zu sehen:

![](xaml-images/sub-classed.png "Subclassed Native Views")

Unter IOS und Android verwenden die systemeigenen Picker Methoden zum Einrichten der Steuerelemente. Daher müssen diese Picker unter klassifiziert werden, um Eigenschaften verfügbar zu machen, damit Sie XAML-freundlich sind. Beim universelle Windows-Plattform (UWP) `ComboBox` ist das bereits XAML-freundlich und erfordert daher keine Unterklassen.

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

Die `MyUIPickerView` -Klasse macht `ItemsSource` -und- `SelectedItem` Eigenschaften sowie ein- `SelectedItemChanged` Ereignis verfügbar. Ein [`UIPickerView`](xref:UIKit.UIPickerView) benötigt ein zugrunde liegendes [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) Datenmodell, auf das die `MyUIPickerView` Eigenschaften und das Ereignis zugreifen. Das `UIPickerViewModel` Datenmodell wird von der- `PickerModel` Klasse bereitgestellt:

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

Die- `PickerModel` Klasse stellt den zugrunde liegenden Speicher für die- `MyUIPickerView` Klasse über die- `Items` Eigenschaft bereit. Jedes Mal, wenn das ausgewählte Element in `MyUIPickerView` geändert wird, wird die- [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) Methode ausgeführt, die den ausgewählten Index aktualisiert und das- `ItemChanged` Ereignis auslöst. Dadurch wird sichergestellt, dass die- `SelectedItem` Eigenschaft immer das letzte Element zurückgibt, das vom Benutzer ausgewählt wurde. Außerdem `PickerModel` überschreibt die-Klasse Methoden, die zum Einrichten der-Instanz verwendet werden `MyUIPickerView` .

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

Die `MySpinner` -Klasse macht `ItemsSource` -und- `SelectedObject` Eigenschaften sowie ein- `ItemSelected` Ereignis verfügbar. Die Elemente, die von der- `MySpinner` Klasse angezeigt werden, werden von der-Klasse bereitgestellt [`Adapter`](xref:Android.Widget.Adapter) , die der Ansicht zugeordnet ist, und Elemente werden `Adapter` beim ersten Festlegen der-Eigenschaft in den aufgefüllt `ItemsSource` . Wenn sich das ausgewählte Element in der- `MySpinner` Klasse ändert, `OnBindableSpinnerItemSelected` aktualisiert der Ereignishandler die- `SelectedObject` Eigenschaft.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Native Ansichten von Xamarin.Forms XAML-Dateien genutzt werden. Eigenschaften und Ereignishandler können für Native Ansichten festgelegt werden, und Sie können mit Xamarin.Forms Sichten interagieren.

## <a name="related-links"></a>Verwandte Links

- [Nativeswitch (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [Nativeviewinsidecontentview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [Subclassednativecontrols (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
