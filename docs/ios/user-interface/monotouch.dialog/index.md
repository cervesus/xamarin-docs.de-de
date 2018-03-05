---
title: "Einführung in MonoTouch.Dialog"
description: "Die MonoTouch.Dialog (MT. D) Toolkit ist ein unverzichtbares Framework für die schnelle Entwicklung von Benutzeroberflächen in Xamarin.iOS-Anwendung. MT. D ganz schnell und einfach komplexe Anwendungsbenutzeroberfläche bei Verwendung eines deklarativen Ansatzes, anstatt den zeitlichen Aufwand der Navigation Controller, Tabellen usw. zu definieren. Darüber hinaus MT. D ist einen flexiblen Satz von APIs, die bieten Entwicklern eine vollständige Kontrolle oder übergibt die Ansatz sowie zusätzliche Funktionen wie Pull zum Aktualisieren, Hintergrundbild laden, Support und dynamische UI generieren über JSON-Daten zu suchen. Dieses Handbuch enthält die verschiedenen Methoden zum Arbeiten mit MT. D, und klicken Sie dann tief in Verwendung erweiterter Artikel."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b279f3e643e008e88b8ad086c400d992427c6df4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-monotouchdialog"></a>Einführung in MonoTouch.Dialog

_Die MonoTouch.Dialog (MT. D) Toolkit ist ein unverzichtbares Framework für die schnelle Entwicklung von Benutzeroberflächen in Xamarin.iOS-Anwendung. MT. D ganz schnell und einfach komplexe Anwendungsbenutzeroberfläche bei Verwendung eines deklarativen Ansatzes, anstatt den zeitlichen Aufwand der Navigation Controller, Tabellen usw. zu definieren. Darüber hinaus MT. D ist einen flexiblen Satz von APIs, die bieten Entwicklern eine vollständige Kontrolle oder übergibt die Ansatz sowie zusätzliche Funktionen wie Pull zum Aktualisieren, Hintergrundbild laden, Support und dynamische UI generieren über JSON-Daten zu suchen. Dieses Handbuch enthält die verschiedenen Methoden zum Arbeiten mit MT. D, und klicken Sie dann tief in Verwendung erweiterter Artikel._


MonoTouch.Dialog, bezeichnet als MT. D kurz, ist eine schnelle Entwicklung Toolkit, die Entwicklern ermöglicht, die Anwendung Bildschirme und die Navigation mit Informationen, anstatt den zeitlichen Aufwand des Erstellens von View-Controller, Tabellen usw. ausgebaut. Daher wird eine erhebliche Vereinfachung des Entwicklungs- und Code Reduzierung der Benutzeroberfläche bereitgestellt. Betrachten Sie beispielsweise das folgende Bildschirmfoto an:

 [ ![](images/image1.png "Angenommen, haben Sie diesen Screenshot mit")](images/image1.png)

Der folgende Code wurde verwendet, um diese gesamten Bildschirm zu definieren:

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

Bei der Arbeit mit Tabellen in iOS wird häufig eine Unmenge sich wiederholende Code.
Jedes Mal, wenn eine Tabelle erforderlich ist, wird z. B. eine Datenquelle erforderlich, um diese Tabelle aufzufüllen. Unter einer Anwendung mit zwei tabellenbasierte Bildschirme, die über eine Navigation Controller verbunden sind, gelten für jeden Bildschirm ein Großteil der gleiche Code.

MT. D zu vereinfachen, die durch die Kapselung aller diesen Code in eine allgemeine API zum Erstellen der Tabelle. Es stellt eine Abstraktionsebene über diese API, die für eine deklarative Objekt binden die Syntax, die auch leichter ermöglicht bereit. Daher stehen zwei APIs in MT. D:

-   **Low-Level-API für Elemente** – die *Elemente API* basiert auf eine hierarchische Struktur von Elementen, die darstellen, Bildschirme und ihre Komponenten erstellen. Die API-Elemente bietet Entwicklern die größte Flexibilität und Kontrolle bei der Erstellung von Benutzeroberflächen bereit. Die API-Elemente bietet darüber hinaus Unterstützung für die deklarative Definition über JSON, erweiterte sowohl äußerst schnell Deklaration als auch dynamische UI Generieren von einem Server ermöglicht. 
-   **Allgemeine Reflektions-API** – auch bekannt als die *binden**API* , in die Klassen mit UI-Hinweise, und klicken Sie dann MT. versehen werden D automatisch Bildschirme, die auf der Grundlage der Objekte erstellt und bietet eine Bindung zwischen Was wird angezeigt (und optional bearbeitet) auf dem Bildschirm und die zugrunde liegende Objekt sichern.   Das obige Beispiel veranschaulicht die Verwendung von Reflektions-API. Diese API nicht die Differenzierte Steuerung, die die API-Elemente bieten, aber es automatisch Ausbau der Hierarchie des Elements basierend auf den Klassenattributen Komplexität noch weiter verringert. 


MT. D stammen mit einem umfangreichen Satz von gepackte im UI-Elemente für die Erstellung der Bildschirm erstellt, aber auch das Erfordernis für benutzerdefinierte Elemente und erweiterte Bildschirmlayouts erkennt. Daher ist die Erweiterbarkeit, dass eine erstklassige baked in der API-Funktionsumfang. Entwickler können die vorhandenen Elemente erweitern oder neue erstellen und dann nahtlos integrieren.

Darüber hinaus MT. D verfügt über eine Reihe von allgemeinen iOS-UX-Funktionen, die erstellt werden, z. B. "zum Aktualisieren ziehen" Unterstützung, asynchrone Bild geladen ist, und suchen die Unterstützung.

In diesem Artikel dauert einen umfassenden Einblick in die Arbeit mit MT. D, einschließlich:

-   **MT. D-Komponenten** – dies konzentriert sich auf das Verstehen der Klassen, aus denen MT. D So aktivieren Sie die erste schnell zu beschleunigen. 
-   **Elementreferenz** – eine umfassende Liste der integrierten Elemente des MT. D. 
-   **Erweiterte Verwendung** – Dies umfasst erweiterte Funktionen wie z. B. zum Aktualisieren ziehen, Suche, Background-Image laden, mithilfe von LINQ Element Hierarchien ausgebaut und erstellen benutzerdefinierte Elemente, Zellen, und verwenden Sie den Controller für mit MT. D. 

## <a name="understanding-the-pieces-of-mtd"></a>Grundlegendes zu den Angaben MT. D

Selbst bei Verwendung der Reflektions-API, MT. D wird eine Hierarchie der Elemente im Endeffekt erstellt, als ob sie direkt über die API-Elemente erstellt wurden. Außerdem erstellt der JSON-Unterstützung, die im vorherigen Abschnitt erwähnt auch Elemente aus. Aus diesem Grund ist es wichtig, dass ein grundlegendes Verständnis der einzelnen Teile der MT. D.

MT. D-builds Bildschirme, die mit den folgenden vier Teilen:

-  **DialogViewController**
-  **RootElement**
-  **Bereich**
-  **Element**


### <a name="dialogviewcontroller"></a>DialogViewController

Ein *DialogViewController*, oder *DVC* kurz, erbt Sie von `UITableViewController` und stellt daher einen Bildschirm mit einer Tabelle dar. DVCs kann auf einen Controller Navigation, wie eine reguläre UITableViewController abgelegt werden.

### <a name="rootelement"></a>RootElement

Ein *RootElement* Container der obersten Ebene für die Elemente, die in einem DVC aufgenommen wird. Es enthält Abschnitte, die dann Elemente enthalten kann. RootElements werden nicht gerendert. Stattdessen können sie einfach ein Container für was tatsächlich gerendert wird. Eine RootElement eine DVC zugewiesen ist, und die DVC rendert dann seine untergeordneten Elemente.

### <a name="section"></a>Bereich

Ein Abschnitt ist eine Gruppe von Zellen in einer Tabelle. Wie eine normale Tabelle Abschnitt er optional kann werden eine Kopf- und Fußzeile, die entweder können Text oder sogar benutzerdefinierten Ansichten, der folgende Screenshot zeigt:

 [ ![](images/image2.png "Wie eine normale Tabelle Abschnitt er optional kann werden eine Kopf- und Fußzeile, die entweder können Text oder sogar benutzerdefinierten Ansichten, wie in diesem screenshot")](images/image2.png)

### <a name="element"></a>Element

