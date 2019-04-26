---
title: XIB-Codegenerierung in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie Code zum XIB-Dateien zum Zuordnen von Xamarin.iOS generiert C#, wodurch visuelle Steuerelemente programmgesteuert zugegriffen werden kann.
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 57988374b4383f5659e29edff3834958b8f99f1b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035911"
---
# <a name="xib-code-generation-in-xamarinios"></a>XIB-Codegenerierung in Xamarin.iOS

> [!IMPORTANT]
>  Dieses Dokument erläutert die Visual Studio für Mac-Integration mit nur Interface Builder von Xcode wie Aktionen und Ergebnisdaten nicht in der Xamarin-Designer für iOS verwendet werden. Weitere Informationen zu den iOS-Designer, finden Sie in der [iOS-Designer](~/ios/user-interface/designer/index.md) Dokument.

Das Apple Interface Builder-Tool ("IB") kann verwendet werden, um Benutzeroberflächen visuell zu entwerfen. In die Schnittstellendefinitionen IB erstellt gespeichert **XIB** Dateien. Widgets und anderen Objekten im **XIB** Dateien können ein "-Klasse Identity", die möglicherweise einen benutzerdefinierten Typ für eine benutzerdefinierte zugewiesen werden. Dadurch können Sie zum Anpassen des Verhaltens von Widgets und benutzerdefinierte Widgets zu schreiben.

Diese Benutzerklassen ist normalerweise eine Unterklasse der UI-Controller-Klassen. Sie haben *Outlets* (analog zu den Eigenschaften) und *Aktionen* (analog zu den Ereignissen), können Objekte der Benutzeroberfläche verbunden werden. Zur Laufzeit Wenn die IB-Datei geladen wird, die Objekte werden erstellt, und die Outlets und Aktionen dynamisch mit den verschiedenen UI-Objekte verbunden sind. Wenn Sie diese verwalteten Klassen zu definieren, müssen Sie definieren alle Aktionen und Ergebnisdaten, die mit den Werten übereinstimmen, die IB erwartet. Visual Studio für Mac verwendet eine CodeBehind-Modell, um dies zu vereinfachen. Dies ist vergleichbar mit der Funktionsweise von Xcode für Objective-C, aber die Generation Codemodell und Konventionen für .NET-Entwickler vertrauter werden optimiert wurde.

Arbeiten mit **XIB** Dateien wird derzeit nicht unterstützt in Xamarin.iOS für Visual Studio.

## <a name="xib-files-and-custom-classes"></a>XIB-Dateien und benutzerdefinierte Klassen

Sowie die Verwendung von vorhandener Typen von Cocoa Touch, es ist möglich, Definieren von benutzerdefinierten Typen in **XIB** Dateien. Es ist auch möglich, Typen zu verwenden, die in anderen definiert sind **XIB** Dateien oder ausschließlich in definierten C# Code. Interface Builder ist derzeit nicht über die Details der Typen, die außerhalb des aktuellen definiert **XIB** Datei, sodass es nicht auflisten oder ihrer benutzerdefinierten Outlets und Aktionen anzeigen. Entfernen diese Einschränkung ist für einige Zeit in der Zukunft geplant.

Benutzerdefinierte Klassen definiert werden können, einem **XIB** Datei mithilfe des Befehls "Unterklasse hinzufügen" auf der Registerkarte "Klassen" Interface Builder. Wir bezeichnen diese als "CodeBehind"-Klassen. Wenn die **XIB** Datei hat eine ". xib.designer.cs" welcher Datei im Projekt, klicken Sie dann Visual Studio für Mac füllt er automatisch mit partiellen Klassendefinitionen für alle benutzerdefinierten Klassen in der **XIB**. Wir bezeichnen diese partiellen Klassen als "Designer Classes".

## <a name="generating-code"></a>Generieren von Code

Für alle  **{0}XIB** -Datei mit einer Buildaktion von *Seite*, wenn eine  **{0}. xib.designer.cs** Datei auch im Visual Studio für Mac-Projekt vorhanden ist Generieren von partiellen Klassen werden in der Designerdatei für alle Benutzerklassen sie in finden der **XIB** Datei mit Eigenschaften für die Outlets und partielle Methoden für alle Aktionen. Generierung von Code ist einfach durch das Vorhandensein dieser Datei aktiviert.

