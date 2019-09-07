---
title: Arbeiten mit tvos-Navigationsleisten in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit Navigationsleisten in einer tvos-App arbeiten, die mit xamarin erstellt wurde. Er erläutert das Einrichten von Navigationsleisten in einem Storyboard und das reagieren auf Ereignisse über diese Schaltflächen.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 73474aaeb138d52536dd8ad5a7dca9be566475af
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769087"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Arbeiten mit tvos-Navigationsleisten in xamarin

Navigationsleisten können am oberen Rand der Ansichten hinzugefügt werden, um eine Titel-und optionale Navigationsleisten Schaltfläche anzuzeigen. Normalerweise werden Sie verwendet, wenn der Benutzer von einer Hauptseite aus navigiert ist, z. b. eine Tabellenansicht, eine Auflistung oder ein Menü zu einer unter Ansicht, in der die Details des ausgewählten Elements angezeigt werden.

[![](navigation-bars-images/navbar01.png "Beispiel Navigationsleiste")](navigation-bars-images/navbar01.png#lightbox)

Neben dem Titel (der in der Mitte angezeigt wird) können Navigationsleisten auf der linken und rechten Seite der Leiste eine oder mehrere`UIBarButtonItem`Navigationsleisten Schaltflächen () enthalten.

> [!IMPORTANT]
> Navigationsleisten sind standardmäßig vollständig transparent. Um sicherzustellen, dass der Inhalt der Navigationsleiste über den darin befindlichen Inhalt lesbar bleibt, sollten Sie sorgfältig vorgehen. Wenn beispielsweise Inhalt in einer Tabellenansicht oder Auflistung darunter einen Bildlauf durchführt.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Navigationsleisten und Storyboards

Die einfachste Möglichkeit, mit Navigationsleisten in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken `Main.storyboard` Sie im Lösungspad auf Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** , und legen Sie Sie in der Ansicht am oberen Rand des Bildschirms ab:

    [![](navigation-bars-images/navbar02.png "Navigationsleiste")](navigation-bars-images/navbar02.png#lightbox)
1. Doppelklicken Sie auf die **Navigationsleiste** , um das **Navigationselement**auszuwählen. Auf der Registerkarte **Widget** des **Eigenschaftenpad**können Sie den **Titel**festlegen:

    [![](navigation-bars-images/navbar03.png "Festlegen des Titels")](navigation-bars-images/navbar03.png#lightbox)
1. Als nächstes können Sie am Ende der Leiste ein oder mehrere leisten- **Schaltflächen Elemente** hinzufügen:

    [![](navigation-bars-images/navbar04.png "Ein leisten-Schaltflächen Element")](navigation-bars-images/navbar04.png#lightbox)
1. Verknüpfen Sie schließlich die leisten- **Schaltflächen Elemente** mit Aktionen auf der Registerkarte **Ereignisse** des **Eigenschaften-Explorers**:

    [![](navigation-bars-images/navbar05.png "Eine Schaltfläche für eine Element Aktion")](navigation-bars-images/navbar05.png#lightbox)
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken `Main.storyboard` Sie im Projektmappen-Explorer auf Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** , und legen Sie Sie in der Ansicht am oberen Rand des Bildschirms ab:

    [![](navigation-bars-images/navbar02-vs.png "Navigationsleiste")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Doppelklicken Sie auf die **Navigationsleiste** , um das **Navigationselement**auszuwählen. Auf der Registerkarte **Widget** des **Eigenschaften-Explorers**können Sie den **Titel**festlegen:

    [![](navigation-bars-images/navbar03-vs.png "Festlegen des Titels")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Als nächstes können Sie am Ende der Leiste ein oder mehrere leisten- **Schaltflächen Elemente** hinzufügen:

    [![](navigation-bars-images/navbar04-vs.png "Leisten-Schaltflächen Elemente")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Verknüpfen Sie schließlich die leisten- **Schaltflächen Elemente** mit Aktionen auf der Registerkarte **Ereignisse** des **Eigenschaften-Explorers**:

    [![](navigation-bars-images/navbar05-vs.png "Element Aktionen für eine leisten Schaltfläche")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Speichern Sie die Änderungen.

-----

> [!IMPORTANT]
> Obwohl es möglich ist, Ereignisse wie z `TouchUpInside` . b. einem Benutzeroberflächen Element (z. b. "UIButton") im IOS-Designer zuzuweisen, wird es nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Sie sollten das `Primary Action` -Ereignis immer verwenden, wenn Sie Ereignishandler für tvos-Benutzeroberflächen Elemente erstellen.

Der folgende Code enthält ein Beispiel für Ereignishandler für drei verschiedene barbuttonitems: `ShowFirstHotel`, `ShowSecondHotel`und `ShowThirdHotel`. Wenn auf jedes Element geklickt wird, wird das `HotelImage` Hintergrundbild geändert. Dies wird in der Ansichts Controller Datei ( `ViewController.cs`Beispieldatei) bearbeitet:

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

Solange die-Eigenschaft einer Schalt `Enabled` Fläche ist `true` und Sie nicht von einem anderen Steuerelement oder einer anderen Ansicht abgedeckt wird, kann Sie mithilfe von Siri Remote als Element im Fokus erstellt werden.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Navigationsleisten in einer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
