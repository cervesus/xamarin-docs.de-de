---
title: Anpassen der Darstellung einer Tabelle in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie die Darstellung einer Tabelle in xamarin. IOS angepasst wird. Es werden Zellen Stile, Zubehör, Zellen Trennzeichen und benutzerdefinierte Zell Layouts erläutert.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7b4042d9090823fbeff89face7c00e5753666c97
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021909"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Anpassen der Darstellung einer Tabelle in xamarin. IOS

Die einfachste Möglichkeit, die Darstellung einer Tabelle zu ändern, besteht darin, einen anderen Zellstil zu verwenden. Sie können den Zellstil ändern, der beim Erstellen der einzelnen Zellen in der `GetCell` Methode des `UITableViewSource`verwendet wird.

## <a name="cell-styles"></a>Zell Stile

Es gibt vier integrierte Stile:

- **Standard** – unterstützt eine `UIImageView`.
- Unter **Titel** – unterstützt eine `UIImageView` und einen Untertitel.
- **Value1** – rechtsbündig ausgerichtete Untertitel unterstützt eine `UIImageView`.
- **Value2** – Titel ist rechtsbündig ausgerichtet, und Untertitel ist linksbündig (aber kein Bild).

Diese Screenshots zeigen, wie die einzelnen Stile angezeigt werden:

 [![](customizing-table-appearance-images/image7.png "These screenshots show how each style appears")](customizing-table-appearance-images/image7.png#lightbox)

Die Beispieldatei " **celldefaulobligbar** " enthält den Code zum Entwickeln dieser Bildschirme. Der Zellstil wird im `UITableViewCell`-Konstruktor wie folgt festgelegt:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

Die [unterstützten Eigenschaften](xref:UIKit.UITableViewCell) des Zellstils können dann festgelegt werden:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Bad

Zellen können das folgende Zubehör auf der rechten Seite der Ansicht hinzugefügt werden:

- **Häkchen–** kann verwendet werden, um die Mehrfachauswahl in einer Tabelle anzugeben.
- **Detailbutton** – antwortet unabhängig vom Rest der Zelle und ermöglicht es, eine andere Funktion zum Berühren der Zelle selbst auszuführen (z. b. das Öffnen eines Popups oder eines neuen Fensters, das nicht Teil eines `UINavigationController` Stapels ist).
- " **Disclosure sureindicator** " – wird normalerweise verwendet, um anzugeben, dass durch Berühren der Zelle eine andere Ansicht geöffnet wird
- **Detaildisclosure surebutton** – eine Kombination der `DetailButton` und `DisclosureIndicator`.

Dies sieht wie folgt aus:

 [![](customizing-table-appearance-images/image8.png "Sample Accessories")](customizing-table-appearance-images/image8.png#lightbox)

Um eines dieser Zubehör anzuzeigen, können Sie die `Accessory`-Eigenschaft in der `GetCell`-Methode festlegen:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Wenn die `DetailButton` oder `DetailDisclosureButton` angezeigt werden, sollten Sie die `AccessoryButtonTapped` auch überschreiben, um eine Aktion auszuführen, wenn Sie berührt wird.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Die **cellaccessorytable** -Beispiel Tabelle zeigt ein Beispiel für die Verwendung von Zubehör.

## <a name="cell-separators"></a>Zellen Trennzeichen

Zellen Trennzeichen sind Tabellenzellen, die zum Trennen der Tabelle verwendet werden. Die Eigenschaften werden für die Tabelle festgelegt.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Es ist auch möglich, dem Trennzeichen einen weich-oder vibrancy-Effekt hinzuzufügen:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Das Trennzeichen kann auch eine inmenge aufweisen:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Erstellen von benutzerdefinierten Zell Layouts

Um den visuellen Stil einer Tabelle zu ändern, müssen Sie benutzerdefinierte Zellen bereitstellen, damit Sie angezeigt werden. Die benutzerdefinierte Zelle kann verschiedene Farben und Steuerelement Layouts aufweisen.

Das cellcustomtable-Beispiel implementiert eine `UITableViewCell` Unterklasse, die ein benutzerdefiniertes Layout von `UILabel`s definiert, sowie eine `UIImage` mit verschiedenen Schriftarten und Farben. Die resultierenden Zellen sehen wie folgt aus:

 [![](customizing-table-appearance-images/image9.png "Custom Cell Layouts")](customizing-table-appearance-images/image9.png#lightbox)

Die benutzerdefinierte Cell-Klasse besteht nur aus drei Methoden:

- **Konstruktor** – erstellt die UI-Steuerelemente und legt die benutzerdefinierten Stileigenschaften fest (z. b. Schriftart, Größe und Farben).
- **Updatecell** – eine Methode, mit der `UITableView.GetCell` die Eigenschaften der Zelle festlegen können.
- **Layoutsubviews** – legen Sie den Speicherort der UI-Steuerelemente fest. In dem Beispiel hat jede Zelle das gleiche Layout, aber eine komplexere Zelle (insbesondere diejenigen mit unterschiedlichen Größen) benötigt je nach angezeigter Inhalte möglicherweise unterschiedliche Layoutpositionen.

Der gesamte Beispielcode in **cellcustomtable > CustomVegeCell.cs** folgt:

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

Die `GetCell`-Methode des `UITableViewSource` muss geändert werden, um die benutzerdefinierte Zelle zu erstellen:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```

## <a name="related-links"></a>Verwandte Links

- [Workingwithtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
