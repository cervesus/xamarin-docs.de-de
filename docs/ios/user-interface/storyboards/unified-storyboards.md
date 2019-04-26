---
title: Einheitliche Storyboards in Xamarin.iOS
description: Dieses Dokument beschreibt die einheitliche Storyboards in Xamarin.iOS. Einheitliche Storyboards ermöglichen Entwicklern das zur Unterstützung mehrerer Bildschirmgrößen mit einer einzelnen Schnittstelle-Definition.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 26aeaa3d230a5c104014edd899b8d9231ced31e9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61430215"
---
# <a name="unified-storyboards-in-xamarinios"></a>Einheitliche Storyboards in Xamarin.iOS

iOS 8 umfasst einen neuen, einfacher zu bedienende Mechanismus zum Erstellen der Benutzeroberfläche – einheitliche Storyboards. Mit einem einzelnen Storyboard alle Bildschirmgrößen der unterschiedliche Hardware abgedeckt, schnelle und reaktionsfähige Sichten erstellt werden können, eine "Entwurf-einmal", "verwenden: n" Format.

Da muss der Entwickler nicht mehr ein getrennt und bestimmte Storyboard für iPhone und iPad-Geräte zu erstellen, haben sie die Flexibilität zum Entwerfen von Anwendungen mit der eine allgemeine Schnittstelle und passen Sie diese Schnittstelle mit anderer Größe für Klassen. Auf diese Weise kann eine Anwendung an die Stärken der einzelnen gerätegrößen angepasst werden, und jede Benutzeroberfläche optimiert werden kann, um die bestmögliche Erfahrung zu bieten.

<a name="size-classes" />

## <a name="size-classes"></a>Größenklassen

Vor iOS 8, den der Entwickler verwendet `UIInterfaceOrientation` und `UIInterfaceIdiom` , zwischen Hoch-und Querformat und zwischen iPhone und iPad-Geräten zu unterscheiden. In iOS 8 Ausrichtung und das Gerät wird bestimmt mithilfe *Größenklassen*.

Geräte werden von Größenklassen, definiert, in der vertikalen und horizontalen Achsen, und es gibt zwei Arten von Größenklassen IOS 8:

