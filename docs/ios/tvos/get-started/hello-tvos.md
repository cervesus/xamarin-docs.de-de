---
title: Hello, TvOS – Kurzanleitung
description: Dieser Leitfaden enthält Informationen zum Erstellen Ihrer ersten Xamarin.tvOS-app und die entwicklungstoolkette. Außerdem wird der Xamarin-Designer, stellt der Benutzeroberflächen-Steuerelemente für Code, und veranschaulicht, wie erstellen, ausführen und Testen Sie eine Xamarin.tvOS-Anwendung eingeführt.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 02/02/2018
ms.openlocfilehash: 859bbd22640ba3d09324fcd3853cda26e563a1cd
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865282"
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, TvOS – Kurzanleitung

_Dieser Leitfaden enthält Informationen zum Erstellen Ihrer ersten Xamarin.tvOS-app und die entwicklungstoolkette. Außerdem wird der Xamarin-Designer, stellt der Benutzeroberflächen-Steuerelemente für Code, und veranschaulicht, wie erstellen, ausführen und Testen Sie eine Xamarin.tvOS-Anwendung eingeführt._

Apple hat die 5. Generation von Apple TV, Apple TV 4 K, veröffentlicht der TvOS 11 ausgeführt wird.

Die Apple TV-Plattform ist für Entwickler, sodass sie umfassende, immersive apps erstellen und über die integrierte App Store von Apple TV freigeben.

Wenn Sie mit der Xamarin.iOS-Entwicklung vertraut sind, sollten Sie den Übergang tvos recht einfach suchen. Die meisten APIs und Features sind identisch, allerdings sind viele allgemeine APIs (z. B. WebKit) nicht verfügbar. Darüber hinaus arbeiten mit der mit dem Remoterepository Siri bringt Entwurf, die nicht in Touchscreen-basierte iOS-Geräte vorhanden sind.

