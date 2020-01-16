---
title: 'Hallo, Mac: Exemplarische Vorgehensweise'
description: In diesem Dokument wird das Erstellen einer Xamarin.Mac-App veranschaulicht, zudem ist eine Einführung in Visual Studio für Mac, Xcode und Interface Builder enthalten. Dabei wird das Verfügbarmachen von UI-Steuerelementen für Code über Outlets und Aktionen sowie das Erstellen, Ausführen und Testen einer Xamarin.Mac-App erläutert.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 09/02/2018
ms.openlocfilehash: b56275ef903aa7def239a2e19980f52d83e6194f
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489738"
---
# <a name="hello-mac-walkthrough"></a>Hello, Mac: Exemplarische Vorgehensweise

Mit Xamarin.Mac können Sie vollständig native Mac-Apps in C# und .NET mit den gleichen macOS-APIs entwickeln, die auch bei der Entwicklung von *Objective-C* und *Swift* verwendet werden. Da Xamarin.Mac direkt in Xcode integriert wird, können Entwickler mit _Interface Builder_ von Xcode Benutzeroberflächen von Anwendungen erstellen (optional geht dies auch direkt in C#-Code).

Zusätzlich kann Code für mobile Xamarin.iOS- und Xamarin.Android-Apps freigegeben werden, während auf jeder Plattform eine native Benutzeroberfläche bereitgestellt wird. Dies liegt daran, dass Xamarin.Mac-Anwendungen in C# und .NET geschrieben sind.

In diesem Artikel werden die Hauptkonzepte vorgestellt, die zum Erstellen einer Mac-App mit Xamarin.Mac, Visual Studio für Mac und Interface Builder von Xcode wichtig sind. Zu diesem Zweck führt der Artikel Sie durch die Erstellung einer einfachen **Hallo, Mac**-App, die zählt, wie häufig auf eine Schaltfläche geklickt wird:

