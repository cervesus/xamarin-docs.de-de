---
title: Erweiterte (manuell) Real-World-Beispiel
ms.topic: article
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 67bd1caf26c441e2a89def41ce3189b0dd67d7b1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="advanced-manual-real-world-example"></a>Erweiterte (manuell) Real-World-Beispiel


**Dieses Beispiel verwendet die [POP-Bibliothek von Facebook](https://github.com/facebook/pop).**


Dieser Abschnitt enthält einen erweiterten Ansatz für die Bindung, die wir, auf dem Apple verwenden `xcodebuild` , um zuerst das POP-Projekt zu erstellen, und klicken Sie dann Eingabe für Objective Sharpie manuell abzuleiten. Dies umfasst im Wesentlichen, was im Endeffekt im vorherigen Abschnitt Ziel Sharpie ausführt.

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Da die POP-Bibliothek Xcode-Projekt hat (`pop.xcodeproj`), es können nur `xcodebuild` POP zu erstellen. Durch diesen Prozess kann wiederum Headerdateien generiert, die Ziel Sharpie ggf. zu analysieren. Dies ist deshalb erstellen, bevor Bindung wichtig ist. Beim Erstellen über `xcodebuild` gewährleisten Sie übergeben die SDK-Bezeichner und die Architektur, die Sie beabsichtigen, die zum Ziel Sharpie übergeben (und denken Sie daran, Ziel Sharpie 3.0 kann dies in der Regel für Sie nicht!):

```csharp
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

Es werden viele Informationen der Buildausgabe in der Konsole als Teil `xcodebuild`. Wie oben angezeigt wird, können wir sehen, dass ein "CpHeader" Ziel ausgeführt wurde in Headerdateien in ein Buildausgabeverzeichnis kopiert wurden. Dies ist häufig der Fall, und erleichtert die Bindung: im Rahmen des Builds die systemeigene Bibliothek-Header-Dateien häufig in einen "öffentlich" nutzbar Standort, der für die Bindung einfachere Analyse vornehmen kann kopiert werden. In diesem Fall wissen wir, dass der POP-Header-Dateien sind die `build/Headers` Verzeichnis.

Wir können nun POP zu binden. Wir wissen, dass wir für das SDK erstellen möchten `iphoneos8.1` mit der `arm64` Architektur und, dass die Header-Dateien, die wir von Interesse sind `build/Headers` unter das POP Git Auschecken. Wenn wir, in betrachten dem `build/Headers` Verzeichnis, sehen wir eine Anzahl von Headerdateien:

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Wenn wir betrachten `POP.h`, können wir sehen, ist der Bibliothek Hauptfenster auf oberster Ebene Header-Datei mit `#import`s andere Dateien. Aus diesem Grund müssen wir nur übergeben `POP.h` zum Ziel-Sharpie und Clang führen die übrigen im Hintergrund:

```csharp
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

Sie werden feststellen, dass wir übergeben ein `-scope build/Headers` Argument Ziel Sharpie. Da C und Objective-C-Bibliotheken müssen `#import` oder `#include` andere Headerdateien, die Implementierungsdetails der Bibliothek und keine API, die Sie binden möchten die `-scope` -Argument teilt Ziel Sharpie, um eine beliebige API zu ignorieren, die nicht in definiert ist ein Datei an einer beliebigen Stelle innerhalb der `-scope` Verzeichnis.

Sie finden die `-scope` Argument ist häufig optional ordnungsgemäß implementierten Bibliotheken, allerdings ist keine Schäden explizit bereitstellen.

Darüber hinaus angegebenen `-c -Ibuild/headers`. Erstens die `-c` -Argument teilt Sharpie Zielpunkt, beenden die Befehlszeilenargumente interpretieren und alle nachfolgenden Argumente übergeben _direkt an den Clang Compiler_. Aus diesem Grund `-Ibuild/Headers` ist ein Clang Compiler-Argument, das Clang zu suchende weist umfasst unter `build/Headers`, also die POP-Header, in dem Leben. Ohne dieses Argument Clang würde nicht wissen, wo Sie die Dateien zu suchen, `POP.h` ist `#import`Ing. _Fast alle "Probleme" mit der Verwendung von Ziel Sharpie Kernpunkte, um herauszufinden, was zum Clang übergeben_.

###<a name="completing-the-binding"></a>Abschließen der Bindungsnamens

Objektive Sharpie wurde jetzt generiert `Binding/ApiDefinitions.cs` und `Binding/StructsAndEnums.cs` Dateien.

Hierbei handelt es sich um grundlegende ersten Durchlauf Ziel-Sharpie an der Bindung, und in einigen Fällen möglicherweise alles, was, die Sie benötigen. Wie jedoch oben erwähnt, müssen in der Regel der Entwickler die generierten Dateien manuell zu ändern, nach Abschluss des Ziels Sharpie auf [beseitigen Sie jegliche Probleme](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , konnte nicht automatisch verarbeitet werden vom Tool.

Nachdem die Updates abgeschlossen haben, können nun eine Bindung-Projekt in Visual Studio für Mac hinzugefügt werden oder direkt übergeben werden diese beiden Dateien der `btouch` oder `bmac` Tools, um die letzte Bindung zu erstellen.

Eine ausführliche Beschreibung des Bindungsprozesses, finden Sie unter unsere [vollständige Exemplarische Vorgehensweise Anweisungen](~/ios/platform/binding-objective-c/walkthrough.md).

