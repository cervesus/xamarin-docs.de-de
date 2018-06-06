---
title: Aktualisieren vorhandene Apps auf einheitliche API
description: Dieses enthält Dokumentenlinks zu verschiedenen Handbüchern, die zum Aktualisieren von Xamarin-Anwendungen für die einheitliche API zu beschreiben. Es wird erläutert, Xamarin.iOS apps Xamarin.Mac apps. Xamarin.Forms-apps, systemeigene Typen in plattformübergreifende apps und Bindung-Projekten.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d09be7b85980e5c5a8eb209dc1b4ff3136c34b3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781630"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Aktualisieren vorhandene Apps auf einheitliche API

> [!IMPORTANT]
> Die Xamarin klassischen-API, die die einheitliche API vorangestellt wird, wurde als veraltet markiert. 
> - Die letzte Version von Xamarin.iOS Unterstützung der klassischen-API (monotouch.dll) wurde Xamarin.iOS 9.10.
> - Xamarin.Mac unterstützt nach wie vor die klassische API, aber es ist nicht mehr aktualisiert. Da er als veraltet ist, sollten Entwickler ihre Anwendungen für die einheitliche API verschieben.

## <a name="how-to-update-your-apps"></a>Gewusst wie: Aktualisieren Sie Ihre Apps

Xamarin University hat ein kostenlos verfügbare Video auf **ein Upgrade für den iOS-einheitliche API**. Besuchen Sie [Xamarin University Blitze Vorlesungen](http://university.xamarin.com/lightninglectures) zu überwachenden!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

Es gibt drei Schritte, um Ihre apps zu aktualisieren:

1. Beheben Sie alle compilerwarnungen in vorhandenem Code, insbesondere im Zusammenhang mit nicht mehr unterstützte APIs.

2. Verwenden Sie das Migrationstool in Visual Studio für Mac integriert, um Ihre Projektdateien und Namespaces zu aktualisieren.

3. Verbleibende Compilerfehler, die im Zusammenhang mit dem neuen Update [64 Typen](~/cross-platform/macios/nativetypes.md) und [andere APIs](~/cross-platform/macios/unified/overview.md#deprecated-typos) geändert wurden. Auschecken [diese Tipps](~/cross-platform/macios/unified/updating-tips.md) für Weitere Informationen zum manuellen Aktualisierungen, die erforderlich sind.

Bestimmte Handbücher sind für jedes Produkt beim Aktualisieren Ihrer apps einheitliche API und der 64-Bit-Unterstützung verfügbar:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md)

Vorhandene Xamarin.iOS-apps können aktualisiert werden, um die einheitliche API, die mit dem automatisierte Migrationstool für Mac Visual Studio erstellt wurden Einige zusätzliche Updates möglicherweise erforderlich, wie in beschrieben [diese Anweisungen](~/cross-platform/macios/unified/updating-ios-apps.md) und [Tipps](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

Vorhandene Xamarin.Mac-apps können aktualisiert werden, um die einheitliche API, die mit dem automatisierte Migrationstool für Mac Visual Studio erstellt wurden Einige zusätzliche Updates möglicherweise erforderlich, wie in beschrieben [diese Anweisungen](~/cross-platform/macios/unified/updating-mac-apps.md) und [Tipps](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Beim Aktualisieren einer vorhandenen Xamarin.Forms-Projektmappe mit der ein iOS-Projekt die Unified-API verwenden, gehen Sie wie folgt vor. Einheitliche API-Unterstützung ist nur verfügbar in Xamarin.Forms 1.3 und höher, damit [die Anweisungen](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) wird auch erläutert, wie Ihre app Xamarin.Forms auf Version 1.3 zu aktualisieren. Diese [Tipps](~/cross-platform/macios/unified/updating-tips.md) kann Ihnen helfen, die alle systemeigenen iOS-Code in benutzerdefinierten Renderer oder Abhängigkeitsdienste aktualisieren.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/nativetypes.md)

Dieser Artikel umfasst, verwenden die neue iOS Unified API systemeigene Typen (Nint, Nuint, Nfloat) in einer plattformübergreifenden-Anwendung, in dem Code mit nicht-iOS-Geräten, z. B. Android oder Windows Phone Betriebssysteme freigegebenen. Einblick in die bei der systemeigenen Typen verwendet werden soll, und bietet mehrere mögliche Lösungen für Fälle, in dem der neue Typ mit plattformübergreifenden Code verwendet werden muss.

## <a name="update-bindings-to-the-unified-api"></a>Aktualisieren Sie die Bindungen auf einheitliche API

Kunden, die Bindungen für Objective-C-Bibliotheken erstellt haben müssen zum Aktualisieren der bindungsprojekt, um die Änderungen in der zugrunde liegenden API entsprechen (wobei einige Typen jetzt 64-Bit-werden).
Führen Sie diese Anweisungen, um [Aktualisieren eines vorhandenen Projekts der Bindung zur Unterstützung der einheitliche API](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualisieren von Apps für Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von Apps mit Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Aktualisieren von Tipps](~/cross-platform/macios/unified/updating-tips.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
