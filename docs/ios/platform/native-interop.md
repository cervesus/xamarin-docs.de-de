---
title: Verweisen auf Native Bibliotheken in xamarin. IOS
description: In diesem Dokument wird erläutert, wie Sie native C-Bibliotheken in eine xamarin. IOS-Anwendung verknüpfen. Es wird beschrieben, wie universelle Native Bibliotheken erstellt und von C#auf C-Methoden zugegriffen wird.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/28/2016
ms.openlocfilehash: 426b46f578ac3a09372fa4bf63ace1b6545c4866
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528183"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Verweisen auf Native Bibliotheken in xamarin. IOS

Xamarin. IOS unterstützt das Verknüpfen mit nativen c-Bibliotheken und Ziel-C-Bibliotheken. In diesem Dokument wird erläutert, wie Sie Ihre nativen C-Bibliotheken mit Ihrem xamarin. IOS-Projekt verknüpfen. Informationen dazu, wie Sie für Ziel-c-Bibliotheken dasselbe tun, finden Sie im Dokument [Binden von Zielen-c-Typen](~/ios/platform/binding-objective-c/index.md) .

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Entwickeln von universellen nativen Bibliotheken (i386, ARMv7 und ARM64)

Häufig ist es wünschenswert, ihre nativen Bibliotheken für jede der unterstützten Plattformen für die IOS-Entwicklung zu erstellen (i386 für den Simulator und ARMv7/ARM64 für die Geräte selbst). Wenn Sie bereits ein Xcode-Projekt für Ihre Bibliothek haben, ist dies wirklich trivial.

Führen Sie den folgenden Befehl über ein Terminal aus, um die i386-Version Ihrer nativen Bibliothek zu erstellen:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Dies führt zu einer nativen statischen Bibliothek unter `MyProject.xcodeproj/build/Release-iphonesimulator/`. Kopieren (oder verschieben) Sie die bibliotheksarchiv Datei (libmylibrary. a) zur späteren Verwendung an eine beliebige Stelle, und geben Sie Ihr einen eindeutigen Namen (z. b. **libmylibrary-i386. a**), sodass Sie nicht mit den arm64-und armv7-Versionen der gleichen Bibliothek übereinstimmt, die Sie als Nächstes erstellen werden.

Führen Sie den folgenden Befehl aus, um die ARM64-Version Ihrer nativen Bibliothek zu erstellen:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Dieses Mal wird die integrierte native Bibliothek in `MyProject.xcodeproj/build/Release-iphoneos/`gespeichert. Kopieren Sie diese Datei erneut, oder verschieben Sie Sie an einen sicheren Speicherort, und benennen Sie Sie so um, wie **libmylibrary-arm64. a** , sodass Sie nicht in Konflikt steht.

Erstellen Sie nun die ARMv7-Version der Bibliothek:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Kopieren (oder verschieben) Sie die resultierende Bibliotheksdatei an denselben Speicherort, an den Sie die anderen beiden Versionen der Bibliothek verschoben haben, und benennen Sie Sie in etwas wie **libmylibrary-armv7. a**um.

Um eine universelle Binärdatei zu erstellen, können `lipo` Sie das Tool wie folgt verwenden:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Dadurch wird `libMyLibrary.a` eine universelle (FAT) Bibliothek erstellt, die für alle IOS-Entwicklungsziele geeignet ist.


### <a name="missing-required-architecture-i386"></a>Erforderliche Architektur i386 fehlt.

