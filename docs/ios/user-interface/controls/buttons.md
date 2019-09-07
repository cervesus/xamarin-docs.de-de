---
title: Schaltflächen in xamarin. IOS
description: Die UIButton-Klasse wird verwendet, um verschiedene Arten von Schaltflächen in ios-Bildschirmen darzustellen. In diesem Leitfaden werden die verschiedenen Optionen zum Arbeiten mit Schaltflächen in ios vorgestellt.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/11/2018
ms.openlocfilehash: ce0c4579f13311811106a00390f95a20a0abf979
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768451"
---
# <a name="buttons-in-xamarinios"></a>Schaltflächen in xamarin. IOS

In ios stellt die `UIButton` -Klasse ein Schaltflächen-Steuerelement dar.

Die Eigenschaften einer Schaltfläche können entweder Programm gesteuert oder mit dem **Eigenschaftenpad** des IOS-Designers geändert werden:

![Die Eigenschaftenpad des IOS-Designers](buttons-images/properties.png "Die Eigenschaftenpad des IOS-Designers")

## <a name="creating-a-button-programmatically"></a>Programm gesteuertes Erstellen einer Schaltfläche

Ein `UIButton` kann nur mit wenigen Codezeilen erstellt werden.

- Instanziieren Sie eine Schaltfläche, und geben Sie Ihren Typ an:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Der Typ der Schaltfläche wird durch einen `UIButtonType`angegeben:

  - `UIButtonType.System`-Eine Schaltfläche für allgemeine Zwecke
  - `UIButtonType.DetailDisclosure`: Gibt die Verfügbarkeit detaillierter Informationen an, in der Regel zu einem bestimmten Element in einer Tabelle.
  - `UIButtonType.InfoDark`: Gibt die Verfügbarkeit von Konfigurationsinformationen an. dunkel farbig
  - `UIButtonType.InfoLight`: Gibt die Verfügbarkeit von Konfigurationsinformationen an. hell farbig
  - `UIButtonType..AddContact`-Gibt an, dass ein Kontakt hinzugefügt werden kann.
  - `UIButtonType.Custom`-Anpassbare Schaltfläche

  Weitere Informationen zu den verschiedenen Schaltflächen Typen finden Sie unter:
  
  - Der Abschnitt " [benutzerdefinierte Schaltflächen Typen](#custom-button-types) " in diesem Dokument
  - Die [Schaltflächen Typen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) Rezept
  - Die [IOS-Richtlinien für die Benutzeroberfläche](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)von Apple.

- Definieren Sie die Größe und Position der Schaltfläche:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Legen Sie den Text der Schaltfläche fest. Verwenden Sie `SetTitle` die-Methode, die den Text und `UIControlState` einen-Wert erfordert:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Weitere Informationen zum Formatieren einer Schaltfläche und zum Festlegen des Texts finden Sie unter:

  - Der Abschnitt formatieren [einer Schaltfläche](#styling-a-button) in diesem Dokument
  - Die [Schaltfläche für den Text der Schaltfläche](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) .

## <a name="handling-a-button-tap"></a>Verarbeiten einer Schaltflächen Abzweigung

Wenn Sie auf eine Schaltfläche tippen möchten, geben Sie einen Handler für `TouchUpInside` das-Ereignis der Schaltfläche an:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside`ist nicht das einzige verfügbare Schaltflächen Ereignis. `UIButton`ist eine untergeordnete Klasse `UIControl`von, die [viele verschiedene Ereignisse](xref:UIKit.UIControlEvent)definiert.

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Verwenden des IOS-Designers zum Angeben von Schaltflächen-Ereignis Handlern

Verwenden Sie die Registerkarte **Ereignisse** des **Eigenschaftenpad** , um Ereignishandler für die verschiedenen Ereignisse einer Schaltfläche anzugeben.

Geben Sie für das entsprechende Ereignis entweder den Namen eines neuen Ereignis Handlers ein, oder wählen Sie einen aus der Liste aus. Dadurch wird im Code für den Ansichts Controller der Schaltfläche ein Ereignishandler erstellt.

![Registerkarte "Ereignisse" des Eigenschaftenpad](buttons-images/image1.png "Registerkarte \"Ereignisse\" des Eigenschaftenpad")

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

`UIButton` Steuerelemente können in einer Reihe verschiedener Zustände vorhanden, jede gemäß einem `UIControlState` Wert – `Normal`, `Disabled`, `Focused`, `Highlighted`usw. Jedem Zustand kann ein eindeutiger Stil zugewiesen werden, der Programm gesteuert oder mit dem IOS-Designer angegeben wird.

> [!NOTE]
> Eine vollständige Liste aller `UIControlState` Werte finden Sie unter[`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> Dokumentation.

So legen Sie z. b. die Titel Farbe und Schatten `UIControlState.Normal`Farbe für fest:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Im folgenden Code wird der Titel der Schaltfläche auf eine attributierte (stilisierte `UIControlState.Highlighted`) Zeichenfolge für `UIControlState.Normal` und festgelegt:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Benutzerdefinierte Schaltflächen Typen

Schaltflächen mit `UIButtonType` einer `Custom` von haben keine Standard Stile. Allerdings ist es möglich, die Darstellung der Schaltfläche zu konfigurieren, indem Sie ein Bild für die unterschiedlichen Zustände festlegt:

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

Abhängig davon, ob der Benutzer die Schaltfläche berührt oder nicht, wird er als eines der folgenden Bilder (`UIControlState.Normal`, `UIControlState.Highlighted` und `UIControlState.Selected` Zustände) angezeigt:

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted")
![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

Weitere Informationen zum Arbeiten mit benutzerdefinierten Schaltflächen finden Sie im Rezept zum [Verwenden eines Bilds für eine Schaltfläche](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) .
