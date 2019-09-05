---
title: Apidefinitions-& struclanandenums-Dateien
description: In diesem Dokument werden die von Target Sharpie generierten ApiDefinitions.cs-und StructsAndEnums.cs-Dateien beschrieben. Diese Dateien werden dann verwendet, um über auf den Ziel-C C#-Code zuzugreifen.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 950f9149744cb8aa2abaed60ccefb416405ab110
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290780"
---
# <a name="apidefinitions--structsandenums-files"></a>Apidefinitions-& struclanandenums-Dateien

Wenn der Ziel-Sharpie erfolgreich ausgeführt wurde, `Binding/ApiDefinitions.cs` werden `Binding/StructsAndEnums.cs` die-und-Dateien generiert.
Diese beiden Dateien werden einem Bindungs Projekt in Visual Studio für Mac hinzugefügt oder direkt an das- `btouch` Tool `bmac` oder das-Tool übermittelt, um die endgültige Bindung zu schaffen.

In *einigen* Fällen sind diese generierten Dateien möglicherweise nur erforderlich, aber häufig muss der Entwickler diese generierten Dateien manuell ändern, um Probleme zu beheben, die nicht automatisch vom Tool behandelt werden konnten (z. b. solche, die mit einem [ `Verify`-Attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Einige der nächsten Schritte umfassen Folgendes:

- **Anpassen von Namen**: Manchmal möchten Sie die Namen von Methoden und Klassen so anpassen, dass Sie den .NET Framework Entwurfs Richtlinien entsprechen.
- **Methoden oder Eigenschaften**: Die Heuristik, die von Ziel-Sharpie verwendet wird, wählt manchmal eine Methode aus, die in eine Eigenschaft umgewandelt werden soll. An diesem Punkt können Sie entscheiden, ob es sich um das beabsichtigte Verhalten handelt.
- **Hook-up-Ereignisse**: Sie können Ihre Klassen mit ihren Delegatklassen verknüpfen und automatisch Ereignisse für diese generieren.
- **Hook-Benachrichtigungen**: Es ist nicht möglich, den API-Vertrag von Benachrichtigungen aus den reinen Header Dateien zu extrahieren. Dies erfordert eine Fahrt zur API-Dokumentation. Wenn Sie Benachrichtigungen mit starker Typisierung wünschen, müssen Sie das Ergebnis aktualisieren.
- **API**-Zusammenstellung: An dieser Stelle können Sie zusätzliche Konstruktoren bereitstellen, Methoden hinzufügen (um die Syntax C# initialisieren-bei der Erstellung zuzulassen), Operator Überladung und eigene Schnittstellen in der Datei mit zusätzlichen Definitionen implementieren.

Sehen Sie sich die Beschreibung der [Bindung einer API](~/cross-platform/macios/binding/objective-c-libraries.md) an, um zu sehen, wie diese Dateien in den Bindungsprozess passen, wie in der folgenden Abbildung dargestellt:

![](apidefinitions-structsandenums-images/binding-flowchart.png "Der Bindungsprozess ist in diesem Diagramm dargestellt.")

Weitere Informationen zum Inhalt dieser Dateien finden Sie in der [Bindungs Typen Referenz](~/cross-platform/macios/binding/binding-types-reference.md) .

