---
title: C#6 neue Features (Übersicht)
description: In Version 6 der C# Sprache wird die Sprache weiterentwickelt, sodass Sie weniger Code Bausteine, bessere Übersichtlichkeit und mehr Konsistenz hat. Bessere Initialisierungs Syntax, die Möglichkeit zur Verwendung von "warten in catch/schließlich"-Blöcken und der NULL-bedingten? der-Operator ist besonders nützlich.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 34a77f15391a0f4e10c1902046d8bd37806c14ec
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765279"
---
# <a name="c-6-new-features-overview"></a>C#6 neue Features (Übersicht)

_In Version 6 der C# Sprache wird die Sprache weiterentwickelt, sodass Sie weniger Code Bausteine, bessere Übersichtlichkeit und mehr Konsistenz hat. Bessere Initialisierungs Syntax, die Möglichkeit zur Verwendung von "warten in catch/schließlich"-Blöcken und der NULL-bedingten? der-Operator ist besonders nützlich._

> [!NOTE]
> Informationen zur neuesten Version der C# Sprache – Version 7 – finden Sie im Artikel [Neues in 7,0 C# ](/dotnet/csharp/whats-new/csharp-7)

In diesem Dokument werden die neuen Features C# von 6 vorgestellt. Sie wird vom Mono-Compiler vollständig unterstützt, und Entwickler können die neuen Features auf allen xamarin-Zielplattformen verwenden.

> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Neues in C# 6 Video**

## <a name="using-c-6"></a>Mit C# 6

Der C# 6-Compiler wird in allen neueren Versionen von Visual Studio für Mac verwendet.
Solche, die Befehlszeilen Compiler verwenden, sollten bestätigen `mcs --version` , dass 4,0 oder höher zurückgibt.
Visual Studio für Mac Benutzer überprüfen können, ob Mono 4 (oder höher) installiert ist, indem Sie auf Info **Visual Studio für Mac > Visual Studio für Mac verweisen > Details anzeigen**.

## <a name="less-boilerplate"></a>Weniger Bausteine
### <a name="using-static"></a>verwendet statische
Enumerationen und bestimmte Klassen `System.Math`, wie z. b., sind primär Inhaber statischer Werte und Funktionen. In C# 6 können Sie alle statischen Member eines Typs mit einer einzelnen `using static` Anweisung importieren. Vergleichen Sie eine typische drei metrische Funktion in C# 5 und C# 6:

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

`using static`keine öffentlichen `const` Felder, `Math.PI` wie z. b. `Math.E`und, werden direkt zugänglich:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>Verwenden von static mit Erweiterungs Methoden

Die `using static` -Funktion funktioniert mit Erweiterungs Methoden etwas anders. Obwohl Erweiterungs Methoden mit geschrieben werden `static`, sind Sie nicht sinnvoll, ohne dass eine Instanz verwendet werden muss. Wenn `using static` also mit einem Typ verwendet wird, der Erweiterungs Methoden definiert, werden die Erweiterungs Methoden für den Zieltyp ( `this` Typ der Methode) verfügbar. Beispielsweise kann `using static System.Linq.Enumerable` verwendet werden, um die API von `IEnumerable<T>` Objekten zu erweitern, ohne alle LINQ-Typen zu verwenden:

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

Das vorherige Beispiel veranschaulicht den Unterschied im Verhalten: die Erweiterungs `Enumerable.Where` Methode ist dem Array zugeordnet, während die statische Methode `String.Join` ohne Verweis auf den `String` Typ aufgerufen werden kann.

### <a name="nameof-expressions"></a>nameof-Ausdrücke
Manchmal möchten Sie auf den Namen verweisen, den Sie für eine Variable oder ein Feld angegeben haben. In C# 6 `nameof(someVariableOrFieldOrType)` wird die Zeichenfolge `"someVariableOrFieldOrType"`zurückgegeben. Wenn Sie beispielsweise eine `ArgumentException` auslösen, möchten Sie sehr wahrscheinlich den Namen des ungültigen Arguments benennen:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Der Hauptvorteil von `nameof` Ausdrücken besteht darin, dass Sie typgeprüft sind und mit dem Tool gestützten Refactoring kompatibel sind. Die Typüberprüfung von Ausdrücken `nameof` ist besonders willkommen, wenn eine `string` zum dynamischen Zuordnen von Typen verwendet wird. Beispielsweise wird in ios eine `string` verwendet, um den Typ anzugeben, der zum `UITableViewCell` Prototypen von Objekten `UITableView`in einer verwendet wird. `nameof`kann sicherstellen, dass diese Zuordnung aufgrund einer falschen Rechtschreibprüfung oder eines schlampigen-refactorings nicht fehlschlägt:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Obwohl Sie einen qualifizierten Namen an `nameof`übergeben können, wird nur das letzte Element (nach dem letzten `.`) zurückgegeben. Beispielsweise können Sie eine Datenbindung in xamarin. Forms hinzufügen:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Die beiden Aufrufe von `SetBinding` übergeben identische Werte: `nameof(ReactiveType.StringField)` ist `"StringField"`, nicht `"ReactiveType.StringField"` wie Sie anfänglich erwarten.

