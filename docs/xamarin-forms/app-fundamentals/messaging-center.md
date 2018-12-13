---
title: Xamarin.Forms MessagingCenter
description: In diesem Artikel wird erläutert, wie Xamarin.Forms MessagingCenter zum Senden und Empfangen von Nachrichten verwendet wird, um die Kopplung zwischen Klassen wie ViewModels zu reduzieren.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 7fef4443cacba0fa8bdb8d5df070c4244730b4f5
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675174"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms umfasst einen einfachen Messagingdienst zum Senden und Empfangen von Nachrichten._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Mit Xamarin.Forms `MessagingCenter` können ViewModels und andere Komponenten kommunizieren, ohne etwas über die andere Partei wissen zu müssen, nur mit einem einfachen Nachrichtenvertrag.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Wie funktioniert MessagingCenter?

Es gibt zwei Teile von `MessagingCenter`:

-  **Abonnieren:** Lauschen auf Nachrichten mit einer bestimmten Signatur und Ausführen einiger Aktionen, wenn sie empfangen werden. Mehrere Abonnenten können auf dieselbe Nachricht warten.
-  **Senden:** Veröffentlichen einer Nachricht für Zuhörer, um entsprechend zu handeln. Wenn eine Nachricht von keinem Zuhörer abonniert wurde, wird sie ignoriert.


`MessagingService` ist eine statische Klasse mit `Subscribe`- und `Send`-Methoden, die bei dieser Lösung verwendet werden.

Nachrichten besitzen einen `message`-Zeichenfolgenparameter, der verwendet wird, um Nachrichten zu *adressieren*. Die Methoden `Subscribe` und `Send` verwenden generische Parameter, um weiter zu steuern, wie Nachrichten geliefert werden. Zwei Nachrichten mit demselben `message`-Text aber unterschiedlichen generischen Typargumenten werden nicht an denselben Abonnenten geliefert.

Die API für `MessagingCenter` ist einfach:

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

Diese Methoden werden nachfolgend erklärt.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>Verwenden von MessagingCenter

Nachrichten können als Ergebnis von Benutzerinteraktionen (z. B. Klicken auf eine Schaltfläche), eines Systemereignisses (z. B. Ändern des Steuerelementstatus) oder einiger anderer Incidents (z. B. Abschluss eines asynchronen Downloads) gesendet werden. Abonnenten könnten warten, um die Darstellung der Benutzeroberfläche zu ändern, Daten zu speichern oder einige andere Vorgänge auszulösen.

### <a name="simple-string-message"></a>Einfache Zeichenfolgennachricht

Die einfachste Nachricht enthält nur eine Zeichenfolge im `message`-Parameter. Eine `Subscribe`-Methode, die nach einfachen Zeichenfolgennachrichten *lauscht*, wird unten gezeigt. Beachten Sie den generischen Typ, der die Erwartung angibt, dass der Sender vom Typ `MainPage` ist. Alle Klassen in der Projektmappe können die Nachricht mit der folgenden Syntax abonnieren:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

In der `MainPage`-Klasse *sendet* der folgende Code die Nachricht. Der `this`-Parameter ist eine Instanz von `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

Die Zeichenfolge ändert sich nicht: gibt den *Nachrichtentyp* an und wird zur Ermittlung des zu benachrichtigenden Abonnenten verwendet. Diese Art Nachricht wird verwendet, um anzugeben, dass ein bestimmtes Ereignis aufgetreten ist (z. B. „Upload abgeschlossen“) bei dem keine weiteren Informationen benötigt werden.

### <a name="passing-an-argument"></a>Übergeben von Argumenten

Wenn Sie ein Argument mit der Nachricht übergeben möchten, geben Sie den Argumenttyp in den generischen `Subscribe`-Argumenten und in der Signatur der Aktion an.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

Wenn Sie die Nachricht mit Argument senden möchten, schließen Sie den generischen Typparameter und den Wert des Arguments im Aufruf der `Send`-Methode ein.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

In diesem einfachen Beispiel wird zwar ein `string`-Argument verwendet, es kann jedoch jedes beliebige C#-Objekt übergeben werden.

### <a name="unsubscribe"></a>Abonnement kündigen

Ein Objekt kann eine Nachrichtensignatur abbestellen, so dass keine weiteren Nachrichten mehr gesendet werden. Die Syntax der `Unsubscribe`-Methode sollte die Signatur der Nachricht reflektieren (möglicherweise müssen Sie den generischen Typparameter für das Nachrichtenargument einschließen).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

MessagingCenter ist ein einfacher Weg, Kopplung zu reduzieren, besonders zwischen Anzeigemodellen. Es kann verwendet werden zum Senden und Empfangen einfacher Nachrichten oder zum Übergeben eines Arguments zwischen Klassen. Klassen sollten Nachrichten abbestellen, die sie nicht länger empfangen möchten.


## <a name="related-links"></a>Verwandte Links

- [MessagingCenterSample (MessagingCenter – Beispiel)](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
