---
title: Registerkartenleisten und Registerkartenleisten-Controller in Xamarin.iOS
description: Dieses Dokument beschreibt die iOS-Registerkarte Leiste Controller und wie sie mit Xamarin.iOS verwendet. Es wird veranschaulicht, wie eine UITabBarController einrichten, arbeiten mit Bildern, Badge-Werten, arbeiten mit Ereignissen und mehr festlegen.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 617c1a15dd316ebd4a107d420841a83d4ca1fd6d
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200004"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Registerkartenleisten und Registerkartenleisten-Controller in Xamarin.iOS

Im Registerkartenformat Anwendungen werden in iOS verwendet, zur Unterstützung von Benutzeroberflächen, in denen mehrere Bildschirme zugegriffen werden kann, ohne bestimmte Reihenfolge. Durch die `UITabBarController` -Klasse, Anwendungen können ganz einfach umfassen die Unterstützung für solche Szenarien mit mehreren Bildschirm. `UITabBarController` übernimmt die multiscreen-Verwaltung, die durch die Entwickler der Anwendung auf die Details der einzelnen Bildschirme konzentrieren.

Im Registerkartenformat Anwendungen basieren in der Regel mit der `UITabBarController` wird die `RootViewController` des Hauptfensters. Allerdings können mit etwas zusätzlichem Code, im Registerkartenformat Anwendungen auch in Folge zu einigen anderen ersten Bildschirm, wie z. B. das Szenario verwendet werden, bei der eine Anwendung zunächst einen Anmeldebildschirm, gefolgt von der Schnittstelle im Registerkartenformat präsentiert.

Mithilfe von Registerkarten hier durch das Arbeiten über eine exemplarische Vorgehensweise für eine einfache Anwendung untersucht. Klicken Sie dann, betrachten wir das Arbeiten mit Registerkarten in anderen `RootViewController` Szenario.

## <a name="introducing-uitabbarcontroller"></a>Einführung in UITabBarController

Die `UITabBarController` unterstützt im Registerkartenformat Anwendungsentwicklung durch Folgendes:

- So können mehrere Controller hinzugefügt werden.
- Bereitstellen einer Benutzeroberfläche mit Registerkarten, über die `UITabBar` -Klasse, damit der Benutzer zwischen Controllern und ihre Ansichten wechseln kann. 


Controller werden hinzugefügt, um die `UITabBarController` über seine `ViewControllers` Eigenschaft, die eine `UIViewController` Array. Die `UITabBarController` selbst kümmert sich um den entsprechenden Controller laden und präsentieren die Ansicht auf Grundlage der ausgewählten Registerkarte.

Die Registerkarten werden Instanzen der `UITabBarItem` -Klasse, die in enthaltenen eine `UITabBar` Instanz. Jede `UITabBar` Instanz ist über die `TabBarItem` -Eigenschaft des Controllers auf jeder Registerkarte.

Um einen Überblick über die Arbeit mit erhalten die `UITabBarController`, lassen Sie durch die Erstellung einer einfachen Anwendung, die einen verwendet.

## <a name="tabbed-application-walkthrough"></a>Im Registerkartenformat Anwendung Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise werden wir die folgende Anwendung zu erstellen:

