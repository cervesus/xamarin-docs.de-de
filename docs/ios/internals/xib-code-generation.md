---
title: .XIB Codegenerierung
ms.topic: article
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c98d4100a758e624c851ed2294cfe0c6b7f16fdd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xib-code-generation"></a>.XIB Codegenerierung

> [!IMPORTANT]
>  Dieses Dokument erläutert die Visual Studio für Mac Integration in Xcodes Benutzeroberflächen-Generator nur, wie Aktionen und Ausgänge nicht in die Xamarin-Designer für iOS verwendet werden. Weitere Informationen zu den iOS-Designer, überprüfen Sie die [iOS Designer](~/ios/user-interface/designer/index.md) Dokument.

Das Apple-Schnittstelle-Generator-Tool ("IB") kann verwendet werden, um Benutzeroberflächen visuell zu entwerfen. In die Schnittstellendefinitionen erstellt, indem IB gespeichert werden **.xib** Dateien. Widgets und anderen Objekten im **.xib** Dateien erhalten möglicherweise eine "-Klasse Identität", die einen benutzerdefinierten Typ für eine benutzerdefinierte werden kann. Dadurch können Sie Sie das Verhalten von Widgets sowie zum Schreiben von benutzerdefinierten Widgets anpassen.

Diese Benutzerklassen werden normalerweise Unterklassen des UI-Controller-Klassen. Diese weisen *Steckdosen* (analog zu Eigenschaften) und *Aktionen* (analog zu Ereignisse), die auf Benutzeroberflächenobjekte verbunden werden können. Zur Laufzeit Wenn die IB-Datei geladen wird, die Objekte werden erstellt, und die Steckdosen und Aktionen dynamisch auf die verschiedenen UI-Objekte verbunden sind. Wenn Sie diese verwalteten Klassen definieren, müssen Sie definieren die Aktionen und Ausgänge, mit denen übereinstimmen, die IB erwartet. Visual Studio für Mac verwendet ein CodeBehind-Modell, um dies zu vereinfachen. Dies ähnelt der Funktionsweise von Xcode für Objective-C, aber die Generation Codemodell und Konventionen zum .NET Entwicklern vertrauter sein manipuliert wurde.

Arbeiten mit **.xib** Dateien wird derzeit nicht unterstützt in Xamarin.iOS for Visual Studio.

## <a name="xib-files-and-custom-classes"></a>.XIB Dateien und benutzerdefinierte Klassen

Sowie mithilfe von vorhandenen Typen aus Kakao berühren, es ist möglich, benutzerdefinierte Typen in definieren **.xib** Dateien. Es ist auch möglich, Typen zu verwenden, die in anderen definiert sind **.xib** Dateien oder ausschließlich in C#-Code definiert. Interface-Generator ist derzeit nicht beachten Sie die Details der Typen definiert, die außerhalb des aktuellen **.xib** Datei, damit nicht auflisten oder ihre benutzerdefinierten Steckdosen und Aktionen anzeigen. Entfernen diese Einschränkung ist für einige Zeit in der Zukunft geplant.

Benutzerdefinierte Klassen definiert werden können, einem **.xib** -Datendatei mithilfe des Befehls "Unterklasse hinzufügen" auf der Registerkarte "Klassen" Schnittstelle-Generator. Wir bezeichnen diese als "CodeBehind"-Klassen. Wenn die **.xib** Datei hat eine ". xib.designer.cs" Gegenstück-Datei im Projekt, klicken Sie dann Visual Studio für Mac füllt er automatisch mit partiellen Klassendefinitionen für benutzerdefinierten Klassen in der **.xib**. Wir bezeichnen diese partiellen Klassen als "Designerklassen".

## <a name="generating-code"></a>Generieren von Code

Für eine beliebige **{0} .xib** -Datei mit dem Buildvorgang *Seite*, wenn eine **{0}.xib.designer.cs** Datei auch im Projekt vorhanden ist, generiert Visual Studio für Mac partielle Klassen in der Designer-Datei für alle Benutzerklassen sie in finden der **.xib** -Datei mithilfe von Eigenschaften für die Steckdosen und partielle Methoden für alle Aktionen. Codegenerierung wird einfach durch das Vorhandensein dieser Datei aktiviert.

