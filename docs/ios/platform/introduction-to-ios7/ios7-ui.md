---
title: Übersicht über die iOS 7-Benutzeroberfläche
description: IOS 7 führt eine Vielzahl von Änderungen an der Benutzeroberfläche ein. In diesem Artikel werden einige der größeren Änderungen, sowohl in der visuellen Darstellung von Steuerelementen als auch in den APIs, die den neuen Entwurf unterstützen, hervorgehoben.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 5ebfa7348e13e5b168dd7f2decbef921bd7d69e3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652116"
---
# <a name="ios-7-user-interface-overview"></a>Übersicht über die iOS 7-Benutzeroberfläche

_IOS 7 führt eine Vielzahl von Änderungen an der Benutzeroberfläche ein. In diesem Artikel werden einige der größeren Änderungen, sowohl in der visuellen Darstellung von Steuerelementen als auch in den APIs, die den neuen Entwurf unterstützen, hervorgehoben._

IOS 7 konzentriert sich auf Inhalte über Chrome. Benutzeroberflächen Elemente in ios 7 heben das Hervorheben von Chrome auf, indem Sie Attribute wie z. b. überflüssige Rahmen, Status leisten und Navigationsleisten entfernen, wodurch der von Inhalts Ansichten verwendete Bildschirmbereich reduziert wird. In ios 7 ist der Inhalt so konzipiert, dass er den gesamten Bildschirm verwendet.

IOS 7 führt verschiedene andere Änderungen ein: mit Color werden Benutzeroberflächen Elemente anstelle von Attributen wie Schaltflächen Rahmen unterschieden. Viele Elemente, wie z. b. Navigationsleisten und Status leisten, sind jetzt entweder verschwommen und durchlässig oder transparent, wobei Inhalts Ansichten den Bereich darunter ziehen. Diese Inhalts Ansichten werden durch die unscharfen Balken dargestellt und vermitteln ein Gefühl der Tiefe in der Benutzeroberfläche.

In diesem Artikel werden einige der Änderungen an Benutzeroberflächen Elementen in ios 7 sowie verschiedene APIs behandelt, die sich auf den Entwurf der neuen Benutzeroberfläche beziehen.

## <a name="view-and-control-changes"></a>Anzeigen und Steuern von Änderungen

Alle Ansichten in UIKit entsprechen dem neuen Aussehen und Gefühl von IOS 7. In diesem Abschnitt werden einige der Änderungen an diesen Sichten sowie die zugehörigen APIs hervorgehoben, die sich geändert haben, um die neue Benutzeroberfläche zu unterstützen.

### <a name="uibutton"></a>UIButton

Schaltflächen, die `UIButton` aus der-Klasse erstellt wurden, sind jetzt grenzenlos und standardmäßig ohne Hintergrund, wie unten dargestellt:

 ![](ios7-ui-images/button.png "Beispiel für UIButton")

Der `UIButtonType.RoundedRect` Stil ist veraltet. Wenn `UIButtonType.System` Sie in ios 7 verwendet `UIButtonType.RoundedRect` wird, wird verwendet, wodurch die Standard Schaltfläche ohne Hintergrund oder sichtbare Ränder erstellt wird, wie oben gezeigt.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Ähnlich wie `UIButton`bei sind Balken Schaltflächen auch grenzenlos, wobei der unten gezeigte neue `UIBarButtonItemStyle.Plain` Stil standardmäßig angezeigt wird:

 ![](ios7-ui-images/barbuttonplain.png "Beispiel für uibarbuttonitem")

Außerdem ist der `UIBarButtonItemStyle.Bordered` Stil veraltet. Die `UIBarButtonItemStyle.Bordered` Einstellung in ios 7 führt dazu, `UIBarButtonItemStyle.Plain` dass der Stil verwendet wird.

Der `UIBarButtonItemStyle.Done` Stil ist nicht als veraltet markiert. Es wird jedoch auch eine randlose-Schaltfläche erstellt, die nur mit einem fett formatierten Textstil wie folgt dargestellt wird:

 ![](ios7-ui-images/barbuttondone.png "Beispiel für uibarbuttonitem im done-Stil")

### <a name="uialertview"></a>UIAlertView

Neben der Stiländerung für das neue Erscheinungsbild von IOS 7 unterstützen Warnungs Ansichten die Anpassung nicht mehr über die unter Ansicht. Obwohl `UIAlertView` von `UIView`erbt, hat das `AddSubview` Aufrufen von `UIAlertView` für einen keinerlei Auswirkungen. Beachten Sie z. B. folgenden Code:

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

