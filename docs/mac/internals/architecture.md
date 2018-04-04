---
title: Xamarin.Mac architecture
description: Dieses Handbuch untersucht Xamarin.Mac und seine Beziehung zu Objective-C auf niedriger Ebene. Es wird erläutert, Konzepten wie der Kompilierung, Selektoren Registrierungsstellen, app-Start und den Generator.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac architecture

_Dieses Handbuch untersucht Xamarin.Mac und seine Beziehung zu Objective-C auf niedriger Ebene. Es wird erläutert, Konzepten wie der Kompilierung, Selektoren Registrierungsstellen, app-Start und den Generator._

## <a name="overview"></a>Übersicht

Xamarin.Mac Anwendungen innerhalb der Mono-ausführungsumgebung ausgeführt, und verwenden die Xamarin Compiler auf Intermediate Language (IL) kompiliert, dies ist dann Just-in-Time (JIT) in nativen Code zur Laufzeit kompiliert. Dadurch wird Seite-an-Seite ausgeführt, mit der Objective-C-Laufzeit. Beide Laufzeitumgebungen auf einem UNIX-ähnlichen-Kernel, insbesondere XNU, ausführen und Verfügbarmachen von verschiedenen APIs für die es Entwicklern ermöglicht, den Zugriff auf die zugrunde liegenden systemeigenen oder verwalteten System Benutzercode.

Das folgende Diagramm zeigt eine grundlegende Übersicht über diese Architektur:

