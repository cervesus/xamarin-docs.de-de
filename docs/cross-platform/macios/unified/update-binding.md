---
title: Migrieren einer Bindung zu Unified API
description: Dieser Artikel behandelt die erforderlichen Schritte zum Aktualisieren eines vorhandenen Xamarin binden-Projekts, um die Unified-APIs für Xamarin.IOS- und Xamarin.Mac-Anwendungen zu unterstützen.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: f081ccda507fe3fe65af0e2fb50841aecd7b3c23
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830467"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Migrieren einer Bindung zu Unified API

_Dieser Artikel behandelt die erforderlichen Schritte zum Aktualisieren eines vorhandenen Xamarin binden-Projekts, um die Unified-APIs für Xamarin.IOS- und Xamarin.Mac-Anwendungen zu unterstützen._

## <a name="overview"></a>Übersicht

Ab dem 1. Februar 2015 Apple verlangt, dass alle neue Übermittlungen iTunes und den Mac App Store auf 64-Bit-Anwendungen werden müssen. Daher müssen neue Xamarin.iOS oder Xamarin.Mac-Anwendung die neue einheitliche API zur Unterstützung von 64-Bit anstelle des vorhandenen klassischen MonoTouch / MonoMac-APIs verwenden.

Darüber hinaus muss jedes Projekt für die Xamarin-Bindung auch die neuen Unified-APIs in einer 64-Bit-Xamarin.iOS oder Xamarin.Mac-Projekt einzuschließenden unterstützen. Dieser Artikel behandelt die erforderlichen Schritte für ein vorhandenes bindungsprojekt zum Verwenden der Unified API aktualisieren.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel vorgestellten Schritte ausführen:

- **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und konfiguriert werden, auf dem Entwicklungscomputer.
- **Apple Mac** – eine Apple Mac ist erforderlich, zum Erstellen von Bindung von Projekten für iOS und Mac.

Bindungsprojekte werden in Visual Studio auf einem Windows-Computer nicht unterstützt.

## <a name="modify-the-using-statements"></a>Ändern Sie die Using-Anweisungen

Unified-APIs einfacher als je zuvor Freigeben von Code für Mac und iOS sowie für das können Sie zur Unterstützung von 32 und 64-Bit-Anwendungen mit dem gleichen binären. Durch Löschen der _MonoMac_ und _MonoTouch_ Präfixe aus den Namespaces, einfachere Freigabe für Projekte von Xamarin.Mac- und Xamarin.iOS-Anwendung erreicht ist.

Daher müssen wir unsere Bindung Verträge ändern (und andere `.cs` Dateien in Ihrem bindungsprojekt) So entfernen Sie die _MonoMac_ und _MonoTouch_ Präfixe aus unserer `using` Aussagen.

Betrachten Sie die folgenden using-Anweisungen in einem Vertrag Bindung:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Wir würden entfernen, deaktivieren die `MonoTouch` Präfix folgendermaßen aus:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

In diesem Fall müssen wir dazu für jeden `.cs` Datei in unserem bindungsprojekt. Im nächste Schritt werden mit dieser Änderung vorgenommen wurde aktualisieren Sie unser bindungsprojekt, um die neuen systemeigenen Datentypen verwenden.

Weitere Informationen zur Unified-API finden Sie unter den [Unified API](~/cross-platform/macios/unified/index.md) Dokumentation. Weitere Hintergrundinformationen zur Unterstützung von 32 und 64 Bit-Anwendungen und Informationen zu Frameworks finden Sie unter den [32 und 64-bit-Aspekte zur Geräteplattform](~/cross-platform/macios/32-and-64/index.md) Dokumentation.

## <a name="update-to-native-data-types"></a>Update für systemeigene Datentypen

Objective-C-ordnet die `NSInteger` Datentyp, `int32_t` auf 32-Bit-Systemen und zu `int64_t` auf 64-Bit-Systemen. Um dieses Verhalten übereinstimmen, ersetzt die neue Unified API der vorherigen Verwendung `int` (die in .NET wird als definiert immer `System.Int32`) in einen neuen Datentyp: `System.nint`.

Zusammen mit den neuen `nint` -Datentyp, der Unified API eingeführt wird, die `nuint` und `nfloat` Typen, für die Zuordnung zu den `NSUInteger` und `CGFloat` auch Typen.

Bei Berücksichtigung dieser Vorgaben müssen wir lesen unsere API, und stellen Sie sicher, dass jede Instanz von `NSInteger`, `NSUInteger` und `CGFloat` , die wir bereits zugeordnet `int`, `uint` und `float` aktualisiert, um die neue `nint`, `nuint` und `nfloat` Typen.

Angenommen, eine Definition von Objective-C-Methode:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Wenn vorherige verbindlicher Vertrag die folgende Definition haben:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Wir würden aktualisieren Sie die neue Bindung sein:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Wenn wir zu einer neueren Version 3. Partei Bibliothek zuordnen als was wir zunächst mit verknüpft haben, müssen wir überprüfen den `.h` Headerdateien für die Bibliothek und überprüfen, ob alle vorhandenen explizite Aufrufe von `int`, `int32_t`, `unsigned int`, `uint32_t` oder `float` wurden aktualisiert, um werden ein `NSInteger`, `NSUInteger` oder `CGFloat`. Wenn dies der Fall ist, die gleichen Änderungen an der `nint`, `nuint` und `nfloat` Typen sowie deren Zuordnungen vorgenommen werden müssen.

