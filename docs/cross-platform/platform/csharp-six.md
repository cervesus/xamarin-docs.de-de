---
title: C#Übersicht über die der neuen Funktionen von 6
description: Die neueste Version von der C# Language – Version 6 – wird die Sprache aus, um weniger Textbausteine, bessere Übersichtlichkeit und mehr Konsistenz zu erhalten. Bereinigung Initialization-Syntax, die Möglichkeit, verwenden Sie "await" in Catch/finally-Blöcken, und der Null-bedingte? Operator sind besonders nützlich.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: b69fe417bb521781453042269b9b52609d8e00a0
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070955"
---
# <a name="c-6-new-features-overview"></a>C#Übersicht über die der neuen Funktionen von 6

_Die neueste Version von der C# Language – Version 6 – wird die Sprache aus, um weniger Textbausteine, bessere Übersichtlichkeit und mehr Konsistenz zu erhalten. Bereinigung Initialization-Syntax, die Möglichkeit, verwenden Sie "await" in Catch/finally-Blöcken, und der Null-bedingte? Operator sind besonders nützlich._

In diesem Dokument werden die neuen Features von C# 6. Wird vollständig von dem mono-Compiler unterstützt, und Entwickler können mithilfe der neuen Funktionen auf allen Zielplattformen von Xamarin starten.

> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Neuerungen in C# 6, [Xamarin University](https://university.xamarin.com/)**

## <a name="using-c-6"></a>Mithilfe von C# 6

Die C# 6-Compiler wird verwendet, in allen jüngeren Versionen von Visual Studio für Mac.
Die mit dem Befehlszeilencompiler sollten Sie sicherstellen, dass `mcs --version` gibt 4.0 oder höher.
Visual Studio für Mac-Benutzer kann überprüfen, die ggf. Mono 4 (oder höher) installiert werden, durch einen Verweis auf **zu Visual Studio für Mac > Visual Studio für Mac > Details anzeigen**.

## <a name="less-boilerplate"></a>Weniger Textbausteine
### <a name="using-static"></a>verwendet statische
Enumerationen und bestimmte Klassen wie z. B. `System.Math`, sind in erster Linie werden statische Werte und Funktionen. In C# 6, können Sie alle statischen Member eines Typs mit einem einzelnen importieren `using static` Anweisung. Vergleichen eine typischen trigonometrische Funktion C# 5 und C# 6:

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

`using static` macht nicht öffentlichen `const` Felder, z. B. `Math.PI` und `Math.E`, direkt zugegriffen werden kann:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>Verwenden von statischen mit Erweiterungsmethoden

Die `using static` Funktion arbeitet mit Erweiterungsmethoden bereit, die ein wenig anders. Obwohl Erweiterungsmethoden geschrieben sind mit `static`, sie nicht sinnvoll, ohne eine Instanz, mit denen Sie arbeiten. Dies der Fall bei `using static` wird verwendet, mit der ein Typ, definiert der Erweiterungsmethoden die Erweiterungsmethoden, die für ihre Zieltyp verfügbar (der Methode `this` Typ). Z. B. `using static System.Linq.Enumerable` können verwendet werden, um die API des erweitern `IEnumerable<T>` Objekte ohne die in der LINQ-Typen:

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

Im vorherige Beispiel veranschaulicht die Unterschiede im Verhalten: die Erweiterungsmethode `Enumerable.Where` bezieht sich auf das Array, bei der statischen Methode `String.Join` aufgerufen werden kann, ohne Verweis auf die `String` Typ.

### <a name="nameof-expressions"></a>"nameof"-Ausdrücke
In manchen Fällen für die Sie verweisen möchten Sie haben auf den Namen erteilt, eine Variable oder ein Feld. In C# 6 `nameof(someVariableOrFieldOrType)` gibt die Zeichenfolge `"someVariableOrFieldOrType"`. Z. B. beim Auslösen einer `ArgumentException` Sie ist sehr wahrscheinlich, dass Sie den Namen ein, welches Argument ungültig ist:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Der Hauptvorteil der `nameof` "Ausdrücke" ist, dass sie typgeprüfte und Refactoring Tool-gestützte kompatibel sind. Die Typprüfung des `nameof` Ausdrücke ist besonders in Situationen, in denen eine `string` verwendet, um Typen dynamisch zuzuordnen. In iOS z. B. eine `string` dient zur Angabe verwendet, um den Prototyp `UITableViewCell` Objekte in einer `UITableView`. `nameof` kann zu gewährleisten, dass diese Zuordnung nicht aufgrund eines falsch geschrieben oder Umgestalten von sorgfältig erstellten fehlschlägt:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Obwohl Sie einen qualifizierten Namen zu übergeben, können `nameof`, nur das letzte Element (nach dem letzten `.`) wird zurückgegeben. Beispielsweise können Sie eine Datenbindung in Xamarin.Forms hinzufügen:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Die beiden Aufrufe von `SetBinding` identische Werte übergeben: `nameof(ReactiveType.StringField)` ist `"StringField"`, nicht `"ReactiveType.StringField"` erwartungsgemäß zuerst.

