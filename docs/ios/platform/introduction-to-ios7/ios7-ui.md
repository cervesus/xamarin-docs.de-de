---
title: iOS 7 Übersicht über die Benutzeroberfläche
description: iOS 7 führt eine Fülle von benutzeränderungen-Schnittstelle. In diesem Artikel werden einige der größeren ändert, in die visuelle Darstellung von Steuerelementen und in die APIs, die das neue Design zu unterstützen.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3f5ea8abd41e718f9ac947c5acb290dffe400ddd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 Übersicht über die Benutzeroberfläche

_iOS 7 führt eine Fülle von benutzeränderungen-Schnittstelle. In diesem Artikel werden einige der größeren ändert, in die visuelle Darstellung von Steuerelementen und in die APIs, die das neue Design zu unterstützen._

iOS 7 liegt der Schwerpunkt auf Inhalte über Chrome. Elemente der Benutzeroberfläche in iOS 7 betonen Chrome durch Entfernen von Attributen wie z. B. äußeren Rahmen, Statusleisten und Navigationsleisten, die und die Größe des Bildschirms Speicherplatzes von Content-Ansichten verwendet verringern. Inhalt von iOS 7 dient den gesamten Bildschirm zu verwenden.

iOS 7 werden mehrere andere Änderungen eingeführt: Farbe wird verwendet, um Elemente einer Benutzeroberfläche anstelle von Attributen wie z. B. Schaltfläche Rahmen zu unterscheiden. Viele Elemente, z. B. Navigationsleisten und Statusleisten, sind nun entweder weichgezeichnete und transparent oder transparent sein, und Content-Ansichten Bereich darunter dauert. Dieser Inhalt Ansichten gerendert werden über die weichgezeichnete Balken, einen Eindruck von Tiefe in der Benutzeroberfläche vermittelt.

Dieser Artikel behandelt einige der Änderungen für Elemente der Benutzeroberfläche im iOS-7 sowie verschiedene APIs auf die neue Benutzeroberflächendesign beziehen.

## <a name="view-and-control-changes"></a>Anzeigen und Control Changes

Alle Ansichten in UIKit entsprechen für das neue Aussehen und Verhalten von iOS 7. In diesem Abschnitt werden einige der Änderungen für diesen Ansichten als auch die verwandten APIs, die zur Unterstützung der neuen Benutzeroberflächenautomatisierungs geändert haben.

### <a name="uibutton"></a>UIButton

Schaltflächen, die von der `UIButton` -Klasse sind jetzt hat keinen Rahmen, und kein Hintergrund standardmäßig, wie unten dargestellt:

 ![](ios7-ui-images/button.png "Beispiel UIButton")

Die `UIButtonType.RoundedRect` Stil ist veraltet. Wenn in iOS 7 verwendeten `UIButtonType.RoundedRect` führt zu `UIButtonType.System` verwendeten ADO.NET-Anbieter ab, die erzeugt der Standardschaltfläche ohne Hintergrund oder sichtbar Ränder, wie oben gezeigt.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Ähnlich wie `UIButton`, Strich Schaltflächen sind auch randlos, mit dem neuen direktionales `UIBarButtonItemStyle.Plain` Stil unten:

 ![](ios7-ui-images/barbuttonplain.png "Beispiel UIBarButtonItem")

Darüber hinaus die `UIBarButtonItemStyle.Bordered` Stil ist veraltet. Festlegen von `UIBarButtonItemStyle.Bordered` in iOS 7 führt dazu, die `UIBarButtonItemStyle.Plain` Stil verwendet wird.

Die `UIBarButtonItemStyle.Done` Stil wurde nicht als veraltet. Allerdings wird eine randlos Schaltfläche nur mit einem fett formatierten Text-Format wie gezeigt auch erstellt:

 ![](ios7-ui-images/barbuttondone.png "Beispiel-UIBarButtonItem in "Fertig" ändert Stil")

### <a name="uialertview"></a>UIAlertView

Zusätzlich zu der Änderung Stil für das neue Aussehen und Verhalten iOS 7 werden die Warnungsansichten Anpassung über den Unteransicht nicht mehr unterstützt. Obwohl `UIAlertView` erbt von `UIView`Aufrufen `AddSubview` auf eine `UIAlertView` hat keine Auswirkungen. Beachten Sie z. B. folgenden Code:

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

