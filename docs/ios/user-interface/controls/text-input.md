---
title: Texteingabe
ms.topic: article
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c45ea8cb7c0e3d12e94666d61c6fdf7e5828264e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="text-input"></a>Texteingabe

Benutzereingaben Text erfolgt mit der `UITextField` für einzeilige ein- und UITextView für mehrzeilige bearbeitbares Textfeld. Sie können entweder diese Steuerelemente auf einem Bildschirm ziehen und doppelklicken, um den ursprünglichen Text selbst festgelegt.

Die folgenden Screenshots zeigen die Symbole für diese Steuerelemente, befindet sich in der Toolbox-Block in Visual Studio für Mac:

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

Sobald Sie den Ausgang genannt haben und Storyboard-Datei gespeichert haben, ist Visual Studio für Mac aktualisiert die `.designer.cs` partielle Klasse, und Sie können die C#-Code, der das Steuerelement in der Klassendatei verweist auf Hinzufügen. Jedes Steuerelement verfügt über eigene eindeutige Eigenschaften und Ereignisse, die in C#-Code zugegriffen werden können.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

Die `UITextField` Steuerelement wird am häufigsten verwendet, um eine einzelne Zeile des Texteingabe z. B. ein Benutzername oder das Kennwort zu übernehmen. Einige der Optionen für das Anpassen des Steuerelements werden hier angezeigt:

 [![](text-input-images/image15a.png "UITextField-Eigenschaften")](text-input-images/image15a.png#lightbox)

Diese Steuerelemente werden im folgenden erläutert:

-  **Platzhalter** – Dies ist optional. Wenn festgelegt, es angezeigt wird, wenn das Textfeld "ist leer, in der Regel an, die dem Benutzer wird erläutert, welche Eingabe erwartet wird.
-  **Deaktivieren der Schaltfläche** – Dies steuert, wenn die Schaltfläche standard löschen (Grauer Kreis mit (X)) in das Textfeld "als eine Möglichkeit für den Benutzer mit schnell Klartext angezeigt wird. Es kann sein, dauerhaft ausgeblendet, dauerhaft sichtbar oder dargestellt, je nachdem, ob das Feld bearbeitet wird.
-  **Min-Font-Size** und **anpassen Anpassen** – ermöglicht die Schriftgröße automatisch angepasst, um mehr Text anpassen und zu verhindern, dass abgeschnitten werden, aber nur begrenzte keine kleiner als die angegebene Größe.
-  **Großschreibung** – an, ob Wörter, Sätze oder alle Eingaben automatisch groß geschrieben.
-  **Korrektur** – ob Rechtschreibprüfung und Vorschläge aktiviert sind.
-  **Tastatur** – Steuerelemente die Tastatur-Format angezeigt, für die Eingabe, und daher welche Schlüssel auf der Tastatur verfügbar sind. Dies schließt Ziffernblock, Phone Pad, e-Mail-URL zusammen mit anderen Optionen.
-  **Darstellung** – steuert die Darstellungsart der Tastatur und werden entweder dunkle oder helle Design.
-  **Schlüssel zurückgegeben,** – ändern Sie die Bezeichnung auf die EINGABETASTE, um besser Rechnung, welche Aktion ausgeführt werden soll. Unterstützte Werte sind Go, Join, weiter, Route, Fertig, und suchen.
-  **Sichere** – gibt an, ob die Eingabe maskiert wird (z. B. eine Kennworteingabe).


Wenn eine UITextField aufgerufen `textfield1` wurde um einen Bildschirm mit dem Designer können Sie festlegen oder ändern Sie dessen Eigenschaften in c# wie folgt:

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS bietet Enumerationen gegebenenfalls zu vereinfachen Sie die Einstellungen auswählen, z. B. soll die `UIKeyboardType` und `UIReturnKeyType` im vorherigen Codeausschnitt.

### <a name="display-text-programmatically"></a>Programmgesteuertes Anzeigen von Text

Wenn Sie nicht Ihren Bildschirm-Designer entwerfen möchten oder wenn Sie etwas Text dynamisch zur Laufzeit hinzufügen möchten, Sie erstellen und Anzeigen einer UITextField programmgesteuert in die `ViewDidLoad` Methode eines Controllers anzeigen, wie folgt:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

Die `UITextView` -Steuerelement kann verwendet werden, zum Anzeigen von nur-Lese Text oder mehrere Zeilen Texteingabe akzeptieren. Es weist viele der dieselben Optionen wie die `UITextField` (z. B. Großschreibung, Korrektur, usw.).

 [![](text-input-images/image16a.png "UITextView-Eigenschaften")](text-input-images/image16a.png#lightbox)

Die Eigenschaften umfassen:

-  **Verhalten** – ob der Text bearbeitbar oder schreibgeschützt ist.
-  **Erkennung** – erkennt und konvertiert die eingegebenen Daten durch Klicken aktivierbaren-Elementen wie z. B. Telefonnummern, die einen Aufruf auslösen können Adressen, werden links zu Maps, URLs an, öffnen in Safari, oder Datums- und Uhrzeitangaben, die werden Ereignisse im Kalender.


Wenn ein UITextView an einen Bildschirm mit dem Designer hinzugefügt wurde, können Sie festlegen oder ändern Sie ihre Eigenschaften wie folgt:

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
