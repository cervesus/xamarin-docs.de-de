---
title: Native Ansichten in XAML
description: Native Ansichten in iOS, Android und die universelle Windows-Plattform können direkt von Xamarin.Forms-XAML-Dateien verwiesen werden. Eigenschaften und Ereignishandler für native Ansichten festgelegt werden können, und sie können mit Xamarin.Forms-Ansichten interagieren. In diesem Artikel wird veranschaulicht, wie native Ansichten in Xamarin.Forms XAML-Dateien genutzt wird.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 7a5c09bfe46b9e775383889e07fd93094ba9bf68
ms.sourcegitcommit: a9c60f50b40203dd784e3e790b0d83e2bfc86129
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "65731523"
---
# <a name="native-views-in-xaml"></a>Native Ansichten in XAML

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)

_Native Ansichten in iOS, Android und die universelle Windows-Plattform können direkt von Xamarin.Forms-XAML-Dateien verwiesen werden. Eigenschaften und Ereignishandler für native Ansichten festgelegt werden können, und sie können mit Xamarin.Forms-Ansichten interagieren. In diesem Artikel wird veranschaulicht, wie native Ansichten in Xamarin.Forms XAML-Dateien genutzt wird._

In diesem Artikel werden die folgenden Themen behandelt:

- [Nutzen native Ansichten](#consuming) – der Prozess für die Nutzung von einer einheitlichen Ansicht aus XAML.
- [Verwenden von systemeigenen Bindungen](#native_bindings) : Datenbindung in und aus den Eigenschaften der systemeigenen Ansichten.
- [Übergeben von Argumenten an native Ansichten](#passing_arguments) : Übergeben von Argumenten an die Konstruktoren für native anzeigen und Aufrufen von nativer Sichten Factorymethoden.
- [Verweisen auf native Ansichten aus Code](#native_view_code) – Abrufen von systemeigenen Instanzen in einer XAML-Datei aus der CodeBehind-Datei deklariert.
- [Erstellen von Unterklassen für native Ansichten](#subclassing) – Erstellen von Unterklassen für native Ansichten zum Definieren einer XAML-freundliche-API.  

<a name="overview" />

## <a name="overview"></a>Übersicht

So betten Sie einen einheitlichen Einblick in einer Xamarin.Forms-XAML-Datei ein:

1. Hinzufügen einer `xmlns` Namespacedeklaration in der XAML-Datei für den Namespace, die einheitliche Ansicht enthält.
1. Erstellen Sie eine Instanz der einheitlichen Ansicht, in der XAML-Datei.

> [!IMPORTANT]
> Kompilierte XAML muss für alle XAML-Seiten deaktiviert werden, die native Ansichten verwenden. Dies kann erreicht werden, werden, indem die CodeBehind-Klasse für die XAML-Seite mit den `[XamlCompilation(XamlCompilationOptions.Skip)]` Attribut. Weitere Informationen zu XAML-Kompilierung, finden Sie unter [XAML-Kompilierung in Xamarin.Forms](~/xamarin-forms/xaml/xamlc.md).

Um auf eine einheitliche Ansicht aus einer CodeBehind-Datei verweisen, verwenden Sie eine freigegebene Asset Projekt (SAP) und umschließen Sie den plattformspezifischen Code mit bedingten Kompilierungsdirektiven. Weitere Informationen finden Sie unter [verweisen auf Native Ansichten aus Code](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Nutzen Native Ansichten

Im folgenden Codebeispiel wird veranschaulicht, nutzen native Ansichten für jede Plattform eine xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage):

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

Sowie das Festlegen der `clr-namespace` und `assembly` für einen Namespace einheitlichen Ansicht einer `targetPlatform` muss auch angegeben werden. Sollte auf einen der Werte festgelegt werden die [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) -Enumeration und wird in der Regel festgelegt werden, um `iOS`, `Android`, oder `Windows`. Zur Laufzeit die XAML-Parser ignoriert alle XML-Namespacepräfixe, die eine `targetPlatform` entspricht, die nicht die Plattform, auf dem die Anwendung ausgeführt wird.

Jede Namespacedeklaration kann verwendet werden, um auf einer beliebigen Klasse oder Struktur aus dem angegebenen Namespace zu verweisen. Z. B. die `ios` Namespace-Deklaration kann verwendet werden, auf einer beliebigen Klasse oder Struktur aus dem iOS `UIKit` Namespace. Eigenschaften der systemeigenen Ansicht können über XAML festgelegt werden, aber die Eigenschaft und die Objekt-Typen müssen übereinstimmen. Z. B. die `UILabel.TextColor` -Eigenschaftensatz auf `UIColor.Red` mithilfe der `x:Static` Markuperweiterung und `ios` Namespace.

Bindbare Eigenschaften und angefügte bindbare Eigenschaften können auch festgelegt werden auf native Ansichten unter Verwendung der `Class.BindableProperty="value"` Syntax. Jede systemeigene Ansicht ist eine plattformspezifische umschlossen `NativeViewWrapper` -Instanz, die von abgeleitet ist die [ `Xamarin.Forms.View` ](xref:Xamarin.Forms.View) Klasse. Überträgt die Einstellung eine bindbare Eigenschaft oder die angefügten bindbare Eigenschaft in einer einheitlichen Ansicht den Wert der Eigenschaft an den Wrapper. Beispielsweise kann ein zentriertes horizontales Layout angegeben werden, durch Festlegen von `View.HorizontalOptions="Center"` in der einheitlichen Ansicht.

> [!NOTE]
> Beachten Sie, dass die Stile mit nativer Sichten verwendet werden können, da Stile nur Eigenschaften als Ziel verwenden können, die durch gesichert werden `BindableProperty` Objekte.

Android-Widget-Konstruktoren benötigen im Allgemeinen die Android `Context` Objekt wie ein Argument, und diese über eine statische Eigenschaft in can available Made be der `MainActivity` Klasse. Aus diesem Grund, die beim Erstellen von einem Android-Widget in XAML, die `Context` Objekt muss in der Regel für das Widget Konstruktor übergeben werden mithilfe der `x:Arguments` -Attribut mit einer `x:Static` Markuperweiterung. Weitere Informationen finden Sie unter [Argumente übergeben, um Native Ansichten](#passing_arguments).

> [!NOTE]
> Beachten Sie diese Benennung einer einheitlichen Ansicht mit `x:Name` ist nicht möglich, in einer .NET Standard Library-Projekt oder eine freigegebene Asset-Projekt (SAP). Auf diese Weise wird eine Variable des systemeigenen Typs generiert, der einen Kompilierungsfehler verursacht. Native Ansichten können jedoch in eingebunden werden `ContentView` Instanzen und in der CodeBehind-Datei abgerufen werden, vorausgesetzt, dass eine SAP verwendet wird. Weitere Informationen finden Sie unter [verweisen auf einer einheitlichen Ansicht aus Code](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Systemeigenen Bindungen

Die Datenbindung wird verwendet, um eine Benutzeroberfläche mit der Datenquelle zu synchronisieren und vereinfacht, wie eine Xamarin.Forms-Anwendung zeigt an, und mit den Daten interagiert. Vorausgesetzt, dass das Quellobjekt implementiert die `INotifyPropertyChanged` Schnittstelle, die Änderungen in der *Quelle* Objekt werden automatisch per Push an die *Ziel* Objekt durch das Framework der modellbindung und Änderungen in der *Ziel* Objekt optional verschoben, um die *Quelle* Objekt.

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

Die Seite enthält eine [ `Entry` ](xref:Xamarin.Forms.Entry) , deren [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) Eigenschaft bindet an die `NativeSwitchPageViewModel.IsSwitchOn` Eigenschaft. Die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite auf eine neue Instanz der festgelegt ist die `NativeSwitchPageViewModel` Klasse in der CodeBehind-Datei mit der ViewModel-Klasse implementiert die `INotifyPropertyChanged` Schnittstelle.

Diese Seite enthält auch einen systemeigenen Switch für jede Plattform. Jede native Switch verwendet ein [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay) Bindung an das update der `NativeSwitchPageViewModel.IsSwitchOn` Eigenschaft. Aus diesem Grund werden, wenn der Schalter deaktiviert ist, die `Entry` deaktiviert ist, und wenn der Schalter aktiviert, ist die `Entry` aktiviert ist. Die folgenden Screenshots zeigen diese Funktion auf jeder Plattform:

![](xaml-images/native-switch-disabled.png "Systemeigene deaktiviert")
![](xaml-images/native-switch-enabled.png "systemeigenen Switch aktiviert")

Bidirektionale Bindungen werden automatisch unterstützt, vorausgesetzt, dass die Eigenschaft "native" implementiert `INotifyPropertyChanged`, beobachten von Schlüssel-Wert (KVO) unter iOS unterstützt, oder ist eine `DependencyProperty` in UWP. Allerdings unterstützen viele native Sichten Benachrichtigung der Eigenschaftenänderung nicht. Für diesen Ansichten können Sie angeben einer [ `UpdateSourceEventName` ](xref:Xamarin.Forms.Binding.UpdateSourceEventName) Eigenschaftswert als Teil des Bindungsausdrucks. Diese Eigenschaft sollte auf den Namen eines Ereignisses in der einheitlichen Ansicht festgelegt werden, das signalisiert wird, wenn sich die Zieleigenschaft geändert hat. Klicken Sie dann, wenn der Wert des Schalters native geändert wird, die `Binding` Klasse mitgeteilt, dass der Benutzer die Switch-Wert geändert hat und die `NativeSwitchPageViewModel.IsSwitchOn` Eigenschaftswert aktualisiert wird.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Übergeben von Argumenten mit systemeigenen Ansichten

Konstruktorargumente übergeben werden können, um native Ansichten, die mit der `x:Arguments` -Attribut mit einem `x:Static` Markuperweiterung. Außerdem native Ansicht Factorymethoden (`public static` Methoden, Objekte oder Werte desselben Typs wie die Klasse oder Struktur, die die Methoden definiert) kann durch Angabe der Methode aufgerufen werden, mithilfe der `x:FactoryMethod` -Attribut und dessen Argumente Mithilfe der `x:Arguments` Attribut.

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

Die [ `UIFont.FromName` ](xref:UIKit.UIFont.FromName*) Factorymethode dient zum Festlegen der [ `UILabel.Font` ](xref:UIKit.UILabel.Font) Eigenschaft, um ein neues [ `UIFont` ](xref:UIKit.UIFont) unter iOS. Die `UIFont` werden Name und Größe angegeben, indem das Argument der Methode, die untergeordnete Elemente von der `x:Arguments` Attribut.

Die [ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Factorymethode dient zum Festlegen der [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) Eigenschaft, um ein neues [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) unter Android. Die `Typeface` Familiennamen und den Stil werden angegeben, indem das Argument der Methode, die untergeordnete Elemente von der `x:Arguments` Attribut.

Die [ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor dient zum Festlegen der [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) Eigenschaft, um ein neues `FontFamily` auf die universelle Windows-Plattform (UWP). Die `FontFamily` vom Methodenarguments, das ein untergeordnetes Element des angegebene Name ist der `x:Arguments` Attribut.

> [!NOTE]
> Argumente müssen die Typen, die erforderlich sind, von der Methode oder die Factorymethode übereinstimmen.

Die folgenden Screenshots zeigen das Ergebnis der Angabe der Factory Methoden- und konstruktorenaufrufe Argumente, um die Schriftart für andere native Ansichten festzulegen:

![](xaml-images/passing-arguments.png "Festlegen von Schriftarten für Native Ansichten")

Weitere Informationen zum Übergeben von Argumenten in XAML finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Verweisen auf Native Ansichten aus Code

Obwohl es nicht möglich, den Namen einer einheitlichen Ansicht mit ist der `x:Name` -Attribut, es ist möglich, eine native anzeigen-Instanz, die in einer XAML-Datei aus der CodeBehind-Datei in einem Projekt für freigegebenen Zugriff deklariert abzurufen, vorausgesetzt, dass die systemeigene Ansicht ein untergeordnetes Element von einem ist[ `ContentView` ](xref:Xamarin.Forms.ContentView) gibt, die eine `x:Name` Attributwert. Klicken Sie dann, empfiehlt sich in Anweisungen für bedingte Kompilierung in der CodeBehind-Datei Folgendes:

1. Abrufen der [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) Eigenschaft Wert ein, und wandeln sie in einem plattformspezifischen `NativeViewWrapper` Typ.
1. Abrufen der `NativeViewWrapper.NativeElement` Eigenschaft und wandeln Sie sie in den Ansichtstyp der einheitlichen.

Die systemeigene API kann dann auf die systemeigene Ansicht für die gewünschten Vorgänge aufgerufen werden. Dieser Ansatz bietet dem Vorteil, dass auch, dass mehrere native XAML-Ansichten für verschiedene Plattformen untergeordnete Elemente des gleichen sein können [ `ContentView` ](xref:Xamarin.Forms.ContentView). Im folgenden Codebeispiel wird diese Technik veranschaulicht:

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

Im obigen Beispiel sind die nativen Ansichten für jede Plattform untergeordnete Elemente des [ `ContentView` ](xref:Xamarin.Forms.ContentView) -Steuerelemente, mit der `x:Name` Attributwert verwendet wird, zum Abrufen der `ContentView` im Code-Behind:

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

Die [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) -Eigenschaft zugegriffen wird, zum Abrufen der umschlossenen einheitlichen Ansicht als eine plattformspezifische `NativeViewWrapper` Instanz. Die `NativeViewWrapper.NativeElement` -Eigenschaft klicken Sie dann zum Abrufen der einheitlichen Ansicht als den systemeigenen Typ zugegriffen wird. Der systemeigene Ansicht-API wird dann aufgerufen, um die gewünschten Vorgänge auszuführen.

IOS- und Android native-Schaltflächen dieselbe `OnButtonTap` Ereignishandler, da jede Schaltfläche "systemeigene" verarbeitet ein `EventHandler` als Reaktion auf ein touchereignis delegieren. Die universelle Windows-Plattform (UWP), verwendet jedoch eine Separate `RoutedEventHandler`, die wiederum nutzt die `OnButtonTap` -Ereignishandler in diesem Beispiel. Wenn daher eine native Schaltfläche geklickt wird, die `OnButtonTap` -Ereignishandler ausgeführt wird, die skaliert und dreht das native Steuerelement innerhalb der [ `ContentView` ](xref:Xamarin.Forms.ContentView) mit dem Namen `contentViewTextParent`. Die folgenden Screenshots veranschaulichen dies zu auf jeder Plattform:

![](xaml-images/contentview.png "ContentView, die ein natives Steuerelement enthält.")

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

Die Seite enthält eine [ `Label` ](xref:Xamarin.Forms.Label) , die vom Benutzer über ein natives Steuerelement ausgewählten Obst anzeigt. Die `Label` bindet an die `SubclassedNativeControlsPageViewModel.SelectedFruit` Eigenschaft. Die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite auf eine neue Instanz der festgelegt ist die `SubclassedNativeControlsPageViewModel` Klasse in der CodeBehind-Datei mit der ViewModel-Klasse implementiert die `INotifyPropertyChanged` Schnittstelle.

Diese Seite enthält auch eine native Picker-Ansicht für jede Plattform. Jede systemeigene Ansicht zeigt die Auflistung der Früchte durch die Bindung der `ItemSource` Eigenschaft, um die `SubclassedNativeControlsPageViewModel.Fruits` Auflistung. Dies ermöglicht dem Benutzer eine Frucht ist, wählen Sie, wie in den folgenden Screenshots gezeigt:

![](xaml-images/sub-classed.png "Unterklassen Native Ansichten")

Unter iOS und Android verwenden die nativen Sammlern Methoden zum Einrichten der Steuerelemente. Daher müssen diese Datumsauswahl in Unterklassen unterteilt werden zum Verfügbarmachen von Eigenschaften, um die XAML-freundliche zu. Auf der universellen Windows-Plattform (UWP), die `ComboBox` bereits XAML-freundliche, und daher keine Unterklassen erforderlich.

### <a name="ios"></a>iOS

Die iOS-Implementierung Unterklassen der [ `UIPickerView` ](xref:UIKit.UIPickerView) anzeigen, und macht Eigenschaften und ein Ereignis, das problemlos von XAML genutzt werden kann:

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

Die `MyUIPickerView` -Klasse macht `ItemsSource` und `SelectedItem` Eigenschaften, und ein `SelectedItemChanged` Ereignis. Ein [ `UIPickerView` ](xref:UIKit.UIPickerView) erfordert eine zugrunde liegende [ `UIPickerViewModel` ](xref:UIKit.UIPickerViewModel) Datenmodell, das von zugegriffen werden kann die `MyUIPickerView` Eigenschaften und -Ereignis. Die `UIPickerViewModel` Datenmodell erfolgt über die `PickerModel` Klasse:

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

Die `PickerModel` Klasse stellt den zugrunde liegenden Speicher für die `MyUIPickerView` -Klasse, über die `Items` Eigenschaft. Wenn das ausgewählte Element in der `MyUIPickerView` Änderungen, die [ `Selected` ](xref:UIKit.UIPickerViewModel.Selected*) Methode ausgeführt wird, welche updates den ausgewählten Index und löst die `ItemChanged` Ereignis. Dadurch wird sichergestellt, dass die `SelectedItem` Eigenschaft gibt immer das letzte Element, das vom Benutzer ausgewählt. Darüber hinaus die `PickerModel` Klasse überschreibt die Methoden, die verwendet werden, mit der Einrichtung der `MyUIPickerView` Instanz.

### <a name="android"></a>Android

Die Android-Implementierung Unterklassen der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) anzeigen, und macht Eigenschaften und ein Ereignis, das problemlos von XAML genutzt werden kann:

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

Die `MySpinner` -Klasse macht `ItemsSource` und `SelectedObject` Eigenschaften, und ein `ItemSelected` Ereignis. Die Elemente, die angezeigt wird der `MySpinner` Klasse von bereitgestellt werden die [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) der Ansicht zugeordnet, und die Elemente werden aufgefüllt, in der `Adapter` bei der `ItemsSource` -Eigenschaft festgelegt ist. Wenn das ausgewählte Element in der `MySpinner` Klasse geändert wird, die `OnBindableSpinnerItemSelected` Ereignishandler aktualisiert die `SelectedObject` Eigenschaft.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie native Ansichten in Xamarin.Forms XAML-Dateien nutzen. Eigenschaften und Ereignishandler für native Ansichten festgelegt werden können, und sie können mit Xamarin.Forms-Ansichten interagieren.


## <a name="related-links"></a>Verwandte Links

- [NativeSwitch (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
