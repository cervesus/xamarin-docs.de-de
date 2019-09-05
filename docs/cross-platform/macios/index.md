---
title: Apple-Plattform (IOS und Mac)
description: 'In diesem Dokument werden verschiedene Themen im Zusammenhang mit der xamarin. IOS-und xamarin. Mac-Entwicklung beschrieben: Code Freigabe, die Unified API, Bindungs Ziel-C-Bibliotheken, systemeigene Verweise, Native Typen usw.'
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: b6ac960770320ed100e8b082cabf8240efed070b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290833"
---
# <a name="apple-platform-ios-and-mac"></a>Apple-Plattform (IOS und Mac)

## <a name="code-sharing"></a>Code Freigabe

Für Elemente Ihres Codes, die keine Benutzeroberflächen Elemente aufweisen, ist die beste Möglichkeit zum Freigeben von Code zwischen IOS und Mac weiterhin die Verwendung [portabler Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md).

Für Code, der eine bestimmte Benutzeroberfläche ausführen muss und Sie trotzdem freigeben möchten, sollten Sie frei [gegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) verwenden, die es Ihnen ermöglichen, Code für die Freigabe in einem einzelnen Projekt zu platzieren und ihn mit Mac und IOS zu kompilieren, wenn darauf verwiesen wird.

## <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

Der Unified API für IOS-und Mac-Projekte verwendet die gleichen Namespaces für Frameworks, sodass dieselbe Codedatei auf beiden Plattformen verwendet werden kann, um eine nahtlose Code Freigabe zu bieten. Außerdem werden sowohl 32-als auch 64-Bit-Builds ermöglicht. Der Unified API ist seit Anfang 2015 der Vorlagen Standard und wird für alle neuen Projekte empfohlen. *nur* Unified API Projekte können an den App Store übermittelt werden.

### <a name="classic-apis"></a>Klassische APIs

> [!NOTE]
> **Das klassische Profil ist veraltet:** Wenn neue Plattformen in xamarin. IOS hinzugefügt werden, beginnen wir damit, Features aus dem klassischen Profil ("MonoTouch. dll") allmählich als veraltet zu kennzeichnen. Beispielsweise wurde die Option nicht-NRC (New-Ref-count) entfernt. NRC wurde immer für alle vereinheitlichten Anwendungen aktiviert (d. h. nicht-NRC war nie eine Option) und hat keine bekannten Probleme. In zukünftigen Versionen wird die Option zum Verwenden von Boehm als Garbage Collector entfernt. Dies war auch eine Option, die für einheitliche Anwendungen nicht verfügbar war. Das vollständige Entfernen der klassischen Unterstützung ist für Fall 2016 mit der Veröffentlichung von xamarin. IOS 10,0 geplant.

Die ursprüngliche (nicht vereinheitlichte) xamarin. IOS-und xamarin. Mac-APIs haben die Code Freigabe erschwert, da Native Frame `MonoTouch.` Works `MonoMac.` entweder-oder-Namespace Präfixe enthielt.  Wir haben einige leere Namespaces bereitgestellt, die es Entwicklern ermöglichen, `using` Code gemeinsam zu nutzen, indem Sie Anweisungen hinzufügen, die sowohl auf monomac-als auch auf MonoTouch-Namespaces in derselben Datei verweisen. Dies war jedoch etwas hässlich. Die Classic API sollte nur in Legacy-Apps verwendet werden, die intern verteilt werden (es wird ein Upgrade auf die Unified API empfohlen).


### <a name="updating-from-classic-to-the-unified-api"></a>Aktualisieren von der klassischen zum Unified API

Es gibt ausführliche Anweisungen zum Aktualisieren einer beliebigen Anwendung vom klassischen zum Unified API.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Binden von Objective-C-Bibliotheken](binding/index.md)

Mit xamarin können Sie Native Bibliotheken mit Bindungen in Ihre apps einbringen. In diesem Abschnitt wird Folgendes erläutert:

- Funktionsweise von Bindungen
- Manuelles Erstellen eines Bindungs Projekts, mit dem Sie den Ziel-C-Code in xamarin einbringen können.
- erfahren Sie, wie Sie das **Ziel-Sharpie** -Tool verwenden, um den Prozess zu automatisieren.

## <a name="native-referencesnative-referencesmd"></a>[Native Verweise](native-references.md)

## <a name="macios-native-typesnativetypesmd"></a>[Native Mac/IOS-Typen](nativetypes.md)

Um 32-und 64-Bit-Code C# transparent F#von und zu unterstützen, werden neue Datentypen eingeführt.   Weitere Informationen finden Sie hier.

## <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[Entwickeln von 32-und 64-Bit-apps](32-and-64/index.md)

Was Sie wissen müssen, um 32-und 64-Bit-Anwendungen zu unterstützen.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Arbeiten mit nativen Typen in plattformübergreifenden Apps](native-types-cross-platform.md)

In diesem Artikel wird die Verwendung der neuen Unified API systemeigenen`nint`IOS `nuint`- `nfloat`Typen (,,) in einer plattformübergreifenden Anwendung behandelt, bei der Code für nicht-IOS-Geräte wie Android oder Windows Phone-Betriebssysteme freigegeben ist.
Er bietet Einblicke in den Fall, dass die systemeigenen Typen verwendet werden sollten, und bietet verschiedene mögliche Lösungen für Fälle, in denen der neue Typ mit Platt Form übergreifendem Code verwendet werden muss.

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient-Stapel und SSL/TLS-Implementierungsauswahl](http-stack.md)

Die neue httpclient-Stapel Auswahl steuert, welche httpclient-Implementierung in ihrer xamarin. IOS-, xamarin. tvos-und xamarin. Mac-App verwendet werden soll. Sie können jetzt zu einer Implementierung wechseln, die die nativen Transporte von IOS, tvos oder OS X verwendet`NSUrlSession` ( `CFNetwork` oder je nach Betriebssystem).

SSL (Secure Socket Layer) und dessen Nachfolger, TLS (Transport Layer Security), bieten Unterstützung für http und andere Netzwerkverbindungen `System.Net.Security.SslStream`über. Die neue Option zum Erstellen der SSL/TLS-Implementierung wechselt zwischen dem eigenen TLS-Stapel von Mono und einem von Apple-TLS-Stapel, der in Mac und IOS vorhanden ist.
