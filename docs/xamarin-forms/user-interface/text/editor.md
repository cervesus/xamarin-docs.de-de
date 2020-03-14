---
title: Xamarin.Forms-Editor
description: In diesem Artikel wird erläutert, wie Sie das Xamarin.Forms-Editor-Steuerelement verwenden, um mehrzeilige Texteingabe in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/26/2019
ms.openlocfilehash: 1ae176cfebdde31038c30895d1bf562ff3396eaa
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306608"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms-Editor

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Mehrzeilige Texteingabe_

Das [`Editor`](xref:Xamarin.Forms.Editor) -Steuerelement wird verwendet, um mehrzeilige Eingaben zu akzeptieren. In diesem Artikel wird Folgendes behandelt:

- **[Anpassung](#customization)** &ndash; Tastatur-und Farboptionen.
- **[Interactivity](#interactivity)** &ndash; Ereignisse, die überwacht werden können, um Interaktivität bereitzustellen.

## <a name="customization"></a>Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Der [`Editor`](xref:Xamarin.Forms.Editor)macht wie andere Text Ansichts Ansichten die `Text`-Eigenschaft verfügbar. Diese Eigenschaft kann verwendet werden, um den von der `Editor`dargestellten Text festzulegen und zu lesen. Im folgenden Beispiel wird veranschaulicht, wie die `Text`-Eigenschaft in XAML festgelegt wird:

```xaml
<Editor Text="I am an Editor" />
```

In C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Um Text zu lesen, greifen Sie in C#auf die `Text`-Eigenschaft zu:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>Festlegen von Platzhaltertext

Der [`Editor`](xref:Xamarin.Forms.Editor) kann so festgelegt werden, dass Platzhalter Text angezeigt wird, wenn er keine Benutzereingaben speichert. Dies wird erreicht, indem die [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) -Eigenschaft auf ein `string`festgelegt wird und häufig verwendet wird, um den Typ des Inhalts anzugeben, der für die `Editor`geeignet ist. Außerdem kann die Platzhalter Textfarbe gesteuert werden, indem die [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) -Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color)festgelegt wird:

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>Verhindern des Text Eintrags

Benutzer können daran gehindert werden, den Text in einem [`Editor`](xref:Xamarin.Forms.Editor) zu ändern, indem Sie die `IsReadOnly`-Eigenschaft mit dem Standardwert `false`auf `true`festlegen:

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> Die `IsReadonly`-Eigenschaft ändert nicht die visuelle Darstellung eines [`Editor`](xref:Xamarin.Forms.Editor), anders als die `IsEnabled`-Eigenschaft, die auch die visuelle Darstellung der `Editor` in grau ändert.

### <a name="limiting-input-length"></a>Geben Sie die Länge beschränken

Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) -Eigenschaft kann verwendet werden, um die für die [`Editor`](xref:Xamarin.Forms.Editor)zulässige Eingabe Länge einzuschränken. Diese Eigenschaft muss eine positive ganze Zahl festgelegt werden:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

Ein [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) -Eigenschafts Wert von 0 gibt an, dass keine Eingabe zulässig ist, und der Wert `int.MaxValue`, der der Standardwert für einen [`Editor`](xref:Xamarin.Forms.Editor)ist, gibt an, dass die Anzahl der Zeichen, die eingegeben werden können, nicht wirksam ist.

### <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann auf eine [`Editor`](xref:Xamarin.Forms.Editor) angewendet werden, indem die `Editor.CharacterSpacing`-Eigenschaft auf einen `double` Wert festgelegt wird:

