---
title: Erstellen von IOS-Benutzeroberflächen im Code in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie Code verwenden, um eine Benutzeroberfläche für eine xamarin. IOS-APP zu erstellen. Es werden Ansichts Controller, eine Ansichts Hierarchie, die Verarbeitung einer Drehung und vieles mehr erläutert.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: edd49cc891a86d3323bab319ab811e85f9148640
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997097"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Erstellen von IOS-Benutzeroberflächen im Code in xamarin. IOS

Die Benutzeroberfläche einer IOS-APP ist wie eine Store Front – die Anwendung erhält in der Regel ein Fenster, aber Sie kann das Fenster mit beliebig vielen Objekten auffüllen, und die Objekte und Anordnungen können je nachdem, was die App anzeigen soll, geändert werden. Die Objekte in diesem Szenario – die Dinge, die der Benutzer sieht – werden als Ansichten bezeichnet. Um einen einzelnen Bildschirm in einer Anwendung zu erstellen, werden Ansichten in einer Hierarchie von Inhalts Ansichten übereinander gestapelt, und die Hierarchie wird von einem einzelnen Ansichts Controller verwaltet. Anwendungen mit mehreren Bildschirmen weisen mehrere Hierarchien von Inhaltsansichten auf. Jede davon verfügt über ihren eigenen Ansichtscontroller, und die Anwendung platziert Ansichten in das Fenster, um je nach dem vom Benutzer verwendeten Bildschirm eine andere Hierarchie von Inhaltsansichten zu erstellen.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller, die die Benutzeroberfläche auf den Gerätebildschirm bringen:

