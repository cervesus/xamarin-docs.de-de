---
title: Die ersten Schritte mit dem Ziel-Sharpie
description: Dieses Dokument enthält eine allgemeine Übersicht über den Ziel-Sharpie, das Tool, das zum Automatisieren der Erstellung C# von Bindungen an den Ziel-C-Code verwendet wird.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: b055ecadc007a07ff1946df4ac4203f36ebf88ee
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016216"
---
# <a name="getting-started-with-objective-sharpie"></a>Die ersten Schritte mit dem Ziel-Sharpie

> [!IMPORTANT]
> Ziel-Sharpie ist ein Tool für erfahrene xamarin-Entwickler mit erweiterten Kenntnissen von Target-C (und der Erweiterung C). Bevor Sie versuchen, eine Ziel-C-Bibliothek zu binden, sollten Sie fundierte Kenntnisse darüber haben, wie Sie die native Bibliothek in der Befehlszeile erstellen (und ein gutes Verständnis der Funktionsweise der systemeigenen Bibliothek).

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Ziel-Sharpie wird installiert

Der Ziel-Sharpie ist derzeit ein eigenständiges Befehlszeilen Tool für Mac OS X 10,10 und neuer und _kein vollständig unterstütztes xamarin-Produkt_. Sie sollte nur von erweiterten Entwicklern verwendet werden, um ein Bindungs Projekt für eine Ziel-C-Bibliothek eines Drittanbieters zu erstellen.

Ziel-Sharpie kann als Standard Installationsprogramm für OS X-Pakete heruntergeladen werden.
Führen Sie das Installationsprogramm aus, und folgen Sie den Anweisungen auf dem Bildschirm des Installations-Assistenten:

- **Aktuelle Version: 3,4**
  - [Neueste Version herunterladen](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Forums Ankündigung](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Verwenden Sie den Befehl `sharpie update`, um ein Update auf die neueste Version durchführen.

## <a name="basic-walkthrough"></a>Einfache Exemplarische Vorgehensweise

Ziel-Sharpie ist ein von xamarin bereitgestelltes Befehlszeilen Tool, das die Erstellung der Definitionen unterstützt, die zum Binden einer Drittanbieter C#-Ziel-C-Bibliothek an erforderlich sind.
Selbst bei Verwendung von Ziel-Sharpie muss *der Entwickler die* generierten Dateien ändern, nachdem der Ziel-Sharpie abgeschlossen ist, um Probleme zu beheben, die nicht automatisch vom Tool behandelt werden konnten.

Wenn möglich, wird der Ziel-Sharpie APIs mit Anmerkungen versehen, mit denen es zweifelhaft ist, wie die ordnungsgemäße Bindung erfolgt (viele Konstrukte im nativen Code sind mehrdeutig).
Diese Anmerkungen werden als [`[Verify]` Attribute](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)angezeigt.

Bei der Ausgabe des Ziel-sharkreises handelt es sich um ein Datei Paar [`ApiDefinition.cs` und `StructsAndEnums.cs`](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , mit dem ein Bindungs Projekt erstellt werden kann, das in eine Bibliothek kompiliert wird, die Sie in xamarin-Apps verwenden können.

> [!IMPORTANT]
> Der Ziel-Sharpie enthält eine **wesentliche** Regel für die richtige Verwendung: Sie müssen die richtigen clang-compilerbefehlszeilenargumente übergeben, um eine ordnungsgemäße Parsing sicherzustellen. Dies liegt daran, dass die Ziel-Sharpie-Testphase einfach ein Tool ist, das für [die clang-libtooling-API implementiert](https://clang.llvm.org/docs/LibTooling.html)ist.

Dies bedeutet, dass der Ziel-sharkreis die volle Leistungsfähigkeit von clang hat (der c/C++ Target-C/Compiler, der die native Bibliothek, die Sie binden würden, tatsächlich kompiliert) und alle internen Kenntnisse der Header Dateien für die Bindung.
Anstatt die analysierte [Ast](https://en.wikipedia.org/wiki/Abstract_syntax_tree) in den Objektcode zu übersetzen, übersetzt Target Sharpie die AST in C# eine Bindung "Gerüst", die für die Eingabe in die`bmac`und`btouch`xamarin-Bindungs Tools geeignet ist.

Wenn bei der Analyse-Anforderung ein Fehler auftritt, bedeutet dies, dass clang während der Testphase einen Fehler verursacht hat, wenn versucht wird, die Ast zu erstellen, und Sie müssen herausfinden, warum.

**Neu!** in Version 3,0 werden einige dieser Komplexität durch die direkte Unterstützung von Xcode-Projekten behandelt. Wenn eine native Bibliothek über ein gültiges Xcode-Projekt verfügt, kann Ziel-Sharpie das Projekt für ein angegebenes Ziel und eine bestimmte Konfiguration auswerten, um die erforderlichen Eingabe Header Dateien und Compilerflags herzuleiten.

Wenn kein Xcode-Projekt verfügbar ist, müssen Sie sich mit dem Projekt vertraut machen, indem Sie die richtigen Eingabe Header Dateien, Header Datei Suchpfade und andere erforderliche Compilerflags ableiten. Beachten Sie, dass die Compilerflags, mit denen die native Bibliothek erstellt wird, identisch sind, die an das Ziel-Sharpie übermittelt werden müssen. Dabei handelt es sich um einen manuellen Prozess, bei dem ein wenig Vertrautheit mit der Kompilierung von nativem Code in der Befehlszeile mit der clang-Toolkette erforderlich ist.

**Neu!** in Version 3,0 wird auch ein Tool zum einfachen Binden von [cocoapods](https://cocoapods.org) über den `sharpie pod`-Befehl eingeführt.
Wenn die Bibliothek, an der Sie interessiert sind, als cocoapod verfügbar ist, empfiehlt es sich, zunächst zu versuchen, die cocoapod mit dem Ziel-Sharpie zu binden (im Gegensatz zur direkten Bindung an die Quelle).