Die Designer-Datei wird automatisch aktualisiert, wenn die **XIB** Datei von Änderungen und Visual Studio für Mac den Fokus erhält wieder. Die Designer-Datei sollte nicht manuell geändert werden, da Änderungen auf die Datei überschrieben nächsten Visual Studio für Mac-Updates werden können.

## <a name="registration-and-namespaces"></a>Registrierung und Namespaces

Visual Studio für Mac generiert die Designer-Klassen mit Standardnamespace des Projekts, für den Designer-Datei an, um es mit normalen .NET Projekt Namespacing konsistent zu machen. Der Namespace des Designer-Dateien wird durch den Namespace des Projekts"Standard" und die zugehörigen Einstellungen "Die .NET-Benennung befolgen Richtlinien" gesteuert. Beachten Sie, dass wenn Standardnamespace des Projekts ändert, MD werden die Klassen neu in den neuen Namespace, generieren damit Sie finden können, dass Ihre partiellen Klassen nicht mehr übereinstimmen.

Um die Klasse von der Objective-C-Laufzeit erkennbar zu machen, Visual Studio für Mac gilt eine `[Register (name)]` -Attribut der Klasse. Obwohl Sie automatisch registriert, Xamarin.iOS `NSObject`-abgeleitete Klassen, er verwendet den vollqualifizierten Namen von .NET. Das Attribut angewendet, indem Sie Visual Studio für Mac diese, um sicherzustellen, dass jede Klasse überschrieben wird registriert, mit dem Namen verwendet werden, der **XIB** Datei. Wenn Sie benutzerdefinierte Klassen in IB verwenden, ohne Visual Studio für Mac-Designer-Dateien zu generieren, müssen Sie möglicherweise manuell, um Ihre verwalteten Klassen, die auf die erwartete Objective-C-Klassennamen übereinstimmen, anwenden.

Klassen können nicht definiert werden, in mehr als einer **XIB**, oder sie werden in Konflikt stehen.

## <a name="non-designer-class-parts"></a>Klassenteile von nicht-Designer

Designer partielle Klassen sollen nicht als verwendet werden – ist. Outlets sind privat, und keine Basisklasse angegeben ist. Es wird erwartet, dass jede Klasse eine entsprechende "nicht-Designer"-Klasse Teil in einer anderen Datei, die legt der Basisklasse der Klasse gewährt wird, verwendet oder macht die Outlets und Konstruktoren, die erforderlich sind, um die Klasse von nativem Code zu instanziieren, beim Laden der definiert**XIB**. Der Standardwert **XIB** Vorlagen hierzu allerdings für zusätzlichen benutzerdefinierte Klassen definieren Sie in einem **XIB**, müssen Sie das Webpart nicht-Designer manuell hinzufügen.

Der Grund dafür ist die Notwendigkeit der Flexibilität. Z. B. mehrere CodeBehind-Klassen Unterklasse, die eine allgemeine abstrakte Klasse, die Unterklassen die Klasse als Unterklasse definiert werden, indem IB verwaltet.

Regel werden diese fügen Sie in einem  **{0}. xib.cs** Datei neben den  **{0}. xib.designer.cs** Designer-Datei.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Generierte Aktionen und Ergebnisdaten

In den Designer partiellen Klassen generiert Visual Studio für Mac über Eigenschaften, die für alle verbundenen Outlets IB und partielle Methoden, die für alle verbundenen Aktionen definiert.

### <a name="outlet-properties"></a>Outlet-Eigenschaften

Designer-Klassen enthalten Eigenschaften, die alle Outlets für die benutzerdefinierte Klasse definiert. Die Tatsache, dass diese Eigenschaften sind ist ein Implementierungsdetail der xamarin.IOS Objective-C-Bridge, um eine verzögerte Bindung ermöglichen. Sie sollten diese äquivalent auf private Felder, die nur aus der CodeBehind-Klasse verwendet werden soll. Wenn Sie diese öffentlich machen möchten, fügen Sie Accessoreigenschaften mit dem Webpart nicht-Designer-Klasse, wie bei anderen privaten Feldern.

