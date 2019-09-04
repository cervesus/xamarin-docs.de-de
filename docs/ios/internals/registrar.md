---
title: Typregistrierungs Stelle für xamarin. IOS
description: Dieses Dokument beschreibt die xamarin. IOS-typregistrierungs Stelle, C# die Klassen für die Ziel-C-Laufzeit verfügbar macht.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/29/2018
ms.openlocfilehash: c761290f43d780b2eafcf416fb9edf1e069f65c3
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226032"
---
# <a name="type-registrar-for-xamarinios"></a>Typregistrierungs Stelle für xamarin. IOS

In diesem Dokument wird das von xamarin. IOS verwendete typregistrierungs System beschrieben.

## <a name="registration-of-managed-classes-and-methods"></a>Registrierung von verwalteten Klassen und Methoden

Beim Start wird xamarin. IOS registriert:

- Klassen mit einem [[Register]](xref:Foundation.RegisterAttribute) -Attribut als Ziel-C-Klassen.
- Klassen mit einem [[Category]](xref:ObjCRuntime.CategoryAttribute) -Attribut als Ziel-C-Kategorien.
- Schnittstellen mit einem [[Protocol]](xref:Foundation.ProtocolAttribute) -Attribut als Ziel-C-Protokolle.
- Elemente mit einem [[Export]](xref:Foundation.ExportAttribute), mit denen Ziel-C darauf zugreifen kann.

Angenommen, die verwaltete `Main` Methode wird in xamarin. IOS-Anwendungen häufig eingesetzt:

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

Dieser Code weist die Ziel-C-Laufzeit an, den Typ `AppDelegate` zu verwenden, der als Delegatklasse der Anwendung aufgerufen wird. Damit die Ziel-C-Laufzeit eine Instanz der C# `AppDelegate` -Klasse erstellen kann, muss diese Klasse registriert werden.

Xamarin. IOS führt die Registrierung automatisch aus, entweder zur Laufzeit (dynamische Registrierung) oder zur Kompilierzeit (statische Registrierung).

Die dynamische Registrierung verwendet Reflektion beim Start, um alle Klassen und Methoden zu suchen, die registriert werden sollen, und übergibt sie an die Ziel-C-Laufzeit. Die dynamische Registrierung wird standardmäßig für simulatorbuilds verwendet.

Bei der statischen Registrierung werden die Assemblys, die von der Anwendung verwendet werden, zur Kompilierzeit überprüft. Es bestimmt die Klassen und Methoden, die mit dem Ziel-C registriert werden sollen, und generiert eine Zuordnung, die in die Binärdatei eingebettet ist.
Beim Start wird die Zuordnung dann bei der Ziel-C-Laufzeit registriert. Die statische Registrierung wird für gerätebuilds verwendet.

### <a name="categories"></a>Kategorien

Ab xamarin. IOS 8,10 ist es möglich, Ziel-C-Kategorien mithilfe C# der-Syntax zu erstellen.

Um eine Kategorie zu erstellen, verwenden `[Category]` Sie das-Attribut, und geben Sie den zu erweiternden Typ an. Der folgende Code erweitert `NSString`z. b.:

```csharp
[Category (typeof (NSString))]
```

Jede der Methoden einer Kategorie verfügt über ein `[Export]` Attribut, das für die Ziel-C-Laufzeit verfügbar ist:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

Alle verwalteten Erweiterungs Methoden müssen statisch sein, aber es ist möglich, Ziel-C-Instanzmethoden mithilfe der C# Standard Syntax für Erweiterungs Methoden zu erstellen:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

Das erste Argument der Erweiterungsmethode ist die Instanz, für die die Methode aufgerufen wurde:

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

In diesem Beispiel wird der `toUpper` `NSString` -Klasse eine native Instanzmethode hinzugefügt. Diese Methode kann von "Ziel-C" aufgerufen werden:

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

Ab xamarin. IOS 8,10 werden Schnittstellen mit dem `[Protocol]` -Attribut als Protokolle in "Ziel-C" exportiert:

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

