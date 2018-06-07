---
title: Einführung in iOS 6
description: Dieses enthält Dokumentenlinks zu Anleitungen, die in Ios6 eingeführte Funktionen zu beschreiben. Auflistungsansichten, PassKit, soziale Framework und Änderungen an StoreKit werden alle erläutert.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: cf623c7788137106ddbb2c23c69465f205a5a400
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787613"
---
# <a name="introduction-to-ios-6"></a>Einführung in iOS 6

_iOS 6 enthält eine Vielzahl neuer Technologien für die Entwicklung von apps, die C#-Entwickler Xamarin.iOS 6 Skriptentwicklern bereitstellt._

[ ![](images/ios6-large.jpg "Das iOS 6-logo")](images/ios6-large.jpg#lightbox)

Mit iOS 6 und 6 Xamarin.iOS haben Entwickler jetzt eine Fülle von Funktionen zur Verfügung, iOS-Anwendungen, einschließlich jener, Ziel iPhone 5 erstellen.
Dieses Dokument listet einige der interessanteren verfügbaren neuen Funktionen und Links zu Artikeln, die für jedes Thema. Darüber hinaus berührt es auf wenige Änderungen, die Bedeutung, da Entwickler mit iOS 6 und neue auflösen iPhone 5 verschieben.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Einführung in die Auflistungsansichten](~/ios/user-interface/controls/uicollectionview.md)

Auflistungsansichten können Inhalte mithilfe von beliebigen Layouts angezeigt werden sollen. Raster-ähnliches Layouts ausgegeben, einfach zu erstellen, während Sie benutzerdefinierte Layouts sowie unterstützende Dateien können. Weitere Informationen finden Sie unter der [Einführung zu Auflistungsansichten](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)Handbuch.


## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[Introduction to PassKit](~/ios/platform/passkit.md) (Einführung in PassKit)

Im PassKit können Anwendungen für die Interaktion mit digitalen übergibt, die in der app Passbook verwaltet werden. Weitere Informationen finden Sie unter der [Einführung übergeben Kit-Handbuch](~/ios/platform/passkit.md).


##  <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[Einführung in EventKit](~/ios/platform/eventkit.md)

Das EventKit-Framework bietet eine Möglichkeit zum Zugriff auf Kalender, Kalenderereignisse und Daten in Erinnerungen Kalender-Datenbank gespeichert. Zugriff auf die Kalender und Kalender Ereignisse wurde verfügbar seit iOS 4, aber Ios6 macht jetzt den Zugriff auf Daten von Erinnerungen. Weitere Informationen finden Sie unter der [ich](~/ios/platform/eventkit.md) [Ntroduction auf EventKit](~/ios/platform/eventkit.md) Handbuch.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Einführung in die sozialen Framework](~/ios/platform/social-framework.md)

Das soziale Framework bietet eine einheitliche API, für die Interaktion mit sozialen Netzwerken, einschließlich Twitter und Facebook sowie SinaWeibo für Benutzer in China. Weitere Informationen finden Sie unter der [Einführung in die sozialen Framework](~/ios/platform/social-framework.md) Handbuch.


##  <a name="changes-to-storekitchanges-to-storekitmd"></a>[Änderungen an StoreKit](changes-to-storekit.md)

Apple hat zwei neue Funktionen im Store Kit eingeführt: erwerben und herunterladen iTunes oder App-Store-Inhalt von innerhalb Ihrer app und hosting von Inhaltsdateien für in-app-Käufe!. Weitere Informationen finden Sie unter der [Änderungen an den Store Kit](changes-to-storekit.md) Handbuch.


## <a name="other-changes"></a>Weitere Änderungen


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload und ViewDidUnload veraltet

Die `ViewWillUnload` und `ViewDidUnload` Methoden der `UIViewController` werden nicht mehr als Ios6 bezeichnet. In früheren Versionen von iOS können dieser Methoden unter verwendet wurden Anwendungen für das Speichern von Status vor dem Entladen einer Ansicht und Bereinigungscode.

Visual Studio für Mac würde z. B. eine Methode namens erstellen `ReleaseDesignerOutlets`, unten, die dann von aufgerufen würde `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Allerdings in iOS 6, ist es nicht mehr notwendig, `ReleaseDesignerOutlets`.   
   
   
   
Für den Bereinigungscode, iOS 6-Anwendungen die zu verwendende `DidReceiveMemoryWarning`. Aber der code, Aufrufe `Dispose` sollte nur selten verwendet werden und nur für rechenintensive Speicherobjekte wie unten:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Erneut aufrufen `Dispose` wie oben beschrieben sollten nur selten erforderlich. Im Allgemeinen, die die meisten Anwendungen ausführen, sollten ist, um Ereignishandler zu entfernen.

Für den Fall Zustand zu speichern, können Anwendungen führen hierzu finden Sie unter `ViewWillDisappear` und `ViewDidDisappear` anstelle von `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>iPhone 5 Auflösung

iPhone 5-Geräte haben eine 640 x 1136-Lösung. Anwendungen, die das Ziel von früheren Versionen von iOS erscheinen letterboxed bei Ausführung auf einem iPhone 5, wie unten dargestellt:

 [![](images/01-letterboxed.png "Anwendungen, die das Ziel von früheren Versionen von iOS werden bei Ausführung auf einem iPhone 5 letterboxed angezeigt.")](images/01-letterboxed.png#lightbox)

Damit die Anwendung angezeigt werden hinzufügen Vollbild auf iPhone 5, einfach ein Bild mit dem Namen `Default-568h@2x.png` mit einer Auflösung von 640 x 1136. Der folgende Screenshot zeigt die Anwendung ausgeführt wird, nachdem dieses Bild eingebunden wurde:

 [![](images/02-fullscreen.png "Diese bildschirmabbildung zeigt die Anwendung ausgeführt wird, nachdem dieses Bild eingebunden wurde")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Erstellen von Unterklassen für UINavigationBar

In iOS 6 `UINavigationBar` als Unterklasse werden können. Dies ermöglicht zusätzliche Kontrolle über das Aussehen und Verhalten von der `UINavigationBar`. Anwendungen können z. B. zum Hinzufügen von Unteransichten, animieren diese Sichten aus, und ändern die Grenzen der Unterklasse der `UINavigationBar`.

Der folgende Code zeigt ein Beispiel für ein untergeordnetes `UINavigationBar` , addiert eine `UIImageView`:

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

Mit diesem `UINavigationBar` Unterklasse führt in der Image-Ansicht angezeigt werden, wie im folgenden Screenshot gezeigt:

 [![](images/03-navbar.png "Diese UINavigationBar Unterklasse Ergebnisse verwenden in der Image-Ansicht angezeigt werden, wie in diesem Screenshot dargestellt")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Interface-Ausrichtung

Vor dem iOS 6-Anwendungen außer Kraft setzen konnte `ShouldAutorotateToInterfaceOrientation`, Rückgabe "true" für alle Ausrichtungen bestimmten Controllers unterstützt. Beispielsweise würde der folgende Code verwendet werden, nur Hochformat unterstützen:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

In iOS 6 `ShouldAutorotateToInterfaceOrientation` ist veraltet.
Stattdessen können Anwendungen überschreiben `GetSupportedInterfaceOrientations` auf dem Stamm-View-Controller wie unten dargestellt:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

Auf dem iPad, wird standardmäßig alle vier Ausrichtungen Wenn `GetSupportedInterfaceOrientation` ist nicht implementiert. Auf iPhone- und iPod Touch-Geräte, die Standardeinstellung ist alle Ausrichtungen außer `PortraitUpsideDown`.