## <a name="null-conditional-operator"></a>NULL Bedingter Operator
In früheren Updates C# von wurden die Konzepte von Typen, die NULL-Werte zulassen, und der `??` NULL-Sammel Operator eingeführt, um die Menge des Code Bausteine bei der Behandlung von NULL-Werten zu verringern. C#6 setzt dieses Design mit dem "Null bedingten Operator" `?.`fort. Bei Verwendung in einem Objekt auf der rechten Seite eines Ausdrucks gibt der NULL bedingte Operator den Elementwert zurück, wenn das Objekt nicht `null` ist, und `null` andernfalls:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Sowohl `length0` als `length1` auch werden als Typ `int?`abgeleitet).

Die letzte Zeile im vorherigen Beispiel zeigt den `?` NULL-bedingten Operator in Kombination mit dem `??` NULL-Sammel Operator. Der neue C# NULL-bedingte Operator gibt für `null` das zweite Element im Array zurück. an diesem Punkt springt der NULL-Sammel Operator ein und stellt dem `lengths` Array eine 0 (null) bereit. (ob dies angemessen ist oder nicht, ist natürlich Problem spezifisch).

Der NULL bedingte Operator sollte die Anzahl der in vielen Anwendungen erforderlichen Null-Überprüfungen auf dem Code Bausteine enorm verringern.

Aufgrund von Mehrdeutigkeiten gibt es einige Einschränkungen für den NULL bedingten Operator. Sie können nicht direkt auf `?` eine mit einer Argumentliste in Klammern folgen, wie Sie wahrscheinlich mit einem Delegaten zu tun haben:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Kann jedoch verwendet werden, um die `?` von der Argumentliste zu trennen, und ist immer noch eine markierte `null`Verbesserung gegenüber einem Überprüfungs Block von Boilerplate: `Invoke`

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Zeichenfolgeninterpolierung
Die `String.Format` -Funktion hat traditionell Indizes als Platzhalter in der Format Zeichenfolge verwendet `String.Format("Expected: {0} Received: {1}.", expected, received`, z. b.). Natürlich hat das Hinzufügen eines neuen Werts immer eine lästige kleine Aufgabe zum zählen von Argumenten, zum Umrechnen von Platzhaltern und zum Einfügen des neuen Arguments in der richtigen Reihenfolge in der Argumentliste beteiligt.

C#Das neue Zeichen folgen Interpolations Feature von 6 verbessert `String.Format`sich erheblich. Nun können Sie Variablen in einer Zeichenfolge mit dem `$`Präfix direkt benennen. Zum Beispiel:

```csharp
$"Expected: {expected} Received: {received}."
```

Variablen werden natürlich aktiviert, und eine falsch geschriebene oder nicht verfügbare Variable führt zu einem Compilerfehler.

Die Platzhalter müssen keine einfachen Variablen sein, Sie können ein beliebiger Ausdruck sein. Innerhalb dieser Platzhalter können Sie Anführungszeichen *ohne* Escapezeichen verwenden. Beachten Sie `"s"` beispielsweise Folgendes:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Die Zeichen folgen Interpolations Syntax unterstützt die Ausrichtung `String.Format`und Formatierungs Syntax von. Ebenso wie zuvor haben Sie `{index, alignment:format}`in C# 6 `{placeholder, alignment:format}`geschrieben:

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

```
The value is       1.00.
The value is       2.00.
The value is       3.00.
The value is       4.00.
The value is      12.00.
The value is 123,456.00.
Minimum is 1.00.
```

Die Zeichen folgen interpolung ist syntaktische `String.Format`Sugar für: Sie kann nicht `@""` mit Zeichenfolgenliteralen verwendet `const`werden und ist nicht mit kompatibel, auch wenn keine Platzhalter verwendet werden:

