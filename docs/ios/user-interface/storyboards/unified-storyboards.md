---
title: Einheitliche Storyboards in Xamarin.iOS
description: Dieses Dokument beschreibt die einheitliche Storyboards in Xamarin.iOS. Einheitliche Storyboards ermöglichen Entwicklern das zur Unterstützung mehrerer Bildschirmgrößen mit einer einzelnen Schnittstelle-Definition.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 6d3324a6485f2d240ec339f6ce7f03aafe51c80c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792020"
---
# <a name="unified-storyboards-in-xamarinios"></a>Einheitliche Storyboards in Xamarin.iOS

iOS 8 umfasst einen neuen, einfacher zu verwendende Mechanismus zum Erstellen der Benutzeroberfläche – das einheitliche Storyboard. Mit einem einzelnen Storyboard aller der unterschiedliche Hardware Bildschirmgrößen abdecken, schnelle und reaktionsschnelle Sichten erstellt werden können, einer "Design-einmal, Verwendung n-" Stil.

Wie der Entwickler zum Erstellen eines separaten und bestimmten Storyboards für iPhone und iPad-Geräte nicht mehr benötigt, haben sie die Flexibilität zum Entwerfen von Anwendungen mit eine gemeinsame Schnittstelle, und passen Sie diese Schnittstelle für anderen Größenklassen. Auf diese Weise kann eine Anwendung an die Stärken der einzelnen Formfaktor angepasst werden, und jede Benutzeroberfläche kann optimiert werden, um optimale Ergebnisse zu gewährleisten.

<a name="size-classes" />

## <a name="size-classes"></a>Größenklassen

Bevor Sie iOS 8, den der Entwickler verwendet `UIInterfaceOrientation` und `UIInterfaceIdiom` zur Unterscheidung zwischen Hoch-und Querformat und iPhone und iPad-Geräte. In iOS8, Ausrichtung und Gerät bestimmt ist, indem *Größenklassen*.

Geräte werden in horizontalen und vertikalen Achsen Größenklassen definiert, und es gibt zwei Arten von Größenklassen in iOS 8:

-  **Reguläre** – Dies ist für eine große Bildschirmgröße (z. B. einem iPad) oder ein Gadget, die den Eindruck von eine beträchtliche Größe bietet (z. B. ein `UIScrollView`
-  **Compact** – Dies ist für kleinere Geräte (z. B. eine iPhone). Diese Größe berücksichtigt die Ausrichtung des Geräts.


Wenn die beiden Konzepte zusammen verwendet werden, ist das Ergebnis ein 2 x 2-Raster mit den definiert die verschiedenen möglichen Größen, die in den beiden unterschiedlichen Ausrichtungen verwendet werden können, wie im folgenden Diagramm dargestellt:

 [![](unified-storyboards-images/sizeclassgrid.png "Ein 2 x 2-Raster, die die verschiedenen möglichen Größen, die verwendet werden, können im regulären und Compact Ausrichtungen definiert")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Der Entwickler kann eine View-Controller erstellen, der einen der vier Möglichkeiten, die in verschiedenen Layouts bedingt verwendet (wie in der oben dargestellt).

### <a name="ipad-size-classes"></a>iPad Größe-Klassen

IPad, aufgrund der Datenbankgröße hat eine **reguläre** Größe für beide Ausrichtungen Klasse.

 [![](unified-storyboards-images/image1.png "iPad Größe-Klassen")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone Größe-Klassen

Das iPhone verfügt über andere Größe-Klassen basierend auf die Ausrichtung des Geräts:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone Größe-Klassen")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Wenn das Gerät im Hochformat ist, wird der Bildschirm enthält eine **compact** -Klasse horizontal und **reguläre** vertikal
-  Wenn das Gerät im Querformat ist, werden der Bildschirm-Klassen aus Hochformat storniert.

### <a name="iphone-6-plus-size-classes"></a>Klassen für iPhone 6 Plus Größe

Die Größen sind identisch mit der früheren iPhones im Hochformat, jedoch nicht im Querformat:

[![](unified-storyboards-images/iphone6sizeclasses.png "Klassen für iPhone 6 Plus Größe")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Da das iPhone 6 Plus einen ausreichend großen Bildschirm besitzt, ist es für eine normale Breite Größe Klasse im Querformat Modus verfügbar.

### <a name="support-for-a-new-screen-scale"></a>Unterstützung für einen neuen Bildschirm Skalierung

Das iPhone 6 Plus ein neue HD Retina-Display mit einem Bildschirm Skalierungsfaktor von 3.0 (dreimal so groß wie die ursprüngliche iPhone Bildschirmauflösung) verwendet. Um die bestmögliche Leistung auf diesen Geräten bereitstellen, schließen Sie neue Vorlage für diese Skalierung Bildschirm entworfen ein In Xcode 6 und höher, Asset können Kataloge Bilder auf 1 X, X 2 und 3 X Größen sind: Fügen Sie einfach die neue Bildanlagen und iOS werden, wählen Sie die richtigen Objekte bei Ausführung auf einem iPhone 6 Plus.

Das Bild auch loading-Verhaltens in iOS erkennt ein `@3x` Suffix auf Bilddateien. Wenn der Entwickler ein Standardimage-Medienobjekt (bei einer anderen Auflösung) in das Anwendungspaket mit der folgenden Dateinamen enthält z. B.: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, und `MonkeyIcon@3x.png`. Auf dem iPhone 6 Plus dem `MonkeyIcon@3x.png` Image wird automatisch verwendet, wenn der Entwickler ein Image mithilfe des folgenden Codes lädt:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Oder wenn sie das Abbild ein Benutzeroberflächenelement zuweisen, mit der iOS-Designer als `MonkeyIcon.png`, die `MonkeyIcon@3x.png` verwendet werden, automatisch wieder auf dem iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Dynamische Startbildschirme

Während eine iOS-Anwendung wird gestartet, zum Bereitstellen von Feedback für den Benutzer, die die app tatsächlich Starten von wird der Bildschirm-Startdatei als Begrüßungsbildschirm angezeigt. Bevor Sie iOS 8, der Entwickler müssten Multiple-include- `Default.png` image Bestände für jedes Gerät Typ, Ausrichtung und die Bildschirmauflösung, die die Anwendung ausgeführt werden würde.

Neu für iOS 8, kann der Entwickler erstellen eine einzelne, das atomarische `.xib` Datei in Xcode, mit denen Automatisches Layout und Klassen Größe erstellt eine *dynamische starten Bildschirm* , die für jedes Gerät, Auflösung und Ausrichtung funktioniert. Dies verringert nicht nur der Arbeitsaufwand, der Entwickler erstellen und verwalten alle erforderlichen Bildanlagen, verringert jedoch auch die Größe des installierten Anwendungspaket.

## <a name="traits"></a>Merkmale

Merkmale sind Eigenschaften, die verwendet werden können, um zu bestimmen, wie ein Layout ändert, wenn sich die anwendungsumgebung ändert. Sie bestehen aus einem Satz von Eigenschaften (die `HorizontalSizeClass` und `VerticalSizeClass` basierend auf `UIUserInterfaceSizeClass`), sowie die Schnittstelle Ausdrucksweise ( `UIUserInterfaceIdiom`) und die Anzeige-Skalierung.

Alle vorherigen Zustände werden eingeschlossen in einen Container, auf den Apple als Merkmal Auflistung verweist ( `UITraitCollection`), die nicht nur die Eigenschaften, sondern auch deren Werte enthält.

## <a name="trait-environment"></a>Merkmal ""-Umgebung

Merkmals Umgebungen sind eine neue Schnittstelle in iOS 8 und können zum Zurückgeben einer Auflistung Merkmal für die folgenden Objekte:

-  Bildschirme ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Anzeigen von Domänencontrollern ( `UIViewController` ).
-  Sichten ( `UIView` ).
-  Präsentation Controller ( `UIPresentationController` ).


Der Entwickler mithilfe von einer Umgebung Merkmals zurückgegebene Auflistung Merkmal "" um zu bestimmen, wie eine Benutzeroberfläche verteilt werden soll.

Alle Merkmal ""-Umgebungen stellen eine Hierarchie, wie im folgenden Diagramm dargestellt:

 [![](unified-storyboards-images/viewhierarchy.png "Hierarchiediagramm Merkmals Umgebungen")](unified-storyboards-images/viewhierarchy.png#lightbox)

Das Merkmal ""-Auflistung, die von den oben genannten Merkmals Umgebungen aufweisen fließt, wird standardmäßig vom übergeordneten Element an die untergeordneten-Umgebung.

Zusätzlich zum Abrufen der aktuellen Merkmal ""-Auflistung, die Merkmals-Umgebung verfügt über eine `TraitCollectionDidChange` -Methode, die in der Sicht oder View Controller-Unterklassen überschrieben werden kann. Der Entwickler kann diese Methode verwenden, die Elemente der Benutzeroberfläche ändern, von denen Traits abhängen, wenn diese Merkmale geändert haben.

## <a name="typical-trait-collections"></a>Merkmal "Regel" Sammlungen

In diesem Abschnitt werden die normalen Typen von Merkmals Sammlungen behandelt, die der Benutzer beim Arbeiten mit iOS 8 tun würden.

Im folgenden finden eine typische Merkmal ""-Auflistung, die der Entwickler möglicherweise auf einem iPhone angezeigt:

|Eigenschaft|Wert|
|--- |--- |
|`HorizontalSizeClass`|Komprimieren|
|`VerticalSizeClass`|Regulär|
|`UserInterfaceIdom`|Telefon|
|`DisplayScale`|2.0|

Die oben genannten Satz würden eine vollständig qualifizierte Merkmals Sammlung darstellen, wie sie Werte für alle zugehörigen Eigenschaften Merkmal "" enthält.

Es ist auch möglich, ein Merkmal Sammlung verfügen, die einige ihrer Werte fehlen (Apple bezieht sich auf als *Unspecified*):

|Eigenschaft|Wert|
|--- |--- |
|`HorizontalSizeClass`|Komprimieren|
|`VerticalSizeClass`|Nicht angegeben.|
|`UserInterfaceIdom`|Nicht angegeben.|
|`DisplayScale`|Nicht angegeben.|

Im Allgemeinen jedoch, wird Wenn der Entwickler der Umgebung Merkmal "" mitfinanziert Merkmals anfordert, es eine vollqualifizierte Auflistung zurückgegeben wie im obigen Beispiel zu sehen.

Ist ein Merkmal ""-Umgebung (z. B. eine Sicht oder View Controller) nicht innerhalb der aktuellen Hierarchie anzeigen, kann der Entwickler nicht angegebene Werte für eine oder mehrere der Eigenschaften Merkmal "" zurückkehren.

Der Entwickler auch eine teilweise gekennzeichnete Merkmals Auflistung abrufen, wenn sie einen der anderen Erstellungsmethoden bereitgestellt von Apple, z. B. `UITraitCollection.FromHorizontalSizeClass`, um eine neue Sammlung zu erstellen.

Ein Vorgang, der für mehrere Merkmals Sammlungen ausgeführt werden kann wird zu "other" vergleichen die umfasst eine Sammlung von Merkmal "" gefragt werden, wenn sie eine andere enthält. Was soll von *Containment* darin, dass für alle Merkmal "" in der zweiten Sammlung angegeben, der Wert genau mit dem Wert in der ersten Sammlung entsprechen muss.

So testen Sie zwei Traits verwenden die `Contains` Methode der `UITraitCollection` durch Übergeben des Werts, der das Merkmal "" die zu testende.

Entwickler kann die Qualität der Vergleiche manuell ausführen im Code, um zu bestimmen, wie Layoutansichten oder View-Controller. Allerdings `UIKit` verwendet Sie diese Methode intern, um einige ihrer Funktionen, wie in der Darstellung-Proxy, z. B. zu ermöglichen.

## <a name="appearance-proxy"></a>Darstellung Proxy

Der Proxy Darstellung wurde in früheren Versionen von iOS verwenden, um Entwicklern das Anpassen der Eigenschaften von ihren Sichten ermöglichen eingeführt. Es wurde in iOS 8 Merkmals Auflistungen unterstützen erweitert.

Darstellung Proxys enthalten nun eine neue Methode `AppearanceForTraitCollection`, einen neuen Proxy für die Darstellung für die angegebene Merkmal ""-Auflistung, die übergebenen zurückgibt. Alle Anpassungen, die der Entwickler, die ausführt werden Darstellung Proxy erst wirksam für Sichten, die entsprechen der angegebenen Auflistung von Merkmal "".

Im Allgemeinen übergibt der Entwickler in einen teilweise angegebenen Merkmal ""-Auflistung, an die `AppearanceForTraitCollection` Methode, z. B. eine, die nur eine horizontale Größe Klasse der komprimiert, angegeben, sodass sie einer beliebigen Ansicht in der Anwendung anpassen können, die horizontal compact ist.

## <a name="uiimage"></a>UIImage

Eine andere Klasse, die Apple-Merkmal zu Auflistung ist hinzugefügt `UIImage`. In der Vergangenheit musste der Entwickler angeben einer @1X und @2x Version einen Bitmap-Graphic Bestand, die sie in der Anwendung (z. B. ein Symbol ") einschließen soll wurden.

iOS 8 wurde erweitert, damit kann den Entwickler mehrere Versionen eines Bilds in einem Image-Katalog auf Grundlage einer Auflistung Merkmals eingeschlossen werden sollen. Der Entwickler kann z. B. ein kleineres Image für die Arbeit mit einer Compact Merkmal ""-Klasse und ein Vollbild Größe für eine andere Sammlung enthalten.

Wenn eines der Bilder innerhalb des verwendet wird eine `UIImageView` -Klasse, die Image-Ansicht wird automatisch die richtige Version des Bilds mitfinanziert Merkmal "" angezeigt. Wenn das Merkmal ""-Umgebung (z. B. dem Benutzer wechseln das Gerät im Hochformat, Querformat) geändert wird, wählt die Image-Sicht automatisch die neue Bildgröße neue Merkmal ""-Sammlung ausgewählt und seine Größe angepasst, die der aktuellen Version des Bilds ändern angezeigt.

## <a name="uiimageasset"></a>UIImageAsset

Apple hat eine neue Klasse für iOS 8 Namens hinzugefügt `UIImageAsset` an dem Entwickler auch mehr Kontrolle über Bildauswahl erhalten.

Ein Standardimage-Medienobjekt schließt alle anderen Versionen eines Bilds und ermöglicht dem Entwickler, die für ein bestimmtes Abbild zu bitten, die eine Auflistung Merkmals entspricht, das im übergeben wurde. Bilder können hinzugefügt oder daraus ein Standardimage-Medienobjekt auf dynamische.

Weitere Informationen zu Bildanlagen, finden Sie in der Apple [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) Dokumentation.

## <a name="combining-trait-collections"></a>Kombinieren von Auflistungen Merkmal ""

Eine andere Funktion, die ein Entwickler Merkmals Auflistungen ausführen können zwei wird zusammengefügt wird dazu führen, in der kombinierten Auflistung, die nicht angegebene Werte aus einer Auflistung, wobei durch die angegebenen Werte in einer zweiten Ausdruck ersetzt werden. Dies erfolgt mithilfe der `FromTraitsFromCollections` Methode der `UITraitCollection` Klasse.

Wie oben angegeben, wenn die Merkmale ist in einer der Sammlungen Merkmal "" nicht angegeben und wird in einer anderen angegeben, wird der Wert der angegebenen Version festgelegt werden. Jedoch wenn es mehrere Versionen eines angegebenen Werts angegeben gibt, den Wert aus der letzten Merkmals Sammlung den Wert werden, der verwendet wird.

## <a name="adaptive-view-controllers"></a>Adaptive View-Controller

Dieser Abschnitt befasst sich mit Details wie die iOS-Ansicht und Controller Ansicht die Konzepte von "Traits" und Größenklassen automatisch in der Entwickler Anwendungen mehr adaptive werden übernommen haben.

### <a name="split-view-controller"></a>Split-View-Controller

Eine View Controller-Klassen, die die meisten in iOS 8 geänderten ist die `UISplitViewController` Klasse. In der Vergangenheit häufig würde der Entwickler einen Split-View-Controller verwenden, in der iPad-Version der Anwendung, und klicken Sie dann müssen sie eine vollständig andere Version von ihrer Hierarchie anzeigen für die iPhone-Version der app bereitstellen.

In iOS 8 die `UISplitViewController` Klasse ist auf beiden Plattformen (iPad und iPhone), kann der Entwickler einer View Controller-Hierarchie zu erstellen, die für iPhone und iPad ordnungsgemäß funktionieren.

Wenn iPhone im Querformat ist, wird der Split-View-Controller seine Ansichten Seite-an-Seite, darstellen, ebenso wie dies der Fall wäre, wenn auf einem iPad angezeigt wird.

### <a name="overriding-traits"></a>Überschreiben der Merkmale

Merkmal ""-Umgebungen, die vom übergeordneten Container zu den untergeordneten Containern, wie die folgende Abbildung zeigt eine Split-View-Controller auf einem iPad im Querformat kaskadiert werden:

 [![](unified-storyboards-images/cascadingclasses01.png "Ein Split-View-Controller auf einem iPad im Querformat")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Da das iPad reguläre Größe Klasse sich auf die horizontale und vertikale Ausrichtung befindet, wird der geteilten Ansicht Ansichten "Master" und "Details angezeigt.

Auf einem iPhone, wobei die Größe-Klasse in beide Ausrichtungen compact ist, zeigt der Split-View-Controller nur der Detailansicht an, wie unten dargestellt:

 [![](unified-storyboards-images/cascadingclasses02.png "Der Split-View-Controller wird nur die Detailansicht angezeigt.")](unified-storyboards-images/cascadingclasses02.png#lightbox)

Der Entwickler muss in einer Anwendung, bei denen der Entwickler sowohl die Master- und Detailtabelle auf einem iPhone im Querformat anzeigen möchte, fügen Sie einen übergeordneten Container der Split-View-Controller und überschreiben die Merkmal ""-Auflistung. Wie in der folgenden Abbildung dargestellt:

 [![](unified-storyboards-images/cascadingclasses03.png "Der Entwickler muss fügen Sie einen übergeordneten Container der Split-View-Controller und überschreiben die Auflistung Merkmal \""")](unified-storyboards-images/cascadingclasses03.png#lightbox)

Ein `UIView` festgelegt ist, als das übergeordnete Element des Split-View-Controller und die `SetOverrideTraitCollection` Methode wird aufgerufen, für die Sicht eine neue Auflistung des Merkmals übergeben und als Ziel der Split-View-Controller. Neue Merkmal ""-Sammlung außer Kraft setzt die `HorizontalSizeClass`, bei der Einstellung `Regular`, damit der Split-View-Controller sowohl die Master- und Detailtabelle Ansichten auf einem iPhone im Querformat angezeigt werden.

Beachten Sie, dass die `VerticalSizeClass` wurde `unspecified`, die im resultierenden kann die neue Merkmals Auflistung der Merkmal ""-Auflistung in das übergeordnete Element hinzugefügt werden eine `Compact VerticalSizeClass` für das untergeordnete Element Split-View-Controller.

### <a name="trait-changes"></a>Änderungen Merkmal ""

In diesem Abschnitt werden sehen, im Detail beschrieben, wie Sammlungen Merkmal "" Übergang erfolgt, wenn das Merkmal ""-Umgebung geändert wird. Beispielsweise bei das Gerät von Hochformat, Querformat gedreht wird.

 [![](unified-storyboards-images/traittransitions01.png "Die Hochformat Querformat Merkmals Änderungen (Übersicht)")](unified-storyboards-images/traittransitions01.png#lightbox)

IOS 8 ist zunächst einige Setup zur Vorbereitung des Übergangs erfolgen. Als Nächstes wird das System die Übergangsstatus animiert. Schließlich bereinigt iOS 8 temporären Zustände, die während des Übergangs erforderlich.

iOS 8 bietet mehrere Rückrufe, mit denen Entwickler kann die Änderung des Merkmals teilnehmen, wie in der folgenden Tabelle dargestellt:

|Phase|Rückruf|Beschreibung|
|--- |--- |--- |
|Setup|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Diese Methode ruft am Anfang eine Änderung des Merkmals aufgerufen, bevor ein Merkmal ""-Auflistung auf den neuen Wert festgelegt wird.</li><li>Die Methode wird aufgerufen, wenn der Wert des Merkmals Auflistung geändert wurde, aber bevor Animationen stattfindet.</li></ul>|
|Animation|`WillTransitionToTraitCollection`|Der Übergang Koordinator, das an diese Methode übergeben wurde ein `AnimateAlongside` Clustereigenschaft, mit der Entwickler Animationen hinzufügen, die zusammen mit die Animationen an Standardeinstellung ausgeführt wird.|
|Leeren von Volltextkatalogen|`WillTransitionToTraitCollection`|Stellt eine Methode für Entwickler, ihre eigenen Bereinigungscode enthalten, nachdem der Übergang stattfindet.|

Die `WillTransitionToTraitCollection` Methode eignet sich hervorragend für die Animation View Controller zusammen mit der Merkmals Auflistung geändert wird. Die `WillTransitionToTraitCollection` Methode ist nur verfügbar für View-Controller ( `UIViewController`) und nicht auf anderen Merkmals Umgebungen wie `UIViews`.

Die `TraitCollectionDidChange` eignet sich hervorragend für das Arbeiten mit der `UIView` -Klasse, bei denen der Entwickler die Benutzeroberfläche zu aktualisieren, die die Merkmale ändern möchte.

### <a name="collapsing-the-split-view-controllers"></a>Reduzieren der Split-View-Controller

Nachdem wir Take genauere Betrachtung was geschieht, wenn es sich bei ein Split-View-Controller einer Ansicht eine Spalte aus einer zweispaltigen reduziert wird. Im Rahmen dieser Änderung gibt es zwei Prozesse, die auftreten, müssen:

-  Standardmäßig wird der Split-View-Controller den Controller die Hauptansicht wie die Sicht verwendet, nach dem Auftreten der reduzieren. Entwickler kann dieses Verhalten überschreiben, indem Sie überschreiben die `GetPrimaryViewControllerForCollapsingSplitViewController` Methode der `UISplitViewControllerDelegate` und über alle View-Controller, die sie im reduzierten Zustand angezeigt werden soll.
-  Sekundäre View-Controller muss in der primären View-Controller zusammengeführt werden. Entwickler müssen im Allgemeinen keine Aktion für diesen Schritt ausführen; Split-View-Controller enthält die automatische Behandlung von dieser Phase basierend auf dem Gerät. Allerdings gibt es möglicherweise einige Sonderfälle, in denen möchte der Entwickler wird für die Interaktion mit dieser Änderung. Aufrufen der `CollapseSecondViewController` Methode der `UISplitViewControllerDelegate` ermöglicht die Masteransicht Controller tritt die Reduzierung, statt die Detailansicht angezeigt werden.


### <a name="expanding-the-split-view-controller"></a>Erweitern die Split-View-Controller

Nachdem wir Take was genauere Betrachtung geschieht, wenn es sich bei einem Split-View-Controller vom Zustand "reduzierter" erweitert wird. Noch einmal: erfolgt in zwei Stufen, die auftreten, müssen:

-  Definieren Sie zuerst die neue primäre View-Controller. Standardmäßig verwendet der Split-View-Controller automatisch der primäre View-Controller von reduzierten Ansicht. Erneut, kann der Entwickler dieses Verhalten mit überschreiben die `GetPrimaryViewControllerForExpandingSplitViewController` Methode der `UISplitViewControllerDelegate` .
-  Sobald der primäre View-Controller ausgewählt wurde, muss der sekundäre Controller für die Sicht neu erstellt werden. Der Split-View-Controller enthält erneut, automatische Behandlung von dieser Phase basierend auf dem Gerät. Entwickler kann dieses Verhalten überschreiben, durch Aufrufen der `SeparateSecondaryViewController` Methode der `UISplitViewControllerDelegate` .


In einer Split-View-Controller, primärer View-Controller spielt eine sowohl das Erweitern und Reduzieren der Sichten durch Implementieren der `CollapseSecondViewController` und `SeparateSecondaryViewController` Methoden die `UISplitViewControllerDelegate`. `UINavigationController` Diese Methoden automatisch push und pop der sekundären View-Controller implementiert.

### <a name="showing-view-controllers"></a>View-Controller mit

Besteht eine weitere Änderung, die Apple iOS 8 vorgenommen hat wird die Möglichkeit, dass der Entwickler View Controller anzeigt. In der Vergangenheit musste die Anwendung einen Endknoten-View-Controller (z. B. eine Tabelle-View-Controller) und der Entwickler ergab einen anderen (z. B. als Antwort auf die Benutzer Tippen auf eine Zelle), würde die Anwendung wieder durch die Controller-Hierarchie zu erreichen der Navigation-View-Controller, und rufen die `PushViewController` -Methode an, um die neue Ansicht anzuzeigen.

Dies dargestellt eine sehr enge Kopplung zwischen dem Navigationsbereich Controller und der Umgebung, die sie im ausgeführt wurde. In iOS 8 hat Apple dies entkoppelt durch zwei neue Methoden zur Verfügung:

-  `ShowViewController` – Zum Anzeigen des neuen Sicht Controllers basierend auf ihrer Umgebung passt. Beispielsweise ist in einem `UINavigationController` einfach die neue Ansicht auf den Stapel verschoben. In einer Split-View-Controller wird der neue View-Controller als die neue primäre View-Controller auf der linken Seite angezeigt. Wenn keine Container-View-Controller vorhanden ist, wird die neue Ansicht als eine modale View-Controller angezeigt.
-  `ShowDetailViewController` – Funktioniert in ähnlicher Weise wie `ShowViewController`, jedoch implementiert, nicht auf einen Split-View-Controller, ersetzen Sie die Detailansicht mit der neuen View-Controller übergeben werden. Wenn Split-View-Controller (wie in einem iPhone Anwendung dargestellt werden kann) reduziert ist, wird der Aufruf an umgeleitet werden die `ShowViewController` -Methode und die neue Ansicht wird als primäre View-Controller angezeigt werden. Wenn keine Container-View-Controller vorhanden ist, die neue Ansicht als eine modale View-Controller angezeigt.


Diese Methoden arbeiten möchten, indem Sie auf der Blattebene-View-Controller ab und Abarbeiten der Hierarchie anzeigen, bis sie den richtigen Container Ansicht Controller behandelt die Anzeige der die neue Ansicht gefunden.

Entwickler können implementieren `ShowViewController` und `ShowDetailViewController` in ihre eigenen benutzerdefinierten View-Controller abgerufen automatisiert die gleiche Funktionalität, die `UINavigationController` und `UISplitViewController` bietet.

### <a name="how-it-works"></a>Informationen zur Funktionsweise

In diesem Abschnitt werden wir sehen Sie sich wie in iOS 8 tatsächlich diese Methoden implementiert werden. Zuerst sehen wir uns das neue `GetTargetForAction` Methode:

 [![](unified-storyboards-images/gettargetforaction.png "Die neue GetTargetForAction-Methode")](unified-storyboards-images/gettargetforaction.png#lightbox)

Diese Methode führt der Hierarchiekette, bis der richtige Container View-Controller gefunden wird. Zum Beispiel:

1.  Wenn eine `ShowViewController` -Methode aufgerufen wird, in der Kette, die diese Methode implementiert die erste View-Controller ist der Navigation-Controller aus, damit er als das übergeordnete Element des der neuen Sicht verwendet wird.
1.  Wenn eine `ShowDetailViewController` Methode wurde aufgerufen, das der Split-View-Controller ist der erste View-Controller, implementieren, damit er als das übergeordnete Element verwendet wird.


Die `GetTargetForAction` Methode funktioniert durch Suchen nach einem View-Controller, die eine bestimmte Aktion implementiert, und klicken Sie dann mithilfe dieser View-Controller, wenn sie diese Aktion empfangen möchte. Da diese Methode öffentlich ist, können Entwickler erstellen ihre eigenen benutzerdefinierten Methoden, die Funktion wie die integrierte im `ShowViewController` und `ShowDetailViewController` Methoden.

## <a name="adaptive-presentation"></a>Adaptive Präsentation

In iOS 8 machte Apple Popover Präsentationen ( `UIPopoverPresentationController`) adaptive ebenfalls. Damit eine Popover Presentation-View-Controller einer normalen Popover-Ansicht wird automatisch in einer regulären Größe Klasse vorgelegt wird ihn jedoch Ganzer Bildschirm in einer horizontal compact Größe-Klasse anzeigen (z. B. auf einem iPhone).

Um die Änderungen innerhalb des Systems einheitliche Storyboard zu berücksichtigen, wurde eine neue Domänencontroller-Objekts zum Verwalten der dargestellten View-Controller erstellt wurde – `UIPresentationController`. Dieser Controller wird ab dem Zeitpunkt erstellt, die die View-Controller angezeigt werden, solange sie geöffnet ist. Entspricht einer Klasse verwalten, können sie eine übergeordnete Klasse über die View-Controller berücksichtigt werden, da auf Gerät Änderungen reagiert wird, die die View-Controller (z. B. Ausrichtung) betreffen, welche werden dann starre wieder zurück in die View-Controller der Präsentation Controller steuert.

Wenn der Entwickler führte die Verwendung von einem View Controller der `PresentViewController` -Methode, die Verwaltung von der Präsentation-Prozess wird zum übergeben `UIKit`. UIKit verarbeitet (unter anderem) des richtige Controllers für das Format erstellt wird, mit die einzige Ausnahme ist, wenn eine View-Controller hat das Format festgelegt `UIModalPresentationCustom`. Hier kann die Anwendung ist eigene PresentationController anstatt mit Bereitstellen der `UIKit` Controller.

### <a name="custom-presentation-styles"></a>Benutzerdefinierte Präsentationsformate

Mit einer Formatvorlage benutzerdefinierte Präsentation haben Entwickler die Möglichkeit, einen benutzerdefinierten Presentation-Controller verwenden. So ändern Sie das Aussehen und Verhalten der Sicht, die es zu allied ist, kann dieser benutzerdefinierten Controller verwendet werden.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Arbeiten mit Größenklassen

Das Adaptive Fotos Xamarin-Projekt, das in diesem Artikel enthalten ist, bietet ein funktionsfähiges Beispiel der Verwendung von Größenklassen und Adaptive View-Controller in einer Anwendung für iOS 8 vereinheitlichte Schnittstelle.

Während die Anwendung ihre Benutzeroberfläche vollständig aus dem Code im Gegensatz zur Verwendung der IOS-Designer erstellt, und erstellen ein Storyboard Unified, die gleichen Techniken anwenden. Weiter unten in diesem Artikel zeigen wir, wie Größenklassen mit einem Storyboard Unified und den iOS-Designer in einer Xamarin-Anwendung verwendet werden.

Jetzt sehen wir uns näher an wie das Adaptive Fotos Projekt mehrere der Größe Klasse Funktionen in iOS 8 zum Erstellen einer Anwendung Adaptive implementiert.

### <a name="adapting-to-trait-environment-changes"></a>Anpassung an Umgebungsänderungen Merkmal ""

Bei der Ausführung der Anwendung Adaptive Fotos auf einem iPhone, wenn der Benutzer das Gerät im Hochformat, Querformat dreht wird der Split-View-Controller Master und Details anzuzeigen:

 [![](unified-storyboards-images/rotation.png "Der Split-View-Controller zeigt sowohl den Master und Details anzuzeigen, wie hier zu sehen")](unified-storyboards-images/rotation.png#lightbox)

Dies erfolgt durch Überschreiben der `UpdateConstraintsForTraitCollection` Methode des View-Controller und passen die Einschränkungen basierend auf den Wert der `VerticalSizeClass`. Zum Beispiel:

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

### <a name="adding-transition-animations"></a>Hinzufügen von Übergangsanimationen

Wenn der Split-View-Controller in der Anwendung von wechselt Adaptive Fotos zu erweiterten reduziert, Animationen hinzugefügt. die Animationen an Standardeinstellung durch Überschreiben der `WillTransitionToTraitCollection` -Methode des Controllers anzeigen. Zum Beispiel:

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

### <a name="overriding-the-trait-environment"></a>Überschreiben die Umgebung Merkmal ""

Oben ist ersichtlich, erzwingt die Anwendung Adaptive Fotos der Teilung View-Controller, um die Details anzuzeigen und die master-Ansichten aus, wenn die iPhone-Geräte in die Ansicht im Querformat ist.

Zu diesem Zweck verwenden den folgenden Code in die View-Controller:

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

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Erweitern und Reduzieren der Split-View-Controller

Weiter wird untersucht, wie das Verhalten erweiterte und reduzierende der der Split-View-Controller in Xamarin implementiert wurde. In der `AppDelegate`, wenn der Split-View-Controller erstellt wird, dessen Delegat wird so behandeln Sie diese Änderungen zugewiesen:

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

Die `SeparateSecondaryViewController` Methode überprüft, ob ein Foto angezeigt wird und führt die Aktionen, die für den Status angemessenen. Ist kein Foto reduziert angezeigt wird der sekundäre View-Controller, so, dass die Master-View-Controller angezeigt wird.

Die `CollapseSecondViewController` Methode wird verwendet, wenn der Split-View-Controller erweitern um festzustellen, ob Fotos auf dem Stapel vorhanden, wenn Sie also auf die jeweilige Sicht reduziert wird.

### <a name="moving-between-view-controllers"></a>Verschieben zwischen View-Controller

Als Nächstes werfen wir einen Blick auf wie die Anwendung Adaptive Fotos zwischen Domänencontrollern Ansicht bewegt. In der `AAPLConversationViewController` Klasse, wenn der Benutzer eine Zelle aus der Tabelle wählt die `ShowDetailViewController` Methode wird aufgerufen, um die Details anzuzeigen:

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

### <a name="displaying-disclosure-indicators"></a>Anzeigen von Offenlegung von Indikatoren

In der Anwendung Adaptive Foto stehen Ihnen mehrere Orte, wo Offenlegung von Indikatoren ausgeblendet oder basierend auf Änderungen an der Umgebung Merkmal "" angezeigt. Dies erfolgt durch den folgenden Code:

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

Diese werden implementiert, mit der `GetTargetViewControllerForAction` oben ausführlich erläutert.

View-Controller eine Tabelle zum Anzeigen von Daten ist, verwendet es die Methoden, die oben implementiert werden, um anzuzeigen, und zwar unabhängig davon, ob ein Push Begriff ist, durchgeführt werden soll, und ob ein- oder Ausblenden von der Offenlegung von Indikator entsprechend:

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

### <a name="new-showdetailtargetdidchangenotification-type"></a>Neue `ShowDetailTargetDidChangeNotification` Typ

Apple hat einen neuen Benachrichtigungstyp zum Arbeiten mit Klassen Größe und Merkmal ""-Umgebungen in einer Split-View-Controller hinzugefügt `ShowDetailTargetDidChangeNotification`. Diese Benachrichtigung wird gesendet, bei jeder Änderung des Ziels für ein Split-View-Controller Detailansicht z. B. wenn der Controller erweitert oder reduziert.

Die Anwendung Adaptive Fotos verwendet diese Benachrichtigung, um den Status des Indikators Offenlegung bei Änderung der Detail-View-Controller zu aktualisieren:

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

Näher an die Anwendung Adaptive Fotos, der die Weise, Größe-Klassen finden Sie unter, Merkmals Sammlungen und Adaptive View-Controller kann problemlos eine Anwendung Unified in Xamarin.iOS erstellt verwendet werden.

## <a name="unified-storyboards"></a>Einheitliche Storyboards

Neu für iOS 8, Storyboards Unified Storyboarddatei, die auf iPhone und iPad-Geräte angezeigt werden kann, indem Sie als Ziel mehrere Größenklassen unified ermöglichen es dem Entwickler, erstellen. Mithilfe von Unified Storyboards der Entwickler schreibt weniger bestimmten UI-Code und verfügt über nur eine Schnittstelle zum Erstellen und verwalten.

Die wichtigsten Vorteile von Storyboards Unified sind:

-  Verwenden Sie die gleiche Storyboarddatei für iPhone und iPad.
-  Abwärtskompatibilität für iOS 6 und iOS 7 bereitstellen.
-  Das Layout für verschiedene Geräte, Ausrichtungen und OS-Versionen von Xamarin iOS-Designer eine Vorschau anzeigen.

Diese Funktion wird vollständig in Visual Studio für Mac unterstützt.

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Aktivieren die von Größenklassen

Standardmäßig werden alle neues Xamarin.iOS Projekt uns size-Klassen. Um Größenklassen und Adaptive Segues in ein Storyboard aus ein älteres Projekt verwenden möchten, müssen sie zuerst in das Xcode 6 Unified Storyboard-Format in der iOS-Designer konvertiert werden.

Öffnen Sie führen Sie das Storyboard in der iOS-Designer und Kontrollkästchen konvertiert werden die **verwendbaren Größenklassen** Kontrollkästchen:

 [![](unified-storyboards-images/sizeclass01.png "Aktivieren Sie das Kontrollkästchen verwendbaren Größenklassen")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS-Designer wird bestätigt, dass der Entwickler kann das Format des Storyboards Größe a-Klassen konvertieren:

 [![](unified-storyboards-images/sizeclass02.png "Die Verwendung Größenklassen Warnung")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Automatisches Layout muss Größenklassen ordnungsgemäß funktioniert auch überprüft werden.

### <a name="generic-device-types"></a>Generische Gerätetypen

Sobald das Storyboard konvertiert wurde, um Größenklassen zu verwenden, wird es in der Entwurfsoberfläche erneut und die **Ansicht als** Gerät generisch sein wird:

 [![](unified-storyboards-images/sizeclass03.png "Als einen generischen Gerätetyp anzeigen")](unified-storyboards-images/sizeclass03.png#lightbox)

Wenn der generische Gerätetyp aktiviert ist, werden alle View-Controller in einem Quadrat 600 x 600 einnimmt. Dieses Rechteck stellt die Größen der Breite und jeder beliebigen Höhe dar. Bei iOS-Designer in diesem Modus ist, gelten alle Änderungen für alle Klassen Größe.

Der Entwickler hat auch die Möglichkeit die Entwurfsoberfläche als ein iPhone anzeigen:

 [![](unified-storyboards-images/sizeclass04.png "Anzeigen von der Entwurfsoberfläche als ein iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Oder sie als einem iPad anzeigen:

 [![](unified-storyboards-images/sizeclass05.png "Anzeigen von der Entwurfsoberfläche als einem iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Wählen Sie eine Größe-Klasse

Die Größe Klassenauswahl-Schaltfläche ist an der oberen linken Ecke der Entwurfsoberfläche angezeigt (in der Nähe der Ansicht als Dropdownliste). Es kann der Entwickler wählen, welche Größenklassen derzeit bearbeitet wird:

 [![](unified-storyboards-images/sizeclass06.png "Wählen Sie eine Größe-Klasse")](unified-storyboards-images/sizeclass06.png#lightbox)

Die Auswahl stellt die Auswahl der Größe Klasse als eine 3 x 3-Raster. Jede der Quadrate im Raster stellt eine Kombination aus einer Klasse für die Breite und eine Höhe-Klasse dar. Das Quadrat Center wählt die Any Width/Any Höhe Größe-Klasse (Dies ist die Standardansicht für ein Drehbuch Unified). Wenn diese Quadrat ausgewählt ist, ist der Entwickler das Standardlayout bearbeiten, die von den anderen Konfigurationen geerbt wird.

Das Quadrat in der oberen linken Ecke des Rasters stellt die Compact Breite/Compact Höhe Größe-Klasse dar:

 [![](unified-storyboards-images/sizeclass07.png "Der Compact Breite/Compact Höhe Größe-Klasse")](unified-storyboards-images/sizeclass07.png#lightbox)

Dieser Modus entspricht einem iPhone im Querformat. Das Quadrat in der unteren rechten Ecke des Rasters reguläre Breite/reguläre Höhe Größe stellt die Klasse dar, die einem iPad darstellt:

 [![](unified-storyboards-images/sizeclass08.png "Die normale Breite/reguläre Höhe Größe-Klasse")](unified-storyboards-images/sizeclass08.png#lightbox)

Um das Layout für das iPhone im Hochformat zu bearbeiten, wählen Sie das Quadrat in der unteren linken Ecke aus. Dies stellt die Compact Breite/reguläre Höhe Größe-Klasse:

 [![](unified-storyboards-images/sizeclass09.png "Der Compact Breite/reguläre Höhe Größe-Klasse")](unified-storyboards-images/sizeclass09.png#lightbox)

Klicken Sie in das Quadrat, um sie auszuwählen, und der Entwurfsoberfläche ändert sich die Größe des View-Controller auf die neue Auswahl zu entsprechen:

 [![](unified-storyboards-images/sizeclass10.png "Der Entwurfsoberfläche wird die Größe des View-Controller auf die neue Auswahl zu entsprechen, entsprechend ändern.")](unified-storyboards-images/sizeclass10.png#lightbox)

Finden Sie im Abschnitt Größe Klasse diesem Artikel finden Sie weitere Informationen über Größenklassen und deren Layout für iPhones und iPads Einfluss auf.

### <a name="adaptive-segue-types"></a>Adaptive Segue Typen

Wenn der Entwickler hat Storyboards, bevor Sie verwendet, werden mit den vorhandenen Segue vertraut **Push**, **modale** und **Popover**. Wenn Größenklassen für eine Unified Storyboard-Datei aktiviert sind, werden die folgenden Adaptive Segue Typen (die die neue Ansicht-API von Netzwerkcontroller weiter oben erläuterten entsprechen) zur Verfügung gestellt: **anzeigen** und **Details anzeigen** .

> [!IMPORTANT]
> Wenn Größenklassen aktiviert sind, alle vorhandenen segues wird in die neuen Typen konvertiert werden.

Nehmen Sie als Beispiel eine IOS 8-Anwendung, ein Storyboard Unified mit einem Split-View-Controller verwendet, die eine einfache Spiel Navigationsmenü in der Master-Ansicht verfügt. Wenn der Benutzer auf eine Schaltfläche klickt, sollte des ausgewählten Elements-View-Controller im Abschnitt "Details" des Controllers geteilten Ansicht angezeigt werden, bei Ausführung auf einem iPad. Auf einem iPhone sollte des Elements-View-Controller auf Navigationsstapel abgelegt werden.

Um diesen Effekt zu erreichen, in der iOS-Designer-Steuerelement-klicken Sie auf die Schaltfläche und ziehen Sie eine Zeile die View-Controller, die angezeigt werden. Wenn die Maustaste losgelassen wird, wählen Sie `Show Detail` im Popupmenü Typ Segue:

 [![](unified-storyboards-images/segue01.png "Wählen Sie im Popupmenü Typ Segue Details anzeigen")](unified-storyboards-images/segue01.png#lightbox)

Die neue Segue wird zwischen der Schaltfläche und die View-Controller erstellt. Führen Sie die Anwendung jetzt in der iPhone-Simulator aus, und klicken Sie im Hauptmenü angezeigt:

 [![](unified-storyboards-images/segue02.png "Im Hauptmenü")](unified-storyboards-images/segue02.png#lightbox)

Klicken Sie auf die **wählen Spiel** Schaltfläche und des Elements-View-Controller wird im Navigationsbereich Stapel geschoben werden:

 [![](unified-storyboards-images/segue03.png "Die Elemente View Controller werden auf dem Navigationsstapel abgelegt werden, wie dargestellt")](unified-storyboards-images/segue03.png#lightbox)

Beenden Sie die iPhone-Simulator aus, und führen Sie die Anwendung auf dem iPad Simulator. Wechseln Sie zur Querformat und den Hauptknoten im Menü wird wieder angezeigt:

 [![](unified-storyboards-images/segue04.png "Im Hauptmenü angezeigt")](unified-storyboards-images/segue04.png#lightbox)

Erneut aus, klicken Sie auf die **wählen Spiel** Schaltfläche und des Elements-View-Controller wird im Abschnitt "Details" Split-View-Controller gezeigt:

 [![](unified-storyboards-images/segue05.png "Die Elemente View-Controller gezeigt, die im Abschnitt \"Details\" des Controllers geteilte Ansicht")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Ein Element ausschließen aus einer Klasse Größe

Es gibt Situationen, wenn ein angegebenes Element (z. B. eine Sicht, Steuerelement oder eine Einschränkung) nicht innerhalb einer bestimmten Größe Klasse erforderlich ist. Wenn ein Element aus einer Klasse Größe ausschließen möchten, wählen Sie das gewünschte Element auszuschließenden der **Entwurfsoberfläche**. Führen Sie einen Bildlauf zum unteren Rand der **Property Explorer** , und klicken Sie auf die **"Zahnrad"-Symbols** Dropdown-Menüs. Wählen Sie die Kombination von **Breite** und **Höhe** auf das Element ausgeschlossen:

[![](unified-storyboards-images/exclude-a.png "Wählen Sie die Kombination aus Breite und Höhe")](unified-storyboards-images/exclude-a.png#lightbox)

Ein neues *Ausschluss Groß-/Kleinschreibung* wird hinzugefügt werden, auf das Element in den unteren Rand der **Property Explorer**. Als Nächstes deaktivieren Sie die **installiert** Kontrollkästchen für die angegebene Größe-Klasse:

[![](unified-storyboards-images/exclude-b.png "Deaktivieren Sie das Kontrollkästchen installiert")](unified-storyboards-images/exclude-b.png#lightbox)

Wechseln der Entwurfsoberfläche auf die Breite und Höhe, die das Element ausgeschlossen wurde, wurde es aus der angegebenen Größe-Klasse, aber nicht das gesamte Design UI entfernt:

 [![](unified-storyboards-images/exclude02.png "Wechseln Sie der Entwurfsoberfläche auf die Breite und Höhe, die das Element ausgeschlossen wurde")](unified-storyboards-images/exclude02.png#lightbox)

Wechseln zurück an die Höhe der Any Width/Any Größe-Klasse und das Element ist weiterhin gültig:

 [![](unified-storyboards-images/exclude03.png "Wechseln zurück an die Höhe der Any Width/Any Größe-Klasse")](unified-storyboards-images/exclude03.png#lightbox)

Wenn die Anwendung auf dem iPad Simulator ausgeführt wird, wird das Element angezeigt:

 [![](unified-storyboards-images/exclude04.png "Das Element angezeigt wird, wenn der ausgeführten app auf dem iPad-Simulator")](unified-storyboards-images/exclude04.png#lightbox)

Und wenn die Anwendung auf dem iPhone-Simulator ausgeführt wird, ist das Element nicht vorhanden:

 [![](unified-storyboards-images/exclude05.png "Das Element fehlt, wenn der ausgeführten app in der iPhone-Simulator")](unified-storyboards-images/exclude05.png#lightbox)

Um einen Ausschluss Groß-/Kleinschreibung von einem Element zu entfernen, wählen Sie einfach das Element in der **Entwurfsoberfläche**, einen Bildlauf zum unteren Rand der **Property Explorer** , und klicken Sie auf die **-** Schaltfläche neben die Groß-/Kleinschreibung zu entfernen.

Um eine Implementierung der Storyboards Unified anzuzeigen, sehen die `UnifiedStoryboard` Beispiel Xamarin iOS 8-Anwendung, die auf dieses Dokument angefügt.

## <a name="dynamic-launch-screens"></a>Dynamische Startbildschirme

Während eine iOS-Anwendung wird gestartet, zum Bereitstellen von Feedback für den Benutzer, die die app tatsächlich Starten von wird der Bildschirm-Startdatei als Begrüßungsbildschirm angezeigt. Bevor Sie iOS 8, der Entwickler müssten Multiple-include- `Default.png` image Bestände für jedes Gerät Typ, Ausrichtung und die Bildschirmauflösung, die die Anwendung ausgeführt werden würde. Beispielsweise `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`usw.

Umgestalten in der neuen iPhone 6 und iPhone 6 Plus Geräte (und die bevorstehende Apple Watch) mit allen vorhandenen iPhone und iPad-Geräte, stellt dies ein großes Array von verschiedenen Größen, Ausrichtungen und Auflösung von `Default.png` Start Bildschirm Bildanlagen, die müssen erstellt und verwaltet werden. Darüber hinaus diese Dateien können sehr groß sein und werden lieferleistung Anwendungspaket, erhöhen die erforderliche Zeit zum Herunterladen der Anwendung im iTunes App Store (möglicherweise da diese nicht über einem Mobilfunknetz Übermittlung) "Aufblasen" und die Menge an Speicherplatz auf dem Gerät des Endbenutzers zu erhöhen.

Neu für iOS 8, kann der Entwickler erstellen eine einzelne, das atomarische `.xib` Datei in Xcode, mit denen Automatisches Layout und Klassen Größe erstellt eine *dynamische starten Bildschirm* , die für jedes Gerät, Auflösung und Ausrichtung funktioniert. Dies verringert nicht nur der Arbeitsaufwand, der Entwickler erstellen und verwalten alle erforderlichen Bildanlagen, aber es erheblich reduziert die Größe des installierten Anwendungspaket.


Dynamische starten Bildschirme haben die folgenden Einschränkungen und Überlegungen:

 - Verwenden Sie ausschließlich `UIKit` Klassen.
 - Verwenden Sie eine einzelnen Stamm-Sicht, ist eine `UIView` oder `UIViewController` Objekt.
 - Machen Sie alle Verbindungen mit dem Code der Anwendung keine (keine hinzufügen **Aktionen** oder **Steckdosen**).
 - Hinzufügen von nicht `UIWebView` Objekte.
 - Verwenden Sie keine benutzerdefinierte Klassen.
 - Verwenden Sie keine Attribute der Laufzeit.

Mit den oben aufgeführten Richtlinien Denken Sie daran sehen wir uns einen dynamischen starten Bildschirm zu einem vorhandenen Xamarin iOS 8-Projekt hinzufügen.

Führen Sie folgende Schritte aus:

1. Open **Visual Studio für Mac** und Laden Sie die **Lösung** den dynamischen starten Bildschirm hinzufügen.
2. In der **Projektmappen-Explorer**, mit der rechten Maustaste die `MainStoryboard.storyboard` Datei, und wählen Sie **Öffnen mit** > **Xcode Schnittstelle-Generator**:

    [![](unified-storyboards-images/dls01.png "Mit Xcode-Schnittstelle-Generator öffnen")](unified-storyboards-images/dls01.png#lightbox)
3. Wählen Sie in Xcode **Datei** > **neu** > **Datei...** :

    [![](unified-storyboards-images/dls02.png "Wählen Sie die Datei / neu")](unified-storyboards-images/dls02.png#lightbox)
4. Wählen Sie **iOS** > **Benutzeroberfläche** > **starten Bildschirm** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](unified-storyboards-images/dls03.png "Wählen Sie die iOS / Benutzeroberfläche / Bildschirm starten")](unified-storyboards-images/dls03.png#lightbox)
5. Nennen Sie die Datei `LaunchScreen.xib` , und klicken Sie auf die **erstellen** Schaltfläche:

    [![](unified-storyboards-images/dls04.png "Nennen Sie die Datei LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. Bearbeiten Sie den Entwurf des Startbildschirms, indem Sie grafische Elemente hinzufügen und positionieren Sie sie für Geräte, Ausrichtungen und Bildschirmgrößen mithilfe Layout Einschränkungen:

    [![](unified-storyboards-images/dls05.png "Bearbeiten den Entwurf des Startbildschirms")](unified-storyboards-images/dls05.png#lightbox)
7. Speichern Sie die Änderungen zu `LaunchScreen.xib`.
8. Wählen Sie die **Anwendungen Ziel** und **allgemeine** Registerkarte:

    [![](unified-storyboards-images/dls06.png "Wählen Sie das Ziel der Anwendungen und die Registerkarte \"Allgemein\"")](unified-storyboards-images/dls06.png#lightbox)
9. Klicken Sie auf die **wählen Sie "Info.plist"** auswählen die `Info.plist` für die Xamarin-app, und klicken Sie auf die **auswählen** Schaltfläche:

    [![](unified-storyboards-images/dls07.png "Wählen Sie die Datei \"Info.plist\" für die Xamarin-app")](unified-storyboards-images/dls07.png#lightbox)
10. In der **App-Symbole und starten Sie Bilder** , öffnen Sie im Abschnitt der **starten Bildschirm Datei** Dropdownliste, und wählen Sie die `LaunchScreen.xib` oben erstellten:

    [![](unified-storyboards-images/dls08.png "Wählen Sie die LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. Speichern Sie die Änderungen in der Datei und zurück zu Visual Studio für Mac.
12. Warten Sie, Visual Studio für Mac, in die Synchronisierung von Änderungen mit Xcode abzuschließen.
13. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Ressource** Ordner, und wählen **hinzufügen** > **Dateien hinzufügen...** :

    [![](unified-storyboards-images/dls09.png "Wählen Sie hinzufügen oder Dateien hinzufügen...")](unified-storyboards-images/dls09.png#lightbox)
14. Wählen Sie die `LaunchScreen.xib` oben erstellte Datei, und klicken Sie auf die **öffnen** Schaltfläche:

    [![](unified-storyboards-images/dls10.png "Wählen Sie die Datei LaunchScreen.xib")](unified-storyboards-images/dls10.png#lightbox)
15. Erstellen Sie die Anwendung.

### <a name="testing-the-dynamic-launch-screen"></a>Testen die dynamische-Startbildschirm

Wählen Sie in Visual Studio für Mac die iPhone-4-Retina-Simulator, und führen Sie die Anwendung. Die dynamische starten Bildschirm wird im richtigen Format und Ausrichtung angezeigt:

[![](unified-storyboards-images/dls11.png "Die dynamische starten Bildschirm angezeigt, in die vertikale Ausrichtung")](unified-storyboards-images/dls11.png#lightbox)

Beenden Sie die Anwendung in Visual Studio für Mac, und wählen Sie ein iPad iOS 8-Gerät. Führen Sie die Anwendung und der Startbildschirm wird ordnungsgemäß formatiert werden, für dieses Gerät und die Ausrichtung:

[![](unified-storyboards-images/dls12.png "Die dynamische starten Bildschirm angezeigt, in die horizontale Ausrichtung")](unified-storyboards-images/dls12.png#lightbox)

Zurück zu Visual Studio für Mac, und beenden Sie die Anwendung ausgeführt wird.

### <a name="working-with-ios-7"></a>Arbeiten mit iOS 7

Um die Abwärtskompatibilität mit iOS 7 zu gewährleisten, fügen Sie einfach die üblichen `Default.png` image Bestand wie gewohnt in iOS 8-Anwendung. iOS wird zum früheren Verhalten zurückzukehren, und verwenden diese Dateien als Startbildschirm ein, wenn auf einem iOS 7-Gerät ausführen.

Um eine Implementierung eines dynamischen starten-Bildschirms in Xamarin anzuzeigen, sehen die [dynamische starten Bildschirme](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) Beispiel iOS 8-Anwendung, die auf dieses Dokument angefügt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel benötigte einen Überblick über Größenklassen und wie sie das Berichtslayout im iPhone und iPad-Geräte auswirken. Es erläutert die Funktionsweise von Merkmale, Merkmals Umgebungen und Merkmals Sammlungen mit Größenklassen vereinheitlichte Schnittstellen erstellt werden. Dies dauerte kurzen Blick auf Adaptive View-Controller und deren Funktionsweise mit Größenklassen innerhalb vereinheitlichte Schnittstellen. Er erläutert, Größe-Klassen und Schnittstellen Unified vollständig von C#-Code in einer Xamarin iOS 8-Anwendung implementieren.

Schließlich in diesem Artikel behandelt die Grundlagen der Erstellung von Unified Storyboards mit Xamarin-iOS-Designer, die auf iOS-Geräten eingesetzt werden, und erstellen eine einzelne, dynamische starten Bildschirm, als Startbildschirm auf jedem iOS 8-Gerät angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Adaptive Fotos (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro-Beispiel](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Dynamische starten Bildschirme (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Dynamische Layouts in iOS8 - weiterentwickelt 2014 (Video)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
