---
title: Xamarin.Forms MessagingCenter
description: Die Xamarin.Forms-Klasse MessagingCenter implementiert das Veröffentlichen-Abonnieren-Muster und ermöglicht so eine nachrichtenbasierte Kommunikation zwischen Komponenten, für die eine Verknüpfung über Objekt- und Typverweise ungünstig ist.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2019
ms.openlocfilehash: a4d246419c7449c2395759cf5a8b04469e7a2309
ms.sourcegitcommit: 266e75fa6893d3732e4e2c0c8e79c62be2804468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68821010"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)

Das Veröffentlichen-Abonnieren-Muster ist ein Messagingmuster, bei dem Herausgeber Nachrichten senden, ohne über Kenntnisse zu Empfängern zu verfügen, die als Abonnenten bezeichnet werden. Auf ähnliche Weise lauschen Abonnenten auf bestimmte Nachrichten, ohne dass sie über Kenntnisse zu Herausgebern verfügen.

Die Xamarin.Forms-Klasse [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) implementiert das Veröffentlichen-Abonnieren-Muster und ermöglicht so eine nachrichtenbasierte Kommunikation zwischen Komponenten, für die eine Verknüpfung über Objekt- und Typverweise ungünstig ist. Dieser Mechanismus ermöglicht es Herausgebern und Abonnenten, ohne einen Verweis aufeinander zu kommunizieren, und trägt dazu bei, Abhängigkeiten zwischen ihnen zu verringern.

Die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)-Klasse bietet Multicast-Funktionen zum Veröffentlichen und Abonnieren. Dies bedeutet, dass mehrere Herausgeber vorhanden sein können, die eine einzelne Nachricht veröffentlichen, und es können mehrere Abonnenten vorhanden sein, die auf dieselbe Nachricht lauschen:

![](messaging-center-images/messaging-center.png "Multicast-Funktionen zum Veröffentlichen und Abonnieren")

Herausgeber senden Nachrichten mithilfe der [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Methode, während Abonnenten mithilfe der [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Methode auf Nachrichten lauschen. Darüber hinaus können Abonnenten bei Bedarf mit der [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)-Methode auch Nachrichtenabonnements kündigen.

> [!IMPORTANT]
> Intern verwendet die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)-Klasse schwache Verweise. Dies bedeutet, dass Objekte nicht aktiv bleiben und Garbage Collection ermöglicht wird. Daher sollte es nur erforderlich sein, eine Nachricht zu kündigen, wenn eine Klasse die Nachricht nicht mehr empfangen möchte.

## <a name="publish-a-message"></a>Veröffentlichen einer Nachricht

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)-Nachrichten sind Zeichenfolgen. Herausgeber benachrichtigen Abonnenten einer Nachricht mit einer der [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Überladungen. Im folgenden Codebeispiel wird eine `Hi`-Nachricht veröffentlicht:

```csharp
MessagingCenter.Send<MainPage>(this, "Hi");
```

In diesem Beispiel gibt die [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Methode ein generisches Argument an, das den Absender darstellt. Zum Empfangen der Nachricht muss ein Abonnent das gleiche generische Argument angeben, das anzeigt, dass er auf eine Nachricht von diesem Absender lauscht. Außerdem gibt dieses Beispiel zwei Methodenargumente an:

- Mit dem ersten Argument wird die Absenderinstanz angegeben.
- Das zweite Argument gibt die Nachricht an.

Nutzlastdaten können ebenfalls mit einer Nachricht gesendet werden:

```csharp
MessagingCenter.Send<MainPage, string>(this, "Hi", "John");
```

In diesem Beispiel gibt die [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Methode zwei generische Argumente an. Das erste Argument ist der Typ, der die Nachricht sendet, und das zweite Argument gibt den Typ der zu sendenden Nutzlastdaten an. Zum Empfangen der Nachricht muss ein Abonnent dieselben generischen Argumente angeben. Dies ermöglicht mehrere Nachrichten, die eine Nachrichtenidentität gemeinsam nutzen, aber unterschiedliche Nutzlastdatentypen senden, die von verschiedenen Abonnenten empfangen werden. Außerdem gibt dieses Beispiel ein drittes Methodenargument an, das die Nutzlastdaten darstellt, die an den Abonnenten gesendet werden sollen. In diesem Fall handelt es sich bei den Nutzlastdaten um ein `string`-Element.

Die [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Methode veröffentlicht die Nachricht und alle Nutzlastdaten unter Verwendung eines Fire-and-Forget-Ansatzes. Daher wird die Nachricht auch dann gesendet, wenn für den Empfang der Nachricht keine Abonnenten registriert sind. Unter diesen Umständen wird die gesendete Nachricht ignoriert.

## <a name="subscribe-to-a-message"></a>Abonnieren einer Nachricht

Abonnenten können sich registrieren, um eine Nachricht mit einer der [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Überladungen zu empfangen. Das folgende Codebeispiel veranschaulicht dieses Verhalten:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) =>
{
    // Do something whenever the "Hi" message is received
});
```

In diesem Beispiel abonniert die [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Methode das `this`-Objekt für `Hi`-Nachrichten, die vom `MainPage`-Typ gesendet werden, und führt als Antwort auf den Empfang der Nachricht einen Rückrufdelegaten aus. Der als Lambdaausdruck angegebene Rückrufdelegat könnte Code sein, der die Benutzeroberfläche aktualisiert, einige Daten speichert oder einen anderen Vorgang auslöst.

> [!NOTE]
> Ein Abonnent muss möglicherweise nicht jede Instanz einer veröffentlichten Nachricht verarbeiten, und dies kann durch die generischen Typargumente gesteuert werden, die für die [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Methode angegeben werden.

Im folgenden Beispiel wird gezeigt, wie eine Nachricht abonniert wird, die Nutzlastdaten enthält:

```csharp
MessagingCenter.Subscribe<MainPage, string>(this, "Hi", async (sender, arg) =>
{
    await DisplayAlert("Message received", "arg=" + arg, "OK");
});
```

In diesem Beispiel abonniert die [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Methode `Hi`-Nachrichten, die vom `MainPage`-Typ gesendet werden, dessen Nutzlastdaten ein `string`-Objekt sind. Ein Rückrufdelegat wird als Reaktion auf den Empfang einer solchen Nachricht ausgeführt, der die Nutzlastdaten in einer Benachrichtigung anzeigt.

## <a name="unsubscribe-from-a-message"></a>Kündigen des Abonnements einer Nachricht

Abonnenten können Nachrichten kündigen, die Sie nicht mehr erhalten möchten. Dies wird mit einer der [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)-Überladungen erreicht:

```csharp
MessagingCenter.Unsubscribe<MainPage>(this, "Hi");
```

In diesem Beispiel kündigt die [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)-Methode das Abonnement des `this`-Objekts aus der vom `MainPage`-Typ gesendeten `Hi`-Nachricht.

Das Abonnement von Nachrichten, die Nutzlastdaten enthalten, sollte mithilfe der [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)-Überladung gekündigt werden, die zwei generische Argumente angibt:

```csharp
MessagingCenter.Unsubscribe<MainPage, string>(this, "Hi");
```

In diesem Beispiel kündigt die [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)-Methode das Abonnement des `this`-Objekts aus der vom `MainPage`-Typ gesendeten `Hi`-Nachricht, deren Nutzlastdaten ein `string`-Objekt sind.

## <a name="related-links"></a>Verwandte Links

- [MessagingCenterSample (MessagingCenter – Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)
