---
title: Erstellen von iOS-Benutzeroberflächen im Code in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Code verwenden, um eine neue Benutzeroberfläche für eine Xamarin.iOS-app zu erstellen. Es wird erläutert, View-Controller, erstellen eine Hierarchie anzeigen, behandeln eine Drehung und vieles mehr.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 5e8abc2cea2e2ca8abfada8bc85379d93d183768
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784633"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Erstellen von iOS-Benutzeroberflächen im Code in Xamarin.iOS

Die Benutzeroberfläche einer iOS-App ist, wie eine Storefront – die Anwendung ruft in der Regel ein Fenster festlegen, aber es kann Auffüllen des Fensters mit, wie viele Objekte Sie benötigt, und die Objekte und die Modalitäten geändert werden, können je nachdem, was die app anzeigen möchte. Die Objekte in diesem Szenario – die Dinge, die der Benutzer sieht – werden als Ansichten bezeichnet. Zum Erstellen einer einzigen Seite in einer Anwendung Ansichten sind aufeinander gestapelt in einer Hierarchie mit Inhalt anzeigen und durch einen einzelnen Controller für die Ansicht die Hierarchie verwaltet wird. Anwendungen mit mehreren Bildschirmen weisen mehrere Hierarchien von Inhaltsansichten auf. Jede davon verfügt über ihren eigenen Ansichtscontroller, und die Anwendung platziert Ansichten in das Fenster, um je nach dem vom Benutzer verwendeten Bildschirm eine andere Hierarchie von Inhaltsansichten zu erstellen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller, die die Benutzeroberfläche auf den Gerätebildschirm bringen: 