[![Eine grundlegende Übersicht über die Architektur für Diagramm](architecture-images/mac-arch.png "Diagramm eine grundlegende Übersicht über die Architektur")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Systemeigenem und verwaltetem code

Bei der Entwicklung für Xamarin, die Begriffe *native* und *verwaltet* Code häufig verwendet werden. Verwalteter Code ist Code, der die Ausführung von .NET Framework Common Language Runtime oder in die Xamarin Groß-/Kleinschreibung verwaltet aufweist: die Mono-Laufzeit.

Systemeigener Code ist Code, der direkt auf die jeweilige Plattform (z. B. Objective-C oder sogar AOT kompilierten Code auf einem ARM-Chip) ausgeführt wird. Dieses Handbuch wird erklärt, wie verwaltete Code in systemeigenen Code kompiliert wird, und erläutert, wie eine Xamarin.Mac Anwendung funktioniert ausführenden Apple Macintosh-APIs durch die Verwendung von Bindungen, während Sie auch den Zugriff auf eine optimal nutzen. NET BCL und eine anspruchsvolle Sprache wie z. B. c#.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um eine macOS-Anwendung mit Xamarin.Mac zu erstellen:

- Ein Mac ausgeführten MacOS Sierra (10.12) oder größer.
- Die neueste Version von Xcode (aus installiert die [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Die neueste Version von Xamarin.Mac und Visual Studio für Mac

Für das Ausführen von mit Xamarin.Mac erstellten Mac-Anwendungen müssen die folgenden Systemanforderungen erfüllt werden:

- Einem Mac, Mac OS X 10,7 oder höher ausgeführt.

## <a name="compilation"></a>Kompilierung

Wenn Sie eine Xamarin-Plattform-Anwendung kompilieren, wird der Compiler Mono c# (oder f#) wird ausgeführt, und Ihrer c# und f#-Code in Microsoft Intermediate Language (MSIL oder IL) kompiliert. Xamarin.Mac verwendet dann eine *Just-in-Time (JIT)* Compiler zur Laufzeit zum Kompilieren von systemeigenem Code, ermöglicht die Ausführung auf die richtige Architektur nach Bedarf.

Dies ist im Gegensatz zu Xamarin.iOS der AOT Kompilierung verwendet. Wenn den Compiler AOT verwenden zu können, werden alle Assemblys und alle Methoden in ihnen während des Buildvorgangs kompiliert. Mit JIT geschieht, Kompilierung bei Bedarf nur für die Methoden, die ausgeführt werden.

Mit Xamarin.Mac-Anwendungen ist normalerweise Mono in der app-Bündel eingebettet (vorgelegt **eingebettete Mono**). Wenn Sie die klassische Xamarin.Mac-API verwenden, wird die Anwendung konnte stattdessen verwenden **System Mono**, jedoch wird dies nicht unterstützt, in der Unified-API. System Mono bezieht sich auf Mono, die im Betriebssystem installiert wurde. Beim Anwendungsstart verwenden die app Xamarin.Mac dies.

## <a name="selectors"></a>Selektoren

Xamarin, haben wir zwei separate Ökosysteme .NET und Apple, die wir benötigen zusammen, um scheint als optimierte wie möglich sein, um sicherzustellen, dass das Ziel eine reibungslose benutzererfahrung. Wir haben gesehen, im Abschnitt über die zwei Laufzeiten wie kommunizieren, und Sie möglicherweise sehr gut kenne des Begriffs "Bindungen" aus der systemeigenen Mac-APIs in Xamarin verwendet werden kann. Bindungen werden ausführlich im erläutert die [Objective-C-Bindung Dokumentation](~/mac/platform/binding.md), sodass jetzt untersuchen wir die Funktionsweise von Xamarin.Mac hinter den Kulissen.

Es muss zuerst eine Herangehensweise an Objective-C zu C#-, verfügbar macht, die über Selektoren vorgenommen wird. Ein Selektor ist eine Nachricht, die auf ein Objekt oder eine Klasse gesendet wird. Mit Objective-C erfolgt dies über die [Objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) Funktionen. Weitere Informationen zur Verwendung von Selektoren, finden Sie in der iOS [Objective-C-Selektoren](~/ios/internals/objective-c-selectors.md) Handbuch. Es muss außerdem eine Herangehensweise an das Verfügbarmachen von verwalteten Codes für Objective-C, die ist komplizierter, Objective-C nichts über den verwalteten Code nicht bekannt ist. Um dies zu umgehen, verwenden wir eine [Registrierungsstelle](~/mac/internals/registrar.md). Dadurch werden ausführlicher im nächsten Abschnitt.

## <a name="registrar"></a>Registrierung

Wie bereits erwähnt, ist die Registrierungsstelle, verwalteter Code macht Objective-c-code Dies geschieht durch Erstellen einer Liste von jeder verwalteten Klasse, die von NSObject abgeleitet wird:

- Für alle Klassen, die keine vorhandene Objective-C-Klasse umschlossen werden, erstellt er eine neue Objective-C-Klasse mit Objective-C-Elemente, die alle verwalteten Member mit Spiegelung ein `[Export]` Attribut.
- In den Implementierungen für jedes Objective-C-Element wird automatisch Code hinzugefügt, den gespiegelten verwalteten Member aufrufen.

Der folgende Pseudocode zeigt ein Beispiel, wie dies funktioniert:

**C# (verwalteter Code):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective-C (systemeigener Code):**

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

Der verwaltete Code kann die Attribute enthalten `[Register]` und `[Export]`, die die Registrierungsstelle verwendet wird, zu wissen, dass das Objekt für Objective-c verfügbar gemacht werden muss Das Attribut [registrieren] wird verwendet, um den Namen der generierten Klasse Objective-C anzugeben, für den Fall, dass Sie der Standardnamen generiert nicht geeignet ist. Alle von NSObject abgeleitete Klassen werden automatisch mit Objective-c registriert. Das erforderliche [Export]-Attribut enthält eine Zeichenfolge, die die Auswahl in der generierten Objective-C-Klasse verwendet wird.

Es gibt zwei Arten von Registrierungsstellen in Xamarin.Mac – dynamische und statische verwendet:

- Dynamische Registrierungsstellen – Dies ist die Standard-Registrierungsstelle für alle Xamarin.Mac-Builds. Die dynamische Registrierungsstelle wird die Registrierung aller Typen in der Assembly zur Laufzeit. Dies geschieht mithilfe von Objective-C-Laufzeit-API bereitgestellten Funktionen. Die dynamische Registrierungsstelle weist daher einen langsameren Start, aber eine schnellere Time erstellen. Aufrufen systemeigene Funktionen (normalerweise in C), Trampolines, werden als Implementierungen der Dienstmethode verwendet, bei Verwendung der dynamischen Registrierungsstellen. Sie variieren in verschiedenen Architekturen.
- Statische Registrierungsstellen – generiert die statische Registrierungsstelle Objective-C-Code während des Builds, die dann in einer statischen Bibliothek kompiliert und in die ausführbare Datei verknüpft. Dies ermöglicht einen schnelleren Start, jedoch während des Buildvorgangs länger dauert.

## <a name="application-launch"></a>Anwendung starten

Startlogik für Xamarin.Mac variieren je nach, ob eingebettet oder System Mono verwendet wird. Um den Code und die Schritte für den Anwendungsstart Xamarin.Mac anzuzeigen, finden Sie in der [starten Header](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) -Datei im Xamarin-Macios öffentliche Repository.

## <a name="generator"></a>Generator

Xamarin.Mac enthält Definitionen für jede Mac-API. Durchsuchen Sie diese auf die [MaciOS Github-Repository](https://github.com/xamarin/xamarin-macios/tree/master/src). Diese Definitionen enthalten, Schnittstellen, Attribute, als auch alle erforderlichen Methoden und Eigenschaften. Der folgende Code ist z. B. dient zum Definieren einer NSBox in der [AppKit Namespace](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Beachten Sie, dass es sich um eine Schnittstelle mit einer Reihe von Methoden und Eigenschaften ist:

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

Der Generator aufgerufen `bmac` in Xamarin.Mac, diesen Definitionsdateien und benötigt .NET Tools diese in eine temporäre Assembly zu kompilieren. Dieser temporäre Assembly ist jedoch nicht für nutzbare Objective-C-Code aufrufen. Der Generator klicken Sie dann die temporäre Assembly liest und generiert C#-Code, der zur Laufzeit verwendet werden kann. Dies ist die Warum, z. B. Wenn Sie Ihre Definition cs-Datei einen zufälligen Attribut hinzugefügt haben, es in der ausgegebene Code angezeigt wird nicht. Der Generator ist nicht bekannt, und daher `bmac` weiß nicht, dafür gesucht werden soll, in der temporären Assembly, die mit ihm ausgegeben werden.

Sobald die Xamarin.Mac.dll den Packager erstellt wurde, `mmp`, werden alle Komponenten zusammen bündeln.

Auf hoher Ebene erreicht er dies durch die folgenden Aufgaben ausführen:

- Erstellen einer app-Bundle-Struktur.
- Kopieren Sie die verwalteten Assemblys.
- Wenn Verknüpfen aktiviert ist, führen Sie die verwalteten Linker aus, um die Assemblys zu optimieren, indem Sie nicht verwendete Komponenten entfernen.
- Erstellen Sie eine Startprogramm-Anwendung, in den Startprogramm Code erläutert, die zusammen mit dem Registar Code befindet sich im Netzwerkmodus mit statischer verknüpfen.

Dies ist und als Teil des Benutzers ausführen erstellen Prozess, der Benutzercode in einer Assembly kompiliert wird, diesen Verweis Xamarin.Mac.dll und führt dann `mmp` machen ein Paket

Für ausführlichere Informationen zu den Linker und wie diese verwendet werden, finden Sie in der iOS [Linker](~/ios/deploy-test/linker.md) Handbuch.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, die Kompilierung von Xamarin.Mac-apps und untersucht Xamarin.Mac und seine Beziehung zu Objective-c
