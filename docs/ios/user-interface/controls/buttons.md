---
title: Schaltflächen in xamarin. IOS
description: Die UIButton-Klasse wird verwendet, um verschiedene Arten von Schaltflächen in ios-Bildschirmen darzustellen. In diesem Leitfaden werden die verschiedenen Optionen zum Arbeiten mit Schaltflächen in ios vorgestellt.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2018
ms.openlocfilehash: 0619488199c202e1877e4cfa60d622ef247e2b3f
ms.sourcegitcommit: 24883be72e485e5311dd0eb91f9a22f78eeec11a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2020
ms.locfileid: "77374119"
---
# <a name="buttons-in-xamarinios"></a>Schaltflächen in xamarin. IOS

In ios stellt die `UIButton`-Klasse ein Schaltflächen-Steuerelement dar.

Die Eigenschaften einer Schaltfläche können entweder Programm gesteuert oder mit dem **Eigenschaftenpad** des IOS-Designers geändert werden:

![Die Eigenschaftenpad des IOS-Designers](buttons-images/properties.png "Die Eigenschaftenpad des IOS-Designers")

## <a name="creating-a-button-programmatically"></a>Programm gesteuertes Erstellen einer Schaltfläche

Eine `UIButton` kann nur mit wenigen Codezeilen erstellt werden.

- Instanziieren Sie eine Schaltfläche, und geben Sie Ihren Typ an:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Der Typ der Schaltfläche wird durch einen `UIButtonType`angegeben:

  - `UIButtonType.System`-Schaltfläche "Allgemein"
  - `UIButtonType.DetailDisclosure`: gibt die Verfügbarkeit detaillierter Informationen an, in der Regel zu einem bestimmten Element in einer Tabelle.
  - `UIButtonType.InfoDark`: gibt die Verfügbarkeit von Konfigurationsinformationen an. dunkel farbig
  - `UIButtonType.InfoLight`: gibt die Verfügbarkeit von Konfigurationsinformationen an. hell farbig
  - `UIButtonType..AddContact`: gibt an, dass ein Kontakt hinzugefügt werden kann.
  - `UIButtonType.Custom` anpassbare Schaltfläche

  Weitere Informationen zu den verschiedenen Schaltflächen Typen finden Sie unter:
  
  - Der Abschnitt " [benutzerdefinierte Schaltflächen Typen](#custom-button-types) " in diesem Dokument
  - Die [Schaltflächen Typen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) Rezept
  - Die [IOS-Richtlinien für die Benutzeroberfläche](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)von Apple.

- Definieren Sie die Größe und Position der Schaltfläche:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Legen Sie den Text der Schaltfläche fest. Verwenden Sie die `SetTitle`-Methode, die den Text und einen `UIControlState` Wert erfordert:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Weitere Informationen zum Formatieren einer Schaltfläche und zum Festlegen des Texts finden Sie unter:

  - Der Abschnitt formatieren [einer Schaltfläche](#styling-a-button) in diesem Dokument
  - Die [Schaltfläche für den Text der Schaltfläche](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) .

## <a name="handling-a-button-tap"></a>Verarbeiten einer Schaltflächen Abzweigung

Wenn Sie auf eine Schaltfläche tippen möchten, geben Sie einen Handler für das `TouchUpInside`-Ereignis der Schaltfläche an:

```csharp
myButton.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` ist nicht das einzige verfügbare Schaltflächen Ereignis. `UIButton` ist eine untergeordnete Klasse von `UIControl`, die [viele verschiedene Ereignisse](xref:UIKit.UIControlEvent)definiert.

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Verwenden des IOS-Designers zum Angeben von Schaltflächen-Ereignis Handlern

Verwenden Sie die Registerkarte **Ereignisse** des **Eigenschaftenpad** , um Ereignishandler für die verschiedenen Ereignisse einer Schaltfläche anzugeben.

Geben Sie für das entsprechende Ereignis entweder den Namen eines neuen Ereignis Handlers ein, oder wählen Sie einen aus der Liste aus. Dadurch wird im Code für den Ansichts Controller der Schaltfläche ein Ereignishandler erstellt.

![Registerkarte "Ereignisse" des Eigenschaftenpad](buttons-images/image1.png "Registerkarte "Ereignisse" des Eigenschaftenpad")

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

`UIButton` Steuerelemente können in einer Reihe von verschiedenen Zuständen vorhanden sein, die jeweils durch einen `UIControlState` Wert angegeben werden, der `Normal`, `Disabled`, `Focused`, `Highlighted`usw. Jedem Zustand kann ein eindeutiger Stil zugewiesen werden, der Programm gesteuert oder mit dem IOS-Designer angegeben wird.

> [!NOTE]
> Eine vollständige Liste aller `UIControlState` Werte finden Sie im [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> Onlinedokumentation.

So legen Sie z. b. die Titel Farbe und Schatten Farbe für `UIControlState.Normal`fest:

```csharp
myButton.SetTitleColor(UIColor.White, UIControlState.Normal);
myButton.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Im folgenden Code wird der Titel der Schaltfläche auf eine attributierte (stilisierte) Zeichenfolge für `UIControlState.Normal` und `UIControlState.Highlighted`festgelegt:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Benutzerdefinierte Schaltflächen Typen

Schaltflächen mit einer `UIButtonType` `Custom` haben keine Standard Stile. Allerdings ist es möglich, die Darstellung der Schaltfläche zu konfigurieren, indem Sie ein Bild für die unterschiedlichen Zustände festlegt:

```csharp
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
myButton.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

Abhängig davon, ob der Benutzer die Schaltfläche berührt oder nicht, wird er als eines der folgenden Bilder (`UIControlState.Normal`, `UIControlState.Highlighted` und `UIControlState.Selected` Zustände) angezeigt:

![Uicontrolstate. Normal](buttons-images/image22.png "Uicontrolstate. Normal")
![uicontrolstate. hervorgehoben](buttons-images/image23.png "Uicontrolstate. hervorgehoben")
![uicontrolstate. Selected](buttons-images/image24.png "Uicontrolstate. Selected")

Weitere Informationen zum Arbeiten mit benutzerdefinierten Schaltflächen finden Sie im Rezept zum [Verwenden eines Bilds für eine Schaltfläche](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) .
