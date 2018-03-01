---
title: Erste Schritte mit Java
ms.topic: article
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 1a29481e4da3a7c1f72a513db70afc1ca6e5f3e8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-java"></a>Erste Schritte mit Java


Dies ist die abrufen gestarteten Seite für Java, die mit die Grundlagen von allen unterstützten Plattformen behandelt.

## <a name="requirements"></a>Anforderungen

Einbetten von .NET mit Java zu verwenden, die Sie benötigen:

* Java 1.8 oder höher
* [Mono 5.0](http://www.mono-project.com/download/)

Für Mac:
* Xcode 8.3.2 oder höher

Für Windows:
* Visual Studio 2017 mit C++-Unterstützung
* Windows 10 SDK

Für Android:
* Xamarin.Android 7.4.99 oder höher (Erstellen von [Jenkins](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/))
* [Android Studio 3.x](https://developer.android.com/studio/index.html) mit Java 1.8

Sie können [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) bearbeiten und Kompilieren von C#-Code.

> [!NOTE]
> Frühere Versionen von Visual Studio, Xcode, Xamarin.Android, Android Studio und Mono _möglicherweise_ arbeiten, sind aber nicht getestet und wird nicht unterstützt.

## <a name="installation"></a>Installation

Einbetten von .NET wird derzeit auf [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```csharp
nuget install Embeddinator-4000
```
Dies abzulegen `Embeddinator-4000.exe` in die `packages/Embeddinator-4000/tools` Verzeichnis.

Darüber hinaus können Sie Embeddinator von Quell-zu erstellen, finden Sie unter unsere [Git-Repository](https://github.com/mono/Embeddinator-4000/) und [beitragen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) Dokument Anweisungen.

## <a name="platforms"></a>Plattformen

Java ist derzeit in einem vorschauzustand MacOS, Windows und Android.

Die Plattform ist ausgewählt, durch Übergeben der `--platform=<platform>` Befehlszeilenargument die Embeddinator. Derzeit `macOS`, `Windows`, und `Android` werden unterstützt.

### <a name="macos-and-windows"></a>MacOS und Windows

Für die Entwicklung sollte jeder Java-IDE zu verwenden, die Java 1.8 unterstützt. Sie können sogar Android Studio für diese gegebenenfalls [finden Sie hier](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Sie können die Ausgabe der JAR-Datei verwenden, wie alle standardmäßigen Java-Jar-Datei.

### <a name="android"></a>Android

Stellen Sie sicher, dass Sie bereits so eingerichtet sind Android-Anwendungen entwickeln, bevor Sie versuchen, erstellen Sie eine Embeddinator verwenden. Die [Anweisungen](~/tools/dotnet-embedding/get-started/java/android.md) wird davon ausgegangen, dass Sie bereits erfolgreich erstellt und eine Android-Anwendung auf Ihrem Computer bereitstellen.

Android Studio wird empfohlen, für die Entwicklung, aber anderen IDEs Formulargröße als Unterstützung für die [AAR-Dateiformat](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>Weiterführende Themen

* [Erste Schritte für Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Rückrufe für Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Vorläufige Android Research](~/tools/dotnet-embedding/android/index.md)
* [Embeddinator Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
