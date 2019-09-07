---
title: Einführung in MonoTouch. Dialog für xamarin. IOS
description: In diesem Dokument wird MonoTouch. Dialog (MT) beschrieben. D), ein Framework für die schnelle, deklarative UI-Entwicklung mit xamarin. IOS. Es wird erläutert, wie Sie mit den MonoTouch. Dialog-APIs eine Schnittstelle im Code oder JSON erstellen und Features wie die Pull-to-Refresh-Funktion, die Suche, das Laden des Hintergrund Bilds usw. verwenden.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: conceptdev
ms.author: crdun
ms.openlocfilehash: cbe1b374b97f64e0c28b2f89ca9f6d510511b74d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768843"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Einführung in MonoTouch. Dialog für xamarin. IOS

MonoTouch. Dialog, wird als "MT" bezeichnet. D kurz gesagt, ist ein schnelles UI Development Toolkit, das Entwicklern das Erstellen von Anwendungs Bildschirmen und der Navigation mithilfe von Informationen ermöglicht, anstatt das aufwändigen Schritte der Erstellung von Ansichts Controllern, Tabellen usw. Dadurch wird eine deutliche Vereinfachung der Benutzeroberflächen Entwicklung und der Code Reduzierung ermöglicht. Sehen Sie sich beispielsweise den folgenden Screenshot an:

 [![](images/image1.png "Sehen Sie sich beispielsweise den folgenden Screenshot an:")](images/image1.png#lightbox)

Der folgende Code wurde verwendet, um den gesamten Bildschirm zu definieren:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Beim Arbeiten mit Tabellen in IOS gibt es häufig eine Menge wiederholten Codes.
Wenn z. b. eine Tabelle benötigt wird, wird eine Datenquelle benötigt, um diese Tabelle aufzufüllen. In einer Anwendung, die über zwei Tabellen basierte Bildschirme verfügt, die über einen Navigations Controller verbunden sind, gibt jeder Bildschirm einen großen Teil des gleichen Codes aus.

UV. D vereinfacht dies, indem der gesamte Code in einer generischen API für die Tabellenerstellung gekapselt wird. Anschließend wird eine Abstraktion über diese API bereitstellt, die eine deklarative Objekt Bindungs Syntax ermöglicht, mit der es noch einfacher wird. Daher stehen in Mt zwei APIs zur Verfügung. D

- **Low-Level-Element-API** – die *Elements-API* basiert auf der Erstellung einer hierarchalen Struktur von Elementen, die Bildschirme und deren Komponenten darstellen. Die Elements-API bietet Entwicklern die flexibelste Flexibilität und Kontrolle bei der Erstellung von Benutzeroberflächen. Außerdem bietet die Elements-API erweiterte Unterstützung für deklarative Definitionen über JSON, was sowohl eine unglaublich schnelle Deklaration als auch die dynamische Generierung von Benutzeroberflächen von einem Server ermöglicht. 
- **Allgemeine Reflektions-API** – auch bekannt als die *Bindung* *API* , in die Klassen mit Benutzeroberflächen-Hinweise, und klicken Sie dann masterziel versehen sind D erstellt Bildschirme, die auf der Grundlage der Objekte automatisch und stellt eine Bindung zwischen dem, was angezeigt (und optional bearbeitet) auf dem Bildschirm, und die zugrunde liegende Objekt sichern. Im obigen Beispiel wird die Verwendung der Reflection-API veranschaulicht. Diese API bietet nicht die differenzierte Steuerung, die die Elements-API leistet, aber Sie verringert die Komplexität noch weiter, indem die Element Hierarchie auf der Grundlage von Klassen Attributen automatisch aufgebaut wird. 

UV. D ist mit einem großen Satz integrierter Benutzeroberflächen Elemente für die Bildschirm Erstellung verpackt, aber auch die Notwendigkeit für angepasste Elemente und erweiterte Bildschirmlayouts wird erkannt. Daher ist die Erweiterbarkeit eine erste Klasse, die in der API integriert ist. Entwickler können die vorhandenen Elemente erweitern oder neue erstellen und dann nahtlos integrieren.

Außerdem wird Mt. D verfügt über eine Reihe von gängigen IOS-UX-Features, wie z. b. "Pull-to-refresh"-Unterstützung, Asynchrones Laden von Bildern und Suchunterstützung.

Dieser Artikel bietet einen umfassenden Einblick in die Arbeit mit Mt. D, einschließlich:

- **Mt. D-Komponenten** – dies konzentriert sich auf das Verständnis der Klassen, die MT bilden. D, um den schnellen Einstieg zu ermöglichen. 
- **Element Referenz** – eine umfassende Liste der integrierten Elemente von Mt. D. 
- **Erweiterte Verwendung** – Dies umfasst erweiterte Funktionen, wie z. b. Pull-to-refresh, Search, das Laden von Hintergrundbildern, die Verwendung von LINQ zum Erstellen von Element Hierarchien und das Erstellen benutzerdefinierter Elemente, Zellen und Controller für die Verwendung mit Mt. D. 

## <a name="setting-up-mtd"></a>Einrichten von Mt. D

UV. D wird mit xamarin. IOS verteilt. Um es zu verwenden, klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** eines xamarin. IOS-Projekts in Visual Studio 2017 oder Visual Studio für Mac, und fügen Sie einen Verweis auf die Assembly **MonoTouch. Dialog-1** hinzu. Fügen Sie dann `using MonoTouch.Dialog` ggf. Anweisungen im Quellcode hinzu.

## <a name="understanding-the-pieces-of-mtd"></a>Grundlegendes zu den Teilen von Mt. D

Selbst bei Verwendung der reflektionserapi, Mt. D erstellt eine Element Hierarchie, so als ob Sie direkt über die Elements-API erstellt würde. Außerdem werden in der JSON-Unterstützung, die im vorherigen Abschnitt erwähnt wurde, auch-Elemente erstellt. Aus diesem Grund ist es wichtig, dass Sie über grundlegende Kenntnisse der Bestandteile von MT verfügen. D.

UV. D erstellt Bildschirme mit den folgenden vier Teilen:

- **DialogViewController**
- **RootElement**
- **Bereich**
- **Element**

### <a name="dialogviewcontroller"></a>DialogViewController

Ein *dialogviewcontroller*(kurz *DVC* ) erbt von `UITableViewController` und stellt daher einen Bildschirm mit einer Tabelle dar. Dvcs können wie ein regulärer uitableviewcontroller auf einen Navigations Controller geschoben werden.

### <a name="rootelement"></a>RootElement

Ein *rootelements* ist der Container der obersten Ebene für die Elemente, die zu einem DVC wechseln. Sie enthält Abschnitte, die dann Elemente enthalten können. Rootelements werden nicht gerendert. Stattdessen sind Sie einfach Container für das, was tatsächlich gerendert wird. Ein rootelements ist einem DVC zugewiesen, und dann rendert das DVC seine untergeordneten Elemente.

### <a name="section"></a>Abschnitt

Bei einem Abschnitt handelt es sich um eine Gruppe von Zellen in einer Tabelle. Wie bei einem normalen Tabellen Abschnitt können Sie optional auch eine Kopf-und Fußzeile enthalten, die entweder Text oder sogar benutzerdefinierte Ansichten sein kann, wie im folgenden Screenshot gezeigt:

 [![](images/image2.png "Ebenso wie bei einem normalen Tabellen Abschnitt kann es optional eine Kopf-und Fußzeile geben, die entweder Text oder sogar benutzerdefinierte Ansichten sein kann, wie in diesem Screenshot gezeigt.")](images/image2.png#lightbox)

### <a name="element"></a>Element

Ein-Element stellt eine tatsächliche Zelle in der Tabelle dar. UV. D ist mit einer Vielzahl von Elementen verpackt, die unterschiedliche Datentypen oder andere Eingaben darstellen. Die folgenden Screenshots veranschaulichen z. b. einige verfügbare Elemente:

 [![](images/image3.png "Diese Screenshots veranschaulichen z. b. einige der verfügbaren Elemente.")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Weitere Informationen zu Abschnitten und rootelements

Wir besprechen nun die rootelements und Abschnitte ausführlicher.

### <a name="rootelements"></a>Rootelements

Mindestens ein rootelements ist erforderlich, um den MonoTouch. Dialog-Prozess zu starten.

Wenn ein RootElement mit einem Abschnitts-/Elementwert initialisiert wird, wird dieser Wert verwendet, um nach einem untergeordneten Element zu suchen, das eine Zusammenfassung der Konfiguration bereitstellt, die auf der rechten Seite der Anzeige gerendert wird. Der folgende Screenshot zeigt z. b. eine Tabelle auf der linken Seite mit einer Zelle, die den Titel des Detail Bildschirms auf der rechten Seite, "Dessert", zusammen mit dem Wert der ausgewählten Wüste enthält.

 [![](images/image4.png "Dieser Screenshot zeigt eine Tabelle auf der linken Seite mit einer Zelle, die mit dem Titel des Detail-Bildschirm, auf der rechten Seite, Dessert, zusammen mit dem Wert von der ausgewählten Desert")](images/image4.png#lightbox) [![](images/image5.png "dies folgende Screenshot zeigt eine Tabelle auf der linken Seite mit einer Zelle, die mit dem Titel des Detail-Bildschirm, auf der rechten Seite, Dessert, zusammen mit dem Wert von der ausgewählten desert")](images/image5.png#lightbox)

Stamm Elemente können auch in Abschnitten verwendet werden, um das Laden einer neuen geschachtelten Konfigurationsseite, wie oben gezeigt, zu initiieren. Bei Verwendung in diesem Modus wird die angegebene Beschriftung verwendet, während Sie in einem Abschnitt gerendert wird, und wird auch als Titel für die Unterseite verwendet. Zum Beispiel:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner") {
        new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
            new Section () {
                new RadioElement ("Ice Cream", "dessert"),
                new RadioElement ("Milkshake", "dessert"),
                new RadioElement ("Chocolate Cake", "dessert")
            }
        }
    }
};
```

Wenn der Benutzer im obigen Beispiel auf "Nachtisch" tippt, erstellt das Dialog Feld "MonoTouch. Dialog" eine neue Seite und navigiert mit dem Stammverzeichnis "Dessert" und verfügt über eine Optionsgruppe mit drei Werten.

In diesem speziellen Beispiel wählt die Optionsgruppe "Chocolate Cake" im Abschnitt "Dessert" aus, da der Wert "2" an die Radiogruppe weitergegeben wurde. Dies bedeutet, dass das dritte Element in der Liste ausgewählt wird (null-Index).

Durch Aufrufen der Add-Methode oder C# mithilfe der Syntax von 4 Initialisierern werden Abschnitte hinzugefügt.
Die Insert-Methoden werden zum Einfügen von Abschnitten mit einer Animation bereitgestellt.

Wenn Sie das rootelative-Element mit einer Gruppeninstanz (anstelle einer Radiogruppe) erstellen, ist der zusammenfassende Wert von rootelative, wenn er in einem Abschnitt angezeigt wird, die kumulierte Anzahl aller booleanelements und checkboxelements, die den gleichen Schlüssel wie der Group. Key-Wert aufweisen.

### <a name="sections"></a>Abschnitte

Abschnitte werden zum Gruppieren von Elementen auf dem Bildschirm verwendet, und Sie sind die einzigen gültigen direkten untergeordneten Elemente des rootelements. Abschnitte können beliebige Standardelemente enthalten, einschließlich neuer rootelements.

In einem Abschnitt eingebettete rootelements-Elemente werden verwendet, um zu einer neuen tieferen Ebene zu navigieren.

Abschnitte können Kopf-und Fußzeilen als Zeichen folgen oder als UIViews aufweisen.
In der Regel verwenden Sie nur die Zeichen folgen, aber um benutzerdefinierte Benutzeroberflächen zu erstellen, können Sie beliebige UIView als Kopf-oder Fußzeile verwenden. Sie können eine Zeichenfolge verwenden, um Sie wie folgt zu erstellen:

```csharp
var section = new Section ("Header", "Footer");
```

Wenn Sie Ansichten verwenden möchten, übergeben Sie einfach die Ansichten an den Konstruktor:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header);
```

### <a name="getting-notified"></a>Benachrichtigung erhalten

#### <a name="handling-nsaction"></a>Behandeln von nsaction

UV. D zeigt ein `NSAction` als Delegat für die Behandlung von Rückrufen an.
Angenommen, Sie möchten ein Berührungs Ereignis für eine Tabellenzelle verarbeiten, die von MT erstellt wurde. D. Beim Erstellen eines Elements mit Mt. D, geben Sie einfach eine Rückruffunktion an, wie unten dargestellt:

```csharp
new Section () {
    new StringElement ("Demo Callback", delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Abrufen des Element Werts

In Kombination `Element.Value` mit der-Eigenschaft kann der Rückruf den Wert abrufen, der in anderen Elementen festgelegt ist. Beachten Sie z. B. folgenden Code:

```csharp
var element = new EntryElement (task.Name, "Enter task description", task.Description);
                
var taskElement = new RootElement (task.Name) {
    new Section () { element },
    new Section () { new DateElement ("Due Date", task.DueDate) },
    new Section ("Demo Retrieving Element Value") {
        new StringElement ("Output Task Description", delegate { Console.WriteLine (element.Value); })
    }
};
```

Mit diesem Code wird eine Benutzeroberfläche erstellt, wie unten gezeigt. Eine umfassende Exemplarische Vorgehensweise für dieses Beispiel finden Sie im Tutorial zu den [Elementen API Exemplarische Vorgehensweise](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) .

 [![](images/image6.png "In Kombination mit der Element. Value-Eigenschaft kann der Rückruf den Wert abrufen, der in anderen Elementen festgelegt ist.")](images/image6.png#lightbox)

Wenn der Benutzer die untere Tabellenzelle drückt, wird der Code in der anonymen Funktion ausgeführt, wobei der Wert aus `element` der-Instanz in das **Anwendungs Ausgabe** Pad in Visual Studio für Mac geschrieben wird.

## <a name="built-in-elements"></a>Integrierte Elemente

UV. D enthält eine Reihe integrierter Tabellenzellen Elemente, die als-Elemente bezeichnet werden.
Diese Elemente werden verwendet, um eine Vielzahl unterschiedlicher Typen in Tabellenzellen anzuzeigen, wie z. b. Zeichen folgen, Gleit Komma Zahlen, Datumsangaben und sogar Bilder, um nur ein paar zu benennen. Jedes Element übernimmt die Anzeige des Datentyps entsprechend. Ein boolesches Element zeigt z. b. einen Schalter zum Umschalten des Werts an. Ebenso zeigt ein float-Element einen Schieberegler an, um den float-Wert zu ändern.

Es gibt noch komplexere Elemente, um umfangreichere Datentypen wie Bilder und HTML zu unterstützen. Beispielsweise wird in einem HTML-Element, das eine UIWebView öffnet, um eine Webseite zu laden, eine Beschriftung in der Tabellenzelle angezeigt.

### <a name="working-with-element-values"></a>Arbeiten mit Element Werten

Elemente, die zum Erfassen von Benutzereingaben verwendet werden, `Value` machen eine öffentliche Eigenschaft verfügbar, die den aktuellen Wert des-Elements zu einem beliebigen Zeitpunkt enthält. Sie wird automatisch aktualisiert, wenn der Benutzer die Anwendung verwendet.

Dies ist das Verhalten für alle Elemente, die Teil von MonoTouch. Dialog sind, aber für Benutzer erstellte Elemente nicht erforderlich sind.

### <a name="string-element"></a>String-Element

Eine `StringElement` zeigt eine Beschriftung auf der linken Seite einer Tabellenzelle und den Zeichen folgen Wert auf der rechten Seite der Zelle an.

 [![](images/image7.png "Ein stringelement zeigt eine Beschriftung auf der linken Seite einer Tabellenzelle und den Zeichen folgen Wert auf der rechten Seite der Zelle an.")](images/image7.png#lightbox)

Wenn Sie eine `StringElement` als Schaltfläche verwenden möchten, geben Sie einen Delegaten an.

```csharp
new StringElement ("Click me", () => { 
    new UIAlertView("Tapped", "String Element Tapped", null, "ok", null).Show();
});
```

 [![](images/image8.png "Wenn Sie ein stringelement als Schaltfläche verwenden möchten, geben Sie einen Delegaten an.")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Formatiertes Zeichen folgen Element

Ein `StyledStringElement` ermöglicht das darstellen von Zeichen folgen mithilfe integrierter Tabellenzellen Stile oder mit benutzerdefinierter Formatierung.

 [![](images/image9.png "Mit einem styledstringelement können Zeichen folgen entweder mithilfe integrierter Tabellenzellen Stile oder mit benutzerdefinierter Formatierung dargestellt werden.")](images/image9.png#lightbox)

Die `StyledStringElement` -Klasse wird `StringElement`von abgeleitet, aber Entwickler können einige Eigenschaften wie Schriftart, Textfarbe, Hintergrund Zellen Farbe, Zeilen Unterbrechungsmodus, Anzahl der anzuzeigenden Zeilen und festlegen, ob ein Zubehör angezeigt werden soll.

### <a name="multiline-element"></a>Multiline-Element

 [![](images/image10.png "Multiline-Element")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry-Element

`EntryElement`Wie der Name schon sagt, wird verwendet, um Benutzereingaben zu erhalten. Sie unterstützt entweder reguläre Zeichen folgen oder Kenn Wörter, bei denen Zeichen ausgeblendet sind.

 [![](images/image11.png "Das entryelement wird verwendet, um Benutzereingaben zu erhalten.")](images/image11.png#lightbox)

Es wird mit drei Werten initialisiert:

- Die Beschriftung für den Eintrag, der dem Benutzer angezeigt wird.
- Platzhalter Text (Hierbei handelt es sich um den ausgegrauten Text, der einen Hinweis für den Benutzer bereitstellt). 
- Der Wert des Texts.

Der Platzhalter und der Wert können NULL sein. Die Beschriftung ist jedoch erforderlich.

Der Zugriff auf die Value-Eigenschaft kann an einem beliebigen Punkt abgerufen werden `EntryElement`.

Außerdem kann `KeyboardType` die-Eigenschaft zum Zeitpunkt der Erstellung auf den für den Dateneintrag gewünschten Stil des Tastatur Typs festgelegt werden. Dies kann verwendet werden, um die Tastatur mithilfe der Werte von `UIKeyboardType` zu konfigurieren, wie unten aufgeführt:

- Numeric
- Phone
- Url
- E-Mail

### <a name="boolean-element"></a>Boolesches Element

 [![](images/image12.png "Boolesches Element")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>CheckBox-Element

 [![](images/image13.png "CheckBox-Element")](images/image13.png#lightbox)

### <a name="radio-element"></a>Radio-Element

Erfordert, dass in der `RootElement`angegeben wird. `RadioGroup` `RadioElement`

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0));
```

 [![](images/image14.png "Ein Radio Element erfordert, dass eine Radio Group im RootElement angegeben wird.")](images/image14.png#lightbox)

 `RootElements`werden auch zum Koordinieren von Radio-Elementen verwendet. Die `RadioElement` Elemente können mehrere Abschnitte umfassen (z. b. um eine ähnliche Implementierung wie die Rington Auswahl zu implementieren und benutzerdefinierte Ring Töne von System-Klingeltönen zu trennen). In der Zusammenfassungs Ansicht wird das Optionsfeld angezeigt, das derzeit ausgewählt ist. Um dies zu verwenden, erstellen `RootElement` Sie mit dem gruppenkonstruktor wie folgt:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0));
