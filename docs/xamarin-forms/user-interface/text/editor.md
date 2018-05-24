---
title: Editor
description: Für mehrzeilige Texteingabe
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 035365a22c487039ff811756d91ca0a8d392d628
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2018
---
# <a name="editor"></a>Editor

_Für mehrzeilige Texteingabe_

Die `Editor` Steuerelement wird verwendet, um die mehrzeilige Eingabe akzeptieren. Dieser Artikel umfasst folgende Themen:

- **[Anpassung](#customization)**  &ndash; Tastatur und Farboptionen.
- **[Interaktivität](#interactivity)**  &ndash; Ereignisse, die für überwacht werden können, um Interaktivität bereitzustellen.

## <a name="customization"></a>Anpassung

### <a name="setting-and-reading-text"></a>Festlegen und Lesen von Text

Wie andere Sichten darstellen von Text-Editor macht die `Text` Eigenschaft. `Text` kann verwendet werden, um festzulegen, und Lesen Sie den Text, präsentiert von der `Editor`. Im folgende Beispiel wird veranschaulicht, Text in XAML festlegen:

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

### <a name="keyboards"></a>Tastaturen

Der Tastatur, die angezeigt werden, wenn Benutzer interagieren ein `Editor` programmgesteuert festgelegt werden können, über die [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) Eigenschaft.

Die Optionen für die Tastaturtyp sind:

- **Standardmäßige** &ndash; der Standardtastatur
- **Chat** &ndash; für Texte & Stellen verwendet, in denen Emoji eignen sich
- **E-Mail** &ndash; verwendet, wenn e-Mail-Adressen eingeben.
- **Numerische** &ndash; verwendet, wenn Zahlen eingeben
- **Telefon** &ndash; verwendet, bei der Eingabe von Telefonnummern
- **URL** &ndash; für die Eingabe Dateipfade & Webadressen verwendet

Ist ein [Beispiel für jede Tastatur](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) in unserer Rezepte-Abschnitt.

### <a name="colors"></a>Farben

`Editor` kann festgelegt werden, um eine benutzerdefinierte Hintergrundfarbe über verwenden die `BackgroundColor` Eigenschaft. Besondere Sorgfalt ist erforderlich, um sicherzustellen, dass Farben auf jeder Plattform verwendet werden kann können. Da jede Plattform unterschiedliche Standardwerte für Farbe aufweist, müssen Sie eine benutzerdefinierte Hintergrundfarbe für jede Plattform festzulegen. Finden Sie unter [arbeiten mit Plattform Optimierungsmethoden](~/xamarin-forms/platform/device.md) für Weitere Informationen zum Optimieren der Benutzeroberflächenautomatisierungs für jede Plattform.

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

![](editor-images/textbackgroundcolor.png "-Editor mit BackgroundColor-Beispiel")

Stellen Sie sicher, dass die Hintergrund- und Textfarben gewählten auf jeder Plattform verwendet werden können und keine Platzhaltertext verdecken.

## <a name="interactivity"></a>Interaktivität

`Editor` zwei Ereignisse verfügbar gemacht werden:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; ausgelöst, wenn der Text im Editor geändert wird. Stellt den Text an, vor und nach der Änderung.
- [Abgeschlossen](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; ausgelöst, wenn der Benutzer die Eingabe beendet wurde, durch Drücken der EINGABETASTE auf der Tastatur drücken.

### <a name="completed"></a>Abgeschlossen

Die `Completed` Ereignis wird verwendet, um auf den Abschluss einer Interaktion mit zu reagieren ein `Editor`. `Completed` wird ausgelöst, wenn der Benutzer Eingabe mit einem Feld endet, indem Sie die EINGABETASTE auf der Tastatur eingeben. Der Handler für das Ereignis ist ein allgemeines Ereignis-Handler, dauert des Absenders und `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Abgeschlossene Event kann im Code und die Verwendung von XAML-abonniert werden:

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

Die `TextChanged` Ereignis wird verwendet, um auf eine Änderung im Inhalt eines Felds zu reagieren.

`TextChanged` wird ausgelöst, wenn die `Text` von der `Editor` ändert. Der Handler für das Ereignis akzeptiert eine Instanz von `TextChangedEventArgs`. `TextChangedEventArgs` ermöglicht den Zugriff auf die alten und neuen Werte von der `Editor` `Text` über die `OldTextValue` und `NewTextValue` Eigenschaften:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Abgeschlossene Event kann im Code und die Verwendung von XAML-abonniert werden:

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
- [Editor-API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
