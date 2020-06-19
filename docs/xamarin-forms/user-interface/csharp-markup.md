---
title: Xamarin.FormsC#-Markup
description: C#-Markup ist ein Opt-in-Satz von fließenden Hilfsmethoden und-Klassen, um den Prozess des Ersatzes von deklarativen Xamarin.Forms Benutzeroberflächen in c# zu vereinfachen.
ms.prod: xamarin
ms.assetid: D41B9DCD-5C34-4C2F-B177-FC082AB2E9E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 939727d3dd8d419cdc020d33d3e7241dcedb8158
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137747"
---
# <a name="xamarinforms-c-markup"></a>Xamarin.FormsC#-Markup

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)

C#-Markup ist ein Opt-in-Satz von fließenden Hilfsmethoden und-Klassen, um den Prozess des Ersatzes von deklarativen Xamarin.Forms Benutzeroberflächen in c# zu vereinfachen. Die von c#-Markup bereitgestellte fließende API ist im- `Xamarin.Forms.Markup` Namespace verfügbar.

Ebenso wie bei XAML ermöglicht c#-Markup eine saubere Trennung zwischen Benutzeroberflächen Markup und UI-Logik. Dies kann erreicht werden, indem Sie das Benutzeroberflächen Markup und die Benutzeroberflächen Logik in verschiedene partielle Klassendateien aufteilen. Beispielsweise würde sich das UI-Markup für eine Anmeldeseite in einer Datei namens *LoginPage.cs*befinden, während sich die Benutzeroberflächen Logik in einer Datei namens *LoginPage.Logic.cs*befinden würde.

C#-Markup ist in Xamarin.Forms 4,6 verfügbar. Es ist jedoch zurzeit experimentell und kann nur verwendet werden, indem der *app.cs* -Datei die folgende Codezeile hinzugefügt wird:

```csharp
Device.SetFlags(new string[]{ "Markup_Experimental" });
```

> [!NOTE]
> C#-Markup ist auf allen Plattformen verfügbar, die von unterstützt werden Xamarin.Forms .

## <a name="basic-example"></a>Einfaches Beispiel

Im folgenden Beispiel wird gezeigt, wie der Seiten Inhalt auf einen neuen festgelegt [`Grid`](xref:Xamarin.Forms.Grid) [`Label`](xref:Xamarin.Forms.Label) wird, der einen und einen [`Entry`](xref:Xamarin.Forms.Entry) in c# enthält:

```csharp
Grid grid = new Grid();

Label label = new Label { Text = "Code: " };
grid.Children.Add(label, 0, 1);

Entry entry = new Entry
{
    Placeholder = "Enter number",
    Keyboard = Keyboard.Numeric,
    BackgroundColor = Color.AliceBlue,
    TextColor = Color.Black,
    FontSize = 15,
    HeightRequest = 44,
    Margin = fieldMargin
};
grid.Children.Add(entry, 0, 2);
Grid.SetColumnSpan(entry, 2);
entry.SetBinding(Entry.TextProperty, new Binding("RegistrationCode"));

Content = grid;
```

In diesem Beispiel [`Grid`](xref:Xamarin.Forms.Grid) wird ein-Objekt mit untergeordneten [`Label`](xref:Xamarin.Forms.Label) -und- [`Entry`](xref:Xamarin.Forms.Entry) Objekten erstellt. Das `Label` zeigt Text an, und die `Entry` Daten werden an die- `RegistrationCode` Eigenschaft von ViewModel gebunden. Jede untergeordnete Ansicht wird so festgelegt, dass Sie in einer bestimmten Zeile in der angezeigt `Grid` wird, und das `Entry` umfasst alle Spalten in der `Grid` . Außerdem wird die Höhe von `Entry` zusammen mit der Tastatur, den Farben, dem Schrift Grad des Texts und dessen festgelegt `Margin` . Zum Schluss wird die- `Page.Content` Eigenschaft auf das-Objekt festgelegt `Grid` .

C#-Markup ermöglicht, dass dieser Code mithilfe seiner fließenden API umgeschrieben wird:

