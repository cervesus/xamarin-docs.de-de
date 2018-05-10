---
title: Überprüfen der Attribute
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: b9409b4351ed9233db0edf8e2dd9f516b9727fe0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="verify-attributes"></a>Überprüfen der Attribute


Sie werden häufig feststellen, dass es sich bei Bindungen, die vom Ziel Sharpie erzeugten mit Anmerkung versehen sein der `[Verify]` Attribut. Diese Attribute geben, Sie sollten _überprüfen_ , Ziel Sharpie die richtige Eingabe wurde durch Vergleichen der Bindung mit der ursprünglichen C#/Objective-C-Deklaration (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt werden).

Überprüfung wird empfohlen, für _alle_ Deklarationen gebunden, aber wahrscheinlich _erforderlichen_ für Deklarationen mit Anmerkung versehen der `[Verify]` Attribut. Dies ist in vielen Fällen gibt es ist nicht genügend Metadaten in den ursprünglichen systemeigene Quellcode, wie eine Bindung am besten erzeugt abzuleiten. Sie müssen möglicherweise verweisen Dokumentation oder in den Kommentaren im Code in den Headerdateien, um die beste Bindung Entscheidung zu treffen.

Nachdem Sie überprüft haben, dass die Bindung korrigieren oder wurden behoben, um korrekt ist, werden _entfernen_ der `[Verify]` Attribut von der Bindung.

> [!IMPORTANT]
> `[Verify]` -Attribute bewirken, dass absichtlich c# Kompilierungsfehler, damit Sie gezwungen sind, um zu überprüfen, ob die Bindung. Entfernen Sie die `[Verify]` Attribut, wenn Sie überprüft (und möglicherweise behoben) im Code.

## <a name="verify-hints-reference"></a>Überprüfen Sie die Hinweise-Referenz

Der Hinweis-Argument für das Attribut angegebenen kann, werden mit den unten stehenden Dokumentation verwiesen. Dokumentation für eine beliebige erzeugten `[Verify]` Attribute werden in der Konsole als auch angegeben werden, nachdem die Bindung abgeschlossen wurde.

|Überprüfen Sie Hinweis|Beschreibung|
|---|---|
|InferredFromPreceedingTypedef|Der Name dieser Deklaration durch allgemeine Konvention aus abgeleitet wurde die sofort jeweils vorgehenden `typedef` in den ursprünglichen systemeigene Quellcode einfügen. Stellen Sie sicher, dass der abgeleitete Name korrekt sind, da diese Konvention mehrdeutig ist.|
|ConstantsInterfaceAssociation|Es gibt keine Möglichkeit kinderleicht Nachweis, um zu bestimmen, welche Objective-C-Schnittstelle eine Variablendeklaration "extern" zugeordnet werden kann. Instanzen dieser gebunden sind, als `[Field]` Eigenschaften in einer teilweise-Schnittstelle in einer in der Nähe von "-" durch konkrete Oberfläche erzeugt eine intuitivere API, die möglicherweise eliminieren "Konstanten"-Schnittstelle vollständig.|
|MethodToProperty|Eine Objective-C-Methode wurde als eine C#-Eigenschaft wird aufgrund des Namenskonvention wie, die keine Parameter akzeptiert und Zurückgeben eines Werts (nicht "void" zurückgeben) gebunden. Häufig Methoden wie diese als Eigenschaften gebunden werden sollte, um eine nützlicher API-Oberfläche jedoch gelegentlich Fehlerquote auftreten können, und die Bindung muss sich tatsächlich auf eine Methode.|
|StronglyTypedNSArray|Ein systemeigenes `NSArray*` gebunden wurde, als `NSObject[]`. Es kann möglich sein, stärker Geben Sie das Array in der Bindung, die basierend auf den Erwartungen, die über die API-Dokumentation (z. B. Kommentare in der Headerdatei) festlegen oder durch Untersuchen der Arrayinhalt durch Tests. Angenommen, ein NSArray *, enthält nur NSNumber * Instancescan gebunden werden, als `NSNumber[]` anstelle von `NSObject[]`.|

Erhalten Sie die Dokumentation für einen Hinweis mit auch schnell die `sharpie verify-docs` tool, zum Beispiel:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