```

Der Name der Gruppe in `RadioGroup` wird verwendet, um den ausgewählten Wert in der enthaltenden Seite (sofern vorhanden) anzuzeigen, und der-Wert, der in diesem Fall 0 (null) ist, ist der Index des ersten ausgewählten Elements.

### <a name="badge-element"></a>Badge-Element

 [![](images/image15.png "Badge-Element")](images/image15.png#lightbox)

### <a name="float-element"></a>Float-Element

 [![](images/image16.png "Float-Element")](images/image16.png#lightbox)

### <a name="activity-element"></a>Activity-Element

 [![](images/image17.png "Activity-Element")](images/image17.png#lightbox)

### <a name="date-element"></a>Date-Element

 ![](images/image18.png "Date-Element")

Wenn die Zelle, die dem dateelement entspricht, ausgewählt ist, wird eine Datumsauswahl wie unten dargestellt angezeigt:

 [![](images/image19.png "Wenn die Zelle, die dem dateelement entspricht, ausgewählt ist, wird eine Datumsauswahl angezeigt.")](images/image19.png#lightbox)

### <a name="time-element"></a>Time-Element

 [![](images/image20.png "Time-Element")](images/image20.png#lightbox)

Wenn die Zelle ausgewählt ist, die dem timeelement entspricht, wird eine Zeitauswahl wie unten dargestellt angezeigt:

 [![](images/image21.png "Wenn die Zelle ausgewählt ist, die dem timeelement entspricht, wird eine Zeitauswahl wie angezeigt angezeigt.")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime-Element

 [![](images/image22.png "DateTime-Element")](images/image22.png#lightbox)

Wenn die Zelle, die dem datetimeelement entspricht, ausgewählt ist, wird eine DateTime-Auswahl wie unten dargestellt angezeigt:

 [![](images/image23.png "Wenn die Zelle, die dem datetimeelement entspricht, ausgewählt ist, wird eine DateTime-Auswahl angezeigt.")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML-Element

 [![](images/image24.png "HTML-Element")](images/image24.png#lightbox)

Der `HTMLElement` zeigt den Wert seiner `Caption` -Eigenschaft in der Tabellenzelle an. Wenn Sie die Option ausgewählt `Url` haben, wird das, das dem Element `UIWebView` zugewiesen ist, wie unten gezeigt in ein Steuerelement geladen:

 [![](images/image25.png "Wenn Sie ausgewählt haben, wird die URL, die dem Element zugewiesen ist, wie unten gezeigt in ein UIWebView-Steuerelement geladen.")](images/image25.png#lightbox)

### <a name="message-element"></a>Message-Element

 [![](images/image26.png "Message-Element")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Weitere Elemente laden

Verwenden Sie dieses Element, um Benutzern das Laden mehrerer Elemente in der Liste zu ermöglichen. Sie können das normale und das Laden von Beschriftungen sowie die Schriftart und die Textfarbe anpassen.
Der `UIActivity` Indikator startet die Animation, und die Lade Beschriftung wird angezeigt, wenn ein Benutzer auf die Zelle tippt, und `NSAction` dann wird der an den Konstruktor übergebenen ausgeführt. Sobald der Code im `NSAction` fertig ist, beendet der `UIActivity` Indikator die Animation, und die normale Beschriftung wird erneut angezeigt.

### <a name="uiview-element"></a>UIView-Element

Außerdem können alle Benutzer `UIView` definierten mit dem `UIViewElement`angezeigt werden.

### <a name="owner-drawn-element"></a>Vom Besitzer gezeichnetes Element

Dieses Element muss unter klassifiziert werden, da es sich um eine abstrakte Klasse handelt. Sie sollten die `Height(RectangleF bounds)` -Methode überschreiben, `Draw(RectangleF bounds, CGContext context, UIView view)` in der Sie die Höhe des-Elements zurückgeben sollten. Außerdem sollten Sie die gesamte angepasste Zeichnung innerhalb der angegebenen Begrenzungen mit den Kontext-und Sicht Parametern durchführen. Mit diesem Element wird die Unterklasse a `UIView`stark angehalten, und es wird in die Zelle gesetzt, die zurückgegeben werden soll, sodass Sie nur zwei einfache über schreibungen implementieren müssen. Eine bessere Beispiel Implementierung finden Sie in der-Beispiel-app in `DemoOwnerDrawnElement.cs` der-Datei.

Hier ist ein sehr einfaches Beispiel für die Implementierung der-Klasse:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
{
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text { get; set; }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
}
```