Weitere Informationen zu diesen Änderungen des Datentyps, finden Sie unter den [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Dokument.

## <a name="update-the-coregraphics-types"></a>Aktualisieren Sie die CoreGraphics-Typen

Der Punkt, Größe und Rechteck-Datentypen, mit denen `CoreGraphics` verwenden 32 oder 64 Bits je nach Gerät sie ausgeführt werden. Wenn Xamarin ursprünglich die IOS- und Mac-APIs gebunden verwendet vorhandene Datenstrukturen, die aufgetreten sind, entsprechen die Datentypen in `System.Drawing` (`RectangleF` z. B.).

Aufgrund der Anforderungen zur Unterstützung von 64-Bit- und die neuen systemeigenen Datentypen, die folgenden Anpassungen müssen am vorhandenen Code vorgenommen werden, wenn der Aufruf von `CoreGraphic` Methoden:

- **CGRect** -Verwendung `CGRect` anstelle von `RectangleF` beim Definieren von floating rechteckige Bereiche zeigen.
- **CGSize** -Verwendung `CGSize` anstelle von `SizeF` beim Definieren von floating Point-Größe (Breite und Höhe).
- **CGPoint** -Verwendung `CGPoint` anstelle von `PointF` beim definieren einen Gleitkommawert Speicherort (X- und Y-Koordinaten) zeigen.

Bei Berücksichtigung dieser Vorgaben wir benötigen, lesen unsere API, und stellen Sie sicher, dass jede Instanz von `CGRect`, `CGSize` oder `CGPoint` , die zuvor an gebunden wurde `RectangleF`, `SizeF` oder `PointF` geändert werden, in den systemeigenen Typ `CGRect`, `CGSize` oder `CGPoint` direkt.

Betrachten Sie z. B. den Objective-C-Initialisierer:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Wenn die vorherige Bindung auf den folgenden Code enthalten:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Wir würden den Code für aktualisieren:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Alle in der Änderungen am Code steht müssen wir unser bindungsprojekt geändert werden, oder stellen die Datei, die für die Unified-APIs zu binden.

## <a name="modify-the-binding-project"></a>Ändern Sie das Bindungsprojekt

Im letzten Schritt zum Aktualisieren von unserer bindungsprojekt, um die Unified-APIs verwenden, müssen wir entweder ändern Sie die `MakeFile` , die wir verwenden, um das Projekt erstellen oder den Xamarin-Projekttyp (Wenn wir von in Visual Studio für Mac Binden), und weisen Sie _Btouch_  für die Unified-APIs, anstatt dem klassischen Bereitstellungsmodell zu binden.


### <a name="updating-a-makefile"></a>Aktualisieren eines Makefiles

Wenn wir unser bindungsprojekt in Xamarin integrieren ein Makefiles verwenden. DLL, wir müssen enthalten die `--new-style` -Befehlszeilenoption, und rufen `btouch-native` anstelle von `btouch`.

Also gemäß der folgenden `MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

Wir wechseln von aufrufenden müssen `btouch` zu `btouch-native`, damit wir unsere Makrodefinition wie folgt anpassen möchten:

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Aktualisieren wir den Aufruf von `btouch` und Hinzufügen der `--new-style` option wie folgt:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Führen wir jetzt unsere `MakeFile` wie gewohnt, um die neue 64-Bit-Version unserer API zu erstellen.

### <a name="updating-a-binding-project-type"></a>Aktualisieren einen Projekttyp Bindung

Wenn wir unsere API, erstellen ein Visual Studio für Mac-Binding-Projektvorlage verwenden, müssen wir auf die neue Unified API-Version von der Projektvorlage Bindung zu aktualisieren. Die einfachste Möglichkeit hierzu ist ein neues Unified API-Bindung Projekt und eine Kopie aller vorhandenen Code und Einstellungen beginnen.

Führen Sie folgende Schritte aus:

1. Starten Sie Visual Studio für Mac.
2. Wählen Sie **Datei** > **neue** > **Projektmappe...**
3. Wählen Sie in das neue Dialogfeld Projektmappe **iOS** > **Unified API** > **iOS-Projekts Bindung**: 

    [![](update-binding-images/image01new.png "Wählen Sie in das neue Dialogfeld Projektmappe iOS / Unified API / iOS Bindung Project")](update-binding-images/image01new.png#lightbox)
4. Geben Sie im Dialogfeld "Neues Projekt konfigurieren" einen **Namen** für das neue bindungsprojekt und klicken Sie auf die **OK** Schaltfläche.
5. Enthalten Sie die 64-Bit-Version von Objective-C-Bibliothek, die Sie beabsichtigen, die Bindungen für erstellt werden.
6. Kopieren Sie den Quellcode aus Ihrem vorhandenen 32-Bit-Classic API-Bindung-Projekt (z. B. die `ApiDefinition.cs` und `StructsAndEnums.cs` Dateien).
7. Stellen Sie die oben genannten Änderungen, um die Quellcodedateien.

Alle diese Änderungen vorgenommen können Sie die neue 64-Bit-Version der API erstellen, wie Sie die 32-Bit-Version.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir gezeigt, die Änderungen, die zur Unterstützung der neuen Unified-APIs und die 64-Bit-Geräten zu einem vorhandenen Xamarin-Bindung Projekt vorgenommen werden müssen, und die Schritte erforderlich, um die neue kompatible 64-Bit-Version einer API zu erstellen.



## <a name="related-links"></a>Verwandte Links

- [Mac- und iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [Überlegungen zur 32/64-Bit-Plattform](~/cross-platform/macios/32-and-64/index.md)
- [Aktualisieren von vorhandenen iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