Dieses Handbuch, erhalten eine Einführung in die Arbeit mit TvOS in einer Xamarin-app. Weitere Informationen unter TvOS finden Sie unter Apple [machen Sie sich bereit für Apple TV 4K](https://developer.apple.com/tvos/) Dokumentation.

## <a name="overview"></a>Übersicht

Xamarin.tvOS können Sie vollständig native Apple TV-apps in entwickeln C# und mit dem gleichen OS X-Bibliotheken und Steuerelemente der Benutzeroberfläche, die verwendet werden, bei der Entwicklung in .NET *Swift* (oder *Objective-C-* ) und  *Xcode*.

Darüber hinaus seit Xamarin.tvOS-apps geschrieben sind C# und .NET, verwendeter und Back-End-Code mit Xamarin.iOS, Xamarin.Android und Xamarin.Mac-apps freigegeben werden kann. und liefert so eine systemeigene Erfahrung auf jeder Plattform.

Dieser Artikel bietet eine Einführung in die grundlegenden Konzepte, die erforderlich sind, zum Erstellen einer Apple TV-App mithilfe von Xamarin.tvOS und Visual Studio werden Sie schrittweise durch den Prozess der Erstellung eines Grundlegendes **Hello, TvOS** -app, die die zählt, wie oft eine Schaltfläche verfügt. geklickt wurde:

[![](hello-tvos-images/run05.png "Beispiel-app-Ausführung")](hello-tvos-images/run05.png#lightbox)

Die folgenden Konzepte erläutert:

-  **Visual Studio für Mac** – Einführung in Visual Studio für Mac und wie Sie Xamarin.tvOS-Anwendungen schreiben können.
-  **Anatomie einer Xamarin.tvOS-App** – was ein Xamarin.tvOS-app besteht aus.
-  **Erstellen einer Benutzeroberfläche** – wie Sie Xamarin-Designer für iOS zu verwenden, um eine Benutzeroberfläche zu erstellen.
-  **Bereitstellung und Tests** – ausführen und Testen Sie Ihre app in die TvOS-Simulator und TvOS-real-Hardware.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Starten Sie eine neue Xamarin.tvOS-App in Visual Studio für Mac

Wie bereits erwähnt, erstellen wir eine Apple TV-app mit dem Namen `Hello-tvOS` , eine einzelne Schaltfläche und einer Bezeichnung zum Hauptbildschirm von addiert. Wenn Sie auf die Schaltfläche klicken, zeigt die Beschriftung an, wie oft sie bereits geklickt wurde.

Zunächst sehen wir wie folgt vor:

1. Starten Sie Visual Studio für Mac:

    [![](hello-tvos-images/setup01.png "Visual Studio für Mac")](hello-tvos-images/setup01.png#lightbox)
2. Klicken Sie auf die **neue Projektmappe...**  -Link in der oberen linken Ecke des Bildschirms, um das Öffnen der **neues Projekt** Dialogfeld.
3. Wählen Sie **TvOS** > **App** > **Einzelansicht-App** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](hello-tvos-images/setup02.png "Ein Einzelansicht-App auswählen")](hello-tvos-images/setup02.png#lightbox)
4. Geben Sie `Hello, tvOS` für die **Anwendungsnamen**, geben Sie Ihre **Organisations-ID** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](hello-tvos-images/setup04.png "Geben Sie Hello, tvOS")](hello-tvos-images/setup04.png#lightbox)
5. Geben Sie `Hello_tvOS` für die **Projektname** , und klicken Sie auf die **erstellen** Schaltfläche:

    [![](hello-tvos-images/setup03.png "Geben Sie HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Visual Studio für Mac erstellt die neue Xamarin.tvOS-app und zeigt die Standarddateien an, die mit Ihrer Anwendung Lösung hinzugefügt werden:

 [![](hello-tvos-images/project01.png "Die Standardansicht für Dateien")](hello-tvos-images/project01.png#lightbox)

Visual Studio für Mac verwendet **Lösungen** und **Projekte**, in Visual Studio ist genau die gleiche Weise. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthalten kann. Projekte sind z.B. Anwendungen, unterstützende Bibliotheken, Testanwendungen usw. In diesem Fall hat Visual Studio für Mac für Sie sowohl eine Projektmappe als auch ein Anwendungsprojekt erstellt.

Falls gewünscht, können Sie ein oder mehrere Library-Projekte erstellen, die gängigen freigegebenen Code enthalten. Diese Bibliotheksprojekte genutzt werden, indem Sie das Anwendungsprojekt oder andere Xamarin.tvOS-app-Projekte freigegeben werden können (oder Xamarin.iOS, Xamarin.Android und Xamarin.Mac basiert auf den Typ des Codes), wie wenn Sie eine Standard-.NET-Anwendung erstellen.

## <a name="anatomy-of-a-xamarintvos-app"></a>Anatomie einer Xamarin.tvOS-App

Wenn Sie mit iOS-Programmierung vertraut sind, werden Sie viele ähnlichkeiten hier feststellen. In der Tat ist TvOS 9 eine Teilmenge von iOS 9, daher hier sehr viele Konzepte werden.

Werfen wir einen Blick auf die Dateien im Projekt:

-   `Main.cs`: Diese Datei enthält den Haupteinstiegspunkt für die Anwendung. Wenn die App gestartet wird, enthält diese Datei die erste Klasse und Methode, die ausgeführt werden.
-   `AppDelegate.cs` : Diese Datei enthält die Hauptanwendungsklasse, die zum Überwachen von Ereignissen aus dem Betriebssystem verantwortlich ist.
-   `Info.plist` : Diese Datei enthält die Eigenschaften der Anwendung wie Anwendungsname, Symbole usw.
-   `ViewController.cs` – Dies ist die Klasse, die stellt das Hauptfenster und steuert den Lebenszyklus des Zertifikats.
-   `ViewController.designer.cs` : Diese Datei enthält Grundlagencode, der Ihnen die Integration in die Benutzeroberfläche des Hauptbildschirms hilft.
-  `Main.storyboard` – Die Benutzeroberfläche für das Hauptfenster. Diese Datei kann erstellt und von Xamarin für iOS Designer verwaltet werden.

In den folgenden Abschnitten Unternehmen wir einen kurzen Blick auf einige dieser Dateien. Werden wir sie später noch ausführlicher untersuchen, aber es ist eine gute Idee, die Grundlagen nun kennen.

### <a name="maincs"></a>Main.cs

Die `Main.cs` Datei enthält eine statische `Main` Methode erstellt eine neue Instanz der Xamarin.tvOS-app, und übergibt dem Namen der Klasse, die OS-Ereignis behandelt, was in unserem Fall ist die `AppDelegate` Klasse:

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

Die `AppDelegate.cs` -Datei enthält unsere `AppDelegate` -Klasse, die für das Fenster zu erstellen und das Lauschen auf OS-Ereignisse verantwortlich ist:

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

Dieser Code ist wahrscheinlich unbekannt, es sei denn, Sie haben eine iOS-Anwendung vor dem erstellt, aber es ziemlich einfach ist. Betrachten Sie die wichtigen Zeilen an.

Zunächst sehen wir uns ein an der Deklaration der Variablen auf Klassenebene:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

Die `Window` Eigenschaft bietet Zugriff auf das Hauptfenster. TvOS verwendet, so genannten der *Model View Controller* Schema (MVC). Für jedes erstellte Fenster (und die für viele andere Dinge in Windows), besteht in der Regel ein Controller, der für den Lebenszyklus des Fensters, z. B. anzeigen oder das Hinzufügen von neuer Ansichten (Steuerelementen) usw. zuständig ist.

Als Nächstes müssen wir die `FinishedLaunching` Methode. Diese Methode ausgeführt wird, nachdem die Anwendung instanziiert wurde, und er verantwortlich ist für die tatsächlich erstellen das Anwendungsfenster und beginnen mit dem Anzeigen der Ansicht darin. Da unsere app ein Storyboard verwendet, um die Benutzeroberfläche zu definieren, ist hier kein zusätzlicher Code erforderlich.

Es gibt viele andere Methoden, die in der Vorlage, z. B. bereitgestellt werden `DidEnterBackground` und `WillEnterForeground`. Diese können problemlos entfernt werden, wenn die Anwendungsereignisse nicht in Ihrer app verwendet werden.

### <a name="viewcontrollercs"></a>ViewController.cs

Die `ViewController` -Klasse ist das Hauptfenster Controller. Das bedeutet, dass es für den Lebenszyklus des Hauptfensters verantwortlich ist. Wir werden dies später im Detail untersuchen, vorerst nur werfen wir einen kurzen Blick auf diese:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Die Designerdatei für die Hauptfensterklasse ist zurzeit leer, aber es wird automatisch aufgefüllt, von Visual Studio für Mac während wir unserer Benutzeroberfläche mit dem iOS-Designer zu erstellen:

```csharp
using Foundation;

namespace HellotvOS
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Wir sind nicht in der Regel betrifft das Designer-Dateien, wie sie automatisch von Visual Studio für Mac verwaltet werden, und geben Sie nur den erforderlichen Grundlagencode, der Zugriff auf Steuerelemente ermöglicht, dass wir zu einem beliebigen Fenster hinzufügen oder in unserer Anwendung anzeigen.

Nun, wir haben unsere Xamarin.tvOS-app erstellt, und wir ein grundlegendes Verständnis seiner Komponenten haben, sehen wir uns die Benutzeroberfläche erstellen.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>Erstellen der Benutzeroberfläche

Sie müssen keine Xamarin-Designer für iOS verwenden, um die Benutzeroberfläche für Ihre Xamarin.tvOS-app zu erstellen, die Benutzeroberfläche kann erstellt werden, direkt aus C# Code jedoch den Rahmen dieses Artikels sprengen. Der Einfachheit halber wird der iOS-Designer verwendet zum Erstellen unserer Benutzeroberfläche den Rest dieses Tutorials.

Doppelklicken Sie damit beginnen, Ihre Benutzeroberfläche erstellen, lassen Sie uns auf die `Main.storyboard` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung in der iOS-Designer zu öffnen:

[![](hello-tvos-images/designer01.png "Die Datei „Main.storyboard“ in Projektmappen-Explorer")](hello-tvos-images/designer01.png#lightbox)

Dies sollte den Designer zu starten und wie folgt aussehen:

[![](hello-tvos-images/designer02.png "Der Designer")](hello-tvos-images/designer02.png#lightbox)

Weitere Informationen zu den iOS-Designer, und wie es funktioniert, finden Sie in der [Einführung in den Xamarin-Designer für iOS](~/ios/user-interface/designer/introduction.md) Guide.

Wir können nun das Hinzufügen von Steuerelementen auf der Entwurfsoberfläche unserer Xamarin.tvOS-App.

Führen Sie folgende Schritte aus:

1. Suchen Sie die **Toolbox**, die rechts neben der Entwurfsoberfläche angezeigt werden sollte:

    [![](hello-tvos-images/designer03.png "Der Toolbox")](hello-tvos-images/designer03.png#lightbox)

    Wenn Sie ihn hier nicht finden, navigieren Sie zum **Ansicht > Pads > Toolbox** um ihn anzuzeigen.
2. Ziehen Sie eine **Bezeichnung** aus der **Toolbox** auf die Entwurfsoberfläche:

    [![](hello-tvos-images/designer04.png "Ziehen Sie eine Bezeichnung aus der Toolbox")](hello-tvos-images/designer04.png#lightbox)
3. Klicken Sie auf die **Titel** -Eigenschaft in der **Pad "Eigenschaft"** , und ändern Sie den Titel der Schaltfläche zum `Hello, tvOS` und legen Sie die **Schriftgrad** 128:

    [![](hello-tvos-images/designer05.png "Legen Sie den Titel auf Hello, TvOS und den Schriftgrad auf 128 festgelegt.")](hello-tvos-images/designer05.png#lightbox)
4. Ändern Sie die Bezeichnung, sodass alle der Wörter sichtbar sind, und platzieren Sie sie am oberen Rand des Fensters zentriert:

    [![](hello-tvos-images/designer06.png "Ändern der Größe und zentrieren die Bezeichnung")](hello-tvos-images/designer06.png#lightbox)
5. Die Bezeichnung müssen nun auf die Position, eingeschränkt werden, damit er wie vorgesehen angezeigt wird. unabhängig von der Größe des Bildschirms. Zu diesem Zweck klicken Sie auf die Bezeichnung bis auf die *T gestalteten Handle* angezeigt wird:

    [![](hello-tvos-images/designer07.png "Das Handle T gestalteten")](hello-tvos-images/designer07.png#lightbox)
6. Um die Bezeichnung horizontal zu beschränken, wählen Sie das Quadrat Center, und ziehen Sie es auf die vertikale gestrichelte Linie:

    [![](hello-tvos-images/designer08.png "Wählen Sie das Quadrat center")](hello-tvos-images/designer08zoom.png#lightbox)

     Die Bezeichnung aktivieren Orange.
7. Wählen Sie den Ziehpunkt T oben auf der die Bezeichnung, und ziehen Sie sie am oberen Rand des Fensters aus:

    [![](hello-tvos-images/designer09.png "Ziehen Sie den Ziehpunkt am oberen Rand des Fensters")](hello-tvos-images/designer09.png#lightbox)
8. Klicken Sie dann auf die Breite und klicken Sie dann auf die Höhe *Knochenarbeit Handle* wie unten gezeigt:

    [![](hello-tvos-images/designer10.png "Die Breite und Höhe Knochenarbeit handles")](hello-tvos-images/designer10.png#lightbox)

     Wenn jede *Knochenarbeit Handle* ist geklickt haben, wählen Sie Breite und Höhe jeweils um feste Abmessungen festzulegen.
9. Abschließend sollte die Einschränkungen, die unter der Registerkarte "Layout" des eigenschaftenpads ähneln:

    [![](hello-tvos-images/designer11.png "Beispiel-Einschränkungen")](hello-tvos-images/designer11.png#lightbox)
10. Ziehen Sie eine **Schaltfläche** aus der **Toolbox** und platzieren Sie es unter der Bezeichnung.
11. Klicken Sie auf die **Titel** -Eigenschaft in der **Pad "Eigenschaft"** , und ändern Sie den Titel der Schaltfläche zum `Click Me`:

    [![](hello-tvos-images/designer12.png "Ändern Sie den Titel der Schaltflächen zum hier klicken")](hello-tvos-images/designer12.png#lightbox)
12. Wiederholen Sie die Schritte 5 bis 8 oben, um die Schaltfläche im TvOS-Fenster zu beschränken. Aber anstatt zu ziehen das T-Handle, am oberen Rand des Fensters (wie in Schritt #7), ziehen Sie es an das Ende der Bezeichnung:

    [![](hello-tvos-images/designer14.png "Beschränken Sie die Schaltfläche \"")](hello-tvos-images/designer14.png#lightbox)
13. Ziehen Sie eine andere Bezeichnung unter der Schaltfläche, um die gleiche Breite als erste Bezeichnung und legen Sie die Größe der **Ausrichtung** zu **Center**:

    [![](hello-tvos-images/designer15.png "Ziehen Sie eine andere Bezeichnung unter der Schaltfläche, seine Größe ändern Sie, werden die gleiche Breite wie die erste Bezeichnung aus, und legen die Ausrichtung auf Center")](hello-tvos-images/designer15.png#lightbox)
14. Wie die erste Bezeichnung und die Schaltfläche legen Sie diese Bezeichnung zentrieren und Heften sie in die Position und Größe:

    [![](hello-tvos-images/designer16.png "Heften Sie die Bezeichnung in Position und Größe")](hello-tvos-images/designer16.png#lightbox)
15. Speichern Sie die Änderungen an der Benutzeroberfläche.

Beim Bewegen von Steuerelementen und Ändern der Größe wurden, Sie sollten aufgefallen, dass der Designer Sie hilfreiche Hinweise gibt auf der Grundlage von [Apple TV-Human Interface Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/). Diese Richtlinien können Sie hochwertige Anwendungen zu erstellen, die eine vertraute Aussehen und Verhalten für Apple TV-Benutzer verfügen sollen.

Wenn Sie, in Suchen der **Dokumentgliederung** Abschnitt, beachten Sie, wie das Layout und die Hierarchie der Elemente, aus denen unser Benutzeroberfläche dargestellt werden:

[![](hello-tvos-images/designer17.png "Im Abschnitt für die Dokumentgliederung")](hello-tvos-images/designer17.png#lightbox)

Von hier aus können Sie auswählen, Elemente bearbeiten, oder ziehen, um die UI-Elemente neu anordnen, falls erforderlich. Wenn ein Element der Benutzeroberfläche von einem anderen Element verdeckt wird, können Sie es z. B. am Ende der Liste, um erleichtern das oberste Element im Fenster ziehen.

Nun, da wir unserer Benutzeroberfläche erstellt haben, müssen wir die UI-Elemente verfügbar machen, sodass Xamarin.tvOS zugreifen können, und mit ihnen in interagieren C# Code.

### <a name="accessing-the-controls-in-the-code-behind"></a>Zugreifen auf die Steuerelemente im CodeBehind

Es gibt zwei Hauptmethoden, um die Steuerelemente zugreifen, die Sie in der iOS-Designer aus Code hinzugefügt haben:

* Erstellen einen Ereignishandler für ein Steuerelement an.
* Kontrolle der einen Namen ein, sodass wir später darauf verweisen können.

Wenn eines dieser hinzugefügt werden, die partielle Klasse innerhalb der `ViewController.designer.cs` wird aktualisiert, um die Änderungen widerzuspiegeln. Dadurch können Sie die Steuerelemente in den Ansichtscontroller klicken Sie dann auf.

### <a name="creating-an-event-handler"></a>Erstellen eines ereignishandlers

In dieser beispielanwendung wird die Schaltfläche geklickt wird möchten wir _etwas_ auf das Eintreten ein ereignishandlers muss auf ein bestimmtes Ereignis auf die Schaltfläche hinzugefügt werden. Um dies einzurichten, führen Sie folgende Schritte aus:

1. Wählen Sie in der Xamarin.IOS-Designer die Schaltfläche auf den Ansichtscontroller ein.
2. Wählen Sie im Bereich "Eigenschaften" die **Ereignisse** Registerkarte:

    [![](hello-tvos-images/event1.png "Die Registerkarte \"Ereignisse\"")](hello-tvos-images/event1.png#lightbox)
3. Suchen Sie das TouchUpInside-Ereignis aus, und geben sie einen Ereignishandler namens `Clicked`:

    [![](hello-tvos-images/event2.png "Das Ereignis TouchUpInside")](hello-tvos-images/event2.png#lightbox)
4. Beim Drücken **EINGABETASTE**, **ViewController**cs-Datei wird geöffnet, schlägt die Speicherorte für Ihre Ereignishandler im Code. Verwenden Sie die Pfeiltasten auf der Tastatur, um den Speicherort festzulegen:

    [![](hello-tvos-images/event3.png "Festlegen des Speicherorts")](hello-tvos-images/event3.png#lightbox)
5. Dadurch wird eine partielle Methode erstellt, wie unten dargestellt:

    [![](hello-tvos-images/event4.png "Die partielle Methode")](hello-tvos-images/event4.png#lightbox)

Wir sind jetzt damit beginnen, Hinzufügen von Code, um die Schaltfläche, um die Funktion zu ermöglichen.

### <a name="naming-a-control"></a>Benennen ein Steuerelement

Wenn die Schaltfläche geklickt wird, sollte die Beschriftung zu aktualisieren basierend auf der Anzahl der Klicks. Zu diesem Zweck müssen wir die Bezeichnung im Code zugreifen. Dies erfolgt, indem Sie ihm einen Namen. Führen Sie folgende Schritte aus:

1. Öffnen Sie das Storyboard aus, und wählen Sie die Bezeichnung wird am unteren Rand der View-Controller.
2. Wählen Sie im Bereich "Eigenschaften" die **Widget** Registerkarte:

    [![](hello-tvos-images/name1.png "Wählen Sie die Registerkarte \"Widget\"")](hello-tvos-images/name1.png#lightbox)
3. Klicken Sie unter **Identität > Namen**, fügen `ClickedLabel`:

    [![](hello-tvos-images/name2.png "Legen Sie ClickedLabel")](hello-tvos-images/name2.png#lightbox)

Wir können nun die Bezeichnung wird aktualisiert.

### <a name="how-controls-are-accessed"></a>Wie die Steuerelemente zugegriffen werden

Bei Auswahl der `ViewController.designer.cs` in die **Projektmappen-Explorer** können, finden Sie unter wie die `ClickedLabel` Bezeichnung und die `Clicked` Ereignishandler zugeordnet wurde ein **Outlet** und **Aktion** in C#:

[![](hello-tvos-images/accesscontrol.png "Outlets und Aktionen")](hello-tvos-images/accesscontrol.png#lightbox)

Sie können auch feststellen, dass `ViewController.designer.cs` wird von eine partielle Klasse sind, damit Visual Studio für Mac nicht so ändern Sie `ViewController.cs` überschreibt die Änderungen, die wir auf die Klasse vorgenommen haben.

Die Elemente der Benutzeroberfläche auf diese Weise verfügbar zu machen, können Sie in den Ansichtscontroller auf Sie zugreifen.

Sie normalerweise müssen nie zum Öffnen der `ViewController.designer.cs` selbst, es wurde hier vorgestellten zum besseren.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>Schreiben des Codes

Mit unserer Benutzeroberfläche erstellt und die Elemente der Benutzeroberfläche verfügbar gemacht werden, um einen Überprüfungscode **Outlets** und **Aktionen**, sind Sie nun bereit ist, Schreiben Sie den Code, um die Anwendung mit Funktionen auszustatten.

In unserer Anwendung jedes Mal, wenn die erste Schaltfläche geklickt wird, werden wir aktualisieren unsere Bezeichnung aus, um wie viele Male anzeigen, auf die Schaltfläche geklickt hat. Zu diesem Zweck müssen wir öffnen den `ViewController.cs` Datei zur Bearbeitung durch Doppelklick im der **Lösungspad**:

[![](hello-tvos-images/code01.png "Das Pad \"Projektmappe\"")](hello-tvos-images/code01.png#lightbox)

Zunächst müssen wir zum Erstellen einer Variablen auf Klassenebene in unserer `ViewController` Klasse, um die Anzahl der Klicks zu erfassen. Bearbeiten Sie die Klassendefinition, sodass sie danach folgendermaßen aussieht:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Als Nächstes in derselben Klasse (`ViewController`), außer Kraft setzen müssen wir die **ViewDidLoad** Methode und Hinzufügen von Code, um die erste Meldung für die Bezeichnung festgelegt:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Müssen wir verwenden `ViewDidLoad`, statt einer anderen Methode wie z. B. `Initialize`, da `ViewDidLoad` heißt *nach* das Betriebssystem geladen und instanziiert die Benutzeroberfläche aus hat die `.storyboard` Datei. Wenn wir versuchen, das Label-Steuerelement, bevor Sie den Zugriff auf die `.storyboard` Datei vollständig geladen und instanziiert wurde, erhalten wir eine `NullReferenceException` Fehler, da das Bezeichnungssteuerelement noch nicht erstellt werden sollen.

Als Nächstes müssen wir den Code, um Antworten an den Benutzer, die Sie auf die Schaltfläche hinzufügen. Fügen Sie Folgendes, um partielle Klasse an, dass wir erstellt haben:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Dieser Code wird jedes Mal aufgerufen, der Benutzer auf die Schaltfläche klickt.

Nach Abschluss der Vorbereitung können wir nun zum Erstellen und Testen unsere Xamarin.tvOS-Anwendung.

## <a name="testing-the-application"></a>Testen der Anwendung

Es ist Zeit, die zum Erstellen und Ausführen unserer Anwendung, um sicherzustellen, dass es wie erwartet ausgeführt wird. Erstellt und in einem Schritt ausgeführt werden kann, oder wir können es erstellen, ohne Sie auszuführen.

Wenn wir eine Anwendung erstellen, können wir auswählen, welche Art von Build werden soll:

-   **Debuggen von** – in ein Debugbuild kompiliert ein "(Anwendung)-Datei mit zusätzlichen Metadaten, mit der wir Debuggen, was passiert, wenn die Anwendung ausgeführt wird.
-   **Release** – ein Releasebuild erstellt auch eine "Datei, aber diese keine Debuginformationen, sodass sie kleiner und schneller ausgeführt werden.  

Sie können auswählen, den Typ des Builds aus der **Konfigurationsauswahl** auf die linke obere Ecke der Visual Studio für Mac-Bildschirms:

[![](hello-tvos-images/run01.png "Wählen Sie den Typ des Builds")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Erstellen der Anwendung

In unserem Fall wir nur einen Debugbuild installieren möchten, stellen wir sicher, dass **Debuggen** ausgewählt ist. Erstellen Sie die Anwendung zuerst entweder drücken **⌘B**, oder aus der **erstellen** Menü wählen **Build All**.

Wenn Fehler auftreten, sehen Sie eine **Buildvorgang erfolgreich** Meldung in Visual Studio für Mac Statusleiste angezeigt. Wenn Fehler auftreten, überprüfen Sie das Projekt, und stellen Sie sicher, dass Sie alle Schritte richtig befolgt haben. Zunächst sicher, dass Ihr Code (sowohl in Xcode und Visual Studio für Mac) den Code im Tutorial übereinstimmt.

### <a name="running-the-application"></a>Ausführen der Anwendung

Um die Anwendung ausführen, gibt es drei Optionen:

-  Drücken Sie **⌘+EINGABE**.
-  Klicken Sie im Menü **Ausführen** auf **Debug**.
-  Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche **Wiedergabe** (über dem **Projektmappen-Explorer**).

Die Anwendung wird erstellt (sofern er noch nicht erstellt wurde), Start im Debugmodus befindet, die TvOS-Simulator wird gestartet und die app startet und dessen Hauptschnittstelle Fenster anzeigen:

[![Home-Bildschirm der Beispiel-app](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

Von der **Hardware** Menü die Option **anzeigen Apple TV-Remote** damit Sie den Simulator steuern können.

[![](hello-tvos-images/run04.png "Wählen Sie anzeigen, Apple TV-Remote")](hello-tvos-images/run04.png#lightbox)

Mithilfe der vom Simulator Remote, wenn Sie mehrmals die Bezeichnung der Schaltfläche klicken, sollten mit der Anzahl aktualisiert werden:

[![](hello-tvos-images/run05.png "Die Bezeichnung mit der Anzahl der aktualisierten")](hello-tvos-images/run05.png#lightbox)

Herzlichen Glückwunsch! Wir haben viele neue Informationen behandelt, aber wenn Sie in diesem Tutorial von Anfang bis Ende ausgeführt, Sie verfügen nun ein solides Grundwissen über die Komponenten einer Xamarin.tvOS-app als auch die Tools, die zu ihrer Erstellung verwendet.

## <a name="where-to-next"></a>Wo weiter?

Entwickeln von Apple TV-apps zeigt, wenige Herausforderungen, da die Trennung zwischen dem Benutzer und die Schnittstelle (er befindet sich im selben Raum nicht in der Benutzer manuell) und die Einschränkungen Tvos, platziert, in app-Größe und Speicher.

Daher wird dringend empfohlen, Ihre lesen die folgenden Dokumente, vor der Installation ein Xamarin.tvOS-app-Design:

- [Einführung in TvOS 9](~/ios/tvos/platform/tvos9.md) – in diesem Artikel werden alle neuen und geänderten APIs und in TvOS 9 verfügbaren Features für Entwickler mit Xamarin.tvOS.
- [Arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) – Benutzer Ihrer Xamarin.tvOS-app werden nicht mit seinem-Schnittstelle direkt als mit iOS, in dem sie tippen Sie auf Bilder auf den Bildschirm des Geräts, sondern indirekt über den Raum, die mit der entfernten Siri, interagiert werden. Dieser Artikel beschreibt das Konzept der Fokus und wie diese verwendet werden, um die Behandlung der Navigation in einer Xamarin.tvOS-app-Benutzeroberfläche.
- [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) – die wichtigste Methode, die Benutzer wird mit der Apple-TV und Ihrer app Xamarin.tvOS interagieren werden, über die enthaltene Siri Remote ist. Wenn Ihre app ein Spiel ist, können Sie optional Unterstützung für 3rd Party, gemacht für Bluetooth-Gamecontroller iOS (MFI) in Ihrer app sowie erstellen. Dieser Artikel behandelt die neuen Siri Remote- und Bluetooth Gamecontroller in Ihren Xamarin.tvOS-apps unterstützen.
- [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) – im Gegensatz zu iOS-Geräte, die neue Apple TV bietet keine persistenten und lokalen Speicher für TvOS-apps. Wenn Ihre Xamarin.tvOS-app Informationen (z. B. benutzereinstellungen) beibehalten werden muss, muss es daher speichern und Abrufen von Daten aus der iCloud. Dieser Artikel behandelt die Arbeit mit Ressourcen und die persistente Speicherung von Daten in einer Xamarin.tvOS-app.
- [Arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) – Erstellung ansprechender Symbole und Bilder sind ein wichtiger Bestandteil der Entwicklung eine beeindruckende benutzererfahrung für Ihre Apple TV-apps. Dieses Handbuch behandelt die erforderlichen Schritte zum Erstellen und integrieren Sie die erforderlichen grafischen Ressourcen für Ihre Xamarin.tvOS-apps.
- [Benutzeroberfläche](~/ios/tvos/user-interface/index.md) – einschließlich der Steuerelemente der Benutzeroberfläche (UI), Allgemein Benutzererfahrung (UX)-Abdeckung Xcodes Interface Builder und UX-Entwurfsprinzipien verwenden, bei der Arbeit mit Xamarin.tvOS.
- [Bereitstellung und Tests](~/ios/tvos/deploy-test/index.md) – dieser Abschnitt enthält Themen, die zum Testen und Verteilen einer app sowie Informationen verwendet. Hier enthalten aufgeführte Themen z.B. Tools zum Debuggen, die Bereitstellung für Tester sowie wie Sie eine Anwendung in der Apple TV App Store veröffentlichen.

Arbeiten mit Xamarin.tvOS Probleme auftreten, finden Sie unsere [Problembehandlung](~/ios/tvos/troubleshooting.md) -Dokumentation auf Probleme und Lösungen zu kennen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält einen schnellen Einstieg in die Entwicklung von apps für tvos verwendet. mit Visual Studio für Mac eine einfache Hello, TvOS-app erstellen. Sie behandelt die Grundlagen der TvOS-Gerät-Bereitstellung, Erstellung von Netzwerkschnittstellen für TvOS codieren und Testen im Simulator TvOS.


## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Erstellen von apps für TvOS mit Xamarin (video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
