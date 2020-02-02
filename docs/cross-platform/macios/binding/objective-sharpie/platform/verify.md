---
title: Ziel-Sharpie-Verifizierungs Attribute
description: Dieses Dokument beschreibt das [verify]-Attribut, das von Target Sharpie generiert wurde. Mit dem [verify]-Attribut werden Entwickler hervorgehoben, wo die Ausgabe des Ziel sharlers manuell überprüft werden sollte.
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: d951952103a94dfc60a8083a75998611b635cda9
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940786"
---
# <a name="objective-sharpie-verify-attributes"></a>Ziel-Sharpie-Verifizierungs Attribute

Sie werden häufig feststellen, dass Bindungen, die von Ziel-Sharpie erzeugt werden, mit dem `[Verify]`-Attribut versehen werden. Diese Attribute geben an, dass Sie sicherstellen sollten, dass Ziel-Sharpie das richtige _ist, indem_ Sie die Bindung mit der ursprünglichen c/Ziel-c-Deklaration vergleichen (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt wird).

Die Überprüfung wird für _alle_ gebundenen Deklarationen empfohlen, ist aber wahrscheinlich für Deklarationen _erforderlich_ , die mit dem `[Verify]`-Attribut kommentiert werden. Dies liegt daran, dass in vielen Situationen nicht genügend Metadaten im ursprünglichen systemeigenen Quellcode vorhanden sind, um abzuleiten, wie eine Bindung am besten erzeugt wird. Möglicherweise müssen Sie in den Header Dateien auf Dokumentation oder Code Kommentare verweisen, um die beste Bindungs Entscheidung zu treffen.

Wenn Sie überprüft haben, ob die Bindung korrekt ist oder korrigiert wurde, _Entfernen_ Sie das `[Verify]`-Attribut aus der Bindung.

> [!IMPORTANT]
> `[Verify]` Attribute verursachen C# absichtlich Kompilierungsfehler, sodass Sie die Bindung überprüfen müssen. Entfernen Sie das `[Verify]`-Attribut, wenn Sie den Code überprüft (und möglicherweise korrigiert) haben.

## <a name="verify-hints-reference"></a>Referenz zu Überprüfungen

Das Argument Argument, das für das Attribut bereitgestellt wird, kann mit der folgenden Dokumentation quer referenziert werden. Die Dokumentation zu den erstellten `[Verify]` Attributen wird auch auf der Konsole bereitgestellt, nachdem die Bindung abgeschlossen wurde.

|`[Verify]` Hinweis|Beschreibung|
|---|---|
|InferredFromPreceedingTypedef|Der Name dieser Deklaration wurde durch eine gängige Konvention von der unmittelbar vorangehenden `typedef` im ursprünglichen systemeigenen Quellcode abgeleitet. Überprüfen Sie, ob der abherzuende Name korrekt ist, da diese Konvention mehrdeutig ist|
|ConstantsInterfaceAssociation|Es gibt keine Möglichkeit, zu bestimmen, mit welcher Ziel-C-Schnittstelle eine externe Variablen Deklaration verknüpft werden kann. Instanzen dieser werden als `[Field]` Eigenschaften in einer partiellen Schnittstelle an eine nahe konkrete Oberfläche gebunden, um eine intuitivere API zu erhalten, sodass die "Konstanten"-Schnittstelle möglicherweise vollständig ausgeschlossen wird.|
|Methodtoproperty|Eine Ziel-C-Methode wurde aufgrund der C# Konvention (z. b. ohne Rückgabe von Parametern und Rückgabe eines Werts) als Eigenschaft gebunden. Häufig werden Methoden wie diese als Eigenschaften an eine schönere API gebunden, aber manchmal können falsch positive Ergebnisse auftreten, und die Bindung sollte eigentlich eine Methode sein.|
|StronglyTypedNSArray|Eine systemeigene `NSArray*` wurde als `NSObject[]`gebunden. Möglicherweise ist es möglich, das Array in der Bindung basierend auf den durch die API-Dokumentation festgelegten Erwartungen (z. b. Kommentare in der Header Datei) oder durch Überprüfen der Array Inhalte durch Tests stärker einzugeben. Z. B. eine nsarray im*, enthält nur NSNumber* Instancescan als gebunden werden `NSNumber[]` anstelle von `NSObject[]`.|

Mit dem `sharpie verify-docs` Tool können Sie auch schnell eine Dokumentation für einen Hinweis erhalten, z. b.:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```
