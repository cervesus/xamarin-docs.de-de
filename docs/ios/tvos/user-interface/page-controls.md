---
title: Arbeiten mit TvOS Seitensteuerelemente in Xamarin
description: Dieses Dokument beschreibt, wie Sie arbeiten mit Seitensteuerelementen für TvOS in einer app mit Xamarin erstellt wurde. Es enthält eine allgemeine Beschreibung der Steuerelemente der Seite wird erläutert, wie diese in Storyboards einrichten und untersucht, wie Sie auf der Seitenereignisse reagieren.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 173bc7713b5b8c330d4d4c5863bef24be8bdcb52
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179614"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>Arbeiten mit TvOS Seitensteuerelemente in Xamarin

Manchmal müssen Sie möglicherweise eine Reihe von Seiten oder Bilder in Ihrer Xamarin.tvOS-app anzuzeigen. Ein Seitensteuerelement wurde entwickelt, auf welche Seite übersichtlich ein Benutzer aus die maximale Anzahl der Seiten in befindet. Ein Seitensteuerelement zeigt eine Reihe von Punkten mit einem dunklen Oval Hintergrund strukturiert. Anzeigen der aktuellen Seite einen ausgefüllten Punkt, alle anderen Seiten angezeigt werden, als leere Punkte. Das Seiten-Steuerelement wird die äußere die meisten Punkte zugeschnitten, wenn zu viele im Hintergrundbereich anpassen.

[![](page-controls-images/page01.png "Beispiel-Seitensteuerelement")](page-controls-images/page01.png#lightbox)

Ein Steuerelement der Seite in einem nicht interaktiven Element soll nur dem Benutzer Feedback geben. Sie müssen zum Hinzufügen von anderen Steuerelementen um die aktuelle Seitenzahl (z. B. Schaltflächen oder Bewegungen) zu ändern.

Apple hat die folgenden Vorschläge, wenn ein Seitensteuerelement zu verwenden:

- **Verwendung auf voll Sammlungen nur** -Steuerelemente der Seite funktionieren am besten in einer Umgebung Vollbildmodus zum Anzeigen von mehreren Seiten, die vorhanden sind in einer einzelnen Auflistung.
- **Beschränken der Anzahl der Seiten** -Steuerelemente der Seite funktionieren am besten für zehn (10) oder weniger Seiten und höchstens zwanzig (20) Seiten. Bei mehr als 20 Seiten, erwägen Sie die Verwendung einer [Auflistungsansicht](~/ios/tvos/user-interface/collection-views.md) und die Seiten in einem Raster anzeigen.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Steuerelemente der Seite und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Seitensteuerelementen in einer Xamarin.tvOS-app ist so fügen sie der app Benutzeroberfläche, die Verwendung des iOS Designers hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

    
1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Seitensteuerelement** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](page-controls-images/page02.png "Ein Seitensteuerelement")](page-controls-images/page02.png#lightbox)
1. In der **Widget Registerkarte** von der **Pad "Eigenschaften"**, können Sie verschiedene Eigenschaften des Steuerelements wie z. B. Anpassen der **aktuelle Seite** und **Anzahl von Seiten**: 

    [![](page-controls-images/page03.png "Die Registerkarte \"Widget\"")](page-controls-images/page03.png#lightbox)
1. Fügen Sie Steuerelemente oder Gesten zur Ansicht, um rückwärts und Vorwärts durch die Auflistung von Seiten.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie auf diese Dateien im reagieren können C# Code. Zum Beispiel: 

    [![](page-controls-images/page04.png "Name des Steuerelements")](page-controls-images/page04.png#lightbox)
1. Speichern Sie die Änderungen.
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Seitensteuerelement** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](page-controls-images/page02-vs.png "Ein Seitensteuerelement")](page-controls-images/page02-vs.png#lightbox)
1. In der **Widget Registerkarte** von der **Eigenschaften-Explorer**, können Sie verschiedene Eigenschaften des Steuerelements wie z. B. Anpassen der **aktuelle Seite** und **Anzahl von Seiten**: 

    [![](page-controls-images/page03-vs.png "Die Registerkarte \"Widget\"")](page-controls-images/page03-vs.png#lightbox)
1. Fügen Sie Steuerelemente oder Gesten zur Ansicht, um rückwärts und Vorwärts durch die Auflistung von Seiten.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie auf diese Dateien im reagieren können C# Code. Zum Beispiel: 

    [![](page-controls-images/page04-vs.png "Name des Steuerelements")](page-controls-images/page04-vs.png#lightbox)
1. Speichern Sie die Änderungen.
    

-----

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Ereignisse wie z. B. `TouchUpInside` an ein UI-Element (z. B. ein UIButton) in der iOS-Designer, niemals aufgerufen wird da Apple TV besitzt eine Fingereingabe Bildschirm oder Touch-Ereignissen zu unterstützen. Verwenden Sie immer die `Primary Action` Ereignis aus, wenn der Ereignishandler für tvos verwendet. die Elemente der Benutzeroberfläche erstellen.

Bearbeiten Sie Ihr View-Controller (Beispiel `ViewController.cs`)-Datei aus, und fügen Sie den Code zum Behandeln der Seiten, die geändert wird. Zum Beispiel:

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

Werfen Sie einen genaueren Blick auf zwei Eigenschaften des Steuerelements an. Um die maximale Anzahl von Seiten anzugeben, verwenden Sie zunächst Folgendes ein:

```csharp
PageView.Pages = 6;
```

Um die aktuelle Seitenzahl zu ändern, verwenden Sie den folgenden Code:

```csharp
PageView.CurrentPage = PageNumber;
```

Die `CurrentPage` -Eigenschaft ist NULL (0) basiert, damit die erste Seite wird 0 (null) und das letzte eins minus die maximale Anzahl von Seiten.

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Seite-Steuerelement in ein Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
