---
title: Verweisen auf systemeigene Bibliotheken in Xamarin.iOS
description: In diesem Dokument erläutert, wie systemeigene C++-Bibliotheken in einer Xamarin.iOS-Anwendung zu verknüpfen. Es wird beschrieben, wie zum Erstellen von universal systemeigene Bibliotheken und den Zugriff auf die C-Methoden von C#.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/28/2016
ms.openlocfilehash: a94c70bb7068847ed1b410dd7eddc70921fdf307
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106038"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Verweisen auf systemeigene Bibliotheken in Xamarin.iOS

Xamarin.iOS unterstützt das Verknüpfen mit nativen C++-Bibliotheken und Objective-C-Bibliotheken. Dieses Dokument erläutert, wie Sie Ihre systemeigenen C-Bibliotheken mit Ihrem Xamarin.iOS-Projekt verknüpfen. Informationen zum Ausführen für Objective-C-Bibliotheken finden Sie in unserem [Binden von Objective-C-Typen](~/ios/platform/binding-objective-c/index.md) Dokument.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Erstellen von Universal systemeigenen Bibliotheken (i386 ARMv7 und ARM64)

Es ist häufig wünschenswert, erstellen Sie Ihre systemeigenen Bibliotheken für jedes der unterstützten Plattformen für iOS-Entwicklung (i386 für den Simulator und ARMv7/ARM64 für die Geräte selbst). Wenn Sie bereits über ein Xcode-Projekt für Ihre Bibliothek haben, ist dies nicht schwierig zu tun.

Um die i386-Version der nativen Bibliothek zu erstellen, führen Sie den folgenden Befehl über ein Terminal aus:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Dies führt zu einer nativen, statischen Bibliothek unter `MyProject.xcodeproj/build/Release-iphonesimulator/`. Kopieren Sie die Bibliothek-Archivdatei (libMyLibrary.a) in Stelle sicher für die spätere Verwendung, ihm einen eindeutigen Namen (oder verschieben) (z. B. **LibMyLibrary-i386.a**), damit es mit arm64 sowie auf armv7-Versionen der gleichen Bibliothek miteinander in Konflikt geraten nicht, die Sie als Nächstes erstellen.

Um die ARM64-Version der nativen Bibliothek zu erstellen, führen Sie den folgenden Befehl aus:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Dieses Mal die erstellte native Bibliothek sich im befinden `MyProject.xcodeproj/build/Release-iphoneos/`. Auch hier kopieren (oder verschieben) diese Datei an einem sicheren Ort, z. B. umbenennen **LibMyLibrary-arm64.a** , damit sie miteinander in Konflikt geraten wird nicht.

Erstellen Sie jetzt die ARMv7-Version der Bibliothek:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Die resultierende Bibliotheksdatei zu kopieren (oder zu verschieben) an denselben Speicherort verschoben Sie anderen 2 Versionen der Bibliothek, z. B. umbenennen **LibMyLibrary-armv7.a**.

Um einen Universal binäre machen, können Sie die `lipo` Tool wie folgt:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Dadurch `libMyLibrary.a` werden die universellen (fat)-Bibliothek, die geeignet ist, die für alle Ziele der iOS-Entwicklung verwendet wird.


### <a name="missing-required-architecture-i386"></a>Fehlende erforderliche Architektur i386

