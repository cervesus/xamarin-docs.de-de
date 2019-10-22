---
title: Xamarin. Forms-Eintrag
description: In diesem Artikel wird erläutert, wie Sie die xamarin. Forms-Einstiegsklasse verwenden, um einzeilige Text-oder Kenn Wort Eingaben in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: 106d22cd7f20f8368bc728aa45c4d7f19d09c44c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697054"
---
# <a name="xamarinforms-entry"></a>Xamarin. Forms-Eintrag

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Einzeilige Text-oder Kenn Wort Eingabe_

Die xamarin. Forms- [`Entry`](xref:Xamarin.Forms.Entry) wird für einzeilige Texteingaben verwendet. Der `Entry` unterstützt wie die [`Editor`](xref:Xamarin.Forms.Editor) Ansicht mehrere Tastaturtypen. Außerdem kann die `Entry` als Kennwort-Feld verwendet werden.

## <a name="display-customization"></a>Anpassung anzeigen

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Der `Entry` macht wie andere Text Ansichts Ansichten die [`Text`](xref:Xamarin.Forms.Entry.Text) -Eigenschaft verfügbar. Diese Eigenschaft kann verwendet werden, um den von der `Entry` dargestellten Text festzulegen und zu lesen. Im folgenden Beispiel wird veranschaulicht, wie die `Text`-Eigenschaft in XAML festgelegt wird:

```xaml
<Entry Text="I am an Entry" />
```

In C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Um Text zu lesen, greifen Sie in C#auf die `Text`-Eigenschaft zu:

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>Festlegen von Platzhalter Text

Der [`Entry`](xref:Xamarin.Forms.Entry) kann so festgelegt werden, dass Platzhalter Text angezeigt wird, wenn er keine Benutzereingaben speichert. Dies wird erreicht, indem die [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) -Eigenschaft auf ein `string` festgelegt wird und häufig verwendet wird, um den Typ des Inhalts anzugeben, der für die `Entry` geeignet ist. Außerdem kann die Platzhalter Textfarbe gesteuert werden, indem die [`PlaceholderColor`](xref:Xamarin.Forms.Entry.PlaceholderColor) -Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color)festgelegt wird:

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> Die Breite einer `Entry` kann durch Festlegen der `WidthRequest`-Eigenschaft definiert werden. Hängen nicht von der Breite einer `Entry`, die basierend auf dem Wert ihrer `Text`-Eigenschaft definiert wird.

### <a name="preventing-text-entry"></a>Verhindern des Text Eintrags

Benutzer können daran gehindert werden, den Text in einem [`Entry`](xref:Xamarin.Forms.Entry) zu ändern, indem Sie die `IsReadOnly`-Eigenschaft mit dem Standardwert `false` auf `true` festlegen:

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> Die `IsReadonly`-Eigenschaft ändert nicht die visuelle Darstellung eines [`Entry`](xref:Xamarin.Forms.Entry), anders als die `IsEnabled`-Eigenschaft, die auch die visuelle Darstellung der `Entry` in grau ändert.

### <a name="limiting-input-length"></a>Einschränken der Eingabe Länge

Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) -Eigenschaft kann verwendet werden, um die für die [`Entry`](xref:Xamarin.Forms.Entry)zulässige Eingabe Länge einzuschränken. Diese Eigenschaft sollte auf eine positive ganze Zahl festgelegt werden:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

Ein [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) -Eigenschafts Wert von 0 gibt an, dass keine Eingabe zulässig ist, und der Wert `int.MaxValue`, der der Standardwert für einen [`Entry`](xref:Xamarin.Forms.Entry)ist, gibt an, dass die Anzahl der Zeichen, die eingegeben werden können, nicht wirksam ist.

### <a name="character-spacing"></a>Zeichenabstand

Der Zeichenabstand kann auf eine [`Entry`](xref:Xamarin.Forms.Entry) angewendet werden, indem die `Entry.CharacterSpacing`-Eigenschaft auf einen `double` Wert festgelegt wird:

```xaml
<Entry ...
       CharacterSpacing="10" />
```

Der entsprechende C#-Code lautet:

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

Das Ergebnis ist, dass Zeichen in dem Text, der vom [`Entry`](xref:Xamarin.Forms.Entry) angezeigt wird, `CharacterSpacing` geräteunabhängigen Einheiten voneinander entfernt sind.

> [!NOTE]
> Der `CharacterSpacing`-Eigenschafts Wert wird auf den Text angewendet, der von den Eigenschaften `Text` und `Placeholder` angezeigt wird.

### <a name="password-fields"></a>Kenn Wort Felder

`Entry` stellt die `IsPassword`-Eigenschaft bereit. Wenn `IsPassword` `true` ist, wird der Inhalt des Felds als schwarze Kreise angezeigt:

In XAML:

```xaml
<Entry IsPassword="true" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![Eintrag IsPassword-Beispiel](entry-images/password.png)

Platzhalter können mit Instanzen von `Entry` verwendet werden, die als Kenn Wort Felder konfiguriert sind:

In XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![Eintrag IsPassword und Platzhalter Beispiel](entry-images/passwordplaceholder.png)

### <a name="setting-the-cursor-position-and-text-selection-length"></a>Festlegen der Cursor Position und der Text Auswahl Länge

Die [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) -Eigenschaft kann verwendet werden, um die Position zurückzugeben oder festzulegen, an der das nächste Zeichen in die Zeichenfolge eingefügt wird, die in der [`Text`](xref:Xamarin.Forms.Entry.Text) -Eigenschaft gespeichert ist:

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

Der Standardwert der [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) -Eigenschaft ist 0 (null), was angibt, dass Text am Anfang des `Entry` eingefügt wird.

Außerdem kann die [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) -Eigenschaft verwendet werden, um die Länge der Textauswahl innerhalb des `Entry` zurückzugeben oder festzulegen:

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

Der Standardwert der [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) -Eigenschaft ist 0 (null), was bedeutet, dass kein Text ausgewählt ist.

### <a name="displaying-a-clear-button"></a>Anzeigen einer Schaltfläche "Löschen"

Die `ClearButtonVisibility`-Eigenschaft kann verwendet werden, um zu steuern, ob ein [`Entry`](xref:Xamarin.Forms.Entry) eine Schaltfläche Löschen anzeigt, die dem Benutzer ermöglicht, den Text zu löschen. Diese Eigenschaft sollte auf einen `ClearButtonVisibility` Enumerationsmember festgelegt werden:

- `Never` gibt an, dass nie eine Schaltfläche "Löschen" angezeigt wird. Dies ist der Standardwert der `Entry.ClearButtonVisibility`-Eigenschaft.
- `WhileEditing` gibt an, dass eine Schaltfläche Löschen in der [`Entry`](xref:Xamarin.Forms.Entry)angezeigt wird, während Sie den Fokus und den Text hat.

Im folgenden Beispiel wird gezeigt, wie die-Eigenschaft in XAML festgelegt wird:

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

Der entsprechende C#-Code lautet:

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

Die folgenden Screenshots zeigen eine [`Entry`](xref:Xamarin.Forms.Entry) , bei der die Schaltfläche Löschen aktiviert ist:

![Screenshot eines Eintrags mit der Schaltfläche "Clear" unter IOS und Android](entry-images/entry-clear-button.png)

### <a name="customizing-the-keyboard"></a>Anpassen der Tastatur

Die Tastatur, die angezeigt wird, wenn Benutzer mit einem [`Entry`](xref:Xamarin.Forms.Entry) interagieren, kann Programm gesteuert über die [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) -Eigenschaft auf eine der folgenden Eigenschaften der [`Keyboard`](xref:Xamarin.Forms.Keyboard) -Klasse festgelegt werden:

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
<Entry Keyboard="Chat" />
```