```csharp
using Xamarin.Forms.Markup;
using static Xamarin.Forms.Markup.GridRowsColumns;

Content = new Grid
{
  Children =
  {
    new Label { Text = "Code:" }
               .Row (BodyRow.CodeHeader) .Column (BodyCol.Header),

    new Entry { Placeholder = "Enter number", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
               .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
               .Bind (nameof(vm.RegistrationCode))
  }
}};
```

Dieses Beispiel ist mit dem vorherigen Beispiel identisch, aber die c#-Markup-API vereinfacht den Prozess der Benutzeroberfläche in c#.

> [!NOTE]
> C#-Markup enthält Erweiterungs Methoden, mit denen bestimmte Ansichts Eigenschaften festgelegt werden. Diese Erweiterungs Methoden sollen nicht alle Eigenschaften Setter ersetzen. Stattdessen wurden Sie zur Verbesserung der Lesbarkeit von Code entwickelt und können in Kombination mit Eigenschaften Setter verwendet werden. Es wird empfohlen, immer eine Erweiterungsmethode zu verwenden, wenn eine für eine Eigenschaft vorhanden ist, Sie können aber auch Ihren bevorzugten Saldo auswählen.

## <a name="data-binding"></a>Datenbindung

C#-Markup enthält eine `Bind` -Erweiterungsmethode sowie über Ladungen, die eine Datenbindung zwischen einer Eigenschaft für die binable-Eigenschaft und einer angegebenen Eigenschaft erstellt. Die- `Bind` Methode kennt die standardmäßige bindbare Eigenschaft für die Mehrzahl der Steuerelemente, die in enthalten sind Xamarin.Forms . Daher ist es in der Regel nicht notwendig, die Ziel Eigenschaft anzugeben, wenn diese Methode verwendet wird. Sie können jedoch auch die bindbare Standard Eigenschaft für zusätzliche Steuerelemente registrieren:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.Register(HoverButton.CommandProperty, RadialGauge.ValueProperty);
```

Die- `Bind` Methode kann verwendet werden, um eine Bindung an eine beliebige bindbare Eigenschaft herzustellen:

```csharp
using Xamarin.Forms.Markup;
// ...

new Label { Text = "No data available" }
           .Bind (Label.IsVisibleProperty, nameof(vm.Empty))
```

Außerdem `BindCommand` kann die Erweiterungsmethode eine Bindung an die Standard-und-Eigenschaften eines Steuer Elements `Command` `CommandParameter` in einem einzelnen Methoden aufzurufen:

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap me" }
              .BindCommand (nameof(vm.TapCommand))
```

Standardmäßig ist der `CommandParameter` an den Bindungs Kontext gebunden. Sie können auch den Bindungs Pfad und die Quelle für die `Command` Bindungen und angeben `CommandParameter` :

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap Me" }
              .BindCommand (nameof(vm.TapCommand), vm, nameof(Item.Id))
```

In diesem Beispiel ist der Bindungs Kontext eine `Item` Instanz, sodass Sie keine Quelle für die Bindung angeben müssen `Id` `CommandParameter` .

Wenn Sie nur an binden müssen `Command` , können Sie `null` an das- `parameterPath` Argument der-Methode übergeben `BindCommand` . Verwenden Sie alternativ die- `Bind` Methode.

Sie können auch die Standard `Command` -und- `CommandParameter` Eigenschaften für zusätzliche Steuerelemente registrieren:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.RegisterCommand(
    (CustomViewA.CommandProperty, CustomViewA.CommandParameterProperty),
    (CustomViewB.CommandProperty, CustomViewB.CommandParameterProperty)
);
```

Inline Konvertercode kann `Bind` mit den `convert` Parametern und an die-Methode übermittelt werden `convertBack` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth),
                  convert: (int depth) => new Thickness(depth * 20, 0, 0, 0))
```

Typsichere Konverterparameter werden ebenfalls unterstützt:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { }
           .Bind (nameof(viewModel.Text),
                  convert: (string text, int repeat) => string.Concat(Enumerable.Repeat(text, repeat)))
```

Außerdem können Konvertercode und-Instanzen mit der-Klasse wieder verwendet werden `FuncConverter` :

```csharp
using Xamarin.Forms.Markup;
//...

FuncConverter<int, Thickness> treeMarginConverter = new FuncConverter<int, Thickness>(depth => new Thickness(depth * 20, 0, 0, 0));
new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth), converter: treeMarginConverter),
```