```csharp
const string s = $"Foo"; //Error : const requires value
```

Bei der allgemeinen Verwendung von Funktions Argumenten mit Zeichen folgen Interpolationen müssen Sie immer noch vorsichtig sein, um Probleme mit Escapezeichen, Codierungen und Kulturen zu vermeiden. SQL-und URL-Abfragen sind natürlich wichtig für die bereinigen. Wie bei `String.Format`verwendet die `CultureInfo.CurrentCulture`Zeichen folgen Interpolations-. Die `CultureInfo.InvariantCulture` Verwendung von ist etwas komplizierter:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Initialisierung

C#6 bietet eine Reihe präziser Möglichkeiten, Eigenschaften, Felder und Member anzugeben.

### <a name="auto-property-initialization"></a>Automatische Eigenschaften Initialisierung

Auto-Eigenschaften können jetzt in der gleichen präzisen Weise wie Felder initialisiert werden. Unveränderliche Auto-Eigenschaften können nur mit einem Getter geschrieben werden:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

Im Konstruktor können Sie den Wert einer reinen Getter-Eigenschaft festlegen:

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

Diese Initialisierung von Auto-Eigenschaften ist eine allgemeine Funktion zum Speichern von Speicherplatz und ein Segen für Entwickler, die die Unveränderlichkeit in ihren Objekten hervorheben möchten.

### <a name="index-initializers"></a>Indexinitialisierer

C#6 führt indexinitialisierer ein, mit denen Sie sowohl den Schlüssel als auch den Wert in Typen festlegen können, die über einen Indexer verfügen. In der Regel handelt es `Dictionary`sich hierbei um Datenstrukturen im Stil:

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

### <a name="expression-bodied-function-members"></a>Ausdrucks Körper-Funktionsmember

Lambda-Funktionen haben mehrere Vorteile, von denen eine einfach Speicherplatz spart. Ebenso ermöglichen Ausdrucks Körper-Klassenmember, dass kleine Funktionen etwas ausführlicher ausgedrückt werden, als dies in früheren Versionen von C# 6 möglich war.

Ausdrucks Körper-Funktionsmember verwenden die Lambda Pfeil Syntax anstelle der herkömmlichen Block Syntax:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Beachten Sie, dass die Lambda-Pfeil Syntax keine explizite `return`verwendet. Für Funktionen, die `void`zurückgeben, muss der Ausdruck auch eine-Anweisung sein:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

Ausdruckskörpermember unterliegen weiterhin der Regel, `async` die für Methoden, aber nicht für Eigenschaften unterstützt wird:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Ausnahmen

Es gibt keine zwei Möglichkeiten: die Ausnahmebehandlung ist schwierig zu beheben. Mit den neuen C# Features in 6 wird die Ausnahmebehandlung flexibler und konsistent.

### <a name="exception-filters"></a>Ausnahmefilter

Definitionsgemäß treten Ausnahmen in ungewöhnlichen Fällen auf, und es kann schwierig sein, die Ursache zu finden und Code zu *allen* Möglichkeiten, wie eine Ausnahme eines bestimmten Typs auftreten könnte. C#6 bietet die Möglichkeit, einen Ausführungs Handler mit einem von der Laufzeit ausgewerteten Filter zu schützen. Dies erfolgt durch Hinzufügen eines `when (bool)` Musters nach der normalen `catch(ExceptionType)` Deklaration. Im folgenden unterscheidet ein Filter einen Analysefehler im Zusammenhang mit dem `date` -Parameter im Gegensatz zu anderen Analyse Fehlern.

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

### <a name="await-in-catchfinally"></a>warten in catch... und schließlich...

Die `async` in C# 5 eingeführten Funktionen waren für die Sprache ein Spielwechsler. In C# 5 ist `await` in `catch` -und- `finally` Blöcken nicht zulässig, ein Ärgernis, wenn der Wert `async/await` der Funktion angegeben wird. C#6 entfernt diese Einschränkung und ermöglicht so, dass asynchrone Ergebnisse konsistent durch das Programm gewartet werden, wie im folgenden Code Ausschnitt gezeigt:

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

Die C# Sprache wird weiterentwickelt, um Entwicklern eine höhere Produktivität zu ermöglichen, während gleichzeitig gute Verfahren und unterstützende Tools entwickelt werden. Dieses Dokument enthält eine Übersicht über die neuen sprach Features in C# 6 und zeigt kurz, wie Sie verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Neue sprach Features in C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
