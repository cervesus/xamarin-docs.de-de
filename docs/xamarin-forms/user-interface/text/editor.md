---
title: Xamarin.Forms-Editor
description: In diesem Artikel wird erläutert, wie Sie das Xamarin.Forms-Editor-Steuerelement verwenden, um mehrzeilige Texteingabe in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 4879ff88d5bbdab5aa92024bee7f50239a141e3b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995863"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms-Editor

_Mehrzeilige Texteingabe_

Die `Editor` Steuerelement wird verwendet, um mit mehrzeiligen Eingaben akzeptieren. Dieser Artikel behandelt:

- **[Anpassung](#customization)**  &ndash; Tastatur und Farboptionen für.
- **[Interaktivität](#interactivity)**  &ndash; Ereignisse, die für die Interaktivität zu überwacht werden können.

## <a name="customization"></a>Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Die `Editor`, wie andere Ansichten darstellen von Text macht den `Text` Eigenschaft. Diese Eigenschaft kann verwendet werden, festlegen und Lesen Sie den Text an, das von der `Editor`. Das folgende Beispiel veranschaulicht das Festlegen der `Text` -Eigenschaft in XAML:

```xaml
<Editor Text="I am an Editor" />
```

In C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Um Text zu lesen, Zugriff auf die `Text` Eigenschaft in c#:

```csharp
var text = MyEditor.Text;
```

### <a name="limiting-input-length"></a>Geben Sie die Länge beschränken

Die [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Eigenschaft kann verwendet werden, um die Eingabe begrenzt, die zulässige Menge für die [ `Editor` ](xref:Xamarin.Forms.Editor). Diese Eigenschaft muss eine positive ganze Zahl festgelegt werden:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

Ein [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) der Eigenschaftswert 0 gibt an, dass keine Eingabe zulässig ist, und den Wert `int.MaxValue`, dies ist der Standardwert für eine [ `Editor` ](xref:Xamarin.Forms.Editor), gibt an, dass keine effektive Höchstwert für die Anzahl der Zeichen, die eingegeben werden können.

### <a name="keyboards"></a>Tastaturen

Die Tastatur, die angezeigt werden, wenn Benutzer mit interagieren ein `Editor` programmgesteuert festgelegt werden können, über die [ ``Keyboard`` ](xref:Xamarin.Forms.Keyboard) Eigenschaft.

Die Optionen für den Tastaturtyp sind:

- **Standardmäßige** &ndash; die Standardtastatur
- **Chat** &ndash; für Hilfsmittel und stellen verwendet, in denen Emoji eignen sich
- **E-Mail-Adresse** &ndash; verwendet werden, wenn Sie e-Mail-Adressen eingeben
- **Numerische** &ndash; verwendet, wenn Zahlen eingeben
- **Telefon** &ndash; verwendet, bei der Eingabe von Telefonnummern
- **URL** &ndash; zum Eingeben von Dateipfaden & Webadressen

Gibt es eine [Beispiel für die einzelnen Tastatur](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) im Abschnitt mit Anleitungen.

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und deaktivieren die Rechtschreibprüfung

Die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft steuert, ob Rechtschreibprüfung ist aktiviert. Standardmäßig ist die Eigenschaft auf festgelegt `true`. Wenn der Benutzer Text eingibt, werden von Rechtschreibfehlern angegeben.

Für einige Szenarien für das Text-Eintrag, z. B. den Benutzernamen eingeben Rechtschreibprüfung bietet jedoch eine negative Erfahrung und daher sollte deaktiviert werden, indem die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaftensatz auf `false`, und eine benutzerdefinierte Tastatur nicht verwendet wird, die native Rechtschreibprüfung wird deaktiviert. Jedoch wenn eine [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) hat wurde Satz, der deaktiviert die Rechtschreibprüfung überprüfen, wie z. B. [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` Eigenschaft wird ignoriert. Aus diesem Grund kann nicht die Eigenschaft verwendet werden, aktivieren Sie die Rechtschreibprüfung für eine `Keyboard` , die explizit deaktiviert.

### <a name="colors"></a>Farben

`Editor` kann festgelegt werden, verwenden Sie eine benutzerdefinierte Hintergrundfarbe über die `BackgroundColor` Eigenschaft. Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform unterschiedliche Standardwerte für die Textfarbe verfügt, müssen Sie eine benutzerdefinierte Hintergrundfarbe für jede Plattform festgelegt. Finden Sie unter [arbeiten mit der Plattform Optimierungsmethoden](~/xamarin-forms/platform/device.md) für Weitere Informationen zum Optimieren der Benutzeroberflächenautomatisierungs für jede Plattform.

In C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "-Editor mit dem BackgroundColor-Beispiel")

Stellen Sie sicher, dass die Hintergrund- und Textfarben können auf jeder Plattform verwendet werden können und keine Platzhaltertext verdecken.

## <a name="interactivity"></a>Interaktivität

`Editor` macht zwei Ereignisse:

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash; wird ausgelöst, wenn der Text im Editor ändert. Stellt den Text an, vor und nach der Änderung.
- [Abgeschlossen](xref:Xamarin.Forms.Editor.Completed) &ndash; wird ausgelöst, wenn der Benutzer die Eingabe beendet wurde, durch Drücken der EINGABETASTE auf der Tastatur.

### <a name="completed"></a>Abgeschlossen

Die `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit reagieren ein `Editor`. `Completed` wird ausgelöst, wenn der Benutzer Eingaben mit einem Feld beendet, indem Sie die return-Taste auf der Tastatur eingeben. Der Handler für das Ereignis wird von einem generischen Ereignishandler an, dass des Absenders und `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Das abgeschlossene Ereignis kann im Code und XAML abonniert werden:

In C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

Die `TextChanged` Ereignis wird verwendet, um auf eine Änderung in den Inhalt eines Felds zu reagieren.

`TextChanged` wird ausgelöst, wenn die `Text` von der `Editor` Änderungen. Der Handler für das Ereignis benötigt eine Instanz der `TextChangedEventArgs`. `TextChangedEventArgs` ermöglicht den Zugriff auf die alten und neuen Werte der `Editor` `Text` über die `OldTextValue` und `NewTextValue` Eigenschaften:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Das abgeschlossene Ereignis kann im Code und XAML abonniert werden:

In Code:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Editor-API](xref:Xamarin.Forms.Editor)