Der entsprechende C#-Code lautet:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
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
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Der entsprechende C#-Code lautet:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Anpassen der Rückgabetaste

Das Aussehen der Rückgabetaste auf der Bildschirmtastatur, die angezeigt wird, wenn ein [`Entry`](xref:Xamarin.Forms.Entry) den Fokus besitzt, kann angepasst werden, indem die [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType) -Eigenschaft auf einen Wert der [`ReturnType`](xref:Xamarin.Forms.ReturnType) -Enumeration festgelegt wird:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – gibt an, dass keine bestimmte Rückgabetaste erforderlich ist und dass die Plattform standardmäßig verwendet wird.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – gibt einen "Done"-Rückgabe Schlüssel an.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – gibt einen "Go"-Rückgabe Schlüssel an.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – gibt einen "Next"-Rückgabe Schlüssel an.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – gibt eine "Search"-Rückgabetaste an.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – gibt einen "Send"-Rückgabe Schlüssel an.

Das folgende XAML-Beispiel zeigt, wie Sie die Rückgabetaste festlegen:

```xaml
<Entry ReturnType="Send" />
```

Der entsprechende C#-Code lautet:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Die genaue Darstellung der Rückgabetaste hängt von der Plattform ab. Unter IOS ist die Rückgabetaste eine textbasierte Schaltfläche. Auf den Windows-Plattformen Android und Universal ist die Rückgabetaste jedoch eine Symbol basierte Schaltfläche.