### <a name="json-element"></a>JSON-Element

Der `JsonElement` ist eine Unterklasse von `RootElement` , die einen `RootElement` erweitert, um den Inhalt eines untergeordneten Elements aus einer lokalen URL oder Remote-URL laden zu können.

Der `JsonElement` ist ein `RootElement` , der in zwei Formen instanziiert werden kann. Eine Version erstellt einen `RootElement` , mit dem der Inhalt bei Bedarf geladen wird. Diese werden erstellt, indem die `JsonElement` Konstruktoren verwendet werden, die am Ende ein zusätzliches Argument akzeptieren, und die URL, aus der die Inhalte geladen werden:

```csharp
var je = new JsonElement ("Dynamic Data", "https://tirania.org/tmp/demo.json");
```

Das andere Formular erstellt die Daten aus einer lokalen Datei oder einem vorhandenen `System.Json.JsonObject` , das Sie bereits analysiert haben:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Weitere Informationen zur Verwendung von JSON mit Mt. Weitere Informationen finden Sie im Tutorial zum [JSON-Element](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) Exemplarische Vorgehensweise.

## <a name="other-features"></a>Weitere Features

### <a name="pull-to-refresh-support"></a>Pull-to-refresh-Unterstützung

 *Pull-zu-* Die *Aktualisierung* ist ein visueller Effekt, der ursprünglich in der *Tweetie2* -APP zu finden war, was in vielen Anwendungen zu einer beliebten Auswirkung geworden ist.

