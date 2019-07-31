---
title: Einheitliche Storyboards in xamarin. IOS
description: In diesem Dokument werden Unified Storyboards in xamarin. IOS beschrieben. Mit Unified Storyboards können Entwickler mehrere Bildschirmgrößen mit einer einzelnen Schnittstellen Definition unterstützen.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 02bc6fe7109f13629e776c800657846fca02641e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657131"
---
# <a name="unified-storyboards-in-xamarinios"></a>Einheitliche Storyboards in xamarin. IOS

IOS 8 umfasst einen neuen, einfacher zu verwendenden Mechanismus zum Erstellen der Benutzeroberfläche – das vereinheitlichte Storyboard. Mit einem einzelnen Storyboard, das alle unterschiedlichen Hardware Bildschirmgrößen abdeckt, können schnelle und reaktionsfähige Ansichten im Stil "Design-Once, use-many" erstellt werden.

Da der Entwickler nicht mehr ein separates und bestimmtes Storyboard für iPhone-und iPad-Geräte erstellen muss, ist die Flexibilität, Anwendungen mit einer gemeinsamen Schnittstelle zu entwerfen und diese Schnittstelle für verschiedene Größenklassen anzupassen. Auf diese Weise kann eine Anwendung an die Stärken der einzelnen Formfaktoren angepasst werden, und jede Benutzeroberfläche kann optimiert werden, um die beste Benutzeroberfläche bereitzustellen.

<a name="size-classes" />

## <a name="size-classes"></a>Größenklassen

Vor IOS 8 nutzte `UIInterfaceOrientation` der Entwickler und `UIInterfaceIdiom` , um zwischen dem hoch-und Querformat sowie zwischen iPhone-und iPad-Geräten zu unterscheiden. In iOS8 werden Ausrichtung und Gerät mithilfe von *Größenklassen*bestimmt.

Geräte werden von Größenklassen in vertikalen und horizontalen Achsen definiert, und es gibt zwei Typen von Größenklassen in ios 8:

