---
title: Einführung in iOS 6
description: Dieses Dokument enthält Links zu Leitfäden, in denen die in ios 6 eingeführten Features beschrieben werden. Alle Sammlungs Ansichten, passkit, das Social Framework und Änderungen an storekit werden erläutert.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: dc8f2c3a11dd283db46fd0a2530fcca435da75f9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752051"
---
# <a name="introduction-to-ios-6"></a>Einführung in iOS 6

_IOS 6 umfasst eine Reihe von neuen Technologien für die Entwicklung von apps, die xamarin. IOS 6 C# Entwicklern bietet._

[![](images/ios6-large.jpg "Das IOS 6-Logo")](images/ios6-large.jpg#lightbox)

Mit IOS 6 und xamarin. IOS 6 verfügen Entwickler nun über eine Vielzahl von Funktionen zur Erstellung von IOS-Anwendungen, einschließlich der Ziele für iPhone 5.
In diesem Dokument werden einige der spannendsten neuen Features aufgeführt, die verfügbar sind, sowie Links zu Artikeln für die einzelnen Themen. Außerdem werden einige Änderungen behandelt, die wichtig sind, wenn Entwickler zu IOS 6 und der neuen Auflösung von iPhone 5 wechseln.

## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Einführung in Sammlungs Ansichten](~/ios/user-interface/controls/uicollectionview.md)

Mithilfe von Auflistungs Ansichten können Inhalte mit beliebigen Layouts angezeigt werden. Sie ermöglichen die einfache Erstellung von Raster ähnlichen Layouts, während gleichzeitig auch benutzerdefinierte Layouts unterstützt werden. Weitere Informationen finden Sie in der [Anleitung](~/ios/user-interface/controls/uicollectionview.md) zur [Einführung in Sammlungsansichten](~/ios/user-interface/controls/uicollectionview.md).

## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[Introduction to PassKit](~/ios/platform/passkit.md) (Einführung in PassKit)

Das passkit-Framework ermöglicht Anwendungen die Interaktion mit digitalen Übergängen, die in der Passbook-App verwaltet werden. Weitere Informationen finden [Sie im Leitfaden Einführung in das Pass-Kit](~/ios/platform/passkit.md).

## <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[Einführung in eventkit](~/ios/platform/eventkit.md)

Das eventkit-Framework bietet eine Möglichkeit, auf die Kalender-, Kalender-und Erinnerungs Daten zuzugreifen, die von der Kalender Datenbank gespeichert werden. Der Zugriff auf die Kalender-und Kalenderereignisse war seit IOS 4 verfügbar, aber IOS 6 macht jetzt den Zugriff auf Erinnerungs Daten verfügbar. Weitere Informationen finden Sie im Leitfaden [I](~/ios/platform/eventkit.md) [Einführung to eventkit](~/ios/platform/eventkit.md) .

## <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Einführung in das Social Framework](~/ios/platform/social-framework.md)

Das Social Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook, sowie sinaweibo für Benutzer in China. Weitere Informationen finden [Sie unter Einführung in das Handbuch zum Social Framework](~/ios/platform/social-framework.md) .

## <a name="changes-to-storekitchanges-to-storekitmd"></a>[Änderungen an StoreKit](changes-to-storekit.md)

Apple hat zwei neue Features im Store-Kit eingeführt: das kaufen und Herunterladen von iTunes-oder App Store-Inhalten in Ihrer APP und das Hosting Ihrer Inhalts Dateien für in-App-Käufe!. Weitere Informationen finden Sie im Handbuch [zu Änderungen am Store-Kit](changes-to-storekit.md) .

## <a name="other-changes"></a>Weitere Änderungen

### <a name="viewwillunload-and-viewdidunload-deprecated"></a>"Viewwillentladen" und "viewdidunload" als veraltet markiert

Die `ViewWillUnload` - `ViewDidUnload` Methode und `UIViewController` die-Methode von werden in ios 6 nicht mehr aufgerufen. In früheren Versionen von IOS wurden diese Methoden möglicherweise von Anwendungen zum Speichern des Zustands vor dem Entladen einer Ansicht und Bereinigungs Code verwendet.

Beispielsweise wird Visual Studio für Mac eine Methode namens `ReleaseDesignerOutlets`erstellen, die unten dargestellt ist, die dann von `ViewDidUnload`aufgerufen wird:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Allerdings ist es in ios 6 nicht mehr erforderlich, aufzurufen `ReleaseDesignerOutlets`.   

Für Bereinigungs Code sollten IOS 6-Anwendungen `DidReceiveMemoryWarning`verwenden. Code, der aufruft `Dispose` , sollte jedoch sparsam und nur für Arbeitsspeicher intensive Objekte verwendet werden, wie unten dargestellt:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Auch hier sollten `Dispose` Sie nur selten aufrufen, wie oben beschrieben. Im Allgemeinen sollten die meisten Anwendungen die Ereignishandler entfernen.

Für den Fall, dass der Zustand gespeichert wird, können Anwendungen `ViewWillDisappear` dies `ViewDidDisappear` in und `ViewWillUnload`anstelle von durchführen.

### <a name="iphone-5-resolution"></a>iPhone 5-Auflösung

bei iPhone 5-Geräten gibt es eine Auflösung von 640 x1136. Anwendungen, die auf frühere Versionen von IOS abzielen, werden bei der Ausführung auf einem iPhone 5 als Briefkasten angezeigt, wie unten dargestellt:

 [![](images/01-letterboxed.png "Anwendungen, die auf frühere Versionen von IOS ausgerichtet sind, werden bei der Ausführung auf einem iPhone 5 als Letterbox angezeigt.")](images/01-letterboxed.png#lightbox)

Damit die Anwendung auf iPhone 5 voll Bildschirm angezeigt werden kann, fügen Sie einfach ein Bild namens `Default-568h@2x.png` mit einer Auflösung von 640 x 1136 hinzu. Der folgende Screenshot zeigt die Anwendung, die ausgeführt wird, nachdem dieses Image eingeschlossen wurde:

 [![](images/02-fullscreen.png "Dieser Screenshot zeigt die Anwendung, die ausgeführt wird, nachdem dieses Image enthalten ist.")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Unterklassen UINavigationBar

In ios 6 `UINavigationBar` kann unter klassifiziert werden. Dies ermöglicht eine zusätzliche Kontrolle über das Erscheinungsbild von `UINavigationBar`. Beispielsweise können Anwendungen Unterklassen hinzufügen, um untergeordnete Sichten hinzuzufügen, diese Sichten zu animieren und `UINavigationBar`die Begrenzungen von zu ändern.

Der folgende Code zeigt ein Beispiel für eine Unterklasse `UINavigationBar` , die einen `UIImageView`hinzufügt:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Verwenden `UINavigationBar` Sie den `UIToolbar` `UINavigationController` -`UINavigationController` Konstruktor, der den Typ von und annimmt, wie unten dargestellt, um einem eine untergeordnete Klasse hinzuzufügen: `UINavigationBar`

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Die Verwendung `UINavigationBar` dieser Unterklasse führt dazu, dass die Bildansicht wie im folgenden Screenshot gezeigt angezeigt wird:

 [![](images/03-navbar.png "Die Verwendung dieser UINavigationBar-Unterklasse führt dazu, dass die Bildansicht angezeigt wird, wie in diesem Screenshot gezeigt.")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Schnittstellen Ausrichtung

Vor IOS 6 konnten Anwendungen außer Kraft `ShouldAutorotateToInterfaceOrientation`setzen, und die Rückgabe von true für jede Ausrichtungen, die der jeweilige Controller unterstützte Der folgende Code wird z. b. verwendet, um nur das Hochformat zu unterstützen:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

In ios 6 `ShouldAutorotateToInterfaceOrientation` ist veraltet.
Stattdessen können Anwendungen auf `GetSupportedInterfaceOrientations` dem Root View Controller überschreiben, wie unten dargestellt:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

Auf dem iPad werden standardmäßig alle vier Ausrichtungen `GetSupportedInterfaceOrientation` verwendet, wenn nicht implementiert ist. Bei iPhone und iPod Touchscreen ist der Standardwert alle Ausrichtungen `PortraitUpsideDown`außer.
