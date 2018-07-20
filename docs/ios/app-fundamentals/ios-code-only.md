---
title: Erstellen von iOS-Benutzeroberflächen im Code in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie Code verwenden, um eine Benutzeroberfläche für eine Xamarin.iOS-app zu erstellen. Es wird erläutert, View-Controller, erstellen eine Hierarchie von Inhaltsansichten, behandeln eine Drehung und vieles mehr.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 5e9bf9555d10c8b34ad9323529d4af5ea66110f8
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156781"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Erstellen von iOS-Benutzeroberflächen im Code in Xamarin.iOS

Die Benutzeroberfläche einer iOS-app ist, wie eine Storefront – die Anwendung erhält in der Regel ein Fenster, aber es kann Auffüllen mit wie vielen Objekten wie benötigt, und die Objekte und Anordnungen können abhängig von der app geändert werden anzeigen möchte. Die Objekte in diesem Szenario – die Dinge, die der Benutzer sieht – werden als Ansichten bezeichnet. Zum Erstellen eines einzigen Bildschirms in einer Anwendung Ansichten sind aufeinander gestapelt in einer Hierarchie von Inhaltsansichten, und die Hierarchie wird von einem einzelnen Ansichtscontroller verwaltet. Anwendungen mit mehreren Bildschirmen weisen mehrere Hierarchien von Inhaltsansichten auf. Jede davon verfügt über ihren eigenen Ansichtscontroller, und die Anwendung platziert Ansichten in das Fenster, um je nach dem vom Benutzer verwendeten Bildschirm eine andere Hierarchie von Inhaltsansichten zu erstellen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller, die die Benutzeroberfläche auf den Gerätebildschirm bringen: 

