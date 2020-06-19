---
title: Kommunikation zwischen lose gekoppelten Komponenten
description: 'In diesem Kapitel wird erläutert, wie die eshoponcontainers-Mobile App das Veröffentlichen-Abonnieren-Muster implementiert, sodass Nachrichten basierte Kommunikation zwischen Komponenten ermöglicht wird, die für die Verknüpfung durch Objekt-und Typverweise ungeeignet sind. '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c35cd6e30e7843cda0431581025aa7440a21cc29
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140048"
---
# <a name="communicating-between-loosely-coupled-components"></a>Kommunikation zwischen lose gekoppelten Komponenten

Das Veröffentlichen-Abonnieren-Muster ist ein Messagingmuster, bei dem Herausgeber Nachrichten senden, ohne über Kenntnisse zu Empfängern zu verfügen, die als Abonnenten bezeichnet werden. Auf ähnliche Weise lauschen Abonnenten auf bestimmte Nachrichten, ohne dass sie über Kenntnisse zu Herausgebern verfügen.

Ereignisse in .NET implementieren das Veröffentlichen-Abonnieren-Muster und sind der einfachste und unkomplizierteste Ansatz für eine Kommunikationsschicht zwischen Komponenten, wenn keine lose Kopplung erforderlich ist, wie beispielsweise ein Steuerelement und die Seite, die es enthält. Die Lebensdauer von Herausgeber und Abonnent sind jedoch durch Objektverweise miteinander gekoppelt, und der Abonnententyp muss einen Verweis auf den Herausgebertypen haben. Dies kann zu Problemen bei der Speicherverwaltung führen, insbesondere wenn es kurzlebige Objekte gibt, die ein Ereignis eines statischen oder langlebigen Objekts abonnieren. Wenn der Ereignishandler nicht entfernt wird, bleibt der Abonnent durch den Verweis darauf im Herausgeber erhalten, was die Garbage Collection des Abonnenten verhindert oder verzögert.

## <a name="introduction-to-messagingcenter"></a>Einführung in messagingcenter

Die Xamarin.Forms-Klasse [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) implementiert das Veröffentlichen-Abonnieren-Muster und ermöglicht so eine nachrichtenbasierte Kommunikation zwischen Komponenten, für die eine Verknüpfung über Objekt- und Typverweise ungünstig ist. Dieser Mechanismus ermöglicht es Verlegern und Abonnenten, ohne einen Verweis aufeinander zu kommunizieren, sodass Abhängigkeiten zwischen Komponenten reduziert werden können, während Komponenten unabhängig entwickelt und getestet werden können.

Die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)-Klasse bietet Multicast-Funktionen zum Veröffentlichen und Abonnieren. Dies bedeutet, dass mehrere Verleger vorhanden sein können, die eine einzelne Nachricht veröffentlichen, und es kann mehrere Abonnenten geben, die dieselbe Nachricht überwachen. In Abbildung 4-1 wird diese Beziehung veranschaulicht:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multicast publish-subscribe functionality")

**Abbildung 4-1:** Multicast Veröffentlichung: Abonnieren von Funktionen

