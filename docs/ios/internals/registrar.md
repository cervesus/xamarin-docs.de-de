---
title: Typ-Registrierungsstelle für Xamarin.iOS
description: Dieses Dokument beschreibt die Xamarin.iOS Typ Registrierungsstelle, die C#-Klassen für Objective-C-Laufzeit verfügbar macht.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e818d6a2092f408823e4a635a70c4f6666e3a7a9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786173"
---
# <a name="type-registrar-for-xamarinios"></a>Typ-Registrierungsstelle für Xamarin.iOS

Dieses Dokument beschreibt das Typsystem-Registrierung von Xamarin.iOS verwendet.

## <a name="registration-of-managed-classes-and-methods"></a>Registrierung von verwalteten Klassen und Methoden

Während des Starts von registriert Xamarin.iOS:

  - Klassen, mit einem [[registrieren]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Attribut als Objective-C-Klassen.
  - Klassen, mit einem [[Category]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) Attribut als Objective-C-Kategorien.
  - Kommuniziert mit einem [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) Attribut als Objective-C-Protokolle.

und in den einzelnen Fällen Member mit einem [[Exportieren]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) Attribut in der Objective-c exportiert werden Dies ermöglicht es verwaltete Klassen werden erstellt und verwaltet Sie Methoden aus einem Objective-C und besteht die Möglichkeit, Methoden und Eigenschaften zwischen der C#-Welt und Objective-C eine verknüpft sind.

Ein sehr einfaches Beispiel ist die AppDelegate-Klasse besitzt jede Anwendung. Sie denken Sie daran, dass die verwaltete Main-Methode eine Zeile wie diesen verfügt:

    UIApplication.Main (args, null, "AppDelegate");

Das weist die Objective-C-Laufzeit zum Erstellen des Typs mit dem Namen "AppDelegate" als die Anwendung Delegate-Klasse.  Für die Objective-C-Laufzeit wissen, wie zum Erstellen einer Instanz der Klasse "AppDelegate" in c# geschrieben wurde muss diese Klasse registriert werden.

Xamarin.iOS Laufzeit übernimmt die Registrierung, intern, diese Registrierung vollständig zur Laufzeit (dynamische Registrierung) ausgeführt werden kann, oder kann zum Zeitpunkt der Kompilierung (statische Registrierung) ausgeführt werden.  Die dynamische Möglichkeit besteht darin, mithilfe von Reflektion beim Start um zu suchen, die Klassen und Methoden zum Registrieren und die für die Objective-C-Laufzeit zu übergeben.  Die statische Methode untersucht, die von der Anwendung zum Zeitpunkt der Kompilierung verwendeten Assemblys.  Bestimmt, die Klassen und Methoden zu registrierenden Objective-C und generiert eine Karte, die in die Binärdatei eingebettet ist.  Anschließend registrieren Sie beim Start die Zuordnung mit der Objective-C-Laufzeit.

### <a name="categories"></a>Kategorien

Starten mit Xamarin.iOS 8.10 werden möglich, Objective-C-Kategorien, die mithilfe von C#-Syntax zu erstellen.

Dies erfolgt mit dem [Category]-Attribut, die Angabe des Typs als Argument für das Attribut zu erweitern.
Im folgende Beispiel wird z. B. die NSString erweitert:

    [Category (typeof (NSString))]

Jede Kategorie-Methode verwendet das normale Verfahren zum Exportieren von Methoden in Objective-C mit dem [Export]-Attribut:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Alle verwalteten Erweiterungsmethoden müssen statisch sein, aber es ist möglich, verwenden die Standardsyntax für Erweiterungsmethoden in c# Objective-C-Instanzmethoden zu erstellen:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

und das erste Argument der Erweiterungsmethode werden auf der Instanz auf der die Methode aufgerufen wurde.

Vollständiges Beispiel:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

In diesem Beispiel wird eine systemeigene ToUpper-Instanzmethode zu der Klasse NSString hinzuzufügen, die von Objective-c aufgerufen werden können

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>Protokolle

Beginnend mit Xamarin.iOS 8.10 werden Schnittstellen, mit dem Attribut [Protocol] in Objective-C als Protokolle exportiert werden.

Beispiel:

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

Dies wird in Objective-C als ein Protokoll (MyProtocol) und eine Klasse (MyClass), die das Protokoll implementiert exportiert werden.

 **Dynamische Registrierung**

wird für Builds, den Simulator verwendet werden, wie sie den Build/Debug-Zyklus beschleunigt.  Dies ist das Ergebnis des Entfernens der Schritte, das die Zuordnung für die Klasse und die Kompilierung dieser Zuordnung-Tabelle in Ihrem Anwendungsstarter generiert, jedes Mal, wenn Sie die Anwendung starten und stattdessen wird ein allgemeines Startprogramm jedes Mal verwendet.  Desktopcomputer verfügt über ausreichend Potenz, in der Laufzeit ausführen Scannen von Klassen schnell, sodass Leistung nie ein Problem ist.

 **Statische Registrierung**

ist für Geräte-Builds vorgesehen mobile Geräte sind langsamer als desktop-PCs und Ausführen von Common Language Runtime Scannen langsam ist.  Da Gerät erstellt immer eine neue Binärdatei erstellen müssen, der Build/Debug-Zyklus wird nicht von der Erstellung der Registrierung Zuordnung beeinflusst.

## <a name="new-registration-system"></a>Neue Registrierungssystem

Die stabile 6.2.6 ab Version und die Betaversion 6.3.4 haben wir eine neue statische Registrar hinzugefügt. In der 7.2.1 Version wir haben der neuen Registrierungsstelle die Standardeinstellung.

Dieses neue Registrierungssystem bietet die folgenden neuen Features:

- Kompilieren Sie die Zeit zur Erkennung der Programmierer Fehler:
    - Zwei Klassen, die mit dem gleichen Namen registriert wird.
    - Mehr als eine Methode, die zum Antworten auf die gleiche Auswahl exportiert



- Nicht verwendete systemeigenem Code können entfernt werden.
    - Das neue Registrierungssystem wird starke Verweise auf Code in statischen Bibliotheken verwendet ermöglicht die systemeigenen Linker entfernt nicht verwendeten systemeigenen Code aus der resultierende Binärdatei hinzugefügt.
      Für die Xamarin Beispiel-Bindungen werden die meisten Anwendungen über mindestens 300 KB kleiner.

- Unterstützung für generische Unterklassen des NSObject. Finden Sie unter [NSObject Generika](~/ios/internals/api-design/nsobject-generics.md) für Weitere Informationen. Darüber hinaus wird im neuen Registrierungssystem nicht unterstützte generische Konstrukte abfangen, die zuvor zufälliges Verhalten zur Laufzeit verursacht hätte.

Dies sind einige Beispiele für die Fehler, die durch die neue Registar abgefangen:

Exportieren die gleiche Auswahl mehr als einmal in der gleichen Klasse.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

Exportieren von mehr als eine verwaltete Klasse mit dem gleichen Objective-C-Namen ein.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

Generische Methoden werden exportiert.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



Einige Aspekte, die über die neue Registrierungsstelle bedenken:
- Einige Drittanbieter-Bibliotheken müssen aktualisiert werden, um die Arbeit mit der neuen Registrierung-Systems finden Sie im Abschnitt [erforderlichen Änderungen unten](#required_modifications) Weitere Details.
- Ein kurzfristiger Nachteil ist jedoch auch Clang muss verwendet werden, wenn das Framework Konten verwendet wird (Dies ist da Apple accounts.h Header Clang nur kompiliert werden kann). Hinzufügen <code>--compiler:clang</code> auf die zusätzlichen Mtouch Argumente Clang verwenden, wenn Ihre Xcode 4.6 oder früher verwenden (Xamarin.iOS wählt automatisch Clang in Xcode 5.0 oder höher.)

    <li>Wenn Xcode 4.6 (oder früher) wird verwendet, GCC / G ++ muss ausgewählt werden, wenn der Typ exportiert dürfen nicht-ASCII-Zeichen (handelt, da die Version des Clang geliefert Xcode 4.6 das Ascii-Zeichen in Bezeichnern in Objective-C-Code nicht unterstützt) enthalten. Hinzufügen <code>--compiler:gcc</code> auf die zusätzlichen Mtouch Argumente GCC verwenden.


## <a name="selecting-a-registrar"></a>Auswählen einer Registrierungsstelle

Sie können eine andere Registrierungsstelle auswählen, indem Sie eine der folgenden Optionen an die Mtouch zusätzliche Argumente in iOS-Buildoptionen für das Projekt:

-  `--registrar:static` : der Standardwert für die Geräte-builds
-  `--registrar:dynamic` : der Standardwert für die Simulator-builds
-  `--registrar:legacystatic` : der Standardwert für die Geräte bis Xamarin.iOS 7.2.1 builds
-  `--registrar:legacydynamic` : der Standardwert für die Simulator bis Xamarin.iOS 7.2.1 builds


## <a name="shortcomings-in-the-old-registration-system"></a>Mängel in der alten Registrierungssystem

Das alte Registrierungssystem hat die folgenden Nachteile:

-  Es wurde keine (systemeigenen) statische Verweise auf Objective-C-Klassen und Methoden im Drittanbieter-systemeigenen Bibliotheken, die bedeutet, dass wir konnte nicht fordern, dass den systemeigenen Linker systemeigenem Code von Drittanbietern zu entfernen, die tatsächlich verwendet wurden keine, (da alles entfernt würde). Dies ist der Grund für die "-Force_load libNative.a", dass jede Bindung eines Drittanbieters musste (oder die entsprechende ForceLoad = "true" in das Attribut [LinkWith]).
-  Sie können zwei verwaltete Typen mit dem gleichen Objective-C-Namen ohne Warnung exportieren. Ein seltener Fall wurde endet nicht mit zwei AppDelegate-Klassen (in verschiedenen Namespaces) einrichten. Zur Laufzeit wäre vollständig zufällige es, (in der Faktentabelle, die sie zwischen den Ausführungen einer App, die auch neu erstellt, wurde nicht - variiert die für eine Debugleistung erzielen Sie sehr verwirrend und frustrierend gestellt) ausgewählt wurde.
-  Sie können zwei Methoden, mit der gleichen Objective-C-Signatur exportieren. Noch erneut welches aus Objective-C aufgerufen werden, würde wurde zufällige (jedoch dieses Problem nicht so üblich wie das vorherige war, hauptsächlich, da die einzige Möglichkeit, diesen Fehler tatsächlich auftritt, die weniger verwaltete Methode überschreiben wurde).
-  Der Satz von Methoden, der exportiert wurde, wurde geringfügig zwischen dynamischen und statischen Builds.
-  Es funktioniert nicht ordnungsgemäß beim Exportieren von generischer Klassen (welche genaue generische Implementierung zur Laufzeit ausgeführt wäre zufälligen, was zu unbestimmten Verhalten führt).


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>Neue Registrierungsstelle: Erforderliche Änderungen Bindungen


Vorhandene Objective-C-Bindungen müssen möglicherweise aktualisiert werden, um mit der neuen Registar zu arbeiten.

Hier ist eine Liste der Änderungen, die ausgeführt werden müssen.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokolle müssen die [Protocol]-Attribut aufweisen.

Protokolle Implementierungen benötigen jetzt das [Protocol]-Attribut angewendet werden.  Wenn Sie dies nicht tun, werden Sie einen systemeigene Linkerfehler wie diesen:

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Selektoren müssen eine gültige Anzahl von Parametern haben.

Alle Selektoren müssen die Anzahl der Parameter ordnungsgemäß angeben.  Zuvor werden diese Fehler wurden ignoriert und Laufzeit verursachen könnte.

Kurz gesagt, muss die Anzahl der Doppelpunkte die Anzahl von Parametern überein.

Zum Beispiel:

-  Keine Parameter: "Foo"
-  Ein Parameter: "Foo:"
-  Zwei Parameter: "Foo:parameterName2:"


Im folgenden sind falsch verwendet:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Verwenden Sie IsVariadic-Parameter in der Exportdatei

Variadic-Funktionen müssen daher in ihren Export Attribut sagen (Dies ist, da der Selektor über die Anzahl der Argumente falsch ist, d. h. 2 zeigen. von oben verletzt wird):

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Mit vorhandenen Symbole müssen verknüpft werden.

Es ist unmöglich, Klassen zu binden, die in die systemeigene Bibliothek nicht vorhanden waren.

Sie erhalten einen einheitlichen Linkerfehler, wenn Sie versuchen, eine nicht vorhandene Klassen zu binden.

Dies geschieht in der Regel auf, wenn eine Bindung für einige Zeit vorhanden ist und der systemeigene Code, damit eine bestimmte systemeigene Klasse entweder entfernt oder umbenannt wurde, während die Bindung nicht aktualisiert wurde während dieser Zeit geändert wurde.