Ein Element stellt eine tatsächliche Zelle in der Tabelle dar. MT. D kommt gepackte mit einer Vielzahl von Elementen, die unterschiedliche Datentypen oder andere Eingaben darstellen. Die folgenden Screenshots veranschaulicht z. B. nur einige der verfügbaren Elemente:

 [ ![](images/image3.png "Diese Screenshots zeigen z. B. nur einige der verfügbaren Elemente")](images/image3.png)

## <a name="more-on-sections-and-rootelements"></a>Weitere Abschnitte ein "und" RootElements

Jetzt betrachten wir RootElements und Abschnitten ausführlicher.

### <a name="rootelements"></a>RootElements

Mindestens ein RootElement ist erforderlich, um den MonoTouch.Dialog-Prozess zu starten.

Wenn eine RootElement mit einem Wert Section-Element initialisiert wird, wird dieser Wert verwendet, um ein untergeordnetes Element zu suchen, die auf der rechten Seite der Anzeige gerendert wird eine Zusammenfassung der Konfiguration bereitstellen wird. Der folgende Screenshot zeigt z. B. eine Tabelle auf der linken Seite mit einer Zelle, die den Titel des Bildschirms Detail auf das Recht "Dessert", zusammen mit den Wert des ausgewählten Desert enthält.

 [ ![](images/image4.png "Diese Abbildung zeigt eine Tabelle auf der linken Seite mit einer Zelle mit dem Titel des Bildschirms Details auf der rechten Seite Dessert, zusammen mit den Wert des ausgewählten Desert") ](images/image4.png) [ ![ ] (images/image5.png "dies Screenshot unten zeigt eine Tabelle auf der linken Seite mit einer Zelle mit dem Titel des Bildschirms Details auf der rechten Seite Dessert, zusammen mit den Wert des ausgewählten desert")](images/image5.png)

Stammelemente können auch in Abschnitten dienen zum Laden einer neuen geschachtelten Konfigurationsseite auslösen wie oben gezeigt. Bei der Verwendung in diesem Modus wird die angegebene Beschriftung wird verwendet, während in einem Abschnitt gerendert und dient auch als Titel für die Unterseite. Zum Beispiel:

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

Im obigen Beispiel beim Tippen auf "Dessert", MonoTouch.Dialog erstellt eine neue Seite und navigieren sie mit dem Stamm wird "Dessert" und das Vorhandensein einer Radio Gruppe mit drei Werten.

In diesem speziellen Beispiel wird der Optionsfeldgruppe "Schokoladenkuchen" im Abschnitt "Dessert" auswählen, da wir den Wert "2" an die RadioGroup übergeben. Dies bedeutet das 3. Element in der Liste (0-Index) auswählen.

Die Add-Methode aufrufen, oder verwenden die Syntax des Objektinitialisierers c# 4 fügt Abschnitte.
Einfügemethoden dienen zum Einfügen von Abschnitten mit einer Animation.

Bei der Erstellung von RootElement mit Gruppeninstanz (statt einer RadioGroup) werden zusammenfassende Wert ist der RootElement bei der Anzeige in einem Abschnitt der kumulierten Anzahl aller BooleanElements und CheckboxElements, die den gleichen Schlüssel wie die Group.Key-Wert aufweisen.

### <a name="sections"></a>Abschnitte

Abschnitte dienen zum Gruppieren von Elementen in den Bildschirm, und sie sind die einzigen gültigen direkte untergeordnete Elemente des RootElement. Abschnitte darf keines der Standardelemente, einschließlich der neuen RootElements.

Eingebettet in einem Abschnitt RootElements werden verwendet, um zu einer neuen tieferen Ebene zu navigieren.

Abschnitte können Kopf- und Fußzeilen entweder als Zeichenfolgen oder als UIViews aufweisen.
In der Regel verwenden Sie nur die Zeichenfolgen, aber Sie können alle UIView als die Kopf- oder Fußzeile verwenden, um benutzerdefinierte Benutzeroberflächen zu erstellen. Sie können entweder eine Zeichenfolge verwenden, um sie wie folgt erstellen:

```csharp
var section = new Section ("Header", "Footer")
```

Um Sichten zu verwenden, müssen Sie nur übergeben Sie die Ansichten an den Konstruktor:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Erhalten von Benachrichtigungen

