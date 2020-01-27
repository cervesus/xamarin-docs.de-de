---
title: Ziel-Sharpie-releaseverlauf
description: In diesem Dokument wird der releaseverlauf von Ziel-Sharpie beschrieben, das Tool, das C# zum Automatisieren der Erstellung von Bindungen an den Ziel-C-Code verwendet wird.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: f60be3f7dc14749f5cd58d5228c17fa85282cd78
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725350"
---
# <a name="objective-sharpie-release-history"></a>Ziel-Sharpie-releaseverlauf

## <a name="34-october-11-2017"></a>3,4 (11. Oktober 2017)

[Download v 3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Unterstützung für Xcode 9: IOS 11, macOS 10,13, tvos 11 und watchos 4
* Probleme mit SIMD und tgmath sollten jetzt behoben werden.
* Die Telemetriedaten wurden vollständig entfernt.

## <a name="33-august-3-2016"></a>3,3 (3. August 2016)

[Download v 3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Unterstützung für Xcode 8 Beta 4, IOS 10, macOS 10,12, tvos 10 und watchos 3.
* Aktualisiert auf den neuesten clang-masterbuild (2016-08-02)
* Belassen Sie die [telemetrieübermittlungs-Optionen](https://twitter.com/Symbiatch/status/760373403878559744) von `sharpie pod bind` in `sharpie bind`.

## <a name="32-june-14-2016"></a>3,2 (14. Juni 2016)

[Download v 3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Unterstützung für Xcode 8 Beta 1, IOS 10, macOS 10,12, tvos 10 und watchos 3.

## <a name="31-may-31-2016"></a>3,1 (31. Mai 2016)

[Download v 3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Unterstützung für cocoapods 1,0
* Verbesserte koapods-Bindungs Zuverlässigkeit durch erstes aufbauen eines systemeigenen `.framework` und anschließendes Binden
* Kopieren Sie die cocoapods-`.framework` und die Bindungs Definition in ein `Binding` Verzeichnis, um die Integration mit xamarin. IOS-und xamarin. Mac-Bindungs Projekten zu vereinfachen.

## <a name="30-october-5-2015"></a>3,0 (5. Oktober 2015)

[Download v 3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Unterstützung für neue Ziel-C-sprach Features einschließlich Lightweight Generika und NULL-Zulässigkeit, wie in Xcode 7 eingeführt
* Unterstützung für die neuesten IOS-und Mac-SDOs.
* Unterstützung für das entwickeln und unterstützen von Xcode-Projekten! Sie können jetzt ein vollständiges Xcode-Projekt an das Ziel-Sharpie übergeben, um das richtige zu finden (z. b. `sharpie bind Project.xcodeproj -sdk ios`).
* Unterstützung für [cocoapods](https://cocoapods.org) ! Sie können nun cocoapods direkt aus dem Ziel-Sharpie (z. b. `sharpie pod init ios AFNetworking && sharpie pod bind`) konfigurieren, erstellen und binden.
* Beim Binden von Frameworks werden clang-Module aktiviert, wenn Sie vom Framework unterstützt werden. Dies führt zu einer optimalen Verarbeitung eines Frameworks, da die Struktur des Frameworks durch die `module.modulemap`definiert wird.
* Analysieren Sie für Xcode-Projekte, die ein frameworkprodukt erstellen, das Produkt anstelle von zwischengeschalteten Produkt Zielen, da nicht-Framework-Ziele in einem Xcode-Projekt möglicherweise noch Mehrdeutigkeiten aufweisen, die nicht automatisch aufgelöst werden können.

## <a name="216-march-17-2015"></a>2.1.6 (17. März 2015)

* Korrigieren der Ausdrucks Bindung des binären Operators: die linke Seite des Ausdrucks wurde fälschlicherweise mit der rechten Seite ausgetauscht (z. b. `1 << 0` fälschlicherweise als `0 << 1`gebunden). Vielen Dank, dass Adam Kemp dies bemerkt hat!
* Es wurde ein Problem behoben, bei dem `NSInteger` und `NSUInteger` als `int` und `uint` als `nint` und `nuint` auf i386 gebunden wurden. `-DNS_BUILD_32_LIKE_64` wird jetzt an clang übergeben, damit die `objc/NSObjCRuntime.h` auf dem i386 erwartungsgemäß funktioniert.
* Die Standard Architektur für Mac OS X SDI (z. b. `-sdk macosx10.10`) wird jetzt anstelle von i386 x86_64, sodass `-arch` weggelassen werden kann, es sei denn, das Überschreiben der Standardeinstellung ist erwünscht.

## <a name="210-march-15-2015"></a>2.1.0 (15. März 2015)

[Download v 2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [BXC # 27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Stellen Sie sicher, dass `using ObjCRuntime;` bei der Verwendung `ArgumentSemantic` erzeugt wird.
* [BXC # 27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Stellen Sie sicher, dass `using System.Runtime.InteropServices;` bei der Verwendung `DllImport` erzeugt wird.
* [BXC # 27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): Standard `DllImport` zum Laden von Symbolen aus `__Internal`.
* [BXC # 27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Überspringen von vorwärts deklarierten Ziel-C-Container Deklarationen.
* [BXC # 27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): Binden von Protokolltypen mit einer einzelnen Qualifikation als konkrete Schnittstellen (`id<Foo>` als `Foo` anstelle `Foundation.NSObject<Foo>`).
* [BXC # 28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): Binden von `UInt32`, `UInt64`und `Int64` literalen als `Int32`, um die `u`-und/oder `uL` Suffixe zu löschen, wenn die Werte sicher in `Int32`passen.
* [BXC # 28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): Korrektur der Enumeration-Namenszuordnung, wenn der ursprüngliche Native Name mit einem `k` Präfix beginnt.
* `sizeof` C-Ausdrücke, deren Argumenttyp keinem primitiven Typ C# zugeordnet ist, werden in clang ausgewertet und als Ganzzahlliteral gebunden, um C#zu vermeiden, dass ein ungültiges erzeugt wird.
* Korrigieren Sie die Ziel-c-Syntax für Eigenschaften, deren Typ ein Block ist (der Ziel-c-Code wird in Kommentaren oberhalb der gebundenen Deklarationen angezeigt).
* Binden Sie veraltete Typen als den ursprünglichen Typ (`int[]` bei der semantischen Analyse in clang `int*`, aber binden Sie Sie stattdessen als den ursprünglichen `int[]`.

Vielen Dank, dass Dave Dunkin viele der in dieser Punkt Version behobenen Fehler meldet!

## <a name="200-march-9-2015"></a>2.0.0:9. März 2015

[Download v 2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Ziel-Sharpie 2,0 ist ein Haupt Release, das einen verbesserten clang-basierten Treiber und Parser und ein neues nrefactory-basiertes Bindungs Modul enthält. Diese verbesserten Komponenten bieten _viel_ bessere Bindungen, insbesondere bei der Typbindung. Viele weitere Verbesserungen wurden vorgenommen, die für den Ziel-Sharpie intern sind und in zukünftigen Versionen viele Benutzer sichtbare Features liefern.

Ziel-Sharpie 2,0 basiert auf clang-3.6.1.

### <a name="type-binding-improvements"></a>Verbesserte Typbindung

* Ziel-C-Blöcke werden jetzt unterstützt. Dies schließt anonyme/Inline-Blöcke und-Blöcke ein, die über `typedef`benannt werden. Anonyme Blöcke werden als `System.Action` oder `System.Func` Delegaten gebunden, während benannte Blöcke als stark benannte `delegate` Typen gebunden werden.

* Es gibt eine verbesserte Benennungs-Heuristik für anonyme Enumerationen, denen unmittelbar eine `typedef` der Auflösung in einen integralen integralen Typ (z. b. `long` oder `int`) vorangestellt ist.

* C-Zeiger werden nun als C# `unsafe` Zeiger anstelle `System.IntPtr`gebunden. Dies führt zu einer besseren Übersichtlichkeit in der Bindung, wenn Sie Zeiger Parameter möglicherweise in `out`-oder `ref` Parameter umwandeln möchten. Es ist nicht möglich, immer abzuleiten, ob ein Parameter `out` oder `ref`werden soll, sodass der Zeiger in der Bindung beibehalten wird, um eine einfachere Überwachung zu ermöglichen.

* Eine Ausnahme von der obigen Zeiger Bindung ist, wenn ein 2-Rang-Zeiger auf ein Ziel-C-Objekt als Parameter gefunden wird. In diesen Fällen ist die Konvention vorherrschend, und der Parameter wird als `out` (z. b. `NSError **error` → `out NSError error`) gebunden.

### <a name="verify-attribute"></a>Attribut überprüfen

Sie werden häufig feststellen, dass Bindungen, die von Ziel-Sharpie erzeugt werden, jetzt mit dem `[Verify]`-Attribut versehen werden. Diese Attribute geben an, dass Sie sicherstellen sollten, dass Ziel-Sharpie das richtige _ist, indem_ Sie die Bindung mit der ursprünglichen c/Ziel-c-Deklaration vergleichen (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt wird).

Die Überprüfung wird für alle gebundenen Deklarationen _empfohlen_ , ist aber wahrscheinlich für Deklarationen _erforderlich_ , die mit dem `[Verify]`-Attribut kommentiert werden. Dies liegt daran, dass in vielen Situationen nicht genügend Metadaten im ursprünglichen systemeigenen Quellcode vorhanden sind, um abzuleiten, wie eine Bindung am besten erzeugt wird. Möglicherweise müssen Sie in den Header Dateien auf Dokumentation oder Code Kommentare verweisen, um die beste Bindungs Entscheidung zu treffen.

Weitere Informationen finden Sie in der Dokumentation [Überprüfen von Attributen](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) .

### <a name="other-notable-improvements"></a>Weitere wichtige Verbesserungen

* `using`-Anweisungen werden nun basierend auf gebundenen Typen generiert. Wenn beispielsweise ein `NSURL` gebunden wurde, wird auch eine `using Foundation;` Anweisung generiert.

* `struct`-und `union` Deklarationen werden nun mit dem `[FieldOffset]`-Trick für Unions gebunden.

* Enumerationswerte mit konstanten Ausdrucks Initialisierern werden nun ordnungsgemäß gebunden. der vollständige Ausdruck wird in C#übersetzt.

* Variadic-Methoden und-Blöcke sind nun gebunden.

* Frameworks werden nun über die Option `-framework` unterstützt. Weitere Informationen finden Sie in der Dokumentation zum Binden von systemeigenen [Frameworks](~/cross-platform/macios/binding/objective-sharpie/index.md) .

* Der Ziel-C-Quellcode wird jetzt automatisch erkannt, wodurch die Notwendigkeit entfällt, `-ObjC` oder `-xobjective-c` manuell an clang zu übergeben.

* Die Verwendung von clang-Modulen (`@import`) wird jetzt automatisch erkannt, sodass die Notwendigkeit entfällt, `-fmodules` für Bibliotheken, die die neue Modulunterstützung in clang verwenden, manuell an clang zu übergeben.

* Das xamarin-Unified API ist nun das Standard Bindungs Ziel. Verwenden Sie die Option `-classic`, um nur das 32-Bit-Classic API als Ziel zu verwenden.

### <a name="notable-bug-fixes"></a>Wichtige Fehlerbehebungen

* Korrektur `instancetype` Bindung bei Verwendung in einer Ziel-C-Kategorie
* Ziel-C-Kategorien vollständig benennen
* Präfix von Ziel-C-Protokollen mit `I` (z. b. `INSCopying` anstelle `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35:21. Dezember 2014

[Download v 1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Behebung geringfügiger Programmfehler.

## <a name="111-december-15-2014"></a>1.1.1:15. Dezember 2014

[Download v 1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 war das erste Haupt Release nach 1,5 Jahren interner Verwendung und Entwicklung bei xamarin nach der ersten Vorschau von Ziel-Sharpie im April 2013. Diese Version ist die erste, die allgemein als stabil eingestuft und für eine Vielzahl von nativen Bibliotheken verwendet werden kann, die ein neues clang-Back-End enthalten.
