---
title: XIb-Code Generierung in xamarin. IOS
description: In diesem Dokument C#wird beschrieben, wie xamarin. IOS Code generiert, um XIb-Dateien zuzuordnen. Dadurch können visuelle Steuerelemente Programm gesteuert zugänglich gemacht werden.
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f6218977e9ad0d4c396ef127c3c3ca53dc56d7d3
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78912563"
---
# <a name="xib-code-generation-in-xamarinios"></a>XIb-Code Generierung in xamarin. IOS

> [!IMPORTANT]
> In diesem Dokument wird die Integration von Visual Studio für Mac in xInterface Builder Code nur erläutert, da im Xamarin Designer für IOS keine Aktionen und Outlets verwendet werden. Weitere Informationen zum IOS-Designer finden Sie im IOS- [Designer](~/ios/user-interface/designer/index.md) -Dokument.

Das Apple Interface Builder Tool ("IB") kann zum visuellen Entwerfen von Benutzeroberflächen verwendet werden. Die von IB erstellten Schnittstellendefinitionen werden in **XIb** -Dateien gespeichert. Widgets und andere Objekte in **XIb** -Dateien können eine "klassenidentität" erhalten, bei der es sich um einen benutzerdefinierten Typ handeln kann. Auf diese Weise können Sie das Verhalten von Widgets anpassen und benutzerdefinierte Widgets schreiben.

Diese Benutzer Klassen sind normalerweise Unterklassen von UI-Controller Klassen. Sie verfügen über *Outlets* (analog zu Eigenschaften) und *Aktionen* (analog zu Ereignissen), die mit Schnittstellen Objekten verbunden werden können. Wenn die IB-Datei geladen wird, werden die Objekte zur Laufzeit erstellt, und die Outlets und Aktionen werden dynamisch mit den verschiedenen UI-Objekten verbunden. Wenn Sie diese verwalteten Klassen definieren, müssen Sie alle Aktionen und Outlets definieren, damit Sie mit den von IB erwarteten Aktionen identisch sind. In Visual Studio für Mac wird ein Code Behind-ähnliches Modell verwendet, um dies zu vereinfachen. Dies ähnelt der Vorgehensweise von Xcode für "Ziel-C", aber das Code Generierungs Modell und die Konventionen wurden optimiert, um .NET-Entwicklern besser vertraut zu sein.

Das Arbeiten mit **XIb** -Dateien wird in xamarin. IOS für Visual Studio derzeit nicht unterstützt.

## <a name="xib-files-and-custom-classes"></a>XIb-Dateien und benutzerdefinierte Klassen

Ebenso wie die Verwendung vorhandener Typen aus Cocoa berühren ist es möglich, benutzerdefinierte Typen in **XIb** -Dateien zu definieren. Es ist auch möglich, Typen zu verwenden, die in anderen **XIb** -Dateien definiert oder ausschließlich im C# Code definiert sind. Derzeit sind Interface Builder die Details von Typen, die außerhalb der aktuellen **XIb** -Datei definiert sind, nicht bekannt, sodass Sie nicht aufgelistet werden oder Ihre benutzerdefinierten Outlets und Aktionen nicht anzeigen. Das Entfernen dieser Einschränkung ist für einen späteren Zeitpunkt geplant.

Benutzerdefinierte Klassen können in einer **XIb** -Datei mithilfe des Befehls "Unterklasse hinzufügen" auf der Registerkarte "Klassen" der Interface Builder definiert werden. Diese werden als "Code Behind"-Klassen bezeichnet. Wenn die **XIb** -Datei im Projekt eine ". XIb.Designer.cs"-Entsprechung enthält, wird Sie von Visual Studio für Mac automatisch mit partiellen Klassendefinitionen für alle benutzerdefinierten Klassen in der **XIb**-Datei aufgefüllt. Diese partiellen Klassen werden als "Designer Klassen" bezeichnet.

## <a name="generating-code"></a>Generieren von Code

Wenn eine Datei mit dem Namen " **{0}. XIb.Designer.cs** " im Projekt vorhanden ist, generieren Visual Studio für Mac in der Designer-Datei für alle Benutzer Klassen, die in der XIb-Datei gefunden werden können, für alle Benutzer *Klassen, die*in der **XIb** -Datei enthalten sind, die Eigenschaften für die Outlets und partielle Methoden für alle Aktionen. **{0}** Die Code Generierung wird einfach durch das vorhanden sein dieser Datei ermöglicht.