#### <a name="handling-nsaction"></a>Behandlung von NSAction

MT. D Flächen ein `NSAction` als einen Delegaten für die Behandlung von Rückrufen.
Angenommen Sie, dass das Behandeln eines Ereignisses Touch bei einer Tabellenzelle MT. erstellt werden soll D. Beim Erstellen eines Elements mit MT. D, einfach geben eine Rückruffunktion, wie unten dargestellt:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Abrufen von Elementwert

In Kombination mit der `Element.Value` Eigenschaft kann der Rückruf in andere Elemente der festgelegte Wert abzurufen. Beachten Sie z. B. folgenden Code:

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

Dieser Code erstellt eine Benutzeroberfläche, wie unten dargestellt. Eine vollständige Exemplarische Vorgehensweise dieses Beispiels finden Sie unter der [Elemente API Exemplarische Vorgehensweise](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) Lernprogramm.

 [ ![](images/image6.png "In Kombination mit der Element.Value-Eigenschaft kann der Rückruf Abrufen des Werts in andere Elemente festlegen")](images/image6.png)

Wenn der Benutzer die unterste Tabellenzelle drückt, der Code in der anonymen Funktion ausgeführt wird, schreiben den Wert aus der `element` -Instanz, auf die **Anwendungsausgabe** Pad in Visual Studio für Mac.

## <a name="built-in-elements"></a>Integrierte Elemente

MT. D enthält eine Reihe von integrierten Tabelle Zelle Elementen als Elemente bezeichnet.
Diese Elemente werden verwendet, um eine Vielzahl verschiedener Typen in Tabellenzellen, z. B. Zeichenfolgen, Gleitkommazahlen, Datumsangaben und auch Images nur einige zu nennen anzuzeigen. Jedes Element übernimmt den Datentyp entsprechend angezeigt. Beispielsweise wird ein boolesches Element einen Schalter zum Umschalten des Objektwerts angezeigt. Ebenso wird ein Float-Element einen Schieberegler zum Ändern des Werts "float" angezeigt.

Es gibt auch komplexere Elemente zur Unterstützung von umfangreichere Datentypen wie z. B. Bilder und html. Ein html-Element, ein UIWebView zum Laden einer Webseite, die bei Auswahl öffnen, zeigt z. B. eine Beschriftung in der Tabellenzelle.

### <a name="working-with-element-values"></a>Arbeiten mit Elementwerte

Elemente, die verwendet werden, um Benutzereingaben zu erfassen verfügbar zu machen einen öffentlichen `Value` Eigenschaft, die den aktuellen Wert des Elements zu einem beliebigen Zeitpunkt enthält. Es wird automatisch aktualisiert, als der Benutzer die Anwendung verwendet.

Dies ist das Verhalten für alle Elemente, die Teil der MonoTouch.Dialog sind, aber es ist nicht erforderlich, damit Benutzer erstellten Elemente.

### <a name="string-element"></a>Zeichenfolgenelement

Ein `StringElement` zeigt eine Beschriftung auf der linken Seite einer Tabellenzelle und den Zeichenfolgenwert auf der rechten Seite der Zelle.

 [ ![](images/image7.png "Ein StringElement zeigt eine Beschriftung auf der linken Seite einer Tabellenzelle und den Zeichenfolgenwert auf der rechten Seite der Zelle")](images/image7.png)

Verwenden einer `StringElement` als Schaltfläche, einen Delegaten bereitstellen.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [ ![](images/image8.png "Um eine StringElement als Schaltfläche verwenden möchten, geben Sie einen Delegaten")](images/image8.png)

### <a name="styled-string-element"></a>Formatierte Zeichenfolgenelement

Ein `StyledStringElement` Zeichenfolgen, die mit entweder Zellenstile integrierte Tabelle präsentiert werden können oder eine benutzerdefinierte Formatierung.

 [ ![](images/image9.png "Eine StyledStringElement kann Zeichenfolgen mit entweder Zellenstile integrierte Tabelle dargestellt werden soll oder eine benutzerdefinierte Formatierung")](images/image9.png)

Die `StyledStringElement` Klasse leitet sich von `StringElement`, jedoch können Entwickler anpassen eine Handvoll Eigenschaften, z. B. die Schriftart, die Textfarbe, Hintergrundfarbe der Zelle, wichtige Zeilenmodus, Anzahl der Zeilen angezeigt, und gibt an, ob Zubehör angezeigt werden soll.

