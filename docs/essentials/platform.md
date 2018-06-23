---
title: Xamarin.Essentials-Plattform
description: Die Plattform-Klasse ermöglicht Anwendungen, Code auf die wichtigsten Ausführungsthread auszuführen.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E0
author: charlespetzold
ms.author: chape
ms.date: 06/03/2018
ms.openlocfilehash: 82e248ee702e104dff98b342aec72179273fc34f
ms.sourcegitcommit: d9ecac62bcf743aff52408fbbd09c5509921a000
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36329947"
---
# <a name="xamarinessentials-platform"></a>Xamarin.Essentials-Plattform

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Plattform** -Klasse erlaubt Anwendungen das Ausführen von Code auf der Haupt-Thread der Ausführung, und bestimmen, ob auf einen bestimmten Codeblock wird derzeit auf der Haupt-Thread ausgeführt.

## <a name="background"></a>Hintergrund

Die meisten Betriebssysteme, einschließlich iOS, Android und universellen Windows-Plattform – eine Single-threading-Modell für Code, die im Zusammenhang mit der Benutzeroberfläche verwenden. Dieses Modell ist notwendig, ordnungsgemäß Benutzeroberflächen-Ereignisse, einschließlich der Tastaturanschläge zu serialisieren und Fingereingabe. Dieser Thread wird häufig aufgerufen, die _Hauptthread_ oder _Benutzeroberflächenthread_ oder _UI-Thread_. Der Nachteil dieses Modells ist, dass der gesamte Code, der Elemente der Benutzeroberfläche greift auf Hauptthread der Anwendung ausgeführt werden muss. 

Anwendungen müssen gelegentlich Ereignisse verwenden, die auf einem sekundären Thread der Ausführung den Ereignishandler aufgerufen. (Die Klassen Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), und [ `OrientationSensor` ](orientation-sensor.md) möglicherweise alle Informationen auf einem sekundären Thread bei der Verwendung mit schnellere Geschwindigkeiten zurück.) Wenn der Ereignishandler Zugriff auf die Elemente der Benutzeroberfläche muss, müssen sie diesen Code im Hauptthread ausführen. Die **Plattform** Klasse ermöglicht der Anwendung, die diesen Code im Hauptthread auszuführen.

## <a name="running-code-on-the-main-thread"></a>Ausführen von Code auf der Haupt-Thread

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Um Code im Hauptthread auszuführen, rufen Sie die statische `Platform.BeginInvokeOnMainThread` Methode. Das Argument ist ein [ `Action` ](xref:System.Action) Objekt, das einfach eine Methode ohne Argumente und keinen Wert zurückgibt:

```csharp
Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Wenn der Aufruf dieser Methode in einer Xamarin.Forms-Anwendung innerhalb einer Klasse, die abgeleitet werden müssen `Element` (inklusive der jede Klasse, die abgeleitet `View` oder `Page`), wird die `Platform` Klassenname muss mit vollständig qualifiziert werden die Namespace-Name:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Es ist auch möglich, eine separate Methode für den Code zu definieren, die auf der Haupt-Thread ausgeführt werden muss:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Anschließend können Sie diese Methode auf der Haupt-Thread ausführen, durch Verweisen auf diese in die `BeginInvokeOnMainThread` Methode:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms verfügt über eine Methode namens [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) , die bewirkt, dass dasselbe als `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`. Während Sie die beiden Methoden in einer Xamarin.Forms-app verwenden können, erwägen Sie, und zwar unabhängig davon, ob der aufrufende Code die Notwendigkeit für eine Abhängigkeit auf Xamarin.Forms verfügt. Wenn dies nicht der Fall ist, `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)` ist wahrscheinlich eine bessere Option.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Bestimmen, ob der Code in der Main-Thread ausgeführt wird

Die `Platform` Klasse kann auch eine Anwendung zu bestimmen, ob Sie ein bestimmter Codeblock auf der Haupt-Thread ausgeführt wird. Die `IsMainThread` -Eigenschaft gibt `true` , wenn der Code, der Aufruf der Eigenschaft auf der Haupt-Thread ausgeführt wird. Ein Programm kann diese Eigenschaft verwenden, unterschiedlichen Code für den Hauptthread oder einem sekundären Thread ausgeführt werden:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Möglicherweise Fragen Sie sich, wenn Sie überprüfen soll, wenn auf einem sekundären Thread vor dem Aufrufen von Code ausgeführt wird `BeginInvokeOnMainThread`, z. B. wie folgt:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Sie vermuten, dass diese Überprüfung Leistung verbessert werden kann, wenn der Codeblock, der bereits auf der Haupt-Thread ausgeführt wird.

_Diese Überprüfung ist jedoch nicht erforderlich._ Die Plattform-Implementierungen von `BeginInvokeOnMainThread` selbst überprüfen Sie, ob der Aufruf im Hauptthread ausgeführt wird. Gibt es kaum Leistungseinbußen, beim Aufrufen `BeginInvokeOnMainThread` Wenn es keine wirklich notwendig ist.

## <a name="api"></a>API

- [Platform-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Platform)
- [Platform-API-Dokumentation](xref:Xamarin.Essentials.Platform)
