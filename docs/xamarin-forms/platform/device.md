---
title: Xamarin.Forms-Device-Klasse
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms-Device-Klasse, für eine präzisere Kontrolle über Funktionen und Layouts individuell für jede Plattform zu verwenden.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2018
ms.openlocfilehash: 084c0c292cb7e527d74c77937bc69f76fc8c0658
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114315"
---
# <a name="xamarinforms-device-class"></a>Xamarin.Forms-Device-Klasse

Die [ `Device` ](xref:Xamarin.Forms.Device) Klasse enthält eine Reihe von Eigenschaften und Methoden für Entwickler, die Layout und die Funktionen auf einer Basis pro Plattform anpassen können.

Neben den Methoden und Eigenschaften zum Code des ereignisdateiziels auf bestimmte Hardwaretypen und Größen der `Device` Klasse enthält die [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) Methode, die verwendet werden sollen, bei der Interaktion mit UI-Steuerelemente Hintergrundthreads.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Clientplattform-spezifische Werte bereitstellen

Vor dem Xamarin.Forms 2.3.4 konnte anhand die Plattform, die die Anwendung ausgeführt wurde abgerufen werden die [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) -Eigenschaft und durch vergleichen, die [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS), [ `TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android), [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone), und [ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows) -Enumerationswerte fest. Auf ähnliche Weise eines der [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) Überladungen verwendet werden, um Werte für ein Steuerelement die plattformspezifische bereitzustellen.

Allerdings haben da Xamarin.Forms 2.3.4 diese APIs als veraltet markiert ersetzt und wurde. Die [ `Device` ](xref:Xamarin.Forms.Device) Klasse enthält jetzt öffentliche Zeichenfolgenkonstanten, die Cloudplattformen identifizieren [ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS), [ `Device.Android` ](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`() als veraltet markiert), `Device.WinRT` (veraltet), [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP), und [ `Device.macOS` ](xref:Xamarin.Forms.Device.macOS). Auf ähnliche Weise die [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) Überladungen wurden durch ersetzt die [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) und [ `On` ](xref:Xamarin.Forms.On) APIs.

In c# können Plattform-spezifische Werte angegeben werden durch das Erstellen einer `switch` -Anweisung für die [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) -Eigenschaft, und anschließend `case` Anweisungen für die erforderlichen Plattformen:

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

Die [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) und [ `On` ](xref:Xamarin.Forms.On) Klassen bieten die gleiche Funktionalität in XAML:

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