Dies erzeugt eine Warnung Standardsicht mit den Unteransicht ignoriert wird, wie unten dargestellt:

 ![](ios7-ui-images/alert.png "Beispiel UIAlertView")
 
 Hinweis: UIAlertView wurde in iOS 8 veraltet. Anzeigen der [Warnung Controller](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/) Anleitung zur Verwendung einer Warnungsansicht in iOS 8 und höher.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Segmentierte Steuerelemente in iOS 7 sind transparent und Farbton zu unterstützen. Der Farbton wird für die Farbe von Text und Rahmen verwendet. Wenn ein Segment ausgewählt ist, wird die Farbe zwischen den Hintergrund und den Text ein, mit der Farbton Farbe zum Markieren Sie des ausgewählten Segments verwendet wird, wie unten dargestellt ausgetauscht:

 ![](ios7-ui-images/segmentedcontrol.png "Beispiel UISegmentedControl")

Darüber hinaus die `UISegmentedControlStyle` in iOS 7 ist veraltet.

### <a name="picker-views"></a>Auswahl-Ansichten

Die API für die Auswahl einer Sichten ist größtenteils unverändert. Allerdings Status iOS 7-Entwurfsrichtlinien jetzt Datumsauswahl Ansichten Inline angezeigt, sondern als Eingabe Ansichten von animiert im unteren Bereich des Bildschirms oder über einen neuen Domänencontroller auf einen Navigation Controller Stapel abgelegt, wie in vorherigen iOS Versionen. Dadurch kann im System Kalender-app angezeigt werden:

 ![](ios7-ui-images/inlinepicker.png "Dies wird im System Kalender-app angezeigt.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Die Suchleiste wird jetzt in der Navigationsleiste angezeigt Leiste, wenn die `UISearchDisplayController.DisplaysSearchBarInNavigationBar` Eigenschaftensatz wird auf "true". Bei Festlegung auf "false" - Standard - Navigationsleiste wird ausgeblendet, wenn die Suche Controller angezeigt wird.

Der folgende Screenshot zeigt die Suchleiste innerhalb einer `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Beispiel UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Die APIs im Zusammenhang mit `UITableView` werden hauptsächlich unverändert; allerdings der Stil erheblich zum Anpassen an die neue Benutzeroberflächendesign geändert hat. Die interne Darstellung Hierarchie ist auch einige Unterschiede. Diese Änderung hat keine Auswirkung auf den meisten apps, aber es ist etwas zu berücksichtigen.

#### <a name="grouped-table-style"></a>Gruppierte Tabellenformat

Die gruppierte Formatvorlage geändert wurde aktualisiert, mit dem Inhalt an den Rändern des Bildschirms wie folgt erweitert:

 ![](ios7-ui-images/table1.png "Sample gruppierten Tabellenformat")

#### <a name="separatorinset"></a>SeparatorInset

Zeilentrennzeichen können jetzt mit Einzug dargestellt durch Festlegen der `UITableVIewCell.SeparatorInset` Eigenschaft. Beispielsweise würde der folgende Code verwendet werden, auf die Zellen aus dem linken Rand ziehen Sie ein:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Dies erzeugt in der Tabellenansicht mit eingezogenen Zellen, wie unten dargestellt:

 ![](ios7-ui-images/separatorinset.png "Beispiel UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Tabelle-Schaltflächenstile

Die verschiedenen Schaltflächen in Tabellenansichten verwendet werden, haben alle geändert. Der folgende Screenshot zeigt eine Tabellenansicht im Bearbeitungsmodus dargestellt:

 ![](ios7-ui-images/table2.png "Diese Abbildung zeigt eine Tabellenansicht im Bearbeitungsmodus")

### <a name="additional-control-changes"></a>Zusätzliche Kontrolle Änderungen

Andere Steuerelemente UIKit haben ebenfalls geändert wurden, einschließlich Schieberegler, Switches und Stepper. Diese Änderungen sind rein visuelle. Weitere Informationen finden Sie in der Apple- [iOS 7 UI Übergang geführt](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Benutzeroberfläche für allgemeine Änderungen

Zusätzlich zu den Änderungen in UIKit, iOS 7 führt zu einer Vielzahl von visuelle Änderungen an der Benutzeroberfläche, einschließlich:

-  Vollbild-Inhalt
-  Leiste Darstellung
-  Farbton

<a name="fullscreen" />

### <a name="full-screen-content"></a>Vollbild-Inhalt

iOS 7 dient der Anwendungen, die den gesamten Bildschirm nutzen zu können. View-Controller werden jetzt durch eine Statusleiste und Navigationsleiste - überlappender angezeigt, wenn einer vorhanden ist, im Gegensatz zu unterhalb der Status und die Navigation Balken angezeigt werden:.

Wie Sie Ihre Anwendung für iOS 7 vorbereitet haben, können Sie neu Unteransichten visuell mit ausrichten *Schnittstelle-Generator* oder *Xamarin iOS Designer*. Sie können auch eine der neuen APIs verwenden, können Vollbild-Inhalt programmgesteuert zu verarbeiten. Diese APIs werden unten eingeführt.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide und BottomLayoutGuide

 `TopLayoutGuide` und `BottomLayoutGuide` dienen als Referenz für, in denen Ansichten beginnen oder beenden, sollten, damit der Inhalt nicht durch ein transparentes überlappenden ist `UIKit` Leiste, wie im folgenden Beispiel gezeigt:

 [![](ios7-ui-images/clipped.png "Beispielinhalte nicht überlappenden ein transparentes UIKit Strich")](ios7-ui-images/clipped.png#lightbox)

Diese APIs können verwendet werden, um eine Sicht Verschiebung vom oberen oder unteren Rand des Bildschirms berechnen und Platzierung von Inhalt entsprechend anpassen:

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

Wir können den Wert oben festzulegenden berechneten unsere `ImageView`der Verschiebung aus dem oberen Rand des Bildschirms, sodass das gesamte Bild angezeigt wird:

 [![](ios7-ui-images/good2.png "Beispiel ImageViews Verschiebung vom oberen Rand des Bildschirms")](ios7-ui-images/good2.png#lightbox)

Finden Sie in der [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) ein funktionierendes Beispiel.

Der Verschiebungswert wird dynamisch generiert, nachdem der Hierarchie, also beim Lesen die Sicht hinzugefügt wurden `TopLayoutGuide` und `BottomLayoutGuide` Werte in `ViewDidLoad` gibt 0 zurück. Die Sicht – z. B. im geladen hat, berechnen Sie den Wert der `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` und `BottomLayoutGuide` sind in iOS 11 zugunsten von neuen abgesicherten Bereichslayout veraltet. Apple haben angegeben, die mithilfe des sicheren Bereichs mit iOS-Version als iOS 11 kompatibel ist. Weitere Informationen finden Sie unter der [aktualisieren Ihre app für iOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) Handbuch.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Diese API gibt an, welche Ränder einer Sicht in den Vollbildmodus, unabhängig von Balken Lichtdurchlässigkeit erweitert werden soll. In iOS 7 Navigation und Symbolleisten angezeigt werden in den Ebenen oberhalb des Controllers-Ansicht – im Gegensatz zu in vorherigen iOS-Versionen, in denen haben nicht sie den gleichen Platz einnehmen. Die iOS 7 Fotos Anwendung veranschaulicht die `UIViewController.EdgesForExtendedLayout` Wert `UIRectEdge.All`. Diese Einstellung füllt alle vier Seiten in der Ansicht mit Inhalt, den überlappenden und Vollbild-Effekt zu erstellen:

 [![](ios7-ui-images/photos.png "Beispiel EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Tippen Sie auf das Bild entfernt die Balken und zeigt die Image-Vollbild:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout der Balken entfernt")](ios7-ui-images/photos2.png#lightbox)

Da Vollbild-Inhalt, der Standardwert ist, müssen für iOS 6 konfigurierte Anwendungen Teil der Ansicht abgeschnitten, wie in der folgende Screenshot:

 [![](ios7-ui-images/clipped.png "Für iOS 6 konfigurierten Apps müssen Teil der Ansicht abgeschnitten, wie in diesem screenshot")](ios7-ui-images/clipped.png#lightbox)

Ändern der `UIViewController.EdgesForExtendedLayout` Eigenschaft für dieses Verhalten passt. Wir können angeben, dass die Sicht alle Ränder, nicht ausfüllen, damit unser Ansicht anzeigen von Inhalt in die bedeckte durch Navigation oder Symbolleisten (auf jedem Ausrichtung) vermieden werden:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

In dieser app sehen, dass die Sicht erneut positioniert wird, sodass das gesamte Bild angezeigt wird:

 [![](ios7-ui-images/good.png "Beispiel mit dem gesamten Image sichtbar")](ios7-ui-images/good.png#lightbox)

Beachten Sie, dass während der Auswirkungen der `TopLayoutGuide/BottomLayoutGuide` und `EdgesForExtendedLayout` APIs ähneln, werden sie zum Füllen verschiedene Ziele vorgesehen. Ändern der `EdgesForExtendedLayout` Einstellung vom Standardwert abgeschnittene Sichten kann möglicherweise gelöst werden, in Anwendungen für iOS 6, aber ein guten iOS 7-Entwurf sollten berücksichtigt den Vollbildmodus ästhetischen, und geben Sie eine Vollbild-Videowiedergabe, Vertrauensstellungen der vertrauenden Seite auf `TopLayoutGuide` und `BottomLayoutGuide`ordnungsgemäß Inhalt einfügen, die vorgesehen ist, in einen komfortablen Ort für den Benutzer bearbeitet werden.

Finden Sie in der [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) ein funktionierendes Beispiel.

### <a name="status-and-navigation-bars"></a>Status und Navigationsleisten

Die Statusleiste und Navigationsleisten werden Zwischenfarbe gerendert. Statusleisten sind transparent, Symbolleisten und Navigationsleisten transparent und weichgezeichnete, um den Eindruck von Tiefe in der Benutzeroberfläche zu übermitteln. Der folgende Screenshot zeigt diese Weichzeichnen und Transparenz zeigt, wo die blauen Hintergrundfarbe die Auflistungsansicht über den Status und die Navigation Balken, weisen Sie ihnen eine helle blaue Darstellung:

 ![](ios7-ui-images/transparent-navbar.png "Beispiel-Status und die Navigationsleiste Weichzeichnen")

#### <a name="status-bar-styles"></a>Statusleiste-Stile

Zusammen mit Weichzeichnen und Transparenz kann der Vordergrund einer Statusleiste hell oder dunkel (Dunkles wird die Standardeinstellung). Der Standardstil der Statusleiste Status kann über die View-Controller festgelegt werden. Ein Ansicht Controller kann auch festlegen, ob die Statusleiste angezeigt oder ausgeblendet wird.

Im folgenden Codebeispiel beispielsweise überschreibt die `PreferredStatusBarStyle` Methode eines Controllers anzeigen, stellen die Statusleiste einen hellen Vordergrund anzuzeigen:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Dies bewirkt, dass die Statusleiste angezeigt werden folgendermaßen aus:

 ![](ios7-ui-images/light-status-bar.png "Beispiel einer Statusleiste")

Zum Ausblenden der Statusleiste aus der Sicht des Controllers Code überschreiben `PrefersStatusBarHidden`, wie unten dargestellt:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Die Statusleiste wird ausgeblendet:

 ![](ios7-ui-images/status-bar-hidden.png "Statusleiste ausgeblendet")

### <a name="tint-color"></a>Farbton

Schaltflächen werden jetzt als Chrome kleiner Text angezeigt. Die Textfarbe kann gesteuert werden, mit dem neuen `TintColor` Eigenschaft `UIView`. Festlegen der `TintColor` gilt die Farbe für die komplette Ansichtshierarchie für die Ansicht, die er festlegt. Anwenden einer `TintColor`legen Sie ihn in eine app auf die `Window`. Sie können auch erkennen, wenn der Farbton über wechselt die `UIView.TintColorDidChange` Methode.

Beispielsweise der folgende Screenshot zeigt die Auswirkungen der Änderung der Farbton auf eine Navigation controlleransicht Violett:

 ![](ios7-ui-images/tint-color.png "Violettem Farbton auf ein Controllern Navigationsansicht")

Der Farbton kann auf Bilder auch wenn angewendet werden die `RenderingMode` festgelegt ist, um `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Farbton kann nicht festgelegt werden, mithilfe von `UIAppearance`.


### <a name="dynamic-type"></a>Dynamische Typ

In iOS 7 kann der Benutzer die Textgröße in den Systemeinstellungen angeben. Mit dynamischen Typ ist die Schriftart dynamisch angepasst, um unabhängig von der Größe optimal. `UIFont.PreferredFontForTextStyle` sollte verwendet werden, um eine Schriftart, die optimiert ist für die Benutzer festgelegte Größe abzurufen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Änderungen für Elemente der Benutzeroberfläche in iOS 7. Er überprüft mehrere Ansichten und-Steuerelementen in UIKit, sowohl die Visualisierung ändert sich Hervorhebung vorgenommenen Änderungen sowie Änderungen an verknüpften APIs. Schließlich stellt neue APIs zum Arbeiten mit Vollbild-Inhalte, neue Farbton Farbe Unterstützung und dynamische Typ.

## <a name="related-links"></a>Verwandte Links

- [ImageViewer (sample)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
