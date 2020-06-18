---
title: 'Xamarin.Essentials: Plattformerweiterungen'
description: Xamarin.Essentials umfasst mehrere Plattformerweiterungsmethoden für die Arbeit mit Plattformtypen wie „Rect“, „Size“ und „Point“.
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 56cb9619a4132f6568cee8fbf590965934024639
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801922"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials: Plattformerweiterungen

Xamarin.Essentials umfasst mehrere Plattformerweiterungsmethoden für die Arbeit mit Plattformtypen wie „Rect“, „Size“ und „Point“. Das bedeutet, dass Sie die `System`-Version dieser Typen in die iOS-, Android- und UWP-spezifischen Typen konvertieren können.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>Verwenden von Plattformerweiterungen

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Alle Plattformerweiterungen können nur über das iOS-, Android- oder UWP-Projekt aufgerufen werden.

## <a name="android-extensions"></a>Android-Erweiterungen

Auf diese Erweiterungen kann nur in einem Android-Projekt zugegriffen werden.

### <a name="application-context--activity"></a>Anwendungskontext und -aktivität

Mithilfe der Plattformerweiterungen in der `Platform`-Klasse können Sie Zugriff auf den aktuellen `Context` oder die aktuelle `Activity` für die ausgeführte App erhalten.

```csharp

var context = Platform.AppContext;

// Current Activity or null if not initialized or not started.
var activity = Platform.CurrentActivity;
```

Bei einer Situation, in der die `Activity` benötigt wird, die Anwendung aber noch nicht vollständig gestartet wurde, muss die `WaitForActivityAsync`-Methode verwendet werden.

```csharp
var activity = await Platform.WaitForActivityAsync();
```

### <a name="activity-lifecycle"></a>Aktivitätslebenszyklus

Zusätzlich zum Abrufen der aktuellen Aktivität können Sie auch Lebenszyklusereignisse registrieren.

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);

    Xamarin.Essentials.Platform.Init(this, bundle);

    Xamarin.Essentials.Platform.ActivityStateChanged += Platform_ActivityStateChanged;
}

protected override void OnDestroy()
{
    base.OnDestroy();
    Xamarin.Essentials.Platform.ActivityStateChanged -= Platform_ActivityStateChanged;
}

void Platform_ActivityStateChanged(object sender, Xamarin.Essentials.ActivityStateChangedEventArgs e) =>
    Toast.MakeText(this, e.State.ToString(), ToastLength.Short).Show();
```

Es gibt die folgenden Aktivitätszustände:

* Erstellt
* Resumed
* Paused
* Wird zerstört
* SaveInstanceState
* Started
* Beendet

Weitere Informationen finden Sie in der Dokumentation zum [Aktivitätslebenszyklus](https://docs.microsoft.com/xamarin/android/app-fundamentals/activity-lifecycle/).

## <a name="ios-extensions"></a>iOS-Erweiterungen

Auf diese Erweiterungen kann nur in einem iOS-Projekt zugegriffen werden.

### <a name="current-uiviewcontroller"></a>Aktueller UIViewController

Greifen Sie auf den aktuell sichtbaren `UIViewController` zu:

```csharp
var vc = Platform.GetCurrentUIViewController();
```

Diese Methode gibt `null` zurück, wenn kein `UIViewController` erkannt werden kann.

## <a name="cross-platform-extensions"></a>Plattformübergreifende Erweiterungen

Diese Erweiterungen sind auf jeder Plattform vorhanden.

### <a name="point"></a>Punkt

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Point
var system2 = platform.ToSystemPoint();
```

### <a name="size"></a>Größe

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>Rechteck

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Rectangle
var system2 = platform.ToSystemRectangle();
```

## <a name="api"></a>API

- [Converters source code (Quellcode für den Konverter)](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Types/PlatformExtensions)
- [Point Converters API documentation (Dokumentation der Punktkonverter-API)](xref:Xamarin.Essentials.PointExtensions)
- [Rectangle Converters API documentation (Dokumentation der Rechteckkonverter-API)](xref:Xamarin.Essentials.RectangleExtensions)
- [Size Converters API documentation (Dokumentation der Größenkonverter-API)](xref:Xamarin.Essentials.SizeExtensions)
