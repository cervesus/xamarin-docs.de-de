---
title: Anzeigen von Warnungen
ms.topic: article
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 09f4178d5d6e12388771fa63875fbe1f489c959a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="displaying-alerts"></a>Anzeigen von Warnungen

Ab mit iOS 8 hat UIAlertController abgeschlossenen ersetzten UIActionSheet und UIAlertView die nun veraltet sind.

Im Gegensatz zu den Klassen, mit denen, die er ersetzt, die Unterklassen von UIView sind, ist die UIAlertController eine Unterklasse von UIViewController.

Verwendung `UIAlertControllerStyle` an, dass der Typ der Warnung angezeigt. Diese Warnungstypen sind:

- **UIAlertControllerStyleActionSheet**
    * Erforderliche iOS 8 Dies hätte eine UIActionSheet
- **UIAlertControllerStyleAlert**
    * Erforderliche iOS 8 Dies hätte UIAlertView 

Es gibt drei erforderlichen Schritte an, die bei einer Warnung Controller zu erstellen:

- Erstellen Sie und konfigurieren Sie die Warnung mit a:
    * Titel
    * message
    * preferredStyle
    
- (Optional) Hinzufügen eines Textfelds
- Fügen Sie die erforderlichen Aktionen hinzu.
- Präsentieren Sie die View-Controller

Die einfachste Warnung enthält ein einzelnes Optionsfeld an, wie in diesem Screenshot gezeigt:

 ![Warnung mit einer Schaltfläche](alerts-images/alert1.png)

Der Code zum Anzeigen der einer einfachen Warnung lautet wie folgt:

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

In ähnlicher Weise erfolgt eine Warnung mit mehreren Optionen, aber zwei Aktionen hinzufügen. Das folgende Bildschirmfoto zeigt z. B. eine Warnung mit zwei Schaltflächen:

 ![ Warnung mit zwei Schaltflächen](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Warnungen können auch eine Aktion Arbeitsblatt, mit dem folgenden Screenshot vergleichbarer anzeigen:

 ![Aktion Blatt Warnung](alerts-images/alert3.png)

Schaltflächen hinzugefügt werden, auf die Warnung mit der `AddAction` Methode:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
- [Warnung-Controller](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