Dies erzeugt eine Standard Warnungs Ansicht, in der die unter Ansicht ignoriert wird, wie unten dargestellt:

 ![](ios7-ui-images/alert.png "Beispiel für UIAlertView")
 
 Hinweis: UIAlertView wurde in ios 8 als veraltet markiert. Anzeigen des Warnungs [Controllers](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) für die Verwendung einer Warnungs Ansicht in ios 8 und höher.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

Segmentierte Steuerelemente in ios 7 sind transparent und unterstützen Tönungs-Farben. Die Tönungs-Farbe wird für den Text und die Rahmenfarbe verwendet. Wenn ein Segment ausgewählt wird, wird die Farbe zwischen dem Hintergrund und dem Text ausgetauscht. dabei wird die Tönungs-Farbe verwendet, um das ausgewählte Segment hervorzuheben, wie unten dargestellt:

 ![](ios7-ui-images/segmentedcontrol.png "Beispiel für uisegmentedcontrol")

`UISegmentedControlStyle` Außerdem ist in ios 7 veraltet.

### <a name="picker-views"></a>Auswahl Ansichten

Die API für die Auswahl Ansicht ist größtenteils unverändert. die Entwurfs Richtlinien für IOS 7 sollten nun aber Inline angezeigt werden, anstatt als Eingabe Ansichten, die vom unteren Bildschirmrand animiert werden, oder über einen neuen Controller, der auf den Stapel eines Navigations Controllers geschoben wurde, wie in früheren IOS-Versionen. Dies ist in der System Kalender-APP zu sehen:

 ![](ios7-ui-images/inlinepicker.png "Dies ist in der System Kalender-APP zu sehen.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Die Suchleiste wird jetzt in der Navigationsleiste angezeigt, wenn `UISearchDisplayController.DisplaysSearchBarInNavigationBar` die-Eigenschaft auf true festgelegt ist. Wenn diese Einstellung auf false festgelegt ist, wird die Navigationsleiste ausgeblendet, wenn der Such Controller angezeigt wird.

Der folgende Screenshot zeigt die Suchleiste in einem `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Beispiel für UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Die APIs `UITableView` sind in erster Linie unverändert. der Stil hat sich jedoch erheblich geändert, sodass er dem Design der neuen Benutzeroberfläche entspricht. Die interne Ansichts Hierarchie ist auch etwas anders. Diese Änderung wirkt sich nicht auf die meisten apps aus, aber Sie ist zu beachten.

#### <a name="grouped-table-style"></a>Stil der gruppierten Tabelle

Der gruppierte Stil wurde aktualisiert, und der Inhalt wird nun auf die Ränder des Bildschirms erweitert, wie unten dargestellt:

 ![](ios7-ui-images/table1.png "Beispiel für eine gruppierte Tabelle")

#### <a name="separatorinset"></a>SeparatorInset

Zeilen Trennzeichen können jetzt durch Festlegen der `UITableVIewCell.SeparatorInset` -Eigenschaft eingezogen werden. Der folgende Code wird z. b. verwendet, um die Zellen des linken Rands abzuleiten:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Dies erzeugt in der Tabellenansicht mit eingerückt-Zellen, wie unten dargestellt:

 ![](ios7-ui-images/separatorinset.png "Beispiel für \"uitableview separatorinset\"")

#### <a name="table-button-styles"></a>Tabellen Schaltflächen Stile

Alle in Tabellen Sichten verwendeten Schaltflächen wurden geändert. Der folgende Screenshot zeigt eine Tabellenansicht im Bearbeitungsmodus:

 ![](ios7-ui-images/table2.png "Dieser Screenshot zeigt eine Tabellenansicht im Bearbeitungsmodus.")

### <a name="additional-control-changes"></a>Weitere Änderungen an Steuerelementen

Andere UIKit-Steuerelemente wurden ebenfalls geändert, einschließlich Schieberegler, Switches und Steppers. Diese Änderungen sind rein visuell. Weitere Informationen finden Sie im Leitfaden für die [Benutzeroberfläche der IOS 7-Benutzeroberfläche](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)von Apple.

## <a name="general-user-interface-changes"></a>Allgemeine Änderungen an der Benutzeroberfläche

Zusätzlich zu den Änderungen in UIKit führt IOS 7 eine Reihe von visuellen Änderungen an der Benutzeroberfläche ein, einschließlich:

-  Vollbild-Inhalt
-  Balken Darstellung
-  Tint-Farbe

<a name="fullscreen" />

### <a name="full-screen-content"></a>Voll Bildinhalt

IOS 7 ist so konzipiert, dass Anwendungen den gesamten Bildschirm nutzen können. Ansichts Controller werden nun durch eine Statusleiste und eine Navigationsleiste überlappend angezeigt, wenn eine vorhanden ist, anstatt unterhalb des Status und der Navigationsleisten angezeigt zu werden.

Wenn Sie Ihre Anwendung für IOS 7 vorbereiten, können Sie untergeordnete Sichten mit *Interface Builder* oder dem *xamarin IOS-Designer*visuell ausrichten. Sie können auch eine der neuen APIs verwenden, um voll Bildinhalte Programm gesteuert zu verarbeiten. Diese APIs werden im folgenden vorgestellt.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>Toplayoutguide und bottomlayoutguide

 `TopLayoutGuide`und `BottomLayoutGuide` dienen als Referenz für den Beginn oder das Ende von Sichten, sodass der Inhalt nicht durch eine durchlässiges `UIKit` Balken überlappen wird, wie im folgenden Beispiel gezeigt:

 [![](ios7-ui-images/clipped.png "Beispiel Inhalt, der nicht durch eine durchsichtige UIKit-Leiste überlappen wird")](ios7-ui-images/clipped.png#lightbox)

Diese APIs können verwendet werden, um die Verschiebung einer Ansicht von oben nach unten oder unten auf dem Bildschirm zu berechnen und die Platzierung von Inhalten entsprechend anzupassen:

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

Mit dem oben berechneten Wert können wir die Verschiebung von `ImageView`oben nach unten auf dem Bildschirm festlegen, sodass das gesamte Bild sichtbar ist:

 [![](ios7-ui-images/good2.png "Beispiel für die Verschiebung von imageviews vom oberen Bildschirmrand")](ios7-ui-images/good2.png#lightbox)

Ein funktionierendes Beispiel finden Sie unter [imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/) .

Der Verschiebungs Wert wird dynamisch generiert, nachdem die Sicht der Hierarchie hinzugefügt wurde. der Versuch `TopLayoutGuide` , `BottomLayoutGuide` zu lesen `ViewDidLoad` und Werte in zu lesen, gibt also 0 zurück. Berechnen Sie den Wert, nachdem die Ansicht geladen wurde, z `ViewDidLayoutSubviews`. b. in.

> [!IMPORTANT]
> `TopLayoutGuide`und `BottomLayoutGuide` sind in ios 11 anstelle des neuen sicheren Bereichs Layouts veraltet. Apple hat angegeben, dass die Verwendung des sicheren Bereichs mit der IOS-Version vor IOS 11 kompatibel ist. Weitere Informationen finden Sie im Handbuch [Aktualisieren Ihrer APP für IOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) .

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Diese API gibt an, welche Ränder einer Ansicht auf den Vollbildmodus erweitert werden sollen, unabhängig von der Strich Durchlässigkeit. In ios 7 werden Navigationsleisten und Symbolleisten oberhalb der Ansicht des Controllers oberhalb der Ansicht des Controllers angezeigt, anders als in früheren IOS-Versionen, in denen Sie nicht denselben Platz belegen. Die IOS 7 Fotos-Anwendung veranschaulicht den `UIViewController.EdgesForExtendedLayout` Standard `UIRectEdge.All`Wert. Diese Einstellung füllt alle vier Ränder in der Ansicht mit Inhalt aus, wodurch die Überlappung und der voll Bildschirm wirksam werden:

 [![](ios7-ui-images/photos.png "Beispiel für edgesforextendedlayout")](ios7-ui-images/photos.png#lightbox)

Wenn Sie auf das Bild tippen, werden die Balken entfernt und das Bild voll Bildschirm angezeigt:

 [![](ios7-ui-images/photos2.png "Edgesforextendedlayout mit entfernten Balken")](ios7-ui-images/photos2.png#lightbox)

Da der voll Bildinhalt standardmäßig verwendet wird, werden für IOS 6 konfigurierte Anwendungen als Teil der Ansicht abgeschnitten, wie im folgenden Screenshot gezeigt:

 [![](ios7-ui-images/clipped.png "Für IOS 6 konfigurierte apps wird ein Teil der Ansicht abgeschnitten, wie in diesem Screenshot gezeigt.")](ios7-ui-images/clipped.png#lightbox)

Durch ändern `UIViewController.EdgesForExtendedLayout` der Eigenschaft wird dieses Verhalten angepasst. Wir können angeben, dass die Ansicht keine Kanten ausfüllen soll, sodass die Ansicht keine Inhalte in dem Bereich anzeigt, der von Navigations-oder Symbolleisten (bei jeder Ausrichtung) belegt wird:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

In unserer APP wird angezeigt, dass die Ansicht erneut positioniert wird, sodass das gesamte Bild sichtbar ist:

 [![](ios7-ui-images/good.png "Beispiel für das gesamte Bild sichtbar")](ios7-ui-images/good.png#lightbox)

Beachten Sie, dass die Auswirkungen `TopLayoutGuide/BottomLayoutGuide` der-und- `EdgesForExtendedLayout` APIs ähnlich sind, dass Sie unterschiedliche Ziele erfüllen sollen. Wenn Sie `EdgesForExtendedLayout` die Einstellung von der Standardeinstellung ändern, werden die ausgeschnittenen Sichten in Anwendungen, die für IOS 6 entwickelt wurden, möglicherweise behoben, aber ein guter IOS 7-Entwurf sollte die Vollbild-Ästhetik `TopLayoutGuide` berücksichtigen und eine voll Bild Anzeige `BottomLayoutGuide`bieten.zum ordnungsgemäßen Positionieren von Inhalten, die für den Benutzer an einem bequemen Ort geändert werden sollen.

Ein funktionierendes Beispiel finden Sie unter [imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/) .

### <a name="status-and-navigation-bars"></a>Status und Navigationsleisten

Die Statusleiste und die Navigationsleisten werden mit Transparenz gerendert. Status leisten sind transparent, während Symbolleisten und Navigationsleisten durchsichtig und verschwommen sind, um das Gefühl der Tiefe in der Benutzeroberfläche zu vermitteln. Der folgende Screenshot zeigt diese Unschärfe und Transparenz, bei der die blaue Hintergrundfarbe der Auflistungs Ansicht sowohl über den Status als auch über die Navigationsleisten angezeigt wird, sodass Sie hell blau dargestellt werden:

 ![](ios7-ui-images/transparent-navbar.png "Beispiel für Status und Navigationsleiste")

#### <a name="status-bar-styles"></a>StatusBar-Stile

Zusammen mit der Unschärfe und Transparenz kann der Vordergrund einer Statusleiste entweder hell oder dunkel sein (dunkel ist die Standardeinstellung). Der Status leisten Stil kann über den Ansichts Controller festgelegt werden. Mit einem Ansichts Controller kann auch festgelegt werden, ob die Statusleiste ausgeblendet oder angezeigt wird.

Beispielsweise überschreibt der folgende Code die `PreferredStatusBarStyle` -Methode eines Ansichts Controllers, damit die Statusleiste einen hellen Vordergrund anzeigt:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Dadurch wird die Statusleiste wie folgt angezeigt:

 ![](ios7-ui-images/light-status-bar.png "Beispiel Status Leiste")

Um die Statusleiste im Code des Ansichts Controllers auszublenden, `PrefersStatusBarHidden`überschreiben Sie, wie unten dargestellt:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Dadurch wird die Statusleiste ausgeblendet:

 ![](ios7-ui-images/status-bar-hidden.png "Status Leiste ausgeblendet")

### <a name="tint-color"></a>Tint-Farbe

Schaltflächen werden nun als weniger Chrome-Text angezeigt. Die Textfarbe kann mithilfe der neuen `TintColor` Eigenschaft von `UIView`gesteuert werden. Durch das Festlegen von wird die Farbe auf die gesamte Ansichts Hierarchie für die Ansicht angewendet,dieSiefestlegt.`TintColor` Wenn Sie einen `TintColor`in einer APP anwenden möchten, legen Sie `Window`ihn auf fest. Sie können auch erkennen, wenn die Tönungs-Farbe über `UIView.TintColorDidChange` die-Methode geändert wird.

Der folgende Screenshot zeigt z. b. die Auswirkung der Änderung der Tönungs-Farbe in der Ansicht eines Navigations Controllers auf lila:

 ![](ios7-ui-images/tint-color.png "Lila Tönungs-Farbe in einer Navigations Controller Ansicht")

Die Tönungs-Farbe kann auch auf Bilder angewendet werden, wenn `RenderingMode` auf `UIImageRenderingMode.AlwaysTemplate`festgelegt ist.

> [!IMPORTANT]
> Die tint-Farbe kann nicht `UIAppearance`mit festgelegt werden.


### <a name="dynamic-type"></a>Dynamischer Typ

In ios 7 kann der Benutzer die Textgröße in den Systemeinstellungen angeben. Beim dynamischen Typ wird die Schriftart dynamisch angepasst, sodass Sie unabhängig von der Größe gut aussieht. `UIFont.PreferredFontForTextStyle`sollte verwendet werden, um eine Schriftart zu erhalten, die für die benutzergesteuerte Größe optimiert ist.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden die Änderungen an Benutzeroberflächen Elementen in ios 7 behandelt. Es werden einige der Änderungen untersucht, die an Sichten und Steuerelementen in UIKit vorgenommen werden, und sowohl die visuellen Änderungen als auch Änderungen an verwandten APIs hervorheben. Schließlich werden neue APIs eingeführt, um mit voll Bildinhalten, der neuen Unterstützung für Tönungs-Farben und dem dynamischen Typ zu arbeiten.

## <a name="related-links"></a>Verwandte Links

- [Imageviewer (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates)