Wenn Sie eine `does not implement methodSignatureForSelector` -oder `does not implement doesNotRecognizeSelector` -Nachricht in der Lauf Zeit Ausgabe erhalten, wenn Sie versuchen, eine Ziel-C-Bibliothek im IOS-Simulator zu verwenden, wurde die Bibliothek wahrscheinlich nicht für die i386-Architektur kompiliert (siehe [Building Universal Native ](#building_native)Abschnitt "Bibliotheken" oben).

Um die von einer bestimmten Bibliothek unterstützten Architekturen zu überprüfen, verwenden Sie den folgenden Befehl im Terminal:

```bash
lipo -info /full/path/to/libraryname.a
```

Dabei steht für den vollständigen Pfad zur verwendeten Bibliothek und `libraryname.a` für den Namen der betreffenden Bibliothek. `/full/path/to/`

Wenn Sie die Quelle der Bibliothek haben, müssen Sie Sie auch für die i386-Architektur kompilieren und bündeln, wenn Sie Ihre APP im IOS-Simulator testen möchten.

### <a name="linking-your-library"></a>Verknüpfen Ihrer Bibliothek

Alle von Ihnen genutzten Drittanbieterbibliotheken müssen statisch mit Ihrer Anwendung verknüpft werden. 

Wenn Sie die Bibliothek "libmylibrary. a", die Sie aus dem Internet erhalten haben, statisch verknüpfen oder mit Xcode erstellen möchten, müssen Sie einige Dinge tun:

- Verschieben der Bibliothek in Ihr Projekt
- Konfigurieren von xamarin. IOS zum Verknüpfen der Bibliothek
- Greifen Sie auf die Methoden aus der Bibliothek zu.


Um **die Bibliothek in das Projekt**zu übertragen, wählen Sie das Projekt aus dem Projektmappen-Explorer aus, und drücken Sie **Command + Option + a**. Navigieren Sie zur Datei libmylibrary. a, und fügen Sie Sie dem Projekt hinzu. Wenn Sie dazu aufgefordert werden, geben Sie Visual Studio für Mac oder Visual Studio an, um Sie in das Projekt zu kopieren. Suchen Sie nach dem Hinzufügen des Projekts die Datei libfoo. a, klicken Sie mit der rechten Maustaste darauf , und legen Sie die Buildaktion auf **keine**fest.

Zum **Konfigurieren von xamarin. IOS zum Verknüpfen der Bibliothek**müssen Sie in den Projektoptionen für Ihre endgültige ausführbare Datei (nicht für die Bibliothek selbst, aber das endgültige Programm) das zusätzliche Argument von **IOS-Builds**hinzufügen (diese sind Teil der Projektoptionen), "-gcc_flags". , gefolgt von einer Zeichenfolge in Anführungszeichen, die alle zusätzlichen Bibliotheken enthält, die für das Programm erforderlich sind, z. b.:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Im obigen Beispiel wird **libmylibrary. a verknüpft.**

Sie können verwenden `-gcc_flags` , um einen beliebigen Satz von Befehlszeilen Argumenten anzugeben, die an den GCC-Compiler übergeben werden, der zum Abschließen der endgültigen Verknüpfung der ausführbaren Datei verwendet wird. Diese Befehlszeile verweist z. b. auch auf das CFNetwork-Framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Wenn die native Bibliothek Code C++ enthält, müssen Sie auch das Flag "-CXX" in ihren "zusätzlichen Argumenten" übergeben, damit xamarin. IOS weiß, dass der richtige Compiler verwendet wird. C++ Die vorherigen Optionen würden wie folgt aussehen:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>Zugreifen auf c-Methoden von c&#35;

Es gibt zwei Arten von nativen Bibliotheken, die unter IOS verfügbar sind:

- Freigegebene Bibliotheken, die Teil des Betriebssystems sind.

- Statische Bibliotheken, die Sie mit Ihrer Anwendung ausgeliefert haben.


Für den Zugriff auf Methoden, die in einer dieser beiden definiert sind, verwenden Sie die [Funktion "-P/aufrufen" von Mono](https://www.mono-project.com/docs/advanced/pinvoke/) , bei der es sich um dieselbe Technologie handelt, die Sie in .NET verwenden würden.

- Bestimmen Sie, welche C-Funktion Sie aufrufen möchten.
- Bestimmen der Signatur
- Bestimmen der Bibliothek, in der Sie sich befindet
- Schreiben der entsprechenden P/Aufruf-Deklaration

Wenn Sie P/aufrufen verwenden, müssen Sie den Pfad der Bibliothek angeben, mit der Sie eine Verknüpfung herstellen. Wenn Sie freigegebene IOS-Bibliotheken verwenden, können Sie entweder den Pfad hart codieren, oder Sie können die in unserer `Constants`definierten feedkonstanten verwenden. diese Konstanten sollten die freigegebenen IOS-Bibliotheken abdecken.

Wenn Sie z. b. die uirectframeusingblendmode-Methode aus der UIKit-Bibliothek von Apple aufrufen möchten, die diese Signatur in C hat:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Ihre P/Aufruf-Deklaration würde wie folgt aussehen:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Die "Konstanten. uikitlibrary" ist lediglich eine Konstante, die als "/System/Library/Frameworks/UIKit.Framework/UIKit" definiert ist, und der Einstiegspunkt ermöglicht es uns, optional den externen Namen (uirectframusingblendmode) C#anzugeben, während ein anderer Name in verfügbar gemacht wird. kürzerer rectframeusingblendmode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Zugreifen auf C-dylisb

Wenn Sie in ihrer xamarin. IOS-Anwendung eine C-dylib verwenden müssen, müssen Sie vor dem Aufrufen des `DllImport` -Attributs etwas zusätzliches Setup durchführen.

Wenn wir z. b. über `Animal.dylib` eine- `Animal_Version` Methode mit einer-Methode verfügen, die in unserer Anwendung aufgerufen wird, müssen wir xamarin. IOS über den Speicherort der Bibliothek informieren, bevor Sie versuchen, Sie zu nutzen.

Bearbeiten Sie hierzu die `Main.CS` Datei, und führen Sie Sie wie folgt aus:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Dabei `/full/path/to/` ist der vollständige Pfad zur verwendeten dylib. Wenn dieser Code vorhanden ist, können wir wie folgt eine Verknüpfung `Animal_Version` mit der-Methode herstellen:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Statische Bibliotheken

Da Sie nur statische Bibliotheken unter IOS verwenden können, gibt es keine externe freigegebene Bibliothek, mit der eine Verknüpfung hergestellt werden kann. Daher muss der path-Parameter im DllImport `__Internal` -Attribut den besonderen Namen verwenden (Beachten Sie die doppelten Unterstriche am Anfang des Namens), im Gegensatz zu der Pfadname.

Dadurch wird DllImport gezwungen, das Symbol der Methode zu suchen, auf die Sie im aktuellen Programm verweisen, anstatt zu versuchen, Sie aus einer freigegebenen Bibliothek zu laden.