Die- `FuncConverter` Klasse unterstützt auch- `CultureInfo` Objekte:

```csharp
using Xamarin.Forms.Markup;
//...

cultureAwareConverter = new FuncConverter<DateTimeOffset, string, int>(
    (date, daysToAdd, culture) => date.AddDays(daysToAdd).ToString(culture)
);
```

Es ist auch möglich, Daten an Objekte zu binden `Span` , die mit der-Eigenschaft angegeben werden `FormattedText` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { } .FormattedText (
    new Span { Text = "Built with " },
    new Span { TextColor = Color.Blue, TextDecorations = TextDecorations.Underline }
              .BindTapGesture (nameof(vm.ContinueToCSharpForMarkupCommand))
              .Bind (nameof(vm.Title))
)
```

### <a name="gesture-recognizers"></a>Gestenerkennung

`Command`die-Eigenschaft und `CommandParameter` die-Eigenschaft `GestureElement` können `View` mithilfe der `BindClickGesture` `BindSwipeGesture` Erweiterungs Methoden, und an-und-Typen gebunden werden `BindTapGesture` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .BindTapGesture (nameof(vm.TapCommand))
```

In diesem Beispiel wird eine Gestenerkennung des angegebenen Typs erstellt und dem hinzugefügt [`Label`](xref:Xamarin.Forms.Label) . Die `Bind*Gesture` Erweiterungs Methoden bieten dieselben Parameter wie die `BindCommand` Erweiterungs Methoden. In der Standardeinstellung wird jedoch `Bind*Gesture` nicht gebunden `CommandParameter` `BindCommand` .

Verwenden Sie die `ClickGesture` `PanGesture` Erweiterungs Methoden,,, und, um eine Gestenerkennung mit Parametern zu initialisieren `PinchGesture` `SwipeGesture` `TapGesture` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .TapGesture (g => g.Bind(nameof(vm.DoubleTapCommand)).NumberOfTapsRequired = 2)
```

Da eine Gestenerkennung eine ist `BindableObject` , können Sie die `Bind` -und- `BindCommand` Erweiterungs Methoden verwenden, wenn Sie Sie initialisieren. Sie können auch benutzerdefinierte Gesten Erkennungs Typen mit der- `Gesture<TGestureElement, TGestureRecognizer>` Erweiterungsmethode initialisieren.

## <a name="layout"></a>Layout

C#-Markup umfasst eine Reihe von layouterweiterungsmethoden, die das Positionieren von Sichten in Layouts und den Inhalt in Sichten unterstützen:

| type | Erweiterungsmethoden |
|---|---|
| `FlexLayout` | `AlignSelf`, `Basis`, `Grow`, `Menu`, `Order`, `Shrink` |
| `Grid` | `Row`, `Column`, `RowSpan`, `ColumnSpan` |
| `Label` | `TextLeft`, `TextCenterHorizontal`, `TextRight` <br/> `TextTop`, `TextCenterVertical`, `TextBottom` <br/> `TextCenter` |
| `Layout` | `Padding`, `Paddings` |
| `LayoutOptions` | `Left`, `CenterHorizontal`, `FillHorizontal`, `Right` <br/> `LeftExpand`, `CenterExpandHorizontal`, `FillExpandHorizontal`, `RightExpand` <br /> `Top`, `Bottom`, `CenterVertical`, `FillVertical` <br /> `TopExpand`, `BottomExpand`, `CenterExpandVertical`, `FillExpandVertical` <br /> `Center`, `Fill`, `CenterExpand`, `FillExpand` |
| `View` | `Margin`, `Margins` |
| `VisualElement` | `Height`, `Width`, `MinHeight`, `MinWidth`, `Size`, `MinSize` |

### <a name="left-to-right-and-right-to-left-support"></a>Von links nach rechts und von rechts nach Links unterstützt

Für c#-Markup, das entweder die Fluss Richtung von links nach rechts (LTR) oder von rechts nach links (RTL) unterstützen soll, bieten die oben aufgeführten Erweiterungs Methoden die intuitivste Gruppe von Namen `Left` : `Right` , `Top` und `Bottom` .

Um den korrekten Satz von linken und rechten Erweiterungs Methoden verfügbar zu machen, und in dem Prozess explizit die Fluss Richtung, für die das Markup entworfen wurde, können Sie eine der folgenden beiden `using` Direktiven einschließen: `using Xamarin.Forms.Markup.LeftToRight;` , oder `using Xamarin.Forms.Markup.RightToLeft;` .

Für c#-Markup, das sowohl von links nach rechts und von rechts nach links Fluss Richtung unterstützt werden soll, empfiehlt es sich, die Erweiterungs Methoden in der folgenden Tabelle anstelle eines der obigen Namespaces zu verwenden:

| type | Erweiterungsmethoden |
|---|---|
| `Label` | `TextStart`, `TextEnd` |
| `LayoutOptions` | `Start`, `End` <br/> `StartExpand`, `EndExpand` |

### <a name="layout-line-convention"></a>Layoutlinienkonvention

Die empfohlene Konvention besteht darin, alle layouterweiterungs-Methoden für eine Ansicht in einer einzelnen Zeile in der folgenden Reihenfolge zu platzieren:

1. Die Zeile und die Spalte, die die Sicht enthalten.
1. Ausrichtung innerhalb der Zeile und der Spalte.
1. Ränder um die Ansicht.
1. Ansichts Größe.
1. Abstand in der Ansicht.
1. Inhalts Ausrichtung innerhalb des Padding.

Der folgende Code zeigt ein Beispiel für diese Konvention:

```csharp
new Label { }
           .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal () // Layout line
