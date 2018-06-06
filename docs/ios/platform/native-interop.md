---
title: Verweisen auf systemeigene Bibliotheken in Xamarin.iOS
description: Dieses Dokument erläutert, wie systemeigene C-Bibliotheken in einem Xamarin.iOS-Anwendung zu verknüpfen. Es wird beschrieben, wie universelle systemeigene Bibliotheken und C-Zugriffsmethoden in c# zu erstellen.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: bb27ba8b2d9c1b66448f22b7f80f17ba2e483544
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787724"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Verweisen auf systemeigene Bibliotheken in Xamarin.iOS

Xamarin.iOS unterstützt systemeigene C-Bibliotheken und Objective-C-Bibliotheken verknüpfen. Dieses Dokument erläutert, wie Sie Ihre systemeigenen C-Bibliotheken in Ihrem Projekt Xamarin.iOS verknüpfen. Informationen zum Ausführen für Objective-C-Bibliotheken, finden Sie unter unsere [Objective-C-Typen binden](~/ios/platform/binding-objective-c/index.md) Dokument.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Erstellen von universellen systemeigenen Bibliotheken (i386 ARMv7 und ARM64)

Es ist häufig wünschenswert, Ihre systemeigene Bibliotheken für jede der unterstützten Plattformen für die Entwicklung für iOS zu erstellen (i386 für den Simulator und ARMv7/ARM64 für die Geräte selbst). Wenn Sie bereits über ein Xcode-Projekt für die Bibliothek haben, ist dies wirklich möchten trivial.

Führen Sie den folgenden Befehl aus einer Terminal, um die i386-Version der systemeigenen Bibliothek zu erstellen:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Dies führt zu einer systemeigenen, statischen Bibliothek unter `MyProject.xcodeproj/build/Release-iphonesimulator/`. Die Bibliothek-Archivdatei (libMyLibrary.a) zum an Unbekannter sicher für die spätere Verwendung, indem sie einen eindeutigen Namen kopieren (oder verschieben) (z. B. **LibMyLibrary i386.a**), damit es mit arm64 und armv7-Versionen der gleichen Bibliothek miteinander in Konflikt geraten nicht, die Sie als Nächstes erstellen.

Um die Version ARM64 der systemeigenen Bibliothek zu erstellen, führen Sie den folgenden Befehl ein:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Dieses Mal die integrierte systemeigene Bibliothek sich in befinden `MyProject.xcodeproj/build/Release-iphoneos/`. Noch einmal: Kopieren (oder verschieben) diese Datei an einem sicheren Ort, z. B. umbenennen **LibMyLibrary arm64.a** , damit sie miteinander in Konflikt geraten wird nicht.

Erstellen Sie jetzt die ARMv7-Version der Bibliothek:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Die resultierende Bibliotheksdatei zu kopieren (oder verschieben) an denselben Speicherort verschoben Sie die anderen 2 Versionen der Bibliothek, z. B. umbenennen **LibMyLibrary armv7.a**.

Um ein Universal binäre machen, können Sie die `lipo` Tool wie folgt:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Dies erstellt `libMyLibrary.a` der ist eine universelle (fat)-Bibliothek die eignet sich für alle Ziele der iOS-Entwicklung verwendet werden.


### <a name="missing-required-architecture-i386"></a>Fehlende erforderliche Architektur i386

