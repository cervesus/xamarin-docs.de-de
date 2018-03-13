---
title: "Hello, tvos. außerdem wurden Quick Start Guide"
description: "Dieses Handbuch behandelt, durch die Erstellung der ersten Xamarin.tvOS-app und seine Entwicklung-toolkette. Es wird außerdem der Xamarin-Designer, die UI-Steuerelemente für Code verfügbar macht, und veranschaulicht das Erstellen, ausführen und Testen einer Anwendung Xamarin.tvOS eingeführt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: 5eccb36b3c6a437ddc1ec055e779d8f78460643e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvos. außerdem wurden Quick Start Guide

_Dieses Handbuch behandelt, durch die Erstellung der ersten Xamarin.tvOS-app und seine Entwicklung-toolkette. Es wird außerdem der Xamarin-Designer, die UI-Steuerelemente für Code verfügbar macht, und veranschaulicht das Erstellen, ausführen und Testen einer Anwendung Xamarin.tvOS eingeführt._

Apple hat die 5. Generierung von der Apple TV, Apple TV 4 K, veröffentlicht der tvos. außerdem wurden 11 ausgeführt wird.

Die Apple TV-Plattform ist für Entwickler, sodass sie umfassende-faszinierend apps erstellen und über den integrierten Apple TV-App-Store freigibt geöffnet.

Wenn Sie bei der Entwicklung von Xamarin.iOS vertraut sind, sollten Sie den Übergang zu tvos. außerdem wurden relativ einfach suchen. Die meisten APIs und -Funktionen sind identisch, allerdings viele gängige APIs (z. B. WebKit) nicht verfügbar sind. Darüber hinaus arbeiten mit den mit der Remoteinstanz Siri bringt Entwurf, die nicht in Touchscreen basierend iOS-Geräte vorhanden sind.