### <a name="multiline-element"></a>Multiline-Element

 [ ![](images/image10.png "Multiline-Element")](images/image10.png)

### <a name="entry-element"></a>Entry-Element

Die `EntryElement`, wie der Name impliziert, wird verwendet, um Benutzereingaben zu erhalten. Unterstützt reguläre Zeichenfolgen oder Kennwörter, wobei Zeichen ausgeblendet sind.

 [ ![](images/image11.png "Die EntryElement wird verwendet, um Benutzereingaben zu erhalten")](images/image11.png)

Es wird mit drei Werten initialisiert:

-  Die Beschriftung für den Eintrag, der dem Benutzer angezeigt wird.
-  Der Platzhaltertext ist (Dies ist der abgeblendet horizontaler Text, der einen Hinweis für dem Benutzer). 
-  Der Wert des Texts.


Der Platzhalter und der Wert können null sein. Die Beschriftung ist jedoch erforderlich.

Zu jedem Zeitpunkt den Zugriff auf die Value-Eigenschaft der Abrufen des Werts eines kann die `EntryElement`.

Darüber hinaus die `KeyboardType` Eigenschaft kann zum Zeitpunkt der Erstellung auf der Tastatur Schriftstil PowerShell DSC für die Dateneingabe festgelegt werden. Dies kann verwendet werden, so konfigurieren Sie die Tastatur, die mit den Werten der `UIKeyboardType` wie im folgenden aufgeführt:

-  Numeric
-  Telefon
-  URL
-  E-Mail


### <a name="boolean-element"></a>Boolean-Element

 [ ![](images/image12.png "Boolean-Element")](images/image12.png)

### <a name="checkbox-element"></a>CheckBox-Element

 [ ![](images/image13.png "CheckBox-Element")](images/image13.png)

### <a name="radio-element"></a>Sender-Element

