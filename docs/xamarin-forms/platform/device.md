---
title: Xamarin. Forms-Geräteklasse
description: In diesem Artikel wird erläutert, wie Sie die xamarin. Forms-Geräteklasse verwenden, um die Funktionalität und Layouts auf Platt Form Ebene genauer steuern zu können.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
ms.openlocfilehash: 25ddbea75d0fd6858f848499281da5d5f0b68171
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697198"
---
# <a name="xamarinforms-device-class"></a>Xamarin. Forms-Geräteklasse

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

Die [`Device`](xref:Xamarin.Forms.Device) -Klasse enthält eine Reihe von Eigenschaften und Methoden, die Entwicklern dabei helfen, Layout und Funktionalität plattformspezifisch anzupassen.

Zusätzlich zu den Methoden und Eigenschaften für den Code auf bestimmten Hardwaretypen und-Größen enthält die `Device`-Klasse Methoden, die für die Interaktion mit UI-Steuerelementen aus Hintergrundthreads verwendet werden können. Weitere Informationen finden Sie unter [Interaktion mit der Benutzeroberfläche von Hintergrundthreads](#interact-with-the-ui-from-background-threads).

## <a name="providing-platform-specific-values"></a>Bereitstellen von plattformspezifischen Werten

Vor xamarin. Forms 2.3.4 konnte die Plattform, auf der die Anwendung ausgeführt wurde, durch Untersuchen der [`Device.OS`](xref:Xamarin.Forms.Device.OS) -Eigenschaft und vergleichen mit der [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)-, [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android)-, [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone)-und [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) -Enumeration abgerufen werden. Werte. Ebenso kann eine der [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) Überladungen verwendet werden, um plattformspezifische Werte für ein Steuerelement bereitzustellen.

Seit xamarin. Forms 2.3.4 sind diese APIs jedoch veraltet und wurden ersetzt. Die [`Device`](xref:Xamarin.Forms.Device) -Klasse enthält nun öffentliche Zeichen folgen Konstanten, die Plattformen identifizieren – [`Device.iOS`](xref:Xamarin.Forms.Device.iOS), [`Device.Android`](xref:Xamarin.Forms.Device.Android), `Device.WinPhone` (deprecated), `Device.WinRT` (veraltet), [`Device.UWP`](xref:Xamarin.Forms.Device.UWP)und [1](xref:Xamarin.Forms.Device.macOS). Ebenso wurden die [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) Überladungen durch die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -und [`On`](xref:Xamarin.Forms.On) -APIs ersetzt.

In C#können plattformspezifische Werte bereitgestellt werden, indem eine `switch`-Anweisung für die [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) -Eigenschaft erstellt und dann `case` Anweisungen für die erforderlichen Plattformen bereitgestellt werden:

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

Die Klassen [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) und [`On`](xref:Xamarin.Forms.On) bieten die gleiche Funktionalität in XAML:

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

Die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse, die mit einem `x:TypeArguments` Attribut instanziiert werden muss, das mit dem Zieltyp übereinstimmt. In der [`On`](xref:Xamarin.Forms.On) -Klasse kann das [`Platform`](xref:Xamarin.Forms.On.Platform) -Attribut einen einzelnen `string` Wert oder mehrere durch Trennzeichen getrennte `string` Werte akzeptieren.

> [!IMPORTANT]
> Die Angabe eines falschen `Platform` Attribut Werts in der `On` Klasse führt nicht zu einem Fehler. Stattdessen wird der Code ausgeführt, ohne dass der plattformspezifische Wert angewendet wird.

Alternativ kann die `OnPlatform` Markup Erweiterung in XAML verwendet werden, um die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen. Weitere Informationen finden Sie unter [onplatform-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="deviceidiom"></a>Device. Idiom

Mit der `Device.Idiom`-Eigenschaft können Layouts oder Funktionen abhängig vom Gerät, auf dem die Anwendung ausgeführt wird, geändert werden. Die [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) -Enumeration enthält die folgenden Werte:

- **Phone** – iPhone, iPod Touchscreen und Android-Geräte, die kleiner als 600 Dips ^ sind
- **Tablet** – iPad, Windows-Geräte und Android-Geräte, die breiter als 600 Dips ^ sind
- **Desktop** – wird nur in [UWP-apps](~/xamarin-forms/platform/windows/installation/index.md) auf Windows 10-Desktop Computern zurückgegeben (gibt `Phone` auf mobilen Windows-Geräten zurück, einschließlich in Continuum-Szenarien).
- **TV-** – tizen TV-Geräte
- **Ansehen** – tizen Watch-Geräte
- **Nicht unterstützt – nicht** verwendet

*^ Dips ist nicht notwendigerweise die physische Pixelanzahl.*

Die `Idiom`-Eigenschaft ist besonders nützlich, um Layouts zu erstellen, die größere Bildschirme nutzen, wie hier:

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

Alternativ kann die `OnIdiom` Markup Erweiterung in XAML verwendet werden, um die Darstellung der Benutzeroberfläche basierend auf dem Aussehen des Geräts anzupassen, auf dem die Anwendung ausgeführt wird. Weitere Informationen finden Sie unter [onidiom-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom).

## <a name="deviceflowdirection"></a>Device. FlowDirection

Der [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert Ruft einen [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) Enumerationswert ab, der die aktuelle Fluss Richtung darstellt, die vom Gerät verwendet wird. Die Leserichtung ist die Richtung, in der Benutzeroberflächenelemente auf einer Seite vom Auge wahrgenommen werden. Diese Enumerationswerte lauten:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

In XAML kann der [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert mit der `x:Static` Markup Erweiterung abgerufen werden:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Der entsprechende Code in C# ist:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Weitere Informationen zur Fluss Richtung finden Sie unter von [rechts nach links lokalisierte Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="devicestyles"></a>Device. Styles

Die [`Styles`-Eigenschaft](~/xamarin-forms/user-interface/styles/index.md) enthält integrierte Formatvorlagen Definitionen, die auf einige Steuerelemente (z. b. `Label`) `Style` Eigenschaft angewendet werden können. Folgende Stile sind verfügbar:

- BodyStyle
- Captionstyle
- Listitemdetailtextstyle
- Listitemtextstyle
- Subtitlestyle
- TitleStyle

## <a name="devicegetnamedsize"></a>Device. getnamedsize

`GetNamedSize` kann verwendet werden, wenn [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) im C# Code festgelegt wird:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicestarttimer"></a>Device. StartTimer

Die `Device`-Klasse verfügt auch über eine `StartTimer`-Methode, die eine einfache Möglichkeit bietet, zeitabhängige Aufgaben zu initiieren, die im allgemeinen Code von xamarin. Forms funktionieren, einschließlich einer .NET Standard Bibliothek. Übergeben Sie einen `TimeSpan`, um das Intervall festzulegen und `true` zurückzugeben, damit der Timer ausgeführt wird oder `false`, um ihn nach dem aktuellen Aufruf anzuhalten.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Wenn der Code im Timer mit der Benutzeroberfläche interagiert (z. b. das Festlegen des Texts einer `Label` oder das Anzeigen einer Warnung), sollte er innerhalb eines `BeginInvokeOnMainThread` Ausdrucks erfolgen (siehe unten).

> [!NOTE]
> Die Klassen `System.Timers.Timer` und `System.Threading.Timer` sind .NET Standard Alternativen zur Verwendung der `Device.StartTimer`-Methode.

## <a name="interact-with-the-ui-from-background-threads"></a>Interaktion mit der Benutzeroberfläche über Hintergrundthreads

Die meisten Betriebssysteme, einschließlich IOS, Android und der universelle Windows-Plattform, verwenden ein Single-Threading-Modell für Code, der die Benutzeroberfläche einbezieht. Dieser Thread wird häufig als *Hauptthread* oder UI- *Thread*bezeichnet. Eine Folge dieses Modells ist, dass sämtlicher Code, der auf Benutzeroberflächen Elemente zugreift, im Haupt Thread der Anwendung ausgeführt werden muss.

Anwendungen verwenden manchmal Hintergrundthreads, um potenziell lange ausgeführten Vorgänge auszuführen, z. b. das Abrufen von Daten aus einem Webdienst. Wenn Code, der in einem Hintergrund Thread ausgeführt wird, auf Benutzeroberflächen Elemente zugreifen muss, muss dieser Code im Haupt Thread ausgeführt werden.

Die `Device`-Klasse enthält die folgenden `static` Methoden, die für die Interaktion mit Benutzeroberflächen Elementen aus Hintergrundthreads verwendet werden können:

| Methode | Argumente | Rückgabe | Zweck |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | Ruft eine `Action` auf dem Haupt Thread auf und wartet nicht auf den Abschluss des Vorgangs. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Ruft `Func<T>` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Ruft `Action` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Ruft `Func<Task<T>>` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Ruft `Func<Task>` auf dem Hauptthread auf, und wartet auf den Abschluss |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | Gibt `SynchronizationContext` für den Hauptthread zurück |

Der folgende Code zeigt ein Beispiel für die Verwendung der `BeginInvokeOnMainThread`-Methode:

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>Verwandte Links

- [Geräte Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [Beispiel für Stile](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Gerät](xref:Xamarin.Forms.Device)