[![](creating-tabbed-applications-images/00-app.png "Beispiel-app-Registerkarten")](creating-tabbed-applications-images/00-app.png#lightbox)

Obwohl es bereits eine Anwendungsvorlage im Registerkartenformat in Visual Studio für Mac, in diesem Beispiel verfügbar ist, werden wir arbeiten von einem leeren Projekt erhalten einen besseren Überblick darüber, wie die Anwendung erstellt wird.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>Erstellen der Anwendung

Wir erstellen zunächst eine neue Anwendung.

Wählen Sie die **Datei > Neu > Projektmappe** Menüelement in Visual Studio für Mac, und wählen eine **iOS > App > leeres Projekt** Vorlage, nennen Sie das Projekt `TabbedApplication`, wie unten dargestellt:

[![](creating-tabbed-applications-images/newsolution1.png "Wählen Sie die leere Projektvorlage")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Nennen Sie das Projekt TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>Hinzufügen der UITabBarController

Fügen Sie als nächstes eine leere Klasse hinzu, indem Sie **Datei > neue Datei** auswählen und die **Option Allgemein auswählen: Leere Klassen** Vorlage. Nennen Sie die Datei `TabController` wie unten dargestellt:

[![](creating-tabbed-applications-images/02-newclass.png "Fügen Sie die TabController-Klasse")](creating-tabbed-applications-images/02-newclass.png#lightbox)

Die `TabController` Klasse enthält die Implementierung der `UITabBarController` verwalten, die ein Array von `UIViewControllers`. Bei der Auswahl einer Registerkarte für die `UITabBarController` kümmert sich die Ansicht für den entsprechenden ansichtscontroller darstellen.

Zum Implementieren der `UITabBarController` müssen wir die folgenden Aktionen ausführen:

1. Legen Sie die Basisklasse der `TabController` zu `UITabBarController` . 
1. Erstellen Sie `UIViewController` Instanzen hinzugefügt der `TabController` . 
1. Hinzufügen der `UIViewController` Instanzen, um ein Array zugewiesen der `ViewControllers` Eigenschaft der `TabController` . 


Fügen Sie den folgenden Code der `TabController` Klasse folgendermaßen erreichen:

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

Beachten Sie, dass für jede `UIViewController` Instanz, legen wir die `Title` Eigenschaft der `UIViewController`. Wenn die Controller hinzugefügt werden die `UITabBarController`, `UITabBarController` liest die `Title` für jeden Controller und zeigen Sie es in der entsprechenden Registerkarte Bezeichnung aus, wie unten dargestellt:

[![](creating-tabbed-applications-images/00-app.png "Führen Sie die Beispiel-app")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Legen Sie die TabController als die RootViewController

Die Reihenfolge an, dass die Controller, auf den Registerkarten platziert werden entspricht der Reihenfolge, die sie hinzugefügt werden die `ViewControllers` Array.

Zum Abrufen der `UITabController` um als ersten Bildschirm zu laden, müssen wir ihn des Fensters, `RootViewController`, wie im folgenden Code dargestellt die `AppDelegate`:

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

Wenn wir die Anwendung jetzt ausführen der `UITabBarController` lädt mit der ersten Registerkarte, die standardmäßig aktiviert. Wenn Sie einen der anderen Registerkarten in der zugeordneten Controllers Ergebnisansicht wird präsentiert von der `UITabBarController,` wie unten, in dem der Endbenutzer auf der zweite Registerkarte ausgewählt hat:

[![](creating-tabbed-applications-images/03-secondtab.png "Die zweite Registerkarte angezeigt")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>TabBarItems ändern

Nun, wir eine laufende Anwendung Registerkarte haben, ändern wir die `TabBarItem` so ändern Sie das Bild und Text, der angezeigt wird, als auch einen Badge auf eine der Registerkarten hinzufügen.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>Ein System-Element festlegen

Zunächst legen wir die erste Registerkarte enthält ein System-Element verwenden. Im Konstruktor des der `TabController`, entfernen Sie die Zeile, die des Controllers festlegt `Title` für die `tab1` -Instanz, und Ersetzen Sie ihn durch den folgenden Code zum Festlegen des Controllers `TabBarItem` Eigenschaft:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Beim Erstellen der `UITabBarItem` mithilfe einer `UITabBarSystemItem`, den Titel und das Bild automatisch bereitgestellt unter iOS, wie im Screenshot unten zeigt die **Favoriten** Symbol und dem Titel der ersten Registerkarte:

 ![](creating-tabbed-applications-images/04a-tabimage.png "Die erste Registerkarte mit einem Sternsymbol")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>Legen Sie den Titel und Image

Zusätzlich zur Verwendung einer System-Element, den Titel und die Abbildung des eine `UITabBarItem` auf benutzerdefinierte Werte festgelegt werden kann. Ändern Sie z. B. den Code, der festlegt der `TabBarItem` -Eigenschaft des Controllers mit dem Namen `tab2` wie folgt:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Der obige Code geht davon aus, ein Image namens `second.png` in das Stammverzeichnis des Projekts in Visual Studio für Mac hinzugefügt wurde Wir haben drei Images tatsächlich für unser Projekt, um Lösungen für alle Geräte, abzudecken, wie unten dargestellt hinzugefügt:

 [![](creating-tabbed-applications-images/tabbedimages7new.png "Die Bilder dem Projekt hinzugefügt")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

Die Registerkarte Bild muss eine 30 x 30 PNG-Datei mit Transparenz für die normale Auflösung 60 x 60 für hohe Auflösung und 90 x 90 für iPhone 6 Plus Auflösung. In unserem Code nur muss beim Laden der Datei mit dem Namen `second.png` und iOS wird automatisch geladen, die hohe Auflösung auf Geräten mit einem Retina-Display. Weitere Informationen finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Anleitungen. Standardmäßig sind die Elemente der Registerkarte grau, mit einem blauen Farbton, wenn ausgewählt.

**Hinweis**

Der oben genannten Images können auch hinzugefügt werden, um die **Ressourcen** Verzeichnis, das eines speziellen Verzeichnisses ist, deren Inhalt automatisch in das Stammverzeichnis des Anwendungspaket kopiert werden werden:

[![](creating-tabbed-applications-images/tabbedapplication8.png "Die Bilder als Ressourcen")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

Wenn wir darüber hinaus Festlegen der `Title` Eigenschaft direkt auf die `TabBarItem`, würden sie einen beliebigen Wert festgelegt für `Title` auf dem Controller selbst.

Wenn die Anwendung nun ausgeführt wird, zeigt die zweite Registerkarte unserer benutzerdefinierten Titel und das Bild aus, wie unten dargestellt:

[![](creating-tabbed-applications-images/05-customtab.png "Die zweite Registerkarte mit einem quadratischen Symbol")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>Festlegen des Werts Badge

Eine Registerkarte kann auch einen Badge anzeigen. Fügen Sie z.B. Code aus, um einen Badge auf der dritten Registerkarte die folgende Zeile hinzu:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Dies Ausführung erzeugt eine rote Beschriftung mit der Zeichenfolge "Hi" in links oben auf der Registerkarte wie folgt:

[![](creating-tabbed-applications-images/06-badge.png "Die zweite Registerkarte mit einer Hi-badge")](creating-tabbed-applications-images/06-badge.png#lightbox)

Der Badge wird häufig zum Anzeigen einer Anzahl Angabe ungelesene, neue Elemente. Um den Badge zu entfernen, legen die `BadgeValue` auf null fest, wie unten dargestellt:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Registerkarten in nicht-RootViewController-Szenarien

Im obigen Beispiel haben wir arbeiten mit einem `UITabBarController` Wenn es sich um die `RootViewController` des Fensters. In diesem Beispiel untersuchen wir mit einem `UITabBarController` wird er nicht die `RootViewController` und verwenden Sie Storyboards anzeigen, wie diese erstellt wird.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>Startbildschirm-Beispiel

In diesem Szenario wird auf dem Startbildschirm von einem Controller, der nicht lädt eine `UITabBarController`. Wenn der Benutzer mit dem Bildschirm durch Tippen auf eine Schaltfläche interagiert, wird der gleiche Ansichtscontroller geladen werden, in eine `UITabBarController`, der dann an den Benutzer präsentiert wird. Der folgende Screenshot zeigt den anwendungsfluss:

[![](creating-tabbed-applications-images/inital-screen-application.png "Dieser Screenshot zeigt den anwendungsfluss")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Beginnen Sie eine neue Anwendung für dieses Beispiel an. In diesem Fall verwenden wir die **iPhone > App > leeres Projekt (C#)** Vorlage diesmal das Projekt `InitialScreenDemo`.


In diesem Beispiel benötigen wir ein Storyboard aus, um unsere View-Controller zu speichern. So fügen Sie ein Storyboard hinzu:

- Mit der rechten Maustaste auf den Projektnamen, und wählen **hinzufügen > neue Datei**.

- Wenn das Dialogfeld "neue Datei" angezeigt wird, navigieren Sie zu **iOS > leeres iPhone-Storyboard**.

Wir nennen das neue Storyboard **MainStoryboard** , wie unten gezeigt: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Fügen Sie dem Projekt eine MainStoryboard-Datei")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Es gibt einige wichtige Schritte beachten Sie beim Hinzufügen eines Storyboards in eine zuvor nicht-Storyboard-Datei, die beschrieben werden die [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) Guide. Diese lauten wie folgt:

 
1. Fügen Sie den Namen Ihres Storyboards, um die **Hauptschnittstelle** Teil der `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Legen Sie die Hauptkomponente der Benutzeroberfläche auf MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. In Ihrer `App Delegate`, überschreiben Sie die Fenster-Methode, mit dem folgenden Code:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

Wir werden drei Ansichtscontroller für dieses Beispiel benötigen. Eine, die mit dem Namen `ViewController1`, werden als unsere Initial View-Controller und in der ersten Registerkarte verwendet werden. Die beiden anderen, mit dem Namen `ViewController2` und `ViewController3`, der verwendet wird auf den zweiten und dritten Registerkarten entsprechend.

Öffnen Sie den Designer, indem Sie auf die MainStoryboard.storyboard-Datei doppelklicken, und ziehen Sie drei View-Controller auf die Entwurfsoberfläche. Soll jede von diesen View-Controller auf ihrer eigenen Klasse für den oben genannten Namen haben in diesem Fall unter **Identität > Klasse**, geben Sie in ihrem Namen, wie im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/class-name.png "Legen Sie die Klasse auf ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio für Mac generiert automatisch die Klassen und Designer-Dateien erforderlich, dies ist in der Projektmappe, wie unten gezeigt:

[![](creating-tabbed-applications-images/solution-pad2.png "Automatisch generierte Dateien im Projekt")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>Erstellen der Benutzeroberfläche

Als Nächstes erstellen eine einfache Benutzeroberfläche für jede ViewControllers Ansichten, wir mit dem Xamarin.IOS-Designer.

Wir möchten, ziehen eine `Label` und `Button` auf ViewController1 aus der **ToolBox** auf der rechten Seite. Als Nächstes verwenden wir das Pad "Eigenschaften", so bearbeiten Sie den Namen und den Text der Steuerelemente auf Folgendes:

- **Bezeichnung** : `Text` = **Einseitig**
- **Schaltfläche** : `Title` = **Benutzer nimmt einige anfängliche Aktionen vor**


Wir steuern die Sichtbarkeit der Schaltfläche in einem `TouchUpInside` -Ereignis, und wir müssen in der CodeBehind darauf verweisen. Wir identifizieren sie mit der **Namen** `aButton` in das Pad "Eigenschaften", wie im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/abutton-properties.png "Legen Sie den Namen aButton im Bereich \"Eigenschaften\"")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Ihrer Entwurfsoberfläche sollte nun ähnlich wie im folgenden Screenshot aussehen:

[![](creating-tabbed-applications-images/design-surface1.png "Ihrer Entwurfsoberfläche sollte jetzt etwa so aussehen.")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Fügen Sie ein wenig detaillierter auf `ViewController2` und `ViewController3`, indem Sie eine Bezeichnung auf die einzelnen hinzufügen und ändern den Text bzw. in 'Zwei' und 'Drei'. Dies hebt hervor, welche Registerkarte/Ansicht, wir betrachten, die dem Benutzer.

#### <a name="wiring-up-the-button"></a>Verknüpfen Sie die Schaltfläche "

Wir wollen laden `ViewController1` beim ersten Starten der Anwendung. Wenn der Benutzer die Schaltfläche tippt, wir Ausblenden der Schaltfläche und Laden eine `UITabBarController` mit der `ViewController1` -Instanz in der ersten Registerkarte.

Wenn der Benutzer gibt die `aButton`, ein TouchUpInside-Ereignis ausgelöst werden soll. Wählen Sie die Schaltfläche, und klicken Sie in der **Registerkarte "Ereignisse"** des eigenschaftenpads, deklarieren Sie den Ereignishandler – `InitialActionCompleted` – damit sie im Code verwiesen werden kann. Dies ist im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/event-handler.png "Wenn der Benutzer die aButton freigibt, Auslösen eines Ereignisses TouchUpInside")](creating-tabbed-applications-images/event-handler.png#lightbox)

Wir müssen jetzt den View-Controller zum Ausblenden der Schaltfläche, wenn das Ereignis auslöst mitteilen `InitialActionCompleted`. In `ViewController1`, fügen Sie die folgende partielle Methode hinzu:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Speichern Sie die Datei, und führen Sie die Anwendung. Es sollte sich Bildschirm eines angezeigt werden und die Schaltfläche mit den berühren Sie nicht mehr angezeigt werden.

#### <a name="adding-the-tab-bar-controller"></a>Hinzufügen der Registerkartenleiste Controller

Wir haben jetzt unsere erste Ansicht wie erwartet funktioniert. Als Nächstes hinzufügen soll eine `UITabBarController`, sowie Ansichten 2 und 3. Öffnen Sie das Storyboard wir im Designer.

In der **Toolbox**, suchen Sie nach der **Registerkartenleistencontroller** unter Controller & Objekte, und ziehen Sie dies auf die Entwurfsoberfläche. Wie im folgenden Screenshot sehen ist, wird die Registerkartenleistencontroller UI-lose und aus diesem Grund bietet zwei Ansichtscontrollern dabei standardmäßig:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Das Layout hinzugefügt einer Registerkartenleistencontroller")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Löschen Sie diese neue Ansicht-Controller, indem die schwarze Leiste am unteren Rand, und drücken ENTF.

In unserem Storyboards können wir Segues verwenden, um die Übergänge zwischen den TabBarController und unsere View-Controller zu verarbeiten. Nach dem Arbeiten mit die anfängliche Ansicht, möchten wir sie in der dem Benutzer angezeigten TabBarController laden. Richten wir dies im Designer.

**Strg + Klick** und **Drag** über die Schaltfläche, um die TabBarController. In der Maustaste oben wird ein Kontextmenü angezeigt. Eine modale Segue verwendet werden soll. 
 
Unsere Registerkarten, einrichten **Strg + Klick** aus der TabBarController aller unserer View-Controller in der Reihenfolge von einer bis zu drei, und wählen Sie die Beziehung **Registerkarte** aus dem Kontextmenü aus, wie unten gezeigt:

[![](creating-tabbed-applications-images/context-menu.png "Wählen Sie die Registerkarte \"-Beziehung")](creating-tabbed-applications-images/context-menu.png#lightbox)

Ihr Storyboard sollte wie im folgenden Screenshot aussehen:

[![](creating-tabbed-applications-images/segue-layout.png "Das Storyboard sollten diesem Screenshot entsprechen.")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Wenn wir klicken Sie auf eines der Elemente der Registerkarte und den Eigenschaftenbereich untersuchen, sehen Sie eine Reihe von verschiedenen Optionen, wie unten gezeigt:

[![](creating-tabbed-applications-images/properties-panel.png "Festlegen der Optionen auf der Registerkarte im Eigenschaften-Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Wir können dies verwenden, um bestimmte Attribute wie den Badge, den Titel und den iOS-bearbeiten [Bezeichner](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), u. a.

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

[![](creating-tabbed-applications-images/first-view.png "Die Beispiel-app-Ausgabe")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie Sie verwenden eine `UITabBarController` in einer Anwendung. Wir durchgearbeitet Laden Sie die Controller in jede Registerkarte sowie Festlegen von Eigenschaften auf Registerkarten solche Titel, Bild und Badge. Wir dann untersucht, mit Storyboards, das Laden einer `UITabBarController` zur Laufzeit, wenn es nicht die `RootViewController` des Fensters.


## <a name="related-links"></a>Verwandte Links

- [Anwendungserstellung im Registerkartenformat (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