```

Durch konsistente Nachverfolgung der Konvention können Sie das c#-Markup schnell lesen und eine mentale Karte erstellen, in der sich der Ansichts Inhalt in der Benutzeroberfläche befindet.

## <a name="grid-rows-and-columns"></a>Zeilen und Spalten im Raster

Enumerationen können verwendet werden, um [`Grid`](xref:Xamarin.Forms.Grid) Zeilen und Spalten zu definieren, anstatt Zahlen zu verwenden. Dies bietet den Vorteil, dass die Umbenennung beim Hinzufügen oder Entfernen von Zeilen oder Spalten nicht erforderlich ist.

> [!IMPORTANT]
> Zum Definieren von [`Grid`](xref:Xamarin.Forms.Grid) Zeilen und Spalten mithilfe von Enumerationen ist die folgende `using` Direktive erforderlich:`using static Xamarin.Forms.Markup.GridRowsColumns;`

Der folgende Code zeigt ein Beispiel für das definieren und Verwenden von [`Grid`](xref:Xamarin.Forms.Grid) Zeilen und Spalten mithilfe von Enumerationen:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms.Markup.LeftToRight;
using static Xamarin.Forms.Markup.GridRowsColumns;
// ...

enum BodyRow
{
    Prompt,
    CodeHeader,
    CodeEntry,
    Button
}

enum BodyCol
{
    FieldLabel,
    FieldValidation
}

View Build() => new Grid
{
    RowDefinitions = Rows.Define(
        (BodyRow.Prompt    , 170 ),
        (BodyRow.CodeHeader, 75  ),
        (BodyRow.CodeEntry , Auto),
        (BodyRow.Button    , Auto)
    ),

    ColumnDefinitions = Columns.Define(
        (BodyCol.FieldLabel     , 160 ),
        (BodyCol.FieldValidation, Star)
    ),

    Children =
    {
        new Label { LineBreakMode = LineBreakMode.WordWrap } .Font (15) .Bold ()
                   .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal ()
                   .Bind (nameof(vm.RegistrationPrompt)),

        new Label { Text = "Registration code" } .Bold ()
                   .Row (BodyRow.CodeHeader) .Column(BodyCol.FieldLabel) .Bottom () .Margin (fieldNameMargin),

        new Label { } .Italic ()
                   .Row (BodyRow.CodeHeader) .Column (BodyCol.FieldValidation) .Right () .Bottom () .Margin (fieldNameMargin)
                   .Bind (nameof(vm.RegistrationCodeValidationMessage)),

        new Entry { Placeholder = "E.g. 123456", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                   .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                   .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay),

        new Button { Text = "Verify" } .Style (FilledButton)
                    .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (PageMarginSize)
                    .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode))
                    .Bind (nameof(vm.VerifyRegistrationCodeCommand)),
    }
};
```