```xaml
<Editor ...
        CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen in dem Text, der vom [`Editor`](xref:Xamarin.Forms.Editor) angezeigt wird, `CharacterSpacing` geräteunabhängigen Einheiten voneinander entfernt sind.

> [!NOTE]
> Der `CharacterSpacing`-Eigenschafts Wert wird auf den Text angewendet, der von den Eigenschaften `Text` und `Placeholder` angezeigt wird.

### <a name="auto-sizing-an-editor"></a>Automatische Größenanpassung eines Editors

Ein [`Editor`](xref:Xamarin.Forms.Editor) kann so festgelegt werden, dass die Größe seines Inhalts automatisch geändert wird, indem die [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) -Eigenschaft auf [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)festgelegt wird. Dies ist ein Wert der [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) -Enumeration. Diese Enumeration verfügt über zwei Werte:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) gibt an, dass die automatische Größe der Größe deaktiviert ist, und ist der Standardwert.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) gibt an, dass die automatische Größe der Größe aktiviert ist.

Dies kann im Code wie folgt erreicht werden:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Wenn die automatische Größenänderung aktiviert ist, erhöht sich die Höhe des [`Editor`](xref:Xamarin.Forms.Editor) , wenn der Benutzer es mit Text füllt, und die Höhe wird verringert, wenn der Benutzer Text löscht.

> [!NOTE]
> Ein [`Editor`](xref:Xamarin.Forms.Editor) wird nicht automatisch Größe angezeigt, wenn die [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) -Eigenschaft festgelegt wurde.

### <a name="customizing-the-keyboard"></a>Anpassen der Tastaturfokus

Die Tastatur, die angezeigt wird, wenn Benutzer mit einem [`Editor`](xref:Xamarin.Forms.Editor) interagieren, kann Programm gesteuert über die [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) -Eigenschaft auf eine der folgenden Eigenschaften der [`Keyboard`](xref:Xamarin.Forms.Keyboard) -Klasse festgelegt werden:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat): Wird zum Schreiben von Texten verwendet und in Situationen, in denen Emojis nützlich sind.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default): die Standardtastatur.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email): Wird beim Eingeben von E-Mail-Adressen verwendet.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric): Wird beim Eingeben von Zahlen verwendet.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain): Wird beim Eingeben von Text verwendet, ohne Angabe von [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags).
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone): Wird beim Eingeben von Telefonnummern verwendet.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text): Wird beim Eingeben von Text verwendet.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url): Wird beim Eingeben von Dateipfaden und Webadressen verwendet.

Dies kann in XAML folgendermaßen erfüllt werden:

```xaml
<Editor Keyboard="Chat" />
```

Der entsprechende C#-Code lautet:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Beispiele für jede Tastatur finden Sie in unserem [Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) -Repository.

Die [`Keyboard`](xref:Xamarin.Forms.Keyboard)-Klasse verfügt auch über eine [`Create`](xref:Xamarin.Forms.Keyboard.Create*)-Factorymethode, die zum Anpassen einer Tastatur durch Festlegen des Verhaltens für Groß-/Kleinschreibung, Rechtschreibprüfung und Vorschläge verwendet werden kann. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags)-Enumerationswerte werden als Argumente der Methode festgelegt, wobei das benutzerdefinierte `Keyboard` zurückgegeben wird. Die `KeyboardFlags`-Enumeration verfügt über folgende Werte:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None): Der Tastatur werden keine Features hinzugefügt.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence): Gibt an, dass der erste Buchstabe des ersten Worts jedes Satzes automatisch groß geschrieben wird.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck): Gibt an, dass die Rechtschreibprüfung für den eingegebenen Text durchgeführt wird.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions): Gibt an, dass Wortvervollständigungen für den eingegebenen Text angeboten werden.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord): Gibt an, dass der erste Buchstabe von jedem Wort automatisch groß geschrieben wird.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter): Gibt an, dass jedes Zeichen automatisch groß geschrieben wird.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone): Gibt an, dass keine automatische Großschreibung erfolgt.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All): Gibt an, dass für den eingegebenen Text die Rechtschreibprüfung, die Vervollständigung von Wörtern und die Großschreibung am Satzanfang erfolgen.

Das folgende XAML-Codebeispiel zeigt, wie Sie den Standardwert für [`Keyboard`](xref:Xamarin.Forms.Keyboard) anpassen können, um Wortvervollständigungen anzubieten und jedes eingegebene Zeichen groß zu schreiben:

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

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und deaktivieren die Rechtschreibprüfung

Die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft steuert, ob die Rechtschreibprüfung aktiviert ist. Standardmäßig ist die-Eigenschaft auf `true`festgelegt. Wenn der Benutzer Text eingibt, werden von Rechtschreibfehlern angegeben.

Für einige Texteingabe Szenarios, z. b. die Eingabe eines Benutzernamens, bietet die Rechtschreibprüfung jedoch eine negative Darstellung und sollte daher durch Festlegen der [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft auf `false`deaktiviert werden:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft auf `false`festgelegt ist und eine benutzerdefinierte Tastatur nicht verwendet wird, wird die native Rechtschreibprüfung deaktiviert. Wenn jedoch ein [`Keyboard`](xref:Xamarin.Forms.Keyboard) festgelegt wurde, der die Rechtschreibprüfung deaktiviert (z. b. [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat)), wird die Eigenschaft `IsSpellCheckEnabled` ignoriert. Daher kann die-Eigenschaft nicht verwendet werden, um die Rechtschreibprüfung für eine `Keyboard` zu aktivieren, die Sie explizit deaktiviert.

### <a name="enabling-and-disabling-text-prediction"></a>Aktivieren und Deaktivieren von Textvorhersage

Die `IsTextPredictionEnabled`-Eigenschaft steuert, ob die Text Vorhersage und die automatische Textkorrektur aktiviert ist. Standardmäßig ist die-Eigenschaft auf `true`festgelegt. Wenn der Benutzer Text eingibt, werden Word Vorhersagen angezeigt.

Bei manchen Texteingabe Szenarios, z. b. bei der Eingabe eines Benutzernamens, bietet die Text Vorhersage und die automatische Textkorrektur jedoch eine negative Benutzerumgebung und sollte durch Festlegen der `IsTextPredictionEnabled`-Eigenschaft auf `false`deaktiviert werden:

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Wenn die `IsTextPredictionEnabled`-Eigenschaft auf `false`festgelegt ist und eine benutzerdefinierte Tastatur nicht verwendet wird, werden die Text Vorhersage und die automatische Textkorrektur deaktiviert. Wenn jedoch ein [`Keyboard`](xref:Xamarin.Forms.Keyboard) festgelegt wurde, der die Text Vorhersage deaktiviert, wird die `IsTextPredictionEnabled`-Eigenschaft ignoriert. Daher kann die-Eigenschaft nicht verwendet werden, um die Text Vorhersage für eine `Keyboard` zu aktivieren, die Sie explizit deaktiviert.

### <a name="colors"></a>Farben

`Editor` kann so festgelegt werden, dass eine benutzerdefinierte Hintergrundfarbe über die `BackgroundColor`-Eigenschaft verwendet wird. Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform unterschiedliche Standardwerte für die Textfarbe verfügt, müssen Sie eine benutzerdefinierte Hintergrundfarbe für jede Plattform festgelegt. Weitere Informationen zur Optimierung der Benutzeroberfläche für die einzelnen Plattformen finden Sie unter [Arbeiten mit Platt Form Anpassungen](~/xamarin-forms/platform/device.md) .

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

Stellen Sie sicher, dass die Hintergrund- und Textfarben können auf jeder Plattform verwendet werden können und keine Platzhaltertext verdecken.

## <a name="interactivity"></a>Interaktivität

`Editor` macht zwei Ereignisse verfügbar:

- [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash; ausgelöst, wenn sich der Text im Editor ändert. Stellt den Text an, vor und nach der Änderung.
- [Abgeschlossen](xref:Xamarin.Forms.Editor.Completed) &ndash; ausgelöst, wenn der Benutzer die Eingabe beendet hat, indem er die Rückgabetaste auf der Tastatur gedrückt hat.

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse, von der [`Entry`](xref:Xamarin.Forms.Entry) erbt, verfügt auch über [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) -und [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) -Ereignisse.

### <a name="completed"></a>Abgeschlossen

Das `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit einem `Editor`zu reagieren. `Completed` wird ausgelöst, wenn der Benutzer die Eingabe mit einem Feld beendet, indem die Rückgabetaste auf der Tastatur eingegeben wird (oder durch Drücken der Tab-Taste auf der UWP). Der Handler für das-Ereignis ist ein generischer Ereignishandler, der den Absender und den `EventArgs`annimmt:

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

Das `TextChanged` Ereignis wird verwendet, um auf eine Änderung im Inhalt eines Felds zu reagieren.

`TextChanged` wird immer dann ausgelöst, wenn sich die `Text` des `Editor` ändert. Der Handler für das-Ereignis nimmt eine Instanz von `TextChangedEventArgs`an. `TextChangedEventArgs` bietet Zugriff auf die alten und neuen Werte der `Editor` `Text` über die Eigenschaften `OldTextValue` und `NewTextValue`:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Das abgeschlossene Ereignis kann im Code und XAML abonniert werden:

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