Die [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse, die mit instanziiert werden, muss ein `x:TypeArguments` -Attribut, das mit dem Zieltyp übereinstimmt. In der [ `On` ](xref:Xamarin.Forms.On) -Klasse, die [ `Platform` ](xref:Xamarin.Forms.On.Platform) Attribut kann eine einzelne akzeptieren `string` Wert oder mehrere durch Kommas getrennte `string` Werte.

> [!IMPORTANT]
> Bereitstellen eines falschen `Platform` -Attributwert in der `On` Klasse führt nicht zu einem Fehler. Stattdessen führt der Code ohne den plattformspezifischen-Wert, der angewendet wird.

Sie können auch die `OnPlatform` Markuperweiterung in XAML verwendet werden kann, zum Anpassen der Darstellung der Benutzeroberfläche auf einer Basis pro Plattform. Weitere Informationen finden Sie unter [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>"Device.Idiom"

Die `Device.Idiom` Eigenschaft kann verwendet werden, um Layouts zu ändern oder Funktionen, die je nach Gerät die Anwendung ausgeführt wird. Die [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom) Enumeration enthält die folgenden Werte:

-  **Phone** – iPhone, iPod Touch und Android-Geräten schmaler als 600 Dips ^
-  **Tablet** : iPad, Windows-Geräte und Android-Geräte mehr als 600 Dips ^
-  **Desktop** – nur in zurückgegeben [UWP-apps](~/xamarin-forms/platform/windows/installation/index.md) auf Windows 10-Desktopcomputern (gibt `Phone` auf mobilen Windows-Geräten, die auch in Szenarien mit Continuum)
-  **TV** – Tizen TV-Geräten
-  **Sehen Sie sich** – Tizen Watch-Geräten
-  **Nicht unterstützte** : nicht verwendeter

*^ Dips ist nicht unbedingt die Anzahl der physischen Pixel*

Die `Idiom` Eigenschaft ist besonders nützlich zum Erstellen von Layouts, die größere Bildschirme, wie folgt nutzen:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

Die [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) Klasse bietet dieselbe Funktionalität in XAML:

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

Die [ `OnIdiom` ](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse, die mit instanziiert werden, muss ein `x:TypeArguments` -Attribut, das mit dem Zieltyp übereinstimmt.

Sie können auch die `OnIdiom` Markuperweiterung in XAML verwendet werden kann, zum Anpassen der UI-Darstellung, die abhängig von der Sprache des Geräts auf die Anwendung ausgeführt wird. Weitere Informationen finden Sie unter [OnIdiom Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom).

## <a name="deviceflowdirection"></a>Device.FlowDirection

Die [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert Ruft eine [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) Enumerationswert, der die aktuelle flussrichtung, der vom Gerät verwendeten darstellt. Flussrichtung ist die Richtung, in der die Elemente der Benutzeroberfläche auf der Seite vom Auge gescannt werden. Diese Enumerationswerte lauten:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

In XAML die [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert abgerufen werden kann, mit der `x:Static` Markuperweiterung:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Der entsprechende Code in C# geschrieben ist:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Weitere Informationen zu Richtung des Inhaltsflusses, finden Sie unter [rechts-nach-links-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

Die [ `Styles` Eigenschaft](~/xamarin-forms/user-interface/styles/index.md) enthält integrierte Formatdefinitionen, die auf einige Steuerelemente angewendet werden können (z. B. `Label`) `Style` Eigenschaft. Die verfügbaren Formate sind:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` kann verwendet werden, wenn Sie festlegen [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) in C#-Code:

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

Die `OpenUri` Methode kann zum Auslösen von Vorgängen auf die zugrunde liegende Plattform, z. B. eine URL öffnen, in das native Webbrowser verwendet werden (**Safari** unter iOS oder **Internet** unter Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

Die [WebView Beispiel](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) enthält ein Beispiel mit `OpenUri` URLs öffnen und außerdem lösen Telefonanrufe.

Die [Beispiel Maps](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) verwendet auch `Device.OpenUri` zum Anzeigen von Karten und wegbeschreibungen mithilfe der systemeigenen **ordnet** apps auf IOS- und Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

Die `Device` -Klasse verfügt auch über eine `StartTimer` Methode bietet eine einfache Möglichkeit zum Auslösen von zeitabhängigen Aufgaben, die in Xamarin.Forms gemeinsame Code, einschließlich der .NET Standard-Bibliothek funktioniert. Übergeben Sie eine `TimeSpan` um das Intervall festzulegen und zurückzugeben `true` zu den Timer ausgeführt wird oder `false` nach dem aktuellen Aufruf beenden.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Wenn der Code in den Timer mit der Benutzeroberfläche interagiert (z. B. das Festlegen des Texts des eine `Label` oder eine Warnung) sollte in erfolgen eine `BeginInvokeOnMainThread` Ausdruck (siehe unten).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Elemente der Benutzeroberfläche sollten niemals von Hintergrundthreads, z. B. Code für einen Timer oder einen Abschlusshandler für asynchrone Vorgänge wie webanforderungen zugegriffen werden. Alle Hintergrund-Code, der zum Aktualisieren der Benutzeroberfläche muss sollten umhüllt [ `BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)). Dies ist die Entsprechung der `InvokeOnMainThread` unter iOS, `RunOnUiThread` unter Android und `Dispatcher.RunAsync` für die universelle Windows-Plattform.

Der Xamarin.Forms-Code ist:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Beachten Sie, dass Methoden, die mit `async/await` müssen nicht mit `BeginInvokeOnMainThread` würden sie aus dem Hauptbenutzeroberflächen-Thread ausgeführt werden.

## <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms `Device` -Klasse ermöglicht eine präzisere Kontrolle über Funktionen und Layouts individuell für jede Plattform – sogar gemeinsamen Code (.NET Standard Library-Projekten oder freigegebene Projekte).


## <a name="related-links"></a>Verwandte Links

- [-Gerätebeispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Stile-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Gerät](xref:Xamarin.Forms.Device)