Wenn die Rückgabetaste gedrückt wird, wird das [`Completed`](xref:Xamarin.Forms.Entry.Completed) -Ereignis ausgelöst, und alle von der [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) -Eigenschaft angegebenen `ICommand` werden ausgeführt. Außerdem werden alle von der [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) -Eigenschaft angegebenen `object` als Parameter an die `ICommand` übergeben. Weitere Informationen zu Befehlen finden Sie unter [The Xamarin.Forms Command Interface (Die Xamarin.Forms-Befehlsschnittstelle)](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und Deaktivieren der Rechtschreibprüfung

Die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft steuert, ob die Rechtschreibprüfung aktiviert ist. Standardmäßig ist die-Eigenschaft auf `true` festgelegt. Wenn der Benutzer Text eingibt, werden falsche Schreibweisen angegeben.

Für einige Texteingabe Szenarios, z. b. die Eingabe eines Benutzernamens, bietet die Rechtschreibprüfung jedoch eine negative Benutzerumgebung und sollte durch Festlegen der [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft auf `false` deaktiviert werden:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaft auf `false` festgelegt ist und eine benutzerdefinierte Tastatur nicht verwendet wird, wird die native Rechtschreibprüfung deaktiviert. Wenn jedoch ein [`Keyboard`](xref:Xamarin.Forms.Keyboard) festgelegt wurde, der die Rechtschreibprüfung deaktiviert (z. b. [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat)), wird die Eigenschaft `IsSpellCheckEnabled` ignoriert. Daher kann die-Eigenschaft nicht verwendet werden, um die Rechtschreibprüfung für eine `Keyboard` zu aktivieren, die Sie explizit deaktiviert.

### <a name="enabling-and-disabling-text-prediction"></a>Aktivieren und Deaktivieren der Text Vorhersage

Die [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) -Eigenschaft steuert, ob die Text Vorhersage und die automatische Textkorrektur aktiviert ist. Standardmäßig ist die-Eigenschaft auf `true` festgelegt. Wenn der Benutzer Text eingibt, werden Word-Vorhersagen angezeigt.

Bei manchen Texteingabe Szenarios, z. b. bei der Eingabe eines Benutzernamens, bietet die Text Vorhersage und die automatische Textkorrektur jedoch eine negative Benutzerumgebung und sollte durch Festlegen der [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) -Eigenschaft auf `false` deaktiviert werden:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Wenn die [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) -Eigenschaft auf `false` festgelegt ist und eine benutzerdefinierte Tastatur nicht verwendet wird, werden die Text Vorhersage und die automatische Textkorrektur deaktiviert. Wenn jedoch ein [`Keyboard`](xref:Xamarin.Forms.Keyboard) festgelegt wurde, der die Text Vorhersage deaktiviert, wird die `IsTextPredictionEnabled`-Eigenschaft ignoriert. Daher kann die-Eigenschaft nicht verwendet werden, um die Text Vorhersage für eine `Keyboard` zu aktivieren, die Sie explizit deaktiviert.

### <a name="colors"></a>Farben

Der Eintrag kann so festgelegt werden, dass benutzerdefinierte Hintergrund-und Textfarben über die folgenden bindbaren Eigenschaften verwendet werden:

- **TextColor** &ndash; legt die Textfarbe fest.
- **BackgroundColor** &ndash; legt die Farbe hinter dem Text fest.

Eine besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform über andere Standardwerte für Text-und Hintergrundfarben verfügt, müssen Sie häufig beide festlegen, wenn Sie einen festlegen.

Verwenden Sie den folgenden Code, um die Textfarbe eines Eintrags festzulegen:

In XAML:

```xaml
<Entry TextColor="Green" />
```

In C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![Eintrag TextColor-Beispiel](entry-images/textcolor.png)

Beachten Sie, dass der Platzhalter vom angegebenen `TextColor` nicht betroffen ist.

So legen Sie die Hintergrundfarbe in XAML fest:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

In C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![Beispiel für den Eintrag BackgroundColor](entry-images/textbackgroundcolor.png)

Achten Sie darauf, dass die von Ihnen ausgewählten Hintergrund-und Textfarben auf den einzelnen Plattformen verwendbar sind und keinen Platzhalter Text verbergen.

## <a name="events-and-interactivity"></a>Ereignisse und Interaktivität

Der Eintrag macht zwei Ereignisse verfügbar:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; ausgelöst, wenn sich der Text im Eintrag ändert. Stellt den Text vor und nach der Änderung bereit.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; ausgelöst, wenn der Benutzer die Eingabe beendet hat, indem er die Rückgabetaste auf der Tastatur gedrückt hat.

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse, von der [`Entry`](xref:Xamarin.Forms.Entry) erbt, verfügt auch über [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) -und [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) -Ereignisse.

### <a name="completed"></a>Abgeschlossen

Das `Completed`-Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit einem-Eintrag zu reagieren. `Completed` wird ausgelöst, wenn der Benutzer die Eingabe mit einem Feld beendet, indem die Rückgabetaste auf der Tastatur gedrückt wird (oder durch Drücken der Tab-Taste auf der UWP). Der Handler für das-Ereignis ist ein generischer Ereignishandler, der den Absender und den `EventArgs` annimmt:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Das abgeschlossene Ereignis kann in XAML abonniert werden:

```xaml
<Entry Completed="Entry_Completed" />
```

und C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

Nachdem das [`Completed`](xref:Xamarin.Forms.Entry.Completed) -Ereignis ausgelöst wurde, werden alle von der [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) -Eigenschaft angegebenen `ICommand` ausgeführt, wobei der `object` von der [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) -Eigenschaft angegeben wird, die an die `ICommand` übermittelt wird.

### <a name="textchanged"></a>TextChanged

Das `TextChanged` Ereignis wird verwendet, um auf eine Änderung im Inhalt eines Felds zu reagieren.

`TextChanged` wird immer dann ausgelöst, wenn sich die `Text` des `Entry` ändert. Der Handler für das-Ereignis nimmt eine Instanz von `TextChangedEventArgs` an. `TextChangedEventArgs` bietet Zugriff auf die alten und neuen Werte der `Entry` `Text` über die Eigenschaften `OldTextValue` und `NewTextValue`:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Das `TextChanged` Ereignis kann in XAML abonniert werden:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

und C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Entry-API](xref:Xamarin.Forms.Entry)
