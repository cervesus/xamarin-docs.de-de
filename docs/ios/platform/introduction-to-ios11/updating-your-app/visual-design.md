---
title: Visuelle Design-Updates
description: Untersuchen die neuen Funktionen von iOS 11
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2904a7da73f5bf6e8960f65239d1f8dc52ab1aba
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="visual-design-updates"></a>Visuelle Design-Updates

_Untersuchen die neuen Funktionen von iOS 11_

Apple iOS 11 eingeführt neue visuelle Änderungen, einschließlich Updates auf der Navigationsleiste in der Suchleiste und Tabellensichten. Darüber hinaus wurden Verbesserungen vorgenommen, um mehr Flexibilität über Ränder und Vollbild-Inhalt zu ermöglichen. Diese Änderungen werden in diesem Handbuch behandelt.

## <a name="uikit"></a>UIKit

UIKit Balken haben in iOS 11, um sie Endbenutzern leichter zugänglich machen angepasst wurde.

Eine solche Änderung wird eine neue HUD angezeigt, die angezeigt wird, wenn ein Benutzer lang – auf einen Balken drückt Element. Um dies zu ermöglichen, legen Sie die `largeContentSizeImage` Eigenschaft `UIBarItem` und hinzufügen ein größeres Bilds über eine [Asset-Katalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Navigationsleiste
iOS 11 eingeführt, neue Funktionen zur Navigation Leiste Titel leichter lesbar zu machen. Apps können größere Titel anzeigen, indem Zuweisen der `PrefersLargeTitles` Eigenschaft auf "true":

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Einstellung größere Titel in Ihrer app macht _alle_ Navigation Balken Titel über Ihre app größer angezeigt werden, wie im folgenden Screenshot gezeigt:

![Große Navigationstitel](visual-design-images/image7.png)

Um zu steuern, wenn ein großer Titel auf der Navigationsleiste angezeigt wird, legen Sie die `LargeTitleDisplayMode` auf das Navigationselement `Always`, `Never`, oder `Automatic`.

### <a name="search-controller"></a>Search-Controller

iOS 11 machte es einfacher, einen Controller Suche direkt auf der Navigationsleiste hinzuzufügen. Fügen Sie es nach der Erstellung eines Controllers Suche zur Navigationsleiste mit der `SearchController` Eigenschaft:

```csharp
NavigationItem.SearchController = searchController;
```

[![Große Navigationstitel mit Suchleiste](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png)

Abhängig von den Funktionen der app müssen Sie möglicherweise oder sollten sich nicht auf die Suchleiste ausblenden, wenn ein Benutzer eine Liste einen Bildlauf. Sie können anpassen, dies mithilfe der `HidesSearchBarWhenScrolling` Eigenschaft.

## <a name="margins"></a>Seitenränder

Apple hat eine neue Eigenschaft – `directionalLayoutMargins` –, die verwendet werden kann, um den Abstand zwischen Ansichten und Unteransichten festzulegen. Verwendung `directionalLayoutMargins` mit `leading` oder `trailing` Abstände. Unabhängig davon, ob das System keine Links-nach-rechts oder rechts-nach-links-Sprache ist wird der Abstand in Ihrer app von iOS entsprechend festlegen.

In iOS 10 und vor dem hatten alle Ansichten einer minimale Randgröße auf die Ausrichtung würden. iOS 11 eingeführt, die Option zum Überschreiben, dass die Verwendung `ViewRespectsSystemMinimumLayoutMargins`. Beispielsweise ermöglicht das Festlegen dieser Eigenschaft auf "false" Sie Ihre Edge Abstände auf 0 (null) anpassen:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Bild mit Rand mit 0 (null) Inset in Ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Vollbild-Inhalt

iOS 7 [eingeführt](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` und `bottomLayoutGuide` als eine Möglichkeit zum Einschränken von Ansichten, damit sie nicht durch UIKit Balken ausgeblendet und befinden sich in einer sichtbaren Bereich des Bildschirms. Diese sind in iOS 11 zugunsten des veraltet die _abgesicherten Bereich_.

Der sichere Bereich ist eine neue Art der darum geht, sichtbare Bereich von der Anwendung und Einschränkungen zwischen einer Ansicht und eine übergeordnete Ansicht hinzugefügt werden. Betrachten Sie beispielsweise die folgende Abbildung aus:

[![Abgesicherten Bereich Vs oberen und unteren Layout-Handbuch](visual-design-images/image10-sml.png)](visual-design-images/image10.png)

Vorher, wenn Sie hatten eine Ansicht hinzugefügt und auf den grünen Bereich oberhalb sichtbar sein sollte, Sie würden schränken Sie ihn auf die _unten_ von der `TopLayoutGuide` und die _oben_ von der `BottomLayoutGuide`. In iOS 11, Sie möchten stattdessen schränken Sie ihn auf die _oben_ und _unteren_ des sicheren Bereichs. Beispiel:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Tabellenansicht

Die UITableView gab es eine Reihe von kleinen, aber wichtig, Änderungen in iOS 11.

Standardmäßig werden Kopfzeilen, Fußzeilen und Zellen jetzt automatisch angepasst, basierend auf deren Inhalt. Um dieser Gruppe für die automatische größenanpassung Verhalten abzuwählen der `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, oder `EstimatedSectionFooterHeight` auf 0 (null).

Jedoch in einigen Fällen (z. B. beim Hinzufügen von UITableViewController in der iOS-Designer oder bei Verwendung von vorhandenen Storboards in Benutzeroberflächen-Generator) müssen Sie möglicherweise manuell aktivieren von Self-sizing Zellen. Zu diesem Zweck stellen Sie sicher, dass Sie die folgenden Eigenschaften für die Tabellenansicht für Zellen, Kopf- und Fußzeilen, bzw. festgelegt haben:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 hat die Funktionalität der Zeilenaktionen erweitert. `UISwipeActionsConfiguration` wurde eingeführt, um eine Reihe von Aktionen zu definieren, die ausgeführt werden soll, wenn der Benutzer in beide Richtungen für eine Zeile in einer Tabellenansicht Kundenkarte. Dieses Verhalten ähnelt, die systemeigene Mail.app. Weitere Informationen finden Sie unter der [Zeilenaktionen](~/ios/user-interface/controls/tables/row-action.md) Handbuch.

Tabellenansichten haben Unterstützung für Drag & drop in iOS-11. Weitere Informationen finden Sie unter der [Drag & Drop](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) Handbuch.


## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualisieren Ihre App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
