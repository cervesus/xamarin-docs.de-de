---
title: Einführung in iOS 6
description: Dieses Dokument enthält Links zu Leitfäden, die in iOS 6 eingeführte Features beschrieben. Auflistungsansichten, PassKit, bei dem soziale Framework, und Änderungen an StoreKit werden alle erläutert.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: f0cfeaa049355c2b524ead748696eafd884a1c54
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865700"
---
# <a name="introduction-to-ios-6"></a>Einführung in iOS 6

_iOS 6 enthält eine Reihe von neuen Technologien für die Entwicklung von apps, die Xamarin.iOS-6 bringt C# Entwickler._

[![](images/ios6-large.jpg "Das iOS 6-logo")](images/ios6-large.jpg#lightbox)

Mit iOS 6 und Xamarin.iOS-6 verfügen Entwickler jetzt über eine Fülle von Funktionen zum Erstellen von iOS-Anwendungen, einschließlich jener, Ziel-iPhone 5 orchestrierungsmöglichkeiten.
Dieses Dokument listet einige der Links zu Artikeln, die für jedes Thema und weitere interessante neue Features, die verfügbar sind. Darüber hinaus betrifft es auf ein paar Änderungen, die wichtig, da Entwickler mit iOS 6 und die neue Auflösung iPhone 5 wechseln.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Einführung in die von Auflistungsansichten](~/ios/user-interface/controls/uicollectionview.md)

Auflistungsansichten können Inhalte mithilfe von beliebigen Layouts angezeigt werden. Sie können problemlos Raster-ähnliches Layouts standardmäßig unterstützt auch benutzerdefinierte Layouts erstellen. Weitere Informationen finden Sie die [Einführung in die Auflistungsansichten](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)Guide.


## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[Introduction to PassKit](~/ios/platform/passkit.md) (Einführung in PassKit)

Das PassKit-Framework kann Anwendungen mit digitalen übergibt interagieren, die in die Passbook-app verwaltet werden. Weitere Informationen finden Sie die [Guide Kit übergeben – Einführung](~/ios/platform/passkit.md).


## <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[Einführung in die EventKit](~/ios/platform/eventkit.md)

Das EventKit-Framework bietet eine Möglichkeit zum Zugriff auf den Kalender, Termine im Kalender und Erinnerungen-Daten, die Kalender-Datenbank speichert. Zugriff auf der Kalender und Kalender Ereignisse seit verfügbaren iOS 4, aber iOS 6 stellt jetzt den Zugriff auf Daten von Erinnerungen. Weitere Informationen finden Sie unter den [ich](~/ios/platform/eventkit.md) [Einführung in EventKit](~/ios/platform/eventkit.md) Guide.


## <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Einführung in die sozialen Framework](~/ios/platform/social-framework.md)

Das soziale Framework bietet eine einheitliche API für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook als auch SinaWeibo für Benutzer in China. Weitere Informationen finden Sie die [Einführung in die sozialen Framework](~/ios/platform/social-framework.md) Guide.


## <a name="changes-to-storekitchanges-to-storekitmd"></a>[Änderungen an StoreKit](changes-to-storekit.md)

Apple hat zwei neue Features im Store Kit eingeführt: erwerben und herunterladen iTunes oder App-Store-Inhalt aus, in Ihrer app-hosting von Inhaltsdateien für in-app-Käufe. Weitere Informationen finden Sie die [Änderungen an den Store Kit](changes-to-storekit.md) Guide.


## <a name="other-changes"></a>Weitere Änderungen


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload und ViewDidUnload als veraltet markiert

Die `ViewWillUnload` und `ViewDidUnload` Methoden `UIViewController` in iOS 6 nicht mehr aufgerufen werden. In früheren Versionen von iOS diese Methoden möglicherweise unter verwendet wurden Anwendungen zum Speichern von Status vor dem Entladen einer Ansicht und Bereinigungscode.

