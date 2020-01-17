---
title: Xamarin für Java-Entwickler
description: Wenn Sie ein Java-Entwickler sind, sind Sie auf dem besten Weg, Ihre Fähigkeiten und vorhandenen Code auf der Xamarin-Plattform zu nutzen und gleichzeitig die Vorteile von C# für die Wiederverwendung von Code auszuschöpfen. Sie werden feststellen, dass die C#-Syntax der Java-Syntax sehr ähnlich ist und dass beide Sprachen sehr ähnliche Funktionen bieten. Darüber hinaus werden Sie Features entdecken, die einzigartig in C# sind und Ihnen das Entwicklerleben erleichtern.
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: b9c6694ea49607b839a3658e5cc8bac5fb529c85
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728056"
---
# <a name="xamarin-for-java-developers"></a>Xamarin für Java-Entwickler

_Wenn Sie ein Java-Entwickler sind, sind Sie auf dem besten Weg, Ihre Fähigkeiten und vorhandenen Code auf der Xamarin-Plattform zu nutzen und gleichzeitig die Vorteile von C# für die Wiederverwendung von Code auszuschöpfen. Sie werden feststellen, dass die C#-Syntax der Java-Syntax sehr ähnlich ist und dass beide Sprachen sehr ähnliche Funktionen bieten. Darüber hinaus werden Sie Features entdecken, die einzigartig in C# sind und Ihnen das Entwicklerleben erleichtern._

## <a name="overview"></a>Übersicht

Dieser Artikel bietet eine Einführung in die C#-Programmierung für Java-Entwickler und konzentriert sich vor allem auf die C#-Sprachfunktionen, die Ihnen bei der Entwicklung von Xamarin.Android-Anwendungen begegnen werden. Außerdem wird in diesem Artikel erläutert, wie sich diese Features von ihren Java-Gegenstücken unterscheiden, und es werden wichtige C#-Features (relevant für Xamarin.Android) vorgestellt, die in Java nicht verfügbar sind. Links zu weiterem Referenzmaterial sind enthalten, sodass Sie diesen Artikel als „Ausgangspunkt“ für weitere Studien zu C# und .NET verwenden können.

Wenn Sie mit Java vertraut sind, werden Sie sich in der Syntax von C# sofort zu Hause fühlen. C#-Syntax ist der Java-Syntax sehr ähnlich &ndash; C# ist eine Sprache mit „geschweiften Klammern“ wie Java, C und C++. In vielerlei Hinsicht liest sich die C#-Syntax wie eine Obermenge der Java-Syntax, aber mit ein paar umbenannten und hinzugefügten Schlüsselwörtern.

Viele Schlüsselmerkmale von Java finden sich in C#:

- Klassenbasierte objektorientierte Programmierung

- Starke Typisierung

- Unterstützung für Schnittstellen

- Generics

- Garbage Collection

- Laufzeitkompilierung

Sowohl Java als auch C# werden zu einer Zwischensprache kompiliert, die in einer verwalteten Ausführungsumgebung ausgeführt wird. Sowohl C# als auch Java sind statisch typisiert, und beide Sprachen behandeln Zeichenfolgen als unveränderliche Typen.
Beide Sprachen verwenden eine Klassenhierarchie mit nur einem Stamm. Wie Java unterstützt auch C# nur die einfache Vererbung und erlaubt keine globalen Methoden.
In beiden Sprachen werden Objekte mit dem Schlüsselwort `new` auf dem Heap erstellt, und für Objekte wird Garbage Collection ausgeführt, wenn sie nicht mehr verwendet werden. Beide Sprachen bieten formale Ausnahmebehandlung mit `try`/`catch`-Semantik. Beide bieten Unterstützung für die Verwaltung und Synchronisierung von Threads.

Es gibt jedoch auch zahlreiche Unterschiede zwischen Java und C#. Zum Beispiel:

- Java (wie es in Android verwendet wird) unterstützt keine implizit typisierten lokalen Variablen (C# unterstützt das Schlüsselwort `var`).

- In Java können Sie Parameter nur als Wert übergeben, während Sie sie in C# sowohl als Verweis als auch als Wert übergeben können. (C# stellt die Schlüsselwörter `ref` und `out` für die Übergabe von Parametern durch Verweis zur Verfügung. Es gibt kein Äquivalent zu diesen Schlüsselwörtern in Java).

- Java unterstützt keine Präprozessordirektiven wie `#define`.

- Java unterstützt keine Integertypen ohne Vorzeichen, während C# Integertypen ohne Vorzeichen bereitstellt, z.B. `ulong`, `uint`, `ushort` und `byte`.

- Java unterstützt keine Operatorüberladung. In C# können Sie Operatoren und Konvertierungen überladen.

- In einer Java `switch`-Anweisung kann Code in den nächsten Switch-Abschnitt fallen, aber in C# muss das Ende jedes `switch`-Abschnitts den Switch beenden (das Ende jedes Abschnitts muss mit einer `break`-Anweisung abgeschlossen werden).

- In Java geben Sie die Ausnahmen, die von einer Methode ausgelöst werden, mit dem Schlüsselwort `throws` an, aber C# hat kein Konzept von geprüften Ausnahmen &ndash; das Schlüsselwort `throws` wird in C# nicht unterstützt.

- C# unterstützt LINQ (Language-Integrated Query). Damit können Sie die reservierten Wörter `from`, `select` und `where` verwenden, um Abfragen für Sammlungen ähnlich wie Datenbankabfragen zu schreiben.

Natürlich gibt es noch viele weitere Unterschiede zwischen C# und Java, die in diesem Artikel behandelt werden können. Außerdem entwickeln sich sowohl Java als auch C# ständig weiter (z.B. Java 8, das noch nicht in der Android-Toolkette enthalten ist und Lambdaausdrücke im C#-Stil unterstützt), sodass sich diese Unterschiede im Lauf der Zeit ändern werden. Hier werden nur die wichtigsten Unterschiede aufgezeigt, die Java-Entwickler, die neu in Xamarin.Android einsteigen, zurzeit feststellen.

- [Umstieg von Java- auf C#-Entwicklung](#fundamentals) bietet eine Einführung in die grundlegenden Unterschiede zwischen C# und Java.

- [Objektorientierte Programmierfeatures](#oopfeatures) umreißt die wichtigsten objektorientierten Featureunterschiede zwischen den beiden Sprachen.

- [Schlüsselwortunterschiede](#keywords) bietet eine Tabelle mit nützlichen Schlüsselwortäquivalenten, Schlüsselwörtern nur in C# und Links zu C#-Schlüsselwortdefinitionen.

C# bringt viele wichtige Features in Xamarin.Android ein, die zurzeit für Java-Entwickler unter Android nicht ohne weiteres verfügbar sind. Diese Features können Sie dabei unterstützen, besseren Code in kürzerer Zeit zu schreiben:

- [Eigenschaften](#properties): Mit dem Eigenschaftensystem von C# können Sie sicher und direkt auf Membervariablen zugreifen, ohne Setter- und Getter-Methoden schreiben zu müssen.

- [Lambdaausdrücke](#lambdas): In C# können Sie anonyme Methoden (auch als *Lambdaausdrücke* bezeichnet) verwenden, um eine bestimmte Logik prägnanter und effizienter auszudrücken. Sie können den Mehraufwand für das Schreiben von Objekten vermeiden, die nur ein Mal verwendet werden, und Sie können den lokalen Zustand an eine Methode übergeben, ohne Parameter hinzufügen zu müssen.

- [Ereignisbehandlung](#events): C# bietet Sprachunterstützung für *ereignisgesteuerte Programmierung*, bei der ein Objekt registriert werden kann, um benachrichtigt zu werden, wenn ein Ereignis von Interesse eintritt. Das Schlüsselwort `event` definiert einen Multicast-Broadcast-Mechanismus, den eine Publisher-Klasse verwenden kann, um Ereignisabonnenten zu benachrichtigen.

- [Asynchrone Programmierung](#async): Die asynchronen Programmierfunktionen von C# (`async`/`await`) sorgen dafür, dass Apps schnell reagieren.
    Durch die Unterstützung dieses Features auf Sprachebene ist die asynchrone Programmierung einfach zu implementieren und weniger fehleranfällig.

Schließlich erlaubt es Xamarin, [vorhandene Java-Ressourcen](#interop) mithilfe einer Technologie zu nutzen, die als *Bindung* bezeichnet wird. Sie können Ihren vorhandenen Java-Code, Frameworks und Bibliotheken aus C# aufrufen, indem Sie die automatischen Bindungsgeneratoren von Xamarin verwenden. Zu diesem Zweck erstellen Sie einfach eine statische Bibliothek in Java und stellen sie C# über eine Bindung zur Verfügung.

> [!NOTE]
> In der Android-Programmierung wird eine spezifische Version der Java-Sprache verwendet, die alle Java 7-Features [und eine Teilmenge von Java 8](https://developer.android.com/studio/write/java8-support.html) unterstützt.
>
> Einige Features, die auf dieser Seite erwähnt werden (z. B. das Schlüsselwort `var` in C#), sind in neueren Versionen von Java verfügbar (z. B. [`var` in Java 10](https://developer.oracle.com/java/jdk-10-local-variable-type-inference.html)), sind aber für Android-Entwickler weiterhin nicht verfügbar.

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>Umstieg von Java- auf C#-Entwicklung

In den folgenden Abschnitten werden die grundlegenden Unterschiede bei den ersten Schritten zwischen C# und Java beschrieben, ein späterer Abschnitt erläutert die objektorientierten Unterschiede zwischen diesen Sprachen.

### <a name="libraries-vs-assemblies"></a>Bibliotheken im Vergleich zu Assemblys

Java verpackt verwandte Klassen normalerweise in **JAR**-Dateien. In C# und .NET werden jedoch wiederverwendbare Teile von vorkompiliertem Code in *Assemblys* verpackt, die in der Regel als *DLL*-Dateien verpackt sind. Eine Assembly ist eine Bereitstellungseinheit für C#-/.NET-Code, und jede Assembly ist in der Regel einem C#-Projekt zugeordnet. Assemblys enthalten Zwischencode (IL), der Just-In-Time zur Laufzeit kompiliert wird.

Weitere Informationen zu Assemblys finden Sie im Artikel [Assemblys und der globale Assemblycache](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/assemblies-gac/).

### <a name="packages-vs-namespaces"></a>Pakete im Vergleich zu Namespaces

C# verwendet das Schlüsselwort `namespace`, um verwandte Typen zu gruppieren. Dies ähnelt dem Schlüsselwort `package` von Java. In der Regel befindet sich eine Xamarin.Android-App in einem Namespace, der für die jeweilige App erstellt wird. Beispielsweise deklariert der folgende C#-Code den Namespacewrapper `WeatherApp` für eine Wetterbericht-App:

```csharp
namespace WeatherApp
{
    ...
```

### <a name="importing-types"></a>Importieren von Typen

Wenn Sie Typen verwenden, die in externen Namespaces definiert sind, importieren Sie diese Typen mit einer `using`-Anweisung (die der `import`-Anweisung von Java sehr ähnlich ist). In Java könnten Sie einen einzelnen Typ mit einer Anweisung wie der folgenden importieren:

```java
import javax.swing.JButton
```

Sie können ein vollständiges Java-Paket mit einer Anweisung wie der folgenden importieren:

```java
import javax.swing.*
```

Die C#-Anweisung `using` funktioniert auf eine sehr ähnliche Weise, aber sie erlaubt es Ihnen, ein ganzes Paket zu importieren, ohne einen Platzhalter anzugeben. Beispielsweise werden Sie häufig eine Reihe von `using`-Anweisungen am Anfang von Xamarin.Android-Quelldateien sehen, wie in diesem Beispiel gezeigt:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

Diese Anweisungen importieren Funktionalität aus den Namespaces `System`, `Android.App`, `Android.Content` usw.

### <a name="generics"></a>Generics

Sowohl Java als auch C# unterstützen *Generics*. Dabei handelt es sich um Platzhalter, mit denen Sie verschiedene Typen zur Kompilierungszeit einbinden können. Generics funktionieren jedoch in C# etwas anders. In Java stellt [Type Erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) Typinformationen nur zur Kompilierungszeit, nicht aber zur Laufzeit zur Verfügung. Im Gegensatz dazu bietet die .NET-CLR (Common Language Runtime) explizite Unterstützung für generische Typen. Dies bedeutet, dass C# zur Laufzeit Zugriff auf Typinformationen besitzt. In der täglichen Xamarin.Android-Entwicklung ist die Wichtigkeit dieser Unterscheidung häufig nicht offensichtlich, aber wenn Sie [Reflektion](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/reflection) verwenden, sind Sie von diesem Feature abhängig, um zur Laufzeit auf Typinformationen zuzugreifen.

In Xamarin.Android wird Ihnen auffallen, dass häufig die generische Methode `FindViewById` verwendet wird, um einen Verweis auf ein Layoutsteuerelement zu erhalten. Diese Methode akzeptiert einen generischen Typparameter, der den Typ des Steuerelements angibt, der nachgeschlagen werden soll. Zum Beispiel:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

In diesem Codebeispiel ruft `FindViewById` einen Verweis auf das `TextView`-Steuerelement ab, das im Layout als **Bezeichnung** definiert ist, und gibt es dann als `TextView`-Typ zurück.

Weitere Informationen zu Generics finden Sie im Artikel [Generics](https://docs.microsoft.com/dotnet/csharp/programming-guide/generics/index).
Beachten Sie, dass es einige Einschränkungen in der Unterstützung von Xamarin.Android für generische C#-Klassen gibt. Weitere Informationen finden Sie unter [Einschränkungen](~/android/internals/limitations.md).

<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>Objektorientierte Programmierfeatures

Sowohl Java als auch C# verwenden sehr ähnliche objektorientierte Programmieridiome:

- Alle Klassen werden letztlich von einem einzigen Stammobjekt abgeleitet &ndash; alle Java-Objekte von `java.lang.Object`, während alle C#-Objekte von `System.Object` abgeleitet werden.

- Instanzen von Klassen sind Verweistypen.

- Wenn Sie auf die Eigenschaften und Methoden einer Instanz zugreifen, verwenden Sie den Operator „`.`“.

- Alle Klasseninstanzen werden auf dem Heap über den `new`-Operator erstellt.

- Da beide Sprachen Garbage Collection verwenden, gibt es keine Möglichkeit, nicht verwendete Objekte explizit freizugeben (es gibt also kein Schlüsselwort wie `delete`, das in C++ verwendet wird).

- Sie können Klassen durch Vererbung erweitern, und beide Sprachen erlauben nur eine einzige Basisklasse pro Typ.

- Sie können Schnittstellen definieren, und eine Klasse kann von mehreren Schnittstellendefinitionen erben (d.h. diese implementieren).

Es gibt jedoch auch einige wichtige Unterschiede:

- Java weist zwei leistungsstarke Features auf, die C# nicht unterstützt: anonyme Klassen und innere Klassen. (Allerdings erlaubt C# die Schachtelung von Klassendefinitionen &ndash; die geschachtelten Klassen von C# sind wie die statischen geschachtelten Klassen von Java.)

- C# unterstützt Strukturtypen im C-Stil (`struct`), Java hingegen nicht.

- In C# können Sie eine Klassendefinition in separaten Quelldateien implementieren, indem Sie das Schlüsselwort `partial` verwenden.

- C#-Schnittstellen können keine Felder deklarieren.

- C# verwendet Destruktorsyntax im C++-Stil, um Finalizer auszudrücken. Die Syntax unterscheidet sich von der `finalize`-Methode von Java, aber die Semantik ist nahezu identisch. (Beachten Sie, dass Destruktoren in C# automatisch den Destruktor der Basisklasse aufrufen &ndash; im Gegensatz zu Java, wo ein expliziter Aufruf von `super.finalize` verwendet wird.)

### <a name="class-inheritance"></a>Klassenvererbung

Um eine Klasse in Java zu erweitern, verwenden Sie das Schlüsselwort `extends`. Um eine Klasse in C# zu erweitern, verwenden Sie einen Doppelpunkt (`:`), um die Ableitung anzugeben. Beispielsweise werden Sie in Xamarin.Android-Apps häufig Klassenableitungen sehen, die dem folgenden Codefragment ähneln:

```csharp
public class MainActivity : Activity
{
    ...
```

In diesem Beispiel erbt `MainActivity` von der `Activity`-Klasse.

Um Unterstützung für eine Schnittstelle in Java zu deklarieren, verwenden Sie das Schlüsselwort `implements`. In C# fügen Sie jedoch einfach Schnittstellennamen zur Liste der Klassen hinzu, von denen geerbt werden soll, wie in diesem Codefragment gezeigt:

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

In diesem Beispiel erbt `SensorsActivity` von `Activity` und implementiert die in der `ISensorEventListener`-Schnittstelle deklarierte Funktionalität. Beachten Sie, dass die Liste der Schnittstellen nach der Basisklasse aufgeführt werden muss (andernfalls erhalten Sie einen Kompilierungsfehler). Nach der Konvention wird C#-Schnittstellennamen ein „I“ in Großbuchstaben vorangestellt, wodurch es möglich ist, zu bestimmen, welche Klassen Schnittstellen sind, ohne ein Schlüsselwort `implements` zu benötigen.

Wenn Sie verhindern möchten, dass eine Klasse in C# weiter in Unterklassen aufgeteilt wird, stellen Sie dem Klassennamen in Java `sealed` voran &ndash; in Java stellen Sie dem Klassennamen `final` voran.

Weitere Informationen zu C#-Klassendefinitionen finden Sie in den Artikeln [Klassen](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/classes) und [Vererbung](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/inheritance).

### <a name="properties"></a>Eigenschaften

In Java werden häufig Mutatormethoden (Setter) und Inspektormethoden (Getter) verwendet, um zu steuern, wie Änderungen an Klassenmembern vorgenommen werden, während diese Member vor externem Code verborgen und geschützt werden. Beispielsweise stellt die Android-Klasse `TextView` die `getText`- und `setText`-Methoden zur Verfügung. C# bietet einen ähnlichen, aber direkteren Mechanismus, der als *Eigenschaften* bezeichnet wird.
Benutzer einer C#-Klasse können auf eine Eigenschaft auf die gleiche Weise zugreifen wie auf ein Feld, aber jeder Zugriff führt tatsächlich zu einem Methodenaufruf, der für den Aufrufer transparent ist. Diese Methode „hinter den Kulissen“ kann Nebenwirkungen wie das Festlegen anderer Werte, das Ausführen von Konvertierungen oder das Ändern des Objektzustands implementieren.

Eigenschaften werden häufig für den Zugriff auf und die Änderung von UI-Objektmembern (Benutzeroberfläche) verwendet. Zum Beispiel:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

In diesem Beispiel werden die Werte für Breite und Höhe aus dem Objekt `rulerView` gelesen, indem auf dessen Eigenschaften `MeasuredWidth` und `MeasuredHeight` zugegriffen wird. Beim Lesen dieser Eigenschaften werden Werte aus den zugehörigen (aber verborgenen) Feldwerten hinter den Kulissen abgerufen und an den Aufrufer zurückgegeben. Das `rulerView`-Objekt kann Breiten- und Höhenwerte in einer Maßeinheit (z.B. Pixel) speichern und diese Werte sofort in eine andere Maßeinheit (z.B. Millimeter) umwandeln, wenn auf die Eigenschaften `MeasuredWidth` und `MeasuredHeight` zugegriffen wird.

Das `rulerView`-Objekt weist auch eine Eigenschaft namens `DrawingCacheEnabled` auf &ndash; der Beispielcode legt diese Eigenschaft auf `true` fest, um den Zeichencache in `rulerView` zu aktivieren. Im Hintergrund wird ein zugehöriges verborgenes Feld mit dem neuen Wert aktualisiert, und möglicherweise werden andere Aspekte des `rulerView`-Zustands geändert. Wenn `DrawingCacheEnabled` z.B. auf `false` festgelegt ist, kann `rulerView` auch alle Informationen aus dem Zeichencache löschen, die sich bereits im Objekt angesammelt haben.

Der Zugriff auf Eigenschaften kann Lese-/Schreibzugriff, schreibgeschützt oder lesegeschützt sein. Außerdem können Sie verschiedene Zugriffsmodifizierer zum Lesen und Schreiben verwenden. Beispielsweise können Sie eine Eigenschaft definieren, die über öffentlichen Lesezugriff, aber privaten Schreibzugriff verfügt.

Weitere Informationen zu C#-Eigenschaften finden Sie im Artikel [Eigenschaften](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/properties).

### <a name="calling-base-class-methods"></a>Aufrufen von Basisklassenmethoden

Um einen Basisklassenkonstruktor in C# aufzurufen, verwenden Sie einen Doppelpunkt (`:`), gefolgt vom Schlüsselwort `base` und einer Initialisierungsliste. Dieser `base` Konstruktoraufruf wird unmittelbar hinter der abgeleiteten Konstruktorparameterliste platziert. Der Basisklassenkonstruktor wird beim Eintritt in den abgeleiteten Konstruktor aufgerufen. Der Compiler fügt den Aufruf des Basiskonstruktors am Anfang des Methodentexts ein. Das folgende Codefragment zeigt einen Basiskonstruktor, der von einem abgeleiteten Konstruktor in einer Xamarin.Android-App aufgerufen wird:

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

In diesem Beispiel wird die `PictureLayout`-Klasse von der `ViewGroup`-Klasse abgeleitet. Der in diesem Beispiel gezeigte `PictureLayout`-Konstruktor akzeptiert ein `context`-Argument und übergibt es über den `base(context)`-Aufruf an den `ViewGroup`-Konstruktor.

Um eine Basisklassenmethode in C# aufzurufen, verwenden Sie das Schlüsselwort `base`. Beispielsweise rufen Xamarin.Android-Apps häufig Basismethoden auf, wie hier gezeigt:

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

In diesem Fall ruft die `OnCreate`-Methode, die durch die abgeleitete Klasse (`MainActivity`) definiert ist, die `OnCreate`-Methode der Basisklasse (`Activity`) auf.

### <a name="access-modifiers"></a>Zugriffsmodifizierer

Java und C# unterstützen die Zugriffsmodifizierer `public`, `private`, und `protected`. Allerdings unterstützt C# zwei weitere Zugriffsmodifizierer:

- **`internal`** : Auf den Klassenmember kann nur in der aktuellen Assembly zugegriffen werden.

- **`protected internal`** : Auf den Klassenmember kann innerhalb der definierenden Assembly, der definierenden Klasse und der abgeleiteten Klassen (abgeleitete Klassen sowohl innerhalb als auch außerhalb der Assembly besitzen Zugriff) zugegriffen werden.

Weitere Informationen zu C#-Zugriffsmodifizierern finden Sie im Artikel [Zugriffsmodifizierer](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers).

### <a name="virtual-and-override-methods"></a>Virtuelle und Außerkraftsetzungsmethoden

Sowohl Java als auch C# unterstützen *Polymorphismus*: die Fähigkeit, verwandte Objekte auf die gleiche Weise zu behandeln. In beiden Sprachen können Sie einen Basisklassenverweis verwenden, um auf ein abgeleitetes Klassenobjekt zu verweisen, und die Methoden einer abgeleiteten Klasse können die Methoden ihrer Basisklassen überschreiben. Beide Sprachen verfügen über das Konzept einer *virtuellen* Methode, einer Methode in einer Basisklasse, die darauf ausgelegt ist, durch eine Methode in einer abgeleiteten Klasse ersetzt zu werden.
Wie Java unterstützt C# `abstract`-Klassen und -Methoden.

Allerdings gibt es einige Unterschiede zwischen Java und C# in der Art und Weise, wie virtuelle Methoden deklariert und überschrieben werden:

- In C# sind Methoden standardmäßig nicht virtuell. Übergeordnete Klassen müssen explizit angeben, welche Methoden mit dem Schlüsselwort `virtual` überschrieben werden sollen. Im Gegensatz dazu sind alle Methoden in Java standardmäßig virtuelle Methoden.

- Um zu verhindern, dass eine Methode in C# überschrieben wird, lassen Sie einfach das `virtual`-Schlüsselwort aus. Im Gegensatz dazu verwendet Java das Schlüsselwort `final`, um eine Methode mit „Überschreiben unzulässig“ zu markieren.

- Abgeleitete Klassen in C# müssen das Schlüsselwort `override` verwenden, um explizit anzugeben, dass eine Methode der virtuellen Basisklasse überschrieben wird.

Weitere Informationen zur Unterstützung von C# für Polymorphismus finden Sie im Artikel [Polymorphismus](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/polymorphism).

<a name="lambdas" />

## <a name="lambda-expressions"></a>Lambdaausdrücke

C# ermöglicht es, *Umschließungen* zu erstellen: anonyme Inlinemethoden, die auf den Zustand der Methode zugreifen können, in der sie eingeschlossen sind.
Mit Lambdaausdrücken können Sie weniger Zeilen Code schreiben, um die gleiche Funktionalität zu implementieren, die Sie in Java ggf. mit viel mehr Codezeilen implementiert haben.

Lambdaausdrücke ermöglichen es Ihnen, den zusätzlichen Aufwand zu umgehen, der bei der Erstellung einer Klasse für die einmalige Verwendung oder einer anonymen Klasse wie in Java entstehen würde &ndash; stattdessen können Sie einfach die Geschäftslogik Ihres Methodencodes inline schreiben. Da Lambdaausdrücke Zugriff auf die Variablen in der umgebenden Methode besitzen, müssen Sie auch keine lange Parameterliste erstellen, um den Status an Ihren Methodencode zu übergeben.

In C# werden Lambdaausdrücke mit dem Operator `=>` erstellt, wie hier gezeigt:

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

In Xamarin.Android werden Lambdaausdrücke häufig zum Definieren von Ereignishandlern verwendet. Zum Beispiel:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

In diesem Beispiel inkrementiert der Lambdaausdruckscode (der Code innerhalb der geschweiften Klammern) die Anzahl der Klicks und aktualisiert den `button`-Text, um die Anzahl der Klicks anzuzeigen. Dieser Lambdaausdruck wird mit dem `button`-Objekt als Klickereignishandler registriert, der immer dann aufgerufen wird, wenn auf die Schaltfläche getippt wird. (Ereignishandler werden weiter unten näher erläutert.) In diesem einfachen Beispiel werden die Parameter `sender` und `args` nicht vom Lambdaausdruckscode verwendet, aber sie sind im Lambdaausdruck erforderlich, um die Methodensignaturanforderungen für die Ereignisregistrierung zu erfüllen. Im Hintergrund übersetzt der C#-Compiler den Lambdaausdruck in eine anonyme Methode, die immer dann aufgerufen wird, wenn Klickereignisse der Schaltfläche stattfinden.

Weitere Informationen zu C# und Lambdaausdrücken finden Sie im Artikel [Lambdaausdrücke](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).

<a name="events" />

## <a name="event-handling"></a>Ereignisbehandlung

Ein *Ereignis* ist eine Möglichkeit für ein Objekt, registrierte Abonnenten zu benachrichtigen, wenn diesem Objekt etwas Interessantes passiert. Anders als in Java, wo ein Abonnent typischerweise eine `Listener`-Schnittstelle implementiert, die eine Rückrufmethode enthält, bietet C# Unterstützung auf Sprachebene für die Ereignisbehandlung durch *Delegaten*. Ein *Delegat* ähnelt einem objektorientierten typsicheren Funktionszeiger &ndash; er kapselt einen Objektverweis und ein Methodentoken. Wenn ein Clientobjekt ein Ereignis abonnieren möchte, erstellt es einen Delegaten und übergibt den Delegaten an das benachrichtigende Objekt.
Wenn das Ereignis eintritt, ruft das benachrichtigende Objekt die Methode auf, die durch das Delegatobjekt dargestellt wird, und benachrichtigt das abonnierende Clientobjekt über das Ereignis. In C# sind Ereignishandler im Wesentlichen nichts anderes als Methoden, die durch Delegaten aufgerufen werden.

Weitere Informationen zu Delegaten finden Sie im Artikel [Delegaten](https://docs.microsoft.com/dotnet/csharp/programming-guide/delegates/index).

In C# sind Ereignisse *multicast*. Das heißt, dass mehrere Listener benachrichtigt werden können, wenn ein Ereignis stattfindet. Dieser Unterschied wird beobachtet, wenn Sie die syntaktischen Unterschiede zwischen Java- und C#-Ereignisregistrierung berücksichtigen. In Java rufen Sie `SetXXXListener` auf, um sich für Ereignisbenachrichtigungen zu registrieren. In C# verwenden Sie den Operator `+=`, um sich für Ereignisbenachrichtigungen zu registrieren, indem Sie Ihren Delegaten der Liste der Ereignislistener „hinzufügen“.
In Java rufen Sie `SetXXXListener` auf, um die Registrierung aufzuheben, während Sie in C# `-=` verwenden, um Ihren Delegaten von der Liste der Listener zu „subtrahieren“.

In Xamarin.Android werden Ereignisse häufig dazu verwendet, Objekte zu benachrichtigen, wenn ein Benutzer eine Aktion mit einem Oberflächensteuerelement ausführt. Normalerweise besitzt ein Oberflächensteuerelement Member, die mit dem Schlüsselwort `event` definiert sind. Sie fügen Ihre Delegaten diesen Membern hinzu, um Ereignisse dieses Oberflächensteuerelements zu abonnieren.

So abonnieren Sie ein Ereignis:

1. Erstellen Sie ein Delegatobjekt, das auf die Methode verweist, die beim Auftreten des Ereignisses aufgerufen werden soll.

2. Verwenden Sie den Operator `+=`, um Ihren Delegaten an das Ereignis anzufügen, das Sie abonnieren.

Das folgende Beispiel definiert einen Delegaten (mit einer expliziten Verwendung des Schlüsselworts `delegate`), der Schaltflächenklicks abonniert.
Dieser Schaltflächenklickhandler startet eine neue Aktivität:

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

Allerdings können Sie auch einen Lambdaausdruck verwenden, um sich für Ereignisse zu registrieren, und das Schlüsselwort `delegate` ganz auslassen. Zum Beispiel:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

In diesem Beispiel weist das `startActivityButton`-Objekt ein Ereignis auf, das einen Delegaten mit einer bestimmten Methodensignatur erwartet: einen Delegaten, der Absender- und Ereignisargumente akzeptiert und void zurückgibt. Da wir uns jedoch nicht die Mühe machen wollen, einen solchen Delegaten oder seine Methode explizit zu definieren, deklarieren wir die Signatur der Methode mit `(sender, e)` und verwenden einen Lambdaausdruck, um den Text des Ereignishandlers zu implementieren.
Beachten Sie, dass wir diese Parameterliste deklarieren müssen, obwohl wir die Parameter `sender` und `e` nicht verwenden.

Es ist wichtig, sich daran zu erinnern, dass Sie einen Delegaten austragen können (über den Operator `-=`), aber Sie können einen Lambdaausdruck nicht austragen &ndash; ein solcher Versuch kann zu Arbeitsspeicherverlusten führen. Verwenden Sie die Lambdaform der Ereignisregistrierung nur dann, wenn Ihr Handler das Ereignis nicht austragen wird.

In der Regel werden Lambdaausdrücke verwendet, um Ereignishandler im Xamarin.Android-Code zu deklarieren. Diese verkürzte Art und Weise, Ereignishandler zu deklarieren, mag zunächst kryptisch erscheinen, spart aber enorm viel Zeit beim Schreiben und Lesen von Code. Mit zunehmender Vertrautheit gewöhnen Sie sich daran, dieses Muster zu erkennen (das häufig in Xamarin.Android-Code vorkommt), und verbringen mehr Zeit damit, über die Geschäftslogik Ihrer Anwendung nachzudenken, anstatt sich mit syntaktischem Mehraufwand herumzuschlagen.

<a name="async" />

## <a name="asynchronous-programming"></a>Asynchrone Programmierung

*Asynchrone Programmierung* ist eine Möglichkeit, die allgemeine Reaktionsfähigkeit Ihrer Anwendung zu verbessern. Asynchrone Programmierungsfeatures ermöglichen es, dass der Rest des Programmcodes weiterhin ausgeführt wird, während ein Teil der Anwendung durch einen langwierigen Vorgang blockiert wird.
Der Zugriff auf das Web, die Verarbeitung von Bildern und das Lesen/Schreiben von Dateien sind Beispiele für Vorgänge, die dazu führen können, dass eine ganze App einzufrieren scheint, wenn sie nicht asynchron geschrieben wird.

C# enthält Unterstützung auf Sprachebene für asynchrone Programmierung über die Schlüsselwörter `async` und `await`. Diese Sprachfeatures machen es sehr einfach, Code zu schreiben, der Tasks mit langer Ausführungszeit ausführt, ohne den Hauptthread Ihrer Anwendung zu blockieren. Kurz gesagt, verwenden Sie das Schlüsselwort `async` für eine Methode, um anzugeben, dass der Code in der Methode asynchron ausgeführt werden soll und nicht den Thread des Aufrufers blockiert. Sie verwenden das Schlüsselwort `await`, wenn Sie Methoden aufrufen, die mit `async` gekennzeichnet sind. Der Compiler interpretiert `await` als den Punkt, an dem Ihre Methodenausführung in einen Hintergrundthread verschoben werden soll (ein Task wird an den Aufrufer zurückgegeben). Wenn dieser Task beendet ist, wird die Ausführung des Codes für den Thread des Anrufers am `await`-Punkt in Ihrem Code fortgesetzt, und die Ergebnisse des `async`-Aufrufs werden zurückgegeben. Gemäß der Konvention verwenden Methoden, die asynchron ausgeführt werden, `Async` als Suffix ihres Namens.

In Xamarin.Android-Anwendungen werden `async` und `await` in der Regel verwendet, um den UI-Thread freizugeben, sodass er auf Benutzereingaben reagieren kann (wie z.B. das Tippen auf eine Schaltfläche **Abbrechen**), während ein Vorgang mit langer Ausführungszeit in einem Hintergrundtask stattfindet.

Im folgenden Beispiel bewirkt ein Schaltflächenklick-Ereignishandler, dass ein asynchroner Vorgang ein Bild aus dem Web herunterlädt:

```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

Wenn der Benutzer in diesem Beispiel auf das Steuerelement `downloadButton` klickt, erstellt der `downloadAsync`-Ereignishandler ein `WebClient`-Objekt und ein `Uri`-Objekt, um ein Bild von der angegebenen URL abzurufen. Anschließend ruft er die `DownloadDataTaskAsync`-Methode des `WebClient`-Objekts mit dieser URL zum Abrufen des Bilds auf.

Beachten Sie, dass die Methodendeklaration von `downloadAsync` durch das Schlüsselwort `async` eingeleitet wird, um anzugeben, dass sie asynchron ausgeführt wird und einen Task zurückgibt. Beachten Sie auch, dass dem Aufruf von `DownloadDataTaskAsync` das Schlüsselwort `await` vorangestellt ist. Die App verschiebt die Ausführung des Ereignishandlers (beginnend an der Stelle, an der `await` auftritt) in einen Hintergrundthread, bis `DownloadDataTaskAsync` beendet ist und die Rückgabe erfolgt.
In der Zwischenzeit kann der UI-Thread der App immer noch auf Benutzereingaben reagieren und Ereignishandler für die anderen Steuerelemente auslösen. Wenn `DownloadDataTaskAsync` abgeschlossen ist (dies kann einige Sekunden dauern), wird die Ausführung fortgesetzt, wobei die Variable `bytes` auf das Ergebnis des Aufrufs von `DownloadDataTaskAsync` festgelegt wird und der Rest des Ereignishandlercodes das heruntergeladene Bild im Thread des Anrufers (Benutzeroberfläche) anzeigt.

Eine Einführung zu `async`/`await` in C# finden Sie im Artikel [Asynchrones Programmieren mit Async und Await](https://docs.microsoft.com/dotnet/csharp/async).
Weitere Informationen zur Xamarin-Unterstützung von asynchronen Programmierfeatures finden Sie unter [Unterstützung asynchroner Features: Übersicht](~/cross-platform/platform/async.md).

<a name="keywords" />

## <a name="keyword-differences"></a>Schlüsselwortunterschiede

Viele Sprachschlüsselwörter, die in Java verwendet werden, werden auch in C# verwendet. Es gibt auch eine Reihe von Java-Schlüsselwörtern, die ein äquivalentes, aber anders benanntes Gegenstück in C# haben, wie in der folgenden Tabelle aufgeführt:

|Java|C#|Beschreibung|
|---|---|---|
|`boolean`|[bool](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/bool)|Wird zum Deklarieren der booleschen Werte „true“ und „false“ verwendet.|
|`extends`|`:`|Wird der Klasse und den Schnittstellen, von denen geerbt werden soll, vorangestellt.|
|`implements`|`:`|Wird der Klasse und den Schnittstellen, von denen geerbt werden soll, vorangestellt.|
|`import`|[using](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/using)|Importiert Typen aus einem Namespace. Wird auch zum Erstellen eines Namespacealias verwendet.|
|`final`|[sealed](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/sealed)|Verhindert die Ableitung von Klassen. Verhindert, dass Methoden und Eigenschaften in abgeleiteten Klassen überschrieben werden.|
|`instanceof`|[is](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/is)|Wertet aus, ob ein Objekt mit einem angegebenen Typ kompatibel ist.|
|`native`|[extern](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/extern)|Deklariert eine Methode, die extern implementiert wird.|
|`package`|[namespace](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/namespace)|Deklariert einen Bereich für eine in Beziehung stehende Gruppe von Objekten.|
|`T...`|[params T](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/params)|Gibt einen Methodenparameter an, der eine variable Anzahl von Argumenten annimmt.|
|`super`|[base](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/base)|Wird verwendet, um auf Member der übergeordneten Klasse aus einer abgeleiteten Klasse heraus zuzugreifen.|
|`synchronized`|[lock](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/lock-statement)|Umschließt einen wichtigen Abschnitt des Codes durch den Abruf und die Freigabe von Sperren.|

Außerdem gibt es viele Schlüsselwörter, die einzigartig in C# sind und kein Gegenstück in der unter Android verwendeten Java-Variante besitzen. Xamarin.Android-Code verwendet häufig die folgenden C#-Schlüsselwörter (diese Tabelle ist nützlich, um sich auf sie zu beziehen, wenn Sie Xamarin.Android-[Beispielcode](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android) lesen):

|C#|Beschreibung|
|---|---|
|[as](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/as)|Führt Konvertierungen zwischen kompatiblen Verweistypen oder Nullwerte zulassenden Typen aus.|
|[async](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)|Gibt an, dass eine Methode oder ein Lambdaausdruck asynchron ist.|
|[await](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/await)|Hält die Ausführung einer Methode an, bis ein Task abgeschlossen ist.|
|[byte](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/byte)|8-Bit-Ganzzahltyp ohne Vorzeichen.|
|[delegate](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/delegate)|Wird verwendet, um eine Methode oder eine anonyme Methode zu kapseln.|
|[enum](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/enum)|Deklariert eine Enumeration, einen Satz benannter Konstanten.|
|[event](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/event)|Deklariert ein Ereignis in einer Publisher-Klasse.|
|[fixed](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/fixed-statement)|Verhindert, dass eine Variable verschoben wird.|
|`get`|Definiert eine Accessormethode, die den Wert einer Eigenschaft abruft.|
|[in](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/in-generic-modifier)|Ermöglicht es einem Parameter, einen weniger abgeleiteten Typ in einer generischen Schnittstelle zu akzeptieren.|
|[object](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/object)|Ein Alias für den Objekttyp in .NET Framework.|
|[out](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/out)|Parametermodifizierer oder generische Typparameterdeklaration.|
|[override](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/override)|Erweitert oder ändert die Implementierung eines geerbten Member.|
|[partial](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/partial-method)|Deklariert, dass eine Definition in mehrere Dateien aufgeteilt werden soll, oder trennt eine Methodendefinition von ihrer Implementierung.|
|[readonly](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/readonly)|Deklariert, dass ein Klassenmember nur zum Zeitpunkt der Deklaration oder durch den Klassenkonstruktor zugewiesen werden kann.|
|[ref](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/ref)|Bewirkt, dass ein Argument nicht als Wert, sondern als Verweis übergeben wird.|
|[set](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/set)|Definiert eine Accessormethode, die den Wert einer Eigenschaft festlegt.|
|[string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)|Ein Alias für den Zeichenfolgentyp in .NET Framework.|
|[struct](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/struct)|Ein Werttyp, der eine Gruppe in Beziehung stehender Variablen kapselt.|
|[typeof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/typeof)|Ruft den Typ eines Objekts ab.|
|[var](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/var)|Deklariert eine implizit typisierte lokale Variable.|
|[value](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value)|Verweist auf den Wert, den der Clientcode einer Eigenschaft zuweisen möchte.|
|[virtual](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/virtual)|Ermöglicht das Überschreiben einer Methode in einer abgeleiteten Klasse.|

<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>Interoperabilität mit vorhandenen Java-Code

Wenn Sie über vorhandene Java-Funktionalität verfügen, die Sie nicht nach C# konvertieren möchten, können Sie Ihre vorhandenen Java-Bibliotheken in Xamarin.Android-Anwendungen mithilfe von zwei Techniken wiederverwenden:

- **Erstellen einer Java Bindungsbibliothek**: Mit diesem Ansatz verwenden Sie Xamarin-Tools, um C#-Wrapper um Java-Typen zu generieren. Diese Wrapper werden als *Bindungen* bezeichnet. Als Ergebnis kann Ihre Xamarin.Android-Anwendung Ihre *JAR*-Datei verwenden, indem sie diese Wrapper aufruft.

- **Java Native Interface**: *Java Native Interface* (JNI) ist ein Framework, das es ermöglicht, dass C#-Apps Java-Code aufrufen oder durch Java-Code aufgerufen werden können.

Weitere Informationen zu diesen Techniken finden Sie unter [Übersicht über die Java-Integration](~/android/platform/java-integration/index.md).

## <a name="further-reading"></a>Weiterführende Themen

Das [C#-Programmierhandbuch](https://docs.microsoft.com/dotnet/csharp/programming-guide/) von MSDN ist eine großartige Möglichkeit, um mit dem Erlernen der Programmiersprache C# zu beginnen, und Sie können die [C#-Referenz](https://docs.microsoft.com/dotnet/csharp/language-reference/) verwenden, um bestimmte C#-Sprachfeatures nachzuschlagen.

So wie Java-Kenntnisse mindestens genauso viel mit der Vertrautheit mir den Java-Klassenbibliotheken zu tun haben wie mit Kenntnissen der Java-Sprache, so erfordern praktische Kenntnisse von C# eine gewisse Vertrautheit mit .NET-Framework. Das Lernpaket [Umstieg auf C# und .NET Framework für Java-Entwickler](https://www.microsoft.com/download/details.aspx?id=6073) ist ein guter Weg, um mehr über .NET Framework aus Java-Perspektive zu erfahren (und gleichzeitig ein tieferes Verständnis von C# zu erlangen).

Wenn Sie bereit sind, Ihr erstes Xamarin.Android-Projekt in C# in Angriff zu nehmen, kann Ihnen unsere [Hallo Android](~/android/get-started/hello-android/index.md)-Serie dabei helfen, Ihre erste Xamarin.Android-Anwendung zu erstellen und Ihr Verständnis der Grundlagen der Android-Anwendungsentwicklung mit Xamarin weiter zu vertiefen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine Einführung in die C#-Programmierumgebung Xamarin.Android aus der Sicht eines Java-Entwicklers bereitgestellt. Die Ähnlichkeiten zwischen C# und Java wurde gezeigt, und ihre praktischen Unterschiede wurden erläutert. Assemblys und Namespaces wurden vorgestellt, der Import externer Typen wurde erläutert und ein Überblick über die Unterschiede bei Zugriffsmodifizierern ermöglicht. Außerdem wurden Informationen zu Generics, Klassenableitung, Aufruf von Methoden der Basisklasse, Methodenüberschreibung und Ereignisbehandlung bereitgestellt. Der Artikel hat C#-Funktionen vorgestellt, die in Java nicht verfügbar sind (z.B. Eigenschaften) sowie asynchrone `async`/`await`-Programmierung, Lambdaausdrücke, C#-Delegaten und das C#-Ereignisverarbeitungssystem erläutert. Er enthält Tabellen mit wichtigen C#-Schlüsselwörtern, beschreibt, wie die Interaktion mit vorhandenen Java-Bibliotheken erfolgt, und bietet Links zu verwandter Dokumentation für weitere Studien.

## <a name="related-links"></a>Verwandte Links

- [Übersicht über die Java-Integration](~/android/platform/java-integration/index.md)
- [C#-Programmierhandbuch](https://docs.microsoft.com/dotnet/csharp/programming-guide/)
- [C#-Referenz](https://docs.microsoft.com/dotnet/csharp/language-reference/index)
- [Umstieg auf C# und .NET Framework für Java-Entwickler](https://www.microsoft.com/download/details.aspx?id=6073)
