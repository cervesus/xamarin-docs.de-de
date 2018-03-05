---
title: "Schaltflächen"
description: "Die UIButton-Klasse wird zum Darstellen von verschiedenen verschiedene Stile Schaltfläche iOS Bildschirme verwendet. In diesem Abschnitt werden die verschiedenen Optionen für die Arbeit mit den Schaltflächen im iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: ab7a77c44412bd22427e17a696eb43278ff7dd94
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="buttons"></a>Schaltflächen

_Die UIButton-Klasse wird zum Darstellen von verschiedenen verschiedene Stile Schaltfläche iOS Bildschirme verwendet. In diesem Abschnitt werden die verschiedenen Optionen für die Arbeit mit den Schaltflächen im iOS._

Die `UIButton`Klasse stellt ein Schaltflächen-Steuerelement in iOS. 

Eigenschaften können bearbeitet werden, der `Properties Pad` des Designers iOS:


![](buttons-images/properties.png "Das Auffüllzeichen Eigenschaften des iOS-Designers")

## <a name="creating-a-button"></a>Erstellen einer Schaltfläche

Eine UIButton kann mit nur wenigen Codezeilen in erstellt werden.

Zuerst, instanziieren Sie eine Schaltfläche "Neu", und geben Sie die Schaltfläche, die Sie benötigen:

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

Die UIButtonType sollte als einer der folgenden angegeben werden:

- **System** -Dies ist die Standardschaltfläche Typ von iOS verwendet, ist der Typ, die am häufigsten verwendet werden.
- **DetailDisclosure** -zeigt einen "nach unten drehen"-Typ der Schaltfläche "verwendet, um ausführliche Informationen anzeigen oder ausblenden.
- **InfoDark** -einem dunklen detaillierte Informationen, die Schaltfläche ein "i" in einem Kreis angezeigt.
- **InfoLight** -eine helle detaillierte Informationen, die Schaltfläche ein "i" in einem Kreis angezeigt.
- **AddContact** -Schaltfläche als eine Schaltfläche zum Hinzufügen von Kontakten angezeigt.
- **Benutzerdefinierte** -können Sie mehrere Merkmale der Schaltfläche anpassen.

Weitere Informationen zu Schaltflächentypen finden Sie der [Schaltflächentypen](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) Rezept.

Als Nächstes definieren Sie auf dem Bildschirm Größe und Position der Schaltfläche. Beispiel:

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

Um den Text in eine Schaltfläche zu ändern, verwenden die `SetTitle` Eigenschaft auf die Schaltfläche, wofür Sie eine Textzeichenfolge festlegen und eine `UIControlStyle`. Zum Beispiel:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

Festlegen von verschiedenen Eigenschaften für jeden Status können Sie für die Kommunkation Weitere Informationen an den Benutzer (z. b. Stellen Sie die Textfarbe für den deaktivierten Zustand grauen). Sie können zwischen jeden Zustand mit der iOS-Designer wechseln, oder programmgesteuert durchführen. Weitere Informationen zu den Schaltflächentext Einstellung und Status, finden Sie in der [Schaltflächentext festgelegt](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) Rezept.

## <a name="dealing-with-user-interactions"></a>Umgang mit Benutzerinteraktionen


Schaltflächen sind nicht sehr sinnvoll, es sei denn, sie beim Klicken auf Aktionen! 

In iOS werden Ereignisse auf Schaltflächen fast immer Berührungsereignisse, wie die Verwendung mit der Schaltfläche auf dem Bildschirm interagiert, durch berühren. Eine Liste aller möglichen UIControl Ereignisse aufgelisteten [hier](https://developer.apple.com/documentation/uikit/uicontrolevents), jedoch ist das am häufigsten verwendete Ereignis auf iOS `TouchUpInside`. Sie können dann einen Ereignishandler etwas tun, sobald die gedrückt erstellen:


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>Hinzufügen von Ereignissen in der iOS-Designer
 
Verwenden Sie die Registerkarte "Ereignisse" Pad-Eigenschaft, um Ereignisse für Steuerelemente hinzuzufügen.

Wählen Sie das Ereignis aus, und geben Sie den Namen des neuen Ereignishandler oder wählen Sie eine aus der Liste. Dies erstellt eine neue partielle Methode in Ihrer View Controller-Klasse.

![Registerkarte "Ereignisse"](buttons-images/image1.png)

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

UIButtons werden anders als die meisten UIKit steuert, insofern, dass sie einen Status aufweisen, damit Sie nicht einfach nur den Titel ändern können, müssen Sie es ändern Sie für jede `UIControlState`. Festlegen der Titelfarbe und Schattenfarbe erfolgt auf ähnliche Weise:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Darüber hinaus können Sie mit der Schaltfläche Titel attributierte Text. Zum Beispiel:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Benutzerdefinierte Schaltflächentypen


Beim Festlegen der `Custom` Schaltflächentyp, der das Objekt hat keine Standardrendering zurückgreifen. Sie können die Darstellung der Schaltfläche durch Festlegen eines Bilds für die verschiedenen Zustände konfigurieren. Der folgende Code veranschaulicht z. B. zum Hinzufügen von verschiedene Bilder für die `Normal`, `Highlighted` und `Selected` Zustände:


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


Je nachdem, ob der Benutzer die Schaltfläche "" oder nicht berührt, wird er als eines der folgenden Images dargestellt werden (`Normal`, `Highlighted` und `Selected` bzw. angegeben):


![](buttons-images/image22.png "UIButton Status Normal")
![](buttons-images/image23.png "UIButton Zustand hervorgehoben")
![](buttons-images/image24.png "UIButton Zustand ausgewählt")

Weitere Informationen zum Arbeiten mit benutzerdefinierten Schaltflächen finden Sie in der [verwenden Sie ein Image für eine Schaltfläche](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/).


## <a name="related-links"></a>Verwandte Links

- [UIButton-Arbeitsmappe](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
