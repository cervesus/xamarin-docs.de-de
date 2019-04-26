---
title: Funktionsweise von Xamarin.Mac
description: Dieses Dokument beschreibt die interne Funktionsweise von Xamarin.Mac. Insbesondere wird in den Konstruktoren, Speicherverwaltung, vor der Time-Kompilierung, und der Registrierungsstelle gesucht.
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/25/2017
ms.openlocfilehash: 0635e110cb2aa7bc00234d3d06df57e0fd6f966e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033844"
---
# <a name="how-xamarinmac-works"></a>Funktionsweise von Xamarin.Mac

In den meisten Fällen der Entwickler wird nie Gedanken über die interne "magische" von Xamarin.Mac, machen jedoch müssen Sie einen groben Überblick darüber, wie Dinge funktioniert im Hintergrund in beiden testinterpretation vorhandenen Dokumentation hilft eine C# Fokus und Debuggen Probleme bei deren Auftreten informieren.

Xamarin.Mac verbindet eine Anwendung zwei Welten: Es gibt die Objective-C-basierte-Runtime, die Instanzen von systemeigenen Klassen (`NSString`, `NSApplication`usw.) und es gibt die C# Common Language Runtime, die Instanzen von verwalteten Klassen (`System.String`, `HttpClient`usw.). Zwischen diesen beiden Umgebungen, Xamarin.Mac erstellt eine zwei-Wege-Brücke, damit eine app in Objective-C-Methoden (Selektoren) aufgerufen werden kann (z. B. `NSApplication.Init`) und Objective-C kann der app Aufrufen C# Methoden (z. B. die Methoden auf einen app-Delegaten) zurück. Im Allgemeinen Aufrufe in Objective-C erfolgt transparent über **P/Invokes** und einige Laufzeitcode Xamarin bietet.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>Verfügbarmachen von C# Klassen / Methoden mit Objective-C

Allerdings für Objective-C Rückruf in einer app C# Objekte verwenden, müssen sie auf eine Weise verfügbar gemacht werden, die Objective-C-verstehen kann. Dies erfolgt mithilfe der `Register` und `Export` Attribute. Betrachten Sie das folgende Beispiel:

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

In diesem Beispiel wird die Objective-C-Laufzeit jetzt über eine Klasse namens wissen `MyClass` mit Selektoren namens `init` und `run`.

In den meisten Fällen ist dies ein Implementierungsdetail, die der Entwickler ignorieren können, wie die meisten Rückrufe, die eine Anwendung empfängt werden über die überschriebenen Methoden auf `base` Klassen (z. B. `AppDelegate`, `Delegates`, `DataSources`) oder auf  **Aktionen** -APIs übergeben. In allen diesen Fällen `Export` Attribute sind nicht erforderlich, in der C# Code.

## <a name="constructor-runthrough"></a>Konstruktor runthrough

In vielen Fällen muss der Entwickler der app verfügbar machen C# Klassen Konstruktion API für die Objective-C-Laufzeit, damit es von Stellen, z. B. wenn instanziiert werden kann als im Storyboard oder XIB-Dateien bezeichnet. Hier sind die fünf am häufigsten verwendeten Konstruktoren, die in einer Xamarin.Mac-apps verwendet werden:

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

Im Allgemeinen sollten Entwickler lassen die `IntPtr` und `NSCoder` Konstruktoren, die generiert werden, wenn einige Typen wie z. B. benutzerdefinierte erstellen `NSViews` allein. Wenn Xamarin.Mac einer dieser Konstruktoren als Reaktion auf ein Objective-C-Runtime-Anforderung aufgerufen werden muss, und Sie es ausgebaut haben, die app stürzt in systemeigenen Code, und es kann schwierig sein, genau das Problem ermitteln.

## <a name="memory-management-and-cycles"></a>Speicherverwaltung und Zyklen

