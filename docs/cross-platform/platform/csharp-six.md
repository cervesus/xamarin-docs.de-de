---
title: C# 6 der neuen Funktionen (Übersicht)
description: Die neueste Version der C#-Sprache – Version 6 – weiterhin die Sprache aus, um weniger Textbausteine und Klarheit mehr Konsistenz wurden weiterentwickelt. Bereinigung – Syntax zur objektinitialisierung, die Möglichkeit, mit "await" in Catch/finally-Blöcke und die Null-Bedingung? Operator sind besonders nützlich.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.technology: xamarin-cross-platform
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 2a189a19280576876e5d5a6a4fa34d2d00cab330
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="c-6-new-features-overview"></a>C# 6 der neuen Funktionen (Übersicht)

_Die neueste Version der C#-Sprache – Version 6 – weiterhin die Sprache aus, um weniger Textbausteine und Klarheit mehr Konsistenz wurden weiterentwickelt. Bereinigung – Syntax zur objektinitialisierung, die Möglichkeit, mit "await" in Catch/finally-Blöcke und die Null-Bedingung? Operator sind besonders nützlich._

Dieses Dokument bietet es sich um die neuen Funktionen von c# 6. Wird vom Compiler Mono / vollständig unterstützt, und Entwickler können mithilfe der neuen Funktionen auf allen Zielplattformen von Xamarin starten.

Dieser Artikel enthält kurze Codeausschnitte der C#-6, die grundlegende Verwendung zu demonstrieren.
Die beispielanwendung ist ein Befehlszeilenprogramm, das auf allen Zielplattformen für Xamarin ausgeführt wird und die verschiedenen Funktionen ausführt.


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Was ist neu in C#-6, indem [Xamarin University](https://university.xamarin.com/)**


## <a name="development-environment"></a>Entwicklungsumgebung

### <a name="mac"></a>Mac

* **Visual Studio für Mac** bietet Unterstützung für C#-6: Erstellen und kompilieren Sie die Xamarin-apps mit C#-6-Funktionen.
  Erfahren Sie mehr über [Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/).

### <a name="windows"></a>Windows

* **Visual Studio 2015 und 2017** und höher bieten vollständige Unterstützung für c# 6. C# 6 werden von frühere Versionen von Visual Studio nicht unterstützt.

* **Xamarin Studio für Windows** c# 6-Features im Editor nicht unterstützt.



## <a name="compiler"></a>Compiler

Der Compiler Mono c# 6-dient in Mono 4.0 und höher, also [kostenlos zum Download zur Verfügung](http://www.mono-project.com/download/).
Visual Studio für Mac aktualisiert automatisch die Mono-Installation auf Ihrem System.

