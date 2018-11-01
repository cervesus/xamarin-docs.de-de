---
title: Xamarin.Forms MessagingCenter
description: In diesem Artikel wird erläutert, wie Sie mit der Xamarin.Forms-MessagingCenter zum Senden und Empfangen von Nachrichten, die Kopplung zwischen Klassen wie z. B. anzeigemodelle reduzieren.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 7fef4443cacba0fa8bdb8d5df070c4244730b4f5
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675174"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms umfasst einen einfachen Messagingdienst zum Senden und Empfangen von Nachrichten._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Xamarin.Forms `MessagingCenter` ermöglicht das anzeigen, Modelle und andere Komponenten für die Kommunikation mit ohne nichts über den anderen neben einem einfachen Nachrichtenvertrag kennen zu müssen.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Wie funktioniert die MessagingCenter

Es gibt zwei Teilen `MessagingCenter`:

-  **Abonnieren** : Lauschen auf Nachrichten mit einer bestimmten Signatur, und führen Sie eine Aktion aus, wenn sie empfangen werden. Mehrere Abonnenten können die gleiche Nachricht überwacht werden.
-  **Senden von** – veröffentlichen eine Nachricht für den Listener zu reagieren. Wenn keine Listener abonniert haben, wird die Meldung ignoriert.


Die `MessagingService` ist eine statische Klasse mit `Subscribe` und `Send` Methoden, die in der Lösung verwendet werden.

Nachrichten müssen Sie eine Zeichenfolge `message` Parameter, der verwendet wird, als Möglichkeit zum *Adresse* Nachrichten. Die `Subscribe` und `Send` Methoden, die generische Parameter verwenden, um weiter zu steuern, wie Nachrichten übermittelt werden – zwei Nachrichten mit der gleichen `message` Text, jedoch eine andere generische Typargumente werden nicht mit dem gleichen Abonnenten übermittelt werden.

Die API für `MessagingCenter` ist einfach:

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

Diese Methoden werden nachfolgend erklärt.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>Verwenden die MessagingCenter

Nachrichten können aufgrund von Benutzerinteraktionen (z.B. eine Schaltfläche klicken), ein Systemereignis (z. B. Steuerelemente, die Status ändern) oder andere Incident (z. B. einen asynchronen Download abgeschlossen) gesendet werden. Abonnenten können überwacht werden, zum Ändern der Darstellung der Benutzeroberfläche, Speichern von Daten oder einen anderen Vorgang ausgelöst.

### <a name="simple-string-message"></a>Einfache Zeichenfolgennachricht

Die einfachste Nachricht enthält nur eine Zeichenfolge in die `message` Parameter. Ein `Subscribe` Methode, die *lauscht* für eine einfache Zeichenfolgennachricht unten wird – Beachten Sie, dass den generischen Typ angeben, wird der Absender des Typs erwartet `MainPage`. Alle Klassen in der Projektmappe können abonnieren, die Nachricht mit der folgenden Syntax:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

In der `MainPage` -Klasse den folgenden Code *sendet* der Nachricht. Die `this` Parameter ist eine Instanz des `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

Die Zeichenfolge nicht ändern – es gibt die *Nachrichtentyp* und dient zum Ermitteln der Abonnenten zu benachrichtigen. Diese Art von Nachricht wird verwendet, um anzugeben, dass ein Ereignis aufgetreten ist, z. B. "Upload abgeschlossen", wobei keine weiteren Informationen erforderlich ist.

### <a name="passing-an-argument"></a>Übergeben von Argumenten

Um ein Argument mit der Nachricht übergeben möchten, geben Sie das Argument Typ in der `Subscribe` generischen Argumente und in der Signatur der Aktion.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

Zum Senden der Nachricht mit dem Argument enthalten, die generischen Typparameter und der Wert des Arguments in den `Send` Methodenaufruf.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

In diesem einfache Beispiel verwendet eine `string` Argument, aber alle C# Objekt übergeben werden.

### <a name="unsubscribe"></a>Das Abonnement kündigen

Ein Objekt kann eine Nachrichtensignatur abbestellen, damit keine zukünftigen Nachrichten übermittelt werden. Die `Unsubscribe` Methodensyntax sollte die Signatur der Nachricht entsprechend festgelegt werden (also möglicherweise müssen Sie die generischen Typparameter für die Message-Argument enthalten).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Die MessagingCenter ist eine einfache Möglichkeit zum Reduzieren der Kopplung zu geraten, insbesondere zwischen anzeigemodelle. Es kann zum Senden und Empfangen von Nachrichten mit einfachen oder ein Argument zu übergeben, zwischen den Klassen verwendet werden. Klassen abbestellen sollten von Nachrichten, die sie nicht mehr erhalten möchten.


## <a name="related-links"></a>Verwandte Links

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
