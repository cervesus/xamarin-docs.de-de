---
title: Binden von Mac-Bibliotheken für xamarin. Mac
description: Dieses Dokument enthält Links zu Anleitungen, in denen beschrieben wird, wie Sie mit Ziel-C-Bindungen in einer xamarin. Mac-Anwendung arbeiten, einschließlich Ziel-Sharpie und Beispielcode.
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 01/13/2017
ms.openlocfilehash: 59ac5a4f9949f1e65e67b9629c43ddb4b822bf43
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290050"
---
# <a name="binding-mac-libraries-for-xamarinmac"></a>Binden von Mac-Bibliotheken für xamarin. Mac

Befolgen Sie diese Links, um mehr über das Binden von Ziel-C-Bibliotheken auf xamarin. Mac zu erfahren:

- [**Übersicht**](~/cross-platform/macios/binding/overview.md) -
  Beschreibt, wie die Bindung funktioniert.
- [**Binden von Ziel-C-Bibliotheken**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Anweisungen zum Binden von Ziel-C-Bibliotheken für die Verwendung in xamarin-Projekten.
- [**Referenzhandbuch zur Typdefinition**](~/cross-platform/macios/binding/binding-types-reference.md) -
  Beschreibt alle verfügbaren Attribute für das Binden von Autoren, um den Bindungs Generierungsprozess zu steuern.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objektive Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Ziel-Sharpie ist ein Befehlszeilen Tool, mit dem der erste Durchlauf einer Bindung Bootstrap gestartet werden kann.
Dies funktioniert durch das Überprüfen der Header Dateien einer nativen Bibliothek, um die öffentliche API der [Bindungs Definition](~/cross-platform/macios/binding/binding-types-reference.md) zuzuordnen (ein Prozess, der andernfalls manuell ausgeführt wird). Der Ziel-Sharpie erstellt nicht allein eine Bindung, aber er kann Ihnen beim Einstieg helfen!

## <a name="examples"></a>Beispiele

Weitere Informationen zum Erstellen einer Mac-Bindung mithilfe von Bindungs Projekten finden Sie im [XMBindingExample Mac-Beispiel](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample) .

## <a name="related-links"></a>Verwandte Links

- [Binden von Objective-C](~/cross-platform/macios/binding/index.md)
- [Binden von IOS-Bibliotheken](~/ios/platform/binding-objective-c/index.md)
