---
title: Migrieren einer Bindung zu Unified API
description: In diesem Artikel werden die Schritte beschrieben, die zum Aktualisieren eines vorhandenen xamarin-Bindungs Projekts erforderlich sind, um die Unified APIs für xamarin. IOS-und xamarin. Mac-Anwendungen zu unterstützen.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 90ef47d4e105dc401369c92e9196111c060314e3
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976420"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Migrieren einer Bindung zu Unified API

_In diesem Artikel werden die Schritte beschrieben, die zum Aktualisieren eines vorhandenen xamarin-Bindungs Projekts erforderlich sind, um die Unified APIs für xamarin. IOS-und xamarin. Mac-Anwendungen zu unterstützen._

## <a name="overview"></a>Übersicht

Ab dem 1. Februar 2015 erfordert Apple, dass alle neuen Übermittlungen an iTunes und den Mac App Store 64-Bit-Anwendungen sein müssen. Folglich müssen für jede neue xamarin. IOS-oder xamarin. Mac-Anwendung die neuen Unified API anstelle der vorhandenen klassischen MonoTouch-und monomac-APIs zur Unterstützung von 64 Bit verwendet werden.

Außerdem muss jedes xamarin-Bindungs Projekt auch die neuen vereinheitlichten APIs unterstützen, die in ein xamarin. IOS-oder xamarin. Mac-Projekt mit 64 Bit eingeschlossen werden sollen. In diesem Artikel werden die erforderlichen Schritte zum Aktualisieren eines vorhandenen Bindungs Projekts für die Verwendung des Unified API behandelt.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel beschriebenen Schritte auszuführen:

- **Visual Studio für Mac** : die neueste Version von Visual Studio für Mac auf dem Entwicklungs Computer installiert und konfiguriert.
- **Apple Mac** : ein Apple Mac ist erforderlich, um Bindungs Projekte für IOS und Mac zu erstellen.

Bindungs Projekte werden in Visual Studio auf einem Windows-Computer nicht unterstützt.

## <a name="modify-the-using-statements"></a>Ändern der using-Anweisungen

Die vereinheitlichten APIs machen es einfacher als je zuvor, Code zwischen Mac und IOS gemeinsam zu nutzen, und Sie können 32-und 64-Bit-Anwendungen mit derselben Binärdatei unterstützen. Durch das Löschen der _monomac_ -und _MonoTouch_ -Präfixe aus den Namespaces wird die einfachere Freigabe über xamarin. Mac-und xamarin. IOS-Anwendungsprojekte hinweg erreicht.

Daher müssen wir unsere Bindungs Verträge (und `.cs` andere Dateien in unserem Bindungs Projekt) ändern, um die _monomac_ -und `using` MonoTouch-Präfixe aus den Anweisungen zu entfernen.

Angenommen, die folgenden using-Anweisungen sind in einem Bindungs Vertrag angegeben:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Wir würden das `MonoTouch` Präfix entfernen, das Folgendes ergibt:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Wir müssen dies auch für jede `.cs` Datei im Bindungs Projekt durchführen. Wenn diese Änderung vorhanden ist, besteht der nächste Schritt darin, das Bindungs Projekt so zu aktualisieren, dass die neuen systemeigenen Datentypen verwendet werden.

Weitere Informationen zum Unified API finden Sie in der [Unified API](~/cross-platform/macios/unified/index.md) -Dokumentation. Weitere Informationen zur Unterstützung von 32-und 64-Bit-Anwendungen sowie Informationen zu Frameworks finden Sie in der Dokumentation zu den [Überlegungen zu 32 und 64 Bit](~/cross-platform/macios/32-and-64/index.md)

## <a name="update-to-native-data-types"></a>Aktualisieren auf systemeigene Datentypen

