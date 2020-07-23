---
title: IOS-App-Architektur
description: In diesem Dokument wird xamarin. IOS auf niedriger Ebene beschrieben und erläutert, wie System eigener und verwalteter Code interagieren, AOT-Kompilierungen, Selektoren, Registrare, Anwendungs Starts und der Generator.
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 832071023bfe2ea9d947946ff2b70428d93fc866
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937538"
---
# <a name="ios-app-architecture"></a>IOS-App-Architektur

Xamarin. IOS-Anwendungen werden innerhalb der Mono-Ausführungsumgebung ausgeführt und verwenden die vollständige vorab Kompilierung (AOT), um c#-Code in die Arm-Assemblysprache zu kompilieren. Dies wird parallel zur [Ziel-C-Laufzeit](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)ausgeführt. Beide Laufzeitumgebungen werden auf einem UNIX-ähnlichen Kernel, insbesondere [XNU](https://en.wikipedia.org/wiki/XNU), ausgeführt und stellen verschiedene APIs für den Benutzercode bereit, sodass Entwickler auf das zugrunde liegende systemeigene oder verwaltete System zugreifen können.

Das folgende Diagramm zeigt eine grundlegende Übersicht über diese Architektur:

[![Dieses Diagramm zeigt eine grundlegende Übersicht über die Architektur der Vorkompilierung (Ahead of Time, AOT).](architecture-images/ios-arch-small.png)](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>Nativer und verwalteter Code: eine Erläuterung

Bei der Entwicklung für xamarin werden die Begriffe System eigener *und verwalteter* Code häufig verwendet. [Verwalteter Code](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) ist Code, dessen Ausführung von der [.NET Framework Common Language Runtime](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)verwaltet wird, oder im Fall von xamarin: der Mono-Laufzeit. Dies wird als zwischen Sprache bezeichnet.

Bei System eigenem Code handelt es sich um Code, der auf der jeweiligen Plattform nativ ausgeführt wird (z. b. mit Ziel-C oder sogar AOT-kompiliertem Code auf einem Arm-Chip). In dieser Anleitung wird erläutert, wie AOT Ihren verwalteten Code in systemeigenen Code kompiliert und erläutert, wie eine xamarin. IOS-Anwendung funktioniert, wobei die IOS-APIs von Apple durch die Verwendung von Bindungen vollständig genutzt werden, während gleichzeitig auf zugegriffen werden kann. BCL von NET und eine ausgereifte Sprache wie z. b. c#.

## <a name="aot"></a>AOT

Wenn Sie eine xamarin Platform-Anwendung kompilieren, wird der Mono c#-Compiler (oder f #) ausgeführt, und der c#-und f #-Code wird in Microsoft Intermediate Language (MSIL) kompiliert. Wenn Sie eine xamarin. Android-Anwendung, eine xamarin. Mac-Anwendung oder sogar eine xamarin. IOS-Anwendung im Simulator ausführen, kompiliert die [.NET Common Language Runtime (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) die MSIL mit einem JIT-Compiler (Just in Time). Zur Laufzeit wird dieser in einen systemeigenen Code kompiliert, der in der richtigen Architektur für die Anwendung ausgeführt werden kann.

Für IOS gibt es jedoch eine Sicherheits Einschränkung, die von Apple festgelegt wird und die Ausführung von dynamisch generiertem Code auf einem Gerät nicht zulässt.
Um sicherzustellen, dass diese Sicherheitsprotokolle befolgt werden, verwendet xamarin. IOS stattdessen einen AOT-Compiler (Ahead-of-Time), um den verwalteten Code zu kompilieren. Dies erzeugt eine native IOS-Binärdatei, die optional mit llvm für Geräte optimiert wird und auf dem ARM-basierten Prozessor von Apple bereitgestellt werden kann. Eine grobe Darstellung der kombinieren dieser Vorgehensweise ist im folgenden dargestellt:

[![Ein grobes Diagramm der kombinieren dieser kombinieren](architecture-images/aot.png)](architecture-images/aot-large.png#lightbox)

Die Verwendung von AOT weist eine Reihe von Einschränkungen auf, die im Leitfaden [Einschränkungen](~/ios/internals/limitations.md) ausführlich erläutert werden. Außerdem bietet es eine Reihe von Verbesserungen im Vergleich zu JIT durch eine Verringerung der Startzeit und verschiedene Leistungsoptimierungen.

Nun, da wir untersucht haben, wie der Code von der Quelle in systemeigenen Code kompiliert wird, sehen wir uns die Kulissen an, um zu sehen, wie xamarin. IOS es uns ermöglicht, vollständig Native IOS-Anwendungen zu schreiben.

## <a name="selectors"></a>Selektoren

Mit xamarin verfügen wir über zwei separate Ökosysteme, .net und Apple, die wir zusammenbringen müssen, um so optimiert zu werden, dass das Endziel eine reibungslose Benutzerfunktion ist. Wir haben im obigen Abschnitt erläutert, wie die beiden Laufzeiten kommunizieren, und vielleicht haben Sie den Begriff "Bindungen" gehört, der die Verwendung der nativen IOS-APIs in xamarin ermöglicht. Bindungen werden in unserer [Ziel-C-Bindungs](~/cross-platform/macios/binding/overview.md) Dokumentation ausführlich erläutert. wir sehen uns nun an, wie IOS im Hintergrund arbeitet.

Zunächst müssen Sie eine Möglichkeit haben, "Ziel-C" für c# verfügbar zu machen. Dies erfolgt über Selektoren. Eine Auswahl ist eine Nachricht, die an ein Objekt oder eine Klasse gesendet wird. Mit "Ziel-C" erfolgt dies über die [objc_msgSend](~/cross-platform/macios/binding/overview.md) Funktionen.
Weitere Informationen zur Verwendung von Selectors finden Sie im Leitfaden zu den [Ziel-C-Selektoren](~/ios/internals/objective-c-selectors.md) . Außerdem muss verwalteter Code für "Ziel-c" verfügbar gemacht werden, was komplizierter ist, da "Ziel-c" nichts über den verwalteten Code weiß. Um dies zu umgehen, verwenden wir *Registrars*. Diese werden im nächsten Abschnitt ausführlicher erläutert.

## <a name="registrars"></a>Registrierungsstellen

Wie bereits erwähnt, ist die Registrierungsstelle Code, der verwalteten Code für "Ziel-C" verfügbar macht. Hierzu wird eine Liste aller verwalteten Klassen erstellt, die von NSObject abgeleitet werden:

- Für alle Klassen, die keine vorhandene Ziel-c-Klasse umwickeln, wird eine neue Ziel-c-Klasse erstellt, wobei die Ziel-c-Member alle verwalteten Member mit einem [ `Export` ]-Attribut spiegeln.

- In den Implementierungen für jedes Ziel – C-Member wird automatisch Code hinzugefügt, um den gespiegelten verwalteten Member aufzurufen.

Der folgende Pseudo Code zeigt ein Beispiel für die Vorgehensweise:

**C# (verwalteter Code)**

```csharp
 class MyViewController : UIViewController{
     [Export ("myFunc")]
     public void MyFunc ()
     {
     }
 }
```

**Ziel-C:**

```objectivec
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end

@implementation MyViewController {}

    -(void) myFunc
    {
        /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

Der verwaltete Code kann die Attribute und enthalten, die `[Register]` `[Export]` von der Registrierungsstelle verwendet werden, um zu wissen, dass das Objekt für "Ziel-C" verfügbar gemacht werden muss.
Das- `[Register]` Attribut wird verwendet, um den Namen der generierten Ziel-C-Klasse anzugeben, falls der generierte Standardname nicht geeignet ist. Alle von NSObject abgeleiteten Klassen werden automatisch bei Ziel-C registriert.
Das erforderliche `[Export]` Attribut enthält eine Zeichenfolge, bei der es sich um den in der generierten Ziel-C-Klasse verwendeten Selektor handelt.

In xamarin. IOS werden zwei Arten von Registrierungsstellen verwendet – dynamisch und statisch:

- **Dynamische** Registrierungsstellen – die dynamische Registrierungsstelle führt die Registrierung aller Typen in der Assembly zur Laufzeit durch. Dies erfolgt mithilfe von Funktionen, die von [der Runtime-API von Ziel-C](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)bereitgestellt werden. Die dynamische Registrierungsstelle hat daher einen langsameren Start, aber eine schnellere Buildzeit. Dies ist der Standardwert für den IOS-Simulator. Native Funktionen (normalerweise in C), die als "Trampolines" bezeichnet werden, werden als Methoden Implementierungen verwendet, wenn die dynamischen Registrare verwendet werden. Sie variieren zwischen verschiedenen Architekturen.

- **Statische Registrierungs** stellen – die statische Registrierungsstelle generiert den Ziel-C-Code während des Builds, der dann in eine statische Bibliothek kompiliert und mit der ausführbaren Datei verknüpft wird. Dies ermöglicht einen schnelleren Start, dauert aber während der Buildzeit länger. Diese wird standardmäßig für gerätebuilds verwendet. Die statische Registrierungsstelle kann auch mit dem IOS-Simulator verwendet werden `--registrar:static` , indem `mtouch` Sie als Attribut in den Buildoptionen des Projekts übergeben, wie unten dargestellt:

    [![Festlegen zusätzlicher mberührungs-Argumente](architecture-images/image1.png)](architecture-images/image1.png#lightbox)

Weitere Informationen zu den Besonderheiten des IOS-typregistrierungs Systems, das von xamarin. IOS verwendet wird, finden Sie im Handbuch zur [typregistrierungs](~/ios/internals/registrar.md) Stelle.

## <a name="application-launch"></a>Anwendungsstart

Der Einstiegspunkt aller ausführbaren xamarin. IOS-Dateien wird von einer Funktion mit dem Namen bereitgestellt `xamarin_main` , die Mono initialisiert.

Abhängig vom Projekttyp erfolgt Folgendes:

- Für reguläre IOS-und tvos-Anwendungen wird die verwaltete Main-Methode, die von der xamarin-App bereitgestellt wird, aufgerufen. Diese verwaltete Main-Methode ruft dann `UIApplication.Main` auf. Dies ist der Einstiegspunkt für "Ziel-C". UIApplication. Main ist die Bindung für die-Methode von Ziel C `UIApplicationMain` .
- Bei Erweiterungen wird die native Funktion –  `NSExtensionMain` oder ( `NSExtensionmain` für watchos Extensions) –, die von Apple-Bibliotheken bereitgestellt wird, aufgerufen. Da es sich bei diesen Projekten um Klassenbibliotheken und nicht um ausführbare Projekte handelt, sind keine verwalteten Hauptmethoden zum Ausführen vorhanden.

Alle diese Startsequenz wird in eine statische Bibliothek kompiliert, die dann mit der endgültigen ausführbaren Datei verknüpft wird, damit Ihre APP weiß, wie Sie von Grund auf loslegen kann.

An dieser Stelle wurde die APP gestartet, Mono wird ausgeführt, wir sind in verwaltetem Code und wissen, wie Sie systemeigenen Code aufrufen und zurückgerufen werden. Als nächstes müssen wir tatsächlich Steuerelemente hinzufügen und die APP interaktiv gestalten.

## <a name="generator"></a>Generator

Xamarin. IOS enthält Definitionen für jede einzelne IOS-API. Sie können diese im [macios-GitHub](https://github.com/xamarin/xamarin-macios/tree/master/src)-Repository durchsuchen. Diese Definitionen enthalten Schnittstellen mit Attributen sowie alle notwendigen Methoden und Eigenschaften. Der folgende Code wird z. b. verwendet, um eine uitoolbar im UIKit- [Namespace](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)zu definieren. Beachten Sie, dass es sich um eine Schnittstelle mit einer Reihe von Methoden und Eigenschaften handelt:

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Der [`btouch`](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) in xamarin. IOS aufgerufene Generator übernimmt diese Definitions Dateien und verwendet .NET-Tools, um [Sie in eine temporäre Assembly zu kompilieren](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Diese temporäre Assembly kann jedoch nicht zum Aufruf von Ziel-C-Code aufgerufen werden. Der Generator liest dann die temporäre Assembly und generiert c#-Code, der zur Laufzeit verwendet werden kann.
Wenn Sie z. b. ein zufälliges Attribut zu ihrer Definition. cs-Datei hinzufügen, wird es im ausgegebenen Code nicht angezeigt. Der Generator weiß es nicht, und er `btouch` weiß daher nicht, dass er in der temporären Assembly suchen muss, um ihn auszugeben.

Nachdem die Xamarin.iOS.dll erstellt wurde, werden alle Komponenten von mtouchscreen zusammen gebündelt.

Auf hoher Ebene wird dies erreicht, indem die folgenden Aufgaben ausgeführt werden:

- Erstellen Sie eine APP Bundle Struktur.
- Kopieren Sie die verwalteten Assemblys.
- Wenn die Verknüpfung aktiviert ist, führen Sie den verwalteten Linker aus, um die Assemblys zu optimieren, indem Sie nicht verwendete Teile auslagern
- AOT-Kompilierung.
- Erstellen Sie eine systemeigene ausführbare Datei, die eine Reihe von statischen Bibliotheken (jeweils eine für jede Assembly) ausgibt, die mit der nativen ausführbaren Datei verknüpft sind, sodass die native ausführbare Datei aus dem Start Programmcode, dem Registrierungscode (wenn statisch) und allen Ausgaben des AOT-Compilers besteht.

Ausführlichere Informationen zum Linker und seiner Verwendung finden Sie im [Linker](~/ios/deploy-test/linker.md) -Handbuch.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch befasst sich mit der AOT-Kompilierung von xamarin. IOS-apps und hat xamarin. IOS und seine Beziehung zu Ziel C ausführlich untersucht.

## <a name="related-links"></a>Ähnliche Themen

- [Einschränkungen](~/ios/internals/limitations.md)
- [Binden von Objective-C](~/cross-platform/macios/binding/overview.md)
- [Ziel-C-Selektoren](~/ios/internals/objective-c-selectors.md)
- [Typregistrierungs Stelle](~/ios/internals/registrar.md)
- [Linker](~/ios/deploy-test/linker.md)
