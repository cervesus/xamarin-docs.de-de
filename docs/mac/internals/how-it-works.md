---
title: Funktionsweise von Xamarin.Mac
description: In diesem Dokument wird die interne Funktionsweise von xamarin. Mac beschrieben. Insbesondere werden die Konstruktoren, die Speicherverwaltung, die vorab Kompilierung und die Registrierungsstelle untersucht.
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 05/25/2017
ms.openlocfilehash: 05bb9a5552022ea1eb5cd92df90659f7ebacb7cc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571648"
---
# <a name="how-xamarinmac-works"></a>Funktionsweise von Xamarin.Mac

In den meisten Fällen muss sich der Entwickler nicht um die interne "Magic" von xamarin. Mac kümmern, aber ein grundlegendes Verständnis der Funktionsweise im Hintergrund ist die Interpretation der vorhandenen Dokumentation mit einem c#-Foto und das Debuggen von Problemen, wenn diese auftreten.

In xamarin. Mac Bridges eine Anwendung zwei Welten: die Ziel-C-basierte Laufzeit enthält Instanzen von systemeigenen Klassen ( `NSString` , `NSApplication` usw.), und die c#-Laufzeit enthält Instanzen von verwalteten Klassen ( `System.String` , `HttpClient` usw.). Zwischen diesen beiden Welten erstellt xamarin. Mac eine bidirektionale Bridge, sodass eine APP Methoden (Selectors) in Ziel-c (z. b. `NSApplication.Init` ) aufruft und Ziel-c die c#-Methoden der APP zurück abrufen kann (wie Methoden für einen app-Delegaten). Im Allgemeinen werden Aufrufe von Ziel-C transparent über **P/** Calls und einigen Lauf Zeit Code, den xamarin bietet, verarbeitet.

<a name="exposing-classes"></a>

## <a name="exposing-c-classes--methods-to-objective-c"></a>Verfügbar machen von c#-Klassen/-Methoden für Ziel-C

Damit Ziel-c die c#-Objekte einer APP wieder aufzurufen, müssen Sie jedoch auf eine Weise verfügbar gemacht werden, die von Ziel-c verstanden werden kann. Dies erfolgt über das `Register` -Attribut und das- `Export` Attribut. Betrachten Sie das folgende Beispiel:

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

In diesem Beispiel kennt die Ziel-C-Laufzeit nun eine Klasse mit dem Namen `MyClass` und mit Selektoren `init` `run` .

In den meisten Fällen handelt es sich hierbei um ein Implementierungsdetail, das vom Entwickler ignoriert werden kann, da die meisten Rückrufe, die eine App empfängt, entweder über überschriebene Methoden für `base` Klassen (z. b. `AppDelegate` , `Delegates` , `DataSources` ) oder über an APIs übergebenen **Aktionen** sein werden. In allen diesen Fällen `Export` sind Attribute im c#-Code nicht erforderlich.

## <a name="constructor-runthrough"></a>Konstruktorrunthrough

In vielen Fällen muss der Entwickler die Erstellungs-API der c#-Klassen der APP für die Ziel-C-Laufzeit verfügbar machen, damit er von Stellen wie z. b. aufgerufen werden kann, wenn er in Storyboard-oder XIb-Dateien aufgerufen wird. Hier sind die fünf häufigsten Konstruktoren, die in xamarin. Mac-Apps verwendet werden:

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

Im Allgemeinen sollte der Entwickler die `IntPtr` -und- `NSCoder` Konstruktoren belassen, die generiert werden, wenn einige Typen, z. b. Benutzer definiert, allein erstellt werden `NSViews` . Wenn xamarin. Mac einen dieser Konstruktoren als Reaktion auf eine Ziel-C-Lauf Zeit Anforderung aufruft und Sie ihn entfernt hat, stürzt die APP innerhalb von System eigenem Code ab, und es kann schwierig sein, genau das Problem zu ermitteln.

## <a name="memory-management-and-cycles"></a>Speicherverwaltung und Zyklen