Die Designer-Datei wird automatisch aktualisiert, wenn die **.xib** -Konfigurationsdatei geändert und Visual Studio für Mac erhält den Fokus wieder. Die Designer-Datei sollte nicht manuell geändert werden, wie Änderungen überschrieben nächsten Visual Studio für Mac-Updates der Datei werden.

## <a name="registration-and-namespaces"></a>Registrierung und Namespaces

Designerklassen, die mithilfe von Standard-Namespace das Projekt für den Designer-Datei an, um es mit normaler .NET Projekt Namespacing konsistent zu machen, generiert Visual Studio für Mac. Der Namespace des Designer-Dateien wird vom das Projekt "Standardnamespace" und die Einstellungen ".NET naming Policies" gesteuert. Beachten Sie, dass wenn Standardnamespace des Projekts ändert, MD erneut die Klassen in dem neuen Namespace, generieren wird um zu ermitteln, möglicherweise, dass die partiellen Klassen nicht mehr überein.

Um die Klasse von der Objective-C-Laufzeit erkennbar zu machen, Visual Studio für Mac gilt eine `[Register (name)]` -Attribut der Klasse. Obwohl Xamarin.iOS automatisch registriert `NSObject`-abgeleitete Klassen, die .NET voll qualifizierten Namen verwendet. Das Attribut angewendet, indem Sie Visual Studio für Mac dadurch um sicherzustellen, dass jede Klasse überschrieben wird registriert, mit dem Namen verwendet, der **.xib** Datei. Wenn Sie benutzerdefinierte Klassen in IB verwenden, ohne mithilfe von Visual Studio für Mac-Designer-Dateien zu generieren, müssen Sie möglicherweise manuell, um Ihre verwalteten Klassen, die die erwartete Objective-C-Klassennamen entsprechen stellen anwenden.

Klassen können nicht definiert werden, in mehrere **.xib**, oder sie können in Konflikt stehen.

## <a name="non-designer-class-parts"></a>Teile der nicht-Designer-Klasse

Partielle Designerklassen dürfen nicht als verwendet werden soll-ist. Steckdosen privat sind, und keine Basisklasse angegeben ist. Es wird erwartet, dass jede Klasse einen entsprechende "nicht-Designer"-Klasse Teil in einer anderen Datei hat die legt der Basisklasse, verwendet oder die Steckdosen verfügbar macht, und definiert die Konstruktoren, die zum Instanziieren der Klasse von systemeigenem Code, beim Laden der erforderlichsind**.xib**. Die Standardeinstellung **.xib** Vorlagen hierzu, jedoch für jede weiteren benutzerdefinierten Klassen definieren einer **.xib**, müssen Sie das Webpart nicht-Designer manuell hinzufügen.

Der Grund hierfür ist die Notwendigkeit der Flexibilität. Beispielsweise könnte mehrere Klassen von CodeBehind Unterklasse, die eine gemeinsame abstrakte Klasse, die Unterklassen von IB als Unterklasse werden die Klasse verwaltet.

Herkömmliche diese in den versetzt ist eine **{0}.xib.cs** neben der Datei die **{0}.xib.designer.cs** Designer-Datei.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Generierte Aktionen und Ausgänge

In der partiellen Klassen-Designer generiert Visual Studio für Mac für alle verbundenen Steckdosen in IB und partielle Methoden, die für alle verbundenen Aktionen definierten Eigenschaften.

### <a name="outlet-properties"></a>Eigenschaften

Designerklassen enthalten Eigenschaften, die alle Steckdosen für die benutzerdefinierte Klasse definiert. Die Tatsache, dass diese Eigenschaften sind ist ein Implementierungsdetail, der die Xamarin.iOS Objective C-Brücke, um eine verzögerte Bindung ermöglichen. Sie sollten sie entsprechende private Felder, die nur aus der CodeBehind-Klasse verwendet werden soll. Wenn Sie diese öffentliches Profil festzulegen möchten, fügen Sie Accessoreigenschaften wie bei anderen privaten Feldern mit dem Webpart nicht-Designer-Klasse.

