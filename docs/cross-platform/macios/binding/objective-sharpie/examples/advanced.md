---
title: Erweitertes (Manuelles) Beispiel
description: In diesem Dokument wird beschrieben, wie die Ausgabe von xcodebuild als Eingabe für das Ziel-Sharpie verwendet wird, das Einblicke in den Ziel-sharkreis bietet.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 2dea16633181d6b1120a5f9a90da685df66e5451
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79031017"
---
# <a name="advanced-manual-real-world-example"></a>Erweitertes (Manuelles) Beispiel

**In diesem Beispiel wird die [Pop-Bibliothek von Facebook](https://github.com/facebook/pop)verwendet.**

In diesem Abschnitt wird ein erweiterter Ansatz für die Bindung behandelt, wobei wir das `xcodebuild` Tool von Apple verwenden, um zuerst das Pop-Projekt zu erstellen, und dann die Eingabe für das Ziel "Sharpie" manuell ableiten. Dabei geht es im Wesentlichen darum, welches Ziel Sharpie im vorherigen Abschnitt im Hintergrund ausgeführt wird.

```bash
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Da die Pop-Bibliothek über ein Xcode-Projekt (`pop.xcodeproj`) verfügt, können wir einfach `xcodebuild` verwenden, um Pop zu erstellen. Dieser Prozess kann wiederum Header Dateien generieren, die vom Ziel-Sharpie möglicherweise analysiert werden müssen. Dies ist der Grund, warum das aufbauen vor der Bindung wichtig ist. Bei der Erstellung über `xcodebuild` stellen Sie sicher, dass Sie denselben SDK-Bezeichner und dieselbe Architektur übergeben, die Sie an das Ziel shar3,0 Pie übergeben möchten.

```bash
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

In der-Konsole werden viele Buildinformationen als Teil `xcodebuild`ausgegeben. Wie oben gezeigt, können wir sehen, dass ein "cpheader"-Ziel ausgeführt wurde, in dem Header Dateien in ein Buildausgabeverzeichnis kopiert wurden. Dies ist häufig der Fall, und die Bindung wird vereinfacht: als Teil der Erstellung der nativen Bibliothek werden Header Dateien häufig in einen "öffentlich" verwendbaren Speicherort kopiert, der die Datenbindung vereinfachen kann. In diesem Fall wissen wir, dass sich die Header Dateien des Popups im `build/Headers` Verzeichnis befinden.

Wir sind nun bereit, den Pop zu binden. Wir wissen, dass wir für SDK-`iphoneos8.1` mit der `arm64`-Architektur erstellen möchten, und dass sich die Header Dateien, die für Sie wichtig sind, in `build/Headers` unter dem Pop-git-Checkout befinden. Wenn wir uns das `build/Headers` Verzeichnis ansehen, sehen wir eine Reihe von Header Dateien:

```bash
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Wenn wir uns `POP.h`ansehen, sehen wir, dass es sich um die Haupt Header Datei der obersten Ebene der Bibliothek handelt, die andere Dateien `#import`. Aus diesem Grund müssen wir nur `POP.h` an das Ziel "Sharpie" übergeben, und clang führt den Rest im Hintergrund aus:

```bash
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

Sie werden feststellen, dass wir ein `-scope build/Headers` Argument an target Sharpie übermittelt haben. Da C-und Target-C-Bibliotheken andere Header Dateien `#import` oder `#include` müssen, bei denen es sich um Implementierungsdetails der Bibliothek handelt, nicht um die API, die Sie binden möchten, weist das `-scope`-Argument an, dass alle APIs ignoriert werden, die in einer Datei irgendwo innerhalb des `-scope` Verzeichnisses nicht definiert sind.

Sie werden feststellen, dass das `-scope`-Argument häufig für ordnungsgemäß implementierte Bibliotheken optional ist, aber es gibt keine Beschädigung bei der expliziten Bereitstellung. 

> [!TIP]
> Wenn die Header der Bibliothek beliebige IOS-SDK-Header importieren, z. b. `#import <Foundation.h>`, müssen Sie den Bereich festlegen, da das Sharpie-Ergebnis für den importierten IOS SDK-Header die Bindungs Definitionen generiert. Dies führt zu einer großen Bindung, die beim Kompilieren des Bindungs Projekts wahrscheinlich Fehler generiert. 

Außerdem haben wir `-c -Ibuild/headers`angegeben. Erstens weist das `-c`-Argument das Ziel Sharpie an, das Interpretieren von Befehlszeilen Argumenten zu verhindern und nachfolgende Argumente _direkt an den clang-Compiler zu_übergeben. Daher ist `-Ibuild/Headers` ein clang-compilerargument, das clang anweist, unter `build/Headers`nach includes zu suchen, wo sich die Pop-Header befinden. Ohne dieses Argument weiß clang nicht, wo die Dateien zu finden sind, die `POP.h` `#import`. _Fast alle "Probleme" mit der Verwendung von "Ziel-Sharpie", um herauszufinden, was an clang übergeben werden soll_.

### <a name="completing-the-binding"></a>Abschließen der Bindung

Der Ziel-Sharpie hat jetzt `Binding/ApiDefinitions.cs`-und `Binding/StructsAndEnums.cs` Dateien generiert.

Dabei handelt es sich um die erste grundlegende Sharpie-Basis, die an der Bindung liegt, und in einigen Fällen ist es möglicherweise alles, was Sie benötigen. Wie bereits erwähnt, muss der Entwickler die generierten Dateien in der Regel manuell ändern, nachdem der Ziel-Sharpie abgeschlossen ist, um [Probleme zu beheben](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , die nicht automatisch vom Tool behandelt werden konnten.

Nachdem die Updates abgeschlossen sind, können diese beiden Dateien nun zu einem Bindungs Projekt in Visual Studio für Mac hinzugefügt oder direkt an die `btouch` oder `bmac` Tools zur Erstellung der endgültigen Bindung geleitet werden.

Eine ausführliche Beschreibung des Bindungs Vorgangs finden Sie in unseren [vollständigen Anweisungen](~/ios/platform/binding-objective-c/walkthrough.md)für die exemplarische Vorgehensweise.
