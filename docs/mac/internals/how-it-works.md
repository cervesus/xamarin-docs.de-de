---
title: Funktionsweise von Xamarin.Mac
description: Dieses Dokument beschreibt die interne Funktionsweise von Xamarin.Mac. Insbesondere wird am Konstruktoren, die Verwaltung des Arbeitsspeichers, vor der Kompilierung, und die Registrierungsstelle gesucht.
ms.topic: article
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: a1dbff32b113bd1c3a6b2058a34c73977c59c9e5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="how-xamarinmac-works"></a>Funktionsweise von Xamarin.Mac

In den meisten Fällen der Entwickler also nie haben über den internen "magische" des Xamarin.Mac, machen Sie jedoch, dass groben Überblick über die wie Dinge Works hinter den Kulissen hilft in vorhandenen Dokumentation mit einem C#-objektiv zu interpretieren und Debuggen von Problemen bei der Sie auftreten.

In Xamarin.Mac, eine Anwendung verbindet zwei Welten: vorhanden ist, die Instanzen von systemeigenen Klassen Objective-C-basierte Runtime (`NSString`, `NSApplication`usw.), und es ist die Runtime in c#, die Instanzen von verwalteten Klassen (`System.String`, `HttpClient`usw.). Zwischen diesen beiden Welten Xamarin.Mac erstellt eine zwei-Wege-Bridge, damit eine app in Objective-C-Methoden (Selektoren) aufgerufen werden kann (z. B. `NSApplication.Init`) Objective-C kann die app C#-Methoden aufrufen (wie Methoden für ein app-Delegaten) zurück. Im Allgemeinen Aufrufe in Objective-C erfolgen transparent über **P/Invokes** und einige Laufzeitcode Xamarin bietet.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>Verfügbarmachen von Klassen in c# / Methoden Objective-C

Für Objective-C wieder in eine app C#-Objekte aufrufen, müssen sie jedoch auf eine Weise verfügbar gemacht werden, die Objective-C zu bringen. Dies erfolgt über die `Register` und `Export` Attribute. Betrachten Sie das folgende Beispiel:

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

In diesem Beispiel wird jetzt Objective-C-Laufzeit über eine Klasse mit dem Namen wissen `MyClass` mit Selektoren aufgerufen `init` und `run`.

In den meisten Fällen ist dies ein Implementierungsdetail, die der Entwickler ignoriert werden kann, wie die meisten Rückrufe eine Anwendung empfängt werden entweder über die überschriebenen Methoden auf `base` Klassen (z. B. `AppDelegate`, `Delegates`, `DataSources`) oder auf  **Aktionen** APIs übergeben. In all diesen Fällen `Export` Attribute sind nicht in der C#-Code erforderlich.

## <a name="constructor-runthrough"></a>Konstruktor runthrough

In vielen Fällen muss der Entwickler der app C#-Klassen-Konstruktion API Objective-C-Laufzeit verfügbar zu machen, damit es z. B. von Orten instanziiert werden kann beim Aufruf in Storyboard oder XIB-Dateien. Hier werden die fünf am häufigsten verwendeten Konstruktoren in Xamarin.Mac apps verwendet:

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

Im Allgemeinen sollten Entwickler lassen die `IntPtr` und `NSCoder` Konstruktoren, die generiert werden, wenn einige Typen, z. B. benutzerdefinierte erstellen `NSViews` allein. Wenn Xamarin.Mac einer dieser Konstruktoren als Antwort auf eine Objective-C-Laufzeit-Anforderung aufgerufen muss, und Sie es ausgebaut haben, stürzt die Anwendung in systemeigenen Code, und es kann schwierig sein, genau das Problem zu ermitteln.

## <a name="memory-management-and-cycles"></a>Die Verwaltung des Arbeitsspeichers und Zyklen

