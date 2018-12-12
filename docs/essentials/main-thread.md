---
title: 'Xamarin.Essentials: MainThread'
description: Mit der MainThread-Klasse können Anwendungen Code im Hauptausführungsthread ausführen.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 7ec1420d87c898f63614eb6d980c28834e980afd
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899004"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

Mit der **MainThread**-Klasse können Anwendungen Code im Hauptausführungsthread ausführen und bestimmen, ob ein bestimmter Codeblock aktuell im Hauptthread ausgeführt wird.

## <a name="background"></a>Hintergrund

Die meisten Betriebssysteme – einschließlich iOS, Android und der universellen Windows-Plattform (UWP) – verwenden ein Single-Threading-Modell für Code, der die Benutzeroberfläche betrifft. Dieses Modell ist notwendig, um Ereignisse der Benutzeroberfläche, einschließlich Tastaturanschläge und Toucheingabe, ordnungsgemäß zu serialisieren. Dieser Thread wird oft als _Hauptthread_, _Benutzeroberflächenthread_ oder _UI-Thread_ bezeichnet. Der Nachteil dieses Modells ist, dass Code, der auf Elemente der Benutzeroberfläche zugreift, im Hauptthread der Anwendung ausgeführt werden muss. 

Anwendungen müssen manchmal Ereignisse verwenden, die den Ereignishandler in einem sekundären Ausführungsthread aufrufen. Die Xamarin.Essentials-Klassen [`Accelerometer`](accelerometer.md), [`Compass`](compass.md), [`Gyroscope`](gyroscope.md), [`Magnetometer`](magnetometer.md) und [`OrientationSensor`](orientation-sensor.md) geben bei Verwendung mit höheren Geschwindigkeiten ggf. Informationen zu einem Sekundärthread zurück. Wenn der Ereignishandler auf Elemente der Benutzeroberfläche zugreifen muss, muss er diesen Code im Hauptthread ausführen. Mit der **MainThread**-Klasse können Anwendungen diesen Code im Hauptthread ausführen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="running-code-on-the-main-thread"></a>Ausführen von Code im Hauptthread

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Um Code im Hauptthread auszuführen, rufen Sie die statische `MainThread.BeginInvokeOnMainThread`-Methode auf. Das Argument ist ein [`Action`](xref:System.Action)-Objekt, das einfach eine Methode ohne Argumente und ohne Rückgabewert ist:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Es ist auch möglich, eine separate Methode für den Code zu definieren, der im Hauptthread ausgeführt werden muss:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Sie können diese Methode dann im Hauptthread ausführen, indem Sie sie in der `BeginInvokeOnMainThread`-Methode referenzieren:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms verfügt über eine Methode namens [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread),
> mit der Sie dieselbe Aufgabe wie mit `MainThread.BeginInvokeOnMainThread(Action)` ausführen können. Sie können beide Methoden in einer Xamarin.Forms-App verwenden. Erwägen Sie jedoch, ob der aufrufende Code andere Abhängigkeiten von Xamarin.Forms benötigt. Wenn das nicht der Fall ist, ist `MainThread.BeginInvokeOnMainThread(Action)` besser geeignet.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Festlegen, ob Code im Hauptthread ausgeführt wird

Mit der `MainThread`-Klasse kann eine Anwendung außerdem bestimmen, ob ein bestimmter Codeblock im Hauptthread ausgeführt wird. Die `IsMainThread`-Eigenschaft gibt `true` zurück, wenn der Code, der die Eigenschaft aufruft, im Hauptthread ausgeführt wird. Ein Programm kann diese Eigenschaft verwenden, um anderen Code für den Hauptthread oder einen Sekundärthread auszuführen:

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

Vielleicht möchten Sie überprüfen, ob Code auf einem sekundären Thread ausgeführt wird, bevor Sie `BeginInvokeOnMainThread` aufrufen:

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

Möglicherweise vermuten Sie, dass diese Überprüfung die Leistung verbessert, wenn der Codeblock bereits auf dem Hauptthread läuft.

_Diese Überprüfung ist jedoch nicht erforderlich._ Die Plattformimplementierungen von `BeginInvokeOnMainThread` überprüfen selbst, ob der Aufruf im Hauptthread erfolgt. Es gibt nur sehr geringe Leistungseinbußen, wenn Sie `BeginInvokeOnMainThread` aufrufen, ohne dass es wirklich notwendig ist.

## <a name="api"></a>API

- [MainThread-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread-API-Dokumentation](xref:Xamarin.Essentials.MainThread)
