---
title: Xamarin. Mac-Architektur
description: In diesem Handbuch werden xamarin. Mac und seine Beziehung zu Ziel-C auf niedriger Ebene behandelt. Darin werden Konzepte wie Kompilierung, Selektoren, Registrars, App-Start und Generator erläutert.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 04/12/2017
ms.openlocfilehash: 2c9bbd663257e937e35e062f03b4aa84813edb27
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287782"
---
# <a name="xamarinmac-architecture"></a>Xamarin. Mac-Architektur

_In diesem Handbuch werden xamarin. Mac und seine Beziehung zu Ziel-C auf niedriger Ebene behandelt. Darin werden Konzepte wie Kompilierung, Selektoren, Registrars, App-Start und Generator erläutert._

## <a name="overview"></a>Übersicht

Xamarin. Mac-Anwendungen werden innerhalb der Mono-Ausführungsumgebung ausgeführt und verwenden den-Compiler von xamarin, um Sie in Intermediate Language (IL) zu kompilieren. Dies ist dann Just-in-time (JIT), das zur Laufzeit in nativen Code kompiliert wird. Dies wird parallel zur Ziel-C-Laufzeit ausgeführt. Beide Laufzeitumgebungen werden auf einem UNIX-ähnlichen Kernel, insbesondere XNU, ausgeführt und stellen verschiedene APIs für den Benutzercode bereit, sodass Entwickler auf das zugrunde liegende systemeigene oder verwaltete System zugreifen können.

Das folgende Diagramm zeigt eine grundlegende Übersicht über diese Architektur:

[![Diagramm mit einer grundlegenden Übersicht über die Architektur](architecture-images/mac-arch.png "Diagramm mit einer grundlegenden Übersicht über die Architektur")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Nativer und verwalteter Code

Bei der Entwicklung für xamarin werden die *Begriffe System* eigener und *verwalteter* Code häufig verwendet. Verwalteter Code ist Code, dessen Ausführung von der .NET Framework Common Language Runtime verwaltet wird, oder im Fall von xamarin: der Mono-Laufzeit.

Bei System eigenem Code handelt es sich um Code, der auf der jeweiligen Plattform nativ ausgeführt wird (z. b. mit Ziel-C oder sogar AOT-kompiliertem Code auf einem Arm-Chip). In dieser Anleitung wird erläutert, wie der verwaltete Code in nativen Code kompiliert wird, und es wird erläutert, wie eine xamarin. Mac-Anwendung funktioniert, und die Mac-APIs von Apple werden mithilfe von Bindungen vollständig genutzt, während gleichzeitig auf zugegriffen werden kann. BCL von NET und eine ausgereifte Sprache wie C#.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um eine macOS-Anwendung mit Xamarin.Mac zu erstellen:

- Ein Mac, auf dem macOS Sierra (10,12) oder höher ausgeführt wird.
- Die neueste Version von Xcode (installiert aus dem [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Die neueste Version von xamarin. Mac und Visual Studio für Mac

Für das Ausführen von mit Xamarin.Mac erstellten Mac-Anwendungen müssen die folgenden Systemanforderungen erfüllt werden:

- Ein Mac, der Mac OS X 10,7 oder höher ausgeführt wird.

## <a name="compilation"></a>Kompilierung

Wenn Sie eine xamarin Platform-Anwendung kompilieren, wird C# der Mono F#-Compiler (oder) ausgeführt, und C# der F# Compiler und der Code werden in Microsoft Intermediate Language (MSIL oder Il) kompiliert. Xamarin. Mac verwendet dann einen *Just-in-Time-Compiler (JIT)* zur Laufzeit, um systemeigenen Code zu kompilieren, sodass bei Bedarf die Ausführung in der richtigen Architektur möglich ist.

Dies steht im Gegensatz zu xamarin. IOS, bei dem die AOT-Kompilierung verwendet wird. Bei Verwendung des AOT-Compilers werden alle Assemblys und alle darin enthaltenen Methoden zur Buildzeit kompiliert. Mit JIT erfolgt die Kompilierung nur bei Bedarf für die Methoden, die ausgeführt werden.

Mit xamarin. Mac-Anwendungen ist mono in der Regel in die APP Bundle eingebettet (und als **eingebettetes Mono**bezeichnet). Wenn Sie die klassische xamarin. Mac-API verwenden, könnte die Anwendung stattdessen **System Mono**verwenden, dies wird jedoch im Unified API nicht unterstützt. System Mono bezieht sich auf Mono, das im Betriebssystem installiert ist. Beim Starten der Anwendung wird dies von der xamarin. Mac-App verwendet.

## <a name="selectors"></a>Selektoren

Mit xamarin verfügen wir über zwei separate Ökosysteme, .net und Apple, die wir zusammenbringen müssen, um so optimiert zu werden, dass das Endziel eine reibungslose Benutzerfunktion ist. Wir haben im obigen Abschnitt erläutert, wie die beiden Laufzeiten kommunizieren, und vielleicht haben Sie den Begriff "Bindungen" gehört, der die Verwendung der nativen Mac-APIs in xamarin ermöglicht. Bindungen werden in der [Ziel-C-Bindungs Dokumentation](~/mac/platform/binding.md)ausführlich erläutert. wir sehen uns nun an, wie xamarin. Mac im Hintergrund funktioniert.

Zuerst muss eine Möglichkeit zum verfügbar machen von Ziel-C für C#verfügbar sein, was über Selektoren erreicht wird. Eine Auswahl ist eine Nachricht, die an ein Objekt oder eine Klasse gesendet wird. Mit "Ziel-C" erfolgt dies über die [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) -Funktionen. Weitere Informationen zur Verwendung von Selectors finden Sie im Leitfaden für IOS [-Ziel-C-Selektoren](~/ios/internals/objective-c-selectors.md) . Außerdem muss verwalteter Code für "Ziel-c" verfügbar gemacht werden, was komplizierter ist, da "Ziel-c" nichts über den verwalteten Code weiß. Um dieses Problem zu umgehen, wird eine [Registrierungs](~/mac/internals/registrar.md)Stelle verwendet. Dies wird im nächsten Abschnitt ausführlicher erläutert.

## <a name="registrar"></a>Registrierung

Wie bereits erwähnt, ist die Registrierungsstelle Code, der verwalteten Code für "Ziel-C" verfügbar macht. Hierzu wird eine Liste aller verwalteten Klassen erstellt, die von NSObject abgeleitet werden:

- Für alle Klassen, die keine vorhandene Ziel-c-Klasse umwickeln, wird eine neue Ziel-c-Klasse erstellt, wobei die Ziel-c-Member alle verwalteten Member `[Export]` mit einem-Attribut spiegeln.
- In den Implementierungen für jedes Ziel – C-Member wird automatisch Code hinzugefügt, um den gespiegelten verwalteten Member aufzurufen.

Der folgende Pseudo Code zeigt ein Beispiel für die Vorgehensweise:

**C#(verwalteter Code):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Ziel-C (nativer Code):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

Der verwaltete Code kann die Attribute `[Register]` und `[Export]`enthalten, die von der Registrierungsstelle verwendet werden, um zu wissen, dass das Objekt für "Ziel-C" verfügbar gemacht werden muss. Das [Register]-Attribut wird verwendet, um den Namen der generierten Ziel-C-Klasse anzugeben, falls der generierte Standardname nicht geeignet ist. Alle von NSObject abgeleiteten Klassen werden automatisch bei Ziel-C registriert. Das erforderliche [Export]-Attribut enthält eine Zeichenfolge, bei der es sich um den in der generierten Ziel-C-Klasse verwendeten Selektor handelt.

Es gibt zwei Typen von Registrierungsstellen, die in xamarin. Mac – Dynamic und static verwendet werden:

- Dynamische Registrare – Dies ist die Standard Registrierungsstelle für alle xamarin. Mac-Builds. Die dynamische Registrierungsstelle führt die Registrierung aller Typen in der Assembly zur Laufzeit durch. Dies erfolgt mithilfe von Funktionen, die von der Runtime-API von Ziel-C bereitgestellt werden. Die dynamische Registrierungsstelle hat daher einen langsameren Start, aber eine schnellere Buildzeit. Native Funktionen (normalerweise in C), die als "Trampolines" bezeichnet werden, werden als Methoden Implementierungen verwendet, wenn die dynamischen Registrare verwendet werden. Sie variieren zwischen verschiedenen Architekturen.
- Statische Registrierungsstellen – die statische Registrierungsstelle generiert den Ziel-C-Code während des Builds, der dann in eine statische Bibliothek kompiliert und mit der ausführbaren Datei verknüpft wird. Dies ermöglicht einen schnelleren Start, dauert aber während der Buildzeit länger.

## <a name="application-launch"></a>Anwendungsstart

Die xamarin. Mac-Start Logik unterscheidet sich abhängig davon, ob Embedded oder System Mono verwendet wird. Wenn Sie den Code und die Schritte für das Starten der xamarin. Mac-Anwendung anzeigen möchten, finden Sie weitere Informationen in der [Start Header](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) Datei im öffentlichen Repository von xamarin-macios.

## <a name="generator"></a>Generator

Xamarin. Mac enthält Definitionen für jede Mac-API. Sie können diese im [macios-GitHub](https://github.com/xamarin/xamarin-macios/tree/master/src)-Repository durchsuchen. Diese Definitionen enthalten Schnittstellen mit Attributen sowie alle notwendigen Methoden und Eigenschaften. Der folgende Code wird z. b. verwendet, um eine NSBox im [AppKit-Namespace](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)zu definieren. Beachten Sie, dass es sich um eine Schnittstelle mit einer Reihe von Methoden und Eigenschaften handelt:

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

    …

    [Export ("borderRect")]
    CGRect BorderRect { get; }

    [Export ("titleRect")]
    CGRect TitleRect { get; }

    [Export ("titleCell")]
    NSObject TitleCell { get; }

    [Export ("sizeToFit")]
    void SizeToFit ();

    [Export ("contentViewMargins")]
    CGSize ContentViewMargins { get; set; }

    [Export ("setFrameFromContentFrame:")]
    void SetFrameFromContentFrame (CGRect contentFrame);

    …

}
```

Der in xamarin. Mac aufgerufene `bmac` Generator übernimmt diese Definitions Dateien und verwendet .NET-Tools, um Sie in eine temporäre Assembly zu kompilieren. Diese temporäre Assembly kann jedoch nicht zum Aufruf von Ziel-C-Code aufgerufen werden. Der Generator liest dann die temporäre Assembly und generiert C# Code, der zur Laufzeit verwendet werden kann. Wenn Sie z. b. ein zufälliges Attribut zu ihrer Definition. cs-Datei hinzufügen, wird es im ausgegebenen Code nicht angezeigt. Der Generator weiß es nicht, und er weiß `bmac` daher nicht, dass er in der temporären Assembly suchen muss, um ihn auszugeben.

Nachdem xamarin. Mac. dll erstellt wurde, bündelt der Packager `mmp`alle Komponenten zusammen.

Auf hoher Ebene wird dies erreicht, indem die folgenden Aufgaben ausgeführt werden:

- Erstellen Sie eine APP Bundle Struktur.
- Kopieren Sie die verwalteten Assemblys.
- Wenn die Verknüpfung aktiviert ist, führen Sie den verwalteten Linker aus, um die Assemblys zu optimieren, indem Sie nicht verwendete
- Erstellen Sie eine Start Programm Anwendung, und verknüpfen Sie im Start Programmcode, der im Zusammenhang mit dem Registrierungscode gesprochen wird, im statischen Modus.

Diese wird dann im Rahmen des benutzerbuildprozesses ausgeführt, der Benutzercode in eine Assembly kompiliert, die `mmp` auf xamarin. Mac. dll verweist und ausgeführt wird, um Sie zu einem Paket zu machen.

Ausführlichere Informationen zum Linker und seiner Verwendung finden Sie im IOS [Linker](~/ios/deploy-test/linker.md) Guide.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch befasst sich mit der Kompilierung von xamarin. Mac-apps und untersucht xamarin. Mac und seine Beziehung zu "Ziel-C".
