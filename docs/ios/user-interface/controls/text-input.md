---
title: Text Eingabe in xamarin. IOS
description: In diesem Dokument wird die Texteingabe in einer xamarin. IOS-App beschrieben. Es wird erläutert, wie UITextField und uitextview sowohl Programm gesteuert als auch im IOS-Designer verwendet werden.
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8f47ebdd8c1ba220229c6e652af99e8fa3ae2960
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768814"
---
# <a name="text-input-in-xamarinios"></a>Text Eingabe in xamarin. IOS

Das akzeptieren von Benutzer Texteingaben wird mit `UITextField` dem für einzeilige Eingaben und uitextview für mehrzeiligen bearbeitbaren Text erreicht. Sie können eines dieser Steuerelemente auf einen Bildschirm ziehen und doppelklicken, um den Anfangstext festzulegen.

Die folgenden Screenshots zeigen die Symbole für diese Steuerelemente, die sich im Toolbox-Pad in Visual Studio für Mac befinden:

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

Nachdem Sie das Outlet benannt und die storyboarddatei gespeichert haben, wird Visual Studio für Mac die `.designer.cs` partielle Klasse aktualisieren, und Sie können Code hinzufügen C# , der auf das Steuerelement verweist, auf die Klassendatei. Jedes Steuerelement verfügt über eigene eindeutige Eigenschaften und Ereignisse, auf die im C# Code zugegriffen werden kann.

 <a name="UITextField" />

## <a name="uitextfield"></a>UITextField

Das `UITextField` Steuerelement wird am häufigsten verwendet, um eine einzelne Textzeile zu akzeptieren, z. b. einen Benutzernamen oder ein Kennwort. Einige der Optionen, die zum Anpassen des Steuer Elements verfügbar sind, werden hier angezeigt:

 [![](text-input-images/image15a.png "UITextField-Eigenschaften")](text-input-images/image15a.png#lightbox)

Diese Steuerelemente werden im folgenden erläutert:

- **Platzhalter** – Dies ist optional. Wenn diese Einstellung festgelegt ist, wird Sie angezeigt, wenn das Textfeld leer ist, in der Regel, um dem Benutzer zu erklären, welche Eingaben erwartet werden.
- **Schaltfläche "Löschen** " – diese steuert, wenn die Standard Schaltfläche "Standard" (der graue Kreis mit (X)) im Textfeld angezeigt wird, damit der Benutzer Text schnell löschen kann. Abhängig davon, ob das Feld bearbeitet wird oder nicht, kann es dauerhaft ausgeblendet, dauerhaft sichtbar oder angezeigt werden.
- **Minimale Schriftart Größe** und **Anpassung an anpassen** – ermöglicht, dass der Schrift Grad automatisch angepasst wird, sodass er länger Text einfügt und abgeschnitten, aber auf die angegebene Größe beschränkt wird.
- Groß **Schreibung – gibt** an, ob Wörter, Sätze oder alle Eingaben automatisch groß geschrieben werden sollen.
- **Korrektur** – gibt an, ob Rechtschreibprüfung und Vorschläge aktiviert sind.
- **Tastatur** – steuert den Tastatur Stil, der für die Eingabe angezeigt wird, und damit die auf der Tastatur verfügbaren Tasten. Dies schließt das Zahlen Pad, den telefonpad, die e-Mail, die URL und andere Optionen ein.
- Darstellung **– steuert** den Stil der Tastatur und ist entweder dunkel oder hell.
- **Return Key** – ändern Sie die Bezeichnung in der Rückgabetaste, um besser widerzuspiegeln, welche Aktion durchgeführt wird. Zu den unterstützten Werten zählen go, Join, Next, Route, done und Search.
- **Secure** – gibt an, ob die Eingabe maskiert wird (z. b. für eine Kenn Wort Eingabe).

Wenn ein UITextField mit `textfield1` dem Namen einem Bildschirm mit dem Designer hinzugefügt wurde, können Sie seine Eigenschaften in C# wie folgt festlegen oder ändern:

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin. IOS bietet ggf. Enumerationen, um die Auswahl der gewünschten Einstellungen zu erleichtern, z `UIKeyboardType` . b. und `UIReturnKeyType` im obigen Code Ausschnitt.

### <a name="display-text-programmatically"></a>Programm gesteuertes Anzeigen von Text

Wenn Sie den Bildschirm nicht mit dem Designer entwerfen möchten oder wenn Sie zur Laufzeit dynamisch Text hinzufügen möchten, können Sie ein UITextField in der `ViewDidLoad` -Methode eines Ansichts Controllers wie folgt Programm gesteuert erstellen und anzeigen:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />

## <a name="uitextview"></a>UITextView

Das `UITextView` -Steuerelement kann verwendet werden, um schreibgeschützten Text anzuzeigen oder mehrzeilige Texteingaben zu akzeptieren. Es verfügt über viele der gleichen Optionen wie die `UITextField` (z. b. Groß-/Kleinschreibung, Korrektur usw.).

 [![](text-input-images/image16a.png "Uitextview-Eigenschaften")](text-input-images/image16a.png#lightbox)

Zu den spezifischen Eigenschaften gehören:

- **Verhalten** – gibt an, ob der Text bearbeitbar oder schreibgeschützt ist.
- **Erkennung** – erkennt und konvertiert die ertierten Daten in klickbare Elemente, wie z. b. Telefonnummern, die einen Aufruf auslöst, Adressen, die zu Verknüpfungen zu Zuordnungen werden, URLs, die in Safari geöffnet werden, oder Datums-und Uhrzeitangaben, die im Kalender

Wenn eine uitextview einem Bildschirm mit dem Designer hinzugefügt wurde, können Sie die Eigenschaften wie folgt festlegen oder ändern:

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