Verwaltung des Arbeitsspeichers in Xamarin.Mac ist in vielerlei Hinsicht sehr ähneln Xamarin.iOS. Es ist auch ein komplexes Thema, einem Gegenstand dieses Dokuments. Lesen Sie die [Arbeitsspeicher und bewährte Methoden für Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Der Time-Kompilierung fort

In der Regel Anwendungen für .NET können nicht auf Computercode kompiliert, werden bei ihrer Erstellung, stattdessen diese kompilieren, namens IL-Code, der abruft Zwischenschicht _Just-In-Time-_ (JIT) in Computercode kompiliert, wenn die app gestartet wird.

Die Zeit, die die mono-Laufzeit zu JIT Kompilieren kann diese Computercode den Start einer Xamarin.Mac-App um bis zu 20 %, verlangsamen wie die erforderlichen Computercode generiert werden kann.

Aufgrund der Einschränkungen seitens Apple unter iOS ist die JIT-Kompilierung des IL-Codes nicht für Xamarin.iOS verfügbar. Daher sind alle Xamarin.iOS-app voll _Ahead-Of-Time-_ (AOT), die während des Buildzyklus in Computercode kompiliert.

Neue kann zu Xamarin.Mac AOT der IL-Code während des Buildzyklus app, ebenso wie Xamarin.iOS. Xamarin.Mac verwendet eine _Hybrid_ AOT-Ansatz, der einen Großteil der erforderlichen Computercode kompiliert wird, ermöglicht jedoch die Laufzeit zum Kompilieren erforderlichen Trampolines und die Flexibilität, um den Vorgang fortzusetzen, zur Unterstützung von "Reflection.Emit" (und andere Anwendungsfälle derzeit funktioniert für Xamarin.Mac).

Es gibt zwei wichtige Bereichen, die in einer Xamarin.Mac-app der AOT unterstützen kann:

- **"Native" absturzprotokolle besser** : bei eine Xamarin.Mac-Anwendung in nativem Code, stürzt ab, handelt es sich häufig auftreten, wenn ungültige Aufrufe Cocoa-APIs (wie z. B. das Senden einer `null` in eine Methode, die sie akzeptieren keine), systemeigene absturzprotokolle mit JIT-Kompilierung Frames sind schwer zu analysieren. Da die JIT-Frames keine Debuginformationen zu haben, werden mehrere Zeilen mit hexadezimal-Offsets und keine Ahnung, was da vorgeht. AOT generiert "real" benannte Frames aus, und die ablaufverfolgungen sind viel leichter zu lesen. Dies bedeutet auch die Xamarin.Mac-app interagiert besser mit systemeigenen Tools wie z. B. **Lldb** und **Instruments**.
- **Starten Sie eine bessere Leistung** – für große Xamarin.Mac-Anwendungen mit mehreren Start nochmals, JIT-Kompilierung der gesamte Code viel Zeit ausführen können. AOT funktioniert diese voraus.

### <a name="enabling-aot-compilation"></a>Aktivieren der AOT-Kompilierung

AOT in Xamarin.Mac aktiviert ist, durch Doppelklicken auf die **Projektname** in die **Projektmappen-Explorer**, Navigation zu **Mac Build** und Hinzufügen von `--aot:[options]` auf die  **Zusätzlichen Mmp-Argumenten:** Feld (wobei `[options]` ist eine oder mehrere Optionen zum Steuern des AOT-Typs finden weiter unten). Zum Beispiel:

![Hinzufügen von AOT zu zusätzlichen Mmp-Argumenten](how-it-works-images/aot01.png "AOT hinzufügen, um zusätzlichen Mmp-Argumenten")

> [!IMPORTANT]
> Aktivieren der AOT Kompilierung dramatisch zunimmt, Zeitpunkt der Erstellung, manchmal bis zu einigen Minuten, jedoch können sie app-Startzeiten verbessern, um durchschnittlich 20 %. Daher AOT-Kompilierung nur aktiviert werden soll **Version** einer Xamarin.Mac-App erstellt.

### <a name="aot-compilation-options"></a>Optionen der AOT-Kompilierung

Es gibt mehrere Möglichkeiten, die beim Aktivieren der AOT-Kompilierung für eine Xamarin.Mac-app angepasst werden können:

- `none` – Kein AOT-Kompilierung. Dies ist die Standardeinstellung.
- `all` -AOT kompiliert jede Assembly in den MonoBundle.
- `core` -AOT kompiliert die `Xamarin.Mac`, `System` und `mscorlib` Assemblys.
- `sdk` -AOT kompiliert die `Xamarin.Mac` und Base Class Libraries (BCL)-Assemblys.
- `|hybrid` : Diese Option, um eine der oben genannten Optionen aktiviert hybrides AOT, was IL-stripping ermöglicht, jedoch wird zu längeren kompilierzeiten führen Mal hinzufügen.
- `+` : Enthält eine einzelne Datei für die AOT-Kompilierung.
- `-` : Entfernt eine einzelne Datei aus der AOT-Kompilierung.

Z. B. `--aot:all,-MyAssembly.dll` AOT-Kompilierung für alle Assemblys in der MonoBundle ermöglicht _außer_ `MyAssembly.dll` und `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` Hybrid ermöglichen würde, AOT-Code enthalten die `MyOtherAssembly.dll` und die ausschließen`mscorlib.dll`.

## <a name="partial-static-registrar"></a>Partielle statische Registrierungsstelle

Wenn Sie eine Xamarin.Mac-app zu entwickeln, kann minimiert die Zeit zwischen den Abschluss einer Änderung, und Testen für die Einhaltung von entwicklungsfristen wichtiger. Strategien wie z. B. die modularisierung der Codebasis und Komponententests können Sie die um Kompilierdauer zu verringern, wie sie die weniger häufig, dass eine app eine teure vollständige Neuerstellung erforderlich sind.

Darüber hinaus und noch nicht mit Xamarin.Mac, _teilweise statische Registrierungsstelle_ (wie von Xamarin.iOS wurde) können erheblich reduzieren, die Startzeiten einer Xamarin.Mac-App in der **Debuggen** Konfiguration. Gestaucht zu verstehen, wie mithilfe der partiellen statische Registrierungsstelle kann eine fast eine 5 X Verbesserung der Debugstart dauert ein wenig Hintergrundinformationen zu den neuerungen von der Registrierungsstelle, was ist der Unterschied zwischen statischen und dynamischen und was bewirkt, dass diese Version von "partielle Static".

### <a name="about-the-registrar"></a>Informationen zu der Registrierungsstelle

Unter die Haube der alle Xamarin.Mac liegt die Anwendung das Cocoa-Framework von Apple und die Objective-C-Laufzeit. Erstellen eine Brücke zwischen diesem "native World" und der "verwalteten Welt" von C# ist die primäre Verantwortung von Xamarin.Mac. Teil dieser Aufgabe wird von der Registrierungsstelle, die ausgeführt wird, in verarbeitet `NSApplication.Init ()` Methode. Dies ist ein Grund, die Verwendung von Cocoa-APIs in Xamarin.Mac erfordert `NSApplication.Init` zuerst aufgerufen werden.

Die Registrierungsstelle Aufgabe besteht darin, die Objective-C-Runtime von der Existenz von der app zu informieren C# Klassen, die von Klassen, z. B. ableiten `NSApplicationDelegate`, `NSView`, `NSWindow`, und `NSObject`. Dies erfordert eine Überprüfung aller Typen in der app, um zu bestimmen, was registrieren und welche Elemente auf den einzelnen Bericht.

Diese Überprüfung kann erfolgen **dynamisch**, beim Start der Anwendung mit Reflektion oder **statisch**, als Buildschritt Zeit. Wenn einen Registrierungstyp auswählen möchten, muss der Entwickler folgende Punkte zu beachten:

- Statische Registrierung Startzeiten kann erheblich reduziert, aber Sie können Builds Mal deutlich langsamer (in der Regel mehr als das doppelte Debug Buildzeit). Hier werden die Standardeinstellung für **Version** Konfiguration erstellt.
- Dynamische Registrierung wird verzögert, diese Arbeit bis Anwendung starten, und überspringt die Generierung von Code, aber dieser zusätzliche Arbeitsaufwand eine deutliche Pause (mindestens zwei Sekunden) erstellen kann, in der Anwendung starten. Dies ist vor allem dann bemerkbar in Debugbuilds-Konfiguration, die standardmäßig auf die dynamische Registrierung und deren Reflexion langsamer ist.

Partielle statische Registrierung, die erstmals in Xamarin.iOS 8.13, erhält der Entwickler die Vorteile beider Optionen. Mithilfe der Informationen zur produktregistrierung jedes Elements in `Xamarin.Mac.dll` und versenden diese Informationen mit Xamarin.Mac in einer statischen Bibliothek (die nur zum Zeitpunkt der Erstellung mit verknüpft sein muss), hat Microsoft in den meisten Fällen Reflektion des dynamischen entfernt Die Registrierungsstelle und ohne Auswirkungen auf erstellen.

### <a name="enabling-the-partial-static-registrar"></a>Aktivieren die partielle statische Registrierungsstelle

Die partielle statische Registrierungsstelle in Xamarin.Mac aktiviert ist, durch Doppelklicken auf die **Projektname** in die **Projektmappen-Explorer**, Navigation zu **Mac Build** und Hinzufügen`--registrar:static` auf die **zusätzlichen Mmp-Argumenten:** Feld. Zum Beispiel:

![Hinzufügen von der partiellen statische Registrierungsstelle zu zusätzlichen Mmp-Argumenten](how-it-works-images/psr01.png "zusätzlichen Mmp-Argumenten, die teilweise statische Registrierungsstelle hinzugefügt")

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Hier sind einige ausführlichen Erklärungen intern Funktionsweise:

- [Objective-C-Selektoren](~/ios/internals/objective-c-selectors.md)
- [Registrierungsstelle](~/ios/internals/registrar.md)
- [Xamarin-Unified-API für iOS und OS X](~/cross-platform/macios/unified/index.md)
- [Theading-Grundlagen](~/ios/app-fundamentals/threading.md)
- [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Informationen zu `newrefcount`](~/ios/internals/newrefcount.md)

