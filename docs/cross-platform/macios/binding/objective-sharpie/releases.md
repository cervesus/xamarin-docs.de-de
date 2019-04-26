---
title: Objektive Sharpie-Versionsverlauf
description: Dieses Dokument beschreibt die Versionsgeschichte von Ziel Sharpie, das Tool verwendet, um die Erstellung von automatisieren C# -Bindungen mit Objective-C-Code.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 03e4a5ac8906d2593cbdf3c15f6b2d1f4a2c6d19
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199648"
---
# <a name="objective-sharpie-release-history"></a>Objektive Sharpie-Versionsverlauf

## <a name="34-october-11-2017"></a>3.4 (11. Oktober 2017)

[V3.4.0 herunterladen](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Unterstützung für Xcode 9: iOS 11, MacOS 10.13, TvOS 11 und WatchOS 4
* Probleme mit SIMD und Tgmath soll nun behoben werden
* Telemetrie wurde vollständig entfernt

## <a name="33-august-3-2016"></a>3.3 (3. August 2016)

[V3.3.0 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Unterstützung für Xcode 8 Beta 4 "," iOS 10 "," MacOS 10.12 "," TvOS 10 und WatchOS 3.
* Aktualisiert, um die neuesten Clang-masterbuild (2016-08-02)
* [Speichern Sie die Optionen für die Übermittlung von Telemetriedaten](https://twitter.com/Symbiatch/status/760373403878559744) aus `sharpie pod bind` zu `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14. Juni 2016)

[V3.2.3 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Unterstützung für Xcode 8 Beta 1, 10-iOS, MacOS 10.12, TvOS 10 und WatchOS 3.

## <a name="31-may-31-2016"></a>3.1 (31. Mai 2016)

[3.1.1 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Unterstützung für CocoaPods 1.0
* CocoaPods Bindung Zuverlässigkeit verbessert, indem Sie zuerst ein systemeigenes `.framework` und das anschließende binden, die
* Kopieren Sie CocoaPods `.framework` und binden die Definition in einem `Binding` Directory-Integration mit Xamarin.iOS- und Xamarin.Mac-bindungsprojekte zu vereinfachen

## <a name="30-october-5-2015"></a>3.0 (5. Oktober 2015)

[V3.0.8 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Unterstützung für neue einfache generische Typen und NULL-Zulässigkeit, einschließlich der in Xcode 7 eingeführten Objective-C-Sprachfunktionen
* Unterstützung für die aktuelle iOS und Mac-SDKs.
* Xcode-Projekt erstellen und zu analysieren Unterstützung! Sie können jetzt ein vollständiges Xcode-Projekt an Ziel Sharpie übergeben und ist der Fall empfiehlt es sich um das richtige tun zu ermitteln (z. B. `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) unterstützen! Sie können jetzt konfigurieren, erstellen und binden Sie CocoaPods direkt vom Ziel Sharpie (z. B. `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Beim Binden von Frameworks, Clang-Module werden aktiviert, wenn das Framework unterstützt, wodurch die korrektesten Analyse des ein Framework, da durch die Struktur des Frameworks definiert ist seine `module.modulemap`.
* Für Xcode-Projekte, die eine Framework-Produkt erstellen, Analysieren dieses Produkt anstelle einer intermediate Produktziele wie nicht-Framework-Ziele in einem Xcode-Projekt weiterhin Mehrdeutigkeiten haben können, die nicht automatisch aufgelöst werden kann.

## <a name="216-march-17-2015"></a>2.1.6 (17. März 2015)

[V2.1.6 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Ausdrucksbindung festen binärer Operator: die linke Seite des Ausdrucks nicht ordnungsgemäß mit der rechten Seite ausgetauscht wurde (z. B. `1 << 0` wurde fälschlicherweise als gebunden `0 << 1`). Vielen Dank, Adam Kemp für diese an!
* Korrigieren eines Problems mit `NSInteger` und `NSUInteger` gebunden wird, als `int` und `uint` anstelle von `nint` und `nuint` auf i386; `-DNS_BUILD_32_LIKE_64` jetzt an Clang zu Analyse übergeben `objc/NSObjCRuntime.h` auf i386 wie erwartet ausgeführt.
* Die standardmäßige Architektur für Mac OS X SDKs (z. B. `-sdk macosx10.10`) ist jetzt x86_64 anstelle von "I386", also `-arch` kann ausgelassen werden, es sei denn, Sie überschreiben die Standardeinstellung gewünscht wird.

## <a name="210-march-15-2015"></a>2.1.0 (15. März 2015)

[Herunterladen von 2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxC#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Stellen Sie sicher `using ObjCRuntime;` wird erzeugt, wenn `ArgumentSemantic` verwendet wird.
* [bxC#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Stellen Sie sicher `using System.Runtime.InteropServices;` wird erzeugt, wenn `DllImport` verwendet wird.
* [bxC#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): Standard `DllImport` zum Laden von Symbolen aus `__Internal`.
* [bxC#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Objective-C-Container-Deklarationen vorwärts deklarierte zu überspringen.
* [bxC#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): Binden Sie Protokolltypen mit einer einzelnen Qualifikation als konkrete Schnittstellen (`id<Foo>` als `Foo` anstelle von `Foundation.NSObject<Foo>`).
* [bxC#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): Binden Sie `UInt32`, `UInt64`, und `Int64` Literale als `Int32` zum Löschen der `u` und/oder `uL` hinzuzufügen, wenn die Werte in sicher passt `Int32`.
* [bxC#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): Beheben Sie die Enumeration Namen zuordnen, bei der ursprünglichen systemeigenen Name mit beginnt eine `k` Präfix.
* `sizeof` C-Ausdrücke, deren Typ des Arguments entspricht keine C# Grundtyp in Clang ausgewertet und als ein Integer-literal gebunden sind, Generieren von ungültigen vermeiden C#.
* Beheben von Objective-C-Syntax für Eigenschaften, deren Typ ein Block ist (Objective-C-Code wird in den Kommentaren über gebundene Deklarationen angezeigt).
* Decayed Typen als ihren ursprünglichen Typ binden (`int[]` zu dämpft `int*` während der semantischen Analyse in Clang, aber binden wie das Original als geschriebenen `int[]` stattdessen).

Vielen Dank, Dave Dunkin für die berichterstellung vieler der in diesem Release einer Unterversion behobene Probleme!

## <a name="200-march-9-2015"></a>2.0.0: 9. März 2015

[Laden Sie Version 2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Objektive Sharpie 2.0 ist eine Hauptversion, die eine verbesserte Clang-basierten Treibers und Parser und eine neue NRefactory basierende Bindungs-Engine-features. Diese verbesserte Komponenten bereitzustellen, für die _viel_ bessere Bindungen, insbesondere im Hinblick auf Typs "Bindung". Viele andere Verbesserungen wurden vorgenommen, die für die Ziel-Sharpie intern sind, die viele Benutzer sichtbaren Funktionen in zukünftigen Versionen zurückgibt.

Objektive Sharpie 2.0 basiert auf Clang 3.6.1.

### <a name="type-binding-improvements"></a>Verbesserungen des geschäftstyps-Bindung

* Objective-C-Blöcke werden jetzt unterstützt. Dies umfasst die anonyme/Inline-Blöcke und Blöcke, die mit dem Namen über `typedef`. Anonyme Blöcken werden als gebunden werden `System.Action` oder `System.Func` delegiert, während der benannte Blöcken gebunden werden, z.B. der starken Namen `delegate` Typen.

* Es wird eine verbesserte Benennung Heuristik für anonyme Enumerationen, die unmittelbar vorangestellt sind eine `typedef` wie z. B. auf einen integrierten ganzzahligen Typ aufgelöst `long` oder `int`.

* C-Zeiger sind jetzt als gebunden C# `unsafe` Zeiger anstelle `System.IntPtr`. Dies führt zu mehr Klarheit wünschen, in der Bindung für, wenn Sie möglicherweise Zeigerparameter in aktivieren möchten `out` oder `ref` Parameter. Es ist nicht möglich, immer abzuleiten, ob ein Parameter sein soll `out` oder `ref`, sodass der Zeiger, in der Bindung beibehalten wird, um einfacher Überwachung zu ermöglichen.

* Eine Ausnahme von der obigen Bindung der Zeiger ist, wenn es sich bei ein 2-Rang Zeiger auf ein Objective-C-Objekt als Parameter gefunden wird. In diesen Fällen Konvention verbreitetsten ist und der Parameter gebunden werden wird, als `out` (z. B. `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Überprüfen von Attribut

Sie werden häufig feststellen, dass es sich bei Bindungen, die Ziel-Sharpie erzeugten jetzt mit versehen werden, werden die `[Verify]` Attribut. Diese Attribute geben, Sie sollten _überprüfen_ , wurde die richtige Ziel Sharpie vergleichen die Bindung mit der ursprünglichen C/Objective-C-Deklaration (die in einem Kommentar über der gebundenen Deklaration bereitgestellt wird).

Ist eine Überprüfung der _empfohlen_ für alle gebundenen Deklarationen, ist Sie jedoch wahrscheinlich _erforderlichen_ für Deklarationen mit Anmerkung versehen der `[Verify]` Attribut. Dies ist, da in vielen Fällen nicht genügend Metadaten in den ursprünglichen Code von nativen Quelltypen, wie Sie eine Bindung am besten erzeugen abzuleiten. Möglicherweise müssen Sie sich auf Dokumentation oder in den Kommentaren im Code in den Headerdateien, um die beste Entscheidung für die Bindung zu treffen.

Finden Sie unter den [Attribute überprüfen](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) Dokumentation.

### <a name="other-notable-improvements"></a>Weitere wichtigen Verbesserungen

* `using` -Anweisungen werden jetzt generiert auf Grundlage von Typen gebunden. Z. B. wenn ein `NSURL` gebunden wurde, eine `using Foundation;` Anweisung wird ebenfalls generiert werden.

* `struct` und `union` Deklarationen werden jetzt gebunden ist, die mit der `[FieldOffset]` Trick für Unions.

* Enum-Werte mit konstanter Ausdruck Initialisierern werden jetzt ordnungsgemäß gebunden werden. in der vollständige Ausdruck übersetzt C#.

* Nun werden Variadic-Methoden und Blöcke gebunden.

* Frameworks werden jetzt unterstützt, über die `-framework` Option. Finden Sie in der Dokumentation auf [Binden von nativen Frameworks](https://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) Weitere Details.

* Objective-C-Quellcode werden automatisch erkannt, das sollte nicht erforderlich, übergeben `-ObjC` oder `-xobjective-c` , Clang-manuell.

* Clang-modulverwendung (`@import`) wird jetzt automatisch erkannt, die sollte überflüssig übergeben `-fmodules` , Clang manuell für Bibliotheken, die die neue Unterstützung für das Modul in Clang zu verwenden.

* Der Xamarin-Unified-API ist jetzt das Standardziel für die Bindung. Verwenden Sie die `-classic` Option aus, um die einzige klassischen 32-Bit-API als Ziel.

### <a name="notable-bug-fixes"></a>Wichtige Fehlerbehebungen

* Beheben Sie `instancetype` Bindung bei der Verwendung in einer Objective-C-Kategorie
* Benennen Sie vollständig Objective-C-Kategorien
* Objective-C-Protokolle mit Präfix `I` (z. B. `INSCopying` anstelle von `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 21. Dezember 2014

[V1.1.35 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Kleinere Fehlerbehebungen.

## <a name="111-december-15-2014"></a>1.1.1: Am 15. Dezember 2014

[1.1.1 herunterladen](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 war die erste Hauptversion nach 1,5 Jahren für interne Verwendung und Entwicklung von Xamarin, befolgen die erste Vorschau der Ziel-Sharpie im April 2013 an. Diese Version ist die erste in der Regel stabiler und für eine Vielzahl von systemeigenen Bibliotheken, die mit einem neuen Clang-Back-End berücksichtigt werden.