Die Speicherverwaltung in xamarin. Mac ist in vielerlei Hinsicht vergleichbar mit xamarin. IOS. Außerdem handelt es sich um ein komplexes Thema, das über den Rahmen dieses Dokuments hinausgeht. Lesen Sie die [bewährten Methoden für Arbeitsspeicher und Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Vor der Kompilierung

.NET-Anwendungen werden in der Regel nicht in den Computercode kompiliert, wenn Sie erstellt werden. Sie werden stattdessen in eine Zwischenebene namens IL-Code kompiliert, die _Just-in-Time_ (JIT)-Kompilierung in Computercode beim Starten der APP abruft.

Die Zeit, die die Mono-Laufzeit zur JIT-Kompilierung benötigt, kann diesen Computercode das Starten einer xamarin. Mac-app um bis zu 20% verlangsamen, da es Zeit dauert, bis der erforderliche Computercode generiert wird.

Aufgrund von Einschränkungen, die von Apple unter IOS auferlegt werden, ist die JIT-Kompilierung des Il-Codes für xamarin. IOS nicht verfügbar. Folglich werden alle xamarin. IOS _-_ apps während des Buildvorgangs vollständig in den Computercode kompiliert.

Neu bei xamarin. Mac ist die Möglichkeit, den IL-Code während des App-Buildvorgangs zu erstellen, ebenso wie xamarin. IOS. Xamarin. Mac verwendet einen _Hybriden_ AOT-Ansatz, der den größten Teil des benötigten Computercodes kompiliert, aber der Laufzeit die Kompilierung erforderlicher Trampoline und die Flexibilität zur Unterstützung von Reflektion. ausgeben (und anderen Anwendungsfällen, die derzeit auf xamarin. Mac funktionieren).

Es gibt zwei Hauptbereiche, in denen AOT eine xamarin. Mac-app unterstützen kann:

- **Bessere "systemeigene" Absturz Protokolle** : Wenn eine xamarin. Mac-Anwendung in System eigenem Code abstürzt, was bei ungültigen Aufrufen von Cocoa-APIs üblich ist (z. b. das Senden eines `null` in eine Methode, die es nicht akzeptiert), sind native Absturz Protokolle mit JIT-Frames schwer zu analysieren. Da die JIT-Frames keine Debuginformationen aufweisen, gibt es mehrere Zeilen mit hexadezimalen Offsets und keinen Hinweis darauf, was passiert ist. AOT generiert "echte" benannte Frames, und die Ablauf Verfolgungen sind viel leichter zu lesen. Dies bedeutet auch, dass die xamarin. Mac-app mit nativen Tools wie **lldb** und **Instruments**besser interagieren kann.
- **Bessere Leistung bei der Startzeit** : bei großen xamarin. Mac-Anwendungen mit einer Zeitspanne von mehreren Sekunden kann die JIT-Kompilierung des gesamten Codes eine beträchtliche Zeit in Anspruch nehmen. Dies erfolgt im voraus.

### <a name="enabling-aot-compilation"></a>Aktivieren der AOT-Kompilierung

AOT ist in xamarin. Mac aktiviert, indem Sie im **Projektmappen-Explorer**auf den **Projektnamen** doppelklicken, zum **Mac-Build** navigieren und `--aot:[options]` zum **zusätzlichen MMP-Argument** hinzufügen (wobei mindestens `[options]` eine Option zum Steuern des AOT-Typs ist, siehe unten). Beispiel:

![Hinzufügen von AOT zu zusätzlichen MMP-Argumenten](how-it-works-images/aot01.png "Hinzufügen von AOT zu zusätzlichen MMP-Argumenten")

> [!IMPORTANT]
> Das Aktivieren der AOT-Kompilierung erhöht die Buildzeit erheblich, manchmal bis zu mehrere Minuten, kann jedoch die APP-Startzeiten um durchschnittlich 20% verbessern. Folglich sollte die AOT-Kompilierung nur für **Releasebuilds** einer xamarin. Mac-App aktiviert werden.

### <a name="aot-compilation-options"></a>AOT-Kompilierungsoptionen

Es gibt verschiedene Optionen, die beim Aktivieren der AOT-Kompilierung für eine xamarin. Mac-app angepasst werden können:

- `none`-Keine AOT-Kompilierung. Dies ist die Standardeinstellung.
- `all`-AOT kompiliert jede Assembly im monobundle.
- `core`-AOT kompiliert die Assemblys `Xamarin.Mac` , `System` und `mscorlib` .
- `sdk`-AOT kompiliert die `Xamarin.Mac` -und-Basisklassen Bibliotheken-Assemblys (BCL).
- `|hybrid`-Das Hinzufügen zu einer der oben genannten Optionen ermöglicht Hybrid-AOT, das Il-Speicher Abstriche ermöglicht, aber längere Kompilierungszeiten zur Folge hat.
- `+`: Enthält eine einzelne Datei für die AOT-Kompilierung.
- `-`: Entfernt eine einzelne Datei aus der AOT-Kompilierung.

Beispielsweise `--aot:all,-MyAssembly.dll` würde die AOT-Kompilierung für alle Assemblys im monobundle _mit Ausnahme_ von aktivieren `MyAssembly.dll` und `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` Hybrid aktivieren `MyOtherAssembly.dll` `mscorlib.dll`

## <a name="partial-static-registrar"></a>Partielle statische Registrierungsstelle

Beim Entwickeln einer xamarin. Mac-app kann es wichtig sein, den Zeitraum zwischen dem Abschließen einer Änderung und dem Testen zu verkürzen. Strategien wie die Modularisierung von Codebasen und Komponententests können dabei helfen, die Kompilier Zeiten zu verkürzen, da Sie die Anzahl der Zeiten verringern, in denen eine APP eine aufwändige vollständige Neuerstellung erfordert.

Zusätzlich zu xamarin. Mac können von der _partiellen statischen Registrierungs_ Stelle (als Pionier von xamarin. IOS) die Startzeiten einer xamarin. Mac-app in der **Debugkonfiguration** drastisch reduziert werden. Wenn Sie wissen, wie die partielle statische Registrierungsstelle eine fast 5-fache Verbesserung beim Debugstart beheben kann, wird ein wenig Hintergrund der Registrierungsstelle, der Unterschied zwischen static und Dynamic und der "partiell static"-Version angezeigt.

### <a name="about-the-registrar"></a>Informationen zur Registrierungsstelle

In einer beliebigen xamarin. Mac-Anwendung befindet sich das Cocoa-Framework von Apple und die Ziel-C-Laufzeit. Die Hauptaufgabe von xamarin. Mac besteht darin, eine Brücke zwischen dieser "nativen Welt" und der "verwalteten Welt" von c# zu entwickeln. Ein Teil dieser Aufgabe wird von der Registrierungsstelle behandelt, die innerhalb der-Methode ausgeführt wird `NSApplication.Init ()` . Dies ist ein Grund, warum die Verwendung von Cocoa-APIs in xamarin. Mac `NSApplication.Init` zuerst aufgerufen werden muss.

Der Auftrag der Registrierungsstelle besteht darin, die Ziel-C-Laufzeit über das vorhanden sein der c#-Klassen der APP zu informieren, die von Klassen wie `NSApplicationDelegate` , `NSView` , und abgeleitet werden `NSWindow` `NSObject` . Dies erfordert eine Überprüfung aller Typen in der APP, um zu bestimmen, was registriert werden muss und welche Elemente für die einzelnen Typen gemeldet werden sollen.

Diese Überprüfung kann entweder **dynamisch**, beim Starten der Anwendung mit Reflektion oder **statisch**, als buildzeitschritt ausgeführt werden. Wenn Sie einen Registrierungstyp auswählen, sollte der Entwickler Folgendes beachten:

- Die statische Registrierung kann die Startzeiten drastisch verkürzen, kann jedoch die Buildzeiten erheblich verlangsamen (in der Regel mehr als doppelte Debug-Buildzeit). Dies ist die Standardeinstellung für Builds der **Releasekonfiguration** .
- Die dynamische Registrierung verzögert diese Arbeit, bis die Anwendung gestartet wird und die Codegenerierung überspringt. durch diese zusätzliche Arbeit kann jedoch eine merkliche Pause (mindestens zwei Sekunden) beim Starten der Anwendung erstellt werden. Dies ist besonders bei Debug-Konfigurations Builds erkennbar, bei denen standardmäßig die dynamische Registrierung verwendet wird und deren Reflektion langsamer ist.

Die teilweise statische Registrierung, die erstmals in xamarin. IOS 8,13 eingeführt wurde, bietet dem Entwickler das Beste aus beiden Optionen. Durch das vorab Berechnen der Registrierungsinformationen für jedes Element in `Xamarin.Mac.dll` und das Versenden dieser Informationen mit xamarin. Mac in einer statischen Bibliothek (die nur zur Buildzeit verknüpft werden muss) hat Microsoft den größten Teil der Reflexions Zeit der dynamischen Registrierungsstelle entfernt, ohne dass sich dies auf die Buildzeit auswirkt.

### <a name="enabling-the-partial-static-registrar"></a>Aktivieren der partiellen statischen Registrierungsstelle

Die partielle statische Registrierungsstelle ist in xamarin. Mac aktiviert, indem Sie im **Projektmappen-Explorer**auf den **Projektnamen** doppelklicken, zu **Mac Build** navigieren und `--registrar:static` dem Feld **zusätzliche MMP-Argumente** hinzufügen. Beispiel:

![Hinzufügen der partiellen statischen Registrierungsstelle zu zusätzlichen MMP-Argumenten](how-it-works-images/psr01.png "Hinzufügen der partiellen statischen Registrierungsstelle zu zusätzlichen MMP-Argumenten")

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Im folgenden finden Sie einige Ausführlichere Erläuterungen zur internen Funktionsweise:

- [Ziel-C-Selektoren](~/ios/internals/objective-c-selectors.md)
- [Registrierung](~/ios/internals/registrar.md)
- [Xamarin-Unified API für IOS und OS X](~/cross-platform/macios/unified/index.md)
- [Grundlagen der Grundlagen](~/ios/app-fundamentals/threading.md)
- [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Zu`newrefcount`](~/ios/internals/newrefcount.md)
