---
title: Texteingabe in Xamarin.iOS
description: Dieses Dokument beschreibt die Texteingabe in einer Xamarin.iOS-app. Es erläutert die Verwendung von UITextField und UITextVIew sowohl programmgesteuert als auch in der iOS-Designer.
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b309cbdf37acaa71740a4d5d03e4824efd40f359
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61200890"
---
# <a name="text-input-in-xamarinios"></a>Texteingabe in Xamarin.iOS

Akzeptieren die Texteingabe des Benutzers erfolgt mithilfe der `UITextField` für einzeilige Eingaben und UITextView für mehrzeiligen bearbeitbaren Text. Können Sie diese beiden Steuerelemente auf einem Bildschirm ziehen und doppelklicken Sie auf, um den Begrüßungstext festzulegen.

Die folgenden Screenshots zeigen die Symbole für diese Steuerelemente, die im Verzeichnis der Pad "Toolbox" in Visual Studio für Mac:

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

Nachdem Sie Outlet benannt haben und die Storyboard-Datei gespeichert haben, ist Visual Studio für Mac aktualisiert die `.designer.cs` partielle Klasse, und Sie können hinzufügen C# Code, der das Steuerelement in der Klassendatei verweist. Jedes Steuerelement verfügt über eine eigene eindeutige Eigenschaften und Ereignisse, die in zugegriffen werden können Ihre C# Code.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

Die `UITextField` Steuerelement wird am häufigsten verwendet, eine einzige Codezeile Texteingabe wie z. B. ein Benutzername oder das Kennwort zu akzeptieren. Einige der verfügbaren Optionen zum Anpassen des Steuerelements werden hier angezeigt:

 [![](text-input-images/image15a.png "UITextField-Eigenschaften")](text-input-images/image15a.png#lightbox)

Diese Steuerelemente werden im folgenden erläutert:

-  **Platzhalter** – dieser Schritt ist optional. Wenn festgelegt, es angezeigt wird, wenn das Textfeld leer ist, in der Regel auf, die dem Benutzer wird erläutert, welche Eingabe erwartet wird.
-  **Schaltfläche "löschen"** – Dies steuert, wenn die standardmäßige Schaltfläche "löschen" (den grauen Kreis mit (X)) in das Textfeld als eine Möglichkeit für den Benutzer mit Klartext schnell angezeigt wird. Es kann sein, dauerhaft ausgeblendeten, permanent sichtbar oder dargestellt, je nachdem, ob das Feld bearbeitet wird.
-  **Minimaler Schriftgrad** und **anpassen Anpassen** – können den Schriftgrad automatisch angepasst, um mehr Text anpassen und zu verhindern, dass abgeschnitten werden, aber nur begrenzte keine kleiner als die angegebene Größe.
-  **Großschreibung** – angibt, ob für die automatische Großschreibung von Wörtern, Sätzen oder alle Eingaben.
-  **Korrektur** – ob Rechtschreibprüfung und Vorschläge aktiviert sind.
-  **Tastatur** – Steuerelemente der Tastatur-Stil angezeigt, für die Eingabe, und welche Schlüssel sind daher verfügbar, auf der Tastatur. Dies schließt Pad "Anzahl" "," Phone Pad ","-e-Mail "," URL zusammen mit anderen Optionen.
-  **Darstellung** – steuert die Darstellungsart des der Tastatur und werden entweder dunkel oder helles Design.
-  **Schlüssel zurückgegeben,** – ändern Sie die Bezeichnung für die Return-Taste, besser widerzuspiegeln, welche Aktion durchgeführt wird. Unterstützte Werte sind Go, Join, weiter, Route, Fertig, und suchen.
-  **Sichere** – gibt an, ob die Eingabe maskiert wird (z. B. eine Kennworteingabe).


Wenn ein UITextField aufgerufen `textfield1` wurde auf einen Bildschirm mit dem Designer können Sie festlegen oder ändern seine Eigenschaften im C# wie folgt:

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS enthält Enumerationen, gegebenenfalls zu vereinfachen, um die Einstellungen auswählen, Sie, wie z. B. möchten, die `UIKeyboardType` und `UIReturnKeyType` im oben stehenden Codeausschnitt.

### <a name="display-text-programmatically"></a>Programmgesteuertes Anzeigen von Text

Wenn Sie nicht auf den Bildschirm mit dem Designer entwerfen oder wenn Sie Text dynamisch zur Laufzeit hinzufügen möchten, können Sie diese erstellen und Anzeigen von ein UITextField programmgesteuert in die `ViewDidLoad` Methode einen ansichtscontroller wie folgt:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

Die `UITextView` -Steuerelement kann verwendet werden, zum Anzeigen von schreibgeschützten Text oder mehrere Zeilen Texteingaben akzeptieren. Es verfügt über zahlreiche dieselben Optionen wie die `UITextField` (z. B. Großschreibung, Korrektur, usw.).

 [![](text-input-images/image16a.png "UITextView-Eigenschaften")](text-input-images/image16a.png#lightbox)

Bestimmte Eigenschaften enthalten:

-  **Verhalten** : Legt fest, ob der Text bearbeitbar oder schreibgeschützt ist.
-  **Erkennung** – erkennt und konvertiert die eingegebenen Daten durch Klicken aktivierbaren Elemente wie z. B. Telefonnummern, die einen Aufruf, auslösen können behandelt werden, werden links zu Karten, URLs, die in Safari öffnen oder Datums- und Uhrzeitangaben, die Ereignisse werden, im Kalender.


Wenn eine UITextView zu einem Bildschirm mit dem Designer hinzugefügt wurde, können Sie festlegen oder ändern seine Eigenschaften wie folgt:

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