[![](ios-code-only-images/image9.png "Dieses Diagramm veranschaulicht die Beziehungen zwischen den Fenster, Ansichten, Unteransichten und View Controller")](ios-code-only-images/image9.png#lightbox)

Diese Ansicht Hierarchien können erstellt werden, mithilfe der [Xamarin-Designer für iOS](~/ios/user-interface/designer/index.md) in Visual Studio ist es jedoch grundlegende Kenntnisse über die vollständig im Code arbeiten können. Dieser Artikel führt Sie durch einige grundlegende Punkte einrichten und ausgeführt wird, mit der reinen Entwicklung der Benutzeroberfläche.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller, die die Benutzeroberfläche auf den Gerätebildschirm bringen: 

[![](ios-code-only-images/image9.png "Dieses Diagramm veranschaulicht die Beziehungen zwischen den Fenster, Ansichten, Unteransichten und View Controller")](ios-code-only-images/image9.png#lightbox)

Diese Ansicht Hierarchien können erstellt werden, mithilfe der [Xamarin-Designer für iOS](~/ios/user-interface/designer/index.md) in Visual Studio für Mac ist es jedoch grundlegende Kenntnisse über die vollständig im Code arbeiten können. Dieser Artikel führt Sie durch einige grundlegende Punkte einrichten und ausgeführt wird, mit der reinen Entwicklung der Benutzeroberfläche.

-----

## <a name="creating-a-code-only-project"></a>Erstellen eines reinen-Projekts

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS leere Projektvorlage

Erstellen Sie zunächst ein iOS-Projekt in Visual Studio mithilfe der **Datei > Neues Projekt > Visual c# > iPhone & iPad > iOS-App (Xamarin)** Projekt unten angezeigt:

[![Dialogfeld "Neues Projekt"](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Wählen Sie dann die **leere App** Projektvorlage:

[![Wählen Sie eine Vorlage](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Die leere Projektvorlage fügt 4 Dateien zum Projekt hinzu:

[![Projektdateien](ios-code-only-images/empty-project.w157-sml.png "Projektdateien")](ios-code-only-images/empty-project.w157.png#lightbox)


1. **AppDelegate.cs** -enthält eine `UIApplicationDelegate` -Unterklasse, die `AppDelegate` , dem wird verwendet, um iOS-Anwendungsereignisse behandeln. Das Anwendungsfenster wird erstellt, der `AppDelegate`des `FinishedLaunching` Methode.
1. **Main.cs** -enthält den Einstiegspunkt für die Anwendung, die die Klasse gibt an, für die `AppDelegate` .
1. **Datei "Info.plist"** -eigenschaftenlistendatei an, die Anwendungskonfigurationsinformationen enthält.
1. **Entitlements.plist** – eigenschaftenlistendatei an, die Informationen zu den Funktionen und die Berechtigungen der Anwendung enthält.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS-Vorlagen


Visual Studio für Mac bietet keine leere Vorlage. Alle Vorlagen stammen mit Storyboard-Unterstützung, die als die primäre Methode zum Erstellen einer Benutzeroberflächenautomatisierungs Apple empfiehlt. Allerdings ist es möglich, die Benutzeroberfläche vollständig im Code zu erstellen. 

Die folgenden Schritte begleiten Sie das Storyboard aus einer Anwendung entfernen: 


1. Verwenden Sie die einzelne Ansicht App-Vorlage so erstellen Sie eine neue iOS-Projekt:
    
    [![](ios-code-only-images/single-view-app.png "Verwenden Sie die einzelnen Ansicht App-Projektvorlage")](ios-code-only-images/single-view-app.png#lightbox)

1. Löschen der `Main.Storyboard` und `ViewController.cs` Dateien. Führen Sie **nicht** Löschen der `LaunchScreen.Storyboard`. Die View-Controller sollte gelöscht werden, da es der Code-behind für den Controller Sicht handelt, in das Storyboard erstellt wird:
1. Stellen Sie sicher, dass **löschen** über das Popup-Dialogfeld:
    
    [![](ios-code-only-images/delete.png "Wählen Sie im Popup-Dialogfeld Löschen")](ios-code-only-images/delete.png#lightbox)

1. Löschen Sie in der Datei "Info.plist", die Informationen in den **Bereitstellungsinformationen > Hauptbenutzeroberfläche** Option:
    
    [![](ios-code-only-images/main-interface.png "Löschen Sie die Informationen in die Main-Interface-option")](ios-code-only-images/main-interface.png#lightbox)

1. Zum Schluss fügen Sie folgenden Code zu Ihrem `FinishedLaunching` Methode in der AppDelegate-Klasse:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

Der Code, der hinzugefügt wurde die `FinishedLaunching` Methode in Schritt 5 oben, beträgt die Mindestmenge des Codes erforderlich, um ein Fenster für die iOS-Anwendung zu erstellen.


-----



Mithilfe von iOS-Anwendungen erstellt die [MVC-Musters](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Der erste Bildschirm, in dem eine Anwendung angezeigt wird anhand des Fensters Stamm modellansichtcontroller erstellt. Finden Sie unter der [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) für Weitere Informationen über das MVC-selbst Muster geführt.

Die Implementierung für die `AppDelegate` hinzugefügt, indem die Vorlage erstellt, im Anwendungsfenster angezeigt, von der es ist nur ein für alle iOS-Anwendung, und macht sie sichtbar, durch den folgenden Code:

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
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

Wenn Sie diese Anwendung nun ausführen würden, würden Sie wahrscheinlich eine Ausnahme ausgelöst wird, besagt, dass erhalten `Application windows are expected to have a root view controller at the end of application launch`. Wir fügen Sie einen Controller hinzu, und stellen sie der app-Stamm-View-Controller.

## <a name="adding-a-controller"></a>Hinzufügen eines Controllers

Ihre app kann viele View-Controller enthalten, aber es muss ein Stamm-View-Controller zu steuern, die View-Controller verfügen.  Hinzufügen ein Controllers in das Fenster durch das Erstellen einer `UIViewController` -Instanz und bei der Einstellung der `window.RootViewController` Eigenschaft:

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

Jeder Controller verfügt über eine zugeordnete Ansicht aus zugänglich ist die `View` Eigenschaft. Der obige Code ändert der Ansicht `BackgroundColor` Eigenschaft `UIColor.LightGray` , damit es sichtbar ist, werden wie folgt:

 [![](ios-code-only-images/image1.png "Die Ansicht im Hintergrund ist eine sichtbare hellgrau")](ios-code-only-images/image1.png#lightbox)

Wir legen konnte `UIViewController` als Unterklasse der `RootViewController` auf diese Weise ebenfalls, einschließlich Domänencontrollern von UIKit als auch wir uns schreiben. Z. B. der folgende Code Fügt eine `UINavigationController` als die `RootViewController`:

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

Hierdurch wird den Controller in den Navigationsbereich Controller geschachtelt werden, wie unten dargestellt:

 [![](ios-code-only-images/image2.png "Der Controller in den Navigationsbereich Controller geschachtelt")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Erstellen eines Domänencontrollers anzeigen

Nun, dass wir zum Hinzufügen von einem Domänencontroller als gesehen haben die `RootViewController` des Fensters, sehen wir, wie Sie einen benutzerdefinierte Ansicht Controller im Code zu erstellen.

Fügen Sie eine neue Klasse mit dem Namen `CustomViewController` wie unten dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Fügen Sie eine neue Klasse mit dem Namen CustomViewController")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Fügen Sie eine neue Klasse mit dem Namen CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

Die Klasse erben soll `UIViewController`, dies ist der `UIKit` Namespace, wie dargestellt:

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

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>Initialisieren die Ansicht

`UIViewController` enthält eine Methode namens `ViewDidLoad` , die aufgerufen wird, wenn die View-Controller zuerst in den Arbeitsspeicher geladen wird. Dies ist eine geeignete Position für die Initialisierung der Sicht, z. B. das dessen Eigenschaften festlegen.

Der folgende Code fügt beispielsweise eine Schaltfläche und ein Ereignishandler push eine neue View-Controller auf den Navigationsstapel beim Klicken auf die Schaltfläche aufgerufen wird:

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

Um diese Domänencontroller in der Anwendung geladen, und zeigen Sie die einfache Navigation, erstellen Sie eine neue Instanz der `CustomViewController`. Erstellen Sie einen neuen Domänencontroller für die Navigation, übergeben Sie in Ihrer Ansicht Controller-Instanz aus, und legen Sie den neuen Controller für die Navigation in des Fensters `RootViewController` in der `AppDelegate` wie zuvor:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Wenn die Anwendung lädt nun, die `CustomViewController` innerhalb eines Controllers Navigation geladen wird:

 [![](ios-code-only-images/customvc.png "Die CustomViewController wird innerhalb eines Controllers Navigation geladen.")](ios-code-only-images/customvc.png#lightbox)
 
Klicken auf die Schaltfläche wird _Push_ eine neue View-Controller auf den Navigationsstapel:

[![](ios-code-only-images/customvca.png "Eine neue View-Controller zu leisten Navigationsstapel")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Erstellen die Hierarchie anzeigen

Im obigen Beispiel gestartet wir eine neue Benutzeroberfläche im Code zu erstellen, indem die View-Controller eine Schaltfläche hinzufügen.

iOS-Benutzeroberflächen bestehen aus einer Hierarchie anzeigen. Zusätzliche Ansichten, z. B. Bezeichnungen, Schaltflächen, Schieberegler usw. werden als Unteransichten einige übergeordnete Ansicht hinzugefügt.

Beispielsweise ermöglicht das Bearbeiten der `CustomViewController` Anmeldebildschirm erstellen, in dem der Benutzer einen Benutzernamen und ein Kennwort eingeben kann. Der Bildschirm besteht aus zwei Textfelder und eine Schaltfläche.

### <a name="adding-the-text-fields"></a>Hinzufügen von Textfeldern

Entfernen Sie zuerst die Schaltfläche und ein Ereignishandler, die in hinzugefügt wurde die [initialisieren die Ansicht](#Initializing_the_View) Abschnitt. 

Fügen Sie ein Steuerelement für den Benutzernamen durch Erstellen und Initialisieren einer `UITextField` , und klicken Sie dann auf die Hierarchie anzeigen, hinzufügen, wie unten dargestellt:

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

Wenn wir erstellen die `UITextField`, legen wir den `Frame` -Eigenschaft seine Position und Größe zu definieren. In iOS die Koordinate 0,0 ist, in der oberen linken Ecke mit + x auf der rechten Seite und + y nach unten. Nach dem Festlegen der `Frame` zusammen mit einigen anderen Eigenschaften nennen wir `View.AddSubview` Hinzufügen der `UITextField` in der Hierarchie anzeigen. Dadurch wird die `usernameField` Unteransicht der `UIView` Instanz, die `View` Eigenschaftenverweise. Eine Unteransicht wird mit einer Z-Reihenfolge hinzugefügt, die höher als die übergeordnete Ansicht ist, damit er vor der übergeordneten Ansicht auf dem Bildschirm angezeigt wird.

Die Anwendung mit der `UITextField` eingeschlossen wird unten gezeigt:

 [![](ios-code-only-images/image4.png "Die Anwendung mit der UITextField enthalten")](ios-code-only-images/image4.png#lightbox)

Wir können hinzufügen eine `UITextField` für das Kennwort in ähnlicher Weise nur diesmal legen wir die `SecureTextEntry` Eigenschaft auf "true", wie unten dargestellt,:

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

Festlegen von `SecureTextEntry = true` Blendet Sie aus den im eingegebenen Text die `UITextField` vom Benutzer wie unten dargestellt:

 [![](ios-code-only-images/image4a.png "\"True\" Blendet den Text, der vom Benutzer eingegebenen SecureTextEntry festlegen")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Die Schaltfläche hinzufügen

Als Nächstes fügen wir eine Schaltfläche, damit der Benutzer Benutzername und Kennwort senden kann. Durch, und übergeben sie als Argument an der übergeordnete Ansicht der Hierarchie anzeigen, wie jedes andere Steuerelement die Schaltfläche hinzugefügt `AddSubview` -Methode erneut.

Der folgende Code fügt die Schaltfläche hinzu und registriert einen Ereignishandler für das `TouchUpInside` Ereignis:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Mit diesem Ort ist wird jetzt der Anmeldebildschirm angezeigt, wie unten dargestellt:

 [![](ios-code-only-images/image5.png "Anmeldebildschirm")](ios-code-only-images/image5.png#lightbox)

Anders als in früheren Versionen von iOS, ist die Schaltfläche Standardhintergrund transparent. Ändern der Schaltfläche `BackgroundColor` eigenschaftenänderungen dies:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Dies führt zu einer quadratische Schaltfläche, anstatt die typische mit Kante Schaltfläche gerundet. Verwenden Sie zum Abrufen der abgerundeten Kante der folgende Codeausschnitt ein:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Mit diesen Änderungen wird die Ansicht sieht wie folgt aus:

[![](ios-code-only-images/image6.png "Führen Sie ein Beispiel der Ansicht")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Hinzufügen mehrerer Ansichten zu der Hierarchie anzeigen

iOS bietet die Möglichkeit, mehrere Ansichten mithilfe der Ansichtshierarchie hinzufügen `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Hinzufügen einer Schaltfläche-Funktion

Wenn eine Schaltfläche geklickt wird, werden Ihre Benutzer etwas geschehen erwarten. Beispielsweise wird eine Warnung angezeigt, oder Navigation zu einem anderen Bildschirm ausgeführt wird. 

Wir fügen Sie Code zum zweiten View-Controller auf dem Navigationsbereich-Stapel abgelegt.

Erstellen Sie zunächst den zweiten View-Controller:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Fügen Sie dann die entsprechende Funktionalität zum die `TouchUpInside` Ereignis:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Nachstehend ist die Navigation dargestellt:

[![](ios-code-only-images/navigation.png "In diesem Diagramm wird die Navigation veranschaulicht.")](ios-code-only-images/navigation.png#lightbox)

Beachten Sie, dass standardmäßig bei der Verwendung eines Controllers Navigation iOS der Anwendung eine Navigationsleiste und eine zurück-Schaltfläche, die durch den Stapel zurück verschieben können gibt.

## <a name="iterating-through-the-view-hierarchy"></a>Durchlaufen die Hierarchie anzeigen

Es ist möglich, den Unteransicht Hierarchie durchlaufen, und wählen Sie eine bestimmte Ansicht. Beispielsweise, um die einzelnen finden `UIButton` , und geben Sie der Schaltfläche eine andere `BackgroundColor`, kann der folgende Ausschnitt verwendet werden

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

Hierzu jedoch nicht funktioniert, wenn die Ansicht wird für durchlaufen ist ein `UIView` wie alle Ansichten als zurückkehren werden eine `UIView` wie die Objekte, die für die übergeordnete Ansicht hinzugefügt, die sich selbst erben `UIView`.

## <a name="handling-rotation"></a>Behandlung von Drehung

Wenn der Benutzer das Gerät auf Querformat dreht, führen Sie die Steuerelemente nicht entsprechend, ändern, wie der folgende Screenshot veranschaulicht:

 [![](ios-code-only-images/image7.png "Wenn der Benutzer das Gerät auf Querformat dreht, führen Sie die Steuerelemente nicht entsprechend ändern")](ios-code-only-images/image7.png#lightbox)

Eine Möglichkeit, dieses Problem zu beheben ist durch Festlegen der `AutoresizingMask` Eigenschaft für jede Sicht. In diesem Fall möchten wir die Steuerelemente horizontal gestreckt, damit wir jede festgelegt wäre `AutoresizingMask`. Im folgende Beispiel wird für `usernameField`, aber die gleiche müsste auf jede Gadget in der Ansichtshierarchie angewendet werden soll.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Jetzt Wenn wir das Gerät oder den Simulator drehen, wird angepasst alles, was den zusätzlichen Speicherplatz, wie unten dargestellt:

 [![](ios-code-only-images/image8.png "Alle Steuerelemente werden gestreckt, um den zusätzlichen Platz ausfüllen")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Erstellen benutzerdefinierter Ansichten

Zusätzlich zur Verwendung von Steuerelementen, die Teil der UIKit sind, können auch benutzerdefinierte Ansichten verwendet werden. Eine benutzerdefinierte Ansicht erstellt werden, indem Sie die Vererbung von `UIView` und überschreiben `Draw`. Wir eine benutzerdefinierte Ansicht erstellen und Hinzufügen der Hierarchie anzeigen, um zu veranschaulichen.

### <a name="inheriting-from-uiview"></a>Erben von UIView

Im ersten Schritt erforderlich ist eine Klasse für die benutzerdefinierte Ansicht erstellen. Wir müssen hierfür das **Klasse** Vorlage in Visual Studio zum Hinzufügen einer leeren Klasse mit dem Namen `CircleView`. Die Basisklasse sollte festgelegt werden, um `UIView`, die wir zurückgerufen werden, der `UIKit` Namespace. Wir müssen auch die `System.Drawing` auch Namespace. Die anderen verschiedenen `System.*` Namespaces nicht in diesem Beispiel verwendet, also gerne entfernen.

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

### <a name="drawing-in-a-uiview"></a>Zeichnen in einem UIView

Jede `UIView` verfügt über eine `Draw` Methode, die vom System aufgerufen wird, wenn diese gezeichnet werden muss. `Draw` sollte nie direkt aufgerufen werden. Er wird während der Ausführung der Schleife Verarbeitung vom System aufgerufen. Die erste Schleifendurchlauf ausführen, nachdem die Hierarchie anzeigen eine Sicht hinzugefügt wird seine `Draw` -Methode aufgerufen wird. Nachfolgende Aufrufe `Draw` auftreten, wenn die Sicht gekennzeichnet ist, gezeichnet werden soll, durch den Aufruf eines Kennzeichnung angegeben, dass `SetNeedsDisplay` oder `SetNeedsDisplayInRect` für die Sicht.

Wir können unsere Ansicht zeichnen Code hinzufügen, durch Hinzufügen von solchen Code innerhalb der überschriebenen `Draw` Methode wie unten dargestellt:

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

Da `CircleView` ist ein `UIView`, wir können auch festlegen, `UIView` sowie Eigenschaften. Es können z. B. Festlegen der `BackgroundColor` im Konstruktor:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Verwenden der `CircleView` soeben erstellt wurde, wir können entweder als Hinzufügen einer Unteransicht der anzeigen-Hierarchie in einem vorhandenen Domänencontroller wie wir mit der `UILabels` und `UIButton` weiter oben, oder wir können wie die Sicht eines neuen Controllers geladen werden. Wir sind der zweite Wert.

### <a name="loading-a-view"></a>Laden eine Sicht

 `UIViewController` verfügt über eine Methode mit dem Namen `LoadView` , die vom Controller zum Erstellen der Ansicht aufgerufen wird. Dies ist eine Ansicht zu erstellen und Zuweisen des Controllers einer geeigneten Stelle `View` Eigenschaft.

Wir benötigen zunächst einen Controller, daher erstellen Sie eine neue leere-Klasse, die mit dem Namen `CircleController`.

In `CircleController` fügen Sie den folgenden Code aus, um die `View` auf eine `CircleView` (rufen Sie nicht die `base` Implementierung der Außerkraftsetzung):

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

Schließlich müssen wir den Controller zur Laufzeit vorhanden. Wir dazu ein Ereignishandler hinzugefügt, auf die Schaltfläche "Absenden", die wir, weiter oben, wie folgt hinzugefügt:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Wenn wir die Anwendung auszuführen, und tippen Sie auf die Schaltfläche "Absenden", wird nun die neue Ansicht durch einen Kreis angezeigt:

 [![](ios-code-only-images/circles.png "Die neue Ansicht durch einen Kreis wird angezeigt.")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Erstellen einen Startbildschirm

Ein [Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md) wird angezeigt, wenn Ihre app gestartet wird als eine Möglichkeit, Ihre Benutzer angezeigt, dass er erreichbar ist. Da einen Startbildschirm angezeigt wird, wenn Ihre app geladen wird, können sie im Code erstellt werden, wie die Anwendung weiterhin in den Arbeitsspeicher geladen wird. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Ihr iOS-Projekt in Visual Studio, eines Bildschirms starten, um Sie in Form einer .xib-Datei, die sich bereitgestellt wird in erstellen die **Ressourcen** Ordner innerhalb des Projekts. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Ihr iOS-Projekt in Visual Studio erstellen, für Mac, eines Bildschirms starten Sie in Form einer Storyboard-Datei bereitgestellt wird. 

-----

Dies kann durch doppelte sie darauf klicken, und öffnen sie in der iOS-Designer bearbeitet werden.

Apple empfiehlt, dass ein .xib oder eine Storyboard-Datei für Anwendungen für iOS 8 verwendet wird, oder später, wenn Sie eine Datei in der iOS-Designer starten, Sie verwenden Größenklassen und automatisches Layout das Layout angepasst werden kann, damit es scheinbar in Ordnung, und zeigt ordnungsgemäß für alle Geräte Größen. Zusätzlich keine .xib oder Storyboard kann eine statische Launch-Image verwendet werden, um Unterstützung für Anwendungen für frühere Versionen zu ermöglichen.

Weitere Informationen zum Erstellen eines Bildschirms starten finden Sie in den folgenden Dokumenten:

- [Erstellen einen Startbildschirm mithilfe einer .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Sie Startbildschirme mit Storyboards verwalten](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Seit iOS 9 empfiehlt Apple Storyboards als primäre Methode zum Erstellen eines Bildschirms starten verwendet werden soll.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Erstellen ein Bild zu starten, für vor iOS 8-Anwendungen

Ein statisches Bild kann zusätzlich zu einer .xib oder Storyboard-Startbildschirm verwendet werden, wenn Sie Anwendung-Versionen vor iOS 8 ausgerichtet ist. 

Diese statische Bilder kann in der Datei "Info.plist" oder als eine Asset-Katalog (für iOS 7) in der Anwendung festgelegt werden. Sie müssen separate Bilder für jede GeräteGröße (320 x 480 beträgt, 640 x 960 640 x 1136) bereitstellen, die Ihre Anwendung ausgeführt werden kann. Weitere Informationen zu starten Bildschirmgrößen, zeigen Sie an der [starten Bilder](~/ios/app-fundamentals/images-icons/launch-screens.md) Handbuch.

> [!IMPORTANT]
> Wenn Ihre app keine starten Bildschirm verfügt, werden Sie möglicherweise feststellen, es vollständig den Bildschirm passt. Wenn dies der Fall ist, stellen Sie sicher, mindestens ein 640 x 1136-Image mit dem Namen einschließen `Default-568@2x.png` auf die Datei "Info.plist". 



## <a name="summary"></a>Zusammenfassung

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Artikel erläutert, wie Sie iOS-Anwendungen programmgesteuert in Visual Studio entwickeln. Erläutert, wie Sie ein Projekt aus eine leere Projektvorlage erstellen diskutieren das Erstellen und fügen einen Stamm View-Controller an das Fenster. Wir dann wurde gezeigt, wie Steuerelemente aus UIKit verwenden, um eine Hierarchie anzeigen innerhalb eines Controllers zum Entwickeln eines Anwendung Bildschirms zu erstellen. Als Nächstes untersucht wie die Ansichten zu gestalten entsprechend in verschiedenen Ausrichtungen und wurde erläutert, wie eine benutzerdefinierte Ansicht erstellen, durch die Erstellung von Unterklassen von `UIView`, und wie die Ansicht innerhalb eines Controllers zu laden. Schließlich untersucht es wie eine Anwendung einen Startbildschirm hinzugefügt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Artikel erläuterten zum Entwickeln von iOS-Anwendungen programmgesteuert in Visual Studio für Mac. Erläutert, zum Erstellen eines Projekts aus einer einzigen Ansicht Vorlage diskutieren das Erstellen und das Fenster einen Stamm View-Controller hinzugefügt. Wir dann wurde gezeigt, wie Steuerelemente aus UIKit verwenden, um eine Hierarchie anzeigen innerhalb eines Controllers zum Entwickeln eines Anwendung Bildschirms zu erstellen. Als Nächstes untersucht wie die Ansichten zu gestalten entsprechend in verschiedenen Ausrichtungen und wurde erläutert, wie eine benutzerdefinierte Ansicht erstellen, durch die Erstellung von Unterklassen von `UIView`, und wie die Ansicht innerhalb eines Controllers zu laden. Schließlich untersucht es wie eine Anwendung einen Startbildschirm hinzugefügt.

-----




## <a name="related-links"></a>Verwandte Links

- [SimpleLogin (Beispiel)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
