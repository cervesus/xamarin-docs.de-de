---
title: Hallo, iOS – Ausführliche Erläuterungen
description: In diesem Dokument wird die Beispielanwendung „Hallo, iOS“ ausführlicher beschrieben. Dabei werden unter anderem die Architektur, die Benutzeroberfläche, die Hierarchie der Inhaltsansicht, das Testen und die Entwicklung erläutert.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 0fa6c05b3aa90f1d1875b9169350b197d882d863
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023261"
---
# <a name="hello-ios--deep-dive"></a>Hallo, iOS – Ausführliche Erläuterungen

In der exemplarischen Vorgehensweise zum Schnellstart wurde das Erstellen und Ausführen einer einfachen Xamarin.iOS-Anwendung eingeführt. Nun ist es an der Zeit, ein tieferes Verständnis für die Funktionsweise von iOS-Anwendungen zu entwickeln, sodass Sie komplexere Programme erstellen können. Dieses Handbuch führt durch die Schritte der Hallo, iOS-Anleitung, um das Verständnis der grundlegenden Konzepte der Entwicklung von iOS-Anwendungen zu ermöglichen.

Dieses Handbuch unterstützt Sie dabei, die Fertigkeiten und Kenntnisse zu entwickeln, die zum Erstellen einer iOS-Anwendung für einen Bildschirm erforderlich sind. Nachdem Sie dieses durchgearbeitet haben, sollten Sie die verschiedenen Bestandteile einer Xamarin.iOS-Anwendung und deren Zusammenwirken verstehen können.

::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Einführung in Visual Studio für Mac

Visual Studio für Mac ist eine kostenlose Open Source-IDE, die Funktionen von Visual Studio und Xcode kombiniert. Sie enthält z.B. einen vollständig integrierten visuellen Designer, einen Text-Editor mit Refactoringtools, einen Assembly-Browser und Quellcodeintegration. Dieser Leitfaden stellt einige grundlegende Visual Studio für Mac-Funktionen vor. Wenn Sie jedoch noch nicht mit Visual Studio für Mac vertraut sind, lesen Sie vorher die Dokumentation zu [Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/).

Die Codeorganisation in Visual Studio für Mac baut auf Visual Studio auf und gliedert sich in *Projektmappen* und *Projekte*. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann beispielsweise eine Anwendung (z.B. für iOS oder Android), eine unterstützende Bibliothek oder eine Testanwendung sein. In der Phoneword-App wurde ein neues iPhone-Projekt mithilfe der Vorlage **Einzelansichtsanwendung** hinzugefügt. Die ursprüngliche Projektmappe sah folgendermaßen aus:

![](hello-ios-deepdive-images/image30.png "A screenshot of the initial solution")

::: zone-end
::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Einführung in Visual Studio

Visual Studio ist eine leistungsstarke IDE von Microsoft. Sie enthält z.B. einen vollständig integrierten visuellen Designer, einen Text-Editor mit Refactoringtools, einen Assembly-Browser und Quellcodeintegration. In diesem Handbuch werden einige grundlegende Features von Visual Studio mit Xamarin Tools vorgestellt.

Visual Studio organisiert Code in Projektmappen und Projekten. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann beispielsweise eine Anwendung (z.B. für iOS oder Android), eine unterstützende Bibliothek oder eine Testanwendung sein. In der Phoneword-App wurde ein neues iPhone-Projekt mithilfe der Vorlage **Einzelansichtsanwendung** hinzugefügt. Die ursprüngliche Projektmappe sah folgendermaßen aus:

![](hello-ios-deepdive-images/vs-image30.png "A screenshot of the initial solution")

::: zone-end

## <a name="anatomy-of-a-xamarinios-application"></a>Aufbau einer Xamarin.iOS-Anwendung

::: zone pivot="macos"

Links sehen Sie den **Lösungspad**, der die Verzeichnisstruktur und alle Dateien enthält, die der Projektmappe zugeordnet sind:

![](hello-ios-deepdive-images/image31.png "The solution Pad, which contains the directory structure and all the files associated with the solution")

::: zone-end
::: zone pivot="windows"

Rechts sehen Sie den **Projektmappenbereich**, der die Verzeichnisstruktur und alle Dateien enthält, die der Projektmappe zugeordnet sind:

![](hello-ios-deepdive-images/vs-image31.png "The solution Pane, which contains the directory structure and all the files associated with the solution")

::: zone-end

In der exemplarischen Vorgehensweise [Hallo, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) haben Sie eine Projektmappe namens **Phoneword** erstellt und darin ein iOS-Projekt namens **Phoneword_iOS** platziert. Die folgenden Elemente befinden sich im Projekt:

- **Verweise**: Enthält die Assemblys, die zum Erstellen und Ausführen der Anwendung erforderlich sind. Erweitern Sie das Verzeichnis, um Verweise auf .NET-Assemblys anzuzeigen, z.B. [System](https://docs.microsoft.com/dotnet/api/system), System.Core und [System.xml](https://docs.microsoft.com/dotnet/api/system.xml), sowie einen Verweis auf die Xamarin.iOS-Assembly.
- **Pakete:** Das Paketverzeichnis enthält vordefinierte NuGet-Pakete.
- **Ressourcen:** Der Ressourcenordner speichert andere Medien.
- **Main.cs**: Dies enthält den Haupteinstiegspunkt der Anwendung. Zum Start der Anwendung wird der Name der Hauptanwendungsklasse, `AppDelegate`, übergeben.
- **AppDelegate.cs**: Diese Datei enthält die Hauptanwendungsklasse und ist zuständig für das Erstellen des Fensters und der Benutzeroberfläche und das Lauschen auf Ereignisse aus dem Betriebssystem.
- **Main.Storyboard**: Das Storyboard enthält den visuellen Entwurf der Benutzeroberfläche der Anwendung. Storyboard-Dateien werden in einem grafischen Editor namens iOS-Designer geöffnet.
- **ViewController.cs:** Der Ansichtscontroller betreibt den Bildschirm (die Ansicht), den ein Benutzer sieht und berührt. Der Ansichtscontroller ist verantwortlich für die Verarbeitung von Interaktionen zwischen dem Benutzer und der Ansicht.
- **ViewController.designer.cs:** `designer.cs` ist eine automatisch generierte Datei, die als Verbindung zwischen Steuerelementen in der Ansicht und ihren Codedarstellungen im Ansichtscontroller fungiert. Da es sich dabei um eine interne Grundlagendatei handelt, überschreibt die IDE alle manuellen Änderungen, und in den meisten Fällen kann diese Datei ignoriert werden. Weitere Informationen zur Beziehung zwischen dem visuellen Designer und dem zugrunde liegenden Code finden Sie im Handbuch [Einführung in den iOS-Designer](~/ios/user-interface/designer/introduction.md).
- **Info.plist:** Anwendungseigenschaften, z.B. der Anwendungsname, die Symbole, die Startbilder und mehr sind in der Eigenschaftenliste **Info.plist** festgelegt. Diese Datei ist leistungsstark. Eine ausführliche Einführung in diese finden Sie im Handbuch [Arbeiten mit Eigenschaftenlisten](~/ios/app-fundamentals/property-lists.md).
- **Entitlements.plist:** Mit der Eigenschaftenliste „Berechtigungen“ können Sie *Anwendungsfunktionen* (auch als „App Store-Technologien“ bezeichnet) wie iCloud, PassKit und mehr angeben. Weitere Informationen zur Eigenschaftenliste **Entitlements.plist** finden Sie im Handbuch [Arbeiten mit Eigenschaftenlisten](~/ios/app-fundamentals/property-lists.md). Eine allgemeine Einführung in Berechtigungen finden Sie im Handbuch [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md).

## <a name="architecture-and-app-fundamentals"></a>Architektur und Anwendungsgrundlagen

Bevor eine iOS-Anwendung eine neue Benutzeroberfläche laden kann, müssen zwei Voraussetzungen erfüllt sein. Zuerst muss die Anwendung einen *Einstiegspunkt* definieren – der erste Code, der ausgeführt wird, wenn der Anwendungsprozess in den Arbeitsspeicher geladen wird. Als Nächstes muss sie eine Klasse definieren, die anwendungsweite Ereignisse verarbeitet und mit dem Betriebssystem interagiert.

In diesem Abschnitt werden die in der folgenden Abbildung dargestellten Beziehungen behandelt:

[![](hello-ios-deepdive-images/image32.png "The Architecture and App Fundamentals relationships are illustrated in this diagram")](hello-ios-deepdive-images/image32.png#lightbox)

### <a name="main-method"></a>Main-Methode

Der Haupteinstiegspunkt einer iOS-Anwendung ist die `Application`-Klasse. Die `Application`-Klasse wird in der Datei **Main.cs** definiert und enthält eine statische `Main`-Methode. Sie erstellt eine neue Xamarin.iOS-Anwendungsinstanz und übergibt den Namen der *AppDelegate*-Klasse, die Betriebssystemereignisse verarbeitet. Der Vorlagencode für die statische `Main`-Methode wird unten angezeigt:

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>Anwendungsdelegat

Unter iOS verarbeitet die *AppDelegate*-Klasse Systemereignisse. Diese Klasse befindet sich in **AppDelegate.cs**. Die `AppDelegate`-Klasse verwaltet die Anwendung *Fenster*. Das Fenster ist eine einzelne Instanz der `UIWindow`-Klasse, die als Container für die Benutzeroberfläche fungiert. Standardmäßig erhält eine Anwendung nur ein Fenster zum Laden ihres Inhalts, und das Fenster ist an einen *Bildschirm* (einzelne `UIScreen`-Instanz) angefügt. Dieser stellt das umschließende Rechteck bereit, das den Dimensionen des physischen Gerätebildschirms entspricht.

Der *Anwendungsdelegat* ist auch für das Abonnieren von Systemupdates zu wichtigen Anwendungsereignissen verantwortlich. Zu solchen Ereignissen zählt z.B. wenn der Start der Anwendung abgeschlossen oder der Arbeitsspeicher gering ist.

Der Vorlagencode für den Anwendungsdelegaten wird nachfolgend dargestellt:

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

Nachdem die Anwendung das Fenster definiert hat, kann sie mit dem Laden der Benutzeroberfläche beginnen. Im nächsten Abschnitt wird das Erstellen der Benutzeroberfläche erklärt.

## <a name="user-interface"></a>Benutzeroberfläche

Die Benutzeroberfläche einer iOS-App ist wie eine Storefront. Die Anwendung erhält in der Regel ein Fenster, kann dieses aber mit so vielen Objekten wie nötig auffüllen, und die Objekte und Anordnungen können geändert werden, je nachdem, was die App anzeigen möchte. Die Objekte in diesem Szenario – die Dinge, die der Benutzer sieht – werden als Ansichten bezeichnet. Zum Erstellen eines einzigen Bildschirms in einer Anwendung werden Ansichten in einer *Hierarchie von Inhaltsansichten* aufeinander gestapelt, und die Hierarchie wird von einem einzelnen Ansichtscontroller verwaltet. Anwendungen mit mehreren Bildschirmen weisen mehrere Hierarchien von Inhaltsansichten auf. Jede davon verfügt über ihren eigenen Ansichtscontroller, und die Anwendung platziert Ansichten in das Fenster, um je nach dem vom Benutzer verwendeten Bildschirm eine andere Hierarchie von Inhaltsansichten zu erstellen.

Dieser Abschnitt beschreibt die Benutzeroberfläche ausführlich, also Ansichten, Hierarchien von Inhaltsansichten und den iOS-Designer.

### <a name="ios-designer-and-storyboards"></a>iOS-Designer und Storyboards

Der iOS-Designer ist ein visuelles Tool zum Erstellen von Benutzeroberflächen in Xamarin. Der Designer kann durch Doppelklicken auf eine beliebige Storyboarddatei (.storyboard) gestartet werden, um eine Ansicht zu öffnen, die dem folgenden Screenshot ähnelt:

::: zone pivot="macos"

![](hello-ios-deepdive-images/image33.png "iOS Designer Interface")

Ein *Storyboard* ist eine Datei, die visuelle Entwürfe der Bildschirme unserer Anwendung sowie Übergänge und Beziehungen zwischen den Bildschirmen enthält. Die Darstellung des Bildschirms einer Anwendung in einem Storyboard wird _Szene_ genannt. Jede Szene stellt einen Ansichtscontroller und die verwalteten Ansichten dar (Hierarchie von Inhaltsansichten). Wenn ein neues **Einzelansichtsanwendungs-Projekt** aus einer Vorlage erstellt wird, generiert Visual Studio für Mac automatisch eine Storyboarddatei namens `Main.storyboard` und füllt sie wie im folgenden Screenshot veranschaulicht mit einer einzigen Szene:

![](hello-ios-deepdive-images/image34.png "Visual Studio for Mac automatically generates a Storyboard file called Main.storyboard and populates it with a single Scene")

Die schwarze Leiste unten auf der Storyboardanzeige kann zur Auswahl des Ansichtscontrollers für die Szene ausgewählt werden. Der Ansichtscontroller ist eine Instanz der `UIViewController`-Klasse, die den zugrunde liegenden Code für die Hierarchie von Inhaltsansichten enthält. Eigenschaften auf diesem Ansichtscontroller können wie im folgenden Screenshot veranschaulicht im **Eigenschaftenpad** angezeigt und festgelegt werden:

![](hello-ios-deepdive-images/image35.png "The Properties Pane")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image33.png "iOS Designer Interface")

Ein *Storyboard* ist eine Datei, die visuelle Entwürfe der Bildschirme unserer Anwendung sowie Übergänge und Beziehungen zwischen den Bildschirmen enthält. Die Darstellung des Bildschirms einer Anwendung in einem Storyboard wird _Szene_ genannt. Jede Szene stellt einen Ansichtscontroller und die verwalteten Ansichten dar (Hierarchie von Inhaltsansichten). Wenn ein neues **Einzelansichtsanwendungs-Projekt** aus einer Vorlage erstellt wird, generiert Visual Studio automatisch eine Storyboarddatei namens `Main.storyboard` und füllt sie wie im folgenden Screenshot veranschaulicht mit einer einzigen Szene:

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio automatically generates a Storyboard file called Main.storyboard and populates it with a single Scene")

Die Leiste unten auf der Storyboardanzeige kann zur Auswahl des Ansichtscontrollers für die Szene ausgewählt werden. Der Ansichtscontroller ist eine Instanz der `UIViewController`-Klasse, die den zugrunde liegenden Code für die Hierarchie von Inhaltsansichten enthält. Eigenschaften auf diesem Ansichtscontroller können wie im folgenden Screenshot veranschaulicht im **Eigenschaftenbereich** angezeigt und festgelegt werden:

![](hello-ios-deepdive-images/vs-image35.png "The Properties Pane")

::: zone-end

Die _Ansicht_ kann durch einen Klick in den weißen Teil der Szene ausgewählt werden. Die Ansicht ist eine Instanz der `UIView`-Klasse, die einen Bereich des Bildschirms definiert und Schnittstellen zum Arbeiten mit dem Inhalt in diesem Bereich bereitstellt. Die Standardansicht ist eine einzelne *Stammansicht*, die den gesamten Bildschirm ausfüllt.

Auf der linken Seite der Szene befindet sich wie im folgenden Screenshot veranschaulicht ein grauer Pfeil mit einem Flaggensymbol:

 [![](hello-ios-deepdive-images/image37.png "A gray arrow with a flag icon")](hello-ios-deepdive-images/image37.png#lightbox)

Der graue Pfeil stellt einen Storyboardübergang namens *Segue* („Segway“ ausgesprochen) dar. Da dieser Segue keinen Ursprung hat, handelt es sich um einen *Sourceless Segue*. Ein „Sourceless Segue“ verweist auf die erste Szene, deren Ansichten beim Anwendungsstart in unser Anwendungsfenster geladen werden. Die Szene und die darin enthaltenen Ansichten werden dem Benutzer als Erstes angezeigt, wenn die Anwendung geladen wird.

Wenn Sie eine Benutzeroberfläche erstellen, können zusätzliche Ansichten wie im folgenden Screenshot veranschaulicht aus der **Toolbox** in die Hauptansicht auf der Designoberfläche gezogen werden:

::: zone pivot="macos"

![](hello-ios-deepdive-images/image38.png "Additional Views can be dragged from the Toolbox onto the main View on the design surface")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image38.png "Additional Views can be dragged from the Toolbox onto the main View on the design surface")

::: zone-end

Diese zusätzlichen Ansichten heißen *Unteransichten*. Zusammen sind die Stammansicht und die Unteransichten Teil einer *Hierarchie von Inhaltsansichten*, die durch `ViewController` verwaltet wird. Die Gliederung aller Elemente in der Szene kann im Pad **Dokumentgliederung** angezeigt werden:

::: zone pivot="macos"

![](hello-ios-deepdive-images/image39.png "The Document Outline pad")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image39.png "The Document Outline pad")

