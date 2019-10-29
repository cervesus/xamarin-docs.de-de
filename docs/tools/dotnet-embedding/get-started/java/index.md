---
title: Einstieg in Java
description: In diesem Dokument werden die ersten Schritte beim Einbetten von .net mit Java beschrieben. Es werden die Systemanforderungen, die Installation und die unterstützten Plattformen erläutert.
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: 09ea33724c2b1184654ce7768ea1cb2525b62c28
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007379"
---
# <a name="getting-started-with-java"></a>Einstieg in Java

Dies ist die Seite "Getting Started" für Java, die die Grundlagen für alle unterstützten Plattformen abdeckt.

## <a name="requirements"></a>Anforderungen

Um .net-Einbettung mit Java verwenden zu können, benötigen Sie Folgendes:

* Java 1,8 oder höher
* [Mono 5,0](https://www.mono-project.com/download/)

Für Mac:

* Xcode 8.3.2 oder höher

Für Windows:

* Visual Studio 2017 mit C++ Unterstützung
* Windows 10 SDK

Für Android:

* [Xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) oder höher
* [Android Studio 3. x](https://developer.android.com/studio/index.html) mit Java 1,8

Sie können [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) verwenden, um Ihren C# Code zu bearbeiten und zu kompilieren.

> [!NOTE]
> Frühere Versionen von Xcode, Visual Studio, xamarin. Android, Android Studio und Mono funktionieren _möglicherweise_ , sind jedoch nicht getestet und werden nicht unterstützt.

## <a name="installation"></a>Installation

.Net-Einbettungen sind zurzeit für [nuget](https://www.nuget.org/packages/Embeddinator-4000/)verfügbar:

```shell
nuget install Embeddinator-4000
```

Dadurch wird " **Embeddinator-4000. exe** " in das Verzeichnis " **Packages/embeddinator-4000/Tools** " platziert.

Darüber hinaus können Sie .net-Einbettungen aus der Quelle erstellen. Weitere Informationen finden Sie in unserem [git-Repository](https://github.com/mono/Embeddinator-4000/) [und im zugehörigen](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) Dokument.

## <a name="platforms"></a>Plattformen

Java befindet sich derzeit in einem Vorschau Zustand für macOS, Windows und Android.

Die Plattform wird ausgewählt, indem das Befehlszeilenargument `--platform=<platform>` an das .net-Einbettungs Tool übergeben wird. Derzeit werden `macOS`, `Windows`und `Android` unterstützt.

### <a name="macos-and-windows"></a>macOS und Windows

Für die Entwicklung sollten Sie eine beliebige Java-IDE verwenden können, die Java 1,8 unterstützt. Sie können bei Bedarf sogar Android Studio hierfür verwenden. Weitere Informationen hierzu finden Sie [hier](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Sie können die Ausgabe der JAR-Datei wie jede beliebige Standard-java-jar-Datei verwenden.

### <a name="android"></a>Android

Stellen Sie sicher, dass Sie bereits für die Entwicklung von Android-Anwendungen eingerichtet sind, bevor Sie versuchen, eine mit .net-Einbettung zu erstellen. Die [folgenden Anweisungen](~/tools/dotnet-embedding/get-started/java/android.md) setzen voraus, dass Sie bereits erfolgreich eine Android-Anwendung von Ihrem Computer erstellt und bereitgestellt haben.

Android Studio wird für die Entwicklung empfohlen, aber andere IDES sollten so lange funktionieren, wie das Aar- [Dateiformat](https://developer.android.com/studio/projects/android-library.html)unterstützt wird.

## <a name="further-reading"></a>Weiterführende Themen

* [Einstieg in Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Rückrufe unter Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Vorläufige Android-Recherche](~/tools/dotnet-embedding/android/index.md)
* [.Net-Einbettungs Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirken am Open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