-  **Reguläre** – Dies ist für eine große Bildschirmgröße (z. B. einem iPad) oder eine minianwendung, die den Eindruck eine beträchtliche Größe bietet (z. B. einen `UIScrollView`
-  **Compact** – Dies ist für kleinere Geräte (z. B. eine iPhone). Diese Größe berücksichtigt die Ausrichtung des Geräts.


Wenn die beiden Konzepte zusammen verwendet werden, ist das Ergebnis ein 2 x 2-Raster, das definiert die verschiedenen möglichen Größen, die in die beiden unterschiedlichen Ausrichtungen verwendet werden können, wie im folgenden Diagramm dargestellt:

 [![](unified-storyboards-images/sizeclassgrid.png "Eine aus 2 x 2-Raster, das definiert die verschiedenen möglichen Größen, die verwendet werden können, im regulären und Compact Ausrichtungen")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Entwickler kann eine View-Controller erstellen, der einen der vier Möglichkeiten, die in verschiedenen Layouts führen würden verwendet, (wie in der Abbildung oben dargestellt).

### <a name="ipad-size-classes"></a>iPad Größenklassen

Das iPad, aufgrund der Größe, verfügt über eine **regulären** Klasse Größe für beide Ausrichtungen.

 [![](unified-storyboards-images/image1.png "iPad Größenklassen")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone Größenklassen

Das iPhone verfügt über verschiedene Größenklassen je nach Ausrichtung des Geräts:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone Größenklassen")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Wenn das Gerät im Hochformat ist, wird der Bildschirm enthält ein **compact** horizontal Klasse und **regulären** vertikal
-  Wenn das Gerät im Querformatmodus ausgeführt wird, werden die Klassen Bildschirm vom Hochformat rückgängig gemacht werden.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus Größenklassen

Die Größen sind identisch mit der früheren iPhones im Hochformat genutzt, aber unterschiedliche im Querformat:

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus Größenklassen")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Da das iPhone 6 Plus verfügt über einen Bildschirm groß genug ist, es ist eine normale Breite Größe-Klasse in das Querformat zu erhalten.

### <a name="support-for-a-new-screen-scale"></a>Unterstützung für einen neuen Bildschirm Skalierung

Das iPhone 6 Plus verwendet eine neue Retina HD-Anzeige mit einem Bildschirm Skalierungsfaktor von 3.0 (dreimal die ursprünglichen iPhone Bildschirmauflösung). Um die bestmögliche Leistung auf diesen Geräten zu ermöglichen, zählen Sie neue Grafik, die für diese Skalierung Bildschirm konzipiert. In Xcode 6 und höher Asset können Kataloge Images auf 1 X, 2 X und 3 X-Größen sind: Fügen Sie einfach den neuen Bildanlagen und iOS wählt die richtigen Ressourcen bei der Ausführung auf einem iPhone 6 Plus.

Das Bild auch in iOS Ladeverhaltens erkennt eine `@3x` Suffix auf Bilddateien. Wenn der Entwickler ein Bildobjekt (in unterschiedlichen Auflösungen) in das Anwendungspaket mit den folgenden Dateinamen enthält z. B.: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, und `MonkeyIcon@3x.png`. Auf dem iPhone 6 Plus dem `MonkeyIcon@3x.png` Image wird automatisch verwendet, wenn der Entwickler ein Image mithilfe des folgenden Codes lädt:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Oder wenn sie das Bild für ein Benutzeroberflächenelement, mit dem iOS-weisen Sie als Designer `MonkeyIcon.png`, `MonkeyIcon@3x.png` verwendet werden, automatisch wieder auf dem iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Dynamische Startbildschirme

Die Start-Bildschirm-Datei wird als Begrüßungsbildschirm angezeigt, während eine iOS-Anwendung ausgeführt werden, um dem Benutzer Feedback bereitzustellen, die die app tatsächlich starten-registriert ist. Vor iOS 8, hätte der Entwickler sollen mehrere `Default.png` Bildanlagen für jedes Gerät Typ, Ausrichtung und die Bildschirmauflösung, die die Anwendung ausgeführt werden sollen.

Neue IOS 8, kann der Entwickler erstellen Sie eine einzelne, unteilbare `.xib` -Datei in Xcode, die Automatisches Layout und die Größenklassen verwendet werden, zum Erstellen einer *dynamische Startbildschirm* funktioniert, die für jedes Gerät, Auflösung und Ausrichtung. Dies reduziert nicht nur die Menge der Arbeit des Entwicklers zum Erstellen und verwalten alle erforderlichen Bildanlagen erforderlich werden, verringert jedoch die Größe des installierten Paket der Anwendung.

## <a name="traits"></a>Merkmale

Merkmale sind Eigenschaften, die verwendet werden können, um zu bestimmen, wie ein Layout als seine Umgebung ändert sich ändert. Sie bestehen aus einem Satz von Eigenschaften (die `HorizontalSizeClass` und `VerticalSizeClass` basierend auf `UIUserInterfaceSizeClass`), sowie die Ausdrucksweise Schnittstelle ( `UIUserInterfaceIdiom`) und die anzeigeskalierung.

Alle vorherigen Zustände werden eingeschlossen in einem Container, auf die Apple als Merkmal Auflistung verweist ( `UITraitCollection`), nicht nur die Eigenschaften, sondern auch deren Werte enthält.

## <a name="trait-environment"></a>Trait-Umgebung

Merkmal Umgebungen sind eine neue Schnittstelle IOS 8 und können zum Zurückgeben einer Auflistung Merkmal für die folgenden Objekte:

-  Screens ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Anzeigen von Controllern ( `UIViewController` ).
-  Sichten ( `UIView` ).
-  Präsentation Controller ( `UIPresentationController` ).


Der Entwickler verwendet die Trait-Auflistung, die von einer Umgebung für die Eigenschaft zurückgegeben, um zu bestimmen, wie eine Benutzeroberfläche angeordnet werden soll.

Alle Trait-Umgebungen stellen eine Hierarchie, wie im folgenden Diagramm dargestellt:

 [![](unified-storyboards-images/viewhierarchy.png "Hierarchiediagramm-Merkmal-Umgebungen")](unified-storyboards-images/viewhierarchy.png#lightbox)

Die Trait-Sammlung, von denen jeder der oben genannten Merkmal Umgebungen fließt, wird standardmäßig vom übergeordneten Element der untergeordneten Umgebung.

Zusätzlich zum Abrufen der aktuellen Auflistung der Eigenschaft, die Trait-Umgebung verfügt über eine `TraitCollectionDidChange` -Methode, die in der Ansicht "oder" View Controller-Unterklassen überschrieben werden kann. Der Entwickler kann diese Methode verwenden, die Elemente der Benutzeroberfläche ändern, die "traits" abhängig, wenn sich die "traits" geändert haben.

## <a name="typical-trait-collections"></a>Typisches Merkmal Sammlungen

In diesem Abschnitt werden die typischen Arten von Merkmal Sammlungen behandelt, die Benutzeroberfläche verwendet wird bei der Arbeit mit iOS 8.

Im folgenden finden eine typische-Merkmal-Auflistung, die der Entwickler möglicherweise auf einem iPhone angezeigt:

|Eigenschaft|Wert|
|--- |--- |
|`HorizontalSizeClass`|Komprimieren|
|`VerticalSizeClass`|Regulär|
|`UserInterfaceIdom`|Phone|
|`DisplayScale`|2.0|

Haben würden eine vollständig qualifizierte Merkmal Sammlung darstellen, wie sie Werte für alle zugehörigen Eigenschaften für die Eigenschaft enthält.

Es ist auch möglich, dass ein Merkmal-Auflistung, die einige ihrer Werte nicht vorhanden ist (die in Apple als bezeichnet *Unspecified*):

|Eigenschaft|Wert|
|--- |--- |
|`HorizontalSizeClass`|Komprimieren|
|`VerticalSizeClass`|Nicht angegeben.|
|`UserInterfaceIdom`|Nicht angegeben.|
|`DisplayScale`|Nicht angegeben.|

Im Allgemeinen jedoch gibt Wenn der Entwickler die Trait-Umgebung für die Trait-Sammlung gefragt werden, es eine vollqualifizierte Sammlung zurück wie im obigen Beispiel gezeigt.

Ist ein Merkmal-Umgebung (z. B. eine Sicht oder View Controller) nicht innerhalb der aktuellen Hierarchie von Inhaltsansichten, erhalten Entwickler möglicherweise wieder nicht angegebenen Werte für eine oder mehrere Eigenschaften Merkmal.

Der Entwickler erhalten auch eine teilweise qualifizierte-Merkmal-Auflistung, wenn sie eine dieser Methoden zur Clustererstellung bereitgestellt von Apple, z. B. verwenden `UITraitCollection.FromHorizontalSizeClass`, um eine neue Sammlung zu erstellen.

Ein Vorgang, der für mehrere Merkmal Sammlungen ausgeführt werden kann ist diese miteinander vergleichen Hierbei ist eine Sammlung von Merkmal aufgefordert, wenn sie einen anderen enthält. Was bedeutet *Kapselung* besteht darin, dass für alle Merkmal, das in der zweiten Auflistung angegeben, der Wert genau mit dem Wert in der ersten Auflistung entsprechen muss.

Zum Testen verwenden, zwei Merkmale der `Contains` Methode der `UITraitCollection` und den Wert des Merkmals getestet werden.

Der Entwickler kann die Vergleiche durchführen, in der manuell im Code, um zu bestimmen, wie Layoutansichten oder View Controller. Allerdings `UIKit` verwendet Sie diese Methode intern, um einen Teil der Funktionen, wie in den Proxy Darstellung, z. B. bereitstellen.

## <a name="appearance-proxy"></a>Darstellung-Proxy

Der Proxy für die Darstellung wurde in früheren Versionen von iOS verwenden, um Entwicklern ermöglichen, die die Eigenschaften des ihre Ansichten anpassen eingeführt. Es wurde in iOS 8-Merkmal-Sammlungen unterstützt, die erweitert.

Darstellung Proxys umfassen jetzt eine neue Methode `AppearanceForTraitCollection`, einen neuen Proxy für die Darstellung zurückgibt, für die angegebene Eigenschaft-Auflistung, der in übergeben wurde. Alle Anpassungen, die die Entwickler dazu führt Darstellung Proxy dauert nur Auswirkungen auf die Ansichten, die entsprechen die angegebene Eigenschaft-Auflistung.

In der Regel der Entwickler in einer teilweise angegebenen Trait-Auflistung, übergibt die `AppearanceForTraitCollection` Methode, z. B. eine, die nur eine horizontale Größe-Klasse der Compact, angegeben, sodass sie einer beliebigen Ansicht in der Anwendung anpassen können, die horizontal compact ist.

## <a name="uiimage"></a>UIImage

Eine andere Klasse, die Apple-Merkmal zu Sammlung hinzugefügt hat `UIImage`. In der Vergangenheit mussten Entwickler geben Sie eine @1X und @2x Version alle als Bitmap verfügbares grafische Assets, die sie hier in der Anwendung (z. B. ein Symbol) eingeschlossen werden sollen.

iOS 8 wurde erweitert, um die Entwickler mehrere Versionen eines Bilds in einem Image-Katalog auf Grundlage einer Auflistung Merkmal enthalten können. Der Entwickler kann z. B. ein kleineres Bild für die Arbeit mit einer Compact Trait-Klasse und ein vergrößern für jede andere Auflistung enthalten.

Wenn eines der Images verwendet wird in der ein `UIImageView` -Klasse, die Image-Ansicht zeigt automatisch die richtige Version des Bilds für die Trait-Auflistung. Wenn die Trait-Umgebung (z. B. der Benutzer Umschalten des Geräts vom Hochformat zum Querformat) geändert wird, wählt den Image View automatisch die neue Bildgröße entsprechen der neuen Trait-Sammlung, und ändern die Größe des die aktuelle Version der Images, angepasst angezeigt.

## <a name="uiimageasset"></a>UIImageAsset

Apple wurde hinzugefügt, eine neue Klasse IOS 8 namens `UIImageAsset` dem Entwickler noch präzisere Kontrolle über Bildauswahl gewähren.

Ein Bildobjekt dient als Wrapper für alle von den verschiedenen Versionen eines Bilds und ermöglicht dem Entwickler, die für ein bestimmtes Image bitten, die eine Auflistung der Eigenschaft entspricht, der in übergeben wurde. Bilder können hinzugefügt oder entfernt Sie aus einem Medienobjekt Bild für dynamische.

Weitere Informationen zu den Bildanlagen, finden Sie unter Apple [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) Dokumentation.

## <a name="combining-trait-collections"></a>Kombinieren von Trait-Auflistungen

Eine andere Funktion, die ein Entwickler auf Sammlungen des Merkmals ausführen können zwei wird zusammengefügt wird führen Sie in der kombinierten Auflistung, die nicht angegebene Werte aus einer Sammlung, wobei durch die angegebenen Werte in ein zweites ein ersetzt werden. Dies erfolgt mithilfe der `FromTraitsFromCollections` Methode der `UITraitCollection` Klasse.

Wie oben angegeben, eines der Merkmale in einem anderen angegeben und ist in einer der Sammlungen Eigenschaft nicht angegeben, wird der Wert der angegebenen Version festgelegt werden. Jedoch treten mehrere Versionen eines angegebenen Werts angegeben, den Wert aus der letzten Merkmal Sammlung den Wert werden, der verwendet wird.

## <a name="adaptive-view-controllers"></a>Adaptive Ansichtscontroller

In diesem Abschnitt behandelt die Details wie das iOS-Ansicht "und" View Controller die Konzepte der "traits" und Größenklassen automatisch in die Anwendungen der Entwickler anpassungsfähiger übernommen haben.

### <a name="split-view-controller"></a>Controller für geteilte Ansicht

Der View Controller-Klassen, die die meisten in iOS 8 geändert hat er die `UISplitViewController` Klasse. In der Vergangenheit verwendet die Entwickler häufig einen Controller für geteilte Ansicht auf der iPad-Version der Anwendung, und klicken Sie dann müssten sie die iPhone-Version der app eine völlig andere Version von ihre Hierarchie von Inhaltsansichten bereit.

IOS 8 die `UISplitViewController` -Klasse ist verfügbar auf beiden Plattformen (iPad und iPhone), dadurch kann den Entwickler einer View Controller-Hierarchie zu erstellen, die für iPhone und iPad ordnungsgemäß ausgeführt werden.

Wenn einem iPhone Querformat ist, werden seine Ansichten Seite-an-Seite, der Controller für geteilte Ansicht angezeigt, genau wie bei der auf einem iPad angezeigt wird.

### <a name="overriding-traits"></a>Überschreiben die "traits"

Merkmal Umgebungen, die vom übergeordneten Container auf den untergeordneten Containern, wie die folgende Abbildung zeigt einen Controller für geteilte Ansicht auf einem iPad im Querformat kaskadiert werden:

 [![](unified-storyboards-images/cascadingclasses01.png "Ein Controller für geteilte Ansicht auf einem iPad im Querformat")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Da das iPad einer normalen Größe-Klasse in die horizontale und vertikale Ausrichtung verfügt, wird der geteilten Ansicht sowohl die Masteransichten und Detailansichten angezeigt.

Auf einem iPhone, wobei die Größenklasse in beiden Ausrichtungen compact ist, wird der Controller für geteilte Ansicht nur die Detailansicht angezeigt, wie unten dargestellt:

 [![](unified-storyboards-images/cascadingclasses02.png "Der Controller für geteilte Ansicht wird nur die Detailansicht angezeigt.")](unified-storyboards-images/cascadingclasses02.png#lightbox)

In einer Anwendung, in dem der Entwickler sowohl die Master- und Detailtabelle auf einem iPhone im Querformat anzeigen möchte, muss der Entwickler ein übergeordneten Containers für den Controller für geteilte Ansicht einfügen und überschreiben die Trait-Auflistung. Wie in der folgenden Abbildung dargestellt:

 [![](unified-storyboards-images/cascadingclasses03.png "Der Entwickler muss fügen Sie einen übergeordneten Container der Split-View-Controller und überschreiben die Auflistung Merkmal")](unified-storyboards-images/cascadingclasses03.png#lightbox)

Ein `UIView` als übergeordnetes Element von der Controller für geteilte Ansicht festgelegt ist und die `SetOverrideTraitCollection` Methode wird aufgerufen, für die Sicht eine neue Trait-Sammlung übergeben und als Ziel der Controller für geteilte Ansicht. Überschreibt die neue Sammlung für die Eigenschaft der `HorizontalSizeClass`, wenn diese Option auf `Regular`, damit der Controller für geteilte Ansicht sowohl die Master- und Ansichten auf einem iPhone im Querformat angezeigt wird.

Beachten Sie, dass die `VerticalSizeClass` wurde `unspecified`, dem Was können Sie die neue Sammlung von Merkmal der Trait-Auflistung, auf das übergeordnete Element hinzugefügt werden eine `Compact VerticalSizeClass` für das untergeordnete Element Controller für geteilte Ansicht.

### <a name="trait-changes"></a>Trait-Änderungen

In diesem Abschnitt werden sehen, im Detail, wie die Trait-Auflistungen übergehen, bei der Änderung der Umgebung Merkmal. Z. B. wenn das Gerät vom Hochformat zum Querformat gedreht wird.

 [![](unified-storyboards-images/traittransitions01.png "Das Hoch-/ Querformat Übersicht über die Trait-Änderungen")](unified-storyboards-images/traittransitions01.png#lightbox)

IOS 8 führt zunächst eine Einrichtung zur Vorbereitung des Übergangs durchgeführt werden. Als Nächstes wird das System den Status des Übergangs animiert. Zum Schluss bereinigt iOS 8 temporäre Zustände während des Übergangs erforderlich.

iOS 8 bietet mehrere Rückrufe, mit denen der Entwickler die Änderung des Merkmals teilnehmen, wie in der folgenden Tabelle dargestellt:

|Phase|Rückruf|Beschreibung|
|--- |--- |--- |
|Setup|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Diese Methode ruft zu Beginn einer Änderung der Eigenschaft aufgerufen, bevor eine Auflistung der Eigenschaft auf den neuen Wert festgelegt wird.</li><li>Die Methode wird aufgerufen, wenn der Wert der Eigenschaft Auflistung geändert wurde, aber bevor alle Animationen stattfindet.</li></ul>|
|Animation|`WillTransitionToTraitCollection`|Der Übergang Koordinator, das an diese Methode übergeben wurde ein `AnimateAlongside` Eigenschaft, mit den Entwickler Animationen hinzufügen, die zusammen mit der standardanimationen ausgeführt werden können.|
|Leeren von Volltextkatalogen|`WillTransitionToTraitCollection`|Stellt eine Methode für Entwickler, die ihre eigenen Bereinigungscode enthalten, nachdem der Übergang erfolgt ist.|

Die `WillTransitionToTraitCollection` Methode eignet sich hervorragend für die Animation von Ansichtscontrollern zusammen mit der Eigenschaft Auflistung geändert wird. Die `WillTransitionToTraitCollection` Methode ist nur verfügbar, auf dem View Controller ( `UIViewController`) und nicht auf anderen Merkmals Umgebungen wie `UIViews`.

Die `TraitCollectionDidChange` eignet sich hervorragend für die Arbeit mit der `UIView` -Klasse, in dem der Entwickler die Benutzeroberfläche zu aktualisieren, wie die Merkmale ändern möchte.

### <a name="collapsing-the-split-view-controllers"></a>Reduzieren die Split-View-Controller

Nachdem wir nehmen einen genaueren Blick auf was geschieht, wenn ein Controller für geteilte Ansicht zu einer Ansicht eine Spalte aus einer Spalte zwei reduziert wird. Es gibt zwei Prozesse, die auftreten müssen, im Rahmen dieser Änderung wird:

-  Standardmäßig wird der Controller für geteilte Ansicht des ansichtscontrollers für die primäre wie die Sicht verwendet, nach dem Auftreten der Reduzierung. Entwickler kann dieses Verhalten überschreiben, durch Überschreiben der `GetPrimaryViewControllerForCollapsingSplitViewController` Methode der `UISplitViewControllerDelegate` und Bereitstellen eines Ansichtscontroller, die sie im reduzierten Zustand angezeigt werden soll.
-  Der sekundäre View-Controller muss in den primären Ansichtscontroller zusammengeführt. In der Regel muss der Entwickler nicht keinen Aktionen für diesen Schritt; der Controller für geteilte Ansicht enthält die automatische Behandlung von dieser Phase, die basierend auf dem Hardwaregerät. Möglicherweise gibt es jedoch einige besondere Fälle, in denen der Entwickler sollten für die Interaktion mit dieser Änderung. Aufrufen der `CollapseSecondViewController` Methode der `UISplitViewControllerDelegate` den master-View-Controller bei die Reduzierung, anstatt die Detailansicht angezeigt werden können.


### <a name="expanding-the-split-view-controller"></a>Erweitern den Controller für geteilte Ansicht

Nachdem wir nehmen einen genaueren Blick auf was geschieht, wenn es sich bei einem Controller für geteilte Ansicht aus dem reduzierten Zustand erweitert wird. Es gibt wieder zwei Phasen, die auftreten müssen:

-  Definieren Sie zuerst die neue primäre View-Controller. Standardmäßig wird der Controller für geteilte Ansicht automatisch aus der reduzierten Ansicht der primären View-Controller verwendet. In diesem Fall kann der Entwickler dieses Verhalten mithilfe überschreiben die `GetPrimaryViewControllerForExpandingSplitViewController` Methode der `UISplitViewControllerDelegate` .
-  Nachdem der primäre View-Controller ausgewählt wurde, muss der sekundäre View-Controller neu erstellt werden. In diesem Fall enthält der Controller für geteilte Ansicht automatische Behandlung von dieser Phase, die basierend auf dem Hardwaregerät. Entwickler kann dieses Verhalten überschreiben, durch den Aufruf der `SeparateSecondaryViewController` Methode der `UISplitViewControllerDelegate` .


In einem Controller für geteilte Ansicht, den primären Ansichtscontroller spielt eine in die erweitern und Reduzieren der Sichten durch die Implementierung der `CollapseSecondViewController` und `SeparateSecondaryViewController` Methoden der `UISplitViewControllerDelegate`. `UINavigationController` implementiert diese Methoden für das automatische push und pop des sekundären ansichtscontrollers an.

### <a name="showing-view-controllers"></a>Mit View-Controller

Eine weitere Änderung, die Apple iOS 8 hat ist auf die Weise, dass der Entwickler Ansichtscontroller angezeigt wird. In der Vergangenheit musste die Anwendung einen Endknoten-View-Controller (z. B. ein Tabellenansichtscontroller), und der Entwickler wurde gezeigt, eine andere (z. B. in der Antwort an den Benutzer Tippen auf eine Zelle), würde die Anwendung wieder durch die Controller-Hierarchie zu erreichen die Navigation-View-Controller, und rufen die `PushViewController` -Methode für die neue Ansicht angezeigt.

Diese präsentiert eine sehr enge Kopplung zwischen der Navigationscontroller und die Umgebung, die er in ausgeführt wurde. In iOS 8 hat Apple dies entkoppelt durch zwei neue Methoden zur Verfügung:

-  `ShowViewController` – Passt sich an, um den neuen ansichtscontroller, die basierend auf ihrer Umgebung anzuzeigen. Z. B. in einem `UINavigationController` sie einfach die neue Ansicht im Stapel überträgt. In einem Controller für geteilte Ansicht wird der neue View Controller als neue primäre Ansichtscontroller auf der linken Seite angezeigt. Wenn keine Container-ansichtscontroller vorhanden ist, wird die neue Ansicht als eine modale View Controller angezeigt.
-  `ShowDetailViewController` – Funktioniert in ähnlicher Weise wie `ShowViewController`, jedoch implementiert, nicht auf einen Controller für geteilte Ansicht, ersetzen die Detailansicht mit der neuen View-Controller, der übergeben wird. Wenn der Controller für geteilte Ansicht (wie in einem iPhone-Anwendung dargestellt werden kann) reduziert ist, werden der Aufruf zur umgeleitet der `ShowViewController` -Methode, und die neue Ansicht wird als primäre Ansichtscontroller angezeigt werden. In diesem Fall wird die neue Ansicht, wenn kein Container-View-Controller vorhanden ist, als eine modale View Controller angezeigt.


Diese Methoden arbeiten, indem Sie auf der Blattebene-View-Controller ab, und die Hierarchie von Inhaltsansichten aufwärts, bis sie den richtigen Container-View-Controller, behandeln die Anzeige der neuen Ansicht finden.

Entwickler können implementieren `ShowViewController` und `ShowDetailViewController` in ihre eigenen benutzerdefinierten View-Controller abgerufen automatisiert die gleiche Funktionalität, die `UINavigationController` und `UISplitViewController` enthält.

### <a name="how-it-works"></a>So funktioniert es

In diesem Abschnitt werden wir sehen Sie sich wie diese Methoden in iOS 8 tatsächlich implementiert werden. Zuerst sehen wir uns das neue `GetTargetForAction` Methode:

 [![](unified-storyboards-images/gettargetforaction.png "Die neue GetTargetForAction-Methode")](unified-storyboards-images/gettargetforaction.png#lightbox)

Diese Methode führt die Hierarchiekette, bis der richtige Container-View-Controller gefunden wird. Zum Beispiel:

1.  Wenn eine `ShowViewController` -Methode aufgerufen wird, den ersten Ansichtscontroller, in der Kette, die diese Methode implementiert wird der Navigationscontroller, damit sie als übergeordnetes Element der neuen Ansicht verwendet wird.
1.  Wenn eine `ShowDetailViewController` Methode wurde aufgerufen, stattdessen wird der Controller für geteilte Ansicht den ersten Ansichtscontroller, implementieren, damit sie als übergeordnetes Element verwendet wird.


Die `GetTargetForAction` Methode funktioniert durch Suchen eines Ansichtscontrollers, der eine bestimmte Aktion implementiert, und klicken Sie dann bitten, View-Controller, wenn sie diese Aktion empfangen möchte. Da diese Methode öffentlich ist, können Entwickler ihre eigenen benutzerdefinierten Methoden, die Funktion genau wie die integrierten in erstellen `ShowViewController` und `ShowDetailViewController` Methoden.

## <a name="adaptive-presentation"></a>Adaptive Präsentation

In iOS 8 hat Apple Popover Präsentationen ( `UIPopoverPresentationController`) adaptive ebenfalls. Damit einen Popover Presentation-View-Controller einer normalen Popover-Ansicht in einer normalen Klasse für die Größe wird automatisch angezeigt werden ihn jedoch, Vollbildmodus in einer horizontal compact Größe-Klasse anzeigen (z. B. auf einem iPhone).

Um die Änderungen innerhalb des Systems für unified Storyboards zu unterstützen, ein neues Controllerobjekt erstellt, um die der angegebene View-Controller verwalten – `UIPresentationController`. Dieser Controller wird ab dem Zeitpunkt erstellt, die die View-Controller angezeigt wird, bis es geschlossen wird. Wie es zum Verwalten von Klasse ist, können sie eine übergeordnete Klasse über die View-Controller gilt als geräteänderungen reagiert werden soll, die die View-Controller (z. B. Ausrichtung) steuert betreffen, welche werden dann Herd zurück in den Ansichtscontroller des Controllers für die Präsentation.

Wenn der Entwickler stellt eine View Controller mithilfe der `PresentViewController` -Methode, die Verwaltung des Prozesses Präsentation klebt `UIKit`. UIKit behandelt (unter anderem) die richtigen Controller für das Format, das erstellt wird, mit die einzige Ausnahme ist, wenn eine View-Controller den Stil festgelegt hat `UIModalPresentationCustom`. Hier kann die Anwendung kann eigene PresentationController statt weisen die `UIKit` Controller.

### <a name="custom-presentation-styles"></a>Benutzerdefinierte Präsentationsformate

Mit einem benutzerdefinierten Präsentation haben Entwickler die Möglichkeit, einen benutzerdefinierten Präsentation-Controller verwenden. So ändern Sie das Aussehen und Verhalten von der Ansicht, die es zu allied ist, kann dieser benutzerdefinierten Controller verwendet werden.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Arbeiten mit Größenklassen

Adaptive Fotos Xamarin-Projekt, das in diesem Artikel enthalten ist, bietet ein funktionsfähiges Beispiel der Verwendung von Größenklassen und Adaptive Ansichtscontroller in iOS 8 Unified Interface-Anwendungen.

Während die Anwendung die Benutzeroberfläche vollständig aus dem Code im Gegensatz zur Verwendung der IOS-Designer erstellt, und erstellen eine einheitliche Storyboards, die gleichen Verfahren anwenden. Weiter unten in diesem Artikel zeigen wir, wie Sie Größenklassen mit Unified Storyboards und den iOS-Designer in einer Xamarin-Anwendung verwenden.

Nun werfen wir einen genaueren Blick auf wie das Projekt für die Adaptive Fotos Reihe Größenklasse Features in iOS 8 zum Erstellen einer Anwendung Adaptive implementiert.

### <a name="adapting-to-trait-environment-changes"></a>Zur Anpassung an den Umgebungsänderungen Merkmal

Bei der Ausführung der Anwendung Adaptive Fotos auf einem iPhone, wenn der Benutzer das Gerät vom Hochformat zum Querformat dreht, wird der Controller für geteilte Ansicht sowohl die Master-als auch die Details anzeigen:

 [![](unified-storyboards-images/rotation.png "Der Controller für geteilte Ansicht zeigt sowohl den Master und Details anzuzeigen, wie hier zu sehen")](unified-storyboards-images/rotation.png#lightbox)

Dies erfolgt durch Überschreiben der `UpdateConstraintsForTraitCollection` -Methode der Ansichtscontroller und passen die Einschränkungen basierend auf den Wert der `VerticalSizeClass`. Zum Beispiel:

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

Wenn der Controller für geteilte Ansicht auf die Anwendung von wechselt Adaptive Fotos zu erweiterten reduziert, Animationen werden hinzugefügt, die standardanimationen indem überschreiben die `WillTransitionToTraitCollection` Methode des ansichtscontrollers. Zum Beispiel:

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

### <a name="overriding-the-trait-environment"></a>Überschreiben die Trait-Umgebung

Wie oben gezeigt, zwingt die Adaptive Fotos-Anwendung den Controller für geteilte Ansicht zum Anzeigen der Details und die master-Ansichten aus, wenn die iPhone-Geräte in der querformatansicht ist.

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

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Erweitern und reduzieren den Controller für geteilte Ansicht

Weiter untersuchen wir, wie das Verhalten erweiterte und reduzierende der Controller für geteilte Ansicht in Xamarin implementiert wurde. In der `AppDelegate`, wenn der Controller für geteilte Ansicht erstellt wird, der Delegat wird zum Verarbeiten dieser Änderungen zugewiesen:

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

Die `SeparateSecondaryViewController` Methode überprüft, ob ein Foto angezeigt wird, und Aktionen basierend auf diesen Zustand führt. Ist kein Foto reduziert angezeigt wird der sekundäre View-Controller, damit der Master-View-Controller angezeigt wird.

Die `CollapseSecondViewController` Methode wird verwendet, wenn der Controller für geteilte Ansicht erweitern um festzustellen, ob die Fotos, die auf dem Stapel vorhanden sind, wenn Sie also zurück zu dieser Ansicht wird reduziert.

### <a name="moving-between-view-controllers"></a>Verschieben zwischen Ansichtscontrollern

Als Nächstes sehen wir uns an, wie die Anwendung Adaptive Fotos zwischen ansichtscontrollern verschoben wird. In der `AAPLConversationViewController` Klasse, wenn der Benutzer eine Zelle in der Tabelle auswählt der `ShowDetailViewController` Methode wird aufgerufen, um die Details anzuzeigen:

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

### <a name="displaying-disclosure-indicators"></a>Anzeigen der Offenlegung von Indikatoren

In der Adaptive Fotogalerie-Anwendung ein, es gibt verschiedene Stellen, die Offenlegung von Indikatoren sind, in dem ausgeblendet oder angezeigt, basierend auf Änderungen in der Umgebung Merkmal. Dies erfolgt durch den folgenden Code:

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

Diese werden implementiert, mit der `GetTargetViewControllerForAction` Methode, die oben genannten ausführlich erläutert.

Ein Tabellenansichtscontroller Anzeigen von Daten ist, verwendet es die Methoden, die über implementiert werden, um festzustellen, ob ein Push passiert ist und ob Sie ein- oder Ausblenden der Offenlegung von Indikator entsprechend an:

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

Apple hat einen neuen Benachrichtigungstyp hinzugefügt, für die Arbeit mit Größenklassen und -Merkmal-Umgebungen in einem Controller für geteilte Ansicht, `ShowDetailTargetDidChangeNotification`. Diese Benachrichtigung wird gesendet, wenn sich das Ziel-Detail-Ansicht für einen Controller für geteilte Ansicht ändert, z. B. wenn der Controller erweitert oder reduziert.

Die Anwendung Adaptive Fotos verwendet diese Benachrichtigung so aktualisieren den Status des Indikators, Offenlegung von aus, wenn der Detail-View-Controller geändert:

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

Nehmen einen genaueren Blick auf die Anwendung Adaptive Fotos, um alle Möglichkeiten, Größenklassen, Trait-Sammlungen und Adaptive View-Controller kann verwendet werden, um eine einheitliche Anwendung problemlos in Xamarin.iOS zu erstellen.

## <a name="unified-storyboards"></a>Einheitliche Storyboards

Neue iOS 8 ermöglichen es Entwicklern, erstellen Sie einen Unified Storyboards Storyboard-Datei, die auf dem iPhone und iPad-Geräte angezeigt werden kann, durch eine Ausrichtung auf mehrere Größenklassen vereinheitlicht. Mithilfe von Unified Storyboards, der Entwickler weniger bestimmten UI-Code geschrieben und hat nur eine Benutzeroberfläche entwerfen, erstellen und verwalten.

Die wichtigsten Vorteile von Unified Storyboards sind:

-  Verwenden Sie die gleichen Storyboarddatei für iPhone und iPad.
-  Stellen Sie für iOS 6 und iOS 7 Abwärtskompatibilität bereit.
-  Das Layout für verschiedene Geräte, Ausrichtungen und BS-Versionen über den Xamarin.IOS-Designer eine Vorschau anzeigen.

Dieses Feature wird vollständig in Visual Studio für Mac unterstützt.

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Größenklassen aktivieren

Standardmäßig werden alle neuen Xamarin.iOS-Projekts uns Größenklassen. Um Größenklassen und Adaptive Übergänge in ein Storyboard aus der ein älteres Projekt verwenden möchten, müssen sie zuerst dem Xcode 6 Unified Storyboards-Format aus in der iOS-Designer konvertiert werden.

Sie dadurch öffnen Sie das Storyboard in der iOS-Designer und Kontrollkästchen konvertiert werden die **verwenden Größenklassen** Kontrollkästchen:

 [![](unified-storyboards-images/sizeclass01.png "Aktivieren Sie das Kontrollkästchen Größenklassen verwenden")](unified-storyboards-images/sizeclass01.png#lightbox)

Der iOS-Designer wird bestätigen Sie, dass der Entwickler möchte, um das Format des Storyboards mit Größenklassen konvertieren:

 [![](unified-storyboards-images/sizeclass02.png "Die Verwendung von Größenklassen-Warnung")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Automatisches Layout muss auch auf Größenklassen ordnungsgemäße überprüft werden.

### <a name="generic-device-types"></a>Generische Gerätetypen

Sobald das Storyboard zur Verwendung von Größenklassen konvertiert wurde, es wird wieder angezeigt in der Entwurfsoberfläche und den **anzeigen als** Gerät werden generische:

 [![](unified-storyboards-images/sizeclass03.png "Als ein generisches Gerät anzeigen")](unified-storyboards-images/sizeclass03.png#lightbox)

Wenn der generische Gerätetyp ausgewählt ist, werden alle View-Controller zu einem Quadrat 600 x 600 Größe geändert werden. Diese Quadrat stellt die Größe der beliebiger Breite und in jeder beliebigen Höhe dar. Bei der iOS-Designer in diesem Modus ist, gilt die Änderungen für alle Klassen Größe.

Der Entwickler hat auch die Option zum Anzeigen der Entwurfsoberfläche als ein iPhone:

 [![](unified-storyboards-images/sizeclass04.png "Anzeigen von der Entwurfsoberfläche als einem iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Oder wie einem iPad anzeigen:

 [![](unified-storyboards-images/sizeclass05.png "Anzeigen von der Entwurfsoberfläche als einem iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Wählen Sie eine Größenklasse

Die Schaltfläche "Größe Klassenselektor" ist die linke obere Ecke der Entwurfsoberfläche (in der Nähe der Dropdownliste anzeigen als). Es kann der Entwickler wählen, welche Größenklassen derzeit bearbeitet wird:

 [![](unified-storyboards-images/sizeclass06.png "Wählen Sie eine Größenklasse")](unified-storyboards-images/sizeclass06.png#lightbox)

Die Auswahl enthält die Größenklasse Auswahl als eine 3 x 3-Raster. Jede der Quadrate im Raster stellt eine Kombination aus einer Breite und eine Höhe-Klasse dar. Das Quadrat Center wählt aus, die jede Width/Any Höhe Größe-Klasse (die die Standardansicht für eine Unified Storyboards). Bei diesem Quadrat ausgewählt ist, wird der Entwickler das Standardlayout, bearbeiten, die von den anderen Konfigurationen geerbt wird.

Das Quadrat in der oberen linken Ecke des Rasters darstellt, die Compact Größenklasse der Breite/Compact Höhe:

 [![](unified-storyboards-images/sizeclass07.png "Die Breite/Compact Compact Höhe Größenklasse")](unified-storyboards-images/sizeclass07.png#lightbox)

Dieser Modus entspricht einem iPhone im Querformat. Das Quadrat in der unteren rechten Ecke des Rasters regulären Breite/regulären Höhe Größe stellt die Klasse dar, die einem iPad darstellt:

 [![](unified-storyboards-images/sizeclass08.png "Die normale Breite/regulären Höhe Größenklasse")](unified-storyboards-images/sizeclass08.png#lightbox)

Um das Layout für ein iPhone im Hochformat zu bearbeiten, wählen Sie das Quadrat in der unteren linken Ecke aus. Dies stellt die Compact Größenklasse der Breite/regulären Höhe dar:

 [![](unified-storyboards-images/sizeclass09.png "Die Breite/regulären Compact Höhe Größenklasse")](unified-storyboards-images/sizeclass09.png#lightbox)

Klicken Sie in das Quadrat, um es auszuwählen, und der Entwurfsoberfläche ändert sich die Größe des View-Controller, entsprechend die neue Auswahl:

 [![](unified-storyboards-images/sizeclass10.png "Die Entwurfsoberfläche ändert sich die Größe des View-Controller, um die neue Auswahl überein, siehe")](unified-storyboards-images/sizeclass10.png#lightbox)

Finden Sie im Abschnitt "Size Class" in diesem Artikel Weitere Informationen auf Größenklassen und deren Auswirkung auf Layout für iPhones und iPads.

### <a name="adaptive-segue-types"></a>Adaptive Segue Typen

Wenn der Entwickler hat Storyboards, bevor Sie verwendet, sie werden mit den vorhandenen Segue vertraut **Push**, **modale** und **Popover**. Wenn auf eine einheitliche Storyboarddatei Größenklassen aktiviert sind, werden die folgenden adaptiven Segue Typen (die die neue View Controller-API aus, die weiter oben erläuterten entsprechen) zur Verfügung gestellt: **Anzeigen** und **zeigen Details**.

> [!IMPORTANT]
> Wenn Größenklassen aktiviert sind, alle vorhandenen Übergänge werden in die neuen Typen konvertiert werden.

Betrachten Sie beispielsweise ein iOS 8-Anwendung, eines Unified Storyboards mit einem Controller für geteilte Ansicht verwendet werden, die ein einfaches Spiel Navigationsmenü in der Master-Ansicht. Wenn der Benutzer auf eine Schaltfläche klickt, sollte Ansichtscontroller für des ausgewählten Elements im Abschnitt "Details" der der Controller für geteilte Ansicht angezeigt werden, bei der Ausführung auf einem iPad. Auf einem iPhone sollte des Elements des View-Controller auf den Navigationsstapel übertragen.

Um diesen Effekt zu erreichen, in der iOS-Designer-Steuerelement, klicken Sie auf die Schaltfläche und ziehen Sie eine Zeile der View-Controller, der angezeigt werden. Wenn die Maustaste losgelassen wird, wählen Sie `Show Detail` im Segue-Typ-Popupmenü:

 [![](unified-storyboards-images/segue01.png "Wählen Sie im Popupmenü Typ Segue Details anzeigen")](unified-storyboards-images/segue01.png#lightbox)

Der neue Segue wird zwischen der Schaltfläche und die View-Controller erstellt. Führen Sie die Anwendung jetzt in der iPhone-Simulator, und klicken Sie im Hauptmenü angezeigt:

 [![](unified-storyboards-images/segue02.png "Hauptmenü")](unified-storyboards-images/segue02.png#lightbox)

Klicken Sie auf die **Spiel wählen** Schaltfläche und die View Controller des Elements wird auf den Navigationsstapel übertragen werden:

 [![](unified-storyboards-images/segue03.png "Die View Controller-Elemente werden in den Navigationsstapel übertragen wie gezeigt")](unified-storyboards-images/segue03.png#lightbox)

Beenden Sie die iPhone-Simulator, und führen Sie die Anwendung auf dem iPad-Simulator. Wechseln Sie in das Querformat gegenüber und in der Vorgang wird ein Menü angezeigt:

 [![](unified-storyboards-images/segue04.png "Im Hauptmenü angezeigt")](unified-storyboards-images/segue04.png#lightbox)

Klicken Sie in diesem Fall auf die **Spiel wählen** Schaltfläche und die View Controller des Elements in der der Detailabschnitt der der Controller für geteilte Ansicht angezeigt:

 [![](unified-storyboards-images/segue05.png "Die Elemente View-Controller, die im Abschnitt \"Details\" der der Controller für geteilte Ansicht")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Ein Element ausschließen aus einer Größenklasse

Es gibt Situationen, wenn ein angegebenes Element (z. B. eine Sicht, Control oder Einschränkung) nicht innerhalb einer bestimmten Größenklasse erforderlich ist. Um ein Element aus einer Klasse Größe auszuschließen, wählen Sie das gewünschte Element zu schließen Sie in der **Entwurfsoberfläche**. Führen Sie einen Bildlauf zum unteren Rand der **Property Explorer** , und klicken Sie auf die **Zahnradsymbol** Dropdown-Menü. Wählen Sie die Kombination von **Breite** und **Höhe** , schließen Sie das Element aus:

[![](unified-storyboards-images/exclude-a.png "Wählen Sie die Kombination aus Breite und Höhe")](unified-storyboards-images/exclude-a.png#lightbox)

Ein neues *Ausschluss Groß-/Kleinschreibung* wird hinzugefügt werden, auf das Element am unteren Rand der **Property Explorer**. Deaktivieren Sie als Nächstes die **installiert** Kontrollkästchen für die angegebene Größe-Klasse:

[![](unified-storyboards-images/exclude-b.png "Deaktivieren Sie das Kontrollkästchen installiert")](unified-storyboards-images/exclude-b.png#lightbox)

Wechseln von der Entwurfsoberfläche auf die Breite und Höhe, die das Element von ausgeschlossen wurde, wurde Sie aus der angegebenen Größe-Klasse, aber nicht das gesamte Design der Benutzeroberfläche entfernt:

 [![](unified-storyboards-images/exclude02.png "Wechseln von der Entwurfsoberfläche auf die Breite und Höhe, die das Element von ausgeschlossen wurde")](unified-storyboards-images/exclude02.png#lightbox)

Wechseln zurück an die Größenklasse Any Width/Any Höhe und das Element ist weiterhin gültig:

 [![](unified-storyboards-images/exclude03.png "Wechseln zurück an die Höhe der Any Width/Any Größenklasse")](unified-storyboards-images/exclude03.png#lightbox)

Wenn die Anwendung auf dem iPad Simulator ausgeführt wird, wird das Element angezeigt:

 [![](unified-storyboards-images/exclude04.png "Das Element angezeigt wird, wenn der ausgeführten app auf dem iPad-Simulator")](unified-storyboards-images/exclude04.png#lightbox)

Und wenn die Anwendung auf dem iPhone-Simulator ausgeführt wird, ist das Element nicht vorhanden:

 [![](unified-storyboards-images/exclude05.png "Das Element fehlt, wenn die ausgeführte app in der iPhone-Simulator")](unified-storyboards-images/exclude05.png#lightbox)

Um einen Fall Ausschluss aus einem Element zu entfernen, wählen Sie einfach das Element in der **Entwurfsoberfläche**, führen Sie einen Bildlauf zum unteren Rand der **Property Explorer** , und klicken Sie auf die **-** Schaltfläche neben der Fall zu entfernen.

Um eine Implementierung von Unified Storyboards anzuzeigen, sehen Sie sich die `UnifiedStoryboard` Beispiel Xamarin iOS 8-Anwendung, die in diesem Dokument angefügt.

## <a name="dynamic-launch-screens"></a>Dynamische Startbildschirme

Die Start-Bildschirm-Datei wird als Begrüßungsbildschirm angezeigt, während eine iOS-Anwendung ausgeführt werden, um dem Benutzer Feedback bereitzustellen, die die app tatsächlich starten-registriert ist. Vor iOS 8, hätte der Entwickler sollen mehrere `Default.png` Bildanlagen für jedes Gerät Typ, Ausrichtung und die Bildschirmauflösung, die die Anwendung ausgeführt werden sollen. Z. B. `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`usw.

Umgestaltung in der neuen iPhone 6 und iPhone 6 Plus Geräte (und die bevorstehende Apple Watch) mit allen vorhandenen iPhone und iPad-Geräte, stellt dies ein großes Array aus unterschiedlichen Größen, Ausrichtungen und Auflösungen von `Default.png` Start Bildschirm Bildanlagen, die müssen erstellt und verwaltet werden. Darüber hinaus diese Dateien können sehr groß sein und werden das Ergebnis anwendungsbündel, erhöhen die Zeitspanne, die erforderlich, um die Anwendung im iTunes App Store ("möglicherweise" dafür, über ein Mobilfunknetz übermittelt werden können) herunterzuladen "Müll" und erhöhen die Menge an Speicherplatz auf dem Gerät des Benutzers erforderlich.

Neue IOS 8, kann der Entwickler erstellen Sie eine einzelne, unteilbare `.xib` -Datei in Xcode, die Automatisches Layout und die Größenklassen verwendet werden, zum Erstellen einer *dynamische Startbildschirm* funktioniert, die für jedes Gerät, Auflösung und Ausrichtung. Dies reduziert nicht nur die Menge der Arbeit des Entwicklers zum Erstellen und verwalten alle erforderlichen Bildanlagen erforderlich, aber es erheblich reduziert die Größe des installierten Paket der Anwendung.


Dynamische Startbildschirme haben die folgenden Überlegungen und Einschränkungen:

 - Verwenden Sie nur `UIKit` Klassen.
 - Verwenden Sie eine einzelnen Stamm-Sicht, ist eine `UIView` oder `UIViewController` Objekt.
 - Nehmen Sie keine Verbindungen mit den Code der Anwendung nicht (Fügen Sie nicht **Aktionen** oder **Outlets**).
 - Fügen Sie nicht `UIWebView` Objekte.
 - Verwenden Sie keine benutzerdefinierten Klassen.
 - Verwenden Sie keine Common Language Runtime-Attribute.

Mit den oben aufgeführten Richtlinien beachten Sie wir sehen uns eine dynamische Startbildschirm zu einem vorhandenen Xamarin-iOS 8-Projekt hinzufügen.

Führen Sie folgende Schritte aus:

1. Open **Visual Studio für Mac** und laden die **Lösung** den dynamischen Startbildschirm auf Hinzufügen.
2. In der **Projektmappen-Explorer**, mit der rechten Maustaste die `MainStoryboard.storyboard` und wählen Sie **Öffnen mit** > **Xcode Interface Builder**:

    [![](unified-storyboards-images/dls01.png "Mit Xcode Interface Builder öffnen")](unified-storyboards-images/dls01.png#lightbox)
3. Wählen Sie in Xcode **Datei** > **neu** > **Datei...** :

    [![](unified-storyboards-images/dls02.png "Wählen Sie die Datei / neu")](unified-storyboards-images/dls02.png#lightbox)
4. Wählen Sie **iOS** > **Benutzeroberfläche** > **Startbildschirm** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](unified-storyboards-images/dls03.png "Wählen Sie die iOS / Benutzeroberfläche / Startbildschirm")](unified-storyboards-images/dls03.png#lightbox)
5. Nennen Sie die Datei `LaunchScreen.xib` , und klicken Sie auf die **erstellen** Schaltfläche:

    [![](unified-storyboards-images/dls04.png "Nennen Sie die Datei LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. Bearbeiten Sie den Entwurf des Startbildschirms, indem hinzufügen grafischer Elemente und Verwenden von Layout-Einschränkungen, die sie für Geräte, Ausrichtungen und Bildschirmgrößen positionieren:

    [![](unified-storyboards-images/dls05.png "Bearbeiten den Entwurf des Startbildschirms")](unified-storyboards-images/dls05.png#lightbox)
7. Speichern Sie die Änderungen an `LaunchScreen.xib`.
8. Wählen Sie die **Anwendungen Ziel** und **allgemeine** Registerkarte:

    [![](unified-storyboards-images/dls06.png "Wählen Sie das Ziel der Anwendungen und die Registerkarte \"Allgemein\"")](unified-storyboards-images/dls06.png#lightbox)
9. Klicken Sie auf die **wählen Sie "Info.plist"** Klicken die `Info.plist` für die Xamarin-app und klicken Sie auf die **auswählen** Schaltfläche:

    [![](unified-storyboards-images/dls07.png "Wählen Sie die Datei \"Info.plist\" für die Xamarin-app")](unified-storyboards-images/dls07.png#lightbox)
10. In der **App-Symbole und Startbilder** , öffnen Sie im Abschnitt der **starten Bildschirm Datei** Dropdownliste, und wählen Sie die `LaunchScreen.xib` oben erstellt haben:

    [![](unified-storyboards-images/dls08.png "Wählen Sie die LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. Speichern Sie die Änderungen in der Datei und zurück zu Visual Studio für Mac.
12. Warten Sie, bis Visual Studio für Mac, um den Vorgang abzuschließen, Synchronisieren von Änderungen mit Xcode.
13. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Ressource** Ordner, und wählen **hinzufügen** > **Dateien hinzufügen...** :

    [![](unified-storyboards-images/dls09.png "Wählen Sie Add / Hinzufügen von Dateien...")](unified-storyboards-images/dls09.png#lightbox)
14. Wählen Sie die `LaunchScreen.xib` oben erstellte Datei, und klicken Sie auf die **öffnen** Schaltfläche:

    [![](unified-storyboards-images/dls10.png "Wählen Sie die Datei LaunchScreen.xib")](unified-storyboards-images/dls10.png#lightbox)
15. Erstellen Sie die Anwendung.

### <a name="testing-the-dynamic-launch-screen"></a>Testen die dynamische-Startbildschirm

Klicken Sie in Visual Studio für Mac wählen Sie iPhone 4 Retina-Simulator, und führen Sie die Anwendung. Das richtige Format und die Ausrichtung wird die dynamische Startbildschirm angezeigt:

[![](unified-storyboards-images/dls11.png "Die dynamische Startbildschirm angezeigt, in die vertikale Ausrichtung")](unified-storyboards-images/dls11.png#lightbox)

Beenden Sie die Anwendung in Visual Studio für Mac und wählen Sie iPad iOS 8-Gerät aus. Führen Sie die Anwendung, und der Startbildschirm wird für dieses Gerät und Ausrichtung ordnungsgemäß formatiert sein:

[![](unified-storyboards-images/dls12.png "Die dynamische Startbildschirm angezeigt, in die horizontale Ausrichtung")](unified-storyboards-images/dls12.png#lightbox)

Zurück zu Visual Studio für Mac, und beenden Sie die Anwendung ausgeführt wird.

### <a name="working-with-ios-7"></a>Arbeiten mit iOS 7

Um die Abwärtskompatibilität mit iOS 7 zu, schließen Sie einfach die üblichen `Default.png` Bildanlagen wie gewohnt in die iOS 8-Anwendung. iOS zurück zum vorherigen Verhalten und diese Dateien als Startbildschirm verwenden, wenn auf einem iOS 7-Gerät ausgeführt.

Um eine Implementierung einer dynamischen Startbildschirm in Xamarin anzuzeigen, sehen Sie sich die [dynamische Startbildschirme](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) Beispiel iOS 8-Anwendung, die in diesem Dokument angefügt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben einen kurzen Blick auf Größenklassen und deren Auswirkung auf Layout auf iPhone und iPad-Geräten. Es erläutert die Funktionsweise von "traits" "," Trait-Umgebungen "und" Trait-Sammlungen mit Größenklassen Unified Schnittstellen erstellt werden. Es dauerte kurzen Überblick über Adaptive View-Controller und deren Funktionsweise mit Größenklassen in Unified-Schnittstellen. Sie finden ihn auf die Implementierung von Größenklassen und einheitliche Schnittstellen komplett C# Code in einer Xamarin iOS 8-Anwendung.

Abschließend in diesem Artikel behandelt die Grundlagen der Erstellung von Unified Storyboards mit dem Xamarin.IOS-Designer, das auf iOS-Geräten funktioniert, und erstellen eine einzelne dynamische Startbildschirm, als Startbildschirm auf jedem iOS 8-Gerät angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Adaptive Fotos (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro-Beispiel](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Dynamische Startbildschirme (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Dynamische Layouts in iOS8 - weiterentwickeln 2014 (Video)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