[![Dieses Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Sichten, unter Sichten und dem Ansichts Controller.](ios-code-only-images/image9.png)](ios-code-only-images/image9.png#lightbox)

Diese Sicht Hierarchien können mithilfe des [Xamarin Designer für IOS](~/ios/user-interface/designer/index.md) in Visual Studio erstellt werden. es ist jedoch sinnvoll, ein grundlegendes Verständnis der vollständigen Arbeit im Code zu haben. In diesem Artikel werden einige grundlegende Punkte erläutert, mit denen Sie sich mit der Entwicklung von nur-Code-Benutzeroberflächen verbinden können.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller, die die Benutzeroberfläche auf den Gerätebildschirm bringen:

[![Dieses Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Sichten, unter Sichten und dem Ansichts Controller.](ios-code-only-images/image9.png)](ios-code-only-images/image9.png#lightbox)

Diese Sicht Hierarchien können mithilfe des- [Xamarin Designer für IOS](~/ios/user-interface/designer/index.md) in Visual Studio für Mac erstellt werden. es ist jedoch sinnvoll, ein grundlegendes Verständnis der vollständigen Arbeit im Code zu haben. In diesem Artikel werden einige grundlegende Punkte erläutert, mit denen Sie sich mit der Entwicklung von nur-Code-Benutzeroberflächen verbinden können.

-----

## <a name="creating-a-code-only-project"></a>Erstellen eines reinen Code Projekts

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>leere IOS-Projektvorlage

Erstellen Sie zunächst ein IOS-Projekt in Visual Studio mithilfe der **Datei > neues Projekt > Visual c# > iPhone & iPad > IOS-app (xamarin)** , wie unten gezeigt:

[![Dialogfeld "Neues Projekt"](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Wählen Sie dann die Vorlage **leere App** -Projekt aus:

[![Dialog Feld "Vorlage auswählen"](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Mit der leeren Projektvorlage werden dem Projekt vier Dateien hinzugefügt:

[![Projektdateien](ios-code-only-images/empty-project.w157-sml.png "Projektdateien")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.cs** -enthält eine `UIApplicationDelegate` Unterklasse, `AppDelegate` , die verwendet wird, um Anwendungs Ereignisse von IOS zu verarbeiten. Das Anwendungsfenster wird in der `AppDelegate` -Methode von erstellt `FinishedLaunching` .
1. **Main.cs** -enthält den Einstiegspunkt für die Anwendung, der die-Klasse für angibt `AppDelegate` .
1. **Info. plist** -Eigenschaften Listen Datei, die Konfigurationsinformationen für die Anwendung enthält.
1. **Berechtigungen. plist** – Eigenschaften Listen Datei, die Informationen zu den Funktionen und Berechtigungen der Anwendung enthält.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

## <a name="ios-templates"></a>IOS-Vorlagen

Visual Studio für Mac stellt keine leere Vorlage bereit. Alle Vorlagen verfügen über Storyboard-Unterstützung, die Apple als primäre Methode zum Erstellen einer Benutzeroberfläche empfiehlt. Allerdings ist es möglich, die Benutzeroberfläche vollständig im Code zu erstellen.

Die folgenden Schritte führen Sie durch das Entfernen des Storyboards aus einer Anwendung:

1. Erstellen Sie mit der Vorlage für die Einzelansicht-App ein neues IOS-Projekt:

    [![Verwenden der Einzelansicht-App-Vorlage](ios-code-only-images/single-view-app.png)](ios-code-only-images/single-view-app.png#lightbox)

1. Löschen Sie `Main.Storyboard` die `ViewController.cs` Dateien und. Löschen Sie **nicht** `LaunchScreen.Storyboard` . Der Ansichts Controller sollte gelöscht werden, da es sich um den Code Behind für den Ansichts Controller handelt, der im Storyboard erstellt wurde:
1. Stellen Sie sicher, dass Sie im Popup Dialogfeld **Löschen** auswählen:

    [![Wählen Sie im Popup Dialogfeld Löschen aus.](ios-code-only-images/delete.png)](ios-code-only-images/delete.png#lightbox)

1. Löschen Sie die Informationen in der Info. plist-Datei in der **Bereitstellungs Info > Haupt Schnittstellen** Option:

    [![Löschen der Informationen in der Haupt Schnittstellen Option](ios-code-only-images/main-interface.png)](ios-code-only-images/main-interface.png#lightbox)

1. Fügen Sie abschließend der- `FinishedLaunching` Methode in der appdelegatklasse den folgenden Code hinzu:

    ```csharp
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
    ```

Der Code, der `FinishedLaunching` in Schritt 5 oben der-Methode hinzugefügt wurde, ist die minimale Menge an Code, der zum Erstellen eines Fensters für Ihre IOS-Anwendung erforderlich ist.

-----

IOS-Anwendungen werden mit dem [MVC-Muster](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc)erstellt. Der erste Bildschirm, der von einer Anwendung angezeigt wird, wird aus dem Root View Controller des Fensters erstellt. Weitere Informationen zum MVC-Muster selbst finden Sie im Handbuch für [Hello, IOS-Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) .

Mit der-Implementierung für das `AppDelegate` , das von der Vorlage hinzugefügt wurde, wird das Anwendungsfenster erstellt, in dem nur ein für jede IOS-Anwendung vorhanden ist, und es wird mit folgendem Code sichtbar:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Wenn Sie diese Anwendung jetzt ausführen, erhalten Sie wahrscheinlich eine Ausnahme, die angibt, dass die Ausnahme ausgelöst wurde `Application windows are expected to have a root view controller at the end of application launch` . Fügen Sie einen Controller hinzu, und legen Sie ihn als root View Controller der APP an.

## <a name="adding-a-controller"></a>Hinzufügen eines Controllers

Ihre APP kann viele Ansichts Controller enthalten, muss jedoch über einen Stamm Ansichts Controller verfügen, um alle Ansichts Controller zu steuern.  Fügen Sie dem-Fenster einen Controller hinzu, indem `UIViewController` Sie eine-Instanz erstellen und Sie auf die- `Window.RootViewController` Eigenschaft festlegen:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Jedem Controller ist eine zugeordnete Ansicht zugeordnet, auf die über die-Eigenschaft zugegriffen werden kann `View` . Der obige Code ändert die- `BackgroundColor` Eigenschaft der Sicht `UIColor.LightGray` so, dass Sie sichtbar wird, wie unten dargestellt:

 [![Der Hintergrund der Sicht ist ein sichtbares hellgrau.](ios-code-only-images/image1.png)](ios-code-only-images/image1.png#lightbox)

Wir könnten auch eine beliebige `UIViewController` Unterklasse als `RootViewController` auf diese Weise festlegen, einschließlich der Controller von UIKit und derjenigen, die wir selbst schreiben. Mit dem folgenden Code wird beispielsweise ein `UINavigationController` als hinzugefügt `RootViewController` :

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Dies erzeugt den Controller, der im Navigations Controller geschachtelt ist, wie unten dargestellt:

 [![Der Controller, der im Navigations Controller geschachtelt ist.](ios-code-only-images/image2.png)](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Erstellen eines Ansichts Controllers

Nachdem Sie nun gesehen haben, wie Sie einen Controller als `RootViewController` des Fensters hinzufügen, sehen wir uns an, wie Sie einen benutzerdefinierten Ansichts Controller im Code erstellen.

Fügen Sie eine neue Klasse mit dem Namen hinzu, `CustomViewController` wie unten dargestellt:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Neue Klasse mit dem Namen "customviewcontroller" hinzufügen](ios-code-only-images/customviewcontroller.w157-sml.png)](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Neue Klasse mit dem Namen "customviewcontroller" hinzufügen](ios-code-only-images/new-file.png)](ios-code-only-images/new-file.png#lightbox)

-----

Die Klasse sollte `UIViewController` wie im folgenden gezeigt von erben, der sich im- `UIKit` Namespace befindet:

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

## <a name="initializing-the-view"></a>Initialisieren der Sicht

`UIViewController`enthält eine Methode namens `ViewDidLoad` , die aufgerufen wird, wenn der Ansichts Controller zum ersten Mal in den Arbeitsspeicher geladen wird. Dies ist ein geeigneter Speicherort für die Initialisierung der Ansicht, z. b. das Festlegen der Eigenschaften.

Der folgende Code fügt z. b. eine Schaltfläche und einen Ereignishandler hinzu, um einen neuen Ansichts Controller beim Drücken der Schaltfläche auf den Navigations Stapel zu übertragen:

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Wenn Sie diesen Controller in Ihre Anwendung laden und die einfache Navigation veranschaulichen möchten, erstellen Sie eine neue Instanz von `CustomViewController` . Erstellen Sie einen neuen Navigations Controller, übergeben Sie die View Controller-Instanz, und legen Sie den neuen Navigations Controller `RootViewController` wie zuvor auf das Fenster in fest `AppDelegate` :

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Wenn nun die Anwendung geladen wird, `CustomViewController` wird der in einen Navigations Controller geladen:

 [![Der customviewcontroller wird in einen Navigations Controller geladen.](ios-code-only-images/customvc.png)](ios-code-only-images/customvc.png#lightbox)

Wenn Sie auf die Schaltfläche klicken, wird ein neuer Ansichts Controller per _Push_ auf den Navigations Stapel Übertragung

[![Ein neuer Ansichts Controller, der auf den Navigations Stapel verschoben wird.](ios-code-only-images/customvca.png)](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Aufbau der Ansichts Hierarchie

Im obigen Beispiel haben wir begonnen, eine Benutzeroberfläche im Code zu erstellen, indem Sie dem Ansichts Controller eine Schaltfläche hinzufügen.

IOS-Benutzeroberflächen bestehen aus einer Ansichts Hierarchie. Weitere Sichten, wie z. b. Bezeichnungen, Schaltflächen, Schieberegler usw., werden als unter Ansichten einer übergeordneten Ansicht hinzugefügt.

Beispielsweise bearbeiten wir die `CustomViewController` , um einen Anmeldebildschirm zu erstellen, auf dem der Benutzer einen Benutzernamen und ein Kennwort eingeben kann. Der Bildschirm besteht aus zwei Textfeldern und einer Schaltfläche.

### <a name="adding-the-text-fields"></a>Hinzufügen von Textfeldern

Entfernen Sie zunächst die Schaltfläche und den Ereignishandler, der im Abschnitt [Initialisieren der Sicht](#initializing-the-view) hinzugefügt wurde.

Fügen Sie ein Steuerelement für den Benutzernamen hinzu, indem Sie einen erstellen und initialisieren `UITextField` und ihn dann der Ansichts Hierarchie hinzufügen, wie unten dargestellt:

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

Wenn Sie das Erstellen `UITextField` , legen Sie die `Frame` -Eigenschaft fest, um deren Position und Größe zu definieren. In ios befindet sich die Koordinate 0 in der oberen linken Ecke mit + x nach rechts und + y nach unten. Nachdem Sie das `Frame` zusammen mit einigen anderen Eigenschaften festgelegt haben, wird aufgerufen, um `View.AddSubview` der `UITextField` Ansichts Hierarchie hinzuzufügen. Dadurch wird `usernameField` eine unter Ansicht der- `UIView` Instanz, auf die die- `View` Eigenschaft verweist. Eine unter Ansicht wird mit einer z-Reihenfolge hinzugefügt, die höher als die übergeordnete Ansicht ist, sodass Sie vor der übergeordneten Ansicht auf dem Bildschirm angezeigt wird.

Die Anwendung mit der `UITextField` enthaltenen wird unten angezeigt:

 [![Die Anwendung, in der das UITextField enthalten ist](ios-code-only-images/image4.png)](ios-code-only-images/image4.png#lightbox)

Wir können ein `UITextField` für das Kennwort auf ähnliche Weise hinzufügen, nur dieses Mal legen wir die- `SecureTextEntry` Eigenschaft auf "true" fest, wie unten dargestellt:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField);
      View.AddSubview(passwordField);
   }
}

```

`SecureTextEntry = true`Durch die Einstellung wird der Text, der `UITextField` vom Benutzer eingegeben wurde, wie unten dargestellt ausgeblendet:

 [![Durch Festlegen von securetextentry true wird der vom Benutzer eingegebene Text ausgeblendet.](ios-code-only-images/image4a.png)](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Hinzufügen der Schaltfläche

Als Nächstes fügen wir eine Schaltfläche hinzu, sodass der Benutzer den Benutzernamen und das Kennwort übermitteln kann. Die Schaltfläche wird wie jedes andere Steuerelement der Ansichts Hierarchie hinzugefügt, indem Sie Sie als Argument an die-Methode der übergeordneten Ansicht übergibt `AddSubview` .

Der folgende Code fügt die Schaltfläche hinzu und registriert einen Ereignishandler für das- `TouchUpInside` Ereignis:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Wenn dieses Fenster vorhanden ist, wird der Anmeldebildschirm nun wie unten dargestellt angezeigt:

 [![Der Anmeldebildschirm](ios-code-only-images/image5.png)](ios-code-only-images/image5.png#lightbox)

Anders als in früheren Versionen von IOS ist der Standard Hintergrund für Schaltflächen transparent. Durch Ändern der-Eigenschaft der Schaltfläche wird `BackgroundColor` Folgendes geändert:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Dies führt zu einer quadratischen Schaltfläche anstelle der üblichen gerundeten Schaltfläche. Verwenden Sie den folgenden Code Ausschnitt, um den abgerundeten Rand zu erhalten:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Mit diesen Änderungen sieht die Ansicht wie folgt aus:

[![Beispiel für die Durchführung der Ansicht](ios-code-only-images/image6.png)](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Hinzufügen mehrerer Ansichten zur Ansichts Hierarchie

IOS bietet eine Möglichkeit, der Ansichts Hierarchie mithilfe von mehrere Ansichten hinzuzufügen `AddSubviews` .

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton });
```

## <a name="adding-button-functionality"></a>Hinzufügen von Schaltfläche

Wenn auf eine Schaltfläche geklickt wird, erwarten Ihre Benutzer, dass etwas passiert. Beispielsweise wird eine Warnung angezeigt, oder die Navigation wird auf einem anderen Bildschirm ausgeführt.

Fügen Sie Code hinzu, um einen zweiten Ansichts Controller per Push auf den Navigations Stapel zu verschieben.

Erstellen Sie zunächst den zweiten Ansichts Controller:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Fügen Sie dann dem-Ereignis die-Funktion hinzu `TouchUpInside` :

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Die Navigation wird unten veranschaulicht:

[![Die Navigation ist in diesem Diagramm dargestellt.](ios-code-only-images/navigation.png)](ios-code-only-images/navigation.png#lightbox)

Beachten Sie, dass IOS bei Verwendung eines Navigations Controllers standardmäßig eine Navigationsleiste und eine Schaltfläche "zurück" übergibt, damit Sie wieder durch den Stapel wechseln können.

## <a name="iterating-through-the-view-hierarchy"></a>Durchlaufen der Ansichts Hierarchie

Es ist möglich, die unter Ansichts Hierarchie zu durchlaufen und eine bestimmte Ansicht auszuwählen. Wenn Sie z. b. `UIButton` die Schaltfläche Suchen und diese Schaltfläche einen anderen geben möchten `BackgroundColor` , können Sie den folgenden Code Ausschnitt verwenden:

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

Dies funktioniert jedoch nicht, wenn die Sicht, die durchlaufen wird, eine ist, da `UIView` alle Sichten als ein-Objekt zurückgegeben werden, `UIView` da die Objekte, die der übergeordneten Ansicht hinzugefügt werden, selbst erben `UIView` .

## <a name="handling-rotation"></a>Behandeln der Drehung

Wenn der Benutzer das Gerät in das Querformat dreht, ändern sich die Größe der Steuerelemente nicht entsprechend, wie im folgenden Screenshot veranschaulicht:

[![Wenn der Benutzer das Gerät in das Querformat dreht, ändern sich die Größe der Steuerelemente nicht entsprechend.](ios-code-only-images/image7.png)](ios-code-only-images/image7.png#lightbox)

Eine Möglichkeit, dieses Problem zu beheben, besteht darin, die- `AutoresizingMask` Eigenschaft für jede Ansicht festzulegen. In diesem Fall möchten wir, dass die Steuerelemente horizontal gestreckt werden. Daher legen wir die Steuerelemente fest `AutoresizingMask` . Das folgende Beispiel ist für `usernameField` , aber das gleiche muss auf jede Mini Anwendung in der Ansichts Hierarchie angewendet werden.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Wenn wir nun das Gerät oder den Simulator Drehen, wird alles gestreckt, um den zusätzlichen Platz auszufüllen, wie unten dargestellt:

[![Alle Steuerelemente werden gestreckt, um den zusätzlichen Platz auszufüllen.](ios-code-only-images/image8.png)](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Erstellen von benutzerdefinierten Ansichten

Zusätzlich zur Verwendung von Steuerelementen, die Teil von UIKit sind, können auch benutzerdefinierte Ansichten verwendet werden. Eine benutzerdefinierte Ansicht kann erstellt werden, indem von geerbt und überschrieben wird `UIView` `Draw` . Erstellen Sie eine benutzerdefinierte Ansicht, und fügen Sie Sie der Ansichts Hierarchie hinzu, um dies zu veranschaulichen.

### <a name="inheriting-from-uiview"></a>Erben von UIView

Als erstes müssen wir eine Klasse für die benutzerdefinierte Ansicht erstellen. Hierfür wird die **Klassen** Vorlage in Visual Studio verwendet, um eine leere Klasse mit dem Namen hinzuzufügen `CircleView` . Die Basisklasse sollte auf festgelegt werden `UIView` , was wir uns im- `UIKit` Namespace merken. Wir benötigen auch den `System.Drawing` Namespace. Die anderen verschiedenen `System.*` Namespaces werden in diesem Beispiel nicht verwendet. Sie können Sie also entfernen.

Die Klasse sollte wie folgt aussehen:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Zeichnen in einer UIView

Jede `UIView` verfügt über eine `Draw` Methode, die vom System aufgerufen wird, wenn Sie gezeichnet werden muss. `Draw`sollte nie direkt aufgerufen werden. Sie wird vom System während der Verarbeitung der Lauf Schleifen Verarbeitung aufgerufen. Beim ersten Mal durch die Lauf Schleife, nachdem der Ansichts Hierarchie eine Ansicht hinzugefügt wurde, wird die zugehörige- `Draw` Methode aufgerufen. Nachfolgende Aufrufe `Draw` von treten auf, wenn die Ansicht durch das Aufrufen von `SetNeedsDisplay` oder für die Sicht als gezeichnet markiert ist `SetNeedsDisplayInRect` .

Wir können unserer Ansicht Zeichnungs Code hinzufügen, indem wir diesen Code in der überschriebenen Methode hinzufügen, `Draw` wie unten gezeigt:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Da `CircleView` ein ist `UIView` , können wir auch `UIView` Eigenschaften festlegen. Beispielsweise können wir `BackgroundColor` im Konstruktor festlegen:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Um das `CircleView` soeben erstellte zu verwenden, können wir es entweder als unter Ansicht zur Ansichts Hierarchie in einem vorhandenen Controller hinzufügen, wie dies bei und früher der Fall war `UILabels` `UIButton` , oder wir können es als Ansicht eines neuen Controllers laden. Führen wir nun die zweite Aufgabe aus.

### <a name="loading-a-view"></a>Laden einer Ansicht

 `UIViewController`verfügt über eine Methode `LoadView` mit dem Namen, die vom Controller aufgerufen wird, um die Ansicht zu erstellen. Dies ist ein geeigneter Ort, um eine Ansicht zu erstellen und Sie der-Eigenschaft des Controllers zuzuweisen `View` .

Zuerst benötigen wir einen Controller. Erstellen Sie daher eine neue leere Klasse mit dem Namen `CircleController` .

`CircleController`Fügen Sie in folgenden Code hinzu, um `View` auf festzulegen `CircleView` (Sie sollten die-Implementierung nicht in der Überschreibung aufzurufen `base` ):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Schließlich müssen wir den Controller zur Laufzeit präsentieren. Fügen Sie hierzu wie folgt einen Ereignishandler auf der Schaltfläche "Senden" hinzu, den Sie zuvor hinzugefügt haben:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Wenn Sie nun die Anwendung ausführen und auf die Schaltfläche "Senden" tippen, wird die neue Ansicht mit einem Kreis angezeigt:

[![Die neue Ansicht mit einem Kreis wird angezeigt.](ios-code-only-images/circles.png)](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Erstellen eines Startbildschirms

Ein [Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md) wird angezeigt, wenn Ihre APP gestartet wird, um Ihren Benutzern zu zeigen, dass Sie reaktionsfähig ist. Da beim Laden der APP ein Startbildschirm angezeigt wird, kann er nicht im Code erstellt werden, da die Anwendung noch in den Arbeitsspeicher geladen wird.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Wenn Sie ein IOS-Projekt in Visual Studio erstellen, wird ein Startbildschirm in Form einer XIb-Datei bereitgestellt, die sich im **Ressourcen** Ordner Ihres Projekts befindet.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie ein IOS-Projekt in Visual Studio für Mac erstellen, wird ein Startbildschirm in Form einer storyboarddatei bereitgestellt.

-----

Dies kann bearbeitet werden, indem Sie darauf doppelklicken und im IOS-Designer öffnen.

Apple empfiehlt, eine XIb-oder storyboarddatei für Anwendungen zu verwenden, die auf IOS 8 oder höher abzielen. Wenn Sie eine der beiden Dateien im IOS-Designer starten, verwenden Sie Größenklassen und das automatische Layout, um das Layout so anzupassen, dass es gut aussieht und für alle Gerätegrößen ordnungsgemäß angezeigt wird. Ein statisches Start Bild kann zusätzlich zu einem. XIb oder Storyboard verwendet werden, um die Unterstützung für Anwendungen zu ermöglichen, die auf frühere Versionen abzielen.

Weitere Informationen zum Erstellen eines Startbildschirms finden Sie in den folgenden Dokumenten:

- [Erstellen eines Startbildschirms mithilfe von ". XIb"](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [Verwalten von Start Bildschirmen mit Storyboards](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Ab IOS 9 empfiehlt Apple, Storyboards als primäre Methode zum Erstellen eines Startbildschirms zu verwenden.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Erstellen eines Start Abbilds für Anwendungen vor IOS 8

Ein statisches Bild kann zusätzlich zu einem. XIb-oder Storyboard-Startbildschirm verwendet werden, wenn Ihre Anwendung auf Versionen vor IOS 8 abzielt.

Dieses statische Image kann in der Info. plist-Datei oder als Asset-Katalog (für IOS 7) in der Anwendung festgelegt werden. Sie müssen separate Images für jede Gerätegröße (320x480, 640 x 960, 640 x 1136) bereitstellen, auf der Ihre Anwendung möglicherweise ausgeführt wird. Weitere Informationen zu den Startbildschirm Größen finden Sie im Handbuch zum [Starten von Bildschirmen](~/ios/app-fundamentals/images-icons/launch-screens.md) .

> [!IMPORTANT]
> Wenn Ihre APP über keinen Startbildschirm verfügt, werden Sie möglicherweise bemerken, dass Sie nicht vollständig in den Bildschirm passt. Wenn dies der Fall ist, sollten Sie sicherstellen, dass Sie mindestens ein 640 x1136-Image mit dem Namen in die Datei " `Default-568@2x.png` Info. plist" einschließen.

## <a name="summary"></a>Zusammenfassung

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

In diesem Artikel wurde erläutert, wie Sie IOS-Anwendungen Programm gesteuert in Visual Studio entwickeln. Wir haben uns mit dem Erstellen eines Projekts aus einer leeren Projektvorlage beschäftigt, in dem erläutert wird, wie ein root View Controller erstellt und dem Fenster hinzugefügt wird. Anschließend haben wir gezeigt, wie Sie mithilfe von Steuerelementen von UIKit eine Ansichts Hierarchie innerhalb eines Controllers erstellen, um einen Anwendungsbildschirm zu entwickeln. Als nächstes haben wir untersucht, wie die Ansichten entsprechend in verschiedenen Ausrichtungen angeordnet werden, und wir haben gesehen, wie Sie eine benutzerdefinierte Ansicht durch Unterklassen erstellen und wie `UIView` Sie die Sicht innerhalb eines Controllers laden können. Schließlich haben wir untersucht, wie Sie einer Anwendung einen Startbildschirm hinzufügen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

In diesem Artikel wurde erläutert, wie Sie IOS-Anwendungen in Visual Studio für Mac Programm gesteuert entwickeln. Wir haben uns mit dem Erstellen eines Projekts aus einer einzelnen Ansichts Vorlage beschäftigt, in dem erläutert wird, wie ein root View Controller erstellt und dem Fenster hinzugefügt wird. Anschließend haben wir gezeigt, wie Sie mithilfe von Steuerelementen von UIKit eine Ansichts Hierarchie innerhalb eines Controllers erstellen, um einen Anwendungsbildschirm zu entwickeln. Als nächstes haben wir untersucht, wie die Ansichten entsprechend in verschiedenen Ausrichtungen angeordnet werden, und wir haben gesehen, wie Sie eine benutzerdefinierte Ansicht durch Unterklassen erstellen und wie `UIView` Sie die Sicht innerhalb eines Controllers laden können. Schließlich haben wir untersucht, wie Sie einer Anwendung einen Startbildschirm hinzufügen.

-----

## <a name="related-links"></a>Verwandte Links

- [Simplelogin (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplelogin)
