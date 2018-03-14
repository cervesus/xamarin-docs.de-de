---
title: Erste Schritte
ms.topic: article
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 5d1de42a398affc1b6b6c9b863df5bc86a016c08
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started"></a>Erste Schritte

> [!IMPORTANT]
> **Warnung:** objektiven Sharpie ist ein Tool für erfahrene Entwickler von Xamarin mit Kenntnissen von Objective-C (und durch Erweiterung auch C). Bevor Sie versuchen, eine Bibliothek für Objective-C-Bindung sollten Sie solide Kenntnisse im Erstellen Sie die systemeigene Bibliothek auf der Befehlszeile aus (und ein gutes Verständnis der Funktionsweise der systemeigenen Bibliothek) verfügen.

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Installieren von Dienstziel Sharpie

Objektive Sharpie befindet sich derzeit ein eigenständiges Befehlszeilentool für Mac OS X 10.10 und höher, und ist _nicht vollständig unterstützt Xamarin-Produkt_. Nur sollte von erfahrenen Entwicklern verwendet werden, beim Erstellen eines Projekts für die Bindung an a 3rd Objective-C-Bibliothek eines Drittanbieters.

Objektive Sharpie kann als standard OS X-Paket-Installationsprogramm heruntergeladen werden.
Führen Sie das Installationsprogramm, und führen Sie die aufforderungen vom Installations-Assistenten:

- **Aktuelle Version: 3.4**
  - [Herunterladen der neuesten Version](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Forum-Ankündigung](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Verwenden der `sharpie update` Befehl aus, um auf die neueste Version zu aktualisieren.

## <a name="basic-walkthrough"></a>Grundlegende exemplarische Vorgehensweise

Objektive Sharpie ist ein Befehlszeilentool, bereitgestellten von Xamarin, die hilft bei der Erstellung der Definitionen erforderlich, um eine 3rd Party Objective-C-Bibliothek in c# zu binden.
Selbst bei Verwendung des Ziels Sharpie, der Entwickler *wird* müssen die generierten Dateien ändern, nachdem Ziel Sharpie abgeschlossen ist, um auftretende Probleme zu behandeln, die vom Tool nicht automatisch verarbeitet werden konnte.

Wenn möglich, Ziel Sharpie wird mit einer Anmerkung versehen APIs, mit denen sie einige unsicher hat, auf wie ordnungsgemäß gebunden (viele Konstrukte in systemeigenen Code sind mehrdeutig).
Diese Anmerkungen hostnamensadresse [ `[Verify]` Attribute](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Die Ausgabe des Ziels Sharpie besteht aus einem Paar von Dateien - [ `ApiDefinition.cs` und `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -, die verwendet werden kann, um eine bindungsprojekt zu erstellen, welche Kompilierungen ein, die in einer Bibliothek in die Xamarin-apps verwendet werden können.

> [!IMPORTANT]
> Objektive Sharpie stammen, mit einem **wichtigen** Regel für die richtige Verwendung: müssen Sie unbedingt übergeben sie die richtige Clang Compiler-Befehlszeilenargumente um sicherzustellen, dass der richtige analysieren. Dies ist, da der Ziel-Sharpie Analyse Phase ein einfaches Tool ist [für die Clang-Libtooling API implementiert](http://clang.llvm.org/docs/LibTooling.html).

Dies bedeutet, dass das Ziel Sharpie hat die volle Leistung von Clang (die C/Objective-C/C++-Compiler, die tatsächlich die systemeigene Bibliothek kompiliert, die Sie binden möchten) und alle internen Beleg der Headerdateien für die Bindung.
Anstatt das analysierte übersetzen [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) zu Objektcode, Ziel Sharpie übersetzt der AST, ein C#-Bindung "Gerüst" geeignet für die Eingabe, die `bmac` und `btouch` Xamarin-Bindung-Tools.

Wenn Ziel Sharpie Fehlern während der Analyse, es, errored out während Clang bedeutet, dass seine Analyse phase AST erstellen möchten, und Sie müssen herausfinden, warum.

**NEU!** Version 3.0 Versuche lösen einige dieser Komplexität, indem Sie Xcode-Projekte direkt unterstützt. Verfügt eine systemeigene Bibliothek über ein gültiges Xcode-Projekt, kann das Projekt für eine angegebene Ziel und die Konfiguration der erforderlichen Eingabe Headerdateien und Compilerflags abzuleiten Ziel Sharpie ausgewertet werden.

Wenn kein Xcode-Projekt verfügbar ist, müssen Sie vertrauter mit dem Projekt werden durch die richtige Eingabe Header-Dateien, Suchpfade Header und anderen erforderlichen Compilerflags ableiten. Es ist wichtig zu bedenken, dass der Compilerflags verwendet, um die systemeigene Bibliothek erstellen identisch sind, die zum Ziel Sharpie übergeben werden muss. Dies ist eine stärker manuellen Prozess, und eine, die einen Teil der Kenntnisse über das Kompilieren von systemeigenem Code in der Befehlszeile mit der Clang-toolkette erfordert.

**NEU!** Version 3.0 führt außerdem ein Tool für die einfache Bindung [CocoaPods](https://cocoapods.org) über die `sharpie pod` Befehl.
Wenn die Bibliothek, der Sie interessiert sind als ein CocoaPod verfügbar ist, wird empfohlen, dass Sie starten, indem die CocoaPod mit Ziel Sharpie Bindungsversuch (im Gegensatz zu versuchen, direkt für die Quelle die Bindung).
