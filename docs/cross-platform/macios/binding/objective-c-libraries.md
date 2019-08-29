---
title: Binden von Ziel-C-Bibliotheken
description: Dieses Dokument enthält eine allgemeine Übersicht über das Erstellen C# von Bindungen mit dem Ziel-C-Code, in dem beschrieben wird, wie Ereignisse, Methoden, benutzerdefinierte Steuerelemente und vieles mehr gebunden werden.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 667a3726a2d214c9e33e20a73f629c9ca532eab1
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120476"
---
# <a name="binding-objective-c-libraries"></a>Binden von Ziel-C-Bibliotheken

Bei der Arbeit mit xamarin. IOS oder xamarin. Mac können Sie Fälle erkennen, in denen Sie eine Ziel-C-Bibliothek eines Drittanbieters verwenden möchten. In diesen Fällen können Sie xamarin-Bindungs Projekte verwenden, um eine C# Bindung mit den nativen Ziel-C-Bibliotheken zu erstellen. Das Projekt verwendet die gleichen Tools, die wir verwenden, um die IOS-und Mac C#-APIs zu verwenden.

In diesem Dokument wird beschrieben, wie Sie die Ziel-C-APIs binden. Wenn Sie nur C-APIs binden, sollten Sie hierfür den standardmäßigen .net [-](https://www.mono-project.com/docs/advanced/pinvoke/)Mechanismus verwenden.
Details zur statischen Verknüpfung einer C-Bibliothek finden Sie auf der Seite Verknüpfungs systemeigene [Bibliotheken](~/ios/platform/native-interop.md) .

Weitere Informationen finden Sie im Referenzhandbuch zu begleitenden [Bindungs Typen](~/cross-platform/macios/binding/binding-types-reference.md)
Wenn Sie mehr darüber erfahren möchten, was im Hintergrund geschieht, überprüfen Sie die Seite [Bindungs Übersicht](~/cross-platform/macios/binding/overview.md) .

Bindungen können sowohl für IOS-als auch für Mac-Bibliotheken erstellt werden.
Auf dieser Seite wird beschrieben, wie Sie eine IOS-Bindung bearbeiten, aber Mac-Bindungen sind sehr ähnlich.

**Beispiel Code für IOS**

Sie können das Beispiel Projekt für [IOS](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) -Bindungen verwenden, um mit Bindungen zu experimentieren.

<a name="Getting_Started" />

## <a name="getting-started"></a>Erste Schritte

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die einfachste Möglichkeit zum Erstellen einer Bindung besteht darin, ein xamarin. IOS-Bindungs Projekt zu erstellen.
Hierzu können Sie Visual Studio für Mac den Projekttyp, die **IOS-> Bibliothek > Bindungs Bibliothek**auswählen:

[![](objective-c-libraries-images/00-sml.png "Wählen Sie hierzu Visual Studio für Mac den Projekttyp, die IOS-Bibliotheks Bindungs Bibliothek aus.")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die einfachste Möglichkeit zum Erstellen einer Bindung besteht darin, ein xamarin. IOS-Bindungs Projekt zu erstellen.
Sie können dies in Visual Studio unter Windows durchführen, indem Sie den Projekttyp,  **C# Visual > IOS > Bindungs Bibliothek (IOS)** auswählen:

[![](objective-c-libraries-images/00vs-sml.png "IOS-Bindungs Bibliothek (IOS)")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Hinweis: Bindungs Projekte für **xamarin. Mac** werden nur in Visual Studio für Mac unterstützt.

-----

Das generierte Projekt enthält eine kleine Vorlage, die Sie bearbeiten können, die zwei Dateien enthält `ApiDefinition.cs` : `StructsAndEnums.cs`und.

In `ApiDefinition.cs` der können Sie den API-Vertrag definieren. Hierbei handelt es sich um die Datei, in der beschrieben wird, wie die zugrundeliegende Ziel-C-API in C#projiziert wird. Die Syntax und der Inhalt dieser Datei sind das Hauptthema der Erörterung dieses Dokuments, und der Inhalt der Datei ist auf C# Schnittstellen C# und Delegatdeklarationen beschränkt. Bei `StructsAndEnums.cs` der Datei handelt es sich um die Datei, in die Sie alle für die Schnittstellen und Delegaten erforderlichen Definitionen eingeben. Dies schließt Enumerationswerte und Strukturen ein, die von Ihrem Code verwendet werden können.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Binden einer API

Um eine umfassende Bindung zu erreichen, sollten Sie die Definition der Ziel-C-API verstehen und sich mit den .NET Framework Entwurfs Richtlinien vertraut machen.

Um die Bibliothek zu binden, beginnen Sie in der Regel mit einer API-Definitionsdatei. Eine API-Definitionsdatei ist lediglich C# eine Quelldatei, C# die Schnittstellen enthält, die mit einer Handvoll von Attributen versehen wurden, die die Bindung steuern.  Diese Datei definiert, was der Vertrag zwischen C# und Ziel-C ist.

Dies ist z. b. eine triviale API-Datei für eine Bibliothek:

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

Im obigen Beispiel wird eine Klasse namens `Cocos2D.Camera` definiert, die `NSObject` vom Basistyp abgeleitet wird (dieser Typ `Foundation.NSObject`stammt von) und der eine statische Eigenschaft`ZEye`() definiert, zwei Methoden, die keine Argumente akzeptieren, und eine Methode, die drei Argumentation.

Eine ausführliche Erläuterung des Formats der API-Datei und der Attribute, die Sie verwenden können, finden Sie unten im Abschnitt [API-Definitionsdatei](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) .

Zum Herstellen einer kompletten Bindung arbeiten Sie in der Regel mit vier Komponenten:

- Die API-Definitionsdatei`ApiDefinition.cs` (in der Vorlage).
- Optional: Alle Enumerationstypen, Typen, Strukturen, die von der API`StructsAndEnums.cs` -Definitionsdatei (in der Vorlage) benötigt werden.
- Optional: zusätzliche Quellen, die die generierte Bindung erweitern können oder eine C# benutzerfreundlichere API (alle C# Dateien, die Sie dem Projekt hinzufügen) bereitstellen.
- Die native Bibliothek, die Sie binden.

Dieses Diagramm zeigt die Beziehung zwischen den Dateien:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Dieses Diagramm zeigt die Beziehung zwischen den Dateien.")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Die API-Definitionsdatei enthält nur Namespaces und Schnittstellendefinitionen (mit allen Membern, die eine Schnittstelle enthalten kann) und sollte keine Klassen, Enumerationen, Delegaten oder Strukturen enthalten. Die API-Definitionsdatei ist lediglich der Vertrag, der zum Generieren der API verwendet wird.

Zusätzlicher Code, den Sie wie Enumerationen oder Unterstützungs Klassen benötigen, sollte in einer separaten Datei gehostet werden, im obigen Beispiel ist der "cameramode" ein Enumerationswert, der in der CS-Datei nicht vorhanden ist und in einer separaten Datei gehostet werden soll `StructsAndEnums.cs` , z. b. :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

Die `APIDefinition.cs` Datei wird mit der `StructsAndEnum` -Klasse kombiniert und verwendet, um die Kern Bindung der Bibliothek zu generieren. Die resultierende Bibliothek kann unverändert verwendet werden, aber in der Regel sollten Sie die resultierende Bibliothek optimieren, um einige C# Features für die Vorteile Ihrer Benutzer hinzuzufügen. Beispiele hierfür sind das Implementieren `ToString()` einer Methode, C# das Bereitstellen von indexatoren, das Hinzufügen impliziter Konvertierungen zu und aus einigen systemeigenen Typen oder das Bereitstellen stark typisierter Versionen einiger Methoden Diese Verbesserungen werden in C# zusätzlichen Dateien gespeichert. Fügen Sie dem C# Projekt lediglich die Dateien hinzu, die in diesen Buildprozess eingeschlossen werden.

Dies zeigt, wie Sie den Code in `Extra.cs` der Datei implementieren. Beachten Sie, dass Sie partielle Klassen verwenden, da diese die partiellen Klassen erweitern, die aus der Kombi `ApiDefinition.cs` Nation von `StructsAndEnums.cs` und der Kern Bindung generiert werden:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Wenn Sie die Bibliothek aufbauen, wird Ihre Native Bindung erstellt.

Um diese Bindung abzuschließen, sollten Sie die native Bibliothek zum Projekt hinzufügen.  Hierzu können Sie die native Bibliothek zu Ihrem Projekt hinzufügen, indem Sie die native Bibliothek von Finder auf das Projekt im Projektmappen-Explorer ziehen und ablegen oder indem Sie mit der rechten Maustaste auf das Projekt klicken und hinzufügen**Dateien** hinzu **fügen** > auswählen. Wählen Sie die native Bibliothek aus.
Native Bibliotheken nach Konvention beginnen mit dem Wort "lib" und enden mit der Erweiterung ". a". Wenn Sie dies tun, werden Visual Studio für Mac zwei Dateien hinzufügen: die Datei ". a" und C# eine automatisch aufgefüllte Datei, die Informationen darüber enthält, was die native Bibliothek enthält:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Native Bibliotheken nach Konvention beginnen mit der Wort lib und enden mit der Erweiterung. a")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Der Inhalt `libMagicChord.linkwith.cs` der Datei enthält Informationen darüber, wie diese Bibliothek verwendet werden kann, und weist ihre IDE an, diese Binärdatei in die resultierende DLL-Datei zu packen:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Ausführliche Informationen zur Verwendung des[`[LinkWith]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 
das Attribut ist im [Referenzhandbuch zu Bindungs Typen](~/cross-platform/macios/binding/binding-types-reference.md)dokumentiert.

Wenn Sie nun das Projekt erstellen, erhalten Sie eine `MagicChords.dll` Datei, die die Bindung und die native Bibliothek enthält. Sie können dieses Projekt oder die resultierende DLL zur eigenen Verwendung an andere Entwickler verteilen.

Manchmal stellen Sie möglicherweise fest, dass Sie einige Enumerationswerte, Delegatdefinitionen oder andere Typen benötigen. Platzieren Sie diese nicht in der API-Definitionsdatei, da dies lediglich ein Vertrag ist.

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Die API-Definitionsdatei

Die API-Definitionsdatei besteht aus einer Reihe von Schnittstellen. Die Schnittstellen in der API-Definition werden in eine Klassen Deklaration umgewandelt, und Sie müssen mit [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) dem-Attribut versehen werden, um die Basisklasse für die-Klasse anzugeben.

Sie Fragen sich vielleicht, warum wir keine Klassen anstelle von Schnittstellen für die Vertrags Definition verwendet haben. Wir haben Schnittstellen gewählt, da es uns ermöglichte, den Vertrag für eine Methode zu schreiben, ohne einen Methoden Text in der API-Definitionsdatei bereitstellen zu müssen oder einen Text bereitzustellen, der eine Ausnahme auslösen oder einen sinnvollen Wert zurückgeben musste.

Da wir jedoch die-Schnittstelle als Gerüst zum Generieren einer Klasse verwenden, mussten wir die verschiedenen Teile des Vertrags mit Attributen versehen, um die Bindung zu steuern.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Bindungsmethoden

Die einfachste Bindung besteht darin, eine Methode zu binden. Deklarieren Sie einfach eine Methode in der Schnitt C# Stelle mit den Benennungs Konventionen, und ergänzen Sie die Methode mit dem[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
versehen. Das [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut verknüpft Ihren C# Namen mit dem Ziel-C-Namen in der xamarin. IOS-Laufzeit. Der-Parameter des[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
Attribut ist der Name des Ziels-C-Selektor. Einige Beispiele:

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

Die obigen Beispiele veranschaulichen, wie Instanzmethoden gebunden werden können. Zum Binden statischer Methoden müssen Sie das [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) -Attribut wie folgt verwenden:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Dies ist erforderlich, da der Vertrag Teil einer Schnittstelle ist und Schnittstellen keine Darstellung statischer vs-instanzdeklarationen aufweisen, sodass es erneut erforderlich ist, auf Attribute zurückzugreifen. Wenn Sie eine bestimmte Methode aus der Bindung ausblenden möchten, können Sie die Methode mit dem [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) -Attribut ergänzen.

Der `btouch-native` Befehl führt eine Überprüfung auf Verweis Parameter aus, die nicht NULL sind. Wenn Sie NULL-Werte für einen bestimmten Parameter zulassen möchten, verwenden Sie das[`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
-Attribut für den-Parameter wie folgt:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Beim Exportieren eines Verweis Typs mit dem [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Schlüsselwort können Sie auch die Zuordnungs Semantik angeben. Dies ist erforderlich, um sicherzustellen, dass keine Daten verloren gehen.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Bindungseigenschaften

Genau wie Methoden werden die Ziel-C-Eigenschaften mithilfe der[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
-Attribut, und ordnen C# Sie Eigenschaften direkt zu. Ebenso wie Methoden können Eigenschaften mit dem[`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)
Und die[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
legt.

Wenn Sie das [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut für eine Eigenschaft im Untertitel verwenden, bindet bberührungs-Native zwei Methoden: Getter und Setter. Der Name, den Sie für den Export angeben, ist der **baseName** , und der Setter wird berechnet, indem das Wort "Set" vorangestellt wird. dabei wird der erste Buchstabe des **baseName** in Großbuchstaben umgewandelt, und der Selektor nimmt ein Argument an. Dies bedeutet, `[Export ("label")]` dass die Anwendung auf eine Eigenschaft tatsächlich die Bezeichnungen "Label" und "setlabel:" bindet. Ziel-C-Methoden.

Manchmal folgen die Eigenschaften von "Ziel-C" nicht dem oben beschriebenen Muster, und der Name wird manuell überschrieben. In diesen Fällen können Sie steuern, wie die Bindung generiert wird, indem Sie die[`[Bind]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) 
-Attribut für den Getter oder Setter, z. b.:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Dadurch werden "ismenuvisible" und "setmenuvisible:" gebunden. Optional kann eine Eigenschaft mit der folgenden Syntax gebunden werden:

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

Dabei sind Getter und Setter explizit als in den `name` Bindungen und `setName` definiert.

Zusätzlich zur Unterstützung statischer Eigenschaften mithilfe [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)von können Sie Thread statische Eigenschaften mit [`[IsThreadStatic]`](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute)ergänzen, z. b.:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Ebenso wie Methoden das kennzeichnen einiger Parameter mit [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)zulassen, können Sie Folgendes anwenden:[`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
für eine Eigenschaft, die angibt, dass NULL ein gültiger Wert für die Eigenschaft ist, z. b.:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

Der [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) -Parameter kann auch direkt auf dem Setter angegeben werden:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Einschränkungen der Bindung von benutzerdefinierten Steuerelementen

Beachten Sie die folgenden Einschränkungen, wenn Sie die Bindung für ein benutzerdefiniertes Steuerelement einrichten:

1. **Bindungseigenschaften müssen statisch sein** : beim Definieren der Bindung von Eigenschaften muss das [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) -Attribut verwendet werden.
2. **Eigenschaftsnamen müssen exakt übereinstimmen** . der Name, der zum Binden der Eigenschaft verwendet wird, muss mit dem Namen der Eigenschaft im benutzerdefinierten Steuerelement exakt übereinstimmen.
3. **Eigenschafts Typen müssen exakt übereinstimmen** : der Variablentyp, mit dem die Eigenschaft gebunden wird, muss mit dem Typ der Eigenschaft im benutzerdefinierten Steuerelement exakt übereinstimmen.
4. Halte **Punkte und Getter/Setter** -Breakpoints, die in den Getter-oder Setter-Methoden der Eigenschaft platziert werden, werden nie getroffen.
5. **Beobachten** von Rückrufen: Sie müssen Überwachungs Rückrufe verwenden, um über Änderungen in den Eigenschafts Werten der benutzerdefinierten Steuerelemente benachrichtigt zu werden.

Wenn die oben aufgeführten Einschränkungen nicht beachtet werden, kann die Bindung zur Laufzeit automatisch fehlschlagen.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Änderbare-Muster und Eigenschaften von Ziel-C

Ziel-C-Frameworks verwenden eine Ausdrucks Formatierung, bei der einige Klassen mit einer änderbaren Unterklasse unveränderlich sind. Beispielsweise `NSString` ist die unveränderliche Version, während `NSMutableString` die Unterklasse ist, die Mutation zulässt.

In diesen Klassen ist es üblich, dass die unveränderliche Basisklasse Eigenschaften mit einem Getter, aber ohne Setter enthält. Und für die änderbare Version, um den Setter einzuführen. Da dies mit C#nicht wirklich möglich ist, mussten wir diese Ausdrucksweise einer Ausdrucksweise zuordnen, die mit C#funktioniert.

Die Zuordnung zu C# erfolgt durch Hinzufügen von Getter und Setter für die Basisklasse, aber durch das Kennzeichnen des Setters mit einem[`[NotImplemented]`](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute)
versehen.

Verwenden Sie dann in der änderbare-Unterklasse das[`[Override]`](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 
-Attribut für die-Eigenschaft, um sicherzustellen, dass die-Eigenschaft das übergeordnete Verhalten tatsächlich überschreibt.

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

### <a name="binding-constructors"></a>Bindungskonstruktoren

Das `btouch-native` Tool generiert automatisch Fours-Konstruktoren in der Klasse, die für eine bestimmte `Foo`Klasse generiert werden:

- `Foo ()`: der Standardkonstruktor (wird dem "init"-Konstruktor von "Ziel-C" zugeordnet)
- `Foo (NSCoder)`: der Konstruktor, der während der Deserialisierung von nib-Dateien verwendet wird (entspricht dem "initWithCoder:"-Konstruktor von Ziel-C).
- `Foo (IntPtr handle)`: der Konstruktor für die auf dem Handle basierende Erstellung, der von der Laufzeit aufgerufen wird, wenn die Laufzeit ein verwaltetes Objekt aus einem nicht verwalteten Objekt verfügbar machen muss.
- `Foo (NSEmptyFlag)`: wird von abgeleiteten Klassen verwendet, um eine doppelte Initialisierung zu verhindern.

Für Konstruktoren, die Sie definieren, müssen Sie mit der folgenden Signatur in der Schnittstellen Definition deklariert werden: Sie müssen einen `IntPtr` -Wert zurückgeben, und der Name der Methode sollte Konstruktor sein. Um z. b. `initWithFrame:` den Konstruktor zu binden, verwenden Sie Folgendes:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Bindungs Protokolle

Wie im Dokument zum API-Design beschrieben, ordnet xamarin. IOS die Ziele-C-Protokolle im Abschnitt [erörtern von Modellen und Protokollen](~/ios/internals/api-design/index.md#Models)den Klassen zu, die mit dem[`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)
versehen. Dies wird in der Regel verwendet, wenn Ziel-C-Delegatklassen implementiert werden.

Der große Unterschied zwischen einer regulären gebundenen Klasse und einer Delegatklasse besteht darin, dass die Delegatklasse eine oder mehrere optionale Methoden aufweisen kann.

Nehmen wir beispielsweise `UIKit` an `UIAccelerometerDelegate`, dass die-Klasse in xamarin. IOS gebunden ist:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Da dies eine optionale Methode für `UIAccelerometerDelegate` die Definition ist, müssen Sie nichts anderes tun. Wenn jedoch eine erforderliche Methode für das Protokoll vorhanden wäre, sollten Sie das[`[Abstract]`](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute)
-Attribut zur-Methode. Dadurch wird der Benutzer der-Implementierung gezwungen, tatsächlich einen Text für die-Methode bereitzustellen.

Im Allgemeinen werden Protokolle in Klassen verwendet, die auf Nachrichten reagieren. Dies erfolgt in der Regel in Ziel-C durch Zuweisen der Eigenschaft "Delegat" zu einer Instanz eines Objekts, die auf die Methoden im Protokoll antwortet.

Die Konvention in xamarin. IOS besteht darin, sowohl das lose gekoppelte Ziel-C-Format zu unterstützen `NSObject` , in dem eine beliebige Instanz von dem Delegaten zugewiesen werden kann, und auch eine stark typisierte Version davon verfügbar zu machen. Aus diesem Grund stellen wir in der Regel eine `Delegate` Eigenschaft mit starker Typisierung und eine `WeakDelegate` dar, die lose typisiert ist. In der Regel binden wir die lose typisierte [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)Version mit, und wir [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) verwenden das-Attribut, um die stark typisierte Version bereitzustellen.

Dies zeigt, wie wir die `UIAccelerometer` Klasse gebunden haben:

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

**Neu in MonoTouch 7,0**

Ab MonoTouch 7,0 wurde eine neue und verbesserte Protokoll Bindungs Funktion integriert.  Diese neue Unterstützung vereinfacht die Verwendung von Ziel-C-Ausdrücke für die Übernahme eines oder mehrerer Protokolle in einer bestimmten Klasse.

Für jede Protokoll Definition `MyProtocol` in Ziel-C gibt es jetzt eine `IMyProtocol` Schnittstelle, die alle erforderlichen Methoden aus dem Protokoll auflistet, und eine Erweiterungs Klasse, die alle optionalen Methoden bereitstellt.  Die oben genannten, in Kombination mit der neuen Unterstützung im-Xamarin Studio-Editor, ermöglichen es Entwicklern, Protokoll Methoden zu implementieren, ohne die separaten Unterklassen der vorherigen abstrakten Modellklassen verwenden zu müssen.

Jede Definition, die das [`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) -Attribut enthält, generiert tatsächlich drei unterstützende Klassen, die die Vorgehensweise bei der Nutzung von Protokollen erheblich verbessern:

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

Die **Klassen Implementierung** stellt eine vollständige abstrakte Klasse bereit, mit der Sie einzelne Methoden von überschreiben und vollständige Typsicherheit erhalten können.  Da es jedoch C# nicht möglich ist, mehrere Vererbung zu unterstützen, gibt es Szenarios, in denen Sie möglicherweise eine andere Basisklasse haben, aber trotzdem eine Schnittstelle implementieren möchten, wo die

Die generierte **Schnittstellen Definition** kommt ins Spiel.  Es handelt sich um eine Schnittstelle, die über alle erforderlichen Methoden aus dem Protokoll verfügt.  Dadurch können Entwickler, die Ihr Protokoll implementieren möchten, nur die-Schnittstelle implementieren.  Der Typ wird von der Common Language Runtime automatisch bei der Übernahme des Protokolls registriert.

Beachten Sie, dass die-Schnittstelle nur die erforderlichen Methoden auflistet und die optionalen Methoden verfügbar macht.  Dies bedeutet, dass Klassen, die das Protokoll übernehmen, eine vollständige Typüberprüfung für die erforderlichen Methoden erhalten, aber auf eine schwache Typisierung zurückgreifen müssen (manuell mithilfe von[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
Attribute und Übereinstimmung der Signatur) für die optionalen Protokoll Methoden.

Um es zu vereinfachen, eine API zu verwenden, die Protokolle verwendet, erzeugt das Bindungs Tool auch eine Klasse der Erweiterungs Methoden, die alle optionalen Methoden verfügbar macht.  Dies bedeutet, dass Sie, solange Sie eine API nutzen, Protokolle als alle Methoden behandeln können.

Wenn Sie die Protokoll Definitionen in ihrer API verwenden möchten, müssen Sie in ihrer API-Definition Skelett leere Schnittstellen schreiben.  Wenn Sie das MyProtocol in einer API verwenden möchten, müssen Sie Folgendes tun:

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

Der obige Wert ist erforderlich, da der zum `IMyProtocol` Zeitpunkt der Bindung nicht vorhanden ist. aus diesem Grund müssen Sie eine leere Schnittstelle bereitstellen.

#### <a name="adopting-protocol-generated-interfaces"></a>Anwenden von Protokoll generierten Schnittstellen

Wenn Sie eine der Schnittstellen implementieren, die für die Protokolle generiert werden, wie folgt:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Die Implementierung für die Schnittstellen Methoden wird automatisch mit dem richtigen Namen exportiert, sodass Sie dem folgenden entspricht:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Es spielt keine Rolle, ob die Schnittstelle implizit oder explizit implementiert wird.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Bindungs Klassen Erweiterungen

In Ziel-C ist es möglich, Klassen mit neuen Methoden zu C#erweitern, ähnlich wie bei den Erweiterungs Methoden von Spirit. Wenn eine dieser Methoden vorhanden ist, können Sie die[`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
Attribut zum Markieren der Methode als Empfänger der Ziel-C-Nachricht.

In xamarin. IOS haben wir z. b. die Erweiterungs Methoden gebunden, die `NSString` für `UIKit` definiert sind, wenn als Methoden `NSStringDrawingExtensions`in der importiert wird. Dies geschieht wie folgt:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Bindungs Ziel-C-Argumentlisten

Ziel-C unterstützt Variadic-Argumente. Beispiel:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Wenn Sie diese Methode von C# aufrufen möchten, sollten Sie eine Signatur wie die folgende erstellen:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Dadurch wird die Methode als intern deklariert, die oben beschriebene API wird von Benutzern ausgeblendet, aber der Bibliothek verfügbar gemacht. Anschließend können Sie eine Methode wie die folgende schreiben:

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

### <a name="binding-fields"></a>Bindungs Felder

Manchmal möchten Sie auf öffentliche Felder zugreifen, die in einer Bibliothek deklariert wurden.

Normalerweise enthalten diese Felder Zeichen folgen oder ganzzahlige Werte, auf die verwiesen werden muss. Sie werden in der Regel als Zeichenfolge verwendet, die eine bestimmte Benachrichtigung und als Schlüssel in Wörterbüchern darstellt.

Fügen Sie der Schnittstellen Definitionsdatei eine-Eigenschaft hinzu, und ergänzen Sie die-Eigenschaft mit dem [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) -Attribut, um ein Feld zu binden. Dieses Attribut übernimmt einen Parameter: den C-Namen des Symbols, das gesucht werden soll. Beispiel:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Wenn Sie verschiedene Felder in einer statischen Klasse einschließen möchten, die nicht von `NSObject`abgeleitet ist, können Sie das[`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 
-Attribut für die-Klasse wie folgt:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Im `LonelyClass` obigen Beispiel `NSSomeEventNotification` `NSString` `NSObject` 
 wird eine generiert, die nicht von abgeleitet ist und eine Bindung an die enthält, die als verfügbar gemacht wird. `NSString`

Das [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) -Attribut kann auf die folgenden Datentypen angewendet werden:

- `NSString`Verweise (nur schreibgeschützte Eigenschaften)
- `NSArray`Verweise (nur schreibgeschützte Eigenschaften)
- 32-Bit-int`System.Int32`()
- 64-Bit-int`System.Int64`()
- 32-Bit-Gleit`System.Single`Komma Zahlen ()
- 64-Bit-Gleit`System.Double`Komma Zahlen ()
- `System.Drawing.SizeF`
- `CGSize`

Zusätzlich zum Namen des systemeigenen Felds können Sie den Namen der Bibliothek, in der sich das Feld befindet, angeben, indem Sie den Bibliotheksnamen übergeben:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Wenn Sie statisch verknüpfen, ist keine Bibliothek für die Bindung vorhanden. Sie müssen daher den `__Internal` Namen verwenden:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Bindungs Aufstände

Sie können direkt `enum` in den Bindungs Dateien hinzufügen, um die Verwendung innerhalb von API-Definitionen zu vereinfachen, ohne eine andere Quelldatei zu verwenden (die in den Bindungen und im abschließenden Projekt kompiliert werden muss).

Beispiel:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Es ist auch möglich, eigene enumerationsenumeraten `NSString` zu erstellen, um Konstanten zu ersetzen. In diesem Fall erstellt der Generator **automatisch** die Methoden zum Konvertieren von Enumerationswerten und NSString-Konstanten für Sie.

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

Im obigen Beispiel können Sie sich entscheiden, `void Perform (NSString mode);` mit einem [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) -Attribut zu versehen. Dadurch wird die Konstante basierte API von ihren bindungsconsumers **ausgeblendet** .

Dies würde jedoch die Unterklassen des Typs einschränken, da die schönere API- [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) Alternative ein-Attribut verwendet. Diese generierten Methoden sind nicht `virtual`, d. h., Sie können Sie nicht überschreiben, was möglicherweise eine gute Wahl ist.

Eine Alternative besteht darin, die ursprüngliche, `NSString`-basierte Definition von als `[Protected]`zu markieren. Dadurch kann die Unterklassen bei Bedarf funktionieren, und die Version von wrapds funktioniert weiterhin, und die overridas-Methode wird aufgerufen.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Binden `NSValue`von `NSNumber`, und`NSString` an einen besseren Typ

Das [`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) `NSValue` -Attribut ermöglicht `NSNumber`das Binden `NSString`von und (enumeraten) C# in genauere Typen. Das-Attribut kann verwendet werden, um eine bessere, präzisere .NET-API über die Native API zu erstellen.

Sie können Methoden (bei Rückgabewert), Parameter und Eigenschaften mit [`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)ergänzen. Die einzige Einschränkung besteht darin, dass Ihr Member **nicht** innerhalb eines[`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 
oder [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) -Schnittstelle.

Beispiel:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Ausgabe:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

`bool?` Intern werden die  <->  Konvertierungen und`CGRect` durchführen. <->  `NSNumber` `NSValue`

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)unterstützt auch Arrays `NSNumber` von `NSString` `NSValue` und (-Aufständen).

Beispiel:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Ausgabe:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll`ist eine `NSString` gesicherte Enumeration, rufen wir den richtigen `NSString` Wert ab und behandeln die Typkonvertierung.

Weitere Informationen zu [`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) unterstützten Konvertierungs Typen finden Sie in der Dokumentation.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Bindungs Benachrichtigungen

Benachrichtigungen sind Nachrichten, die an den `NSNotificationCenter.DefaultCenter` gesendet werden und als Mechanismus zum Übertragen von Nachrichten von einem Teil der Anwendung an einen anderen verwendet werden. Entwickler abonnieren Benachrichtigungen in der Regel mithilfe der [addobserver](xref:Foundation.NSNotificationCenter.AddObserver(Foundation.NSString,System.Action{Foundation.NSNotification})) -Methode von [nsnotificationcenter](xref:Foundation.NSNotificationCenter). Wenn eine Anwendung eine Nachricht an das Benachrichtigungs Center sendet, enthält Sie in der Regel eine im [nsnotification. userinfo](xref:Foundation.NSNotification.UserInfo) -Wörterbuch gespeicherte Nutzlast. Dieses Wörterbuch ist schwach typisiert, und das erhalten von Informationen ist fehleranfällig, da Benutzer in der Regel die Dokumentation lesen müssen, welche Schlüssel im Wörterbuch verfügbar sind, und welche Werte im Wörterbuch gespeichert werden können. Das vorhanden sein von Schlüsseln wird manchmal auch als boolescher Wert verwendet.

Der xamarin. IOS-Bindungs Generator bietet Unterstützung für Entwickler, um Benachrichtigungen zu binden. Legen Sie hierzu das[`[Notification]`](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute)
Attribut für eine Eigenschaft, die auch mit einem gekennzeichnet wurde[`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)
Property (kann öffentlich oder privat sein).

Dieses Attribut kann ohne Argumente für Benachrichtigungen verwendet werden, die keine Nutzlast enthalten, oder Sie können einen `System.Type` angeben, der auf eine andere Schnittstelle in der API-Definition verweist, in der Regel mit dem Namen, der mit "EventArgs" endet. Der Generator schaltet die Schnittstelle in eine Klasse um, die `EventArgs` Unterklassen enthält, und schließt alle dort aufgeführten Eigenschaften ein. Das [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut sollte in der EventArgs-Klasse verwendet werden, um den Namen des Schlüssels aufzulisten, der verwendet wird, um das Ziel-C-Wörterbuch zu suchen und den Wert abzurufen.

Beispiel:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Der obige Code generiert eine-Klasse `MyClass.Notifications` mit den folgenden Methoden:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Benutzer Ihres Codes können dann problemlos Benachrichtigungen abonnieren, die an [nsdefaultcenter](xref:Foundation.NSNotificationCenter.DefaultCenter) gesendet werden, indem Sie Code wie den folgenden verwenden:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Der zurückgegebene Wert `ObserveDidStart` von kann verwendet werden, um den Empfang von Benachrichtigungen auf einfache Weise zu verhindern, wie hier:

```csharp
token.Dispose ();
```

Oder Sie können [nsnotification. defaultcenter. removeobserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) und das Token übergeben. Wenn Ihre Benachrichtigung Parameter enthält, sollten Sie wie folgt eine `EventArgs` Hilfsschnittstelle angeben:

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

Im obigen Beispiel wird eine `MyScreenChangedEventArgs` -Klasse mit `ScreenX` den `ScreenY` -und-Eigenschaften generiert, die die Daten aus dem [nsnotification. userinfo](xref:Foundation.NSNotification.UserInfo) -Wörterbuch mit den Schlüsseln "screenxkey" und "screenykey" abrufen und die richtige be. Das `[ProbePresence]` -Attribut wird für den Generator verwendet `UserInfo`, um zu überprüfen, ob der Schlüssel in festgelegt wird, anstatt zu versuchen, den Wert zu extrahieren. Dies wird für Fälle verwendet, in denen das vorhanden sein des Schlüssels der Wert ist (in der Regel für boolesche Werte).

Auf diese Weise können Sie Code wie den folgenden schreiben:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Bindungs Kategorien

Kategorien sind ein Ziel-C-Mechanismus, mit dem der in einer Klasse verfügbare Satz von Methoden und Eigenschaften erweitert wird.   In der Praxis werden Sie verwendet, um die Funktionalität einer Basisklasse zu erweitern (z `NSObject`. b.), wenn ein bestimmtes Framework in verknüpft ist (z `UIKit`. b.), sodass Ihre Methoden verfügbar sind, jedoch nur, wenn das neue Framework in verknüpft ist.   In einigen anderen Fällen werden Sie verwendet, um Funktionen in einer Klasse nach Funktionalität zu organisieren.   Sie ähneln den C# Erweiterungs Methoden. So sieht eine Kategorie in "Ziel-C" aus:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Im obigen Beispiel, wenn es in einer Bibliothek gefunden wird, `UIView` werden die Instanzen `makeBackgroundRed`von mit der-Methode erweitert.

Um diese zu binden, können Sie das [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) -Attribut für eine Schnittstellen Definition verwenden.  Wenn Sie die[`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
Attribute, die Bedeutung des[`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
Attribut Änderungen von werden verwendet, um die zu erweiternde Basisklasse anzugeben, als Typ, der erweitert werden soll.

Im folgenden wird gezeigt, `UIView` wie die Erweiterungen gebunden und in C# Erweiterungs Methoden umgewandelt werden:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Im obigen Beispiel wird eine `MyUIViewExtension` Klasse erstellt, die die `MakeBackgroundRed` Erweiterungsmethode enthält.  Dies bedeutet, dass Sie jetzt "makebackgroundred" für jede `UIView` Unterklasse abrufen können, sodass Sie die gleiche Funktionalität erhalten, die Sie für "Ziel-C" erhalten. In einigen anderen Fällen werden Kategorien nicht verwendet, um eine System Klasse zu erweitern, sondern um die Funktionalität zu organisieren.  Und zwar so:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Sie können zwar das[`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
das Attribut wird auch für diese Art von Deklarationen von Deklarationen hinzugefügt. Sie können auch alle Elemente der Klassendefinition hinzufügen.  Beide werden gleichzeitig erreicht:

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

In diesen Fällen ist es nur kürzer, die Kategorien zusammenzuführen:

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

### <a name="binding-blocks"></a>Bindungs Blöcke

Blöcke sind ein neues Konstrukt, das von Apple eingeführt wurde, um das C# funktionale Äquivalent anonymer Methoden zu "Ziel-C" zu bringen. Beispielsweise macht die `NSSet` -Klasse jetzt diese Methode verfügbar:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Die obige Beschreibung deklariert eine Methode namens `enumerateObjectsUsingBlock:` , die ein Argument mit `block`dem Namen annimmt. Dieser Block ähnelt einer C# anonymen Methode darin, dass er Unterstützung für das Erfassen der aktuellen Umgebung ("This"-Zeiger, Zugriff auf lokale Variablen und Parameter) hat. Die obige Methode in `NSSet` Ruft den-Block mit zwei `NSObject` Parametern ( `id obj` dem-Teil) und einem Zeiger auf einen booleschen `BOOL *stop`Teil (den) ab.

Um diese Art von API mit btouchscreen zu binden, müssen Sie zuerst die Blocktyp Signatur als C# Delegat deklarieren und dann wie folgt von einem API-Einstiegspunkt aus darauf verweisen:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Jetzt kann Ihr Code Ihre Funktion von folgenden Code C#aus abrufen:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Sie können auch Lambdas verwenden, wenn Sie dies bevorzugen:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Asynchrone Methoden

Der Bindungs Generator kann eine bestimmte Klasse von Methoden in Async-freundliche Methoden (Methoden, die eine Aufgabe oder Aufgabe&lt;T&gt;zurückgeben) umwandeln.

Sie können das[`[Async]`](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) 
Attribut für Methoden, die "void" zurückgeben und deren letztes Argument ein Rückruf ist.  Wenn Sie dies auf eine Methode anwenden, generiert der Bindungs Generator eine Version dieser Methode mit dem Suffix `Async`.  Wenn der Rückruf keine Parameter annimmt, ist der Rückgabewert ein `Task`. wenn der Rückruf einen-Parameter annimmt, ist das Ergebnis ein. `Task<T>`  Wenn der Rückruf mehrere Parameter erfordert, sollten Sie den `ResultType` oder `ResultTypeName` den gewünschten Namen des generierten Typs festlegen, der alle Eigenschaften enthält.

Beispiel:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Der obige Code generiert sowohl die LoadFile-Methode als auch die folgenden:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Starke Typen für schwache NSDictionary-Parameter

An vielen Stellen in der Ziel-C-API werden Parameter als schwach typisierte `NSDictionary` APIs mit bestimmten Schlüsseln und Werten übergeben. diese sind jedoch fehleranfällig (Sie können ungültige Schlüssel übergeben und keine Warnungen erhalten, Sie können ungültige Werte übergeben und keine Warnungen erhalten) und frustrierend sein. für die Verwendung von benötigen Sie mehrere Fahrten zur Dokumentation, um die möglichen Schlüsselnamen und-Werte zu suchen.

Die Lösung besteht darin, eine stark typisierte Version bereitzustellen, die die stark typisierte Version der API bereitstellt und hinter den Kulissen die verschiedenen zugrunde liegenden Schlüssel und Werte zuordnet.

Wenn z. b. die Ziel-C-API eine `NSDictionary` akzeptiert und Sie dokumentiert ist, dass Sie `XyzVolumeKey` den Schlüssel annimmt `NSNumber` , der einen mit einem volumewert von 0,0 `XyzCaptionKey` bis 1,0 und einen mit einer Zeichenfolge annimmt, sollen die Benutzer über eine schöne API verfügen. Das sieht wie folgt aus:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

Die `Volume` -Eigenschaft wird als float definiert, der NULL-Werte zulässt, da die Konvention in Ziel-C nicht erfordert, dass diese Wörterbücher den Wert aufweisen, sodass es Szenarien gibt, in denen der Wert möglicherweise nicht festgelegt ist.

Zu diesem Zweck müssen Sie einige Aufgaben ausführen:

- Erstellen Sie eine stark typisierte Klasse, die Unterklassen " [didirekarycontainer](xref:Foundation.DictionaryContainer) " ist und die verschiedenen Getter und Setter für jede Eigenschaft bereitstellt.
- Deklarieren Sie über Ladungen für die `NSDictionary` Methoden, die akzeptieren, um die neue, stark typisierte Version zu verwenden.

Sie können die stark typisierte Klasse entweder manuell erstellen oder den Generator verwenden, um die Arbeit zu erledigen.  Wir untersuchen zunächst, wie Sie dies manuell durchführen, um zu verstehen, was passiert, und dann die automatische Vorgehensweise.

Hierfür müssen Sie eine Unterstützungs Datei erstellen, die nicht in Ihre Vertrag-API wechselt.  So müssen Sie schreiben, um die xyzoptions-Klasse zu erstellen:

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

Sie sollten dann eine Wrapper Methode bereitstellen, die die API auf hoher Ebene auf der Low-Level-API aufbringt.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Wenn Ihre API nicht überschrieben werden muss, können Sie die auf NSDictionary basierende API auf sichere Weise ausblenden, indem Sie die[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
versehen.

Wie Sie sehen können, verwenden wir das[`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)
-Attribut, um einen neuen API-Einstiegspunkt zu verwenden, und wir verwenden unsere stark `XyzOptions` typisierte-Klasse.  Die Wrapper Methode ermöglicht auch das passieren von NULL.

Nun haben wir nicht erwähnt, wo die `XyzOptionsKeys` Werte stammen.  In der Regel gruppieren Sie die Schlüssel, die eine API in einer statischen Klasse `XyzOptionsKeys`wie z. b. wie folgt:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Sehen wir uns die automatische Unterstützung für die Erstellung dieser stark typisierten Wörterbücher an.  Dadurch wird die Menge der Bausteine vermieden, und Sie können das Wörterbuch direkt in Ihrem API-Vertrag definieren, anstatt eine externe Datei zu verwenden.

Um ein stark typisiertes Wörterbuch zu erstellen, führen Sie eine Schnittstelle in ihrer API ein, und ergänzen Sie Sie mit dem [strongdictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) -Attribut.  Dies weist den Generator an, dass eine Klasse mit dem gleichen Namen wie die Schnittstelle erstellt werden soll, `DictionaryContainer` die von abgeleitet wird, und stellt starke typisierte Accessoren dafür bereit.

Das [`[StrongDictionary]`](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) -Attribut nimmt einen Parameter an, der den Namen der statischen Klasse enthält, die ihre Wörterbuch Schlüssel enthält.  Anschließend wird jede Eigenschaft der Schnittstelle zu einem stark typisierten Accessor.  Standardmäßig verwendet der Code den Namen der Eigenschaft mit dem Suffix "Key" in der statischen Klasse, um den Accessor zu erstellen.

Dies bedeutet, dass für das Erstellen Ihres stark typisierten Accessors keine externe Datei mehr erforderlich ist, und dass Sie für jede Eigenschaft keine Get-und Setter manuell erstellen müssen, und dass Sie die Schlüssel nicht selbst manuell suchen müssen.

Die gesamte Bindung würde wie folgt aussehen:

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

Wenn Sie in Ihren `XyzOption` Membern ein anderes Feld referenzieren müssen (das nicht der Name der Eigenschaft mit dem Suffix `Key`ist), können Sie die Eigenschaft mit einem[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
Attribut mit dem Namen, den Sie verwenden möchten.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Typzuordnungen

In diesem Abschnitt wird erläutert, C# wie Ziel-C-Typen Typen zugeordnet werden.

<a name="Simple_Types" />

### <a name="simple-types"></a>Einfache Typen

In der folgenden Tabelle wird gezeigt, wie Sie der xamarin. IOS-Welt Typen von der Ziel-C-und der cocoatouch-Welt zuordnen:

|Ziel-C-Typname|Xamarin. IOS-Unified API-Typ|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString`([Weitere Informationen zur Bindung von NSString](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string`(siehe auch: [`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|CoreFoundation-Typen`CF*`()|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Foundation-Typen`NS*`()|`Foundation.NS*`|
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

Die xamarin. IOS-Laufzeit übernimmt automatisch die Konvertierung C# von Arrays `NSArrays` in und führt die Konvertierung zurück, z. b. die imaginäre Ziel-C-Methode `NSArray` , `UIViews`die eine von zurückgibt:

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

Die Idee besteht darin, ein stark typisiertes C# Array zu verwenden, da die IDE die ordnungsgemäße Codevervollständigung mit dem tatsächlichen Typ bereitstellen kann, ohne den Benutzer zu erraten, oder die Dokumentation nachschlagen, um den tatsächlichen Typ der im Array enthaltenen Objekte herauszufinden.

In Fällen, in denen Sie den tatsächlich am meisten abgeleiteten Typ, der im Array enthalten ist, nicht `NSObject []` verfolgen können, können Sie als Rückgabewert verwenden.

<a name="Selectors" />

### <a name="selectors"></a>Selektoren

Selectors werden in der Ziel-C-API als spezieller Typ `SEL`angezeigt. Wenn Sie einen Selektor binden, ordnen Sie den Typ `ObjCRuntime.Selector`zu.  Selektoren werden in der Regel in einer API mit einem Objekt, dem Zielobjekt und einem Selektor verfügbar gemacht, der im Zielobjekt aufgerufen werden soll. Das Bereitstellen beider Elemente entspricht im Grunde C# dem-Delegaten: etwas, das sowohl die aufzurufende Methode als auch das Objekt kapselt, in dem die Methode aufgerufen werden soll.

Die Bindung sieht wie folgt aus:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

Und auf diese Weise wird die-Methode in der Regel in einer Anwendung verwendet:

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

Um die Bindung für Entwickler zu C# vereinfachen, stellen Sie in der Regel eine Methode bereit, `NSAction` die einen-Parameter C# annimmt, mit dem Delegaten und Lambdas anstelle `Target+Selector`von verwendet werden können. Zu diesem Zweck würden Sie die `SetTarget` Methode in der Regel ausblenden, indem Sie Sie mit einem[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
-Attribut, und dann würden Sie eine neue Hilfsmethode wie die folgende verfügbar machen:

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

Nun kann der Benutzercode wie folgt geschrieben werden:

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

Wenn Sie eine Methode binden, die einen `NSString`annimmt, können Sie dies durch einen C# Zeichen foldtyp ersetzen, sowohl bei Rückgabe Typen als auch bei Parametern.

Der einzige Fall, wenn Sie einen `NSString` direkt verwenden möchten, ist, wenn die Zeichenfolge als Token verwendet wird. Weitere Informationen zu Zeichen folgen und `NSString`finden Sie im Dokument " [API-Design für NSString](~/ios/internals/api-design/nsstring.md) ".

In einigen seltenen Fällen kann eine API eine C-ähnliche Zeichenfolge (`char *`) anstelle einer Ziel-C-Zeichenfolge (`NSString *`) verfügbar machen. In diesen Fällen können Sie den Parameter mit dem Parameter[`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring)
versehen.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>Out/ref-Parameter

Einige APIs geben Werte in ihren Parametern zurück oder übergeben Parameter als Verweis.

In der Regel sieht die Signatur wie folgt aus:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

Im ersten Beispiel wird eine gängige Ziel-C-Ausdrucksweise zum Zurückgeben von Fehlercodes, `NSError` ein Zeiger auf einen Zeiger und bei der Rückgabe der Wert festgelegt.   Die zweite Methode zeigt, wie eine Ziel-C-Methode ein Objekt annehmen und seinen Inhalt ändern kann.   Dabei handelt es sich um eine Pass-by-Referenz anstelle eines reinen Ausgabe Werts.

Ihre Bindung würde wie folgt aussehen:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Attribute der Speicherverwaltung

Wenn Sie das [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut verwenden und Daten übergeben, die von der aufgerufenen Methode beibehalten werden, können Sie die Argument Semantik angeben, indem Sie Sie als zweiten Parameter übergeben, z. b.:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Der obige Wert würde den Wert als "beibehalten"-Semantik markieren. Die verfügbare Semantik lautet:

- Zuweisen
- Kopieren
- Erhalten

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Stilrichtlinien

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>Using [Internal]

Sie können das[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
das Attribut, um eine Methode aus der öffentlichen API auszublenden. Dies empfiehlt sich in Fällen, in denen die verfügbar gemachte API zu niedrig ist und Sie basierend auf dieser Methode eine Implementierung auf hoher Ebene in einer separaten Datei bereitstellen möchten.

Sie können diese Option auch verwenden, wenn Sie Einschränkungen im Bindungs Generator feststellen, z. b. einige erweiterte Szenarios können nicht gebundene Typen verfügbar machen, und Sie möchten diese Typen selbst in eine eigene Weise einbinden.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Ereignishandler und Rückrufe

Ziel-c-Klassen senden in der Regel Benachrichtigungen oder fordern Informationen an, indem eine Nachricht an eine Delegatklasse gesendet wird (Ziel-c-Delegat).

Dieses Modell kann zwar vollständig unterstützt werden und von xamarin. IOS unterstützt werden, ist jedoch manchmal mühsam. Xamarin. IOS macht das C# Ereignis Muster und ein Methoden rückrufsystem für die Klasse verfügbar, das in diesen Situationen verwendet werden kann. Dadurch kann Code wie dieser ausgeführt werden:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Der Bindungs Generator ist in der Lage, die Menge der Typisierung zu verringern, die erforderlich ist, um C# das Ziel-C-Muster dem Muster zuzuordnen.

Ab xamarin. IOS 1,4 ist es auch möglich, den Generator anzuweisen, Bindungen für bestimmte Ziel-C-Delegaten zu generieren und den Delegaten C# als Ereignisse und Eigenschaften für den Hosttyp verfügbar zu machen.

An diesem Prozess sind zwei Klassen beteiligt: die Host Klasse, die derzeit Ereignisse ausgibt und diese `Delegate` an oder `WeakDelegate` und die tatsächliche Delegatklasse sendet.

Berücksichtigen Sie Folgendes:

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

Um die Klasse zu schließen, müssen Sie folgende Schritte ausführen:

- Fügen Sie in der Host Klasse zu Ihrem[`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   Deklaration der Typ, der als Delegat und der C# von Ihnen verfügbar gemachte Name fungiert. In unserem obigen Beispiel sind `typeof (MyClassDelegate)` `WeakDelegate` bzw.
- In ihrer Delegatklasse müssen Sie für jede Methode, die über mehr als zwei Parameter verfügt, den Typ angeben, den Sie für die automatisch generierte EventArgs-Klasse verwenden möchten.

Der Bindungs Generator ist nicht darauf beschränkt, nur ein einzelnes Ereignis Ziel zu umwickeln. es ist möglich, dass einige Ziel-C-Klassen Nachrichten an mehr als einen Delegaten ausgeben, sodass Sie Arrays zur Unterstützung dieses Setups bereitstellen müssen. Die meisten Setups benötigen Sie nicht, aber der Generator ist bereit, diese Fälle zu unterstützen.

Der resultierende Code lautet wie folgt:

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

Wird verwendet, um den Namen `EventArgs` der zu generierenden Klasse anzugeben. `EventArgs` Sie sollten eine pro Signatur verwenden (in diesem Beispiel enthält die `EventArgs` eine `With` Eigenschaft des Typs NINT).

Mit den oben aufgeführten Definitionen erzeugt der Generator das folgende Ereignis in der generierten MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Nun können Sie den Code wie den folgenden verwenden:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Rückrufe sind genau wie Ereignis Aufrufe, der Unterschied besteht darin, dass anstelle mehrerer potenzieller Abonnenten (z. b. mehrere Methoden in einem `Clicked` Ereignis oder einem `DownloadFinished` -Ereignis eingebunden werden können) Rückrufe nur einen einzelnen Abonnenten haben können.

Der Prozess ist identisch. der einzige Unterschied besteht darin, dass anstelle des namens `EventArgs` der Klasse, die generiert werden soll, der EventArgs-Parameter verwendet wird, um den resultierenden C# Delegatnamen zu benennen.

Wenn die-Methode in der Delegatklasse einen Wert zurückgibt, ordnet der Bindungs Generator dies einer Delegatmethode in der übergeordneten Klasse anstelle eines Ereignisses zu. In diesen Fällen müssen Sie den Standardwert angeben, der von der-Methode zurückgegeben werden soll, wenn der Benutzer nicht an den Delegaten gebunden ist. Verwenden Sie hierzu das[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)
\- [`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) oder-Attribute.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)wird einen Rückgabewert hart codieren, während[`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)
wird verwendet, um anzugeben, welches Eingabe Argument zurückgegeben wird.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Enumerationen und Basis Typen

Sie können auch auf Enumerationen oder Basis Typen verweisen, die nicht direkt vom btouchscreen-Schnittstellen definitionssystem unterstützt werden. Fügen Sie zu diesem Zweck ihre Enumerationen und Kern Typen in eine separate Datei ein, und fügen Sie Sie als Teil einer der zusätzlichen Dateien ein, die Sie für bnote bereitstellen.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Verknüpfen der Abhängigkeiten

Wenn Sie APIs binden, die nicht Teil der Anwendung sind, müssen Sie sicherstellen, dass die ausführbare Datei mit diesen Bibliotheken verknüpft ist.

Sie müssen xamarin. IOS informieren, wie Ihre Bibliotheken verknüpft werden können. Dies kann entweder durch Ändern der Buildkonfiguration geändert werden, `mtouch` um den Befehl mit einigen zusätzlichen buildargumenten aufzurufen, die angeben, wie mit den neuen Bibliotheken mithilfe der Option "-gcc_flags" eine Verknüpfung hergestellt werden soll. gefolgt von einer Zeichenfolge in Anführungszeichen, die alle zusätzlichen Bibliotheken enthält, die für das Programm erforderlich sind, wie hier:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Im obigen Beispiel wird eine `libMyLibrary.a`Verknüpfung `libSystemLibrary.dylib` mit der `CFNetwork` frameworkbibliothek mit der endgültigen ausführbaren Datei erstellt.

Oder Sie können die auf Assemblyebene [`[LinkWithAttribute]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute)nutzen, die Sie in Ihre Vertrags Dateien einbetten können ( `AssemblyInfo.cs`z. b.).
Wenn Sie verwenden [`[LinkWithAttribute]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), müssen Sie die native Bibliothek zum Zeitpunkt der Bindung verfügbar machen, da dadurch die native Bibliothek in Ihre Anwendung eingebettet wird. Beispiel:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Vielleicht Fragen Sie sich, warum Sie einen `-force_load` Befehl benötigen, und der Grund ist, dass das-ObjC-Flag, obwohl der Code in kompiliert wird, die für die Unterstützung von Kategorien erforderlichen Metadaten nicht beibehält (der Linker/Compiler entfernt den Code, der Sie zur Laufzeit für xamarin. IOS erforderlich.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Unterstützte Verweise

Einige vorübergehende Objekte, wie z. b. Aktions Blätter und Warnungs Felder, sind umständlich, damit Entwickler nicht nachverfolgen können, und der Bindungs Generator kann etwas weiter helfen.

Wenn Sie z. b. eine Klasse haben, die eine Meldung anzeigt und `Done` dann ein-Ereignis generiert hat, ist die herkömmliche Vorgehensweise bei der Behandlung von:

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

Im obigen Szenario muss der Entwickler den Verweis auf das Objekt selbst aufbewahren und den Verweis für das Feld selbst löschen oder aktiv löschen.  Beim Binden von Code unterstützt der Generator die Nachverfolgung des Verweises für Sie. Wenn eine spezielle Methode aufgerufen wird, wird der obige Code dann wie folgt angezeigt:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Beachten Sie, dass es nicht mehr notwendig ist, die Variable in einer Instanz beizubehalten, dass Sie mit einer lokalen Variablen funktioniert und dass es nicht notwendig ist, den Verweis zu löschen, wenn das Objekt ausfällt.

Um dies zu nutzen, sollte für Ihre Klasse in der [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) -Deklaration eine-Ereignis Eigenschaft festgelegt sein. Außerdem sollte die `KeepUntilRef` Variable auf den Namen der Methode festgelegt werden, die aufgerufen wird, wenn das Objekt die Arbeit abgeschlossen hat, wie folgt:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Erben von Protokollen

Ab xamarin. IOS v 3.2 unterstützen wir die Vererbung von Protokollen, die mit der [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) -Eigenschaft gekennzeichnet wurden. Dies ist in bestimmten API-Mustern nützlich, z. `MapKit` b. `MKOverlay` in `MKAnnotation` , wenn das Protokoll vom Protokoll erbt und von einer Reihe von Klassen übernommen wird, die `NSObject`von erben.

In der Vergangenheit mussten wir das Protokoll in jede Implementierung kopieren, aber in diesen Fällen kann die `MKShape` Klasse jetzt `MKOverlay` vom Protokoll erben, und es werden automatisch alle erforderlichen Methoden generiert.

## <a name="related-links"></a>Verwandte Links

- [Bindungs Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/bindingsample/)
 