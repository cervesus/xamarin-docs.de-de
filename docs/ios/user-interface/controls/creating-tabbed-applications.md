---
title: Registerkarten und Tab-leisten-Controller in xamarin. IOS
description: In diesem Dokument werden die IOS-Registerkarten-Steuerelemente und deren Verwendung mit xamarin. IOS beschrieben. Es zeigt, wie Sie einen uitabbarcontroller einrichten, mit Bildern arbeiten, Badge-Werte festlegen, mit Ereignissen arbeiten und vieles mehr.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 9f8a5e568946e1aea8541211ec3adc45a25f1897
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022137"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Registerkarten und Tab-leisten-Controller in xamarin. IOS

Anwendungen mit Registerkarten werden in ios verwendet, um Benutzeroberflächen zu unterstützen, bei denen auf mehrere Bildschirme in einer bestimmten Reihenfolge zugegriffen werden kann Durch die `UITabBarController`-Klasse können Anwendungen problemlos Unterstützung für solche Szenarien mit mehreren Bildschirmen enthalten. `UITabBarController` kümmert sich um die Verwaltung mit mehreren Bildschirmen, sodass sich der Anwendungsentwickler auf die Details der einzelnen Bildschirme konzentrieren kann.

In der Regel werden Anwendungen im Registerkarten Format erstellt, wobei die `UITabBarController` der `RootViewController` des Hauptfensters ist. Mit etwas zusätzlichem Code können Anwendungen im Registerkarten Format jedoch auch nacheinander für einen anderen anfangs Bildschirm verwendet werden, z. b. für das Szenario, in dem eine Anwendung zuerst einen Anmeldebildschirm anzeigt, gefolgt von der Registerkarte im Registerkarten Format.

Wir untersuchen hier die Verwendung von Registerkarten, indem wir eine exemplarische Vorgehensweise für eine einfache Anwendung durcharbeiten. Dann sehen wir uns an, wie Sie mit Registerkarten im Szenario ohne `RootViewController` arbeiten.

## <a name="introducing-uitabbarcontroller"></a>Einführung in uitabbarcontroller

Der `UITabBarController` unterstützt die Anwendungsentwicklung im Registerkarten Format wie folgt:

- Das Hinzufügen mehrerer Controller ermöglicht.
- Bereitstellen einer Benutzeroberfläche im Registerkarten Format über die `UITabBar`-Klasse, um Benutzern das Wechseln zwischen Controllern und ihren Ansichten zu ermöglichen. 

Controller werden dem `UITabBarController` über seine `ViewControllers`-Eigenschaft hinzugefügt, bei der es sich um ein `UIViewController` Array handelt. Die `UITabBarController` selbst verarbeitet das Laden des richtigen Controllers und die Darstellung der Ansicht basierend auf der ausgewählten Registerkarte.

Die Registerkarten sind Instanzen der `UITabBarItem`-Klasse, die in einer `UITabBar` Instanz enthalten sind. Auf jede `UITabBar` Instanz kann über die `TabBarItem`-Eigenschaft des Controllers auf jeder Registerkarte zugegriffen werden.

Um sich mit der Verwendung der `UITabBarController`vertraut zu machen, können Sie eine einfache Anwendung entwickeln, die eine Anwendung verwendet.

## <a name="tabbed-application-walkthrough"></a>Exemplarische Vorgehensweise zum Registerkarten Format

In dieser exemplarischen Vorgehensweise wird die folgende Anwendung erstellt:

[![](creating-tabbed-applications-images/00-app.png "Sample tabbed app")](creating-tabbed-applications-images/00-app.png#lightbox)

Obwohl bereits eine Anwendungs Vorlage im Registerkarten Format in Visual Studio für Mac verfügbar ist, wird in diesem Beispiel ein leeres Projekt verwendet, um besser zu verstehen, wie die Anwendung erstellt wird.

 <a name="Creating_the_Application" />

### <a name="creating-the-application"></a>Erstellen der Anwendung

Beginnen wir mit dem Erstellen einer neuen Anwendung.

Wählen Sie in Visual Studio für Mac das Menü Element **Datei > neue > Projekt Mappe** aus, und wählen Sie eine **IOS >-app > leere Projekt** Vorlage aus, und nennen Sie das Projekt `TabbedApplication`, wie unten dargestellt:

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Hinzufügen von "uitabbarcontroller"

Fügen Sie als nächstes eine leere Klasse hinzu, indem Sie **Datei > neue Datei** auswählen und die Vorlage " **Allgemein: leere Klasse** " auswählen. Benennen Sie die Datei `TabController` wie unten dargestellt:

[![](creating-tabbed-applications-images/02-newclass.png "Add the TabController class")](creating-tabbed-applications-images/02-newclass.png#lightbox)

Die `TabController`-Klasse enthält die Implementierung des `UITabBarController`, mit dem ein Array von `UIViewControllers`verwaltet wird. Wenn der Benutzer eine Registerkarte auswählt, wird der `UITabBarController` die Ansicht für den entsprechenden Ansichts Controller darstellen.

Zum Implementieren des `UITabBarController` müssen wir die folgenden Schritte ausführen:

1. Legen Sie die Basisklasse von `TabController` auf `UITabBarController` fest. 
1. Erstellen Sie `UIViewController` Instanzen, die der `TabController` hinzugefügt werden sollen. 
1. Fügen Sie die `UIViewController` Instanzen zu einem Array hinzu, das der `ViewControllers`-Eigenschaft der `TabController` zugewiesen ist. 

Fügen Sie der `TabController`-Klasse den folgenden Code hinzu, um die folgenden Schritte auszuführen:

```csharp
using System;
using UIKit;

namespace TabbedApplication {
    public class TabController : UITabBarController {

        UIViewController tab1, tab2, tab3;

        public TabController ()
        {
            tab1 = new UIViewController();
            tab1.Title = "Green";
            tab1.View.BackgroundColor = UIColor.Green;

            tab2 = new UIViewController();
            tab2.Title = "Orange";
            tab2.View.BackgroundColor = UIColor.Orange;

            tab3 = new UIViewController();
            tab3.Title = "Red";
            tab3.View.BackgroundColor = UIColor.Red;

            var tabs = new UIViewController[] {
                tab1, tab2, tab3
            };

            ViewControllers = tabs;
        }
    }
}
```

Beachten Sie, dass für jede `UIViewController` Instanz die `Title`-Eigenschaft der `UIViewController`festgelegt wird. Wenn die Controller der `UITabBarController`hinzugefügt werden, liest der `UITabBarController` den `Title` für jeden Controller und zeigt ihn auf der Bezeichnung der zugehörigen Registerkarte an, wie unten dargestellt:

[![](creating-tabbed-applications-images/00-app.png "The sample app run")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Festlegen des tabcontrollers als rootviewcontroller

Die Reihenfolge, in der die Controller auf den Registerkarten platziert werden, entspricht der Reihenfolge, in der Sie dem `ViewControllers` Array hinzugefügt werden.

Um die `UITabController` zu laden, die als erster Bildschirm geladen werden soll, müssen wir das Fenster `RootViewController`, wie im folgenden Code für die `AppDelegate`dargestellt:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TabController tabController;
    
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        
        tabController = new TabController ();
        window.RootViewController = tabController;
        
        window.MakeKeyAndVisible ();
        
        return true;
    }
}
```

Wenn Sie die Anwendung jetzt ausführen, wird die `UITabBarController` mit der standardmäßig ausgewählten Registerkarte geladen. Wenn Sie eine der anderen Registerkarten auswählen, wird die Ansicht des zugeordneten Controllers vom `UITabBarController,` dargestellt, wie unten dargestellt, wo der Endbenutzer die zweite Registerkarte ausgewählt hat:

[![](creating-tabbed-applications-images/03-secondtab.png "The second tab shown")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />

### <a name="modifying-tabbaritems"></a>Ändern von tabbaritems

Nachdem wir nun eine Registerkarten Anwendung ausgeführt haben, ändern wir die `TabBarItem`, um das Bild und den Text zu ändern, die angezeigt werden, und um einem der Registerkarten einen Badge hinzuzufügen.

 <a name="Setting_a_System_Item" />

#### <a name="setting-a-system-item"></a>Festlegen eines System Elements

Legen Sie zunächst die erste Registerkarte für die Verwendung eines System Elements fest. Entfernen Sie im Konstruktor der `TabController`die Zeile, in der die `Title` des Controllers für die `tab1` Instanz festgelegt wird, und ersetzen Sie Sie durch den folgenden Code, um die `TabBarItem`-Eigenschaft des Controllers festzulegen:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Wenn Sie die `UITabBarItem` mit einer `UITabBarSystemItem`erstellen, werden Titel und Bild automatisch von IOS bereitgestellt, wie im folgenden Screenshot zu sehen: das Symbol " **Favoriten** " und der Titel auf der ersten Registerkarte:

 ![](creating-tabbed-applications-images/04a-tabimage.png "The first tab with a star icon")

 <a name="Setting_the_Title_and_Image" />

#### <a name="setting-the-title-and-image"></a>Festlegen des Titels und Bilds

Zusätzlich zur Verwendung eines System Elements können Titel und Bild eines `UITabBarItem` auf benutzerdefinierte Werte festgelegt werden. Ändern Sie beispielsweise den Code, mit dem die `TabBarItem`-Eigenschaft des Controllers mit dem Namen `tab2` wie folgt festgelegt wird:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Der obige Code geht davon aus, dass ein Image mit dem Namen `second.png` dem Stamm des Projekts in Visual Studio für Mac hinzugefügt wurde. Wir haben unserem Projekt tatsächlich drei Images hinzugefügt, um alle Geräte Auflösungen abzudecken, wie unten dargestellt:

 [![](creating-tabbed-applications-images/tabbedimages7new.png "The images added to the project")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

Das Tabulator Bild muss eine quadratisches 30x30 PNG mit Transparenz für die normale Auflösung, 60X 60 für hohe Auflösung und 90 x 90 für iPhone 6 Plus Auflösung sein. In unserem Code muss nur die Datei mit dem Namen `second.png` geladen werden, und IOS lädt automatisch die hochauflösende Lösung auf Geräten mit einem Retina-Display. Weitere Informationen hierzu finden Sie in den Handbüchern [Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) . Standardmäßig sind Registerkarten-Elemente grau, mit einem blauen Tönungs, wenn Sie ausgewählt sind.

**Hinweis**

Die obigen Images können auch dem **Ressourcen** Verzeichnis hinzugefügt werden. dabei handelt es sich um ein spezielles Verzeichnis, dessen Inhalt automatisch in das Stammverzeichnis des Anwendungspakets kopiert wird:

[![](creating-tabbed-applications-images/tabbedapplication8.png "The images as Resources")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

Wenn Sie die `Title`-Eigenschaft direkt auf der `TabBarItem`festlegen, würde außerdem jeder Wert überschrieben, der für `Title` auf dem Controller selbst festgelegt wurde.

Wenn wir die Anwendung jetzt ausführen, werden auf der zweiten Registerkarte die benutzerdefinierten Titel und das Bild angezeigt, wie unten dargestellt:

[![](creating-tabbed-applications-images/05-customtab.png "The second tab with a square icon")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />

#### <a name="setting-the-badge-value"></a>Festlegen des Badge-Werts

Eine Registerkarte kann auch einen Badge anzeigen. Fügen Sie z. b. die folgende Codezeile hinzu, um einen Badge auf der dritten Registerkarte festzulegen:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Das Ausführen dieser führt in der oberen linken Ecke der Registerkarte zu einer roten Bezeichnung mit der Zeichenfolge "Hi", wie unten dargestellt:

[![](creating-tabbed-applications-images/06-badge.png "The second tab with a Hi badge")](creating-tabbed-applications-images/06-badge.png#lightbox)

Das Badge wird häufig verwendet, um einen ungelesenen Zahlen Hinweis und neue Elemente anzuzeigen. Um das Badge zu entfernen, legen Sie die `BadgeValue` wie unten dargestellt auf NULL fest:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Registerkarten in Szenarios ohne rootviewcontroller

Im obigen Beispiel haben wir gezeigt, wie Sie mit einer `UITabBarController` arbeiten, wenn dies der `RootViewController` des Fensters ist. In diesem Beispiel untersuchen wir, wie ein `UITabBarController` verwendet werden kann, wenn es sich nicht um die `RootViewController` handelt, und wie diese erstellt wird, verwenden Sie Storyboards.

 <a name="Initial_Screen_Example" />

### <a name="initial-screen-example"></a>Beispiel für den Anfangs Bildschirm

In diesem Szenario wird der anfängliche Bildschirm von einem Controller geladen, der keine `UITabBarController`ist. Wenn der Benutzer mit dem Bildschirm durch Tippen auf eine Schaltfläche interagiert, wird derselbe Ansichts Controller in eine `UITabBarController`geladen, die dann dem Benutzer angezeigt wird. Der folgende Screenshot zeigt den Anwendungs Fluss:

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Wir beginnen mit einer neuen Anwendung für dieses Beispiel. Auch hier verwenden wir die Vorlage **iPhone > app > Empty Project (C#)** . dieses Mal wird das Projekt`InitialScreenDemo`benannt.

In diesem Beispiel benötigen wir ein Storyboard zum Speichern unserer Ansichts Controller. So fügen Sie ein Storyboard hinzu:

- Klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **> neue Datei hinzufügen**aus.

- Wenn das Dialogfeld "neue Datei" angezeigt wird, navigieren Sie zu **IOS > leeres iPhone-Storyboard**.

Nennen wir dieses neue Storyboard **mainstoryboard** , wie unten gezeigt: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Beachten Sie beim Hinzufügen eines Storyboards zu einer zuvor nicht-Storyboard-Datei einige wichtige Schritte, die im Handbuch [Introduction to Storyboards (Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) ) beschrieben werden. Diese lauten wie folgt:

1. Fügen Sie den Storyboardnamen zum **Haupt Schnittstellen** Abschnitt der `Info.plist`hinzu:

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. Überschreiben Sie im `App Delegate`die Window-Methode mit dem folgenden Code:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

Wir benötigen drei Ansichts Controller für dieses Beispiel. Eine mit dem Namen "`ViewController1`" wird als erster Ansichts Controller und auf der ersten Registerkarte verwendet. Die anderen beiden, mit dem Namen `ViewController2` und `ViewController3`, die auf der zweiten bzw. dritten Registerkarte verwendet werden.

Öffnen Sie den Designer durch Doppelklicken auf die Datei mainstoryboard. Storyboard, und ziehen Sie drei Ansichts Controller auf die Entwurfs Oberfläche. Wir möchten, dass jeder dieser Ansichts Controller eine eigene Klasse hat, die dem oben genannten Namen entspricht. Geben Sie also unter **Identity > Class**den Namen ein, wie im folgenden Screenshot veranschaulicht:

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio für Mac werden automatisch die erforderlichen Klassen und Designer Dateien generieren, wie Lösungspad im folgenden dargestellt:

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />

#### <a name="creating-the-ui"></a>Erstellen der Benutzeroberfläche

Als Nächstes erstellen wir eine einfache Benutzeroberfläche für jede Ansicht des viewcontrollers mithilfe des xamarin IOS-Designers.

Wir möchten eine `Label` und ein `Button` aus der **Toolbox** auf der rechten Seite auf ViewController1 ziehen. Als nächstes verwenden wir die Eigenschaftenpad, um den Namen und den Text der Steuerelemente wie folgt zu bearbeiten:

- **Bezeichnung** : **`Text` = **
- **Schaltfläche** : `Title` = **Benutzer einige erste Aktionen durchführt**

Wir steuern die Sichtbarkeit unserer Schaltfläche in einem `TouchUpInside`-Ereignis, und wir müssen im Code Behind darauf verweisen. Wir identifizieren ihn mit dem **Namen** `aButton` im Eigenschaftenpad, wie im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Die Designoberfläche sollte nun in etwa wie im folgenden Screenshot aussehen:

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Fügen Sie `ViewController2` und `ViewController3`ein paar weitere Details hinzu, indem Sie jeweils eine Bezeichnung hinzufügen und den Text in "Two" bzw. "Three" ändern. Dadurch wird der Benutzer hervorgehoben, welche Registerkarte/Ansicht wir betrachten.

#### <a name="wiring-up-the-button"></a>Die Schaltfläche wird verknüpft.

Beim ersten Start der Anwendung werden `ViewController1` geladen. Wenn der Benutzer auf die Schaltfläche tippt, blenden Sie die Schaltfläche aus und laden eine `UITabBarController` mit der `ViewController1` Instanz auf der ersten Registerkarte.

Wenn der Benutzer die `aButton`freigibt, soll ein touchupinside-Ereignis ausgelöst werden. Wählen Sie die Schaltfläche aus, und deklarieren Sie auf der **Registerkarte Ereignisse** des eigenschaftenpad den Ereignishandler – `InitialActionCompleted` –, damit auf ihn im Code verwiesen werden kann. Dies wird im folgenden Screenshot veranschaulicht:

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

Wir müssen dem Ansichts Controller nun mitteilen, dass die Schaltfläche ausgeblendet wird, wenn das Ereignis `InitialActionCompleted`ausgelöst wird. Fügen Sie in `ViewController1`die folgende partielle Methode hinzu:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Speichern Sie die Datei, und führen Sie die Anwendung aus. Der Bildschirm wird angezeigt, und die Schaltfläche wird bei der einschaltfläche ausgeblendet.

#### <a name="adding-the-tab-bar-controller"></a>Hinzufügen des Registerkarten leisten-Controllers

Unsere anfängliche Ansicht funktioniert jetzt erwartungsgemäß. Als nächstes möchten wir Sie einem `UITabBarController`hinzufügen, zusammen mit den Sichten 2 und 3. Öffnen Sie das Storyboard im Designer.

Suchen Sie in der **Toolbox**unter Controller & Objekte nach dem Registerkarten leisten **Controller** , und ziehen Sie diesen auf die Designoberfläche. Wie Sie im nachfolgenden Screenshot sehen können, ist der Registerkarten-Steuerelemente auf der Benutzeroberfläche kleiner und führt daher standardmäßig zwei Ansichts Controller ein:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Löschen Sie diese neuen Ansichts Controller, indem Sie die schwarze Leiste unten auswählen und auf Löschen klicken.

In unserem Storyboard können wir die Übergänge zwischen tabbarcontroller und unseren Ansichts Controllern verwenden. Nachdem Sie mit der ersten Ansicht interagiert haben, möchten wir Sie in den tabbarcontroller laden, der dem Benutzer angezeigt wird. Wir legen dies im Designer fest.

Drücken **Sie die STRG-** Taste, und **ziehen** Sie Sie von der Schaltfläche auf tabbarcontroller. Bei einem Mausklick wird ein Kontextmenü angezeigt. Wir möchten einen modalen "*" verwenden. 

Zum Einrichten der einzelnen Registerkarten **Klicken Sie mit der STRG-** Taste auf die einzelnen Ansichts Controller, und wählen Sie die **Registerkarte** Beziehung aus dem Kontextmenü aus, wie unten dargestellt:

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

Ihr Storyboard sollte in etwa wie im folgenden Screenshot aussehen:

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Wenn Sie auf eine der Registerkarten Elemente klicken und den Eigenschaften Bereich Durchsuchen, sehen Sie eine Reihe verschiedener Optionen, wie unten dargestellt:

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Wir können dies verwenden, um bestimmte Attribute, wie z. b. das Badge, den Titel und den IOS- [Bezeichner](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), zu bearbeiten.

Wenn wir die Anwendung jetzt speichern und ausführen, werden Sie feststellen, dass die Schaltfläche erneut angezeigt wird, wenn die ViewController1-Instanz in den tabbarcontroller geladen wird. Korrigieren Sie dies, indem Sie überprüfen, ob die aktuelle Ansicht über einen übergeordneten Ansichts Controller verfügt. Wenn dies der Fall ist, wissen wir, dass wir uns im tabbarcontroller befinden, und daher sollte die Schaltfläche ausgeblendet werden. Fügen Sie der ViewController1-Klasse den folgenden Code hinzu:

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

Wenn die Anwendung ausgeführt wird und der Benutzer auf die Schaltfläche auf dem ersten Bildschirm tippt, wird der uitabbarcontroller geladen, wobei sich die Ansicht vom ersten Bildschirm auf der ersten Registerkarte befindet, wie unten dargestellt:

[![](creating-tabbed-applications-images/first-view.png "The sample app output")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie eine `UITabBarController` in einer Anwendung verwenden. Wir haben das Laden von Controllern in jede Registerkarte und das Festlegen von Eigenschaften auf Registerkarten wie Titel, Bild und Badge erläutert. Anschließend wird mithilfe von Storyboards überprüft, wie ein `UITabBarController` zur Laufzeit geladen wird, wenn es sich nicht um die `RootViewController` des Fensters handelt.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Anwendungen im Registerkarten Format (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images. zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Uitabbarcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
