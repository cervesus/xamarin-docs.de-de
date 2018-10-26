---
title: iOS 7 Übersicht über die Benutzeroberfläche
description: iOS 7 führt eine Unmenge von Änderungen an der Benutzeroberfläche. In diesem Artikel werden einige der größeren Änderungen in die visuelle Darstellung von Steuerelementen und in den APIs, die das neue Design zu unterstützen.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 132265c27e1d1ba3b8f3fc8db10d7b3cfa746197
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109015"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 Übersicht über die Benutzeroberfläche

_iOS 7 führt eine Unmenge von Änderungen an der Benutzeroberfläche. In diesem Artikel werden einige der größeren Änderungen in die visuelle Darstellung von Steuerelementen und in den APIs, die das neue Design zu unterstützen._

iOS 7 liegt der Schwerpunkt auf Inhalte über Chrome. Elemente der Benutzeroberfläche in iOS 7 betonen Chrome durch Entfernen von Attributen wie z. B. überflüssige Rahmen, Statusleisten und Navigationsleisten, die verringert sich die Größe des bildschirmplatzes, die von Inhalt von Ansichten verwendet wird. In iOS 7 Inhalt soll den gesamten Bildschirm zu verwenden.

iOS 7 werden mehrere andere Änderungen eingeführt: Farbe wird verwendet, um die Elemente der Benutzeroberfläche, durch Attribute wie z. B. Schaltfläche Rahmen zu unterscheiden. Viele Elemente, z. B. Navigationsleisten und Statusleisten, sind jetzt entweder weichgezeichnete und lichtdurchlässiger Ansichten "oder" transparent mit Inhalt und Bereich darunter an. Dieser Inhalt Ansichten rendern über weichgezeichnete Balken, vermitteln einen Eindruck von Tiefe auf der Benutzeroberfläche.

Dieser Artikel behandelt einige der Änderungen an Elementen der Benutzeroberfläche in iOS 7 als auch für verschiedene APIs, die neue Benutzeroberfläche verknüpft.

## <a name="view-and-control-changes"></a>Anzeigen und Steuerelement geändert wird

Alle Ansichten in UIKit entsprechen. das neue Aussehen und Verhalten von iOS 7. In diesem Abschnitt werden einige der Änderungen an diesen Ansichten als auch die verwandten APIs, die geändert wurden, um die neue Benutzeroberfläche zu unterstützen.

### <a name="uibutton"></a>UIButton

Schaltflächen, die von der `UIButton` -Klasse sind jetzt hat keinen Rahmen, und keine Hintergrundupdates standardmäßig, wie unten dargestellt:

 ![](ios7-ui-images/button.png "Beispiel-UIButton")

Die `UIButtonType.RoundedRect` Format ist veraltet. Wenn in iOS 7 verwendet `UIButtonType.RoundedRect` führt zu `UIButtonType.System` verwendet wird, erzeugt das Standardformat für die Schaltfläche ohne Hintergrund oder Rand angezeigt, wie oben gezeigt.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Ähnlich wie `UIButton`, Leiste die Schaltflächen sind auch randlose, auf den neuen Standardwert `UIBarButtonItemStyle.Plain` unten gezeigten Format:

 ![](ios7-ui-images/barbuttonplain.png "Beispiel-UIBarButtonItem")

Darüber hinaus die `UIBarButtonItemStyle.Bordered` Format ist veraltet. Festlegen von `UIBarButtonItemStyle.Bordered` in iOS 7 führt dazu, dass die `UIBarButtonItemStyle.Plain` Stil verwendet wird.

Die `UIBarButtonItemStyle.Done` Stil nicht veraltet. Es wird jedoch auch eine Schaltfläche "randlose" nur mit einem fett formatierter Text wie gezeigt erstellen:

 ![](ios7-ui-images/barbuttondone.png "Beispiel-UIBarButtonItem im Done-Format")

### <a name="uialertview"></a>UIAlertView

Zusätzlich zu der Änderung Stil für das Aussehen und Verhalten der neuen iOS 7 werden die Warnungsansichten Anpassung über Unteransicht nicht mehr unterstützt. Obwohl `UIAlertView` erbt `UIView`, wird beim Aufruf `AddSubview` auf eine `UIAlertView` hat keine Auswirkungen. Beachten Sie z. B. folgenden Code:

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

