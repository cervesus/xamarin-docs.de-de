---
title: Kommunikation zwischen lose gekoppelten Komponenten
description: 'In diesem Kapitel wird erläutert, wie die eShopOnContainers-mobile-app veröffentlichen implementiert-abonnieren-Muster, ermöglicht die nachrichtenbasierte Kommunikation zwischen Komponenten, die unpraktisch, die vom Objekt und Typverweise verknüpfen '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ddc33d28aad4e00c9259893c0f8e7a1ab40ee429
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998543"
---
# <a name="communicating-between-loosely-coupled-components"></a>Kommunikation zwischen lose gekoppelten Komponenten

Der Veröffentlichen-Abonnieren Muster ist eine messaging-Muster in der senden Herausgeber Nachrichten ohne Kenntnisse in jedem Empfänger, bekannt als Abonnenten. Auf ähnliche Weise Lauschen Abonnenten bestimmte Nachrichten, ohne Wissen von keinem Verleger.

Ereignisse in .NET Implementieren der Publish-subscribe-Muster und sind die meisten einfach und unkompliziert Ansatz für eine Kommunikationsebene zwischen Komponenten, wenn die loser Kopplung ist nicht erforderlich, z. B. ein Steuerelement und die Seite, die sie enthält. Allerdings sind der Verleger und Abonnent Lebensdauer von Objektverweisen miteinander gekoppelt, und welche Abonnenten benötigen einen Verweis auf den Typ des Verlegers. Dadurch kann Speicher Probleme, erstellen, besonders wenn es kurzlebige Objekte, die ein Ereignis eines Objekts statisch oder langlebige abonnieren. Wenn der Ereignishandler nicht entfernt werden, der Abonnenten wird aktiv bleiben durch den Verweis darauf in der Verleger und wird dies zu verhindern oder verzögert die Garbagecollection des Abonnenten.

## <a name="introduction-to-messagingcenter"></a>Einführung in die MessagingCenter

Der Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) -Klasse implementiert, das Veröffentlichen-Abonnieren-Muster, die nachrichtenbasierte Kommunikation zwischen Komponenten, die unpraktisch, die vom Objekt und Typverweise verknüpfen können. Dieser Mechanismus ermöglicht es, Verlegern und Abonnenten zu kommunizieren, ohne einen Verweis auf, beim Reduzieren von Abhängigkeiten zwischen Komponenten, gleichzeitig Komponenten unabhängig voneinander entwickelt und getestet werden können.

