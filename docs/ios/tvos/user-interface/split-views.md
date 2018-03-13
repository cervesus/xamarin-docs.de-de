---
title: Arbeiten mit Split-View-Controller
description: Dieser Artikel umfasst das Entwerfen und Arbeiten mit Split-View-Controller innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 86a7690d4cf7291a4e44507a6250e3469c8f7ed2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-split-view-controllers"></a>Arbeiten mit Split-View-Controller

_Dieser Artikel umfasst das Entwerfen und Arbeiten mit Split-View-Controller innerhalb einer Xamarin.tvOS-app._


Ein Split-View-Controller präsentiert und verwaltet eine Master "und" Detail-View-Controller Seite-an-Seite, auf dem Bildschirm zur gleichen Zeit. Split-View-Controller verwendet werden, persistent, den Fokus erhalten kann Inhalt in die Masteransicht (dem kleineren Bereich auf der linken Seite) dargestellt und im Zusammenhang Details in der Detailansicht (dem größeren Bereich auf der rechten Seite).

[![](split-views-images/intro01.png "Beispiel geteilte Ansicht")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>Zum Split-View-Controller

Wie bereits erwähnt, verwaltet ein Split-View-Controller einer Master- und Detail-View-Controller, die Seite-an-Seite mit dem Master wird der kleinere Ansicht auf der linken Seite, die Details der größeren auf der rechten Seite angezeigt werden. 

Darüber hinaus kann die Master-View-Controller ausgeblendet oder angezeigt wurde nach Bedarf: 

[![](split-views-images/intro02.png "Die Master-View-Controller ausgeblendet")](split-views-images/intro02.png#lightbox)

Geteilte Ansichten Controller werden häufig verwenden, um eine Liste von Inhalten, die filterbar die Kategorien in der Master-Ansicht und den gefilterten Ergebnissen in der Detailansicht angezeigt. Dies wird in der Regel als einer Tabellenansicht auf der linken Seite angezeigt und ein [Auflistungsansicht](~/ios/tvos/user-interface/collection-views.md) auf der rechten Seite.

Beim Entwerfen einer Benutzeroberfläche, die eine Split-View-Controller erfordert empfiehlt Apple Master "und" Detail-View-Controller, die nicht (nur der Inhalt geändert, nicht der Struktur) ändern. Wenn Sie für die horizontale Swap-View-Controller benötigen, ist es am besten, einen Controller Navigation als die Basis des View-Controller verwenden, die ("Master" oder "Details") geändert werden muss.

Apple hat die folgenden Vorschläge für die Arbeit mit Split-View-Controller:

- **Verwenden Sie den richtigen Prozentsatz der Teilung** -standardmäßig verwendet der Split-View-Controller ein Drittel des Bildschirms für die Master-View-Controller und zwei Drittel für das Detail-View-Controller. Optional können Sie eine 50/50 Teilung. Wählen Sie den richtigen Prozentsatz auf Ihre Inhalte mit Lastenausgleich auf dem Bildschirm sichtbar.
- **Beibehalten der Main-Auswahl** – während der Inhalt für die Detailansicht können Änderung als Antwort auf eine Benutzerauswahl in der Master-Ansicht ist, den Inhalt der Master korrigiert werden soll. Darüber hinaus sollten Sie das aktuell ausgewählte Element in der Master-Ansicht aufzuzeigen.
- **Verwenden Sie einen einzelnen, einheitlichen Titel** – in der Regel möchten Sie verwenden einen einzelnen, zentrierten Titel in der Detailansicht, statt einen Titel in die Details und die Masteransicht.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Split-View-Controller und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Split-View-Controller in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Split-View-Controller** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](split-views-images/activity01.png "Ein Split-View-Controller")](split-views-images/activity01.png#lightbox)
1. Standardmäßig wird der iOS-Designer in der Master-Ansicht ein Controller für die Navigation und eine View-Controller installiert. Wenn dies nicht der app-Anforderungen passt, löschen Sie diese einfach.
1. Wenn Sie die Standardeinstellung Masteransicht entfernen, ziehen Sie die neue View-Controller auf der Entwurfsoberfläche angezeigt: 

    [![](split-views-images/activity02.png "Eine View-Controller")](split-views-images/activity02.png#lightbox)
1. Steuerelement klicken und ziehen Sie aus der Split-View-Controller auf die neue Master-View-Controller. 
1. Wählen Sie **Master** aus der **Popupmenü**: 

    [![](split-views-images/activity03.png "Wählen Sie im Popupmenü Master")](split-views-images/activity03.png#lightbox)
1. Entwerfen Sie den Inhalt der Master und Detailansichten: 

    [![](split-views-images/activity04.png "Beispiellayout für")](split-views-images/activity04.png#lightbox)
1. Weisen Sie **Namen** in der **Registerkarte "Widget"** von der **Eigenschaften aufgefüllt** mit UI-Steuerelemente in C#-Code arbeiten.
1. Die Änderungen zu speichern und zurück zu Visual Studio für Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Split-View-Controller** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](split-views-images/activity01-vs.png "Ein Split-View-Controller")](split-views-images/activity01-vs.png#lightbox)
1. Standardmäßig wird der iOS-Designer eine Navigation und View-Controller in der Master-Ansicht hinzufügen. Wenn dies nicht der app-Anforderungen passt, löschen Sie diese einfach.
1. Wenn Sie die Standardeinstellung Masteransicht entfernen, ziehen Sie die neue View-Controller auf der Entwurfsoberfläche angezeigt: 

    [![](split-views-images/activity02-vs.png "Eine View-Controller")](split-views-images/activity02-vs.png#lightbox)
1. Steuerelement klicken und ziehen Sie aus der Split-View-Controller auf die neue Master-View-Controller. 
1. Wählen Sie **Master** aus der **Popupmenü**: 

    [![](split-views-images/activity03-vs.png "Wählen Sie im Popupmenü Master")](split-views-images/activity03-vs.png#lightbox)
1. Entwerfen Sie den Inhalt der Master und Detailansichten: 

    [![](split-views-images/activity04.png "Content-layout")](split-views-images/activity04.png#lightbox)
1. Weisen Sie **Namen** in der **Registerkarte "Widget"** von der **Eigenschaften-Explorer** mit UI-Steuerelemente in C#-Code arbeiten.
1. Speichern Sie die Änderungen.
    
-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Arbeiten mit Split-View-Controller

Wie bereits erwähnt, ist ein Split-View-Controller häufig in Situationen verwendet, in dem Sie gefilterte Inhalt an den Benutzer angezeigt werden. Hauptkategorien unterteilt werden auf der linken Seite in der Master-Ansicht angezeigt, und die gefilterten Ergebnisse auf der rechten Seite in der Detailansicht auf Grundlage der Auswahl des Benutzers.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Zugreifen auf Master- und Detailtabelle

Wenn Sie programmgesteuert auf die Master und Details-View-Controller zugreifen müssen, verwenden Sie die `ViewControllers ` Eigenschaft des Split-View-Controller. Zum Beispiel:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Es wird ein Array dargestellt, wo ist das erste Element (0), in der Master-View-Controller und das zweite Element (1) das Detail.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Beim Zugriff auf Informationen aus Master

Da Sie in der Detailansicht, basierend auf der Auswahl des Benutzers in der Masterdatenbank in der Regel ausführliche Informationen anzeigen, benötigen Sie eine Möglichkeit, die Details aus dem Master-zugreifen.

Die einfachste Möglichkeit hierzu ist eine Eigenschaft für die Master-View-Controller-Klasse, z. B. verfügbar machen:

```csharp
public DetailViewController DetailController { get; set;}
```

In der Split-View-Controller, überschreiben die `ViewDidLoad` -Methode und Tie der zwei Ansichten zusammen. Zum Beispiel:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

Sie können Eigenschaften und Methoden auf Ihre Detail-View-Controller verfügbar machen, die die Master verwenden können, um neue Daten nach Bedarf zu präsentieren.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Ein- und Ausblenden von Master

Optional können Sie ein- und Ausblenden der Master-View-Controller mit dem `PreferredDisplayMode` Eigenschaft des Split-View-Controller. Zum Beispiel:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

Die `UISplitViewControllerDisplayMode` Enumeration definiert, wie die Master-View-Controller als eines der folgenden angezeigt werden:

- **Automatische** -tvos. außerdem wurden die Präsentation der Master und Detailansichten steuert.
- **PrimaryHidden** -Master-View-Controller wird ausgeblendet.
- **AllVisible** -Master und die Detail-View-Controller Seite-an-Seite angezeigt. Dies ist normal, Standardpräsentation.
- **PrimaryOverlay** -der Detail-View-Controller unter erweitert, und wird vom Master behandelt.

Verwenden Sie zum Abrufen des aktuelle Status der Präsentation der `DisplayMode` Eigenschaft des Split-View-Controller.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Split-View-Controller innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
