---
title: Aktualisieren vorhandener apps auf die Unified API
description: Dieses Dokument ist mit verschiedenen Anleitungen verknüpft, in denen beschrieben wird, wie xamarin-Anwendungen auf die Unified API aktualisiert werden. Er erläutert xamarin. IOS-apps, xamarin. Mac-apps. Xamarin. Forms-apps, Native Typen in plattformübergreifenden apps und Bindungs Projekte.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 3379b9672b344e8e424f95e273683f4c5e241b71
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280798"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Aktualisieren vorhandener apps auf die Unified API

> [!IMPORTANT]
> Das xamarin-Classic API, das dem Unified API vorangestellt ist, ist veraltet.
> - Die letzte Version von xamarin. IOS zur Unterstützung der Classic API ("MonoTouch. dll") war xamarin. IOS 9,10.
> - Xamarin. Mac unterstützt zwar weiterhin die Classic API, wird jedoch nicht mehr aktualisiert. Da sie veraltet ist, sollten Entwickler ihre Anwendungen in die Unified API verschieben.

## <a name="how-to-update-your-apps"></a>Aktualisieren Ihrer Apps

Zum Aktualisieren Ihrer apps sind drei Schritte erforderlich:

1. Beheben Sie alle Compilerwarnungen im vorhandenen Code, insbesondere solche, die sich auf veraltete APIs beziehen.

2. Verwenden Sie das in integrierte Migrations Tool, um zu Visual Studio für Mac, Ihre Projektdateien und Namespaces zu aktualisieren.

3. Beheben Sie verbleibende Compilerfehler im Zusammenhang mit den neuen [64-Typen](~/cross-platform/macios/nativetypes.md) und [anderen APIs](~/cross-platform/macios/unified/overview.md#deprecated-typos) , die sich geändert haben. Weitere Informationen zu manuellen Updates, die möglicherweise erforderlich sind, finden Sie in [diesen Tipps](~/cross-platform/macios/unified/updating-tips.md) .

Für jedes Produkt sind bestimmte Leitfäden verfügbar, damit Sie Ihre apps auf die Unified API und 64-Bit-Unterstützung aktualisieren können:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md)

Vorhandene xamarin. IOS-Apps können mit dem automatisierten Migrationstool, das in Visual Studio für Mac integriert ist, auf die Unified API aktualisiert werden. Einige zusätzliche Korrekturen sind möglicherweise erforderlich, wie in [diesen Anweisungen](~/cross-platform/macios/unified/updating-ios-apps.md) und [Tipps](~/cross-platform/macios/unified/updating-tips.md)erläutert.

### <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

Vorhandene xamarin. Mac-Apps können mit dem automatisierten Migrationstool, das in Visual Studio für Mac integriert ist, auf die Unified API aktualisiert werden. Einige zusätzliche Korrekturen sind möglicherweise erforderlich, wie in [diesen Anweisungen](~/cross-platform/macios/unified/updating-mac-apps.md) und [Tipps](~/cross-platform/macios/unified/updating-tips.md)erläutert.

### <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms-Apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Befolgen Sie diese Anweisungen, um eine vorhandene xamarin. Forms-Projekt Mappe mit einem IOS-Projekt zu aktualisieren, um die Unified API zu verwenden. Unified API-Unterstützung ist nur in xamarin. Forms 1,3 und höher verfügbar. Außerdem wird in [den Anweisungen](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) erläutert, wie Sie Ihre xamarin. Forms-App auf Version 1,3 aktualisieren. Diese [Tipps](~/cross-platform/macios/unified/updating-tips.md) können dabei helfen, den systemeigenen IOS-Code in benutzerdefinierten renderatoren oder Abhängigkeits Diensten zu aktualisieren.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/nativetypes.md)

In diesem Artikel wird beschrieben, wie Sie die neuen systemeigenen IOS-Unified API Typen (NINT, nuint, nfloat) in einer plattformübergreifenden Anwendung verwenden, bei der Code für nicht-IOS-Geräte (z. b. Android-oder Windows Phone-Betriebssysteme) Er bietet Einblicke in den Fall, dass die systemeigenen Typen verwendet werden sollten, und bietet verschiedene mögliche Lösungen für Fälle, in denen der neue Typ mit Platt Form übergreifendem Code verwendet werden muss.

## <a name="update-bindings-to-the-unified-api"></a>Aktualisieren von Bindungen auf den Unified API

Kunden, die Bindungen für die Ziel-C-Bibliotheken erstellt haben, müssen das Bindungs Projekt aktualisieren, um Änderungen an der zugrunde liegenden API (bei denen es sich nun um 64-Bit handelt) wiederzugeben.
Befolgen Sie diese Anweisungen, um [ein vorhandenes Bindungs Projekt zu aktualisieren und die Unified API zu unterstützen](~/cross-platform/macios/unified/update-binding.md).

## <a name="related-links"></a>Verwandte Links

- [Aktualisieren von IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualisieren von Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualisieren von xamarin. Forms-apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualisieren von Bindungen](~/cross-platform/macios/unified/update-binding.md)
- [Tipps werden aktualisiert](~/cross-platform/macios/unified/updating-tips.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
