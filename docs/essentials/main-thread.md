---
title: 'Xamarin.Essentials: MainThread'
description: Die MainThread-Klasse ermöglicht Anwendungen, Code auf die wichtigsten Ausführungsthread auszuführen.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080482"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **MainThread** -Klasse erlaubt Anwendungen das Ausführen von Code auf der Haupt-Thread der Ausführung, und bestimmen, ob auf einen bestimmten Codeblock wird derzeit auf der Haupt-Thread ausgeführt.

## <a name="background"></a>Hintergrund

Die meisten Betriebssysteme, einschließlich iOS, Android und universellen Windows-Plattform – eine Single-threading-Modell für Code, die im Zusammenhang mit der Benutzeroberfläche verwenden. Dieses Modell ist notwendig, ordnungsgemäß Benutzeroberflächen-Ereignisse, einschließlich der Tastaturanschläge zu serialisieren und Fingereingabe. Dieser Thread wird häufig aufgerufen, die _Hauptthread_ oder _Benutzeroberflächenthread_ oder _UI-Thread_. Der Nachteil dieses Modells ist, dass der gesamte Code, der Elemente der Benutzeroberfläche greift auf Hauptthread der Anwendung ausgeführt werden muss. 

Anwendungen müssen gelegentlich Ereignisse verwenden, die auf einem sekundären Thread der Ausführung den Ereignishandler aufgerufen. (Die Klassen Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), und [ `OrientationSensor` ](orientation-sensor.md) möglicherweise alle Informationen auf einem sekundären Thread bei der Verwendung mit schnellere Geschwindigkeiten zurück.) Wenn der Ereignishandler Zugriff auf die Elemente der Benutzeroberfläche muss, müssen sie diesen Code im Hauptthread ausführen. Die **MainThread** Klasse ermöglicht der Anwendung, die diesen Code im Hauptthread auszuführen.

## <a name="running-code-on-the-main-thread"></a>Ausführen von Code auf der Haupt-Thread

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Um Code im Hauptthread auszuführen, rufen Sie die statische `MainThread.BeginInvokeOnMainThread` Methode. Das Argument ist ein [ `Action` ](xref:System.Action) Objekt, das einfach eine Methode ohne Argumente und keinen Wert zurückgibt:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
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
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms verfügt über eine Methode namens [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) , die bewirkt, dass dasselbe als `MainThread.BeginInvokeOnMainThread(Action)`. Während Sie die beiden Methoden in einer Xamarin.Forms-app verwenden können, erwägen Sie, und zwar unabhängig davon, ob der aufrufende Code die Notwendigkeit für eine Abhängigkeit auf Xamarin.Forms verfügt. Wenn dies nicht der Fall ist, `MainThread.BeginInvokeOnMainThread(Action)` ist wahrscheinlich eine bessere Option.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Bestimmen, ob der Code in der Main-Thread ausgeführt wird

Die `MainThread` Klasse kann auch eine Anwendung zu bestimmen, ob Sie ein bestimmter Codeblock auf der Haupt-Thread ausgeführt wird. Die `IsMainThread` -Eigenschaft gibt `true` , wenn der Code, der Aufruf der Eigenschaft auf der Haupt-Thread ausgeführt wird. Ein Programm kann diese Eigenschaft verwenden, unterschiedlichen Code für den Hauptthread oder einem sekundären Thread ausgeführt werden:

```csharp
if (MainThread.IsMainThread)
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
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Sie vermuten, dass diese Überprüfung Leistung verbessert werden kann, wenn der Codeblock, der bereits auf der Haupt-Thread ausgeführt wird.

_Diese Überprüfung ist jedoch nicht erforderlich._ Die Plattform-Implementierungen von `BeginInvokeOnMainThread` selbst überprüfen Sie, ob der Aufruf im Hauptthread ausgeführt wird. Gibt es kaum Leistungseinbußen, beim Aufrufen `BeginInvokeOnMainThread` Wenn es keine wirklich notwendig ist.

## <a name="api"></a>API

- [MainThread-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API-Dokumentation](xref:Xamarin.Essentials.MainThread)
