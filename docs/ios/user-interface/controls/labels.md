---
title: Bezeichnungen in xamarin. IOS
description: In diesem Dokument wird erläutert, wie Bezeichnungen in xamarin. IOS verwendet werden. Es wird beschrieben, wie Bezeichnungen Programm gesteuert und mit dem IOS-Designer erstellt werden.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: 41cd0eb93cee216311ea42f7ca027a1556b322e6
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227077"
---
# <a name="labels-in-xamarinios"></a>Bezeichnungen in xamarin. IOS

Das `UILabel` -Steuerelement wird zum Anzeigen von einzeiligen und mehrzeiligen schreibgeschützten Text verwendet.

## <a name="implementing-a-label"></a>Implementieren einer Bezeichnung

Eine neue Bezeichnung wird erstellt, indem eine [`UILabel`](xref:UIKit.UILabel)instanziiert wird:

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Bezeichnungen und Storyboards

Sie können Ihrer Benutzeroberfläche auch eine Bezeichnung hinzufügen, wenn Sie den IOS-Designer verwenden. Suchen Sie in der **Toolbox** nach **Bezeichnung** , und ziehen Sie Sie in die Ansicht:

![Bezeichnung in Toolbox](labels-images/image3.png)

Die folgenden Eigenschaften können im eigenschaftenpad angepasst werden:

![Eigenschaften Panel "Bezeichnung"](labels-images/image2.png)

- **Text Kontext** – plain oder attributiert. Mit nur-Text können Sie die [Formatierungs Attribute](#Formatting_Text_and_Label) für die gesamte Zeichenfolge festlegen. Mit attributierten Texten können Sie die Formatierung auf verschiedene Zeichen oder Wörter in der Zeichenfolge festlegen.
- **Farbe, Schriftart, Ausrichtung** – Formatierungs Attribute, die auf die Bezeichnung angewendet werden können.
- **Lines** – legt die Anzahl der Zeilen fest, die die Bezeichnung umfassen kann. Legen Sie diese Einstellung auf 0 fest, damit die Bezeichnung beliebig viele Zeilen verwenden kann.
- **Behavior** – kann entweder auf aktiviert oder hervorgehoben festgelegt werden. Aktiviert ist standardmäßig festgelegt. deaktivierter Text wird in einer helleren grauen Farbe angezeigt. Hervorgehoben ist standardmäßig deaktiviert und ermöglicht, dass die Bezeichnung mit einem markierten Zustand neu gezeichnet wird, wenn Sie von einem Benutzer ausgewählt wird.
- **Baselane und Zeilenumbruch** –
  - Basline bestimmt, wie der Text positioniert wird, wenn sich die Schriftart Größen von der angegebenen unterscheiden.
  - Zeilenumbrüche bestimmen, wie eine Zeichenfolge umschließt oder abgeschnitten wird, wenn Sie länger als eine einzelne Zeile ist.
- **Autoshrink** – bestimmt, wie die Schriftgröße bei Bedarf in einer Bezeichnung minimiert wird.
- **Hervorgehoben, Schatten, Offset** – ermöglicht es Ihnen, die hoch-und Schatten Farbe sowie den Schatten Offset festzulegen.

## <a name="truncating-and-wrapping"></a>Abschneiden und umwickeln

Informationen zum Verwenden der Zeilenumbrüche in ios finden Sie in der Anleitung zum [kürzen und](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) umschließen von Text.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Formatieren von Text und Bezeichnung

Zum Formatieren der Zeichenfolge, die Sie in einer Bezeichnung verwenden, können Sie entweder Formatierungs Attribute für die gesamte Zeichenfolge festlegen, oder Sie können attributierte Zeichen folgen verwenden. In den folgenden Beispielen wird gezeigt, wie diese implementiert werden:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Weitere Informationen zum Formatieren von Text `NSAttributedString` mithilfe von finden Sie im [Stil Text](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) Rezept.

Standardmäßig ist für Bezeichnungen `Enabled` der Wert true festgelegt, aber es ist möglich, ihn auf deaktiviert festzulegen, um dem Benutzer einen Hinweis zu bieten, dass ein bestimmtes Steuerelement deaktiviert ist:

```csharp
label.Enabled = false;
```

Dadurch wird die Bezeichnung auf eine helle graue Farbe festgelegt, wie im folgenden Beispiel Bild des Bildschirms "Einschränkungen" in ios veranschaulicht:

![Schaltfläche "deaktiviert" in ios](labels-images/image1.png)

Sie können auch die Farben für Hervorhebung und Schatten Text auf den Beschriftungs Text festlegen, um zusätzliche Effekte zu haben:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Dadurch wird der Text angezeigt, der wie folgt aussieht:

![Hervorhebung und Schatten Satz für Text](labels-images/image4.png)

Weitere Informationen zum Ändern der Schriftart eines UILabel finden Sie unter [Ändern der Schriftart](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) .





