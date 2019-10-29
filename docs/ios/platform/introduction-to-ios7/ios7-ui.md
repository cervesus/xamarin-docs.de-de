---
title: Übersicht über die iOS 7-Benutzeroberfläche
description: IOS 7 führt eine Vielzahl von Änderungen an der Benutzeroberfläche ein. In diesem Artikel werden einige der größeren Änderungen, sowohl in der visuellen Darstellung von Steuerelementen als auch in den APIs, die den neuen Entwurf unterstützen, hervorgehoben.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4731be58c1fadae0bba6768570ecfd181b071dd2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031866"
---
# <a name="ios-7-user-interface-overview"></a>Übersicht über die iOS 7-Benutzeroberfläche

_IOS 7 führt eine Vielzahl von Änderungen an der Benutzeroberfläche ein. In diesem Artikel werden einige der größeren Änderungen, sowohl in der visuellen Darstellung von Steuerelementen als auch in den APIs, die den neuen Entwurf unterstützen, hervorgehoben._

IOS 7 konzentriert sich auf Inhalte über Chrome. Benutzeroberflächen Elemente in ios 7 heben das Hervorheben von Chrome auf, indem Sie Attribute wie z. b. überflüssige Rahmen, Status leisten und Navigationsleisten entfernen, wodurch der von Inhalts Ansichten verwendete Bildschirmbereich reduziert wird. In ios 7 ist der Inhalt so konzipiert, dass er den gesamten Bildschirm verwendet.

IOS 7 führt verschiedene andere Änderungen ein: mit Color werden Benutzeroberflächen Elemente anstelle von Attributen wie Schaltflächen Rahmen unterschieden. Viele Elemente, wie z. b. Navigationsleisten und Status leisten, sind jetzt entweder verschwommen und durchlässig oder transparent, wobei Inhalts Ansichten den Bereich darunter ziehen. Diese Inhalts Ansichten werden durch die unscharfen Balken dargestellt und vermitteln ein Gefühl der Tiefe in der Benutzeroberfläche.

In diesem Artikel werden einige der Änderungen an Benutzeroberflächen Elementen in ios 7 sowie verschiedene APIs behandelt, die sich auf den Entwurf der neuen Benutzeroberfläche beziehen.

## <a name="view-and-control-changes"></a>Anzeigen und Steuern von Änderungen

Alle Ansichten in UIKit entsprechen dem neuen Aussehen und Gefühl von IOS 7. In diesem Abschnitt werden einige der Änderungen an diesen Sichten sowie die zugehörigen APIs hervorgehoben, die sich geändert haben, um die neue Benutzeroberfläche zu unterstützen.

### <a name="uibutton"></a>UIButton

Schaltflächen, die aus der `UIButton`-Klasse erstellt wurden, sind jetzt grenzenlos und standardmäßig ohne Hintergrund, wie unten dargestellt:

 ![](ios7-ui-images/button.png "Sample UIButton")

Der `UIButtonType.RoundedRect` Stil ist veraltet. Bei Verwendung in ios 7 führt `UIButtonType.RoundedRect` zu `UIButtonType.System` verwendet werden, der den Standard Schaltflächenstil ohne Hintergrund oder sichtbare Ränder erstellt, wie oben gezeigt.

### <a name="uibarbuttonitem"></a>Uibarbuttonitem

Ähnlich wie bei `UIButton`sind leisten Schaltflächen auch grenzenlos, wobei der neue `UIBarButtonItemStyle.Plain` Stil standardmäßig angezeigt wird:

 ![](ios7-ui-images/barbuttonplain.png "Sample UIBarButtonItem")

Außerdem ist der `UIBarButtonItemStyle.Bordered` Stil veraltet. Wenn Sie `UIBarButtonItemStyle.Bordered` in ios 7 festlegen, wird der `UIBarButtonItemStyle.Plain` Stil verwendet.

Der `UIBarButtonItemStyle.Done` Stil ist nicht als veraltet markiert. Es wird jedoch auch eine randlose-Schaltfläche erstellt, die nur mit einem fett formatierten Textstil wie folgt dargestellt wird:

 ![](ios7-ui-images/barbuttondone.png "Sample UIBarButtonItem in the Done style")

### <a name="uialertview"></a>UIAlertView

