---
title: Anpassen der Darstellung einer Tabelle
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a447c59e7384ce7da168efdd018bc23c2abb25c2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-tables-appearance"></a>Anpassen der Darstellung einer Tabelle

Die einfachste Methode zum Ändern der Darstellung einer Tabelle ist die Verwendung einer anderen Zellstil. Sie können ändern, welche Zellstil verwendet wird, für die Erstellung jede Zelle in der `UITableViewSource`des `GetCell` Methode.

## <a name="cell-styles"></a>Zellenstile

Es gibt vier Formatvorlagen:

-  **Standard** – unterstützt eine `UIImageView`.
-  **Untertitel** – unterstützt eine `UIImageView` und Untertitel hinzu.
-  **Wert1** – rechts ausgerichteten Untertitel unterstützt eine `UIImageView`.
-  **Wert2** – Titel ist rechts ausgerichtet und Untertitel ist (jedoch kein Bild) linksbündig ausgerichtet.


Diese Screenshots zeigen, wie jeder Produktart angezeigt wird:

 [![](customizing-table-appearance-images/image7.png "Diese Screenshots zeigen, wie jeder Produktart angezeigt wird")](customizing-table-appearance-images/image7.png#lightbox)

Im Beispiel **CellDefaultTable** enthält den Code, um diese Bildschirme zu erstellen. Der Zellenstil festgelegt ist, der `UITableViewCell` -Konstruktor wie folgt:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Unterstützt Eigenschaften](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) der Zelle Stil kann dann festgelegt werden:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Zubehör

Zellen können die folgenden Zubehör rechts von der Sicht hinzugefügt haben:

-   **Häkchen** – an, dass Mehrfachauswahl in einer Tabelle verwendet werden können.
-   **DetailButton** – sendet unabhängig von der Rest der Zelle, führen Sie eine andere Funktion auf die Zelle selbst berühren mobilgerätverwaltungszertifikat anzufassenden (z. B. Öffnen eines Popup oder ein neues Fenster, die nicht Teil einer `UINavigationController` Stapel).
-   **DisclosureIndicator** – normalerweise verwendet, um anzugeben, dass die Zelle berühren einer anderen Ansicht geöffnet wird.
-   **DetailDisclosureButton** – eine Kombination der `DetailButton` und `DisclosureIndicator`.


Dies ist das aussehen:

 [![](customizing-table-appearance-images/image8.png "Beispiel-Zubehör")](customizing-table-appearance-images/image8.png#lightbox)

Diese Zubehör von angezeigt, Sie festlegen können, die `Accessory` Eigenschaft in der `GetCell` Methode:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Wenn die `DetailButton` oder `DetailDisclosureButton` angezeigt werden, sollten Sie auch überschreiben die `AccessoryButtonTapped` eine Aktion auszuführen, wenn es verwendet wird.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Im Beispiel **CellAccessoryTable** zeigt ein Beispiel für die Verwendung von Zubehör.

## <a name="cell-separators"></a>Zelle Trennzeichen

Zelle Trennzeichen sind die Zellen der Tabelle verwendet, um die Tabelle zu trennen. Die Eigenschaften werden für die Tabelle festgelegt.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Es ist auch möglich, einen Effekt Blur oder Dynamik der Trennzeichen hinzufügen:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Das Trennzeichen kann auch ein Inset haben:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Erstellen von benutzerdefinierten Zelle Layouts

So ändern Sie den visuellen Stil einer Tabelle Sie benutzerdefinierte Zellen dafür anzuzeigenden bereitstellen müssen. Die benutzerdefinierte Zelle kann unterschiedliche Farben und Steuerelement Layouts haben.

Das CellCustomTable-Beispiel implementiert eine `UITableViewCell` Unterklasse, die definiert ein benutzerdefiniertes Layout des `UILabel`s und einer `UIImage` mit verschiedenen Schriftarten und Farben. Die resultierenden Zellen aussehen wie folgt:

 [![](customizing-table-appearance-images/image9.png "Benutzerdefinierte Zelle Layouts")](customizing-table-appearance-images/image9.png#lightbox)

Die benutzerdefinierte Zellenklasse besteht nur drei Methoden:

-   **Konstruktor** – die UI-Steuerelemente erstellt, und legt die Eigenschaften des benutzerdefinierten Stil (z. b. Schriftart, Größe und Farben).
-   **UpdateCell** – eine Methode zum `UITableView.GetCell` zu verwenden, um die Zelle Eigenschaften festzulegen.
-   **LayoutSubviews** – Festlegen des Speicherortes für die UI-Steuerelemente. Im Beispiel wird jede Zelle dasselbe Layout verfügt, aber eine komplexere Zelle (speziell jene mit unterschiedlichen Größen) möglicherweise ein anderes Layout Positionen je nach den Inhalt angezeigt wird.


Der vollständige Beispielcode in **CellCustomTable > CustomVegeCell.cs** folgt:

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

Die `GetCell` Methode der `UITableViewSource` muss geändert werden, um die benutzerdefinierte Zelle zu erstellen:

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

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
