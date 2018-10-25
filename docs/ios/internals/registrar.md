---
title: Typ-Registrierungsstelle für Xamarin.iOS
description: Dieses Dokument beschreibt die Xamarin.iOS-Typ-Registrierungsstelle, wodurch C# Klassen, die die Objective-C-Laufzeit zur Verfügung.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 8/29/2018
ms.openlocfilehash: cdd57095b03c24472abec5646ee3a70350770d7c
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "34786173"
---
# <a name="type-registrar-for-xamarinios"></a>Typ-Registrierungsstelle für Xamarin.iOS

Dieses Dokument beschreibt die Registrierung-Typsystem, die von Xamarin.iOS verwendet wird.

## <a name="registration-of-managed-classes-and-methods"></a>Registrierung von verwalteten Klassen und Methoden

Während des Starts werden Xamarin.iOS registrieren:

- -Klassen mit einem [[registrieren]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) -Attribut als Objective-C-Klassen.
- -Klassen mit einem [[Category]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) -Attribut als Objective-C-Kategorien.
- Eine Schnittstelle mit einer [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) -Attribut als Objective-C-Protokolle.
- Elemente mit einer [[Export]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/), wodurch für Objective-C, um darauf zuzugreifen.

Betrachten Sie beispielsweise die verwaltete `Main` Methode, die häufig in Xamarin.iOS-Anwendungen:

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

Dieser Code weist die Objective-C-Laufzeit Typs mit dem Namen `AppDelegate` als "Delegate"-Klasse der Anwendung. Für die Objective-C-Laufzeit, um eine Instanz erstellen zu können die C# `AppDelegate` Klasse, dass Klasse erfasst werden muss.

Xamarin.iOS führt die Registrierung automatisch zur Laufzeit (dynamische Registrierung) oder zum Zeitpunkt der Kompilierung (statische Registrierung).

Dynamische Registrierung verwendet Reflektion, beim Start auf alle Klassen und Methoden zu registrieren, finden sie an die Objective-C-Laufzeit übergeben. Dynamische Registrierung wird standardmäßig verwendet, bei simulatorbuilds.

Statische Registrierung untersucht, zum Zeitpunkt der Kompilierung, Assemblys, die von der Anwendung verwendet. Bestimmt die Klassen und Methoden zum Registrieren mit Objective-C und generiert eine Karte, die in die Binärdatei eingebettet ist.
Klicken Sie dann registriert beim Starten sie die Zuordnung für Objective-C-Laufzeit. Statische Registrierung wird für Geräte-Builds verwendet.

### <a name="categories"></a>Kategorien

Ab Xamarin.iOS 8.10, es ist möglich, mithilfe von Objective-C-Kategorien erstellen C# Syntax.

Verwenden Sie zum Erstellen einer Kategorie die `[Category]` Attribut, und geben Sie den Typ erweitern. Der folgende Code z. B. erweitert `NSString`:

```csharp
[Category (typeof (NSString))]
```

Jede der Methoden von einer Kategorie verfügt über eine `[Export]` Attribut, und somit die Objective-C-Laufzeit zur Verfügung:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

Alle verwalteten Erweiterungsmethoden müssen statisch sein, aber es ist möglich, zum Erstellen von Objective-C-Instanz-Methoden, die mit dem Standard C# Syntax für Erweiterungsmethoden:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

Das erste Argument für die Erweiterungsmethode ist die Instanz, auf der die Methode aufgerufen wurde:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
 }
 ```

In diesem Beispiel wird ein systemeigenes hinzufügen `toUpper` Instanzmethode, die `NSString` Klasse. Diese Methode kann aufgerufen werden, von Objective-c:

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

### <a name="protocols"></a>Protokolle

Ab Xamarin.iOS 8.10, eine Schnittstelle mit der `[Protocol]` Attribut wird als Protokolle mit Objective-C exportiert werden:

```csharp
[Protocol ("MyProtocol")]
interface IMyProtocol
{
    [Export ("method")]
    void Method ();
}

