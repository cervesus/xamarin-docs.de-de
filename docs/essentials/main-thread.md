---
title: 'Xamarin.Essentials: MainThread'
description: Die MainThread-Klasse kann Anwendungen zur Ausführung von Code in den Hauptausführungsthread.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831424"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **MainThread** -Klasse erlaubt Anwendungen das Ausführen von Code im primären Thread der Ausführung wird und an einen bestimmten Codeblock zu bestimmen, ob derzeit im Hauptthread ausgeführt.

## <a name="background"></a>Hintergrund

Die meisten Betriebssysteme, einschließlich iOS, Android und die universelle Windows-Plattform – verwenden Sie eine Single-threading-Modell für Code, die im Zusammenhang mit der Benutzeroberfläche. Dieses Modell ist notwendig, ordnungsgemäß Benutzeroberflächen-Ereignisse, einschließlich des Tastatureingaben zu serialisieren und touch-Eingabe. Dieser Thread wird häufig aufgerufen, die _Hauptthread_ oder _Benutzeroberflächen-Thread_ oder _UI-Thread_. Der Nachteil dieses Modells ist, dass sämtlicher Code, der Elemente der Benutzeroberfläche greift auf Hauptthread der Anwendung ausgeführt werden muss. 

Anwendungen müssen manchmal Ereignisse verwenden, die in einem zweiten Thread der Ausführung der-Ereignishandler aufrufen. (Die Klassen Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), und [ `OrientationSensor` ](orientation-sensor.md) möglicherweise alle Informationen in einem zweiten Thread bei der Verwendung mit höherer Geschwindigkeit zurück.) Wenn der Ereignishandler auf Benutzeroberflächenelemente zugreifen muss, müssen sie diesen Code im Hauptthread ausführen. Die **MainThread** Klasse ermöglicht der Anwendung, den Code im Hauptthread auszuführen.

## <a name="running-code-on-the-main-thread"></a>Ausführen von Code auf dem Hauptthread

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Um Code im Hauptthread auszuführen, rufen Sie die statische `MainThread.BeginInvokeOnMainThread` Methode. Das Argument ist ein [ `Action` ](xref:System.Action) Objekt, das ist einfach eine Methode ohne Argumente und keinen Wert zurückgibt:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Es ist auch möglich, eine separate Methode für den Code zu definieren, die im Hauptthread ausgeführt werden muss:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Anschließend können Sie diese Methode auf dem Hauptthread ausführen, durch Verweisen auf in die `BeginInvokeOnMainThread` Methode:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms verfügt über eine Methode namens [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) , das ist dasselbe wie `MainThread.BeginInvokeOnMainThread(Action)`. Während Sie die beiden Methoden in einer Xamarin.Forms-app verwenden können, erwägen Sie, ob der aufrufende Code andere Bedarf für eine Abhängigkeit auf Xamarin.Forms verfügt. Wenn dies nicht der Fall ist, `MainThread.BeginInvokeOnMainThread(Action)` ist wahrscheinlich eine bessere Option.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Bestimmen, ob Code auf dem Hauptthread ausgeführt wird

Die `MainThread` Klasse kann auch eine Anwendung zu bestimmen, ob Sie ein bestimmter Codeblock im Hauptthread ausgeführt wird. Die `IsMainThread` -Eigenschaft gibt `true` , wenn der Code, das Aufrufen der Eigenschaft im Hauptthread ausgeführt wird. Ein Programm kann diese Eigenschaft verwenden, um unterschiedlichen Code für den Hauptthread oder einem sekundären Thread auszuführen zu können:

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

Vielleicht fragen Sie sich, wenn Sie überprüfen sollten, wenn Code in einem zweiten Thread vor dem Aufruf ausgeführt wird `BeginInvokeOnMainThread`, z. B. wie folgt aus:

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

Sie vermuten, dass diese Überprüfung Leistung verbessert werden kann, wenn der Codeblock bereits im Hauptthread ausgeführt wird.

_Diese Überprüfung ist jedoch nicht erforderlich._ Die plattformimplementierungen der `BeginInvokeOnMainThread` selbst zu überprüfen, ob der Aufruf im Hauptthread ausgeführt wird. Es gibt nur sehr wenig Leistungseinbußen, wenn es sich bei rufen Sie `BeginInvokeOnMainThread` Wenn es nicht notwendig ist.

## <a name="api"></a>API

- [MainThread-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread-API-Dokumentation](xref:Xamarin.Essentials.MainThread)
