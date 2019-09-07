---
title: Xamarin.Forms-Editor
description: In diesem Artikel wird erläutert, wie Sie das Xamarin.Forms-Editor-Steuerelement verwenden, um mehrzeilige Texteingabe in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2018
ms.openlocfilehash: 684bdb21309028e02362dcc6bfd8c91c539430bd
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770186"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms-Editor

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Mehrzeilige Texteingabe_

Die [ `Editor` ](xref:Xamarin.Forms.Editor) Steuerelement wird verwendet, um mit mehrzeiligen Eingaben akzeptieren. Dieser Artikel behandelt Folgendes:

- **[Anpassung](#customization)**  &ndash; Tastatur und Farboptionen für.
- **[Interaktivität](#interactivity)**  &ndash; Ereignisse, die für die Interaktivität zu überwacht werden können.

## <a name="customization"></a>Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Die [ `Editor` ](xref:Xamarin.Forms.Editor), wie andere Ansichten darstellen von Text macht den `Text` Eigenschaft. Diese Eigenschaft kann verwendet werden, festlegen und Lesen Sie den Text an, das von der `Editor`. Das folgende Beispiel veranschaulicht das Festlegen der `Text` -Eigenschaft in XAML:

```xaml
<Editor Text="I am an Editor" />
```

In C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Um Text zu lesen, Zugriff auf die `Text` Eigenschaft in C#:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>Festlegen von Platzhaltertext

Die [ `Editor` ](xref:Xamarin.Forms.Editor) Platzhaltertext angezeigt, wenn es keine Benutzereingaben gespeichert sind, die festgelegt werden können. Dies geschieht durch Festlegen der [ `Placeholder` ](xref:Xamarin.Forms.Editor.Placeholder) Eigenschaft, um eine `string`, und wird häufig verwendet, um den Typ des Inhalts anzugeben, dass für die `Editor`. Darüber hinaus kann die Farbe des Textes Platzhalter gesteuert werden, durch Festlegen der [ `PlaceholderColor` ](xref:Xamarin.Forms.Editor.PlaceholderColor) Eigenschaft, um eine [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>Verhindern des Text Eintrags

Benutzer können daran gehindert werden, den Text in einer [`Editor`](xref:Xamarin.Forms.Editor) zu ändern, `IsReadOnly` indem Sie die-Eigenschaft mit dem Standard `false`Wert auf `true`festlegen:

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> Die `IsReadonly` -Eigenschaft ändert nicht die visuelle Darstellung [`Editor`](xref:Xamarin.Forms.Editor)eines, anders als die `IsEnabled` -Eigenschaft `Editor` , die auch die visuelle Darstellung von in grau ändert.

### <a name="limiting-input-length"></a>Geben Sie die Länge beschränken

Die [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Eigenschaft kann verwendet werden, um die Eingabe begrenzt, die zulässige Menge für die [ `Editor` ](xref:Xamarin.Forms.Editor). Diese Eigenschaft muss eine positive ganze Zahl festgelegt werden:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

Ein [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) der Eigenschaftswert 0 gibt an, dass keine Eingabe zulässig ist, und den Wert `int.MaxValue`, dies ist der Standardwert für eine [ `Editor` ](xref:Xamarin.Forms.Editor), gibt an, dass keine effektive Höchstwert für die Anzahl der Zeichen, die eingegeben werden können.

### <a name="auto-sizing-an-editor"></a>Automatische Größenanpassung eines Editors

Ein [ `Editor` ](xref:Xamarin.Forms.Editor) können für automatische Anpassung an seinen Inhalt erstellt werden, durch Festlegen der [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize) Eigenschaft [ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), ist ein Wert, der die [ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption) Enumeration. Diese Enumeration verfügt über zwei Werte:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) Gibt an, dass die automatische Größenänderung ist deaktiviert, und der Standardwert ist.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) Gibt an, dass automatische Größenänderung aktiviert ist.

Dies kann im Code wie folgt erreicht werden:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Wenn die automatische Größenänderung aktiviert ist, die die Höhe des der [ `Editor` ](xref:Xamarin.Forms.Editor) erhöht, wenn der Benutzer mit Text gefüllt, und Verringern der Höhe wie der Benutzer Text löscht.

> [!NOTE]
> Ein [ `Editor` ](xref:Xamarin.Forms.Editor) wird keine automatische Größenänderung If der [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) -Eigenschaft festgelegt wurde.

### <a name="customizing-the-keyboard"></a>Anpassen der Tastaturfokus

Die Tastatur, die angezeigt werden, wenn Benutzer mit interagieren ein [ `Editor` ](xref:Xamarin.Forms.Editor) programmgesteuert festgelegt werden können, über die [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) Eigenschaft verwenden, um eine der folgenden Eigenschaften aus der [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Klasse:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – zum Hilfsmittel und Orten, in denen Emoji eignen.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – die Standardtastatur.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – verwendet, wenn Sie e-Mail-Adressen eingeben.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – verwendet, wenn Zahlen eingeben.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – verwendet, bei der Eingabe von Text, ohne [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags) angegebenen.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – verwendet, bei der Eingabe von Telefonnummern.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – verwendet, wenn Text eingibt.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – zum Eingeben von Dateipfaden & Webadressen verwendet.

Dies kann in XAML wie folgt erreicht werden:

```xaml
<Editor Keyboard="Chat" />
```

Der entsprechende C#-Code ist:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Beispiele für die einzelnen Tastatur finden Sie in unserem [Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) Repository.

Die [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) -Klasse verfügt auch über eine [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) Factorymethode, die auf eine Tastatur anpassen, indem Groß-/Kleinschreibung, Rechtschreibprüfung und Vorschlag Verhalten verwendet werden kann. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) Enumerationswerte werden als Argumente an die Methode mit einer benutzerdefinierten angegeben `Keyboard` zurückgegeben wird. Die `KeyboardFlags` Enumeration enthält die folgenden Werte:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – keine Funktionen auf der Tastatur hinzugefügt werden.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – Gibt an, dass es sich bei der erste Buchstaben des ersten Worts eines eingegebenen Satzes automatisch groß geschrieben wird.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) – Gibt an, der eingegebene Text, Rechtschreibprüfung ausgeführt.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) – Gibt an, dass-Abschlüsse werden für den eingegebenen Text.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – Gibt an, dass der erste Buchstabe jedes Worts wird automatisch aktiviert werden.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – Gibt an, dass jedes Zeichen wird automatisch aktiviert werden.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – Gibt an, dass keine automatische Groß-und Kleinschreibung erfolgt.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – Gibt an, dass die Rechtschreibprüfung, Vervollständigung des Worts und die Groß-/Kleinschreibung Satz an eingegebenen Text wiederholt werden soll.

