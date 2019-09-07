---
title: Arbeiten mit tvos-Warnungen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit tvos-Warnungen in xamarin arbeiten. Es wird erläutert, wie eine Warnung angezeigt, Textfelder hinzugefügt und eine Hilfsklasse hinzugefügt wird.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 2578272dcd38399f23f2aac67503ea4e1b09a027
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769074"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Arbeiten mit tvos-Warnungen in xamarin

_In diesem Artikel wird das Arbeiten mit uialertcontroller erläutert, um dem Benutzer eine Warnmeldung in xamarin. tvos anzuzeigen._

Wenn Sie den tvos-Benutzer aufmerksam machen oder die Berechtigung zum Ausführen einer destruktiven Aktion (z. b. zum Löschen einer Datei) anfordern müssen, können Sie eine Warnmeldung `UIAlertViewController`mit dem anzeigen:

[![](alerts-images/alert01.png "Ein Beispiel für uialertviewcontroller")](alerts-images/alert01.png#lightbox)

Wenn Sie eine Meldung hinzufügen, können Sie einer Warnung Schaltflächen und Textfelder hinzufügen, damit der Benutzer auf Aktionen Antworten und Feedback bereitstellen kann.

<a name="About-Alerts" />

## <a name="about-alerts"></a>Informationen zu Warnungen

Wie bereits erwähnt, werden Warnungen verwendet, um die Aufmerksamkeit des Benutzers zu erhalten und Sie über den Status Ihrer APP zu informieren oder Feedback anzufordern. Warnungen müssen einen Titel enthalten, Sie können optional eine Meldung und eine oder mehrere Schaltflächen oder Textfelder enthalten.

[![](alerts-images/alert04.png "Beispiel für eine Warnung")](alerts-images/alert04.png#lightbox)

Apple hat die folgenden Vorschläge zum Arbeiten mit Warnungen:

- **Verwenden Sie Warnungen sparsam** : Warnungen stören den Benutzer Fluss mit der APP und unterbrechen die Benutzer Abläufe und sollten daher nur für wichtige Situationen wie Fehler Benachrichtigungen, in-App-Käufe und zerstörerische Aktionen verwendet werden.
- **Bietet nützliche** Optionen: Wenn die Warnung dem Benutzeroptionen anzeigt, sollten Sie sicherstellen, dass jede Option wichtige Informationen bietet und nützliche Aktionen für den Benutzer bereitstellt.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Warn Titel und-Meldungen

Apple bietet die folgenden Vorschläge, um den Titel einer Warnung und die optionale Nachricht zu präsentieren:

- **Verwendung von Multiword-Titeln** : der Titel einer Warnung sollte den Punkt der Situation eindeutig erkennen, während Sie weiterhin einfach bleibt. Ein einzelner Word-Titel stellt selten genug Informationen bereit.
- **Verwenden Sie beschreibende Titel, für die keine Meldung erforderlich** ist. wenn möglich, sollten Sie den Titel der Warnung so beschreiben, dass der optionale Meldungs Text nicht erforderlich ist.
- **Richten Sie einen kurzen, vollständigen Satz ein,** wenn die optionale Nachricht erforderlich ist, um den Punkt der Warnung zu erhalten, halten Sie Sie so einfach wie möglich, und machen Sie einen vollständigen Satz mit ordnungsgemäßer groß-und Kleinschreibung.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Warn Schaltflächen

Apple hat den folgenden Vorschlag zum Hinzufügen von Schaltflächen zu einer Warnung:

- **Begrenzen auf zwei Schalt** Flächen: Wenn möglich, beschränken Sie die Warnung auf maximal zwei Schaltflächen. Mit einzelnen Schaltflächen Warnungen werden Informationen, aber keine Aktionen bereitgestellt. Zwei Schaltflächen Warnungen bieten eine einfache Ja/Nein-Aktion.
- **Verwenden Sie prägnante, logische Schaltflächen Titel** -einfache ein bis zwei Word-Schaltflächen Titel, die die Funktionsweise der Schaltfläche am besten beschreiben. Weitere Informationen finden Sie in der Dokumentation [Arbeiten mit Schalt](~/ios/tvos/user-interface/buttons.md) Flächen.
- Entfernen Sie **destruktive Schaltflächen eindeutig** : bei Schaltflächen, die eine destruktive Aktion ausführen (z. b `UIAlertActionStyle.Destructive` . das Löschen einer Datei), markieren Sie Sie deutlich

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Anzeigen einer Warnung

Wenn Sie eine Warnung anzeigen möchten, erstellen Sie eine Instanz `UIAlertViewController` von, und konfigurieren Sie Sie, indem Sie Aktionen (Schaltflächen) hinzufügen und den Stil der Warnung auswählen. Im folgenden Code wird beispielsweise eine Warnung vom Typ "OK/Abbrechen" angezeigt:

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

Sehen wir uns diesen Code ausführlich an. Zuerst erstellen wir eine neue Warnung mit dem angegebenen Titel und der Nachricht:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Als Nächstes erstellen wir für jede Schaltfläche, die in der Warnung angezeigt werden soll, eine Aktion, die den Titel der Schaltfläche definiert, den Stil und die Aktion, die Sie durchführen möchten, wenn die Schaltfläche gedrückt wird:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ =>
    // Do something when the button is pressed
    ...
);
```

Mit `UIAlertActionStyle` der-Enumeration können Sie den Stil der Schaltfläche auf einen der folgenden Werte festlegen:

- **Standard** -die Schaltfläche ist die Standard Schaltfläche, die ausgewählt wird, wenn die Warnung angezeigt wird.
- **Abbrechen** : die Schaltfläche ist die Schaltfläche Abbrechen für die Warnung.
- **Destruktiv** : hebt die Schaltfläche als zerstörerische Aktion hervor, z. b. beim Löschen einer Datei. Derzeit rendert tvos die destruktive Schaltfläche mit einem roten Hintergrund.

Die `AddAction` `UIAlertViewController` -Methode fügt die angegebene Aktion dem hinzu, und `PresentViewController (alertController, true, null)` die-Methode zeigt dem Benutzer schließlich die angegebene Warnung an.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Hinzufügen von Text Feldern

Zusätzlich zum Hinzufügen von Aktionen (Schaltflächen) zur Warnung können Sie der Warnung Text Felder hinzufügen, um dem Benutzer das Ausfüllen von Informationen wie Benutzer-IDs und Kenn Wörtern zu ermöglichen:

[![](alerts-images/alert02.png "Textfeld in einer Warnung")](alerts-images/alert02.png#lightbox)

Wenn der Benutzer das Textfeld auswählt, wird die Standardtastatur von tvos angezeigt, sodass Sie einen Wert für das Feld eingeben können:

[![](alerts-images/alert03.png "Eingeben von Text")](alerts-images/alert03.png#lightbox)

Der folgende Code zeigt eine OK/Cancel-Warnung mit einem einzelnen Textfeld für die Eingabe eines Werts an:

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

Die `AddTextField` -Methode fügt der Warnung ein neues Textfeld hinzu, das Sie dann konfigurieren können, indem Sie Eigenschaften festlegen, wie z. b. den Platzhalter Text (der Text, der angezeigt wird, wenn das Feld leer ist), den Standard Textwert und den Typ der Tastatur. Beispiel:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Damit wir später auf den Wert des Textfelds reagieren können, speichern wir auch eine Kopie von mit dem folgenden Code:

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

Nachdem der Benutzer einen Wert in das Textfeld eingegeben hat, können wir die `field` Variable verwenden, um auf diesen Wert zuzugreifen.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Warnungs Ansicht Controller-Hilfsklasse

Da die Anzeige einfacher, allgemeiner Typen von Warnungen `UIAlertViewController` mithilfe von etwas doppelten Code zur Folge haben kann, können Sie eine Hilfsklasse verwenden, um die Menge an wiederholtem Code zu verringern. Beispiel:

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

Die Verwendung dieser Klasse, die Anzeige von und die Reaktion auf einfache Warnungen können wie folgt erfolgen:

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

In diesem Artikel wurde beschrieben `UIAlertController` , wie eine Warnmeldung für den Benutzer in xamarin. tvos angezeigt wird. Zuerst wurde gezeigt, wie Sie eine einfache Warnung anzeigen und Schaltflächen hinzufügen. Im nächsten Schritt wurde gezeigt, wie Text Felder zu einer Warnung hinzugefügt werden. Schließlich haben Sie erfahren, wie Sie eine Hilfsklasse verwenden, um die Menge an wiederholtem Code zu reduzieren, der zum Anzeigen einer Warnung erforderlich ist.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
