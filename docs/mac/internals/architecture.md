---
title: Xamarin.Mac-Architektur
description: Dieses Handbuch beschreibt Xamarin.Mac und dessen Beziehung mit Objective-C auf niedriger Ebene. Es wird erläutert Konzepte, z. B. der Kompilierung, Selektoren, Registrierungsstellen, app-Start und des Generators.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 04/12/2017
ms.openlocfilehash: 1ea38b527acaa89b9f25690de4e55664a7afd9e8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034128"
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac-Architektur

_Dieses Handbuch beschreibt Xamarin.Mac und dessen Beziehung mit Objective-C auf niedriger Ebene. Es wird erläutert Konzepte, z. B. der Kompilierung, Selektoren, Registrierungsstellen, app-Start und des Generators._

## <a name="overview"></a>Übersicht

Xamarin.Mac-Anwendungen in die Mono-ausführungsumgebung ausgeführt, und der Compiler von Xamarin auf Intermediate Language (IL) kompiliert, dies ist dann Just-in-Time (JIT) kompiliert, die in systemeigenen Code zur Laufzeit verwenden. Dadurch wird Seite-an-Seite ausgeführt, mit der Objective-C-Laufzeit. Beide Umgebungen von Common Language Runtime auf einem UNIX-ähnlichen-Kernel, insbesondere XNU, ausgeführt und verfügbar machen verschiedene APIs für den Benutzercode und ermöglichen es Entwicklern den Zugriff auf die zugrunde liegenden systemeigenen oder verwalteten System.

Das folgende Diagramm zeigt eine grundlegende Übersicht über diese Architektur:

