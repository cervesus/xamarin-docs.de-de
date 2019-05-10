---
title: Aktualisieren von vorhandenen Apps zu Unified API
description: Dieses Dokument enthält Links zu verschiedenen Anleitungen, die beschreiben, wie Sie Xamarin-Anwendungen, die Unified API aktualisieren. Es wird die Xamarin.iOS-apps, Xamarin.Mac-apps erläutert. Xamarin.Forms-apps, systemeigene Typen in plattformübergreifenden apps und bindungsprojekte.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 62b8e82a1191f3213453e9a213ea615e476662d5
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978224"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Aktualisieren von vorhandenen Apps zu Unified API

> [!IMPORTANT]
> Die Xamarin klassische-API, die der Unified API vorangestellt, wurde als veraltet markiert.
> - Die letzte Version von Xamarin.iOS Unterstützung der klassischen-API (monotouch.dll) war Xamarin.iOS 9.10.
> - Xamarin.Mac unterstützt weiterhin die klassische-API, aber es ist nicht mehr aktualisiert. Da sie veraltet ist, sollten Entwickler ihre Anwendungen für die Unified API verschieben.

## <a name="how-to-update-your-apps"></a>Gewusst wie: Aktualisieren Sie Ihre Apps

Es gibt drei Schritte zum Aktualisieren Ihrer apps:

1. Beheben Sie alle compilerwarnungen im vorhandenen Code, insbesondere in Bezug auf veraltete APIs.

2. Verwenden Sie das Migrationstool, das in Visual Studio für Mac integriert, um Ihre Projektdateien und -Namespaces zu aktualisieren.

3. Verbleibende Compiler-Fehler im Zusammenhang mit der neuen Lösung [64-Typen](~/cross-platform/macios/nativetypes.md) und [andere APIs](~/cross-platform/macios/unified/overview.md#deprecated-typos) geändert wurden. Sehen Sie sich [diese Tipps](~/cross-platform/macios/unified/updating-tips.md) Weitere Informationen zum manuellen Updates, die erforderlich sind.

Stehen speziellen Anleitungen für die einzelnen Produkte können Sie Ihre apps aktualisieren, Unified API und der 64-Bit-Unterstützung zur Verfügung:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md)

Vorhandenes Xamarin.iOS-apps können auf die einheitliche API, die mit dem automatisierten Migrationstool, das in Visual Studio für Mac integriert aktualisiert werden Einige zusätzliche Updates möglicherweise erforderlich, siehe [diese Anweisungen](~/cross-platform/macios/unified/updating-ios-apps.md) und [Tipps](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

Vorhandene Xamarin.Mac-apps können auf die einheitliche API, die mit dem automatisierten Migrationstool, das in Visual Studio für Mac integriert aktualisiert werden Einige zusätzliche Updates möglicherweise erforderlich, siehe [diese Anweisungen](~/cross-platform/macios/unified/updating-mac-apps.md) und [Tipps](~/cross-platform/macios/unified/updating-tips.md).

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Um einer vorhandenen Xamarin.Forms-Projektmappe mit einem iOS-Projekt zur Verwendung der Unified API zu aktualisieren, gehen Sie wie folgt vor. Einheitliche API-Unterstützung ist nur verfügbar in Xamarin.Forms-Version 1.3 und höher, damit [Anweisungen](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) auch erläutert, wie Sie Ihre Xamarin.Forms-app auf Version 1.3 zu aktualisieren. Diese [Tipps](~/cross-platform/macios/unified/updating-tips.md) kann hilfreich sein, die native iOS-Code in benutzerdefinierten Renderern oder Abhängigkeitsdienste aktualisieren.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/nativetypes.md)

Dieser Artikel behandelt die mithilfe des neuen iOS Unified API Native Typen (Nint, Nuint Nfloat) in einer plattformübergreifenden Anwendung, in dem Code mit nicht-iOS-Geräte wie Android oder Windows Phone-Betriebssystemen freigegeben. Es bietet einen Einblick in die bei der systemeigenen Typen verwendet werden soll, und bietet mehrere mögliche Lösungen für Fälle, in dem der neue Typ muss mit der plattformübergreifenden Code verwendet werden.

## <a name="update-bindings-to-the-unified-api"></a>Bindungen zu Unified API aktualisieren

Aktualisieren der bindungsprojekt, um Änderungen an der zugrunde liegenden API zu berücksichtigen, (, in dem einige Typen jetzt 64-Bit-sein werden) müssen Kunden, die Bindungen für Objective-C-Bibliotheken erstellt haben.
Führen Sie diese Anweisungen, um [Aktualisieren eines vorhandenen Projekts der Bindung zur Unterstützung der Unified API](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualisieren von Mac-Apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von Xamarin.Forms-Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Aktualisieren von Tipps](~/cross-platform/macios/unified/updating-tips.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
