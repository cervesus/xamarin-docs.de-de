---
title: Arbeiten mit Navigation Controller
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Navigationsleisten innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 3d5b4b0d3e6e9388906efa3bff2db0ea38fa8605
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-navigation-controllers"></a>Arbeiten mit Navigation Controller

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Navigationsleisten innerhalb einer Xamarin.tvOS-app._

Navigationsleisten können an den Anfang Ansichten einen Titel und eine optionale Schaltflächen Navigationsleiste angezeigt hinzugefügt werden. In der Regel werden sie verwendet, wenn der Benutzer eine Hauptseite, wie eine Tabellenansicht, Auflistung oder ein Menü zu einer Unteransicht mit den Details des ausgewählten Elements navigiert.

[ ![](navigation-bars-images/navbar01.png "Beispiel-Navigationsleiste")](navigation-bars-images/navbar01.png)

Zusätzlich zu den Titel (die im mittleren Bereich angezeigt wird), Navigationsleisten kann eine oder mehrere Schaltflächen Navigationsleiste enthalten (`UIBarButtonItem`) auf der linken und rechten Seite des Balkens.

> [!IMPORTANT]
> **Hinweis:** Navigationsleisten sind standardmäßig vollständig transparent. Sollte geachtet werden, stellen Sie sicher, dass der Inhalt der Navigationsleiste über den Inhalt darunter lesbar bleibt. Z. B. wenn Inhalt in einer Tabelle anzeigen oder die Auflistung darunter einen Bildlauf durchführt.




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Navigationsleisten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Navigationsleisten in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


1. In der **Lösung Pad**, doppelklicken Sie auf `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** und legen Sie sie in der Ansicht am oberen Rand des Bildschirms: 

    [ ![](navigation-bars-images/navbar02.png "Eine Navigationsleiste")](navigation-bars-images/navbar02.png)
1. Doppelklicken Sie auf die **Navigationsleiste** auswählen **Navigationselement**. In der **Widget** auf der Registerkarte die **Eigenschaften Pad**, können Sie festlegen, die **Titel**: 

    [ ![](navigation-bars-images/navbar03.png "Festlegen des Titels")](navigation-bars-images/navbar03.png)
1. Als Nächstes können Sie hinzufügen, eine oder mehrere **Leiste Schaltfläche Elemente** an beiden Enden des Balkens: 

    [ ![](navigation-bars-images/navbar04.png "Ein Balkenbereichsdiagramm Schaltflächenelement")](navigation-bars-images/navbar04.png)
1. Zum Schluss über das Netzwerk nach der **Leiste Schaltfläche Elemente** Aktionen in der **Ereignisse** auf der Registerkarte die **Eigenschaften-Explorer**: 

    [ ![](navigation-bars-images/navbar05.png "Ein Balkenbereichsdiagramm Schaltfläche Elementaktion")](navigation-bars-images/navbar05.png)
1. Speichern Sie die Änderungen.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** und legen Sie sie in der Ansicht am oberen Rand des Bildschirms: 

    [ ![](navigation-bars-images/navbar02-vs.png "Eine Navigationsleiste")](navigation-bars-images/navbar02-vs.png)
1. Doppelklicken Sie auf die **Navigationsleiste** auswählen **Navigationselement**. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, Sie können festlegen, die **Titel**: 

    [ ![](navigation-bars-images/navbar03-vs.png "Festlegen des Titels")](navigation-bars-images/navbar03-vs.png)
1. Als Nächstes können Sie hinzufügen, eine oder mehrere **Leiste Schaltfläche Elemente** an beiden Enden des Balkens: 

    [ ![](navigation-bars-images/navbar04-vs.png "Ein Balkenbereichsdiagramm Schaltfläche Elemente")](navigation-bars-images/navbar04-vs.png)
1. Zum Schluss über das Netzwerk nach der **Leiste Schaltfläche Elemente** Aktionen in der **Ereignisse** auf der Registerkarte die **Eigenschaften-Explorer**: 

    [ ![](navigation-bars-images/navbar05-vs.png "Ein Balkenbereichsdiagramm Element Aktionen")](navigation-bars-images/navbar05-vs.png)
1. Speichern Sie die Änderungen.


-----

> [!IMPORTANT]
> **Hinweis:** während es möglich ist, z. B. Ereignisse weisen `TouchUpInside` auf ein UI-Element (z. B. eine UIButton) in der iOS-Designer nie aufgerufen wird da Apple TV hat eine Fingereingabe Bildschirm oder Berührungsereignisse unterstützen. Sie sollten immer verwenden die `Primary Action` Ereignis aus, wenn Ereignishandler für tvos. außerdem wurden die Elemente der Benutzeroberfläche erstellen.




Der folgende Code bietet ein Beispiel für Ereignishandler auf drei verschiedene BarButtonItems: `ShowFirstHotel`, `ShowSecondHotel`, und `ShowThirdHotel`. Wenn jedes Element geklickt wird, wird das Hintergrundbild `HotelImage` geändert wird. Dies ist die View-Controller bearbeitet (Beispiel `ViewController.cs`) Datei:

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

Solange einer Schaltfläche `Enabled` Eigenschaft ist `true` und fällt nicht von einem anderen Steuerelement oder Sicht, die im Fokus-Element, das mithilfe von Siri Remote vorgenommen werden.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Navigationsleisten innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [Beispiele für tvos. außerdem wurden](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
