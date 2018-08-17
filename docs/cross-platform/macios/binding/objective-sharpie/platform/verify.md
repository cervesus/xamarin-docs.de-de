---
title: Objektive Sharpie Überprüfen von Attributen
description: Dieses Dokument beschreibt die [Verify]-Attribut, das vom Ziel Sharpie generiert. Das Attribut [Verify] werden für Entwickler, in dem sie manuell die Ziel-Sharpie Ausgabe überprüft werden soll.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4bca896afb4dfc96fd6c1d7cdf489feb6a879e31
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855028"
---
# <a name="objective-sharpie-verify-attributes"></a>Objektive Sharpie Überprüfen von Attributen

Sie werden häufig feststellen, dass es sich bei Bindungen, die Ziel-Sharpie erzeugten mit Anmerkung versehen sein werden die `[Verify]` Attribut. Diese Attribute geben, Sie sollten _überprüfen_ , wurde die richtige Ziel Sharpie vergleichen die Bindung mit der ursprünglichen C/Objective-C-Deklaration (die in einem Kommentar über der gebundenen Deklaration bereitgestellt wird).

Überprüfung wird empfohlen, für die _alle_ Deklarationen gebunden, aber wahrscheinlich _erforderlichen_ für Deklarationen mit Anmerkung versehen der `[Verify]` Attribut. Dies ist, da in vielen Fällen nicht genügend Metadaten in den ursprünglichen Code von nativen Quelltypen, wie Sie eine Bindung am besten erzeugen abzuleiten. Möglicherweise müssen Sie sich auf Dokumentation oder in den Kommentaren im Code in den Headerdateien, um die beste Entscheidung für die Bindung zu treffen.

Nachdem Sie überprüft haben, dass die Bindung zu beheben oder wurden korrigiert, um richtig ist, werden _entfernen_ der `[Verify]` Attribut aus der Bindung.

> [!IMPORTANT]
> `[Verify]` -Attribute bewirken, dass absichtlich Kompilierungsfehler in c#, damit Sie gezwungen sind, um zu überprüfen, ob die Bindung. Entfernen Sie die `[Verify]` Attribut, wenn Sie überprüft (und möglicherweise korrigiert) den Code.

## <a name="verify-hints-reference"></a>Überprüfen Sie die Hinweise-Referenz

Das Hint-Argument, das für das Attribut angegebenen kann, werden mit der folgenden Dokumentation verwiesen. Dokumentation für alle erzeugt `[Verify]` Attribute werden in der Konsole auch angegeben werden, nachdem die Bindung abgeschlossen ist.

|`[Verify]` Hinweis|Beschreibung|
|---|---|
|InferredFromPreceedingTypedef|Der Name dieser Deklaration von üblicherweise aus abgeleitet wurde dem unmittelbar vorangehenden `typedef` in den ursprünglichen Quellcode für native. Stellen Sie sicher, dass der abgeleitete Name richtig ist, wie diese Konvention nicht eindeutig ist.|
|ConstantsInterfaceAssociation|Es gibt keine Möglichkeit narrensicher um zu bestimmen, welche Objective-C-Schnittstelle eine Variablendeklaration "extern" zugeordnet werden kann. Instanzen dieser gebunden sind, als `[Field]` Eigenschaften in eine partielle Schnittstelle in einer in der Nähe von konkreten Oberfläche erstellen Sie eine intuitivere-API, sodass möglicherweise die "Konstanten"-Schnittstelle vollständig.|
|MethodToProperty|Eine Objective-C-Methode wurde als C#-Eigenschaft aufgrund der Konvention, wie z. B. die keine Parameter akzeptiert und Zurückgeben eines Werts (nicht-Void-Rückgabe) gebunden. Häufig Methoden wie diese gebunden werden als Eigenschaften soll eine nützlicher-API-Oberfläche, aber gelegentlich zu falsch positiven Ergebnissen auftreten können, und die Bindung muss sich tatsächlich auf eine Methode.|
|StronglyTypedNSArray|Ein systemeigenes `NSArray*` gebunden wurde, als `NSObject[]`. Es kann möglich sein, stärker Geben Sie das Array in der Bindung, die basierend auf den Erwartungen, die über die API-Dokumentation (z. B. Kommentare in der Headerdatei) festlegen oder durch den Inhalt des Arrays durch Tests zu untersuchen. Z. B. eine nsarray im*, enthält nur NSNumber* Instancescan als gebunden werden `NSNumber[]` anstelle von `NSObject[]`.|

Sie können auch schnell erhalten, die Dokumentation für einen Hinweis mit der `sharpie verify-docs` Tools, z.B.:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen einer Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen einer Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
