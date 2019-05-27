---
title: Anzeigen von Popups
description: 'Xamarin.Forms bietet zwei Benutzeroberflächenelemente, die Popupelementen ähneln: eine Warnung und ein Aktionsblatt. In diesem Artikel veranschaulicht, wie mit dem Blatt "Warnung" und "Aktion" APIs Dialogfelder angezeigt, die Benutzer auf einfache Fragen stellen und führt die Benutzer durch Aufgaben.'
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 58c98aefdf87bcd1ca819de96f67c66646c1723d
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66182302"
---
# <a name="display-pop-ups"></a>Anzeigen von Popups

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)

_Xamarin.Forms bietet zwei Benutzeroberflächenelemente, die Popupelementen ähneln: eine Warnung und ein Aktionsblatt. In diesem Artikel veranschaulicht, wie mit dem Blatt "Warnung" und "Aktion" APIs Dialogfelder angezeigt, die Benutzer auf einfache Fragen stellen und führt die Benutzer durch Aufgaben._

Eine Warnung anzuzeigen oder einen Benutzer zu einer Auswahl aufzufordern, ist eine gängige Benutzeroberflächenaufgabe. Xamarin.Forms verfügt über zwei Methoden für die [`Page`](xref:Xamarin.Forms.Page)-Klasse für die Interaktion mit dem Benutzer über ein Popupelement: [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) und [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*). Sie werden auf jeder Plattform mit entsprechenden nativen Steuerelementen gerendert.

## <a name="display-an-alert"></a>Anzeigen einer Warnung

Alle von Xamarin.Forms unterstützten Plattformen verfügen über ein modales Popupelement, um dem Benutzer eine Warnung anzuzeigen oder einfache Fragen zu stellen. Verwenden Sie die Methode [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) auf einem beliebigen [`Page`](xref:Xamarin.Forms.Page)-Element, um solche Warnungen in Xamarin.Forms anzuzeigen. Mit der folgenden Codezeile wird dem Benutzer eine einfache Meldung angezeigt:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Warnungsdialogfeld mit einer Schaltfläche")

In diesem Beispiel werden keine Informationen des Benutzers erfasst. Die Warnung wird modal angezeigt und geschlossen, sobald der Benutzer erneut mit der Anwendung interagiert.

Mithilfe der [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode kann die Antwort des Benutzers erfasst werden, indem zwei Schaltflächen angezeigt werden und ein `boolean`-Wert zurückgegeben wird. Geben Sie Text für beide Schaltflächen und `await` für die Methode an, um eine Antwort von einer Warnung zu erhalten. Wenn der Benutzer eine der Optionen auswählt, wird die Antwort an Ihren Code zurückgegeben. Beachten Sie die Schlüsselwörter `async` und `await` im folgenden Beispielcode:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "Warnungsdialogfeld mit zwei Schaltflächen")](pop-ups-images/alert2.png#lightbox "Alert Dialog with Two Buttons")

## <a name="guide-users-through-tasks"></a>Für Benutzer durch Aufgaben

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) ist ein allgemeines Benutzeroberflächenelement unter iOS. Mithilfe der [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode von Xamarin.Forms können Sie dieses Steuerelement in plattformübergreifende Apps einfügen, um native Alternativen unter Android und auf der UWP zu rendern.

Mit `await` [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) in einem beliebigen [`Page`](xref:Xamarin.Forms.Page)-Element werden die Bezeichnungen für die Nachricht und die Schaltfläche als Zeichenfolge übergeben, um ein Aktionsblatt anzuzeigen. Die Methode gibt die Bezeichnung der Zeichenfolge für die Schaltfläche zurück, auf die der Benutzer getippt hat. Hier ist ein einfaches Beispiel:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet-Dialogfeld")

Die Schaltfläche `destroy` wird anders als die anderen gerendert. Sie kann den Wert `null` aufweisen oder als dritter Zeichenfolgenparameter festgelegt werden. Im folgenden Beispiel wird die `destroy`-Schaltfläche verwendet:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Aktionsblatt-Dialogfeld mit Destroy-Schaltfläche")](pop-ups-images/action2.png#lightbox "Action Sheet Dialog with Destroy Button")

## <a name="related-links"></a>Verwandte Links

- [Popups (Popupelemente (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