Außerdem können Sie Zeilen und Spalten ohne Enumerationen kurz definieren:

```csharp
new Grid
{
    RowDefinitions = Rows.Define (Auto, Star, 20),
    ColumnDefinitions = Columns.Define (Auto, Star, 20, 40)
    // ...
}
```

## <a name="fonts"></a>Fonts

Mit den Steuerelementen in der folgenden Liste können `FontSize` die `Bold` `Italic` -,-,-und-Erweiterungs Methoden aufgerufen werden, `Font` um die Darstellung des vom-Steuerelement angezeigten Texts festzulegen:

- `Button`
- `DatePicker`
- `Editor`
- `Entry`
- `Label`
- `Picker`
- `SearchBar`
- `Span`
- `TimePicker`

## <a name="effects"></a>Effekte

Effekte können mit der-Erweiterungsmethode an Steuerelemente angefügt werden `Effect` :

```csharp
using Xamarin.Forms.Markup;
// ...

new Button { Text = "Tap Me" }
            .Effects (new ButtonMixedCaps())
```

## <a name="logic-integration"></a>Logik Integration

Die- `Invoke` Erweiterungsmethode kann verwendet werden, um Code Inline in Ihrem c#-Markup auszuführen:

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Invoke (l => l.ItemTapped += OnListViewItemTapped)
```

Darüber hinaus können Sie die- `Assign` Erweiterungsmethode verwenden, um von außerhalb des Benutzeroberflächen Markups (in der Benutzeroberflächen-Logik Datei) auf ein Steuerelement zuzugreifen:

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Assign (out MyListView)
```

## <a name="styles"></a>Stile

Im folgenden Beispiel wird gezeigt, wie implizite und explizite Stile mit c#-Markup erstellt werden:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms;

namespace CSharpForMarkupDemos
{
    public static class Styles
    {
        static Style<Button> buttons, filledButton;
        static Style<Label> labels;
        static Style<Span> link;

        #region Implicit styles

        public static ResourceDictionary Implicit => new ResourceDictionary { Buttons, Labels };

        public static Style<Button> Buttons => buttons ?? (buttons = new Style<Button>(
            (Button.HeightRequestProperty, 44),
            (Button.FontSizeProperty, 13),
            (Button.HorizontalOptionsProperty, LayoutOptions.Center),
            (Button.VerticalOptionsProperty, LayoutOptions.Center)
        ));

        public static Style<Label> Labels => labels ?? (labels = new Style<Label>(
            (Label.FontSizeProperty, 13),
            (Label.TextColorProperty, Color.Black)
        ));

        #endregion Implicit styles

        #region Explicit styles

        public static Style<Button> FilledButton => filledButton ?? (filledButton = new Style<Button>(
            (Button.TextColorProperty, Color.White),
            (Button.BackgroundColorProperty, Color.FromHex("#1976D2")),
            (Button.CornerRadiusProperty, 5)
        )).BasedOn(Buttons);

        public static Style<Span> Link => link ?? (link = new Style<Span>(
            (Span.TextColorProperty, Color.Blue),
            (Span.TextDecorationsProperty, TextDecorations.Underline)
        ));

        #endregion Explicit styles
    }
}
```

Die impliziten Stile können verarbeitet werden, indem Sie in das Anwendungs Ressourcen Wörterbuch geladen werden:

```csharp
public App()
{
    Resources = Styles.Implicit;
    // ...
}
```

Explizite Stile können mit der- `Style` Erweiterungsmethode verwendet werden.

```csharp
using static CSharpForMarkupExample.Styles;
// ...

