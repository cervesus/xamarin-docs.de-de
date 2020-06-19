---
title: Popup anzeigen
description: Xamarin.Formsbietet drei Popup ähnliche Benutzeroberflächen Elemente – eine Warnung, ein Aktions Blatt und eine Eingabeaufforderung. In diesem Artikel wird die Verwendung von Warnungen, Aktions Blättern und Eingabe Aufforderungs-APIs zum Anzeigen von Dialogfeldern veranschaulicht, die Benutzer zu einfachen Fragen auffordern, Benutzer durch Aufgaben leiten und Eingabe Aufforderungen anzeigen.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a7ddd9134b7214b84a883e171d7b0cadaba3390b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136317"
---
# <a name="display-pop-ups"></a>Popup anzeigen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

Wenn Sie eine Warnung anzeigen, einen Benutzer auffordern, eine Auswahl zu treffen oder eine Eingabeaufforderung anzuzeigen, ist dies eine gängige UI-Aufgabe. Xamarin.Formsverfügt über drei Methoden für die- [`Page`](xref:Xamarin.Forms.Page) Klasse für die Interaktion mit dem Benutzer über ein Popup: [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) , [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) und `DisplayPromptAsync` . Sie werden auf jeder Plattform mit entsprechenden nativen Steuerelementen gerendert.

## <a name="display-an-alert"></a>Anzeigen einer Warnung

Alle Xamarin.Forms unterstützten Plattformen verfügen über ein modales Popup, um den Benutzer zu benachrichtigen oder einfache Fragen zu diesen zu stellen. Um diese Warnungen in anzuzeigen Xamarin.Forms , verwenden Sie die- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) Methode für alle [`Page`](xref:Xamarin.Forms.Page) . Mit der folgenden Codezeile wird dem Benutzer eine einfache Meldung angezeigt:

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Alert Dialog with One Button")

In diesem Beispiel werden keine Informationen des Benutzers erfasst. Die Warnung wird modal angezeigt und geschlossen, sobald der Benutzer erneut mit der Anwendung interagiert.

Die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) -Methode kann auch verwendet werden, um die Antwort eines Benutzers zu erfassen, indem zwei Schaltflächen und zurückgegeben werden `boolean` . Geben Sie Text für beide Schaltflächen und `await` für die Methode an, um eine Antwort von einer Warnung zu erhalten. Wenn der Benutzer eine der Optionen auswählt, wird die Antwort an Ihren Code zurückgegeben. Beachten Sie die Schlüsselwörter `async` und `await` im folgenden Beispielcode:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![Display Alert](pop-ups-images/alert2-sml.png "Warn Dialogfeld mit zwei Schaltflächen")](pop-ups-images/alert2.png#lightbox "Warn Dialogfeld mit zwei Schaltflächen")

## <a name="guide-users-through-tasks"></a>Benutzer durch Aufgaben leiten

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) ist ein allgemeines Benutzeroberflächenelement unter iOS. Mit der- Xamarin.Forms [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) Methode können Sie dieses Steuerelement in plattformübergreifende apps einschließen und Native Alternativen in Android und UWP rendern.

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

Um eine Aufforderung anzuzeigen, müssen Sie den `DisplayPromptAsync` in any aufrufen [`Page`](xref:Xamarin.Forms.Page) und einen Titel und eine Nachricht als `string` Argumente übergeben:

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

Die Eingabeaufforderung wird modisch angezeigt:

[![Screenshot einer modalen Eingabeaufforderung unter IOS und Android](pop-ups-images/simple-prompt.png "Modale Eingabeaufforderung")](pop-ups-images/simple-prompt-large.png#lightbox "Modale Eingabeaufforderung")

Wenn die Schaltfläche OK abgetippt wird, wird die eingegebene Antwort als zurückgegeben `string` . Wenn die Schaltfläche Abbrechen angetippt wird, `null` wird zurückgegeben.

Die vollständige Argumentliste für die `DisplayPromptAsync` Methode ist:

- `title`, vom Typ `string` , ist der Titel, der in der Eingabeaufforderung angezeigt werden soll.
- `message`, vom Typ `string` , ist die Meldung, die in der Eingabeaufforderung angezeigt werden soll.
- `accept`ist vom Typ `string` , der Text für die Accept-Schaltfläche. Dies ist ein optionales Argument, dessen Standardwert OK ist.
- `cancel`ist vom Typ `string` , ist der Text für die Schaltfläche Abbrechen. Dies ist ein optionales Argument, dessen Standardwert "Cancel" lautet.
- `placeholder`ist vom Typ `string` , der Platzhalter Text, der in der Eingabeaufforderung angezeigt werden soll. Dies ist ein optionales Argument, dessen Standardwert ist `null` .
- `maxLength`, vom Typ `int` , ist die maximale Länge der Benutzer Antwort. Dies ist ein optionales Argument, dessen Standardwert-1 ist.
- `keyboard`ist vom Typ `Keyboard` , der für die Benutzer Antwort zu verwendende Tastatur-Typ. Dies ist ein optionales Argument, dessen Standardwert ist `Keyboard.Default` .
- `initialValue`ist vom Typ `string` eine vordefinierte Antwort, die angezeigt wird und bearbeitet werden kann. Dies ist ein optionales Argument, dessen Standardwert leer ist `string` .

Das folgende Beispiel zeigt das Festlegen einiger optionaler Argumente:

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", initialValue: "10", maxLength: 2, keyboard: Keyboard.Numeric);
```

Dieser Code zeigt eine vordefinierte Antwort von 10 an, schränkt die Anzahl der Zeichen ein, die in 2 eingegeben werden können, und zeigt die numerische Tastatur für die Benutzereingabe an:

[![Screenshot einer modalen Eingabeaufforderung unter IOS und Android](pop-ups-images/keyboard-prompt.png "Modale Eingabeaufforderung")](pop-ups-images/keyboard-prompt-large.png#lightbox "Modale Eingabeaufforderung")

## <a name="related-links"></a>Verwandte Links

- [Popups (Popupelemente (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
