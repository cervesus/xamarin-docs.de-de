---
title: Arbeiten mit TvOS Split-View-Controller in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit TvOS-Ansichten in einer app mit Xamarin erstellten teilen. Es bietet eine allgemeine Übersicht über Split-View-Controller, wie mit Storyboards, die Zugriff auf die Master- und Detailtabelle Ansichten und angezeigt bzw. ausgeblendet werden die Masteransicht.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f1bd48378faa9ae6a4853083c93377268c38f01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122191"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>Arbeiten mit TvOS Split-View-Controller in Xamarin

Ein Controller für geteilte Ansicht präsentiert und verwaltet eine Master- und Detail-View-Controller Seite-an-Seite, auf dem Bildschirm zur gleichen Zeit. Split-View-Controller sind persistent, den Fokus erhalten kann in der Master-Ansicht (der kleineren Abschnitt auf der linken Seite) zu präsentieren und zugehörigen Informationen in der Detailansicht (im größeren Abschnitt auf der rechten Seite).

[![](split-views-images/intro01.png "Beispiel geteilte Ansicht")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>Informationen zu den Split-View-Controller

Wie bereits erwähnt, verwaltet ein Controller für geteilte Ansicht einer Master- und Detail-View-Controller, die Seite-an-Seite, mit dem Master wird die kleinere Ansicht auf der linken Seite die Details der größeren auf der rechten Seite angezeigt werden. 

Darüber hinaus wurde der Master-View-Controller können ausgeblendet oder angezeigt wie erforderlich: 

[![](split-views-images/intro02.png "Der Master-View-Controller ausgeblendet")](split-views-images/intro02.png#lightbox)

Geteilte Ansichten Controller sind häufig verwenden, um eine Liste der Inhalte gefiltert, die Kategorien in der Master-Ansicht und die gefilterten Ergebnisse in der Detailansicht angezeigt. Dies erfolgt in der Regel als eine Tabellenansicht auf der linken Seite, und ein [Auflistungsansicht](~/ios/tvos/user-interface/collection-views.md) auf der rechten Seite.

Beim Entwerfen einer Benutzeroberfläche, die einen Controller für geteilte Ansicht erfordert empfiehlt Apple Master- und Detail-View-Controller, die nicht ändern (nur der Inhalt geändert, nicht die Struktur). Wenn Sie für die Austausch-Ansichtscontrollern benötigen, empfiehlt es sich um einen Navigationscontroller als die Basis des View Controller zu verwenden, die ("Master" oder "ausführlich") geändert werden muss.

Apple hat die folgenden Vorschläge für die Arbeit mit Split-View-Controller:

- **Verwenden Sie den richtigen Prozentsatz für die Teilung** -standardmäßig verwendet der Controller für geteilte Ansicht ein Drittel des Bildschirms für die Master-View-Controller und zwei Drittel für den Ansichtscontroller Details. Optional können Sie eine 50/50-Teilung. Wählen Sie den richtigen Prozentsatz, um Ihre Inhalte angezeigt, die auf dem Bildschirm mit Lastenausgleich zu machen.
- **Speichern Sie die Main-Auswahl** – während der Inhalt für die Detailansicht können Änderung, die als Antwort auf eine Benutzerauswahl in der Master-Ansicht ist, der Inhalt der Masteransicht korrigiert werden muss. Darüber hinaus sollten Sie das aktuell ausgewählte Element in der Master-Ansicht übersichtlich.
- **Verwenden Sie eine einzige, einheitliche Titel** – in der Regel wird einen einzelnen, zentrierten Titel in der Detailansicht, statt einen Titel in sowohl die Detail-als auch die Masteransicht verwenden möchten.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Split-Ansichtscontroller und Storyboards

Die einfachste Möglichkeit, ein Xamarin.tvOS-app mit Split-View-Controller zu arbeiten, ist sie der app Benutzeroberfläche, die Verwendung des iOS Designers hinzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Split-View-Controller** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](split-views-images/activity01.png "Ein Controller für geteilte Ansicht")](split-views-images/activity01.png#lightbox)
1. Standardmäßig wird der iOS-Designer in der Master-Ansicht einen Navigationscontroller und eine View-Controller installiert. Wenn dies nicht den Anforderungen Ihrer app passt, einfach löschen Sie wieder.
1. Wenn Sie die Master-Standardansicht entfernen, ziehen Sie eine neue View-Controller, auf die Entwurfsoberfläche: 

    [![](split-views-images/activity02.png "Einen Ansichtscontroller")](split-views-images/activity02.png#lightbox)
1. Strg + Klick und ziehen Sie aus dem Controller für geteilte Ansicht in der neuen Master-View-Controller. 
1. Wählen Sie **Master** aus der **Popupmenü**: 

    [![](split-views-images/activity03.png "Wählen Sie im Popupmenü Master")](split-views-images/activity03.png#lightbox)
1. Entwerfen Sie den Inhalt, der die Master- und Detail-Ansichten: 

    [![](split-views-images/activity04.png "Beispiellayout")](split-views-images/activity04.png#lightbox)
1. Weisen Sie **Namen** in die **Widget Registerkarte** von der **Pad "Eigenschaften"** zum Arbeiten mit UI-Steuerelemente in C# Code.
1. Die Änderungen zu speichern und zurück zu Visual Studio für Mac.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Split-View-Controller** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](split-views-images/activity01-vs.png "Ein Controller für geteilte Ansicht")](split-views-images/activity01-vs.png#lightbox)
1. Standardmäßig wird der iOS-Designer einen Navigationscontroller und View Controller in der Master-Ansicht hinzugefügt. Wenn dies nicht den Anforderungen Ihrer app passt, einfach löschen Sie wieder.
1. Wenn Sie die Master-Standardansicht entfernen, ziehen Sie eine neue View-Controller, auf die Entwurfsoberfläche: 

    [![](split-views-images/activity02-vs.png "Einen Ansichtscontroller")](split-views-images/activity02-vs.png#lightbox)
1. Strg + Klick und ziehen Sie aus dem Controller für geteilte Ansicht in der neuen Master-View-Controller. 
1. Wählen Sie **Master** aus der **Popupmenü**: 

    [![](split-views-images/activity03-vs.png "Wählen Sie im Popupmenü Master")](split-views-images/activity03-vs.png#lightbox)
1. Entwerfen Sie den Inhalt, der die Master- und Detail-Ansichten: 

    [![](split-views-images/activity04.png "Inhaltslayout")](split-views-images/activity04.png#lightbox)
1. Weisen Sie **Namen** in die **Widget Registerkarte** von der **Eigenschaften-Explorer** zum Arbeiten mit UI-Steuerelemente in C# Code.
1. Speichern Sie die Änderungen.
    
-----

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Arbeiten mit Split-View-Controller

Wie bereits erwähnt, ist ein Controller für geteilte Ansicht oft in Situationen verwendet, in dem gefilterten Inhalte, die dem Benutzer angezeigt werden. Die Hauptkategorien werden auf der linken Seite in der Master-Ansicht angezeigt, und die gefilterten Ergebnisse auf der rechten Seite in der Detailansicht auf Grundlage der Auswahl des Benutzers.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Zugreifen auf die Master- und Detailtabelle

Wenn Sie programmgesteuert auf die Master- und Detail-View-Controller zugreifen möchten, verwenden Sie die `ViewControllers ` Eigenschaft von der Controller für geteilte Ansicht. Zum Beispiel:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Es wird als Array angezeigt, in dem das erste Element (0), in der Master-View-Controller und das zweite Element (1) die Details ist.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Zugriff auf Informationen aus dem Masterbranch

Da Sie in der Detailansicht, basierend auf der Auswahl des Benutzers in der Masterdatenbank in der Regel ausführliche Informationen anzeigen, benötigen Sie die Möglichkeit, auf die Details aus dem Master-zuzugreifen.

Die einfachste Möglichkeit hierzu ist, eine Eigenschaft in der Master-View-Controller-Klasse, z. B. verfügbar zu machen:

```csharp
public DetailViewController DetailController { get; set;}
```

Überschreiben Sie in den Controller für geteilte Ansicht, die `ViewDidLoad` -Methode und Tie zeigt die beiden zusammen. Zum Beispiel:

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

Sie können Eigenschaften und Methoden in Ihrem Ansichtscontroller Details verfügbar machen, die der Master verwenden können, um neue Daten nach Bedarf zu präsentieren.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Ein- und Ausblenden von-Master

Optional können Sie ein- und ausblenden, die Master-View-Controller mit dem `PreferredDisplayMode` Eigenschaft von der Controller für geteilte Ansicht. Zum Beispiel:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

Die `UISplitViewControllerDisplayMode` Enumeration definiert, wie der Master-View-Controller als eine der folgenden angezeigt werden:

- **Automatische** -TvOS steuern die Darstellung der Master- und Detail-Ansichten.
- **PrimaryHidden** -dies Blendet Sie aus der Master-View-Controller.
- **AllVisible** -zeigt sowohl die Master-als auch die Details View Controller Seite-an-Seite. Dies ist die normale, standarddarstellung.
- **PrimaryOverlay** – The Details View Controller unter erweitert, und wird vom Master behandelt.

Verwenden Sie zum Abrufen des aktuelle Status der Präsentation der `DisplayMode` Eigenschaft von der Controller für geteilte Ansicht.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Ansichtscontrollern Teilen innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
