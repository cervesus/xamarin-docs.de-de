---
title: Arbeiten mit TvOS Navigationsleisten in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Navigationsleisten in einer TvOS-app mit Xamarin erstellt wurde. Einrichten von Navigationsleisten in einem Storyboard und reagieren auf Ereignisse aus dieser Schaltflächen werden erörtert.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 81e6cfe1e532bcfa7616e35adb28b314587bafc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106948"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Arbeiten mit TvOS Navigationsleisten in Xamarin

Navigationsleisten können am Anfang Ansichten auf einen Titel und eine optionale Schaltflächen Navigationsleiste hinzugefügt werden. In der Regel werden diese verwendet, wenn der Benutzer aus einer Hauptseite, wie eine Tabellenansicht, Auflistung oder ein Menü, eine mit den Details des ausgewählten Elements Unteransicht navigiert ist.

[![](navigation-bars-images/navbar01.png "Beispiel-Navigationsleiste")](navigation-bars-images/navbar01.png#lightbox)

Zusätzlich zum Titel (die in der Mitte angezeigt wird), kann ein Navigationsleisten enthalten eine oder mehrere Schaltflächen Navigationsleiste (`UIBarButtonItem`) auf der linken und rechten Seite der Leiste.

> [!IMPORTANT]
> Navigationsleisten sind standardmäßig vollständig transparent. Vorsichtig sollte vorgenommen werden, um sicherzustellen, dass der Inhalt der Navigationsleiste über die Inhalte darunter lesbar bleibt. Z. B. wenn Inhalte in eine Tabelle oder Sammlung darunter verschiebt.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Navigationsleisten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Navigationsleisten in einer Xamarin.tvOS-app ist so fügen sie der app Benutzeroberfläche, die Verwendung des iOS Designers hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**, doppelklicken Sie auf `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** und legen Sie sie in der Ansicht am oberen Rand des Bildschirms: 

    [![](navigation-bars-images/navbar02.png "Eine Navigationsleiste")](navigation-bars-images/navbar02.png#lightbox)
1. Doppelklicken Sie auf die **Navigationsleiste** auswählen **Navigationselement**. In der **Widget** Registerkarte die **Pad "Eigenschaften"**, Sie können festlegen, die **Titel**: 

    [![](navigation-bars-images/navbar03.png "Legen Sie den Titel")](navigation-bars-images/navbar03.png#lightbox)
1. Sie können dann hinzufügen, eine oder mehrere **Schaltfläche Elemente** an beiden Enden des Balkens: 

    [![](navigation-bars-images/navbar04.png "Eine Schaltfläche-Element")](navigation-bars-images/navbar04.png#lightbox)
1. Schließlich über das Netzwerk von der **Schaltfläche Elemente** auf Aktionen in der **Ereignisse** Registerkarte die **Eigenschaften-Explorer**: 

    [![](navigation-bars-images/navbar05.png "Ein Balkenbereichsdiagramm Schaltfläche Elementaktion")](navigation-bars-images/navbar05.png#lightbox)
1. Speichern Sie die Änderungen.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** und legen Sie sie in der Ansicht am oberen Rand des Bildschirms: 

    [![](navigation-bars-images/navbar02-vs.png "Eine Navigationsleiste")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Doppelklicken Sie auf die **Navigationsleiste** auswählen **Navigationselement**. In der **Widget** Registerkarte die **Eigenschaften-Explorer**, Sie können festlegen, die **Titel**: 

    [![](navigation-bars-images/navbar03-vs.png "Legen Sie den Titel")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Sie können dann hinzufügen, eine oder mehrere **Schaltfläche Elemente** an beiden Enden des Balkens: 

    [![](navigation-bars-images/navbar04-vs.png "Ein Balkenbereichsdiagramm Schaltfläche-Elemente")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Schließlich über das Netzwerk von der **Schaltfläche Elemente** auf Aktionen in der **Ereignisse** Registerkarte die **Eigenschaften-Explorer**: 

    [![](navigation-bars-images/navbar05-vs.png "Ein Balkenbereichsdiagramm Schaltflächenaktionen-Element")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Speichern Sie die Änderungen.


-----

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Ereignisse wie z. B. `TouchUpInside` an ein UI-Element (z. B. ein UIButton) in der iOS-Designer, niemals aufgerufen wird da Apple TV besitzt eine Fingereingabe Bildschirm oder Touch-Ereignissen zu unterstützen. Verwenden Sie immer die `Primary Action` Ereignis aus, wenn der Ereignishandler für tvos verwendet. die Elemente der Benutzeroberfläche erstellen.

Der folgende Code zeigt ein Beispiel von Ereignishandlern auf drei verschiedenen BarButtonItems: `ShowFirstHotel`, `ShowSecondHotel`, und `ShowThirdHotel`. Wenn jedes Element geklickt wird, das Hintergrundbild `HotelImage` geändert wird. Dies wird im Ansicht-Controller bearbeitet (Beispiel `ViewController.cs`) Datei:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
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
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

So lange, wie einer Schaltfläche `Enabled` Eigenschaft `true` und nicht durch ein anderes Steuerelement oder die Sicht behandelt wird, das im Fokus-Element, das mit der entfernten Siri vorgenommen werden.

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Navigationsleisten, innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