[![Das Diagramm zeigt eine grundlegende Übersicht über die Architektur](architecture-images/mac-arch.png "das Diagramm zeigt eine grundlegende Übersicht über die Architektur")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Nativem und verwaltetem code

Bei der Entwicklung für Xamarin, die Begriffe *native* und *verwaltet* Code werden häufig verwendet. Verwalteter Code ist Code, die die Ausführung von .NET Framework Common Language Runtime oder im Fall von Xamarin verwaltet: der Mono-Laufzeit.

Systemeigene Code ist Code, der auf die jeweilige Plattform (z. B. Objective-C oder sogar per AOT kompiliert Code auf einem ARM-Chip) nativ ausgeführt wird. Dieses Handbuch beschreibt wie der verwaltete Code in systemeigenen Code kompiliert wird, und erläutert, wie ein Xamarin.Mac-Anwendung funktioniert, voll ausgelastet Apple Mac-APIs durch die Verwendung von Bindungen, und gleichzeitig den Zugriff auf. NET BCL und eine ausgereifte Sprache wie z. B. C#.

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um eine macOS-Anwendung mit Xamarin.Mac zu erstellen:

- Ein Mac mit MacOS Sierra (10.12) oder höher.
- Die neueste Version von Xcode (aus installiert die [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Die neueste Version von Xamarin.Mac und Visual Studio für Mac

Für das Ausführen von mit Xamarin.Mac erstellten Mac-Anwendungen müssen die folgenden Systemanforderungen erfüllt werden:

- Einen Mac, Mac OS X 10.7 oder höher ausführen.

## <a name="compilation"></a>Kompilierung

Beim Kompilieren von alle Xamarin-Plattform-Anwendung, die Mono C# (oder F#) Compiler laufen und kompiliert Ihre C# und F# Code in Microsoft Intermediate Language (MSIL oder IL). Xamarin.Mac verwendet dann ein *Just-in-Time (JIT)* Compiler zur Laufzeit zum Kompilieren von nativem Code können die Ausführung auf die richtige Architektur, je nach Bedarf.

Dies steht im Gegensatz zu Xamarin.iOS der AOT-Kompilierung verwendet. Wenn den AOT-Compiler zu verwenden, werden alle Assemblys und alle darin enthaltenen-Methoden zum Zeitpunkt der Erstellung kompiliert. Kompilierung wird mit JIT-Kompilierung bei Bedarf nur für die Methoden, die ausgeführt werden.

Mit Xamarin.Mac-Anwendungen, Mono in der Regel in das app-Bündel eingebettet ist (bezeichnet als **eingebettete Mono**). Wenn Sie die klassische Xamarin.Mac-API zu verwenden, wird die Anwendung könnte stattdessen verwenden **System Mono**, jedoch wird dies nicht unterstützt, in der Unified API. System Mono bezieht sich auf Mono, die im Betriebssystem installiert wurde. Anwendung starten werden die Xamarin.Mac-app verwenden.

## <a name="selectors"></a>Selektoren

Mit Xamarin, haben wir zwei separate Ökosysteme, .NET und Apple, die wir benötigen Sie zusammen, um wie möglich sein, um sicherzustellen, dass das Endziel ein nahtloses Benutzererlebnis ist als optimierte erscheinen. Wir haben gesehen, im Abschnitt über die zwei Laufzeiten wie kommunizieren, und Sie sehr gut haben gehört des Begriffs "Bindungen", wodurch die native Mac-APIs, die in Xamarin verwendet werden. Bindungen werden ausführlich im erläutert die [Objective-C-Bindung Dokumentation](~/mac/platform/binding.md), sodass Sie jetzt sehen Sie die Funktionsweise von Xamarin.Mac im Hintergrund.

Zuerst, es muss eine Möglichkeit zum Bereitstellen von Objective-C nach C#, das erfolgt über Selektoren. Auswahl wird eine Meldung, die an ein Objekt oder eine Klasse gesendet wird. Mit Objective-C erfolgt dies über die [Objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) Funktionen. Weitere Informationen zur Verwendung von Selektoren finden Sie in der iOS [Objective-C-Selektoren](~/ios/internals/objective-c-selectors.md) Guide. Es verfügt außerdem über eine Möglichkeit, verfügbar zu machen verwalteten Code mit Objective-C, die ist komplizierter, aufgrund der Tatsache, dass Objective-C nichts über den verwalteten Code nicht bekannt ist. Um dies zu umgehen, verwenden wir eine [Registrierungsstelle](~/mac/internals/registrar.md). Dies ausführlicher im nächsten Abschnitt.

## <a name="registrar"></a>Registrierung

Wie bereits erwähnt, ist die Registrierungsstelle, verwalteter Code macht Objective-c-code Hierzu erstellen eine Liste von jeder verwaltete Klasse, die von NSObject abgeleitet wird:

- Für alle Klassen, die keine vorhandene Objective-C-Klasse umschlossen werden, erstellt er eine neue Objective-C-Klasse mit Objective-C-Elemente, spiegeln alle verwalteten Elemente, die eine `[Export]` Attribut.
- In den Implementierungen für jedes Objective-C-Element wird automatisch Code hinzugefügt, das gespiegelte verwaltete Element aufrufen.

Der folgende Pseudocode zeigt ein Beispiel ist dies:

**C#(verwalteter Code):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective-C (nativer Code):**

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

Der verwaltete Code kann die Attribute enthalten `[Register]` und `[Export]`, die die Registrierungsstelle verwendet wird, zu wissen, dass das Objekt für Objective-c verfügbar gemacht werden muss Das [registrieren]-Attribut wird verwendet, um den Namen der generierten Objective-C-Klasse angeben, für den Fall, dass Sie der generierten Standardnamen nicht geeignet ist. Alle von NSObject abgeleitete Klassen werden automatisch mit Objective-c registriert. Das erforderliche [Export]-Attribut enthält eine Zeichenfolge, die die Auswahl in der generierten Objective-C-Klasse verwendet wird.

Es gibt zwei Arten von Registrierungsstellen xamarin.Mac – dynamische und statische verwendet:

- Dynamische Registrierungsstellen – Dies ist der Standard-Registrierungsstelle für alle Xamarin.Mac-Builds. Die dynamische Registrierung wird die Registrierung aller Typen in der Assembly zur Laufzeit. Hierzu werden mithilfe von Objective-C-Laufzeit-API bereitgestellte Funktionen. Die dynamische Registrierung enthält daher einen langsameren Start, aber eine schnellere Erstellung. Aufrufen systemeigene Funktionen (normalerweise in C), Trampolines, werden als Methoden verwendet, bei Verwendung der dynamischen Registrierungsstellen. Sie haben verschiedene Architekturen variieren.
- Statische Registrierungsstellen – wird die statische Registrierungsstelle Objective-C-Code während des Builds, die dann in einer statischen Bibliothek kompiliert und verknüpft, die in die ausführbare Datei generiert. Dies ermöglicht einen schnelleren Start, aber es dauert länger, bei der Erstellung.

## <a name="application-launch"></a>Starten von Anwendungen

Die Startlogik für Xamarin.Mac variieren abhängig, ob eingebettet oder System Mono verwendet wird. Um den Code und die Schritte zum Starten von Xamarin.Mac-Anwendungen anzuzeigen, finden Sie in der [starten Header](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) -Datei in die Xamarin-Macios öffentliches Repository.

## <a name="generator"></a>Generator

Xamarin.Mac enthält Definitionen für alle Mac-APIs. Durchsuchen Sie diese auf die [MaciOS Github-Repository](https://github.com/xamarin/xamarin-macios/tree/master/src). Diese Definitionen enthalten Schnittstellen mit Attributen, als auch alle erforderlichen Methoden und Eigenschaften. Z. B. der folgende Code wird zum Definieren einer NSBox in die [AppKit Namespace](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Beachten Sie, dass es sich um eine Schnittstelle mit einer Reihe von Methoden und Eigenschaften ist:

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

Der Generator, dem Namen `bmac` xamarin.Mac, nimmt diese Definitionsdateien und Tools für .NET verwendet, in eine temporäre Assembly kompiliert. Diese temporäre Assembly ist jedoch nicht zum Aufrufen von Objective-C-Code verwendet. Der Generator klicken Sie dann die temporäre Assembly liest und generiert C# Code, der zur Laufzeit verwendet werden kann. Dies ist die Gründe, z. B. Wenn Sie ein random-Attribut der Definition cs-Datei hinzufügen, sie in der ausgegebene Code nicht angezeigt. Der Generator nicht erkennbar, und daher `bmac` weiß nicht, in der temporären Assembly, die sie ausgeben sucht.

Sobald die "xamarin.Mac.dll" die Objekt-Manager erstellt wurde, `mmp`, werden alle Komponenten zusammen bündeln.

Im Allgemeinen erreicht sie dies durch die folgenden Aufgaben ausführen:

- Erstellen einer app-Bundle-Struktur.
- Kopieren Sie in Ihrer verwalteten Assemblys.
- Wenn linking aktiviert ist, führen Sie die verwaltete Linker aus, um Ihre Assemblys zu optimieren, indem Sie das Entfernen von nicht genutzten Bereichen.
- Erstellen Sie eine Startprogramm-Anwendung, die im Code Startprogramm gesprochen, die zusammen mit dem Registrierungscode im statischen Modus verknüpfen.

Dies ist, und führen Sie als Teil des Benutzers erstellen Prozess, der Benutzercode in einer Assembly kompiliert wird, des Verweises "xamarin.Mac.dll" und führt dann `mmp` zum Vereinfachen eines Pakets

Ausführlichere Informationen zu den Linker und wie diese verwendet werden, finden Sie in der iOS [Linker](~/ios/deploy-test/linker.md) Guide.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch gesucht, bei der Kompilierung von Xamarin.Mac-apps, und untersucht Xamarin.Mac und die Beziehung zu Objective-c