Wenn Sie Ihren Dialogfeldern eine automatische Pull-to-refresh-Unterstützung hinzufügen möchten, müssen Sie nur zwei Dinge tun: richten Sie einen Ereignishandler ein, um benachrichtigt zu werden, `DialogViewController` wenn der Benutzer die Daten abruft, und Benachrichtigen Sie den, wenn die Daten geladen wurden, um wieder in den Standardzustand zurückzukehren.

Das Einbinden einer Benachrichtigung ist einfach. Stellen Sie einfach wie `RefreshRequested` folgt eine Verbindung `DialogViewController`mit dem-Ereignis auf dem her:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Anschließend würden Sie bei `OnUserRequestedRefresh`ihrer Methode das Laden von Daten in die Warteschlange stellen, einige Daten aus dem Netzwerk anfordern oder einen Thread zum Berechnen der Daten drehen. Nachdem die Daten geladen wurden, müssen Sie den `DialogViewController` Benachrichtigen, dass die neuen Daten in liegen, und die Ansicht in Ihrem Standardzustand wiederherstellen, indem Sie Folgendes aufrufen: `ReloadComplete`

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Suchunterstützung

Um die Suche zu unterstützen `EnableSearch` , legen Sie `DialogViewController`die-Eigenschaft auf dem- Sie können auch die `SearchPlaceholder` -Eigenschaft so festlegen, dass Sie als Wasserzeichen Text in der Suchleiste verwendet wird.