Neben der Stiländerung für das neue Erscheinungsbild von IOS 7 unterstützen Warnungs Ansichten die Anpassung nicht mehr über die unter Ansicht. Obwohl `UIAlertView` von `UIView`erbt, hat das Aufrufen von `AddSubview` auf einem `UIAlertView` keinerlei Auswirkungen. Beachten Sie z. B. folgenden Code:

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

 ![](ios7-ui-images/alert.png "Sample UIAlertView")

 Hinweis: UIAlertView wurde in ios 8 als veraltet markiert. Anzeigen des Warnungs [Controllers](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) für die Verwendung einer Warnungs Ansicht in ios 8 und höher.

### <a name="uisegmentedcontrol"></a>Uisegmentedcontrol

Segmentierte Steuerelemente in ios 7 sind transparent und unterstützen Tönungs-Farben. Die Tönungs-Farbe wird für den Text und die Rahmenfarbe verwendet. Wenn ein Segment ausgewählt wird, wird die Farbe zwischen dem Hintergrund und dem Text ausgetauscht. dabei wird die Tönungs-Farbe verwendet, um das ausgewählte Segment hervorzuheben, wie unten dargestellt:

 ![](ios7-ui-images/segmentedcontrol.png "Sample UISegmentedControl")

Außerdem ist der `UISegmentedControlStyle` in ios 7 veraltet.

### <a name="picker-views"></a>Auswahl Ansichten

Die API für die Auswahl Ansicht ist größtenteils unverändert. die Entwurfs Richtlinien für IOS 7 sollten nun aber Inline angezeigt werden, anstatt als Eingabe Ansichten, die vom unteren Bildschirmrand animiert werden, oder über einen neuen Controller, der auf den Stapel eines Navigations Controllers geschoben wurde, wie in früheren IOS-Versionen. Dies ist in der System Kalender-APP zu sehen:

 ![](ios7-ui-images/inlinepicker.png "This can be seen in the system calendar app")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Die Suchleiste wird nun in der Navigationsleiste angezeigt, wenn die `UISearchDisplayController.DisplaysSearchBarInNavigationBar`-Eigenschaft auf true festgelegt ist. Wenn diese Einstellung auf false festgelegt ist, wird die Navigationsleiste ausgeblendet, wenn der Such Controller angezeigt wird.

