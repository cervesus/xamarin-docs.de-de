---
title: Anzeigen von Warnungen in Xamarin.iOS
description: Dieses Dokument beschreibt die Vorgehensweise zum Anzeigen von Warnungen in Xamarin.iOS mithilfe der APIs, die iOS 8 eingeführtes UIAlertController.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 788e62b30dbf533df059b0c3805e04ecf7b857aa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241333"
---
# <a name="displaying-alerts-in-xamarinios"></a>Anzeigen von Warnungen in Xamarin.iOS

Ab iOS 8, UIAlertController hat abgeschlossenen ersetzten UIActionSheet aus, und UIAlertView der sind jetzt veraltet.

Im Gegensatz zu den Klassen, die es ersetzt, die Unterklassen von UIView sind, ist die UIAlertController eine Unterklasse von UIViewController.

Verwendung `UIAlertControllerStyle` an, dass die Art der Warnung angezeigt. Diese Warnungen sind:

- **UIAlertControllerStyleActionSheet**
    * Vor iOS 8 Dies hätte eine UIActionSheet
- **UIAlertControllerStyleAlert**
    * Vor iOS 8 Dies hätte uialertview-Element 

Es gibt drei erforderlichen Schritte an, die beim Erstellen einen Warnungscontroller:

- Erstellen Sie und konfigurieren Sie die Warnung mit:
    * Titel
    * message
    * "preferredstyle"
    
- (Optional) Hinzufügen eines Textfelds
- Fügen Sie die erforderlichen Aktionen hinzu.
- Stellen Sie den Ansichtscontroller

Die einfachste Warnung enthält eine Schaltfläche aus, wie im folgenden Screenshot gezeigt:

 ![Warnung mit einer Schaltfläche](alerts-images/alert1.png)

Der Code zum Anzeigen der einer einfachen Warnung lautet wie folgt aus:

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

Auf ähnliche Weise erfolgt eine Warnung mit mehreren Optionen, aber fügen Sie zwei Aktionen hinzu. Der folgende Screenshot zeigt beispielsweise eine Warnung mit zwei Schaltflächen:

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

Warnungen können auch ein aktionsblatt, ähnlich wie im folgenden Screenshot angezeigt:

 ![Aktion-Stylesheet-Warnung](alerts-images/alert3.png)

Schaltflächen hinzugefügt werden, auf die Warnung mit dem `AddAction` Methode:

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
- [Warnungscontroller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
