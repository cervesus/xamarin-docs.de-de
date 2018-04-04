---
title: Benachrichtigungen
description: In diesem Artikel wird das Arbeiten mit Warnungen in einer Anwendung Xamarin.Mac behandelt. Erstellen und Anzeigen von Warnungen von C#-Code und reagieren auf Benutzerinteraktionen beschrieben.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a451d0a5535915d9e52f687ae07ea028c0ccd5ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="alerts"></a>Benachrichtigungen

_In diesem Artikel wird das Arbeiten mit Warnungen in einer Anwendung Xamarin.Mac behandelt. Erstellen und Anzeigen von Warnungen von C#-Code und reagieren auf Benutzerinteraktionen beschrieben._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf das gleiche benachrichtigt werden, dass ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. 

Eine Warnung ist eine Sonderform des Dialogfelds ", der angezeigt wird, tritt ein schwerwiegendes Problem (z. B. Fehler) oder als Warnung (z. B. Vorbereitung zum Löschen einer Datei). Ein Dialogfeld wird eine Warnung Quellformat, sodass es auch eine Benutzerantwort, bevor sie geschlossen werden kann.

[![](alert-images/alert06.png "Eine Beispiel-Warnung")](alert-images/alert06.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Warnungen in einer Anwendung Xamarin.Mac eingegangen. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Einführung in die Warnungen

Eine Warnung ist eine Sonderform des Dialogfelds ", der angezeigt wird, tritt ein schwerwiegendes Problem (z. B. Fehler) oder als Warnung (z. B. Vorbereitung zum Löschen einer Datei). Da Warnungen den Benutzer unterbrechen, da sie verworfen werden müssen, bevor der Benutzer auf ihre Aufgabe fortsetzen kann, vermeiden Sie eine Warnung angezeigt, es sei denn, es unbedingt notwendig ist.

Apple empfehlen die folgenden Richtlinien:

- Verwenden Sie keine Warnung lediglich darin, Benutzern Informationen ermöglichen.
- Eine Warnung für die allgemeinen, Rückgängig-Aktionen werden nicht angezeigt werden. Auch wenn diese Situation kann zu Datenverlusten führen.
- Ist eine Situation hinaus eine Warnung, zu vermeiden, verwenden beliebige andere Benutzeroberflächenelement oder Methoden angezeigt.
- Symbol "Vorsicht" sollte nur selten verwendet werden.
- Beschreiben Sie die Warnung Situation eindeutig und kurz in der Warnmeldung.
- Der Name der Schaltfläche Standard sollte die Aktion entsprechen, die Sie in der Warnmeldung beschreiben.

Weitere Informationen finden Sie unter der [Warnungen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Aufbau einer Warnung

Wie bereits erwähnt, sollte Warnungen angezeigt werden, Ihre Anwendung Benutzer tritt ein schwerwiegendes Problem oder als Warnung zu Datenverlusten (z. B. das Schließen einer ungespeicherten Datei). In Xamarin.Mac wird eine Warnung beispielsweise in C#-Code erstellt:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Der obige Code zeigt eine Warnung mit dem Anwendungen-Symbol, das auf das Symbol "Warnung", einen Titel, eine Warnmeldung und eine einzelne überlagert **OK** Schaltfläche:

[![](alert-images/alert01.png "Eine Warnung mit einer Schaltfläche "OK"")](alert-images/alert01.png#lightbox)

Apple bietet verschiedene Eigenschaften, die verwendet werden können, um eine Warnung anzupassen:

- **AlertStyle** definiert den Typ einer Warnung als eines der folgenden:
    - **Warnung** – wird verwendet, um den Benutzer ein aktuelle oder bevorstehendes Ereignis zu warnen, die nicht entscheidend ist. Dies ist das Standardformat.
    - **Nur zu Informationszwecken** – wird verwendet, warnt den Benutzer über ein aktuelle oder bevorstehendes Ereignis. Derzeit besteht kein Unterschied zwischen einem **Warnung** und ein **Information**
    - **Kritische** – wird verwendet, warnt den Benutzer bezüglich schwerwiegende Folgen für ein bevorstehendes Ereignis (z. B. das Löschen einer Datei). Diese Art der Warnung sollte nur selten verwendet werden.
- **MessageText** -Dies ist die Hauptnachricht oder der Titel der Warnung und die Situation sollte schnell für den Benutzer definieren.
- **InformativeText** -Dies ist der Text der Warnung, in dem Sie sollten die Situation eindeutig zu definieren und praktikable Optionen für den Benutzer vorhanden.
- **Symbol "** -können Sie ein benutzerdefiniertes Symbol für den Benutzer angezeigt werden.
- **HelpAnchor** & **ShowsHelp** -können Sie die Warnung, die in der Anwendung HelpBook gebunden werden, und zeigt die Hilfe für die Warnung.
- **Schaltflächen** -standardmäßig hat eine Warnung nur die **OK** Schaltfläche aber die **Schaltflächen** Auflistung können Sie weitere Optionen nach Bedarf hinzufügen.
- **ShowsSuppressionButton** – Wenn `true` zeigt ein Kontrollkästchen an, die der Benutzer verwenden können, um die Warnung nach weiteren Vorkommen des Ereignisses zu unterdrücken, das es ausgelöst.
- **AccessoryView** -können Sie die Warnung, die zusätzliche Informationen, z. B. das Hinzufügen einer anderen Unteransicht Zuordnen einer **Textfeld** für die Dateneingabe. Wenn Sie ein neues festlegen **AccessoryView** oder eine vorhandene zu ändern, müssen Sie zum Aufrufen der `Layout()` Methode, um die sichtbaren Layout der Warnung anzupassen.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Eine Warnung

Es gibt zwei Möglichkeiten, dass eine Warnung angezeigt, der nicht typisierte werden kann oder als Blatt. Der folgende Code zeigt eine Warnung als nicht typisierte:

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

Der folgende Code zeigt die Warnung als Blatt:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

Wenn dieser Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert03.png "Eine Warnung angezeigt, die als Blatt")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Arbeiten mit Warnungen Schaltflächen

Standardmäßig zeigt eine Warnung nur die **OK** Schaltfläche. Sie werden jedoch nicht begrenzt auf, dass Sie zusätzliche Schaltflächen erstellen können, durch Anfügen, damit die **Schaltflächen** Auflistung. Der folgende Code erstellt eine nicht typisierte Warnung mit einem **OK**, **"Abbrechen"** und **vielleicht** Schaltfläche:

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

Die erste Schaltfläche hinzugefügt werden die _Schaltfläche Standard_ , werden aktiviert, wenn der Benutzer die EINGABETASTE drückt. Der zurückgegebene Wert wird eine ganze Zahl darstellt, welche Schaltfläche der Benutzer, die gedrückt werden. In unserem Fall werden die folgenden Werte zurückgegeben:

- **OK** - 1000.
- **"Abbrechen"** - 1001.
- **Vielleicht** - 1002.

Wenn wir den Code ausführen, wird Folgendes angezeigt:

[![](alert-images/alert04.png "Eine Warnung mit drei Optionen")](alert-images/alert04.png#lightbox)

Hier ist der Code für die gleiche Warnung als Blatt:

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

[![](alert-images/alert05.png "Eine als Blatt angezeigt drei Schaltfläche-Warnung")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> Sie sollten nie mehr als drei Schaltflächen auf eine Warnung hinzufügen.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Zeigt die Schaltfläche mit den unterdrücken

Wenn der Warnung `ShowSuppressButton` Eigenschaft ist `true`, die Warnung zeigt ein Kontrollkästchen an, die der Benutzer verwenden können, um die Warnung nach weiteren Vorkommen des Ereignisses zu unterdrücken, das es ausgelöst. Der folgende Code zeigt eine nicht typisierte Warnung mit einer Schaltfläche unterdrücken:

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

Wenn der Wert der `alert.SuppressionButton.State` ist `NSCellStateValue.On`, der Benutzer hat das unterdrücken Kontrollkästchen aktiviert, sie haben ansonsten nicht.

Wenn der Code ausgeführt wird, wird Folgendes angezeigt:

[![](alert-images/alert06.png "Eine Warnung mit einer Schaltfläche unterdrücken")](alert-images/alert06.png#lightbox)

Hier ist der Code für die gleiche Warnung als Blatt:

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

[![](alert-images/alert07.png "Eine Warnung mit einer Schaltfläche unterdrücken als Blatt anzeigen")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Hinzufügen einer benutzerdefinierten Unteransicht

Warnungen sind eine `AccessoryView` -Eigenschaft, die verwendet werden kann, um die Warnung weiter anpassen und hinzufügen, Elemente wie ein **Textfeld** für Benutzereingaben. Der folgende Code erstellt eine nicht typisierte Warnung mit einem Eingabefeld Text hinzugefügt:

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

Hier die wichtigsten Zeilen sind `var input = new NSTextField (new CGRect (0, 0, 300, 20));` dadurch erstellt ein neues **Textfeld** , dass wir die Warnung hinzufügen. `alert.AccessoryView = input;` Welche fügt die **Textfeld** auf die Warnung und der Aufruf an die `Layout()` -Methode, die erforderlich ist, um die Größe der Warnung, die in die neue Unteransicht passen.

Wenn wir den Code ausführen, wird Folgendes angezeigt:

[![](alert-images/alert08.png "Wenn wir den Code auszuführen, wird Folgendes angezeigt werden")](alert-images/alert08.png#lightbox)

So sieht die Warnung als Blatt aus:

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

Wenn wir diesen Code auszuführen, wird Folgendes angezeigt:

[![](alert-images/alert09.png "Eine Warnung mit einer benutzerdefinierten Ansicht")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Warnungen in einer Anwendung Xamarin.Mac übernommen. Wir haben gesehen, die verschiedene Typen und Verwendungen von Warnungen, zum Erstellen und Anpassen von Warnungen und zum Arbeiten mit Warnungen in C#-Code.

## <a name="related-links"></a>Verwandte Links

- [MacWindows (Beispiel)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