Der folgende Screenshot zeigt die Suchleiste innerhalb eines `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Sample UISearchDisplayController")

### <a name="uitableview"></a>UITableView

Die APIs um `UITableView` sind hauptsächlich unverändert. Allerdings hat sich der Stil erheblich geändert, sodass er dem Design der neuen Benutzeroberfläche entspricht. Die interne Ansichts Hierarchie ist auch etwas anders. Diese Änderung wirkt sich nicht auf die meisten apps aus, aber Sie ist zu beachten.

#### <a name="grouped-table-style"></a>Stil der gruppierten Tabelle

Der gruppierte Stil wurde aktualisiert, und der Inhalt wird nun auf die Ränder des Bildschirms erweitert, wie unten dargestellt:

 ![](ios7-ui-images/table1.png "Sample Grouped Table Style")

#### <a name="separatorinset"></a>Separatorinset

Zeilen Trennzeichen können nun eingerückt werden, indem die `UITableVIewCell.SeparatorInset`-Eigenschaft festgelegt wird. Der folgende Code wird z. b. verwendet, um die Zellen des linken Rands abzuleiten:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Dies erzeugt in der Tabellenansicht mit eingerückt-Zellen, wie unten dargestellt:

 ![](ios7-ui-images/separatorinset.png "Sample UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Tabellen Schaltflächen Stile

Alle in Tabellen Sichten verwendeten Schaltflächen wurden geändert. Der folgende Screenshot zeigt eine Tabellenansicht im Bearbeitungsmodus:

 ![](ios7-ui-images/table2.png "This screenshot presents a table view in editing mode")

### <a name="additional-control-changes"></a>Weitere Änderungen an Steuerelementen

Andere UIKit-Steuerelemente wurden ebenfalls geändert, einschließlich Schieberegler, Switches und Steppers. Diese Änderungen sind rein visuell. Weitere Informationen finden Sie im Leitfaden für die [Benutzeroberfläche der IOS 7-Benutzeroberfläche](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)von Apple.

## <a name="general-user-interface-changes"></a>Allgemeine Änderungen an der Benutzeroberfläche

Zusätzlich zu den Änderungen in UIKit führt IOS 7 eine Reihe von visuellen Änderungen an der Benutzeroberfläche ein, einschließlich:

- Vollbild-Inhalt
- Balken Darstellung
- Tint-Farbe

<a name="fullscreen" />

### <a name="full-screen-content"></a>Voll Bildinhalt

IOS 7 ist so konzipiert, dass Anwendungen den gesamten Bildschirm nutzen können. Ansichts Controller werden nun durch eine Statusleiste und eine Navigationsleiste überlappend angezeigt, wenn eine vorhanden ist, anstatt unterhalb des Status und der Navigationsleisten angezeigt zu werden.

Wenn Sie Ihre Anwendung für IOS 7 vorbereiten, können Sie untergeordnete Sichten mit *Interface Builder* oder dem *xamarin IOS-Designer*visuell ausrichten. Sie können auch eine der neuen APIs verwenden, um voll Bildinhalte Programm gesteuert zu verarbeiten. Diese APIs werden im folgenden vorgestellt.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>Toplayoutguide und bottomlayoutguide

 `TopLayoutGuide` und `BottomLayoutGuide` dienen als Referenz für den Beginn oder das Ende von Sichten, sodass der Inhalt nicht durch eine durchlässiges `UIKit` Leiste überlagert wird, wie im folgenden Beispiel gezeigt:

 [![](ios7-ui-images/clipped.png "Sample content not overlapped by a translucent UIKit bar")](ios7-ui-images/clipped.png#lightbox)

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

Wir können den obigen Wert verwenden, um die Verschiebung des `ImageView`von oben nach unten auf dem Bildschirm festzulegen, damit das gesamte Bild sichtbar ist:

 [![](ios7-ui-images/good2.png "Example ImageViews displacement from the top of the screen")](ios7-ui-images/good2.png#lightbox)

Ein funktionierendes Beispiel finden Sie unter [imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/) .

Der Verschiebungs Wert wird dynamisch generiert, nachdem die Ansicht der Hierarchie hinzugefügt wurde. Daher wird beim Versuch, `TopLayoutGuide` und `BottomLayoutGuide` Werte in `ViewDidLoad` zu lesen, der Wert 0 zurückgegeben. Berechnen Sie den Wert, nachdem die Ansicht geladen wurde, z. b. in der `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` und `BottomLayoutGuide` werden in ios 11 zugunsten des neuen sicheren Bereichs Layouts eingestellt. Apple hat angegeben, dass die Verwendung des sicheren Bereichs mit der IOS-Version vor IOS 11 kompatibel ist. Weitere Informationen finden Sie im Handbuch [Aktualisieren Ihrer APP für IOS 11](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) .

#### <a name="edgesforextendedlayout"></a>Edgesforextendedlayout

Diese API gibt an, welche Ränder einer Ansicht auf den Vollbildmodus erweitert werden sollen, unabhängig von der Strich Durchlässigkeit. In ios 7 werden Navigationsleisten und Symbolleisten oberhalb der Ansicht des Controllers oberhalb der Ansicht des Controllers angezeigt, anders als in früheren IOS-Versionen, in denen Sie nicht denselben Platz belegen. Die IOS 7 Fotos-Anwendung veranschaulicht den Standard `UIViewController.EdgesForExtendedLayout` Wert `UIRectEdge.All`. Diese Einstellung füllt alle vier Ränder in der Ansicht mit Inhalt aus, wodurch die Überlappung und der voll Bildschirm wirksam werden:

 [![](ios7-ui-images/photos.png "Sample EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Wenn Sie auf das Bild tippen, werden die Balken entfernt und das Bild voll Bildschirm angezeigt:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout with the bars removed")](ios7-ui-images/photos2.png#lightbox)

Da der voll Bildinhalt standardmäßig verwendet wird, werden für IOS 6 konfigurierte Anwendungen als Teil der Ansicht abgeschnitten, wie im folgenden Screenshot gezeigt:

 [![](ios7-ui-images/clipped.png "Apps configured for iOS 6 will have part of the view clipped, as in this screenshot")](ios7-ui-images/clipped.png#lightbox)

Wenn Sie die `UIViewController.EdgesForExtendedLayout`-Eigenschaft ändern, wird dieses Verhalten angepasst. Wir können angeben, dass die Ansicht keine Kanten ausfüllen soll, sodass die Ansicht keine Inhalte in dem Bereich anzeigt, der von Navigations-oder Symbolleisten (bei jeder Ausrichtung) belegt wird:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

In unserer APP wird angezeigt, dass die Ansicht erneut positioniert wird, sodass das gesamte Bild sichtbar ist:

 [![](ios7-ui-images/good.png "Example with whole image visible")](ios7-ui-images/good.png#lightbox)

Beachten Sie, dass die Auswirkungen der `TopLayoutGuide/BottomLayoutGuide`-und `EdgesForExtendedLayout`-APIs ähnlich sind, dass Sie unterschiedliche Ziele erfüllen sollen. Wenn Sie die `EdgesForExtendedLayout` Einstellung von der Standardeinstellung ändern, werden die ausgeschnittenen Sichten in Anwendungen, die für IOS 6 entwickelt wurden, möglicherweise behoben, aber ein guter IOS 7-Entwurf sollte die Vollbild-Ästhetik berücksichtigen und eine voll Bild Anzeige bieten, wobei `TopLayoutGuide` und `BottomLayoutGuide` ordnungsgemäß Positionieren Sie Inhalte, die für den Benutzer an einem bequemen Ort bearbeitet werden sollen.

Ein funktionierendes Beispiel finden Sie unter [imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/) .

### <a name="status-and-navigation-bars"></a>Status und Navigationsleisten

Die Statusleiste und die Navigationsleisten werden mit Transparenz gerendert. Status leisten sind transparent, während Symbolleisten und Navigationsleisten durchsichtig und verschwommen sind, um das Gefühl der Tiefe in der Benutzeroberfläche zu vermitteln. Der folgende Screenshot zeigt diese Unschärfe und Transparenz, bei der die blaue Hintergrundfarbe der Auflistungs Ansicht sowohl über den Status als auch über die Navigationsleisten angezeigt wird, sodass Sie hell blau dargestellt werden:

 ![](ios7-ui-images/transparent-navbar.png "Sample Status and Navigation Bar blurring")

#### <a name="status-bar-styles"></a>StatusBar-Stile

Zusammen mit der Unschärfe und Transparenz kann der Vordergrund einer Statusleiste entweder hell oder dunkel sein (dunkel ist die Standardeinstellung). Der Status leisten Stil kann über den Ansichts Controller festgelegt werden. Mit einem Ansichts Controller kann auch festgelegt werden, ob die Statusleiste ausgeblendet oder angezeigt wird.

Der folgende Code überschreibt z. b. die `PreferredStatusBarStyle`-Methode eines Ansichts Controllers, damit die Statusleiste einen hellen Vordergrund anzeigt:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Dadurch wird die Statusleiste wie folgt angezeigt:

 ![](ios7-ui-images/light-status-bar.png "Sample Status Bar")

Um die Statusleiste im Code des Ansichts Controllers auszublenden, überschreiben Sie `PrefersStatusBarHidden`, wie unten dargestellt:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Dadurch wird die Statusleiste ausgeblendet:

 ![](ios7-ui-images/status-bar-hidden.png "Status Bar hidden")

### <a name="tint-color"></a>Tint-Farbe

Schaltflächen werden nun als weniger Chrome-Text angezeigt. Die Textfarbe kann mithilfe der neuen `TintColor`-Eigenschaft auf `UIView`gesteuert werden. Durch das Festlegen des `TintColor` wird die Farbe auf die gesamte Ansichts Hierarchie für die Ansicht angewendet, die Sie festlegt. Wenn Sie eine `TintColor`in einer APP anwenden möchten, legen Sie Sie auf dem `Window`fest. Sie können auch erkennen, wenn die Tönungs-Farbe über die `UIView.TintColorDidChange`-Methode geändert wird.

Der folgende Screenshot zeigt z. b. die Auswirkung der Änderung der Tönungs-Farbe in der Ansicht eines Navigations Controllers auf lila:

 ![](ios7-ui-images/tint-color.png "Purple tint color on a navigation controllers view")

Die Tönungs-Farbe kann auch auf Bilder angewendet werden, wenn die `RenderingMode` auf `UIImageRenderingMode.AlwaysTemplate`festgelegt ist.

> [!IMPORTANT]
> Die tint-Farbe kann nicht mit `UIAppearance`festgelegt werden.

### <a name="dynamic-type"></a>Dynamischer Typ

In ios 7 kann der Benutzer die Textgröße in den Systemeinstellungen angeben. Beim dynamischen Typ wird die Schriftart dynamisch angepasst, sodass Sie unabhängig von der Größe gut aussieht. `UIFont.PreferredFontForTextStyle` sollte verwendet werden, um eine Schriftart zu erhalten, die für die benutzergesteuerte Größe optimiert ist.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden die Änderungen an Benutzeroberflächen Elementen in ios 7 behandelt. Es werden einige der Änderungen untersucht, die an Sichten und Steuerelementen in UIKit vorgenommen werden, und sowohl die visuellen Änderungen als auch Änderungen an verwandten APIs hervorheben. Schließlich werden neue APIs eingeführt, um mit voll Bildinhalten, der neuen Unterstützung für Tönungs-Farben und dem dynamischen Typ zu arbeiten.

## <a name="related-links"></a>Verwandte Links

- [Imageviewer (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates)
