---
title: Arbeiten mit tvos-Navigationsleisten in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit Navigationsleisten in einer tvos-App arbeiten, die mit xamarin erstellt wurde. Er erläutert das Einrichten von Navigationsleisten in einem Storyboard und das reagieren auf Ereignisse über diese Schaltflächen.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 0f3c91e175e5ccdefeaf3d6c9c83e9eb3e012e14
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935394"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Arbeiten mit tvos-Navigationsleisten in xamarin

Navigationsleisten können am oberen Rand der Ansichten hinzugefügt werden, um eine Titel-und optionale Navigationsleisten Schaltfläche anzuzeigen. Normalerweise werden Sie verwendet, wenn der Benutzer von einer Hauptseite aus navigiert ist, z. b. eine Tabellenansicht, eine Auflistung oder ein Menü zu einer unter Ansicht, in der die Details des ausgewählten Elements angezeigt werden.

[![Beispiel Navigationsleiste](navigation-bars-images/navbar01.png)](navigation-bars-images/navbar01.png#lightbox)

Neben dem Titel (der in der Mitte angezeigt wird) können Navigationsleisten `UIBarButtonItem` auf der linken und rechten Seite der Leiste eine oder mehrere Navigationsleisten Schaltflächen () enthalten.

> [!IMPORTANT]
> Navigationsleisten sind standardmäßig vollständig transparent. Um sicherzustellen, dass der Inhalt der Navigationsleiste über den darin befindlichen Inhalt lesbar bleibt, sollten Sie sorgfältig vorgehen. Wenn beispielsweise Inhalt in einer Tabellenansicht oder Auflistung darunter einen Bildlauf durchführt.

<a name="Navigation-Bars-and-Storyboards"></a>

## <a name="navigation-bars-and-storyboards"></a>Navigationsleisten und Storyboards

Die einfachste Möglichkeit, mit Navigationsleisten in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** , und legen Sie Sie in der Ansicht am oberen Rand des Bildschirms ab:

    [![Navigationsleiste](navigation-bars-images/navbar02.png)](navigation-bars-images/navbar02.png#lightbox)
1. Doppelklicken Sie auf die **Navigationsleiste** , um das **Navigationselement**auszuwählen. Auf der Registerkarte **Widget** des **Eigenschaftenpad**können Sie den **Titel**festlegen:

    [![Festlegen des Titels](navigation-bars-images/navbar03.png)](navigation-bars-images/navbar03.png#lightbox)
1. Als nächstes können Sie am Ende der Leiste ein oder mehrere leisten- **Schaltflächen Elemente** hinzufügen:

    [![Ein leisten-Schaltflächen Element](navigation-bars-images/navbar04.png)](navigation-bars-images/navbar04.png#lightbox)
1. Verknüpfen Sie schließlich die leisten- **Schaltflächen Elemente** mit Aktionen auf der Registerkarte **Ereignisse** des **Eigenschaften-Explorers**:

    [![Eine Schaltfläche für eine Element Aktion](navigation-bars-images/navbar05.png)](navigation-bars-images/navbar05.png#lightbox)
1. Speichern Sie die Änderungen.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie eine **Navigationsleiste** aus der **Toolbox** , und legen Sie Sie in der Ansicht am oberen Rand des Bildschirms ab:

    [![Navigationsleiste](navigation-bars-images/navbar02-vs.png)](navigation-bars-images/navbar02-vs.png#lightbox)
1. Doppelklicken Sie auf die **Navigationsleiste** , um das **Navigationselement**auszuwählen. Auf der Registerkarte **Widget** des **Eigenschaften-Explorers**können Sie den **Titel**festlegen:

    [![Festlegen des Titels](navigation-bars-images/navbar03-vs.png)](navigation-bars-images/navbar03-vs.png#lightbox)
1. Als nächstes können Sie am Ende der Leiste ein oder mehrere leisten- **Schaltflächen Elemente** hinzufügen:

    [![Leisten-Schaltflächen Elemente](navigation-bars-images/navbar04-vs.png)](navigation-bars-images/navbar04-vs.png#lightbox)
1. Verknüpfen Sie schließlich die leisten- **Schaltflächen Elemente** mit Aktionen auf der Registerkarte **Ereignisse** des **Eigenschaften-Explorers**:

    [![Element Aktionen für eine leisten Schaltfläche](navigation-bars-images/navbar05-vs.png)](navigation-bars-images/navbar05-vs.png#lightbox)
1. Speichern Sie die Änderungen.

-----

> [!IMPORTANT]
> Obwohl es möglich ist, Ereignisse wie z. b. einem Benutzeroberflächen Element (z. b. " `TouchUpInside` UIButton") im IOS-Designer zuzuweisen, wird es nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Sie sollten das-Ereignis immer verwenden, `Primary Action` Wenn Sie Ereignishandler für tvos-Benutzeroberflächen Elemente erstellen.

Der folgende Code enthält ein Beispiel für Ereignishandler für drei verschiedene barbuttonitems: `ShowFirstHotel` , `ShowSecondHotel` und `ShowThirdHotel` . Wenn auf jedes Element geklickt wird, wird das Hintergrundbild `HotelImage` geändert. Dies wird in der Ansichts Controller Datei (Beispiel `ViewController.cs` Datei) bearbeitet:

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

Solange die-Eigenschaft einer Schaltfläche `Enabled` ist `true` und Sie nicht von einem anderen Steuerelement oder einer anderen Ansicht abgedeckt wird, kann Sie mithilfe von Siri Remote als Element im Fokus erstellt werden.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Navigationsleisten in einer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Ähnliche Themen

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
