---
title: Anzeigen von Popups
description: Xamarin.Forms stellt zwei pop-Registrierung-ähnliche Elemente der Benutzeroberfläche – eine Warnung und ein aktionsblatt bereit. In diesem Artikel wird veranschaulicht, wie das Blatt "Warnung" und "Aktion" APIs, Benutzer einfache Fragen stellen und Benutzer durch Aufgaben geführt.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996713"
---
# <a name="displaying-pop-ups"></a>Anzeigen von Popups

_Xamarin.Forms stellt zwei pop-Registrierung-ähnliche Elemente der Benutzeroberfläche – eine Warnung und ein aktionsblatt bereit. In diesem Artikel wird veranschaulicht, wie das Blatt "Warnung" und "Aktion" APIs, Benutzer einfache Fragen stellen und Benutzer durch Aufgaben geführt._

Eine Warnung oder den Benutzer aufzufordern, eine Auswahl treffen ist eine häufige benutzeroberflächenaufgabe. Xamarin.Forms verfügt über zwei Methoden für die [ `Page` ](xref:Xamarin.Forms.Page) Klasse für die Interaktion mit dem Benutzer über ein Popup-Fenster: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) und [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*). Sie werden mit entsprechenden systemeigenen Steuerelementen auf jeder Plattform gerendert.

## <a name="displaying-an-alert"></a>Eine Warnung

Alle Plattformen mit Xamarin.Forms-Unterstützung verfügen, um ein modales Popup zu warnen Sie die Benutzer stellen einfache Fragen sie. Um diese Warnungen in Xamarin.Forms anzuzeigen, verwenden Sie die [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) Methode auf einem [ `Page` ](xref:Xamarin.Forms.Page). Die folgende Codezeile zeigt eine einfache Nachricht an dem Benutzer an:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Dialogfeld \"Warnung\" mit einer Schaltfläche")

In diesem Beispiel erfasst keine Informationen des Benutzers. Die Warnung zeigt modal, und sobald den Benutzer geschlossen wird fortgesetzt, mit der Anwendung interagieren.

Die [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) Methode kann auch verwendet werden, um die Antwort des Benutzers zu erfassen, indem Sie zwei Schaltflächen darstellen und die Rückgabe einer `boolean`. Um eine Antwort von einer Warnung erhalten möchten, geben Sie Text für beide Schaltflächen und `await` der Methode. Nachdem der Benutzer eine der Optionen ausgewählt sind, die die Antwort an den Code zurückgegeben wird. Beachten Sie die `async` und `await` Schlüsselwörter in der folgende Beispielcode:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[!["DisplayAlert"](pop-ups-images/alert2-sml.png "Warnung Dialogfeld mit zwei Schaltflächen")](pop-ups-images/alert2.png#lightbox "Warnung Dialogfeld mit zwei Schaltflächen")

## <a name="guiding-users-through-tasks"></a>Grundlegende Benutzer Aufgaben

Die [UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) ist eine allgemeine Benutzeroberflächenelement in iOS. Die Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) Methode können Sie plattformübergreifende apps, native alternativen in Android und UWP Rendern dieses Steuerelement einschließt.

Um ein aktionsblatt anzuzeigen `await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) in einem [ `Page` ](xref:Xamarin.Forms.Page), die Weitergabe der Nachricht und Schaltfläche Bezeichnungen als Zeichenfolgen. Die Methode gibt die Zeichenfolge Bezeichnung der Schaltfläche, die der Benutzer geklickt hat. Ein einfaches Beispiel ist hier dargestellt:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet-Dialogfeld")

Die `destroy` Schaltfläche anders als die anderen gerendert wird, und belassen werden `null` oder als den dritten Parameter angegeben. Im folgenden Beispiel wird die `destroy` Schaltfläche:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Aktion Eigenschaftenblatt-Dialogfeld mit der Schaltfläche \"löschen\"")](pop-ups-images/action2.png#lightbox "Aktion Eigenschaftenblatt-Dialogfeld mit der Schaltfläche \"löschen\"")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, verwenden das Blatt "Warnung" und "Aktion" APIs Benutzer einfache Fragen stellen und Benutzer durch Aufgaben geführt. Xamarin.Forms verfügt über zwei Methoden für die [ `Page` ](xref:Xamarin.Forms.Page) Klasse für die Interaktion mit dem Benutzer über ein Popup-Fenster: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) und [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*), und sie sind sowohl mit den entsprechenden nativen Steuerelementen auf jeder Plattform gerendert.



## <a name="related-links"></a>Verwandte Links

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