Wenn Sie erhalten eine `does not implement methodSignatureForSelector` oder `does not implement doesNotRecognizeSelector` Nachricht in der Common Language Runtime-Ausgabe beim Versuch, eine Objective-C-Bibliothek im iOS-Simulator, Ihre Bibliothek nutzen, wahrscheinlich nicht für die i386-Architektur kompiliert wurde (finden Sie unter der [Erstellen universeller Systemeigene Bibliotheken](#building_native) obigen Abschnitt "").

Um die von einer bestimmten Bibliothek unterstützten Architekturen zu überprüfen, verwenden Sie den folgenden Befehl in der abschließenden aus:

```bash
lipo -info /full/path/to/libraryname.a
```

Wobei `/full/path/to/` ist der vollständige Pfad zur Bibliothek konsumiert und `libraryname.a` ist der Name der Bibliothek fraglich.

Wenn Sie die Datenquelle in der Bibliothek verfügen, müssen Sie zum Kompilieren und für die i386-Architektur auch zu bündeln, wenn Sie Ihre app für iOS-Simulator testen möchten.

### <a name="linking-your-library"></a>Verknüpfen Ihre Bibliothek

Eine Drittanbieter-Bibliothek, die Sie nutzen, muss mit Ihrer Anwendung statisch gebunden werden. 

Wenn Sie die Bibliothek "libMyLibrary.a", die aus dem Internet oder der Build mit Xcode statisch zu verknüpfen möchten, müssen Sie einige Schritte ausführen:

-  Schalten Sie die Bibliothek in Ihr Projekt
-  Konfigurieren von Xamarin.iOS, um die Bibliothek zu verknüpfen.
-  Zugriff auf die Methoden aus der Bibliothek.


Um **schalten Sie die Bibliothek in Ihr Projekt**, wählen Sie das Projekt aus dem Projektmappen-Explorer, und drücken Sie **Befehlsoption + + a**. Navigieren Sie zu der libMyLibrary.a und dem Projekt hinzugefügt. Wenn Sie aufgefordert werden, teilen Sie Visual Studio für Mac oder Visual Studio, um ihn in das Projekt zu kopieren. Nach dem Hinzufügen, suchen Sie die libFoo.a im Projekt, klicken Sie mit der rechten Maustaste darauf und Festlegen der **Buildvorgang** auf **keine**.

Um **Xamarin.iOS konfigurieren, um die Bibliothek zu verknüpfen**, auf den Projektoptionen für die endgültige ausführbare Datei (nicht der Bibliothek selbst, aber die endgültige Anwendung) müssen Sie in hinzufügen **iOS-Build**des Extra-Argument (diese sind in Ihrem Projektoptionen) die "-Gcc_flags" Option, gefolgt von einer Zeichenfolge in Anführungszeichen, die alle zusätzlichen Bibliotheken enthält, z. B. für das Programm erforderlich sind:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Im obigen Beispiel wird verknüpfen **libMyLibrary.a**

Sie können `-gcc_flags` an einen beliebigen Satz von Befehlszeilenargumenten, die an den GCC-Compiler verwendet, um die endgültige Verknüpfung Ihrer ausführbaren Datei übergeben. Diese Befehlszeile verweist beispielsweise auch das CFNetwork Framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Die systemeigene Bibliothek enthält C++-Code müssen Sie das Flag - cxx aufweisen, damit Xamarin.iOS weiß, dass er der richtigen Compiler verwenden auch in die "zusätzliche Argumente" übergeben. Für C++ würde die genannten Optionen so aussehen:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>Der Zugriff auf C#-Methoden von C&#35;

Es stehen zwei Arten von systemeigenen Bibliotheken auf iOS:

-  Freigegebene Bibliotheken, die Teil des Betriebssystems sind.

-  Statische Bibliotheken, die Sie mit der Anwendung ausgeliefert wird.


Verwenden Sie in einem dieser beiden definierten Methoden für den Zugriff auf [Mono P/Invoke-Funktionalität](http://www.mono-project.com/docs/advanced/pinvoke/) ist die gleiche Technologie, die Sie in .NET verwenden würden also ungefähr:

-  Bestimmen Sie die C-Funktion, die Sie aufrufen möchten.
-  Bestimmen der Signatur
-  Bestimmen Sie die Bibliothek, die er in lebt
-  Schreiben Sie die entsprechenden P/Invoke-Deklaration


Bei der Verwendung von P/Invoke müssen Sie den Pfad der Bibliothek angeben, die Verknüpfung mit erfolgt. Wenn mit iOS Bibliotheken freigegebene, Sie den Pfad hart können oder können Sie die Vereinfachung-Konstanten, die wir in definiert haben unsere [konstantenklasse](https://developer.xamarin.com/api/type/Constants/), diese Konstanten überdecken iOS freigegebene Bibliotheken.

Beispielsweise mussten Sie zum Aufrufen der UIRectFrameUsingBlendMode-Methode aus der Apple-UIKit Bibliothek besitzt diese Signatur in "c:"

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Die P/Invoke-Deklaration würde wie folgt aussehen:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Die Constants.UIKitLibrary ist lediglich eine Konstante, die als "/ System/Library/Frameworks/UIKit.framework/UIKit", die EntryPoint kann wir die optional angeben, den externen Namen (UIRectFramUsingBlendMode) beim Verfügbarmachen eines anderen Namens in c# ist die kürzere RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Zugreifen auf C Dylibs

Wenn Sie eine C#-Dylib in Ihrer Anwendung Xamarin.iOS nutzen müssen, ist es einem gewissen Grad zusätzliches Setup, die vor dem Aufruf erforderlich ist der `DllImport` Attribut.

Angenommen, wir haben eine `Animal.dylib` mit einer `Animal_Version` Methode, die wir in unserer Anwendung aufgerufen werden soll, müssen wir Xamarin.iOS des Speicherorts der Bibliothek zu informieren, bevor Sie versuchen, diese Methode verwenden.

Bearbeiten Sie hierzu die `Main.CS` Datei, und stellen sie wie folgt aussehen:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Wobei `/full/path/to/` der vollständige Pfad zu der Dylib aufgebraucht ist. Mit diesem Code werden, können wir anschließend zum Verknüpfen der `Animal_Version` Methode wie folgt:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Statische Bibliotheken

Da Sie nur statische Bibliotheken auf iOS verwenden können, besteht keine externen freigegebenen Bibliothek zu verknüpfen, daher der Path-Parameter in das DllImport-Attribut mit den besonderen Namen muss `__Internal` (Beachten Sie die doppelten Unterstrich-Zeichen am Anfang des Namens) im Gegensatz zu der Pfadname.

Dies zwingt DllImport auf das Symbol der Methode zu suchen, die Sie im aktuellen Programm verweisen, anstatt zu versuchen, die aus einer freigegebenen Bibliothek laden.