## <a name="null-conditional-operator"></a>NULL-bedingten Operator
Zuvor updates C# eingeführt, die Konzepte von nullable-Typen und der Null-Sammeloperator `??` um die Menge Standardcode zu reduzieren, bei der Behandlung von NULL-Werten. C#6 weiterhin dieses Design, mit dem "Null-bedingten Operator" `?.`. Wenn für ein Objekt auf der rechten Seite eines Ausdrucks verwendet wird, gibt die Null-bedingten Operator den Wert der Elements ist das Objekt nicht `null` und `null` andernfalls:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Beide `length0` und `length1` abgeleitet werden, um Typ `int?`)

Die letzte Zeile in das vorherige Beispiel zeigt die `?` nullbedingungsoperators in Kombination mit der `??` Null-Sammeloperator. Die neue C# 6 Null-bedingten Operator gibt `null` im 2. Element im Array, an diesem Punkt der Null-Sammeloperator zum Einsatz kommt, und stellt der Wert 0 für die `lengths` array (ob, die eignet sich oder nicht, natürlich ist Problem-spezifisch).

Der bedingte Null-Operator sollte den Umfang der Codebaustein Null-Überprüfung erforderlich in vielen, vielen Anwendungen erheblich verringern.

Es gibt einige Einschränkungen für den bedingten Null-Operator aufgrund von Mehrdeutigkeiten. Sie können nicht direkt folgen einem `?` mit einer Argumentliste in Klammern, wie Sie möglicherweise hoffen, dass Sie mit einem Delegaten:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Allerdings `Invoke` können verwendet werden, um das Trennen der `?` aus der Argumentliste und ist immer noch eine markierte Verbesserung gegenüber einer `null`-Block häufig überprüfen:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Zeichenfolgeninterpolierung
Die `String.Format` Funktion wurde bisher verwendet Indizes als Platzhalter in der Formatzeichenfolge steht, z. B. `String.Format("Expected: {0} Received: {1}.", expected, received`). Hinzufügen eines neuen Werts hat natürlich immer eine lästige Sache zugewiesen, Argumente, neunummerierung Platzhalter und das neue Argument in der richtigen Reihenfolge in der Argumentliste einfügen beteiligt.

C#6 der neuen Funktion zur zeichenfolgeninterpolation erheblich verbessert `String.Format`. Jetzt können Sie direkt Variablen in eine Zeichenfolge, die mit dem Präfix Benennen einer `$`. Zum Beispiel:

```csharp
$"Expected: {expected} Received: {received}."
```

Variablen sind, natürlich, überprüft und eine Variable falsch geschrieben oder nicht verfügbare führt dazu, dass ein Compilerfehler ausgegeben.

Die Platzhalter werden nicht einfache Variablen sein muss, können sie ein beliebiger Ausdruck sein. In diesen Platzhaltern, können Sie, Anführungszeichen *ohne* Escapezeichen für diese Angebote. Beachten Sie z. B. die `"s"` in der folgenden:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Zeichenfolgeninterpolation unterstützt, die Ausrichtung und die Formatierung Syntax der `String.Format`. Wie bereits mitgeteilt haben `{index, alignment:format}`im C# 6, die Sie schreiben `{placeholder, alignment:format}`:

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
die Ergebnisse in:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

Zeichenfolgeninterpolation ist die Syntax-Zuckerguss für `String.Format`: Es kann nicht verwendet werden, mit `@""` Zeichenfolgenliteralen und ist nicht kompatibel mit `const`, auch wenn keine Platzhalter verwendet werden:

