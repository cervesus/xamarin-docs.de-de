---
title: Xamarin.FormsBe
description: In diesem Artikel wird erläutert, wie das Xamarin.Forms Editor-Steuerelement verwendet wird, um mehrzeilige Texteingaben in einer Anwendung zu akzeptieren.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 02749c9f8f55427bb1742e78464bbc003f1f7358
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136174"
---
# <a name="xamarinforms-editor"></a>Xamarin.FormsBe

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Mehrzeilige Texteingabe_

Das- [`Editor`](xref:Xamarin.Forms.Editor) Steuerelement wird verwendet, um mehrzeilige Eingaben zu akzeptieren. In diesem Artikel wird Folgendes behandelt:

- **[Anpassung](#customization)** &ndash; Tastatur-und Farboptionen.
- **[Interaktivität](#interactivity)** &ndash; Ereignisse, die überwacht werden können, um Interaktivität bereitzustellen.

## <a name="customization"></a>Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Die [`Editor`](xref:Xamarin.Forms.Editor) -Eigenschaft wird wie bei anderen Text dargestellten Sichten von verfügbar gemacht `Text` . Diese Eigenschaft kann verwendet werden, um den von dargestellten Text festzulegen und zu lesen `Editor` . Im folgenden Beispiel wird veranschaulicht, wie die- `Text` Eigenschaft in XAML festgelegt wird:

```xaml
<Editor Text="I am an Editor" />
```

In C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Um Text zu lesen, greifen Sie `Text` in c# auf die-Eigenschaft zu:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>Festlegen von Platzhalter Text

[`Editor`](xref:Xamarin.Forms.Editor)Kann so festgelegt werden, dass Platzhalter Text angezeigt wird, wenn keine Benutzereingaben gespeichert werden. Dies wird erreicht, indem die [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) -Eigenschaft auf festgelegt wird `string` und häufig verwendet wird, um den Typ des Inhalts anzugeben, der für geeignet ist `Editor` . Außerdem kann die Platzhalter Textfarbe gesteuert werden, indem die-Eigenschaft auf festgelegt wird [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>Verhindern von Texteinträgen

Benutzer können daran gehindert werden, den Text in einer [`Editor`](xref:Xamarin.Forms.Editor) zu ändern, indem Sie die-Eigenschaft mit dem `IsReadOnly` Standardwert `false` auf festlegen `true` :

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> Die- `IsReadonly` Eigenschaft ändert nicht die visuelle Darstellung eines [`Editor`](xref:Xamarin.Forms.Editor) , anders als die- `IsEnabled` Eigenschaft, die auch die visuelle Darstellung von `Editor` in grau ändert.

### <a name="limiting-input-length"></a>Einschränken der Eingabe Länge

Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) -Eigenschaft kann verwendet werden, um die für zulässige Eingabe Länge einzuschränken [`Editor`](xref:Xamarin.Forms.Editor) . Diese Eigenschaft sollte auf eine positive ganze Zahl festgelegt werden:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

Ein [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) -Eigenschafts Wert von 0 gibt an, dass keine Eingabe zulässig ist, und der Wert `int.MaxValue` , der der Standardwert für ein ist [`Editor`](xref:Xamarin.Forms.Editor) , gibt an, dass die Anzahl der Zeichen, die eingegeben werden können, nicht wirksam ist.

### <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann auf ein angewendet werden, [`Editor`](xref:Xamarin.Forms.Editor) indem die- `Editor.CharacterSpacing` Eigenschaft auf einen Wert festgelegt wird `double` :

```xaml
<Editor ...
        CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen im Text, die von angezeigt [`Editor`](xref:Xamarin.Forms.Editor) werden, `CharacterSpacing` geräteunabhängige Einheiten voneinander getrennt sind.

> [!NOTE]
> Der `CharacterSpacing` -Eigenschafts Wert wird auf den Text angewendet, der von den `Text` Eigenschaften und angezeigt wird `Placeholder` .

### <a name="auto-sizing-an-editor"></a>Automatische Größenanpassung eines Editors

Eine [`Editor`](xref:Xamarin.Forms.Editor) kann so festgelegt werden, dass die Größe des Inhalts automatisch geändert wird, indem die-Eigenschaft auf festgelegt wird [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) . Dies ist ein Wert der- [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) Enumeration. Diese Enumeration verfügt über zwei Werte:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled)Gibt an, dass die automatische Größe der Größe deaktiviert ist und der Standardwert ist.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)Gibt an, dass die automatische Größe der Größe aktiviert ist.

Dies kann im Code wie folgt erreicht werden:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Wenn die automatische Größenänderung aktiviert ist, erhöht sich die Höhe des, [`Editor`](xref:Xamarin.Forms.Editor) Wenn der Benutzer ihn mit Text füllt, und die Höhe wird verringert, wenn der Benutzer Text löscht.

> [!NOTE]
> [`Editor`](xref:Xamarin.Forms.Editor)Wenn die Eigenschaft festgelegt wurde, wird keine automatische Größe angezeigt [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) .

### <a name="customizing-the-keyboard"></a>Anpassen der Tastatur

Die Tastatur, die angezeigt wird, wenn Benutzer mit einer interagieren [`Editor`](xref:Xamarin.Forms.Editor) , kann Programm gesteuert über die- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) Eigenschaft auf eine der folgenden Eigenschaften der-Klasse festgelegt werden [`Keyboard`](xref:Xamarin.Forms.Keyboard) :

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)– wird für SMS und Orte verwendet, an denen Emoji nützlich ist.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default)– die Standardtastatur.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)– wird beim Eingeben von e-Mail-Adressen verwendet.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric)– wird beim Eingeben von Zahlen verwendet.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain)– wird bei der Eingabe von Text ohne Angabe eines [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) angegeben.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)– wird bei der Eingabe von Telefonnummern verwendet.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)– wird bei der Eingabe von Text verwendet.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)– wird zum Eingeben von Dateipfaden & Webadressen verwendet.

Dies kann in XAML folgendermaßen erfüllt werden:

```xaml
<Editor Keyboard="Chat" />
```

Der entsprechende C#-Code lautet:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Beispiele für jede Tastatur finden Sie in unserem [Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) -Repository.

Die- [`Keyboard`](xref:Xamarin.Forms.Keyboard) Klasse verfügt auch über eine Factorymethode [`Create`](xref:Xamarin.Forms.Keyboard.Create*) , mit der eine Tastatur angepasst werden kann, indem Sie die Groß-/Kleinschreibung, Rechtschreibprüfung und das Vorschlags Verhalten [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags)Enumerationswerte werden als Argumente für die-Methode angegeben, wobei eine angepasste `Keyboard` zurückgegeben wird. Die `KeyboardFlags`-Enumeration verfügt über folgende Werte:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None)– der Tastatur werden keine Funktionen hinzugefügt.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence)– Gibt an, dass der erste Buchstabe des ersten Worts jedes eingegebenen Satzes automatisch groß geschrieben wird.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck)– Gibt an, dass die Rechtschreibprüfung für eingegebenen Text ausgeführt wird.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions)– Gibt an, dass Wort Vervollständigungen für eingegebenen Text angeboten werden.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord)– Gibt an, dass der erste Buchstabe jedes Worts automatisch groß geschrieben wird.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter)– Gibt an, dass jedes Zeichen automatisch groß geschrieben wird.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone)– Gibt an, dass keine automatische groß Schreibung auftritt.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All)– Gibt an, dass die Rechtschreibprüfung, die Wortvervollständigung und die Satz-groß Schreibung für eingegebenen Text auftreten.

Im folgenden XAML-Codebeispiel wird veranschaulicht, wie Sie die Standardeinstellung anpassen [`Keyboard`](xref:Xamarin.Forms.Keyboard) , um Wort Vervollständigungen zu bieten und jedes eingegebene Zeichen groß zu schreiben:

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

Der entsprechende C#-Code lautet:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und Deaktivieren der Rechtschreibprüfung

Die- [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft steuert, ob die Rechtschreibprüfung aktiviert ist. Standardmäßig ist die-Eigenschaft auf festgelegt `true` . Wenn der Benutzer Text eingibt, werden falsche Schreibweisen angegeben.

Für einige Texteingabe Szenarios, z. b. die Eingabe eines Benutzernamens, bietet die Rechtschreibprüfung jedoch eine negative Darstellung und sollte daher durch Festlegen der- [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft auf deaktiviert werden `false` :

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft auf festgelegt ist `false` und eine benutzerdefinierte Tastatur nicht verwendet wird, wird die native Rechtschreibprüfung deaktiviert. Wenn jedoch ein [`Keyboard`](xref:Xamarin.Forms.Keyboard) festgelegt wurde, der die Rechtschreibprüfung deaktiviert (z. b.), [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) wird die- `IsSpellCheckEnabled` Eigenschaft ignoriert. Daher kann die-Eigenschaft nicht verwendet werden, um die Rechtschreibprüfung für einen zu aktivieren `Keyboard` , der diese explizit deaktiviert.

### <a name="enabling-and-disabling-text-prediction"></a>Aktivieren und Deaktivieren der Textvorhersage

Die `IsTextPredictionEnabled` -Eigenschaft steuert, ob die Text Vorhersage und die automatische Textkorrektur aktiviert ist. Standardmäßig ist die-Eigenschaft auf festgelegt `true` . Wenn der Benutzer Text eingibt, werden Word-Vorhersagen angezeigt.

Für einige Texteingabe Szenarios, wie z. b. die Eingabe eines Benutzernamens, bietet die Text Vorhersage und die automatische Textkorrektur jedoch eine negative Benutzerumgebung und sollte durch Festlegen der- `IsTextPredictionEnabled` Eigenschaft auf deaktiviert werden `false` :

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Wenn die `IsTextPredictionEnabled` -Eigenschaft auf festgelegt ist `false` und keine benutzerdefinierte Tastatur verwendet wird, werden die Text Vorhersage und die automatische Textkorrektur deaktiviert. Wenn jedoch ein [`Keyboard`](xref:Xamarin.Forms.Keyboard) festgelegt wurde, der die Text Vorhersage deaktiviert, wird die- `IsTextPredictionEnabled` Eigenschaft ignoriert. Daher kann die-Eigenschaft nicht zum Aktivieren der Text Vorhersage für einen verwendet werden, der diese `Keyboard` explizit deaktiviert.

### <a name="colors"></a>Farben

`Editor`kann festgelegt werden, um eine benutzerdefinierte Hintergrundfarbe über die-Eigenschaft zu verwenden `BackgroundColor` . Eine besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform über andere Standardwerte für die Textfarbe verfügt, müssen Sie möglicherweise für jede Plattform eine benutzerdefinierte Hintergrundfarbe festlegen. Weitere Informationen zur Optimierung der Benutzeroberfläche für die einzelnen Plattformen finden Sie unter [Arbeiten mit Platt Form Anpassungen](~/xamarin-forms/platform/device.md) .

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

![](editor-images/textbackgroundcolor.png "Editor with BackgroundColor Example")

Stellen Sie sicher, dass die von Ihnen ausgewählten Hintergrund-und Textfarben auf jeder Plattform verwendbar sind und keinen Platzhalter Text verbergen.

## <a name="interactivity"></a>Interaktivität

`Editor`macht zwei Ereignisse verfügbar:

- [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash; wird ausgelöst, wenn der Text im Editor geändert wird. Stellt den Text vor und nach der Änderung bereit.
- [Abgeschlossen](xref:Xamarin.Forms.Editor.Completed) &ndash; wird ausgelöst, wenn der Benutzer die Eingabe durch Drücken der Rückgabetaste auf der Tastatur beendet hat.

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse, von der [`Entry`](xref:Xamarin.Forms.Entry) erbt, verfügt auch über die [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) -und- [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) Ereignisse.

### <a name="completed"></a>Abgeschlossen

Das- `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit einer zu reagieren `Editor` . `Completed`wird ausgelöst, wenn der Benutzer die Eingabe mit einem Feld beendet, indem die Rückgabetaste auf der Tastatur eingegeben wird (oder durch Drücken der Tab-Taste auf der UWP). Der Handler für das-Ereignis ist ein generischer Ereignishandler, der den Absender und Folgendes annimmt `EventArgs` :

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Das abgeschlossene Ereignis kann im Code und in XAML abonniert werden:

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

Das- `TextChanged` Ereignis wird verwendet, um auf eine Änderung im Inhalt eines Felds zu reagieren.

`TextChanged`wird immer dann ausgelöst, wenn der der `Text` `Editor` geändert wird. Der Handler für das-Ereignis nimmt eine Instanz von an `TextChangedEventArgs` . `TextChangedEventArgs`ermöglicht den Zugriff auf die alten und neuen Werte der `Editor` `Text` über die `OldTextValue` -Eigenschaft und die-Eigenschaft `NewTextValue` :

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Das abgeschlossene Ereignis kann im Code und in XAML abonniert werden:

Im Code:

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
