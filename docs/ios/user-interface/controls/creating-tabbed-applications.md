---
title: Registerkartenleisten und Registerkartenleisten-Controller in Xamarin.iOS
description: Dieses Dokument beschreibt die iOS-Registerkarte Leiste Controller und wie sie mit Xamarin.iOS verwendet. Es wird veranschaulicht, wie eine UITabBarController einrichten, arbeiten mit Bildern, Badge-Werten, arbeiten mit Ereignissen und mehr festlegen.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 25d8563288cce614bc2823b0146e5121688c6f02
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725480"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Registerkarten und Tab-leisten-Controller in xamarin. IOS

Im Registerkartenformat Anwendungen werden in iOS verwendet, zur Unterstützung von Benutzeroberflächen, in denen mehrere Bildschirme zugegriffen werden kann, ohne bestimmte Reihenfolge. Durch die `UITabBarController`-Klasse können Anwendungen problemlos Unterstützung für solche Szenarien mit mehreren Bildschirmen enthalten. `UITabBarController` kümmert sich um die Verwaltung mit mehreren Bildschirmen, sodass sich der Anwendungsentwickler auf die Details der einzelnen Bildschirme konzentrieren kann.

In der Regel werden Anwendungen im Registerkarten Format erstellt, wobei die `UITabBarController` der `RootViewController` des Hauptfensters ist. Allerdings können mit etwas zusätzlichem Code, im Registerkartenformat Anwendungen auch in Folge zu einigen anderen ersten Bildschirm, wie z. B. das Szenario verwendet werden, bei der eine Anwendung zunächst einen Anmeldebildschirm, gefolgt von der Schnittstelle im Registerkartenformat präsentiert.

Auf dieser Seite werden beide Szenarien erläutert: Wenn sich die Registerkarten im Stammverzeichnis der Anwendungs Ansichts Hierarchie und auch im Szenario ohne`RootViewController` befinden.

## <a name="introducing-uitabbarcontroller"></a>Einführung in UITabBarController

Der `UITabBarController` unterstützt die Anwendungsentwicklung im Registerkarten Format wie folgt:

- So können mehrere Controller hinzugefügt werden.
- Bereitstellen einer Benutzeroberfläche im Registerkarten Format über die `UITabBar`-Klasse, um Benutzern das Wechseln zwischen Controllern und ihren Ansichten zu ermöglichen.

Controller werden dem `UITabBarController` über seine `ViewControllers`-Eigenschaft hinzugefügt, bei der es sich um ein `UIViewController` Array handelt. Die `UITabBarController` selbst verarbeitet das Laden des richtigen Controllers und die Darstellung der Ansicht basierend auf der ausgewählten Registerkarte.

Die Registerkarten sind Instanzen der `UITabBarItem`-Klasse, die in einer `UITabBar` Instanz enthalten sind. Auf jede `UITabBar` Instanz kann über die `TabBarItem`-Eigenschaft des Controllers auf jeder Registerkarte zugegriffen werden.

Um sich mit der Verwendung der `UITabBarController`vertraut zu machen, können Sie eine einfache Anwendung entwickeln, die eine Anwendung verwendet.

## <a name="tabbed-application-walkthrough"></a>Exemplarische Vorgehensweise zum Registerkarten Format

In dieser exemplarischen Vorgehensweise werden wir die folgende Anwendung zu erstellen:

