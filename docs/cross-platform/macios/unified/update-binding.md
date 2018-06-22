---
title: Migrieren Sie eine Bindung an die einheitliche API
description: Dieser Artikel behandelt die erforderlichen Schritte zum Aktualisieren einer vorhandenen Xamarin binden-Projekt, um die Unified-APIs für Xamarin.IOS und Xamarin.Mac Anwendungen zu unterstützen.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d57f27bf0d3aaa2a7ba14f23481a8f2bb2d87f2
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33918486"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Migrieren Sie eine Bindung an die einheitliche API

_Dieser Artikel behandelt die erforderlichen Schritte zum Aktualisieren einer vorhandenen Xamarin binden-Projekt, um die Unified-APIs für Xamarin.IOS und Xamarin.Mac Anwendungen zu unterstützen._

## <a name="overview"></a>Übersicht

Ab dem 1. Februar 2015 Apple erfordert, dass alle neue Übermittlungen Mac App Store und iTunes 64-Bit-Anwendungen. Daher müssen neue Xamarin.iOS oder Xamarin.Mac-Anwendung die neue einheitliche API zur Unterstützung von 64-Bit anstelle des vorhandenen klassischen MonoTouch und MonoMac-APIs verwenden.

Bindung Xamarin-Projekt muss darüber hinaus auch die neuen Unified-APIs in einem 64-Bit-Xamarin.iOS oder Xamarin.Mac-Projekt einzuschließenden unterstützen. In diesem Artikel werden die erforderlichen Schritte zum Aktualisieren eines vorhandenen Bindung-Projekts zum Verwenden der API Unified behandelt.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, die in diesem Artikel vorgestellten Schritte ausführen:

 -  **Visual Studio für Mac** -die neueste Version von Visual Studio für Mac installiert und konfiguriert werden, auf dem Entwicklungscomputer.
 -  **Apple Macintosh** – eine Apple Macintosh ist erforderlich, zum Erstellen von Projekten binden, für iOS und Mac

Bindung Projekte werden in Visual Studio auf einem Windows-Computer nicht unterstützt.

## <a name="modify-the-using-statements"></a>Ändern Sie die Using-Anweisungen

Die APIs Unified einfacher als je zuvor Mac und iOS sowie für das können Sie zur Unterstützung der 32- und 64-Bit-Anwendungen mit dem binary-Code freigeben. Durch Löschen der _MonoMac_ und _MonoTouch_ Präfixe aus den Namespaces einfachere Freigabe projektübergreifend Anwendung Xamarin.Mac und Xamarin.iOS erreicht ist.

Daher müssen wir alle unsere Bindung Verträge ändern (und andere `.cs` Dateien in unsere bindungsprojekt) So entfernen Sie die _MonoMac_ und _MonoTouch_ Präfixe von unserer `using` -Anweisungen.

Angenommen, die folgenden using-Anweisungen in einem Vertrag Bindung:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Wir würden entfernt, von der `MonoTouch` Präfix führt die folgenden:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Wir müssen erneut aus, klicken Sie hierzu für jede `.cs` Datei in unsere bindungsprojekt. Im nächste Schritt werden durch diese Änderung festliegen aktualisieren unsere bindungsprojekt, um die neuen systemeigenen Datentypen verwenden.

Weitere Informationen zu den einheitliche API, finden Sie unter der [einheitliche API](~/cross-platform/macios/unified/index.md) Dokumentation. Weitere Hintergrundinformationen zur Unterstützung von 32 und 64 Bit-Anwendungen und Informationen zu Frameworks finden Sie unter der [32 und 64-bit-Plattform Überlegungen](~/cross-platform/macios/32-and-64/index.md) Dokumentation.

## <a name="update-to-native-data-types"></a>Update von systemeigenen Datentypen

Objective-C ordnet die `NSInteger` Datentyp `int32_t` unter 32-Bit-Systemen und zu `int64_t` auf 64-Bit-Systemen. Um dieses Verhalten übereinstimmen, ersetzt die neue einheitliche API der vorherigen Verwendung `int` (die in .NET wird als definiert immer `System.Int32`) in einen neuen Datentyp: `System.nint`.

Zusammen mit den neuen `nint` -Datentyp, der einheitliche API führt die `nuint` und `nfloat` Typen, für die Zuordnung zu den `NSUInteger` und `CGFloat` auch Typen.

Berücksichtigung der oben beschriebenen, müssen wir unserer API überprüfen und sicherstellen, dass jede Instanz von `NSInteger`, `NSUInteger` und `CGFloat` , die wir zuvor zugeordnet `int`, `uint` und `float` aktualisiert, um die neue `nint`, `nuint` und `nfloat` Typen.

Angenommen, der Definition einer Objective-C:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Wenn die vorherige Bindung Vertrag die folgende Definition haben:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Aktualisieren Sie die neue Bindung sein:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Wenn wir eine zu einer neueren Version 3. Partei Bibliothek Zuordnung als was wir zunächst mit verknüpft war, müssen wir überprüfen die `.h` Headerdateien für die Bibliothek und ausführliche Informationen finden Sie ggf. vorhandene explizite Aufrufe von `int`, `int32_t`, `unsigned int`, `uint32_t` oder `float` wurden aktualisiert, um werden ein `NSInteger`, `NSUInteger` oder ein `CGFloat`. Wenn dies der Fall ist, die gleichen Änderungen an der `nint`, `nuint` und `nfloat` Typen sowie deren Zuordnung vorgenommen werden müssen.

Weitere Informationen zu diesen Änderungen des Datentyps finden Sie unter der [systemeigene Typen](~/cross-platform/macios/nativetypes.md) Dokument.

