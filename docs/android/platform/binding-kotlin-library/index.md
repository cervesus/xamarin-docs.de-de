---
title: Binden von Android-Kotlin-Bibliotheken
description: In diesem Dokument wird beschrieben, C# wie Sie Bindungen für den Code von Kotlin erstellen, sodass Native Bibliotheken in einer xamarin. Android-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: ec7d154b0d7fcb055bd398089e142fe8b1d9f60e
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497965"
---
# <a name="bind-android-kotlin-libraries"></a>Binden von Android-Kotlin-Bibliotheken

Die Android-Plattform wird zusammen mit den nativen Sprachen und Tools ständig weiterentwickelt, und es gibt zahlreiche Bibliotheken von Drittanbietern, die mit den neuesten angeboten entwickelt wurden. Das maximieren von Code und der Wiederverwendung von Komponenten ist eines der wichtigsten Ziele der plattformübergreifenden Entwicklung. Die Möglichkeit zur Wiederverwendung von mit Kotlin erstellten Komponenten ist für xamarin-Entwickler immer wichtiger geworden, da ihre Beliebtheit bei Entwicklern weiterhin zunimmt. Möglicherweise sind Sie bereits mit dem Binden regulärer [Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) -Bibliotheken vertraut. Es ist jetzt eine zusätzliche Dokumentation verfügbar, in der der Prozess der [Bindung einer Kotlin-Bibliothek](walkthrough.md)beschrieben wird, sodass Sie von einer xamarin-Anwendung auf die gleiche Weise genutzt werden können. Der Zweck dieses Dokuments ist es, einen allgemeinen Ansatz zum Erstellen einer Kotlin-Bindung für xamarin zu beschreiben.

## <a name="high-level-approach"></a>Allgemeiner Ansatz

Mit xamarin können Sie eine native Bibliothek von Drittanbietern binden, damit Sie von einer xamarin-Anwendung genutzt werden kann. Kotlin ist die neue Sprache und das Erstellen einer Bindung für Bibliotheken, die mit dieser Sprache erstellt wurden, erfordert einige zusätzliche Schritte und Tools. Diese Vorgehensweise umfasst die folgenden vier Schritte:

1. Erstellen der nativen Bibliothek
1. Vorbereiten der xamarin-Metadaten, die xamarin-Tools das C# Generieren von Klassen ermöglichen
1. Erstellen einer xamarin-Bindungs Bibliothek mithilfe der nativen Bibliothek und der Metadaten
1. Verwenden der xamarin-Bindungs Bibliothek in einer xamarin-Anwendung

In den folgenden Abschnitten werden diese Schritte mit zusätzlichen Details erläutert.

### <a name="build-the-native-library"></a>Erstellen der nativen Bibliothek

Der erste Schritt besteht darin, eine systemeigene-und-v-Bibliothek (AAR-Paket, ein Android-Archiv) abzurufen. Sie können Sie entweder direkt von einem Anbieter anfordern oder selbst erstellen.

### <a name="prepare-the-xamarin-metadata"></a>Vorbereiten der xamarin-Metadaten

Der zweite Schritt besteht darin, die metadatentransformations-Datei vorzubereiten, die von den xamarin-Tools verwendet wird, C# um die jeweiligen Klassen zu generieren. Im besten Fall ist diese Datei möglicherweise leer, wo alle Klassen von den xamarin-Tools erkannt und generiert werden. In einigen Fällen muss die Metadatentransformation angewendet werden, um korrekten und/oder C# gewünschten Code zu generieren. In vielen Fällen muss ein Aar-Disassembler (z. b. Java-Debug [(JD))](http://java-decompiler.github.io/)verwendet werden, um verborgene Abhängigkeiten und unerwünschte Klassen zu identifizieren, die Sie aus der endgültigen C# Liste der zu generierenden Klassen ausschließen möchten. Die endgültigen Metadaten sollten die öffentliche Schnittstelle darstellen, in der die verweisende xamarin. Android-Anwendung mit interagiert.

### <a name="build-a-xamarinandroid-binding-library"></a>Erstellen einer xamarin. Android-Bindungs Bibliothek

Der dritte Schritt besteht darin, ein spezielles Projekt zu erstellen: eine xamarin. Android-Bindungs Bibliothek. Sie enthält die-Umwandlungs Bibliotheken als Native Verweise und die Metadatentransformation, die im vorherigen Schritt definiert wurde. Zum Zeitpunkt des Schreibens ist ein separates Projekt für eine Android-Bindungs Bibliothek erforderlich, das für jedes zu referendende Aar-Paket erforderlich ist. Die Bindungs Bibliothek muss das [xamarin. Kotlin. stdlib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) -Paket hinzufügen, um die-Standard Bibliothek von Kotlin zu unterstützen.

### <a name="consume-the-xamarin-binding-library"></a>Verwenden der xamarin-Bindungs Bibliothek

Der vierte und letzte Schritt besteht darin, in einer xamarin. Android-Anwendung auf die Bindungs Bibliothek zu verweisen. Durch das Hinzufügen eines Verweises auf die xamarin. Android-Bindungs Bibliothek kann Ihre xamarin-Anwendung die verfügbar gemachten Kotlin-Klassen innerhalb des Pakets verwenden.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Der obige Ansatz beschreibt die auf der obersten Ebene erforderlichen Schritte zum Erstellen einer Kotlin-Bindung für xamarin. Es gibt zahlreiche Schritte auf niedrigerer Ebene und weitere Details, die beim Vorbereiten dieser Bindungen in der Praxis berücksichtigt werden müssen, einschließlich der Anpassung an Änderungen in den systemeigenen Tools und Sprachen. Ziel ist es, Sie dabei zu unterstützen, ein besseres Verständnis dieses Konzepts und der grundlegenden Schritte zu erhalten, die an diesem Vorgang beteiligt sind. Eine ausführliche Schritt-für-Schritt-Anleitung finden Sie in der exemplarischen Vorgehensweise zur [xamarin-Kotlin-Bindung](walkthrough.md) .

## <a name="related-links"></a>Verwandte Links

- [Android Studio](https://developer.android.com/studio)
- [Gradle-Installation](https://gradle.org/install/)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Java-compilercompiler](http://java-decompiler.github.io/)
- [Bubblepicker-Kotlin-Bibliothek](https://github.com/igalata/Bubble-Picker)
- [Binden der Java-Bibliothek](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Metadaten der Java-Bindung](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin. Kotlin. stdlib-nuget](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Beispielprojektrepository](https://github.com/xamcat/xamarin-binding-kotlin-framework)