Die Designer Datei wird automatisch aktualisiert, wenn sich die **XIb** -Datei ändert, und Visual Studio für Mac den Fokus erhält. Die Designer Datei sollte nicht manuell geändert werden, da Änderungen das nächste Mal überschrieben werden, Visual Studio für Mac die Datei aktualisiert wird.

## <a name="registration-and-namespaces"></a>Registrierung und Namespaces

Visual Studio für Mac generiert die Designer Klassen mithilfe des Standard Namespace des Projekts für den Datei Speicherort des Designers, um ihn mit dem normalen .net-Projekt Namespace konsistent zu machen. Der Namespace der Designer Dateien wird durch den "Standard Namespace" des Projekts und seine ".net Naming Policies"-Einstellungen gesteuert. Wenn sich der Standard Namespace des Projekts ändert, werden die Klassen im neuen Namespace von MD erneut generiert, sodass Sie möglicherweise feststellen, dass die partiellen Klassen nicht mehr zueinander passen.

Um die Klasse durch die Ziel-C-Laufzeit erkennbar zu machen, wendet Visual Studio für Mac ein `[Register (name)]` Attribut auf die Klasse an. Obwohl xamarin. IOS automatisch `NSObject`abgeleitete Klassen registriert, werden die voll qualifizierten .net-Namen verwendet. Das von Visual Studio für Mac angewendete-Attribut überschreibt dieses, um sicherzustellen, dass jede Klasse mit dem in der **XIb** -Datei verwendeten Namen registriert wird. Wenn Sie benutzerdefinierte Klassen in IB verwenden, ohne Visual Studio für Mac zum Generieren von Designer Dateien zu verwenden, müssen Sie dies möglicherweise manuell anwenden, damit die verwalteten Klassen den erwarteten Ziel-C-Klassennamen entsprechen.

Klassen können nicht in mehr als einer **. XIb**definiert werden, oder Sie verursachen einen Konflikt.

## <a name="non-designer-class-parts"></a>Nicht-Designer-Klassen Teile

Die partiellen Klassen des Designers sind nicht für die Verwendung ohne die Verwendung vorgesehen. Outlets sind privat, und es wird keine Basisklasse angegeben. Es wird erwartet, dass jede Klasse einen entsprechenden "Non-Designer"-Klassen Teil in einer anderen Datei hat, die die Basisklasse festlegt, die Outlets verwendet oder verfügbar macht, und definiert Konstruktoren, die zum Instanziieren der Klasse aus nativem Code beim Laden der **XIb**erforderlich sind. Dies wird in den Vorlagen für "Default **. XIb** " durchführen, aber für alle zusätzlichen benutzerdefinierten Klassen, die Sie in einer **XIb**definieren, müssen Sie den nicht-Designer-Teil manuell hinzufügen.

Der Grund hierfür ist die Notwendigkeit der Flexibilität. Beispielsweise können mehrere Code Behind-Klassen eine gemeinsame verwaltete abstrakte Klasse Unterklassen Unterklassen aufweisen, die die Klasse Unterklassen von IB Unterklassen unterordnen.

Es ist konventionell, dass diese Dateien in eine Datei mit **{0}. XIb.cs** neben der Designer Datei **{0}. XIb.Designer.cs** eingefügt werden.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Generierte Aktionen und Outlets

In den partiellen Designer Klassen generiert Visual Studio für Mac Eigenschaften, die den in IB definierten verbundenen Outlets entsprechen, sowie partielle Methoden, die allen verbundenen Aktionen entsprechen.

### <a name="outlet-properties"></a>Outlet-Eigenschaften

Designer Klassen enthalten Eigenschaften, die allen in der benutzerdefinierten Klasse definierten Outlets entsprechen. Die Tatsache, dass es sich hierbei um Eigenschaften handelt, ist ein Implementierungsdetail der xamarin. IOS-zu-Ziel-C-Bridge zum Aktivieren der verzögerten Bindung. Beachten Sie, dass Sie den privaten Feldern entsprechen, die nur von der Code Behind-Klasse verwendet werden sollen. Wenn Sie diese öffentlich machen möchten, fügen Sie dem nicht-Designer-Klassen Teil Accessoreigenschaften hinzu, wie es für jedes andere private Feld der Fall wäre.