Ein `RadioElement` erfordert eine `RadioGroup` in angegeben werden die `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [ ![](images/image14.png "Ein RadioElement erfordert eine RadioGroup in RootElement angegeben werden")](images/image14.png)

 `RootElements` Koordinieren von Optionsfeld-Elemente werden auch verwendet werden. Die `RadioElement` Elemente können mehrere Abschnitte (z. B. etwas Ähnliches wie der Ring Ton Selektor und die eigenen benutzerdefinierten Klingeltöne aus System Klingeltöne implementieren) umfassen. Die Zusammenfassungsansicht zeigt das Optionsfeld-Element, das derzeit ausgewählt ist. Um dies zu verwenden, erstellen die `RootElement` mit dem Group-Konstruktor wie folgt:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Der Name der Gruppe in `RadioGroup` wird verwendet, um den ausgewählten Wert der enthaltenden Seite (sofern vorhanden) und den Wert, der 0 (null) in diesem Fall ist, wird der Index des ersten ausgewählten Elements anzeigen.

### <a name="badge-element"></a>Badge-Element

 [ ![](images/image15.png "Badge-Element")](images/image15.png)

### <a name="float-element"></a>Float-Element

 [ ![](images/image16.png "Float-Element")](images/image16.png)

### <a name="activity-element"></a>Aktivitätselements

 [ ![](images/image17.png "Aktivitätselements")](images/image17.png)

### <a name="date-element"></a>Datum-Element

 ![](images/image18.png "Datum-Element")

Wenn die Zelle, die für die DateElement ausgewählt ist, wird eine Datumsauswahl dargestellt, wie unten dargestellt:

 [ ![](images/image19.png "Wenn die Zelle für die DateElement aktiviert ist, wird eine Datumsauswahl dargestellt, wie dargestellt")](images/image19.png)

### <a name="time-element"></a>Time-Element

 [ ![](images/image20.png "Time-Element")](images/image20.png)

Wenn die Zelle, die für die TimeElement ausgewählt ist, wird eine Zeitauswahl dargestellt, wie unten dargestellt:

 [ ![](images/image21.png "Wenn die Zelle für die TimeElement aktiviert ist, eine Zeitauswahl erhält gezeigten")](images/image21.png)

### <a name="datetime-element"></a>"DateTime"-Element

 [ ![](images/image22.png ""DateTime"-Element")](images/image22.png)

Wenn die Zelle für die DateTimeElement aktiviert ist, wird eine Auswahl von "DateTime" dargestellt, wie unten dargestellt:

 [ ![](images/image23.png "Wenn die Zelle für die DateTimeElement aktiviert ist, wird eine Auswahl von "DateTime" angezeigt, wie dargestellt")](images/image23.png)

### <a name="html-element"></a>HTML-Element

 [ ![](images/image24.png "HTML-Element")](images/image24.png)

Die `HTMLElement` zeigt den Wert von dessen `Caption` Eigenschaft in der Tabellenzelle. Whe ausgewählt, die `Url` dem Element zugewiesenen wird geladen, in einem `UIWebView` steuern, wie unten dargestellt:

 [ ![](images/image25.png "Whe ausgewählt, wird die Url für das Element in einem Steuerelement UIWebView geladen werden, wie unten dargestellt")](images/image25.png)

### <a name="message-element"></a>Message-Element

 [ ![](images/image26.png "Message-Element")](images/image26.png)

### <a name="load-more-element"></a>Laden Sie weitere-Element

Verwenden Sie dieses Element, um Benutzern beim Laden von mehr Elementen in der Liste zu ermöglichen. Sie können die normalen und Beschriftungen laden als auch die Schriftart und Farbe anpassen.
Die `UIActivity` Indikator beginnt animieren, und die Beschriftung für das Laden wird angezeigt, wenn ein Benutzer auf die Zelle tippt und dann die `NSAction` übergebenen der Konstruktor wird ausgeführt. Einmal für den Code in der `NSAction` beendet ist, die `UIActivity` Indikator beendet animieren und die normale Beschriftung wird erneut angezeigt.

### <a name="uiview-element"></a>UIView-Element

Darüber hinaus benutzerdefinierten `UIView` können angezeigt werden, mithilfe der `UIViewElement`.

### <a name="owner-drawn-element"></a>Ownerdrawn-Element

Dieses Element muss als Unterklasse definiert werden, da es sich um eine abstrakte Klasse ist. Sie überschreiben, sollte die `Height(RectangleF bounds)` Methode in dem Sie die Höhe des Elements zurückgeben sollte als auch `Draw(RectangleF bounds, CGContext context, UIView view)` in dem Sie Ihre benutzerdefinierte Zeichnung innerhalb der angegebenen Grenzen tun sollten mit den Parametern und. Dieses Element ist die anstrengende Unterklassen eine `UIView`, und legen Sie das Element in der Zelle, die zurückgegeben werden, lassen Sie nur zwei einfachen Außerkraftsetzungen implementieren müssen. Sehen Sie eine bessere beispielimplementierung in der Beispiel-app in die `DemoOwnerDrawnElement.cs` Datei.

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

Die `JsonElement` ist eine Unterklasse von `RootElement` , reicht ein `RootElement` , damit der Inhalt der geschachtelte untergeordnete von einer lokalen oder remote-Url geladen werden.

Die `JsonElement` ist eine `RootElement` , die in zwei Formen instanziiert werden können. Erstellt eine Version einer `RootElement` wird, die den Inhalt bei Bedarf geladen. Diese werden erstellt, mit der `JsonElement` Konstruktoren, die ein zusätzliches Argument am Ende der Url, laden den Inhalt enthalten:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Die andere Art erstellt die Daten aus einer lokalen Datei oder eine vorhandene `System.Json.JsonObject` , die Sie haben bereits so analysiert:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Weitere Informationen zur Verwendung von JSON mit MT. D, finden Sie unter der [JSON Element Exemplarische Vorgehensweise](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) Lernprogramm.

## <a name="other-features"></a>Andere Funktionen

### <a name="pull-to-refresh-support"></a>Unterstützung zum Aktualisieren ziehen

 *Pull-zu-* *aktualisieren* ein visuellen Effekts ursprünglich befindet sich der *Tweetie2* eine beliebte Auswirkung für viele Anwendungen war-app.

Um die Dialoge automatische Pull-Refresh-Unterstützung hinzugefügt haben, müssen Sie nur zwei Dinge: Verknüpfen mit einem Ereignishandler benachrichtigt, wenn der Benutzer die Daten abruft, und benachrichtigen Sie die `DialogViewController` beim Laden der Daten wurde wurde, auf den Standardzustand zurückgesetzt.

Einbinden von einer Benachrichtigung ist einfach. nur eine Verbindung mit herstellen die `RefreshRequested` Ereignis auf der `DialogViewController`, wie folgt:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Klicken Sie dann auf die Methode `OnUserRequestedRefresh`, würden Sie einige Laden von Daten in die Warteschlange, einige Daten aus dem Netz anfordern oder starten Sie einen Thread, um die Daten zu berechnen. Sobald die Daten geladen wurden, müssen Sie benachrichtigen der `DialogViewController` , die die neuen Daten und um die Ansicht auf den Standardzustand wiederherzustellen, rufen Sie dazu `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Unterstützung der Suche

