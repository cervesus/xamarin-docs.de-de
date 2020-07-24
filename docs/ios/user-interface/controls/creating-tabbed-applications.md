---
title: Registerkarten und Tab-leisten-Controller in xamarin. IOS
description: In diesem Dokument werden die IOS-Registerkarten-Steuerelemente und deren Verwendung mit xamarin. IOS beschrieben. Es zeigt, wie Sie einen uitabbarcontroller einrichten, mit Bildern arbeiten, Badge-Werte festlegen, mit Ereignissen arbeiten und vieles mehr.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 2e8dde87456c6e33eda6846967ceea13eb412b93
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934302"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Registerkarten und Tab-leisten-Controller in xamarin. IOS

Anwendungen mit Registerkarten werden in ios verwendet, um Benutzeroberflächen zu unterstützen, bei denen auf mehrere Bildschirme in einer bestimmten Reihenfolge zugegriffen werden kann Mithilfe der- `UITabBarController` Klasse können Anwendungen problemlos Unterstützung für solche Szenarien mit mehreren Bildschirmen enthalten. `UITabBarController`kümmert sich um die Verwaltung mit mehreren Bildschirmen, sodass sich der Anwendungsentwickler auf die Details der einzelnen Bildschirme konzentrieren kann.

In der Regel werden Anwendungen im Registerkarten Format mit dem `UITabBarController` `RootViewController` des Hauptfensters erstellt. Mit etwas zusätzlichem Code können Anwendungen im Registerkarten Format jedoch auch nacheinander für einen anderen anfangs Bildschirm verwendet werden, z. b. für das Szenario, in dem eine Anwendung zuerst einen Anmeldebildschirm anzeigt, gefolgt von der Registerkarte im Registerkarten Format.

Auf dieser Seite werden beide Szenarien erläutert: Wenn sich die Registerkarten im Stammverzeichnis der Anwendungs Ansichts Hierarchie und auch im nicht- `RootViewController` Szenario befinden.

## <a name="introducing-uitabbarcontroller"></a>Einführung in uitabbarcontroller

`UITabBarController`Unterstützt die Anwendungsentwicklung im Registerkarten Format wie folgt:

- Das Hinzufügen mehrerer Controller ermöglicht.
- Bereitstellen einer Benutzeroberfläche im Registerkarten Format über die- `UITabBar` Klasse, um es einem Benutzer zu ermöglichen, zwischen Controllern und ihren Ansichten zu wechseln.

Controller werden über die- `UITabBarController` `ViewControllers` Eigenschaft, die ein-Array ist, dem hinzugefügt `UIViewController` . Die `UITabBarController` selbst verarbeitet das Laden des richtigen Controllers und die Darstellung der Ansicht basierend auf der ausgewählten Registerkarte.

Die Registerkarten sind Instanzen der- `UITabBarItem` Klasse, die in einer- `UITabBar` Instanz enthalten sind. `UITabBar`Auf jede Instanz kann über die- `TabBarItem` Eigenschaft des Controllers auf jeder Registerkarte zugegriffen werden.

Um sich mit der Arbeit mit vertraut zu machen `UITabBarController` , betrachten wir das Entwickeln einer einfachen Anwendung, die eine Anwendung verwendet.

## <a name="tabbed-application-walkthrough"></a>Exemplarische Vorgehensweise zum Registerkarten Format

In dieser exemplarischen Vorgehensweise wird die folgende Anwendung erstellt:

[![Beispiel-App im Registerkarten Format](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Obwohl bereits eine Anwendungs Vorlage im Registerkarten Format in Visual Studio für Mac verfügbar ist, können diese Anweisungen in diesem Beispiel von einem leeren Projekt verwendet werden, um ein besseres Verständnis der Erstellung der Anwendung zu erhalten.

### <a name="creating-the-application"></a>Erstellen der Anwendung

Beginnen Sie mit dem Erstellen einer neuen Anwendung.

Wählen Sie in Visual Studio für Mac das Menü Element **Datei > neue > Projekt Mappe** aus, und wählen Sie eine **IOS >-App > leere Projekt** Vorlage aus, und nennen Sie das Projekt `TabbedApplication` , wie unten dargestellt:

[![Leere Projektvorlage auswählen](creating-tabbed-applications-images/newsolution1.png)](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![Benennen Sie das Projekt mit tabbedapplikation.](creating-tabbed-applications-images/newsolution2.png)](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Hinzufügen von "uitabbarcontroller"

Fügen Sie als nächstes eine leere Klasse hinzu, indem Sie **Datei > neue Datei** auswählen und die Vorlage " **Allgemein: leere Klasse** " auswählen. Benennen Sie die Datei `TabController` wie unten dargestellt:

[![Hinzufügen der tabcontroller-Klasse](creating-tabbed-applications-images/02-newclass.png)](creating-tabbed-applications-images/02-newclass.png#lightbox)

Die- `TabController` Klasse enthält die Implementierung von `UITabBarController` , die ein Array von verwaltet `UIViewControllers` . Wenn der Benutzer eine Registerkarte `UITabBarController` auswählt, übernimmt die Darstellung der Ansicht für den entsprechenden Ansichts Controller.

Zum Implementieren von `UITabBarController` müssen wir die folgenden Schritte ausführen:

1. Legen Sie die Basisklasse von `TabController` auf fest `UITabBarController` .
1. Erstellen Sie `UIViewController` Instanzen, die dem hinzugefügt werden sollen `TabController` .
1. Fügen Sie die- `UIViewController` Instanzen zu einem Array hinzu, das der- `ViewControllers` Eigenschaft von zugewiesen ist `TabController` .

Fügen Sie der-Klasse den folgenden Code hinzu `TabController` , um die folgenden Schritte auszuführen:

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

Beachten Sie, dass für jede `UIViewController` Instanz die- `Title` Eigenschaft von festgelegt wird `UIViewController` . Wenn die Controller dem hinzugefügt werden `UITabBarController` , `UITabBarController` liest das `Title` für jeden Controller und zeigt es wie unten dargestellt auf der Bezeichnung der zugehörigen Registerkarte an:

[![Ausführen der Beispiel-App](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Festlegen des tabcontrollers als rootviewcontroller

Die Reihenfolge, in der die Controller auf den Registerkarten platziert werden, entspricht der Reihenfolge, in der Sie dem Array hinzugefügt werden `ViewControllers` .

Damit der `UITabController` als erster Bildschirm geladen werden kann, müssen wir ihn `RootViewController` wie im folgenden Code für das Fenster sehen, wie im folgenden Code dargestellt `AppDelegate` :

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

Wenn Sie die Anwendung jetzt ausführen, `UITabBarController` wird die mit der ersten standardmäßig ausgewählten Registerkarte geladen. Wenn Sie eine der anderen Registerkarten auswählen, wird die Ansicht des zugeordneten Controllers vom dargestellt, `UITabBarController,` wie unten dargestellt, wo der Endbenutzer die zweite Registerkarte ausgewählt hat:

![Die zweite angezeigte Registerkarte](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>Ändern von tabbaritems

Nachdem wir nun eine Registerkarte mit Registerkarten ausgeführt haben, ändern wir die, `TabBarItem` um das Bild und den Text zu ändern, die angezeigt werden, und um einem der Registerkarten einen Badge hinzuzufügen.

#### <a name="setting-a-system-item"></a>Festlegen eines System Elements

Legen Sie zunächst die erste Registerkarte für die Verwendung eines System Elements fest. Entfernen Sie im Konstruktor von `TabController` die Zeile, in der die Controller `Title` für die-Instanz festgelegt wird, `tab1` und ersetzen Sie Sie durch den folgenden Code, um die-Eigenschaft des Controllers festzulegen `TabBarItem` :

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Wenn `UITabBarItem` Sie mit einem erstellen `UITabBarSystemItem` , werden Titel und Bild automatisch von IOS bereitgestellt, wie im nachfolgenden Screenshot gezeigt, auf dem das **Favoriten** Symbol und der Titel auf der ersten Registerkarte angezeigt werden:

 ![Die erste Registerkarte mit einem Stern Symbol](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>Festlegen des Bilds

Zusätzlich zur Verwendung eines System Elements können Titel und Bild eines `UITabBarItem` auf benutzerdefinierte Werte festgelegt werden. Ändern Sie beispielsweise den Code, mit dem die- `TabBarItem` Eigenschaft des Controllers mit dem Namen festgelegt wird, `tab2` wie folgt:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Der obige Code geht davon aus, dass ein Bild `second.png` mit dem Namen dem Stamm des Projekts (oder einem **Ressourcen** Verzeichnis) hinzugefügt wurde. Um alle Bildschirm dichten zu unterstützen, benötigen Sie drei Bilder, wie unten dargestellt:

![Die dem Projekt hinzugefügten Bilder](creating-tabbed-applications-images/tabbedimages7new.png)

Eine Anleitung zu den richtigen Dimensionen finden Sie im Abschnitt " **Symbolgröße** " der [Seite "benutzerdefinierte Symbole](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/) " von Apple. Die empfohlene Größe variiert je nach Art des Bilds (zirkulär, quadratisch, breit oder hoch).

Die- `Image` Eigenschaft muss nur auf den Dateinamen **second.png** festgelegt werden. bei Bedarf werden die Dateien mit höherer Auflösung automatisch von IOS geladen. Weitere Informationen hierzu finden Sie in den Handbüchern [Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) . Standardmäßig sind Registerkarten-Elemente grau, mit einem blauen Tönungs, wenn Sie ausgewählt sind.

#### <a name="overriding-the-title"></a>Überschreiben des Titels

Wenn die- `Title` Eigenschaft direkt auf festgelegt wird `TabBarItem` , wird jeder für `Title` den Controller selbst festgelegte Wert überschrieben.

Die zweite (mittlere) Registerkarte in diesem Screenshot zeigt einen benutzerdefinierten Titel und ein Bild:

![Die zweite Registerkarte mit einem quadratischen Symbol](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>Festlegen des Badge-Werts

Eine Registerkarte kann auch einen Badge anzeigen. Fügen Sie z. b. die folgende Codezeile hinzu, um einen Badge auf der dritten Registerkarte festzulegen:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Das Ausführen dieser führt in der oberen linken Ecke der Registerkarte zu einer roten Bezeichnung mit der Zeichenfolge "Hi", wie unten dargestellt:

![Die zweite Registerkarte mit einem Hi-Badge](creating-tabbed-applications-images/06-badge-sml.png)

Das Badge wird häufig verwendet, um einen ungelesenen Zahlen Hinweis und neue Elemente anzuzeigen. Um das Badge zu entfernen, legen `BadgeValue` Sie den auf NULL fest, wie unten dargestellt:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Registerkarten in Szenarios ohne rootviewcontroller

Im obigen Beispiel haben wir gezeigt, wie Sie mit einem arbeiten, `UITabBarController` Wenn es der `RootViewController` des Fensters ist. In diesem Beispiel wird untersucht, wie ein verwendet wird, `UITabBarController` Wenn es sich nicht um die Verwendung von `RootViewController` Storyboards handelt.

### <a name="initial-screen-example"></a>Beispiel für den Anfangs Bildschirm

In diesem Szenario lädt der anfängliche Bildschirm von einem Controller, der keine ist `UITabBarController` . Wenn der Benutzer mit dem Bildschirm durch Tippen auf eine Schaltfläche interagiert, wird derselbe Ansichts Controller in eine geladen `UITabBarController` , die dann dem Benutzer angezeigt wird. Der folgende Screenshot zeigt den Anwendungsfluss:

[![Dieser Screenshot zeigt den Anwendungs Fluss.](creating-tabbed-applications-images/inital-screen-application.png)](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Wir beginnen mit einer neuen Anwendung für dieses Beispiel. Auch hier verwenden wir die **iPhone-> App > Empty Project (c#)** -Vorlage. dieses Mal benennen wir das Projekt `InitialScreenDemo` .

In diesem Beispiel wird ein Storyboard zum Anordnen von Ansichts Controllern verwendet. So fügen Sie ein Storyboard hinzu:

- Klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **> neue Datei hinzufügen**aus.

- Wenn das Dialogfeld "neue Datei" angezeigt wird, navigieren Sie zu **IOS > leeres iPhone-Storyboard**.

Nennen wir dieses neue Storyboard **mainstoryboard** , wie unten gezeigt:

[![Fügen Sie dem Projekt eine mainstoryboard-Datei hinzu.](creating-tabbed-applications-images/new-file-dialog.png)](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Beachten Sie beim Hinzufügen eines Storyboards zu einer zuvor nicht-Storyboard-Datei einige wichtige Schritte, die im Handbuch [Introduction to Storyboards (Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) ) beschrieben werden. Dies sind:

1. Fügen Sie den Storyboardnamen zum **Haupt Schnittstellen** Abschnitt von hinzu `Info.plist` :

    [![Legen Sie die Hauptschnittstelle auf mainstoryboard fest.](creating-tabbed-applications-images/project-options.png)](creating-tabbed-applications-images/project-options.png#lightbox)
1. `App Delegate`Überschreiben Sie in der die Window-Methode mit dem folgenden Code:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

Wir benötigen drei Ansichts Controller für dieses Beispiel. Eine `ViewController1` mit dem Namen wird als erster Ansichts Controller und auf der ersten Registerkarte verwendet. Die anderen beiden, mit dem Namen `ViewController2` und `ViewController3` , die auf der zweiten bzw. dritten Registerkarte verwendet werden.

Öffnen Sie den Designer durch Doppelklicken auf die Datei mainstoryboard. Storyboard, und ziehen Sie drei Ansichts Controller auf die Entwurfs Oberfläche. Wir möchten, dass jeder dieser Ansichts Controller eine eigene Klasse hat, die dem oben genannten Namen entspricht. Geben Sie also unter **Identity > Class**den Namen ein, wie im folgenden Screenshot veranschaulicht:

[![Legen Sie die Klasse auf ViewController1 fest.](creating-tabbed-applications-images/class-name.png)](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio für Mac werden automatisch die erforderlichen Klassen und Designer Dateien generieren, wie Lösungspad im folgenden dargestellt:

[![Automatisch generierte Dateien im Projekt](creating-tabbed-applications-images/solution-pad2.png)](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>Erstellen der Benutzeroberfläche

Als Nächstes erstellen wir eine einfache Benutzeroberfläche für jede Ansicht des viewcontrollers mithilfe des xamarin IOS-Designers.

Wir möchten a `Label` und ein `Button` auf ViewController1 aus der **Toolbox** auf der rechten Seite ziehen. Als nächstes verwenden wir die Eigenschaftenpad, um den Namen und den Text der Steuerelemente wie folgt zu bearbeiten:

- **Bezeichnung** : `Text`  =  **eins**
- **Schaltfläche** : `Title`  =  **Benutzer nimmt zunächst eine Aktion** vor

Wir steuern die Sichtbarkeit unserer Schaltfläche in einem `TouchUpInside` -Ereignis, und wir müssen im Code Behind darauf verweisen. Wir identifizieren ihn mit dem **Namen** `aButton` im Eigenschaftenpad, wie im folgenden Screenshot dargestellt:

[![Legen Sie den Namen im Eigenschaftenpad auf "aButton" fest.](creating-tabbed-applications-images/abutton-properties.png)](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Die Designoberfläche sollte nun in etwa wie im folgenden Screenshot aussehen:

[![Ihr Designoberfläche sollte nun in etwa wie in diesem Screenshot aussehen.](creating-tabbed-applications-images/design-surface1.png)](creating-tabbed-applications-images/design-surface1.png#lightbox)

Fügen Sie ein paar weitere Details zu `ViewController2` und hinzu `ViewController3` , indem Sie jeweils eine Bezeichnung hinzufügen und den Text in "Two" bzw. "Three" ändern. Dadurch wird der Benutzer hervorgehoben, welche Registerkarte/Ansicht wir betrachten.

#### <a name="wiring-up-the-button"></a>Die Schaltfläche wird verknüpft.

`ViewController1`Beim ersten Start der Anwendung werden wir geladen. Wenn der Benutzer auf die Schaltfläche tippt, blenden Sie die Schaltfläche aus, und laden Sie eine `UITabBarController` mit der `ViewController1` Instanz auf der ersten Registerkarte.

Wenn der Benutzer das- `aButton` Ereignis freigibt, soll ein touchupinside-Ereignis ausgelöst werden. Wählen Sie die Schaltfläche aus, und deklarieren Sie auf der **Registerkarte Ereignisse** des eigenschaftenpad den Ereignishandler – `InitialActionCompleted` –, damit auf ihn im Code verwiesen werden kann. Dies wird im folgenden Screenshot veranschaulicht:

[![Wenn der Benutzer den aButton freigibt, wird ein touchupinside-Ereignis auslöst.](creating-tabbed-applications-images/event-handler.png)](creating-tabbed-applications-images/event-handler.png#lightbox)

Wir müssen dem Ansichts Controller nun mitteilen, dass die Schaltfläche ausgeblendet wird, wenn das Ereignis ausgelöst wird `InitialActionCompleted` . `ViewController1`Fügen Sie in die folgende partielle Methode hinzu:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Speichern Sie die Datei, und führen Sie die Anwendung aus. Der Bildschirm wird angezeigt, und die Schaltfläche wird bei der einschaltfläche ausgeblendet.

#### <a name="adding-the-tab-bar-controller"></a>Hinzufügen des Registerkarten leisten-Controllers

Unsere anfängliche Ansicht funktioniert jetzt erwartungsgemäß. Als nächstes möchten wir ihn zu einem `UITabBarController` zusammen mit den Sichten 2 und 3 hinzufügen. Öffnen Sie das Storyboard im Designer.

Suchen Sie in der **Toolbox**unter Controller & Objekte nach dem Registerkarten leisten **Controller** , und ziehen Sie diesen auf die Designoberfläche. Wie Sie im nachfolgenden Screenshot sehen können, ist der Registerkarten-Steuerelemente auf der Benutzeroberfläche kleiner und führt daher standardmäßig zwei Ansichts Controller ein:

[![Hinzufügen eines Registerkarten leisten-Controllers zum Layout](creating-tabbed-applications-images/tabbarcontroller.png)](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Löschen Sie diese neuen Ansichts Controller, indem Sie die schwarze Leiste unten auswählen und auf Löschen klicken.

In unserem Storyboard können wir die Übergänge zwischen tabbarcontroller und unseren Ansichts Controllern verwenden. Nachdem Sie mit der ersten Ansicht interagiert haben, möchten wir Sie in den tabbarcontroller laden, der dem Benutzer angezeigt wird. Wir legen dies im Designer fest.

Drücken **Sie die STRG-** Taste, und **ziehen** Sie Sie von der Schaltfläche auf tabbarcontroller. Bei einem Mausklick wird ein Kontextmenü angezeigt. Wir möchten einen modalen "*" verwenden.

Zum Einrichten der einzelnen Registerkarten **Klicken Sie mit der STRG-** Taste auf die einzelnen Ansichts Controller, und wählen Sie die **Registerkarte** Beziehung aus dem Kontextmenü aus, wie unten dargestellt:

[![Registerkarte "Beziehung" auswählen](creating-tabbed-applications-images/context-menu.png)](creating-tabbed-applications-images/context-menu.png#lightbox)

Ihr Storyboard sollte in etwa wie im folgenden Screenshot aussehen:

[![Das Storyboard sollte diesem Screenshot ähneln.](creating-tabbed-applications-images/segue-layout.png)](creating-tabbed-applications-images/segue-layout.png#lightbox)

Wenn Sie auf eine der Registerkarten Elemente klicken und den Eigenschaften Bereich Durchsuchen, sehen Sie eine Reihe verschiedener Optionen, wie unten dargestellt:

[![Festlegen der Registerkarten Optionen im Eigenschaften-Explorer](creating-tabbed-applications-images/properties-panel.png)](creating-tabbed-applications-images/properties-panel.png#lightbox)

Wir können dies verwenden, um bestimmte Attribute, wie z. b. das Badge, den Titel und den IOS-Bezeichner, zu bearbeiten.

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

[![Ausgabe der Beispiel-App](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie eine `UITabBarController` in einer-Anwendung verwenden. Wir haben das Laden von Controllern in jede Registerkarte und das Festlegen von Eigenschaften auf Registerkarten wie Titel, Bild und Badge erläutert. Anschließend wird mithilfe von Storyboards überprüft, wie ein `UITabBarController` zur Laufzeit geladen wird, wenn es sich nicht um das `RootViewController` des Fensters handelt.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Anwendungen im Registerkarten Format (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Uitabbarcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
