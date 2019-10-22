---
title: Popup anzeigen
description: Xamarin. Forms bietet drei Popup ähnliche Benutzeroberflächen Elemente – eine Warnung, ein Aktions Blatt und eine Eingabeaufforderung. In diesem Artikel wird die Verwendung von Warnungen, Aktions Blättern und Eingabe Aufforderungs-APIs zum Anzeigen von Dialogfeldern veranschaulicht, die Benutzer zu einfachen Fragen auffordern, Benutzer durch Aufgaben leiten und Eingabe Aufforderungen anzeigen.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: ddf0b96295f7153803db65a1fd741cc5df473730
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697092"
---
# <a name="display-pop-ups"></a>Popup anzeigen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

Wenn Sie eine Warnung anzeigen, einen Benutzer auffordern, eine Auswahl zu treffen oder eine Eingabeaufforderung anzuzeigen, ist dies eine gängige UI-Aufgabe. Xamarin. Forms verfügt über drei Methoden für die [`Page`](xref:Xamarin.Forms.Page) -Klasse für die Interaktion mit dem Benutzer über ein Popup Fenster: [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*), [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)und `DisplayPromptAsync`. Sie werden auf jeder Plattform mit entsprechenden nativen Steuerelementen gerendert.

## <a name="display-an-alert"></a>Anzeigen einer Warnung

Alle von Xamarin.Forms unterstützten Plattformen verfügen über ein modales Popupelement, um dem Benutzer eine Warnung anzuzeigen oder einfache Fragen zu stellen. Verwenden Sie die Methode [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) auf einem beliebigen [`Page`](xref:Xamarin.Forms.Page)-Element, um solche Warnungen in Xamarin.Forms anzuzeigen. Mit der folgenden Codezeile wird dem Benutzer eine einfache Meldung angezeigt:

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Alert Dialog with One Button")

In diesem Beispiel werden keine Informationen des Benutzers erfasst. Die Warnung wird modal angezeigt und geschlossen, sobald der Benutzer erneut mit der Anwendung interagiert.

Mithilfe der [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode kann die Antwort des Benutzers erfasst werden, indem zwei Schaltflächen angezeigt werden und ein `boolean`-Wert zurückgegeben wird. Geben Sie Text für beide Schaltflächen und `await` für die Methode an, um eine Antwort von einer Warnung zu erhalten. Wenn der Benutzer eine der Optionen auswählt, wird die Antwort an Ihren Code zurückgegeben. Beachten Sie die Schlüsselwörter `async` und `await` im folgenden Beispielcode:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![Display Alert](pop-ups-images/alert2-sml.png "Warn Dialogfeld mit zwei Schaltflächen")](pop-ups-images/alert2.png#lightbox "Warn Dialogfeld mit zwei Schaltflächen")

## <a name="guide-users-through-tasks"></a>Benutzer durch Aufgaben leiten

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) ist ein allgemeines Benutzeroberflächenelement unter iOS. Mithilfe der [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode von Xamarin.Forms können Sie dieses Steuerelement in plattformübergreifende Apps einfügen, um native Alternativen unter Android und auf der UWP zu rendern.

Mit `await` [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) in einem beliebigen [`Page`](xref:Xamarin.Forms.Page)-Element werden die Bezeichnungen für die Nachricht und die Schaltfläche als Zeichenfolge übergeben, um ein Aktionsblatt anzuzeigen. Die Methode gibt die Bezeichnung der Zeichenfolge für die Schaltfläche zurück, auf die der Benutzer getippt hat. Hier ist ein einfaches Beispiel:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet Dialog")

Die Schaltfläche `destroy` wird anders als die anderen gerendert. Sie kann den Wert `null` aufweisen oder als dritter Zeichenfolgenparameter festgelegt werden. Im folgenden Beispiel wird die `destroy`-Schaltfläche verwendet:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![Displayaktionsheet](pop-ups-images/action2-sml.png "Dialog Feld mit der Schaltfläche "zerstören"")](pop-ups-images/action2.png#lightbox "Dialog Feld mit der Schaltfläche "zerstören"")

## <a name="display-a-prompt"></a>Anzeigen einer Eingabeaufforderung

Um eine Aufforderung anzuzeigen, müssen Sie die `DisplayPromptAsync` in einem beliebigen [`Page`](xref:Xamarin.Forms.Page)aufrufen und dabei einen Titel und eine Nachricht als `string` Argumente übergeben:

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

Die Eingabeaufforderung wird modisch angezeigt:

[![Screenshot einer modalen Eingabeaufforderung unter IOS und Android](pop-ups-images/simple-prompt.png "Modale Eingabeaufforderung")](pop-ups-images/simple-prompt-large.png#lightbox "Modale Eingabeaufforderung")

Wenn die Schaltfläche OK abgetippt wird, wird die eingegebene Antwort als `string` zurückgegeben. Wenn die Schaltfläche Abbrechen angetippt wird, wird `null` zurückgegeben.

Die vollständige Argumentliste für die `DisplayPromptAsync`-Methode lautet:

- `title` vom Typ `string` ist der Titel, der in der Eingabeaufforderung angezeigt werden soll.
- `message` vom Typ `string` ist die Meldung, die in der Eingabeaufforderung angezeigt werden soll.
- `accept` vom Typ `string` ist der Text für die Accept-Schaltfläche. Dies ist ein optionales Argument, dessen Standardwert OK ist.
- `cancel` vom Typ "`string`" ist der Text für die Schaltfläche "Abbrechen". Dies ist ein optionales Argument, dessen Standardwert "Cancel" lautet.
- `placeholder` vom Typ `string` ist der Platzhalter Text, der in der Eingabeaufforderung angezeigt werden soll. Dies ist ein optionales Argument, dessen Standardwert `null` ist.
- `maxLength` vom Typ `int` ist die maximale Länge der Benutzer Antwort. Dies ist ein optionales Argument, dessen Standardwert-1 ist.
- `keyboard` vom Typ `Keyboard` ist der Tastatur Typ, der für die Benutzer Antwort verwendet werden soll. Dies ist ein optionales Argument, dessen Standardwert `Keyboard.Default` ist.

Das folgende Beispiel zeigt das Festlegen einiger optionaler Argumente:

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", maxLength: 2, keyboard: Keyboard.Numeric);
}
```

Dieser Code schränkt die Anzahl von Zeichen ein, die in 2 eingegeben werden können, und zeigt die numerische Tastatur für die Benutzereingabe an:

[![Screenshot einer modalen Eingabeaufforderung unter IOS und Android](pop-ups-images/keyboard-prompt.png "Modale Eingabeaufforderung")](pop-ups-images/keyboard-prompt-large.png#lightbox "Modale Eingabeaufforderung")

> [!NOTE]
> Die `DisplayPromptAsync`-Methode ist derzeit nur unter IOS und Android implementiert.

## <a name="related-links"></a>Verwandte Links

- [Popups (Popupelemente (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
