---
title: Kommunikation zwischen lose gekoppelte Komponenten
ms.topic: article
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: e05cd0ec7d03a033e24dcbfb8124cfc2ccfa438e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="communicating-between-loosely-coupled-components"></a>Kommunikation zwischen lose gekoppelte Komponenten

Das Veröffentlichen-Abonnieren Muster ist ein messaging-Muster in der Herausgeber Senden von Nachrichten ohne Kenntnis alle Empfänger, bekannt als Abonnenten. Auf ähnliche Weise Lauschen Abonnenten bestimmte Nachrichten ohne Kenntnisse über keine Verleger.

Ereignisse in .NET implementieren Veröffentlichen-Abonnieren-Muster und sind die meisten einfach und unkompliziert Ansatz für eine Kommunikationsebene zwischen Komponenten, wenn lose Kopplung ist nicht erforderlich, z. B. ein Steuerelement und die Seite, die es enthält. Allerdings sind der Verleger und Abonnent Lebensdauer von Objektverweisen miteinander verbunden, und welche Abonnenten benötigen einen Verweis auf den Typ des Verlegers. Dadurch kann Speicher Problemen, erstellt, insbesondere, wenn kurzlebigere Objekte, die ein Ereignis eines Objekts statisch oder langlebige abonnieren. Wenn der Ereignishandler entfernt wird, der Abonnenten wird bereitgehalten werden durch den Verweis darauf auf dem Verleger, und dies verhindern, dass oder verzögert die Garbagecollection des Abonnenten.

## <a name="introduction-to-messagingcenter"></a>Einführung in MessagingCenter

Der Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse implementiert die Veröffentlichen-Abonnieren-Muster, sodass meldungsbasierte Kommunikation zwischen Komponenten, die unpraktisch, Objekt und Typverweise verknüpft sind. Dieser Mechanismus erlaubt Verlegern und Abonnenten zu kommunizieren, ohne einen Verweis zu "other" und helfen, reduzieren die Abhängigkeiten zwischen Komponenten, und lässt außerdem Komponenten unabhängig entwickelt und getestet werden.