Im folgende XAML-Codebeispiel veranschaulicht das Anpassen der [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) bieten Vervollständigung des Worts und jedes eingegebene Zeichen zu profitieren:

```xaml
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Der entsprechende C#-Code ist:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

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

### <a name="enabling-and-disabling-text-prediction"></a>Aktivieren und Deaktivieren von Textvorhersage

Die `IsTextPredictionEnabled` -Eigenschaft steuert, ob die Text Vorhersage und die automatische Textkorrektur aktiviert ist. Standardmäßig ist die Eigenschaft auf festgelegt `true`. Wenn der Benutzer Text eingibt, werden Word Vorhersagen angezeigt.

Für einige Texteingabe Szenarios, wie z. b. die Eingabe eines Benutzernamens, bietet die Text Vorhersage und die automatische Textkorrektur jedoch eine negative Benutzer `IsTextPredictionEnabled` Umgebung und `false`sollte durch Festlegen der-Eigenschaft auf deaktiviert werden:

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Wenn die `IsTextPredictionEnabled` -Eigenschaft auf `false`festgelegt ist und keine benutzerdefinierte Tastatur verwendet wird, werden die Text Vorhersage und die automatische Textkorrektur deaktiviert. Jedoch, wenn eine [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) wurde festgelegt, Textvorhersage deaktiviert, die `IsTextPredictionEnabled` Eigenschaft wird ignoriert. Aus diesem Grund kann nicht die Eigenschaft verwendet werden, aktivieren Sie die Textvorhersage für eine `Keyboard` , die explizit deaktiviert.

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

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse, von [`Entry`](xref:Xamarin.Forms.Entry) der erbt, verfügt [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) auch [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) über die-und-Ereignisse.

### <a name="completed"></a>Abgeschlossen

Die `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit reagieren ein `Editor`. `Completed`wird ausgelöst, wenn der Benutzer die Eingabe mit einem Feld beendet, indem die Rückgabetaste auf der Tastatur eingegeben wird (oder durch Drücken der Tab-Taste auf der UWP). Der Handler für das Ereignis wird von einem generischen Ereignishandler an, dass des Absenders und `EventArgs`:

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

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Editor-API](xref:Xamarin.Forms.Editor)
