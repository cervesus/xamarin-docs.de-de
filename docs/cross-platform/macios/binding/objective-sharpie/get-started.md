---
title: Die ersten Schritte mit dem Ziel-Sharpie
description: Dieses Dokument enthält eine allgemeine Übersicht über den Ziel-Sharpie, das Tool, das zum Automatisieren der Erstellung C# von Bindungen an den Ziel-C-Code verwendet wird.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: conceptdev
ms.author: crdun
ms.date: 10/11/2017
ms.openlocfilehash: c34a6c09bf1298fd710e3e39a244294821a714ae
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290737"
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
> Verwenden Sie `sharpie update` den Befehl, um ein Update auf die neueste Version durchführen.

## <a name="basic-walkthrough"></a>Einfache Exemplarische Vorgehensweise

Ziel-Sharpie ist ein von xamarin bereitgestelltes Befehlszeilen Tool, das die Erstellung der Definitionen unterstützt, die zum Binden einer Drittanbieter C#-Ziel-C-Bibliothek an erforderlich sind.
Selbst bei Verwendung von Ziel-Sharpie muss *der Entwickler die* generierten Dateien ändern, nachdem der Ziel-Sharpie abgeschlossen ist, um Probleme zu beheben, die nicht automatisch vom Tool behandelt werden konnten.

Wenn möglich, wird der Ziel-Sharpie APIs mit Anmerkungen versehen, mit denen es zweifelhaft ist, wie die ordnungsgemäße Bindung erfolgt (viele Konstrukte im nativen Code sind mehrdeutig).
Diese Anmerkungen werden als [ `[Verify]` Attribute](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)angezeigt.

Bei der Ausgabe des [ `ApiDefinition.cs` `StructsAndEnums.cs` Ziel-](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) sharkreises handelt es sich um ein Datei Paar, das zum Erstellen eines Bindungs Projekts verwendet werden kann, das in eine Bibliothek kompiliert wird, die Sie in xamarin-Apps verwenden können.

> [!IMPORTANT]
> Der Ziel-Sharpie enthält eine **wesentliche** Regel für die richtige Verwendung: Sie müssen die richtigen clang-compilerbefehlszeilenargumente übergeben, um eine ordnungsgemäße Parsing sicherzustellen. Dies liegt daran, dass die Ziel-Sharpie-Testphase einfach ein Tool ist, das für [die clang-libtooling-API implementiert](http://clang.llvm.org/docs/LibTooling.html)ist.

Dies bedeutet, dass der Ziel-sharkreis die volle Leistungsfähigkeit von clang hat (der c/C++ Target-C/Compiler, der die native Bibliothek, die Sie binden würden, tatsächlich kompiliert) und alle internen Kenntnisse der Header Dateien für die Bindung.
Anstatt die analysierte [Ast](https://en.wikipedia.org/wiki/Abstract_syntax_tree) in den Objektcode zu übersetzen, übersetzt Target Sharpie die AST- C# Datei in eine Bindung "Gerüst", die für `bmac` die `btouch` Eingabe in die-und xamarin-Bindungs Tools geeignet ist.

Wenn bei der Analyse-Anforderung ein Fehler auftritt, bedeutet dies, dass clang während der Testphase einen Fehler verursacht hat, wenn versucht wird, die Ast zu erstellen, und Sie müssen herausfinden, warum.

**NEU!** in Version 3,0 werden einige dieser Komplexität durch die direkte Unterstützung von Xcode-Projekten behandelt. Wenn eine native Bibliothek über ein gültiges Xcode-Projekt verfügt, kann Ziel-Sharpie das Projekt für ein angegebenes Ziel und eine bestimmte Konfiguration auswerten, um die erforderlichen Eingabe Header Dateien und Compilerflags herzuleiten.

Wenn kein Xcode-Projekt verfügbar ist, müssen Sie sich mit dem Projekt vertraut machen, indem Sie die richtigen Eingabe Header Dateien, Header Datei Suchpfade und andere erforderliche Compilerflags ableiten. Beachten Sie, dass die Compilerflags, mit denen die native Bibliothek erstellt wird, identisch sind, die an das Ziel-Sharpie übermittelt werden müssen. Dabei handelt es sich um einen manuellen Prozess, bei dem ein wenig Vertrautheit mit der Kompilierung von nativem Code in der Befehlszeile mit der clang-Toolkette erforderlich ist.

**NEU!** Version 3,0 führt außerdem ein Tool zum einfachen Binden von [cocoapods](https://cocoapods.org) über `sharpie pod` den Befehl ein.
Wenn die Bibliothek, an der Sie interessiert sind, als cocoapod verfügbar ist, empfiehlt es sich, zunächst zu versuchen, die cocoapod mit dem Ziel-Sharpie zu binden (im Gegensatz zur direkten Bindung an die Quelle).
