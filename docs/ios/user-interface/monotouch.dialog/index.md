---
title: Einführung in die MonoTouch.Dialog für Xamarin.iOS
description: Dieses Dokument beschreibt MonoTouch.Dialog (MT.) (D), ein Framework für die schnelle, deklarativen Entwicklung von Benutzeroberflächen mit Xamarin.iOS. Es wird erläutert, wie Sie mit den MonoTouch.Dialog-APIs erstellen Sie eine Schnittstelle im Code oder in JSON, und Verwenden von Features wie zum Aktualisieren ziehen, Suche, Bild laden im Hintergrund und mehr.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bee4b460552c7273021b16955b52ba3d95d3e07c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038403"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Einführung in die MonoTouch.Dialog für Xamarin.iOS

MonoTouch.Dialog, bezeichnet als MT. D kurz, ist eine schnelle entwicklungstoolkit, die Entwickler Anwendung Bildschirme und Navigation über die Informationen, anstatt den Aufwand zum Erstellen von View Controller, Tabellen usw. erstellen kann. Daher wird eine erhebliche Vereinfachung der Entwicklung und Code Verringerung der Benutzeroberfläche. Betrachten Sie z. B. des folgenden Screenshots aus:

 [![](images/image1.png "Betrachten Sie beispielsweise die in diesem screenshot")](images/image1.png#lightbox)

Der folgende Code wurde verwendet, diesen gesamten Bildschirm definieren:

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

Bei der Arbeit mit Tabellen in iOS ist häufig eine Unmenge wiederkehrende Code.
Jedes Mal, wenn eine Tabelle erforderlich ist, wird z. B. eine Datenquelle erforderlich, um diese Tabelle aufzufüllen. In einer Anwendung mit zwei Tabellen basierenden Bildschirme, die über einen navigationscontroller verbunden sind, teilt jeden Bildschirm einen Großteil der gleiche Code.

MT. D vereinfacht, die durch die gesamte Code in eine generische API zum Erstellen der Tabelle zu kapseln. Es zeigt dann eine Abstraktionsebene über diese API, die es ermöglicht eine deklarative Objekt binden die Syntax, die noch einfacher macht. Daher ist es möglich, gibt es zwei APIs im masterziel verfügbar "D:"

-   **Low-Level-Elemente-API** – die *Elemente-API* basiert auf erstellen eine hierarchische Struktur von Elementen, die Bildschirme und deren Komponenten darstellen. Die Elemente-API bietet Entwicklern die Flexibilität und Kontrolle bei der Erstellung von Benutzeroberflächen. Darüber hinaus bietet der Elemente-API-Unterstützung für die deklarative Definition über JSON, erweiterte, also für sowohl unglaublich schnell Deklaration als auch dynamische Benutzeroberfläche-Generierung von einem Server. 
-   **Allgemeine Reflektions-API** – auch bekannt als die *Bindung**API* , in die Klassen mit Benutzeroberflächen-Hinweise, und klicken Sie dann masterziel versehen sind D erstellt Bildschirme, die auf der Grundlage der Objekte automatisch und stellt eine Bindung zwischen dem, was angezeigt (und optional bearbeitet) auf dem Bildschirm, und die zugrunde liegende Objekt sichern.   Das obige Beispiel veranschaulicht die Verwendung der Reflektions-API. Diese API nicht die eine präzisere Kontrolle, die die API-Elemente bieten, aber sie verringert die Komplexität noch einen Schritt weiter, die Hierarchie des Elements basierend auf den Klassenattributen automatisch erstellen. 


MT. D ist mit einem großen Satz des packed-im UI-Elemente für die Erstellung von Bildschirm erstellt, aber auch die Notwendigkeit, benutzerdefinierte Elemente und erweiterte Bildschirmlayouts erkennt. Daher ist die Erweiterbarkeit, dass eine leistungsstarke mitgelieferten an die API vorgestellt. Entwickler können die vorhandenen Elemente zu erweitern oder neue zu erstellen und dann nahtlos integrieren.

Darüber hinaus MT. D verfügt über eine Reihe von allgemeinen iOS-UX-Funktionen, die erstellt werden, z. B. "zum Aktualisieren ziehen" zu unterstützen, asynchrone Bild laden, und die Unterstützung der Suche.

In diesem Artikel dauert einen umfassenden Überblick über die Arbeit mit MT. D, einschließlich:

-   **MT. D-Komponenten** – dies konzentriert sich auf Grundlegendes zu den Klassen, aus denen MT. D, um zu ermöglichen, sich schnell abrufen. 
-   **Elementreferenz** – eine umfassende Liste der integrierten Elemente des masterzielservers D. 
-   **Erweiterte Verwendung** – Dies umfasst erweiterte Funktionen wie z. B. zum Aktualisieren ziehen, Suche, Bild laden im Hintergrund, unter Verwendung von LINQ zum Element Hierarchien erstellen und erstellen benutzerdefinierte Elemente, Zellen, und -Controller für mit MT. D. 

## <a name="setting-up-mtd"></a>Masterziel einrichten D

MT. D ist mit Xamarin.iOS verteilt. Um es zu verwenden, mit der Maustaste auf die **Verweise** Knoten eine Xamarin.iOS-Projekt in Visual Studio 2017 oder Visual Studio für Mac und Hinzufügen eines Verweises auf die **MonoTouch.Dialog-1-** Assembly. Fügen Sie dann `using MonoTouch.Dialog` Anweisungen in Ihrer Quelle code nach Bedarf.

## <a name="understanding-the-pieces-of-mtd"></a>Grundlegendes zu den Bestandteilen des masterzielservers D

Auch bei Verwendung der Reflektions-API, masterziel D erstellt eine Hierarchie der Elemente in die Hintergründe, als ob sie direkt über die Elemente-API erstellt wurden. Darüber hinaus erstellt die JSON-Unterstützung, die im vorherigen Abschnitt erwähnt auch Elemente aus. Aus diesem Grund ist es wichtig, um einen grundlegenden Überblick über die einzelnen Teile des masterzielservers zu erhalten. D.

MT. D erstellt Bildschirme, die mit den folgenden vier Teilen:

-  **DialogViewController**
-  **RootElement**
-  **Bereich**
-  **Element**


### <a name="dialogviewcontroller"></a>DialogViewController

Ein *DialogViewController*, oder *DVC* kurz, erbt `UITableViewController` und stellt daher einen Bildschirm mit einer Tabelle dar. Zur können auf einen navigationscontroller genau wie eine reguläre UITableViewController abgelegt werden.

### <a name="rootelement"></a>RootElement

Ein *RootElement* ist der Container der obersten Ebene für die Elemente, die in einem DVC aufgenommen. Es enthält Abschnitte, die dann die Elemente enthalten kann. RootElements werden nicht gerendert. Stattdessen sind sie einfach ein Container für was tatsächlich gerendert wird. Eine RootElement eine DVC zugewiesen wird, und klicken Sie dann die DVC rendert die untergeordneten Elemente.

### <a name="section"></a>Bereich

Ein Abschnitt ist eine Gruppe von Zellen in einer Tabelle. Wie bei einer normalen Tabellenabschnitt es optional kann sein eine Kopf- und Fußzeile, die entweder Text oder sogar benutzerdefinierten Ansichten, wie im folgenden Screenshot dargestellt:

 [![](images/image2.png "Wie bei einer normalen Tabellenabschnitt es optional kann eine Kopf- und Fußzeile, die können entweder Text oder sein auch benutzerdefinierte Ansichten, wie in diesem screenshot")](images/image2.png#lightbox)

### <a name="element"></a>Element

Ein Element stellt eine tatsächliche Zelle in der Tabelle dar. MT. D enthält gepackte mit einer Vielzahl von Elementen, die unterschiedliche Datentypen oder verschiedene Eingaben darstellen. Die folgenden Screenshots veranschaulichen beispielsweise nur einige der verfügbaren Elemente:

 [![](images/image3.png "Diese Screenshots veranschaulichen, z. B. nur einige der verfügbaren Elemente")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Weitere Abschnitte und RootElements

Jetzt betrachten wir RootElements und Abschnitten ausführlicher.

### <a name="rootelements"></a>RootElements

Mindestens RootElement ist erforderlich, um den MonoTouch.Dialog zu starten.

Wenn eine RootElement mit einem Wert Section-Element initialisiert wird, wird dieser Wert verwendet, um ein untergeordnetes Element zu suchen, die eine Zusammenfassung der Konfiguration, erhalten die auf der rechten Seite der Anzeige gerendert wird. Der folgende Screenshot zeigt beispielsweise eine Tabelle auf der linken Seite mit einer Zelle, die mit dem Titel des Detail-Bildschirm, auf der rechten Seite, "Dessert", und den Wert der ausgewählten Desert.

 [![](images/image4.png "Dieser Screenshot zeigt eine Tabelle auf der linken Seite mit einer Zelle, die mit dem Titel des Detail-Bildschirm, auf der rechten Seite, Dessert, zusammen mit dem Wert von der ausgewählten Desert")](images/image4.png#lightbox) [![](images/image5.png "dies folgende Screenshot zeigt eine Tabelle auf der linken Seite mit einer Zelle, die mit dem Titel des Detail-Bildschirm, auf der rechten Seite, Dessert, zusammen mit dem Wert von der ausgewählten desert")](images/image5.png#lightbox)

Elementgruppenstamm-Elemente können auch in Abschnitte verwendet werden um auszulösen, laden eine neue Seite für die geschachtelte Konfiguration wie oben gezeigt. Bei der Verwendung in diesem Modus wird die angegebene Beschriftung wird verwendet, während in einem Abschnitt gerendert und wird auch als Titel für den Nachverfolgungscode verwendet. Zum Beispiel:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

Im obigen Beispiel wenn der Benutzer auf "Dessert", tippt MonoTouch.Dialog erstellt eine neue Seite und navigieren sie mit dem Stamm "Dessert" wird und dass von einer Optionsfeldgruppe mit drei Werten.

In diesem bestimmten Beispiel wählt die Optionsfeldgruppe "Schokoladenkuchen" im Abschnitt "Dessert", da wir den Wert "2" an die RadioGroup übergeben. Dies bedeutet, wählen Sie die 3. Element in der Liste (0 (null)-Index).

Die Add-Methode aufrufen, oder verwenden die Syntax des Initialisierers c# 4 fügt Abschnitte.
Einfügemethoden werden bereitgestellt, um Abschnitte mit einer Animation einzufügen.

Wenn Sie eine Instanz der Gruppe (anstelle einer RadioGroup) RootElement erstellen werden zusammenfassende Wert ist der RootElement bei der Anzeige in einem Abschnitt der kumulierten Anzahl aller BooleanElements und CheckboxElements, die den gleichen Schlüssel wie die Group.Key-Wert aufweisen.

### <a name="sections"></a>Abschnitte

Abschnitte dienen zum Gruppieren von Elementen auf dem Bildschirm, und sie sind die einzigen gültigen direkten untergeordneten Elemente des RootElement. Abschnitte können eines der standard-Elemente, einschließlich der neuen RootElements enthalten.

RootElements in einem Abschnitt eingebettet werden verwendet, um zu einer neuen tieferen Ebene zu navigieren.

Abschnitte können Kopf- und Fußzeilen entweder als Zeichenfolgen oder als UIViews enthalten.
In der Regel verwenden Sie nur die Zeichenfolgen, aber Sie können alle UIView als die Kopf- oder Fußzeile verwenden, um benutzerdefinierte Benutzeroberflächen zu erstellen. Sie können entweder eine Zeichenfolge verwenden, sie wie folgt erstellt:

```csharp
var section = new Section ("Header", "Footer")
```

Um Sichten zu verwenden, müssen übergeben Sie die Ansichten einfach an den Konstruktor:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Erhalten von Benachrichtigungen

#### <a name="handling-nsaction"></a>Behandeln von NSAction

MT. D-Oberflächen ein `NSAction` als Delegat für die Behandlung von Rückrufen.
Angenommen Sie, dass Sie ein touchereignis bei einer Tabellenzelle, die durch das masterziel erstellt behandeln möchten. D. Wenn Sie ein Element mit masterziel zu erstellen D, einfach geben ein Rückruf-Funktion, wie unten dargestellt:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Abrufen der Wert des Elements

In Kombination mit der `Element.Value` -Eigenschaft des Rückrufs kann rufen Sie den Wert festgelegt, die in anderen Elementen. Beachten Sie z. B. folgenden Code:

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

Dieser Code erstellt ein Benutzeroberflächenelement an, wie unten dargestellt. Eine vollständige Exemplarische Vorgehensweise in diesem Beispiel wird, finden Sie unter den [Elemente-API-Exemplarische Vorgehensweise](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) Tutorial.

 [![](images/image6.png "In Kombination mit der Eigenschaft Element.Value kann der Rückruf Abrufen des Werts in andere Elemente festlegen")](images/image6.png#lightbox)

Wenn der Benutzer die unterste Tabellenzelle drückt, der Code in der anonymen Funktion ausgeführt wird, schreiben den Wert aus der `element` -Instanz, auf die **Anwendungsausgabe** Pad in Visual Studio für Mac.

## <a name="built-in-elements"></a>Integrierte Elemente

MT. D enthält eine Reihe von integrierten Tabelle Zelle Elementen als Elemente bezeichnet.
Diese Elemente werden verwendet, um eine Vielzahl verschiedener Typen in Tabellenzellen, z. B. Zeichenfolgen, Gleitkommazahlen, Datumsangaben und auch Images, um nur einige zu nennen anzuzeigen. Jedes Element übernimmt den Datentyp entsprechend angezeigt. Beispielsweise zeigt ein boolesches Element einen Switch aus, um den Wert zu wechseln. Ebenso wird ein Float-Element einen Schieberegler zum Ändern des Werts von "float" angezeigt.

Es gibt noch komplexer Elemente zur Unterstützung von mehr Datentypen, z. B. Bilder und html. Ein html-Element, dem ein UIWebView zum Laden einer Webseite, die bei Auswahl geöffnet wird, zeigt z. B. eine Beschriftung in der Tabellenzelle.

### <a name="working-with-element-values"></a>Arbeiten mit Werten

Elemente, die verwendet werden, um Benutzereingaben zu erfassen, verfügbar zu machen eine öffentliche `Value` Eigenschaft, die den aktuellen Wert des Elements zu einem beliebigen Zeitpunkt enthält. Automatisch wird aktualisiert, wenn der Benutzer die Anwendung verwendet.

Dies ist das Verhalten für alle Elemente, die Teil des MonoTouch.Dialog sind, aber es ist nicht erforderlich, damit Benutzer erstellten Elemente.

### <a name="string-element"></a>Zeichenfolgenelement

Ein `StringElement` zeigt eine Beschriftung auf der linken Seite einer Tabellenzelle und den Zeichenfolgenwert auf der rechten Seite der Zelle.

 [![](images/image7.png "Eine StringElement zeigt eine Beschriftung auf der linken Seite einer Tabellenzelle und den Zeichenfolgenwert auf der rechten Seite der Zelle")](images/image7.png#lightbox)

Verwenden einer `StringElement` als Schaltfläche, einen Delegaten bereitstellen.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Um eine StringElement als Schaltfläche zu verwenden, geben Sie einen Delegaten")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Formatierte Zeichenfolgenelement

Ein `StyledStringElement` können Zeichenfolgen, die angezeigt werden, verwenden entweder Zellstile Tabelle integrierter oder mit benutzerdefiniertem Format.

 [![](images/image9.png "Eine StyledStringElement kann Zeichenfolgen, die angezeigt werden, verwenden entweder Zellstile Tabelle integrierter oder benutzerdefinierte Formatierung")](images/image9.png#lightbox)

Die `StyledStringElement` Klasse leitet sich von `StringElement`, jedoch können Entwickler anpassen, eine Reihe von Eigenschaften wie Schriftart, Farbe des Textes, hintergrundzellenfarbe, wichtige Zeilenmodus, Anzahl von Zeilen angezeigt werden, und gibt an, ob ein Zubehör angezeigt werden soll.

### <a name="multiline-element"></a>Multiline-Element

 [![](images/image10.png "Multiline-Element")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry-Element

Die `EntryElement`, wie der Name impliziert, wird verwendet, um Benutzereingaben zu erhalten. Es unterstützt reguläre Zeichenfolgen oder Kennwörter, in denen Zeichen ausgeblendet sind.

 [![](images/image11.png "Die EntryElement wird verwendet, um Benutzereingaben zu erhalten.")](images/image11.png#lightbox)

Es wird mit drei Werten initialisiert:

-  Die Beschriftung für den Eintrag, der dem Benutzer angezeigt wird.
-  Der Platzhaltertext ist (Dies ist der ausgegrauten-Text, der einen Hinweis für dem Benutzer bereitstellt). 
-  Der Wert des Texts.


Der Platzhalter und der Wert können null sein. Die Beschriftung ist jedoch erforderlich.

Zu jedem Zeitpunkt den Zugriff auf die Value-Eigenschaft der Abrufen des Werts kann die `EntryElement`.

Darüber hinaus die `KeyboardType` Eigenschaft kann zum Zeitpunkt der Erstellung auf der Tastatur Schriftstil gewünscht, für die Dateneingabe festgelegt werden. Dies kann verwendet werden, die mit den Werten der Tastatur konfigurieren `UIKeyboardType` wie im folgenden aufgeführt:

-  Numeric
-  Telefon
-  URL
-  E-Mail


### <a name="boolean-element"></a>Boolesches Element

 [![](images/image12.png "Boolesches Element")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>CheckBox-Element

 [![](images/image13.png "CheckBox-Element")](images/image13.png#lightbox)

### <a name="radio-element"></a>Radio-Element

Ein `RadioElement` erfordert eine `RadioGroup` in angegeben werden die `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "Eine RadioElement erfordert eine RadioGroup in RootElement angegeben werden")](images/image14.png#lightbox)

 `RootElements` Koordinieren von Optionsfeld-Elemente werden auch verwendet werden. Die `RadioElement` Mitglieder können mehrere Abschnitte (z. B. etwas Ähnliches wie in der Ring Ton Selektor und die eigenen benutzerdefinierten Klingeltöne vom System Klingeltöne implementieren) umfassen. Die Zusammenfassungsansicht zeigt das Radio-Element, das derzeit ausgewählt ist. Um dies zu verwenden, erstellen die `RootElement` mit dem Group-Konstruktor wie folgt:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Der Name der Gruppe in `RadioGroup` wird verwendet, um den ausgewählten Wert in der entsprechenden Seite (sofern vorhanden) und den Wert, der 0 (null) in diesem Fall ist, ist der Index des ersten ausgewählten Elements.

### <a name="badge-element"></a>Badge-Element

 [![](images/image15.png "Badge-Element")](images/image15.png#lightbox)

### <a name="float-element"></a>"Float"-Element

 [![](images/image16.png "\"Float\"-Element")](images/image16.png#lightbox)

### <a name="activity-element"></a>Aktivitätselement

 [![](images/image17.png "Aktivitätselement")](images/image17.png#lightbox)

### <a name="date-element"></a>Datum-Element

 ![](images/image18.png "Datum-Element")

Wenn die Zelle, die für die DateElement ausgewählt ist, wird eine Datumsauswahl dargestellt, wie unten dargestellt:

 [![](images/image19.png "Wenn die Zelle, die für die DateElement ausgewählt ist, wird eine Datumsauswahl angezeigt, siehe")](images/image19.png#lightbox)

### <a name="time-element"></a>Time-Element

 [![](images/image20.png "Time-Element")](images/image20.png#lightbox)

Wenn die Zelle, die für die TimeElement ausgewählt ist, wird eine Zeitauswahl dargestellt, wie unten dargestellt:

 [![](images/image21.png "Wenn die Zelle, die für die TimeElement ausgewählt ist, wird eine Zeitauswahl angezeigt, siehe")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime-Element

 [![](images/image22.png "DateTime-Element")](images/image22.png#lightbox)

Wenn die Zelle, die für die DateTimeElement ausgewählt ist, wird eine Auswahl von "DateTime" dargestellt, wie unten dargestellt:

 [![](images/image23.png "Wenn die Zelle, die für die DateTimeElement ausgewählt ist, wird eine \"DateTime\"-Auswahl angezeigt, siehe")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML-Element

 [![](images/image24.png "HTML-Element")](images/image24.png#lightbox)

Die `HTMLElement` zeigt den Wert der `Caption` -Eigenschaft in der Tabellenzelle. Whe ausgewählt, die `Url` zugewiesen, auf das Element wird geladen, in eine `UIWebView` steuern, wie unten dargestellt:

 [![](images/image25.png "Whe ausgewählt, wird die Url, die dem Element zugewiesenen wie unten gezeigt in einem Steuerelement UIWebView geladen")](images/image25.png#lightbox)

### <a name="message-element"></a>Message-Element

 [![](images/image26.png "Message-Element")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Laden Sie weitere-Element

Verwenden Sie dieses Element, um Benutzern beim Laden der weitere Elemente in der Liste zu ermöglichen. Sie können die normalen und Laden von Beschriftungen, als auch die Farbe, Schriftart und Text anpassen.
Die `UIActivity` Indikator startet animieren und die Beschriftung laden wird angezeigt, wenn ein Benutzer die Zelle tippt, und klicken Sie dann die `NSAction` übergeben der Konstruktor ausgeführt wird. Einmal den Code in die `NSAction` fertig gestellt wurde, die `UIActivity` Indikator beendet animieren und die normalen Beschriftung wird erneut angezeigt.

### <a name="uiview-element"></a>UIView-Element

Darüber hinaus jegliche benutzerdefinierten `UIView` können angezeigt werden, mithilfe der `UIViewElement`.

### <a name="owner-drawn-element"></a>Ownerdrawn-Element

Dieses Element muss als Unterklasse definiert werden, da es sich um eine abstrakte Klasse ist. Sollten Sie überschreiben die `Height(RectangleF bounds)` Methode, die in dem Sie die Höhe des Elements zurückgeben sollte als auch `Draw(RectangleF bounds, CGContext context, UIView view)` in dem die benutzerdefinierte Zeichnung innerhalb der angegebenen Begrenzungen, Sie mit den Parametern und müssen. Dieses Element leistet die Schwerarbeit von Unterklassen eine `UIView`, und legen Sie das Element in der Zelle zurückgegeben werden, lassen Sie nur zwei einfache Außerkraftsetzungen implementieren müssen. Sehen Sie eine bessere beispielimplementierung in der Beispiel-app in der `DemoOwnerDrawnElement.cs` Datei.

Hier ist ein sehr einfaches Beispiel der Implementierung der Klasse:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

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

Die `JsonElement` ist eine Unterklasse von `RootElement` , reicht eine `RootElement` , um den Inhalt der geschachtelten untergeordneten aus einer lokalen oder remote-Url laden zu können.

Die `JsonElement` ist eine `RootElement` , die in zwei Formen instanziiert werden. Erstellt eine Version einer `RootElement` lädt, die den Inhalt nach Bedarf. Diese werden erstellt, mit der `JsonElement` Konstruktoren verwenden, ein zusätzliches Argument am Ende der Url beim Laden des Inhalts von:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Die andere Form erstellt die Daten aus einer lokalen Datei oder eine vorhandene `System.Json.JsonObject` , die Sie haben bereits so analysiert:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Weitere Informationen zur Verwendung von JSON mit MT. D, finden Sie unter den [JSON-Element Exemplarische Vorgehensweise](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) Tutorial.

## <a name="other-features"></a>Andere Funktionen

### <a name="pull-to-refresh-support"></a>Zum Aktualisieren ziehen-Unterstützung

 *Pull-zu-* *aktualisieren* ein visuellen Effekts ursprünglich befindet sich der *Tweetie2* -app, die einen beliebten Effekt, Nutzung durch mehrere Anwendungen wurden.

Um Unterstützung für automatische Pull zum Aktualisieren Ihrer Dialogfelder hinzuzufügen, müssen Sie nur zwei Dinge tun: Verknüpfen eines ereignishandlers benachrichtigt, wenn der Benutzer die Daten abruft, und Benachrichtigen der `DialogViewController` beim Laden der Daten hat wurde um auf den Standardzustand zurück zu wechseln.

Einbinden einer benachrichtigungs ist einfach. verbindet sich einfach mit der `RefreshRequested` Ereignis auf der `DialogViewController`, wie folgt aus:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Klicken Sie dann auf die Methode `OnUserRequestedRefresh`, würden Sie einige Laden von Daten in die Warteschlange, einige Daten aus dem Netz anfordern oder erstellen Sie einen Thread, um die Daten zu berechnen. Sobald die Daten geladen wurden, müssen Sie benachrichtigen den `DialogViewController` , die die neuen Daten und um die Ansicht auf den Standardzustand wiederherstellen möchten, rufen Sie dazu `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Support durchsuchen

Legen Sie zur Unterstützung von Suchvorgängen, die `EnableSearch` Eigenschaft Ihre `DialogViewController`. Sie können auch Festlegen der `SearchPlaceholder` Eigenschaft, die als der Wasserzeichentext in der Suchleiste.

Suchen ändert den Inhalt der Ansicht, wenn der Benutzer eingibt. Es durchsucht die sichtbaren Feldern und zeigt diese an den Benutzer. Die `DialogViewController` macht drei Methoden, um programmgesteuert zu initiieren, beenden und lösen einen neuen Filtervorgang auf den Ergebnissen. Diese Methoden sind nachfolgend aufgeführt:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Das System ist erweiterbar, damit Sie dieses Verhalten ändern können, wenn Sie möchten.

### <a name="background-image-loading"></a>Bild laden im Hintergrund

MonoTouch.Dialog umfasst die [TweetStation](https://github.com/migueldeicaza/TweetStation) Bildladeprogramm der Anwendung. Dieses Image-Ladeprogramm kann zum Laden von Bildern im Hintergrund unterstützt, die Zwischenspeicherung verwendet werden und kann um Ihren Code benachrichtigen, wenn das Bild geladen wurde.

Es wird auch die Anzahl der ausgehenden Netzwerkverbindungen begrenzen.

Das Bildladeprogramm wird implementiert, der `ImageLoader` -Klasse, Sie müssen lediglich ist der Aufruf der `DefaultRequestImage` -Methode, Sie benötigen, geben Sie den Uri für das Bild, das Sie laden möchten, sowie eine Instanz von der `IImageUpdated` Schnittstelle die wird aufgerufen, wenn das Bild ha s geladen wurde.

Der folgende Code lädt beispielsweise ein Bild aus einer Url in eine `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader-Klasse macht eine löschen-Methode, die Sie aufrufen können, wenn alle Bilder freizugeben, die derzeit im Arbeitsspeicher zwischengespeichert werden sollen. Der aktuelle Code verfügt über einen Cache für 50 Bilder. Wenn eine anderer Cache-Größe verwenden (z. B. Wenn Sie erwarten, sind die Bilder zu groß werden, dass 50 Bilder zu viel wäre) möchten, können Sie nur Instanzen ImageLoader erstellen und übergeben Sie die Anzahl der Images im Cache gespeichert werden sollen.

## <a name="using-linq-to-create-element-hierarchy"></a>Erstellen Sie die Hierarchie der Elemente mithilfe von LINQ

Über die intelligente Verwendung von LINQ und # Initialization-Syntax kann LINQ verwendet werden, um eine Hierarchie der Elemente zu erstellen. Beispielsweise der folgende Code erstellt einen Bildschirm von einigen Zeichenfolgen-Arrays und des Handles-Auswahl über eine anonyme Funktion, die in jede übergeben wird `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

Dies kann problemlos mit einem XML-Datenspeicher oder die Daten aus einer Datenbank zum Erstellen von komplexer Anwendungen nahezu vollständig aus der Daten kombiniert werden.

## <a name="extending-mtd"></a>Erweitern von MT. D

### <a name="creating-custom-elements"></a>Erstellen von benutzerdefinierten Elementen

Sie können Ihr eigenes Element durch Erben von entweder ein vorhandenes Element oder durch Ableiten von der Stammklasse Element erstellen.

Um Ihr eigenes Element zu erstellen, sollten Sie die folgenden Methoden außer Kraft setzen:

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

Wenn das Element eine Variable Größe haben kann, müssen Sie implementieren die `IElementSizing` -Schnittstelle, die eine Methode enthält:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Wenn Sie, auf die Implementierung Planen Ihrer `GetCell` Methode durch Aufrufen von `base.GetCell(tv)` und die zurückgegebene Zelle angepasst haben, müssen Sie auch überschreiben die `CellKey` Eigenschaft, um einen Schlüssel zurückzugeben, die nur für das Element, werden wie folgt aus:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Dies funktioniert für die meisten Elemente, aber nicht für die `StringElement` und `StyledStringElement` als diese ihren eigenen Satz von Schlüsseln für verschiedene Szenarien für Rendering verwenden. Sie müssten den Code in diesen Klassen zu replizieren.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Sowohl der Elemente-API als auch die Reflektion verwenden die gleiche `DialogViewController`. Manchmal möchten zum Anpassen der Darstellung der Ansicht oder möglicherweise möchten Sie einige Funktionen verwenden die `UITableViewController` hinausgehen, die die einfache Erstellung von Benutzeroberflächen.

Die `DialogViewController` ist lediglich eine Unterklasse von der `UITableViewController` und Sie können es anpassen, auf die gleiche Weise, die Sie anpassen, würde ein `UITableViewController`.

Z. B., wenn Sie so ändern Sie die Formatvorlage entweder werden `Grouped` oder `Plain`, Sie können diesen Wert festlegen, durch Ändern der Eigenschaft, wenn Sie den Controller, wie folgt erstellen:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Für erweiterte Anpassungen der `DialogViewController`, z. B. das Festlegen des Hintergrunds, würde Sie Unterklasse es und überschreiben Sie die richtige Methoden, wie im folgenden Beispiel gezeigt:

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

Ein weiterer Aspekt der Anpassung ist die folgenden virtuellen Methoden in der `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Diese Methode sollte eine Unterklasse von zurückgeben `DialogViewController.Source` für Fälle, in denen die Zellen sind gleich großen, oder eine Unterklasse von `DialogViewController.SizingSource` Wenn die Zellen ungleich sind.

Können Sie diese Außerkraftsetzung Erfassen der `UITableViewSource` Methoden. Z. B. [TweetStation](https://github.com/migueldeicaza/TweetStation) verwendet diese, um nachzuverfolgen, wenn der Benutzer an den Anfang ein Bildlauf durchgeführt wurde, und aktualisieren Sie entsprechend die Anzahl der ungelesenen Tweets.

## <a name="validation"></a>Validierung

-Elemente bieten keine Validierung selbst als die Modelle, die sich gut für die Webseiten sind, und desktop-Anwendungen nicht direkt an das iPhone Interaktionsmodell zugeordnet.

Wenn Sie die datenüberprüfung möchten, sollten Sie dies tun, wenn der Benutzer eine Aktion mit den eingegebenen Daten auslöst. Zum Beispiel eine <span class="ui">Fertig</span> oder <span class="ui">Weiter</span> Schaltfläche auf der oberen Symbolleiste, oder einige `StringElement` als Schaltfläche verwendet wird, um auf die nächste Stufe zu gelangen.

Dies ist, in denen Sie grundlegende Validierung von Benutzereingaben ausführen würden, und möglicherweise kompliziertere Validierung, z. B. für die Gültigkeit einer Benutzername und Kennwort-Kombination mit einem Server überprüfen.

Wie Sie die Benutzer über einen Fehler zu benachrichtigen ist anwendungsspezifisch. Sie Popup konnte eine `UIAlertView` oder einen Hinweis angezeigt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel behandelt einen Großteil der Informationen zu MonoTouch.Dialog. Es erläutert die Grundlagen der Verwendung MT. D funktioniert und behandelt die verschiedenen Komponenten MT. D. Es hat auch gezeigt, die Vielzahl von Elementen und Tabelle Anpassungen von MT. unterstützt werden D und erläutert, wie MT. D kann mit benutzerdefinierten Elementen erweitert werden. Außerdem erläutert er die JSON-Unterstützung in MT. D, die ermöglicht, Elemente dynamisch aus JSON-Datei erstellen.


## <a name="related-links"></a>Verwandte Links

- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Reflektions-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Exemplarische Vorgehensweise: Erstellen einer Benutzeroberfläche mithilfe eines JSON-Elements](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog-JSON-Markup](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController-Klassenreferenz](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