Windows-Benutzer benötigen [Visual Studio 2015 oder 2017 ^](https://www.visualstudio.com/) installiert, um die C#-6-Code kompilieren (auch wenn Sie Xamarin Studio für Windows als Ihre IDE).

^ oder *[Microsoft Build Tools 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48159)* , z. B. für Befehl Zeile Kompilierung oder Buildserver.

## <a name="using-c-6"></a>Mithilfe von c# 6

Der Compiler von c# 6 wird in allen früheren Versionen von Visual Studio für Mac verwendet.
Entitäten mit Befehlszeilencompiler sollten Sie sicherstellen, dass `mcs --version` gibt 4.0 oder höher.
Visual Studio für Mac-Benutzer kann die Überprüfung der besäßen sie Mono 4 (oder höher) installiert werden, durch einen Verweis auf **zu Visual Studio für Mac > Visual Studio für Mac > Details anzeigen**.

## <a name="less-boilerplate"></a>Weniger Textbausteine
### <a name="using-static"></a>verwendet statische
Enumerationen und bestimmte Klassen wie z. B. `System.Math`, sind in erster Linie Inhaber von Funktionen und statische Werte. In C#-6, importieren Sie alle statischen Member eines Typs mit einem einzelnen `using static` Anweisung. Vergleichen Sie eine typische trigonometrische Funktion in c# 5 und 6 c#:

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` führt nicht dazu, dass öffentliche `const` Felder, wie z. B. `Math.PI` und `Math.E`, direkt zugegriffen werden:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>Verwenden von statischen mit Erweiterungsmethoden [c#]

Die `using static` Funktion arbeitet mit Erweiterungsmethoden etwas anders. Obwohl Erweiterungsmethoden geschrieben sind mit `static`, sie nicht sinnvoll ohne eine Instanz, mit denen Sie arbeiten. Daher `using static` wird verwendet, mit einem Typ, der definiert, Erweiterungsmethoden, die Erweiterungsmethoden, die für ihre Zieltyp verfügbar (der Methode `this` Typ). Z. B. `using static System.Linq.Enumerable` können verwendet werden, um die API der erweitern `IEnumerable<T>` Objekte, ohne dass in allen LINQ-Typen zu schalten:

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

Im vorherige Beispiel veranschaulicht die Unterschiede im Verhalten: der Erweiterungsmethode `Enumerable.Where` bezieht sich auf das Array, während die statische Methode `String.Join` aufgerufen werden kann, ohne Verweis auf die `String` Typ.

### <a name="nameof-expressions"></a>Nameof Ausdrücke
In einigen Fällen, die Sie verweisen möchten, den Namen zugewiesen haben eine Variable oder ein Feld. In c# 6 `nameof(someVariableOrFieldOrType)` wird die Zeichenfolge zurückzugeben `"someVariableOrFieldOrType"`. Z. B. beim Auslösen einer `ArgumentException` voraussichtlich sehr Sie den Namen ein, welches Argument ungültig ist:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Der Hauptvorteil der `nameof` Ausdrücke ist, dass sie hinsichtlich des Typs überprüft und mit Umgestaltung Tool Energieversorgung kompatibel sind. Die typüberprüfung des `nameof` Ausdrücken ist besonders in Situationen Willkommen, in denen ein `string` wird verwendet, um Typen dynamisch zuzuordnen. Für die Instanz, in der iOS-eine `string` dient zur Angabe verwendet, um Prototyp `UITableViewCell` Objekte in einer `UITableView`. `nameof` kann zu gewährleisten, dass diese Zuordnung nicht aufgrund eines falsch geschrieben oder minderwertigem Umgestaltung fehlschlägt:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Obwohl Sie einen qualifizierten Namen zu übergeben, können `nameof`, nur dem letzten Element (nach dem letzten `.`) wird zurückgegeben. Beispielsweise können Sie eine Datenbindung in Xamarin.Forms hinzufügen:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Die beiden Aufrufe von `SetBinding` identische Werte übergeben werden: `nameof(ReactiveType.StringField)` ist `"StringField"`, nicht `"ReactiveType.StringField"` wie zu Beginn erwarten.

## <a name="null-conditional-operator"></a>NULL-Bedingung-Operator
Frühere Updates für C#-eingeführt, die Konzepte von auf NULL festlegbaren Typen und die Null-Sammeloperator `??` um den Standardcode zu reduzieren beim Behandeln von NULL-Werte. C# 6-weiterhin das Design mit dem "Null-Bedingung-Operator" `?.`. Wenn für ein Objekt auf der rechten Seite eines Ausdrucks verwendet wird, gibt die Null-Bedingung-Operator den Elementwert ist das Objekt nicht `null` und `null` andernfalls:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Beide `length0` und `length1` Typ abgeleitet werden `int?`)

Die letzte Zeile in das vorherige Beispiel zeigt die `?` Null-Bedingung-Operator in Kombination mit der `??` Null-Sammeloperator. Gibt der neue C#-6-Null-Bedingung-Operator `null` auf den 2. Element im Array, an welchem Punkt die Null-Sammeloperator zum Einsatz kommt und liefert eine 0 bis der `lengths` array (ob, die eignet sich oder nicht natürlich spezifische Problem ist).

Der Null-Bedingung-Operator sollte die Textbaustein Null-Prüfung erforderlich in vielen, vielen Anwendungen erheblich reduzieren.

Es gibt einige Einschränkungen auf die Null-Bedingte Operatoren aufgrund von Mehrdeutigkeiten. Sie können nicht unmittelbar folgen einem `?` mit einer Argumentliste in Klammern als Sie möglicherweise hoffen, dass mit Delegaten möchten:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Allerdings `Invoke` dienen zum Trennen der `?` aus der Argumentliste und ist immer noch eine markierte Verbesserung gegenüber einer `null`-Codeblock Textbaustein überprüfen:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Zeichenfolgeninterpolierung
Die `String.Format` Funktion wurde bisher verwendet Indizes als Platzhalter in der Formatzeichenfolge, z. B. `String.Format("Expected: {0} Received: {1}.", expected, received`). Hinzufügen eines neuen Werts ist natürlich immer eine störende wenig Aufgabe Argumente gezählt, neunumerierung Platzhalter und das neue Argument in der richtigen Reihenfolge in der Argumentliste einfügen beteiligt.

C# 6 der neuen Zeichenfolge Interpolation Funktion erheblich verbessert `String.Format`. Jetzt können Sie direkt Variablen in eine Zeichenfolge mit dem Präfix Benennen einer `$`. Zum Beispiel:

```csharp
$"Expected: {expected} Received: {received}."
```

Variablen natürlich überprüft werden, und eine Variable falsch geschrieben oder nicht verfügbaren einen Compilerfehler verursacht wird.

Die Platzhalter erübrigt sich einfache Variablen werden, sie können ein beliebiger Ausdruck sein. Innerhalb dieser Platzhalter verwenden Sie Anführungszeichen *ohne* Escapezeichen diese Angebote. Beachten Sie z. B. die `"s"` in der folgenden:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Zeichenfolgeninterpolierung unterstützt, die Ausrichtung und die Formatierung Syntax `String.Format`. Wie bereits mitgeteilt haben `{index, alignment:format}`, in C#-6, die Sie schreiben `{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```
Ergebnisse in:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

Zeichenfolgeninterpolierung ist syntaktischer Füllstoff für `String.Format`: Es kann nicht verwendet werden, mit `@""` Zeichenfolgenliterale und ist nicht kompatibel mit `const`, auch wenn keine Platzhalter verwendet werden:

```csharp
const string s = $"Foo"; //Error : const requires value
```

Im allgemeinen-Anwendungsfall Funktionsargumente mit zeichenfolgeninterpolierung erstellen müssen Sie weiterhin mit Escapezeichen zu versehen, Codierung und Kultur Probleme achten. SQL- und URL-Abfragen sind natürlich wichtig, zu bereinigen. Wie bei `String.Format`, string Interpolation verwendet die `CultureInfo.CurrentCulture`. Mithilfe von `CultureInfo.InvariantCulture` etwas mehr umfangreich ist:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Initialisierung

C# 6 bietet eine Reihe von präzise Möglichkeiten zum Festlegen von Eigenschaften, Felder und Member.

### <a name="auto-property-initialization"></a>Initialisierung der Auto-Eigenschaft

Auto-Eigenschaften können jetzt genauso präziser als Felder initialisiert werden. Unveränderliche Auto-Eigenschaften können mit nur einen Getter geschrieben werden:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

Im Konstruktor können Sie den Wert einer nur-Auto-Eigenschaft festlegen:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

Diese Initialisierung des Auto-Eigenschaften ist eine allgemeine speicherplatzsparende-Funktion und ein Entwickler möchte, um Unveränderlichkeit in ihren Objekten zu betonen gesteigert werden.

### <a name="index-initializers"></a>Indexinitialisierer

C# 6 führt indexinitialisierer, die können Sie den Schlüssel und den Wert in Typen festlegen, die über einen Indexer verfügen. Dies ist normalerweise für `Dictionary`-Datenstrukturen formatieren:

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>Ausdruck ohne Text Funktionsmember

Lambda-Funktionen haben mehrere Vorteile, von denen Speicherplatz einfach gespeichert ist. Auf ähnliche Weise erlauben Ausdruck ohne Text Klassenmember kleine Funktionen etwas prägnanter als in früheren Versionen von c# 6 möglich war ausgedrückt werden.

Ausdruck ohne Text Funktionsmember verwenden Lambda-pfeilsyntax anstatt der herkömmlichen Block-Syntax:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Beachten Sie, dass der Lambda-pfeilsyntax keiner expliziten verwenden `return`. Bei Funktionen zurückgegebene `void`, der Ausdruck muss eine Anweisung auch aufweisen:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

Ausdruck ohne Text-Elemente, unterliegen auch weiterhin die Regel, die `async` wird für Methoden, aber keine Eigenschaften unterstützt:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Ausnahmen

Es gibt keine zwei Möglichkeiten zu: Behandlung von Ausnahmen ist schwer richtig machen. Neue Funktionen in c# 6-Stellen Ausnahmebehandlung, flexibler und konsistent.

### <a name="exception-filters"></a>Ausnahmefilter

Laut Definition sind Ausnahmen in besonderen Fällen auftreten und es kann sehr schwer zu Ursache und Code zu *alle* die Möglichkeiten, eine Ausnahme eines bestimmten Typs auftreten. C# 6 führt die Möglichkeit, einen Handler für die Ausführung mit einem Filter Runtime ausgewertet zu schützen. Dies erfolgt durch Hinzufügen einer `when (bool)` Muster nach der normalen `catch(ExceptionType)` Deklaration. In den folgenden Unterschieden ein Filter einen Analysefehler, die im Zusammenhang mit der `date` Parameter im Gegensatz zu anderen Analysefehler.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>... "await" in Catch schließlich...

Die `async` in c# 5 eingeführte Funktionen wurden Maßstäbe für die Sprache. In c# 5 `await` war nicht zulässig `catch` und `finally` blockiert wird, anhand des Werts eines aufwendig die `async/await` Funktion. C# 6-entfernt diese Einschränkung ermöglicht die asynchrone Ergebnisse einheitlich über das Programm gewartet wird, wie im folgenden Codeausschnitt gezeigt:

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>Zusammenfassung

Die C#-Sprache ständig weiterentwickelt um Entwickler produktiver, auch bewährte heraufstufen und Tools unterstützen. Dieses Dokument hat einen Überblick über die neuen Sprachfeatures in c# 6- und hat kurz gezeigt, wie sie verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Neue Sprachfeatures in c# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [Mithilfe von c# 6 Arbeitsmappe](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
