---
title: Geräteklasse
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 1fc3fb17ec97ce9028abbf63cdedbfc5fec12204
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="device-class"></a>Geräteklasse

Die [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Klasse enthält eine Reihe von Eigenschaften und Methoden können Entwickler, die Layout und Funktionen auf einem plattformbezogen anzupassen.

Zusätzlich zu den Methoden und Eigenschaften für den Zielcode auf bestimmte Hardwaretypen und Größen verfügbar die `Device` Klasse enthält die [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) Methode, die verwendet werden soll, wenn die Interaktion mit UI-Steuerelemente Hintergrundthreads ausgeführt.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Clientplattform-spezifische Werte bereitstellen

Vor dem Xamarin.Forms 2.3.4, die Plattform, die die Anwendung ausgeführt wurde abgerufen werden konnte, durch Untersuchen der [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) Eigenschaft und durch vergleichen, die [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), und [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) Enumerationswerte. Auf ähnliche Weise eine von der [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) Überladungen können verwendet werden, um die Clientplattform-spezifische Werte für ein Steuerelement bereitzustellen.

Allerdings haben seit Xamarin.Forms 2.3.4 diese APIs veraltet ersetzt und wurden. Die [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) -Klasse beinhaltet nun öffentliche Zeichenfolgenkonstanten, die Plattformen – identifizieren [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/) (veraltet) [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/) (veraltet) [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), und [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/). Auf ähnliche Weise die [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) Überladungen wurden durch ersetzt die [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) und [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) APIs.

In c# können Clientplattform-spezifische Werte bereitgestellt werden durch das Erstellen einer `switch` -Anweisung für die [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) -Eigenschaft, und klicken Sie dann bereitgestellt, `case` -Anweisungen für die erforderlichen Plattformen:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

Die [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) und [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) Klassen bieten die gleiche Funktionalität in XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

Die [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Klasse ist eine generische Klasse und müssen deshalb instanziiert werden, mit einer `x:TypeArguments` Attribut, das den Zieltyp entspricht. In der [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) -Klasse, die [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) Attribut akzeptiert ein einzelnes `string` Wert oder mehrere durch Kommas getrennte `string` Werte.

> [!IMPORTANT]
> Bereitstellen eines falschen `Platform` -Attributwert in die `On` Klasse führt nicht zu einem Fehler. Stattdessen führt der Code ohne die plattformspezifischen Wert angewendet wird.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

Die `Device.Idiom` können verwendet werden, um Layouts oder Funktionen hängen vom Gerät die Anwendung ausgeführt wird. Die [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) Enumeration enthält die folgenden Werte:

-  **Phone** – iPhone, iPod Touch und Android-Geräte enger gefasst als 600 Dips ^
-  **Tablet** – iPad, Windows-Geräten und Android-Geräte breiter als 600 Dips ^
-  **Desktop** – nur im zurückgegebenen [uwp-apps](~/xamarin-forms/platform/windows/installation/index.md) auf Windows 10-Desktopcomputern (gibt `Phone` auf mobilen Windows-Geräten, die in bestimmten Szenarien einschließlich)
-  **TV** – Tizen TV-Geräte
-  **Nicht unterstützte** : nicht verwendeter

*^ Dips ist nicht notwendigerweise die Anzahl der physischen Pixel*

`Idiom` ist besonders nützlich zum Erstellen von Layouts, mit die nutzen größere Bildschirme, wie folgt:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

## <a name="deviceflowdirection"></a>Device.FlowDirection

Die [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) Wert Ruft eine [ `FlowDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlowDirection/) -Enumerationswert ab, der der aktuellen flussrichtung vom Gerät verwendet wird. Flussrichtung ist die Richtung, in der die Elemente der Benutzeroberfläche auf der Seite vom Auge gescannt werden. Diese Enumerationswerte lauten:

- [`LeftToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.LeftToRight/)
- [`RightToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.RightToLeft/)
- [`MatchParent`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.MatchParent/)

In XAML wird die [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) Wert kann abgerufen werden, indem die `x:Static` Markuperweiterung:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Der entsprechende Code in C# geschrieben ist:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Weitere Informationen zu flussrichtung, finden Sie unter [rechts-nach-links-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

Die [ `Styles` Eigenschaft](~/xamarin-forms/user-interface/styles/index.md) enthält Definitionen von integrierten Formatvorlage, die auf einigen Steuerelements angewendet werden können (z. B. `Label`) `Style` Eigenschaft. Die verfügbaren Formate sind:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` kann verwendet werden, der zum Einstellen [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) in C#-Code:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

Die `OpenUri` Methode kann verwendet werden, um Vorgänge auf die zugrunde liegende Plattform, z. B. Öffnen Sie eine URL in die systemeigene Webbrowser auslösen (**Safari** auf iOS oder **Internet** auf Android-Geräten).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

Die [WebView-Beispiel](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) enthält ein Beispiel zur Verwendung `OpenUri` URLs öffnen und Anrufe auch ausgelöst werden.

Die [Maps Sample](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) verwendet auch `Device.OpenUri` zum Anzeigen von Zuordnungen und erfahren Sie, wie mithilfe der systemeigenen **ordnet** apps auf IOS- und Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

Die `Device` -Klasse verfügt auch über eine `StartTimer` Methode, die eine einfache Möglichkeit zum Auslösen der zeitabhängige Aufgaben, die in Xamarin.Forms gemeinsamen Code bereitstellt (einschließlich PCLs) funktioniert. Übergeben einer `TimeSpan` um das Intervall festzulegen und zurückzugeben `true` zu den Timer ausgeführt oder `false` um nach dem aktuellen Aufruf zu beenden.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Wenn der Code innerhalb der Zeitgeber mit der Benutzeroberfläche interagiert (z. B. den Text des eine `Label` oder eine Warnung) sollte in erfolgen eine `BeginInvokeOnMainThread` Ausdruck (siehe unten).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Elemente der Benutzeroberfläche sollte nie von Hintergrundthreads, z. B. in einen Timer oder ein Abschlusshandler für asynchrone Vorgänge wie das webanforderungen ausgeführte Code zugegriffen werden. Alle Hintergrund-Code, der muss die Benutzeroberfläche aktualisieren sollten umhüllt [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Dies ist die Entsprechung der `InvokeOnMainThread` iOS, `RunOnUiThread` auf Android-Geräten und `Dispatcher.RunAsync` auf die universelle Windows-Plattform.

Der Code Xamarin.Forms ist:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Beachten Sie, dass Methoden, die mit `async/await` müssen nicht mit `BeginInvokeOnMainThread` Wenn sie aus dem Hauptbenutzeroberflächen-Thread ausgeführt werden.

## <a name="summary"></a>Zusammenfassung

Der Xamarin.Forms `Device` Klasse ermöglicht eine differenzierte Steuerung Funktionalität und Layouts auf der Basis eines je Plattform – Dies gilt auch für gemeinsamen Code (PCL oder freigegebenen Projekten).


## <a name="related-links"></a>Verwandte Links

- [Geräte-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Formatvorlagen, Beispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Gerät](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