Durch das suchen wird der Inhalt der Ansicht als Benutzer Typen geändert. Es durchsucht die sichtbaren Felder und zeigt Sie dem Benutzer an. `DialogViewController` Bietet drei Methoden zum programmgesteuerten initiieren, beenden oder Auslösen eines neuen Filter Vorgangs für die Ergebnisse. Diese Methoden sind unten aufgeführt:

- `StartSearch`
- `FinishSearch`
- `PerformFilter`

Das System ist erweiterbar, sodass Sie dieses Verhalten bei Bedarf ändern können.

### <a name="background-image-loading"></a>Laden des Hintergrund Bilds

MonoTouch. Dialog enthält das Bild Lade Modul der [tweetstation](https://github.com/migueldeicaza/TweetStation) -Anwendung. Dieses Bild Lade Modul kann verwendet werden, um Bilder im Hintergrund zu laden. unterstützt das Zwischenspeichern und kann den Code Benachrichtigen, wenn das Image geladen wurde.

Außerdem wird die Anzahl der ausgehenden Netzwerkverbindungen eingeschränkt.

Das Bild Lade Modul ist in der `ImageLoader` -Klasse implementiert. Sie müssen lediglich die `DefaultRequestImage` -Methode aufrufen. Sie müssen den URI für das Bild angeben, das Sie laden möchten, sowie eine Instanz der `IImageUpdated` Schnittstelle, die aufgerufen wird, wenn das Bild hoch- en wurden geladen.

Der folgende Code lädt z. b. ein Bild aus einer URL `BadgeElement`in eine:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
    new Section() {
        new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
    }
};
```

Die ImageLoader-Klasse macht eine Löschmethode verfügbar, die aufgerufen werden kann, wenn Sie alle Abbilder freigeben möchten, die derzeit im Arbeitsspeicher zwischengespeichert sind. Der aktuelle Code weist einen Cache für 50-Images auf. Wenn Sie eine andere Cache Größe verwenden möchten (z. bWeise, wenn Sie erwarten, dass die Images zu groß sind, dass 50-Images zu groß wären), können Sie einfach Instanzen von ImageLoader erstellen und die Anzahl der Images, die im Cache aufbewahrt werden sollen, übergeben.

## <a name="using-linq-to-create-element-hierarchy"></a>Verwenden von LINQ zum Erstellen der Element Hierarchie

Mithilfe der intelligenten Verwendung von LINQ und C#der Initialisierungs Syntax kann LINQ verwendet werden, um eine Element Hierarchie zu erstellen. Der folgende Code erstellt z. b. einen Bildschirm aus einigen Zeichen folgen Arrays und behandelt die Zellen Auswahl über eine anonyme Funktion, `StringElement`die an die jeweilige Funktion übermittelt wird:

```csharp
var rootElement = new RootElement ("LINQ root element") {
    from x in new string [] { "one", "two", "three" }
    select new Section (x) {
        from y in "Hello:World".Split (':')
        select (Element) new StringElement (y, delegate { Debug.WriteLine("cell tapped"); })
    }
};
```

Dies kann problemlos mit einem XML-Datenspeicher oder Daten aus einer Datenbank kombiniert werden, um komplexe Anwendungen nahezu vollständig aus Daten zu erstellen.

## <a name="extending-mtd"></a>Erweitern von Mt. D

### <a name="creating-custom-elements"></a>Erstellen von benutzerdefinierten Elementen

Sie können ein eigenes-Element erstellen, indem Sie entweder von einem vorhandenen Element erben oder von dem Stamm Klassen Element ableiten.

Wenn Sie ein eigenes-Element erstellen möchten, sollten Sie die folgenden Methoden überschreiben:

```csharp
// To release any heavy resources that you might have
void Dispose (bool disposing);

