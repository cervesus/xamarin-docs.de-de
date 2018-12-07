---
title: Xamarin.Forms-Eintrag
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms-Eintrag-Klasse verwenden, um einzeiligen Text oder Kennworteingabe in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
ms.openlocfilehash: 39af3b0e28bbbf9397ceece55adc330e364dcc3d
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061784"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms-Eintrag

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_Einzeiligen Text oder das Kennwort eingeben_

Die Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) als Eingabe für einzeiligen Text verwendet. Die `Entry`, z. B. die [ `Editor` ](xref:Xamarin.Forms.Editor) anzeigen, die mehrere Tastaturtypen unterstützt. Darüber hinaus die `Entry` kann als ein Feld "Kennwort" verwendet werden.

## <a name="display-customization"></a>Anzeigen der Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Die `Entry`, wie andere Ansichten darstellen von Text macht den [ `Text` ](xref:Xamarin.Forms.Entry.Text) Eigenschaft. Diese Eigenschaft kann verwendet werden, festlegen und Lesen Sie den Text an, das von der `Entry`. Das folgende Beispiel veranschaulicht das Festlegen der `Text` -Eigenschaft in XAML:

```xaml
<Entry Text="I am an Entry" />
```

In C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Um Text zu lesen, Zugriff auf die `Text` Eigenschaft in c#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Die Breite des ein `Entry` definiert werden, indem Sie die Einstellung der `WidthRequest` Eigenschaft. Hängen nicht von der Breite des ein `Entry` definiert basierend auf den Wert der `Text` Eigenschaft.

### <a name="limiting-input-length"></a>Geben Sie die Länge beschränken

