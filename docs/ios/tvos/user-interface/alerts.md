---
title: Arbeiten mit TvOS-Warnungen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit TvOS-Warnungen in Xamarin. Es wird erläutert, eine Warnung, Hinzufügen von Textfeldern und eine Hilfsklasse.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 13c5ae3fac76ec1ec1a0ade135d5919403066226
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111283"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Arbeiten mit TvOS-Warnungen in Xamarin

_Dieser Artikel behandelt die Arbeit mit UIAlertController eine Warnmeldung, die dem Benutzer in Xamarin.tvOS angezeigt._

Wenn Sie die Aufmerksamkeit des Benutzers TvOS zu erhalten oder Fragen die Berechtigung zum Ausführen einer destruktiven Aktion (z. B. das Löschen einer Datei) müssen, können Sie darstellen, eine Warnmeldung mit dem `UIAlertViewController`:

[![](alerts-images/alert01.png "Ein Beispiel für UIAlertViewController")](alerts-images/alert01.png#lightbox)

Wenn zusätzlich zum Anzeigen einer Meldung, Sie hinzufügen können Schaltflächen und Textfelder auf eine Warnung, damit Benutzer die Aktionen zu reagieren und Feedback geben.

<a name="About-Alerts" />

## <a name="about-alerts"></a>Informationen zu Warnungen

Wie bereits erwähnt, werden Warnungen verwendet, die Aufmerksamkeit des Benutzers und informieren Sie ihn über den Status der Feedback-app oder die Anforderung. Warnungen müssen einen Titel vorhanden ist, können sie optional eine Nachricht und eine oder mehrere Schaltflächen oder Text-Felder haben.

[![](alerts-images/alert04.png "Ein Beispiel für Warnung")](alerts-images/alert04.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit Warnungen:

- **Verwenden Sie Warnungen nur selten** -Warnungen der benutzerflow mit der app unterbrochen und die benutzerfreundlichkeit zu unterbrechen und sollte daher nur für wichtige Situationen wie Benachrichtigungen, die In-App-Käufe und schädlicher Aktionen verwendet werden. 
- **Auswahlmöglichkeiten für nützlich** : Wenn die Warnung Optionen für den Benutzer präsentiert Ihre sollten sicherstellen, dass jede Option wichtigen Informationen bietet und empfohlene Maßnahmen für den Benutzer zu.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Warnungstitel und Nachrichten

Apple hat die folgenden Vorschläge zur Darstellung von Titel und eine optionale Nachricht einer Warnung an:

- **Multiword Titel** – eine Warnung der Titel sollte den Punkt der Situation in klar und trotzdem einfache abrufen. Ein einzelnes Wort-Titel enthält nur selten genügend Informationen.
- **Beschreibende Titel, die eine Nachricht nicht benötigen** : wo immer dies möglich ist, markieren Sie die Warnung Titel beschreibenden-ausreichend, die optional die Meldung nicht wird erforderlich. 
- **Stellen Sie der Nachricht einen kurzen vollständiger Satz** : Wenn die optionale Nachricht erforderlich ist, um den Punkt der Warnung auf, halten Sie es so einfach wie möglich aus, und stellen es einen vollständigen Satz mit korrekter Großschreibung und Interpunktion.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Warnung Schaltflächen

Apple hat die folgenden Vorschläge für das Hinzufügen von Schaltflächen zu einer Warnung an:

- **Beschränkung für die zwei Schaltflächen** : wo immer dies möglich ist, beschränken Sie die Warnung auf ein Maximum von zwei Schaltflächen. Einzelne Schaltfläche-Warnungen bieten Informationen, aber keine Aktionen. Zwei Schaltfläche Warnungen – bieten eine einfache Ja/Nein-Auswahl der Aktion.
- **Succinct, logische Schaltfläche Titel verwenden** -einfach am besten, ein bis zwei Word Schaltfläche Softwaretitel an, die klar auf der Schaltfläche Aktion beschreiben funktionieren. Weitere Informationen finden Sie unserem [arbeiten mit Schaltflächen](~/ios/tvos/user-interface/buttons.md) Dokumentation.
- **Klar Mark destruktive Schaltflächen** – für Schaltflächen, die eine destruktive Aktion (z. B. das Löschen einer Datei) mit eindeutig kennzeichnen die `UIAlertActionStyle.Destructive` Stil.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Eine Warnung

Um eine Warnung angezeigt wird, erstellen Sie eine Instanz von der `UIAlertViewController` und konfigurieren Sie sie durch Hinzufügen von Aktionen (Schaltflächen), und wählen den Stil der Warnung. Der folgende Code zeigt z. B. eine Warnung OK/Abbrechen:

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Werfen wir einen Blick auf diesen Code im Detail. Erstellen Sie zunächst eine neue Warnung mit dem angegebenen Titel und eine Nachricht:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Als Nächstes erstellen wir für jede Schaltfläche, die wir in der Warnung anzeigen möchten eine Aktion definieren den Titel der Schaltfläche mit den, den Stil und die Aktion, die genutzt werden, wenn die Schaltfläche gedrückt wird:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

Die `UIAlertActionStyle` Enum ermöglichen es Ihnen, den Stil der Schaltfläche als eine der folgenden festlegen:

- **Standardmäßige** -werden die Schaltfläche als Standardschaltfläche ausgewählt wird, wenn die Warnung angezeigt wird.
- **Abbrechen** -die Schaltfläche ist die Schaltfläche "Abbrechen", für die Warnung.
- **Destruktive** -Schaltfläche mit den als destruktiver Vorgang, wie z. B. das Löschen einer Datei hervorgehoben. TvOS rendert derzeit die destruktive Schaltfläche mit einem roten Hintergrund.

Die `AddAction` Methode fügt die angegebene Aktion, die die `UIAlertViewController` und schließlich die `PresentViewController (alertController, true, null)` Methode zeigt die angegebene Warnung für den Benutzer.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Hinzufügen von Textfeldern

Zusätzlich zum Hinzufügen von Aktionen (Schaltflächen) für die Warnung, können Sie auf die Warnung, die dem Benutzer ermöglichen, Informationen wie Benutzer-IDs und Kennwörter Textfelder hinzufügen:

[![](alerts-images/alert02.png "Textfeld in einer Warnung")](alerts-images/alert02.png#lightbox)

Wenn der Benutzer das Textfeld auswählt, wird die standard TvOS-Tastatur ermöglicht ihnen einen Wert für das Feld eingeben angezeigt:

[![](alerts-images/alert03.png "Eingeben von text")](alerts-images/alert03.png#lightbox)

Der folgende Code zeigt eine OK/Abbrechen-Warnung mit einem einzelnen Text-Feld für die Eingabe eines Werts zu ändern:

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

Die `AddTextField` Methode fügt ein neues Textfeld mit der Warnung, die Sie dann durch Festlegen von Eigenschaften wie der Platzhalter, Text (der Text, der angezeigt wird, wenn das Feld leer ist), der Standardwert für den Text und den Typ der Tastatur konfigurieren können. Zum Beispiel:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Damit wir auf dem Wert des Textfelds höher fungieren kann, speichern wir auch eine Kopie des mit dem folgenden Code:

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

Nachdem der Benutzer einen Wert in das Textfeld eingegeben hat, können wir die `field` Variable, die auf diesen Wert zugreifen.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Warnungsansicht Controller-Hilfsklasse

Da einfache, allgemeine Typen von Warnungen mithilfe von anzeigen `UIAlertViewController` einiges an duplizierten Code kann dazu führen, können Sie eine Hilfsklasse, die sich wiederholenden Code verringern. Zum Beispiel:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

Diese Klasse verwenden, anzeigen und auf einfache Warnungen reagieren können wie folgt durchgeführt werden:

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Arbeit mit behandelt `UIAlertController` eine Warnmeldung, die dem Benutzer in Xamarin.tvOS angezeigt. Zunächst wird aufgezeigt, wie Sie eine einfache Warnung anzeigen und Hinzufügen von Schaltflächen. Als Nächstes wird aufgezeigt, wie Sie eine Warnung Textfelder hinzufügen. Zum Schluss wird aufgezeigt, wie Sie eine Hilfsklasse, um die Reduzierung des sich wiederholenden Code erforderlich, um eine Warnung angezeigt wird.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
