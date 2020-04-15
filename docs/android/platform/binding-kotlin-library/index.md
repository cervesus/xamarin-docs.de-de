---
title: Binden von Android-Kotlin-Bibliotheken
description: In diesem Dokument wird beschrieben, wie C#-Bindungen an Kotlin-Code erstellt werden, sodass native Bibliotheken in einer Xamarin.Android-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: ec7d154b0d7fcb055bd398089e142fe8b1d9f60e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "77497965"
---
# <a name="bind-android-kotlin-libraries"></a>Binden von Android-Kotlin-Bibliotheken

Die Android-Plattform wird zusammen mit den nativen Sprachen und Tools ständig weiterentwickelt, und es gibt eine Vielzahl von Bibliotheken von Drittanbietern, die unter Verwendung der neuesten Angebote entwickelt wurden. Die maximale Wiederverwendung von Code und Komponenten ist eines der wichtigsten Ziele der plattformübergreifenden Entwicklung. Die Möglichkeit zur Wiederverwendung von Komponenten, die mit Kotlin erstellt wurden, ist für Xamarin-Entwickler immer wichtiger geworden, da deren Beliebtheit bei Entwicklern weiterhin zunimmt. Möglicherweise sind Sie bereits mit dem Binden regulärer [Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)-Bibliotheken vertraut. Jetzt ist eine weitere Dokumentation verfügbar, in der das [Binden einer Kotlin-Bibliothek](walkthrough.md) beschrieben wird, damit die Bibliothek von einer Xamarin-Anwendung wie üblich genutzt werden kann. Der Zweck dieses Dokuments besteht darin, eine übergeordnete Vorgehensweise zum Erstellen einer Kotlin-Bindung für Xamarin zu beschreiben.

## <a name="high-level-approach"></a>Übergeordnete Vorgehensweise

Mit Xamarin können Sie eine beliebige native Bibliothek von Drittanbietern binden, sodass sie von einer X-Anwendung verwendet werden kann. Kotlin ist die neue Sprache und zum Erstellen einer Bindung für Bibliotheken, die mit dieser Sprache erstellt wurden, sind einige zusätzliche Schritte und Tools erforderlich. Diese Vorgehensweise umfasst die folgenden vier Schritte:

1. Erstellen der nativen Bibliothek
1. Vorbereiten der Xamarin-Metadaten, damit die Xamarin-Tools C#-Klassen generieren können
1. Erstellen einer Xamarin-Bindungsbibliothek unter Verwendung der nativen Bibliothek und der Metadaten
1. Verwenden der Xamarin-Bindungsbibliothek in einer Xamarin-Anwendung

In den folgenden Abschnitten werden diese Schritte mit weiteren Details erläutert.

### <a name="build-the-native-library"></a>Erstellen der nativen Bibliothek

Der erste Schritt besteht darin, eine native Kotlin-Bibliothek (AAR-Paket, ein Android-Archiv) zu erhalten. Sie können sie direkt von einem Anbieter anfordern oder selbst erstellen.

### <a name="prepare-the-xamarin-metadata"></a>Vorbereiten der Xamarin-Metadaten

Der zweite Schritt besteht darin, die Metadatentransformationsdatei vorzubereiten, die von den Xamarin-Tools verwendet wird, um die jeweiligen C#-Klassen zu generieren. Im besten Fall ist diese Datei leer, d. h. alle Klassen werden von den Xamarin-Tools erkannt und generiert. In einigen Fällen muss die Metadatentransformation angewendet werden, um den richtigen und/oder gewünschten C# -Code zu generieren. In vielen Fällen muss ein AAR-Disassembler, z. B. [Java Decompiler (JD)](http://java-decompiler.github.io/), verwendet werden, um verborgene Abhängigkeiten und unerwünschte Klassen zu ermitteln, die aus der endgültigen Liste der zu generierenden C#-Klassen ausgeschlossen werden sollen. Die endgültigen Metadaten müssen die öffentliche Schnittstelle darstellen, mit der die verweisende Xamarin.Android-Anwendung interagiert.

### <a name="build-a-xamarinandroid-binding-library"></a>Erstellen einer Xamarin.Android-Bindungsbibliothek

Der dritte Schritt besteht darin, ein spezielles Projekt zu erstellen: eine Xamarin.Android-Bindungsbibliothek. Diese enthält die-Kotlin-Bibliotheken als native Verweise und die Metadatentransformation, die im vorherigen Schritt definiert wurde. Beim Schreiben ist für jedes referenzierte AAR-Paket ein separates Android-Bindungsbibliothekprojekt erforderlich. Die Bindungsbibliothek muss das [Xamarin.Kotlin.StdLib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)-Paket hinzufügen, damit die Kotlin-Standardbibliothek unterstützt wird.

### <a name="consume-the-xamarin-binding-library"></a>Verwenden der Xamarin-Bindungsbibliothek

Der vierte und letzte Schritt besteht darin, in einer Xamarin.Android-Anwendung auf die Bindungsbibliothek zu verweisen. Durch das Hinzufügen eines Verweises auf die Xamarin.Android-Bindungsbibliothek kann die Xamarin-Anwendung die verfügbar gemachten Kotlin-Klassen im betreffenden Paket verwenden.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Die obige Vorgehensweise beschreibt die übergeordneten Schritte, die zum Erstellen einer Kotlin-Bindung für Xamarin erforderlich sind. Es gibt viele untergeordnete Schritte und weitere Details, die beim Vorbereiten dieser Bindungen in der Praxis berücksichtigt werden müssen, z. B. die Anpassung an Änderungen in den nativen Tools und Sprachen. Das Ziel besteht darin, Ihnen ein besseres Verständnis dieses Konzepts und der übergeordneten Schritte in diesem Prozess zu ermöglichen. Eine ausführliche Schritt-für-Schritt-Anleitung finden Sie in der Dokumentation [Exemplarische Vorgehensweise: Xamarin-Kotlin-Bindung](walkthrough.md).

## <a name="related-links"></a>Verwandte Links

- [Android Studio](https://developer.android.com/studio)
- [Gradle-Installation](https://gradle.org/install/)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Java Decompiler](http://java-decompiler.github.io/)
- [Kotlin-Bibliothek „BubblePicker“](https://github.com/igalata/Bubble-Picker)
- [Binden einer Java-Bibliothek](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Metadaten für Java-Bindungen](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Beispielprojektrepository](https://github.com/xamcat/xamarin-binding-kotlin-framework)
