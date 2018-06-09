---
title: Xamarin.Forms MessagingCenter
description: Dieser Artikel beschreibt, wie Sie das Xamarin.Forms-MessagingCenter zum Senden und Empfangen von Nachrichten, zum Verringern der Kopplung zwischen Klassen, z. B. Modelle anzeigen.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 71f526f87a2536110a6d2292cd66a0f4d81c0bfc
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240336"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms enthält einen einfachen messaging-Dienst zum Senden und Empfangen von Nachrichten._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Xamarin.Forms `MessagingCenter` ermöglicht das anzeigen, Modelle und andere Komponenten mit kommunizieren, ohne Informationen über miteinander neben einem einfachen Nachrichtenvertrag kennen zu müssen.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Wie funktioniert die MessagingCenter

Es gibt zwei Komponenten für `MessagingCenter`:

-  **Abonnieren** : Lauschen auf Nachrichten mit einer bestimmten Signatur, und führen Sie eine Aktion aus, wenn sie empfangen werden. Mehrere Abonnenten können dieselbe Nachricht überwacht werden.
-  **Senden von** – veröffentlichen Sie eine Nachricht für den Listener zu reagieren. Wenn keine Listener abonniert haben, wird die Meldung ignoriert.


Die `MessagingService` ist eine statische Klasse mit `Subscribe` und `Send` Methoden, die in der Lösung verwendet werden.

Nachrichten verfügen über eine Zeichenfolge `message` Parameter, der verwendet wird, als Möglichkeit zum *Adresse* Nachrichten. Die `Subscribe` und `Send` Methoden generische Parameter verwenden, um weiter zu steuern, wie Nachrichten übermittelt werden – zwei Nachrichten mit dem gleichen `message` Text, jedoch unterschiedliche generische Typargumente werden nicht an den gleichen Abonnenten übermittelt werden.

Die API für `MessagingCenter` ist einfach:

-  Abonnieren&lt;TSender > (Objekt-Abonnenten, Zeichenfolgennachricht, Aktion&lt;TSender > Rückruf, TSender Quelle = Null)
-  Abonnieren&lt;TSender, TArgs > (Objekt-Abonnenten, Zeichenfolgennachricht, Aktion&lt;TSender, TArgs > Rückruf, TSender Quelle = Null)
-  Senden von&lt;TSender > (TSender Absender, Zeichenfolgennachricht)
-  Senden von&lt;TSender, TArgs > (TSender Absender, Zeichenfolgennachricht, TArgs Args)
-  Kündigen&lt;TSender, TArgs > (Objekt-Abonnenten, Zeichenfolgennachricht)
-  Kündigen&lt;TSender > (Objekt-Abonnenten, Zeichenfolgennachricht)


Diese Methoden werden nachfolgend erläutert.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>Verwenden die MessagingCenter

Nachrichten können aufgrund der Benutzerinteraktion (z. B. eine Schaltfläche klicken), ein Systemereignis (z. B. Ändern des Status-Steuerelemente) oder einige andere Incident (z. B. einen asynchronen Download abschließen) gesendet werden. Abonnenten können überwacht werden, zum Ändern der Darstellung der Benutzeroberfläche, Speichern von Daten oder einen anderen Vorgang ausgelöst.

### <a name="simple-string-message"></a>Einfache Zeichenfolgennachricht

Die einfachste Nachricht enthält bloß eine Zeichenfolge, die in der `message` Parameter. Ein `Subscribe` Methode, die *überwacht* für eine einfache Zeichenfolgennachricht-angezeigt wird, beachten Sie den generischen Typ angeben, wird der Absender Typ erwartet `MainPage`. Alle Klassen in der Lösung können die Nachricht mithilfe dieser Syntax abonnieren:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

In der `MainPage` -Klasse den folgenden Code *sendet* der Nachricht. Die `this` Parameter ist eine Instanz des `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

Die Zeichenfolge nicht ändern – gibt die *Nachrichtentyp* und dient zur Bestimmung der Abonnenten zu benachrichtigen. Diese Art von Nachricht wird verwendet, um anzugeben, dass ein Ereignis aufgetreten ist, z. B. "Upload abgeschlossen", wobei keine weiteren Informationen erforderlich ist.

### <a name="passing-an-argument"></a>Übergeben von Argumenten

Um ein Argument mit der Nachricht zu übergeben, geben Sie das Argument Typ in der `Subscribe` generische Argumente und in der Signatur der Aktion.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

Zum Senden der Nachricht mit dem Argument enthalten, der generischen Typparameter und der Wert des Arguments in den `Send` -Methodenaufruf.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

Dieses einfache Beispiel verwendet eine `string` Argument, aber alle C#-Objekt übergeben werden kann.

### <a name="unsubscribe"></a>Kündigen des Abonnements

Ein Objekt kann eine Nachrichtensignatur kündigen, so, dass keine weiteren Nachrichten übermittelt werden. Die `Unsubscribe` Methodensyntax sollte die Signatur der Nachricht entsprechen (daher müssen eventuell den generischen Typparameter für das Argument für die Nachricht verwenden).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Die MessagingCenter ist eine einfache Möglichkeit zum Verringern der Kopplung, insbesondere zwischen Modelle anzeigen. Es kann zum Senden und Empfangen von Nachrichten einfacher oder übergeben eines Arguments zwischen Klassen verwendet werden. Klassen sollten Nachrichten Abonnement kündigen, die werden nicht mehr empfangen soll.


## <a name="related-links"></a>Verwandte Links

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
