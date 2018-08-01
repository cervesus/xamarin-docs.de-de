---
title: Auf der Registerkarte Balken und Registerkartenleiste Controller im Xamarin.iOS
description: Dieses Dokument beschreibt iOS Registerkarte Leiste Controller und wie sie mit Xamarin.iOS verwendet. Es zeigt, wie eine UITabBarController einrichten, arbeiten mit Bildern, Badge Werte und Arbeiten mit Ereignissen festgelegt.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: d8b096774e60ec0e0b69e109fa5da53c25e66d25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789757"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Auf der Registerkarte Balken und Registerkartenleiste Controller im Xamarin.iOS

Im Registerkartenformat Anwendungen werden in iOS verwendet, zur Unterstützung von Benutzeroberflächen, in denen mehrere Bildschirme zugegriffen werden kann, ohne bestimmte Reihenfolge. Über die `UITabBarController` -Klasse, Unterstützung für solche Szenarien mit mehreren Bildschirm problemlos zu Anwendungen integrieren können. `UITabBarController` übernimmt der Multi-Bildschirm-Verwaltung, sodass des Anwendungsentwicklers die Details jeder Bildschirm konzentrieren.

In der Regel im Registerkartenformat erstellten Anwendungen die `UITabBarController` wird die `RootViewController` des Hauptfensters. Allerdings können mit etwas zusätzlicher Code, im Registerkartenformat Anwendungen werden auch nacheinander einigen anderen ersten Bildschirm, z. B. das Szenario verwendet, bei eine Anwendung auf einen Anmeldebildschirm, gefolgt von der Schnittstelle im Registerkartenformat präsentiert.

Untersucht, mithilfe von Registerkarten hier entwickeln, indem über eine exemplarische Vorgehensweise für eine einfache Anwendung. Klicken Sie dann, betrachten wir arbeiten mit Registerkarten in nicht- `RootViewController` Szenario.

## <a name="introducing-uitabbarcontroller"></a>Einführung in UITabBarController

Die `UITabBarController` unterstützt als die Anwendungsentwicklung durch die folgenden Registerkarten:

-  Zulassen, dass mehrere Controller hinzugefügt werden.
-  Bereitstellen von einer im Registerkartenformat Benutzeroberfläche, über die `UITabBar` -Klasse, um einem Benutzer ermöglichen, die zwischen Controller und ihre Ansichten zu wechseln. 


Controller hinzugefügt werden die `UITabBarController` über seine `ViewControllers` Eigenschaft, die eine `UIViewController` Array. Die `UITabBarController` selbst behandelt, laden den entsprechenden Controller und präsentieren die Ansicht auf Grundlage der ausgewählten Registerkarte.

Registerkarten sind Instanzen der `UITabBarItem` -Klasse, die in enthaltenen eine `UITabBar` Instanz. Jede `UITabBar` Instanz Zugriff erfolgt über die `TabBarItem` Eigenschaft des Controllers auf jeder Registerkarte.

Um ein Verständnis dafür zu zum Arbeiten mit der `UITabBarController`, wir die einzelnen Schritte beim Erstellen einer einfachen Anwendung, die einen verwendet.

## <a name="tabbed-application-walkthrough"></a>Im Registerkartenformat Anwendung Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise werden wir die folgende Anwendung zu erstellen:

