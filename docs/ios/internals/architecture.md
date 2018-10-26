---
title: iOS-App-Architektur
description: Dieses Dokument beschreibt Xamarin.iOS auf ein low-Level, besprechen wie nativem und verwalteter Code interagieren, AOT-Kompilierung, Selektoren, Registrierungsstellen, Starten von Anwendungen und den Generator an.
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 106357e9442d51fdd31bb30b4f0342e2b59f67fd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118486"
---
# <a name="ios-app-architecture"></a>iOS-App-Architektur

Xamarin.iOS-Anwendungen in die Mono-ausführungsumgebung ausgeführt, und verwenden Sie vollständige nun der Zeitpunkt (AOT)-Kompilierung kompiliert C# Code zum ARM-Assemblysprache. Dadurch wird Seite-an-Seite ausgeführt, mit der [Objective-C-Laufzeit](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Beide Laufzeitumgebungen ausführen auf einem UNIX-ähnlichen-Kernel, insbesondere [XNU](https://en.wikipedia.org/wiki/XNU), und machen Sie verschiedene APIs für den Benutzercode und ermöglichen es Entwicklern den Zugriff auf die zugrunde liegenden systemeigenen oder verwalteten System verfügbar.

Das folgende Diagramm zeigt eine grundlegende Übersicht über diese Architektur:

[ ![](architecture-images/ios-arch-small.png "Dieses Diagramm zeigt eine grundlegende Übersicht über die Architektur der nun der Zeitpunkt (AOT)-Kompilierung")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>Systemeigenen und verwalteten Code: eine Erläuterung

Bei der Entwicklung für Xamarin die Begriffe *systemeigenen und verwalteten* Code werden häufig verwendet. [Verwalteter Code](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) ist Code, der die Ausführung von verwalteten hat die [.NET Framework Common Language Runtime](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx), oder in die Xamarin Fall: der Mono-Laufzeit. Dies ist eine Zwischensprache nennen wir.

Systemeigene Code ist Code, der auf die jeweilige Plattform (z. B. Objective-C oder sogar per AOT kompiliert Code auf einem ARM-Chip) nativ ausgeführt wird. Dieses Handbuch untersucht, wie AOT auf Ihren verwalteten Code in systemeigenen Code kompiliert, und es wird erläutert, wie eine Xamarin.iOS-Anwendung funktioniert, voll ausgelastet Apple iOS-APIs durch die Verwendung von Bindungen, und gleichzeitig den Zugriff auf. NET BCL und eine ausgereifte Sprache wie z. B. C#.

## <a name="aot"></a>AOT

Beim Kompilieren von alle Xamarin-Plattform-Anwendung, die Mono C# (oder F#) Compiler laufen und kompiliert Ihre C# und F# Code in Microsoft Intermediate Language (MSIL). Wenn Sie eine xamarin.Android-Anwendung, einer Xamarin.Mac-Anwendung oder sogar einer Xamarin.iOS-Anwendung im Simulator, Ausführen der [.NET Common Language Runtime (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) die MSIL über einen Just-in-Time (JIT)-Compiler kompiliert. Zur Laufzeit, die dies in einer nativen Code kompiliert wird, können die auf die richtige Architektur für Ihre Anwendung ausführen.

Es ist jedoch eine sicherheitseinschränkung, die unter iOS, Festlegen von Apple, die Ausführung von dynamisch generierten Code auf einem Gerät nicht möglich.
Um sicherzustellen, dass wir auf diese Protokolle für die Sicherheit halten, verwendet Xamarin.iOS stattdessen einen nun der Zeitpunkt (AOT)-Compiler zum Kompilieren des verwalteten Codes. Dies führt zu einer systemeigenen iOS-Binärdatei, optional mit LLVM optimiert, für Geräte, die auf der Apple ARM-basierten Prozessor bereitgestellt werden können. Eine grobe Diagramm wie diese zusammenpasst ist nachstehend dargestellt:

[![](architecture-images/aot.png "Eine grobe Diagramm wie diese zusammenpasst")](architecture-images/aot-large.png#lightbox)

Mithilfe von AOT bietet eine Reihe von Einschränkungen, die in beschrieben werden die [Einschränkungen](~/ios/internals/limitations.md) Guide. Darüber hinaus werden eine Reihe von Verbesserungen für die JIT-Kompilierung durch eine Reduzierung der Startdauer und verschiedene leistungsoptimierungen

Nun, da wir untersucht haben, wie der Code aus der Quelle in systemeigenen Code kompiliert wird, werfen wir einen Blick unter die Haube sehen, wie Xamarin.iOS zum Schreiben von vollständig native iOS-Anwendungen ermöglicht

## <a name="selectors"></a>Selektoren

Mit Xamarin, haben wir zwei separate Ökosysteme, .NET und Apple, die wir benötigen Sie zusammen, um wie möglich sein, um sicherzustellen, dass das Endziel ein nahtloses Benutzererlebnis ist als optimierte erscheinen. Wir haben gesehen, im Abschnitt über die zwei Laufzeiten wie kommunizieren, und Sie sehr gut haben gehört des Begriffs "Bindungen", wodurch die native iOS-APIs, die in Xamarin verwendet werden. Bindungen werden ausführlich im erläutert unsere [Objective-C-Bindung](~/cross-platform/macios/binding/overview.md) Dokumentation, also jetzt sehen wir uns wie iOS hinter den Kulissen funktioniert.

Zuerst, es muss eine Möglichkeit zum Bereitstellen von Objective-C nach C#, das erfolgt über Selektoren. Auswahl wird eine Meldung, die an ein Objekt oder eine Klasse gesendet wird. Mit Objective-C erfolgt dies über die [Objc_msgSend](~/cross-platform/macios/binding/overview.md) Funktionen.
Weitere Informationen zur Verwendung von Selektoren finden Sie in der [Objective-C-Selektoren](~/ios/internals/objective-c-selectors.md) Guide. Es verfügt außerdem über eine Möglichkeit, verfügbar zu machen verwalteten Code mit Objective-C, die ist komplizierter, aufgrund der Tatsache, dass Objective-C nichts über den verwalteten Code nicht bekannt ist. Um dies zu umgehen, verwenden wir *Registrierungsstellen*. Diese werden im nächsten Abschnitt ausführlicher erläutert.

## <a name="registrars"></a>Registrierungsstellen

Wie bereits erwähnt, ist die Registrierungsstelle, verwalteter Code macht Objective-c-code Hierzu erstellen eine Liste von jeder verwaltete Klasse, die von NSObject abgeleitet wird:

- Für alle Klassen, die keine vorhandene Objective-C-Klasse umschlossen werden, erstellt er eine neue Objective-C-Klasse mit Objective-C-Elemente, spiegeln alle verwalteten Elemente, die eine [`Export`]-Attribut.

- In den Implementierungen für jedes Objective-C-Element wird automatisch Code hinzugefügt, das gespiegelte verwaltete Element aufrufen.

Der folgende Pseudocode zeigt ein Beispiel ist dies:

**C#(Verwalteter Code)**

```csharp
 class MyViewController : UIViewController{
     [Export ("myFunc")]
     public void MyFunc ()
     {
     }
 }
```

**Objective-c:**

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

Der verwaltete Code kann die Attribute enthalten `[Register]` und `[Export]`, die die Registrierungsstelle verwendet wird, zu wissen, dass das Objekt für Objective-c verfügbar gemacht werden muss
Die `[Register]` Attribut wird verwendet, um den Namen der generierten Objective-C-Klasse angeben, für den Fall, dass Sie der generierten Standardnamen nicht geeignet ist. Alle von NSObject abgeleitete Klassen werden automatisch mit Objective-c registriert.
Die erforderlichen `[Export]` Attribut enthält eine Zeichenfolge, die die Auswahl in der generierten Objective-C-Klasse verwendet wird.

Es gibt zwei Arten von Registrierungsstellen in Xamarin.iOS – dynamische und statische verwendet:


- **Dynamische Registrierungsstellen** – die dynamische Registrierungsstelle ist die Registrierung aller Typen in der Assembly zur Laufzeit. Hierzu werden mithilfe der bereitgestellten Funktionen [Objective-C-Laufzeit-API-](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Die dynamische Registrierung enthält daher einen langsameren Start, aber eine schnellere Erstellung. Dies ist die Standardeinstellung für die iOS-Simulator. Aufrufen systemeigene Funktionen (normalerweise in C), Trampolines, werden als Methoden verwendet, bei Verwendung der dynamischen Registrierungsstellen. Sie haben verschiedene Architekturen variieren.

- **Statische Registrierungsstellen** – die statische Registrierungsstelle generiert Objective-C-Code während des Builds, die dann in einer statischen Bibliothek kompiliert und verknüpft, die in die ausführbare Datei. Dies ermöglicht einen schnelleren Start, aber es dauert länger, bei der Erstellung. Dies wird in der Standardeinstellung verwendet, für Geräte-builds. Die statische Registrierungsstelle kann auch mit dem iOS-Simulator verwendet werden, durch Übergabe `--registrar:static` als ein `mtouch` Attribut in den Buildoptionen des Projekts, wie unten dargestellt:

    [![](architecture-images/image1.png "Weitere Mtouch-Argumente festlegen")](architecture-images/image1.png#lightbox)

Weitere Informationen zu den Besonderheiten des iOS Typregistrierung-System, die von Xamarin.iOS verwendet, finden Sie in der [Typ Registrierungsstelle](~/ios/internals/registrar.md) Guide.

## <a name="application-launch"></a>Starten von Anwendungen

Der Einstiegspunkt, der alle ausführbaren Dateien für Xamarin.iOS erfolgt durch eine Funktion namens `xamarin_main`, die Mono initialisiert.

Je nach Projekttyp geschieht Folgendes:

- Bei regulären iOS und TvOS-Anwendungen heißt die verwaltete Main-Methode, die von der Xamarin-app bereitgestellt. Dies verwaltet "Main"-Methode ruft dann `UIApplication.Main`, dies ist der Einstiegspunkt für Objective-c UIApplication.Main ist die Bindung für Objective-C `UIApplicationMain` Methode.
- Für Erweiterungen, die systemeigene Funktion – `NSExtensionMain` oder (`NSExtensionmain` für WatchOS-Erweiterungen) – bereitgestellt von Apple Bibliotheken aufgerufen wird. Da diese Projekte Klassenbibliotheken und keine ausführbaren Projekten sind, werden keine verwalteten Main-Methoden zum Ausführen.

Alle diese Start-Sequenz wird kompiliert, in einer statischen Bibliothek, die dann in der ausführbaren Datei verknüpft wird, damit Ihre app weiß, wie an Fahrt gewinnt.

An diesem Punkt die app gestartet wurde, Mono ausgeführt wird, werden in verwaltetem Code, und wir wissen, wie Sie systemeigenen Code aufrufen und wieder aufgerufen werden. Als Nächstes wir müssen ist tatsächlich starten, Hinzufügen von Steuerelementen, und stellen die app interaktiver.


## <a name="generator"></a>Generator

Xamarin.iOS enthält Definitionen für jede einzelne iOS-API. Durchsuchen Sie diese auf die [MaciOS Github-Repository](https://github.com/xamarin/xamarin-macios/tree/master/src). Diese Definitionen enthalten Schnittstellen mit Attributen, als auch alle erforderlichen Methoden und Eigenschaften. Der folgende Code ist z. B. dient zum Definieren einer UIToolbar in die UIKit [Namespace](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327). Beachten Sie, dass es sich um eine Schnittstelle mit einer Reihe von Methoden und Eigenschaften ist:

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

Der Generator, dem Namen [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) in Xamarin.iOS, verwendet diese Definitionsdateien und Tools für .NET zu [in eine temporäre Assembly Kompilieren](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Diese temporäre Assembly ist jedoch nicht zum Aufrufen von Objective-C-Code verwendet. Der Generator klicken Sie dann die temporäre Assembly liest und generiert C# Code, der zur Laufzeit verwendet werden kann.
Dies ist die Gründe, z. B. Wenn Sie ein random-Attribut der Definition cs-Datei hinzufügen, sie in der ausgegebene Code nicht angezeigt. Der Generator nicht erkennbar und somit auch `btouch` weiß nicht, in der temporären Assembly, die sie ausgeben sucht.

Nachdem die Xamarin.iOS.dll erstellt wurde, wird von Mtouch alle Komponenten bündeln.

Im Allgemeinen erreicht sie dies durch die folgenden Aufgaben ausführen:

-   Erstellen einer app-Bundle-Struktur.
-   Kopieren Sie in Ihrer verwalteten Assemblys.
-   Wenn linking aktiviert ist, führen Sie die verwaltete Linker aus, um Ihre Assemblys zu optimieren, indem Sie nicht verwendete Teile, die Sie entfernen.
-   AOT-Kompilierung.
-   Erstellen Sie eine native ausführbare Datei, die Ausgaben von einer Reihe von statischen Bibliotheken (eine für jede Assembly), die in die native ausführbare Datei verknüpft sind, damit die native ausführbare Datei besteht aus den Code für Startfeld, Registrierungscode (wenn statisch) und alle Ausgaben aus der AOT Compilerfehler


Ausführlichere Informationen zu den Linker und wie diese verwendet werden, finden Sie in der [Linker](~/ios/deploy-test/linker.md) Guide.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert AOT-Kompilierung von Xamarin.iOS-apps, und untersucht Xamarin.iOS und seine Beziehung mit Objective-C im Detail.

## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/ios/internals/limitations.md)
- [Binden von Objective-C](~/cross-platform/macios/binding/overview.md)
- [Objective-C-Selektoren](~/ios/internals/objective-c-selectors.md)
- [Typ-Registrierungsstelle](~/ios/internals/registrar.md)
- [Linker](~/ios/deploy-test/linker.md)