Die [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse bietet Multicast Veröffentlichen / Abonnieren-Funktionalität. Dies bedeutet, dass mehrere Verleger, die eine einzelne Nachricht veröffentlichen kann, und es können mehrere Abonnenten, die für die gleiche Nachricht überwacht werden. Abbildung 4 – 1 wird diese Beziehung veranschaulicht:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multicast Veröffentlichen / Abonnieren-Funktion")

**Abbildung 4 – 1:** Multicast Veröffentlichen / Abonnieren-Funktion

Herausgeber Senden von Nachrichten mithilfe der [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) -Methode, während Abonnenten Abhören von Nachrichten mithilfe der [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) Methode. Darüber hinaus Abonnenten können auch kündigen von Ereignisabonnements Nachrichtenabonnements, falls erforderlich, mit der [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) Methode.

Intern können die [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse verwendet schwache Verweise. Dies bedeutet, dass er nicht Objekte aktiv hält, und lässt sie Garbage Collections durchgeführt werden. Deshalb sollte es nur erforderlich sein, eine Nachricht kündigen, wenn eine Klasse nicht mehr die Meldung empfangen möchte.

Der eShopOnContainers mobile app verwendet die [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse für die Kommunikation zwischen lose gekoppelter Komponenten. Die app definiert drei Nachrichten:

-   Die `AddProduct` Nachricht wird veröffentlicht, durch die `CatalogViewModel` Klasse bei der ein Element in den Warenkorb hinzugefügt wird. Im Gegenzug die `BasketViewModel` Klasse abonniert die Nachricht und erhöht die Anzahl der Elemente in den Warenkorb Reaktion. Darüber hinaus die `BasketViewModel` Klasse auch hebt das Abonnement von aus dieser Nachricht.
-   Die `Filter` Nachricht wird veröffentlicht, durch die `CatalogViewModel` Klasse, wenn der Benutzer eine Marke oder Typ filtern zu den Elementen angezeigt, aus dem Katalog anwendet. Im Gegenzug die `CatalogView` Klasse abonniert die Nachricht und aktualisiert die Benutzeroberfläche, sodass nur Elemente, die den Filterkriterien entsprechen angezeigt werden.
-   Die `ChangeTab` Nachricht wird veröffentlicht, durch die `MainViewModel` Klasse an, wenn die `CheckoutViewModel` navigiert zu der `MainViewModel` nach der erfolgreichen Erstellung und Übermittlung eine neue Bestellung. Im Gegenzug die `MainView` Klasse abonniert die Nachricht und die Updates der Benutzeroberfläche daher, die die **Mein Profil** Registerkarte aktiv ist, um Aufträge des Benutzers anzuzeigen.

> [!NOTE]
> Während der [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse ermöglicht die Kommunikation zwischen lose verknüpfte Klassen, bietet nicht die nur architektonische Lösung für dieses Problem. Beispielsweise kann die Kommunikation zwischen einem Ansichtsmodell und einer Ansicht durch das Bindungsmodul und über änderungsbenachrichtigungen für die Eigenschaft auch erreicht werden. Darüber hinaus kann die Kommunikation zwischen zwei Ansichtsmodelle auch durch das Übergeben von Daten während der Navigation erreicht werden.

In der mobilen app eShopOnContainers[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) dient zum Aktualisieren der Benutzeroberfläche als Antwort auf eine Aktion, die in einer anderen Klasse ausgeführt. Daher werden Nachrichten mit Abonnenten empfangen der Nachricht auf dem gleichen Thread auf den UI-Thread und veröffentlicht.

> [!TIP]
> Beim Ausführen der Benutzeroberfläche aktualisiert zum UI-Thread zu marshallen. Wenn eine Nachricht, die aus einem Hintergrundthread gesendet wird zum Aktualisieren der Benutzeroberfläche erforderlich ist, verarbeitet die Nachricht an den UI-Thread auf dem Abonnenten durch Aufrufen der [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) Methode.

Weitere Informationen zu [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), finden Sie unter [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Definieren einer Meldung

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Nachrichten handelt es sich um Zeichenfolgen, die verwendet werden, um Nachrichten zu identifizieren. Im folgenden Codebeispiel wird veranschaulicht, die Nachrichten in der mobilen Anwendung für eShopOnContainers definiert:

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

In diesem Beispiel werden Nachrichten mithilfe von Konstanten definiert. Der Vorteil dieses Ansatzes besteht darin, dass es während der Kompilierung typsicherheit und umgestaltungsunterstützung.

## <a name="publishing-a-message"></a>Veröffentlichen einer Nachricht

Herausgeber benachrichtigen Abonnenten einer Nachricht mit einem der [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Überladungen. Im folgenden Codebeispiel wird veranschaulicht, Veröffentlichung der `AddProduct` Nachricht:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

In diesem Beispiel wird die [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Methode gibt drei Argumente:

-   Das erste Argument gibt die Absender-Klasse. Die Absender-Klasse muss durch Abonnenten angegeben werden, die die Nachricht empfangen möchten.
-   Das zweite Argument gibt die Meldung an.
-   Das dritte Argument gibt die Nutzlastdaten an den Abonnenten gesendet werden. In diesem Fall die Nutzlastdaten ist eine `CatalogItem` Instanz.

Die [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Methode veröffentlicht die Nachricht und dessen Nutzlastdaten mithilfe des Ansatzes auslösen und vergessen. Aus diesem Grund wird die Nachricht gesendet, auch wenn keine Abonnenten registriert zum Empfangen der Nachricht vorhanden sind. In diesem Fall wird die gesendete Nachricht ignoriert.

> [!NOTE]
> Die [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Methode können generische Parameter steuern, wie Nachrichten übermittelt werden. Aus diesem Grund können mehrere Nachrichten, die eine Nachrichtenidentität freigeben, aber Senden von Daten für verschiedene Nutzlasttypen durch verschiedene Abonnenten empfangen werden.

## <a name="subscribing-to-a-message"></a>Eine Nachricht abonnieren

Abonnenten können den Empfang eine Nachricht mit einer der Registrieren der [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) Überladungen. Im folgenden Codebeispiel wird veranschaulicht, wie die mobilen Anwendung für eShopOnContainers abonniert und Prozesse, die `AddProduct` Nachricht:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

In diesem Beispiel wird die [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) Methode abonniert das `AddProduct` Nachricht und führt einen Rückrufdelegaten als Antwort auf den Empfang der Nachricht. Dieser Rückrufdelegat, angegeben als Lambda-Ausdruck führt Code, der die Benutzeroberfläche aktualisiert wird.

> [!TIP]
> Unveränderliche Nutzlastdaten in Betracht. Versuchen Sie nicht die Nutzlastdaten innerhalb einer Rückrufdelegat geändert werden, da mehrere Threads können gleichzeitig auf die empfangenen Daten zugreifen, werden. In diesem Szenario muss die Nutzlastdaten unveränderlichen Parallelitätsfehler zu vermeiden.

Ein Abonnent müssen nicht jede Instanz einer veröffentlichten Nachricht zu verarbeiten, und dies kann gesteuert werden, indem die generischen Typargumente, die auf angegeben sind die [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) Methode. In diesem Beispiel nur die Abonnenten empfangen wird `AddProduct` Nachrichten aus der `CatalogViewModel` -Klasse, deren Nutzlastdaten hat eine `CatalogItem` Instanz.

## <a name="unsubscribing-from-a-message"></a>Kündigen des Abonnements aus einer Nachricht

Abonnenten können Nachrichten Abonnement zu kündigen, die nicht mehr empfangen werden soll. Dies erfolgt mit einem der [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) überlädt, wie im folgenden Codebeispiel gezeigt:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

In diesem Beispiel wird die [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) Methodensyntax widerspiegelt, die Typargumente, die beim Empfang abonnieren angegeben die `AddProduct` Nachricht.

## <a name="summary"></a>Zusammenfassung

Der Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse implementiert die Veröffentlichen-Abonnieren-Muster, sodass meldungsbasierte Kommunikation zwischen Komponenten, die unpraktisch, Objekt und Typverweise verknüpft sind. Dieser Mechanismus erlaubt Verlegern und Abonnenten zu kommunizieren, ohne einen Verweis zu "other" und helfen, reduzieren die Abhängigkeiten zwischen Komponenten, und lässt außerdem Komponenten unabhängig entwickelt und getestet werden.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