Die [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse bietet Multicast Veröffentlichen / Abonnieren-Funktionen. Dies bedeutet, dass es können mehrere Verleger, die eine einzelne Nachricht veröffentlichen, und können mehrere Abonnenten warten auf die gleiche Nachricht vorhanden sein. Abbildung 4-1 veranschaulicht diese Beziehung:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multicast Veröffentlichen / Abonnieren-Funktionen")

**Abbildung 4-1:** Multicast Veröffentlichen / Abonnieren-Funktionen

Senden Herausgeber Nachrichten, die mit der [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) -Methode, während Abonnenten Lauschen auf Nachrichten, die mit der [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) Methode. Darüber hinaus Abonnenten können auch abbestellen Nachrichtenabonnements, falls erforderlich, mit der [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) Methode.

Intern wird die [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse verwendet, schwache Verweise. Dies bedeutet, dass sie nicht Objekte aktiv bleiben und erlaubt die Garbage Collection bereinigt werden. Aus diesem Grund sollte es nur erforderlich sein, eine Nachricht kündigen, wenn eine Klasse nicht mehr wünscht, dass zum Empfangen der Nachricht.

Die eShopOnContainers-mobile app verwendet die [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse für die Kommunikation zwischen lose gekoppelten Komponenten. Die app definiert drei Nachrichten:

-   Die `AddProduct` Nachricht wird veröffentlicht, durch die `CatalogViewModel` Klasse, wenn ein Element zum Einkaufswagen hinzugefügt wird. Im Gegenzug die `BasketViewModel` Klasse abonniert die Nachricht und erhöht die Anzahl der Elemente in den Warenkorb gelegt Reaktion. Darüber hinaus die `BasketViewModel` Klasse auch kündigt das Abonnement aus dieser Nachricht.
-   Die `Filter` Nachricht wird veröffentlicht, durch die `CatalogViewModel` Klasse, wenn der Benutzer einen Marke oder Ihr Typ Filter aus dem Katalog angezeigten Elemente angewendet wird. Im Gegenzug die `CatalogView` -Klasse, die Nachricht abonniert, und aktualisiert die Benutzeroberfläche, sodass nur Elemente, die den Filterkriterien entsprechen angezeigt werden.
-   Die `ChangeTab` Nachricht wird veröffentlicht, durch die `MainViewModel` Klasse an, wenn die `CheckoutViewModel` navigiert zu der `MainViewModel` nach der erfolgreichen Erstellung und Übermittlung einer neuen Bestellung. Im Gegenzug die `MainView` Klasse abonniert die Nachricht und die Updates der Benutzeroberfläche also, die die **Mein Profil** Registerkarte aktiv ist, um des Benutzers Bestellungen anzuzeigen.

> [!NOTE]
> Während der [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) Klasse ermöglicht die Kommunikation zwischen lose gekoppelten Klassen, die er bietet nicht nur die Lösung für dieses Problem. Beispielsweise kann die Kommunikation zwischen einem View Model und eine Ansicht auch durch die Bindungs-Engine und über Benachrichtigungen über eigenschaftsänderungen erreicht werden. Kommunikation zwischen zwei Ansichtsmodelle kann darüber hinaus auch erreicht werden, indem Daten während der Navigation übergeben.

In der mobilen app "eshoponcontainers"[ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) dient zum Aktualisieren der Benutzeroberfläche als Reaktion auf eine Aktion, die in einer anderen Klasse auftreten. Aus diesem Grund werden die Nachrichten mit Abonnenten empfangen der Nachricht auf dem gleichen Thread der UI-Thread veröffentlicht.

> [!TIP]
> Beim Ausführen von UI-updates zum UI-Thread zu marshallen. Wenn eine Nachricht, die aus einem Hintergrundthread gesendet wird zur Aktualisierung der Benutzeroberfläche erforderlich ist, verarbeitet die Nachricht im UI-Thread auf dem Abonnenten durch Aufrufen der [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) Methode.

Weitere Informationen zu [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), finden Sie unter [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definieren einer Meldung

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) Nachrichten sind Zeichenfolgen, die verwendet werden, um Nachrichten zu identifizieren. Das folgende Codebeispiel zeigt die Nachrichten in der mobilen app für "eshoponcontainers" definiert:

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

In diesem Beispiel werden Nachrichten mithilfe von Konstanten definiert. Der Vorteil dieses Ansatzes besteht darin, dass es während der Kompilierung typsicherheit und refactoring-Unterstützung.

## <a name="publishing-a-message"></a>Veröffentlichen einer Nachricht

Herausgeber benachrichtigen Abonnenten einer Nachricht mit einem der [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Überladungen. Im folgenden Codebeispiel wird veranschaulicht, Veröffentlichung der `AddProduct` Nachricht:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

In diesem Beispiel die [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Methode gibt drei Argumente:

-   Das erste Argument gibt an, das Absender-Klasse. Durch Abonnenten, die die Nachricht empfangen möchten, muss die Sender-Klasse angegeben werden.
-   Das zweite Argument gibt die Meldung.
-   Das dritte Argument gibt an, die Nutzlastdaten, die auf den Abonnenten gesendet werden. In diesem Fall die Nutzlastdaten ist eine `CatalogItem` Instanz.

Die [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Veröffentlichungsmethode wird die Nachricht und die Nutzlastdaten, die mit einem Fire-and-forget-Ansatz. Aus diesem Grund wird die Nachricht gesendet, auch wenn keine registrierten, sodass die Nachricht erhalten Abonnenten vorhanden sind. In diesem Fall wird die gesendete Nachricht ignoriert.

> [!NOTE]
> Die [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) Methode kann generische Parameter verwenden, um die steuern, wie Nachrichten übermittelt werden. Aus diesem Grund können mehrere Nachrichten, die eine Nachrichtenidentität Teilen, aber Daten für verschiedene Nutzlasttypen zu senden, von anderen Abonnenten empfangen werden.

## <a name="subscribing-to-a-message"></a>Eine Nachricht abonnieren

Abonnenten können zum Empfangen einer Meldung, die mit einer der Registrieren der [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) Überladungen. Im folgenden Codebeispiel wird veranschaulicht, wie die "eshoponcontainers" mobile app abonniert und verarbeitet, die `AddProduct` Nachricht:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

In diesem Beispiel die [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) Methode abonniert das `AddProduct` Nachricht und führt einen Callback-Delegaten als Reaktion auf den Empfang der Nachricht. Diese Callback-Delegaten, der als ein Lambda-Ausdruck angegeben führt Code aus, die die Benutzeroberfläche aktualisiert.

> [!TIP]
> Erwägen Sie die Verwendung von Nutzlastdaten mit unveränderlicher. Versuchen Sie nicht die Nutzlastdaten innerhalb einer Callback-Delegaten zu ändern, da mehrere Threads können gleichzeitig auf die empfangenen Daten zugreifen, werden. In diesem Fall sollte die Nutzlastdaten Parallelitätsfehler vermeiden unveränderlich sein.

Ein Abonnent müssen nicht jede Instanz einer veröffentlichten Nachricht zu verarbeiten, und dies kann gesteuert werden, indem Sie die generischen Typargumente, die auf angegeben werden die [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) Methode. In diesem Beispiel die Abonnenten erhalten nur `AddProduct` von gesendeten Nachrichten der `CatalogViewModel` -Klasse, die Nutzlastdaten, dessen ist eine `CatalogItem` Instanz.

## <a name="unsubscribing-from-a-message"></a>Kündigen des Abonnements aus einer Nachricht

Abonnenten können von Nachrichten wieder abmelden, die sie nicht mehr erhalten möchten. Dies erfolgt mit einem der [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) überlädt, wie im folgenden Codebeispiel gezeigt:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

In diesem Beispiel die [ `Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) Methodensyntax spiegelt wider, die Typargumente angegeben wird, abonnieren Sie den Empfang der `AddProduct` Nachricht.

## <a name="summary"></a>Zusammenfassung

Der Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) -Klasse implementiert, das Veröffentlichen-Abonnieren-Muster, die nachrichtenbasierte Kommunikation zwischen Komponenten, die unpraktisch, die vom Objekt und Typverweise verknüpfen können. Dieser Mechanismus ermöglicht es, Verlegern und Abonnenten zu kommunizieren, ohne einen Verweis auf, beim Reduzieren von Abhängigkeiten zwischen Komponenten, gleichzeitig Komponenten unabhängig voneinander entwickelt und getestet werden können.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