Mit Ziel-C wird `NSInteger` der- `int32_t` Datentyp auf `int64_t` 32-Bit-Systemen und auf 64-Bit-Systemen zugeordnet. Um dieses Verhalten zu erfüllen, ersetzt das neue Unified API die vorherigen Verwendungen von `int` (die in .net als `System.Int32`immer bezeichnet wird) auf einen neuen Datentyp `System.nint`:.

Zusammen mit dem neuen `nint` -Datentyp führt der Unified API die `nuint` - `nfloat` und-Typen ein, um `NSUInteger` auch `CGFloat` die-und-Typen zu Mapping.

Im obigen Beispiel müssen wir unsere API überprüfen und sicherstellen, dass jede Instanz `NSInteger`von `NSUInteger` `CGFloat` , die wir zuvor `int`zugeordnet haben `uint` `float` , und auf die neue `nint` aktualisiertwerden.`nuint` - `nfloat` und-Typen.

Angenommen, eine Definition der Ziel-C-Methode von:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Wenn der vorherige Bindungs Vertrag die folgende Definition enthielt:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Die neue Bindung wird wie folgt aktualisiert:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```

Wenn eine Zuordnung zu einer neueren Version von Drittanbietern als mit der `.h` ursprünglich verknüpften Version stattfinden soll, müssen die Header Dateien für die Bibliothek überprüft werden, und es wird geprüft, ob es sich um das Beenden, explizite Aufrufe von `int`, `int32_t`, `unsigned int` `uint32_t` oder handelt. wurde aktualisiert, sodass Sie ein `NSInteger`oder `NSUInteger` ein `CGFloat`ist. `float` Wenn dies der Fall ist, müssen die `nint`gleichen `nuint` Änderungen `nfloat` an den-und-Typen auch an Ihren Zuordnungen vorgenommen werden.

Weitere Informationen zu diesen Datentyp Änderungen finden Sie im Dokument [native Typen](~/cross-platform/macios/nativetypes.md) .

## <a name="update-the-coregraphics-types"></a>Aktualisieren der CoreGraphics-Typen

Abhängig vom Gerät, auf dem Sie ausgeführt werden, werden die `CoreGraphics` Datentypen "Point", "size" und "Rechteck" 64 32 verwendet, die in verwendet werden. Wenn xamarin die IOS-und Mac-APIs ursprünglich gebunden hat, wurden vorhandene Datenstrukturen verwendet, die mit den `System.Drawing` Daten`RectangleF` Typen in (z. b.) verglichen wurden.

Aufgrund der Anforderungen zur Unterstützung von 64 Bits und den neuen systemeigenen Datentypen müssen die folgenden Anpassungen an vorhandenem Code vorgenommen werden, wenn `CoreGraphic` Methoden aufgerufen werden:

- **CGRect** : verwenden `CGRect` Sie anstelle `RectangleF` von, wenn Sie Gleit Komma-rechteckige Bereiche definieren.
- **CGSize** : verwenden `CGSize` Sie anstelle `SizeF` von, wenn Sie Gleit Komma Größen (Breite und Höhe) definieren.
- **CGPoint** : verwenden `CGPoint` Sie anstelle `PointF` von, wenn Sie eine Gleit Komma Position (X-und Y-Koordinaten) definieren.

Im obigen Beispiel müssen wir unsere API überprüfen und sicherstellen, dass jede Instanz von `CGRect` `CGSize` oder, `CGPoint` die zuvor an `PointF` `RectangleF` `SizeF` den systemeigenen Typ gebunden oder in den systemeigenen `CGRect`Typ geändert wurde, `CGSize` oder`CGPoint` direkt.

Beispielsweise bei einem Ziel-C-Initialisierer von:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Wenn unsere vorherige Bindung den folgenden Code enthielt:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Wir aktualisieren den Code auf Folgendes:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Wenn alle Codeänderungen vorhanden sind, müssen wir das Bindungs Projekt ändern oder die Datei an die vereinheitlichten APIs binden.

## <a name="modify-the-binding-project"></a>Ändern des Bindungs Projekts

Als letzten Schritt zum Aktualisieren des Bindungs Projekts für die Verwendung der Unified APIs muss entweder der `MakeFile` geändert werden, der zum Erstellen des Projekts oder des xamarin-Projekt Typs verwendet wird (wenn die Bindung aus Visual Studio für Mac) und das anweisen von _bberühren_ an BIND für die vereinheitlichten APIs anstelle der klassischen APIs.


### <a name="updating-a-makefile"></a>Aktualisieren eines Makefile

Wenn wir ein Makefile verwenden, um das Bindungs Projekt in xamarin zu erstellen. DLL, wir müssen die `--new-style` Befehlszeilenoption einschließen und anstelle von `btouch`aufruft `btouch-native` .

Es gibt also Folgendes `MakeFile`:

<!--markdownlint-disable MD010 -->
```makefile
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
<!--markdownlint-enable MD010 -->

