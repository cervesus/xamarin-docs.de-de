---
title: Hello, tvOS – Kurzanleitung
description: Dieser Leitfaden führt Sie durch die Erstellung Ihrer ersten xamarin. tvos-APP und ihrer Entwicklungs Toolkette. Außerdem wird der xamarin-Designer eingeführt, der UI-Steuerelemente für Code verfügbar macht, und es wird veranschaulicht, wie Sie eine xamarin. tvos-Anwendung erstellen, ausführen und testen.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 02/02/2018
ms.openlocfilehash: 55e41c01421e2cd5a0bb5c3a0a9fe2d025c8a223
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938046"
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvOS – Kurzanleitung

_Dieser Leitfaden führt Sie durch die Erstellung Ihrer ersten xamarin. tvos-APP und ihrer Entwicklungs Toolkette. Außerdem wird der xamarin-Designer eingeführt, der UI-Steuerelemente für Code verfügbar macht, und es wird veranschaulicht, wie Sie eine xamarin. tvos-Anwendung erstellen, ausführen und testen._

Apple hat die 5. Generation von Apple TV, dem Apple TV 4K, veröffentlicht, das tvos 11 ausführt.

Die Apple TV-Plattform ist Entwicklern offen, sodass Sie umfassende, immersive Apps erstellen und über den integrierten App Store von Apple TV veröffentlichen können.

Wenn Sie mit der xamarin. IOS-Entwicklung vertraut sind, sollten Sie den Übergang zu tvos recht einfach finden. Die meisten APIs und Features sind identisch, aber viele gängige APIs sind nicht verfügbar (z. b. WebKit). Außerdem sind bei der Arbeit mit der Siri-Remote einige Entwurfs Herausforderungen zu meistern, die auf Touchscreen-basierten IOS-Geräten nicht vorhanden sind.

