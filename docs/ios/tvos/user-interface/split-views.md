---
title: Arbeiten mit tvos Split View Controllers in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit tvos geteilte Sichten in einer mit xamarin erstellten App arbeiten. Sie bietet eine allgemeine Übersicht über geteilte Ansichts Controller, ihre Verwendung mit Storyboards, den Zugriff auf die Master-und Detailansichten sowie das Anzeigen und Ausblenden der Master Ansicht.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: e42912add9dd94b9cce16d725a456b1b4da30e35
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022207"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>Arbeiten mit tvos Split View Controllers in xamarin

Ein Split View Controller stellt einen Master-und Detail Ansichts Controller nebeneinander und verwaltet Sie gleichzeitig auf dem Bildschirm. Geteilte Ansichts Controller werden verwendet, um persistente, Fokus nutzbare Inhalte in der Master Ansicht (der kleinere Abschnitt auf der linken Seite) und verwandte Details in der Detail Ansicht (der größere Abschnitt auf der rechten Seite) darzustellen.

[![](split-views-images/intro01.png "Sample Split View")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>Informationen zu geteilten Ansichts Controllern

Wie bereits erwähnt, verwaltet ein Split View Controller einen Master-und einen Detail Ansichts Controller, der nebeneinander dargestellt wird, wobei der Master die kleinere Ansicht auf der linken Seite ist. 

Außerdem kann der Master Ansichts Controller ausgeblendet oder nach Bedarf angezeigt werden: 

[![](split-views-images/intro02.png "The Master View Controller hidden")](split-views-images/intro02.png#lightbox)

Geteilte Ansichts Controller werden häufig verwendet, um eine Liste mit Filter barem Inhalt mit den Kategorien in der Master Ansicht und den gefilterten Ergebnissen in der Detail Ansicht darzustellen. Dies wird in der Regel als Tabellenansicht auf der linken Seite und in einer Auflistungs [Ansicht](~/ios/tvos/user-interface/collection-views.md) auf der rechten Seite dargestellt.

Beim Entwerfen einer Benutzeroberfläche, die einen Split View Controller erfordert, schlägt Apple die Verwendung von Master-und Detail Ansichts Controllern vor, die sich nicht ändern (nur die Inhalte werden geändert, nicht die Struktur). Wenn Sie Ansichts Controller austauschen müssen, empfiehlt es sich, einen Navigations Controller als Basis des Ansichts Controllers zu verwenden, der geändert werden muss (Master oder Detail).

Apple hat die folgenden Vorschläge zum Arbeiten mit Split View Controllers:

- **Verwenden Sie den korrekten Teilen-Prozentsatz** . standardmäßig verwendet der Split View Controller ein Drittel des Bildschirms für den Master Ansichts Controller und zwei Drittel für den Detail Ansichts Controller. Optional können Sie eine 50/50-Teilung verwenden. Wählen Sie den korrekten Prozentsatz aus, damit Ihre Inhalte auf dem Bildschirm ausgeglichen angezeigt werden.
- Beibehalten **der Hauptauswahl** : während der Inhalt in der Detail Ansicht geändert werden kann, ist die Antwort auf die Auswahl eines Benutzers in der Master Ansicht. der Inhalt der Master Ansicht sollte korrigiert werden. Außerdem sollte das aktuell ausgewählte Element in der Master Ansicht eindeutig angezeigt werden.
- **Verwenden Sie einen einzelnen, einheitlichen Titel** . in der Regel sollten Sie anstelle eines Titels sowohl in der Detailansicht als auch in der Master Ansicht einen einzelnen, zentrierten Titel in der Detailansicht verwenden.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Geteilte Ansichts Controller und Storyboards

Die einfachste Möglichkeit, mit Split View-Controllern in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie einen **geteilten Ansichts Controller** aus der **Toolbox** , und legen Sie ihn in der Ansicht ab: 

    [![](split-views-images/activity01.png "A Split View Controller")](split-views-images/activity01.png#lightbox)
1. Standardmäßig installiert der IOS-Designer einen Navigations Controller und einen Ansichts Controller in der Master Ansicht. Wenn dies nicht den Anforderungen Ihrer APP entspricht, löschen Sie Sie einfach.
1. Wenn Sie die standardmäßige Master Ansicht entfernen, ziehen Sie einen neuen Ansichts Controller auf die Entwurfs Oberfläche: 

    [![](split-views-images/activity02.png "A View Controller")](split-views-images/activity02.png#lightbox)
1. Klicken Sie mit der Maustaste auf den geteilten Ansichts Controller, und ziehen Sie ihn auf den neuen Master Ansichts Controller. 
1. Wählen Sie im **Popupmenü**die Option **Master** aus: 

    [![](split-views-images/activity03.png "Select Master from the Popup Menu")](split-views-images/activity03.png#lightbox)
1. Entwerfen Sie den Inhalt Ihrer Master-und Detail Ansichten: 

    [![](split-views-images/activity04.png "Example layout")](split-views-images/activity04.png#lightbox)
1. Weisen Sie **Namen** auf der **Registerkarte widget** des **Eigenschaftenpad** zu, um mit ihren UI C# -Steuerelementen im Code zu arbeiten.
1. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie einen **geteilten Ansichts Controller** aus der **Toolbox** , und legen Sie ihn in der Ansicht ab: 

    [![](split-views-images/activity01-vs.png "A Split View Controller")](split-views-images/activity01-vs.png#lightbox)
1. Standardmäßig fügt der IOS-Designer in der Master Ansicht einen Navigations Controller und einen Ansichts Controller hinzu. Wenn dies nicht den Anforderungen Ihrer APP entspricht, löschen Sie Sie einfach.
1. Wenn Sie die standardmäßige Master Ansicht entfernen, ziehen Sie einen neuen Ansichts Controller auf die Entwurfs Oberfläche: 

    [![](split-views-images/activity02-vs.png "A View Controller")](split-views-images/activity02-vs.png#lightbox)
1. Klicken Sie mit der Maustaste auf den geteilten Ansichts Controller, und ziehen Sie ihn auf den neuen Master Ansichts Controller. 
1. Wählen Sie im **Popupmenü**die Option **Master** aus: 

    [![](split-views-images/activity03-vs.png "Select Master from the Popup Menu")](split-views-images/activity03-vs.png#lightbox)
1. Entwerfen Sie den Inhalt Ihrer Master-und Detail Ansichten: 

    [![](split-views-images/activity04.png "Content layout")](split-views-images/activity04.png#lightbox)
1. Weisen Sie im **Eigenschaften-Explorer** auf der **Registerkarte widget** **Namen** zu, um mit Ihren C# UI-Steuerelementen im Code zu arbeiten.
1. Speichern Sie die Änderungen.

-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Arbeiten mit Split View-Controllern

Wie bereits erwähnt, wird ein Split View Controller häufig in Situationen verwendet, in denen dem Benutzer gefilterte Inhalt angezeigt wird. Die Hauptkategorien werden auf der linken Seite in der Master Ansicht angezeigt, und die gefilterten Ergebnisse werden auf der rechten Seite in der Detail Ansicht basierend auf der Auswahl des Benutzers angezeigt.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Zugreifen auf Master und Details

Wenn Sie Programm gesteuert auf die Master-und die Detail Ansicht-Controller zugreifen müssen, verwenden Sie die `ViewControllers`-Eigenschaft des Split View-Controllers. Beispiel:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Sie wird als Array dargestellt, wobei das erste Element (0) im Master Ansichts Controller und das zweite Element (1) die Details darstellen.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Zugreifen auf Details von Master

Da Sie in der Regel ausführliche Informationen in der Detail Ansicht auf der Grundlage der Benutzer Auswahl im Master anzeigen, benötigen Sie eine Möglichkeit, auf die Details vom Master zuzugreifen.

Die einfachste Möglichkeit hierfür ist das verfügbar machen einer Eigenschaft für die Master View Controller-Klasse, z. b.:

```csharp
public DetailViewController DetailController { get; set;}
```

Überschreiben Sie im Split View Controller die `ViewDidLoad`-Methode, und verknüpfen Sie die beiden Ansichten miteinander. Beispiel:

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

Sie können Eigenschaften und Methoden auf dem Detail Ansichts Controller verfügbar machen, die der Master verwenden kann, um nach Bedarf neue Daten darzustellen.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Ein-und Ausblenden des Masters

Optional können Sie den Master Ansichts Controller mit der `PreferredDisplayMode`-Eigenschaft des Split View-Controllers anzeigen und ausblenden. Beispiel:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

Die `UISplitViewControllerDisplayMode`-Aufzählung definiert, wie der Master Ansichts Controller wie folgt dargestellt wird:

- **Automatic** -tvos steuert die Darstellung der Master-und Detail Ansichten.
- **Primaryhidden** : Dadurch wird der Master Ansichts Controller ausgeblendet.
- **Allvisible** : zeigt die Master-und die Detail Ansichts Controller nebeneinander an. Dies ist die übliche Standard Präsentation.
- **Primaryoverlay** -der Detail Ansichts Controller erweitert sich unter und wird vom Master abgedeckt.

Um den aktuellen Präsentations Zustand zu erhalten, verwenden Sie die `DisplayMode`-Eigenschaft des Split View-Controllers.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Split View Controller in einer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