Wir müssen vom Aufruf `btouch` von zu wechseln, damit wir die `btouch-native`Makro Definition wie folgt anpassen:

```makefile
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Aktualisieren Sie den-Befehl, `btouch` und fügen Sie `--new-style` die-Option wie folgt hinzu:

<!--markdownlint-disable MD010 -->
```makefile
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```
<!--markdownlint-enable MD010 -->

Wir können nun die neue `MakeFile` 64-Bit-Version unserer API als normale Version ausführen.

### <a name="updating-a-binding-project-type"></a>Aktualisieren eines Bindungs Projekt Typs

Wenn wir eine Visual Studio für Mac Bindungs Projektvorlage verwenden, um unsere API zu erstellen, müssen wir auf die neue Unified API Version der Bindungs Projektvorlage aktualisieren. Die einfachste Möglichkeit hierfür ist, ein neues Unified API Bindungs Projekt zu starten und den gesamten vorhandenen Code und die Einstellungen zu kopieren.

Führen Sie folgende Schritte aus:

1. Starten Sie Visual Studio für Mac.
2. Wählen **Sie Datei** > neue > Projekt Mappe **...**
3. Wählen Sie im Dialog Feld neue Projekt Mappe die Option **IOS** > -**Unified API** > **IOS-Bindungs Projekt**aus: 

    [![](update-binding-images/image01new.png "Wählen Sie im Dialog Feld neue Projekt Mappe die Option IOS/Unified API/IOS-Bindungs Projekt aus.")](update-binding-images/image01new.png#lightbox)
4. Geben Sie im Dialogfeld "Neues Projekt konfigurieren" einen **Namen** für das neue Bindungs Projekt ein, und klicken Sie auf die Schaltfläche **OK** .
5. Schließen Sie die 64-Bit-Version der Ziel-C-Bibliothek ein, für die Bindungen erstellt werden sollen.
6. Kopieren Sie den Quellcode aus Ihrem vorhandenen 32-Bit-Classic API Bindungs Projekt (z. `ApiDefinition.cs` b `StructsAndEnums.cs` . die Dateien und).
7. Legen Sie die oben aufgeführten Änderungen an den Quell Code Dateien fest.

Wenn alle diese Änderungen vorhanden sind, können Sie die neue 64-Bit-Version der API wie die 32-Bit-Version erstellen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir die Änderungen erläutert, die an einem vorhandenen xamarin-Bindungs Projekt vorgenommen werden müssen, um die neuen Unified APIs und 64 Bit-Geräte zu unterstützen, sowie die Schritte, die erforderlich sind, um die neue, mit 64 Bit kompatible Version einer API zu erstellen.



## <a name="related-links"></a>Verwandte Links

- [Mac und IOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 Bitplattform-Überlegungen](~/cross-platform/macios/32-and-64/index.md)
- [Aktualisieren vorhandener IOS-apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://docs.microsoft.com/samples/xamarin/ios-samples/bindingsample/)