Wenn Outlet Eigenschaften definiert sind, damit ein `id` (Äquivalent zu `NSObject`) und dann der Designer-Code-Generator derzeit die stärksten möglich Typs auf Grundlage der Objekte an, an, der Einfachheit halber verbunden bestimmt.
Allerdings kann dies nicht in zukünftigen Versionen unterstützt werden daher wird empfohlen, dass Sie explizit stark der Outlets eingeben, wenn Sie die benutzerdefinierte Klasse zu definieren.

### <a name="action-properties"></a>Aktionseigenschaften

Designerklassen enthalten partielle Methoden, die für alle Aktionen, die für die benutzerdefinierte Klasse definiert. Hierbei handelt es sich um Methoden ohne Implementierung. Die partiellen Methoden wird zwei:

1.  Wenn Sie eingeben `partial` im Text des Parts nicht-Designer-Klasse, Visual Studio für Mac bietet automatisch vervollständigt die Signaturen aller nicht implementierte partielle Methoden.
2.  Die partielle Methodensignaturen haben ein Attribut angewendet, die sie in der Objective-C-Welt verfügbar macht, damit sie als die entsprechende Aktion verarbeitet abrufen können.


Wenn Sie möchten, können Sie ignorieren partielle Methode, und implementieren die Aktion durch Anwenden des Attributs auf eine andere Methode oder, damit sie sich auf eine Basisklasse fortfahren.

Wenn die Aktionen definiert werden, damit ein Sender `id` (entspricht `NSObject`), und klicken Sie dann die Designer-Code-Generator derzeit stärksten möglich basierend auf Objekten, die mit dieser Aktion verbunden Fensterart hängt es. Allerdings kann dies nicht in zukünftigen Versionen unterstützt werden daher wird empfohlen, dass Sie die Aktionen explizit stark eingeben, wenn Sie die benutzerdefinierte Klasse zu definieren.

Beachten Sie, die diese partiellen Methoden, nur für erstellt werden C#, da CodeDOM partielle Methoden, nicht unterstützt, sodass sie nicht für andere Sprachen generiert werden.

## <a name="cross-xib-class-usage"></a>Cross-XIB-Verwendung

In einigen Fällen möchten Benutzer die gleiche Klasse von mehreren verweisen **XIB** -Dateien, z. B. mit der Registerkarte "-Controller. Dies ist möglich durch Verweisen auf die Definition der Klasse von einem anderen explizit **XIB** aus, oder durch Definieren von denselben Klassennamen erneut in der zweiten **XIB**.

Letzteres kann problematisch sein, weil Visual Studio für Mac-Verarbeitung werden **XIB** Dateien einzeln. Es kann nicht automatisch erkennen und Zusammenführen von doppelten Definitionen, damit erhalten Sie möglicherweise Konflikte, die das Register-Attribut mehrmals anwenden, wenn die gleiche partielle Klasse in mehreren Designer-Dateien definiert ist. Neuere Versionen von Visual Studio für Mac zum Beheben dieses Problems versuchen, aber es kann immer funktioniert nicht wie erwartet. Dies wird in Zukunft wahrscheinlich nicht unterstützt werden, und stattdessen Visual Studio für Mac werden alle in allen definierte Typen **XIB** Dateien und verwalteten Code in das Projekt direkt sichtbar ist, von allen **XIB** Dateien.

## <a name="type-resolution"></a>Typauflösung

IB verwendete Typen sind Objective-C-Typnamen. Diese werden mithilfe von Attributen der Register-CLR-Typen zugeordnet. Beim Generieren von Code von outlets und Aktionen wird Visual Studio für Mac die entsprechenden CLR-Typen für alle Objective-C-Typen, die von der Xamarin.iOS-Kern umschlossen beheben, und ihre Namen vollständig qualifizieren.

Allerdings kann nicht vom Code-Generator derzeit aufgelöst werden CLR-Typen von Objective-C-Typnamen im Benutzercode oder Bibliotheken, und in solchen Fällen den wörtlichen Namen ausgegeben. Dies bedeutet, dass die entsprechende CLR-Typ muss den gleichen Namen wie der Objective-C-Typ und im selben Namespace wie der Code, der sie verwendet werden. Dies ist geplant, einige Zeit in der Zukunft behoben werden, indem geprüft wird, alle Objective-C-Typen in das Projekt während der codegenerierung.
