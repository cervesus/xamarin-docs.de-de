---
title: Arbeiten mit tvos-Seiten Steuerelementen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit tvos-Seiten Steuerelementen in einer mit xamarin erstellten App arbeiten. Sie bietet eine allgemeine Beschreibung der Seiten Steuerelemente, erläutert, wie Sie in Storyboards eingerichtet werden, und untersucht, wie auf Seiten Änderungs Ereignisse reagiert wird.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: decbbdab09b514bd49784f6ba45575ae845c547f
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226473"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>Arbeiten mit tvos-Seiten Steuerelementen in xamarin

Manchmal müssen Sie möglicherweise eine Reihe von Seiten oder Bildern in ihrer xamarin. tvos-App anzeigen. Ein Seiten Steuerelement wurde so entworfen, dass die Seite, auf der sich ein Benutzer befindet, deutlich von der maximalen Anzahl von Seiten angezeigt wird. Ein Seiten Steuerelement zeigt eine Reihe von Punkten gegen einen dunklen, oval-förmigen Hintergrund an. Auf der aktuellen Seite wird ein ausgefüllter Punkt angezeigt. alle anderen Seiten werden als hohl Punkte angezeigt. Das Seiten Steuerelement durchsucht die äußeren meisten Punkte, wenn zu viele für den Hintergrundbereich vorhanden sind.

[![](page-controls-images/page01.png "Sample Page-Steuerelement")](page-controls-images/page01.png#lightbox)

Ein Seiten Steuerelement in einem nicht interaktiven Element, das nur dem Benutzer Feedback geben soll. Sie müssen weitere Steuerelemente hinzufügen, um die aktuelle Seitenzahl (z. b. Gesten oder Schaltflächen) zu ändern.

Apple hat folgende Vorschläge, wenn Sie ein Seiten Steuerelement verwenden:

- **Nur für vollständige Sammlungen verwenden** : Seiten Steuerelemente funktionieren am besten in einer voll Bildumgebung, um mehrere Seiten anzuzeigen, die in einer einzelnen Auflistung vorhanden sind.
- **Begrenzen Sie die Anzahl der Seiten, die** Seiten Steuerelemente für zehn (10) oder weniger Seiten und maximal 20 (20) Seiten am besten funktionieren. Bei mehr als 20 Seiten sollten Sie die Verwendung einer [Sammlungsansicht](~/ios/tvos/user-interface/collection-views.md) und die Anzeige der Seiten in einem Raster in Erwägung gezogen.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Seiten Steuerelemente und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Seiten Steuerelementen in einer xamarin. tvos-App besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)


1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie ein **Seiten Steuer** Element aus der **Toolbox** , und legen Sie es in der Ansicht ab:

    [![](page-controls-images/page02.png "Ein Seiten Steuerelement")](page-controls-images/page02.png#lightbox)
1. Auf der **Registerkarte widget** des **Eigenschaftenpad**können Sie verschiedene Eigenschaften des Seiten Steuer Elements anpassen, z. b. die **aktuelle Seite** und die Anzahl **der Seiten**:

    [![](page-controls-images/page03.png "Die Widget-Registerkarte")](page-controls-images/page03.png#lightbox)
1. Fügen Sie als nächstes der Ansicht Steuerelemente oder Gesten hinzu, um die Auflistung der Seiten rückwärts und vorwärts zu bewegen.
1. Weisen Sie den Steuerelementen schließlich **Namen** zu, damit Sie im C# Code darauf reagieren können. Beispiel:

    [![](page-controls-images/page04.png "Benennen des Steuer Elements")](page-controls-images/page04.png#lightbox)
1. Speichern Sie die Änderungen.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie ein **Seiten Steuer** Element aus der **Toolbox** , und legen Sie es in der Ansicht ab:

    [![](page-controls-images/page02-vs.png "Ein Seiten Steuerelement")](page-controls-images/page02-vs.png#lightbox)
1. Auf der **Registerkarte widget** des **Eigenschaften-Explorers**können Sie verschiedene Eigenschaften des Seiten Steuer Elements anpassen, z. b. die **aktuelle Seite** und die Anzahl **der Seiten**:

    [![](page-controls-images/page03-vs.png "Die Widget-Registerkarte")](page-controls-images/page03-vs.png#lightbox)
1. Fügen Sie als nächstes der Ansicht Steuerelemente oder Gesten hinzu, um die Auflistung der Seiten rückwärts und vorwärts zu bewegen.
1. Weisen Sie den Steuerelementen schließlich **Namen** zu, damit Sie im C# Code darauf reagieren können. Beispiel:

    [![](page-controls-images/page04-vs.png "Benennen des Steuer Elements")](page-controls-images/page04-vs.png#lightbox)
1. Speichern Sie die Änderungen.


-----

> [!IMPORTANT]
> Obwohl es möglich ist, Ereignisse wie z `TouchUpInside` . b. einem Benutzeroberflächen Element (z. b. "UIButton") im IOS-Designer zuzuweisen, wird es nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Sie sollten das `Primary Action` -Ereignis immer verwenden, wenn Sie Ereignishandler für tvos-Benutzeroberflächen Elemente erstellen.

Bearbeiten Sie die Ansichts Controller `ViewController.cs`Datei (Beispieldatei), und fügen Sie den Code hinzu, der die geänderten Seiten behandelt. Beispiel:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Sehen wir uns nun die zwei Eigenschaften des Seiten Steuer Elements genauer an. Verwenden Sie zum Angeben der maximalen Anzahl von Seiten zunächst Folgendes:

```csharp
PageView.Pages = 6;
```

Verwenden Sie den folgenden Code, um die aktuelle Seitenzahl zu ändern:

```csharp
PageView.CurrentPage = PageNumber;
```

Die `CurrentPage` -Eigenschaft ist auf NULL (0) basiert, sodass die erste Seite 0 (null) und der letzte eine minus der maximalen Anzahl von Seiten sein wird.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit der Seitensteuerung in einer xamarin. tvos-App behandelt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
