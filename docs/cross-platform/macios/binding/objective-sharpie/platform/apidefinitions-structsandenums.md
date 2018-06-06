---
title: ApiDefinitions & StructsAndEnums-Dateien
description: Dieses Dokument beschreibt die ApiDefinitions.cs und StructsAndEnums.cs-Dateien, die Ziel-Sharpie generiert. Diese Dateien werden dann zum Zugriff auf die Objective-C-Code in c# verwendet.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3b991f6105c6053f473b049d195aaef63cbcdd57
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780892"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums-Dateien

Beim Ziel Sharpie erfolgreich ausgeführt wurde, generiert er `Binding/ApiDefinitions.cs` und `Binding/StructsAndEnums.cs` Dateien.
Diese beiden Dateien bindungsprojekt hinzugefügt, eine in Visual Studio für Mac oder direkt zu übergeben sind die `btouch` oder `bmac` Tools, um die letzte Bindung zu erstellen.

In *einige* Fälle, die diese generierten Dateien möglicherweise müssen Sie jedoch weitere häufig der Entwickler diese manuell ändern müssen, generierte Dateien, um Probleme zu beheben, die nicht automatisch vom Tool (z. B. die gekennzeichneten verarbeitet werden konnte mit einem [ `Verify` Attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Zu den nächsten Schritten gehören:

- **Anpassen der Namen**: in einigen Fällen möchten die Namen von Methoden und Klassen, die .NET Framework-Entwurfsrichtlinien entsprechend anpassen.
- **Methoden oder Eigenschaften**: der Heuristik, die vom Ziel Sharpie verwendet wird, manchmal wählt eine Methode in einer Eigenschaft aktiviert werden. An diesem Punkt könnten Sie, ob dies das beabsichtigte Verhalten ist.
- **Einbinden von Ereignissen**: konnte und verknüpfen Sie die Klassen mit Ihren Delegatklassen Ereignisse für diese automatisch zu generieren.
- **Verknüpfen mit Benachrichtigungen**: Es ist nicht möglich, die API-Vertrag, der Benachrichtigungen aus den reinen Headerdateien zu extrahieren, die dies erfordert, dass ein Trip zur API-Dokumentation. Wenn Sie stark typisierte Benachrichtigungen werden sollen, müssen Sie das Ergebnis zu aktualisieren.
- **API-Kuratierung**: an dieser Stelle empfiehlt sich bieten zusätzliche Konstruktoren, Methoden (C#-Initialisierung auf Konstruktion Syntax zu ermöglichen), Überladen von Operatoren und implementieren Hinzufügen eigener Schnittstellen für die zusätzliche Definitionsdatei.

Finden Sie unter der [Binden einer API](~/cross-platform/macios/binding/objective-c-libraries.md) Beschreibung finden Sie unter wie diese Dateien in den Bindungsprozess passen, wie im folgenden Diagramm dargestellt:

![](apidefinitions-structsandenums-images/binding-flowchart.png "In diesem Diagramm wird der Bindungsprozess angezeigt.")

Finden Sie in der [binden Types Reference](~/cross-platform/macios/binding/binding-types-reference.md) für Weitere Informationen zu den Inhalt dieser Dateien.