Legen Sie zur Unterstützung von Suchvorgängen, die `EnableSearch` Eigenschaft auf Ihre `DialogViewController`. Sie können auch Festlegen der `SearchPlaceholder` Eigenschaft zur Verwendung als den Wasserzeichentext in der Suchleiste.

Suchen, wird der Inhalt der Ansicht als vom Benutzer eingegebenen ändern. Es sucht die sichtbaren Feldern und zeigt diese an den Benutzer. Die `DialogViewController` macht drei Methoden, mit denen programmgesteuert starten, beenden und einen neuen Filtervorgang auf den Ergebnissen auslösen. Diese Methoden sind unten aufgeführt:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Das System ist erweiterbar und Sie können dieses Verhalten ändern, wenn Sie möchten.

### <a name="background-image-loading"></a>Laden von Images "Hintergrund"

MonoTouch.Dialog umfasst die [TweetStation](https://github.com/migueldeicaza/TweetStation) Bildladeprogramm Anwendungsverzeichnis. Diese das Bildladeprogramm stellt kann verwendet werden, um das Laden von Bildern im Hintergrund caching unterstützt und kann Ihren Code benachrichtigen, wenn das Bild geladen wurde.

Es wird auch die Anzahl der ausgehende Netzwerkverbindungen einschränken.

Das Bildladeprogramm stellt die Implementierung in der `ImageLoader` -Klasse, Sie müssen lediglich ist der Aufruf der `DefaultRequestImage` -Methode, Sie benötigen, geben Sie den Uri für das Bild, das Sie laden möchten, sowie eine Instanz von der `IImageUpdated` Schnittstelle die aufgerufen wird, wenn das Bild mit hoher Verfügbarkeit s geladen wurde.

Der folgende Code lädt beispielsweise ein Bild von einer Url in eine `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader-Klasse macht eine Purge-Methode, die Sie aufrufen können, wenn alle Bilder freizugeben, die derzeit im Arbeitsspeicher zwischengespeichert werden sollen. Der aktuelle Code verfügt über einen Cache für 50 Bilder. Wenn Sie eine anderen Cachegröße verwenden (z. B. Wenn Sie erwarten, sind die Bilder zu groß sein, dass 50 Bilder zu viel wäre) möchten, können Sie nur Instanzen ImageLoader erstellen und übergeben Sie die Anzahl von Images im Cache gespeichert werden sollen.

## <a name="using-linq-to-create-element-hierarchy"></a>Verwenden von LINQ zum Erstellen der Hierarchie der Elemente

Über cleveren Einsatzbereich von LINQ und # Syntax zur objektinitialisierung kann LINQ verwendet werden, um eine Hierarchie der Elemente zu erstellen. Beispielsweise der folgende Code erstellt einen Bildschirm aus einigen Zeichenfolgenarrays und Handles Zelle Auswahl über einer anonymen Funktion, die in jede übergeben wird `StringElement`:

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

Dies kann problemlos mit einem XML-Datenspeicher oder die Daten aus einer Datenbank zum Erstellen komplexer Anwendungen nahezu vollständig von Daten kombiniert werden.

## <a name="extending-mtd"></a>Erweitern von MT. D

### <a name="creating-custom-elements"></a>Erstellen benutzerdefinierte Elemente

Sie können Ihr eigenes Element durch Vererben von entweder ein vorhandenes Element oder durch Ableiten von der Stammklasse Element erstellen.

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

Wenn das Element eine Variable Größe aufweisen kann, müssen Sie zum Implementieren der `IElementSizing` -Schnittstelle, die eine Methode enthält:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Wenn Sie, implementieren Planen Ihrer `GetCell` Methode durch Aufrufen von `base.GetCell(tv)` und die zurückgegebene Zelle anpassen müssen Sie auch überschreiben die `CellKey` Eigenschaft, um einen Schlüssel zurückzugeben, die für das Element eindeutig werden wie folgt:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Dies funktioniert für die meisten Elemente, aber nicht für die `StringElement` und `StyledStringElement` als diese ihren eigenen Satz von Schlüsseln für verschiedene Rendering Szenarien verwenden. Sie müssen dann den Code in diesen Klassen zu replizieren.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Verwenden Sie die Reflektion und die API-Elemente den gleichen `DialogViewController`. In einigen Fällen sollten Sie zum Anpassen der Darstellung der Sicht oder möglicherweise möchten Sie einige Funktionen verwenden die `UITableViewController` hinausgehen, die die Grundlegendes zum Erstellen von Benutzeroberflächen bereit.

Die `DialogViewController` ist lediglich eine Unterklasse von der `UITableViewController` und Sie können es anpassen, auf die gleiche Weise, die Sie anpassen, würde ein `UITableViewController`.

Z. B., wenn Sie so ändern Sie die Formatvorlage entweder werden `Grouped` oder `Plain`, können Sie diesen Wert festlegen, durch Ändern der Eigenschaft, wenn Sie den Controller an, wie folgt erstellen:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Erweiterte Anpassung von der `DialogViewController`, z. B. das Festlegen des Hintergrunds, würden Sie Unterklasse es und überschreiben die entsprechenden Methoden, wie im folgenden Beispiel gezeigt:

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

Zeigen Sie eine andere Anpassung ist die folgenden virtuellen Methoden in der `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Diese Methode sollte eine Unterklasse von zurückgeben `DialogViewController.Source` für Fälle, in denen die Zellen gleich großen, oder eine Unterklasse von `DialogViewController.SizingSource` , wenn die Zellen ungleich sind.

Können Sie diese Außerkraftsetzung Erfassen der `UITableViewSource` Methoden. Beispielsweise [TweetStation](https://github.com/migueldeicaza/TweetStation) verwendet diese, um das Verfolgen der Benutzer einen Bildlauf zum Anfang des wurde und die Anzahl der ungelesenen Tweets entsprechend aktualisieren.

## <a name="validation"></a>Validierung

-Elemente bieten keine Überprüfung selbst als der Modelle, die sich gut für Webseiten sind und desktop-Anwendungen nicht direkt mit der iPhone-Interaktionsmodell zuordnen.

Wenn Sie Daten überprüfen möchten, sollten Sie dieses Verfahren, wenn der Benutzer eine Aktion mit den eingegebenen Daten ausgelöst. Z. B. eine <span class="ui">Fertig</span> oder <span class="ui">Weiter</span> Schaltfläche auf der Hauptsymbolleiste oder einige `StringElement` als Schaltfläche zu an die nächste Stufe verwendet haben.

Dies ist, in denen grundlegende Validierung von Benutzereingaben ausführen und z. B. Validierung, z. B. Überprüfen der Gültigkeit einer Benutzername und Kennwort-Kombination mit einem Server komplizierter.

Wie Sie Benachrichtigung des Benutzers ein Fehler ist anwendungsspezifisch. Sie Popup konnte eine `UIAlertView` oder einen Hinweis anzeigen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt eine Vielzahl von Informationen zu MonoTouch.Dialog. Er behandelt die Grundlagen der Verwendung MT. D funktioniert und behandelt die verschiedenen Komponenten MT. D. Es wurde auch der Palette von Elementen umfasst und Tabelle Anpassungen unterstützt durch MT. D und erläutert, wie MT. D kann mit benutzerdefinierten Elemente erweitert werden. Darüber hinaus erläutert er die JSON-Unterstützung in MT. D, die Elemente dynamisch aus JSON erstellen kann.


## <a name="related-links"></a>Verwandte Links

- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Reflektions-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Exemplarische Vorgehensweise: Erstellen einer Benutzeroberfläche mithilfe eines JSON-Elements](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON-Markup](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController-Klassenreferenz](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
