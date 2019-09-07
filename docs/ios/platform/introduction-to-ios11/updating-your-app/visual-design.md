---
title: Visuelle Entwurfs Updates in ios 11
description: In diesem Dokument werden die in ios 11 eingeführten visuellen Design Updates beschrieben. Es werden Änderungen an Navigationsleisten, Such Controllern, Rändern, voll Bildinhalten und Tabellen Ansichten erläutert.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: bd3adf5d01be0cdb709c752e1ace131b8b3e8d83
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752257"
---
# <a name="visual-design-updates-in-ios-11"></a>Visuelle Entwurfs Updates in ios 11

In ios 11 hat Apple neue visuelle Änderungen eingeführt, einschließlich Aktualisierungen der Navigationsleiste, der Suchleiste und der Tabellen Ansichten. Außerdem wurden Verbesserungen vorgenommen, um mehr Flexibilität über Ränder und voll Bildinhalte zu ermöglichen. Diese Änderungen werden in diesem Handbuch behandelt. 

Weitere Informationen zum Entwerfen von für iPhone x finden Sie im Apple- [Design für iPhone x](https://developer.apple.com/videos/play/fall2017/801/) -Video.

## <a name="uikit"></a>UIKit

UIKit-Balken wurden in ios 11 angepasst, um Endbenutzern den Zugriff auf die Benutzer zu erleichtern.

Eine solche Änderung ist eine neue HUD-Anzeige, die angezeigt wird, wenn ein Benutzer ein Balken Element lange drückt. Um dies zu ermöglichen, legen `largeContentSizeImage` Sie die `UIBarItem` -Eigenschaft auf fest, und fügen Sie ein größeres Bild über einen Ressourcen [Katalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md)hinzu:

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Navigationsleiste
in ios 11 wurden neue Funktionen eingeführt, um das Lesen von Navigationsleisten Titeln zu vereinfachen. Apps können diesen größeren Titel anzeigen, indem Sie `PrefersLargeTitles` die-Eigenschaft auf "true" zuweisen:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Durch das Festlegen größerer Titel in der App werden _alle_ Titel der Navigationsleisten in der APP größer, wie im folgenden Screenshot veranschaulicht:

![Großer Navigations Titel](visual-design-images/image7.png)

Um zu steuern, wann ein großer Titel auf einer Navigationsleiste angezeigt wird, `LargeTitleDisplayMode` legen Sie `Always`für das Navigationselement `Never`auf, `Automatic`oder fest.

### <a name="search-controller"></a>Controller suchen

IOS 11 vereinfacht das Hinzufügen eines Such Controllers direkt zur Navigationsleiste. Nachdem Sie einen Such Controller erstellt haben, fügen Sie ihn der Navigationsleiste mit der `SearchController` -Eigenschaft hinzu:

```csharp
NavigationItem.SearchController = searchController;
```

[![Großer Navigations Titel mit Suchleiste](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

Abhängig von der Funktionalität Ihrer APP möchten Sie möglicherweise, dass die Suchleiste ausgeblendet wird, wenn ein Benutzer einen Bildlauf durch eine Liste durchführt. Sie können dies mithilfe der `HidesSearchBarWhenScrolling` -Eigenschaft anpassen.

## <a name="margins"></a>Seitenränder

Apple hat eine neue Eigenschaft – `directionalLayoutMargins` – erstellt, die verwendet werden kann, um den Raum zwischen Sichten und unter Ansichten festzulegen. Verwenden `directionalLayoutMargins` Sie `leading` with oder`trailing` insets. Unabhängig davon, ob es sich bei dem System um eine Sprache von links nach rechts oder von rechts nach Links handelt, wird der Abstand in der APP von IOS entsprechend festgelegt.

In ios 10 und früher verfügten alle Sichten über eine minimale Rand Größe, auf die Sie ausgerichtet wurden. IOS 11 hat die Option zum Überschreiben der `ViewRespectsSystemMinimumLayoutMargins`mit eingeführt. Wenn Sie diese Eigenschaft beispielsweise auf false festlegen, können Sie die Edge-insets auf NULL festlegen:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```

![Bild, das den Rand mit NULL-Einzug in ios 11 anzeigt](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Vollbild-Inhalt

IOS 7 [wurde eingeführt](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` und `bottomLayoutGuide` als Möglichkeit, ihre Ansichten so einzuschränken, dass Sie nicht durch UIKit-Balken ausgeblendet werden und sich in einem sichtbaren Bereich des Bildschirms befinden. Diese wurden in ios 11 zugunsten des _sicheren Bereichs_eingestellt.

Der sichere Bereich ist eine neue Möglichkeit, den sichtbaren Raum Ihrer Anwendung zu überprüfen und die Einschränkungen zwischen einer Ansicht und einer Super Ansicht hinzuzufügen. Sehen Sie sich z. b. die folgende Abbildung an:

[![Sicherer Bereich im Vergleich zum oberen und unteren layouthandbuch](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Wenn Sie zuvor eine Ansicht hinzugefügt haben und möchten, dass Sie im grünen Bereich angezeigt wird, würden Sie Sie am _unteren Rand_ von `TopLayoutGuide` und `BottomLayoutGuide`am _oberen_ Rand von einschränken. In ios 11 würden Sie Sie stattdessen auf den _oberen_ und _unteren Rand_ des sicheren Bereichs beschränken. Beispiel:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Tabellenansicht

Die uitableview hat eine Reihe kleiner, aber signifikanter Änderungen in ios 11.

Standardmäßig werden Kopfzeilen, Fußzeilen und Zellen automatisch auf Grundlage ihres Inhalts vergrößert. Wenn Sie dieses Verhalten für die `EstimatedRowHeight`automatische Größenänderung ablehnen möchten, legen Sie, `EstimatedSectionHeaderHeight`oder `EstimatedSectionFooterHeight` auf 0 (null) fest.

In einigen Fällen (z. b. beim Hinzufügen von "uitableviewcontroller" im IOS-Designer oder bei Verwendung vorhandener storboards in Interface Builder) müssen Sie die Zellen der selbst Dimensionierung möglicherweise manuell aktivieren. Stellen Sie zu diesem Zweck sicher, dass Sie die folgenden Eigenschaften für die Tabellenansicht für Zellen, Kopfzeilen und Fußzeilen festgelegt haben:

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

IOS 11 hat die Funktionalität von Zeilen Aktionen erweitert. `UISwipeActionsConfiguration`wurde eingeführt, um einen Satz von Aktionen zu definieren, die durchgeführt werden sollten, wenn der Benutzer in einer der Zeilen in einer Tabellenansicht in beide Richtungen bewegt wird. Dieses Verhalten ähnelt dem der nativen Mail. app. Weitere Informationen finden Sie im [Zeilen Aktions](~/ios/user-interface/controls/tables/row-action.md) Handbuch.

Tabellen Sichten unterstützen Drag & Drop in ios 11. Weitere Informationen finden Sie im [Drag & Drop-](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) Handbuch.

## <a name="related-links"></a>Verwandte Links

- [Neuerungen in ios 11 (Apple)](https://developer.apple.com/ios/)
- [Aktualisierte App Store-Produktseite (Apple)](https://developer.apple.com/app-store/product-page/)
- [Entwerfen für iPhone X (Apple) (Video)](https://developer.apple.com/videos/play/fall2017/801/)
- [Aktualisieren Ihrer APP für IOS 11 (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/204/)
