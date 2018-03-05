---
title: Systemeigene Ansichten in XAML
description: "Systemeigene Ansichten von iOS, Android und universellen Windows-Plattform können direkt von Xamarin.Forms XAML-Dateien verwiesen werden. Eigenschaften und Ereignishandler für systemeigene Sichten festgelegt werden können, und sie können mit Xamarin.Forms Sichten interagieren. Dieser Artikel veranschaulicht, wie systemeigene Ansichten Xamarin.Forms XAML-Dateien nutzen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: fc44b2a6080832d11c610661244172ad4a6a0716
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="native-views-in-xaml"></a>Systemeigene Ansichten in XAML

_Systemeigene Ansichten von iOS, Android und universellen Windows-Plattform können direkt von Xamarin.Forms XAML-Dateien verwiesen werden. Eigenschaften und Ereignishandler für systemeigene Sichten festgelegt werden können, und sie können mit Xamarin.Forms Sichten interagieren. Dieser Artikel veranschaulicht, wie systemeigene Ansichten Xamarin.Forms XAML-Dateien nutzen._

In diesem Artikel werden die folgenden Themen erläutert:

- [Verarbeiten von systemeigenen Ansichten](#consuming) – der Prozess für die Nutzung einer Ansicht mit systemeigenen aus XAML.
- [Verwenden von systemeigenen Bindungen](#native_bindings) – die Datenbindung zu und von Eigenschaften der systemeigenen Ansichten.
- [Übergeben von Argumenten mit systemeigenen Ansichten](#passing_arguments) : Übergeben von Argumenten an die Ansicht mit systemeigenen Konstruktoren und Aufrufen der Ansicht mit systemeigenen Factorymethoden.
- [Verweisen auf systemeigene Ansichten aus Code](#native_view_code) – Abrufen der Ansicht mit systemeigenen Instanzen in einer XAML-Datei, aus der CodeBehind-Datei deklariert.
- [Erstellen von Unterklassen für systemeigenen Ansichten](#subclassing) – Erstellen von Unterklassen für systemeigenen Ansichten, um eine XAML-kompatible API definieren.  

<a name="overview" />

## <a name="overview"></a>Übersicht

So betten Sie eine Ansicht mit systemeigenen in einer Xamarin.Forms-XAML-Datei ein:

1. Hinzufügen einer `xmlns` Namespacedeklaration in der XAML-Datei für den Namespace, die Ansicht mit systemeigene enthält.
1. Erstellen Sie eine Instanz der einheitlichen Ansicht in der XAML-Datei.

> [!NOTE]
> XAMLC muss für alle XAML-Seiten, die ausgeschaltet werden, die systemeigene Ansichten verwenden.

Um eine Ansicht mit systemeigenen aus einem Code-Behind-Datei verweisen, müssen Sie mithilfe einer freigegebenen Asset Projekt (SAP) und die plattformspezifischen Code mit bedingten Kompilierungsdirektiven zu umschließen. Weitere Informationen finden Sie unter [verweisen auf systemeigene Ansichten aus Code](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Verwenden von systemeigenen Ansichten

Im folgenden Codebeispiel wird veranschaulicht, nutzen systemeigene Ansichten für jede Plattform in einem Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static formsandroid:Forms.Context}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Sowie die Angabe der `clr-namespace` und `assembly` für eine Ansicht mit systemeigenen-Namespace eine `targetPlatform` muss auch angegeben werden. Dies sollte festgelegt werden, auf einen der Werte von der [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) -Enumeration, und wird in der Regel um festgelegt `iOS`, `Android`, oder `Windows`. Zur Laufzeit die XAML-Parser ignoriert alle XML-Namespacepräfixe, die über eine `targetPlatform` entspricht, die nicht die Plattform, auf dem die Anwendung ausgeführt wird.

Jeder Namespacedeklaration kann verwendet werden, auf eine beliebige Klasse oder Struktur aus dem angegebenen Namespace zu verweisen. Z. B. die `ios` Namespacedeklaration kann verwendet werden, um beliebige Klasse oder Struktur im IOS verweisen `UIKit` Namespace. Eigenschaften der Ansicht mit systemeigenen durch XAML festgelegt werden können, aber die Eigenschaft und die Objekt-Typen müssen übereinstimmen. Z. B. die `UILabel.TextColor` -Eigenschaftensatz auf `UIColor.Red` mithilfe der `x:Static` Markuperweiterung und `ios` Namespace.

Bindbare Eigenschaften und angefügte bindbare Eigenschaften können auch festgelegt werden auf systemeigenen Ansichten mithilfe der `Class.BindableProperty="value"` Syntax. Jede Ansicht mit systemeigenen wird in ein plattformspezifisches umschlossen `NativeViewWrapper` Instanz, abgeleitet von der [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Klasse. Festlegen von eine bindbare Eigenschaft oder angefügten bindbare Eigenschaft für eine Ansicht mit systemeigenen überträgt den Wert der Eigenschaft auf den Wrapper. Beispielsweise kann eine horizontale zentriert angegeben werden, durch Festlegen von `View.HorizontalOptions="Center"` auf die Ansicht mit systemeigenen.

> [!NOTE]
> Beachten Sie, dass Stile mit systemeigenen Ansichten verwendet werden können, da Stile nur Eigenschaften abzielen können, die gesichert werden, indem Sie `BindableProperty` Objekte.

Android Widget Konstruktoren erfordern in der Regel die Android `Context` Objekt als Argument aus, und dies kann über die `Xamarin.Forms.Platform.Android.Forms.Context` Objekt. Daher, beim Erstellen von Android-Widget in XAML, die `Context` Objekt muss im Allgemeinen um das Widget-Konstruktor übergeben werden mithilfe der `x:Arguments` -Attribut mit einer `x:Static` Markuperweiterung. Weitere Informationen finden Sie unter [übergeben von Argumenten mit systemeigenen Ansichten](#passing_arguments).

> [!NOTE]
> Beachten Sie, diese Benennen einer Ansicht mit systemeigenen mit `x:Name` ist nicht möglich, in eine Portable Klassenbibliothek (PCL)-Projekt oder eine freigegebene Asset-Projekt (SAP). Auf diese Weise eine Variable des systemeigenen Typs generiert einen Kompilierungsfehler. Allerdings können systemeigene Ansichten umschlossen werden, `ContentView` Instanzen und in die CodeBehind-Datei abgerufen werden, vorausgesetzt, dass eine SAP verwendet wird. Weitere Informationen finden Sie unter [verweisen auf eine Ansicht mit systemeigenen Code](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Systemeigene Bindungen

Die Datenbindung wird verwendet, um eine Benutzeroberfläche mit ihrer Datenquelle zu synchronisieren und vereinfacht werden, wie eine Xamarin.Forms-Anwendung zeigt an, und mit den Daten interagiert. Vorausgesetzt, dass das Quellobjekt implementiert die `INotifyPropertyChanged` Netzwerkschnittstelle, die Änderungen in der *Quelle* Objekt werden automatisch zum Übertragen der *Ziel* Objekt von der Bindung Framework und die Änderungen in der *Ziel* Objekt kann optional abgelegt werden, um die *Quelle* Objekt.

Eigenschaften der systemeigenen Ansichten können auch die Datenbindung verwenden. Im folgenden Codebeispiel wird veranschaulicht, die Datenbindung, die mit den Eigenschaften des systemeigenen Ansichten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
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
        <androidWidget:Switch x:Arguments="{x:Static formsAndroid:Forms.Context}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Diese Seite enthält eine [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) , deren [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) Eigenschaft bindet an die `NativeSwitchPageViewModel.IsSwitchOn` Eigenschaft. Die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) der Seite festlegen, um eine neue Instanz der dem `NativeSwitchPageViewModel` Klasse in der Code-Behind-Datei mit ViewModel implementierende Klasse die `INotifyPropertyChanged` Schnittstelle.

Diese Seite enthält auch einen systemeigenen Switch für jede Plattform. Jeder systemeigene Switch verwendet eine [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) Bindung für das update der `NativeSwitchPageViewModel.IsSwitchOn` Eigenschaft. Aus diesem Grund, wenn der Schalter deaktiviert ist, die `Entry` deaktiviert ist, und wenn der Schalter aktiviert, ist die `Entry` aktiviert ist. Die folgenden Screenshots zeigen diese Funktionalität auf jeder Plattform:

![](xaml-images/native-switch-disabled.png "Systemeigene deaktiviert")
![](xaml-images/native-switch-enabled.png "systemeigenen Switch aktiviert")

Bidirektionale Bindungen werden automatisch unterstützt, vorausgesetzt, dass die Eigenschaft "native" implementiert `INotifyPropertyChanged`, beobachten von Schlüssel-Wert (KVO) für iOS unterstützt, oder ist eine `DependencyProperty` für universelle Windows-Plattform. Viele systemeigene Ansichten unterstützen keine änderungsbenachrichtigung. Für diese Sichten können Sie angeben einer [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) Eigenschaftswert im Rahmen des Bindungsausdrucks. Diese Eigenschaft sollte auf den Namen eines Ereignisses in der Ansicht mit systemeigenen festgelegt werden, das signalisiert wird, wenn die Zieleigenschaft geändert wurde. Klicken Sie dann, wenn der Wert des Schalters native ändert, ändert der `Binding` Klasse benachrichtigt, dass der Benutzer die Switch-Wert geändert wurde und die `NativeSwitchPageViewModel.IsSwitchOn` Eigenschaftswert aktualisiert wird.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Übergeben von Argumenten mit systemeigenen Ansichten

Konstruktorargumente übergeben werden können, mit systemeigenen Ansichten, die mithilfe der `x:Arguments` -Attribut mit einer `x:Static` Markuperweiterung. Darüber hinaus Ansicht mit systemeigenen Factorymethoden (`public static` Methoden, mit denen Objekte oder Werte desselben Typs wie die Klasse oder Struktur, die die Methoden definiert) kann aufgerufen werden, indem Sie der Methode angeben mithilfe der `x:FactoryMethod` Attribut und seine Argumente Mithilfe der `x:Arguments` Attribut.

Im folgenden Codebeispiel wird veranschaulicht, beide Verfahren:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
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
        <androidWidget:TextView x:Arguments="{x:Static formsAndroid:Forms.Context}"
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

Die [ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Factorymethode dient zum Festlegen der [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) Eigenschaft, um ein neues [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) unter iOS. Die `UIFont` Name und Größe sind die Argumente der Methode, die untergeordneten Elemente sind gemäß der `x:Arguments` Attribut.

Die [ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Factorymethode dient zum Festlegen der [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) Eigenschaft, um ein neues [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) auf Android-Geräten. Die `Typeface` Familiennamen und des Stils sind die Argumente der Methode, die untergeordneten Elemente sind gemäß der `x:Arguments` Attribut.

Die [ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Konstruktor wird zum Festlegen der [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) Eigenschaft, um ein neues `FontFamily` auf die universelle Windows-Plattform (UWP). Die `FontFamily` vom das Methodenargument, das ein untergeordnetes Element der angegebene Name der `x:Arguments` Attribut.

> [!NOTE]
> **Hinweis**: Argumente müssen die Typen, die erforderlich sind, von der Methode oder die Factorymethode übereinstimmen.

Die folgenden Screenshots zeigen das Ergebnis der Angabe der Factory-Methode und Konstruktor Argumente, um die Schriftart für verschiedene systemeigenen Ansichten festlegen:

![](xaml-images/passing-arguments.png "Festlegen von Schriftarten für systemeigenen Ansichten")

Weitere Informationen zum Übergeben von Argumenten in XAML finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Verweisen auf systemeigene Ansichten von Code

Obwohl es nicht möglich, benennen Sie eine Ansicht mit systemeigene mit ist der `x:Name` -Attribut, ist es möglich, eine Ansicht mit systemeigenen-Instanz, die in einer XAML-Datei aus der CodeBehind-Datei in einem Projekt für freigegebenen Zugriff deklariert abzurufen, vorausgesetzt, dass die Ansicht mit systemeigene ein untergeordnetes Element von einem ist[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , die angibt, ein `x:Name` Attributwert. Klicken Sie dann sollten innerhalb von bedingten Kompilierungsdirektiven in der CodeBehind-Datei Folgendes beachten:

1. Abrufen der [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Eigenschaft-Wert, und wandeln Sie ihn in ein plattformspezifisches `NativeViewWrapper` Typ.
1. Abrufen der `NativeViewWrapper.NativeElement` Eigenschaft, und es in der Ansicht mit systemeigenen Typ umwandeln.

Die systemeigene API können Sie dann auf die Ansicht mit systemeigenen zum Ausführen der gewünschten Vorgänge aufgerufen werden. Dieser Ansatz bietet auch den Vorteil, dass mehrere systemeigene XAML-Ansichten für verschiedene Plattformen untergeordnete Elemente des identisch sein können [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Im folgenden Codebeispiel wird diese Technik veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static formsAndroid:Forms.Context}"
                Text="Text in a TextView" />
            <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static formsAndroid:Forms.Context}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>

```

Im Beispiel oben werden die systemeigenen Ansichten für jede Plattform untergeordnete Elemente des [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) -Steuerelemente, mit der `x:Name` Attributwert verwendet wird, zum Abrufen der `ContentView` in den Code-Behind:

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

Die [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Eigenschaft erfolgt zum Abrufen der umschlossenen einheitlichen Ansicht als eine plattformspezifische `NativeViewWrapper` Instanz. Die `NativeViewWrapper.NativeElement` Eigenschaft erfolgt dann um die Ansicht mit systemeigenen als systemeigenen Typ abzurufen. Die systemeigene Ansicht-API wird aufgerufen, um die gewünschten Vorgänge auszuführen.

IOS- und Android native Schaltfläche verwenden dieselbe `OnButtonTap` Ereignishandler, da jede systemeigene Schaltfläche beansprucht ein `EventHandler` in Reaktion auf ein Ereignis für die Toucheingabe zu delegieren. Allerdings verwendet die universelle Windows-Plattform (UWP) eine Separate `RoutedEventHandler`, die wiederum nutzt die `OnButtonTap` -Ereignishandler in diesem Beispiel. Wenn daher eine native Schaltfläche geklickt wird, die `OnButtonTap` -Ereignishandler ausgeführt wird, die skaliert und dreht das native Steuerelement innerhalb der [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) mit dem Namen `contentViewTextParent`. Den folgenden Screenshots veranschaulicht diese auf jeder Plattform auftritt:

![](xaml-images/contentview.png "ContentView mit einem systemeigenen-Steuerelement")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Erstellen von Unterklassen für systemeigenen Ansichten

Viele IOS- und Android native Sichten sind nicht geeignet für in XAML instanziiert werden, da sie statt Eigenschaften, Methoden zum Einrichten des Steuerelements verwenden. Die Lösung für dieses Problem besteht darin Unterklasse systemeigenen Ansichten in Wrappern, die eine weitere Verwendung von XAML-benutzerfreundliche API, die Eigenschaften verwendet, um setup für das Steuerelement, und, plattformunabhängige Ereignisse verwendet, definieren. Die umschlossenen systemeigenen Ansichten können dann werden in einer freigegebenen Asset Projekt (SAP) platziert und umgeben mit bedingten Kompilierungsdirektiven, oder in plattformspezifischen Projekte und aus XAML in einem Projekt der portablen Klassenbibliothek (PCL) verwiesen.

Im folgenden Codebeispiel wird veranschaulicht, dass eine Xamarin.Forms-Seite, die verbraucht systemeigene Ansichten als Unterklasse:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
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
        <androidLocal:MySpinner x:Arguments="{x:Static formsAndroid:Forms.Context}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Diese Seite enthält eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , die aus einem systemeigenen Steuerelement vom Benutzer ausgewählten Früchte anzeigt. Die `Label` bindet an die `SubclassedNativeControlsPageViewModel.SelectedFruit` Eigenschaft. Die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) der Seite festlegen, um eine neue Instanz der dem `SubclassedNativeControlsPageViewModel` Klasse in der Code-Behind-Datei mit ViewModel implementierende Klasse die `INotifyPropertyChanged` Schnittstelle.

Diese Seite enthält auch eine Auswahl einer einheitlichen Ansicht für jede Plattform. Jede Ansicht mit systemeigenen zeigt die Auflistung der Früchte durch Binden der `ItemSource` Eigenschaft, um die `SubclassedNativeControlsPageViewModel.Fruits` Auflistung. Dies ermöglicht es dem Benutzer eine Obst auswählen, wie in den folgenden Screenshots dargestellt:

![](xaml-images/sub-classed.png "Ein untergeordnetes systemeigenen Ansichten")

Auf IOS- und Android verwenden die systemeigenen Bildlaufbereich Methoden So richten Sie die Steuerelemente an. Diese Bildlaufbereich müssen daher als Unterklasse werden, um Eigenschaften, um diese XAML-kompatible Stellen verfügbar zu machen. Auf die universelle Windows-Plattform (UWP), die `ComboBox` bereits XAML-kompatible und nicht, sodass Unterklassen benötigt.

### <a name="ios"></a>iOS

Die iOS-Implementierung Unterklassen der [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) anzeigen, und macht Eigenschaften und ein Ereignis, das einfach aus XAML genutzt werden kann:

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

Die `MyUIPickerView` -Klasse verfügbar macht `ItemsSource` und `SelectedItem` Eigenschaften, und ein `SelectedItemChanged` Ereignis. Ein [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) erfordert eine zugrunde liegende [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) Datenmodell, das durch erfolgt die `MyUIPickerView` Eigenschaften und jedes öffentliche Ereignis. Die `UIPickerViewModel` Datenmodell wird bereitgestellt, indem Sie die `PickerModel` Klasse:

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

Die `PickerModel` -Klasse stellt den zugrunde liegenden Speicher für die `MyUIPickerView` -Klasse, über die `Items` Eigenschaft. Wenn das ausgewählte Element in der `MyUIPickerView` Änderungen, die [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) Methode ausgeführt wird, welche updates den ausgewählten Index und löst die `ItemChanged` Ereignis. Dadurch wird sichergestellt, dass die `SelectedItem` Eigenschaft gibt immer das letzte Element, das vom Benutzer ausgewählt zurück. Darüber hinaus die `PickerModel` Klasse überschreibt die Methoden, die mit Setup verwendet die `MyUIPickerView` Instanz.

### <a name="android"></a>Android

Die Android-Implementierung Unterklassen der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) anzeigen, und macht Eigenschaften und ein Ereignis, das einfach aus XAML genutzt werden kann:

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

Die `MySpinner` -Klasse verfügbar macht `ItemsSource` und `SelectedObject` Eigenschaften, und ein `ItemSelected` Ereignis. Angezeigtes durch die `MySpinner` Klasse bereitgestellt der [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) der Ansicht zugeordnet ist, und Elemente werden aufgefüllt, in der `Adapter` bei der `ItemsSource` -Eigenschaft festgelegt ist. Wenn das ausgewählte Element in der `MySpinner` Klasse geändert wird, die `OnBindableSpinnerItemSelected` Ereignishandler aktualisiert die `SelectedObject` Eigenschaft.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie systemeigene Ansichten Xamarin.Forms XAML-Dateien nutzen. Eigenschaften und Ereignishandler für systemeigene Sichten festgelegt werden können, und sie können mit Xamarin.Forms Sichten interagieren.


## <a name="related-links"></a>Verwandte Links

- [NativeSwitch (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