new Button { Text = "Tap Me" } .Style (FilledButton),
```

> [!NOTE]
> Zusätzlich zur `Style` Erweiterungsmethode gibt es auch die `ApplyToDerivedTypes` `BasedOn` Erweiterungs Methoden,, `Add` und `CanCascade` .

Alternativ können Sie eigene Stil Erweiterungs Methoden erstellen:

```csharp
public static TButton Filled<TButton>(this TButton button) where TButton : Button
{
    button.Buttons(); // Equivalent to Style .BasedOn (Buttons)
    button.TextColor = Color.White;
    button.BackgroundColor = Color.Red;
    return button;
}
```

Die `Filled` Erweiterungsmethode kann dann wie folgt verarbeitet werden:

```csharp
new Button { Text = "Tap Me" } .Filled ()
```

## <a name="platform-specifics"></a>Plattformeigenschaften

Die `Invoke` Erweiterungsmethode kann verwendet werden, um Platt Form Besonderheiten anzuwenden. Um mehrdeutigkeits Fehler zu vermeiden, sollten Sie jedoch keine `using` Direktiven für die `Xamarin.Forms.PlatformConfiguration.*Specific` Namespaces direkt einschließen. Erstellen Sie stattdessen einen Namespacealias, und verwenden Sie den plattformspezifischen über den Alias:

```csharp
using Xamarin.Forms.Markup;
using PciOS = Xamarin.Forms.PlatformConfiguration.iOSSpecific;
// ...

new ListView { } .Invoke (l => PciOS.ListView.SetGroupHeaderStyle(l, PciOS.GroupHeaderStyle.Grouped))
```

Wenn Sie bestimmte Platt Form Besonderheiten häufig nutzen, können Sie darüber hinaus fließende Erweiterungs Methoden für Sie in ihrer eigenen Erweiterungs Klasse erstellen:

```csharp
public static T iOSGroupHeaderStyle<T>(this T listView, PciOS.GroupHeaderStyle style) where T : Forms.ListView
{
  PciOS.ListView.SetGroupHeaderStyle(listView, style);
  return listView;
}
```

Die Erweiterungsmethode kann dann wie folgt verarbeitet werden:

```csharp
new ListView { } .iOSGroupHeaderStyle(PciOS.GroupHeaderStyle.Grouped)
```

Weitere Informationen zu Platt Form Besonderheiten finden Sie unter Funktionen der [Android-Plattform](~/xamarin-forms/platform/android/index.md), [IOS-Platt Form Features](~/xamarin-forms/platform/ios/index.md)und [Features der Windows-Plattform](~/xamarin-forms/platform/windows/index.md).

## <a name="recommended-convention"></a>Empfohlene Konvention

Eine empfohlene Reihenfolge und Gruppierung von Eigenschaften und Hilfsmethoden ist:

- **Zweck**: alle Eigenschaften-oder Hilfsmethoden, deren Wert den Zweck des Steuer Elements identifiziert (z. b., `Text` `Placeholder` ,. `Assign` ).
- **Sonstiges**: alle Eigenschaften oder Hilfsmethoden, die nicht Layout oder Bindung sind, in derselben Zeile oder in mehreren Zeilen.
- **Layout**: das Layout wird instand angeordnet: Zeilen und Spalten, Layoutoptionen, Rand, Größe, Padding und Inhalts Ausrichtung.
- **Bind**: die Datenbindung wird am Ende der Methoden Kette mit einer gebundenen Eigenschaft pro Zeile ausgeführt. Wenn die bindbare *Standard* Eigenschaft gebunden ist, sollte Sie sich am Ende der Methoden Kette befinden.

Der folgende Code zeigt ein Beispiel für die folgenden Konventionen:

```csharp
new Button { Text = "Verify" /* purpose */ } .Style (FilledButton) // other
            .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (10) // layout
            .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode)) // bind
            .Bind (nameof(vm.VerifyRegistrationCodeCommand)), // bind default

new Label { }
           .Assign (out animatedMessageLabel) // purpose
           .Invoke (label => label.SizeChanged += MessageLabel_SizeChanged) // other
           .Row (BodyRow.Message) .ColumnSpan (All<BodyCol>()) // layout
           .Bind (nameof(vm.Message)), // bind default
```

Wenn Sie diese Konvention konsistent anwenden, können Sie Ihr c#-Markup schnell scannen und ein geistiges Bild des Benutzeroberflächen Layouts erstellen.

## <a name="related-links"></a>Verwandte Links

- [Csharpformarkupdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)
- [Funktionen der Android-Plattform](~/xamarin-forms/platform/android/index.md)
- [IOS-Platt Form Features](~/xamarin-forms/platform/ios/index.md)
- [Features der Windows-Plattform](~/xamarin-forms/platform/windows/index.md)
