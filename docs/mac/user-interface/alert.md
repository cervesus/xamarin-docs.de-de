---
title: Warnungen in Xamarin.Mac
description: Dieser Artikel behandelt die Arbeit mit Warnungen in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen und Anzeigen von Warnungen von C# Code und die Reaktion auf Benutzerinteraktionen.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6545b1423b809e42293302baf3eba9521848edc1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61237659"
---
# <a name="alerts-in-xamarinmac"></a>Warnungen in Xamarin.Mac

_Dieser Artikel behandelt die Arbeit mit Warnungen in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen und Anzeigen von Warnungen von C# Code und die Reaktion auf Benutzerinteraktionen._

Bei der Arbeit mit C# und .NET in einer Xamarin.Mac-Anwendung, Sie haben Zugriff auf die gleiche benachrichtigt werden, dass ein Entwickler *Objective-C-* und *Xcode* ist. 

Eine Warnung ist eine besondere Art von Dialogfeld, das angezeigt wird, tritt ein schwerwiegendes Problem (z. B. ein Fehler) oder als eine Warnung (z. B. die Vorbereitung zum Löschen einer Datei). Da eine Warnung ein Dialog ist, benötigen sie auch eine, bevor sie geschlossen werden kann.

[![](alert-images/alert06.png "Ein Beispiel für Warnung")](alert-images/alert06.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Warnungen in einer Xamarin.Mac-Anwendung beschrieben. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Einführung in die Warnungen

Eine Warnung ist eine besondere Art von Dialogfeld, das angezeigt wird, tritt ein schwerwiegendes Problem (z. B. ein Fehler) oder als eine Warnung (z. B. die Vorbereitung zum Löschen einer Datei). Da Warnungen den Benutzer stören, da sie geschlossen werden müssen, bevor der Benutzer auf ihre Aufgabe fortsetzen kann, vermeiden Sie eine Warnung angezeigt, es sei denn, es unbedingt notwendig ist.

Apple empfehlen die folgenden Richtlinien:

- Verwenden Sie eine Warnung nicht nur, um Benutzern Informationen zu ermöglichen.
- Eine Warnung für allgemeine, rückgängig gemacht werden Aktionen werden nicht angezeigt werden. Auch wenn diese Situation kann Datenverlusten führen.
- Ist eine Situation eine Warnung ausgeben, verwenden Sie eine beliebige andere UI-Element oder die Methode angezeigt.
- Das Symbol "Vorsicht" sollte sparsam verwendet werden.
- Beschreiben Sie die Warnung Situation kurz und eindeutig in der Warnmeldung.
- Der Name der Schaltfläche "Standard" sollte die Aktion entsprechen, die Sie in der Warnung Nachricht beschreiben.

Weitere Informationen finden Sie unter den [Warnungen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Aufbau einer Warnung

Wie bereits erwähnt, soll Warnungen angezeigt werden, die der Benutzer der Anwendung tritt ein schwerwiegendes Problem oder als eine Warnung zu Datenverlusten (z. B. das Schließen einer ungespeicherten Datei). Eine Warnung wird erstellt in Xamarin.Mac, im C# code, z.B.:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Der obige Code zeigt eine Warnung mit dem Symbol für Anwendungen angezeigt, auf das Symbol "Warnung", einen Titel, eine Warnmeldung und eine einzelne **OK** Schaltfläche:

[![](alert-images/alert01.png "Eine Warnung mit einer Schaltfläche \"OK\"")](alert-images/alert01.png#lightbox)

Apple bietet mehrere Eigenschaften, die verwendet werden können, um eine Warnung anzupassen:

- **AlertStyle** definiert den Typ einer Warnung als einen der folgenden:
    - **Warnung** – verwendet, um den Benutzer ein Ereignis zum aktuellen oder der bevorstehenden zu warnen, die nicht entscheidend ist. Dies ist das Standardformat.
    - **Nur zu Informationszwecken** : wird verwendet, um den Benutzer zu einem Ereignis aktuellen oder der bevorstehenden zu warnen. Derzeit besteht kein Unterschied zwischen einer **Warnung** und **Information**
    - **Kritische** : wird verwendet, um den Benutzer über schwerwiegende Folgen ein bevorstehendes Ereignis (z. B. das Löschen einer Datei) zu warnen. Diese Art der Warnung sollten sparsam verwendet werden.
- **MessageText** – Dies ist die Haupt-Nachricht oder den Titel der Warnung und die Situation sollte schnell an den Benutzer definieren.
- **InformativeText** – Dies ist der Text der Warnung, in dem Sie das Problem klar zu definieren und praktikable Optionen für den Benutzer vorhanden.
- **Symbol "** -können Sie ein benutzerdefiniertes Symbol für den Benutzer angezeigt werden.
- **HelpAnchor** & **ShowsHelp** -Warnung, die bei der Anwendung HelpBook gebunden werden und die Hilfe für die Warnung anzeigen können.
- **Schaltflächen** -standardmäßig hat eine Warnung nur die **OK** Schaltfläche jedoch **Schaltflächen** Sammlung können Sie weitere Optionen nach Bedarf hinzufügen.
- **ShowsSuppressionButton** – Wenn `true` zeigt ein Kontrollkästchen, die der Benutzer verwenden können, um die Warnung nach weiteren Vorkommen des Ereignisses zu unterdrücken, die sie ausgelöst hat.
- **AccessoryView** -ermöglicht es Ihnen, fügen Sie einen anderen Unteransicht Warnung Geben Sie zusätzliche Informationen, z. B. das Hinzufügen einer **Textfeld** für die Dateneingabe. Wenn Sie ein neues festlegen **AccessoryView** oder eine vorhandene zu ändern, müssen Sie zum Aufrufen der `Layout()` Methode zum Anpassen des Layouts sichtbar, der die Warnung.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Anzeigen einer Warnung

Es gibt zwei Möglichkeiten, eine Warnung angezeigt, der nicht typisierte werden kann oder als eine Tabelle. Der folgende Code zeigt eine Warnung als ist:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
Wenn dieser Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert02.png "Eine einfache Warnung")](alert-images/alert02.png#lightbox)

Der folgende Code zeigt die Warnung aus, als einer Tabelle:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

Wenn dieser Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert03.png "Eine Warnung angezeigt, wie eine Tabelle")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Arbeiten mit Warnungen Schaltflächen

Standardmäßig zeigt eine Warnung nur die **OK** Schaltfläche. Aber Sie sind nicht beschränkt auf, dass Sie zusätzliche Schaltflächen erstellen können, indem Sie sie Anfügen der **Schaltflächen** Auflistung. Der folgende Code erstellt eine nicht typisierte Warnung mit einem **OK**, **Abbrechen** und **vielleicht** Schaltfläche:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Die erste Schaltfläche, die hinzugefügt werden. die _Schaltfläche Standard_ , wird aktiviert, wenn der Benutzer die EINGABETASTE drückt. Der zurückgegebene Wert wird eine ganze Zahl, welche Schaltfläche der Benutzer, die gedrückt werden. In diesem Fall werden die folgenden Werte zurückgegeben werden:

- **OK** - 1000.
- **Abbrechen** : 1001.
- **Vielleicht** : 1002.

Wenn wir den Code ausführen, wird Folgendes angezeigt:

[![](alert-images/alert04.png "Eine Warnung mit drei Schaltflächenoptionen")](alert-images/alert04.png#lightbox)

Hier ist der Code für die Warnung als eine Tabelle ein:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
Wenn dieser Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert05.png "Eine Schaltfläche \"drei\" Warnung als ein Blatt angezeigt")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> Sie sollten nie mehr als drei Schaltflächen auf eine Warnung hinzufügen.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Mit der Schaltfläche "unterdrückt".

Wenn der Warnung `ShowSuppressButton` Eigenschaft `true`, die Benachrichtigung zeigt ein Kontrollkästchen, das der Benutzer verwenden kann, um die Warnung nach weiteren Vorkommen des Ereignisses zu unterdrücken, die sie ausgelöst hat. Der folgende Code zeigt eine nicht typisierte Warnung mit einer Schaltfläche unterdrücken:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Wenn der Wert des der `alert.SuppressionButton.State` ist `NSCellStateValue.On`, der Benutzer hat das unterdrücken Kontrollkästchen aktiviert, sie haben ansonsten nicht.

Wenn der Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert06.png "Eine Warnung mit einer Schaltfläche unterdrücken")](alert-images/alert06.png#lightbox)

Hier ist der Code für die Warnung als eine Tabelle ein:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Wenn dieser Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert07.png "Eine Warnung mit einer unterdrücken-Schaltfläche angezeigt wird, als einer Tabelle")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Hinzufügen einer benutzerdefinierten Unteransicht

