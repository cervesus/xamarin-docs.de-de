---
title: Apidefinitions-& struclanandenums-Dateien
description: In diesem Dokument werden die von Target Sharpie generierten ApiDefinitions.cs-und StructsAndEnums.cs-Dateien beschrieben. Diese Dateien werden dann für den Zugriff auf den Ziel-C-Code aus c# verwendet.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 8d4a05745d8d2ec6e05abd519aef4b9827655e06
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930428"
---
# <a name="apidefinitions--structsandenums-files"></a>Apidefinitions-& struclanandenums-Dateien

Wenn der Ziel-Sharpie erfolgreich ausgeführt wurde, werden die `Binding/ApiDefinitions.cs` -und- `Binding/StructsAndEnums.cs` Dateien generiert.
Diese beiden Dateien werden einem Bindungs Projekt in Visual Studio für Mac hinzugefügt oder direkt an das- `btouch` Tool oder das- `bmac` Tool übermittelt, um die endgültige Bindung zu schaffen.

In *einigen* Fällen sind diese generierten Dateien möglicherweise nur erforderlich, aber häufig muss der Entwickler diese generierten Dateien manuell ändern, um Probleme zu beheben, die nicht automatisch vom Tool behandelt werden konnten (z. b. solche, die mit einem- [ `Verify` Attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)gekennzeichnet sind).

Einige der nächsten Schritte umfassen Folgendes:

- **Anpassen von Namen**: in einigen Fällen möchten Sie die Namen von Methoden und Klassen so anpassen, dass Sie den .NET Framework Entwurfs Richtlinien entsprechen.
- **Methoden oder Eigenschaften**: die Heuristik, die von Ziel-Sharpie verwendet wird, wählt manchmal eine Methode aus, die in eine Eigenschaft umgewandelt werden soll. An diesem Punkt können Sie entscheiden, ob es sich um das beabsichtigte Verhalten handelt.
- **Hook-up-Ereignisse**: Sie können die Klassen mit ihren Delegatklassen verknüpfen und automatisch Ereignisse für diese generieren.
- Hosten von **Benachrichtigungen**: Es ist nicht möglich, den API-Vertrag von Benachrichtigungen aus den reinen Header Dateien zu extrahieren. Dies erfordert eine Fahrt zur API-Dokumentation. Wenn Sie Benachrichtigungen mit starker Typisierung wünschen, müssen Sie das Ergebnis aktualisieren.
- **API**-Zusammenstellung: an dieser Stelle können Sie zusätzliche Konstruktoren bereitstellen, Methoden hinzufügen (um die Syntax von c# Initialize-on-Construction zuzulassen), Operator Überladung und eigene Schnittstellen in der Datei mit zusätzlichen Definitionen implementieren.

Sehen Sie sich die Beschreibung der [Bindung einer API](~/cross-platform/macios/binding/objective-c-libraries.md) an, um zu sehen, wie diese Dateien in den Bindungsprozess passen, wie in der folgenden Abbildung dargestellt:

![Der Bindungsprozess ist in diesem Diagramm dargestellt.](apidefinitions-structsandenums-images/binding-flowchart.png)

Weitere Informationen zum Inhalt dieser Dateien finden Sie in der [Bindungs Typen Referenz](~/cross-platform/macios/binding/binding-types-reference.md) .
