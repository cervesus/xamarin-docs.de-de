---
title: Xamarin.Forms-Eintrag
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-Eintrag-Klasse verwenden, um einzeilige Text- oder Kennworteingabe in einer Anwendung zu akzeptieren.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: b6188b986589a56229ad2e092d4100ff3f75dbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245552"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms-Eintrag

_Einzeilige Text- oder Kennwort eingeben_

Die Xamarin.Forms `Entry` für einzeilige Texteingabe verwendet wird. Die `Entry`, z. B. die `Editor` anzeigen, unterstützt mehrere Tastaturtypen. Darüber hinaus die `Entry` kann als ein Kennwortfeld verwendet werden.

## <a name="display-customization"></a>Anzeige-Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Die `Entry`, wie andere Sichten Text vorlegen macht die `Text` Eigenschaft. Diese Eigenschaft kann verwendet werden, festlegen und Lesen Sie den Text, präsentiert von der `Entry`. Das folgende Beispiel veranschaulicht das Festlegen der `Text` Eigenschaft in XAML:

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
> Die Breite des ein `Entry` können definiert werden, indem seine `WidthRequest` Eigenschaft. Hängen die Breite des kein `Entry` definierende basierend auf dem Wert von dessen `Text` Eigenschaft.

### <a name="limiting-input-length"></a>Beschränken der Länge Eingabe

Die [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Eigenschaft kann verwendet werden, um die Eingabe Länge beschränkt werden, der für zulässig ist die [ `Entry` ](xref:Xamarin.Forms.Entry). Diese Eigenschaft sollte eine positive ganze Zahl festgelegt werden:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

Ein [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) der Eigenschaftswert 0 gibt an, dass keine Eingabe möglich ist, und der Wert `int.MaxValue`, dies ist der Standardwert für eine [ `Entry` ](xref:Xamarin.Forms.Entry), gibt an, dass keine effektive Höchstwert für die Anzahl der Zeichen, die eingegeben werden können.

### <a name="keyboards"></a>Tastaturen

Der Tastatur, die angezeigt werden, wenn Benutzer interagieren ein `Entry` programmgesteuert festgelegt werden können, über die `Keyboard` Eigenschaft.

Die Optionen für die Tastaturtyp sind:

- **Standardmäßige** &ndash; der Standardtastatur
- **Chat** &ndash; für Texte & Stellen verwendet, in denen Emoji eignen sich
- **E-Mail** &ndash; verwendet, wenn e-Mail-Adressen eingeben.
- **Numerische** &ndash; verwendet, wenn Zahlen eingeben
- **Telefon** &ndash; verwendet, bei der Eingabe von Telefonnummern
- **URL** &ndash; für die Eingabe, Dateipfade und Webadressen verwendet

Ist ein [Beispiel für jede Tastatur](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) in unserer Rezepte-Abschnitt.

### <a name="enabling-and-disabling-spell-checking"></a>Aktivieren und Deaktivieren der Rechtschreibprüfung

Die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft steuert, ob Rechtschreibprüfung aktiviert ist. Standardmäßig ist die Eigenschaft auf festgelegt `true`. Wenn der Benutzer Text eingibt, werden orthographische angegeben.

Für einige Szenarien für das Text-Eintrag, z. B. einen Benutzernamen eingeben Rechtschreibprüfung bietet jedoch ein negatives Erlebnis und daher sollte deaktiviert durch Festlegen der [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Eigenschaft `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Wenn die [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) -Eigenschaftensatz auf `false`, und eine benutzerdefinierte Tastatur nicht verwendet wird, die systemeigene Rechtschreibprüfung deaktiviert. Jedoch wenn eine [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) hat wurde Satz, der deaktiviert die Rechtschreibprüfung zur Überprüfung der z. B. [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), die `IsSpellCheckEnabled` Eigenschaft wird ignoriert. Aus diesem Grund kann nicht die Eigenschaft verwendet werden, aktivieren Sie die Rechtschreibprüfung für eine `Keyboard` , von denen explizit deaktiviert.

### <a name="placeholders"></a>Platzhalter

`Entry` kann festgelegt werden, um Platzhaltertext angezeigt, wenn sie Benutzereingaben nicht gespeichert werden. In der Praxis ist dies häufig in Formularen sehen, um zu verdeutlichen, den Inhalt, der für ein bestimmtes Feld geeignet ist. Textfarbe Platzhalter kann nicht angepasst werden und identisch, unabhängig von der `TextColor` Einstellung. Wenn Ihre Strategie für eine benutzerdefinierte Platzhalter Farbe erfordert, müssen Sie ein ausweichen auf einen [benutzerdefinierten Renderers](). Die folgenden erstellt ein `Entry` mit "Username" als Platzhalter in XAML:

```xaml
<Entry Placeholder="Username" />
```

In C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Beispiel für einen Indexeintrag Platzhalter")

### <a name="password-fields"></a>Kennwortfelder

`Entry` Stellt die `IsPassword` Eigenschaft. Wenn `IsPassword` ist `true`, den Inhalt des Felds werden als schwarze Kreise angezeigt werden:

In XAML:

```xaml
<Entry IsPassword="true" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Beispiel für einen Indexeintrag IsPassword")

Platzhalter können verwendet werden, mit Instanzen von `Entry` als Kennwortfelder konfiguriert sind:

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

Eintrag kann festgelegt werden, um einen benutzerdefinierten Hintergrund und Textfarben über die folgenden bindbaren Eigenschaften verwenden:

- **TextColor** &ndash; legt die Farbe des Texts fest.
- **BackgroundColor** &ndash; legt die Farbe des Texthintergrunds gezeigt fest.

Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden kann können. Da jede Plattform unterschiedliche Standardwerte für Text- und Hintergrundfarben aufweist, müssen Sie häufig sowohl festlegen, wenn Sie eine festlegen.

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

Beachten Sie, dass der Platzhalter nicht betroffen, durch das angegebene `TextColor`.

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

Achten Sie darauf, um sicherzustellen, dass die Hintergrund- und Textfarben gewählten auf jeder Plattform verwendet werden können und keine Platzhaltertext verdecken.

## <a name="events-and-interactivity"></a>Ereignisse und Interaktivität

Eintrag zwei Ereignisse verfügbar gemacht werden:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; ausgelöst, wenn der Text im Eintrag ändert. Stellt den Text an, vor und nach der Änderung.
- [Abgeschlossen](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; ausgelöst, wenn der Benutzer die Eingabe beendet wurde, durch Drücken der EINGABETASTE auf der Tastatur drücken.

### <a name="completed"></a>Abgeschlossen

Die `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit einem Eintrag zu reagieren. `Completed` wird ausgelöst, wenn der Benutzer Eingabe mit einem Feld endet, indem Sie die EINGABETASTE auf der Tastatur eingeben. Der Handler für das Ereignis ist ein allgemeines Ereignis-Handler, dauert des Absenders und `EventArgs`:

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

und c#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

Die `TextChanged` Ereignis wird verwendet, um auf eine Änderung im Inhalt eines Felds zu reagieren.

`TextChanged` wird ausgelöst, wenn die `Text` von der `Entry` ändert. Der Handler für das Ereignis akzeptiert eine Instanz von `TextChangedEventArgs`. `TextChangedEventArgs` ermöglicht den Zugriff auf die alten und neuen Werte von der `Entry` `Text` über die `OldTextValue` und `NewTextValue` Eigenschaften:

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

und c#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Verwandte Links

- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Eintrag-API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