class MyClass : IMyProtocol
{
    void Method ()
    {
    }
}
```

Dieser Code exportiert `IMyProtocol` Objective-C als ein Protokoll mit dem Namen `MyProtocol` und eine Klasse mit dem Namen `MyClass` , der das Protokoll implementiert.

## <a name="new-registration-system"></a>Neue Registrierungssystem

Die stabile 6.2.6 ab Version und die Betaversion 6.3.4, wir haben eine neue statische Registrierungsstelle hinzugefügt. In der 7.2.1 Version, wir haben der neuen Registrierungsstelle die Standardeinstellung.

Diese neue Registrierungssystem bietet die folgenden neuen Features:

- Während der Kompilierung die Erkennung von Fehlern für Programmierer:
    - Zwei Klassen, die mit dem gleichen Namen registriert wird.
    - Mehr als eine Methode, die zum Antworten auf die denselben Selektor exportiert
- Entfernen von nicht verwendeten systemeigenen Code:
    - Das neue Registrierungssystem wird starken Verweise in Code verwendet, die in statische Bibliotheken, ermöglicht den nativen Linker, die Sie nicht verwendete nativem Code aus der daraus resultierende Binärdatei entfernen hinzugefügt. Für Xamarin Beispiel-Bindungen werden die meisten Anwendungen mindestens 300 KB kleiner.

- Unterstützung für generische Unterklassen von `NSObject`; finden Sie unter [NSObject Generika](~/ios/internals/api-design/nsobject-generics.md) für Weitere Informationen. Außerdem fängt das neue Registrierungssystem nicht unterstützter generische Konstrukte, die zuvor zufälliges Verhalten zur Laufzeit verursacht hätte.

### <a name="errors-caught-by-the-new-registrar"></a>Fehler, die von der neuen Registrierungsstelle abgefangen

Im folgenden finden Sie einige Beispiele für die Fehler, die von der neuen Registrierungsstelle abgefangen.

- Exportieren den gleichen Selektor mehr als einmal in der gleichen Klasse:

    ```csharp
    [Register]
    class MyDemo : NSObject 
    {
        [Export ("foo:")]
        void Foo (NSString str);
        [Export ("foo:")]
        void Foo (string str)
    }
    ```

- Exportieren von mehr als eine verwaltete Klasse mit dem gleichen Namen für die Objective-C:

    ```csharp
    [Register ("Class")]
    class MyClass : NSObject {}

    [Register ("Class")]
    class YourClass : NSObject {}
    ```

- Exportieren von generischen Methoden ein:

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo")]
        void Foo<T> () {}
    }
    ```

### <a name="limitations-of-the-new-registrar"></a>Einschränkungen für die neue Registrierungsstelle

Einige Aspekte im Hinblick auf die neue Registrierungsstelle zu:

- Einige Drittanbieter-Bibliotheken müssen aktualisiert werden, um die Arbeit mit der neuen Registrierung-Systems. Finden Sie unter [erforderlichen Änderungen](#required_modifications) unten Weitere Informationen.

- Ein kurzfristige Nachteil ist auch Clang muss verwendet werden, wenn das Framework für die Konten verwendet wird (Grund hierfür ist, von Apple **accounts.h** Header kann nur kompiliert werden, indem Clang). Hinzufügen `--compiler:clang` auf die weitere Mtouch-Argumente Clang verwenden, wenn Sie Xcode 4.6 oder früher verwenden (Xamarin.iOS wählt automatisch Clang in Xcode 5.0 oder höher.)

- Wenn Xcode 4.6 (oder früher) verwendet, GCC / G-muss ausgewählt werden, wenn der Typ exportiert Namen enthalten nur ASCII-Zeichen (Dies ist da die Version des Clang geliefert, mit Xcode 4.6 das ASCII-Zeichen in Bezeichnern in Objective-C-Code nicht unterstützt). Hinzufügen `--compiler:gcc` auf die weitere Mtouch-Argumente GCC verwenden.

## <a name="selecting-a-registrar"></a>Auswählen von einer Registrierungsstelle

Sie können eine andere Registrierungsstelle auswählen, indem Sie eine der folgenden Optionen an die weitere Mtouch-Argumente des Projekts **iOS-Build** Einstellungen:

- `--registrar:static` – default für Geräte-builds
- `--registrar:dynamic` – für die simulatorbuilds standardmäßig

> [!NOTE]
> Xamarin Classic-API unterstützt andere Optionen, z. B. `--registrar:legacystatic` und `--registrar:legacydynamic`. Allerdings werden diese Optionen, von der Unified API nicht unterstützt.

## <a name="shortcomings-in-the-old-registration-system"></a>Mängel aus dem alten Registrierungssystem

Das alte Registrierungssystem hat die folgenden Nachteile:

- Es wurde keine (systemeigenen) statischen Verweis auf Objective-C-Klassen und Methoden in Drittanbieter-native Bibliotheken, die bedeutete, dass wir nicht den nativen Linker bitten konnte, nativen Code von Drittanbietern zu entfernen, der tatsächlich verwendeten war nicht (da alles entfernt würde). Dies ist der Grund für die `-force_load libNative.a` , die jede Bindung von Drittanbietern musste (oder einer entsprechenden `ForceLoad=true` in die `[LinkWith]` Attribut).
- Sie können zwei verwaltete Typen ohne Warnung mit dem gleichen Namen für die Objective-C exportieren. Ein seltenes Szenario wurde mit zwei `AppDelegate` Klassen in unterschiedlichen Namespaces. Zur Laufzeit wäre es vollständig wahlfreien, welche (genauer gesagt es variiert zwischen den Ausführungen einer App, die auch neu erstellt, wurde nicht – was für eine sehr Frameworkversionen und frustrierendes Debugleistung) ausgewählt wurde.
- Sie können zwei Methoden, mit der gleichen Signatur von Objective-C exportieren. Erneut zufällige wurde die von Objective-C aufgerufen werden sollen (jedoch dieses Problem nicht so häufig wie das vorherige Beispiel, war, hauptsächlich deshalb, weil die einzige Möglichkeit, diesen Fehler tatsächlich auftreten, die weniger verwaltete Methode überschreiben wurde).
- Der Satz von Methoden, der exportiert wurde, war geringfügig zwischen dynamischen und statischen Builds.
- Es funktioniert nicht ordnungsgemäß beim Exportieren von generischer Klassen (die genaue generische Implementierung zur Laufzeit ausgeführt wäre zufällig, wodurch effektiv unbestimmten Verhalten).

## <a name="new-registrar-required-changes-to-bindings"></a>Neue Registrierung: erforderliche Änderungen für Bindungen

Dieser Abschnitt beschreibt die Änderungen für Bindungen, die für die Arbeit mit dem neuen Registrar vorgenommen werden müssen.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokolle müssen das [Protocol]-Attribut aufweisen.

Protokolle müssen nun die `[Protocol]` Attribut. Wenn Sie dies nicht tun, werden Sie z. B. einen nativen Linker-Fehler:

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Selektoren müssen eine gültige Anzahl von Parametern haben.

Alle Selektoren müssen die Anzahl der Parameter ordnungsgemäß angeben. Zuvor werden diese Fehler wurden ignoriert und Laufzeit Probleme verursachen können.

Die Anzahl der Doppelpunkte muss kurz gesagt: die Anzahl der Parameter übereinstimmen:

- Ohne Parameter: `foo`
- Einen Parameter: `foo:`
- Zwei Parameter: `foo:parameterName2:`

Im folgenden sind falsch verwendet:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Verwenden Sie Export IsVariadic-parameter

Variadic-Funktionen verwenden, müssen die `IsVariadic` Argument für die `[Export]` Attribut:

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Müssen mit der vorhandenen Symbole verknüpfen

Es ist unmöglich, Klassen zu binden, die in der nativen Bibliothek nicht vorhanden sind.
Wenn eine Klasse aus entfernt oder in der nativen Bibliothek umbenannt wurde, stellen Sie sicher, dass die Bindungen entsprechend zu aktualisieren.
