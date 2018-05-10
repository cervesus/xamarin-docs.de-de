---
title: Releaseverlauf
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 9a29b131af706b9dedc808a156cdfaffa4173882
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="release-history"></a>Releaseverlauf

## <a name="34-october-11-2017"></a>3.4 (11 Oktober 2017)

[V3.4.0 herunterladen](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Unterstützung für Xcode 9: iOS 11, MacOS 10,13 tvos. außerdem wurden 11 und WatchOS 4
* Probleme mit SIMD und Tgmath sollte nun behoben werden
* Telemetrie wurde vollständig entfernt wurde

## <a name="33-august-3-2016"></a>3.3 (3. August 2016)

[V3.3.0 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Unterstützung für Xcode 8 Beta 4 "," iOS "10", "MacOS 10.12", "tvos. außerdem wurden 10 und WatchOS 3.
* Aktualisiert, um die neuesten masterbuild (2016-08-02) Clang
* [Beibehalten von Telemetriedaten Übermittlung Optionen](https://twitter.com/Symbiatch/status/760373403878559744) aus `sharpie pod bind` auf `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14 Juni 2016)

[V3.2.3 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Unterstützung für Xcode 8 Beta 1, iOS, 10, MacOS 10.12, tvos. außerdem wurden 10 und WatchOS 3.

## <a name="31-may-31-2016"></a>3.1 (31. Mai 2016)

[V3.1.1 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Unterstützung für CocoaPods 1.0
* Verbesserte CocoaPods Bindung Zuverlässigkeit durch Erstellen von systemeigenen `.framework` und anschließendes Binden, die
* Kopieren CocoaPods `.framework` und binden die Definition in einer `Binding` Verzeichnis zum Vereinfachen der Integration mit Xamarin.iOS und Xamarin.Mac Bindung-Projekten

## <a name="30-october-5-2015"></a>3.0 (5 Oktober 2015)

[V3.0.8 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Unterstützung für neue einfache generische Typen und NULL-Zulässigkeit, einschließlich der in Xcode 7 eingeführten Objective-C-Sprachfunktionen
* Unterstützung für die neueste iOS und Mac-SDKs.
* Xcode-Projekt erstellen und analyseunterstützung! Sie können jetzt ein vollständiger Xcode-Projekt an Ziel Sharpie übergeben und führen empfiehlt es sich um die richtige Vorgehensweise zu ermitteln (z. B. `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) unterstützen! Sie jetzt konfigurieren, erstellen und binden Sie CocoaPods direkt vom Ziel Sharpie (z. B. `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Beim Binden von Frameworks Clang Module werden aktiviert, wenn das Framework unterstützt, wodurch die korrektesten Analyse von einem Framework, da durch die Struktur von Framework definiert ist seine `module.modulemap`.
* Für Xcode-Projekte, die ein Produkt Framework zu erstellen, analysiert werden das Produkt anstelle einer intermediate Produktziele wie nicht-Framework-Zielen in Xcode-Projekt weiterhin Mehrdeutigkeiten haben können, die nicht automatisch aufgelöst werden kann.

## <a name="216-march-17-2015"></a>2.1.6 (17 März 2015)

[V2.1.6 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Feste binärer Operator ausdrucksbindung: die linke Seite des Ausdrucks war nicht ordnungsgemäß mit der rechten ausgetauscht (z. B. `1 << 0` wurde fälschlicherweise als gebunden `0 << 1`). Danke, Adam Kemp für bemerken dies!
* Es wurde ein Problem mit `NSInteger` und `NSUInteger` gebunden wird, als `int` und `uint` anstelle von `nint` und `nuint` auf i386; `-DNS_BUILD_32_LIKE_64` wird jetzt auf Clang Analyse vornehmen übergeben `objc/NSObjCRuntime.h` auf i386 erwartungsgemäß arbeiten.
* Die standardmäßige Architektur für Mac OS X-SDKs (z. B. `-sdk macosx10.10`) ist jetzt x86_64 anstelle von "I386", sodass `-arch` kann ausgelassen werden, es sei denn, Sie überschreiben die Standardeinstellung gewünscht wird.

## <a name="210-march-15-2015"></a>2.1.0 (15. März 2015)

[V2.1.0 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [Bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Stellen Sie sicher `using ObjCRuntime;` wird erzeugt, wenn `ArgumentSemantic` verwendet wird.
* [Bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Stellen Sie sicher `using System.Runtime.InteropServices;` wird erzeugt, wenn `DllImport` verwendet wird.
* [Bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): Default `DllImport` zum Laden von Symbolen aus `__Internal`.
* [Bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Vorwärts deklarierte Objective-C Container Deklarationen überspringen.
* [Bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): Protokolltypen mit einer einzelnen Qualifikation als konkrete Schnittstellen binden (`id<Foo>` als `Foo` anstelle von `Foundation.NSObject<Foo>`).
* [Bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): Binden `UInt32`, `UInt64`, und `Int64` Literale als `Int32` zum Löschen der `u` und/oder `uL` Suffixe bei sicher die Werte in passen `Int32`.
* [Bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): beheben Enum Namen zuordnen, beim Starten von systemeigenen Originalname mit einer `k` Präfix.
* `sizeof` C#-Ausdrücke, deren Argumenttyp ein C#-Grundtyp zugeordnet wird, in Clang ausgewertet und als ein Integer-literal gebunden werden, um zu vermeiden, generiert ungültig c#.
* Beheben Sie Objective-C-Syntax für Eigenschaften, deren Typ ein Block ist (Objective-C-Code wird in den Kommentaren über gebundene Deklarationen).
* Decayed Typen als ihren ursprünglichen Typ binden (`int[]` zu dämpft `int*` während der semantischen Analyse in Clang aber binden wie das Original als geschrieben `int[]` stattdessen).

Danke, dass sehr viel zu Dave Dunkin reporting viele der in diesem Punkt Release behobenen Fehler!

## <a name="200-march-9-2015"></a>2.0.0: 9. März 2015

[Version 2.0.0 und herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Objektive Sharpie 2.0 ist eine Hauptversion, die eine verbesserte Clang-basierten Treiber und Parser und ein neues NRefactory basierende Bindung-Modul. Geben Sie für diese verbesserten Komponenten _viel_ besser Bindungen, insbesondere im Hinblick auf Typs "Bindung". Zahlreiche Verbesserungen wurden vorgenommen, die viele Benutzer sichtbaren Funktionen in zukünftigen Versionen gedachten für Objective Sharpie intern sind.

Objektive Sharpie 2.0 basiert auf Clang 3.6.1.

### <a name="type-binding-improvements"></a>Verbesserungen des geschäftstyps-Bindung

* Objective-C-Blöcke werden jetzt unterstützt. Dies umfasst anonyme/Inline-Blöcke und Blöcke, die mit dem Namen über `typedef`. Anonyme Blöcken werden als gebunden werden `System.Action` oder `System.Func` delegiert, während der benannte Blöcken gebunden werden als starken Namen `delegate` Typen.

* Es wird eine verbesserte naming Heuristik für anonyme Enumerationen, die unmittelbar vorangestellt sind eine `typedef` Auflösen von z. B. in einen Ganzzahltyp "Vordefiniert" `long` oder `int`.

* C-Zeiger gebunden sind jetzt als C#- `unsafe` Zeiger anstelle von `System.IntPtr`. Dies führt zu mehr Klarheit in der Bindung für Wenn Sie möglicherweise Zeigerparametern in aktivieren möchten `out` oder `ref` Parameter. Es ist nicht möglich, immer abzuleiten, ob ein Parameter sein sollten `out` oder `ref`, sodass der Zeiger in der Bindung einfacher Überwachung sicherungsspeicherverwendung beibehalten wird.

* Eine Ausnahme, die oben genannten Zeiger-Bindung ist beim Auftreten eines 2-Rang Zeigers auf ein Objective-C-Objekt als Parameter. In diesen Fällen Konvention vorherrschende ist und der Parameter wird als gebunden werden `out` (z. B. `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Überprüfen von Attribut

Sie werden häufig feststellen, dass es sich bei Bindungen, die vom Ziel Sharpie erzeugt jetzt mit Anmerkung versehen sein werden die `[Verify]` Attribut. Diese Attribute geben, Sie sollten _überprüfen_ , Ziel Sharpie die richtige Eingabe wurde durch Vergleichen der Bindung mit der ursprünglichen C#/Objective-C-Deklaration (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt werden).

Ist eine Überprüfung der _empfohlen_ für alle gebundenen Deklarationen, wird Sie jedoch wahrscheinlich _erforderlichen_ für Deklarationen mit Anmerkung versehen der `[Verify]` Attribut. Dies ist in vielen Fällen gibt es ist nicht genügend Metadaten in den ursprünglichen systemeigene Quellcode, wie eine Bindung am besten erzeugt abzuleiten. Sie müssen möglicherweise verweisen Dokumentation oder in den Kommentaren im Code in den Headerdateien, um die beste Bindung Entscheidung zu treffen.

Finden Sie unter der [überprüfen Attribute](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) Dokumentation.

### <a name="other-notable-improvements"></a>Weitere wichtigen Verbesserungen

* `using` Anweisungen werden jetzt generiert basierend auf Datentypen gebunden. Z. B. wenn ein `NSURL` gebunden wurde, eine `using Foundation;` Anweisung ebenfalls generiert.

* `struct` und `union` Deklarationen werden jetzt gebunden werden, mithilfe der `[FieldOffset]` Trick für Unions.

* Enumerationswerte mit Initialisierern Konstantenausdruck werden jetzt ordnungsgemäß gebunden werden. der vollständige Ausdruck wird in c# übersetzt.

* Variadic-Methoden und Blöcke werden jetzt gebunden.

* Frameworks werden jetzt unterstützt, über die `-framework` Option. Finden Sie in der Dokumentation auf [Native Frameworks binden](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) Weitere Details.

* Objective-C-Quellcode werden automatisch erkannt, die sollte nicht erforderlich, übergeben `-ObjC` oder `-xobjective-c` , manuell Clang.

* Clang Modul Verwendung (`@import`) ist jetzt automatisch erkannt, die sollten die entfällt übergeben `-fmodules` zu Clang manuell für Bibliotheken, die das neue Modul-Unterstützung in Clang verwenden.

* Die Xamarin-Unified-API ist jetzt das Standardziel für die Bindung. Verwenden Sie die `-classic` Option aus, um die einzige klassischen 32-Bit-API als Ziel.

### <a name="notable-bug-fixes"></a>Wichtige Fehlerbehebungen

* Beheben Sie `instancetype` bei der Verwendung in einer Kategorie Objective-C-Bindung
* Benennen Sie vollständig Objective-C-Kategorien
* Objective-C-Protokolle mit Präfix `I` (z. B. `INSCopying` anstelle von `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 21 vom Dezember 2014

[V1.1.35 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Behebung geringfügiger Programmfehler.

## <a name="111-december-15-2014"></a>1.1.1: 15. Dezember 2014

[1.1.1 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 wurde die erste Hauptversion nach 1,5 Jahren für interne Verwendung und Entwicklung von Xamarin, befolgen die erste Vorschau des Ziels Sharpie im April 2013. Diese Version ist die erste in der Regel stabil und von einer Vielzahl von systemeigenen Bibliotheken, die mit einem neuen Clang Back-End-berücksichtigt werden.

