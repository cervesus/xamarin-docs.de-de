---
title: Apple-Plattform (iOS und Mac)
description: 'Dieses Dokument beschreibt die verschiedenen Themen im Zusammenhang mit der Entwicklung für Xamarin.iOS- und Xamarin.Mac: Code freigeben, die Unified API, das Binden von Objective-C-Bibliotheken, systemeigener Verweise, systemeigene Typen und mehr.'
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c30d70d8a36c0e5a9b9ff6ddc74710dec4fb86a4
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864396"
---
# <a name="apple-platform-ios-and-mac"></a>Apple-Plattform (iOS und Mac)

## <a name="code-sharing"></a>Freigeben von Code

Für Elemente des Codes, die keine Elemente der Benutzeroberfläche die beste Methode zum Freigeben zwischen iOS und Mac ist immer noch die Verwendung von [Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md).

Für Code, der einige Arbeit für die Benutzeroberfläche durchgeführt werden und noch, Sie freigeben möchten, verwenden Sie [freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) , mit denen Sie fügen Code in einem einzelnen Projekt freigeben und mit Mac und iOS aus, wenn auf die verwiesen wird kompiliert.

## <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

Die Unified API für IOS- und Mac-Projekte verwendet die gleichen Namespaces für Frameworks, sodass derselben Codedatei auf beiden Plattformen, für die nahtlose Freigeben von Code verwendet werden kann. Darüber hinaus können 32- und 64-Bit-Builds. Der Unified API ist seit Anfang 2015 die Standardvorlage und wird empfohlen, für alle neuen Projekte - *nur* Unified-API-Projekte können an den App Store übermittelt werden.

### <a name="classic-apis"></a>Klassische APIs

> [!NOTE]
> **Veraltung des klassischen Profil:** Hinzufügen von neue Plattformen in Xamarin.iOS beginnen wir nach und nach der Features aus dem klassischen Profil (monotouch.dll) als veraltet markiert. Beispielsweise wurde die Option für nicht-NRC (neue-Ref-Count) entfernt. Für alle einheitlichen Anwendungen immer NRC aktiviert wurde (d. h. nicht NRC war noch nie eine Option) und verfügt über keine bekannten Probleme. Zukünftige Versionen werden die Möglichkeit der Verwendung von Boehm als der Garbage Collector entfernt. Dies war auch eine Option für einheitliche Anwendungen nicht verfügbar. Das vollständige Entfernen der klassischen-Unterstützung ist für Herbst 2016 mit der Version von Xamarin.iOS 10.0 geplant.

Die ursprüngliche (nicht-Unified) Xamarin.iOS- und Xamarin.Mac-APIs vorgenommen Freigeben von Code schwieriger, da nativen Frameworks mussten `MonoTouch.` oder `MonoMac.` Namespacepräfixe.  Wir einige leere Namespaces, die Entwicklern zum Freigeben von Code durch Hinzufügen von ermöglicht bereitgestellt `using` -Anweisungen, verweisen sowohl für MonoMac MonoTouch-Namespaces auf die gleiche Datei, aber dies, war etwas hässlich. Die klassische-API sollte nur fortgesetzt werden, die in legacy-apps verwendet werden, die intern verteilt werden (ein Upgrade auf die Unified API empfohlen wird).


### <a name="updating-from-classic-to-the-unified-api"></a>Aus dem klassischen Bereitstellungsmodell aktualisieren zu Unified API

Es gibt detaillierte Anweisungen für jede Anwendung aus dem klassischen Bereitstellungsmodell der Unified API aktualisieren.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Binden von Objective-C-Bibliotheken](binding/index.md)

Xamarin ermöglicht Ihnen die native Bibliotheken in Ihre apps mit Bindungen zu bringen. Dieser Abschnitt erläutert:

- Funktionsweise von Bindungen
- wie Sie ein bindungsprojekt manuell zu erstellen, mit dem Sie einbringen, Objective-C-Code in Xamarin und
- Gewusst wie: Verwenden Sie unsere **Ziel Sharpie** Tool zum Automatisieren des Prozesses.

## <a name="native-referencesnative-referencesmd"></a>[Native Verweise](native-references.md)

## <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS Native Typen](nativetypes.md)

Zur Unterstützung von 32 und 64-Bit-Code transparent aus C# und F#, wir werden neue Datentypen eingeführt.   Erfahren sie hier.

## <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[Erstellen von 32- und 64-Bit-Anwendungen](32-and-64/index.md)

Was müssen Sie wissen, um 32- und 64-Bit-Anwendungen zu unterstützen.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Arbeiten mit nativen Typen in plattformübergreifenden Apps](native-types-cross-platform.md)

In diesem Artikel behandelt die Verwendung des neuen iOS Unified API systemeigene Typen (`nint`, `nuint`, `nfloat`) in einer plattformübergreifenden Anwendung, in dem Code mit nicht-iOS-Geräte wie Android oder Windows Phone-Betriebssystemen freigegeben wird.
Es bietet einen Einblick in die bei der systemeigenen Typen verwendet werden soll, und bietet mehrere mögliche Lösungen für Fälle, in dem der neue Typ muss mit der plattformübergreifenden Code verwendet werden.

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient-Stapel und SSL/TLS-Implementierungsauswahl](http-stack.md)

Die neue Auswahl von "HttpClient" Stack steuert die HttpClient-Implementierung, die in Ihrer Xamarin.iOS-, Xamarin.tvOS- und Xamarin.Mac-app verwenden. Sie können nun auf eine Implementierung, die iOS verwendet, wechseln, TvOS oder native OS x-Transporte (`NSUrlSession` oder `CFNetwork` je nach Betriebssystem).

SSL (Secure Sockets Layer) und sein Nachfolger TLS (Transport Layer Security), bieten Unterstützung für HTTP und andere Netzwerkverbindungen über `System.Net.Security.SslStream`. Die neue SSL/TLS-Implementierung Buildoption wechselt zwischen der Mono-TLS-Stapel und eine, die von Apple TLS-Stapel in Mac und iOS unterstützt.