Eigenschaften definiert sind, haben Sie einen Typ von **Id** (Äquivalent zu `NSObject`) anschließend die Designer-Code-Generators derzeit die stärksten möglichen Typen basierend auf Objekten, die mit dieser Steckdose, der Einfachheit halber verbunden bestimmt.
Allerdings kann dies nicht in zukünftigen Versionen unterstützt werden daher wird empfohlen, dass Sie die Steckdosen explizit stark eingeben, wenn Sie die benutzerdefinierte Klasse definieren.

### <a name="action-properties"></a>Aktionseigenschaften

Designerklassen enthalten partielle Methoden, die für alle Aktionen, die für die benutzerdefinierte Klasse definiert. Hierbei handelt es sich um Methoden ohne Implementierung. Die partiellen Methoden dient einem doppelten Zweck:

1.  Wenn Sie eingeben `partial` im Hauptteil des nicht-Designer-Klasse Teils Klasse Visual Studio für Mac wird es anbieten, AutoVervollständigen die Signaturen aller partielle Methoden in einer nicht implementiert.
1.  Die partielle Methodensignaturen haben ein Attribut angewendet, die sie auf der Welt Objective-C verfügbar macht, damit sie auf die entsprechende Aktion verarbeitet abrufen können.


Wenn Sie möchten, können Sie ignorieren die partielle Methode und implementieren die Aktion, durch Anwendung des Attributs auf eine andere Methode, oder lassen Sie ihn auf eine Basisklasse zu fortfahren.

Wenn Aktionen definiert werden, um einen Sender verfügen `id` (entspricht `NSObject`), anschließend die Designer-Code-Generators derzeit die stärksten möglichen Typen basierend auf Objekten, die mit dieser Aktion verbunden bestimmt. Allerdings kann dies nicht in zukünftigen Versionen unterstützt werden daher wird empfohlen, dass Sie die Aktionen explizit stark eingeben, wenn Sie die benutzerdefinierte Klasse definieren.

Beachten Sie, dass diese partiellen Methoden nur für c# erstellt werden, da CodeDOM partielle Methoden nicht unterstützt, sodass sie nicht für andere Sprachen generiert werden.

## <a name="cross-xib-class-usage"></a>Verwendung von übergreifenden XIB-Klasse

In einigen Fällen möchten Benutzer die gleiche Klasse von mehreren verweisen **.xib** Dateien, z. B. mit der Registerkarte "-Controller. Dies kann geschehen, indem Memberdirektive verweisen auf die Definition der Klasse aus einer anderen **.xib** -Datei ein, oder definieren Sie denselben Klassennamen durch erneut in der zweiten **.xib**.

Der letztere Fall kann problematisch sein. Dies ist auf Visual Studio für Mac-Verarbeitung **.xib** Dateien einzeln. Zudem kann nicht automatisch erkannt und Zusammenführen von doppelten Definitionen, damit Sie zum Schluss möglicherweise Konflikte, die das Register Attribut mehrmals anwenden, wenn die gleiche partielle Klasse in mehreren Designer-Dateien definiert ist. Neuere Versionen von Visual Studio für Mac versucht wird, um dieses Problem zu beheben, aber dies funktioniert möglicherweise nicht immer wie erwartet. In Zukunft ist wahrscheinlich nicht unterstützt wird, und stattdessen Visual Studio für Mac werden alle Typen in allen definierten **.xib** Dateien und verwaltetem Code im Projekt direkt sichtbar aus allen **.xib** Dateien.

## <a name="type-resolution"></a>Typauflösung

IB verwendete Typen sind Objective-C-Typnamen. Diese werden zugeordnet, um CLR-Typen jedoch die Verwendung von Attributen registrieren. Beim Ausgang und Aktion Code zu generieren, wird die Visual Studio für Mac beheben die entsprechenden CLR-Typen für alle Objective-C-Typen, die von der Kern Xamarin.iOS umschlossen, und ihren Typnamen vollständig qualifizieren.

Der Code-Generator allerdings kann nicht derzeit aufgelöst CLR-Typen von Objective-C-Typnamen in Benutzercode oder Bibliotheken, damit in solchen Fällen gibt sie konsistent den Typnamen wörtliche. Dies bedeutet, dass die entsprechende CLR-Typ muss den gleichen Namen wie der Objective-C-Typ und werden im selben Namespace wie der Code, der verwendet wird. Dies ist geplant, berücksichtigen alle Objective-C-Typen im Projekt während der codegenerierung einige Zeit in der Zukunft behoben werden.
