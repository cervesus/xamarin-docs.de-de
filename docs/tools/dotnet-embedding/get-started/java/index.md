---
title: Erste Schritte mit Java
description: Dieses Dokument beschreibt, wie die ersten Schritte mit Java mit Einbetten von .NET. Es wird erläutert, Systemanforderungen, Installation und unterstützte Plattformen.
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: 79a483743946c4f7509833867f2afe4b1e055183
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667177"
---
# <a name="getting-started-with-java"></a>Erste Schritte mit Java

Dies ist die ersten Schritte Seite für Java, das die Grundlagen für alle unterstützten Plattformen behandelt.

## <a name="requirements"></a>Anforderungen

Einbetten von .NET mit Java verwenden, die Sie benötigen:

* Java 1.8 oder höher
* [Mono 5.0](https://www.mono-project.com/download/)

Für Mac:

* Xcode 8.3.2 oder höher

Für Windows:

* Visual Studio 2017 mit C++-Unterstützung
* Windows 10 SDK

Für Android:

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) oder höher
* [Android Studio 3.x](https://developer.android.com/studio/index.html) mit Java 1.8

Sie können [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) bearbeiten und Kompilieren Ihrer C# Code.

> [!NOTE]
> Frühere Versionen von Visual Studio, Xcode, Xamarin.Android, Android Studio und Mono _möglicherweise_ arbeiten, sind jedoch nicht getestet und wird nicht unterstützt.

## <a name="installation"></a>Installation

Einbetten von .NET wird derzeit auf [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

Dies legt **Embeddinator-4000.exe** in die **Pakete/Embeddinator-4000/Tools** Verzeichnis.

Darüber hinaus können Sie Einbetten von .NET aus der Quelle zu erstellen, finden Sie unserem [Git-Repository](https://github.com/mono/Embeddinator-4000/) und [beitragen](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) Dokument finden Sie Anweisungen.

## <a name="platforms"></a>Plattformen

Java befindet sich derzeit im vorschauzustand für MacOS, Windows und Android.

Die Plattform ausgewählt ist, übergeben die `--platform=<platform>` Befehlszeilenargument für das Tool zum Einbetten von .NET. Derzeit `macOS`, `Windows`, und `Android` werden unterstützt.

### <a name="macos-and-windows"></a>MacOS und Windows

Für die Entwicklung sollten Sie möglicherweise keine Java-IDE verwenden, das Java 1.8 unterstützt. Sie können sogar Android Studio verwenden, für diese bei Bedarf [finden Sie hier](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Sie können die Ausgabe der JAR-Datei verwenden, wie alle standardmäßigen Java-JAR-Datei.

### <a name="android"></a>Android

Stellen Sie sicher, dass Sie sind bereits dafür eingerichtet, Android-Anwendungen zu entwickeln, bevor Sie versuchen, Sie erstellen mithilfe von Einbetten von .NET. Die [Anweisungen](~/tools/dotnet-embedding/get-started/java/android.md) wird davon ausgegangen, dass Sie bereits erfolgreich erstellt und eine Android-Anwendung auf Ihrem Computer bereitgestellt.

Android Studio wird empfohlen, für die Entwicklung, aber andere IDEs sollten funktionieren, solange es Unterstützung für ist die [AAR-Dateiformat](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>Weiterführende Themen

* [Erste Schritte für Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Rückrufe, die unter Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Vorläufige Android Research](~/tools/dotnet-embedding/android/index.md)
* [Einbetten von .NET-Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirkung an open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