::: zone-end

Im folgenden Diagramm werden die Unteransichten hervorgehoben:

::: zone pivot="macos"

![](hello-ios-deepdive-images/image40.png "The Subviews are highlighted in the diagram")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image40.png "The Subviews are highlighted in the diagram")

::: zone-end

Im nächsten Abschnitt wird die durch diese Szene dargestellte Hierarchie von Inhaltsansichten gegliedert.

## <a name="content-view-hierarchy"></a>Hierarchie von Inhaltsansichten

Eine _Hierarchie von Inhaltsansichten_ besteht aus einem Stapel von Ansichten und Unteransichten, die wie im folgenden Diagramm veranschaulicht von einem einzelnen Ansichtscontroller verwaltet werden:

 [![](hello-ios-deepdive-images/image41.png "The Content View Hierarchy")](hello-ios-deepdive-images/image41.png#lightbox)

Die Hierarchie von Inhaltsansichten von `ViewController` kann leichter angezeigt werden, indem Sie die Hintergrundfarbe der Stammansicht im Ansichtsabschnitt des **Eigenschaftenpads** wie im folgenden Screenshot veranschaulicht vorübergehend in Gelb ändern:

::: zone pivot="macos"

![](hello-ios-deepdive-images/image42.png "Changing the background color of the root View to yellow in the View section of the Properties Pad")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image42.png "Changing the background color of the root View to yellow in the View section of the Properties Pad")