Herausgeber senden Nachrichten mithilfe der [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Methode, während Abonnenten mithilfe der [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Methode auf Nachrichten lauschen. Darüber hinaus können Abonnenten bei Bedarf mit der [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)-Methode auch Nachrichtenabonnements kündigen.

Intern verwendet die [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)-Klasse schwache Verweise. Dies bedeutet, dass Objekte nicht aktiv bleiben und Garbage Collection ermöglicht wird. Daher sollte es nur erforderlich sein, eine Nachricht zu kündigen, wenn eine Klasse die Nachricht nicht mehr empfangen möchte.

Der eshoponcontainers-Mobile App verwendet die- [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) Klasse, um zwischen lose gekoppelten Komponenten zu kommunizieren. Die APP definiert drei Nachrichten:

- Die `AddProduct` Meldung wird von der- `CatalogViewModel` Klasse veröffentlicht, wenn dem Warenkorb ein Element hinzugefügt wird. In der Rückgabe `BasketViewModel` abonniert die-Klasse die Nachricht und erhöht die Anzahl der Elemente im Warenkorb als Antwort. Außerdem wird von der- `BasketViewModel` Klasse auch die abonnierten dieser Nachricht abonniert.
- Die `Filter` Meldung wird von der- `CatalogViewModel` Klasse veröffentlicht, wenn der Benutzer einen Marken-oder Typfilter auf die Elemente anwendet, die aus dem Katalog angezeigt werden. In der Rückgabe `CatalogView` abonniert die-Klasse die Nachricht und aktualisiert die Benutzeroberfläche, sodass nur Elemente angezeigt werden, die den Filterkriterien entsprechen.
- Die `ChangeTab` Meldung wird von der- `MainViewModel` Klasse veröffentlicht, wenn der `CheckoutViewModel` zu der `MainViewModel` folgenden erfolgreichen Erstellung und Übermittlung einer neuen Bestellung navigiert. In der Rückgabe `MainView` abonniert die-Klasse die Nachricht und aktualisiert die Benutzeroberfläche, sodass die Registerkarte **mein Profil** aktiv ist, um die Bestellungen des Benutzers anzuzeigen.

> [!NOTE]
> Obwohl die- [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) Klasse die Kommunikation zwischen lose gekoppelten Klassen zulässt, bietet Sie keine einzige architektonische Lösung für dieses Problem. Beispielsweise kann die Kommunikation zwischen einem Ansichts Modell und einer Ansicht auch durch die Bindungs-Engine und durch Benachrichtigungen über Eigenschafts Änderungen erreicht werden. Darüber hinaus kann die Kommunikation zwischen zwei Ansichts Modellen auch durch das Übergeben von Daten während der Navigation erreicht werden.

Im Mobile App eshoponcontainers wird in [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) der Benutzeroberfläche als Reaktion auf eine Aktion, die in einer anderen Klasse ausgeführt wird, verwendet. Daher werden Nachrichten im UI-Thread veröffentlicht, wobei die Abonnenten die Nachricht im gleichen Thread empfangen.

> [!TIP]
> Beim Ausführen von UI-Aktualisierungen zum UI-Thread marshallt. Wenn eine Nachricht, die von einem Hintergrund Thread gesendet wird, zum Aktualisieren der Benutzeroberfläche erforderlich ist, verarbeiten Sie die Nachricht im UI-Thread des Abonnenten, indem Sie die- [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) Methode aufrufen.

Weitere Informationen zu [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) finden Sie unter [messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definieren einer Nachricht

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)Nachrichten sind Zeichen folgen, die zum Identifizieren von Nachrichten verwendet werden. Das folgende Codebeispiel zeigt die Nachrichten, die innerhalb der eshoponcontainers-Mobile App definiert sind:

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

In diesem Beispiel werden Nachrichten mithilfe von Konstanten definiert. Der Vorteil dieses Ansatzes besteht darin, dass er die Typsicherheit für die Kompilierzeit und Refactoring unterstützt.

## <a name="publishing-a-message"></a>Veröffentlichen einer Nachricht

Herausgeber benachrichtigen Abonnenten einer Nachricht mit einer der [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*)-Überladungen. Im folgenden Codebeispiel wird das Veröffentlichen der `AddProduct` Nachricht veranschaulicht:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

In diesem Beispiel gibt die- [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) Methode drei Argumente an:

- Das erste Argument gibt die Sender-Klasse an. Die Absender Klasse muss von allen Abonnenten angegeben werden, die die Nachricht empfangen möchten.
- Das zweite Argument gibt die Nachricht an.
- Das dritte Argument gibt die Nutzlastdaten an, die an den Abonnenten gesendet werden sollen. In diesem Fall ist die Nutzlastdaten eine- `CatalogItem` Instanz.

[`Send`](xref:Xamarin.Forms.MessagingCenter.Send*)Mit der-Methode wird die Nachricht und deren Nutzlastdaten mithilfe eines Fire-and-Forget-Ansatzes veröffentlicht. Daher wird die Nachricht auch dann gesendet, wenn für den Empfang der Nachricht keine Abonnenten registriert sind. Unter diesen Umständen wird die gesendete Nachricht ignoriert.

> [!NOTE]
> Die- [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) Methode kann generische Parameter verwenden, um zu steuern, wie Nachrichten übermittelt werden. Daher können mehrere Nachrichten, die eine Nachrichten Identität gemeinsam nutzen, aber unterschiedliche Nutz Last Datentypen senden, von verschiedenen Abonnenten empfangen werden.

## <a name="subscribing-to-a-message"></a>Abonnieren einer Nachricht

Abonnenten können sich registrieren, um eine Nachricht mit einer der [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Überladungen zu empfangen. Im folgenden Codebeispiel wird veranschaulicht, wie der eshoponcontainers-Mobile App die Nachricht abonniert und verarbeitet `AddProduct` :

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

In diesem Beispiel [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) abonniert die-Methode die `AddProduct` Meldung und führt als Antwort auf den Empfang der Nachricht einen Rückruf Delegaten aus. Dieser Rückruf Delegat, der als Lambda-Ausdruck angegeben wird, führt Code aus, der die Benutzeroberfläche aktualisiert.

> [!TIP]
> Verwenden Sie ggf. unveränderliche Nutzlastdaten. Versuchen Sie nicht, die Nutzlastdaten innerhalb eines Rückruf Delegaten zu ändern, da mehrere Threads gleichzeitig auf die empfangenen Daten zugreifen können. In diesem Szenario sollten die Nutzlastdaten unveränderlich sein, um Parallelitäts Fehler zu vermeiden.

Ein Abonnent muss möglicherweise nicht jede Instanz einer veröffentlichten Nachricht verarbeiten, und dies kann durch die generischen Typargumente gesteuert werden, die für die [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)-Methode angegeben werden. In diesem Beispiel empfängt der Abonnent nur `AddProduct` Nachrichten, die von der-Klasse gesendet werden `CatalogViewModel` , deren Nutzlastdaten eine- `CatalogItem` Instanz ist.

## <a name="unsubscribing-from-a-message"></a>Aufheben des Abonnements für eine Nachricht

Abonnenten können Nachrichten kündigen, die Sie nicht mehr erhalten möchten. Dies wird mit einer der- [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) über Ladungen erreicht, wie im folgenden Codebeispiel gezeigt:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

In diesem Beispiel gibt die [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) Methoden Syntax die Typargumente wieder, die beim Abonnieren der `AddProduct` Nachricht angegeben werden.

## <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms-Klasse [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) implementiert das Veröffentlichen-Abonnieren-Muster und ermöglicht so eine nachrichtenbasierte Kommunikation zwischen Komponenten, für die eine Verknüpfung über Objekt- und Typverweise ungünstig ist. Dieser Mechanismus ermöglicht es Verlegern und Abonnenten, ohne einen Verweis aufeinander zu kommunizieren, sodass Abhängigkeiten zwischen Komponenten reduziert werden können, während Komponenten unabhängig entwickelt und getestet werden können.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