Dieses Handbuch bietet eine Einführung in die Arbeit mit tvos in einer xamarin-app. Weitere Informationen zu tvos finden Sie in der Dokumentation zur Vorbereitung auf [Apple TV 4K](https://developer.apple.com/tvos/) .

## <a name="overview"></a>Übersicht

Mit xamarin. tvos können Sie vollständig native Apple TV-apps in c# und .NET entwickeln, indem Sie die gleichen OS X-Bibliotheken und Schnittstellen-Steuerelemente verwenden, die bei der Entwicklung von *SWIFT* (oder *Ziel-C*) und *Xcode*verwendet werden.

Da xamarin. tvos-apps in c# und .NET geschrieben sind, kann ein gängiger Back-End-Code mit xamarin. IOS-, xamarin. Android-und xamarin. Mac-apps gemeinsam genutzt werden. die gesamte Plattform bietet eine native Darstellung der einzelnen Plattformen.

In diesem Artikel werden die wichtigsten Konzepte vorgestellt, die zum Erstellen einer Apple TV-App mithilfe von xamarin. tvos und Visual Studio erforderlich sind. dabei werden Sie durch den Prozess der Erstellung einer grundlegenden **Hello-, tvos** -APP, die zählt, wie oft auf eine Schaltfläche geklickt wurde:

[![Beispiel-App-Run](hello-tvos-images/run05.png)](hello-tvos-images/run05.png#lightbox)

Wir befassen uns mit den folgenden Konzepten:

- **Visual Studio für Mac** – Einführung in die Visual Studio für Mac und das Erstellen von xamarin. tvos-Anwendungen.
- **Anatomie einer xamarin. tvos-App** – was eine xamarin. tvos-App besteht.
- **Erstellen einer Benutzeroberfläche** – Gewusst wie: Verwenden von zum Xamarin Designer für IOS, um eine Benutzeroberfläche zu erstellen.
- **Bereitstellung und Tests** – ausführen und Testen Ihrer APP im tvos-Simulator und auf echter tvos-Hardware.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Starten einer neuen xamarin. tvos-app in Visual Studio für Mac

Wie bereits erwähnt, erstellen wir eine Apple TV-APP `Hello-tvOS` mit dem Namen, die dem Hauptbildschirm eine einzelne Schaltfläche und eine Bezeichnung hinzufügt. Wenn Sie auf die Schaltfläche klicken, zeigt die Beschriftung an, wie oft sie bereits geklickt wurde.

Gehen Sie dazu wie folgt vor:

1. Starten Sie Visual Studio für Mac:

    [![Visual Studio für Mac](hello-tvos-images/setup01.png)](hello-tvos-images/setup01.png#lightbox)
2. Klicken Sie in der oberen linken Ecke des Bildschirms auf den Link neue Projekt Mappe **...** , um das Dialogfeld **Neues Projekt** zu öffnen.
3. Wählen Sie **tvos**  >  **App**  >  **Single View App** aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![Einzelansicht-App auswählen](hello-tvos-images/setup02.png)](hello-tvos-images/setup02.png#lightbox)
4. Geben Sie `Hello, tvOS` als **APP-Namen**ein, geben Sie Ihren **Organisations Bezeichner** ein, und klicken Sie auf die Schaltfläche **Next**

    [![Geben Sie Hello, tvos ein.](hello-tvos-images/setup04.png)](hello-tvos-images/setup04.png#lightbox)
5. Geben Sie `Hello_tvOS` als **Projektnamen** ein, und klicken Sie auf die Schaltfläche **Erstellen** :

    [![Hellotvos eingeben](hello-tvos-images/setup03.png)](hello-tvos-images/setup03.png#lightbox)

Visual Studio für Mac erstellt die neue xamarin. tvos-APP und zeigt die Standard Dateien an, die der Projekt Mappe Ihrer Anwendung hinzugefügt werden:

 [![Die standardmäßige Dateiansicht](hello-tvos-images/project01.png)](hello-tvos-images/project01.png#lightbox)

Visual Studio für Mac verwendet **Solutions** Projektmappen und **Projekte**auf genau dieselbe Weise wie Visual Studio. Eine Projekt Mappe ist ein Container, der ein oder mehrere Projekte enthalten kann. Projekte können Anwendungen, unterstützende Bibliotheken, Testanwendungen usw. enthalten. In diesem Fall hat Visual Studio für Mac sowohl eine Projekt Mappe als auch ein Anwendungsprojekt für Sie erstellt.

Wenn Sie möchten, können Sie ein oder mehrere Code Bibliotheks Projekte erstellen, die gemeinsamen, freigegebenen Code enthalten. Diese Bibliotheks Projekte können vom Anwendungsprojekt verwendet oder für andere xamarin. tvos-App-Projekte (oder xamarin. IOS, xamarin. Android und xamarin. Mac basierend auf dem Codetyp) freigegeben werden, genauso wie beim Aufbau einer .net-Standardanwendung.

## <a name="anatomy-of-a-xamarintvos-app"></a>Anatomie einer xamarin. tvos-App

Wenn Sie mit der IOS-Programmierung vertraut sind, werden Sie hier viele Ähnlichkeiten feststellen. Tatsächlich handelt es sich bei tvos 9 um eine Teilmenge von IOS 9, sodass sich viele Konzepte hier überkreuzen.

Werfen wir einen Blick auf die Dateien im Projekt:

- `Main.cs`: Diese Datei enthält den Haupteinstiegspunkt für die Anwendung. Wenn die App gestartet wird, enthält diese Datei die erste Klasse und Methode, die ausgeführt werden.
- `AppDelegate.cs`– Diese Datei enthält die Haupt Anwendungsklasse, die für das Lauschen auf Ereignisse vom Betriebssystem verantwortlich ist.
- `Info.plist`– Diese Datei enthält Anwendungseigenschaften, z. b. den Anwendungsnamen, Symbole usw.
- `ViewController.cs`– Dies ist die Klasse, die das Hauptfenster darstellt und den Lebenszyklus der IT steuert.
- `ViewController.designer.cs`– Diese Datei enthält grundlegende Codes, die Ihnen bei der Integration in die Benutzeroberfläche des Hauptbildschirms helfen.
- `Main.storyboard`– Die Benutzeroberfläche für das Hauptfenster. Diese Datei kann vom Xamarin Designer für IOS erstellt und verwaltet werden.

In den folgenden Abschnitten werden einige dieser Dateien kurz erläutert. Wir werden Sie später ausführlicher untersuchen, aber es ist ratsam, sich jetzt mit ihren Grundlagen vertraut zu machen.

### <a name="maincs"></a>Main.cs

Die `Main.cs` Datei enthält eine statische `Main` Methode, die eine neue xamarin. tvos-app-Instanz erstellt und den Namen der Klasse übergibt, die Betriebssystem Ereignisse behandelt, in unserem Fall die- `AppDelegate` Klasse:

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

Die `AppDelegate.cs` Datei enthält die `AppDelegate` -Klasse, die für das Erstellen des Fensters und das Lauschen auf Betriebssystem Ereignisse verantwortlich ist:

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

Dieser Code ist wahrscheinlich nicht vertraut, es sei denn, Sie haben zuvor eine IOS-Anwendung erstellt, aber es ist recht unkompliziert. Sehen wir uns die wichtigen Zeilen genauer an.

Sehen wir uns zunächst die Variablen Deklaration auf Klassenebene an:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

Die- `Window` Eigenschaft ermöglicht den Zugriff auf das Hauptfenster. tvos verwendet das MVC-Muster ( *Model View Controller* ). Im Allgemeinen gibt es für jedes Fenster, das Sie erstellen (und für viele andere Dinge in Windows), einen Controller, der für den Lebenszyklus des Fensters zuständig ist, z. b. die Anzeige, das Hinzufügen neuer Ansichten (Steuerelemente) usw.

Als nächstes haben wir die- `FinishedLaunching` Methode. Diese Methode wird ausgeführt, nachdem die Anwendung instanziiert wurde, und Sie ist dafür verantwortlich, das Anwendungsfenster tatsächlich zu erstellen und den Prozess der Anzeige der Ansicht darin zu beginnen. Da in der APP ein Storyboard zum Definieren der Benutzeroberfläche verwendet wird, ist hier kein zusätzlicher Code erforderlich.

Es gibt viele andere Methoden, die in der Vorlage bereitgestellt werden, z `DidEnterBackground` `WillEnterForeground` . b. und. Diese können sicher entfernt werden, wenn die Anwendungs Ereignisse nicht in Ihrer APP verwendet werden.

### <a name="viewcontrollercs"></a>ViewController.cs

Die `ViewController` -Klasse ist der Controller des Hauptfensters. Dies bedeutet, dass Sie für den Lebenszyklus des Hauptfensters verantwortlich ist. Wir untersuchen dies später ausführlich, und jetzt sehen wir uns das kurz an:

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

Die Designer Datei für die Hauptfenster Klasse ist im Moment leer, wird aber automatisch mit Visual Studio für Mac aufgefüllt, wenn wir die Benutzeroberfläche mit dem IOS-Designer erstellen:

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

Wir kümmern uns in der Regel nicht um Designer Dateien, da Sie gerade automatisch von Visual Studio für Mac verwaltet werden, und stellen lediglich den erforderlichen Grund Code bereit, der den Zugriff auf Steuerelemente ermöglicht, die in der Anwendung zu einem beliebigen Fenster oder einer Ansicht hinzugefügt werden.

Nachdem wir nun unsere xamarin. tvos-App erstellt haben und grundlegende Kenntnisse zu den Komponenten haben, sehen wir uns das Erstellen der Benutzeroberfläche an.

<a name="Creating-the-User-Interface"></a>

## <a name="creating-the-user-interface"></a>Erstellen der Benutzeroberfläche

Sie müssen Xamarin Designer für IOS nicht verwenden, um die Benutzeroberfläche für die xamarin. tvos-APP zu erstellen. die Benutzeroberfläche kann direkt aus c#-Code erstellt werden, dies würde jedoch über den Rahmen dieses Artikels hinausgehen. Der Einfachheit halber verwenden wir den IOS-Designer, um die Benutzeroberfläche im gesamten Rest dieses Tutorials zu erstellen.

Um mit dem Erstellen der Benutzeroberfläche zu beginnen, doppelklicken Sie auf die `Main.storyboard` Datei im **Projektmappen-Explorer** , um Sie im IOS-Designer zur Bearbeitung zu öffnen:

[![Die Datei "Main. Storyboard" im Projektmappen-Explorer](hello-tvos-images/designer01.png)](hello-tvos-images/designer01.png#lightbox)

Dadurch sollte der Designer gestartet und wie folgt aussehen:

[![Der Designer](hello-tvos-images/designer02.png)](hello-tvos-images/designer02.png#lightbox)

Weitere Informationen über den IOS-Designer und seine Funktionsweise finden Sie in der [Einführung in das Xamarin Designer für IOS](~/ios/user-interface/designer/introduction.md) Handbuch.

Nun können Sie der Entwurfs Oberfläche der xamarin. tvos-App Steuerelemente hinzufügen.

Gehen Sie wie folgt vor:

1. Suchen Sie den **Werkzeugkasten**, der sich rechts von der Entwurfs Oberfläche befinden sollte:

    [![Die Toolbox](hello-tvos-images/designer03.png)](hello-tvos-images/designer03.png#lightbox)

    Wenn Sie diese hier nicht finden können, können Sie **> Pads > Toolbox anzeigen** , um Sie anzuzeigen.
2. Ziehen Sie eine **Bezeichnung** aus der **Toolbox** auf die Entwurfs Oberfläche:

    [![Ziehen Sie eine Bezeichnung aus der Toolbox.](hello-tvos-images/designer04.png)](hello-tvos-images/designer04.png#lightbox)
3. Klicken Sie im **eigenschaftenpad** auf die **Title** -Eigenschaft, und ändern Sie den Titel der Schaltfläche in, `Hello, tvOS` und legen Sie den **Schrift** Grad auf 128 fest:

    [![Legen Sie den Titel auf Hello, tvos fest, und legen Sie den Schrift Grad auf 128 fest.](hello-tvos-images/designer05.png)](hello-tvos-images/designer05.png#lightbox)
4. Ändern Sie die Größe der Bezeichnung so, dass alle Wörter sichtbar sind, und platzieren Sie Sie zentriert am oberen Rand des Fensters:

    [![Ändern der Größe und zentrieren der Bezeichnung](hello-tvos-images/designer06.png)](hello-tvos-images/designer06.png#lightbox)
5. Die Bezeichnung muss nun auf ihre Position eingeschränkt werden, damit Sie wie beabsichtigt angezeigt wird. unabhängig von der Bildschirmgröße. Klicken Sie hierzu auf die Bezeichnung, bis das *T-förmige handle* angezeigt wird:

    [![Das T-förmige handle](hello-tvos-images/designer07.png)](hello-tvos-images/designer07.png#lightbox)
6. Um die Bezeichnung horizontal einzuschränken, wählen Sie das Mittel eckige Quadrat aus, und ziehen Sie es auf die vertikal gestrichelte Linie:

    [![Wählen Sie das Mittel Quadrat aus.](hello-tvos-images/designer08.png)](hello-tvos-images/designer08zoom.png#lightbox)

     Die Bezeichnung sollte Orange drehen.
7. Wählen Sie das T-Handle am oberen Rand der Bezeichnung aus, und ziehen Sie es an den oberen Rand des Fensters:

    [![Ziehen Sie das Handle an den oberen Rand des Fensters.](hello-tvos-images/designer09.png)](hello-tvos-images/designer09.png#lightbox)
8. Klicken Sie anschließend wie unten dargestellt auf die Breite und dann auf das Height- *Bone-handle* :

    [![Die breiten-und Höhen Zieh Punkte.](hello-tvos-images/designer10.png)](hello-tvos-images/designer10.png#lightbox)

     Wählen Sie beim Klicken auf jedes *Knochen handle* sowohl Width als auch Height aus, um fixierte Dimensionen festzulegen.
9. Wenn Sie fertig sind, sollten die Einschränkungen den Einstellungen auf der Registerkarte Layout im eigenschaftenpad ähneln:

    [![Beispiel Einschränkungen](hello-tvos-images/designer11.png)](hello-tvos-images/designer11.png#lightbox)
10. Ziehen Sie eine **Schaltfläche** aus der **Toolbox** , und platzieren Sie Sie unter der Bezeichnung.
11. Klicken Sie im **eigenschaftenpad** auf die **Title** -Eigenschaft, und ändern Sie den Titel der Schaltfläche in `Click Me` :

    [![Ändern Sie den Titel der Schaltflächen, um auf me](hello-tvos-images/designer12.png)](hello-tvos-images/designer12.png#lightbox)
12. Wiederholen Sie die Schritte 5 bis 8 oben, um die Schaltfläche im tvos-Fenster einzuschränken. Anstatt das T-handle jedoch an den oberen Rand des Fensters zu ziehen (wie in Schritt #7), ziehen Sie es an den unteren Rand der Bezeichnung:

    [![Einschränken der Schaltfläche](hello-tvos-images/designer14.png)](hello-tvos-images/designer14.png#lightbox)
13. Ziehen Sie eine andere Bezeichnung unter die Schaltfläche, und legen Sie Sie auf die gleiche Breite wie die erste Bezeichnung fest, und legen Sie deren **Ausrichtung** auf **Center**fest:

    [![Ziehen Sie eine andere Bezeichnung unter die Schaltfläche, und legen Sie Sie auf die gleiche Breite wie die erste Bezeichnung fest, und legen Sie deren Ausrichtung auf zentriert fest.](hello-tvos-images/designer15.png)](hello-tvos-images/designer15.png#lightbox)
14. Legen Sie diese Bezeichnung wie die erste Bezeichnung und Schaltfläche auf zentriert fest, und Heften Sie Sie an Position und Größe an:

    [![Heften Sie die Bezeichnung an Position und Größe an.](hello-tvos-images/designer16.png)](hello-tvos-images/designer16.png#lightbox)
15. Speichern Sie die Änderungen an der Benutzeroberfläche.

Wenn Sie die Größe der Steuerelemente geändert haben, sollten Sie feststellen, dass der Designer Ihnen hilfreiche Snap-Ins bietet, die auf [Apple TV Human Interface Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/)basieren. Diese Richtlinien helfen Ihnen, qualitativ hochwertige Anwendungen zu erstellen, die für Apple TV-Benutzer ein bekanntes Aussehen und Gefühl haben werden.

Wenn Sie sich im Abschnitt **Dokument** Gliederung ansehen, sehen Sie, wie das Layout und die Hierarchie der Elemente, aus denen unsere Benutzeroberfläche besteht, angezeigt werden:

[![Der Abschnitt "Dokument Gliederung"](hello-tvos-images/designer17.png)](hello-tvos-images/designer17.png#lightbox)

Von hier aus können Sie Elemente auswählen, die Sie bearbeiten oder ziehen, um Benutzeroberflächen Elemente bei Bedarf neu anzuordnen. Wenn z. b. ein Benutzeroberflächen Element von einem anderen Element abgedeckt wird, können Sie es an das Ende der Liste ziehen, um es zum obersten Element im Fenster zu machen.

Nachdem wir nun die Benutzeroberfläche erstellt haben, müssen wir die Elemente der Benutzeroberfläche verfügbar machen, damit xamarin. tvos in c#-Code auf Sie zugreifen und damit interagieren kann.

### <a name="accessing-the-controls-in-the-code-behind"></a>Zugreifen auf die Steuerelemente im Code Behind

Es gibt zwei Hauptmethoden für den Zugriff auf die Steuerelemente, die Sie im IOS-Designer aus dem Code hinzugefügt haben:

- Erstellen eines Ereignis Handlers für ein Steuerelement.
- Erteilen eines Namens für das Steuerelement, damit wir später darauf verweisen können.

Wenn eines der beiden hinzugefügt wird, wird die partielle Klasse in der `ViewController.designer.cs` aktualisiert, um die Änderungen widerzuspiegeln. Auf diese Weise können Sie auf die Steuerelemente im Ansichts Controller zugreifen.

### <a name="creating-an-event-handler"></a>Erstellen eines Ereignis Handlers

Wenn in dieser Beispielanwendung auf die Schaltfläche geklickt wird, _möchten wir_ , dass ein Ereignishandler zu einem bestimmten Ereignis auf der Schaltfläche hinzugefügt werden muss. Gehen Sie folgendermaßen vor, um dies einzurichten:

1. Wählen Sie im xamarin IOS-Designer die Schaltfläche auf dem Ansichts Controller aus.
2. Wählen Sie im eigenschaftenpad die Registerkarte **Ereignisse** aus:

    [![Die Registerkarte „Ereignisse“](hello-tvos-images/event1.png)](hello-tvos-images/event1.png#lightbox)
3. Suchen Sie das touchupinside-Ereignis, und legen Sie ihm einen Ereignishandler mit dem Namen `Clicked` :

    [![Das touchupinside-Ereignis](hello-tvos-images/event2.png)](hello-tvos-images/event2.png#lightbox)
4. Wenn Sie die **Eingabe**Taste drücken, wird die Datei " **ViewController**. cs" geöffnet, die Speicherorte für den Ereignishandler im Code vorschlägt. Verwenden Sie die Pfeiltasten auf der Tastatur, um den Speicherort festzulegen:

    [![Festlegen des Speicher Orts](hello-tvos-images/event3.png)](hello-tvos-images/event3.png#lightbox)
5. Dadurch wird eine partielle Methode erstellt, wie unten dargestellt:

    [![Die partielle Methode](hello-tvos-images/event4.png)](hello-tvos-images/event4.png#lightbox)

Nun können wir Code hinzufügen, damit die Schaltfläche funktioniert.

### <a name="naming-a-control"></a>Benennen eines Steuer Elements

Wenn auf die Schaltfläche geklickt wird, sollte die Bezeichnung basierend auf der Anzahl der Klicks aktualisiert werden. Zu diesem Zweck müssen wir im Code auf die Bezeichnung zugreifen. Dies erfolgt durch Angabe eines Namens. Gehen Sie wie folgt vor:

1. Öffnen Sie das Storyboard, und wählen Sie die Bezeichnung unten im Ansichts Controller aus.
2. Wählen Sie im eigenschaftenpad die Registerkarte **Widget** aus:

    [![Registerkarte "Widget" auswählen](hello-tvos-images/name1.png)](hello-tvos-images/name1.png#lightbox)
3. Fügen Sie unter **Identity > Name**Folgendes hinzu `ClickedLabel` :

    [![Festlegen von ClickedLabel](hello-tvos-images/name2.png)](hello-tvos-images/name2.png#lightbox)

Wir können nun mit dem Aktualisieren der Bezeichnung beginnen!

### <a name="how-controls-are-accessed"></a>Zugreifen auf Steuerelemente

Wenn Sie `ViewController.designer.cs` in der **Projektmappen-Explorer** , können Sie sehen, wie die `ClickedLabel` Bezeichnung und der `Clicked` Ereignishandler einem **Outlet** und einer **Aktion** in c# zugeordnet wurden:

[![Outlets und Aktionen](hello-tvos-images/accesscontrol.png)](hello-tvos-images/accesscontrol.png#lightbox)

Sie können auch bemerken `ViewController.designer.cs` , dass eine partielle Klasse ist, sodass Visual Studio für Mac nicht ändern muss, `ViewController.cs` wodurch alle Änderungen überschrieben werden, die an der-Klasse vorgenommen wurden.

Wenn Sie die Elemente der Benutzeroberfläche auf diese Weise verfügbar machen, können Sie im Ansichts Controller darauf zugreifen.

Normalerweise müssen Sie das nicht selbst öffnen `ViewController.designer.cs` , sondern nur zu Schulungszwecken.

<a name="Writing-the-Code"></a>

## <a name="writing-the-code"></a>Schreiben des Codes

Nachdem die Benutzeroberfläche erstellt und die zugehörigen Benutzeroberflächen Elemente für den Code über **Outlets** und **Aktionen**verfügbar gemacht wurden, sind wir bereit, den Code zu schreiben, um die Programmfunktionalität zu erhalten.

In unserer Anwendung werden wir jedes Mal, wenn auf die erste Schaltfläche geklickt wird, unsere Bezeichnung aktualisieren, um anzuzeigen, wie oft auf die Schaltfläche geklickt wurde. Um dies zu erreichen, müssen wir die `ViewController.cs` Datei zur Bearbeitung öffnen, indem Sie in der **Lösungspad**auf die Datei doppelklicken:

[![Der Lösungspad](hello-tvos-images/code01.png)](hello-tvos-images/code01.png#lightbox)

Zuerst muss in unserer Klasse eine Variable auf Klassenebene erstellt werden, `ViewController` um die Anzahl der aufgetretenen Klicks zu verfolgen. Bearbeiten Sie die Klassendefinition, sodass sie danach folgendermaßen aussieht:

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

Als nächstes müssen wir in derselben Klasse ( `ViewController` ) die **viewDidLoad** -Methode außer Kraft setzen und Code hinzufügen, um die anfängliche Nachricht für unsere Bezeichnung festzulegen:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Wir müssen `ViewDidLoad` anstelle einer anderen Methode, wie z. b., verwenden, `Initialize` da `ViewDidLoad` aufgerufen wird, *nachdem* das Betriebssystem die Benutzeroberfläche aus der Datei geladen und instanziiert hat `.storyboard` . Wenn Sie versuchen, auf das Label-Steuerelement zuzugreifen, bevor die `.storyboard` Datei vollständig geladen und instanziiert wurde, wird ein Fehler ausgegeben, `NullReferenceException` da das Label-Steuerelement noch nicht erstellt wurde.

Als nächstes müssen wir den Code hinzufügen, um darauf zu reagieren, dass der Benutzer auf die Schaltfläche klickt. Fügen Sie der partiellen Klasse, die wir erstellt haben, Folgendes hinzu:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Dieser Code wird jedes Mal aufgerufen, wenn der Benutzer auf die Schaltfläche klickt.

Nachdem alles vorhanden ist, können wir unsere xamarin. tvos-Anwendung erstellen und testen.

## <a name="testing-the-application"></a>Testen der Anwendung

Es ist an der Zeit, die Anwendung zu erstellen und auszuführen, um sicherzustellen, dass Sie erwartungsgemäß ausgeführt wird. Wir können alles in einem Schritt erstellen und ausführen, oder Sie können es erstellen, ohne es auszuführen.

Wenn Sie eine Anwendung erstellen, können wir auswählen, welche Art von Build wir wünschen:

- **Debuggen** – ein Debugbuild wird in eine ""-Datei (Anwendung) mit zusätzlichen Metadaten kompiliert, mit der wir Debuggen können, was geschieht, während die Anwendung ausgeführt wird.
- **Release** – ein Releasebuild erstellt außerdem eine Datei vom Typ "", es enthält jedoch keine Debuginformationen, sodass er kleiner ist und schneller ausgeführt wird.  

Sie können den Buildtyp aus der **Konfigurations Auswahl** in der oberen linken Ecke des Visual Studio für Mac-Bildschirms auswählen:

[![Typ des Builds auswählen](hello-tvos-images/run01.png)](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Erstellen der Anwendung

In unserem Fall soll nur ein Debugbuild erstellt werden. Stellen Sie also sicher, dass **Debuggen** ausgewählt ist. Erstellen Sie zuerst die Anwendung, indem Sie **⌘ B**drücken, oder wählen Sie im Menü **Erstellen** die Option **alle erstellen**aus.

Wenn keine Fehler aufgetreten sind, wird in der Statusleiste Visual Studio für Mac eine Meldung zum erfolgreichen **Erstellen** angezeigt. Wenn Fehler aufgetreten sind, überprüfen Sie das Projekt, und stellen Sie sicher, dass Sie die Schritte ordnungsgemäß befolgt haben. Beginnen Sie mit der Bestätigung, dass der Code (sowohl in Xcode als auch in Visual Studio für Mac) mit dem Code im Tutorial übereinstimmt.

### <a name="running-the-application"></a>Ausführen der Anwendung

Zum Ausführen der Anwendung stehen drei Optionen zur Anwendung:

- Drücken **Sie ⌘ + Eingabe**Taste.
- Klicken Sie im Menü **Ausführen** auf **Debug**.
- Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche **Wiedergabe** (über dem **Projektmappen-Explorer**).

Die Anwendung wird erstellt (wenn Sie nicht bereits erstellt wurde), starten Sie im Debugmodus. der tvos-Simulator wird gestartet, und die APP wird gestartet und zeigt das Hauptfenster der Benutzeroberfläche an:

[![Der Beispiel-App-Startbildschirm](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

Wählen Sie im Menü **Hardware** die Option **Apple TV Remote anzeigen** aus, damit Sie den Simulator steuern können.

[![Wählen Sie Apple TV Remote anzeigen aus.](hello-tvos-images/run04.png)](hello-tvos-images/run04.png#lightbox)

Wenn Sie mithilfe des Remote Simulators auf die Schaltfläche mehrmals klicken, sollte die Bezeichnung mit der folgenden Anzahl aktualisiert werden:

[![Die Bezeichnung mit der aktualisierten Anzahl.](hello-tvos-images/run05.png)](hello-tvos-images/run05.png#lightbox)

Herzlichen Glückwunsch! Wir haben hier viele Grundlagen behandelt, aber wenn Sie dieses Tutorial von Anfang bis Ende befolgt haben, sollten Sie nun über ein solides Verständnis der Komponenten einer xamarin. tvos-APP und der Tools verfügen, mit denen Sie erstellt wurden.

## <a name="where-to-next"></a>Wo geht es weiter?

Die Entwicklung von Apple TV-Apps ist aufgrund der Trennung zwischen dem Benutzer und der Benutzeroberfläche (im Raum nicht im Benutzer Hand) und den Einschränkungen, die tvos in der APP-Größe und im Speicher durchführt, mit einigen Herausforderungen verbunden.

Daher wird dringend empfohlen, die folgenden Dokumente zu lesen, bevor Sie in den Entwurf einer xamarin. tvos-App springen:

- [Einführung in tvos 9](~/ios/tvos/platform/tvos9.md) – in diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in tvos 9 für xamarin. tvos-Entwickler zur Verfügung stehen.
- [Arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) – Benutzer Ihrer xamarin. tvos-App interagieren nicht direkt mit ihrer Schnittstelle wie IOS, wo Sie auf Bilder auf dem Bildschirm des Geräts tippen, aber indirekt über den Raum mithilfe von Siri. In diesem Artikel wird das Konzept des Fokus behandelt und erläutert, wie es verwendet wird, um die Navigation in einer xamarin. tvos-App-Benutzeroberfläche zu verarbeiten.
- [Siri-Remote-und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) – die Hauptmethode, mit der Benutzer mit dem Apple TV und ihrer xamarin. tvos-App interagieren, erfolgt über die enthaltene Siri-Remote Verbindung. Wenn Ihre APP ein Spiel ist, können Sie optional auch Unterstützung für Drittanbieter erstellen, die für IOS-Spiele Controller (MFI) in ihrer App erstellt wurden. In diesem Artikel wird die Unterstützung der neuen Siri-Remote-und Bluetooth-Spiele Controller in ihren xamarin. tvos-apps behandelt.
- [Ressourcen und Datenspeicherung](~/ios/tvos/app-fundamentals/resources-data-storage.md) – im Gegensatz zu IOS-Geräten bietet das neue Apple TV keinen permanenten lokalen Speicher für tvos-apps. Wenn Ihre xamarin. tvos-App beispielsweise Informationen beibehalten muss (z. b. Benutzereinstellungen), muss Sie diese Daten aus icloud speichern und abrufen. In diesem Artikel wird das Arbeiten mit Ressourcen und der persistenten Datenspeicherung in einer xamarin. tvos-App behandelt.
- [Arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) – das Erstellen von fesselnden Symbolen und Bildern ist ein wichtiger Bestandteil der Entwicklung einer immersiven Benutzererfahrung für Ihre Apple TV-apps. In diesem Leitfaden werden die erforderlichen Schritte zum Erstellen und einschließen der erforderlichen Grafik Ressourcen für Ihre xamarin. tvos-apps behandelt.
- [Benutzeroberfläche](~/ios/tvos/user-interface/index.md) – allgemeine Benutzerfreundlichkeit (UX), einschließlich Steuerelementen der Benutzeroberfläche, verwenden Sie die Interface Builder-und UX-Entwurfs Prinzipien von Xcode bei der Arbeit mit xamarin. tvos.
- [Bereitstellung und Tests](~/ios/tvos/deploy-test/index.md) – in diesem Abschnitt werden Themen behandelt, die zum Testen einer APP und zur Verteilung der APP verwendet werden. Die hier aufgeführten Themen enthalten beispielsweise Tools für das Debuggen, die Bereitstellung für Tester und das Veröffentlichen einer Anwendung im Apple TV App Store.

Wenn Sie Probleme bei der Arbeit mit xamarin. tvos haben, finden Sie in der Dokumentation zur [Problem](~/ios/tvos/troubleshooting.md) Behandlung eine Liste mit bekannten Problemen und Lösungen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel bietet einen schnellen Einstieg in die Entwicklung von Apps für tvos mit Visual Studio für Mac durch das Erstellen einer einfachen Hello-, tvos-app. Es wurden die Grundlagen der tvos-Geräte Bereitstellung, der Schnittstellen Erstellung, der Codierung für tvos und der Tests im tvos-Simulator behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Entwickeln von Apps für tvos mit xamarin (Video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