Dieser Code exportiert `IMyProtocol` in "Ziel-C" als Protokoll `MyProtocol` mit dem Namen und `MyClass` eine Klasse mit dem Namen, die das Protokoll implementiert.

## <a name="new-registration-system"></a>Neues Registrierungssystem

Beginnend mit der stabilen 6.2.6-Version und der Beta Version 6.3.4 haben wir eine neue statische Registrierungsstelle hinzugefügt. In der 7.2.1-Version haben wir die neue Registrierungsstelle zum Standardwert gemacht.

Dieses neue Registrierungssystem bietet die folgenden neuen Features:

- Erkennung von Programmierern zur Kompilierzeit:
  - Zwei Klassen, die mit demselben Namen registriert werden.
  - Es wurde mehr als eine Methode exportiert, um auf denselben Selektor zu reagieren.
- Entfernen von nicht verwendetem nativem Code:
  - Das neue Registrierungssystem fügt starke Verweise auf den Code hinzu, der in statischen Bibliotheken verwendet wird, sodass der Native Linker nicht verwendeten systemeigenen Code aus der resultierenden Binärdatei entfernen kann. Bei den Beispiel Bindungen von xamarin werden die meisten Anwendungen mindestens 300 KB kleiner.

- Unterstützung für generische Unterklassen `NSObject`von. Weitere Informationen finden Sie unter [NSObject Generika](~/ios/internals/api-design/nsobject-generics.md) . Außerdem fängt das neue Registrierungssystem nicht unterstützte generische Konstrukte ab, die vorher ein zufälliges Verhalten zur Laufzeit verursacht haben.

### <a name="errors-caught-by-the-new-registrar"></a>Von der neuen Registrierungsstelle aufgefangene Fehler

Im folgenden finden Sie einige Beispiele für die Fehler, die von der neuen Registrierungsstelle abgefangen wurden.

- Der gleiche Selektor wird in derselben Klasse mehrmals exportiert:

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

- Exportieren von mehr als einer verwalteten Klasse mit dem gleichen Ziel-C-Namen:

  ```csharp
  [Register ("Class")]
  class MyClass : NSObject {}

  [Register ("Class")]
  class YourClass : NSObject {}
  ```

- Exportieren von generischen Methoden:

  ```csharp
  [Register]
  class MyDemo : NSObject
  {
      [Export ("foo")]
      void Foo<T> () {}
  }
  ```

### <a name="limitations-of-the-new-registrar"></a>Einschränkungen der neuen Registrierungsstelle

Einige Dinge, die Sie über die neue Registrierungsstelle beachten sollten:

- Einige Bibliotheken von Drittanbietern müssen aktualisiert werden, um mit dem neuen Registrierungssystem zusammenarbeiten zu können. Weitere Informationen finden Sie unten unter [erforderliche Änderungen](#required-modifications) .

- Ein kurzfristiger Nachteil ist auch, dass clang verwendet werden muss, wenn das Accounts-Framework verwendet wird (Dies liegt daran, dass der " **Accounts. h** "-Header von Apple nur von clang kompiliert werden kann). Fügen `--compiler:clang` Sie den zusätzlichen mtouchscreen-Argumenten hinzu, um clang zu verwenden, wenn Sie Xcode 4,6 oder früher verwenden (xamarin. IOS wählt clang automatisch in Xcode 5,0 oder höher aus).

- Wenn Xcode 4,6 (oder früher) verwendet wird, muss gcc/G + + ausgewählt werden, wenn exportierte Typnamen nicht-ASCII-Zeichen enthalten (Dies liegt daran, dass die in Xcode 4,6 enthaltene Version von clang keine nicht-ASCII-Zeichen innerhalb von Bezeichnerzeichen in Ziel-C-Code unterstützt). Fügen `--compiler:gcc` Sie den zusätzlichen mberührungs-Argumenten hinzu, um gcc zu verwenden.

## <a name="selecting-a-registrar"></a>Auswählen einer Registrierungsstelle

Sie können eine andere Registrierungsstelle auswählen, indem Sie eine der folgenden Optionen zu den zusätzlichen mberührungs-Argumenten in den **IOS** -Buildeinstellungen des Projekts hinzufügen:

- `--registrar:static`– Standard für gerätebuilds
- `--registrar:dynamic`– Standard für simulatorbuilds

> [!NOTE]
> Xamarin Classic API unterstützt andere Optionen, wie `--registrar:legacystatic` z `--registrar:legacydynamic`. b. und. Diese Optionen werden jedoch nicht vom Unified API unterstützt.

## <a name="shortcomings-in-the-old-registration-system"></a>Mängel im alten Registrierungssystem

Das alte Registrierungssystem weist die folgenden Nachteile auf:

- In nativen Bibliotheken von Drittanbietern gab es keinen (nativen) statischen Verweis auf Ziel-C-Klassen und-Methoden, was bedeutete, dass wir den nativen Linker nicht bitten konnten, nativen Code von Drittanbietern zu entfernen, der eigentlich nicht verwendet wurde (weil alles entfernt werden würde). Dies ist der Grund für das `-force_load libNative.a` , das jede Drittanbieter Bindung durchführen musste (oder das Äquivalent `ForceLoad=true` im `[LinkWith]` -Attribut).
- Sie können zwei verwaltete Typen mit dem gleichen Ziel-C-Namen ohne Warnung exportieren. Ein seltenen Szenario besteht darin, zwei `AppDelegate` Klassen in verschiedenen Namespaces zu schließen. Zur Laufzeit wäre es vollständig zufällig, welches ausgewählt wurde (tatsächlich war es zwischen den Ausführungen einer APP, die noch nicht neu erstellt wurde), was für eine sehr Rätsel freie und frustrierende Debuggingumgebung gesorgt hat.
- Sie können zwei Methoden mit der gleichen Ziel-C-Signatur exportieren. Der Aufruf von "Ziel-C" würde jedoch zufällig erfolgen (dieses Problem war jedoch nicht so üblich wie das vorherige, weil die einzige Möglichkeit zum eigentlichen Auftreten dieses Fehlers darin bestand, die nicht glückliche verwaltete Methode zu überschreiben).
- Die exportierten Methoden unterscheiden sich geringfügig von dynamischen und statischen Builds.
- Dies funktioniert nicht ordnungsgemäß, wenn generische Klassen exportiert werden (die exakte generische Implementierung, die zur Laufzeit ausgeführt wird, wäre zufällig, was zu einem unbestimmten Verhalten führt).

<a name="required-modifications" />

## <a name="new-registrar-required-changes-to-bindings"></a>Neue Registrierungsstelle: erforderliche Änderungen an Bindungen

In diesem Abschnitt werden Bindungs Änderungen beschrieben, die vorgenommen werden müssen, um mit der neuen Registrierungsstelle zu arbeiten.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokolle müssen über das [Protocol]-Attribut verfügen.

Protokolle müssen jetzt über das `[Protocol]` -Attribut verfügen. Wenn Sie dies nicht tun, wird ein nativer Linker-Fehler angezeigt, z. b.:

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Selektoren müssen eine gültige Anzahl von Parametern aufweisen.

Alle Selektoren müssen die Anzahl von Parametern ordnungsgemäß angeben. Zuvor wurden diese Fehler ignoriert und können zu Lauf Zeitproblemen führen.

Kurz gesagt, die Anzahl der Doppelpunkte muss mit der Anzahl von Parametern identisch sein:

- Keine Parameter:`foo`
- Ein Parameter:`foo:`
- Zwei Parameter:`foo:parameterName2:`

Im folgenden finden Sie falsche Verwendungsmöglichkeiten:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Isvariadic-Parameter im Export verwenden

Variadic-Funktionen müssen das `IsVariadic` -Argument für `[Export]` das-Attribut verwenden:

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Muss mit vorhandenen Symbolen verknüpft werden.

Es ist nicht möglich, Klassen zu binden, die in der nativen Bibliothek nicht vorhanden sind.
Wenn eine Klasse aus der nativen Bibliothek entfernt oder umbenannt wurde, stellen Sie sicher, dass die Bindungen entsprechend aktualisiert werden.