-  **Regulär** – Dies ist entweder für eine große Bildschirmgröße (z. b. ein iPad) oder eine Mini Anwendung, die den Eindruck einer großen Größe (z. b.`UIScrollView`
-  **Compact** – Dies gilt für kleinere Geräte (z. b. ein iPhone). Diese Größe berücksichtigt die Ausrichtung des Geräts.


Wenn die beiden Konzepte gleichzeitig verwendet werden, ist das Ergebnis ein 2 x 2-Raster, in dem die verschiedenen möglichen Größen definiert werden, die in den unterschiedlichen Ausrichtungen verwendet werden können, wie in der folgenden Abbildung dargestellt:

 [![](unified-storyboards-images/sizeclassgrid.png "Ein 2 x 2-Raster, das die verschiedenen möglichen Größen definiert, die in regulären und kompakten Ausrichtungen verwendet werden können")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Der Entwickler kann einen Ansichts Controller erstellen, der eine der vier Möglichkeiten verwendet, die zu unterschiedlichen Layouts führen würden (wie in der obigen Grafik zu sehen).

### <a name="ipad-size-classes"></a>iPad size-Klassen

Das iPad verfügt aufgrund der Größe über eine **reguläre** Klassengröße für beide Ausrichtungen.

 [![](unified-storyboards-images/image1.png "iPad size-Klassen")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone size-Klassen

Basierend auf der Ausrichtung des Geräts hat das iPhone verschiedene Größenklassen:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone size-Klassen")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Wenn sich das Gerät im Hochformat befindet, hat der Bildschirm eine **kompakte** Klasse horizontal **und vertikal vertikal** .
-  Wenn sich das Gerät im Querformat befindet, werden die Bildschirm Klassen aus dem Hochformat Modus zurückgesetzt.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus Size-Klassen

Die Größen sind identisch mit den früheren iPhones, wenn Sie in der Hochformat Ausrichtung, aber unterschiedlich sind:

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus Size-Klassen")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Da das iPhone 6 Plus über einen ausreichend großen Bildschirm verfügt, kann es im Querformat eine Klasse mit regulärer Breite sein.

### <a name="support-for-a-new-screen-scale"></a>Unterstützung für eine neue Bildschirm Skala

Das iPhone 6 Plus verwendet eine neue Retina HD-Anzeige mit einem Bildschirm Skalierungsfaktor von 3,0 (dreimal der ursprünglichen iPhone-Bildschirmauflösung). Um die bestmögliche Leistung auf diesen Geräten zu gewährleisten, fügen Sie neue, für diese Bildschirm Skala entwickelte Grafiken hinzu. In Xcode 6 und höher können Asset-Kataloge Bilder mit einer Größe von 1 x, 2X und 3X enthalten. Fügen Sie einfach die neuen Image Assets hinzu, und IOS wählt die richtigen Assets aus, wenn Sie auf einem iPhone 6 Plus ausgeführt werden.

Das Bild Ladeverhalten in ios erkennt auch ein `@3x` Suffix für Bilddateien. Beispiel: Wenn der Entwickler ein Image-Asset (bei unterschiedlichen Auflösungen) im Paket der Anwendung mit den folgenden Dateinamen enthält: `MonkeyIcon.png`, `MonkeyIcon@2x.png`und `MonkeyIcon@3x.png`. Auf dem iPhone 6 und wird `MonkeyIcon@3x.png` das Abbild automatisch verwendet, wenn der Entwickler ein Image mit dem folgenden Code lädt:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Wenn Sie das Bild einem Benutzeroberflächen Element mithilfe des IOS-Designers als `MonkeyIcon.png`zuweisen, wird der `MonkeyIcon@3x.png` automatisch auf dem iPhone 6 Plus verwendet.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Dynamische Startbildschirme

Die Startbildschirm Datei wird als Begrüßungsbildschirm angezeigt, während eine IOS-Anwendung gestartet wird, um dem Benutzer Feedback bereitzustellen, dass die APP tatsächlich gestartet wird. Vor IOS 8 musste der Entwickler mehrere `Default.png` Abbild Assets für jeden Gerätetyp, jede Ausrichtung und Bildschirmauflösung einschließen, auf dem die Anwendung ausgeführt wird.

Neu bei IOS 8: Entwickler können eine einzelne atomarische `.xib` Datei in Xcode erstellen, die Auto Layout-und Größenklassen verwendet, um einen *dynamischen Startbildschirm* zu erstellen, der für jedes Gerät, jede Auflösung und jede Ausrichtung funktioniert. Dadurch wird nicht nur der Arbeitsaufwand reduziert, den der Entwickler benötigt, um alle erforderlichen Image Ressourcen zu erstellen und zu verwalten, sondern die Größe des installierten Pakets der Anwendung wird reduziert.

## <a name="traits"></a>Merkmale

Merkmale sind Eigenschaften, die verwendet werden können, um zu bestimmen, wie sich ein Layout ändert, wenn sich die Umgebung ändert. Sie bestehen aus einem `HorizontalSizeClass` Satz von Eigenschaften ( `VerticalSizeClass` basierend auf `UIUserInterfaceSizeClass`) sowie dem Schnittstellen Ausdruck ( `UIUserInterfaceIdiom`) und der Anzeige Skala.

Alle oben genannten Zustände werden in einem Container umschließt, auf den Apple als Merkmals Auflistung ( `UITraitCollection`) verweist, die nicht nur die Eigenschaften, sondern auch ihre Werte enthält.

## <a name="trait-environment"></a>Merkmals Umgebung

Merkmals Umgebungen sind eine neue Schnittstelle in ios 8 und können eine Merkmals Auflistung für die folgenden Objekte zurückgeben:

-  Bildschirme `UIScreens` ().
-  Windows ( `UIWindows` ).
-  Anzeigen von Controllern ( `UIViewController` ).
-  Sichten ( `UIView` ).
-  Präsentations Controller ( `UIPresentationController` ).


Der Entwickler verwendet die von einer Merkmals Umgebung zurückgegebene Merkmals Auflistung, um zu bestimmen, wie eine Benutzeroberfläche angelegt werden soll.

Alle Merkmals Umgebungen machen eine Hierarchie wie in der folgenden Abbildung dargestellt:

 [![](unified-storyboards-images/viewhierarchy.png "Das Hierarchie Diagramm für Merkmals Umgebungen")](unified-storyboards-images/viewhierarchy.png#lightbox)

Die Merkmals Auflistung, die in den obigen Merkmals Umgebungen vorhanden ist, wird standardmäßig von der übergeordneten zur untergeordneten Umgebung geleitet.

Zusätzlich zum erhalten der aktuellen Merkmals Auflistung verfügt die Merkmals Umgebung über `TraitCollectionDidChange` eine-Methode, die in den Ansichts-oder Ansichts Controller-Unterklassen überschrieben werden kann. Der Entwickler kann diese Methode verwenden, um alle Benutzeroberflächen Elemente zu ändern, die von Merkmalen abhängig sind, wenn sich diese Merkmale geändert haben.

## <a name="typical-trait-collections"></a>Typische Merkmals Sammlungen

In diesem Abschnitt werden typische Typen von Merkmals Auflistungen behandelt, die der Benutzer bei der Arbeit mit IOS 8 erleben wird.

Im folgenden finden Sie eine typische Merkmals Auflistung, die dem Entwickler möglicherweise auf einem iPhone angezeigt wird:

|Eigenschaft|Wert|
|--- |--- |
|`HorizontalSizeClass`|Theit|
|`VerticalSizeClass`|Regulär|
|`UserInterfaceIdom`|Phone|
|`DisplayScale`|2.0|

Der obige Satz stellt eine voll qualifizierte Merkmals Auflistung dar, da Sie Werte für alle Merkmals Eigenschaften hat.

Es ist auch möglich, eine Merkmals Auflistung zu haben, in der einige ihrer Werte fehlen (was Apple als *nicht angegeben*bezeichnet):

|Eigenschaft|Wert|
|--- |--- |
|`HorizontalSizeClass`|Theit|
|`VerticalSizeClass`|Nicht angegeben.|
|`UserInterfaceIdom`|Nicht angegeben.|
|`DisplayScale`|Nicht angegeben.|

Wenn der Entwickler jedoch die Merkmals Umgebung für seine Merkmals Auflistung anfordert, wird eine voll qualifizierte Sammlung zurückgegeben, wie im obigen Beispiel gezeigt.

Wenn sich eine Merkmals Umgebung (z. b. ein Ansichts-oder Ansichts Controller) nicht innerhalb der aktuellen Ansichts Hierarchie befindet, erhält der Entwickler möglicherweise nicht angegebene Werte für eine oder mehrere Merkmals Eigenschaften.

Der Entwickler erhält auch eine teilweise qualifizierte Merkmals Auflistung, wenn Sie eine der von Apple bereitgestellten Erstellungs Methoden verwenden, `UITraitCollection.FromHorizontalSizeClass`z. b., um eine neue Sammlung zu erstellen.

Ein Vorgang, der für mehrere Merkmals Auflistungen ausgeführt werden kann, besteht darin, diese miteinander zu vergleichen, wobei eine Merkmals Auflistung abgefragt wird, wenn Sie eine andere Auflistung enthält. Die Bedeutung von *Containment* besteht darin, dass der Wert für jedes in der zweiten Auflistung angegebene Merkmal genau mit dem Wert in der ersten Auflistung übereinstimmen muss.

Zum Testen von zwei Merkmalen verwenden `Contains` Sie die- `UITraitCollection` Methode des-Werts, der den Wert des zu testenden Merkmals übergibt.

Der Entwickler kann die Vergleiche manuell im Code durchführen, um zu bestimmen, wie Sichten oder Ansichts Controller angezeigt werden. Allerdings `UIKit` verwendet diese Methode intern, um einige Funktionen bereitzustellen, wie z. b. den Proxy für die Darstellung.

## <a name="appearance-proxy"></a>Darstellungs Proxy

Der Darstellungs Proxy wurde in früheren Versionen von IOS eingeführt, damit Entwickler die Eigenschaften ihrer Ansichten anpassen können. Es wurde in ios 8 erweitert, um Merkmals Sammlungen zu unterstützen.

Darstellungs Proxys enthalten nun eine `AppearanceForTraitCollection`neue Methode,, die einen neuen Darstellungs Proxy für die angegebene Merkmals Auflistung zurückgibt, die übergeben wurde. Alle Anpassungen, die der Entwickler auf diesem Darstellungs Proxy ausführt, wirken sich nur auf Sichten aus, die der angegebenen Merkmals Auflistung entsprechen.

Im Allgemeinen übergibt der Entwickler eine teilweise angegebene Merkmals Auflistung an die `AppearanceForTraitCollection` Methode, z. b. eine, die gerade eine horizontale Größenklasse von Compact angegeben hat, sodass Sie jede Ansicht in der Anwendung anpassen können, die horizontal komprimiert ist.

## <a name="uiimage"></a>Uiimage

Eine andere Klasse, der Apple die Merkmals Auflistung `UIImage`hinzugefügt hat, ist. In der Vergangenheit musste der Entwickler eine @1X -und- @2x Version eines beliebigen bitzugeordneten Grafik Objekts angeben, das in der Anwendung enthalten sein soll (z. b. ein Symbol).

IOS 8 wurde erweitert, um dem Entwickler zu ermöglichen, mehrere Versionen eines Bilds in einem Bildkatalog auf der Grundlage einer Merkmals Auflistung einzubeziehen. Der Entwickler könnte z. b. ein kleineres Image zum Arbeiten mit einer kompakten Merkmals Klasse und ein Bild mit voller Größe für jede andere Sammlung einschließen.

Wenn eines der Bilder in einer `UIImageView` Klasse verwendet wird, wird in der Bildansicht automatisch die korrekte Version des Bilds für die Merkmals Auflistung angezeigt. Wenn sich die Merkmals Umgebung ändert (z. b. wenn der Benutzer das Gerät vom Hochformat in das Querformat wechselt), wählt die Bildansicht automatisch die neue Bildgröße aus, um der neuen Merkmals Auflistung zu entsprechen, und ändert ihre Größe entsprechend der aktuellen Version des Abbilds. gestellte.

## <a name="uiimageasset"></a>UIImageAsset

Apple hat eine neue Klasse mit dem Namen `UIImageAsset` IOS 8 hinzugefügt, um dem Entwickler noch mehr Kontrolle über die Bildauswahl zu verschaffen.

Ein Image-Asset umfasst alle unterschiedlichen Versionen eines Bilds und ermöglicht es dem Entwickler, ein bestimmtes Bild anzufordern, das einer übergebenen Merkmals Auflistung entspricht. Bilder können einem Image-Asset dynamisch hinzugefügt oder daraus entfernt werden.

Weitere Informationen zu Image Assets finden Sie in der [uiimageasset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) -Dokumentation von Apple.

## <a name="combining-trait-collections"></a>Kombinieren von Merkmal Sammlungen

Eine weitere Funktion, die ein Entwickler für Merkmals Auflistungen ausführen kann, besteht darin, zwei zusammen hinzuzufügen, die zur kombinierten Auflistung führen, wobei die nicht angegebenen Werte aus einer Auflistung durch die angegebenen Werte in einer zweiten ersetzt werden. Dies erfolgt mithilfe der `FromTraitsFromCollections` -Methode `UITraitCollection` der-Klasse.

Wie bereits erwähnt, wird der Wert auf die angegebene Version festgelegt, wenn eine der Merkmale in einer der Merkmals Auflistungen nicht angegeben ist und in einem anderen angegeben wird. Wenn jedoch mehrere Versionen eines bestimmten Werts angegeben sind, ist der Wert aus der letzten Merkmals Auflistung der Wert, der verwendet wird.

## <a name="adaptive-view-controllers"></a>Adaptive Ansichts Controller

In diesem Abschnitt wird erläutert, wie die IOS-Ansichts-und-Ansichts Controller die Konzepte von Merkmalen und Größenklassen übernommen haben, damit Sie in den Anwendungen des Entwicklers automatisch adaptiver werden.

### <a name="split-view-controller"></a>Split View Controller

Eine der Ansichts Controller Klassen, die am meisten in ios 8 geändert wurden, `UISplitViewController` ist die-Klasse. In der Vergangenheit nutzte der Entwickler häufig einen Split View Controller in der iPad-Version der Anwendung und musste dann eine vollkommen andere Version der Ansichts Hierarchie für die iPhone-Version der APP bereitstellen.

In ios 8 ist die `UISplitViewController` -Klasse auf beiden Plattformen (iPad und iPhone) verfügbar, die es dem Entwickler ermöglichen, eine Ansichts Controller Hierarchie zu erstellen, die für iPhone und iPad funktioniert.

Wenn sich ein iPhone im Querformat befindet, zeigt der Split View Controller seine Ansichten nebeneinander an, genauso wie er auf einem iPad angezeigt wird.

### <a name="overriding-traits"></a>Überschreiben von Merkmalen

Merkmals Umgebungen werden aus dem übergeordneten Container nach unten zu den untergeordneten Containern kaskadiert, wie in der folgenden Abbildung dargestellt, die einen Split View Controller auf einem iPad im Querformat anzeigt:

 [![](unified-storyboards-images/cascadingclasses01.png "Einen geteilten Ansichts Controller auf einem iPad im Querformat")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Da das iPad sowohl in horizontaler als auch vertikaler Ausrichtung über eine reguläre Größenklasse verfügt, werden in der geteilten Ansicht sowohl die Master-als auch die Detailansicht angezeigt.

Auf einem iPhone, bei dem die Size-Klasse in beiden Ausrichtungen kompakt ist, zeigt der Split View Controller nur die Detailansicht an, wie unten gezeigt:

 [![](unified-storyboards-images/cascadingclasses02.png "Der Split View Controller zeigt nur die Detailansicht an.")](unified-storyboards-images/cascadingclasses02.png#lightbox)

In einer Anwendung, in der der Entwickler sowohl die Master-als auch die Detailansicht auf einem iPhone im Querformat anzeigen möchte, muss der Entwickler einen übergeordneten Container für den Split View Controller einfügen und die Merkmals Auflistung überschreiben. Wie in der Abbildung unten gezeigt:

 [![](unified-storyboards-images/cascadingclasses03.png "Der Entwickler muss fügen Sie einen übergeordneten Container der Split-View-Controller und überschreiben die Auflistung Merkmal")](unified-storyboards-images/cascadingclasses03.png#lightbox)

Ein `UIView` wird als übergeordnetes Element des Split View-Controllers festgelegt `SetOverrideTraitCollection` , und die-Methode wird für die Ansicht aufgerufen, die eine neue Merkmals Auflistung übergibt und den Split View Controller als Ziel verwendet. Die neue Merkmals Auflistung überschreibt `HorizontalSizeClass`das, indem es `Regular`auf festgelegt wird, sodass der Split View Controller sowohl die Master-als auch die Detailansicht auf einem iPhone in der Querformat Ansicht anzeigt.

Beachten Sie, `VerticalSizeClass` dass auf `unspecified`festgelegt wurde, wodurch die neue Merkmals Auflistung der Merkmals Auflistung auf dem übergeordneten Element hinzugefügt werden `Compact VerticalSizeClass` kann. Dies führt zu einer für den untergeordneten Split View-Controller.

### <a name="trait-changes"></a>Merkmals Änderungen

In diesem Abschnitt wird ausführlich erläutert, wie sich die Merkmals Auflistungen bei der Änderung der Merkmals Umgebung übernehmen. Dies ist beispielsweise der Fall, wenn das Gerät von Hochformat in Querformat gedreht wird.

 [![](unified-storyboards-images/traittransitions01.png "Übersicht über die Merkmale von hoch-zu Querformat Änderungen")](unified-storyboards-images/traittransitions01.png#lightbox)

Zuerst führt IOS 8 einige Setup Schritte aus, um den Übergang vorzubereiten. Im nächsten Schritt animiert das System den Übergangsstatus. Schließlich bereinigt IOS 8 alle temporären Zustände, die während des Übergangs erforderlich sind.

IOS 8 bietet mehrere Rückrufe, mit denen der Entwickler an der Merkmals Änderung teilnehmen kann, wie in der folgenden Tabelle gezeigt:

|Phase|Rückruf|Beschreibung|
|--- |--- |--- |
|Setup|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Diese Methode wird zu Beginn einer Merkmals Änderung aufgerufen, bevor eine Merkmals Auflistung auf Ihren neuen Wert festgelegt wird.</li><li>Die-Methode wird aufgerufen, wenn sich der Wert der Merkmals Auflistung geändert hat, aber bevor eine Animation stattfindet.</li></ul>|
|Animation|`WillTransitionToTraitCollection`|Der Übergangs Koordinator, der an diese Methode weitergegeben wird `AnimateAlongside` , verfügt über eine-Eigenschaft, die es dem Entwickler ermöglicht, Animationen hinzuzufügen, die zusammen mit den Standard Animationen ausgeführt werden.|
|Bereinigen|`WillTransitionToTraitCollection`|Stellt eine Methode bereit, mit der Entwickler ihren eigenen Bereinigungs Code nach der Umstellung einschließen können.|

Die `WillTransitionToTraitCollection` -Methode eignet sich hervorragend zum Animieren von Ansichts Controllern zusammen mit den Änderungen der Merkmals Auflistung. Die `WillTransitionToTraitCollection` -Methode ist nur auf Ansichts Controllern ( `UIViewController`) und nicht in anderen Merkmals `UIViews`Umgebungen verfügbar, wie z. b.

Der `TraitCollectionDidChange` eignet sich hervorragend für die `UIView` Arbeit mit der-Klasse, bei der der Entwickler die Benutzeroberfläche bei Änderung der Merkmale aktualisieren möchte.

### <a name="collapsing-the-split-view-controllers"></a>Reduzieren der geteilten Ansichts Controller

Sehen wir uns nun genauer an, was geschieht, wenn ein Split View Controller von einer zwei Spalte zu einer einzigen Spalten Ansicht reduziert wird. Im Rahmen dieser Änderung müssen zwei Prozesse ausgeführt werden:

-  Standardmäßig verwendet der Split View Controller den primären Ansichts Controller als Ansicht, nachdem der Reduzierungs Vorgang aufgetreten ist. Der Entwickler kann dieses Verhalten überschreiben, indem `GetPrimaryViewControllerForCollapsingSplitViewController` er die `UISplitViewControllerDelegate` -Methode von überschreibt und einen beliebigen Ansichts Controller bereitstellt, der im reduzierten Zustand angezeigt werden soll.
-  Der sekundäre Ansichts Controller muss mit dem primären Ansichts Controller zusammengeführt werden. Der Entwickler muss in der Regel keine Aktion für diesen Schritt durchführen. der Split View Controller umfasst die automatische Verarbeitung dieser Phase basierend auf dem Hardware Gerät. Es gibt jedoch möglicherweise einige Sonderfälle, in denen der Entwickler mit dieser Änderung interagieren möchte. Durch Aufrufen `CollapseSecondViewController` der-Methode `UISplitViewControllerDelegate` von kann der Master Ansichts Controller angezeigt werden, wenn der Reduzierungs Vorgang statt der Detailansicht auftritt.


### <a name="expanding-the-split-view-controller"></a>Erweitern des Split View-Controllers

Sehen wir uns nun genauer an, was geschieht, wenn ein Split View Controller von einem reduzierten Zustand aus erweitert wird. Noch einmal müssen zwei Schritte ausgeführt werden:

-  Definieren Sie zunächst den neuen primären Ansichts Controller. Standardmäßig verwendet der Split View Controller automatisch den primären Ansichts Controller aus der reduzierten Ansicht. Der Entwickler kann dieses Verhalten überschreiben, indem er `GetPrimaryViewControllerForExpandingSplitViewController` die-Methode `UISplitViewControllerDelegate` von verwendet.
-  Nachdem der primäre Ansichts Controller ausgewählt wurde, muss der sekundäre Ansichts Controller neu erstellt werden. Der Split View Controller schließt die automatische Verarbeitung dieser Phase basierend auf dem Hardware Gerät ein. Der Entwickler kann dieses Verhalten überschreiben, indem `SeparateSecondaryViewController` er die- `UISplitViewControllerDelegate` Methode von aufrufen.


In einem Split View Controller spielt der primäre Ansichts Controller eine Rolle beim Erweitern und reduzieren der Sichten, indem die `CollapseSecondViewController` -Methode und die- `SeparateSecondaryViewController` Methode von `UISplitViewControllerDelegate`implementiert werden. `UINavigationController`implementiert diese Methoden, um den sekundären Ansichts Controller automatisch per Push zu übermitteln.

### <a name="showing-view-controllers"></a>Anzeige Controller anzeigen

Eine weitere Änderung, die Apple an IOS 8 vorgenommen hat, ist die Art und Weise, wie der Entwickler Ansichts Controller anzeigt. In der Vergangenheit, wenn die Anwendung einen endansichts Controller besaß (z. b. einen Tabellen Ansichts Controller) und der Entwickler einen anderen angezeigt hat (z. b. als Reaktion auf den Benutzer, der auf eine Zelle tippt), würde die Anwendung wieder durch die Controller Hierarchie zum Navigations Ansichts Controller und ruft `PushViewController` die-Methode auf, um die neue Ansicht anzuzeigen.

Dies führte zu einer sehr engen Kopplung zwischen dem Navigations Controller und der Umgebung, in der er ausgeführt wurde. In ios 8 hat Apple dies durch die Bereitstellung von zwei neuen Methoden entkoppelt:

-  `ShowViewController`– Passt sich an, um den neuen Ansichts Controller auf der Grundlage seiner Umgebung anzuzeigen. Beispielsweise wird in einem `UINavigationController` die neue Ansicht einfach auf den Stapel übertragen. In einem Split View-Controller wird der neue Ansichts Controller auf der linken Seite angezeigt, und zwar im neuen primären Ansichts Controller. Wenn kein Container Ansichts Controller vorhanden ist, wird die neue Ansicht als modaler Ansichts Controller angezeigt.
-  `ShowDetailViewController`– Funktioniert ähnlich wie `ShowViewController`, wird jedoch auf einem geteilten Ansichts Controller implementiert, um die Detailansicht durch den neuen Ansichts Controller zu ersetzen, der weitergegeben wird. Wenn der Split View Controller (wie in einer iPhone-Anwendung angezeigt) reduziert wird, wird der-Befehl an die `ShowViewController` -Methode umgeleitet, und die neue Ansicht wird als primärer Ansichts Controller angezeigt. Wenn kein Container Ansichts Controller vorhanden ist, wird die neue Ansicht als modaler Ansichts Controller angezeigt.


Diese Methoden funktionieren, indem Sie am Blatt Ansichts Controller beginnen und die Ansichts Hierarchie durchlaufen, bis Sie den richtigen Container Ansichts Controller finden, der die Anzeige der neuen Ansicht behandelt.

Entwickler können und `ShowViewController` `ShowDetailViewController` in ihren eigenen benutzerdefinierten Ansichts Controllern implementieren, um `UINavigationController` die gleichen automatisierten `UISplitViewController` Funktionen wie und bereitzustellen.

### <a name="how-it-works"></a>Funktionsweise

In diesem Abschnitt wird erläutert, wie diese Methoden tatsächlich in ios 8 implementiert werden. Zuerst sehen wir uns die neue `GetTargetForAction` Methode an:

 [![](unified-storyboards-images/gettargetforaction.png "Die neue gettargetforaction-Methode")](unified-storyboards-images/gettargetforaction.png#lightbox)

Diese Methode durchläuft die Hierarchie Kette, bis der richtige Container Ansichts Controller gefunden wird. Beispiel:

1.  Wenn eine `ShowViewController` Methode aufgerufen wird, ist der erste Ansichts Controller in der Kette, der diese Methode implementiert, der Navigations Controller und wird daher als übergeordnetes Element der neuen Ansicht verwendet.
1.  Wenn stattdessen `ShowDetailViewController` eine Methode aufgerufen wurde, ist der Split View Controller der erste Ansichts Controller, um ihn zu implementieren, sodass er als übergeordnetes Element verwendet wird.


Die `GetTargetForAction` -Methode sucht mithilfe eines Ansichts Controllers, der eine bestimmte Aktion implementiert, und fragt diesen Ansichts Controller ab, wenn er diese Aktion empfangen möchte. Da diese Methode öffentlich ist, können Entwickler ihre eigenen benutzerdefinierten Methoden erstellen, die genau wie die integrierten `ShowViewController` - `ShowDetailViewController` und-Methoden funktionieren.

## <a name="adaptive-presentation"></a>Adaptive Präsentation

In ios 8 hat Apple popover-Präsentationen ( `UIPopoverPresentationController`) ebenfalls adaptiv gemacht. Ein popover Presentation View-Controller zeigt also automatisch eine normale popover-Ansicht in einer regulären Größenklasse an, zeigt ihn aber voll Bildschirm in einer horizontalen kompakten Größenklasse an (z. b. auf einem iPhone).

Um die Änderungen innerhalb des Unified Storyboard-Systems zu berücksichtigen, wurde ein neues Controller Objekt zum Verwalten der dargestellten Ansichts Controller `UIPresentationController`– erstellt. Dieser Controller wird ab dem Zeitpunkt der Darstellung des Ansichts Controllers erstellt, bis er verworfen wird. Da es sich um eine Verwaltungs Klasse handelt, kann Sie über den Ansichts Controller als eine übergeordnete Klasse angesehen werden, da Sie auf Geräteänderungen reagiert, die sich auf den Ansichts Controller auswirken (z. b. die Ausrichtung), die dann wieder in den Ansichts Controller der Präsentations Controller-Steuerelemente

Wenn der Entwickler mithilfe der `PresentViewController` -Methode einen Ansichts Controller anzeigt, wird die Verwaltung des Präsentations Prozesses an `UIKit`übergeben. UIKit verarbeitet (unter anderem) den korrekten Controller für den Stil, der erstellt wird, wobei die einzige Ausnahme darin besteht, dass der Stil eines Ansichts `UIModalPresentationCustom`Controllers auf festgelegt ist. Hier kann die Anwendung ihren eigenen presentationcontroller bereitstellen, anstatt den `UIKit` Controller zu verwenden.

### <a name="custom-presentation-styles"></a>Benutzerdefinierte Präsentations Stile

Mit einem benutzerdefinierten Präsentationsstil haben Entwickler die Möglichkeit, einen benutzerdefinierten Präsentations Controller zu verwenden. Dieser benutzerdefinierte Controller kann verwendet werden, um die Darstellung und das Verhalten der Ansicht zu ändern, mit der er verbunden ist.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Arbeiten mit Größenklassen

Das xamarin-Projekt für Adaptive Fotos, das in diesem Artikel enthalten ist, enthält ein funktionierendes Beispiel für die Verwendung von Größenklassen und adaptiven Ansichts Controllern in einer IOS 8 Unified Interface-Anwendung.

Obwohl die Anwendung die Benutzeroberfläche vollständig aus dem Code erstellt, werden die gleichen Techniken angewendet, anstatt den IOS-Designer zu verwenden und ein einheitliches Storyboard zu erstellen. Weiter unten in diesem Artikel erfahren Sie, wie Sie Größenklassen mit einem vereinheitlichten Storyboard und dem IOS-Designer in einer xamarin-Anwendung verwenden.

Sehen wir uns nun genauer an, wie das Projekt für Adaptive Fotos mehrere der Größenklassen Features in ios 8 implementiert, um eine Adaptive Anwendung zu erstellen.

### <a name="adapting-to-trait-environment-changes"></a>Anpassen an Merkmals Umgebungs Änderungen

Beim Ausführen der Anwendung für Adaptive Fotos auf einem iPhone zeigt der Split View Controller sowohl die Master-als auch die Detailansicht an, wenn der Benutzer das Gerät vom Hochformat in das Querformat dreht:

 [![](unified-storyboards-images/rotation.png "Der geteilte Ansichts Controller zeigt die Master-und Detailansicht an, wie hier zu sehen ist.")](unified-storyboards-images/rotation.png#lightbox)

Dies wird erreicht, indem die `UpdateConstraintsForTraitCollection` -Methode des Ansichts Controllers überschrieben und die Einschränkungen basierend auf dem Wert `VerticalSizeClass`von angepasst werden. Beispiel:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Hinzufügen von Übergangs Animationen

Wenn der Split View Controller in der Adaptive Photos-Anwendung von reduziert zu erweitert wechselt, werden den Standard Animationen Animationen hinzugefügt, indem `WillTransitionToTraitCollection` die-Methode des View-Controllers außer Kraft gesetzt wird. Beispiel:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Überschreiben der Merkmals Umgebung

Wie oben gezeigt, zwingt die Anwendung Adaptive Photos den Split View Controller, die Details und die Master Ansichten anzuzeigen, wenn sich das iPhone-Gerät in der Querformat Ansicht befindet.

Dies wurde mithilfe des folgenden Codes im Ansichts Controller erreicht:

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Erweitern und reduzieren des geteilten Ansichts Controllers

Nun sehen wir uns an, wie das erweiternde und reduzierende Verhalten des Split View Controller in xamarin implementiert wurde. Wenn in `AppDelegate`der Split View Controller erstellt wird, wird der zugehörige Delegat zugewiesen, um diese Änderungen zu behandeln:

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

Die `SeparateSecondaryViewController` -Methode testet, ob ein Foto angezeigt wird, und führt auf der Grundlage dieses Zustands eine Aktion aus. Wenn kein Foto angezeigt wird, reduziert es den sekundären Ansichts Controller, sodass der Master Ansichts Controller angezeigt wird.

Die `CollapseSecondViewController` -Methode wird verwendet, wenn der Split View Controller erweitert wird, um zu überprüfen, ob Fotos auf dem Stapel vorhanden sind, wenn dies der Fall ist.

### <a name="moving-between-view-controllers"></a>Wechseln zwischen Ansichts Controllern

Als nächstes sehen wir uns an, wie die Anwendung für Adaptive Fotos zwischen Ansichts Controllern verschoben wird. Wenn der `AAPLConversationViewController` Benutzer in der-Klasse eine Zelle aus der Tabelle auswählt `ShowDetailViewController` , wird die-Methode aufgerufen, um die Detailansicht anzuzeigen:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Anzeigen von Offenlegungs Indikatoren

In der adaptiven Foto Anwendung gibt es mehrere Stellen, an denen Offenlegungs Indikatoren auf der Grundlage von Änderungen an der Merkmals Umgebung ausgeblendet oder angezeigt werden. Dies wird mit folgendem Code behandelt:

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Diese werden mithilfe der `GetTargetViewControllerForAction` oben beschriebenen Methode implementiert.

Wenn ein Tabellen Ansichts Controller Daten anzeigt, verwendet er die oben implementierten Methoden, um zu überprüfen, ob ein Push ausgeführt wird oder nicht, und ob der Offenlegungs Indikator entsprechend angezeigt oder ausgeblendet werden soll:

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Neuer `ShowDetailTargetDidChangeNotification` Typ

Apple hat einen neuen Benachrichtigungstyp zum Arbeiten mit Größenklassen und Merkmals Umgebungen in einem Split View-Controller `ShowDetailTargetDidChangeNotification`hinzugefügt. Diese Benachrichtigung wird immer dann gesendet, wenn sich die Ziel Detail Ansicht für einen geteilten Ansichts Controller ändert, z. b. wenn der Controller erweitert oder reduziert wird.

Die Anwendung für Adaptive Fotos verwendet diese Benachrichtigung, um den Status des Offenlegungs Indikators zu aktualisieren, wenn der Detail Ansichts Controller geändert wird:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Sehen Sie sich die Anwendung Adaptive Fotos genauer an, um alle Möglichkeiten anzuzeigen, mit denen Größenklassen, Merkmals Auflistungen und adaptiver Ansichts Controller verwendet werden können, um problemlos eine einheitliche Anwendung in xamarin. IOS zu erstellen.

## <a name="unified-storyboards"></a>Einheitliche Storyboards

Neu in ios 8: Unified Storyboards ermöglichen es dem Entwickler, eine einheitliche Storyboard-Datei zu erstellen, die auf iPhone-und iPad-Geräten durch das Ziel mehrerer Größenklassen angezeigt werden kann. Durch die Verwendung von Unified Storyboards schreibt der Entwickler weniger UI-spezifischen Code und verfügt nur über ein Schnittstellendesign, das erstellt und gewartet werden kann.

Die wichtigsten Vorteile von Unified Storyboards sind:

-  Verwenden Sie dieselbe storyboarddatei für iPhone und iPad.
-  Stellen Sie eine rückwärts Bereitstellung auf IOS 6 und IOS 7 bereit.
-  Vorschau des Layouts für verschiedene Geräte, Ausrichtungen und Betriebssystemversionen innerhalb des xamarin IOS-Designers.

Diese Funktion wird in Visual Studio für Mac vollständig unterstützt.

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Aktivieren von Größenklassen

Standardmäßig werden in jedem neuen xamarin. IOS-Projektgrößen Klassen verwendet. Zum Verwenden von Größenklassen und adaptiven Segues innerhalb eines Storyboards aus einem älteren Projekt muss es zuerst im IOS-Designer in das vereinheitlichte Xcode 6-Storyboard-Format konvertiert werden.

Öffnen Sie hierzu das Storyboard, das in den IOS-Designer konvertiert werden soll, und aktivieren Sie das Kontrollkästchen **use size Classes** :

 [![](unified-storyboards-images/sizeclass01.png "Das Kontrollkästchen \"Größenklassen verwenden\"")](unified-storyboards-images/sizeclass01.png#lightbox)

Der IOS-Designer bestätigt, dass der Entwickler das Format des Storyboards für die Verwendung von Größenklassen konvertieren möchte:

 [![](unified-storyboards-images/sizeclass02.png "Die Warnung \"Größenklassen verwenden\"")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Das automatische Layout muss auch darauf geprüft werden, dass Größenklassen ordnungsgemäß funktionieren.

### <a name="generic-device-types"></a>Generische Gerätetypen

Nachdem das Storyboard in die Verwendung von Größenklassen konvertiert wurde, wird es in der Designoberfläche erneut angezeigt, und die **Ansicht als** Gerät ist generisch:

 [![](unified-storyboards-images/sizeclass03.png "Als generischen Gerätetyp anzeigen")](unified-storyboards-images/sizeclass03.png#lightbox)

Wenn der generische Gerätetyp ausgewählt ist, werden alle Ansichts Controller in ein 600 x 600-Quadrat geändert. Dieses Quadrat stellt die Größen beliebiger Breite und beliebiger Höhe dar. Wenn sich der IOS-Designer in diesem Modus befindet, werden alle bearbeitbaren Änderungen auf alle Größenklassen angewendet.

Der Entwickler hat auch die Möglichkeit, die Entwurfs Oberfläche als iPhone anzuzeigen:

 [![](unified-storyboards-images/sizeclass04.png "Anzeigen der Entwurfs Oberfläche als iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Oder als iPad anzeigen:

 [![](unified-storyboards-images/sizeclass05.png "Anzeigen der Entwurfs Oberfläche als iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Size-Klasse auswählen

Die Auswahl Schaltfläche "Größenklasse" befindet sich in der linken oberen Ecke des Designoberfläche (in der Nähe der Dropdown Liste "Ansicht"). Sie ermöglicht es dem Entwickler, auszuwählen, welche Größenklassen aktuell bearbeitet werden:

 [![](unified-storyboards-images/sizeclass06.png "Size-Klasse auswählen")](unified-storyboards-images/sizeclass06.png#lightbox)

Der Selektor zeigt die Auswahl der Größenklasse als 3 x 3-Raster an. Jedes der Quadrate im Raster stellt eine Kombination aus einer Width-Klasse und einer Height-Klasse dar. Das mittlere Quadrat wählt die Klasse beliebige Breite/beliebige Höhen Größe aus (Dies ist die Standardansicht für ein einheitliches Storyboard). Wenn dieses Quadrat ausgewählt ist, bearbeitet der Entwickler das Standardlayout, das von allen anderen Konfigurationen geerbt wird.

Das Quadrat in der oberen linken Ecke des Rasters stellt die Klasse Compact Width/Compact Height size dar:

 [![](unified-storyboards-images/sizeclass07.png "Die Compact Width/Compact Height size-Klasse")](unified-storyboards-images/sizeclass07.png#lightbox)

Dieser Modus entspricht einem iPhone in der Querformat Ausrichtung. Das Quadrat in der unteren rechten Ecke des Rasters stellt die reguläre Width/Regular Height size-Klasse dar, die ein iPad darstellt:

 [![](unified-storyboards-images/sizeclass08.png "Die reguläre Width/Regular Height size-Klasse.")](unified-storyboards-images/sizeclass08.png#lightbox)

Um das Layout für ein iPhone im Hochformat zu bearbeiten, wählen Sie das Quadrat in der unteren linken Ecke aus. Dies stellt die Klasse "Compact Width/Regular Height size" dar:

 [![](unified-storyboards-images/sizeclass09.png "Die Klasse \"Compact Width/Regular Height size\"")](unified-storyboards-images/sizeclass09.png#lightbox)

Klicken Sie in das Quadrat, um es auszuwählen, und die Designoberfläche die Größe der Ansichts Controller so ändern, dass Sie der neuen Auswahl entspricht:

 [![](unified-storyboards-images/sizeclass10.png "Der Designoberfläche ändert die Größe der Ansichts Controller so, dass Sie der neuen Auswahl entspricht, wie hier gezeigt.")](unified-storyboards-images/sizeclass10.png#lightbox)

Weitere Informationen zu Größenklassen und deren Auswirkungen auf das Layout von iPhones und iPads finden Sie im Abschnitt Größenklasse dieses Artikels.

### <a name="adaptive-segue-types"></a>Adaptive Enumerationstypen

Wenn der Entwickler bereits Storyboards verwendet hat, werden Sie mit den vorhandenen segue-Typen **Push**, **Modal** und **popover**vertraut sein. Wenn Größenklassen für eine einheitliche storyboarddatei aktiviert werden, werden die folgenden adaptiven * Typen (die der oben beschriebenen neuen View Controller-API entsprechen) verfügbar gemacht: **Anzeigen** und **Anzeigen von Details**.

> [!IMPORTANT]
> Wenn Größenklassen aktiviert sind, werden alle vorhandenen-Enumerationstypen in die neuen Typen konvertiert.

Sehen Sie sich das Beispiel einer IOS 8-Anwendung an, die ein einheitliches Storyboard mit einem Split View Controller verwendet, der über ein einfaches Spiel Navigationsmenü in der Master Ansicht verfügt. Wenn der Benutzer auf eine Menü Schaltfläche klickt, sollte der Ansichts Controller des ausgewählten Elements im Detail Abschnitt des Split View Controller angezeigt werden, wenn er auf einem iPad ausgeführt wird. Auf einem iPhone sollte der Ansichts Controller des Elements auf den Navigations Stapel geschoben werden.

Um diesen Effekt zu erzielen, klicken Sie im IOS-Designer-Steuerelement auf die Schaltfläche, und ziehen Sie eine Linie zum Ansichts Controller, die angezeigt werden soll. Wenn die Maustaste losgelassen wird, wählen Sie `Show Detail` im Popup Menü segue-Typ aus:

 [![](unified-storyboards-images/segue01.png "Wählen Sie im popuppopup-Popup Menü die Option Details anzeigen aus.")](unified-storyboards-images/segue01.png#lightbox)

Der neue seegue wird zwischen der Schaltfläche und dem Ansichts Controller erstellt. Führen Sie die Anwendung jetzt im iPhone-Simulator aus, und das Hauptmenü wird angezeigt:

 [![](unified-storyboards-images/segue02.png "Das Hauptmenü")](unified-storyboards-images/segue02.png#lightbox)

Klicken Sie auf die Schaltfläche " **Spiel auswählen** ", und der Ansichts Controller des Elements wird auf dem Navigations Stapel abgelegt:

 [![](unified-storyboards-images/segue03.png "Der Ansichts Controller der Elemente wird wie hier dargestellt auf den Navigations Stapel verschoben.")](unified-storyboards-images/segue03.png#lightbox)

Starten Sie den iPhone-Simulator, und führen Sie die Anwendung im iPad-Simulator aus. Wechseln Sie zum Querformat, und das Hauptmenü wird erneut angezeigt:

 [![](unified-storyboards-images/segue04.png "Das Hauptmenü wird angezeigt.")](unified-storyboards-images/segue04.png#lightbox)

Klicken Sie erneut auf die Schaltfläche " **Spiel auswählen** ", und der Ansichts Controller des Elements wird im Abschnitt "Details" des Controllers "Split View Controller" angezeigt:

 [![](unified-storyboards-images/segue05.png "Der Element Ansichts Controller, der im Detail Abschnitt des geteilten Ansichts Controllers angezeigt wird.")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Ausschließen eines Elements aus einer Size-Klasse

Es gibt Zeiten, in denen ein angegebenes Element (z. b. eine Sicht, ein Steuerelement oder eine Einschränkung) nicht innerhalb einer bestimmten Größenklasse erforderlich ist. Um ein Element aus einer Größenklasse auszuschließen, wählen Sie das gewünschte Element aus, das im **Designoberfläche**ausgeschlossen werden soll. Scrollen Sie zum unteren Rand des **Eigenschaften-Explorers** , und klicken Sie auf das Dropdown Menü " **Zahnrad** ". Wählen Sie die Kombination aus **Breite** und **Höhe** aus, von der das Element ausgeschlossen werden soll:

[![](unified-storyboards-images/exclude-a.png "Kombination aus Breite und Höhe auswählen")](unified-storyboards-images/exclude-a.png#lightbox)

Dem Element unten im **Eigenschaften-Explorer**wird ein neuer *Ausschluss Fall* hinzugefügt. Deaktivieren Sie als nächstes das Kontrollkästchen **installiert** für die angegebene size-Klasse:

[![](unified-storyboards-images/exclude-b.png "Deaktivieren Sie das Kontrollkästchen installiert.")](unified-storyboards-images/exclude-b.png#lightbox)

Wechseln Sie die Designoberfläche zu der Breite und Höhe, von der das Element ausgeschlossen wurde. es wurde aus der angegebenen Größenklasse entfernt, jedoch nicht mit dem gesamten Benutzeroberflächen Entwurf:

 [![](unified-storyboards-images/exclude02.png "Die Designoberfläche wird in die Breite und Höhe gewechselt, von der das Element ausgeschlossen wurde.")](unified-storyboards-images/exclude02.png#lightbox)

Wechseln zurück zur beliebigen Width/any Height size-Klasse, und das-Element ist noch vorhanden:

 [![](unified-storyboards-images/exclude03.png "Zurück wechseln zur beliebigen Width/any Height size-Klasse")](unified-storyboards-images/exclude03.png#lightbox)

Wenn die Anwendung im iPad-Simulator ausgeführt wird, wird das-Element angezeigt:

 [![](unified-storyboards-images/exclude04.png "Das Element, das angezeigt wird, wenn die laufende App im iPad-Simulator")](unified-storyboards-images/exclude04.png#lightbox)

Wenn die Anwendung auf dem iPhone-Simulator ausgeführt wird, fehlt das-Element:

 [![](unified-storyboards-images/exclude05.png "Das Element, das bei der Ausführung der APP im iPhone-Simulator fehlt.")](unified-storyboards-images/exclude05.png#lightbox)

Um einen Ausschluss Fall aus einem Element zu entfernen, wählen Sie einfach das Element im **Designoberfläche**aus, Scrollen Sie zum unteren Rand des **Eigenschaften-Explorers** , und klicken Sie auf die **-** Schaltfläche neben dem Fall, den Sie entfernen möchten.

Eine Implementierung von Unified Storyboards finden Sie in der xamarin IOS 8-Beispielanwendung, die `UnifiedStoryboard` an dieses Dokument angefügt ist.

## <a name="dynamic-launch-screens"></a>Dynamische Startbildschirme

Die Startbildschirm Datei wird als Begrüßungsbildschirm angezeigt, während eine IOS-Anwendung gestartet wird, um dem Benutzer Feedback bereitzustellen, dass die APP tatsächlich gestartet wird. Vor IOS 8 musste der Entwickler mehrere `Default.png` Abbild Assets für jeden Gerätetyp, jede Ausrichtung und Bildschirmauflösung einschließen, auf dem die Anwendung ausgeführt wird. Beispiel `Default@2x.png` :,`Default-Portrait@2x~ipad.png`,usw. `Default-Landscape@2x~ipad.png`

Durch die Umgestaltung der neuen iPhone 6-und iPhone 6-und-Geräte (und der bevorstehenden Apple Watch) mit allen vorhandenen iPhone-und iPad-Geräten stellt dies ein großes Array von unterschiedlichen Größen `Default.png` , Ausrichtungen und Auflösungen von Image Ressourcen für das Startbildschirm dar, das erstellt und verwaltet werden. Darüber hinaus können diese Dateien recht groß sein und das lieferbare Anwendungs Bündel "Bloat", wodurch die erforderliche Zeit für das Herunterladen der Anwendung aus dem iTunes App Store erhöht wird (möglicherweise, damit Sie nicht mehr über ein Mobilfunknetz geliefert werden kann). und erhöhen Sie die Menge an Speicherplatz, die auf dem Gerät des Endbenutzers benötigt wird.

Neu bei IOS 8: Entwickler können eine einzelne atomarische `.xib` Datei in Xcode erstellen, die Auto Layout-und Größenklassen verwendet, um einen *dynamischen Startbildschirm* zu erstellen, der für jedes Gerät, jede Auflösung und jede Ausrichtung funktioniert. Dadurch wird nicht nur der Arbeitsaufwand reduziert, den der Entwickler benötigt, um alle erforderlichen Image Ressourcen zu erstellen und zu verwalten, sondern die Größe des installierten Pakets der Anwendung erheblich reduziert.


Dynamische Startbildschirme haben folgende Einschränkungen und Überlegungen:

- Verwenden Sie `UIKit` nur Klassen.
- Verwenden Sie eine einzelne Stamm Ansicht, die `UIView` ein `UIViewController` -oder-Objekt ist.
- Stellen Sie keine Verbindungen mit dem Anwendungscode her (fügen Sie keine **Aktionen** oder **Outlets**hinzu).
- Fügen Sie `UIWebView` keine Objekte hinzu.
- Verwenden Sie keine benutzerdefinierten Klassen.
- Verwenden Sie keine Lauf Zeit Attribute.

Beachten Sie bei den obigen Richtlinien, wie Sie einem vorhandenen xamarin IOS 8-Projekt einen dynamischen Startbildschirm hinzufügen.

Führen Sie folgende Schritte aus:

1. Öffnen Sie **Visual Studio für Mac** , und laden Sie die Projekt **Mappe, um** den dynamischen Startbildschirm hinzuzufügen.
2. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste `MainStoryboard.storyboard` auf die Datei, und wählen Sie **mit** > **Xcode Interface Builder**öffnen:

    [![](unified-storyboards-images/dls01.png "Öffnen mit Xcode Interface Builder")](unified-storyboards-images/dls01.png#lightbox)
3. Wählen Sie in Xcode die Option **Datei** > **neue** > **Datei...** aus:

    [![](unified-storyboards-images/dls02.png "Datei auswählen/neu")](unified-storyboards-images/dls02.png#lightbox)
4. Wählen Sie **IOS** > -**Benutzeroberfläche** > **starten** aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![](unified-storyboards-images/dls03.png "Wählen Sie IOS/Benutzeroberfläche/Startbildschirm aus.")](unified-storyboards-images/dls03.png#lightbox)
5. Benennen Sie die `LaunchScreen.xib` Datei, und klicken Sie auf die Schaltfläche **Erstellen** :

    [![](unified-storyboards-images/dls04.png "Benennen Sie die Datei \"launchscreen. XIb\".")](unified-storyboards-images/dls04.png#lightbox)
6. Bearbeiten Sie das Design des Startbildschirms, indem Sie grafische Elemente hinzufügen und Layouteinschränkungen verwenden, um Sie für die angegebenen Geräte, Ausrichtungen und Bildschirmgrößen zu positionieren:

    [![](unified-storyboards-images/dls05.png "Bearbeiten des Entwurfs des Startbildschirms")](unified-storyboards-images/dls05.png#lightbox)
7. Speichern Sie die Änderungen `LaunchScreen.xib`in.
8. Wählen Sie das **Anwendungs Ziel** und die Registerkarte **Allgemein** aus:

    [![](unified-storyboards-images/dls06.png "Wählen Sie das Anwendungs Ziel und die Registerkarte Allgemein aus.")](unified-storyboards-images/dls06.png#lightbox)
9. Klicken Sie auf die Schaltfläche **Info. plist auswählen** , wählen Sie `Info.plist` für die xamarin-App aus, und klicken Sie auf die Schaltfläche **auswählen** :

    [![](unified-storyboards-images/dls07.png "Wählen Sie die Datei \"Info. plist\" für die xamarin-app.")](unified-storyboards-images/dls07.png#lightbox)
10. Öffnen Sie im Abschnitt **App-Symbole und Start Bilder** die Dropdown Liste **Startbildschirm Datei** , und `LaunchScreen.xib` wählen Sie die oben erstellte aus:

    [![](unified-storyboards-images/dls08.png "Wählen Sie den launchscreen. XIb aus.")](unified-storyboards-images/dls08.png#lightbox)
11. Speichern Sie die Änderungen in der Datei, und kehren Sie zu Visual Studio für Mac zurück.
12. Warten Sie, bis Visual Studio für Mac die Synchronisierung von Änderungen mit Xcode abgeschlossen haben.
13. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den **Ressourcen** Ordner, und wählen Sie hinzufügen Dateien hinzu **fügen** >  **...** aus:

    [![](unified-storyboards-images/dls09.png "Wählen Sie Dateien hinzufügen/hinzufügen aus.")](unified-storyboards-images/dls09.png#lightbox)
14. Wählen Sie `LaunchScreen.xib` die oben erstellte Datei aus, und klicken Sie auf **Öffnen** :

    [![](unified-storyboards-images/dls10.png "Wählen Sie die Datei launchscreen. XIb aus.")](unified-storyboards-images/dls10.png#lightbox)
15. Erstellen Sie die Anwendung.

### <a name="testing-the-dynamic-launch-screen"></a>Testen des dynamischen Startbildschirms

Wählen Sie in Visual Studio für Mac den iPhone 4 Retina-Simulator aus, und führen Sie die Anwendung aus. Der dynamische Startbildschirm wird im richtigen Format und in der richtigen Ausrichtung angezeigt:

[![](unified-storyboards-images/dls11.png "Der dynamische Startbildschirm, der in vertikaler Ausrichtung angezeigt wird.")](unified-storyboards-images/dls11.png#lightbox)

Beenden Sie die Anwendung in Visual Studio für Mac, und wählen Sie ein iPad IOS 8-Gerät aus. Führen Sie die Anwendung aus, und der Startbildschirm wird ordnungsgemäß für dieses Gerät und die folgende Ausrichtung formatiert:

[![](unified-storyboards-images/dls12.png "Der dynamische Startbildschirm, der in horizontaler Ausrichtung angezeigt wird.")](unified-storyboards-images/dls12.png#lightbox)

Kehren Sie zu Visual Studio für Mac zurück, und verhindern Sie die Ausführung der Anwendung.

### <a name="working-with-ios-7"></a>Arbeiten mit IOS 7

Um die Abwärtskompatibilität mit IOS 7 aufrechtzuerhalten, nehmen Sie einfach `Default.png` die üblichen Image Ressourcen in die IOS 8-Anwendung auf. IOS kehrt zum vorherigen Verhalten zurück und verwendet diese Dateien als Startbildschirm, wenn es auf einem IOS 7-Gerät ausgeführt wird.

Um eine Implementierung eines dynamischen Startbildschirms in xamarin anzuzeigen, sehen Sie sich die in diesem Dokument angefügte IOS 8-Beispielanwendung für [dynamische Startbildschirme](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen) an.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie einen kurzen Blick auf Größenklassen und deren Auswirkungen auf das Layout auf iPhone-und iPad-Geräten genommen. Es wurde erläutert, wie Merkmale, Merkmals Umgebungen und Merkmals Auflistungen mit Größenklassen zum Erstellen einheitlicher Schnittstellen funktionieren. Es wurde eine kurze Betrachtung der adaptiven Ansichts Controller und der Funktionsweise von Größenklassen innerhalb einheitlicher Schnittstellen durchgearbeitet. Dabei wurde die Implementierung von Größenklassen und vereinheitlichten Schnitt C# stellen vollständig aus Code in einer xamarin IOS 8-Anwendung untersucht.

Schließlich wurden in diesem Artikel die Grundlagen der Erstellung vereinheitlichter Storyboards mit dem xamarin IOS-Designer behandelt, der über IOS-Geräte hinweg funktioniert, und es wird ein einzelner dynamischer Startbildschirm erstellt, der auf jedem IOS 8-Gerät als Startbildschirm angezeigt wird.

## <a name="related-links"></a>Verwandte Links

- [Adaptive Fotos (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-adaptivephotos)
- [Dynamische Startbildschirme (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Dynamische Layouts in iOS8-Evolve 2014 (Video)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
