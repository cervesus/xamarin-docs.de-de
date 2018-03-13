---
title: iOS und Mac
description: "In diesem Abschnitt wird die Strategien zum Freigeben von Code für Ihre Projekte Xamarin.iOS und Xamarin.Mac behandelt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 1491e6ec36a9ced9460e083769b2148386d1d518
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="ios-and-mac"></a>iOS und Mac

_In diesem Abschnitt wird die Strategien zum Freigeben von Code für Ihre Projekte Xamarin.iOS und Xamarin.Mac behandelt._

## <a name="code-sharing"></a>Freigeben von Code

Für Elemente des Codes, die keine Elemente der Benutzeroberfläche die beste Methode zum Freigeben haben-Code für iOS und Mac ist immer noch die Verwendung von [portablen Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md).

Für Code, der einige Arbeit für die Benutzeroberfläche zusammen und noch, Sie freigeben möchten Sie die zu verwendende [freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) der ermöglichen es Ihnen, fügen Sie Code in einem einzelnen Projekt freigeben und damit kompiliert mit Mac und iOS, wenn auf die verwiesen wird.

##  <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

Die einheitliche API für IOS- und Mac-Projekte verwendet die gleichen Namespaces für Frameworks, sodass die gleichen Codedatei für eine nahtlose Codefreigabe auf beiden Plattformen verwendet werden kann. Darüber hinaus ermöglicht 32- und 64-Bit-Builds. Die einheitliche API frühen 2015 ist die Standardeinstellung für die Vorlage seit und wird empfohlen, für alle neuen Projekte - *nur* einheitliche API-Projekte können auf den App Store übermittelt werden.

### <a name="classic-apis"></a>Classic-APIs

> [!NOTE]
> **Klassische Profil Veraltung:** als neue Plattformen in Xamarin.iOS hinzugefügt werden beginnen wir schrittweise Funktionen aus dem klassischen Profil (monotouch.dll) als veraltet markiert. Beispielsweise wurde die Option nicht NRC (neue Ref-Anzahl) entfernt. Für alle einheitliche Anwendungen immer NRC aktiviert wurde (d. h. nicht NRC wurde nie eine Option) und sind keine Probleme bekannt. Zukünftige Versionen werden die Möglichkeit der Verwendung von Boehm als der Garbage Collector entfernt. Dies war auch eine Option für einheitliche Anwendungen nicht verfügbar. Die vollständige Entfernen des klassischen Unterstützung fallen 2016 mit der Version von Xamarin.iOS 10.0 ist geplant.

Vom ursprünglichen (nicht Unified) Xamarin.iOS und Xamarin.Mac-APIs durchgeführt Freigeben von Code schwieriger da native Frameworks entweder enthielt `MonoTouch.` oder `MonoMac.` Namespacepräfixe.  Wir bereitgestellt, die einige leere Namespaces, die Entwicklern zur Freigabe des Codes durch Hinzufügen von ermöglicht `using` Anweisungen, verweisen MonoMac und MonoTouch Namespaces auf die gleiche Datei, aber dies, war ein wenig hässlich. Klassische API sollte nur dann weiter in die legacy-apps verwendet werden, die intern verteilt werden (die ein Upgrade auf die einheitliche API wird empfohlen).


### <a name="updating-from-classic-to-the-unified-api"></a>Aktualisieren von klassischen auf einheitliche API

Es gibt detaillierte Anleitung zur Aktualisierung einer Anwendung in der klassischen einheitliche-API.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Binden von Objective-C-Bibliotheken](binding/index.md)

Xamarin ermöglicht Ihnen die systemeigene Bibliotheken in Ihren apps Bindungen zu bringen. In diesem Abschnitt wird Folgendes erläutert:

- Funktionsweise von Bindungen
- wie Sie manuell eine bindungsprojekt erstellen, in dem Sie Xamarin, Objective-C-Code datentransformierungsschritte können und
- Gewusst wie: Verwenden Sie unsere **Ziel Sharpie** Tool zum Automatisieren des Prozesses.

## <a name="native-referencesnative-referencesmd"></a>[Native Verweise](native-references.md)



##  <a name="macios-native-typesnativetypesmd"></a>[Systemeigene Typen Mac/iOS](nativetypes.md)

Zur Unterstützung von 32 und 64-Bit-Code transparent in c# und f# werden neue Datentypen eingeführt.   Weitere Informationen finden sie hier Informationen.

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[Erstellen von 32 und 64-Bit-apps](32-and-64/index.md)

Was Sie wissen müssen zur Unterstützung von 32- und 64-Bit-Anwendungen.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Arbeiten mit nativen Typen in plattformübergreifenden Apps](native-types-cross-platform.md)

In diesem Artikel erläutert die Verwendung der neuen iOS Unified API systemeigene Typen (`nint`, `nuint`, `nfloat`) in einer plattformübergreifenden-Anwendung, in dem Code nicht iOS-Geräte, z. B. Android oder Windows Phone-Betriebssysteme freigegeben ist.
Einblick in die bei der systemeigenen Typen verwendet werden soll, und bietet mehrere mögliche Lösungen für Fälle, in dem der neue Typ mit plattformübergreifenden Code verwendet werden muss.


## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient-Stapel und SSL/TLS-Implementierungsauswahl](http-stack.md)

Der neue Stapel Selektor für "HttpClient" steuert, welche HttpClient-Implementierung in Ihrer app Xamarin.iOS, Xamarin.tvOS und Xamarin.Mac verwendet wird. Sie können jetzt wechseln Sie zur eine Implementierung, die iOS verwendet, tvos. der außerdem wurden oder native OS x-Transporte (`NSUrlSession` oder `CFNetwork` je nach Betriebssystem).

SSL (Secure Sockets Layer) und dessen Nachfolger – TLS (Transport Layer Security) bieten Unterstützung für HTTP und andere Netzwerkverbindungen über `System.Net.Security.SslStream`. Der neuen SSL/TLS-Implementierung Buildoption wechselt zwischen den Mono-TLS-Stapel und eine Unterstützung von Apple TLS-Stapel im Mac iOS vorhanden.
