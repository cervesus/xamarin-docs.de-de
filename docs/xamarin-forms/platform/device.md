---
title: Xamarin. Forms-Geräteklasse
description: In diesem Artikel wird erläutert, wie Sie die xamarin. Forms-Geräteklasse verwenden, um die Funktionalität und Layouts auf Platt Form Ebene genauer steuern zu können.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
ms.openlocfilehash: d0f0fa7dd68e8852dd7a72486c155ec064540644
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517072"
---
# <a name="xamarinforms-device-class"></a>Xamarin. Forms-Geräteklasse

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

Die [`Device`](xref:Xamarin.Forms.Device) -Klasse enthält eine Reihe von Eigenschaften und Methoden, die Entwicklern dabei helfen, Layout und Funktionalität plattformspezifisch anzupassen.

Zusätzlich zu den Methoden und Eigenschaften für den Code bestimmter Hardwaretypen und-Größen enthält die `Device` -Klasse Methoden, die für die Interaktion mit UI-Steuerelementen aus Hintergrundthreads verwendet werden können. Weitere Informationen finden Sie unter [Interaktion mit der Benutzeroberfläche von Hintergrundthreads](#interact-with-the-ui-from-background-threads).

## <a name="provide-platform-specific-values"></a>Bereitstellen von plattformspezifischen Werten

Vor xamarin. Forms 2.3.4 konnte die Plattform, auf der die Anwendung ausgeführt wurde, abgerufen werden, indem [`Device.OS`](xref:Xamarin.Forms.Device.OS) die-Eigenschaft untersucht und [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)mit [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android)den [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone)Enumerationswerten,, und [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) verglichen wird. Ebenso könnte eine der [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) über Ladungen verwendet werden, um plattformspezifische Werte für ein Steuerelement bereitzustellen.

Seit xamarin. Forms 2.3.4 sind diese APIs jedoch veraltet und wurden ersetzt. Die [`Device`](xref:Xamarin.Forms.Device) -Klasse enthält nun öffentliche Zeichen folgen Konstanten, die [`Device.iOS`](xref:Xamarin.Forms.Device.iOS)Platt [`Device.Android`](xref:Xamarin.Forms.Device.Android)Formen `Device.WinPhone`identifizieren –,, ( `Device.WinRT` veraltet), [`Device.UWP`](xref:Xamarin.Forms.Device.UWP), und [`Device.macOS`](xref:Xamarin.Forms.Device.macOS). Ebenso wurden die [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) -über Ladungen durch die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -und- [`On`](xref:Xamarin.Forms.On) APIs ersetzt.

In c# können plattformspezifische Werte bereitgestellt werden, indem eine `switch` -Anweisung für [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) die-Eigenschaft erstellt und `case` dann-Anweisungen für die erforderlichen Plattformen bereitgestellt werden:

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

Die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) - [`On`](xref:Xamarin.Forms.On) Klasse und die-Klasse stellen die gleichen Funktionen in XAML bereit:

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

Die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse, die mit einem `x:TypeArguments` Attribut instanziiert werden muss, das mit dem Zieltyp übereinstimmt. In der [`On`](xref:Xamarin.Forms.On) -Klasse kann [`Platform`](xref:Xamarin.Forms.On.Platform) das-Attribut einen einzelnen `string` Wert oder mehrere durch Trennzeichen getrennte `string` Werte annehmen.

> [!IMPORTANT]
> Die Angabe eines `Platform` falschen Attribut Werts in `On` der Klasse führt nicht zu einem Fehler. Stattdessen wird der Code ausgeführt, ohne dass der plattformspezifische Wert angewendet wird.

