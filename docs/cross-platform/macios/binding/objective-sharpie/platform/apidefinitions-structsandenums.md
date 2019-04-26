---
title: ApiDefinitions und StructsAndEnums-Dateien
description: Dieses Dokument beschreibt die ApiDefinitions.cs und StructsAndEnums.cs-Dateien, die Ziel-Sharpie generiert. Diese Dateien werden dann verwendet, den Objective-C-Code aus den Zugriff auf C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266368"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions und StructsAndEnums-Dateien

Wenn das Ziel Sharpie erfolgreich ausgeführt wurde, generiert er `Binding/ApiDefinitions.cs` und `Binding/StructsAndEnums.cs` Dateien.
Diese zwei Dateien hinzugefügt, einer bindungsprojekt in Visual Studio für Mac oder direkt zu übergeben die `btouch` oder `bmac` Tools, um die letzte Bindung zu erstellen.

In *einige* Fällen diese generierten Dateien möglicherweise müssen Sie jedoch weitere häufig der Entwickler diese manuell bearbeiten muss, generierte Dateien, um Probleme zu beheben, die vom Tool (z. B. die gekennzeichneten nicht automatisch verarbeitet werden konnte mit einem [ `Verify` Attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Zu den nächsten Schritten gehören:

- **Anpassen der Namen**: Manchmal möchten Sie die Namen von Methoden und Klassen, die den .NET Framework-Entwurfsrichtlinien entsprechend anpassen.
- **Methoden oder Eigenschaften**: Heuristik, die manchmal verwendet vom Ziel Sharpie erkennt eine Methode in eine Eigenschaft aktiviert werden. An diesem Punkt können Sie entscheiden, ob dies das beabsichtigte Verhalten oder nicht ist.
- **Einbinden mit Ereignissen**: Könnten Sie verknüpfen die Klassen mit Ihrem-Delegatklasse und Ereignisse für diese automatisch zu generieren.
- **Einbinden von Benachrichtigungen**: Es ist nicht möglich, um die API-Vertrag, der Benachrichtigungen aus den reinen Headerdateien zu extrahieren, dies erfordert, dass eine Reise nach der API-Dokumentation. Wenn Sie stark typisierte Benachrichtigungen werden sollen, müssen Sie das Ergebnis zu aktualisieren.
- **API-Zusammenstellung**: An diesem Punkt empfiehlt sich bieten zusätzliche Konstruktoren, Methoden hinzufügen (ermöglichen C# Initialize-auf-Konstruktionssyntax), Operator überladen, und Implementieren eigener Schnittstellen für die zusätzliche Definitionen-Datei.

Finden Sie unter den [binden eine API](~/cross-platform/macios/binding/objective-c-libraries.md) Beschreibung sehen, wie diese Dateien in des Bindungsprozesses, wie im folgenden Diagramm dargestellt:

![](apidefinitions-structsandenums-images/binding-flowchart.png "In diesem Diagramm wird der Bindungsprozess angezeigt.")

Finden Sie in der [Bindung Datentypreferenz](~/cross-platform/macios/binding/binding-types-reference.md) für Weitere Informationen zum Inhalt dieser Dateien.

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen eine Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen Sie eine Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