// To retrieve the UITableViewCell for your element
// you would need to prepare the cell to be reused, in the
// same way that UITableView expects reusable cells to work
UITableViewCell GetCell (UITableView tv);

// To retrieve a "summary" that can be used with
// a root element to render a summary one level up.  
string Summary ();

// To detect when the user has tapped on the cell
void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path);

// If you support search, to probe if the cell matches the user input
bool Matches (string text);
```

Wenn das Element eine Variable Größe aufweisen kann, müssen Sie die `IElementSizing` -Schnittstelle implementieren, die eine Methode enthält:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Wenn Sie die Implementierung `GetCell` der-Methode durch Aufrufen `base.GetCell(tv)` von und Anpassen der zurückgegebenen Zelle planen, müssen Sie auch die- `CellKey` Eigenschaft überschreiben, um einen Schlüssel zurückzugeben, der für Ihr Element eindeutig ist, wie hier:

```csharp
static NSString MyKey = new NSString ("MyKey");
protected override NSString CellKey {
    get {
        return MyKey;
    }
}
```

Dies funktioniert für die meisten `StringElement` -Elemente, aber nicht für und `StyledStringElement` , da diese ihren eigenen Satz von Schlüsseln für verschiedene renderingszenarios verwenden. Sie müssen den Code in diesen Klassen replizieren.

### <a name="dialogviewcontrollers-dvcs"></a>Dialogviewcontrollers (dvcs)

Sowohl die Reflektion als auch die Elements-API `DialogViewController`verwenden dasselbe. Manchmal wird das Aussehen der Ansicht angepasst, oder Sie möchten möglicherweise einige Features von `UITableViewController` verwenden, die über die grundlegende Erstellung von Benutzeroberflächen hinausgehen.

Ist nur eine Unterklasse `UITableViewController` von und kann auf die gleiche Weise wie ein `UITableViewController`angepasst werden. `DialogViewController`

Wenn Sie z. b. den Listenstil in `Grouped` oder `Plain`ändern möchten, können Sie diesen Wert festlegen, indem Sie die-Eigenschaft beim Erstellen des Controllers wie folgt ändern:

```csharp
var myController = new DialogViewController (root, true) {
    Style = UITableViewStyle.Grouped;
}
```

Für erweiterte Anpassungen von `DialogViewController`, wie z. b. das Festlegen des Hintergrunds, würden Sie es unterteilen und die richtigen Methoden überschreiben, wie im folgenden Beispiel gezeigt:

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Ein weiterer Anpassungs Punkt sind die folgenden virtuellen Methoden in `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Diese Methode sollte eine Unterklasse von `DialogViewController.Source` für Fälle zurückgeben, in denen Ihre Zellen gleichmäßig groß sind, oder eine Unterklasse von `DialogViewController.SizingSource` , wenn die Zellen ungleich sind.