Dadurch wird eine standardmäßige Warnungsansichten ein, mit der Unteransicht ignoriert werden, erzeugt, wie unten dargestellt:

 ![](ios7-ui-images/alert.png "Beispiel-uialertview-Element")
 
 Hinweis: Uialertview-Element wurde in iOS 8 veraltet. Anzeigen der [Warnungscontroller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) Anleitung zur Verwendung einer Warnungsansicht, in iOS 8 und höher.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Segmentierte Steuerelemente in iOS 7 sind transparent und tönungsfarbe zu unterstützen. Die Farbe der Farbton wird für die Farbe von Text und Rahmen verwendet. Wenn ein Segment ausgewählt ist, wird die Farbe mit der Farbton Farbe zum Markieren des ausgewählten Segments, wie unten dargestellt zwischen den Hintergrund und dem Text, ausgetauscht:

 ![](ios7-ui-images/segmentedcontrol.png "Beispiel-UISegmentedControl")

Darüber hinaus die `UISegmentedControlStyle` in iOS 7 veraltet.

### <a name="picker-views"></a>Auswahl-Ansichten

Die API für die Auswahl Ansichten ist größtenteils unverändert; Allerdings Status iOS 7-Entwurfsrichtlinien jetzt Auswahl Ansichten Inline dargestellt werden soll, anstatt wie eingabeansichten von animiert unteren Rand des Bildschirms oder über einen neuen Domänencontroller auf einen navigationscontroller Stapel abgelegt, wie in vorherigen iOS-Versionen. Dies kann in der Kalender-app angezeigt werden:

 ![](ios7-ui-images/inlinepicker.png "Dies kann in der Kalender-app angezeigt werden")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Die Suchleiste wird jetzt angezeigt, in der Navigation Statusleiste an, wenn die `UISearchDisplayController.DisplaysSearchBarInNavigationBar` -Eigenschaftensatz auf "true". Bei Festlegung auf False – der Standardwert – die Navigationsleiste wird ausgeblendet, wenn es sich bei der Suche Controller angezeigt wird.