Wenn die Outlet-Eigenschaften so definiert sind, dass Sie einen Typ von `id` aufweisen (äquivalent zu `NSObject`), bestimmt der Designer Code-Generator aktuell den höchstmöglichen Typ basierend auf Objekten, die mit diesem Outlet verbunden sind.
Dies wird jedoch in zukünftigen Versionen möglicherweise nicht unterstützt. Daher wird empfohlen, dass Sie die Outlets beim Definieren der benutzerdefinierten Klasse explizit eingeben.

### <a name="action-properties"></a>Aktions Eigenschaften

Designer Klassen enthalten partielle Methoden, die allen Aktionen entsprechen, die für die benutzerdefinierte Klasse definiert sind. Dabei handelt es sich um Methoden ohne Implementierung. Der Zweck der partiellen Methoden ist zweierlei:

1. Wenn Sie `partial` in den Klassen Text des nicht-Designer-Klassen Teils eingeben, bietet Visual Studio für Mac die automatische Vervollständigung der Signaturen aller nicht implementierten partiellen Methoden.
2. Die partiellen Methoden Signaturen verfügen über ein angewendetes Attribut, das Sie für die Ziel-C-Welt verfügbar macht, sodass Sie als die entsprechende Aktion behandelt werden können.

Wenn Sie möchten, können Sie die partielle-Methode ignorieren und die-Aktion implementieren, indem Sie das-Attribut auf eine andere Methode anwenden oder das Attribut auf eine Basisklasse durchlaufen lassen.

Wenn Aktionen so definiert sind, dass Sie den Absendertyp `id` (äquivalent zu `NSObject`) aufweisen, bestimmt der Designer Code Generator aktuell den höchstmöglichen Typ auf der Grundlage von Objekten, die mit dieser Aktion verbunden sind. Dies wird jedoch in zukünftigen Versionen möglicherweise nicht unterstützt. Daher empfiehlt es sich, die Aktionen beim Definieren der benutzerdefinierten Klasse explizit zu eingeben.

Beachten Sie, dass diese partiellen Methoden nur C#für erstellt werden, da CodeDom partielle Methoden nicht unterstützt, sodass Sie nicht für andere Sprachen generiert werden.

## <a name="cross-xib-class-usage"></a>Verwendung von Cross-XIb-Klassen

Manchmal möchten Benutzer aus mehreren **XIb** -Dateien, z. b. mit Registerkarten Controllern, auf dieselbe Klasse verweisen. Dies kann durch explizites verweisen auf die Klassendefinition aus einer anderen **XIb** -Datei oder durch das erneute definieren desselben Klassen namens in der zweiten **. XIb**-Datei erfolgen.

Der letztere Fall kann problematisch sein, da Visual Studio für Mac **XIb** -Dateien einzeln verarbeitet. Doppelte Definitionen können nicht automatisch erkannt und zusammengeführt werden. Daher können Konflikte auftreten, wenn das Register Attribut mehrmals angewendet wird, wenn dieselbe partielle Klasse in mehreren Designer Dateien definiert ist. Die neuesten Versionen von Visual Studio für Mac versuchen, dieses Problem zu beheben, aber es funktioniert möglicherweise nicht immer wie erwartet. In Zukunft wird dies wahrscheinlich nicht mehr unterstützt. stattdessen werden Visual Studio für Mac alle Typen, die in allen **XIb** -Dateien und verwaltetem Code im Projekt definiert sind, direkt aus allen **XIb** -Dateien sichtbar gemacht.

## <a name="type-resolution"></a>Typauflösung

Typen, die in IB verwendet werden, sind Ziel-C-Typnamen. Diese werden CLR-Typen durch die Verwendung von Register Attributen zugeordnet. Beim Erstellen von Ausgangs-und Aktions Code werden die entsprechenden CLR-Typen für alle vom xamarin. IOS-Kern umschließenden Ziel-C-Typen von Visual Studio für Mac aufgelöst und deren Typnamen vollständig qualifiziert.

Der Code-Generator kann jedoch zurzeit keine CLR-Typen von Ziel-C-Typnamen in Benutzercode oder Bibliotheken auflösen. in solchen Fällen wird daher der Typname wörtlich ausgegeben. Dies bedeutet, dass der entsprechende CLR-Typ den gleichen Namen wie der Ziel-C-Typ aufweisen muss und sich im selben Namespace wie der verwendete Code befinden muss. Dies ist so geplant, dass Sie irgendwann in der Zukunft korrigiert werden, indem Sie während der Codegenerierung alle Ziel-C-Typen im Projekt berücksichtigen.