::: zone-end

Im folgenden Diagramm werden die Beziehungen zwischen dem Fenster, den Ansichten, Unteransichten und dem Ansichtscontroller veranschaulicht, die die Benutzeroberfläche auf den Gerätebildschirm bringen:

[![](hello-ios-deepdive-images/image43.png "The relationships between the Window, Views, Subviews, and view controller")](hello-ios-deepdive-images/image43.png#lightbox)

Im nächsten Abschnitt wird erläutert, wie im Code mit Ansichten gearbeitet wird, und Sie lernen das Programmieren der Benutzerinteraktion mithilfe von Ansichtscontrollern und dem Ansichtslebenszyklus.

## <a name="view-controllers-and-the-view-lifecycle"></a>Ansichtscontroller und der Ansichtslebenszyklus

Jede Hierarchie von Inhaltsansichten verfügt über einen entsprechenden Ansichtscontroller, um Benutzerinteraktionen auszuführen. Die Rolle des Ansichtscontrollers ist das Verwalten der Ansichten in der Hierarchie von Inhaltsansichten. Der Ansichtscontroller stellt keinen Teil der Hierarchie von Inhaltsansichten und kein Element in der Schnittstelle dar. Stattdessen stellt er den Code bereit, der die Benutzerinteraktionen mit den Objekten auf dem Bildschirm ausführt.

### <a name="view-controllers-and-storyboards"></a>Ansichtscontroller und Storyboards

::: zone pivot="macos"

Der Ansichtscontroller wird in einem Storyboard als Leiste am unteren Rand der Szene dargestellt. Wenn Sie den Ansichtscontroller auswählen, werden seine Eigenschaften im **Eigenschaftenpad** angezeigt:

![](hello-ios-deepdive-images/image44.png "Selecting the view controller brings up its properties in the Properties Pane")

Eine benutzerdefinierte Ansichtscontrollerklasse für die Hierarchie von Inhaltsansichten, die durch diese Szene dargestellt wird, kann durch Bearbeiten der Eigenschaft **Klasse** im Abschnitt **Identität** des **Eigenschaftenpads** festgelegt werden. Beispielsweise legt die **Phoneword**-Anwendung wie im folgenden Screenshot veranschaulicht `ViewController` als Ansichtscontroller für den ersten Bildschirm fest:

![](hello-ios-deepdive-images/image45new.png "The Phoneword application sets the ViewController as the view controller")

::: zone-end
::: zone pivot="windows"

Der Ansichtscontroller wird in einem Storyboard als Leiste am unteren Rand der Szene dargestellt. Wenn Sie den Ansichtscontroller auswählen, werden seine Eigenschaften im **Eigenschaftenbereich** angezeigt:

![](hello-ios-deepdive-images/vs-image44.png "Selecting the view controller brings up its properties in the Properties Pane")

Eine benutzerdefinierte Ansichtscontrollerklasse für die Hierarchie von Inhaltsansichten, die durch diese Szene dargestellt wird, kann durch Bearbeiten der Eigenschaft **Klasse** im Abschnitt **Identität** des **Eigenschaftenbereichs** festgelegt werden. Beispielsweise legt die **Phoneword**-Anwendung wie im folgenden Screenshot veranschaulicht `ViewController` als Ansichtscontroller für den ersten Bildschirm fest:

![](hello-ios-deepdive-images/vs-image45.png "The Phoneword application sets the ViewController as the view controller")

::: zone-end

Auf diese Weise wird die Storyboarddarstellung des Ansichtscontrollers mit der C#-Klasse `ViewController` verknüpft. Öffnen Sie die `ViewController.cs`-Datei, und beachten Sie, dass der Ansichtscontroller wie im folgenden Code dargestellt eine *Unterklasse* von `UIViewController` ist:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` führt jetzt die Interaktionen der Hierarchie von Inhaltsansichten durch, die diesem Ansichtscontroller im Storyboard zugeordnet sind. Als Nächstes erfahren Sie mehr über die Rolle des Ansichtscontrollers bei der Verwaltung der Ansichten, indem ein Prozess namens Ansichtslebenszyklus eingeführt wird.

> [!NOTE]
> Für rein visuelle Anzeigen, die keine Benutzerinteraktion erfordern, kann die Eigenschaft **Class** im **Eigenschaftenpad** leer gelassen werden. Dadurch wird die Sicherungsklasse des Ansichtscontrollers als Standardimplementierung von `UIViewController` festgelegt. Dies eignet sich, wenn Sie keinen benutzerdefinierten Code hinzufügen möchten.

### <a name="view-lifecycle"></a>Ansichtslebenszyklus

Der Ansichtscontroller ist für das Laden und Entladen von Hierarchien von Inhaltsansichten aus dem Fenster verantwortlich. Wenn etwas Wichtiges mit einer Ansicht in der Hierarchie von Inhaltsansichten geschieht, benachrichtigt das Betriebssystem den Ansichtscontroller über Ereignisse im Ansichtslebenszyklus. Durch Überschreiben von Methoden im Ansichtslebenszyklus können Sie mit den Objekten auf dem Bildschirm interagieren und eine dynamische, reaktionsfähige Benutzeroberfläche erstellen.

Dies sind die grundlegenden Lebenszyklusmethoden und ihre Funktionen:

- **ViewDidLoad:** Wird *einmal* aufgerufen, wenn der Ansichtscontroller seine Hierarchie von Inhaltsansichten in den Arbeitsspeicher lädt. Dies ist ein guter Ausgangspunkt für das ursprüngliche Setup, da Unteransichten nun im Code verfügbar sind.
- **ViewWillAppear:** Wird aufgerufen, sobald eine Ansicht des Ansichtscontrollers zu einer Hierarchie von Inhaltsansichten hinzugefügt werden soll und auf dem Bildschirm angezeigt wird.
- **ViewWillDisappear:** Wird aufgerufen, sobald eine Ansicht des Ansichtscontrollers aus einer Hierarchie von Inhaltsansichten entfernt werden soll und vom Bildschirm verschwindet. Dieses Lebenszyklusereignis wird für die Bereinigung und das Speichern des Status verwendet.
- **ViewDidAppear** und **ViewDidDisappear**: Werden aufgerufen, wenn eine Ansicht zur Hierarchie von Inhaltsansichten hinzugefügt oder daraus entfernt wird.

Wenn benutzerdefinierter Code zu einer Phase des Lebenszyklus hinzugefügt wird, muss die *Basisimplementierung* dieses Lebenszyklus *überschrieben* werden. Dazu tippen Sie auf die vorhandene Lebenszyklusmethode, zu der bereits Code hinzugefügt wurde, und erweitern sie mit zusätzlichem Code. Die grundlegende Implementierung wird innerhalb der Methode aufgerufen, um sicherzustellen, dass der ursprüngliche Code vor dem neuen Code ausgeführt wird. Ein Beispiel dafür wird im nächsten Abschnitt dargestellt.

Weitere Informationen zum Arbeiten mit Ansichtscontrollern finden Sie im [Programmierhandbuch zu Ansichtscontrollern für iOS](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1) von Apple und in der [Referenz zu UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc).

### <a name="responding-to-user-interaction"></a>Reagieren auf eine Benutzerinteraktion

Die wichtigste Rolle des Ansichtscontrollers ist die Reaktion auf Benutzerinteraktionen, z.B. das Klicken auf eine Schaltfläche, die Navigation und mehr. Die einfachste Möglichkeit zur Verarbeitung von Benutzerinteraktionen ist die Verbindung mit einem Steuerelement, um Benutzereingaben zu überwachen und einen Ereignishandler hinzuzufügen, der auf die Eingabe reagiert. Eine Schaltfläche könnte beispielsweise so eingerichtet werden, dass sie wie in der Phoneword-Anwendung veranschaulicht auf ein Touchereignis reagiert.

Sehen Sie sich genauer an, wie das funktioniert.
Im `Phoneword_iOS`-Projekt wurde eine Schaltfläche namens `TranslateButton` zur Hierarchie von Inhaltsansichten hinzugefügt:

[![](hello-ios-deepdive-images/image1.png "A button was added called TranslateButton to the Content View Hierarchy")](hello-ios-deepdive-images/image1.png#lightbox)

Wenn ein **Name** dem Steuerelement **Schaltfläche** im **Eigenschaftenpad** zugewiesen ist, wird der iOS-Designer automatisch einem Steuerelement in **ViewController.designer.cs** zugewiesen, sodass `TranslateButton` innerhalb der `ViewController`-Klasse verfügbar ist. Steuerelemente werden erstmals in der `ViewDidLoad`-Phase des Ansichtslebenszyklus verfügbar. Diese Lebenszyklusmethode wird verwendet, um auf die Toucheingabe des Benutzers zu reagieren:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Die Phoneword-Anwendung verwendet ein Touchereignis mit der Bezeichnung `TouchUpInside`, um auf die Toucheingabe des Benutzers zu lauschen. `TouchUpInside` lauscht auf ein Touchereignis (Finger wird vom Bildschirm entfernt) nach einem Touchereignis (Finger berührt den Bildschirm) innerhalb der Grenzen des Steuerelements. Das Gegenteil von `TouchUpInside` ist das `TouchDown`-Ereignis, das ausgelöst wird, wenn der Benutzer auf ein Steuerelement klickt. Das `TouchDown`-Ereignis erfasst viel Rauschen und bietet dem Benutzer keine Option zum Beenden des Touchereignisses, indem er mit dem Finger vom Steuerelement gleitet. `TouchUpInside` ist die gängigste Reaktion auf die Berührung einer **Schaltfläche** und erstellt die Oberfläche, die der Benutzer beim Drücken einer Schaltfläche erwartet. Weitere Informationen dazu finden Sie in den [iOS-Eingaberichtlinien](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html) von Apple.

In dieser Anwendung wird das `TouchUpInside`-Ereignis mit einem Lambdaausdruck verarbeitet, stattdessen könnten jedoch auch ein Delegat oder ein Ereignishandler verwendet werden. Der endgültige Schaltflächencode ähnelt Folgendem:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Zusätzliche in Phoneword eingeführte Konzepte

Die Phoneword-Anwendung enthält weitere Konzepte, die jedoch nicht in diesem Leitfaden behandelt werden. Dies sind z.B. folgende Konzepte:

- **Ändern des Schaltflächentexts**: Die Phoneword-Anwendung veranschaulichte, wie Sie den Text einer **Schaltfläche** ändern können, indem Sie `SetTitle` auf der **Schaltfläche** aufrufen und den neuen Text und den _Steuerelementzustand_ der **Schaltfläche** übergeben. Folgender Code ändert beispielsweise den Schaltflächentext in „Call“ (Anrufen):

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```

- **Aktivieren und Deaktivieren von Schaltflächen**: **Schaltflächen** können sich im Zustand `Enabled` oder `Disabled` befinden. Eine deaktivierte **Schaltfläche** reagiert nicht auf Benutzereingaben. Der folgende Code deaktiviert beispielsweise `CallButton`:

    ```csharp
    CallButton.Enabled = false;
    ```

    Weitere Informationen zu Schaltflächen finden Sie im Handbuch zu [Schaltflächen](~/ios/user-interface/controls/buttons.md).

- **Ausblenden der Tastatur**: Wenn der Benutzer auf das Textfeld tippt, blendet iOS die Tastatur ein, damit der Benutzer eine Eingabe durchführen kann. Es gibt leider keine integrierte Funktionalität zum Ausblenden der Tastatur. Der folgende Code wird zu `TranslateButton` hinzugefügt, um die Tastatur auszublenden, wenn der Benutzer auf `TranslateButton` drückt:

    ```csharp
    PhoneNumberText.ResignFirstResponder ();
    ```

    Ein weiteres Beispiel zum Ausblenden der Tastatur finden Sie unter [Dismiss the Keyboard (Ausblenden der Tastatur)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard).

- **Durchführen eines Telefonanrufs mit URL**: In der Phoneword-App wird ein URL-Schema von Apple verwendet, um die Systemtelefon-App zu starten. Das benutzerdefinierte URL-Schema besteht wie im folgenden Code veranschaulicht aus einem „tel:“-Präfix und der übersetzten Telefonnummer:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```

- **Anzeigen einer Warnung:** Wenn ein Benutzer versucht, einen Telefonanruf auf einem Gerät durchzuführen, das Anrufe nicht unterstützt (z.B. der Simulator oder ein iPod Touch), wird eine Warnung angezeigt, damit der Benutzer weiß, dass der Anruf nicht durchgeführt werden kann. Der folgende Code erstellt und füllt einen Warnungscontroller:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

    Weitere Informationen zu iOS-Warnungsansichten finden Sie in der [Anleitung für den Warnungscontroller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller).

## <a name="testing-deployment-and-finishing-touches"></a>Tests, Bereitstellung und Vollendung

Visual Studio für Mac und Visual Studio stellen beide viele Optionen zum Testen und Bereitstellen einer Anwendung bereit. In diesem Abschnitt werden Debugoptionen behandelt, das Testen von Anwendungen auf einem Gerät veranschaulicht und Tools für das Erstellen von benutzerdefinierten Anwendungssymbolen und Startbildern vorgestellt.

### <a name="debugging-tools"></a>Debuggingtools

Probleme im Anwendungscode können manchmal schwer zu diagnostizieren sein. Für die Diagnose von komplexen Codeproblemen können Sie [einen Haltepunkt festlegen](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Code schrittweise durchlaufen](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) oder [Informationen an das Protokollfenster ausgeben](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### <a name="deploy-to-a-device"></a>Bereitstellen für ein Gerät

Der iOS-Simulator ist eine schnelle Möglichkeit zum Testen einer Anwendung. Der Simulator bietet eine Reihe von nützlichen Optimierungen für Tests, einschließlich simulierten Speicherorten, [simulierten Bewegungen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator) und vieles mehr. Benutzer werden die endgültige Anwendung jedoch nicht in einem Simulator verwenden. Alle Anwendungen sollten frühzeitig und häufig auf realen Geräten getestet werden.

Ein Gerät benötigt Zeit für die Bereitstellung und erfordert ein Apple-Entwicklerkonto. Das Handbuch [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) enthält umfassende Anweisungen, mit denen ein Gerät für die Entwicklung vorbereitet werden kann.

> [!NOTE]
> Für Apple ist aktuell ein Entwicklungszertifikat oder eine _Signierungsidentität_ erforderlich, um Code für ein physisches Gerät oder für den Simulator erstellen zu können. Führen Sie dazu die Schritte im Handbuch [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) aus.

Sobald das Gerät bereitgestellt wurde, können Sie es bereitstellen, indem Sie es anschließen, das Ziel in der Buildsymbolleiste in das iOS-Gerät ändern und wie im folgenden Screenshot veranschaulicht auf **Starten** (**Wiedergeben**) drücken:

::: zone pivot="macos"

![](hello-ios-deepdive-images/image46new.png "Pressing Start/Play")

::: zone-end
::: zone pivot="windows"

![](hello-ios-deepdive-images/vs-image46.png "Pressing Start/Play")

::: zone-end

Die Anwendung wird auf dem iOS-Gerät bereitgestellt:

[![](hello-ios-deepdive-images/image1.png "The app will deploy to the iOS device and run")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Generieren von benutzerdefinierten Symbolen und Startbildern

Nicht jedem steht ein Designer zur Verfügung, der benutzerdefinierte Symbole und Startbilder erstellt, die eine App benötigt, um herauszustechen. Hier finden Sie einige alternative Ansätze, um benutzerdefinierte App-Grafiken zu generieren:

::: zone pivot="macos"

- [**Sketch**](https://www.sketchapp.com"): Sketch ist eine Mac-App für das Entwerfen von Benutzeroberflächen, Symbolen usw. Diese App wurde verwendet, um die Xamarin-App-Symbole und -Startbilder zu entwerfen. Sketch 3 ist im App Store verfügbar. Sie können ebenfalls das kostenlose [Sketch Tool](https://bohemiancoding.com/sketch/tool/) testen.
- [**Pixelmator**](https://www.pixelmator.com/): Eine vielseitige Mac-App für das Bearbeiten von Bildern, die ungefähr 30 Dollar kostet.
- [**Glyphish**](https://www.glyphish.com/): Qualitativ hochwertige, vorgefertigte Symbole, die entweder gratis heruntergeladen oder käuflich erworben werden können.
- [**Fiverr**](https://www.fiverr.com/): Wählen Sie aus einer Vielzahl von Designern aus, die ab 5 Dollar Symbole für Sie erstellen. Obwohl das Ergebnis nicht immer zuverlässig ist, ist dies eine gute Ressource, falls Sie schnell Symbole benötigen.

::: zone-end
::: zone pivot="windows"

- Visual Studio: Sie können dies verwenden, um einfache Symbole für Ihre App direkt in der IDE zu erstellen.
- [**Glyphish**](https://www.glyphish.com/): Qualitativ hochwertige, vorgefertigte Symbole, die entweder gratis heruntergeladen oder käuflich erworben werden können.
- [**Fiverr**](https://www.fiverr.com/): Wählen Sie aus einer Vielzahl von Designern aus, die ab 5 Dollar Symbole für Sie erstellen. Obwohl das Ergebnis nicht immer zuverlässig ist, ist dies eine gute Ressource, falls Sie schnell Symbole benötigen.

::: zone-end

Weitere Informationen zu Größen und Anforderungen von Symbolen und Startbildern finden Sie im Handbuch [Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch! Sie sollten nun mit den Komponenten einer Xamarin.iOS-Anwendung und mit den Tools, die für die Erstellung benötigt werden, vertraut sein.
Im [nächsten Tutorial der Reihe „Erste Schritte“](~/ios/get-started/hello-ios-multiscreen/index.md) erweitern Sie unsere Anwendung zur Verarbeitung von mehreren Bildschirmen. Dabei implementieren Sie einen Navigationscontroller, erfahren mehr über Storyboard-Segues und implementieren das MVC-Muster (Model View Controller).

## <a name="related-links"></a>Verwandte Links

- [Hallo, iOS (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)
- [iOS-Eingaberichtlinien](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [iOS-Bereitstellungsportal](https://developer.apple.com/account/#/overview)