Alternativ kann die `OnPlatform` Markup Erweiterung in XAML verwendet werden, um die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen. Weitere Informationen finden Sie unter [onplatform-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="deviceidiom"></a>Device. Idiom

Die `Device.Idiom` -Eigenschaft kann verwendet werden, um Layouts oder Funktionen abhängig vom Gerät zu ändern, auf dem die Anwendung ausgeführt wird. Die [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) -Enumeration enthält die folgenden Werte:

- **Phone** – iPhone, iPod Touchscreen und Android-Geräte, die kleiner als 600 Dips ^ sind
- **Tablet** – iPad, Windows-Geräte und Android-Geräte, die breiter als 600 Dips ^ sind
- **Desktop** – wird nur in [UWP-apps](~/xamarin-forms/platform/windows/installation/index.md) auf Windows 10-Desktop Computern `Phone` zurückgegeben (wird auf mobilen Windows-Geräten zurückgegeben, einschließlich in Continuum-Szenarios)
- **TV-** – tizen TV-Geräte
- **Ansehen** – tizen Watch-Geräte
- **Nicht unterstützt – nicht** verwendet

*^ Dips ist nicht notwendigerweise die physische Pixelanzahl.*

Die `Idiom` -Eigenschaft ist besonders nützlich, um Layouts zu erstellen, die größere Bildschirme nutzen, wie hier:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

Die [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) -Klasse bietet die gleiche Funktionalität in XAML:

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

Die [`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse, die mit einem `x:TypeArguments` Attribut instanziiert werden muss, das mit dem Zieltyp übereinstimmt.

Alternativ kann die `OnIdiom` Markup Erweiterung in XAML verwendet werden, um die Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts anzupassen, auf dem die Anwendung ausgeführt wird. Weitere Informationen finden Sie unter [onidiom-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom).

## <a name="deviceflowdirection"></a>Device. FlowDirection

Der [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) -Wert Ruft einen [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) -Enumerationswert ab, der die aktuelle Fluss Richtung darstellt, die vom Gerät verwendet wird. Die Leserichtung ist die Richtung, in der Benutzeroberflächenelemente auf einer Seite vom Auge wahrgenommen werden. Diese Enumerationswerte lauten:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

In XAML kann der [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert mit der `x:Static` Markup Erweiterung abgerufen werden:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Der äquivalente Code in c# lautet wie folgt:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Weitere Informationen zur Fluss Richtung finden Sie unter von [rechts nach links lokalisierte Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="devicestyles"></a>Device. Styles

Die [ `Styles` -Eigenschaft](~/xamarin-forms/user-interface/styles/index.md) enthält integrierte Formatvorlagen Definitionen, die auf einige Steuerelemente (z. b. `Label`) `Style` angewendet werden können. Folgende Stile sind verfügbar:

- BodyStyle
- Captionstyle
- Listitemdetailtextstyle
- Listitemtextstyle
- Subtitlestyle
- TitleStyle

## <a name="devicegetnamedsize"></a>Device. getnamedsize

`GetNamedSize`kann verwendet werden, wenn [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) in c#-Code festgelegt wird:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicegetnamedcolor"></a>Device. getnamedcolor

Xamarin. Forms 4,6 führt die Unterstützung für benannte Farben ein. Eine benannte Farbe ist eine Farbe mit einem anderen Wert, je nachdem, welcher System Modus (z. b. "Hell" oder "dunkel") auf dem Gerät aktiv ist. Unter Android wird auf benannte Farben über die [R. Color](https://developer.android.com/reference/android/R.color#constants_2) -Klasse zugegriffen. Unter IOS werden benannte Farben als [Systemfarben](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#system-colors)bezeichnet. Auf der universelle Windows-Plattform werden benannte Farben als [XAML](/windows/uwp/design/controls-and-patterns/xaml-theme-resources)-Design Ressourcen bezeichnet.

Die `GetNamedColor` -Methode kann zum Abrufen benannter Farben auf Android, IOS und UWP verwendet werden. Die-Methode nimmt `string` ein-Argument an [`Color`](xref:Xamarin.Forms.Color)und gibt Folgendes zurück:

```csharp
// Retrieve an Android named color
Color color = Device.GetNamedColor(NamedPlatformColor.HoloBlueBright);
```

`Color.Default`wird zurückgegeben, wenn ein Farbname nicht gefunden werden kann oder `GetNamedColor` wenn auf einer nicht unterstützten Plattform aufgerufen wird.

> [!NOTE]
> Da die `GetNamedColor` Methode einen `Color` -Wert zurückgibt, der für eine Plattform spezifisch ist, sollte Sie in der Regel [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) in Verbindung mit der-Eigenschaft verwendet werden.

Die `NamedPlatformColor` -Klasse enthält die Konstanten, mit denen die benannten Farben für Android, IOS und UWP definiert werden:

| Android | iOS | UWP |
| --- | --- | --- |
| `BackgroundDark` | `Label` | `SystemAltHighColor` |
| `BackgroundLight` | `Link` | `SystemAltLowColor` |
| `Black` | `OpaqueSeparator` | `SystemAltMediumColor` |
| `DarkerGray` | `PlaceholderText` | `SystemAltMediumHighColor` |
| `HoloBlueBright` | `QuaternaryLabel` | `SystemAltMediumLowColor` |
| `HoloBlueDark` | `SecondaryLabel` | `SystemBaseHighColor` |
| `HoloBlueLight` | `Separator` | `SystemBaseLowColor` |
| `HoloGreenDark` | `SystemBlue` | `SystemBaseMediumColor` |
| `HoloGreenLight` | `SystemGray` | `SystemBaseMediumHighColor` |
| `HoloOrangeDark` | `SystemGray2` | `SystemBaseMediumLowColor` |
| `HoloOrangeLight` | `SystemGray3` | `SystemChromeAltLowColor` |
| `HoloPurple` | `SystemGray4` | `SystemChromeBlackHighColor` |
| `HoloRedDark` | `SystemGray5` | `SystemChromeBlackLowColor` |
| `HoloRedLight` | `SystemGray6` | `SystemChromeBlackMediumColor` |
| `TabIndicatorText` | `SystemGreen` | `SystemChromeBlackMediumLowColor` |
| `Transparent` | `SystemIndigo` | `SystemChromeDisabledHighColor` |
| `White` | `SystemListLowColor` | `SystemChromeDisabledLowColor` |
| `WidgetEditTextDark` | `SystemListMediumColor` | `SystemChromeHighColor` |
| | `SystemPink` | `SystemChromeLowColor` |
| | `SystemPurple` | `SystemChromeMediumColor` |
| | `SystemRed` | `SystemChromeMediumLowColor` |
| | `SystemTeal` | `SystemChromeWhiteColor` |
| | `SystemYellow` |
| | `TertiaryLabel` |

## <a name="devicestarttimer"></a>Device. StartTimer

Die `Device` -Klasse verfügt auch `StartTimer` über eine-Methode, die eine einfache Möglichkeit bietet, zeitabhängige Aufgaben zu initiieren, die im allgemeinen Code von xamarin. Forms funktionieren, einschließlich einer .NET Standard-Bibliothek. Übergeben Sie `TimeSpan` einen, um das Intervall fest `true` zulegen, und geben Sie zurück `false` , um den Timer zu halten oder nach dem aktuellen Aufruf anzuhalten.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Wenn der Code im Timer mit der Benutzeroberfläche interagiert (z. b. das Festlegen des Texts eines `Label` oder das Anzeigen einer Warnung), sollte er innerhalb eines `BeginInvokeOnMainThread` -Ausdrucks erfolgen (siehe unten).

> [!NOTE]
> Die `System.Timers.Timer` - `System.Threading.Timer` Klasse und die-Klasse sind .NET Standard `Device.StartTimer` Alternativen zur Verwendung der-Methode.

## <a name="interact-with-the-ui-from-background-threads"></a>Interaktion mit der Benutzeroberfläche über Hintergrundthreads

Die meisten Betriebssysteme, einschließlich IOS, Android und der universelle Windows-Plattform, verwenden ein Single-Threading-Modell für Code, der die Benutzeroberfläche einbezieht. Dieser Thread wird häufig als *Hauptthread* oder UI- *Thread*bezeichnet. Eine Folge dieses Modells ist, dass sämtlicher Code, der auf Benutzeroberflächen Elemente zugreift, im Haupt Thread der Anwendung ausgeführt werden muss.

Anwendungen verwenden manchmal Hintergrundthreads, um potenziell lange ausgeführten Vorgänge auszuführen, z. b. das Abrufen von Daten aus einem Webdienst. Wenn Code, der in einem Hintergrund Thread ausgeführt wird, auf Benutzeroberflächen Elemente zugreifen muss, muss dieser Code im Haupt Thread ausgeführt werden.

Die `Device` -Klasse enthält die `static` folgenden Methoden, die für die Interaktion mit Benutzeroberflächen Elementen aus Hintergrundthreads verwendet werden können:

| Methode | Argumente | Rückgabe | Zweck |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | Ruft eine `Action` im Haupt Thread auf und wartet nicht darauf, dass Sie beendet wird. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Ruft `Func<T>` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Ruft `Action` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Ruft `Func<Task<T>>` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Ruft `Func<Task>` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | Gibt `SynchronizationContext` für den Hauptthread zurück |

Der folgende Code zeigt ein Beispiel für die Verwendung `BeginInvokeOnMainThread` der-Methode:

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>Verwandte Links

- [Geräte Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [Beispiel für Stile](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Geräte-API](xref:Xamarin.Forms.Device)