Wenn Sie erhalten eine `does not implement methodSignatureForSelector` oder `does not implement doesNotRecognizeSelector` Nachricht in der Common Language Runtime-Ausgabe beim Versuch, eine Objective-C-Bibliothek im iOS-Simulator, Ihre Bibliothek wahrscheinlich nutzen wurde nicht für die "I386"-Architektur kompiliert (finden Sie unter der [Erstellen von Universal Native Bibliotheken](#building_native) weiter oben).

Um die von einer bestimmten Bibliothek unterstützten Architekturen zu überprüfen, verwenden Sie den folgenden Befehl im Terminal aus:

```bash
lipo -info /full/path/to/libraryname.a
```

Wo `/full/path/to/` ist der vollständige Pfad der Bibliothek, die verarbeitet und `libraryname.a` ist der Name der Bibliothek.

Wenn Sie die Quelle der Bibliothek verfügen, müssen Sie zum Kompilieren und zu bündeln diese für die "I386"-Architektur, wenn Sie Ihre app für iOS-Simulator testen möchten.

### <a name="linking-your-library"></a>Verknüpfen Ihre Bibliothek

Alle Bibliotheken von Drittanbietern, die Sie nutzen muss, die statisch mit der Anwendung verknüpft werden. 

Falls gewünscht, können statisch verknüpft sein, die Bibliothek "libMyLibrary.a", die Sie aus dem Internet oder ein Build mit Xcode erhalten haben, müssen Sie einige Punkte beachten:

-  Schalten Sie die Bibliothek in Ihr Projekt
-  Konfigurieren von Xamarin.iOS, um die Bibliothek zu verknüpfen.
-  Greifen Sie auf die Methoden aus der Bibliothek.


Um **schalten Sie die Bibliothek in Ihr Projekt**, wählen Sie das Projekt aus dem Projektmappen-Explorer, und drücken Sie **Befehlsoption + + a**. Navigieren Sie zu der libMyLibrary.a, und fügen sie dem Projekt hinzu. Wenn Sie aufgefordert werden, teilen Sie Visual Studio für Mac oder Visual Studio, um ihn in das Projekt zu kopieren. Nach dem Hinzufügen, suchen die libFoo.a im Projekt, klicken Sie mit der rechten Maustaste darauf, und legen Sie die **Buildvorgang** zu **keine**.

Um **Xamarin.iOS konfigurieren, um die Bibliothek zu verknüpfen**, auf die Optionen für das Projekt für eine endgültige ausführbare Datei (nicht auf die Bibliothek selbst, aber die endgültige Anwendung) müssen Sie beim Hinzufügen **iOS-Build**des Extra-Argument (Hierbei handelt es sich Teil Ihrer Projektoptionen) der "-Gcc_flags" Option, gefolgt von einer Zeichenfolge in Anführungszeichen, die alle zusätzlichen Bibliotheken enthält, die z. B. für das Programm erforderlich sind:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Im Beispiel oben werden verknüpft **libMyLibrary.a**

Sie können `-gcc_flags` an einen beliebigen Satz von Befehlszeilenargumenten für die Übergabe an den GCC-Compiler, die Sie mit dem letzten Link Ihrer ausführbaren Datei verwendet. Diese Befehlszeile verweist z. B. auch das Framework CFNetwork:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Wenn Ihre systemeigene Bibliothek C++-Code, die Sie auch das cxx aufweisen - Flag in Ihrer "zusätzliche Argumente" übergeben müssen enthält, damit Sie Xamarin.iOS weiß, dass der richtige Compiler verwenden. Für C++ würde die vorherigen Optionen so aussehen:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>Zugreifen auf C-Methoden von C&#35;

Es gibt zwei Arten von systemeigenen Bibliotheken zur Verfügung, unter iOS:

-  Freigegebene Bibliotheken, die Teil des Betriebssystems sind.

-  Statische Bibliotheken, die Sie mit Ihrer Anwendung zu senden.


Um einer der definierten Methoden zuzugreifen, verwenden Sie [Mono P/Invoke-Funktionalität](http://www.mono-project.com/docs/advanced/pinvoke/) Dies ist die gleiche Technologie, die Sie in .NET verwenden handelt es sich etwa:

-  Bestimmen Sie die C-Funktion, die Sie aufrufen möchten.
-  Ermitteln der Signatur
-  Bestimmen Sie die Bibliothek, die es befindet sich in
-  Schreiben Sie die entsprechende P/Invoke-Deklaration


Bei der Verwendung von P/Invoke müssen Sie den Pfad der Bibliothek an, die Sie eine Verknüpfung mit herstellen. Beim iOS gemeinsam Bibliotheken genutzte, Sie den Pfad hart codieren können oder können Sie der Einfachheit halber-Konstanten, die in der wir definiert haben unsere [konstantenklasse](https://developer.xamarin.com/api/type/Constants/), diese Konstanten, die freigegebenen iOS-Bibliotheken abgedeckt werden soll.

Wenn Sie möchten, dass Sie die UIRectFrameUsingBlendMode aus UIKit-Bibliothek für Apple Methodenaufruf, die diese Signatur in "c:" hat z. B.

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Die P/Invoke-Deklaration würde wie folgt aussehen:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Die Constants.UIKitLibrary ist lediglich eine Konstante, die als "/ System/Library/Frameworks/UIKit.framework/UIKit", der Einstiegspunkt ermöglicht es uns, geben Sie optional den externen Namen (UIRectFramUsingBlendMode) beim Verfügbarmachen von eines anderen Namens in C#, kürzere RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Zugreifen auf C Dylibs

Wenn Sie nutzen eine C zuständige Dylib in Ihrer Xamarin.iOS-Anwendung benötigen, ist etwas zusätzliches Setup, die vor dem Aufruf erforderlich ist der `DllImport` Attribut.

Angenommen, wir haben eine `Animal.dylib` mit einer `Animal_Version` Methode, die wir in unserer Anwendung aufruft, ist, müssen wir Xamarin.iOS des Speicherorts der Bibliothek zu informieren, bevor Sie versuchen, ihn zu nutzen.

Zu diesem Zweck bearbeiten Sie die `Main.CS` Datei, und stellen sie wie folgt aussehen:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Wo `/full/path/to/` ist der vollständige Pfad, um das zuständige Dylib verarbeitet werden. Mit diesem Code werden, können wir dann zum Verknüpfen der `Animal_Version` -Methode wie folgt:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Statische Bibliotheken

Da Sie nur statische Bibliotheken unter iOS verwenden können, besteht keine externen freigegebenen Bibliothek verknüpfen, der Path-Parameter in das DllImport-Attribut muss mit den besonderen Namen `__Internal` (Beachten Sie die doppelten Unterstriche am Anfang des Namens) im Gegensatz zu der Pfadname.

Dies zwingt DllImport, um das Symbol der Methode zu suchen, die Sie in der aktuellen Anwendung verweisen, anstatt zu versuchen, den sie aus einer freigegebenen Bibliothek laden.