```csharp
const string s = $"Foo"; //Error : const requires value
```

Der allgemeine Anwendungsfall Argumente der Funktion mit der zeichenfolgeninterpolation zu erstellen müssen Sie weiterhin Escapezeichen, Codierung und Kultur Probleme vorsichtig sein. SQL- und URL-Abfragen sind natürlich entscheidend für das bereinigen. Wie bei `String.Format`, Interpolation verwendet eine Zeichenfolge der `CultureInfo.CurrentCulture`. Mithilfe von `CultureInfo.InvariantCulture` ist ein wenig mehr sich:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Initialisierung

C#6 bietet es sich um eine Reihe von präzise Möglichkeiten zum Angeben von Eigenschaften, Felder und Elemente.

### <a name="auto-property-initialization"></a>Initialisierung der Auto-Eigenschaft

Auto-Eigenschaften können jetzt im prägnant genauso wie Felder initialisiert werden. Unveränderliche Auto-Eigenschaften können nur mit einem Getter geschrieben werden:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

Im Konstruktor können Sie den Wert von einem nur-Getter-Auto-Eigenschaft festlegen:

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

Diese Initialisierung des Auto-Eigenschaften ist eine allgemeine Funktion der Platz zu sparen und ein Segen für Entwickler, die Unveränderlichkeit in ihre Objekte hervorzuheben.

### <a name="index-initializers"></a>Indexinitialisierer

C#6 führt indexinitialisierer, die dadurch können Sie sowohl den Schlüssel und Wert in Typen festlegen, denen einen Indexer. Dies ist normalerweise für `Dictionary`-Datenstrukturen zu formatieren:

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

### <a name="expression-bodied-function-members"></a>Ausdruckskörper Funktionsmember

Lambda-Funktionen müssen mehrere Vorteile, von denen einfach Speicherplatz gespeichert ist. Ausdruckskörper-Klasse, Elemente können auf ähnliche Weise kleine Funktionen etwas präziser als in früheren Versionen möglich war ausgedrückt werden C# 6.

Ausdruckskörper Funktionsmember verwenden Sie die Pfeil-Syntax von Lambda-anstelle der herkömmlichen Block-Syntax:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Beachten Sie, dass der Lambda-pfeilsyntax keinen expliziten verwendet `return`. Für Funktionen zurückgegebene `void`, der Ausdruck muss auch eine Anweisung sein:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

Ausdruckskörpermember unterliegen auch weiterhin die Regel, die `async` wird für Methoden, aber keine Eigenschaften unterstützt:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Ausnahmen

Es gibt keine zwei Möglichkeiten dazu: Behandlung von Ausnahmen ist schwer zu verwirklichen. Neue Features in C# 6 Stellen für die Ausnahmebehandlung, flexibler und konsistent.

### <a name="exception-filters"></a>Ausnahmefilter

Definitionsgemäß Ausnahmen, die in besonderen Fällen auftreten, und es kann sehr schwierig sein, Ursache und Code zu *alle* die Möglichkeiten, eine Ausnahme eines bestimmten Typs auftreten. C#6 werden die Möglichkeit, einen Handler für die Ausführung mit einem Filter ausgewertet, Common Language Runtime Umgebung. Dies erfolgt durch Hinzufügen einer `when (bool)` Muster, nach der normalen `catch(ExceptionType)` Deklaration. Im folgenden wird ein Filter ein Analysefehler auftritt, die im Zusammenhang mit unterscheidet die `date` Parameter im Gegensatz zu anderen Analysefehler.

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

### <a name="await-in-catchfinally"></a>"await" in catch-... schließlich...

Die `async` eingeführten Funktionen C# 5 war ein Wendepunkt für die Sprache. In C# 5 `await` wurde nicht innerhalb von `catch` und `finally` blockiert wird, ein Ärgernis, die den Wert der `async/await` Funktion. C#6 entfernt dieser Einschränkung können asynchrone Ergebnisse konsistent über das Programm erwartet wird, wie im folgenden Codeausschnitt gezeigt:

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

Die C# Sprache ständig weiterentwickelt, um Entwickler produktiver, auch heraufstufen andere bewährte Verfahren und Tools unterstützen. In diesem Dokument erhält einen Überblick über die neuen Sprachfeatures in C# 6 und wurde kurz veranschaulicht, wie sie verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Neue Sprache-Funktionen in C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)

