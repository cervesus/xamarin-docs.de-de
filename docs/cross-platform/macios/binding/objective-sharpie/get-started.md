---
title: Erste Schritte mit objektive Sharpie
description: Dieses Dokument enthält eine allgemeine Übersicht über Objective Sharpie, das Tool verwendet, um die Erstellung von c#-Bindungen mit Objective-C-Code zu automatisieren.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: da8c51c4ba4df74afac950bbff867221e7307d6e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854778"
---
# <a name="getting-started-with-objective-sharpie"></a>Erste Schritte mit objektive Sharpie

> [!IMPORTANT]
> Objektive Sharpie ist ein Tool für erfahrene Xamarin-Entwickler mit Kenntnissen von Objective-C (und durch Erweiterung, C). Bevor Sie versuchen, binden Sie eine Objective-C-Bibliothek sollten Sie wissen, wie Sie die native Bibliothek erstellen, auf der Befehlszeile aus (und ein gutes Verständnis der Funktionsweise der nativen Bibliothek) verfügen.

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Installieren von objektive Sharpie

Objektive Sharpie befindet sich derzeit ein eigenständiges-Befehlszeilentool für Mac OS X 10.10 oder höher und wird _nicht auf eine vollständig unterstützte Xamarin-Produkt_. Es sollte nur von erfahrenen Entwicklern verwendet werden, zur Unterstützung beim Erstellen eines Projekts für die Bindung an eine 3. Objective-C-Bibliothek.

Objektive Sharpie kann als standard OS X Installer-Paket heruntergeladen werden.
Führen Sie das Installationsprogramm aus, und befolgen Sie alle die aufforderungen auf dem Bildschirm vom Installations-Assistenten:

- **Aktuelle Version: 3.4**
  - [Herunterladen der neuesten Version](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Forum-Ankündigung](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Verwenden der `sharpie update` Befehl aus, um auf die neueste Version zu aktualisieren.

## <a name="basic-walkthrough"></a>Einfache Exemplarische Vorgehensweise

Objektive Sharpie ist ein Befehlszeilentool, bereitgestellt von Xamarin, die bei der Erstellung hilft die Definitionen erforderlich sind, um eine 3rd Party Objective-C-Bibliothek in c# zu binden.
Auch wenn Objective Sharpie, den Entwickler verwenden *wird* müssen die generierten Dateien zu ändern, nachdem Ziel Sharpie abgeschlossen ist, um Probleme zu beheben, die vom Tool nicht automatisch verarbeitet werden konnte.

Nach Möglichkeit Ziel Sharpie wird mit einer Anmerkung versehen APIs, die mit dem sie eine unsichere hat, auf wie ordnungsgemäß gebunden (viele Konstrukte in systemeigenen Code sind nicht eindeutig).
Diese Anmerkungen werden [ `[Verify]` Attribute](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Die Ausgabe des Ziel-Sharpie ist ein Dateipaar - [ `ApiDefinition.cs` und `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -, die verwendet werden kann, um ein bindungsprojekt zu erstellen, Kompilieren in eine Bibliothek in Xamarin-apps verwendet werden können.

> [!IMPORTANT]
> Objektive Sharpie ist mit einem **wichtigen** Regel für die richtige Verwendung: müssen Sie unbedingt übergeben sie die richtige Clang Compiler-Befehlszeilenargumente um sicherzustellen, dass der richtige analysieren. Dies liegt daran die Ziel-Sharpie Analyse einfaches Tool [implementiert, die für die Clang-Libtooling API](http://clang.llvm.org/docs/LibTooling.html).

Dies bedeutet, dass das Ziel Sharpie hat das volle Potenzial von Clang (der C/Objective-C/C++-Compiler, die tatsächlich die native Bibliothek kompiliert, die Sie binden soll) und alle internen sein Wissen um die Headerdateien für die Bindung.
Anstatt das analysierte übersetzen [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) in Objektcode, Ziel Sharpie übersetzt die AST, um eine C#-Bindung "Scaffold" geeignet, für die Eingabe, die `bmac` und `btouch` Tools für Xamarin-Bindung.

Wenn Objective Sharpie Fehler auftreten, während der Analyse, es, Clang, die während der fehlerhaften bedeutet Analysieren von der phase die AST erstellen möchten, und Sie müssen herausfinden, warum.

**NEU!** Version 3.0 versucht, die einen Teil dieser Komplexität durch die Unterstützung von Xcode-Projekten direkt behandeln. Verfügt eine native Bibliothek über ein gültiges Xcode-Projekt, kann das Projekt für ein angegebenes Ziel und die Konfiguration, um die erforderlichen Eingabe Headerdateien und Compilerflags Herleiten Ziel Sharpie ausgewertet werden.

Wenn kein Xcode-Projekt verfügbar ist, müssen Sie immer vertrauter mit dem Projekt werden, indem Sie davon ausgehen, die richtige Eingabe-Header-Dateien, Suchpfade Header und anderen erforderlichen Compilerflags. Es ist wichtig zu wissen, dass die Compilerflags verwendet, um die native Bibliothek identisch sind, die zum Ziel Sharpie übergeben werden müssen. Dies ist eine manuelle Vorgehensweise und, die ein wenig vertraut sind, mit der Kompilierung von nativem Code in der Befehlszeile mit dem Clang-toolkette erforderlich ist.

**NEU!** Version 3.0 führt außerdem ein Tool für die einfache Bindung [CocoaPods](https://cocoapods.org) über die `sharpie pod` Befehl.
Wenn die Bibliothek, der Sie interessiert sind als ein CocoaPod verfügbar ist, empfehlen wir, dass Sie zunächst die CocoaPod mit Ziel Sharpie (im Gegensatz zu versuchen, direkt für die Quelle die Bindung) zu binden.

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen einer Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen einer Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)