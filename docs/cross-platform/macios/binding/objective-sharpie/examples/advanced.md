---
title: Erweiterte (manuelle) Real-World-Beispiel
description: Dieses Dokument beschreibt, wie die Ausgabe des Xcodebuild als Eingabe für die Ziel-Sharpie bietet einen Einblick in die Funktionsweise von Ziel Sharpie im Hintergrund.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: e820a0c208907a95dda4a50427bb4dac27b88964
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977892"
---
# <a name="advanced-manual-real-world-example"></a>Erweiterte (manuelle) Real-World-Beispiel

**Dieses Beispiel verwendet die [POP-Bibliothek von Facebook](https://github.com/facebook/pop).**

Dieser Abschnitt enthält einen erweiterten Ansatz für die Bindung, die wir, in dem Apple verwenden `xcodebuild` um erstellen Sie zunächst auf das POP-Projekt, und leiten Sie die Eingabe für die Ziel-Sharpie dann manuell. Dies umfasst im Wesentlichen, was im Hintergrund im vorherigen Abschnitt Ziel Sharpie ausführt.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Da die POP-Bibliothek ein Xcode-Projekt hat (`pop.xcodeproj`), verwenden wir nur `xcodebuild` POP zu erstellen. Dieser Prozess kann wiederum Headerdateien generieren, die Ziel-Sharpie analysiert werden müssen. Dies ist deshalb erstellen vor der Bindung wichtig ist. Beim Erstellen über `xcodebuild` stellen Sie sicher, Sie übergeben die SDK-Bezeichner und die Architektur, die Sie verwenden möchten, übergeben Sie an der Ziel-Sharpie (und denken Sie daran, Ziel Sharpie 3.0 erreichen dies in der Regel für Sie!):

```
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

Stehen viele Informationen der Buildausgabe in der Konsole als Teil des `xcodebuild`. Wie oben angezeigt wird, sehen wir, dass ein "CpHeader"-Ziel ausgeführt wurde in Header-Dateien in ein Buildausgabeverzeichnis kopiert wurden. Dies ist häufig der Fall ist, und erleichtert die Bindung: im Rahmen der nativen Bibliothek erstellen, Headerdateien oft in einen "öffentlich" verständlichen Speicherort, leichter analysieren, für die Bindung können kopiert werden. In diesem Fall wissen wir, dass der POP-Header-Dateien sind die `build/Headers` Verzeichnis.

Wir können nun POP binden. Wir wissen, dass wir für das SDK erstellen möchten `iphoneos8.1` mit der `arm64` Architektur, und die Headerdateien ist uns wichtig sind `build/Headers` unter der POP-Git-Checkout. Wenn wir, in Betrachten der `build/Headers` Verzeichnis sehen, dass eine Anzahl von Headerdateien:

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Wenn wir uns ansehen `POP.h`, wir können sehen, es ist die wichtigste der obersten Ebene der Bibliothek-Header-Datei, die `#import`s andere Dateien. Aus diesem Grund müssen wir nur übergeben `POP.h` zum Ziel-Sharpie und Clang erledigt den Rest hinter den Kulissen:

```
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Sie werden feststellen, dass wir übergeben eine `-scope build/Headers` Ziel Sharpie Argument. Da C und Objective-C-Bibliotheken müssen `#import` oder `#include` andere Headerdateien, die Details zur Implementierung der Bibliothek und keine API, die Sie binden möchten die `-scope` Argument teilt Ziel Sharpie, jeder API zu ignorieren, die nicht in definiert ist ein Datei an einer beliebigen Stelle innerhalb der `-scope` Verzeichnis.

Sie finden die `-scope` Argument ist häufig optional für ordnungsgemäß implementierte Bibliotheken, allerdings ist nicht schädlich, explizit angeben.

Darüber hinaus angegebene `-c -Ibuild/headers`. Zuerst die `-c` Argument teilt Ziel Sharpie Interpretieren von Befehlszeilenargumenten zu beenden, und übergeben alle nachfolgenden Argumente _direkt an den Clang Compiler_. Aus diesem Grund `-Ibuild/Headers` ist ein Clang Compiler-Argument, das Clang zu suchende weist enthält unter `build/Headers`, die werden die POP-Header gespeichert. Ohne dieses Arguments Clang würde nicht wissen, wo Sie die Dateien zu suchen, die `POP.h` ist `#import`Ing. _Fast alle "Probleme" mit der Ziel-Sharpie erzwingungsebenen, um herauszufinden, was zu übergebende clang-_.

### <a name="completing-the-binding"></a>Die Bindung abgeschlossen

Objektive Sharpie wurde jetzt generiert `Binding/ApiDefinitions.cs` und `Binding/StructsAndEnums.cs` Dateien.

Hierbei handelt es sich um Ziel Sharpie des einfachen ersten Schritt die Bindung, und in einigen Fällen kann es sein, alles, was, die Sie benötigen. Wie jedoch weiter oben erwähnt, muss der Entwickler wird in der Regel die generierten Dateien manuell zu ändern, nach Abschluss der Ziel-Sharpie zu [beheben Sie alle Probleme](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , konnte nicht automatisch verarbeitet werden vom Tool.

Nachdem die Updates abgeschlossen haben, können jetzt eine Bindung-Projekt in Visual Studio für Mac hinzugefügt werden oder direkt übergeben werden diese beiden Dateien die `btouch` oder `bmac` Tools, um die letzte Bindung zu erstellen.

Eine ausführliche Beschreibung des Bindungsprozesses, informieren Sie sich unsere [vollständige Exemplarische Vorgehensweise Anweisungen](~/ios/platform/binding-objective-c/walkthrough.md).
