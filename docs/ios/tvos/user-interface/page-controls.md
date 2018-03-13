---
title: Arbeiten mit Seitensteuerelement
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Seitensteuerelement innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f77eac8179f9e368e767bb4b586ccaa3f93e40a3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-page-control"></a>Arbeiten mit Seitensteuerelement

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Seitensteuerelement innerhalb einer Xamarin.tvOS-app._

In einigen Fällen müssen Sie eine Reihe von Seiten oder Bilder in Ihrer app Xamarin.tvOS anzuzeigen. Ein Steuerelement wurde entwickelt, welche Seite aufzuzeigen ein Benutzer auf Out die maximale Anzahl von Seiten ist. Ein Steuerelement zeigt eine Reihe von Punkten mit einem dunklen Oval Hintergrund strukturiert. Anzeigen der aktuellen Seite einen ausgefüllten Punkt, alle anderen Seiten, die als leere Punkte anzeigen. Das Seitensteuerelement wird die äußere die meisten Punkte zugeschnitten werden soll, wenn zu viele seine Hintergrundbereich zu groß.

[![](page-controls-images/page01.png "Beispiel-Steuerelements")](page-controls-images/page01.png#lightbox)

Ein Steuerelement in einem nicht interaktiven Element entwickelt, um nur dem Benutzer Rückmeldung zu geben. Sie müssen zum Hinzufügen von anderen Steuerelementen um die aktuelle Seitenzahl (z. B. Gesten oder Schaltflächen) zu ändern.

Apple hat die folgenden Vorschläge, wenn ein Steuerelement zu verwenden:

- **Verwendung auf voll Sammlungen nur** -Steuerelemente der Seite funktionieren am besten in einer Vollbild-Umgebung in einer einzelnen Auflistung anzuzeigende mehrere Seiten, die vorhanden sind.
- **Beschränken der Anzahl der Seiten** -Steuerelemente der Seite funktionieren am besten für zehn (10) oder weniger Seiten und maximal 20 (20) Seiten. Erwägen Sie für mehr als zwanzig Seiten eine [Auflistungsansicht](~/ios/tvos/user-interface/collection-views.md) und die Seiten in einem Raster anzeigen.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Steuerelemente der Seite und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Steuerelemente der Seite in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

    
1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Seitensteuerelement** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](page-controls-images/page02.png "Ein Steuerelement")](page-controls-images/page02.png#lightbox)
1. In der **Registerkarte "Widget"** von der **Eigenschaften Pad**, können Sie verschiedene Eigenschaften des Steuerelements Seite z. B. anpassen seine **aktuelle Seite** und **Seitenanzahl**: 

    [![](page-controls-images/page03.png "Die Registerkarte "Widget"")](page-controls-images/page03.png#lightbox)
1. Fügen Sie anschließend Steuerelemente oder Gesten zur Ansicht, um rückwärts und Vorwärts durch die Auflistung von Seiten.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie in C#-Code auf sie reagieren können. Zum Beispiel: 

    [![](page-controls-images/page04.png "Benennen Sie das Steuerelement")](page-controls-images/page04.png#lightbox)
1. Speichern Sie die Änderungen.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Seitensteuerelement** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](page-controls-images/page02-vs.png "Ein Steuerelement")](page-controls-images/page02-vs.png#lightbox)
1. In der **Registerkarte "Widget"** von der **Eigenschaften-Explorer**, können Sie verschiedene Eigenschaften des Steuerelements Seite z. B. Anpassen der **aktuelle Seite** und **Seitenanzahl**: 

    [![](page-controls-images/page03-vs.png "Die Registerkarte "Widget"")](page-controls-images/page03-vs.png#lightbox)
1. Fügen Sie anschließend Steuerelemente oder Gesten zur Ansicht, um rückwärts und Vorwärts durch die Auflistung von Seiten.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie in C#-Code auf sie reagieren können. Zum Beispiel: 

    [![](page-controls-images/page04-vs.png "Benennen Sie das Steuerelement")](page-controls-images/page04-vs.png#lightbox)
1. Speichern Sie die Änderungen.
    

-----

> [!IMPORTANT]
> **Hinweis:** während es möglich ist, z. B. Ereignisse weisen `TouchUpInside` auf ein UI-Element (z. B. eine UIButton) in der iOS-Designer nie aufgerufen wird da Apple TV hat eine Fingereingabe Bildschirm oder Berührungsereignisse unterstützen. Sie sollten immer verwenden die `Primary Action` Ereignis aus, wenn Ereignishandler für tvos. außerdem wurden die Elemente der Benutzeroberfläche erstellen.




Bearbeiten Sie die View-Controller (Beispiel `ViewController.cs`) Datei, und fügen Sie den Code, um die geänderten Seiten zu behandeln. Zum Beispiel:

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

Werfen Sie genauere Betrachtung zwei Eigenschaften des Steuerelements an. Um die maximale Anzahl der Seiten anzugeben, verwenden Sie zuerst Folgendes ein:

```csharp
PageView.Pages = 6;
```

Um die aktuelle Seitenzahl zu ändern, verwenden Sie den folgenden Code ein:

```csharp
PageView.CurrentPage = PageNumber;
```

Die `CurrentPage` Eigenschaft ist NULL (0) basiert, damit die erste Seite 0 (null), und des letzten wird eine minus die maximale Anzahl der Seiten.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Seitensteuerelement innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