[![](hello-mac-images/run02-sml.png "Example of the Hello, Mac app running")](hello-mac-images/run02.png#lightbox)

Die folgenden Konzepte werden besprochen:

- **Visual Studio für Mac**: Einführung in Visual Studio für Mac und die Erstellung von Xamarin.Mac-Anwendungen mit diesem Tool.
- **Die Anatomie einer Xamarin.Mac-Anwendung**: Aufbau einer Xamarin.Mac-Anwendung
- **Interface Builder von Xcode:** Verwendung von Interface Builder von Xcode zum Definieren der App-Benutzeroberfläche
- **Outlets und Aktionen:** Verwendung von Outlets und Aktionen zum Verbinden von Steuerelementen auf der Benutzeroberfläche
- **Bereitstellung/Tests**: So führen Sie eine Xamarin.Mac-App aus und testen diese.

## <a name="requirements"></a>Anforderungen

Zum Entwickeln von Xamarin.Mac-Anwendungen benötigen Sie Folgendes:

- Mac-Computer unter macOS High Sierra (10.13) oder höher.
- [Xcode 10 oder höher](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
- Die neueste Version von [Xamarin.Mac und Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation/).

Zum Ausführen einer mit Xamarin.Mac erstellten Anwendung benötigen Sie Folgendes:

- Einen Mac-Computer mit macOS 10.7 oder höher

> [!WARNING]
> Die anstehende Version Xamarin.Mac 4.8 unterstützt nur macOS 10.9 oder höher.
> Ältere Versionen von Xamarin.Mac unterstützen macOS 10.7 oder höher, aber diese älteren Versionen von MacOS verfügen nicht über ausreichende TLS-Infrastruktur zur Unterstützung von TLS 1.2. Für macOS 10.7 oder macOS 10.8 sollten Sie Xamarin.Mac 4.6 oder niedriger verwenden.

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Starten einer neuen Xamarin.Mac-App in Visual Studio für Mac

Wie oben erwähnt führt dieser Leitfaden Sie durch die Schritte zum Erstellen einer Mac-App mit dem Namen `Hello_Mac`, durch die eine einzelne Schaltfläche und eine Beschriftung im Hauptfenster hinzugefügt wird. Wenn Sie auf die Schaltfläche klicken, zeigt die Beschriftung an, wie oft sie bereits geklickt wurde.

So beginnen Sie

1. Starten Sie Visual Studio für Mac:

    [![](hello-mac-images/setup01-sml.png "The main Visual Studio for Mac interface")](hello-mac-images/setup01.png#lightbox)

2. Klicken Sie auf die Schaltfläche **Neues Projekt...** , um das Dialogfeld **Neues Projekt** zu öffnen, wählen Sie dann **Mac** > **App** > **Cocoa-App** aus, und klicken Sie auf die Schaltfläche **Weiter**:

    [![](hello-mac-images/setup02-sml.png "Selecting a Cocoa App")](hello-mac-images/setup02.png#lightbox)

3. Geben Sie als **App-Namen**`Hello_Mac` ein, und behalten Sie für alles andere die Standardwerte bei. Klicken Sie auf **Weiter**:

    [![](hello-mac-images/setup03-sml.png "Setting the name of the app")](hello-mac-images/setup03.png#lightbox)

4. Bestätigen Sie den Speicherort des neuen Projekts auf Ihrem Computer:

    [![](hello-mac-images/setup04-sml.png "Verifying the new solution details")](hello-mac-images/setup04.png#lightbox)

5. Klicken Sie auf die Schaltfläche **Erstellen**.

Visual Studio für Mac erstellt die neue Xamarin.Mac-App und zeigt die Standarddateien an, die der Projektmappe der App hinzugefügt werden:

[![](hello-mac-images/project01-sml.png "The new solution default view")](hello-mac-images/project01.png#lightbox)

Visual Studio für Mac verwendet die gleiche Struktur für **Projektmappen** und **Projekte** wie Visual Studio 2019. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthalten kann. Projekte sind z.B. Anwendungen, unterstützende Bibliotheken, Testanwendungen usw. Die Vorlage **Datei > Neues Projekt** erstellt automatisch eine Projektmappe und ein Anwendungsprojekt.

## <a name="anatomy-of-a-xamarinmac-application"></a>Aufbau einer Xamarin.Mac-Anwendung

Die Programmierung von Xamarin.Mac-Anwendungen ähnelt der von Xamarin.iOS. iOS verwendet das CocoaTouch-Framework, eine reduzierte Version von Cocoa, die von Mac verwendet wird.

Schauen Sie sich die Dateien im Projekt an:

- **Main.cs** enthält den Haupteinstiegspunkt für die Anwendung. Wenn die App gestartet wird, enthält die `Main`-Klasse die erste Methode, die ausgeführt wird.
- **AppDelegate.cs** enthält die `AppDelegate`-Klasse, die auf Ereignisse des Betriebssystems lauscht.
- **Info.plist** enthält App-Eigenschaften wie den Anwendungsnamen, ihr Symbol usw.
- **Entitlements.plist** enthält die Berechtigungen der Anwendung und ermöglicht den Zugriff auf z.B. Sandboxing- und iCloud-Support.
- **Main.storyboard** definiert die Benutzeroberfläche (Fenster und Menüs) einer App und beschreibt die Verbindungen zwischen Fenstern mit Segues. Storyboards sind XML-Dateien, die eine Definition der Ansichten enthalten (Benutzeroberflächenelemente). Diese Datei kann von Interface Builder in Xcode erstellt und verwaltet werden.
- **ViewController.cs** ist der Controller des Hauptfensters. Controller werden in einem anderen Artikel ausführlicher besprochen. Für den Moment reicht es, sich den Controller als Haupt-Engine einer jeden Ansicht vorzustellen.
- **ViewController.designer.cs** enthält Grundlagencode, der bei der Integration in die Benutzeroberfläche des Hauptbildschirms hilft.

In den folgenden Abschnitten werden diese Dateien kurz behandelt. Sie werden später ausführlicher besprochen. Allerdings wird empfohlen, sich jetzt schon grundlegend mit ihnen vertraut zu machen.

### <a name="maincs"></a>Main.cs

Die Datei **main.cs** ist sehr einfach aufgebaut. Sie enthält eine statische `Main`-Methode, die eine neue Instanz einer Xamarin.Mac-App erstellt und den Namen der Klasse zurückgibt, die das OS-Ereignis behandelt. In diesem Fall ist dies die Klasse `AppDelegate`:

```csharp
using System;
using System.Drawing;
using Foundation;
using AppKit;
using ObjCRuntime;

namespace Hello_Mac
{
    class MainClass
    {
        static void Main (string[] args)
        {
            NSApplication.Init ();
            NSApplication.Main (args);
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

Die `AppDelegate.cs`-Datei enthält eine `AppDelegate`-Klasse, die für das Erstellen von Fenstern und das Lauschen auf OS-Ereignisse verantwortlich ist:

```csharp
using AppKit;
using Foundation;

namespace Hello_Mac
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Dieser Code ist wahrscheinlich unbekannt, es sei denn, der Entwickler hat schon einmal eine iOS-App entwickelt. Er ist jedoch recht einfach.

Die `DidFinishLaunching`-Methode wird ausgeführt, nachdem die App instanziiert wurde. Sie ist dafür verantwortlich, das Fenster der App zu erstellen und die Ansicht in dieser anzuzeigen.

Die `WillTerminate`-Methode wird aufgerufen, wenn der Benutzer oder das System das Beenden der App instanziiert hat. Der Entwickler sollte diese Methode verwenden, um die App fertig zu stellen, bevor sie beendet wird (wie z.B. das Speichern der Benutzereinstellungen oder die Fenstergröße und -stelle).

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (und das davon abgeleitete CocoaTouch) verwendet das sogenannte *MVC*-Muster (Model View Controller). Die `ViewController`-Deklaration stellt das Objekt dar, das das App-Fenster steuert. Für jedes erstellte Fenster (und für viele der Elemente in Fenstern) gibt es einen Controller, der für den Lebenszyklus eines Fensters verantwortlich ist, z.B. für das Anzeigen oder das Hinzufügen von neuen Ansichten (Steuerelementen).

Die `ViewController`-Klasse ist der Controller des Hauptfenster. Der Controller ist für den Lebenszyklus des Hauptfensters verantwortlich. Dies wird später ausführlicher diskutiert. Schauen Sie sich nun aber zunächst dies an:

```csharp
using System;

using AppKit;
using Foundation;

namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }
    }
}
```

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Die Designerdatei für die Hauptfensterklasse ist anfangs leer, wird aber automatisch von Visual Studio für Mac aufgefüllt, wenn die Benutzeroberfläche mit Xcode Interface Builder erstellt wird:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac to store outlets and
// actions made in the UI designer. If it is removed, they will be lost.
// Manual changes to this file may not be handled correctly.
//
using Foundation;

namespace Hello_Mac
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

Designerdateien sollten nicht direkt bearbeitet werden, da sie automatisch von Visual Studio für Mac verwaltet werden, um den Grundlagencode bereitzustellen, der den Zugriff auf Steuerelemente ermöglicht, die Fenstern oder Ansichten in der App hinzugefügt wurden.

Wenn das Xamarin.Mac-App-Projekt erstellt wurde und Sie ein grundlegendes Verständnis seiner Komponenten haben, wechseln Sie zu Xcode, um die Benutzeroberfläche mit Interface Builder zu erstellen.

### <a name="infoplist"></a>Info.plist

Die `Info.plist`-Datei enthält Informationen zur Xamarin.Mac-App wie z.B. deren **Namen** und die **Bundle-ID**:

[![](hello-mac-images/infoplist01.png "The Visual Studio for Mac plist editor")](hello-mac-images/infoplist01.png#lightbox)

Zudem definiert sie das _Storyboard_, mit dem die Benutzeroberfläche für die Xamarin.Mac-App im Dropdownmenü **Main Interface** (Hauptschnittstelle) angezeigt wird. Im obigen Beispiel bezieht sich `Main` im Dropdownmenü auf `Main.storyboard` in der Quellstruktur des Projekts im **Projektmappen-Explorer**. Außerdem definiert die Datei das Symbol der App, indem sie den *Ressourcenkatalog* angibt, der diese enthält (in diesem Fall **AppIcon**).

### <a name="entitlementsplist"></a>Entitlements.plist

Die Berechtigungen der `Entitlements.plist`-Dateisteuerelemente der App, über die die Xamarin.Mac-App verfügt, wie z.B. **Sandboxing** und **iCloud**:

[![](hello-mac-images/entitlements01.png "The Visual Studio for Mac entitlements editor")](hello-mac-images/entitlements01.png#lightbox)

Für das Hallo Welt-Beispiel benötigen Sie keine Berechtigungen. Im nächsten Abschnitt erfahren Sie, wie Sie Interface Builder von Xcode verwenden, um die **Main.storyboard**-Datei zu bearbeiten und die Benutzeroberfläche der Xamarin.Mac-App zu definieren.

## <a name="introduction-to-xcode-and-interface-builder"></a>Einführung in Xcode und Interface Builder

Das Tool „Interface Builder“ wurde von Apple für Xcode entwickelt, damit Entwickler eine Benutzeroberfläche visuell im Designer erstellen können. Xamarin.Mac kann ganz natürlich mit Interface Builder zusammenarbeiten, sodass die UI mit den gleichen Tools entwickelt werden kann, die auch von Objective-C-Benutzern verwendet werden.

Doppelklicken Sie im **Projektmappen-Explorer** auf die `Main.storyboard`-Datei, um diese zur Bearbeitung in Xcode und Interface Builder zu öffnen.

[![](hello-mac-images/xcode01.png "The Main.storyboard file in the Solution Explorer")](hello-mac-images/xcode01.png#lightbox)

Daraufhin sollte Xcode gestartet werden, wie in diesem Screenshot angezeigt:

[![](hello-mac-images/xcode02.png "The default Xcode Interface Builder view")](hello-mac-images/xcode02.png#lightbox)

Bevor Sie mit dem Entwerfen der Oberfläche beginnen, machen Sie sich mit Xcode und den Hauptfunktionen vertraut, die verwendet werden.

> [!NOTE]
> Entwickler müssen nicht auf Xcode und Interface Builder zurückgreifen, um die Benutzeroberfläche einer Xamarin.Mac-App zu erstellen. Sie können dies auch direkt in C#-Code tun. Dies übersteigt jedoch den Umfang dieses Artikels. Der Einfachheit halber wird im Rest dieses Tutorials Interface Builder zum Erstellen der UI verwendet.

### <a name="components-of-xcode"></a>Komponenten von Xcode

Wenn Sie eine **.storyboard**-Datei aus Visual Studio für Mac in Xcode öffnen, wird Folgendes angezeigt: ein **Projektnavigator** auf der linken Seite, die **Schnittstellenhierarchie** und der **Schnittstellen-Editor** in der Mitte sowie der Bereich **Eigenschaften und Hilfsprogramme** auf der rechten Seite:

[![](hello-mac-images/xcode03.png "The various sections of Interface Builder in Xcode")](hello-mac-images/xcode03.png#lightbox)

In den folgenden Abschnitten wird diskutiert, wie diese Xcode-Funktionen funktionieren und wie Sie sie verwenden können, um eine Benutzeroberfläche für eine Xamarin.Mac-App zu erstellen.

### <a name="project-navigation"></a>Projektnavigation

Wenn Sie eine **.storyboard**-Datei zur Bearbeitung in Xcode öffnen, erstellt Visual Studio für Mac im Hintergrund eine *Xcode-Projektdatei*, um mit Xcode Änderungen auszutauschen. Wenn der Entwickler später von Xcode zurück zu Visual Studio für Mac wechselt, werden alle an diesem Projekt vorgenommenen Änderungen von Visual Studio für Mac mit dem Xamarin.Mac-Projekt synchronisiert.

Im Abschnitt **Projektnavigation** kann der Entwickler durch alle Dateien navigieren, aus denen dieses _Shim_-Xcode-Projekt besteht. Normalerweise haben Sie nur Interesse an den `.storyboard`-Dateien in dieser Liste wie z.B. `Main.storyboard`.

### <a name="interface-hierarchy"></a>Schnittstellenhierarchie

Im Abschnitt **Interface Hierarchy** (Schnittstellenhierarchie) können Entwickler leicht auf verschiedene Schlüsseleigenschaften der Benutzeroberfläche zugreifen, z.B. deren **Platzhalter** und **Hauptfenster**. Über diesen Abschnitt besteht auch Zugriff auf die einzelnen Elemente (Ansichten), aus denen die Benutzeroberfläche besteht. Außerdem lässt sich deren Schachtelung anpassen, indem Sie sie in der Hierarchie an eine andere Stelle ziehen.

### <a name="interface-editor"></a>Schnittstellen-Editor

Im Abschnitt **Interface Editor** (Schnittstellen-Editor) finden Sie die Oberfläche, auf der die Benutzeroberfläche grafisch angeordnet wird. Ziehen Sie Elemente aus dem Abschnitt **Bibliothek** des Abschnitts **Eigenschaften & Dienstprogramme**, um den Entwurf zu erstellen. Wenn Sie UI-Elemente (Ansichten) der Entwurfsoberfläche hinzufügen, werden Sie auch im Abschnitt **Schnittstellenhierarchie** hinzugefügt, und zwar in der Reihenfolge, in der Sie im **Schnittstellen-Editor** aufgelistet sind.

### <a name="properties--utilities"></a>Eigenschaften und Dienstprogramme

Der Abschnitt **Eigenschaften & Dienstprogramme** ist in zwei Hauptabschnitte aufgeteilt: **Eigenschaften** (auch als Inspektoren bezeichnet) und **Bibliothek**:

[![](hello-mac-images/xcode04.png "The Properties Inspector")](hello-mac-images/xcode04.png#lightbox)

Zunächst ist dieser Abschnitt nahezu leer. Wenn der Entwickler jedoch ein Element im **Schnittstellen-Editor** oder der **Schnittstellenhierarchie** auswählt, wird der Abschnitt **Eigenschaften** mit Informationen zu diesem Element und Eigenschaften, die angepasst werden können, aufgefüllt.

Im Abschnitt **Eigenschaften** gibt es wie in der folgenden Abbildung dargestellt acht verschiedene *Inspector Tabs* (Inspektorregisterkarten):

[![](hello-mac-images/xcode05.png "An overview of all Inspectors")](hello-mac-images/xcode05.png#lightbox)

### <a name="properties--utility-types"></a>Eigenschaften- und Dienstprogrammtypen

Es gibt die folgenden Registerkarten (von links nach rechts):

- **Dateiinspektor**: Der Dateiinspektor zeigt Dateiinformationen an, wie z.B. den Dateinamen der XIB-Datei, die bearbeitet wird.
- **Schnellhilfe**: Die Registerkarte „Schnellhilfe“ bietet Kontexthilfe, je nachdem, was in Xcode ausgewählt ist.
- **Identitätsinspektor**: Der Identitätsinspektor bietet Informationen zu dem ausgewählten Steuerelement bzw. der ausgewählten Ansicht.
- **Attributinspektor**: Mit dem Attributinspektor kann der Entwickler verschiedene Attribute des ausgewählten Steuerelements bzw. der ausgewählten Ansicht anpassen.
- **Größeninspektor**: Mit dem Größeninspektor kann der Entwickler die Größe und das Größenänderungsverhalten des ausgewählten Steuerelements bzw. der ausgewählten Ansicht steuern.
- **Verbindungsinspektor**: Der Verbindungsinspektor zeigt die **Outlet**- und **Aktionsverbindungen** des ausgewählten Steuerelements an. Outlets und Aktionen werden weiter unten ausführlich vorgestellt.
- **Bindungsinspektor**: Mit dem Bindungsinspektor können Entwickler Steuerelemente so konfigurieren, dass deren Werte automatisch an Datenmodelle gebunden werden.
- **Ansichteffektinspektor**: Mit dem Ansichteffektinspektor können Entwickler Effekte für die Steuerelemente angeben, wie z.B. Animationen.

Verwenden Sie den Abschnitt **Bibliothek**, um Steuerelemente und Objekte zu finden, die Sie im Designer einfügen können, um die Benutzeroberfläche graphisch zu erstellen:

[![](hello-mac-images/xcode06.png "The Xcode Library Inspector")](hello-mac-images/xcode06.png#lightbox)

## <a name="creating-the-interface"></a>Erstellen der Schnittstelle

Jetzt wo die Grundlagen der Xcode-IDE und von Interface Builder geklärt wurden, kann der Entwickler die Benutzeroberfläche für das Hauptfenster erstellen.

So verwenden Sie Interface Builder

1. Ziehen Sie in Xcode eine **Befehlsschaltfläche** aus dem **Bibliotheksbereich**:

    [![](hello-mac-images/xcode07.png "Selecting a NSButton from the Library Inspector")](hello-mac-images/xcode07.png#lightbox)

2. Fügen Sie die Schaltfläche in die **Ansicht** (unter dem **Fenstercontroller**) im **Schnittstellen-Editor** ein:

    [![](hello-mac-images/xcode08.png "Adding a Button to the interface design")](hello-mac-images/xcode08.png#lightbox)

3. Klicken Sie auf die Eigenschaft **Titel** im **Attributinspektor**, und ändern Sie den Titel der Schaltfläche in **Click Me**:

    [![](hello-mac-images/xcode09.png "Setting the button's properties")](hello-mac-images/xcode09.png#lightbox)

4. Ziehen Sie eine **Bezeichnung** aus dem **Bibliotheksbereich**:

    [![](hello-mac-images/xcode10.png "Selecting a Label from the Library Inspector")](hello-mac-images/xcode10.png#lightbox)

5. Fügen Sie die Bezeichnung im **Fenster** neben der Schaltfläche im **Schnittstellen-Editor** ein:

    [![](hello-mac-images/xcode11.png "Adding a Label to the Interface Design")](hello-mac-images/xcode11.png#lightbox)

6. Klicken Sie auf den rechten Ziehpunkt der Bezeichnung, und ziehen Sie diesen nach rechts, bis der Rand des Fensters erreicht ist:

    [![](hello-mac-images/xcode12.png "Resizing the Label")](hello-mac-images/xcode12.png#lightbox)

7. Wählen Sie die gerade hinzugefügte Schaltfläche im **Schnittstellen-Editor** aus, und klicken sie auf das Symbol des **Einschränkungs-Editors** am unteren Rand des Fensters:

    [![](hello-mac-images/xcode13.png "Adding constraints to the button")](hello-mac-images/xcode13.png#lightbox)

8. Klicken Sie oben im Editor auf den oberen und den linken **roten Balken**. Wenn Sie die Größe des Fensters anpassen, bleibt die Schaltfläche an der gleichen Stelle oben links im Bildschirm.

9. Aktivieren Sie als Nächstes die Kontrollkästchen für die **Höhe** und **Breite**, und verwenden Sie die Standardwerte. Dadurch bleibt die Größe der Schaltfläche unverändert, wenn die Größe des Fensters geändert wird.

10. Klicken Sie auf die Schaltfläche **Add 4 Contraints** (4 Einschränkungen hinzufügen), um die Einschränkungen hinzuzufügen und den Editor zu schließen.

11. Wählen Sie die Bezeichnung aus, und klicken Sie erneut auf das Symbol des **Einschränkungs-Editors**:

    [![](hello-mac-images/xcode14.png "Adding constraints to the label")](hello-mac-images/xcode14.png#lightbox)

12. Wenn Sie auf den oberen, rechten und linken **roten Balken** im **Einschränkungs-Editor** klicken, bleibt die Bezeichnung an ihrer ursprünglichen Stelle und wird nur vergrößert oder verkleinert, wenn die Größe des Fensters in der ausgeführten App geändert wird.

13. Aktivieren Sie erneut das Kontrollkästchen **Höhe**, und verwenden Sie den Standardwert. Klicken Sie anschließend auf **Add 4 Constraints** (4 Einschränkungen hinzufügen), um die Einschränkungen hinzuzufügen und den Editor zu schließen.

14. Speichern Sie die Änderungen an der Benutzeroberfläche.

Beachten Sie, dass Interface Builder beim Ändern der Größe und beim Bewegen von Steuerelementen hilfreiche Hinweise gibt, die auf [Eingaberichtlinien für macOS](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/) basieren. Diese Richtlinien helfen dem Entwickler dabei, qualitativ hochwertige Apps zu erstellen, die Mac-Benutzern sowohl optisch als auch bei der Verwendung vertraut sind.

Sehen Sie sich den Bereich **Interface Hierarchy** (Schnittstellenhierarchie) an, um zu sehen, wie das Layout und die Hierarchie von den Elementen dargestellt werden, aus denen die Benutzeroberfläche besteht:

[![](hello-mac-images/xcode15.png "Selecting an element in the Interface Hierarchy")](hello-mac-images/xcode15.png#lightbox)

Hier kann der Entwickler Elemente auswählen, die er bearbeiten möchte, oder UI-Elemente verschieben, um deren Anordnung anzupassen. Wenn ein UI-Elemente z.B. von einem anderen Element verdeckt wird, können sie dieses ans Ende der Liste ziehen, um es im Fenster ganz nach oben zu verschieben.

Wenn die Benutzeroberfläche erstellt wurde, muss der Entwickler die UI-Elemente verfügbar machen, damit Xamarin.Mac auf diese zugreifen und mit ihnen in C#-Code interagieren kann. Dies wird im nächsten Abschnitt, **Outlets und Aktionen**, erklärt.

### <a name="outlets-and-actions"></a>Outlets und Aktionen

Was sind **Outlets** und **Aktionen**? Bei der traditionellen .NET-Programmierung wird ein Steuerelement auf der Benutzeroberfläche automatisch als Eigenschaft verfügbar gemacht, wenn es hinzugefügt wird. Unter Mac ist dies anders. Wenn Sie ein Steuerelement zu einer Ansicht hinzufügen, wird es dadurch nicht automatisch für Code zugänglich. Der Entwickler muss die UI-Elemente explizit für Code verfügbar machen. Dafür bietet Apple zwei Optionen:

- **Outlets**: Outlets sind vergleichbar mit Eigenschaften. Wenn ein Entwickler ein Steuerelement mit einem Outlet verbindet, wird es für den Code über eine Eigenschaft verfügbar gemacht, sodass z.B. Ereignishandler und Aufrufmethoden usw. hinzugefügt werden können.
- **Aktionen**: Aktionen sind vergleichbar mit dem Befehlsmuster in WPF. Wenn z.B. eine Aktion für ein Steuerelement ausgeführt wird, wie etwa das Klicken auf eine Schaltfläche, ruft das Steuerelement automatisch eine Methode im Code auf. Aktionen sind leistungsstark und praktisch, da der Entwickler viele Steuerelemente mit der gleichen Aktion verbinden kann.

In Xcode werden **Outlets** und **Aktionen** direkt über das *Ziehen von Steuerelementen* in den Code eingefügt. Genauer gesagt bedeutet dies, dass der Entwickler zum Erstellen eines **Outlets** oder einer **Aktion** ein Steuerelement auswählen muss, dem ein **Outlet** oder eine **Aktion** hinzugefügt werden soll. Dann muss er die **STRG-TASTE** gedrückt halten und das Steuerelement direkt in den Code ziehen.

Dies bedeutet für Xamarin.Mac-Entwickler, dass die Entwickler in die Objective-C-Stub-Dateien ziehen, die den C#-Dateien entsprechen, in denen Sie das **Outlet** oder die **Aktion** erstellen möchten. Visual Studio für Mac hat eine Datei namens `ViewController.h` als Teil des Shim-Xcode-Projekts erstellt, das zur Verwendung von Interface Builder generiert wurde:

[![](hello-mac-images/xcode16-sml.png "Viewing source in Xcode")](hello-mac-images/xcode16.png#lightbox)

Diese Stub-`.h`-Datei spiegelt die `ViewController.designer.cs`-Datei wider, die automatisch dem Xamarin.Mac-Projekt hinzugefügt wird, wenn ein neues `NSWindow` erstellt wird. Diese Datei wird verwendet, um die von Interface Builder vorgenommenen Änderungen zu synchronisieren. In ihr werden auch die **Outlets** und **Aktionen** erstellt, damit UI-Elemente für den C#-Code verfügbar gemacht werden können.

#### <a name="adding-an-outlet"></a>Hinzufügen eines Outlets

Jetzt wo Sie ein grundlegendes Verständnis von **Outlets** und **Aktionen** haben, erstellen Sie ein **Outlet**, um die erstellte Bezeichnung für unseren C#-Code verfügbar zu machen.

Führen Sie folgende Schritte aus:

1. Klicken Sie in der rechten oberen Ecke in Xcode auf die Schaltfläche mit den **zwei Kreisen**, um den **Assistenten-Editor** zu öffnen:

    [![](hello-mac-images/outlet01.png "Displaying the Assistant Editor")](hello-mac-images/outlet01.png#lightbox)

2. Xcode wechselt in eine geteilte Ansicht mit dem **Schnittstellen-Editor** auf der einen und dem **Code-Editor** auf der anderen Seite.

3. Beachten Sie, dass Xcode automatisch die Datei **ViewController.m** im **Code-Editor** ausgewählt hat, was nicht korrekt ist. Wie oben unter **Outlets** und **Aktionen** erwähnt, muss der Entwickler **ViewController.h** auswählen.

4. Klicken Sie oben im **Code-Editor** auf die **automatische Verknüpfung**, und wählen Sie die Datei `ViewController.h` aus:

    [![](hello-mac-images/outlet02.png "Selecting the correct file")](hello-mac-images/outlet02.png#lightbox)

5. Xcode sollte jetzt die korrekte Datei ausgewählt haben:

    [![](hello-mac-images/outlet03.png "Viewing the ViewController.h file")](hello-mac-images/outlet03.png#lightbox)

6. **Der letzte Schritt war sehr wichtig.** Wenn Sie hier nicht die richtige Datei auswählen, können Sie keine **Outlets** und **Aktionen** erstellen, oder diese werden für die falsche Klasse in C# verfügbar gemacht.

7. Halten Sie im **Schnittstellen-Editor** die **STRG-TASTE** gedrückt, und ziehen Sie die oben erstellte Bezeichnung im Code-Editor unter den `@interface ViewController : NSViewController {}`-Code:

    [![](hello-mac-images/outlet04.png "Dragging to create an Outlet")](hello-mac-images/outlet04.png#lightbox)

8. Es wird ein Dialogfeld geöffnet. Belassen Sie die **Verbindung** bei **Outlet**, und geben Sie für **Name**`ClickedLabel` ein:

    [![](hello-mac-images/outlet05.png "Defining the Outlet")](hello-mac-images/outlet05.png#lightbox)

9. Klicken Sie auf **Verbinden**, um das **Outlet** zu erstellen:

    [![](hello-mac-images/outlet06.png "Viewing the final Outlet")](hello-mac-images/outlet06.png#lightbox)

10. Speichern Sie die Änderungen in der Datei.

#### <a name="adding-an-action"></a>Hinzufügen einer Aktion

Machen Sie als Nächstes die Schaltfläche für C#-Code verfügbar. Der Entwickler kann die Schaltfläche genauso wie oben die Bezeichnung mit einem **Outlet** verbinden. Da wir nur eine Reaktion möchten, wenn auf die Schaltfläche geklickt wird, verwenden Sie stattdessen eine **Aktion**.

Führen Sie folgende Schritte aus:

1. Stellen Sie sicher, dass sich Xcode immer noch im **Assistenten-Editor** befindet und dass die Datei **ViewController.h** im **Code-Editor** zu sehen ist.

2. Halten Sie im **Schnittstellen-Editor** die **STRG-TASTE** gedrückt, und ziehen Sie die oben erstellte Schaltfläche im Code-Editor unter den `@property (assign) IBOutlet NSTextField *ClickedLabel;`-Code:

    [![](hello-mac-images/action01.png "Dragging to create an Action")](hello-mac-images/action01.png#lightbox)

3. Ändern Sie den **Verbindungstyp** in **Aktion**:

    [![](hello-mac-images/action02.png "Defining the Action")](hello-mac-images/action02.png#lightbox)

4. Geben Sie `ClickedButton` als **Namen** ein:

    [![](hello-mac-images/action03.png "Naming the new Action")](hello-mac-images/action03.png#lightbox)

5. Klicken Sie auf **Verbinden**, um die **Aktion** zu erstellen:

    [![](hello-mac-images/action04.png "Viewing the final Action")](hello-mac-images/action04.png#lightbox)

6. Speichern Sie die Änderungen in der Datei.

Wenn die Benutzeroberfläche verbunden und für C#-Code verfügbar gemacht wurde, wechseln Sie zurück zu Visual Studio für Mac, und synchronisieren Sie die in Xcode und Interface Builder vorgenommenen Änderungen.

> [!NOTE]
> Wahrscheinlich hat die Erstellung dieser Benutzeroberfläche sowie der **Outlets** und **Aktionen** viel Zeit in Anspruch genommen, und es kommt Ihnen so vor, als wäre sehr viel Arbeit gewesen. Es wurden jedoch sehr viele neue Konzepte eingeführt und viel Neues erklärt. Wenn Sie dies mehrmals gemacht und sich mit Interface Builder vertraut gemacht haben, erstellen Sie eine Oberfläche mit all Ihren **Outlets** und **Aktionen** in wenigen Minuten.

### <a name="synchronizing-changes-with-xcode"></a>Synchronisieren von Änderungen mit Xcode

Wenn der Entwickler von Xcode zurück zu Visual Studio für Mac wechselt, werden alle in Xcode an diesem Projekt vorgenommenen Änderungen automatisch mit dem Xamarin.Mac-Projekt synchronisiert.

Wählen Sie die Datei **ViewController.designer.cs** im **Projektmappen-Explorer** aus, um zu sehen, wie das **Outlet** und die **Aktion** in C#-Code verbunden wurden:

[![](hello-mac-images/sync01-sml.png "Synchronizing changes with Xcode")](hello-mac-images/sync01.png#lightbox)

Beachten Sie die zwei Definitionen in **ViewController.designer.cs**:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Vergleich mit den Definitionen in der `ViewController.h`-Datei in Xcode:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Visual Studio für Mac lauscht auf Änderungen an der **.h**-Datei und synchronisiert diese automatisch mit der entsprechenden **.designer.cs**-Datei, um die Änderungen für die App verfügbar zu machen. Beachten Sie, dass **ViewController.designer.cs** eine Teilklasse ist, sodass Visual Studio für Mac **ViewController.cs** nicht anpassen muss, wodurch alle Änderungen des Entwicklers überschrieben würden.

Normalerweise muss der Entwickler **ViewController.designer.cs** nicht öffnen. Diese wurde hier zum besseren Verständnis vorgestellt.

> [!NOTE]
> In den meisten Fällen erkennt Visual Studio für Mac automatisch alle in Xcode vorgenommenen Änderungen und synchronisiert diese mit dem Xamarin.Mac-Projekt. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.

## <a name="writing-the-code"></a>Schreiben des Codes

Wenn die Benutzeroberfläche erstellt und ihre UI-Elemente im Code über **Outlets** und **Aktionen** verfügbar gemacht wurden, können Sie nun den Code schreiben, durch den das Programm erstellt wird.

Für diese App wird die Bezeichnung jedes Mal aktualisiert, wenn auf die erste Schaltfläche geklickt wird, sodass sie immer die Zahl an bereits durchgeführten Klicks anzeigt. Öffnen Sie dazu die `ViewController.cs`-Datei zur Bearbeitung, indem Sie auf diese in **Projektmappen-Explorer** doppelklicken:

[![](hello-mac-images/code01-sml.png "Viewing the ViewController.cs file in Visual Studio for Mac")](hello-mac-images/code01.png#lightbox)

Erstellen Sie zunächst eine Variable auf Klassenebene in der `ViewController`-Klasse, um die Zahl an ausgeführten Klicks zu erfassen. Bearbeiten Sie die Klassendefinition, sodass sie danach folgendermaßen aussieht:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Überschreiben Sie als Nächstes in derselben Klasse (`ViewController`) die `ViewDidLoad`-Methode, und fügen Sie Code hinzu, um die erste Meldung für die Bezeichnung anzugeben:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Verwenden Sie `ViewDidLoad` statt einer anderen Methode (wie etwa `Initialize`), da `ViewDidLoad` aufgerufen wird, *nachdem* das Betriebssystem die Benutzeroberfläche aus der **STORYBOARD**-Datei geladen und instanziiert hat. Wenn der Entwickler versucht, auf das Label-Steuerelement zuzugreifen, bevor die **STORYBOARD**-Datei vollständig geladen wurde, erhält er einen `NullReferenceException`-Fehler, da dieses Steuerelement noch nicht erstellt wurde.

Fügen Sie als nächstes den Code hinzu, der auf das Klicken der Schaltfläche reagiert. Fügen Sie der `ViewController`-Klasse folgende Teilklasse hinzu:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {
    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Dieser Code wird an die in Xcode und Interface Builder erstellte **Aktion** angefügt und wird immer dann aufgerufen, wenn der Benutzer auf die Schaltfläche klickt.

## <a name="testing-the-application"></a>Testen der Anwendung

Erstellen Sie nun die App und führen Sie diese aus, um zu testen, ob sie wie erwartet ausgeführt wird. Der Entwickler kann das Erstellen und Ausführen in einem Schritt durchführen, oder er kann die App erstellen, ohne sie auszuführen.

Wenn eine App erstellt wird, kann sich der Entwickler für die Buildart entscheiden:

- **Debug**: Ein Debugbuild wird in eine **APP**-Datei (Anwendung) mit einigen zusätzlichen Metadaten kompiliert, durch die der Entwickler ein Debugging durchführen kann, während die App ausgeführt wird.
- **Release**: Ein Releasebuild erstellt auch eine **APP**-Datei, aber diese enthält keine Debugginginformationen, weshalb sie kleiner ist und schneller ausgeführt werden kann.

Der Entwickler kann den Buildtyp unter **Konfigurationsauswahl** oben links im Visual Studio für Mac-Bildschirm auswählen:

[![](hello-mac-images/run01-sml.png "Selecting a Debug build")](hello-mac-images/run01.png#lightbox)

## <a name="building-the-application"></a>Erstellen der Anwendung

Im Falle dieses Beispiels ist ein Debugbuild erforderlich. Achten Sie also darauf, dass **Debug** ausgewählt ist. Erstellen Sie zunächst die App, indem Sie entweder auf **⌘B** drücken oder im **Buildmenü** auf **Build all** (Alle erstellen) klicken.

Wenn keine Fehler auftreten, wird in der Statusleiste von Visual Studio für Mac die Meldung **Buildvorgang erfolgreich** angezeigt. Wenn Fehler auftreten, überprüfen Sie das Projekt, und achten Sie darauf, dass Sie die oben stehenden Schritte richtig ausgeführt haben. Stellen Sie zunächst sicher, dass der Code (sowohl in Xcode als auch in Visual Studio für Mac) mit dem Code im Tutorial übereinstimmt.

## <a name="running-the-application"></a>Ausführen der Anwendung

Es gibt drei Möglichkeiten, die App auszuführen:

- Drücken Sie **⌘+EINGABE**.
- Klicken Sie im Menü **Ausführen** auf **Debug**.
- Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche **Wiedergabe** (über dem **Projektmappen-Explorer**).

Die App wird erstellt (wenn dies noch nicht geschehen ist), im Debugmodus gestartet und zeigt das Hauptfenster der Benutzeroberfläche an:

[![](hello-mac-images/run02-sml.png "Running the application")](hello-mac-images/run02.png#lightbox)

Wenn Sie mehrmals auf die Schaltfläche klicken, sollte die Zahl der Bezeichnung aktualisiert werden:

[![](hello-mac-images/run03-sml.png "Showing the results of clicking the button")](hello-mac-images/run03.png#lightbox)

## <a name="where-to-next"></a>Weitere Themen

Jetzt wo Sie die Grundlagen einer Xamarin.Mac-App kennen, sehen Sie sich die folgenden Dokumentationen an, um Ihre Kenntnisse auszuweiten:

- [Einführung in Storyboards](~/mac/platform/storyboards/index.md): In diesem Artikel erhalten Sie eine Einführung in die Arbeit mit Storyboards in einer Xamarin.Mac-App. Sie erfahren, wie Sie die UI der App mit Storyboards und Interface Builder von Xcode erstellen und verwalten.
- [Fenster](~/mac/user-interface/window.md): In diesem Artikel erhalten Sie eine Einführung in die Arbeit mit Fenstern und Bereichen in einer Xamarin.Mac-Anwendung. Sie erfahren, wie Sie Fenster und Bereiche in Xcode und Interface Builder erstellen und verwalten, Fenster und Bereiche aus XIB-Dateien laden, Fenster benutzen und in C#-Code auf Fenster reagieren.
- [Dialogfelder](~/mac/user-interface/dialog.md): In diesem Artikel erhalten Sie eine Einführung in die Arbeit mit Dialogfeldern und modalen Fenstern in einer Xamarin.Mac-Anwendung. Sie erfahren, wie Sie modale Fenster in Xcode und Interface Builder erstellen und verwalten, mit Standarddialogfeldern arbeiten und Fenster anzeigen und auf diese in C#-Code reagieren.
- [Warnungen](~/mac/user-interface/alert.md): In diesem Artikel erhalten Sie eine Einführung in die Arbeit mit Warnungen in Xamarin.Mac-Anwendungen. Sie erfahren, wie Sie Warnungen aus C#-Code erstellen und anzeigen, und wie Sie auf Warnungen reagieren können.
- [Menüs](~/mac/user-interface/menu.md): Menüs werden in verschiedenen Teilen der Benutzeroberfläche einer Mac-Anwendung verwendet – vom Hauptmenü der Anwendung am oberen Rand des Bildschirms bis hin zu Popup- und Kontextmenüs, die überall in einem Fenster angezeigt werden können. Menüs sind ein integraler Bestandteil einer Mac-Anwendungsumgebung. Sie erfahren, wie Sie mit Cocoa-Menüs in einer Xamarin.Mac-Anwendung arbeiten können.
- [Symbolleiste](~/mac/user-interface/toolbar.md): In diesem Artikel wird beschrieben, wie Sie mit Symbolleisten in einer Xamarin.Mac-Anwendung arbeiten können. Mit ihrer Hilfe können Sie Elemente erstellen und verwalten. Sie lernen die Funktionsweise von Symbolleisten in Xcode sowie Interface Builder kennen und wie Sie die Symbolleistenelemente für Code mit Outlets und Aktionen verfügbar machen und aktivieren bzw. deaktivieren. Außerdem lernen Sie, wie Sie in C#-Code auf Symbolleistenelemente reagieren.
- [Tabellenansichten](~/mac/user-interface/table-view.md): In diesem Artikel wird beschrieben, wie Sie mit Tabellenansichten in einer Xamarin.Mac-Anwendung arbeiten können. Sie erfahren, wie Sie Tabellenansichten in Xcode und Interface Builder erstellen und verwalten, wie Sie die Tabellenansichtenelemente für Code mit Outlets und Aktionen verfügbar machen, Tabellenelemente auffüllen und in C#-Code auf Tabellenansichtenelemente reagieren.
- [Gliederungsansichten](~/mac/user-interface/outline-view.md): In diesem Artikel wird beschrieben, wie Sie mit Gliederungsansichten in einer Xamarin.Mac-Anwendung arbeiten. Sie erfahren, wie Sie Gliederungsansichten in Xcode und Interface Builder erstellen und verwalten, wie Sie die Gliederungsansichtelemente für Code mit Outlets und Aktionen verfügbar machen, Gliederungselemente auffüllen und in C#-Code auf Gliederungsansichtelemente reagieren.
- [Quelllisten](~/mac/user-interface/source-list.md): In diesem Artikel wird beschrieben, wie Sie mit Quelllisten in einer Xamarin.Mac-Anwendung arbeiten. Sie erfahren, wie Sie Quelllisten in Xcode und Interface Builder erstellen und verwalten, wie Sie die Quelllistenelemente für Code mit Outlets und Aktionen verfügbar machen, Quelllistenelemente auffüllen und in C#-Code auf Quelllistenelemente reagieren.
- [Auflistungsansichten](~/mac/user-interface/collection-view.md): In diesem Artikel wird beschrieben, wie Sie mit Auflistungsansichten in einer Xamarin.Mac-Anwendung arbeiten. Sie erfahren, wie Sie Auflistungsansichten in Xcode und Interface Builder erstellen und verwalten, wie Sie die Auflistungsansichtelemente für Code mit Outlets und Aktionen verfügbar machen, Auflistungselemente auffüllen und in C#-Code auf Auflistungsansichtelemente reagieren.
- [Arbeiten mit Bildern](~/mac/app-fundamentals/image.md): In diesem Artikel wird beschrieben, wie Sie mit Bildern und Symbolen in einer Xamarin.Mac-Anwendung arbeiten. Sie erfahren, wie Sie die erforderlichen Bilder erstellen und verwalten, die Sie für das App-Symbol benötigen, und wie Sie Bilder sowohl in C#-Code als auch in Interface Builder von Xcode verwenden.

Der [Mac-Beispielkatalog](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac) enthält fertige Codebeispiele zum Erlernen von Xamarin.Mac.

Eine vollständige Xamarin.Mac-App, die viele Funktionen enthält, die ein Benutzer in einer typischen Mac-Anwendung erwarten würde, finden Sie unter [SourceWriter-Beispiel-App](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in der Xamarin.Mac-Dokumentation bereitgestellt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Grundlagen einer Standard-Xamarin.Mac-App beschrieben. Sie haben erfahren, wie Sie eine neue App in Visual Studio für Mac erstellen, wie Sie die Benutzeroberfläche in Xcode und Interface Builder entwerfen, wie Sie Benutzeroberflächenelemente für C#-Code mit **Outlets** und **Aktionen** verfügbar machen, wie Sie Code hinzufügen, der mit Benutzeroberflächenelementen arbeitet, und wie Sie eine Xamarin.Mac-App erstellen und testen.

## <a name="related-links"></a>Verwandte Links

- [„Hallo, Mac“-Beispiel](https://docs.microsoft.com/samples/xamarin/mac-samples/hello-mac)
- [macOS-Eingaberichtlinien](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
