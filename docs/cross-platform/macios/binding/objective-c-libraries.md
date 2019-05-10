---
title: Binden von Objective-C-Bibliotheken
description: Dieses Dokument bietet einen Überblick über zum Erstellen von C# Bindungen mit Objective-C-Code, die beschreibt, wie Ereignisse, Methoden, benutzerdefinierte Steuerelemente und vieles mehr zu binden.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 3b81ba51a0fbdf4c684ca602cb083f8da08c7d6a
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977982"
---
# <a name="binding-objective-c-libraries"></a>Binden von Objective-C-Bibliotheken

Bei der Arbeit mit Xamarin.iOS oder Xamarin.Mac können Fällen auftreten, in dem Sie ein Objective-C-Bibliotheken von Drittanbietern nutzen möchten. In diesen Fällen erhalten Sie können Xamarin Bindungsprojekte erstellen eine C# Binden an die nativen Objective-C-Bibliotheken. Das Projekt verwendet die gleichen Tools, mit denen wir schalten Sie die IOS- und Mac-APIs, um C#.

In diesem Dokument wird beschrieben, wie zum Binden von Objective-C-APIs, wenn Sie nur die C-APIs binden, sollten Sie den Standardmechanismus für .NET verwenden, hierzu [der P/Invoke-Framework](https://www.mono-project.com/docs/advanced/pinvoke/).
Informationen dazu, wie eine C-Bibliothek statisch verknüpft sind verfügbar, auf die [Native Bibliotheken verknüpfen](~/ios/platform/native-interop.md) Seite.

Finden Sie in unserem Companion [Typen-Referenzhandbuch Bindung](~/cross-platform/macios/binding/binding-types-reference.md).
Darüber hinaus überprüfen Sie, wenn Sie möchten erfahren mehr darüber, was hinter den Kulissen geschieht, unsere [Übersicht über die Datenbindung](~/cross-platform/macios/binding/overview.md) Seite.

Bindungen können für iOS und Mac-Bibliotheken erstellt werden.
Diese Seite beschreibt die Verwendung auf einem iOS binden, jedoch für die Mac-Bindungen sehr ähnlich sind.

**Beispielcode für iOS**

Können Sie die [iOS Binding Sample](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) Projekt zum Experimentieren mit Bindungen.

<a name="Getting_Started" />

## <a name="getting-started"></a>Erste Schritte

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die einfachste Möglichkeit zum Erstellen einer Bindung ist einer Xamarin.iOS-Bindung-Projekt zu erstellen.
Sie können dazu in Visual Studio für Mac auswählen des Projekttyps **iOS > Bibliothek > Bindungsbibliothek**:

[![](objective-c-libraries-images/00-sml.png "Dazu in Visual Studio für Mac den Projekttyp, die iOS-Bindungsbibliothek-Bibliothek")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die einfachste Möglichkeit zum Erstellen einer Bindung ist einer Xamarin.iOS-Bindung-Projekt zu erstellen.
Sie können dazu Visual Studio unter Windows durch Auswählen des Projekttyps **Visual C# > iOS > Bindungsbibliothek (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS-Bindungsbibliothek iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Hinweis: Bindung von Projekten für **Xamarin.Mac** werden nur in Visual Studio für Mac unterstützt

-----

Das generierte Projekt eine kleine Vorlage, die Sie bearbeiten können, enthält zwei Dateien: `ApiDefinition.cs` und `StructsAndEnums.cs`.

Die `ApiDefinition.cs` ist, in dem Sie die API-Vertrag definieren, dies ist die Datei, die beschreibt, wie die zugrunde liegende Objective-C-API in projiziert wird C#. Die Syntax und den Inhalt dieser Datei sind Hauptthema Diskussion dieses Dokuments und den Inhalt des Zertifikats auf C# Schnittstellen und C# Deklarationen zu delegieren. Die `StructsAndEnums.cs` ist die Datei, Sie werden keiner der Definitionen, die erforderlich sind, in denen eingeben, indem die Schnittstellen und Delegaten. Dies schließt Enumerationswerte und -Strukturen, die Ihren Code verwenden können.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Binden eine API

Um eine umfassende Bindung zu tun, sollten Sie verstehen die Objective-C-API-Definition, und machen Sie sich mit den .NET Framework-Entwurfsrichtlinien.

Zum Binden Ihrer Bibliothek in der Regel beginnen Sie mit einer API-Definitionsdatei. Eine API-Definitionsdatei ist lediglich ein C# Quelldatei mit C# Schnittstellen, die durch eine Reihe von Attributen mit Anmerkungen versehen wurden haben, mit deren Hilfe die Bindung steuern.  Diese Datei ist, was definiert, welche den Vertrag zwischen C# und Objective-C unterscheidet.

Beispielsweise ist dies eine einfache API-Datei für eine Bibliothek:

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

Das obige Beispiel definiert eine Klasse namens `Cocos2D.Camera` abgeleitet, die die `NSObject` Basistyp (dieses Typs stammt `Foundation.NSObject`) und die eine statische Eigenschaft definiert (`ZEye`), zwei Methoden, die keine Argumente und eine Methode, die akzeptiert drei Argumente.

Eine ausführliche Erläuterung des Formats der API-Datei und die Attribute, die Sie verwenden können, finden Sie in der [-API-Definitionsdatei](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) Abschnitt weiter unten.

Um eine vollständige Bindung zu erstellen, werden Sie in der Regel vier Komponenten bearbeiten:

-  Die API-Definitionsdatei (`ApiDefinition.cs` in der Vorlage).
-  Optional: Alle Enumerationen, Typen, Strukturen, die erforderlich sind, indem Sie die API-Definitionsdatei (`StructsAndEnums.cs` in der Vorlage).
-  Optional: zusätzliche Datenquellen, die möglicherweise erweitern die generierte Bindung, oder geben Sie eine C# benutzerfreundliche API (alle C# Dateien, die Sie dem Projekt hinzufügen).
-  Die native Bibliothek, die Sie binden.

Dieses Diagramm zeigt die Beziehung zwischen den Dateien:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Dieses Diagramm zeigt die Beziehung zwischen den Dateien")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Die Datei-API-Definition enthält nur Definitionen für Namespaces und -Schnittstelle (mit der alle Elemente, die eine Schnittstelle enthalten kann), und Sie sollten keine Klassen, Enumerationen, Delegaten oder Strukturen. Die API-Definitionsdatei ist lediglich den Vertrag, der zum Generieren der API verwendet werden.

Zusätzlicher Code, müssen Sie die Enumerationen wie oder Klassen zur Unterstützung für eine separate Datei im Beispiel oben die "CameraMode" gehostet werden soll, ist ein Enumerationswert, der in der CS-Datei nicht vorhanden und sollte in einer separaten Datei gehostet werden, z. B. `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

Die `APIDefinition.cs` Datei wird mit kombiniert die `StructsAndEnum` Klasse und werden verwendet, um die Core-Bindung der Bibliothek zu generieren. Können Sie die erstellte Bibliothek als-ist, aber in der Regel möchten Sie die erstellte Bibliothek, um einige hinzuzufügen optimieren C# Funktionen zur Begutachtung durch Ihre Benutzer. Beispiele hierfür sind implementieren eine `ToString()` -Methode, geben Sie C# Indexer, Hinzufügen von impliziten Konvertierungen zu und von manchen systemeigenen Typen oder stark typisierte Versionen einiger Methoden bereitstellen. Diese Verbesserungen werden in zusätzlichen gespeichert C# Dateien. Fügen Sie lediglich die C# Dateien zum Projekt und sie werden in diesem Buildprozess enthalten sein.

Dies zeigt, wie Sie den Code in implementieren würde Ihre `Extra.cs` Datei. Beachten Sie, dass Sie partielle Klassen verwenden, da diese die partiellen Klassen erweitern, die aus der Kombination von generiert werden die `ApiDefinition.cs` und `StructsAndEnums.cs` core Bindung:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Erstellen der Bibliothek wird die native Bindung erstellt werden.

Um diese Bindung abzuschließen, sollten Sie die native Bibliothek zum Projekt hinzufügen.  Hinzufügen der nativen Bibliothek zu Ihrem Projekt, indem Sie Drag & Drop die native Bibliotheken aus dem Finder auf das Projekt im Projektmappen-Explorer oder durch Rechtsklick auf das Projekt, und wählen Sie hierzu **hinzufügen**  >  **Hinzufügen von Dateien** die native Bibliothek auswählen.
Native Bibliotheken gemäß der Konvention beginnen Sie mit dem Wort "Lib" und enden mit der Erweiterung "a". Wenn Sie dies tun, fügt Visual Studio für Mac zwei Dateien: die a-Datei und einer automatisch angegebenen C# -Datei mit Informationen, was die native Bibliothek enthält:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Native Bibliotheken gemäß der Konvention beginnen Sie mit der Word-Lib und enden mit der Erweiterung a")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Den Inhalt der `libMagicChord.linkwith.cs` -Datei enthält Informationen dazu, wie diese Bibliothek verwendet werden kann, und weist diese Binärdatei in die resultierende DLL-Datei zu packen Ihrer IDE:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Vollständige Informationen zum Verwenden der [`[LinkWith]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 
Attribut in dokumentiert sind die [Typen-Referenzhandbuch Bindung](~/cross-platform/macios/binding/binding-types-reference.md).

Nun, wenn Sie das Projekt erstellen Sie mit letztendlich eine `MagicChords.dll` Datei, die sowohl die Bindung und die native Bibliothek enthält. Sie können dieses Projekt verteilen, oder verwenden Sie die resultierende DLL an andere Entwickler für ihre eigenen.

In einigen Fällen möglicherweise, dass Sie einige Enumerationswerte, delegatdefinitionen oder andere Typen benötigen. Platzieren Sie nicht die in der Datei-API-Definitionen, da es sich lediglich um einen Vertrag handelt

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Die API-Definitionsdatei

Die API-Definitionsdatei besteht aus einer Reihe von Schnittstellen. Die Schnittstellen in der API-Definition werden in einer Klassendeklaration umgewandelt werden, und sie müssen mit ergänzt werden, die [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Attribut, um die Basisklasse für die Klasse anzugeben.

Sie Fragen sich vielleicht warum wir nicht Klassen anstelle Schnittstellen für die Vertragsdefinition verwendet haben. Nahmen wir Schnittstellen, da es ermöglichte, schreiben den Vertrag für eine Methode ohne Methodenkörper in der API-Definitionsdatei bereitstellen müssen, oder um einen Text anzugeben, der musste eine Ausnahme auslösen oder keinen sinnvollen Wert zurück.

Aber da wir die Schnittstelle als ein Grundgerüst zum Generieren einer Klasse verwenden auf ergänzen die verschiedenen Teile des Vertrags mit Attributen, die die Bindung steuern zurückgegriffen werden musste.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Remotedienstbindungen

Die einfachste Bindung, die Sie ausführen können, ist eine Methode binden. Deklarieren Sie eine Methode in der Schnittstelle mit der C# Benennungskonventionen und ergänzen Sie die Methode mit dem [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
-Attribut. Die [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut ist, welche links Ihrer C# Name mit dem Namen "Objective-C" in der Xamarin.iOS-Runtime. Der Parameter, der die [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
Attribut ist der Name des Selektors Objective-C. Einige Beispiele:

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

Die obigen Beispiele zeigen, wie Instanzmethoden gebunden werden kann. Um statische Methoden zu binden, müssen Sie verwenden die [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) Attribut wie folgt:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Dies ist erforderlich, da der Vertrag Teil einer Schnittstelle ist und Schnittstellen keine Deklarationen für statische und-Instanz, kennen damit Sie erneut auf Attribute zurückgegriffen werden muss. Wenn Sie eine bestimmte Methode von der Bindung ausblenden möchten, können Sie ergänzen, um die Methode mit dem [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut.

Die `btouch-native` Befehl entsteht sucht Verweisparameter, der nicht null sein. Wenn Sie für einen bestimmten Parameter null-Werte zulassen möchten, verwenden Sie die [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
Attribut für den Parameter, wie folgt:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Beim Exportieren eines Verweistyps, mit der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Schlüsselwort können Sie auch die Zuordnung Semantik angeben. Dies ist erforderlich, um sicherzustellen, dass keine Daten verloren geht.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Bindungseigenschaften

Genau wie die Methoden, sind Objective-C-Eigenschaften gebunden, mit der [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
Attribut, und ordnen Sie direkt zu C# Eigenschaften. Genau wie die Methoden, Eigenschaften können mit ergänzt werden, die [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)
Und die [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
Attribute.

Bei Verwendung der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribut für eine Eigenschaft in den Hintergrund Btouch Native bindet tatsächlich zwei Methoden: die Getter und Setter. Der Name, den Sie angeben, werden zum Exportieren der **Basename** und der Setter wird berechnet, indem Sie das Wort "Set", aktivieren den ersten Buchstaben des voranstellen der **Basename** in Großbuchstaben und machen die Auswahl, nehmen eine Argument. Dies bedeutet, dass `[Export ("label")]` angewendet, die auf eine Eigenschaft bindet tatsächlich "Label" und "SetLabel:" Objective-C-Methoden.

Manchmal wird die Objective-C-Eigenschaften nicht das oben beschriebene Muster folgen, und der Name ist manuell überschrieben. In diesen Fällen können Sie steuern, die Möglichkeit, dass die Bindung, mithilfe generiert wird der [`[Bind]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) 
Attribut für die Getter oder Setter, z.B.:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Dadurch sind Sie bindet "IsMenuVisible" und "SetMenuVisible:". Optional kann eine Eigenschaft gebunden werden, das mithilfe der folgenden Syntax:

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

In denen die Getter und Setter werden explizit definiert wie in der `name` und `setName` Bindungen, die oben genannten.

Neben der Unterstützung für statische Eigenschaften, die mit [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), Sie können Eigenschaften threadstatischen ergänzen [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), z.B.:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Genau wie die Methoden mit gekennzeichnet werden einige Parameter ermöglichen [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), Sie können anwenden [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
Null ist einen gültigen Wert für die Eigenschaft auf eine Eigenschaft an, dass z. B. aus:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

Die [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Parameter kann auch direkt auf den Setter angegeben werden:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Einschränkungen der Bindung von benutzerdefinierter Steuerelementen

Die folgenden Einschränkungen sollten berücksichtigt werden, wenn Sie die Bindung für ein benutzerdefiniertes Steuerelement einrichten:

1. **Binden der Eigenschaften muss statisch sein** : Wenn die Bindung der Eigenschaften definieren die [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) Attribut muss verwendet werden.
 2. **Eigenschaftsnamen müssen exakt** -muss der Namen, binden Sie die Eigenschaft den Namen der Eigenschaft in das benutzerdefinierte Steuerelement genau entsprechen.
3. **Eigenschaftentypen müssen genau übereinstimmen** : der Variablentyp verwendet, um die Eigenschaft binden muss den Typ der Eigenschaft in das benutzerdefinierte Steuerelement genau entsprechen.
4. **Haltepunkte und dem Getter/Setter** : Haltepunkte platziert werden, in der Getter oder Setter-Methoden von der Eigenschaft werden nie erreicht werden.
5. **Beobachten Sie Rückrufe** – Sie müssen Beobachtung-Rückrufe verwenden, um die Änderungen in den Eigenschaftswerten von benutzerdefinierten Steuerelementen benachrichtigt zu werden.

Beachten Sie die oben aufgeführten Einschränkungen können in der Bindung, die im Hintergrund führt zu einem Laufzeitfehler führen.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Änderbare Objective-C-Muster und Eigenschaften

Objective-C-Frameworks verwenden eine Ausdrucksweise ermöglicht, in dem einige Klassen, die durch eine Unterklasse des änderbar unveränderlich sind. Z. B. `NSString` ist die unveränderliche Version während `NSMutableString` die Unterklasse, die Mutation ermöglicht wird.

In diesen Klassen ist es üblich, die unveränderliche-Basisklasse enthalten Eigenschaften mit Getter, aber kein Setter. Und für die änderbare Version den Setter einführen. Da dies nicht wirklich möglich, mit ist C#, mussten wir diese Sprache in einer Ausdrucksweise ermöglicht zuordnen, die mit funktioniert C#.

Die Möglichkeit, dies das zugeordnet ist C# wird sowohl der Getter und Setter in der Basisklasse hinzufügen, aber das Kennzeichnen des Setters mit einer [`[NotImplemented]`](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute)
-Attribut.

Klicken Sie dann auf die änderbare-Unterklasse, verwenden Sie die [`[Override]`](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 
Attribut für die Eigenschaft, um sicherzustellen, dass die Eigenschaft wirklich des übergeordneten Elements Verhalten außer Kraft gesetzt wird.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors" />

### <a name="binding-constructors"></a>Binden von Konstruktoren

Die `btouch-native` Tool generiert automatisch vieren Konstruktoren in der Klasse für eine bestimmte Klasse `Foo`, generiert:

-  `Foo ()`: der Standardkonstruktor (Maps für Objective-C "Init"-Konstruktor)
-  `Foo (NSCoder)`: der Konstruktor verwendet wird, während der Deserialisierung von NIB-Dateien (ordnet mit Objective-C "InitWithCoder:" Konstruktor).
-  `Foo (IntPtr handle)`: der Konstruktor für die Handle-basierte Erstellung, dies wird von der Laufzeit aufgerufen, wenn die Laufzeit benötigt werden, um ein verwaltetes Objekt aus einem nicht verwalteten Objekt verfügbar zu machen.
-  `Foo (NSEmptyFlag)`: Dies wird von abgeleiteten Klassen verwendet, um doppelte Initialisierung zu verhindern.

Für Konstruktoren, die Sie definieren, müssen sie mithilfe der folgenden Signatur innerhalb der Schnittstellendefinition deklariert werden: sie zurückgeben müssen ein `IntPtr` Wert und den Namen der Methode sollte Konstruktor sein. Beispiel zum Binden der `initWithFrame:` -Konstruktor, wird Sie verwendet werden:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Binden von Protokollen

In der API-Design-Dokument im Abschnitt beschriebenen [besprochen, Modelle und Protokolle](~/ios/internals/api-design/index.md#Models), Xamarin.iOS ordnet die Objective-C-Protokolle in Klassen, die mit eingestuft wurden den [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)
-Attribut. Dies wird normalerweise verwendet, bei der Implementierung von Klassen für Objective-C-Delegaten.

Der große Unterschied zwischen einer normalen gebundene Klasse und ein "Delegate"-Klasse ist, dass der "Delegate"-Klasse eine oder mehrere optionale Methoden.

Betrachten Sie beispielsweise die `UIKit` Klasse `UIAccelerometerDelegate`, dies ist, wie er in Xamarin.iOS gebunden ist:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Da dies eine optionale Methode auf der Definition ist `UIAccelerometerDelegate` besteht keine weiteren Maßnahmen erforderlich. Aber wenn eine erforderliche Methode für das Protokoll wurde, sollten Sie Hinzufügen der [`[Abstract]`](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute)
-Attribut auf die Methode. Dies zwingt den Benutzer der Implementierung der Methode tatsächlich Text bereit.

Im Allgemeinen werden die Protokolle in Klassen verwendet, die auf Nachrichten reagieren. Dies erfolgt normalerweise in Objective-C durch Zuweisen der "Delegate"-Eigenschaft einer Instanz eines Objekts, das auf die Methoden in das Protokoll reagiert.

Die Konvention in Xamarin.iOS ist die Unterstützung von sowohl des Objective-C-lose gekoppelten Stils einen Instanz einer `NSObject` kann zugewiesen werden an den Delegaten und auch machen eine stark typisierte Version davon. Aus diesem Grund wir in der Regel bieten sowohl einen `Delegate` -Eigenschaft, die stark typisiert ist und ein `WeakDelegate` , lose typisiert ist. Wir binden in der Regel die lose typisierten Version mit [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), und wir verwenden die [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) Attribut, die stark typisierte Version bereitzustellen.

Dies zeigt, wie wir gebunden der `UIAccelerometer` Klasse:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport" />

**Neues in MonoTouch 7.0**

Beginnend mit MonoTouch-7.0 einen neuen und verbesserten protokollbindung-Funktionalität wurde integriert.  Diese neue Unterstützung ist es einfacher, Objective-C-Ausdrücke für den Umstieg auf ein oder mehrere Protokolle in einer bestimmten Klasse zu verwenden.

Für jede Protokolldefinition `MyProtocol` in Objective-C, es gibt jetzt eine `IMyProtocol` -Schnittstelle, die die erforderlichen Methoden aus dem Protokoll auflistet sowie eine Erweiterungsklasse, die alle die optionalen Methoden bereitstellt.  Den oben genannten in Kombination mit neue Unterstützung in Xamarin Studio-Editor-Entwickler, Protokollmethoden zu implementieren, ohne separaten Unterklassen der vorhergehenden abstrakte Modellklassen verwenden kann.

Definition, enthält die [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) Attribut generiert tatsächlich drei unterstützende Klassen, die die Möglichkeit zu steigern, um Sie Protokolle verwenden:

```csharp
    // Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

Die **-klassenimplementierung** bietet eine vollständige abstrakte Klasse, dass Sie einzelne Methoden von außer Kraft setzen und vollständige typsicherheit.  Aber aufgrund von C# unterstützen keine mehrfache Vererbung, es gibt Szenarien, in denen Sie möglicherweise, eine andere Basisklasse verfügen muss, aber Sie möchten immer noch eine Schnittstelle zu implementieren, die ist, die

Die generierte **Schnittstellendefinition** ins Spiel.  Es ist eine Schnittstelle, die über die erforderlichen Methoden aus dem Protokoll verfügt.  Dadurch können Entwickler, die Ihr Protokoll, um lediglich die Schnittstelle implementieren implementieren möchten.  Die Laufzeit wird den Typ automatisch registriert, als die Einführung des Protokolls.

Beachten Sie, dass die Schnittstelle nur sind die erforderlichen Methoden aufgeführt, und macht die optionalen Methoden verfügbar.  Dies bedeutet, dass Klassen, die das Protokoll zu übernehmen vollständiger typüberprüfung für die erforderlichen Methoden erhalten, müssen aber zurückgreifen, um schwache Typisierung (manuell mit [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
Attribute und die Signatur) für die optionales Protokoll-Methoden.

Damit wird es praktisch sein, eine API nutzen, die Protokolle, erzeugt das Tool für die Bindung auch eine Extensions-Methode-Klasse, die alle die optionalen Methoden verfügbar macht.  Dies bedeutet, dass, solange Sie eine API verwenden, um Protokolle zu behandeln, als dass alle Methoden werden kann.

Wenn Sie die Definitionen des Protokolls in Ihrer API verwenden möchten, müssen Sie Skelette leere Schnittstellen in Ihrer API-Definition zu schreiben.  Wenn Sie die MyProtocol in eine API verwenden möchten, müssen Sie würde dazu:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Die oben genannten ist erforderlich, da an der Zeit zum Binden der `IMyProtocol` ist nicht vorhanden, also Warum müssen Sie eine leere Schnittstelle angeben.

#### <a name="adopting-protocol-generated-interfaces"></a>Einführung von Protokoll generierten Schnittstellen

Jedes Mal, wenn Sie eine der für die Protokolle, wie folgt generiert Schnittstellen implementieren:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Die Implementierung für die Schnittstellenmethoden ruft automatisch mit dem richtigen Namen, exportiert, damit er dies entspricht:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Es spielt keine Rolle, wenn die Schnittstelle implizit oder explizit implementiert wird.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Bindungserweiterungen-Klasse

In Objective-C-es ist möglich, Klassen zu erweitern, neue Methoden, ähnlich wie im Geiste, C#Erweiterungsmethoden. Wenn eine der folgenden Methoden vorhanden ist, können Sie die [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
Attribut, das die Methode als Empfänger der Nachricht Objective-C-flag.

In Xamarin.iOS gebunden wir z. B. die Erweiterungsmethoden, die in definierten `NSString` beim `UIKit` wird importiert als Methoden in der `NSStringDrawingExtensions`, wie folgt aus:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Binden von Objective-C-Argumentlisten

Objective-C unterstützt die Variadic-Argumente. Zum Beispiel:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Zum Aufrufen dieser Methode von C# möchten eine Signatur wie folgt erstellen:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Hiermit wird die Methode als intern, die oben genannte API vor Benutzern ausgeblendet, aber eine Offenlegung für die Bibliothek deklariert. Anschließend können Sie eine Methode wie folgt schreiben:

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields" />

### <a name="binding-fields"></a>Binden von Feldern

Manchmal möchten Sie die öffentlichen Felder zugreifen, die in einer Bibliothek deklariert wurden.

In der Regel enthalten diese Felder Zeichenketten oder Integer-Werte, die auf die verwiesen werden müssen. Sie werden häufig verwendet, als Zeichenfolge, die eine bestimmte Benachrichtigung darstellt und als Schlüssel in Wörterbüchern.

Um ein Feld zu binden, fügen Sie eine Eigenschaft, um Ihre Benutzeroberflächen-Definitionsdatei, und ergänzen Sie die Eigenschaft mit dem [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Attribut. Dieses Attribut nimmt einen Parameter: den C-Namen des Symbols für die Suche. Zum Beispiel:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Wenn Sie möchten verschiedene Felder in einer statischen Klasse zu umschließen, die nicht von abgeleitet ist `NSObject`, können Sie die [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 
Attribut für die Klasse, wie folgt:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Oben generiert eine `LonelyClass` die nicht von abgeleitet `NSObject` und enthält eine Bindung an die `NSSomeEventNotification` 
 `NSString` verfügbar gemacht werden, als ein `NSString`.

Die [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Attribut kann auf die folgenden Datentypen angewendet werden:

-  `NSString` Verweise (nur schreibgeschützte Eigenschaften)
-  `NSArray` Verweise (nur schreibgeschützte Eigenschaften)
-  32-Bit-Ganzzahlen (`System.Int32`)
-  64-Bit-Ganzzahlen (`System.Int64`)
-  32-Bit-Gleitkommazahlen (`System.Single`)
-  64-Bit-Gleitkommazahlen (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Neben dem Namen des systemeigenen Felds können Sie angeben, den Namen der Bibliothek, in dem das Feld befindet durch Übergeben des Namens der Bibliothek:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Wenn Sie statisch verknüpfen, ist keine Bibliothek zum Binden an, daher Sie müssen die `__Internal` Name:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Binden von Enumerationen

Sie können hinzufügen `enum` direkt in die Bindung zu Dateien erleichtert es, verwenden sie in API-Definitionen – ohne Verwendung einer anderen Quelldatei (die in den Bindungen und das letzte Projekt kompiliert werden muss).

Beispiel:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Es ist auch möglich, erstellen Sie eigene Enumerationen ersetzen `NSString` Konstanten. In diesem Fall wird von der Generator **automatisch** erstellen Sie die Methoden zum Konvertieren von Werten für Enumerationen und NSString-Konstanten für Sie.

Beispiel:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

Im obigen Beispiel könnten Sie zum Dekorieren `void Perform (NSString mode);` mit einer [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut. Dadurch wird **ausblenden** der Konstante-basierte API über Ihre Kunden für die Bindung.

Jedoch dadurch eingeschränkt würden, Erstellen von Unterklassen für den Typ als die nützlicher-API-alternative verwendet eine [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) Attribut. Diese generierten Methoden sind nicht `virtual`, d. h. Sie wird nicht in der Lage – Überschreiben der kann oder nicht, eine gute Wahl.

Eine Alternative besteht darin, markieren Sie das Original `NSString`-basiert, Definition als `[Protected]`. Dadurch können Unterklassen erstellt werden, arbeiten bei Bedarf, und weiterhin die wrap'ed Version arbeitet und rufen Sie die Methode außer Kraft gesetzt.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Binden von `NSValue`, `NSNumber`, und `NSString` zu einem besseren Typ

Die [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Attribut ermöglicht die Bindung `NSNumber`, `NSValue` und `NSString`(Enumerationen) in mehrere genaue C# Typen. Das Attribut kann verwendet werden, erstellen Sie eine bessere und genauere .NET API gegenüber einer systemeigenen API.

Sie können die Methoden (Rückgabewert), Parameter und Eigenschaften mit ergänzen [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Die einzige Einschränkung gehört, die Ihre **darf nicht** innerhalb einer [`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 
oder [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) Schnittstelle.

Zum Beispiel:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Ausgabe wäre:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Intern werden wir die `bool?`  <->  `NSNumber` und `CGRect`  <->  `NSValue` Konvertierungen.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Außerdem unterstützt Arrays mit `NSNumber` `NSValue` und `NSString`(Enumerationen).

Zum Beispiel:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Ausgabe wäre:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` ist eine `NSString` Enumeration gesichert werden wir das Recht abrufen `NSString` Wert ein, und behandelt die typkonvertierung.

Informieren Sie sich die [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Dokumentation auf unterstützte Konvertierung von Datentypen finden Sie unter.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Binden von Benachrichtigungen

Benachrichtigungen sind Nachrichten, die bereitgestellt werden, die `NSNotificationCenter.DefaultCenter` und dienen als Mechanismus zum Senden von Nachrichten von einem Teil der Anwendung in eine andere. Entwickler Abonnieren von Benachrichtigungen, die in der Regel mithilfe der [NSNotificationCenter](xref:Foundation.NSNotificationCenter)des [bei AddObserver](xref:Foundation.NSNotificationCenter.AddObserver(Foundation.NSString,System.Action{Foundation.NSNotification})) Methode. Wenn eine Anwendung eine Nachricht in der mitteilungszentrale veröffentlicht, in der Regel enthält eine Nutzlast, die in gespeicherten der [NSNotification.UserInfo](xref:Foundation.NSNotification.UserInfo) Wörterbuch. Dieses Wörterbuch ist schwach typisiert und Abrufen von Informationen daraus ist fehleranfällig, wie in der Regel müssen Benutzer in der Dokumentation zu lesen, welche Schlüssel finden Sie auf das Wörterbuch und die Typen der Werte, die im Wörterbuch gespeichert werden können. Das Vorhandensein der manchmal auch als Schlüssel dient als auch einen booleschen Wert.

Der Xamarin.iOS-Bindung-Generator bietet Support für Entwickler, um Benachrichtigungen zu binden. Zu diesem Zweck legen Sie die [`[Notification]`](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute)
Attribut für eine Eigenschaft, die auch wurde mit markiert ein [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)
-Eigenschaft (es kann öffentlich oder privat sein).

Dieses Attribut kann verwendet werden, ohne Argumente für Benachrichtigungen, die keine Nutzlast enthalten, oder Sie können angeben, ein `System.Type` , die auf einer anderen Schnittstelle in der API-Definition in der Regel verweist, mit dem Namen "EventArgs" enden. Der Generator wandeln der Schnittstelle in einer Klasse, Unterklassen `EventArgs` und enthält alle Eigenschaften aufgeführt. Die [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribut sollte in der EventArgs-Klasse verwendet werden, den Namen des Schlüssels verwendet, um die Objective-C-Wörterbuch, das Abrufen des Werts zu suchen.

Zum Beispiel:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Im obigen Code wird eine geschachtelte Klasse generiert `MyClass.Notifications` mit den folgenden Methoden:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Benutzer Ihres Codes können dann problemlos Abonnieren von Benachrichtigungen, die bereitgestellt werden, um die [NSDefaultCenter](xref:Foundation.NSNotificationCenter.DefaultCenter) mithilfe von Code wie folgt:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Der zurückgegebene Wert von `ObserveDidStart` können verwendet werden, um ganz einfach keine Benachrichtigungen mehr empfangen, wie folgt:

```csharp
token.Dispose ();
```

Oder Sie rufen [NSNotification.DefaultCenter.RemoveObserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) und übergeben Sie das Token. Wenn die Benachrichtigung an den Parameter enthält, sollten Sie angeben, dass eine Hilfsprogramm `EventArgs` Schnittstelle, die wie folgt:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Der oben generiert eine `MyScreenChangedEventArgs` -Klasse mit der `ScreenX` und `ScreenY` Eigenschaften, mit denen die Daten abgerufen werden die [NSNotification.UserInfo](xref:Foundation.NSNotification.UserInfo) Wörterbuch mit den Schlüsseln "ScreenXKey" und "ScreenYKey" bzw. und wenden Sie die ordnungsgemäße Konvertierungen. Die `[ProbePresence]` -Attribut für den Generator verwendet, um den Prüfpunkt, wenn der Schlüssel, in festgelegt ist der `UserInfo`, anstatt zu versuchen, den Wert zu extrahieren. Dies ist für Fälle verwendet, in denen das Vorhandensein des Schlüssels den Wert (in der Regel für boolesche Werte) ist.

Dadurch können Sie Code wie diesen schreiben:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Binden von Kategorien

Kategorien sind ein Objective-C-Mechanismus verwendet, um den Satz von Methoden und Eigenschaften zur Verfügung stehen, in einer Klasse zu erweitern.   In der Praxis werden sie auf die Funktionalität einer Basisklasse entweder das deduplizierungsverarbeitungsfenster verwendet (z. B. `NSObject`) Wenn ein bestimmtes Framework in verknüpft ist (z. B. `UIKit`), sodass ihre Methoden verfügbar, aber nur, wenn das neue Framework verknüpft ist.   In einigen Fällen werden sie zum Organisieren von Funktionen in einer Klasse von verwendet Funktionen.   Sie ähneln im Geiste, C# Erweiterungsmethoden. Dies ist eine Kategorie in Objective-c: aussehen sollen

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Im obigen Beispiel wenn finden Sie auf eine Bibliothek erweitert Instanzen `UIView` mit der Methode `makeBackgroundRed`.

Um diese zu binden, können Sie die [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) Attribut in einer Schnittstellendefinition.  Bei Verwendung der [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
Attribut, die Bedeutung des das [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
Attribut ändert, die an der Basisklasse zu erweitern, ist der zu erweiternden Typs verwendet werden.

Im folgenden dargestellt wie die `UIView` Erweiterungen gebunden sind und die Umwandlung in C# Erweiterungsmethoden:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Die oben genannten erstellt eine `MyUIViewExtension` einer Klasse enthält die `MakeBackgroundRed` Erweiterungsmethode.  Dies bedeutet, dass Sie jetzt "MakeBackgroundRed" aufrufen, können auf einem `UIView` -Unterklasse, sodass Sie die gleiche Funktionalität Sie in Objective-c erhalten In einigen Fällen werden die Kategorien keine Systemklasse zu erweitern, sondern zum Organisieren von Funktionen, die ausschließlich für Zwecke der Dekoration verwendet.  Und zwar so:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Sie können zwar die [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
Attribut für diese Art der Dekoration Deklarationen, Sie können ebenso gut hinzufügen oder alle der Klassendefinition.  Beides würde dasselbe erreichen:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

Es ist nur in diesen Fällen die Kategorien Zusammenführen kürzer:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks" />

### <a name="binding-blocks"></a>Binden von Blöcken

Blöcke sind ein neues Konstrukt eingeführt, die von Apple, die funktionale Entsprechung von anzuzeigen C# anonyme Methoden, Objective-c Z. B. die `NSSet` Klasse macht diese Methode jetzt verfügbar:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Die obige Beschreibung deklariert eine Methode namens `enumerateObjectsUsingBlock:` , akzeptiert ein Argument mit dem Namen `block`. Dieser Block ist vergleichbar mit einem C# anonyme Methode in, dass sie die Unterstützung für das Erfassen der aktuellen Umgebung ("die this"-Zeigers, den Zugriff auf lokale Variablen und Parameter) hat. Die oben dargestellte Methode im `NSSet` Ruft den Block mit zwei Parametern ein `NSObject` (die `id obj` Teil) und einen Zeiger auf einen booleschen Wert (der `BOOL *stop`) Teil.

Um diese Art der API mit Btouch zu binden, müssen Sie zuerst deklarieren, die Block-Typ-Signatur wie eine C# delegieren und dann von einem API-Einstiegspunkt, wie folgt darauf verweisen:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Und jetzt kann Ihr Code aufrufen Ihrer Funktion aus C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Sie können auch Lambda-Ausdrücke verwenden, falls gewünscht:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Asynchrone Methoden

Der Generator für die Bindung kann eine bestimmte Klasse von Methoden in Async-Methoden umwandeln (Methoden, die ein Task oder Task zurückgeben&lt;T&gt;).

Sie können die [`[Async]`](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) 
ein Attribut auf Methoden, die "void" zurückgeben und deren letzten Argument ist ein Rückruf.  Wenn Sie diese an eine Methode angewendet wird, generiert der Bindung-Generator eine Version dieser Methode mit dem Suffix `Async`.  Wenn der Rückruf keine Parameter akzeptiert, wird der Rückgabewert eine `Task`, wenn der Rückruf einen Parameter akzeptiert, wird das Ergebnis einer `Task<T>`.  Wenn der Rückruf mehrere Parameter akzeptiert, legen Sie die `ResultType` oder `ResultTypeName` den gewünschten Namen des generierten Typs angeben, die alle Eigenschaften enthalten wird.

Beispiel:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Der obige Code generiert sowohl die LoadFile-Methode, als auch:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Anzeigen von leistungsstarker Typen für schwachen NSDictionary-Parameter

An vielen Stellen in der Objective-C-API, werden Parameter übergeben, wie schwach typisierte `NSDictionary` APIs mit bestimmten Schlüsseln und Werten, aber diese sind fehleranfällig (ungültige Schlüssel übergeben werden können, und erhalten Sie keine Warnungen; ungültige Werte übergeben werden können, und erhalten Sie keine Warnungen) und frustrierendes Verwenden Sie, wie sie mehrere Roundtrips in der Dokumentation zum Suchen der möglichen Schlüsselnamen und Werte erfordern.

Die Lösung ist eine stark typisierte Version bereitstellen, die bereitstellt, dass die stark typisierte Version der API und im Hintergrund die zugrunde liegenden verschiedene Schlüssel und Werte zugeordnet.

Also z. B., wenn die Objective-C-API akzeptiert eine `NSDictionary` und es wird dokumentiert, übernehmen Sie den Schlüssel `XyzVolumeKey` nimmt eine `NSNumber` mit einem Volume-Wert von 0,0 1,0 und `XyzCaptionKey` , die eine Zeichenfolge akzeptiert, Sie sollten die Benutzer Sie verwenden eine gute API Das sieht folgendermaßen aus:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

Die `Volume` Eigenschaft wird als NULL-Werte zulassen "float", definiert, wie die Konvention in Objective-C nicht, dass diese Wörterbücher erfordert, um den Wert zu erhalten, daher gibt es Szenarien, in denen der Wert nicht festgelegt werden kann.

Zu diesem Zweck müssen Sie einige Punkte beachten:

* Erstellen Sie eine stark typisierte Klasse diese Unterklassen [DictionaryContainer](xref:Foundation.DictionaryContainer) sowie die verschiedenen Getter und Setter für jede Eigenschaft.
* Deklarieren von Überladungen für die Methoden, denen `NSDictionary` wird die neue Version mit starker typbindung.

Sie können die stark typisierte Klasse entweder manuell erstellen oder verwenden Sie den Generator, um die Arbeit für Sie.  Wir untersuchen zunächst, wie Sie dies manuell durchführen, damit Sie verstehen, was passiert, und klicken Sie dann automatisch.

Müssen Sie dafür eine unterstützende Datei zu erstellen, es geht nicht in der API-Vertrag.  Dies ist, was Sie schreiben müsste, um Ihre XyzOptions-Klasse zu erstellen:

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Klicken Sie dann sollten Sie eine Wrappermethode bereitstellen, die die High-Level-API, auf der Low-Level-API gibt.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Wenn Ihre API nicht überschrieben werden muss, können Sie problemlos die NSDictionary-basierte API ausblenden, mit der [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
-Attribut.

Wie Sie sehen können, verwenden wir die [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)
Attribut, um einen neuen Einstiegspunkt für die API-Oberfläche, und wir Oberfläche mit unseren stark typisierte `XyzOptions` Klasse.  Der Wrappermethode können auch auf Null übergeben werden.

Wo ist eine Sache, die nicht erwähnt die `XyzOptionsKeys` Werte stammt.  Sie würden in der Regel die Schlüssel, die eine API in einer statischen Klasse wie Flächen gruppieren `XyzOptionsKeys`, wie folgt aus:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Sehen wir uns auf die automatische Unterstützung zum Erstellen dieser stark typisierte Wörterbücher.  Dadurch wird vermieden, die häufig viele, und Sie können das Wörterbuch definieren, direkt in Ihrem API-Vertrag, anstatt zu eine externe Datei verwenden.

Um ein Wörterbuch mit fester typbindung zu erstellen, führen Sie eine Schnittstelle in Ihrer API auf, und versehen Sie mit der [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Attribut.  Dies weist dem Generator an, dass es mit dem gleichen Namen wie Ihre Schnittstelle eine Klasse erstellen soll, die abgeleitet werden `DictionaryContainer` und starke typisierte Accessor dafür bietet.

Die [ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Attribut nimmt einen Parameter, dies ist der Name der statischen Klasse, die die Wörterbuchschlüssel enthält.  Anschließend wird jede Eigenschaft der Schnittstelle einer stark typisierten Accessor sein.  Standardmäßig wird der Code die Namen der Eigenschaft mit dem Suffix "Key" in der statischen Klasse verwenden, zum Erstellen des Accessors.

Dies bedeutet, dass eine externe Datei noch Getter und Setter für jede Eigenschaft manuell erstellen müssen, noch müssen die Schlüssel manuell zu suchen. erstellen Ihre stark typisierten Accessor nicht mehr benötigt werden selbst.

Dies ist die gesamte Bindung würde folgendermaßen aussehen:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Für den Fall, dass Sie in verweisen müssen Ihre `XyzOption` Elemente ein anderes Feld (d. h. nicht der Name der Eigenschaft mit dem Suffix `Key`), versehen Sie die Eigenschaft mit dem ein [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
ein Attribut mit dem Namen, den Sie verwenden möchten.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Datentypzuordnungen

Dieser Abschnitt wird beschrieben, wie Objective-C-Typen zugeordnet werden C# Typen.

<a name="Simple_Types" />

### <a name="simple-types"></a>Einfache Typen

Die folgende Tabelle zeigt, wie die Typen aus der Objective-C und das CocoaTouch-Welt auf die Xamarin.iOS-Welt zugeordnet werden sollen:

|Objective-C-Typname|Xamarin.iOS-Unified API-Typ|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([auf NSString Bindung weitere](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (Siehe auch: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|CoreFoundation-Typen (`CF*`)|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation-Typen (`NS*`)|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays" />

### <a name="arrays"></a>Arrays

Die Xamarin.iOS-Runtime übernimmt automatisch konvertiert C# auf arrays `NSArrays` und Durchführen der Konvertierung zurück, z. B. die imaginäre Objective-C-Methode, die zurückgegeben wird ein `NSArray` von `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Ist wie folgt gebunden:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

Die Idee ist, verwenden Sie einen stark typisierten C# array als Dadurch wird die IDE um richtige codevervollständigung, mit dem tatsächlichen Typ bereitzustellen, ohne dass des Benutzers zu erraten, oder suchen Sie die Dokumentation der tatsächliche Typ der Objekte im Array enthaltenen.

In Fällen, in dem Sie nach unten der tatsächliche am stärksten abgeleitete Typ, der im Array enthaltenen nicht verfolgen können, können Sie `NSObject []` als Rückgabewert.

<a name="Selectors" />

### <a name="selectors"></a>Selektoren

Selektoren angezeigt, in der Objective-C-API als besonderen Typ `SEL`. Wenn Sie eine Auswahl zu binden, würden Sie den Typ zuordnen `ObjCRuntime.Selector`.  In der Regel werden in einer API mit sowohl ein Objekt, das Zielobjekt und eine Auswahl in das Zielobjekt aufrufen Selektoren verfügbar gemacht. Beide im Grunde Bereitstellung entspricht die C# delegieren: etwas, das sowohl die aufzurufende Methode als auch das Objekt, das Aufrufen der Methode im kapselt.

Dies ist wie die Bindung aussieht:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

Und dies ist wie die Methode in der Regel in einer Anwendung verwendet werden sollen:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

Um die Bindung nützlicher zu machen C# Entwickler Sie in der Regel werden eine Methode bereitstellen, die akzeptiert eine `NSAction` Parameter, der ermöglicht C# Delegaten und Lambdas, anstelle von verwendet werden soll die `Target+Selector`. Dazu Sie, in der Regel ausblenden würden die `SetTarget` Methode durch eine zeilenkennzeichnung mit einer [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
Attribut aus, und klicken Sie dann Sie macht eine neue Hilfsmethode, die wie folgt:

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Benutzercode kann nun wie folgt geschrieben werden:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings" />

### <a name="strings"></a>Zeichenfolgen

Beim Binden Sie einer Methode, verwendet eine `NSString`, können Sie diese ersetzen, die mit einer C# string-Typ, Rückgabetypen und Parameter.

Der einzige Fall, wenn Sie verwenden möchten, können ein `NSString` direkt ist, wenn die Zeichenfolge als Token verwendet wird. Weitere Informationen zu Verbindungszeichenfolgen und `NSString`erhalten Sie unter den [API-Design auf NSString](~/ios/internals/api-design/nsstring.md) Dokument.

In einigen seltenen Fällen kann eine API verfügbar eine C-ähnlichen Zeichenfolge machen (`char *`) anstelle einer Objective-C-Zeichenfolge (`NSString *`). In diesen Fällen können Sie den Parameter mit versehen der [`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring)
-Attribut.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>Out / Ref-Parameter

Einige APIs Rückgabewerte in ihren Parametern oder Parameter als Verweis übergeben.

In der Regel sieht die Signatur folgendermaßen aus:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

Das erste Beispiel zeigt eine allgemeine Objective-C-Vorgehensweise zum Zurückgeben von Fehlercodes, die einen Zeiger auf ein `NSError` Zeiger übergeben wird, und bei der Rückgabe der Wert festgelegt ist.   Die zweite Methode zeigt, wie eine Objective-C-Methode kann ein Objekt und dessen Inhalt zu ändern.   Dies wäre ein Durchlauf durch Verweis, anstatt einen reinen Ausgabewert.

Die Bindung würde wie folgt aussehen:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Speicher-Management-Attribute

Bei Verwendung der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut, und Sie übergeben Daten, die von der aufgerufenen Methode beibehalten werden, können Sie die Argument-Semantik angeben, indem sie z. B. als zweiten Parameter übergeben:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Die oben genannten, die den Wert mit der Semantik "Beibehalten" kennzeichnen würde. Die Semantik, die verfügbar sind:

-  Zuweisen
-  Kopieren
-  Beibehalten

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Die Richtlinien des Styleguides

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>Mithilfe von [Internal]

Sie können die [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
Attribut, das eine Methode aus der öffentlichen API ausblenden. Sie möchten dazu in Fällen, in denen die verfügbar gemachte API ist auch auf niedriger Ebene, und Sie eine allgemeine Implementierung in einer separaten Datei auf der Grundlage dieser Methode bereitstellen möchten.

Sie können dies auch verwenden, wenn Einschränkungen in der Bindung Generator auftreten, z. B. einige erweiterten Szenarien möglicherweise machen Typen verfügbar, die nicht gebunden werden und auf eigene Weise die Bindung erfolgen soll, und diese Typen selbst auf eigene Weise umschlossen werden sollen.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Ereignishandlern und Rückrufen

Objective-C-Klassen in der Regel senden Sie Mitteilungen sofort oder Anfordern von Informationen durch Senden einer Nachricht an eine Delegatklasse (Objective-C-Delegat).

Dieses Modell kann zwar vollständig unterstützt und von Xamarin.iOS eingeblendet manchmal umständlich sein. Xamarin.iOS macht das C# Ereignismuster und ein System Methode des Rückrufs für die Klasse, die in diesen Situationen verwendet werden kann. Dies ermöglicht Code wie den folgenden ausführen:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Der Generator für die Bindung kann weniger Tipparbeit erforderlich, um die Objective-C-Muster in zuzuordnen ist die C# Muster.

Ab Xamarin.iOS 1.4 es immer möglich, auch anweisen, den Generator zum Erstellen von Bindungen für eine bestimmte Objective-C-Delegaten und verfügbar zu machen den Delegaten als C# Ereignisse und Eigenschaften für den Hosttyp.

Es gibt zwei Klassen am Prozess Beteiligten, die Hostklasse die ist diejenige, die derzeit gibt Ereignisse und sendet diese in die `Delegate` oder `WeakDelegate` und die tatsächliche Delegate-Klasse.

Erwägen die folgende Konfiguration:

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

Um für die Klasse zu umschließen, müssen Sie folgende Aktionen ausführen:

-  Fügen Sie in Ihrer Hostklasse zu Ihrem [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   Deklaration der Typ, der als der Delegat fungiert und die C# Namen, den Sie bereitgestellt haben. Im obigen Beispiel sind `typeof (MyClassDelegate)` und `WeakDelegate` bzw.
-  In der Delegatklasse auf jede Methode, die mehr als zwei Parameter besitzt, müssen Sie den Typ angeben, den Sie für die automatisch generierte EventArgs-Klasse verwenden möchten.

Der Generator für die Bindung ist nicht auf nur ein einzelnes Ereignis Ziel wrapping beschränkt, es ist möglich, dass einige Klassen Objective-C zum Ausgeben von Nachrichten mit mehr als einem zu delegieren, daher müssen Sie Arrays zu, um dieses Setup zu unterstützen, bieten. Die meisten Setups ist dies nicht erforderlich, aber der Generator kann diese Fälle zu unterstützen.

Der resultierende Code werden:

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

Die `EventArgs` wird verwendet, um den Namen der angeben der `EventArgs` zu generierenden Klasse. Sie sollten eine pro Signatur verwenden (in diesem Beispiel die `EventArgs` enthält eine `With` Eigenschaft des Typs Nint).

Mit den oben genannten Definitionen erzeugt der Generator das folgende Ereignis in der generierten MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Daher können Sie nun den Code folgendermaßen:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Rückrufe werden genau wie Ereignis aufrufen, die der Unterschied besteht darin, statt mehrere potenzielle Abonnenten (z. B. können mehrere Methoden in Verknüpfen einer `Clicked` Ereignis oder eine `DownloadFinished` Ereignis) Rückrufe können nur einen einzelnen Abonnenten haben.

Der Prozess ist identisch, der einzige Unterschied ist, statt den Namen des verfügbar zu machen die `EventArgs` -Klasse, die generiert wird, der EventArgs tatsächlich wird verwendet, um die resultierende Namen C# Delegatname.

Wenn die Methode in der "Delegate"-Klasse einen Wert zurückgibt, wird der Generator Bindung dies eine stellvertretermethode in der übergeordneten Klasse anstelle eines Ereignisses zuordnen. In diesen Fällen müssen Sie den Standardwert angeben, der von der-Methode zurückgegeben werden soll, wenn der Benutzer nicht einbinden wird an dem Delegaten. Erreichen Sie dies mit der [`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)
oder [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) Attribute.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) wird hartcodieren einen Wert zurückgegeben, während [`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)
wird verwendet, um anzugeben, welche Eingabeargument zurückgegeben werden.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Enumerationen und Basistypen

Sie können auch verweisen, Enumerationen oder die Basistypen, die vom System Btouch Interface Definition nicht direkt unterstützt werden. Zu diesem Zweck platzieren Sie Ihre Enumerationen und die grundlegenden Typen in einer separaten Datei, und fügen Sie diese als Teil einer zusätzlichen Dateien, die Sie Btouch bereitstellen.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Verknüpfen von Abhängigkeiten

Wenn Sie APIs, die nicht Teil Ihrer Anwendung sind binden, müssen Sie sicherstellen, dass die ausführbare Datei für diese Bibliotheken verknüpft ist.

Xamarin.iOS darüber informieren, wie Ihre Bibliotheken verknüpft werden müssen, dies ist möglich durch Ändern der Buildkonfiguration zum Aufrufen der `mtouch` -Befehl mit einigen zusätzlichen erstellen Argumente, die angeben, wie zur Verknüpfung mit den neuen Bibliotheken, die mithilfe der "-Gcc_flags"-Option gefolgt von einer Zeichenfolge in Anführungszeichen, die alle zusätzlichen Bibliotheken enthält, die für das Programm, wie folgt erforderlich sind:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Im Beispiel oben werden verknüpft `libMyLibrary.a`, `libSystemLibrary.dylib` und `CFNetwork` Frameworkbibliothek in Ihre endgültige ausführbare Datei.

Oder profitieren Sie von auf Assemblyebene [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), die in Ihren Vertragsdateien eingebettet werden können (z. B. `AssemblyInfo.cs`).
Bei Verwendung der [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), Sie müssen Ihre native Bibliothek, die zum Zeitpunkt verfügen, Sie stellen Ihre Bindung, da dies die native Bibliothek mit der Anwendung eingebettet wird. Zum Beispiel:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Sie sich vielleicht, warum müssen Sie `-force_load` Befehl und der Grund dafür ist, dass die ObjC - flag, obwohl sie den Code in kompiliert wird, behält nicht die Metadaten, die zur Unterstützung von Kategorien (die Beseitigung von totem Code Linker/Compiler entfernt sie) erforderlich sind die Sie müssen Sie zur Laufzeit für Xamarin.iOS.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Unterstützte Verweise

Einige vorübergehende Objekte wie Aktion Tabellen und Warnung Felder sind umständlich, um zu verfolgen für Entwickler, und der Generator für die Bindung ein bisschen dazu helfen kann.

Z. B. der Fall wäre eine Klasse, die wurde gezeigt, eine Nachricht und generiert dann eine `Done` -Ereignis, die traditionelle Methode dazu wäre:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

In diesem Szenario muss der Entwickler behalten dem Verweis auf das Objekt, seine und entweder Speicherverlusten oder deaktivieren Sie den Verweis für Box auf seine eigene aktiv.  Zwar Code für die Datenbindung, der Generator unterstützt Keeping Nachverfolgen des Verweises für Sie und deaktivieren, wenn eine spezielle Methode aufgerufen wird, der obige Code würde dann daraus:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Beachten Sie, wie sie nicht mehr erforderlich ist, der Variable in einer Instanz zu halten, dass sie mit einer lokalen Variablen funktioniert und dass es nicht erforderlich, um den Verweis zu löschen, wenn das Objekt nicht mehr aktiv ist.

Um diesen Vorteil nutzen zu können, müssen die Klasse eine Events-Eigenschaft, legen Sie in der [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Deklaration und die `KeepUntilRef` Variable festgelegt wird, auf den Namen der Methode, die aufgerufen wird, wenn das Objekt, z. B. abgeschlossen ist Dies:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Erben von Protokollen

Ab Xamarin.iOS v3. 2, unterstützen wir Sie erben von Protokollen, die mit markiert wurden die [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) Eigenschaft. Dies eignet sich für bestimmte API-Muster, wie z. B. im `MapKit` , in dem die `MKOverlay` Protokoll, erbt die `MKAnnotation` -Protokolls und übernommen wird, indem Sie eine Reihe von Klassen, die von erben `NSObject`.

In der Vergangenheit haben wir angefordert, kopieren das Protokoll auf allen Implementierungen, aber in diesen Fällen wir haben jetzt die `MKShape` Klasse erben, von der `MKOverlay` Protokoll, und es werden alle erforderlichen Methoden automatisch generieren.

## <a name="related-links"></a>Verwandte Links

- [Beispiel einer](https://developer.xamarin.com/samples/BindingSample/)