## <a name="update-the-coregraphics-types"></a>Aktualisieren Sie die CoreGraphics-Typen

Der Punkt, Größe und Rechteck Datentypen, mit denen `CoreGraphics` verwenden 32 oder 64 Bits, je nach Gerät quasi auf ausgeführt werden. Wenn Xamarin ursprünglich IOS- und Mac-APIs gebunden wird vorhandenen Datenstrukturen, die aufgetreten sind, entsprechend der Datentypen in `System.Drawing` (`RectangleF` z. B.).

Aufgrund der Anforderungen zur Unterstützung von 64 Bits und die neuen systemeigenen Datentypen, die folgenden Anpassungen müssen am vorhandenen Code vorgenommen werden, beim Aufrufen von `CoreGraphic` Methoden:

- **CGRect** -Verwendung `CGRect` anstelle von `RectangleF` beim Definieren von Gleitkomma rechteckige Bereiche zeigen.
- **CGSize** -Verwendung `CGSize` anstelle von `SizeF` beim Definieren von floating Point-Größen (Breite und Höhe).
- **CGPoint** -Verwendung `CGPoint` anstelle von `PointF` beim definieren einen Gleitkommawert Speicherort (X- und Y-Koordinaten) zeigen.

Berücksichtigung der oben beschriebenen, wir benötigen, lesen unsere API und stellen Sie sicher, dass jede Instanz von `CGRect`, `CGSize` oder `CGPoint` , die an zuvor gebunden wurde `RectangleF`, `SizeF` oder `PointF` geändert werden, um den systemeigenen Typ `CGRect`, `CGSize` oder `CGPoint` direkt.

Betrachten Sie z. B. den Objective-C-Initialisierer eines:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Wenn unsere vorherige Bindung auf den folgenden Code enthalten:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Aktualisieren Sie die dieser Code hinzu:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Alle codeänderungen nun festliegen müssen wir unsere bindungsprojekt geändert werden, oder stellen die Datei, die auf Unified-APIs gebunden.

## <a name="modify-the-binding-project"></a>Ändern Sie das Bindungsprojekt

Als abschließenden Schritt zum Aktualisieren unsere bindungsprojekt, um die Unified-APIs verwenden, müssen so ändern Sie entweder die `MakeFile` , die wir verwenden, um das Projekt erstellen oder den Typ der Xamarin-Projekt (wenn es von innerhalb von Visual Studio für Mac Binden), und weisen Sie _Btouch_  auf Unified-APIs anstelle der klassischen zu binden.


### <a name="updating-a-makefile"></a>Aktualisieren eines Makefiles

Wenn wir unsere bindungsprojekt in einer Xamarin integrieren ein Makefiles verwenden. DLL, müssen wir die einschließen der `--new-style` -Befehlszeilenoption, und rufen `btouch-native` anstelle von `btouch`.

Angenommen, die folgenden `MakeFile`:

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

Wechseln Sie von aufrufen müssen `btouch` auf `btouch-native`, damit wir unsere Makrodefinition wie folgt anpassen möchten:

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Aktualisieren Sie den Aufruf von `btouch` und Hinzufügen der `--new-style` option wie folgt:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Wir können jetzt ausführen unserer `MakeFile` wie gewohnt, um die neue 64-Bit-Version von unserer API zu erstellen.

### <a name="updating-a-binding-project-type"></a>Aktualisieren von eines Bindung-Projekttyps

Wenn wir unsere API Erstellen eines Visual Studio für Mac-Projektvorlage Bindung verwenden, müssen wir auf die neue einheitliche API-Version der Projektvorlage Bindung zu aktualisieren. Die einfachste Möglichkeit hierzu ist ein neues Unified-API-Bindung Projekt und eine Kopie aller vorhandenen Code und Einstellungen beginnen.

Führen Sie folgende Schritte aus:

1. Starten Sie Visual Studio für Mac.
2. Wählen Sie **Datei** > **neue** > **Lösung...**
3. Wählen Sie im Dialogfeld für neue Projektmappen **iOS** > **einheitliche API** > **iOS-Projekt Binding**: 

    [![](update-binding-images/image01new.png "Wählen Sie im Dialogfeld für neue Projektmappen iOS / einheitliche API / iOS Bindung Projekt")](update-binding-images/image01new.png#lightbox)
4. Geben Sie im Dialogfeld "Neues Projekt konfigurieren" ein **Namen** für die neue bindungsprojekt und klicken Sie auf die **OK** Schaltfläche.
5. Schließen Sie die 64-Bit-Version der Objective-C-Bibliothek, die Sie Bindungen für erstellen möchten.
6. Über den Quellcode aus dem vorhandenen 32-Bit-klassische API bindungsprojekt kopieren (z. B. die `ApiDefinition.cs` und `StructsAndEnums.cs` Dateien).
7. Nehmen Sie die oben genannten notiert wurden Änderungen an die Quellcodedateien.

Alle diese Änderungen vorhanden können Sie die neue 64-Bit-Version der API erstellen, wie Sie die 32-Bit-Version.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir gezeigt, die Änderungen, die zur Unterstützung der neuen Unified-APIs und die 64-Bit-Geräten zu einem vorhandenen Xamarin-Bindung Projekt vorgenommen werden müssen, und die Schritte erforderlich, um die neue kompatible 64-Bit-Version einer API zu erstellen.



## <a name="related-links"></a>Verwandte Links

- [Mac- und iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [Bedingungen bezüglich der Plattform 32/64-bit](~/cross-platform/macios/32-and-64/index.md)
- [Aktualisieren vorhandene iOS-Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
