---
title: Visuelle Designupdates in iOS 11
description: Dieses Dokument beschreibt visuellen Entwurf, die Updates in iOS 11 eingeführt. Es wird Änderungen an Navigationsleisten, Search-Controller, Ränder, Vollbild-Inhalte und Tabellenansichten erläutert.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: c6351f2c25f8e31181c761aea1b471315a8a05e8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400203"
---
# <a name="visual-design-updates-in-ios-11"></a>Visuelle Designupdates in iOS 11

IOS 11 hat Apple neue visuelle Änderungen, einschließlich Updates für die Navigationsleiste, eine Suchleiste und eine Tabellenansichten eingeführt. Darüber hinaus wurden Verbesserungen vorgenommen, um für mehr Flexibilität über Ränder und Vollbild-Inhalte zu ermöglichen. Diese Änderungen werden in diesem Handbuch behandelt. 

Informationen speziell zu entwerfen, die für das iPhone X, sehen Sie sich von Apple [entwerfen, die für das iPhone X](https://developer.apple.com/videos/play/fall2017/801/) video.

## <a name="uikit"></a>UIKit

In iOS 11, um sie für Benutzer leichter zugänglich zu machen haben UIKit Balken angepasst wurde.

Eine solche Änderung ist eine neue HUD-Anzeige, die angezeigt wird, wenn ein Benutzer lang – auf einen Balken drückt Element. Um dies zu aktivieren, legen die `largeContentSizeImage` Eigenschaft `UIBarItem` und fügen Sie ein größeres Bild über eine [Asset-Katalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Navigationsleiste
iOS 11 eingeführte neue Funktionen zur Navigation Leiste Titel leichter lesbar zu machen. Apps können diese größeren Titel anzeigen, indem Zuweisen der `PrefersLargeTitles` Eigenschaft auf "true":

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Einstellung größere Titel in Ihrer app macht _alle_ Navigation Balken Titel für Ihre app größer angezeigt werden, wie im folgenden Screenshot dargestellt:

![Große Navigationstitel](visual-design-images/image7.png)

Um zu steuern, wenn ein große Titel auf einer Navigationsleiste angezeigt wird, legen Sie die `LargeTitleDisplayMode` auf das Navigationselement `Always`, `Never`, oder `Automatic`.

### <a name="search-controller"></a>Search-Controller

iOS 11 hat es zum Hinzufügen eines Search-Controllers direkt auf der Navigationsleiste vereinfacht. Nachdem Sie einen Controller für die Suche erstellt haben, fügen Sie es zur Navigationsleiste mit der `SearchController` Eigenschaft:

```csharp
NavigationItem.SearchController = searchController;
```

[![Große Navigationstitel Suchleiste](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

Abhängig von den Funktionen Ihrer App können Sie oder sollten sich nicht auf die Suchleiste ausblenden, wenn ein Benutzer eine Liste einen Bildlauf. Sie können anpassen, dies mithilfe der `HidesSearchBarWhenScrolling` Eigenschaft.

## <a name="margins"></a>Seitenränder

Apple hat eine neue Eigenschaft: `directionalLayoutMargins` –, die verwendet werden kann, um den Abstand zwischen den Ansichten und Unteransichten festzulegen. Verwendung `directionalLayoutMargins` mit `leading` oder `trailing` Abstände. Unabhängig davon, ob das System einer links-nach-rechts- bzw. rechts-nach-links-Sprache ist wird der Abstand in Ihrer app nach iOS-entsprechend festlegen.

Unter iOS 10 und vor mussten alle Ansichten eine minimale Rand-Größe, an der sie ausgerichtet werden. iOS 11 eingeführt, die Option zum Überschreiben, dass die Verwendung `ViewRespectsSystemMinimumLayoutMargins`. Beispielsweise ermöglicht das Festlegen dieser Eigenschaft auf "false" Sie Ihre Edge-Abstände, 0 (null) anpassen:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Mit der Bildrand mit 0 (null) Inset in Ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Vollbild-Inhalt

iOS 7 [eingeführt](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` und `bottomLayoutGuide` als eine Möglichkeit zum Einschränken von Ansichten, damit sie nicht durch UIKit Balken ausgeblendet und befinden sich in einer sichtbaren Bereich des Bildschirms. Diese sind veraltet in iOS 11 zugunsten des der _Sicherheitsbereich_.

Der sichere Bereich ist eine neue Weise angehen der Raum der Anwendung und wie die Einschränkungen zwischen einer Ansicht und eine übergeordnete Ansicht hinzugefügt werden. Betrachten Sie beispielsweise die folgende Abbildung aus:

[![Sicherheitsbereich-Vs obere und untere Layoutführungslinie](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Zuvor, wenn Sie hatten eine Ansicht hinzugefügt und im grünen Bereich oben angezeigt werden sollte, Sie würden einschränken, damit die _unten_ von der `TopLayoutGuide` und _oben_ von der `BottomLayoutGuide`. In iOS 11, Sie würden stattdessen einschränken, damit die _oben_ und _unten_ des sicheren Bereichs. Beispiel:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Tabellenansicht

Die UITableView gab es eine Reihe von kleinen, aber wesentliche Änderungen in iOS 11.

Standardmäßig werden Kopfzeilen, Fußzeilen und Zellen jetzt automatisch angepasst, auf Grundlage ihres Inhalts. Dieser Satz für automatische größenanpassung Verhalten abwählen der `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, oder `EstimatedSectionFooterHeight` 0 (null).

Jedoch in einigen Fällen (z. B. beim Hinzufügen von UITableViewController im iOS-Designer oder bei Verwendung von vorhandenen Storboards in Interface Builder) müssen Sie möglicherweise selbst sizing Zellen manuell aktivieren. Zu diesem Zweck stellen Sie sicher, dass Sie die folgenden Eigenschaften für die Tabellenansicht für Zellen, Kopf- und Fußzeilen, bzw. festgelegt haben:

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

iOS 11 wurde die Funktionalität von Zeilenaktionen erweitert. `UISwipeActionsConfiguration` wurde eingeführt, um eine Reihe von Aktionen zu definieren, die durchgeführt werden soll, wenn der Benutzer in beide Richtungen für eine Zeile in einer Tabelle mit einer wischbewegung. Dieses Verhalten ist vergleichbar mit der systemeigenen Mail.app. Weitere Informationen finden Sie unter den [Zeilenaktionen](~/ios/user-interface/controls/tables/row-action.md) Guide.

Tabellenansichten haben Unterstützung für Drag & drop in iOS 11. Weitere Informationen finden Sie unter den [Drag & Drop](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) Guide.


## <a name="related-links"></a>Verwandte Links

- [Was ist neu in iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App-Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Entwerfen für das iPhone X (Apple) (Video)](https://developer.apple.com/videos/play/fall2017/801/)
- [Aktualisieren Ihrer App für iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)

