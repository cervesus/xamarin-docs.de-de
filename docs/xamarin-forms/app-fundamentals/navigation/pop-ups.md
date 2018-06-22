---
title: Anzeigen von Popups
description: Xamarin.Forms stellt zwei pop-Einrichtung-ähnliche Elemente der Benutzeroberfläche – eine Warnung und ein Arbeitsblatt Aktion bereit. In diesem Artikel wird veranschaulicht, wie das Blatt "Warnung" und "Aktion" APIs, Benutzer einfache Fragen stellen und als Anleitung für die Benutzer über die Aufgaben.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 97f0917e4e8670ab379aae1b2707ae08cb29bb70
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
ms.locfileid: "32018878"
---
# <a name="displaying-pop-ups"></a>Anzeigen von Popups

_Xamarin.Forms stellt zwei pop-Einrichtung-ähnliche Elemente der Benutzeroberfläche – eine Warnung und ein Arbeitsblatt Aktion bereit. In diesem Artikel wird veranschaulicht, wie das Blatt "Warnung" und "Aktion" APIs, Benutzer einfache Fragen stellen und als Anleitung für die Benutzer über die Aufgaben._

Eine Warnung aus, oder bitten, einen Benutzer eine Auswahl treffen, ist eine häufige Aufgabe für die Benutzeroberfläche. Xamarin.Forms verfügt über zwei Methoden für die [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Klasse für die Interaktion mit der Benutzer über ein Popup: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) und [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/). Sie werden mit entsprechenden systemeigenen Steuerelementen auf jeder Plattform gerendert.

## <a name="displaying-an-alert"></a>Eine Warnung

Alle Plattformen mit Xamarin.Forms unterstützt haben ein modales Popup der Benutzer gewarnt werden, oder einfache Fragen stellen. Um diese Warnungen in Xamarin.Forms anzuzeigen, verwenden Sie die [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) Methode für ein beliebiges [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Die folgende Codezeile zeigt eine einfache Meldung an dem Benutzer an:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Dialogfeld Warnung mit einer Schaltfläche")

In diesem Beispiel werden Informationen vom Benutzer nicht gesammelt. Die Warnung modal angezeigt, und sobald den Benutzer geschlossen wird fortgesetzt, interagieren mit der Anwendung.

Die [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) Methode kann auch verwendet werden, um die Antwort des Benutzers zu erfassen, indem Sie zwei Schaltflächen vorlegen und die Rückgabe einer `boolean`. Um eine Antwort von eine Warnung erhalten möchten, geben Sie ein für beide Schaltflächen und `await` der Methode. Nachdem der Benutzer eine der Optionen ausgewählt, wird die Antwort für Ihren Code zurückgegeben. Beachten Sie die `async` und `await` Schlüsselwörter im folgenden Beispielcode:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "Warnung Dialog mit zwei Schaltflächen")](pop-ups-images/alert2.png#lightbox "Warnung Dialog mit zwei Schaltflächen")

## <a name="guiding-users-through-tasks"></a>Leitfaden Benutzern über Aufgaben

Die [UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) ist eine allgemeine Benutzeroberflächenelement unter iOS. Der Xamarin.Forms [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) Methode können Sie verschiedene Plattformen apps, systemeigene Alternativen für Android und uwp-rendering dieses Steuerelement einschließt.

Ein Blatt Aktion anzuzeigende `await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) in einem [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), die Weitergabe der Nachricht und Schaltfläche Bezeichnungen als Zeichenfolgen. Die Methode gibt die Zeichenfolge Bezeichnung der Schaltfläche, die vom Benutzer auf die geklickt wurde. Ein einfaches Beispiel wird hier gezeigt:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet-Dialogfeld")

Die `destroy` Schaltfläche wird anders als die anderen gerendert, und belassen werden `null` oder als dritten Zeichenfolgenparameter angegeben. Im folgenden Beispiel wird die `destroy` Schaltfläche:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Aktion Eigenschaftenblatt-Dialogfeld mit der Schaltfläche \"löschen\"")](pop-ups-images/action2.png#lightbox "Aktion Eigenschaftenblatt-Dialogfeld mit der Schaltfläche \"löschen\"")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird veranschaulicht, mit der Warnung und Aktion Blatt APIs Benutzer einfache Fragen stellen und als Anleitung für die Benutzer über die Aufgaben. Xamarin.Forms verfügt über zwei Methoden für die [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Klasse für die Interaktion mit der Benutzer über ein Popup: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) und [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/), und sie sind sowohl mit entsprechenden systemeigenen Steuerelementen auf jeder Plattform gerendert.



## <a name="related-links"></a>Verwandte Links

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