Sie können diese außer Kraft Setzung verwenden, um beliebige `UITableViewSource` Methoden zu erfassen. [Tweetstation](https://github.com/migueldeicaza/TweetStation) verwendet diese z. b., um zu verfolgen, wann der Benutzer einen Rollup durchgeführt hat, und die Anzahl der ungelesenen tweets entsprechend zu aktualisieren.

## <a name="validation"></a>Validierung

Elemente bieten keine selbst Validierung, da die Modelle, die für Webseiten und Desktop Anwendungen gut geeignet sind, nicht direkt dem iPhone-Interaktionsmodell zugeordnet werden.

Wenn Sie die Datenvalidierung durchführen möchten, sollten Sie dies tun, wenn der Benutzer eine Aktion mit den eingegebenen Daten auslöst. Beispielsweise eine **done** -oder **Next** -Schaltfläche auf der oberen Symbol `StringElement` Leiste oder einige als Schaltfläche, die zur nächsten Phase verwendet wird.

An dieser Stelle führen Sie eine grundlegende Eingabe Überprüfung durch und vielleicht eine kompliziertere Validierung wie das Überprüfen der Gültigkeit einer Benutzer-/Kennwort-Kombination mit einem Server.

Wie Sie den Benutzer über einen Fehler benachrichtigen, ist anwendungsspezifisch. Sie können ein `UIAlertView` -oder einen-Hinweis anzeigen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden viele Informationen zu MonoTouch. Dialog behandelt. Es wurden die Grundlagen der Verwendung von MT erläutert. D funktioniert und deckt die verschiedenen Komponenten ab, die MT umfassen. D. Außerdem wurde das umfangreiche Array von Elementen und Tabellen Anpassungen gezeigt, die von MT unterstützt werden. D und erläutert, wie Mt. D kann mit benutzerdefinierten Elementen erweitert werden. Außerdem wurde die JSON-Unterstützung in Mt erläutert. D, der das dynamische Erstellen von Elementen aus JSON ermöglicht.

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Reflektions-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Exemplarische Vorgehensweise: Verwenden eines JSON-Elements zum Erstellen einer Benutzeroberfläche](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch. Dialog-JSON-Markup](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialog Feld auf GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Uitableviewcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassen Verweis](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
