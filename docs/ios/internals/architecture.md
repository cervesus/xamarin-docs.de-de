---
title: iOS-Architektur
description: Untersuchen von Xamarin.iOS auf niedriger Ebene
ms.topic: article
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 732f60a413077bc15018679fe8f8bc0a18227246
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="ios-architecture"></a>iOS-Architektur

Xamarin.iOS Anwendungen innerhalb der Mono-ausführungsumgebung ausgeführt, und verwenden Sie die vollständige voraus der Uhrzeit (AOT)-Kompilierung zum Kompilieren der C#-Code zum ARM-Assemblysprache. Dadurch wird Seite-an-Seite ausgeführt, mit der [Objective-C-Laufzeit](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Beide Laufzeitumgebungen ausführen auf einem UNIX-ähnlichen Kernel speziell [XNU](https://en.wikipedia.org/wiki/XNU), und der verschiedenen APIs für die es Entwicklern ermöglicht, den Zugriff auf die zugrunde liegenden systemeigenen oder verwalteten System Benutzercode verfügbar machen.

Das folgende Diagramm zeigt eine grundlegende Übersicht über diese Architektur:

[ ![](architecture-images/ios-arch-small.png "Dieses Diagramm zeigt eine grundlegende Übersicht über die Zukunft der Uhrzeit (AOT) Kompilierung-Architektur")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>Systemeigenen und verwalteten Code: eine Erklärung

Bei der Entwicklung für Xamarin die Begriffe *systemeigenen und verwalteten* Code häufig verwendet werden. [Verwalteter Code](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) ist Code, der die Ausführung von verwalteten der [.NET Framework Common Language Runtime](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx), oder in die Xamarin Fall: die Mono-Laufzeit. Dies ist die intermediate Language sogenannten.

Systemeigener Code ist Code, der direkt auf die jeweilige Plattform (z. B. Objective-C oder sogar AOT kompilierten Code auf einem ARM-Chip) ausgeführt wird. Dieses Handbuch wird erklärt, wie AOT verwalteten Code in systemeigenen Code kompiliert, und es wird erläutert, wie eine Xamarin.iOS Anwendung funktioniert, vollständige nutzen und Apple iOS-APIs durch die Verwendung von Bindungen, während Sie auch eine Zugriff auf. NET BCL und eine anspruchsvolle Sprache wie z. B. c#.


## <a name="aot"></a>AOT

Wenn Sie eine Xamarin-Plattform-Anwendung kompilieren, der Compiler Mono c# (oder f#) wird ausgeführt, und c# und F#-Code in Microsoft Intermediate Language (MSIL) kompiliert wird. Wenn Sie eine Xamarin.Android, eine Xamarin.Mac oder sogar einem Xamarin.iOS-Anwendung im Simulator Ausführen der [.NET Common Language Runtime (CLR)](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx) mithilfe einer Just-in-Time (JIT)-Compiler MSIL kompiliert. Zur Laufzeit, die diese in systemeigenen Code kompiliert wird, können die auf die richtige Architektur für Ihre Anwendung ausführen.

Es ist jedoch eine sicherheitseinschränkung iOS, Festlegen von Apple, die die Ausführung von dynamisch generierten Code auf einem Gerät lässt nicht zu.
Um sicherzustellen, dass wir diese Protokolle für Sicherheit folgen, verwendet Xamarin.iOS stattdessen einen im Voraus der Uhrzeit (AOT)-Compiler zum Kompilieren des verwalteten Codes. Dies führt zu einer systemeigenen iOS binäre, optional mit LLVM optimiert, bei Geräten, die auf der Apple-ARM-basierten Prozessor bereitgestellt werden können. Nachstehend ist ein grober Diagramm wie dies zusammenpasst dargestellt:

[![](architecture-images/aot.png "Eine grobe Diagramm wie dies zusammenpasst")](architecture-images/aot-large.png#lightbox)

Mithilfe von AOT bietet eine Reihe von Einschränkungen in erläutert werden die [Einschränkungen](~/ios/internals/limitations.md) Handbuch. Es bietet auch eine Reihe von Verbesserungen gegenüber JIT durch eine Verringerung der Startzeit und verschiedene leistungsoptimierungen

Nun, da wir untersucht haben, wie der Code aus der Quelle in systemeigenen Code kompiliert wurde, werfen Sie einen Blick hinter den Kulissen um anzuzeigen, wie Xamarin.iOS uns zum Schreiben von vollständig systemeigenen iOS-Anwendungen ermöglicht

## <a name="selectors"></a>Selektoren

Xamarin, haben wir zwei separate Ökosysteme .NET und Apple, die wir benötigen zusammen, um scheint als optimierte wie möglich sein, um sicherzustellen, dass das Ziel eine reibungslose benutzererfahrung. Wir haben gesehen, im Abschnitt über die zwei Laufzeiten wie kommunizieren, und Sie möglicherweise sehr gut auch schon des Begriffs "Bindungen" dem dem systemeigenen iOS-APIs in Xamarin verwendet werden kann. Bindungen werden ausführlich im erläutert unsere [Objective-C-Bindung](~/cross-platform/macios/binding/overview.md) Dokumentation, daher jetzt betrachten wie iOS hinter den Kulissen funktioniert.

Es muss zuerst eine Herangehensweise an Objective-C zu C#-, verfügbar macht, die über Selektoren vorgenommen wird. Ein Selektor ist eine Nachricht, die auf ein Objekt oder eine Klasse gesendet wird. Mit Objective-C erfolgt dies über die [Objc_msgSend](~/cross-platform/macios/binding/overview.md) Funktionen.
Weitere Informationen zur Verwendung von Selektoren, finden Sie in der [Objective-C-Selektoren](~/ios/internals/objective-c-selectors.md) Handbuch. Es muss außerdem eine Herangehensweise an das Verfügbarmachen von verwalteten Codes für Objective-C, die ist komplizierter, Objective-C nichts über den verwalteten Code nicht bekannt ist. Um dies zu umgehen, verwenden wir *Registrierungsstellen*. Diese werden im nächsten Abschnitt ausführlicher erläutert.

## <a name="registrars"></a>Registrierungsstellen

Wie bereits erwähnt, ist die Registrierungsstelle, verwalteter Code macht Objective-c-code Dies geschieht durch Erstellen einer Liste von jeder verwalteten Klasse, die von NSObject abgeleitet wird:

- Für alle Klassen, die keine vorhandene Objective-C-Klasse umschlossen werden, erstellt er eine neue Objective-C-Klasse mit Objective-C-Elemente, die alle verwalteten Member mit Spiegelung ein [`Export`] Attribut.

- In den Implementierungen für jedes Objective-C-Element wird automatisch Code hinzugefügt, den gespiegelten verwalteten Member aufrufen.

Der folgende Pseudocode zeigt ein Beispiel, wie dies funktioniert:

**C#-(verwalteter Code)**

```

 class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }


```

**Objective-C:**

```csharp
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end @implementation

    MyViewController {}
    -(void) myFunc
    {
    /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

Der verwaltete Code kann die Attribute enthalten `[Register]` und `[Export]`, die die Registrierungsstelle verwendet wird, zu wissen, dass das Objekt für Objective-c verfügbar gemacht werden muss
Die `[Register]` Attribut wird verwendet, um den Namen der generierten Objective-C-Klasse angeben, für den Fall, dass Sie der Standardnamen generiert nicht geeignet ist. Alle von NSObject abgeleitete Klassen werden automatisch mit Objective-c registriert.
Die erforderliche `[Export]` Attribut enthält eine Zeichenfolge, die die Auswahl in der generierten Objective-C-Klasse verwendet wird.

Es gibt zwei Arten von Registrierungsstellen in Xamarin.iOS – dynamische und statische verwendet:


- **Dynamische Registrierungsstellen** – dynamische Registrierungsstelle ist die Registrierung aller Typen in der Assembly zur Laufzeit. Dies geschieht mithilfe des bereitgestellten Funktionen [Objective-C-Laufzeit-API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Die dynamische Registrierungsstelle weist daher einen langsameren Start, aber eine schnellere Time erstellen. Dies ist die Standardeinstellung für den iOS-Simulator. Aufrufen systemeigene Funktionen (normalerweise in C), Trampolines, werden als Implementierungen der Dienstmethode verwendet, bei Verwendung der dynamischen Registrierungsstellen. Sie variieren in verschiedenen Architekturen.

- **Statische Registrierungsstellen** – statische Registrierungsstelle generiert Objective-C-Code während des Builds, die dann in einer statischen Bibliothek kompiliert und in die ausführbare Datei verknüpft. Dies ermöglicht einen schnelleren Start, jedoch während des Buildvorgangs länger dauert. Dies wird standardmäßig verwendet, für das Gerät erstellt. Die statische Registrierungsstelle kann auch mit der iOS-Simulator verwendet werden, durch das übergeben `--registrar:static` als ein `mtouch` Attribut in Ihrem Projekt Buildoptionen, wie unten dargestellt:

    [![](architecture-images/image1.png "Zusätzliche Mtouch Argumente festgelegt")](architecture-images/image1.png#lightbox)

Weitere Informationen zu den Besonderheiten des iOS-Typregistrierung-System von Xamarin.iOS verwendet, finden Sie in der [Typ Registrierungsstelle](~/ios/internals/registrar.md) Handbuch.

## <a name="application-launch"></a>Anwendung starten

Einstiegspunkt aller Xamarin.iOS ausführbaren Dateien ist eine Funktion namens gebotenen `xamarin_main`, die Mono initialisiert.

Je nach Projekttyp geschieht Folgendes:

- Für reguläre IOS- und Anwendungen für tvos. außerdem wurden wird der verwaltete Main-Methode, die von der Xamarin-app bereitgestellt aufgerufen. Dies verwaltet Main-Methode ruft dann `UIApplication.Main`, dies ist der Einstiegspunkt für Objective-C. UIApplication.Main wird die Bindung für Objective-C `UIApplicationMain` Methode.
- Für Erweiterungen, die systemeigene Funktion – `NSExtensionMain` oder (`NSExtensionmain` für WatchOS-Erweiterungen) – bereitgestellt von Apple Bibliotheken aufgerufen wird. Da diese Projekte Klassenbibliotheken und keine ausführbaren Projekten sind, stehen keine verwalteten Main-Methoden auszuführen.

Alle Start-Sequenz wird in eine statische Bibliothek, die dann in die endgültige ausführbare verknüpft wird, damit Ihre app das Deaktivieren von Grund auf neu zu Weiß kompiliert.

An diesem Punkt unserer app gestartet wurde, Mono ausgeführt wird, werden in verwaltetem Code ein, und wir wissen, wie Sie systemeigenen Code aufrufen und wieder aufgerufen werden. Im nächsten Schritt erforderlich ist, werden tatsächlich starten, Hinzufügen von Steuerelementen und Bereitstellen der app interaktive ab.


## <a name="generator"></a>Generator

Xamarin.iOS enthält Definitionen für jedes einzelne iOS-API. Durchsuchen Sie diese auf die [MaciOS Github-Repository](https://github.com/xamarin/xamarin-macios/tree/master/src). Diese Definitionen enthalten, Schnittstellen, Attribute, als auch alle erforderlichen Methoden und Eigenschaften. Der folgende Code ist z. B. zum Definieren einer UIToolbar in der UIKit [Namespace](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327). Beachten Sie, dass es sich um eine Schnittstelle mit einer Reihe von Methoden und Eigenschaften ist:

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

Der Generator aufgerufen [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) in Xamarin.iOS, diesen Definitionsdateien und benötigt .NET Tools zur [in eine temporäre Assembly zu kompilieren](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Dieser temporäre Assembly ist jedoch nicht für nutzbare Objective-C-Code aufrufen. Der Generator klicken Sie dann die temporäre Assembly liest und generiert C#-Code, der zur Laufzeit verwendet werden kann.
Dies ist die Warum, z. B. Wenn Sie Ihre Definition cs-Datei einen zufälligen Attribut hinzugefügt haben, es in der ausgegebene Code angezeigt wird nicht. Der Generator kennen keine und daher `btouch` weiß nicht, dafür gesucht werden soll, in der temporären Assembly, die mit ihm ausgegeben werden.

Sobald die Xamarin.iOS.dll erstellt wurde, wird Mtouch alle Komponenten bündeln.

Auf hoher Ebene erreicht er dies durch die folgenden Aufgaben ausführen:

-   Erstellen einer app-Bundle-Struktur.
-   Kopieren Sie die verwalteten Assemblys.
-   Wenn Verknüpfen aktiviert ist, führen Sie die verwalteten Linker aus, um die Assemblys zu optimieren, indem Sie nicht verwendete Teilen, kopieren.
-   AOT-Kompilierung.
-   Erstellen Sie eine systemeigene ausführbare Datei, die gibt eine Reihe von statischen Bibliotheken (eine für jede Assembly), die in die systemeigene ausführbare verknüpft sind, damit die systemeigene ausführbare Datei der Startprogramm Code, der Registrierungsstelle-Code (sofern statisch) und alle Ausgaben von der AOT besteht aus Compilerfehler


Für ausführlichere Informationen zu den Linker und wie diese verwendet werden, finden Sie in der [Linker](~/ios/deploy-test/linker.md) Handbuch.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, AOT Kompilierung von Xamarin.iOS-apps und untersucht Xamarin.iOS und deren Beziehung zur Objective-C im Detail.

## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/ios/internals/limitations.md)
- [Binden von Objective-C](~/cross-platform/macios/binding/overview.md)
- [Selektoren Objective-C](~/ios/internals/objective-c-selectors.md)
- [Typ-Registrierungsstelle](~/ios/internals/registrar.md)
- [Linker](~/ios/deploy-test/linker.md)