[![](creating-tabbed-applications-images/00-app.png "Im Registerkartenformat Beispiel-app")](creating-tabbed-applications-images/00-app.png#lightbox)

Obwohl eine Vorlage im Registerkartenformat Anwendung bereits in Visual Studio für Mac, in diesem Beispiel verfügbar ist, werden wir arbeitsmaterial auf ein leeres Projekt erhalten Sie einen besseren Überblick darüber, wie die Anwendung erstellt wird.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>Erstellen der Anwendung

Beginnen wir, indem Sie eine neue Anwendung erstellen.

Wählen Sie die **Datei > Neu > Projektmappe** Menüelement in Visual Studio für Mac, und wählen eine **iOS > App > leeres Projekt** Vorlage, nennen Sie das Projekt `TabbedApplication`, wie unten dargestellt:

[![](creating-tabbed-applications-images/newsolution1.png "Wählen Sie die leere Projektvorlage")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Nennen Sie das Projekt TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>Hinzufügen der UITabBarController

Fügen Sie eine leere Klasse dazu **Datei > neue Datei** auswählen und die **Allgemein: leere Klasse** Vorlage. Nennen Sie die Datei `TabController` wie unten dargestellt:

[![](creating-tabbed-applications-images/02-newclass.png "Fügen Sie die TabController-Klasse")](creating-tabbed-applications-images/02-newclass.png#lightbox)

Die `TabController` Klasse enthält die Implementierung der `UITabBarController` verwalten, die ein Array von `UIViewControllers`. Bei der Auswahl einer Registerkarte für die `UITabBarController` zur Darstellung der Ansicht für den entsprechenden View-Controller von übernimmt.

Zum Implementieren der `UITabBarController` müssen wir die folgenden Aktionen ausführen:

1.  Legen Sie die Basisklasse der `TabController` auf `UITabBarController` . 
1.  Erstellen Sie `UIViewController` -Instanzen, die zum Hinzufügen der `TabController` . 
1.  Hinzufügen der `UIViewController` -Instanzen, die ein Array zugewiesen der `ViewControllers` Eigenschaft von der `TabController` . 


Fügen Sie folgenden Code, der `TabController` Klasse, um diese Schritte zu erreichen:

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

Beachten Sie, dass für jede `UIViewController` Instanz, legen wir den `Title` Eigenschaft von der `UIViewController`. Wenn die Controller hinzugefügt werden die `UITabBarController`, die `UITabBarController` liest die `Title` für jeden Controller und zeigen Sie es in der entsprechenden Registerkarte Bezeichnung aus, wie unten dargestellt:

[![](creating-tabbed-applications-images/00-app.png "Führen Sie die Beispiel-app")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Festlegen der TabController als die RootViewController

Die Reihenfolge an, dass die Controller, auf den Registerkarten platziert werden entspricht der Reihenfolge, die sie hinzugefügt werden die `ViewControllers` Array.

Zum Abrufen der `UITabController` um als ersten Bildschirm zu laden, müssen wir sie des Fensters machen `RootViewController`, wie im folgenden Code dargestellt die `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

Wenn wir die Anwendung jetzt ausführen der `UITabBarController` wird geladen, mit der ersten Registerkarte standardmäßig ausgewählt. Auswählen einer der anderen Registerkarten die Ergebnisse im zugeordneten Controllers anzeigen angezeigt wird, indem Sie die `UITabBarController,` wie unten, in dem der Endbenutzer die zweite Registerkarte ausgewählt wurde:

[![](creating-tabbed-applications-images/03-secondtab.png "Die zweite Registerkarte angezeigt")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>Ändern von TabBarItems

Nun, da wir haben eine laufende Anwendung Registerkarte, die wir ändern die `TabBarItem` so ändern Sie das Bild und Text, der angezeigt wird, als auch ein Signal einer der Registerkarten hinzu.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>Festlegen einer System-Element

Zunächst legen Sie die erste Registerkarte enthält ein System-Element verwenden. Im Konstruktor der der `TabController`, entfernen Sie die Zeile, die des Controllers legt `Title` für die `tab1` Instanz, und Ersetzen Sie ihn durch den folgenden Code hinzu, legen Sie des Controllers `TabBarItem` Eigenschaft:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Beim Erstellen der `UITabBarItem` mithilfe einer `UITabBarSystemItem`, den Titel und das Bild automatisch bereitgestellt von iOS, wie im Screenshot unten zeigt die **Favoriten** Symbol "und" Title ", auf der ersten Registerkarte:

 ![](creating-tabbed-applications-images/04a-tabimage.png "Die erste Registerkarte mit einem Sternsymbol")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>Festlegen des Titels und Image

Zusätzlich zur Verwendung einer System-Element, den Titel und Image von einem `UITabBarItem` kann auf benutzerdefinierte Werte festgelegt werden. Beispielsweise ändern Sie den Code, der festlegt der `TabBarItem` Eigenschaft des Controllers an, mit dem Namen `tab2` wie folgt:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Der obige Code geht davon aus, ein Bild mit dem Namen `second.png` in das Stammverzeichnis des Projekts in Visual Studio für Mac hinzugefügt wurde Wir haben unsere-Projekt, um alle Geräte Lösungen abzudecken, wie unten dargestellt tatsächlich drei Bildern hinzugefügt:

 [![](creating-tabbed-applications-images/tabbedimages7new.png "Die Bilder dem Projekt hinzugefügt")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

Die Registerkarte Bild muss eine Png 30 x 30 Zwischenfarbe für normale Auflösung 60 x 60 für hochauflösende und 90 x 90 für iPhone 6 Plus Auflösung. In unserem Code wir brauchen nur beim Laden der Datei mit dem Namen `second.png` und iOS wird automatisch die hohe Auflösung auf Geräten mit einem Retina-Display zu laden. Weitere Informationen finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Handbücher. Standardmäßig sind die Leiste Registerkartenelemente grau, mit einem blauen Farbton beim aktiviert.

**Hinweis**

Konnte die oben genannten Bilder auch hinzugefügt werden die **Ressourcen** Datenverzeichnis, das ein bestimmtes Verzeichnis befindet, deren Inhalt automatisch in das Stammverzeichnis des Anwendungspaket kopiert werden wird:

[![](creating-tabbed-applications-images/tabbedapplication8.png "Die Bilder als Ressourcen")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

Wir darüber hinaus beim Festlegen der `Title` -Eigenschaft direkt unter der `TabBarItem`, überschreibt es Wert für `Title` auf dem Controller selbst.

Wenn wir die Anwendung jetzt ausführen, zeigt die zweite Registerkarte unsere benutzerdefinierte Titel und das Bild aus, wie unten dargestellt:

[![](creating-tabbed-applications-images/05-customtab.png "Die zweite Registerkarte mit einem Quadratisches Symbol")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>Festlegen der Signalwert

Eine Registerkarte kann ein Signal angezeigt werden. Fügen Sie z. B. die folgende Codezeile ein Signal für die dritte Registerkarte festlegen:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Bei Ausführen dieses ergibt sich eine rote Bezeichnung mit der Zeichenfolge "Hi" in der oberen linken Ecke der Registerkarte wie folgt:

[![](creating-tabbed-applications-images/06-badge.png "Die zweite Registerkarte mit einem Hi badge")](creating-tabbed-applications-images/06-badge.png#lightbox)

Das Signal wird häufig verwendet, um eine Zahl ungelesen Hinweis angezeigt, der neue Elemente. Um das Signal zu entfernen, legen Sie die `BadgeValue` auf null fest, wie unten dargestellt:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Registerkarten in nicht RootViewController-Szenarien

Im obigen Beispiel haben wir zum Arbeiten mit einem `UITabBarController` Wenn es sich um die `RootViewController` des Fensters. In diesem Beispiel untersuchen wir zum Verwenden einer `UITabBarController` wird er nicht den `RootViewController` und Storyboards verwenden, anzeigen, wie diese erstellt wird.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>Startbildschirm-Beispiel

In diesem Szenario wird der Startbildschirm von einem Controller, der nicht lädt eine `UITabBarController`. Wenn der Benutzer mit dem Bildschirm durch Tippen auf eine Schaltfläche interagiert, werden die gleichen View-Controller geladen werden, in eine `UITabBarController`, ist die klicken Sie dann dem Benutzer angezeigt. Der folgende Screenshot zeigt den Anwendungsablauf:

[![](creating-tabbed-applications-images/inital-screen-application.png "Diese bildschirmabbildung zeigt den Anwendungsablauf")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Beginnen Sie eine neue Anwendung für dieses Beispiel an. Wir verwenden wieder die **iPhone > App > leeres Projekt (c#)** Vorlage, diesmal das Projekt `InitialScreenDemo`.


In diesem Beispiel wird benötigen wir ein Storyboard mit unserem View-Controller zu halten. So fügen Sie ein Storyboard hinzu:

- Mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen > neuen Datei**.

- Wenn das Dialogfeld "neue Datei" angezeigt wird, wechseln Sie zu **iOS > leer iPhone Storyboard**.

Wir nennen diese neue Storyboard **MainStoryboard** , wie unten gezeigt: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Fügen Sie dem Projekt eine MainStoryboard-Datei")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Es gibt einige wichtige Schritte beachten Sie beim Hinzufügen eines Drehbuchs in eine zuvor nicht Storyboard-Datei, die beschrieben werden die [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) Handbuch. Diese lauten wie folgt:

 
1. Fügen Sie Ihr Name Storyboard der **Hauptbenutzeroberfläche** Teil der `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Legen Sie die Hauptkomponente der Benutzeroberfläche auf MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. In Ihrem `App Delegate`, überschreiben Sie die Fenster-Methode, durch den folgenden Code:

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

Wir werden drei View-Controller für dieses Beispiel benötigen. Dasjenige, mit dem Namen `ViewController1`, werden als unserer anfänglichen View-Controller und in der ersten Registerkarte verwendet werden. Die anderen beiden mit dem Namen `ViewController2` und `ViewController3`, die verwendet werden in der zweiten und dritten Registerkarten bzw.

Öffnen Sie den Designer, indem Sie auf die MainStoryboard.storyboard-Datei doppelklicken, und ziehen Sie drei View-Controller der Entwurfsoberfläche. Jede dieser Controller Ansicht ihrer eigenen Klasse entspricht dem oben genannten Namen haben soll dies der Fall ist, klicken Sie unter **Identität > Klasse**, geben Sie in ihrem Namen, wie im folgenden Screenshot gezeigt:

[![](creating-tabbed-applications-images/class-name.png "Legen Sie die Klasse den ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio für Mac generiert automatisch die Klassen und Designer-Dateien erforderlich, dies ist in das Auffüllzeichen Lösung wie unten gezeigt:

[![](creating-tabbed-applications-images/solution-pad2.png "Automatisch generierte Dateien im Projekt")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>Erstellen der Benutzeroberfläche

Als Nächstes erstellen eine einfache Benutzeroberfläche für jede der ViewController Ansichten, wir mit dem Xamarin iOS-Designer.

Wir möchten, ziehen eine `Label` und ein `Button` auf ViewController1 aus der **ToolBox** auf der rechten Seite. Als Nächstes verwenden wir das Auffüllzeichen Eigenschaften so bearbeiten Sie den Namen und den Text der Steuerelemente auf die folgenden:

-  **Bezeichnung** : `Text`  =  **eine**
-  **Schaltfläche** : `Title`  =  **Benutzer nimmt eine anfängliche Aktion**


Wir steuern die Sichtbarkeit der Schaltfläche in einem `TouchUpInside` Ereignis, und es müssen im Code-behind darauf verwiesen. Wir es mit identifiziert die **Namen** `aButton` in den Eigenschaften aufgefüllt, wie im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/abutton-properties.png "Legen Sie den Namen zu aButton in den Eigenschaften mit Leerstellen auffüllen")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Die Entwurfsoberfläche sollte jetzt mit dem folgenden Screenshot entsprechen:

[![](creating-tabbed-applications-images/design-surface1.png "Die Entwurfsoberfläche sollte nun ähnlich wie in diesem Screenshot aussehen.")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Fügen Sie ein wenig mehr Details `ViewController2` und `ViewController3`, indem Sie eine Bezeichnung hinzugefügt, und wenn der Text "Zwei" und "Drei" geändert. Dies unterstreicht, dem Benutzer die Registerkarte "/ Sicht Wir betrachten.

#### <a name="wiring-up-the-button"></a>Verknüpft die Schaltfläche ""

Wir werden laden `ViewController1` beim ersten Starten der Anwendung. Wenn der Benutzer die Schaltfläche tippt, das "ausblenden" und wir Laden einer `UITabBarController` mit der `ViewController1` -Instanz in der ersten Registerkarte.

Wenn der Benutzer lässt die `aButton`, ein TouchUpInside-Ereignis ausgelöst werden soll. Wir wählen Sie die Schaltfläche "", und klicken Sie in der **Registerkarte "Ereignisse"** Pad Eigenschaften deklarieren Sie den Ereignishandler – `InitialActionCompleted` – damit im Code verwiesen werden kann. Dies wird im folgenden Screenshot dargestellt:

[![](creating-tabbed-applications-images/event-handler.png "Wenn der Benutzer die aButton freigibt, Auslösen eines Ereignisses TouchUpInside")](creating-tabbed-applications-images/event-handler.png#lightbox)

Jetzt müssen wir erkennen die View-Controller auf die Schaltfläche ausgeblendet, wenn das Ereignis auslöst `InitialActionCompleted`. In `ViewController1`, fügen Sie die folgende partielle Methode hinzu:

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

Speichern Sie die Datei, und führen Sie die Anwendung. Wir sehen Bildschirm eines angezeigt werden und die Schaltfläche auf berühren sich nicht mehr angezeigt.

#### <a name="adding-the-tab-bar-controller"></a>Hinzufügen der Registerkartenleiste Controller

Wir haben jetzt unsere erste Ansicht wie erwartet funktioniert. Als Nächstes wir möchten sie eine `UITabBarController`, sowie Ansichten 2 und 3. Öffnen Sie das Storyboard wir im Designer.

In der **Toolbox**, suchen Sie nach der **Registerkarte Leiste Controller** unter Controller & Objekte, und ziehen Sie es auf der Entwurfsoberfläche angezeigt. Wie im folgenden Screenshot sehen, ist die Registerkarte Leiste Controller UI kleiner und schaltet Sie daher zwei View-Controller mit standardmäßig:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Hinzufügen einer Registerkarte Leiste Controller des Layouts")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Löschen Sie diese neue Ansicht Domänencontroller durch Auswählen der schwarzen Leiste am unteren und drücken ENTF.

In unserem Storyboard verwenden wir das Segues, um die Übergänge zwischen den TabBarController und unsere View-Controller zu behandeln. Nach der Interaktion mit der Erstansicht, möchten wir laden es in der TabBarController, die dem Benutzer angezeigt. Richten Sie dies im Designer.

**STRG + klicken** und **ziehen Sie** über die Schaltfläche, um die TabBarController. Auf MouseUp wird ein Kontextmenü angezeigt. Wir möchten ein modales Segue verwenden. 
 
So richten Sie unsere Registerkarten ein **STRG + klicken** aus der TabBarController auf die einzelnen unsere View-Controller in der Reihenfolge von eins bis drei, und wählen Sie die Beziehung **Registerkarte** aus dem Kontextmenü aus, wie unten gezeigt:

[![](creating-tabbed-applications-images/context-menu.png "Wählen Sie die Registerkarte \"-Beziehung")](creating-tabbed-applications-images/context-menu.png#lightbox)

Das Storyboard in etwa den folgenden Screenshot:

[![](creating-tabbed-applications-images/segue-layout.png "Das Storyboard in etwa diesen Screenshot mit")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Wenn wir klicken Sie auf eines der Elemente der Registerkarte Balken und untersuchen den Eigenschaftenbereich, sehen Sie eine Anzahl von aktuellen Position unterschiedliche Optionen, wie unten gezeigt:

[![](creating-tabbed-applications-images/properties-panel.png "Festlegen der Optionen auf der Registerkarte im Eigenschaften-Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Wir können Hiermit können Sie bestimmte Attribute wie z. B. das Signal, den Titel und die iOS bearbeiten [Bezeichner](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), u. a.

Wenn wir speichern und die Anwendung jetzt ausführen, finden wir, dass die Schaltfläche angezeigt werden soll, wenn die ViewController1-Instanz in die TabBarController geladen wird. Wir beheben dies überprüfen, um festzustellen, ob die aktuelle Ansicht ein übergeordnetes Element View Controller besitzt. Wenn dies der Fall, wissen wir sind innerhalb der TabBarController und daher die Schaltfläche ausgeblendet werden sollen. Fügen Sie den folgenden Code aus, auf die ViewController1-Klasse:

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

Wenn die Anwendung ausgeführt wird und der Benutzer die Schaltfläche auf dem ersten Bildschirm die UITabBarController tippt wird, mit der Ansicht aus dem ersten Bildschirm in der ersten Registerkarte platziert werden, wie unten dargestellt geladen:

[![](creating-tabbed-applications-images/first-view.png "Die Ausgabe der Beispiel-app")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt, wie eine `UITabBarController` in einer Anwendung. Wir durchlaufen, bis zum Laden von Domänencontrollern in jede Registerkarte sowie zum Festlegen von Eigenschaften auf Registerkarten solche Titel, Bild und Badge. Wir dann überprüft, Storyboards, zum Laden einer `UITabBarController` zur Laufzeit, wenn es nicht die `RootViewController` des Fensters.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Anwendungen im Registerkartenformat (Beispiel)](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
