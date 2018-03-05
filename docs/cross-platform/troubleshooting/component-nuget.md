---
title: Aktualisieren von Komponentenverweisen zu NuGet
description: Ersetzen Sie die Komponente verweist mit NuGet-Pakete in Zukunft Ihrer apps.
ms.topic: article
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f3dbfb52d4fbcb4dd65f695a862f6b041d2b22c0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="updating-component-references-to-nuget"></a>Aktualisieren von Komponentenverweisen zu NuGet

_Ersetzen Sie die Komponente verweist mit NuGet-Pakete in Zukunft Ihrer apps._

Dieses Handbuch erläutert die Vorgehensweise beim Aktualisieren der vorhandener Xamarin-Projektmappen, um Komponentenverweise auf NuGet-Pakete zu ändern.

- [Komponenten, die NuGet-Pakete enthalten](#contain)
- [Komponenten mit NuGet-Ersetzungen](#replace)

Die meisten Komponenten werden in eine der oben genannten Kategorien fallen.
Bei Verwendung eine Komponente, die nicht angezeigt wird, ein Äquivalent NuGet-Paket haben, lesen Sie die [Komponenten ohne einen Migrationspfad NuGet](#require-update) Abschnitt weiter unten.

Auf diese Seiten finden Sie detaillierte Anweisungen zum Hinzufügen von NuGet-Pakete auf [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) oder [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Komponenten, die NuGet-Pakete enthalten

Viele Komponenten bereits NuGet-Pakete enthalten, und der Migrationspfad besteht darin, das Verweisen auf Komponenten zu löschen.

Sie können ermitteln, ob die Komponente bereits ein NuGet-Paket enthält, durch Doppelklicken auf die Komponente in der Projektmappe:

![Komponenten Knoten erweitert](component-nuget-images/solution-sml.png)

Die **Pakete** Registerkarte listet alle NuGet-Pakete, die in der Komponente enthalten:

![Registerkarte "Pakete" enthält NuGet](component-nuget-images/packages-tab-sml.png)

Beachten Sie, dass die **Assemblys** Registerkarte leer sein wird:

![Assemblyregisterkarte ist leer](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualisieren die Projektmappe

Um die Projektmappe zu aktualisieren, löschen Sie die **Komponente** Eintrag aus der Projektmappe:

![Löschen Sie die Komponente](component-nuget-images/delete-component-sml.png)

Das NuGet-Paket bleibt aufgelisteten in der **Pakete** Knoten und die app kompilieren, und wie gewohnt ausgeführt. In zukünftigen Updates für dieses Paket erfolgt über die **Nuget** Updatefunktion:

![Aktualisieren von NuGet-Paket](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Komponenten mit NuGet-Ersetzungen

Wenn die Seite "Komponente" **Assemblys** Registerkarte enthält Einträge, wie unten dargestellt, müssen Sie manuell das entsprechende NuGet-Paket zu suchen.

![Enthält von Assemblys](component-nuget-images/assemblies-tab-sml.png)

Beachten Sie, dass die **Pakete** Registerkarte wahrscheinlich leer sein:

![](component-nuget-images/packages-tab-empty-sml.png)

_Sie kann NuGet Abhängigkeiten enthalten, aber diese Fehler können ignoriert werden._


Um ein Ersatz NuGet bestätigen Paket vorhanden ist, suchen Sie auf [NuGet.org](https://www.nuget.org/packages), verwenden den Namen der Komponente, oder Sie können auch vom Autor.

Beispielsweise können Sie dem beliebten finden **Sqlite-Net-Pcl** Paket durch Suchen nach:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – der Name des Produkts.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – der Autor Profil.


### <a name="updating-the-solution"></a>Aktualisieren die Projektmappe

Nachdem Sie, dass die Komponente im NuGet verfügbar ist sichergestellt haben, gehen Sie folgendermaßen vor:

#### <a name="delete-the-component"></a>Löschen Sie die Komponente

Klicken Sie mit der rechten Maustaste auf die Komponente in der Projektmappe, und wählen Sie **entfernen**:

![Entfernen Sie die Komponente](component-nuget-images/remove-component-sml.png)

Hierdurch wird die Komponente und alle Verweise gelöscht. Dies wird den Build unterbrochen, bis Sie das entsprechende NuGet-Paket, um ihn zu ersetzen hinzufügen.

#### <a name="add-the-nuget-package"></a>Fügen Sie das NuGet-Paket hinzu

1. Mit der rechten Maustaste auf die **Pakete** Knoten, und wählen Sie **Pakete hinzufügen...** .
2. Suchen Sie nach der NuGet-Ersatz anhand des Namens oder Autor:

  ![](component-nuget-images/nuget-search-sml.png)

3. Drücken Sie **Paket hinzufügen**.

Das NuGet-Paket wird dem Projekt, und alle Abhängigkeiten hinzugefügt werden.
Den Build sollte korrigiert werden. Wenn der Build fehlschlägt weiterhin, untersuchen Sie jeden Fehler, um festzustellen, ob die API-Unterschiede zwischen der Komponente und das NuGet-Paket wurden.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Ohne einen Migrationspfad NuGet-Komponenten

Lassen Sie sich Gedanken machen, wenn Sie sofort Ersatz für Komponenten, die in der Anwendung verwendet, die nicht finden. Vorhandene Komponenten werden weiterhin in Visual Studio 15.5 arbeiten und die **Komponenten** Knoten erscheint in der Projektmappe wie gewohnt.

Zukünftige Versionen von Visual Studio, jedoch werden _nicht_ wiederherstellen oder Aktualisieren von Komponenten.
Dies bedeutet, wenn Sie die Projektmappe auf einem neuen Computer öffnen, die Komponente nicht heruntergeladen und installiert werden. und der Autor ist nicht in der Lage, um Sie mit Updates bereitzustellen. Sie sollten zu planen:

* Extrahieren Sie die Assemblys von der Komponente, und verweisen sie direkt in Ihrem Projekt.
* Wenden Sie sich an den Komponentenautor, und bitten Sie Pläne zu NuGet migrieren.
* Untersuchen von alternativen NuGet-Pakete, oder den Quellcode seek, wenn die Komponente Open Source-ist.

Viele Komponente Anbieter weiterhin zum Migrieren zu NuGet arbeiten, und anderen Benutzern (einschließlich erhältlicher Produkte) untersucht alternative Übermittlungsoptionen.


## <a name="related-links"></a>Verwandte Links

- [Installieren und Verwenden von NuGet-Paket (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Sowie ein NuGet-Paket (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
