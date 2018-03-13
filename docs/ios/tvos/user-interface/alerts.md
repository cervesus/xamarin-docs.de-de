---
title: Arbeiten mit Warnungen
description: Dieser Artikel behandelt die Arbeit mit UIAlertController eine Warnmeldung, die der Benutzer im Xamarin.tvOS angezeigt.
ms.topic: article
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 6dabba30c5242d6e7e9ef42a4025f87826a5b89e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-alerts"></a>Arbeiten mit Warnungen

_Dieser Artikel behandelt die Arbeit mit UIAlertController eine Warnmeldung, die der Benutzer im Xamarin.tvOS angezeigt._


Wenn Sie müssen ein Eingreifen des Benutzers tvos. außerdem wurden erhalten oder Fragen Sie die Berechtigung zum Ausführen einer destruktiven Aktion (z. B. das Löschen einer Datei), können Sie eine Warnmeldung mit stellen die `UIAlertViewController`:

[![](alerts-images/alert01.png "Ein Beispiel für UIAlertViewController")](alerts-images/alert01.png#lightbox)

Wenn zusätzlich zum Anzeigen einer Meldung, Sie hinzufügen können, Schaltflächen und Textfelder auf eine Warnung, dass der Benutzer auf Aktionen reagiert und Feedback.

<a name="About-Alerts" />

## <a name="about-alerts"></a>Informationen zu Warnungen

Wie bereits erwähnt, werden Warnungen verwendet, die Aufmerksamkeit des Benutzers abrufen und informieren Sie ihn über den Status Ihres Feedbacks app oder Anforderung. Warnungen müssen einen Titel vorhanden ist, können sie optional eine Nachricht und eine oder mehrere Schaltflächen und Textfeldern aufweisen.

[![](alerts-images/alert04.png "Eine Beispiel-Warnung")](alerts-images/alert04.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit Warnungen an:

- **Verwenden Sie Warnungen nur selten** -Warnungen des Benutzers Datenfluss mit der app unterbrochen und erforderlichen Erfahrungsgrad des Benutzers zu unterbrechen und sollte daher nur für wichtige Situationen wie z. B. fehlerbenachrichtigungen, die In App-Einkäufe und destruktiven Aktionen verwendet werden. 
- **Bietet nützliche Optionen** – Wenn die Warnung Optionen für den Benutzer, enthält Ihre sollten sicherstellen, dass jede Option wichtigen Informationen bietet und nützliche Aktionen für den Benutzer bietet werden.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Warnungstitel und Nachrichten

Apple hat die folgenden Vorschläge zum Darstellen der Titel der Warnung und die optionale Nachricht:

- **Verwenden von Titeln Multiword** -eine Warnung der Titel sollte zum Zeitpunkt des Situation über eindeutig bleibt weiterhin einfache abrufen. Ein einzelnes Wort-Titel aussagekräftig sind nur selten.
- **Beschreibende Titel verwenden, die eine Nachricht nicht erfordern** - möglichst, erwägen Sie die Warnung Titel beschreibenden ausreichend, die optional die Meldung nicht wird erforderlich. 
- **Stellen Sie der Nachricht einen kurzen, einen vollständigen Satz** – Wenn die optionale Nachricht erforderlich ist, um den Punkt der Benachrichtigung über, es so einfach wie möglich zu halten und stellen sie einen vollständigen Satz mit der richtigen Groß-/Kleinschreibung und Interpunktion.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Warnung-Schaltflächen

Apple hat die folgenden Vorschläge zum Hinzufügen von Schaltflächen auf eine Warnung:

- **Beschränkung für zwei Schaltflächen** – nach Möglichkeit die Warnung auf ein Maximum von zwei Schaltflächen zu beschränken. Einzelne Schaltfläche Warnungen – bieten Informationen aber keine Maßnahmen. Zwei Schaltfläche Warnungen – bieten eine einfache Auswahl Ja/Nein-Aktion.
- **Succinct, logische Schaltfläche Titel verwenden** -einfache am besten, ein bis zwei Word Schaltfläche Softwaretitel an, die die Schaltfläche Aktion beschreiben funktionieren. Weitere Informationen finden Sie in unserem [arbeiten mit Schaltflächen](~/ios/tvos/user-interface/buttons.md) Dokumentation.
- **Deutlich destruktiven Schaltflächen** – für Schaltflächen, die einen destruktiven Vorgang (z. B. das Löschen einer Datei) mit eindeutig kennzeichnen die `UIAlertActionStyle.Destructive` Stil.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Eine Warnung

Wenn eine Warnung anzeigen möchten, erstellen Sie eine Instanz von der `UIAlertViewController` und konfigurieren Sie ihn durch Hinzufügen von Aktionen (Schaltflächen), und wählen das Format der Warnung. Der folgende Code zeigt z. B. eine OK/Cancel-Warnung:

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

Werfen wir einen Blick auf diesen Code im Detail. Erstellen Sie zunächst eine neue Warnung mit dem angegebenen Titel und der Meldung:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Als Nächstes erstellen Sie für jede Schaltfläche, die wir in der Warnung angezeigt werden soll eine Aktion definiert den Titel der Schaltfläche mit den, den Stil und die Aktion, die wir an, wenn die Schaltfläche aufgerufen werden soll:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

Die `UIAlertActionStyle` Enum ermöglichen es Ihnen, den Stil der Schaltfläche als eines der folgenden festgelegt:

- **Standardmäßige** -werden die Schaltfläche die Standardschaltfläche, die ausgewählt werden, wenn die Warnung angezeigt wird.
- **"Abbrechen"** -die Schaltfläche ist die Schaltfläche "Abbrechen", für die Warnung.
- **Destruktiven** -hebt die Schaltfläche als destruktiver Vorgang, z. B. Löschen einer Datei. Derzeit rendert tvos. außerdem wurden die destruktive Schaltfläche mit einem roten Hintergrund.

Die `AddAction` Methode fügt die angegebene Aktion aus, um die `UIAlertViewController` und schließlich die `PresentViewController (alertController, true, null)` Methode zeigt die angegebene Warnung für den Benutzer.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Hinzufügen von Textfeldern

Zusätzlich zum Hinzufügen von Aktionen (Schaltflächen) auf die Warnung aus, können Sie die Warnung, die der Benutzer Informationen wie Benutzer-IDs und Kennwörter Auffüllen kann Textfelder hinzufügen:

[![](alerts-images/alert02.png "Textfeld in einer Warnung")](alerts-images/alert02.png#lightbox)

Wenn der Benutzer das Textfeld "auswählt, wird die standardmäßige tvos. außerdem wurden Tastatur ihnen ermöglicht, einen Wert für das Feld eingeben angezeigt:

[![](alerts-images/alert03.png "Eingeben von text")](alerts-images/alert03.png#lightbox)

Der folgende Code zeigt eine OK/Cancel-Warnung mit einem einzelnen Textfeld für die Eingabe eines Werts zu ändern:

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

Die `AddTextField` Methode fügt ein neues Textfeld auf die Warnung, die Sie dann durch Festlegen von Eigenschaften wie der Platzhalter Text (der Text, der angezeigt wird, wenn das Feld leer ist), der Standardwert für den Text und den Typ der Tastatur konfigurieren können. Zum Beispiel:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Damit wir den Wert des Textfelds später bearbeiten können, speichern wir auch eine Kopie des mit dem folgenden Code:

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

## <a name="alert-view-controller-helper-class"></a>Warnungsansicht Controller Hilfsklasse

Da einfache, allgemeine Arten von Benachrichtigungen unter Verwendung von anzeigen `UIAlertViewController` kann sehr viel doppelten Code zu, können Sie eine Hilfsklasse, die sich wiederholenden Code verringern. Zum Beispiel:

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

Mithilfe dieser Klasse, Anzeigen von und reagieren auf einfache Warnungen können wie folgt erfolgen:

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

In diesem Artikel wurden arbeiten mit behandelt `UIAlertController` eine Warnmeldung, die der Benutzer im Xamarin.tvOS angezeigt. Zuerst, es wurde gezeigt, wie eine einfache Warnung angezeigt, und fügen Sie Schaltflächen hinzu. Als Nächstes wurde es gezeigt, wie Textfelder auf eine Warnung hinzugefügt. Schließlich wurde es gezeigt, wie eine Hilfsklasse verwenden, um die sich wiederholenden Code erforderlich, um eine Warnung anzuzeigen, zu reduzieren.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
