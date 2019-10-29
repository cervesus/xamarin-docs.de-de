---
title: Apidefinitions-& struclanandenums-Dateien
description: In diesem Dokument werden die von Target Sharpie generierten ApiDefinitions.cs-und StructsAndEnums.cs-Dateien beschrieben. Diese Dateien werden dann verwendet, um über auf den Ziel-C C#-Code zuzugreifen.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 870733554f3f69b7a0cd9b35c1b89e24c62b264d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016199"
---
# <a name="apidefinitions--structsandenums-files"></a>Apidefinitions-& struclanandenums-Dateien

Wenn der Ziel-Sharpie erfolgreich ausgeführt wurde, werden `Binding/ApiDefinitions.cs`-und `Binding/StructsAndEnums.cs` Dateien generiert.
Diese beiden Dateien werden einem Bindungs Projekt in Visual Studio für Mac hinzugefügt oder direkt an die `btouch` oder `bmac` Tools zur Erstellung der endgültigen Bindung übermittelt.

In *einigen* Fällen sind diese generierten Dateien möglicherweise nur erforderlich, aber häufig muss der Entwickler diese generierten Dateien manuell ändern, um Probleme zu beheben, die nicht automatisch vom Tool behandelt werden konnten (z. b. diejenigen, die mit einem`Verify` gekennzeichnet sind). [ -Attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Einige der nächsten Schritte umfassen Folgendes:

- **Anpassen von Namen**: in einigen Fällen möchten Sie die Namen von Methoden und Klassen so anpassen, dass Sie den .NET Framework Entwurfs Richtlinien entsprechen.
- **Methoden oder Eigenschaften**: die Heuristik, die von Ziel-Sharpie verwendet wird, wählt manchmal eine Methode aus, die in eine Eigenschaft umgewandelt werden soll. An diesem Punkt können Sie entscheiden, ob es sich um das beabsichtigte Verhalten handelt.
- **Hook-up-Ereignisse**: Sie können die Klassen mit ihren Delegatklassen verknüpfen und automatisch Ereignisse für diese generieren.
- Hosten von **Benachrichtigungen**: Es ist nicht möglich, den API-Vertrag von Benachrichtigungen aus den reinen Header Dateien zu extrahieren. Dies erfordert eine Fahrt zur API-Dokumentation. Wenn Sie Benachrichtigungen mit starker Typisierung wünschen, müssen Sie das Ergebnis aktualisieren.
- **API**-Zusammenstellung: an dieser Stelle können Sie zusätzliche Konstruktoren bereitstellen, Methoden hinzufügen (um die Syntax C# initialisieren-bei der Erstellung zuzulassen), Operator Überladung und eigene Schnittstellen in der Datei mit zusätzlichen Definitionen implementieren.

Sehen Sie sich die Beschreibung der [Bindung einer API](~/cross-platform/macios/binding/objective-c-libraries.md) an, um zu sehen, wie diese Dateien in den Bindungsprozess passen, wie in der folgenden Abbildung dargestellt:

![](apidefinitions-structsandenums-images/binding-flowchart.png "The binding process is shown in this diagram")

Weitere Informationen zum Inhalt dieser Dateien finden Sie in der [Bindungs Typen Referenz](~/cross-platform/macios/binding/binding-types-reference.md) .