Warnungen haben eine `AccessoryView` -Eigenschaft, die verwendet werden kann, um die Warnung weiter anpassen, und fügen die Dinge wie einen **Textfeld** für Benutzereingaben. Der folgende Code erstellt eine Warnung ist mit einem Eingabefeld Text hinzugefügt:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Sind die Schlüsselzeilen `var input = new NSTextField (new CGRect (0, 0, 300, 20));` erstellt ein neues **Textfeld** , dass wir die Warnung hinzufügen. `alert.AccessoryView = input;` Welche fügt die **Textfeld** auf die Warnung und der Aufruf von der `Layout()` -Methode, die erforderlich ist, um die Warnung, die in die neue Unteransicht passt die Größe ändern.

Wenn wir den Code ausführen, wird Folgendes angezeigt:

[![](alert-images/alert08.png "Wenn wir den Code ausführen, wird Folgendes angezeigt werden")](alert-images/alert08.png#lightbox)

Hier ist die gleiche Warnung als eine Tabelle aus:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Wenn wir diesen Code ausführen, wird Folgendes angezeigt:

[![](alert-images/alert09.png "Eine Warnung mit einer benutzerdefinierten Ansicht")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit Warnungen in einer Xamarin.Mac-Anwendung geführt. Erläutert die verschiedenen Typen und die Verwendung von Warnungen, die das Erstellen und Anpassen von Warnungen und zum Arbeiten mit Warnungen in C# Code.

## <a name="related-links"></a>Verwandte Links

- [MacWindows (Beispiel)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