Dieses Handbuch erhalten eine Einführung in das Arbeiten mit tvos. außerdem wurden in einer Xamarin-app. Weitere Informationen zu tvos. außerdem wurden, finden Sie im Apple [Vorbereiten für App TV 4K](https://developer.apple.com/tvos/) Dokumentation.

## <a name="overview"></a>Übersicht

Xamarin.tvOS können Sie zum Entwickeln von vollständig systemeigene Apple TV-apps in c# und .NET mit dem gleichen OS X-Bibliotheken und Benutzeroberflächen-Steuerelemente, die verwendet werden, beim Entwickeln *Swift* (oder *Objective-C*) und *Xcode*.

Darüber hinaus seit Xamarin.tvOS apps in c# und .NET geschrieben werden, kann gemeinsam Back-End-Code mit Xamarin.iOS und Xamarin.Android mit Xamarin.Mac apps freigegeben werden; alle bei der Bereitstellung von einer einheitlichen Erfahrung auf jeder Plattform aus.

In diesem Artikel wird dienen als Einführung in die grundlegenden Konzepte, die zum Erstellen einer Apple TV-App mithilfe von Xamarin.tvOS und Visual Studio werden Sie durch den Prozess der Erstellung einer grundlegenden benötigt **Hello tvos. außerdem wurden** app, die zählt, wie oft eine Schaltfläche enthält geklickt wurde:

[![](hello-tvos-images/run05.png "Beispiel-app ausführen")](hello-tvos-images/run05.png#lightbox)

Die folgenden Konzepte beschrieben:

-  **Visual Studio für Mac** – Einführung in Visual Studio für Mac und Xamarin.tvOS Erstellen von Anwendungen mit.
-  **Aufbau einer App Xamarin.tvOS** – was ein Xamarin.tvOS app besteht aus.
-  **Erstellen einer Benutzeroberfläche** – wie Xamarin-Designer für iOS verwenden, um eine Benutzeroberfläche zu erstellen.
-  **Bereitstellung und Tests** – wie ausführen und Testen Sie die app aus, in der tvos. außerdem wurden Simulator und real tvos. außerdem wurden Hardware.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Starten Sie eine neue Xamarin.tvOS-App in Visual Studio für Mac

Wie bereits erwähnt, erstellen wir eine Apple TV-app mit dem Namen `Hello-tvOS` , ein einzelnes Optionsfeld an und eine Bezeichnung auf dem Hauptbildschirm addiert. Wenn Sie auf die Schaltfläche klicken, zeigt die Beschriftung an, wie oft sie bereits geklickt wurde.

Führen Sie zum Einstieg wir Folgendes:

1. Starten Sie Visual Studio für Mac:

    [![](hello-tvos-images/setup01.png "Visual Studio für Mac")](hello-tvos-images/setup01.png#lightbox)
2. Klicken Sie auf die **neue Projektmappe...**  Link in die linke obere Ecke des Bildschirms zum Öffnen der **neues Projekt** (Dialogfeld).
3. Wählen Sie **tvos. außerdem wurden** > **App** > **einzelne Ansicht App** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](hello-tvos-images/setup02.png "Wählen Sie einzelne Sicht App")](hello-tvos-images/setup02.png#lightbox)
4. Geben Sie `Hello, tvOS` für die **Anwendungsnamen**, geben Sie Ihre **Organisations-ID** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](hello-tvos-images/setup04.png "Geben Sie Hello tvos. außerdem wurden")](hello-tvos-images/setup04.png#lightbox)
5. Geben Sie `Hello_tvOS` für die **Projektname** , und klicken Sie auf die **erstellen** Schaltfläche:

    [![](hello-tvos-images/setup03.png "Enter HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Visual Studio für Mac neue Xamarin.tvOS-app erstellen und Anzeigen der Standarddateien, die Projektmappe für Ihre Anwendung hinzugefügt werden:

 [![](hello-tvos-images/project01.png "Die Standardansicht für Dateien")](hello-tvos-images/project01.png#lightbox)

Visual Studio für Mac verwendet **Lösungen** und **Projekte**, in die genaue genauso wie Visual Studio. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthalten kann. Projekte sind z.B. Anwendungen, unterstützende Bibliotheken, Testanwendungen usw. In diesem Fall hat Visual Studio für Mac für Sie eine Projektmappe und ein Anwendungsprojekt erstellt.

Wenn gewünscht, konnte Sie ein oder mehrere Code-Bibliotheksprojekte erstellen, die allgemeinen, freigegebenen Code enthalten. Diese Bibliotheksprojekte gemeinsam mit anderen Xamarin.tvOS app-Projekte oder durch das Anwendungsprojekt genutzt werden können (oder Xamarin.iOS und Xamarin.Android mit Xamarin.Mac basierend auf den Typ des Codes) so fest, wie Sie würden, wenn Sie eine standardmäßige .NET-Anwendung erstellen.

## <a name="anatomy-of-a-xamarintvos-app"></a>Aufbau einer Xamarin.tvOS-App

Wenn Sie mit iOS-Programmierung vertraut sind, bemerken Sie viele ähnlichkeiten hier. Tvos. außerdem wurden 9 ist tatsächlich eine Teilmenge von iOS 9, so viele Konzepte hier überschreitet.

Werfen wir einen Blick auf die Dateien im Projekt:

-   `Main.cs`: Diese Datei enthält den Haupteinstiegspunkt für die Anwendung. Wenn die App gestartet wird, enthält diese Datei die erste Klasse und Methode, die ausgeführt werden.
-   `AppDelegate.cs` – Diese Datei enthält die Hauptassembly der Anwendung, die für den Empfang von Ereignissen vom Betriebssystem zuständig ist.
-   `Info.plist` – Diese Datei enthält Anwendungseigenschaften, z. B. den Anwendungsnamen, Symbole usw. an.
-   `ViewController.cs` – Dies ist die Klasse, die das Hauptfenster darstellt, und steuert den Lebenszyklus des Zertifikats.
-   `ViewController.designer.cs` – Diese Datei enthält die Aufgaben, die Code, mit dem Sie die Benutzeroberfläche der Hauptbildschirm integriert.
-  `Main.storyboard` – Die Benutzeroberfläche des Hauptfensters. Diese Datei kann erstellt und von der Xamarin-Designer für iOS verwaltet werden.

In den folgenden Abschnitten führen wir einen Blick darauf über einige dieser Dateien. Müssen wir sie später ausführlicher untersuchen, aber es ist ratsam, ihre Grundlagen jetzt verstanden haben.

### <a name="maincs"></a>Main.cs

Die `Main.cs` -Datei enthält einen statischen `Main` Methode erstellt eine neue Instanz der Xamarin.tvOS-app und übergibt den Namen der Klasse, die verarbeitet OS-Ereignisse, in unserem Fall ist die `AppDelegate` Klasse:

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

Die `AppDelegate.cs` -Datei enthält unsere `AppDelegate` -Klasse, die für unsere Fenster zum Erstellen und Überwachen von Ereignissen OS verantwortlich ist:

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

Dieser Code ist wahrscheinlich nicht vertraut, es sei denn, Sie haben eine iOS-Anwendung vor erstellt, aber es recht einfach ist. Betrachten Sie die wichtigen Zeilen an.

Zuerst, werfen wir einen Blick auf die Deklaration von Variablen auf Klassenebene:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

Die `Window` Eigenschaft ermöglicht den Zugriff auf das Hauptfenster. tvos. außerdem wurden verwendet, so genannten der *Model View Controller* Schema (MVC). Für jedes Fenster, die Sie erstellen (und die für viele andere Elemente, die in Windows), besteht im Allgemeinen ein Controller, der für den Lebenszyklus des Fensters, z. B. angezeigt wird, das Hinzufügen neuer Ansichten (Steuerelemente) zu, usw. verwendet wird.

Als Nächstes haben wir die `FinishedLaunching` Methode. Diese Methode wird ausgeführt, nachdem die Anwendung instanziiert wurde, und zuständig ist für das tatsächlich erstellen im Anwendungsfenster angezeigt und beginnend beim Anzeigen der Ansicht darin. Da unsere app ein Storyboard zur Festlegung der Benutzeroberfläche verwendet wird, ist hier kein zusätzlicher Code erforderlich.

Es gibt viele Methoden, die in der Vorlage, z. B. bereitgestellt werden `DidEnterBackground` und `WillEnterForeground`. Diese können sicher entfernt werden, wenn die Anwendungsereignisse nicht in Ihrer app verwendet werden.

### <a name="viewcontrollercs"></a>ViewController.cs

Die `ViewController` Klasse ist unsere Hauptfenster Controller. Das bedeutet, dass sie für den Lebenszyklus des Hauptfensters verantwortlich ist. Wir werden um dies später im Detail untersuchen, jetzt nur werfen wir einen kurzen Blick:

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

Die Designer-Datei für die Main-Fensterklasse ist jetzt leer, aber automatisch aufgefüllt wird von Visual Studio für Mac wie wir unsere Benutzeroberfläche mit der iOS-Designer zu erstellen:

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

Wir sind nicht in der Regel Bedenken hinsichtlich Designer-Dateien, wie sie automatisch von Visual Studio für Mac verwaltet, und geben Sie nur den erforderlichen Aufgaben, die Code, der Zugriff auf Steuerelemente ermöglicht, dass es zu einem beliebigen Fenster hinzufügen oder in der vorliegenden Anwendung anzeigen.

Nun, dass wir unsere Xamarin.tvOS app erstellt haben, und wir ein grundlegendes Verständnis der zugehörigen Komponenten haben, sehen wir uns die Benutzeroberfläche erstellen.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>Erstellen der Benutzeroberfläche

Müssen Sie die Xamarin-Designer für iOS verwenden, um die Benutzeroberfläche für Ihre app Xamarin.tvOS erstellen nicht, kann die Benutzeroberfläche direkt aus dem C#-Code erstellt werden, aber, die den Rahmen dieses Artikels sprengen. Der Einfachheit halber werden wir den iOS-Designer verwenden unserer GUI im weiteren Verlauf dieses Lernprogramms erstellen.

Um erstellen Ihre Benutzeroberfläche zu starten, doppelklicken Sie nun auf die `Main.storyboard` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung in der iOS-Designer zu öffnen:

[![](hello-tvos-images/designer01.png "Die Main.storyboard-Datei im Projektmappen-Explorer")](hello-tvos-images/designer01.png#lightbox)

Dieser Designer starten und wie folgt aussehen:

[![](hello-tvos-images/designer02.png "Der Designer")](hello-tvos-images/designer02.png#lightbox)

Weitere Informationen zu den iOS-Designer und deren Funktionsweise finden Sie in der [Einführung in die Xamarin-Designer für iOS](~/ios/user-interface/designer/introduction.md) Handbuch.

Hinzufügen von Steuerelementen auf der Entwurfsoberfläche der unsere Xamarin.tvOS-app kann jetzt gestartet.

Führen Sie folgende Schritte aus:

1. Suchen Sie die **Toolbox**, die rechts neben der Entwurfsoberfläche angezeigt werden soll:

    [![](hello-tvos-images/designer03.png "Der Toolbox")](hello-tvos-images/designer03.png#lightbox)

    Wenn Sie ihn hier nicht finden, navigieren Sie zu **Ansicht > füllt > Toolbox** Anzeigen dieses Fehlerprotokolls.
2. Ziehen Sie eine **Bezeichnung** aus der **Toolbox** auf die Entwurfsoberfläche:

    [![](hello-tvos-images/designer04.png "Ziehen Sie eine Bezeichnung aus der Toolbox")](hello-tvos-images/designer04.png#lightbox)
3. Klicken Sie auf die **Titel** Eigenschaft in der **Eigenschaft Pad** und ändern Sie die Schaltfläche Titel in `Hello, tvOS` und legen Sie die **Schriftgrad** 128 sein:

    [![](hello-tvos-images/designer05.png "Festlegen Sie des Titels auf Hello tvos. außerdem wurden, und legen Sie den Schriftgrad auf 128")](hello-tvos-images/designer05.png#lightbox)
4. Ändern Sie die Bezeichnung, damit alle Wörter sichtbar sind, und platzieren Sie es im oberen Bereich des Fensters zentriert:

    [![](hello-tvos-images/designer06.png "Die Größe und zentrieren die Bezeichnung")](hello-tvos-images/designer06.png#lightbox)
5. Die Bezeichnung müssen jetzt auf dessen Position eingeschränkt werden, damit er wie vorgesehen angezeigt wird. unabhängig von der Größe des Bildschirms an. Zu diesem Zweck klicken Sie auf die Bezeichnung bis auf die *T geformten Handle* angezeigt wird:

    [![](hello-tvos-images/designer07.png "Das Handle T geformten")](hello-tvos-images/designer07.png#lightbox)
6. Die Bezeichnung horizontal zu beschränken, wählen Sie das Quadrat Center, und ziehen ihn auf der vertikalen gestrichelte Linie:

    [![](hello-tvos-images/designer08.png "Wählen Sie das Quadrat center")](hello-tvos-images/designer08zoom.png#lightbox)

     Die Bezeichnung aktivieren Orange.
7. Wählen Sie den Ziehpunkt T am oberen Rand die Bezeichnung, und ziehen Sie am oberen Rand des Fensters:

    [![](hello-tvos-images/designer09.png "Ziehen Sie den Ziehpunkt am oberen Rand des Fensters")](hello-tvos-images/designer09.png#lightbox)
8. Klicken Sie dann auf die Breite und klicken Sie dann auf die Höhe *Bone Handle* wie unten gezeigt:

    [![](hello-tvos-images/designer10.png "Die Breite und die Höhe Bone-handles")](hello-tvos-images/designer10.png#lightbox)

     Wenn jede *Bone Handle* ist geklickt haben, wählen Sie Breite und Höhe bzw. feste Dimensionen festlegen.
9. Abschließend sollte die Einschränkungen ähnlich denen in der Registerkarte "Layout" das Auffüllzeichen Eigenschaften aussehen:

    [![](hello-tvos-images/designer11.png "Beispiel-Einschränkungen")](hello-tvos-images/designer11.png#lightbox)
8. Ziehen Sie eine **Schaltfläche** aus der **Toolbox** und platzieren Sie es unter der Bezeichnung.
9. Klicken Sie auf die **Titel** Eigenschaft in der **Eigenschaft Pad** und ändern Sie die Schaltfläche Titel in `Click Me`:

    [![](hello-tvos-images/designer12.png "Ändern Sie den Titel Schaltflächen, Click Me")](hello-tvos-images/designer12.png#lightbox)
10. Wiederholen Sie die Schritte 5 bis 8 oben, um die Schaltfläche im Fenster tvos. außerdem wurden zu beschränken. Allerdings anstatt zu ziehen das T-Handle an den Anfang des Fensters (wie in Schritt &#7;), ziehen Sie es an das Ende der Bezeichnung:

    [![](hello-tvos-images/designer14.png "Beschränken Sie die Schaltfläche """)](hello-tvos-images/designer14.png#lightbox)
11. Ziehen Sie eine andere Bezeichnung unter der Schaltfläche, seine Größe ändern, um die gleiche Breite wie die erste Bezeichnung, und legen seine **Ausrichtung** auf **Center**:

    [![](hello-tvos-images/designer15.png "Ziehen Sie eine andere Bezeichnung unter der Schaltfläche, Größe, um die gleiche Breite wie die erste Bezeichnung sein, und legen Sie die Ausrichtung auf Center")](hello-tvos-images/designer15.png#lightbox)
12. Legen Sie diese Bezeichnung zum Zentrieren und Heften Sie ihn in die Position und Größe wie die erste Bezeichnung und Schaltfläche:

    [![](hello-tvos-images/designer16.png "Heften Sie die Bezeichnung in Position und Größe")](hello-tvos-images/designer16.png#lightbox)
13. Speichern Sie die Änderungen an der Benutzeroberfläche.

Beim Ändern der Größe und Verschieben von Steuerelementen, um wurden, Sie sollten aufgefallen, dass der Designer Sie Snap hilfreiche Hinweise kann auf der Grundlage von [Apple TV-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/tvos/human-interface-guidelines/). Diese Richtlinien hilft Ihnen hochwertige Anwendungen zu erstellen, die ein vertraut Aussehen und Verhalten für Apple TV-Benutzer verfügen sollen.

Wenn Sie, in Suchen der **Dokumentgliederung** Abschnitt, beachten Sie, wie das Layout und die Hierarchie der Elemente, aus denen unsere Benutzeroberfläche angezeigt werden:

[![](hello-tvos-images/designer17.png "Im Dokumentgliederungsfenster-Abschnitt")](hello-tvos-images/designer17.png#lightbox)

Von hier aus können Sie auswählen, Elemente bearbeiten, oder ziehen Elemente der Benutzeroberfläche neu anordnen, bei Bedarf. Wenn ein Element der Benutzeroberfläche von einem anderen Element abgedeckt wird, wurde, können Sie es z. B. an das Ende der Liste, um erleichtern das oberste Element im Fenster ziehen.

Nachdem wir unsere Benutzeroberfläche erstellt haben, müssen wir die Elemente der Benutzeroberfläche verfügbar zu machen, damit Xamarin.tvOS zugreifen und mit ihnen in C#-Code interagieren können.

### <a name="accessing-the-controls-in-the-code-behind"></a>Zugreifen auf die Steuerelemente im Code-behind

Es gibt zwei Hauptmethoden, die Zugriff auf Steuerelemente, die Sie in der iOS-Designer aus Code hinzugefügt haben:

* Erstellen einen Ereignishandler für ein Steuerelement an.
* Erhalten die Kontrolle einen Namen, damit wir später darauf verweisen können.

Wenn eine dieser Optionen hinzugefügt werden, die partielle Klasse innerhalb der `ViewController.designer.cs` wird aktualisiert, um die Änderungen wiederzugeben. Dies können Sie dann die Steuerelemente in die View-Controller zuzugreifen.

### <a name="creating-an-event-handler"></a>Erstellen eines ereignishandlers

In dieser beispielanwendung wird die Schaltfläche geklickt wird möchten wir _etwas_ durchgeführt werden soll, damit ein Ereignishandler muss auf ein bestimmtes Ereignis auf die Schaltfläche "" hinzugefügt werden. Um dies einzurichten, führen Sie folgende Schritte aus:

1. Wählen Sie in Xamarin iOS-Designer die Schaltfläche "View-Controller" ein.
2. Wählen Sie in das Auffüllzeichen Eigenschaften der **Ereignisse** Registerkarte:

    [![](hello-tvos-images/event1.png "Die Registerkarte "Ereignisse"")](hello-tvos-images/event1.png#lightbox)
3. Suchen Sie das TouchUpInside-Ereignis, und weisen Sie ihm einen Ereignishandler namens `Clicked`:

    [![](hello-tvos-images/event2.png "Das TouchUpInside-Ereignis")](hello-tvos-images/event2.png#lightbox)
4. Wenn Sie drücken **EINGABETASTE**, die **ViewController**cs-Datei wird geöffnet, schlägt die Speicherorte für einen Ereignishandler im Code. Verwenden Sie die Pfeiltasten auf der Tastatur, um den Speicherort festzulegen:

    [![](hello-tvos-images/event3.png "Festlegen des Speicherorts")](hello-tvos-images/event3.png#lightbox)
5. Dadurch wird eine partielle Methode erstellt, wie unten dargestellt:

    [![](hello-tvos-images/event4.png "Die partielle Methode")](hello-tvos-images/event4.png#lightbox)

Wir können nun beginnen, Hinzufügen von Code für die Schaltfläche, um die Funktion zu ermöglichen.

### <a name="naming-a-control"></a>Benennen ein Steuerelement

Wenn die Schaltfläche geklickt wird, sollten die Bezeichnung aktualisieren basierend auf der Anzahl der Klicks. Zu diesem Zweck müssen wir die Bezeichnung im Code zugreifen. Dies erfolgt, indem Sie ihm einen Namen. Führen Sie folgende Schritte aus:

1. Öffnen Sie das Storyboard aus, und wählen Sie die Bezeichnung im unteren Bereich des View-Controller.
2. Wählen Sie in das Auffüllzeichen Eigenschaften der **Widget** Registerkarte:

    [![](hello-tvos-images/name1.png "Wählen Sie die Registerkarte "Widget"")](hello-tvos-images/name1.png#lightbox)
3. Klicken Sie unter **Identität > Name**, hinzufügen `ClickedLabel`:

    [![](hello-tvos-images/name2.png "Set ClickedLabel")](hello-tvos-images/name2.png#lightbox)

Wir können jetzt starten, aktualisieren die Bezeichnung!

### <a name="how-controls-are-accessed"></a>Wie Steuerelemente zugegriffen wird

Bei Auswahl der `ViewController.designer.cs` in der **Projektmappen-Explorer** können, finden Sie unter wie die der `ClickedLabel` Bezeichnung und der `Clicked` Ereignishandler zugeordnet haben eine **Ausgang** und  **Aktion** in c#:

[![](hello-tvos-images/accesscontrol.png "Ausgänge und Aktionen")](hello-tvos-images/accesscontrol.png#lightbox)

Möglicherweise auch feststellen, dass `ViewController.designer.cs` ist eine partielle Klasse so, dass Visual Studio für Mac nicht ändern `ViewController.cs` würde die, die wir auf die Klasse vorgenommenen Änderungen überschrieben.

Verfügbarmachen von UI-Elemente auf diese Weise können in die View-Controller auf Sie zugreifen.

Normalerweise werden nie müssen Sie öffnen die `ViewController.designer.cs` selbst, es wurde hier vorgestellten nur zu Lernzwecken.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>Schreiben des Codes

Mit unserer Benutzeroberfläche erstellt und der UI-Elemente, die verfügbar gemacht werden, um einen Überprüfungscode **Steckdosen** und **Aktionen**, wir können schließlich schreiben den Code, um die Funktionalität der Anwendung zu gewähren.

In der vorliegenden Anwendung jedes Mal, wenn die erste Schaltfläche geklickt wird, werden wir unsere Bezeichnung, um wie viele Male anzuzeigen, klicken auf die Schaltfläche, aktualisieren. Um dies zu erreichen, müssen wir öffnen die `ViewController.cs` Datei für die Bearbeitung durch Doppelklick im die **Lösung Pad**:

[![](hello-tvos-images/code01.png "Die Lösung mit Leerstellen auffüllen")](hello-tvos-images/code01.png#lightbox)

Zunächst muss zum Erstellen einer Variablen auf Klassenebene in unserer `ViewController` Klasse, um die Anzahl der Klicks nachverfolgen, die aufgetreten sind. Bearbeiten Sie die Klassendefinition, sodass sie danach folgendermaßen aussieht:

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

Im nächsten Schritt in der gleichen Klasse (`ViewController`), müssen wir überschreiben die **ViewDidLoad** Methode und fügen Sie Code um die ursprüngliche Nachricht für unsere Bezeichnung festzulegen:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Wir verwenden müssen `ViewDidLoad `, anstelle einer anderen Methode wie z. B. `Initialize`, da `ViewDidLoad ` wird aufgerufen, *nach* das Betriebssystem geladen und instanziiert die Benutzeroberfläche wurde die `.storyboard` Datei. Wenn es wurde versucht, das Label-Steuerelement vor dem Zugriff auf die `.storyboard` Datei vollständig geladen und instanziiert wurde, erhalten wir einen `NullReferenceException` Fehler, da das Label-Steuerelement noch nicht erstellt werden würde.

Als Nächstes müssen wir den Code zum Antworten auf die Benutzer klicken auf die Schaltfläche hinzufügen. Fügen Sie die folgenden, auf Partial Klasse an, dass wir erstellt:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Dieser Code wird der Benutzer unserer drückt jederzeit aufgerufen werden.

Alles vorhanden können wir nun unsere Xamarin.tvOS-Anwendung erstellen und testen.

## <a name="testing-the-application"></a>Testen der Anwendung

Es ist Zeit zum Erstellen und Ausführen der Anwendung um sicherzustellen, dass er erwartungsgemäß ausgeführt wird. Erstellt und in einem Schritt ausgeführt werden können, oder wir können es ohne Ausführung erstellen.

Sobald wir eine Anwendung erstellen, können wir auswählen, welche Art von Builds werden soll:

-   **Debuggen** – in ein Debugbuild kompiliert ein '' (Anwendung)-Datei mit zusätzlichen Metadaten, die uns ermöglicht, Debuggen, was geschieht, während die Anwendung ausgeführt wird.
-   **Release** – ein Releasebuild erstellt außerdem einen "Datei, aber sie keine Debuginformationen einschließen, damit kleiner und schneller ausgeführt.  

Wählen Sie den Typ des Builds von der **Konfiguration Selektor** auf die linke obere Ecke der Visual Studio für Mac-Bildschirm:

[![](hello-tvos-images/run01.png "Wählen Sie den Typ des Builds")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Erstellen der Anwendung

In unserem Fall wir nur einen Debugbuild werden soll, stellen Sie also sicher, dass **Debuggen** ausgewählt ist. Erstellen Sie die Anwendung zuerst entweder drücken **⌘B**, oder aus der **erstellen** Menü Wählen Sie **Build All**.

Wenn es wurden keine Fehler, sehen Sie eine **Buildvorgang erfolgreich** Meldung in Visual Studio für Mac Statusleiste. Wenn Fehler aufgetreten sind, überprüfen Sie das Projekt, und stellen Sie sicher, dass Sie alle Schritte richtig befolgt haben. Starten Sie, indem Sie bestätigen, dass Code (beide in Xcode und in Visual Studio für Mac) mit dem Code im Lernprogramm übereinstimmt.

### <a name="running-the-application"></a>Ausführen der Anwendung

Um die Anwendung ausführen, haben wir drei Optionen:

-  Drücken Sie **⌘+EINGABE**.
-  Klicken Sie im Menü **Ausführen** auf **Debug**.
-  Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche **Wiedergabe** (über dem **Projektmappen-Explorer**).

Erstellen der Anwendung (sofern er noch nicht bereits erstellt), Start im Debugmodus befindet, die tvos. außerdem wurden Simulator startet, und die app starten und Hauptschnittstelle Fenster angezeigt wird:

[![Die Beispiel-app-Startbildschirm](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

Aus der **Hardware** Menü die Option **anzeigen Apple TV Remote** damit Sie den Simulator steuern können.

[![](hello-tvos-images/run04.png "Wählen Sie anzeigen, Apple TV-Remote")](hello-tvos-images/run04.png#lightbox)

Remote den Simulator verwenden, wenn Sie auf die Schaltfläche "mehrmals die Bezeichnung" klicken, sollten mit der Anzahl aktualisiert werden:

[![](hello-tvos-images/run05.png "Die Bezeichnung mit aktualisierten count")](hello-tvos-images/run05.png#lightbox)

Herzlichen Glückwunsch! Wir viele immense hier behandelt, aber wenn Sie dieses Lernprogramm von Anfang bis Ende ausgeführt, Sie verfügen jetzt über ein gutes Verständnis der die Bestandteile einer app Xamarin.tvOS als auch die Tools zu ihrer Erstellung verwendet.

## <a name="where-to-next"></a>Wo weiter?

Entwickeln von Apple TV-apps zeigt nur einige Herausforderungen, da die Trennung zwischen dem Benutzer und die Schnittstelle (er befindet sich über den Raum nicht in der Hand des Benutzers) und die Einschränkungen tvos. außerdem wurden auf die Größe der app und Speicher platziert werden.

Daher dringend empfohlen, die, dass Sie lesen die folgenden Dokumente vor der Installation einer Xamarin.tvOS-app-Entwurfs:

- [Einführung in die tvos. außerdem wurden 9](~/ios/tvos/platform/tvos9.md) – diesem Artikel werden die neuen und geänderten-APIs und in tvos. außerdem wurden 9 verfügbaren Funktionen für Entwickler Xamarin.tvOS eingeführt.
- [Arbeiten mit Navigationsbereich und den Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) – Benutzer Ihrer App Xamarin.tvOS werden nicht werden interagieren mit der Schnittstelle direkt als mit iOS, in dem sie tippen Sie auf Bilder auf dem Gerät Bildschirm jedoch indirekt von über den Raum, der mithilfe von Siri Remote. Dieser Artikel behandelt das Konzept der Fokus hat und wie es zum Behandeln der Navigation in einer Xamarin.tvOS-app-Benutzeroberfläche verwendet wird.
- [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) – die main-Methode, die Benutzer werden mit der Apple TV und Ihre app Xamarin.tvOS Interaktion werden, erfolgt über die enthalten Siri Remote. Wenn Ihre app ein Spiel ist, können Sie optional Unterstützung für 3rd Party, vorgenommen für Bluetooth-Gamecontroller iOS (Konfiguration) in Ihrer app sowie erstellen. Dieser Artikel behandelt die neuen Siri Remote und Bluetooth Gamecontroller in Ihren apps Xamarin.tvOS unterstützen.
- [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) – im Gegensatz zu iOS-Geräte, die neue Apple TV leistet keinen persistenten und lokalen Speicher für tvos. außerdem wurden apps. Wenn Ihre app Xamarin.tvOS Informationen (z. B. benutzereinstellungen) beibehalten muss muss es daher speichern und Abrufen von Daten aus iCloud. In diesem Artikel wird das Arbeiten mit Ressourcen und die persistente Speicherung von Daten in einer app Xamarin.tvOS behandelt.
- [Arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) – Erstellen von fesselnden Symbole und Bilder sind ein wichtiger Bestandteil der Entwicklung einer faszinierend benutzerfreundlichkeit für Ihre apps für Apple TV. Dieses Handbuch befasst sich die erforderlichen Schritte zum Erstellen und schließen Sie die Grafiken erforderlichen Ressourcen für Ihre apps Xamarin.tvOS.
- [Benutzeroberfläche](~/ios/tvos/user-interface/index.md) – beim Arbeiten mit Xamarin.tvOS einschließlich Steuerelemente der Benutzeroberfläche (UI), Allgemein User Experience (UX)-Coverage verwenden Xcodes Benutzeroberflächen-Generator und UX Entwurfsprinzipien.
- [Bereitstellung und Tests](~/ios/tvos/deploy-test/index.md) – dieser Abschnitt enthält Themen, die zum Testen einer app sowie dazu, wie es verteilt. Hier aufgeführten Themen enthalten, z. B. Tools zum Debuggen von Testern und wie Sie eine Anwendung auf den Apple TV-App-Store veröffentlichen Bereitstellung verwendet wird.

Wenn arbeiten mit Xamarin.tvOS Probleme auftreten, lesen Sie bitte unsere [Problembehandlung](~/ios/tvos/troubleshooting.md) Dokumentation finden Sie eine Liste auf kennen, Probleme und Lösungen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel bereitgestellt einen schnellen Einstieg zum Entwickeln von apps für tvos. außerdem wurden mit Visual Studio für Mac durch das Erstellen einer einfachen Hello, tvos. außerdem wurden app. Er behandelt die Grundlagen der tvos. außerdem wurden Gerät bereitstellen, Schnittstelle erstellen, Schreiben von Code für tvos. außerdem wurden aus, und testen den Simulator tvos. außerdem wurden.


## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Erstellen von apps für tvos. außerdem wurden mit Xamarin (video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