Beispielsweise würde Visual Studio für Mac erstellen eine Methode namens `ReleaseDesignerOutlets`, unten, die dann von aufgerufen würde `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Allerdings in iOS 6, es ist nicht mehr notwendig, `ReleaseDesignerOutlets`.   
   
   
   
Für den Bereinigungscode, iOS 6-Anwendungen verwenden sollten `DidReceiveMemoryWarning`. Jedoch code, ruft `Dispose` sollten sparsam eingesetzt werden und nur für rechenintensive Speicherobjekte wie unten:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

In diesem Fall aufrufen `Dispose` wie oben beschrieben sollten nur selten erforderlich. Im Allgemeinen, die die meisten Anwendungen müssen ist, um Ereignishandler zu entfernen.

Im Fall des Status speichern, können Anwendungen führen in `ViewWillDisappear` und `ViewDidDisappear` anstelle von `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>iPhone 5 Auflösung

iPhone 5-Geräte haben eine Auflösung von 640 x 1136. Anwendungen, die das Ziel von früheren Versionen von iOS erscheinen letterboxed bei Ausführung auf einem iPhone 5, wie unten dargestellt:

 [![](images/01-letterboxed.png "Anwendungen, die von früheren Versionen von iOS als Ziel werden bei Ausführung auf ein iPhone 5 letterboxed angezeigt.")](images/01-letterboxed.png#lightbox)

Damit die Anwendung angezeigt werden Vollbildmodus auf iPhone 5, einfach ein Image namens hinzufügen `Default-568h@2x.png` mit einer Auflösung von 640 x 1136. Der folgende Screenshot zeigt die Anwendung ausgeführt wird, nachdem Sie dieses Image eingeschlossen wurde:

 [![](images/02-fullscreen.png "Dieser Screenshot zeigt die Anwendung ausgeführt wird, nachdem Sie dieses Image eingeschlossen wurde")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Erstellen von Unterklassen für UINavigationBar

In iOS 6 `UINavigationBar` kann in Unterklassen unterteilt werden kann. Dies ermöglicht zusätzliche Kontrolle über das Aussehen und Verhalten der `UINavigationBar`. Anwendungen können z. B. Unterklasse zum Hinzufügen von Unteransichten, animieren diese Ansichten aus, und ändern die Grenzen des das `UINavigationBar`.

Der folgende Code zeigt ein Beispiel für ein untergeordnetes `UINavigationBar` , addiert einen `UIImageView`:

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

Ein untergeordnetes hinzufügen `UINavigationBar` auf eine `UINavigationController`, verwenden die `UINavigationController` Konstruktor, der den Typ des akzeptiert die `UINavigationBar` und `UIToolbar`, wie unten dargestellt:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Mit diesem `UINavigationBar` Unterklasse führt in der Image-Ansicht angezeigt wird, wie im folgenden Screenshot gezeigt:

 [![](images/03-navbar.png "Mithilfe dieses UINavigationBar Unterklasse führt in der Image-Ansicht angezeigt wird, wie im folgenden Screenshot gezeigt.")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Anfangsausrichtung der Schnittstelle

Bevor Sie iOS 6-Anwendungen können außer Kraft setzen `ShouldAutorotateToInterfaceOrientation`, Zurückgeben von true für alle Ausrichtungen bestimmten Controllers unterstützt. Beispielsweise würde der folgende Code verwendet werden, nur im Hochformat unterstützen:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

In iOS 6 `ShouldAutorotateToInterfaceOrientation` ist veraltet.
Stattdessen können Anwendungen überschreiben `GetSupportedInterfaceOrientations` in der Root View Controller wie unten dargestellt:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

Auf dem iPad, dies ist standardmäßig auf alle vier Ausrichtungen Wenn `GetSupportedInterfaceOrientation` ist nicht implementiert. Auf dem iPhone und iPod Touch, der Standardwert ist alle Ausrichtungen außer `PortraitUpsideDown`.