Die [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Eigenschaft kann verwendet werden, um die Eingabe begrenzt, die zulässige Menge für die [ `Entry` ](xref:Xamarin.Forms.Entry). Diese Eigenschaft muss eine positive ganze Zahl festgelegt werden:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

Ein [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) der Eigenschaftswert 0 gibt an, dass keine Eingabe zulässig ist, und den Wert `int.MaxValue`, dies ist der Standardwert für eine [ `Entry` ](xref:Xamarin.Forms.Entry), gibt an, dass keine effektive Höchstwert für die Anzahl der Zeichen, die eingegeben werden können.

### <a name="setting-the-cursor-position-and-text-selection-length"></a>Festlegen der Cursorposition und Textlänge-Auswahl

Die [ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition) Eigenschaft kann verwendet werden, um zurückzugeben, oder legen Sie die Position, an dem das nächste Zeichen in der Zeichenfolge, die in gespeicherten eingefügt werden, die [ `Text` ](xref:Xamarin.Forms.Entry.Text) Eigenschaft:

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

Der Standardwert der [ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition) -Eigenschaft ist 0. Dies gibt an, dass der Text am Anfang des eingefügt wird die `Entry`.

Darüber hinaus die [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength) Eigenschaft kann verwendet werden, um zurückzugeben, oder legen Sie die Länge der textmarkierung im der `Entry`:

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

Der Standardwert der [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength) -Eigenschaft ist 0. Dies gibt an, dass kein Text ausgewählt ist.

### <a name="customizing-the-keyboard"></a>Anpassen der Tastaturfokus

Die Tastatur, die angezeigt werden, wenn Benutzer mit interagieren ein [ `Entry` ](xref:Xamarin.Forms.Entry) programmgesteuert festgelegt werden können, über die [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) Eigenschaft verwenden, um eine der folgenden Eigenschaften aus der [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Klasse:

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
<Entry Keyboard="Chat" />
```

Der entsprechende C#-Code ist:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
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

Der entsprechende C#-Code ist:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Anpassen der Return-Taste

Die Darstellung der return-Taste auf die Bildschirmtastatur, handelt es sich angezeigt wird, wenn ein [ `Entry` ](xref:Xamarin.Forms.Entry) den Fokus besitzt, kann angepasst werden, durch Festlegen der [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType) Eigenschaft mit einem Wert, der die [ `ReturnType` ](xref:Xamarin.Forms.ReturnType) Enumeration:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – Gibt an, dass keine bestimmte return-Taste erforderlich ist und Standardwert für die Plattform verwendet wird.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – Gibt an, eine "Done" return-Taste.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – Gibt an, eine "Go" return-Taste.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – Gibt an, eine return-Taste "Weiter".
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – Gibt an, eine return-Taste "Suche".
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – Gibt an, eine return-Taste "Senden".

Im folgende XAML-Beispiel veranschaulicht die legen Sie der EINGABETASTE:

```xaml
<Entry ReturnType="Send" />
```

Der entsprechende C#-Code ist:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Die genaue Darstellung der return-Taste ist abhängig von der Plattform. Unter iOS ist die return-Taste eine textbasierte-Schaltfläche. Auf die Android- und universelle Windows-Plattformen ist die return-Taste jedoch eine Schaltfläche mit Symbol-basierte.

Wenn die return-Taste gedrückt wird, die [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) Ereignis wird ausgelöst, und alle `ICommand` gemäß der [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) Eigenschaft ausgeführt wird. Darüber hinaus alle `object` gemäß der [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) Eigenschaft wird zum Übergeben der `ICommand` als Parameter. Weitere Informationen zu Befehlen finden Sie unter [die Befehlsschnittstelle](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und deaktivieren die Rechtschreibprüfung

Die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft steuert, ob Rechtschreibprüfung ist aktiviert. Standardmäßig ist die Eigenschaft auf festgelegt `true`. Wenn der Benutzer Text eingibt, werden von Rechtschreibfehlern angegeben.

Allerdings für einige Szenarien für das Text-Eintrag, z. B. den Benutzernamen eingeben Rechtschreibprüfung bietet eine negative und sollte deaktiviert werden, indem die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaftensatz auf `false`, und eine benutzerdefinierte Tastatur nicht verwendet wird, die native Rechtschreibprüfung wird deaktiviert. Jedoch wenn eine [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) hat wurde Satz, der deaktiviert die Rechtschreibprüfung überprüfen, wie z. B. [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` Eigenschaft wird ignoriert. Aus diesem Grund kann nicht die Eigenschaft verwendet werden, aktivieren Sie die Rechtschreibprüfung für eine `Keyboard` , die explizit deaktiviert.

### <a name="enabling-and-disabling-text-prediction"></a>Aktivieren und Deaktivieren von Textvorhersage

Die [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) Eigenschaft steuert, ob die Textvorhersage und automatisierter Textkorrektur ist aktiviert. Standardmäßig ist die Eigenschaft auf festgelegt `true`. Wenn der Benutzer Text eingibt, werden Word Vorhersagen angezeigt.

Allerdings für einige Szenarien für den Text-Eintrag, wie das Eingeben von Benutzername, Textvorhersage und automatische Text Korrektur bietet eine negative und sollte deaktiviert werden, indem die [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) Eigenschaft `false`:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Wenn die [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) -Eigenschaftensatz auf `false`, und eine benutzerdefinierte Tastatur wird nicht verwendet, die Textvorhersage und automatische Text deaktiviert ist. Jedoch, wenn eine [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) wurde festgelegt, Textvorhersage deaktiviert, die `IsTextPredictionEnabled` Eigenschaft wird ignoriert. Aus diesem Grund kann nicht die Eigenschaft verwendet werden, aktivieren Sie die Textvorhersage für eine `Keyboard` , die explizit deaktiviert.

### <a name="setting-placeholder-text"></a>Festlegen von Platzhaltertext

Die [ `Entry` ](xref:Xamarin.Forms.Entry) Platzhaltertext angezeigt, wenn es keine Benutzereingaben gespeichert sind, die festgelegt werden können. Dies geschieht durch Festlegen der [ `Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) Eigenschaft, um eine `string`, und wird häufig verwendet, um den Typ des Inhalts anzugeben, dass für die `Entry`. Darüber hinaus kann die Farbe des Textes Platzhalter gesteuert werden, durch Festlegen der [ `PlaceholderColor` ](xref:Xamarin.Forms.Entry.PlaceholderColor) Eigenschaft, um eine [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

### <a name="password-fields"></a>Kennwortfelder

`Entry` Stellt die `IsPassword` Eigenschaft. Wenn `IsPassword` ist `true`, den Inhalt des Felds wird als schwarze Kreise angezeigt:

In XAML:

```xaml
<Entry IsPassword="true" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Beispiel für einen Indexeintrag IsPassword")

Platzhalter können verwendet werden, mit Instanzen von `Entry` , der als Kennwortfelder konfiguriert werden:

In XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Eintrag IsPassword und Platzhalter-Beispiel")

### <a name="colors"></a>Farben

Eintrag kann festgelegt werden, um einen benutzerdefinierten Hintergrund und Textfarben über die folgenden bindbare Eigenschaften verwenden:

- **TextColor** &ndash; legt die Farbe des Texts fest.
- **BackgroundColor** &ndash; legt die Farbe des Texthintergrunds gezeigt fest.

Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden können. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben verfügt, müssen Sie häufig, beide Werte festzulegen, wenn Sie eine Eigenschaft festgelegt.

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

![](entry-images/textcolor.png "Beispiel für einen Indexeintrag TextColor")

Beachten Sie, dass der Platzhalter nicht betroffen, mit dem angegebenen `TextColor`.

So legen die Farbe des Hintergrunds in XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

In C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Beispiel für einen Indexeintrag BackgroundColor")

Achten Sie darauf, um sicherzustellen, dass die Hintergrund- und Textfarben können auf jeder Plattform verwendet werden können und keine Platzhaltertext verdecken.

## <a name="events-and-interactivity"></a>Ereignisse und Interaktivität

Eintrag macht zwei Ereignisse:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; ausgelöst, wenn der Text im Eintrag geändert wird. Stellt den Text an, vor und nach der Änderung.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; wird ausgelöst, wenn der Benutzer die Eingabe beendet wurde, durch Drücken der EINGABETASTE auf der Tastatur.

### <a name="completed"></a>Abgeschlossen

Die `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit einem Eintrag zu reagieren. `Completed` wird ausgelöst, wenn der Benutzer die Eingabe mit einem Feld durch Drücken der EINGABETASTE auf der Tastatur beendet. Der Handler für das Ereignis wird von einem generischen Ereignishandler an, dass des Absenders und `EventArgs`:

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

und C# -Code:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

Nach der [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) Ereignis ausgelöst wird, alle `ICommand` gemäß der [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) Eigenschaft ausgeführt wird, mit der `object` gemäß der [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) Eigenschaft übergeben wird, um die `ICommand`.

### <a name="textchanged"></a>TextChanged

Die `TextChanged` Ereignis wird verwendet, um auf eine Änderung in den Inhalt eines Felds zu reagieren.

`TextChanged` wird ausgelöst, wenn die `Text` von der `Entry` Änderungen. Der Handler für das Ereignis benötigt eine Instanz der `TextChangedEventArgs`. `TextChangedEventArgs` ermöglicht den Zugriff auf die alten und neuen Werte der `Entry` `Text` über die `OldTextValue` und `NewTextValue` Eigenschaften:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Die `TextChanged` Ereignis in XAML abonniert werden kann:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

und C# -Code:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API-Eintrag](xref:Xamarin.Forms.Entry)
