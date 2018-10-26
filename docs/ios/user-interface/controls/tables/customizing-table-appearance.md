---
title: Anpassen der Darstellung einer Tabelle in Xamarin.iOS
description: Dieses Dokument beschreibt das Anpassen der Darstellung einer Tabelle in Xamarin.iOS. Es wird erläutert, Zellstile "," Zubehör "," Zelle Trennzeichen "und" benutzerdefinierte Zelle Layouts.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0282b4b2194411d503ef7eb54b0337272e2be3ed
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121320"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Anpassen der Darstellung einer Tabelle in Xamarin.iOS

Die einfachste Möglichkeit, ändern Sie die Darstellung einer Tabelle ist die Verwendung einer anderen Zellenstil entspricht. Sie können ändern, welche Zellstil verwendet wird, beim Erstellen der jede Zelle in der `UITableViewSource`des `GetCell` Methode.

## <a name="cell-styles"></a>Zellenstile

Es gibt vier integrierte Formate ein:

-  **Standard** – unterstützt eine `UIImageView`.
-  **Untertitel** – unterstützt eine `UIImageView` und Untertitel.
-  **Wert1** – rechts ausgerichteten Untertitel, unterstützt eine `UIImageView`.
-  **Value2** – Titel ist rechts ausgerichtet und Untertitel wird linksbündig ausgerichtet (jedoch kein Bild).


Diese Screenshots zeigen, wie die einzelnen Stile angezeigt wird:

 [![](customizing-table-appearance-images/image7.png "Diese Screenshots zeigen, wie die einzelnen Stile angezeigt wird")](customizing-table-appearance-images/image7.png#lightbox)

Das Beispiel **CellDefaultTable** enthält den Code, um diese Bildschirme zu erstellen. Der Zellstil festgelegt ist, der `UITableViewCell` Konstruktor wie folgt:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Unterstützte Eigenschaften](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) der Zelle Stil kann dann festgelegt werden:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Zubehör

Zellen können die folgenden Zubehör rechts von der Sicht hinzugefügt haben:

-   **Häkchen** – kann verwendet werden, um die Mehrfachauswahl in einer Tabelle anzugeben.
-   **DetailButton** – reagiert auf die touch-unabhängig vom Rest der Zelle, führen Sie eine andere Funktion, die die Zelle selbst berühren ermöglicht (z. B. ein Popupfenster oder ein neues Fenster, die nicht öffnen, Teil einer `UINavigationController` Stack).
-   **DisclosureIndicator** – normalerweise verwendet, um anzugeben, dass die Zelle berühren einer anderen Ansicht geöffnet wird.
-   **DetailDisclosureButton** – eine Kombination aus den `DetailButton` und `DisclosureIndicator`.


Dies ist, was sie wie folgt aussehen:

 [![](customizing-table-appearance-images/image8.png "Beispiel-Zubehör")](customizing-table-appearance-images/image8.png#lightbox)

Um diese Zubehör von anzuzeigen, Sie festlegen können, die `Accessory` -Eigenschaft in der `GetCell` Methode:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Wenn die `DetailButton` oder `DetailDisclosureButton` angezeigt werden, sollten Sie auch überschreiben die `AccessoryButtonTapped` , eine Aktion auszuführen, wenn es verwendet wird.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Das Beispiel **CellAccessoryTable** zeigt ein Beispiel zur Verwendung von Zubehör.

## <a name="cell-separators"></a>Zelle Trennzeichen

Zelle Trennzeichen sind die Zellen der Tabelle verwendet, um die Tabelle zu trennen. Die Eigenschaften werden für die Tabelle festgelegt.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Es ist auch möglich, einen Effekt Weichzeichners oder Dynamik Trennzeichen hinzu:

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

Um den visuellen Stil einer Tabelle ändern, Sie benutzerdefinierte Zellen angezeigt müssen. Die benutzerdefinierte Zelle haben unterschiedliche Farben und Steuerelement Layouts.

Das CellCustomTable-Beispiel implementiert eine `UITableViewCell` Unterklasse, die definiert ein benutzerdefiniertes Layout von `UILabel`s und einem `UIImage` mit verschiedenen Schriftarten und Farben. Die resultierenden Zellen werden wie folgt aussehen:

 [![](customizing-table-appearance-images/image9.png "Benutzerdefinierte Zelle Layouts")](customizing-table-appearance-images/image9.png#lightbox)

Die benutzerdefinierte Zellenklasse besteht aus nur drei Methoden:

-   **Konstruktor** : erstellt die UI-Steuerelemente und legt die benutzerdefinierten Eigenschaften (z. b. Schriftart, Größe und Farben).
-   **UpdateCell** – eine Methode für die `UITableView.GetCell` zu verwenden, um der Zelle Eigenschaften festzulegen.
-   **LayoutSubviews** – legen Sie den Speicherort der UI-Steuerelemente. Im Beispiel hat jeder Zelle das gleiche Layout, jedoch eine komplexere Zelle (insbesondere mit unterschiedlichen Größen) möglicherweise ein anderes Layout Positionen je nach Inhalt, der angezeigt wird.


Den vollständigen Beispielcode in **CellCustomTable > CustomVegeCell.cs** folgt:

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

Die `GetCell` Methode der `UITableViewSource` muss geändert werden, um die benutzerdefinierte Zelle erstellt:

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