Die Verwaltung des Arbeitsspeichers in Xamarin.Mac ist in vielerlei Hinsicht Xamarin.iOS sehr ähnlich. Es ist auch ein komplexes Thema, eine würde den Rahmen dieses Dokuments sprengen. Lesen Sie die [Arbeitsspeicher und bewährte Methoden für Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Der Time-Kompilierung fort

In der Regel werden bei ihrer Erstellung führen Sie nicht .NET Applications auf Computercode kompiliert, stattdessen ein intermediate Ebene aufgerufen, ruft IL-Code kompiliert _Just-in-Time_ (JIT) in Computercode kompiliert, wenn die app gestartet wird.

Die Zeit, die die mono-Laufzeit auf JIT-Kompilierung kann dieser Computercode Start einer app Xamarin.Mac bis zu 20 % beeinträchtigen, wie es Zeit für den erforderlichen Computercode generiert werden soll.

Aufgrund von Einschränkungen, die von Apple iOS auferlegt werden ist die JIT-Kompilierung, der die IL-Code nicht to Xamarin.iOS verfügbar. Daher sind alle Xamarin.iOS app voll _Ahead angegeben_ (AOT), die während des Buildzyklus in Computercode kompiliert.

Neue werden Xamarin.Mac die Möglichkeit, AOT IL-Code während des app-Build-Zyklus wie alle Xamarin.iOS können. Xamarin.Mac verwendet eine _Hybrid_ AOT Ansatz, die einen Großteil der erforderlichen Computercode kompiliert wird jedoch von der Laufzeit zum Kompilieren erforderlichen Trampolines und die Flexibilität, um den Vorgang fortzusetzen, zur Unterstützung von "Reflection.Emit" ermöglicht (und andere Anwendungsfälle derzeit arbeiten Sie Xamarin.Mac).

Es gibt zwei Hauptbereichen, bei denen AOT eine Xamarin.Mac app beitragen kann:

- **"Systemeigene" Absturz Protokolle zu einer besseren** – Wenn eine Xamarin.Mac-Anwendung in systemeigenem Code, also häufiger eintreten abstürzt, wenn ungültige Aufrufe Kakao-APIs (wie etwa das Versenden einer `null` in eine Methode, die sie akzeptieren keine), systemeigene absturzprotokolle mit JIT Frames sind schwer zu analysieren. Da die JIT-Frames keine Debuginformationen, werden es mehrere Zeilen mit hexadezimalen Offsets und keinen Hinweis darauf, was passiert wurde. AOT generiert "real" benannte Frames und die ablaufverfolgungen sind wesentlich einfacher zu lesen. Dies bedeutet auch Xamarin.Mac app besser mit interagieren systemeigenen Tools wie z. B. **Lldb** und **Instrumente**.
- **Starten Sie eine bessere Leistung** – für große Xamarin.Mac Anwendungen mit einem mehrere zweite Startzeit, JIT-Kompilieren der gesamte Code sehr viel Zeit dauern können. AOT führt diese Arbeit voraus.

### <a name="enabling-aot-compilation"></a>Aktivieren der AOT Kompilierung

AOT in Xamarin.Mac aktiviert ist, durch Doppelklicken auf die **Projektname** in der **Projektmappen-Explorer**, navigieren Sie zu **Mac erstellen** und Hinzufügen von `--aot:[options]` auf die  **Zusätzliche Mmp Argumente:** Feld (wobei `[options]` ist eine oder mehrere Optionen zum Steuern des AOT Typs finden weiter unten). Zum Beispiel:

![Argumente für zusätzliche Mmp AOT hinzugefügt](how-it-works-images/aot01.png "AOT hinzufügen, um zusätzliche Mmp-Argumente")

> [!IMPORTANT]
> Aktivieren von AOT Kompilierung deutlich erhöht die Buildzeit in einigen Fällen bis zu einige Minuten, jedoch können sie app-Startzeiten verbessern, indem Sie Durchschnittswert von 20 %. Folglich AOT Kompilierung sollte nur aktiviert werden, auf **Release** Xamarin.Mac-App erstellt.

### <a name="aot-compilation-options"></a>AOT Kompilierungsoptionen

Es gibt mehrere verschiedene Optionen, die bei der Aktivierung von AOT-Kompilierung für eine app Xamarin.Mac angepasst werden können:

- `none` -Keine AOT-Kompilierung. Dies ist die Standardeinstellung.
- `all` -AOT wird jede Assembly in die MonoBundle kompiliert.
- `core` -AOT kompiliert die `Xamarin.Mac`, `System` und `mscorlib` Assemblys.
- `sdk` -AOT kompiliert die `Xamarin.Mac` und die Basisklassenbibliothek (Base Class Libraries, BCL)-Assemblys.
- `|hybrid` – Diese Option, um eine der oben genannten Optionen kann Hybrid AOT IL Striping ermöglicht, jedoch wird im Ergebnis mehr kompiliert, wie oft hinzufügen.
- `+` -Enthält ein einzelnes für AOT Kompilierung.
- `-` -Entfernt eine einzelne Datei AOT Kompilierung an.

Z. B. `--aot:all,-MyAssembly.dll` würde Standardvorlagenbibliotheken AOT auf alle Assemblys in der MonoBundle _außer_ `MyAssembly.dll` und `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` hybride ermöglichen würden, Code AOT enthalten die `MyOtherAssembly.dll` und die ausschließen`mscorlib.dll`.

## <a name="partial-static-registrar"></a>Partielle statische Registrierungsstelle

Wenn Sie eine Xamarin.Mac-app entwickeln, kann minimiert die Zeit zwischen den Abschluss einer Änderung und zu testen, verpflichtet, Zeitlimits für die Entwicklung wichtiger. Strategien, z. B. modularisierung von veröffentlicht Codebasen und Komponententests dabei helfen können, kompilierzeiten, verringern Reduzierung wie oft der, dass eine app eine teure komplett neu erstellen erfordert.

Darüber hinaus und noch nicht mit Xamarin.Mac, _teilweise statische Registrierungsstelle_ (wie von Xamarin.iOS Vorreiterrolle beim) kann deutlich verringert werden die Startzeiten Xamarin.Mac-App in der **Debuggen** Konfiguration. Verstehen, wie die partielle statische Registrierungsstelle bietet gestaucht ein fast eine 5 X Verbesserung im Debugmodus starten dauert ein Bit des Hintergrunds auf was die Registrierungsstelle ist und was ist der Unterschied zwischen statischen und dynamischen Leistungsumfang von dieser Version von "teilweise statischen".

### <a name="about-the-registrar"></a>Über die Registrierungsstelle

Hinter den Kulissen alle Xamarin.Mac liegt Anwendung des Frameworks Kakao Apple und Objective-C-Laufzeit. Erstellen eine Brücke zwischen diesem "systemeigene World" und "verwaltete Welt" der C#-ist der Xamarin.Mac zuständig. Teil dieser Aufgabe erfolgt durch die Registrierungsstelle, die innerhalb von ausgeführt wird `NSApplication.Init ()` Methode. Dies ist ein Grund, die die Verwendung von Kakao-APIs in Xamarin.Mac erfordert `NSApplication.Init` zuerst aufgerufen werden.

Die Registrierungsstelle Aufgabe besteht darin, die Objective-C-Laufzeit über das Vorhandensein der app C#-Klassen zu informieren, die von Klassen, z. B. ableiten `NSApplicationDelegate`, `NSView`, `NSWindow`, und `NSObject`. Dies erfordert eine Überprüfung aller Typen in der app, um zu bestimmen, was registrieren und welche Elemente auf den einzelnen Bericht.

Diese Überprüfung kann erfolgen **dynamisch**, beim Start der Anwendung mit Reflektion oder **statisch**, als Buildschritt Zeit. Wenn einen Registrierung Typ auswählen zu können, muss der Entwickler sollten Sie Folgendes beachten:

- Statische Registrierung Startzeiten kann drastisch verringern, sondern verlangsamen kann Builds Zeiten deutlich (in der Regel mehr als das doppelte Debug Buildzeit). Dies ist die Standardeinstellung für wird **Version** Konfiguration erstellt.
- Dynamische Registrierung verzögert dieses Werk bis Anwendung starten, und überspringt die Generierung von Code, aber diese zusätzliche Arbeit merkliche Pause (mindestens zwei Sekunden) erstellen kann, in Anwendungsstart. Dies ist besonders in Debugbuilds-Konfiguration, die standardmäßig die dynamische Registrierung und dessen Reflektion langsamer wird.

Partielle statische Registrierung erstmals in Xamarin.iOS 8.13, gewährt dem Entwickler das beste aus beiden Optionen. Indem vorab berechnet wird, die Registrierungsinformationen jedes Elements in `Xamarin.Mac.dll` und diese Versandinformationen mit Xamarin.Mac in einer statischen Bibliothek (die nur während des Buildvorgangs mit verknüpft sein muss), hat Microsoft die meisten der Reflektion bei der dynamischen entfernt Registrierungsstelle beim ohne Auswirkungen auf die Buildzeit.

### <a name="enabling-the-partial-static-registrar"></a>Aktivieren die teilweise statische Registrierungsstelle

Die partielle statische Registrierungsstelle in Xamarin.Mac aktiviert ist, durch Doppelklicken auf die **Projektname** in der **Projektmappen-Explorer**, navigieren Sie zu **Mac erstellen** und Hinzufügen`--registrar:static` auf die **Mmp zusätzliche Argumente:** Feld. Zum Beispiel:

![Argumente für zusätzliche Mmp partielle statische Registrar hinzugefügt](how-it-works-images/psr01.png "zusätzliche Mmp Argumente partielle statische Registrar hinzugefügt")

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Hier sind einige weitere ausführlichen erläuterungen zu den intern wie etwas funktioniert:

- [Selektoren Objective-C](~/ios/internals/objective-c-selectors.md)
- [Registrierungsstelle](~/ios/internals/registrar.md)
- [Xamarin einheitliche API für iOS und OS X](~/cross-platform/macios/unified/index.md)
- [Theading-Grundlagen](~/ios/app-fundamentals/threading.md)
- [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Informationen zu `newrefcount`](~/ios/internals/newrefcount.md)