Der folgende Screenshot zeigt die Suchleiste innerhalb einer `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Beispiel-UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Die APIs im Zusammenhang mit `UITableView` wird in erster Linie unverändert, jedoch das Format wurde geändert, drastisch um die neue Benutzeroberfläche entsprechen. Außerdem ist die interne Ansichtshierarchie unterscheiden. Diese Änderung hat keine Auswirkung auf die meisten apps, aber es ist etwas zu berücksichtigen.

#### <a name="grouped-table-style"></a>Gruppierte Tabellenformat

Der gruppierte Stil geändert wurde aktualisiert, mit dem Inhalt jetzt an den Rändern des Bildschirms wie folgt erweitern:

 ![](ios7-ui-images/table1.png "Beispiel für gruppierte Tabellenformat")

#### <a name="separatorinset"></a>SeparatorInset

Zeilentrennzeichen können jetzt mit Einzug dargestellt durch Festlegen der `UITableVIewCell.SeparatorInset` Eigenschaft. Beispielsweise würde der folgende Code verwendet werden, für den Einzug der Zeilen aus der linken Seite:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Dies wird in der Tabellenansicht mit eingezogenen Zellen, wie unten dargestellt:

 ![](ios7-ui-images/separatorinset.png "Beispiel UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Tabelle Button-Stile

Die verschiedenen Schaltflächen, die in Tabellenansichten verwendet werden, wurden alle geändert. Der folgende Screenshot enthält eine Tabellenansicht im Bearbeitungsmodus an:

 ![](ios7-ui-images/table2.png "In diesem Screenshot enthält eine Tabellenansicht im Bearbeitungsmodus")

### <a name="additional-control-changes"></a>Zusätzliche Kontrolle Änderungen

Andere UIKit-Steuerelementen haben ebenfalls geändert wurden, einschließlich der Schieberegler, Schalter und Stepper. Diese Änderungen sind rein visuelle. Weitere Informationen finden Sie im Apple- [iOS 7 UI Übergang Anleitung](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Benutzeroberfläche für allgemeine Änderungen

Zusätzlich zu den Änderungen in UIKit, iOS 7 führt zu einer Vielzahl von visuellen Änderungen an der Benutzeroberfläche, einschließlich:

-  Vollbild-Inhalt
-  Leiste Darstellung
-  Tönungsfarbe

<a name="fullscreen" />

### <a name="full-screen-content"></a>Vollbild-Inhalt

iOS 7 dient, Anwendungen, die den gesamten Bildschirm nutzen können. View Controller werden jetzt durch eine Statusleiste und eine Navigationsleiste - überlappende angezeigt, wenn eine vorhanden ist, statt Sie unterhalb der Status und die Navigation Balken angezeigt werden:.

Wie Sie Ihre Anwendung für iOS 7 vorbereitet haben, können Sie die neu Unteransichten visuell mit ausrichten *Interface Builder* oder *Xamarin.IOS-Designer*. Sie können auch eine der neuen APIs verwenden, können Sie die programmgesteuerte Behandlung der Vollbild-Inhalt. Diese APIs werden nachfolgend vorgestellt.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide und BottomLayoutGuide

 `TopLayoutGuide` und `BottomLayoutGuide` dienen als Referenz für, in denen Sichten beginnen oder beenden möchten, sollten so, dass der Inhalt von einem durchscheinenden nicht überlappt wird `UIKit` Leiste, wie im folgenden Beispiel gezeigt:

 [![](ios7-ui-images/clipped.png "Beispielinhalt, die von einer lichtdurchlässiger UIKit-Leiste nicht überlappt")](ios7-ui-images/clipped.png#lightbox)

Diese APIs können verwendet werden, um eine Ansicht der Verschiebung von oben oder unteren Rand des Bildschirms zu berechnen, und passen die Platzierung von Inhalt entsprechend:

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

Wir können die festzulegende oben berechneten Wert verwenden unsere `ImageView`die Verschiebung aus dem oberen Rand des Bildschirms, sodass das gesamte Bild angezeigt wird:

 [![](ios7-ui-images/good2.png "Beispiel ImageViews Verschiebung aus dem oberen Rand des Bildschirms")](ios7-ui-images/good2.png#lightbox)

Finden Sie in der [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) ein funktionierendes Beispiel.

Der Verschiebungswert werden dynamisch generiert, nachdem die Hierarchie, also beim Lesen die Sicht hinzugefügt wurde `TopLayoutGuide` und `BottomLayoutGuide` Werte im `ViewDidLoad` gibt 0 zurück. Die Ansicht – z. B. im geladen hat, berechnen Sie den Wert der `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` und `BottomLayoutGuide` in iOS 11 zugunsten von neuen Sicherheitsbereich Layout als veraltet markiert werden. Laut Apple mithilfe des sicheren Bereichs mit älteren iOS-Version als iOS 11 kompatibel ist. Weitere Informationen finden Sie unter den [Aktualisieren Ihrer app für iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) Guide.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Diese API gibt an, welche Ränder des eine Ansicht auf Vollbild, unabhängig von Leiste Durchsichtigkeit erweitert werden soll. In iOS 7 Navigationsleisten und Symbolleisten angezeigt werden in den Ebenen oberhalb des Controllers-Ansicht – anders als in vorherigen iOS-Versionen, in denen nicht haben sie den gleichen Raum einnehmen. Die iOS 7-Fotos-Anwendung veranschaulicht die `UIViewController.EdgesForExtendedLayout` Wert `UIRectEdge.All`. Diese Einstellung füllt alle vier Kanten in der Ansicht mit Inhalt, den überlappenden und Vollbild-Effekt zu erstellen:

 [![](ios7-ui-images/photos.png "Beispiel-EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Durch Tippen auf das Bild entfernt die Balken und zeigt die Image-Vollbildmodus:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout der Balken entfernt")](ios7-ui-images/photos2.png#lightbox)

Da der Vollbild-Inhalt, der Standardwert ist, müssen für iOS 6 konfigurierte Anwendungen Teil der Ansicht abgeschnitten, wie im folgenden Screenshot dargestellt:

 [![](ios7-ui-images/clipped.png "Apps für iOS 6 konfiguriert ist Teil der Ansicht, wie in diesem Screenshot abgeschnitten haben.")](ios7-ui-images/clipped.png#lightbox)

Ändern der `UIViewController.EdgesForExtendedLayout` -Eigenschaft für dieses Verhalten automatisch angepasst wird. Wir können angeben, dass die Sicht Ränder, nicht ausfüllen, damit unsere Ansicht anzeigen von Inhalten in der durch die Navigations- oder Symbolleisten (auf jedem Ausrichtung) belegt Speicherplatz vermieden wird:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

In unserer app sehen wir, dass die Sicht erneut positioniert wird, damit das gesamte Bild sichtbar ist:

 [![](ios7-ui-images/good.png "Beispiel mit gesamte Bild sichtbar")](ios7-ui-images/good.png#lightbox)

Beachten Sie, dass bei der die Auswirkungen der `TopLayoutGuide/BottomLayoutGuide` und `EdgesForExtendedLayout` APIs sind ähnlich, sollen, geben Sie unterschiedliche Ziele. Ändern der `EdgesForExtendedLayout` Einstellung vom Standardwert möglicherweise abgeschnittene Ansichten lösen, in Anwendungen für iOS 6, jedoch ein guten iOS 7-Entwurf sollten berücksichtigt der Vollbild-Ästhetik und Vollbild-Benutzeroberfläche anzeigen, der vertrauenden Seite, auf `TopLayoutGuide` und `BottomLayoutGuide`Inhalt ordnungsgemäß zu positionieren, die tatsächlich in einen komfortablen Ort für den Benutzer bearbeitet werden.

Finden Sie in der [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) ein funktionierendes Beispiel.

### <a name="status-and-navigation-bars"></a>Status und Navigationsleisten

Die Statusleiste und Navigationsleisten werden transparent gerendert. StatusBar-Steuerelemente sind transparent, Symbolleisten und Navigationsleisten lichtdurchlässiger und verschwommen, um den Eindruck von Tiefe auf der Benutzeroberfläche zu übermitteln. Der folgende Screenshot zeigt diese Unschärfe und Transparenz, wo die blauen Hintergrundfarbe der Auflistungsansicht über den Status und die Navigation Balken, sodass sie eine einfache blaue Darstellung zeigt:

 ![](ios7-ui-images/transparent-navbar.png "Beispiel-Status und die Navigationsleiste Unschärfe")

#### <a name="status-bar-styles"></a>Statusleiste-Stile

Zusammen mit Unschärfe, und klicken Sie mit der Transparenz kann es sich bei der Vordergrund einer Statusleiste entweder "Light" oder "dunkel (dunkel ist die Standardeinstellung) sein. Die Statusleistenstil kann über den ansichtscontroller festgelegt werden. Ein ansichtscontroller kann auch festlegen, ob die Statusleiste angezeigt oder ausgeblendet wird.

Der folgende code z. B. Außerkraftsetzungen der `PreferredStatusBarStyle` Methode einen ansichtscontroller für stellen die Statusleiste einen hellen Vordergrund anzuzeigen:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Dies bewirkt, dass die Statusleiste angezeigt werden wie folgt:

 ![](ios7-ui-images/light-status-bar.png "Beispiel einer Statusleiste")

Überschreiben, um die Statusleiste aus der View-Controller-Code zu verbergen, `PrefersStatusBarHidden`, wie unten dargestellt:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Dadurch wird die Statusleiste ausgeblendet:

 ![](ios7-ui-images/status-bar-hidden.png "Statusleiste, die ausgeblendet")

### <a name="tint-color"></a>Tönungsfarbe

Schaltflächen werden jetzt als Chrome verkaufsortcomputer Text angezeigt. Die Textfarbe kann gesteuert werden, mit dem neuen `TintColor` Eigenschaft `UIView`. Festlegen der `TintColor` wendet die Farbe auf die gesamte Hierarchie für die Ansicht, die festgelegt. Anwenden einer `TintColor`legen Sie ihn in eine app, auf die `Window`. Sie können auch erkennen, wenn die tönungsfarbe über Änderungen der `UIView.TintColorDidChange` Methode.

Der folgende Screenshot zeigt beispielsweise die Auswirkungen der Änderung der tönungsfarbe auf eine Navigation des ansichtscontrollers in Violett:

 ![](ios7-ui-images/tint-color.png "Violettem Farbton-Farbe für eine Sicht der Navigation Controller")

Die tönungsfarbe auf angewendet werden kann Images als auch bei der `RenderingMode` nastaven NA hodnotu `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Tönungsfarbe kann nicht festgelegt werden, mithilfe von `UIAppearance`.


### <a name="dynamic-type"></a>Typ "Dynamic"

In iOS 7 kann der Benutzer die Textgröße in den Systemeinstellungen angeben. Die Schriftart wird mit einem dynamischen Typ dynamisch angepasst, um unabhängig von der Größe gut aussehen. `UIFont.PreferredFontForTextStyle` sollte verwendet werden, zum Abrufen einer Schriftart an, die optimiert ist für die Größe des Benutzer gesteuert.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Änderungen an Elementen der Benutzeroberfläche in iOS 7. Untersucht einige der die Änderungen an Ansichten und Steuerelementen im UIKit, markieren die visuellen Änderungen sowie Änderungen an verknüpften APIs. Abschließend führt er neue APIs zum Arbeiten mit Vollbild-Inhalte, neuen Farbton farbunterstützung und Typ "dynamic".

## <a name="related-links"></a>Verwandte Links

- [ImageViewer (Beispiel)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