[![](ios-code-only-images/image9.png "Dieses Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, Ansichten, Unteransichten und View Controller")](ios-code-only-images/image9.png#lightbox)

Diese Hierarchien von Inhaltsansichten können erstellt werden, mithilfe der [Xamarin-Designer für iOS](~/ios/user-interface/designer/index.md) in Visual Studio, jedoch ist es gut geeignet, um ein grundlegendes Verständnis der vollständig in Code zu arbeiten. Dieser Artikel erläutert einige wichtige Punkte, einzurichten und mit der reinen Entwicklung der Benutzeroberfläche ab.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller, die die Benutzeroberfläche auf den Gerätebildschirm bringen: 

[![](ios-code-only-images/image9.png "Dieses Diagramm veranschaulicht die Beziehungen zwischen dem Fenster, Ansichten, Unteransichten und View Controller")](ios-code-only-images/image9.png#lightbox)

Diese Hierarchien von Inhaltsansichten können erstellt werden, mithilfe der [Xamarin-Designer für iOS](~/ios/user-interface/designer/index.md) in Visual Studio für Mac, jedoch ist es gut geeignet, um ein grundlegendes Verständnis der vollständig in Code zu arbeiten. Dieser Artikel erläutert einige wichtige Punkte, einzurichten und mit der reinen Entwicklung der Benutzeroberfläche ab.

-----

## <a name="creating-a-code-only-project"></a>Erstellen eines Projekts nur mit Code

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS leere Projektvorlage

Erstellen Sie zunächst ein iOS-Projekt in Visual Studio mithilfe der **Datei > Neues Projekt > Visual c# > iPhone & iPad > iOS-App (Xamarin)** Projekt unten:

[![Dialogfeld "Neues Projekt"](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Wählen Sie dann die **leere App** Projektvorlage aus:

[![Wählen Sie ein Dialogfeld für die Vorlage](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Die leere Projektvorlage fügt 4 Dateien dem Projekt hinzu:

[![Projektdateien](ios-code-only-images/empty-project.w157-sml.png "Projektdateien")](ios-code-only-images/empty-project.w157.png#lightbox)


1. **Datei "appdelegate.cs"** -enthält eine `UIApplicationDelegate` -Unterklasse, die `AppDelegate` , dient zur Verarbeitung von Anwendungsereignissen iOS-. Das Anwendungsfenster wird erstellt, der `AppDelegate`des `FinishedLaunching` Methode.
1. **Main.cs** -enthält den Einstiegspunkt für die Anwendung, die die Klasse gibt an, für die `AppDelegate` .
1. **Datei "Info.plist"** -eigenschaftenlistendatei an, die Anwendungskonfigurationsinformationen enthält.
1. **"Entitlements.plist"** – eigenschaftenlistendatei an, das Informationen zu den Funktionen und die Berechtigungen der Anwendung enthält.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS-Vorlagen


Visual Studio für Mac bietet keine leere Vorlage. Alle Vorlagen verfügen über Storyboard-Unterstützung, die empfiehlt Apple als vorrangigen Weg zum Erstellen einer Benutzeroberflächenautomatisierungs. Allerdings ist es möglich, die die Benutzeroberfläche vollständig im Code erstellen. 

Die folgenden Schritte führen Sie durch das Storyboard aus einer Anwendung zu entfernen: 


1. Verwenden Sie die Einzelansicht-App-Vorlage, um ein neues iOS-Projekt zu erstellen:
    
    [![](ios-code-only-images/single-view-app.png "Verwenden Sie die Einzelansicht-App-Vorlage")](ios-code-only-images/single-view-app.png#lightbox)

1. Löschen der `Main.Storyboard` und `ViewController.cs` Dateien. Führen Sie **nicht** Löschen der `LaunchScreen.Storyboard`. Die View-Controller sollten gelöscht werden, da es sich um den Code für den ansichtscontroller ist, die im Storyboard erstellt wird:
1. Stellen Sie sicher, dass **löschen** über das Popup-Dialogfeld:
    
    [![](ios-code-only-images/delete.png "Wählen Sie im Popupdialog löschen")](ios-code-only-images/delete.png#lightbox)

1. Löschen Sie in der Datei "Info.plist", die Informationen innerhalb der **Bereitstellungsinformationen > Hauptschnittstelle** Option:
    
    [![](ios-code-only-images/main-interface.png "Löschen Sie die Informationen in die Main-Interface-option")](ios-code-only-images/main-interface.png#lightbox)

1. Fügen Sie abschließend den folgenden Code hinzu. Ihre `FinishedLaunching` -Methode in der die AppDelegate-Klasse:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

Der Code, der hinzugefügt wurde die `FinishedLaunching` -Methode im Schritt 5 oben, beträgt die Mindestmenge des Codes erforderlich, um ein Fenster für die iOS-Anwendung zu erstellen.


-----



iOS-Anwendungen werden erstellt, mit der [MVC-Muster](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Der erste Bildschirm, in dem eine Anwendung angezeigt wird anhand des Fensters stammansichtscontroller erstellt. Finden Sie unter den [Hallo, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) führen Sie für Weitere Informationen zu den MVC-selbst Muster.

Die Implementierung für die `AppDelegate` hinzugefügt, indem die Vorlage erstellt das Anwendungsfenster, von dem es ist nur ein für alle iOS-Anwendung, und macht es sichtbar ist, durch den folgenden Code:

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

Wenn Sie diese Anwendung jetzt ausführen würden, würden Sie wahrscheinlich eine Ausnahme ausgelöst wird, besagt, dass erhalten `Application windows are expected to have a root view controller at the end of application launch`. Wir fügen einen Controller hinzu, und machen Sie es der app Root View Controller.

## <a name="adding-a-controller"></a>Hinzufügen eines Controllers

Ihre app kann viele View-Controller enthalten ist, muss einem Root View Controller alle View-Controller zu steuern.  Hinzufügen eines Controllers in das Fenster durch das Erstellen einer `UIViewController` -Instanz und zum Festlegen der `window.RootViewController` Eigenschaft:

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

Jeder Controller verfügt über eine zugeordnete Ansicht aus zugegriffen werden kann die `View` Eigenschaft. Der obige Code ändert die Anzeige des `BackgroundColor` Eigenschaft `UIColor.LightGray` , damit es sichtbar ist, werden wie unten dargestellt:

 [![](ios-code-only-images/image1.png "Der Ansicht im Hintergrund ist hellgrau sichtbar")](ios-code-only-images/image1.png#lightbox)

Wir konnten alle festlegen `UIViewController` Unterklasse als die `RootViewController` auf diese Weise auch einschließlich Controllern aus UIKit als auch wir selbst schreiben. Beispielsweise der folgende Code Fügt eine `UINavigationController` als die `RootViewController`:

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

Hierdurch wird den Controller in der navigationscontroller geschachtelt werden, wie unten dargestellt:

 [![](ios-code-only-images/image2.png "Der Controller in der navigationscontroller geschachtelt")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Erstellen eines Ansichtscontrollers

Nun, wir, dass wie zum Hinzufügen eines Controllers als gesehen haben die `RootViewController` des Fensters, untersucht, wie Sie einen benutzerdefinierten ansichtencontroller in Code zu erstellen.

Fügen Sie eine neue Klasse mit dem Namen `CustomViewController` wie unten dargestellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Fügen Sie eine neue Klasse namens CustomViewController hinzu.")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Fügen Sie eine neue Klasse namens CustomViewController hinzu.")](ios-code-only-images/new-file.png#lightbox)

-----

Die Klasse erbt, von `UIViewController`, dieser befindet sich in der `UIKit` -Namespace, wie gezeigt:

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

## <a name="initializing-the-view"></a>Initialisieren der Ansicht

`UIViewController` enthält eine Methode namens `ViewDidLoad` , wird aufgerufen, wenn der ansichtscontroller zuerst in den Arbeitsspeicher geladen wird. Dies ist ein geeigneter Platz für die Initialisierung der Ansicht, z. B. das Festlegen ihrer Eigenschaften.

Der folgende Code fügt beispielsweise eine Schaltfläche und einen Ereignishandler an einen neuen View-Controller auf den Navigationsstapel übertragen, wenn die Schaltfläche gedrückt wird:

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

Laden diesen Controller in Ihrer Anwendung und die einfache Navigation zu veranschaulichen, erstellen Sie eine neue Instanz der `CustomViewController`. Erstellen Sie einen neuen navigationscontroller, übergeben Sie in der Ansicht-Controller-Instanz aus, und legen Sie die neue navigationscontroller auf des Fensters `RootViewController` in die `AppDelegate` wie zuvor:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Wenn die Anwendung lädt nun, die `CustomViewController` in einen navigationscontroller geladen wird:

 [![](ios-code-only-images/customvc.png "Die CustomViewController wird in einen navigationscontroller geladen.")](ios-code-only-images/customvc.png#lightbox)
 
Durch Klicken auf die Schaltfläche wird _Push_ auf den Navigationsstapel eine neue View-Controller:

[![](ios-code-only-images/customvca.png "Eine neue View Controller mithilfe von Push auf den Navigationsstapel übertragen")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Erstellen die Hierarchie von Inhaltsansichten

Im obigen Beispiel zu Beginn haben wir eine Benutzeroberfläche in Code zu erstellen, indem Sie eine Schaltfläche mit dem Ansicht-Controller hinzufügen.

iOS-Benutzeroberflächen bestehen aus einer Hierarchie von Inhaltsansichten. Zusätzliche Ansichten, z. B. Bezeichnungen, Schaltflächen, Schieberegler usw. werden als Unteransichten eine übergeordnete Ansicht hinzugefügt.

Beispielsweise ermöglicht das Bearbeiten der `CustomViewController` um einen Anmeldebildschirm zu erstellen, in dem der Benutzer einen Benutzernamen und ein Kennwort eingeben kann. Der Bildschirm besteht aus zwei Textfelder und eine Schaltfläche.

### <a name="adding-the-text-fields"></a>Fügen Sie die Textfelder

Entfernen Sie zunächst die Schaltfläche und ein Ereignishandler, die in hinzugefügt wurde die [Initialisieren der Ansicht](#Initializing_the_View) Abschnitt. 

Fügen Sie ein Steuerelement für den Benutzernamen durch Erstellen und initialisieren eine `UITextField` , und klicken Sie dann auf die Hierarchie von Inhaltsansichten hinzugefügt, wie unten dargestellt:

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

Wenn wir erstellen den `UITextField`, legen wir die `Frame` Eigenschaft, um seine Position und Größe zu definieren. In iOS die Koordinate 0,0 ist, in der oberen linken Ecke mit + x auf der rechten Seite und + y nach unten. Nach dem Festlegen der `Frame` sowie einigen andere Eigenschaften rufen wir `View.AddSubview` Hinzufügen der `UITextField` in der Hierarchie anzeigen. Dadurch wird die `usernameField` Unteransicht der `UIView` Instanz, die `View` Eigenschaftenverweise. Eine Unteransicht wird mit einer Z-Reihenfolge hinzugefügt, die höher als die übergeordnete Ansicht, sodass es vor der übergeordneten Ansicht auf dem Bildschirm angezeigt wird.

Die Anwendung mit der `UITextField` enthalten ist unten dargestellt:

 [![](ios-code-only-images/image4.png "Die Anwendung mit der UITextField enthalten")](ios-code-only-images/image4.png#lightbox)

Wir können hinzufügen eine `UITextField` für das Kennwort auf ähnliche Weise, dieses Mal legen wir die `SecureTextEntry` Eigenschaft auf "true" fest, wie unten dargestellt,:

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

Festlegen von `SecureTextEntry = true` Blendet Sie aus den in eingegebenen Text die `UITextField` vom Benutzer wie folgt:

 [![](ios-code-only-images/image4a.png "SecureTextEntry Einstellung blendet \"true\" den vom Benutzer eingegebenen text")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Die Schaltfläche hinzufügen

Als Nächstes fügen wir eine Schaltfläche, damit der Benutzer Benutzername und Kennwort übermitteln kann. Die Schaltfläche wird auf die Hierarchie von Inhaltsansichten wie jedes andere Steuerelement hinzugefügt, durch Übergabe als Argument an der übergeordneten Ansicht `AddSubview` -Methode erneut auf.

Der folgende Code fügt die Schaltfläche hinzu, und registriert einen Ereignishandler für die `TouchUpInside` Ereignis:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Dies geschehen wird nun der Anmeldebildschirm angezeigt, wie unten dargestellt:

 [![](ios-code-only-images/image5.png "Anmeldebildschirm")](ios-code-only-images/image5.png#lightbox)

Anders als in früheren Versionen von iOS, ist der Schaltflächenhintergrund transparent. Ändern der Schaltfläche " `BackgroundColor` Eigenschaft ändert dies:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Dies führt zu einer quadratischen Schaltfläche anstelle der normalen mit Kante Schaltfläche gerundet. Um abgerundeten Ecke zu erhalten, verwenden Sie den folgenden Codeausschnitt ein:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Mit diesen Änderungen wird die Ansicht wie folgt aussehen:

[![](ios-code-only-images/image6.png "Führen Sie ein Beispiel der Ansicht")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Hinzufügen mehrerer Ansichten zu der Hierarchie von Inhaltsansichten

iOS bietet die Möglichkeit, mehrere Ansichten mit der Hierarchie von Inhaltsansichten hinzufügen `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Hinzufügen von Schaltflächenfunktionen

Wenn eine Schaltfläche geklickt wird, werden Ihre Benutzer etwas passiert erwarten. Klicken Sie z. B. eine Warnung wird angezeigt, oder Navigation zu einem anderen Bildschirm ausgeführt wird. 

Fügen Sie Code, um einen zweiten ansichtscontroller auf den Navigationsstapel übertragen.

Erstellen Sie zunächst den zweiten ansichtscontroller an:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Fügen Sie dann auf die Funktionalität für die `TouchUpInside` Ereignis:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Die Navigation wird im folgenden dargestellt:

[![](ios-code-only-images/navigation.png "In diesem Diagramm wird die Navigation veranschaulicht.")](ios-code-only-images/navigation.png#lightbox)

Beachten Sie, dass standardmäßig bei der Verwendung von einen Navigationscontroller iOS der Anwendung eine Navigationsleiste und eine Schaltfläche "zurück", damit Sie durch den Stapel zurückkehren können gibt.

## <a name="iterating-through-the-view-hierarchy"></a>Durchlaufen die Hierarchie von Inhaltsansichten

Es ist möglich, die Hierarchie Unteransicht durchlaufen und jeden Ansicht auszuwählen. Beispielsweise, um die einzelnen finden `UIButton` und geben Sie eine andere Schaltfläche `BackgroundColor`, kann der folgende Codeausschnitt verwendet werden

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

Dies jedoch nicht funktionieren, wenn die Ansicht für in Bearbeitung ist ein `UIView` wie alle Ansichten als zurückkehren, werden eine `UIView` wie die Objekte, die in der übergeordneten Ansicht hinzugefügt, die sich selbst erben `UIView`.

## <a name="handling-rotation"></a>Verarbeiten der Drehung

Wenn der Benutzer das Gerät zum Querformat wechselt, werden die Steuerelemente nicht entsprechend, ändern, wie im folgende Screenshot veranschaulicht:

 [![](ios-code-only-images/image7.png "Wenn der Benutzer das Gerät zum Querformat wechselt, werden die Steuerelemente nicht entsprechend ändern")](ios-code-only-images/image7.png#lightbox)

Eine Möglichkeit zur Behebung dieses Problems wird durch Festlegen der `AutoresizingMask` Eigenschaft zu den einzelnen Ansichten. In diesem Fall sollten die Steuerelemente horizontal gestreckt wird, damit wir jede festlegen würde `AutoresizingMask`. Im folgende Beispiel wird für `usernameField`, jedoch identisch, die auf jede minianwendung in der Hierarchie von Inhaltsansichten angewendet werden müssen.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Nun, wenn wir auf dem Gerät oder Simulator drehen, wird angepasst alles, was den zusätzlichen Speicherplatz, wie unten dargestellt:

 [![](ios-code-only-images/image8.png "Alle Steuerelemente gestreckt wird, um den zusätzlichen Platz ausfüllen")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Erstellen benutzerdefinierter Ansichten

Zusätzlich zur Verwendung von Steuerelementen, die Teil von UIKit sind, können auch benutzerdefinierte Ansichten verwendet werden. Eine benutzerdefinierte Ansicht erstellt werden kann, durch Erben von `UIView` und überschreiben `Draw`. Wir eine benutzerdefinierte Ansicht erstellen, und fügen Sie es in der Hierarchie anzeigen, um zu veranschaulichen.

### <a name="inheriting-from-uiview"></a>UIView erben

Zunächst müssen wir ist eine Klasse für die benutzerdefinierte Ansicht erstellen. Machen wir dies mit der **Klasse** Vorlage in Visual Studio zum Hinzufügen einer leeren Klasse mit dem Namen `CircleView`. Die Basisklasse sollte festgelegt werden, um `UIView`, das wir denken Sie daran den `UIKit` Namespace. Wir müssen auch die `System.Drawing` auch Namespace. Die anderen verschiedenen `System.*` Namespaces nicht in diesem Beispiel verwendet, daher können Sie zur Beseitigung.

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

Jede `UIView` verfügt über eine `Draw` -Methode, die vom System aufgerufen wird, wenn es gezeichnet werden muss. `Draw` sollte nicht direkt aufgerufen werden. Es wird vom System aufgerufen, während der Verarbeitung führen. Beim ersten Durchlaufen der ausführen-Schleife, nachdem eine Ansicht der Hierarchie von Inhaltsansichten hinzugefügt wird seine `Draw` Methode wird aufgerufen. Nachfolgende Aufrufe von `Draw` auftreten, wenn die Ansicht markiert ist, müssen Sie gezeichnet werden soll, durch Aufrufen von entweder `SetNeedsDisplay` oder `SetNeedsDisplayInRect` für die Sicht.

Wir können unsere Ansicht Zeichencode hinzugefügt, durch das Hinzufügen solcher Code in der überschriebenen `Draw` Methode wie folgt:

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

Da `CircleView` ist eine `UIView`, wir können auch festlegen, `UIView` auch Eigenschaften. Wir können z. B. Festlegen der `BackgroundColor` im Konstruktor:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Verwenden der `CircleView` wir gerade erstellt haben, wir können entweder als Hinzufügen einer Unteransicht die Hierarchie von Inhaltsansichten in einem vorhandenen Controller, wie mit der `UILabels` und `UIButton` weiter oben, oder sie als Ansicht eines neuen Controllers. Lassen Sie die zweite.

### <a name="loading-a-view"></a>Laden eine Sicht

 `UIViewController` verfügt über eine Methode mit dem Namen `LoadView` , die vom Controller zum Erstellen der Ansicht aufgerufen wird. Dies ist ein geeigneter Platz zum Erstellen einer Ansicht und Zuweisen des Controllers `View` Eigenschaft.

Wir zunächst einen Controller, daher erstellen Sie eine neue leere Klasse, die mit dem Namen `CircleController`.

In `CircleController` fügen Sie den folgenden Code hinzu, legen Sie die `View` auf eine `CircleView` (sollte nicht aufgerufen werden die `base` Implementierung in der Überschreibung):

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

Abschließend müssen wir den Controller zur Laufzeit vorhanden. Wir dazu ein Ereignishandler hinzugefügt, auf die Schaltfläche "Senden", die wir hinzugefügt, zuvor, wie folgt haben:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Wenn wir die Anwendung auszuführen, und tippen Sie auf die Schaltfläche "Senden", wird nun die neue Ansicht mit einem Kreis angezeigt:

 [![](ios-code-only-images/circles.png "Die neue Ansicht mit einem Kreis wird angezeigt.")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Erstellen eines Startbildschirms

Ein [Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md) wird angezeigt, wenn Ihre app gestartet wird, als eine Möglichkeit, Ihren Benutzern anzuzeigen, dass es reagiert. Da einen Startbildschirm angezeigt wird, wenn Ihre app geladen wird, kann es im Code erstellt werden, wie die Anwendung weiterhin in den Arbeitsspeicher geladen wird. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Ihre Erstellen einer iOS-Projekt in Visual Studio einen Startbildschirm dient in Form einer XIB-Datei, die sich in befinden die **Ressourcen** Ordner in Ihrem Projekt. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Ihre erstellen Sie ein iOS-Projekt in Visual Studio für Mac, einen Startbildschirm für die Sie in der Form einer Storyboard-Datei bereitgestellt wird. 

-----

Dies kann durch doppelte darauf klicken, und öffnen es in der iOS-Designer bearbeitet werden.

Apple empfiehlt, dass eine XIB oder Storyboard-Datei wird verwendet, für Anwendungen, die für iOS 8 oder später, wenn Sie entweder-Datei in der iOS-Designer starten, Sie verwenden Größenklassen und automatisches Layout, das Layout anzupassen, damit sie Ihren vorstellungen entspricht, und zeigt ordnungsgemäß für alle Geräte Größen. Zusätzlich zu einer XIB oder Storyboard kann eine statische startbilds, das verwendet werden, um Unterstützung für Anwendungen, die für frühere Versionen zu ermöglichen.

Weitere Informationen zum Erstellen eines Bildschirms zu starten finden Sie in den folgenden Dokumenten:

- [Erstellen einen Startbildschirm mithilfe einer XIB](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Verwalten von Startbildschirme mit Storyboards](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Ab iOS 9 empfiehlt Apple Storyboards als primäre Methode zum Erstellen eines Bildschirms starten verwendet werden soll.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Erstellen ein Image starten für vor iOS 8-Anwendungen

Zusätzlich zu einer XIB oder Storyboard-Startbildschirm kann ein statisches Bild verwendet werden, wenn Ihre Anwendung mit Versionen vor iOS 8 ausgerichtet ist. 

Diese statische Bilder kann in der Datei "Info.plist" oder als ein Asset-Katalog (für iOS 7) in Ihrer Anwendung festgelegt werden. Sie benötigen separate Abbilder für jeden GeräteGröße (320 x 480, 640 960 x 640 x 1136) bereitstellen, die Ihre Anwendung ausgeführt werden kann. Weitere Informationen zu den Größen der Startbildschirm Anzeigen der [Bildschirm Startbilder](~/ios/app-fundamentals/images-icons/launch-screens.md) Handbuch.

> [!IMPORTANT]
> Wenn Ihre app keine Startbildschirm aufweist, werden Sie feststellen, dass sie nicht vollständig den Bildschirm passt. Wenn dies der Fall ist, Sie sollten sicherstellen sollen, oder zumindest eine 640 x 1136-Image mit dem Namen `Default-568@2x.png` zu Ihrer Datei "Info.plist". 



## <a name="summary"></a>Zusammenfassung

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Artikel erläutert, wie Sie iOS-Anwendungen programmgesteuert in Visual Studio entwickeln. Wir haben uns angeschaut Gewusst wie: Erstellen eines Projekts aus eine leere Projektvorlage, erörtert, wie Sie erstellen und das Fenster einen Root View Controller hinzufügen. Wir hinaus wurde gezeigt, wie Sie Steuerelemente aus UIKit verwenden, um eine Hierarchie von Inhaltsansichten innerhalb eines Controllers zum Entwickeln von eines Bildschirms für die Anwendung zu erstellen. Als Nächstes wir untersucht, wie die Ansichten zu gestalten entsprechend in verschiedenen Ausrichtungen und erläutert, wie Sie eine benutzerdefinierte Ansicht erstellen, indem Unterklassen `UIView`, und wie die Ansicht in einem Controller zu laden. Schließlich haben wir, wie Sie einen Startbildschirm zu einer Anwendung hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Artikel wurde beschrieben, wie zum Entwickeln von iOS-Anwendungen programmgesteuert in Visual Studio für Mac. Erläutert, wie Sie ein Projekt aus einer einzigen Ansicht Vorlage erstellen erörtert, wie Sie erstellen und das Fenster einen Root View Controller hinzufügen. Wir hinaus wurde gezeigt, wie Sie Steuerelemente aus UIKit verwenden, um eine Hierarchie von Inhaltsansichten innerhalb eines Controllers zum Entwickeln von eines Bildschirms für die Anwendung zu erstellen. Als Nächstes wir untersucht, wie die Ansichten zu gestalten entsprechend in verschiedenen Ausrichtungen und erläutert, wie Sie eine benutzerdefinierte Ansicht erstellen, indem Unterklassen `UIView`, und wie die Ansicht in einem Controller zu laden. Schließlich haben wir, wie Sie einen Startbildschirm zu einer Anwendung hinzufügen.

-----




## <a name="related-links"></a>Verwandte Links

- [SimpleLogin (Beispiel)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
