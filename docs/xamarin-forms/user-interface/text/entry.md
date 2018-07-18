---
title: Xamarin.Forms-Eintrag
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms-Eintrag-Klasse verwenden, um einzeiligen Text oder Kennworteingabe in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998231"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms-Eintrag

_Einzeiligen Text oder das Kennwort eingeben_

Die Xamarin.Forms `Entry` als Eingabe für einzeiligen Text verwendet. Die `Entry`, z. B. die `Editor` anzeigen, die mehrere Tastaturtypen unterstützt. Darüber hinaus die `Entry` kann als ein Feld "Kennwort" verwendet werden.

## <a name="display-customization"></a>Anzeigen der Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Die `Entry`, wie andere Ansichten darstellen von Text macht den `Text` Eigenschaft. Diese Eigenschaft kann verwendet werden, festlegen und Lesen Sie den Text an, das von der `Entry`. Das folgende Beispiel veranschaulicht das Festlegen der `Text` -Eigenschaft in XAML:

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

### <a name="keyboards"></a>Tastaturen

Die Tastatur, die angezeigt werden, wenn Benutzer mit interagieren ein `Entry` programmgesteuert festgelegt werden können, über die `Keyboard` Eigenschaft.

Die Optionen für den Tastaturtyp sind:

- **Standardmäßige** &ndash; die Standardtastatur
- **Chat** &ndash; für Hilfsmittel und stellen verwendet, in denen Emoji eignen sich
- **E-Mail-Adresse** &ndash; verwendet werden, wenn Sie e-Mail-Adressen eingeben
- **Numerische** &ndash; verwendet, wenn Zahlen eingeben
- **Telefon** &ndash; verwendet, bei der Eingabe von Telefonnummern
- **URL** &ndash; verwendet für die Eingabe von Dateipfaden und von Webadressen

Gibt es eine [Beispiel für die einzelnen Tastatur](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) im Abschnitt mit Anleitungen.

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und deaktivieren die Rechtschreibprüfung

Die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft steuert, ob Rechtschreibprüfung ist aktiviert. Standardmäßig ist die Eigenschaft auf festgelegt `true`. Wenn der Benutzer Text eingibt, werden von Rechtschreibfehlern angegeben.

Für einige Szenarien für das Text-Eintrag, z. B. den Benutzernamen eingeben Rechtschreibprüfung bietet jedoch eine negative Erfahrung und daher sollte deaktiviert werden, indem die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaftensatz auf `false`, und eine benutzerdefinierte Tastatur nicht verwendet wird, die native Rechtschreibprüfung wird deaktiviert. Jedoch wenn eine [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) hat wurde Satz, der deaktiviert die Rechtschreibprüfung überprüfen, wie z. B. [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` Eigenschaft wird ignoriert. Aus diesem Grund kann nicht die Eigenschaft verwendet werden, aktivieren Sie die Rechtschreibprüfung für eine `Keyboard` , die explizit deaktiviert.

### <a name="placeholders"></a>Platzhalter

`Entry` kann festgelegt werden, um Platzhaltertext angezeigt werden soll, wenn sie Benutzereingaben nicht gespeichert werden. In der Praxis ist dies häufig in Formularen angezeigt, um den Inhalt zu verdeutlichen, der für ein bestimmtes Feld geeignet ist. Textfarbe der Platzhalter kann nicht angepasst werden und identisch unabhängig von der `TextColor` festlegen. Wenn Ihrem Entwurf für eine benutzerdefinierte Platzhalter-Farbe aufruft, müssen Sie ein ausweichen auf einen [benutzerdefinierter Renderer](). Die folgende erstellt eine `Entry` mit "Username" als Platzhalter in XAML:

```xaml
<Entry Placeholder="Username" />
```

In C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Beispiel für einen Platzhalter Indexeintrag")

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

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash; wird ausgelöst, wenn der Text im Eintrag geändert wird. Stellt den Text an, vor und nach der Änderung.
- [Abgeschlossen](xref:Xamarin.Forms.Entry.Completed) &ndash; wird ausgelöst, wenn der Benutzer die Eingabe beendet wurde, durch Drücken der EINGABETASTE auf der Tastatur.

### <a name="completed"></a>Abgeschlossen

Die `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit einem Eintrag zu reagieren. `Completed` wird ausgelöst, wenn der Benutzer Eingaben mit einem Feld beendet, indem Sie die return-Taste auf der Tastatur eingeben. Der Handler für das Ereignis wird von einem generischen Ereignishandler an, dass des Absenders und `EventArgs`:

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