[![Beispiel-App im Registerkarten Format](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Obwohl bereits eine Anwendungs Vorlage im Registerkarten Format in Visual Studio für Mac verfügbar ist, können diese Anweisungen in diesem Beispiel von einem leeren Projekt verwendet werden, um ein besseres Verständnis der Erstellung der Anwendung zu erhalten.

### <a name="creating-the-application"></a>Erstellen der Anwendung

Beginnen Sie mit dem Erstellen einer neuen Anwendung.

Wählen Sie in Visual Studio für Mac das Menü Element **Datei > neue > Projekt Mappe** aus, und wählen Sie eine **IOS >-app > leere Projekt** Vorlage aus, und nennen Sie das Projekt `TabbedApplication`, wie unten dargestellt:

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Hinzufügen der UITabBarController

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

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Legen Sie die TabController als die RootViewController

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

![Die zweite angezeigte Registerkarte](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>TabBarItems ändern

Nachdem wir nun eine Registerkarten Anwendung ausgeführt haben, ändern wir die `TabBarItem`, um das Bild und den Text zu ändern, die angezeigt werden, und um einem der Registerkarten einen Badge hinzuzufügen.

#### <a name="setting-a-system-item"></a>Festlegen eines System Elements

Zunächst legen wir die erste Registerkarte enthält ein System-Element verwenden. Entfernen Sie im Konstruktor der `TabController`die Zeile, in der die `Title` des Controllers für die `tab1` Instanz festgelegt wird, und ersetzen Sie Sie durch den folgenden Code, um die `TabBarItem`-Eigenschaft des Controllers festzulegen:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Wenn Sie die `UITabBarItem` mit einer `UITabBarSystemItem`erstellen, werden Titel und Bild automatisch von IOS bereitgestellt, wie im folgenden Screenshot zu sehen: das Symbol " **Favoriten** " und der Titel auf der ersten Registerkarte:

 ![Die erste Registerkarte mit einem Stern Symbol](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>Festlegen des Bilds

Zusätzlich zur Verwendung eines System Elements können Titel und Bild eines `UITabBarItem` auf benutzerdefinierte Werte festgelegt werden. Ändern Sie beispielsweise den Code, mit dem die `TabBarItem`-Eigenschaft des Controllers mit dem Namen `tab2` wie folgt festgelegt wird:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Der obige Code geht davon aus, dass ein Image mit dem Namen `second.png` dem Stamm des Projekts (oder einem **Ressourcen** Verzeichnis) hinzugefügt wurde. Um alle Bildschirm dichten zu unterstützen, benötigen Sie drei Bilder, wie unten dargestellt:

![Die dem Projekt hinzugefügten Bilder](creating-tabbed-applications-images/tabbedimages7new.png)

Eine Anleitung zu den richtigen Dimensionen finden Sie im Abschnitt " **Symbolgröße** " der [Seite "benutzerdefinierte Symbole](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/) " von Apple. Die empfohlene Größe variiert je nach Art des Bilds (zirkulär, quadratisch, breit oder hoch).

Die `Image`-Eigenschaft muss nur auf den **zweiten PNG** -Dateinamen festgelegt werden, IOS lädt die Dateien mit höherer Auflösung bei Bedarf automatisch. Weitere Informationen hierzu finden Sie in den Handbüchern [Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) . Standardmäßig sind die Elemente der Registerkarte grau, mit einem blauen Farbton, wenn ausgewählt.

#### <a name="overriding-the-title"></a>Überschreiben des Titels

Wenn die `Title`-Eigenschaft direkt auf dem `TabBarItem`festgelegt wird, überschreibt Sie alle Werte, die für `Title` auf dem Controller selbst festgelegt wurden.

Die zweite (mittlere) Registerkarte in diesem Screenshot zeigt einen benutzerdefinierten Titel und ein Bild:

![Die zweite Registerkarte mit einem quadratischen Symbol](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>Festlegen des Badge-Werts

Eine Registerkarte kann auch einen Badge anzeigen. Fügen Sie z.B. Code aus, um einen Badge auf der dritten Registerkarte die folgende Zeile hinzu:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Dies Ausführung erzeugt eine rote Beschriftung mit der Zeichenfolge "Hi" in links oben auf der Registerkarte wie folgt:

![Die zweite Registerkarte mit einem Hi-Badge](creating-tabbed-applications-images/06-badge-sml.png)

Der Badge wird häufig zum Anzeigen einer Anzahl Angabe ungelesene, neue Elemente. Um das Badge zu entfernen, legen Sie die `BadgeValue` wie unten dargestellt auf NULL fest:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Registerkarten in Szenarios ohne rootviewcontroller

Im obigen Beispiel haben wir gezeigt, wie Sie mit einer `UITabBarController` arbeiten, wenn dies der `RootViewController` des Fensters ist. In diesem Beispiel untersuchen wir, wie ein `UITabBarController` verwendet werden kann, wenn es sich nicht um die `RootViewController` handelt, und wie diese erstellt wird, verwenden Sie Storyboards.

### <a name="initial-screen-example"></a>Beispiel für den Anfangs Bildschirm

In diesem Szenario wird der anfängliche Bildschirm von einem Controller geladen, der keine `UITabBarController`ist. Wenn der Benutzer mit dem Bildschirm durch Tippen auf eine Schaltfläche interagiert, wird derselbe Ansichts Controller in eine `UITabBarController`geladen, die dann dem Benutzer angezeigt wird. Der folgende Screenshot zeigt den Anwendungsfluss:

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Beginnen Sie eine neue Anwendung für dieses Beispiel an. Auch hier verwenden wir die Vorlage **iPhone > app > Empty Project (C#)** . dieses Mal wird das Projekt `InitialScreenDemo`benannt.

In diesem Beispiel wird ein Storyboard zum Anordnen von Ansichts Controllern verwendet. So fügen Sie ein Storyboard hinzu:

- Klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **> neue Datei hinzufügen**aus.

- Wenn das Dialogfeld "neue Datei" angezeigt wird, navigieren Sie zu **IOS > leeres iPhone-Storyboard**.

Nennen wir dieses neue Storyboard **mainstoryboard** , wie unten gezeigt:

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Beachten Sie beim Hinzufügen eines Storyboards zu einer zuvor nicht-Storyboard-Datei einige wichtige Schritte, die im Handbuch [Introduction to Storyboards (Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) ) beschrieben werden. Dies sind:

1. Fügen Sie den Storyboardnamen zum **Haupt Schnittstellen** Abschnitt der `Info.plist`hinzu:

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. Überschreiben Sie im `App Delegate`die Window-Methode mit dem folgenden Code:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

Wir werden drei Ansichtscontroller für dieses Beispiel benötigen. Eine mit dem Namen "`ViewController1`" wird als erster Ansichts Controller und auf der ersten Registerkarte verwendet. Die anderen beiden, mit dem Namen `ViewController2` und `ViewController3`, die auf der zweiten bzw. dritten Registerkarte verwendet werden.

Öffnen Sie den Designer, indem Sie auf die MainStoryboard.storyboard-Datei doppelklicken, und ziehen Sie drei View-Controller auf die Entwurfsoberfläche. Wir möchten, dass jeder dieser Ansichts Controller eine eigene Klasse hat, die dem oben genannten Namen entspricht. Geben Sie also unter **Identity > Class**den Namen ein, wie im folgenden Screenshot veranschaulicht:

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio für Mac generiert automatisch die Klassen und Designer-Dateien erforderlich, dies ist in der Projektmappe, wie unten gezeigt:

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>Erstellen der Benutzeroberfläche

Als Nächstes erstellen eine einfache Benutzeroberfläche für jede ViewControllers Ansichten, wir mit dem Xamarin.IOS-Designer.

Wir möchten eine `Label` und ein `Button` aus der **Toolbox** auf der rechten Seite auf ViewController1 ziehen. Als Nächstes verwenden wir das Pad "Eigenschaften", so bearbeiten Sie den Namen und den Text der Steuerelemente auf Folgendes:

- **Bezeichnung** : **`Text` = **
- **Schaltfläche** : `Title` = **Benutzer einige erste Aktionen durchführt**

Wir steuern die Sichtbarkeit unserer Schaltfläche in einem `TouchUpInside`-Ereignis, und wir müssen im Code Behind darauf verweisen. Wir identifizieren ihn mit dem **Namen** `aButton` im Eigenschaftenpad, wie im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Ihrer Entwurfsoberfläche sollte nun ähnlich wie im folgenden Screenshot aussehen:

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Fügen Sie `ViewController2` und `ViewController3`ein paar weitere Details hinzu, indem Sie jeweils eine Bezeichnung hinzufügen und den Text in "Two" bzw. "Three" ändern. Dies hebt hervor, welche Registerkarte/Ansicht, wir betrachten, die dem Benutzer.

#### <a name="wiring-up-the-button"></a>Die Schaltfläche wird verknüpft.

Beim ersten Start der Anwendung werden `ViewController1` geladen. Wenn der Benutzer auf die Schaltfläche tippt, blenden Sie die Schaltfläche aus und laden eine `UITabBarController` mit der `ViewController1` Instanz auf der ersten Registerkarte.

Wenn der Benutzer die `aButton`freigibt, soll ein touchupinside-Ereignis ausgelöst werden. Wählen Sie die Schaltfläche aus, und deklarieren Sie auf der **Registerkarte Ereignisse** des eigenschaftenpad den Ereignishandler – `InitialActionCompleted` –, damit auf ihn im Code verwiesen werden kann. Dies ist im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

Wir müssen dem Ansichts Controller nun mitteilen, dass die Schaltfläche ausgeblendet wird, wenn das Ereignis `InitialActionCompleted`ausgelöst wird. Fügen Sie in `ViewController1`die folgende partielle Methode hinzu:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Speichern Sie die Datei, und führen Sie die Anwendung. Es sollte sich Bildschirm eines angezeigt werden und die Schaltfläche mit den berühren Sie nicht mehr angezeigt werden.

#### <a name="adding-the-tab-bar-controller"></a>Hinzufügen des Registerkarten leisten-Controllers

Wir haben jetzt unsere erste Ansicht wie erwartet funktioniert. Als nächstes möchten wir Sie einem `UITabBarController`hinzufügen, zusammen mit den Sichten 2 und 3. Öffnen Sie das Storyboard wir im Designer.

Suchen Sie in der **Toolbox**unter Controller & Objekte nach dem Registerkarten leisten **Controller** , und ziehen Sie diesen auf die Designoberfläche. Wie im folgenden Screenshot sehen ist, wird die Registerkartenleistencontroller UI-lose und aus diesem Grund bietet zwei Ansichtscontrollern dabei standardmäßig:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Löschen Sie diese neue Ansicht-Controller, indem die schwarze Leiste am unteren Rand, und drücken ENTF.

In unserem Storyboards können wir Segues verwenden, um die Übergänge zwischen den TabBarController und unsere View-Controller zu verarbeiten. Nach dem Arbeiten mit die anfängliche Ansicht, möchten wir sie in der dem Benutzer angezeigten TabBarController laden. Richten wir dies im Designer.

Drücken **Sie die STRG-** Taste, und **ziehen** Sie Sie von der Schaltfläche auf tabbarcontroller. In der Maustaste oben wird ein Kontextmenü angezeigt. Eine modale Segue verwendet werden soll.

Zum Einrichten der einzelnen Registerkarten **Klicken Sie mit der STRG-** Taste auf die einzelnen Ansichts Controller, und wählen Sie die **Registerkarte** Beziehung aus dem Kontextmenü aus, wie unten dargestellt:

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

Ihr Storyboard sollte wie im folgenden Screenshot aussehen:

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Wenn wir klicken Sie auf eines der Elemente der Registerkarte und den Eigenschaftenbereich untersuchen, sehen Sie eine Reihe von verschiedenen Optionen, wie unten gezeigt:

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Wir können dies verwenden, um bestimmte Attribute, wie z. b. das Badge, den Titel und den IOS-Bezeichner, zu bearbeiten.

Wir speichern und die Anwendung jetzt ausführen, werden wir feststellen, dass die Schaltfläche wird erneut angezeigt, wenn die ViewController1-Instanz in der TabBarController geladen wird. Lassen Sie uns dies beheben, indem überprüft wird, wenn die aktuelle Ansicht eine übergeordnete Ansicht-Controller verfügt. Wenn Ja, wir wissen, dass wir innerhalb der TabBarController sind und daher die Schaltfläche ausgeblendet werden soll. Fügen Sie den folgenden Code, der ViewController1-Klasse:

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

Wenn die Anwendung ausgeführt wird und der Benutzer die Schaltfläche auf dem ersten Bildschirm die UITabBarController tippt wird, mit der Ansicht vom ersten Bildschirm in der ersten Registerkarte platziert werden, wie unten gezeigt geladen:

[![der Ausgabe der Beispiel-App](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie eine `UITabBarController` in einer Anwendung verwenden. Wir durchgearbeitet Laden Sie die Controller in jede Registerkarte sowie Festlegen von Eigenschaften auf Registerkarten solche Titel, Bild und Badge. Anschließend wird mithilfe von Storyboards überprüft, wie ein `UITabBarController` zur Laufzeit geladen wird, wenn es sich nicht um die `RootViewController` des Fensters handelt.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Anwendungen im Registerkarten Format (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images. zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Uitabbarcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
