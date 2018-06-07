---
title: Binden von Objective-C-Bibliotheken
description: Dieses Dokument enthält eine allgemeine Übersicht über die zum Erstellen der C#-Bindungen in Objective-C-Code, der beschreibt, wie Ereignisse, Methoden und benutzerdefinierte Steuerelemente binden.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: f7c4be4254ce3e3301c0c1e98d37134f5524c23b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782319"
---
# <a name="binding-objective-c-libraries"></a>Binden von Objective-C-Bibliotheken

Bei der Arbeit mit Xamarin.iOS oder Xamarin.Mac können Fälle auftreten, in dem Sie eine Drittanbieter-Objective-C-Bibliothek nutzen möchten. In solchen Situationen können Sie Projekte für Xamarin-Bindung verwenden, erstellen eine C#-Bindung an den systemeigenen Objective-C-Bibliotheken. Das Projekt verwendet den gleichen Tools, mit denen wir die IOS- und Mac-APIs in c# zu bringen.

Dieses Dokument wird beschrieben, wie Objective-C-APIs gebunden, wenn Sie C-APIs binden, sollten Sie den Standardmechanismus für .NET verwenden, hierzu [das P/Invoke-Framework](http://www.mono-project.com/docs/advanced/pinvoke/).
Details zum C-Bibliothek statisch verknüpft sind verfügbar, auf die [systemeigene Bibliotheken verknüpfen](~/ios/platform/native-interop.md) Seite.

Finden Sie in unserem Companion [Typen Referenzhandbuch binden](~/cross-platform/macios/binding/binding-types-reference.md).
Wenn Sie möchten erfahren mehr darüber, was sich hinter den Kulissen geschieht, darüber hinaus überprüfen Sie unsere [Übersicht über Datenbindung](~/cross-platform/macios/binding/overview.md) Seite.

Bindungen können für IOS- und Mac-Bibliotheken erstellt werden.
Diese Seite beschreibt die Arbeit auf einem iOS binden, jedoch für die Mac-Bindungen sehr ähnlich sind.

**Beispielcode für iOS**

Sie können der [iOS Binding Sample](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) Projekt zum Experimentieren mit Bindungen.

<a name="Getting_Started" />

## <a name="getting-started"></a>Erste Schritte

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die einfachste Möglichkeit, eine Bindung erstellen, ist ein Xamarin.iOS Bindung-Projekt erstellt.
Hierzu können Sie in Visual Studio für Mac durch Auswahl des Projekttyps **iOS > Bibliothek > Bindungen Bibliothek**:

[![](objective-c-libraries-images/00-sml.png "Dazu in Visual Studio für Mac den Projekttyp, den iOS-Bibliothek Bindungen Library")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Die einfachste Möglichkeit, eine Bindung erstellen, ist ein Xamarin.iOS Bindung-Projekt erstellt.
Hierzu können Sie in Visual Studio unter Windows durch Auswahl des Projekttyps **Visual c# > iOS > Bindungen-Bibliothek (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS Bindungen Bibliothek iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Hinweis: Bindung von Testprojekten für **Xamarin.Mac** werden nur in Visual Studio für Mac unterstützt

-----

Das generierte Projekt enthält eine kleine Vorlage, die Sie bearbeiten können, es enthält zwei Dateien: `ApiDefinition.cs` und `StructsAndEnums.cs`.

Die `ApiDefinition.cs` ist, in der Sie die API-Vertrag definieren dies ist die Datei, die beschreibt, wie der zugrunde liegenden Objective-C-API in c# projiziert wird. Die Syntax und den Inhalt dieser Datei sind das Hauptthema Diskussion dieses Dokuments und der Inhalt des Zertifikats auf C#-Schnittstellen und Delegatdeklarationen c# beschränkt. Die `StructsAndEnums.cs` ist die Datei, in dem Sie alle Definitionen, die erforderlich sind, durch die Schnittstellen und Delegaten geben. Dies schließt Enumerationswerte und Strukturen, die den Code verwenden kann.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Binden einer API

Um eine umfassende Bindung zu erreichen, sollten Sie verstehen, die Objective-C-API-Definition und sich mit den Entwurfsrichtlinien von .NET Framework vertraut machen.

Um die Bibliothek zu binden, die Sie in der Regel mit einer API-Definitionsdatei gestartet werden. Eine API-Definitionsdatei ist lediglich eine C#-Quelldatei mit c#-Schnittstellen, die eine Handvoll Attribute kommentiert haben, mit denen Laufwerk der Bindung.  Diese Datei ist, was definiert, was der Vertrag zwischen C#- und Objective-C ist.

Beispielsweise ist dies eine einfache API-Datei für eine Bibliothek an:

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

Im obigen Beispiel definiert eine Klasse namens `Cocos2D.Camera` abgeleitet, die die `NSObject` Basistyp (dieses Typs stammen aus `Foundation.NSObject`) und definiert eine statische Eigenschaft (`ZEye`), zwei Methoden, die keine Argumente und eine Methode, die akzeptiert drei Argumente.

Eine ausführliche Diskussion der das Format der API-Datei und die Attribute, die Sie verwenden können, wird in behandelt die [-API-Definitionsdatei](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) Abschnitt weiter unten.

Um eine vollständige Bindung zu erstellen, werden Sie in der Regel vier Komponenten behandeln:

-  Die API-Definitionsdatei (`ApiDefinition.cs` in der Vorlage).
-  Optional: Alle Enumerationen, Typen, Strukturen, die erforderlich sind, durch die API-Definitionsdatei (`StructsAndEnums.cs` in der Vorlage).
-  Optional: zusätzliche Datenquellen, die möglicherweise die generierte Bindung zu erweitern, oder geben Sie eine weitere c# benutzerfreundliche API (alle C#-Dateien, die Sie dem Projekt hinzu).
-  Die systemeigene Bibliothek, die Sie binden.

Dieses Diagramm zeigt die Beziehung zwischen den Dateien:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Dieses Diagramm zeigt die Beziehung zwischen den Dateien")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Die Datei-API-Definition enthält nur Definitionen von Namespaces und -Schnittstelle (mit allen Mitgliedern, die eine Schnittstelle enthalten kann), und dürfen nicht für Klassen, Enumerationen, Delegaten oder Strukturen. Die API-Definitionsdatei ist lediglich den Vertrag, der zum Generieren der API verwendet werden.

Alle zusätzlichen Code, müssen Sie die Enumerationen wie oder unterstützende Klassen auf einer separaten Datei, in dem Beispiel oben die "CameraMode" gehostet werden soll, ist ein Enumerationswert, der in der CS-Datei nicht vorhanden und sollte in einer separaten Datei gehostet werden, z. B. `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

Die `APIDefinition.cs` Datei wird mit kombiniert die `StructsAndEnum` Klasse und werden verwendet, um die Core-Bindung der Bibliothek zu generieren. Sie können die resultierende Bibliothek als-ist, aber in der Regel empfiehlt sich die resultierende Bibliothek hinzufügen von einige C#-Funktionen für Ihre Benutzer zu optimieren. Einige Beispiele für implementieren eine `ToString()` -Methode, bieten C#-Indexer, Hinzufügen von impliziten Konvertierungen in und aus einigen systemeigenen Datentypen oder stark typisierte Versionen einiger Methoden bereitstellen. Diese Verbesserungen werden in zusätzlichen C#-Dateien gespeichert. Lediglich C#-Dateien zum Projekt hinzufügen, und sie werden in dieser Buildprozess enthalten sein.

Dies zeigt, wie Sie den Code in implementieren würde Ihre `Extra.cs` Datei. Beachten Sie, dass Sie partielle Klassen verwendet werden, wie diese partiellen Klassen erweitern, die aus der Kombination von generiert werden die `ApiDefinition.cs` und `StructsAndEnums.cs` Haupt-Bindung:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Erstellen die Bibliothek wird eine systemeigene Bindung erzeugen.

Um diese Bindung abgeschlossen haben, sollten Sie die systemeigene Bibliothek zum Projekt hinzufügen.  Dazu können Sie die systemeigene Bibliothek entweder per Drag & Drop die systemeigene Bibliothek Finder auf das Projekt im Projektmappen-Explorer oder Ihrem Projekt hinzufügen, indem Sie mit der rechten Maustaste des Projekts und **hinzufügen**  >  **Hinzufügen von Dateien** die systemeigene Bibliothek auswählen.
Systemeigene Bibliotheken gemäß der Konvention mit dem Wort "Lib" beginnen und enden mit der Erweiterung "a". Wenn Sie dies tun, werden zwei Dateien von Visual Studio für Mac hinzufügen: eine Datei und einer automatisch ausgefüllt c#-Datei, die enthält Informationen, was die systemeigene Bibliothek enthält:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Systemeigene Bibliotheken gemäß der Konvention mit dem Word-Lib beginnen und enden mit der Erweiterung eine")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Der Inhalt der `libMagicChord.linkwith.cs` Datei enthält Informationen dazu, wie diese Bibliothek verwendet werden kann, und weist die IDE dieser Binärdatei in die resultierende DLL-Datei Verpacken:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Vollständige Informationen zum Verwenden der [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) Attribut in dokumentiert sind die [Typen Referenzhandbuch binden](~/cross-platform/macios/binding/binding-types-reference.md).

Nun, wenn Sie das Projekt erstellen Sie zum Schluss werden eine `MagicChords.dll` Datei, die die Bindung und die systemeigene Bibliothek enthält. Sie können dieses Projekt verteilen, oder verwenden Sie die resultierende DLL an andere Entwickler für ihre eigenen.

In einigen Fällen finden Sie möglicherweise benötigen Sie ein paar Enumerationswerte, Delegaten Definitionen oder andere Typen. Platzieren Sie nicht die in der Datei-API-Definitionen, wie lediglich einen Vertrag sieht

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Die API-Definitionsdatei

Die API-Definitionsdatei umfasst eine Reihe von Schnittstellen. Schnittstellen in der API-Definition werden in einer Klassendeklaration umgewandelt werden, und sie müssen mit ergänzt werden die [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Attribut, um die Basisklasse für die Klasse anzugeben.

Sie vielleicht, warum wir nicht Klassen anstelle von Schnittstellen für die Vertragsdefinition verwendet haben. Wir Schnittstellen ausgewählt, da es nicht zulässig, schreiben den Vertrag für eine Methode ohne Methodenkörper in der API-Definitionsdatei bereitstellen müssen, müssen oder auf einen Text angeben, mit der eine Ausnahme auslöst oder keinen sinnvollen Wert zurück.

Aber da wir die Schnittstelle als ein Gerüst zum Generieren einer Klasse, die wir hatten verwenden hierfür auf verschiedene Teile des Vertrags mit Attributen auf die Bindung Laufwerk ergänzen.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Binden von Methoden

Die einfachste Bindung, die Sie ausführen können, ist eine Methode zu binden. Nur eine Methode in der Schnittstelle mit den C#-Benennungskonventionen zu deklarieren und ergänzen Sie die Methode mit dem [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribut. Die [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut ist, welche Ihrer C#-Name mit dem Namen "Objective-C" in der Laufzeit Xamarin.iOS links. Die Parameter von der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut ist der Name des dem Objective-C-Selektor. Einige Beispiele:

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

In den obigen Beispielen wird wie Instanzmethoden gebunden werden kann. Um statische Methoden zu binden, müssen Sie verwenden die [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) -Attribut, wie folgt:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Dies ist erforderlich, da der Vertrag Teil einer Schnittstelle ist und Schnittstellen keine statische und Instanz-Deklarationen, kennen daher ist es notwendig noch einmal, die Attribute verwenden. Wenn Sie eine bestimmte Methode von der Bindung ausblenden möchten, können Sie die Methode mit ergänzen die [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut.

Die `btouch-native` Befehl ist, erfordert die sucht Verweisparameter nicht null sein. Wenn Sie für einen bestimmten Parameter null-Werte zulassen möchten, verwenden Sie die [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Attribut für die Parameter wie folgt:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Beim Exportieren eines Verweistyps, mit der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Schlüsselwort können Sie auch die Semantik für die Zuweisung angeben. Dies ist erforderlich, um sicherzustellen, dass keine Daten verloren ist.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Datenbindungseigenschaften

Methoden, wie Objective-C-Eigenschaften gebunden sind mit den [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribut und direkt C#-Eigenschaften zugeordnet. Genau wie die Methoden, Eigenschaften ergänzt werden können, mit der [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) und [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribute.

Bei Verwendung der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribut auf eine Eigenschaft mit dem die Btouch deckt systemeigene bindet tatsächlich zwei Methoden: die Getter und Setter-Methode. Der angegebene Name ist zum Exportieren der **Basename** und die Setter-Methode wird berechnet, indem vorangestellt wird das Wort "festlegen", aktivieren den ersten Buchstaben des der **Basename** in Großbuchstaben und den Selektor dauern, wodurch ein Argument. Dies bedeutet, dass `[Export ("label")]` angewendet auf eine Eigenschaft bindet tatsächlich "Label" und "SetLabel:" Objective-C-Methoden.

In einigen Fällen die Objective-C-Eigenschaften führen Sie das oben beschriebene Muster nicht, und der Name lautet manuell überschrieben. In diesen Fällen können Sie steuern, die Möglichkeit, dass die Bindung, mithilfe generiert wird der [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) Attribut für die Getter oder Setter, zum Beispiel:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Dieser dann bindet "IsMenuVisible" und "SetMenuVisible:". Optional kann eine Eigenschaft gebunden werden, das mit der folgenden Syntax:

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

In den Getter und Setter explizit definiert sind wie in der `name` und `setName` Bindungen oben.

Neben der Unterstützung für statische Eigenschaften, die mit [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), versehen Sie threadstatische Eigenschaften mit [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), beispielsweise:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Wie alle Methoden ermöglichen es einige Parameter mit gekennzeichnet wird [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), können Sie anwenden [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) auf eine Eigenschaft, um anzugeben, dass Null ist ein gültiger Wert für die Eigenschaft, beispielsweise:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

Die [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Parameter kann auch direkt auf den Setter angegeben werden:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Vorbehalte Binden von benutzerdefinierten Steuerelementen

Die folgenden Einschränkungen zu sollten berücksichtigt werden, wenn Sie die Bindung für ein benutzerdefiniertes Steuerelement einrichten:

1. **Bindungseigenschaften muss statisch sein** - beim Binden von Eigenschaften, definieren die [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) -Attribut muss verwendet werden.
 2. **Eigenschaftsnamen müssen exakt übereinstimmen** -Name zum Binden Sie die Eigenschaft muss der Name der Eigenschaft in das benutzerdefinierte Steuerelement genau entsprechen.
3. **Eigenschaftstypen müssen genau übereinstimmen** -der Variablentyp verwendet, um die Eigenschaft binden muss den Typ der Eigenschaft in das benutzerdefinierte Steuerelement genau entsprechen.
4. **Haltepunkte und den Getter/Setter** - Haltepunkte platziert, in der Getter oder Setter-Methode der Eigenschaft werden nie erreicht werden.
5. **Beachten Sie Rückrufe** -müssen Sie Beobachtung Rückrufe zu verwenden, um die Änderungen in die Eigenschaftswerte von benutzerdefinierten Steuerelementen benachrichtigt zu werden.

Keines der oben aufgeführten Vorsichtsmaßnahmen beobachten können in der Bindung automatisch zur Laufzeit fehlschlagen kommen.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Änderbare Objective-C-Muster und Eigenschaften

Objective-C-Frameworks verwenden Redensart, einige Klassen mit einer änderbaren Unterklasse unveränderlich. Z. B. `NSString` ist die Version der unveränderliche während `NSMutableString` ist der Unterklasse, die Mutation ermöglicht.

In diesen Klassen ist es üblich, die unveränderliche Basisklasse enthalten Eigenschaften, die mit einer Getter-Methode, aber keine Set-Methode. Und für die änderbare Version den Setter einzuführen. Da dies nicht wirklich mit c# möglich ist, mussten wir dieses Technik Redensart zuzuordnen, die mit c# funktionieren würden.

Die Möglichkeit, ist dies in c# zugeordnet ist sowohl der Getter und Setter-Methode der Basisklasse hinzufügen, aber das Kennzeichnen des Setters für eine mit einem [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) Attribut.

Klicken Sie dann auf die Unterklasse die änderbare, verwenden Sie die [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) Attribut für die Eigenschaft, um sicherzustellen, dass die Eigenschaft tatsächlich das übergeordnete Verhalten außer Kraft gesetzt wird.

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
-  `Foo (NSCoder)`: der Konstruktor verwendet wird, während der Deserialisierung von NIB-Dateien (ordnet Objective-C "InitWithCoder:" Konstruktor).
-  `Foo (IntPtr handle)`: der Konstruktor für das Handle basierende erstellen, dies ist von der Laufzeit aufgerufen, wenn die Laufzeit ein verwaltetes Objekt aus einem nicht verwalteten Objekt verfügbar zu machen muss.
-  `Foo (NSEmptyFlag)`: Dies wird von abgeleiteten Klassen verwendet, um zu verhindern, dass doppelte Initialisierung.

Konstruktoren, die Sie definieren, müssen sie mit der folgenden Signatur innerhalb der Schnittstellendefinition deklariert werden: Geben sie zurück ein `IntPtr` Wert und der Name der Methode sollten Konstruktor sein. Z. B. zum Binden der `initWithFrame:` Konstruktor, was Sie verwenden würden:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Binden von Protokollen

In der API-Design-Dokument im Abschnitt beschriebenen [diskutieren, Modelle und Protokolle](~/ios/internals/api-design/index.md#Models), Xamarin.iOS ordnet die Objective-C-Protokolle in Klassen, die mit gekennzeichnet wurde, haben die [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) Das Attribut. Dies wird normalerweise verwendet, bei der Implementierung von Klassen für Objective-C-Delegat.

Der große Unterschied zwischen einer regulären gebundene Klasse und einer Delegatklasse ist, dass die Delegate-Klasse eine oder mehrere optionale Methoden haben kann.

Angenommen Sie haben die `UIKit` Klasse `UIAccelerometerDelegate`, dies ist, wie er im Xamarin.iOS gebunden ist:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Da dies eine optionale Methode auf der Definition ist `UIAccelerometerDelegate` ist es nichts zu tun. Wenn eine erforderliche Methode auf dem Protokoll wurde, Sie sollten jedoch hinzufügen, die [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) -Attribut zur Methode. Dies zwingt den Benutzer der Implementierung auf tatsächlich einen Text für die Methode anzugeben.

Protokolle werden im Allgemeinen in Klassen verwendet, die auf Nachrichten reagieren. Dies erfolgt normalerweise in Objective-C durch Zuweisen zur Eigenschaft "Delegat" eine Instanz eines Objekts, das auf die Methoden im Protokoll reagiert.

Normalerweise in Xamarin.iOS wird zur Unterstützung von sowohl des Objective-C lose verbundener Stils denen eine beliebige Instanz von einer `NSObject` kann zugewiesen werden ebenfalls verfügbar gemacht und an den Delegaten eine stark typisierte Version zu. Aus diesem Grund wird in der Regel bieten eine `Delegate` -Eigenschaft, die stark typisiert ist und ein `WeakDelegate` , lose typisiert ist. Wir in der Regel binden Sie die lose typisierten Version mit [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), und wir verwenden die [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) Attribut, das stark typisierte Version bereitzustellen.

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

Beginnend mit MonoTouch 7.0 einen neuen und verbesserten protokollbindung-Funktionalität wurde integriert.  Diese neue Unterstützung ist es leichter, Objective-C-Idiome Übernahme eine oder mehrere Protokolle in einer bestimmten Klasse zu verwenden.

Für jede Protokolldefinition `MyProtocol` in Objective-C, es ist jetzt ein `IMyProtocol` Schnittstelle, die die erforderlichen Methoden aus dem Protokoll aufgeführt sowie eine Erweiterungsklasse an, die die optionalen Methoden bereitstellt.  Den oben genannten in Kombination mit der neuen Unterstützung für in Xamarin Studio-Editor-Entwickler Protokollmethoden implementieren, ohne die separaten Unterklassen des vorherigen abstrakte Modellklassen verwenden kann.

Jede Definition, enthält die [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) Attribut generiert tatsächlich drei unterstützende Klassen, die die Möglichkeit, die Sie Protokolle verwenden erheblich zu verbessern:

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

Die **klassenimplementierung** bietet eine vollständige abstrakte Klasse, dass Sie einzelne Methoden überschreiben und vollständige typsicherheit abrufen können.  Aufgrund von c# unterstützt keine mehrfachvererbung, existieren Szenarien, in denen Sie möglicherweise, eine andere Basisklasse haben muss, jedoch noch eine Schnittstelle implementieren, wo sollen jedoch die

Die generierte **-Schnittstellendefinition** eingeht.  Es ist eine Schnittstelle, die erforderlichen Methoden aus dem Protokoll aufweist.  Dadurch können Entwickler, die Ihr Protokoll zum lediglich Implementieren der Schnittstelle implementieren möchten.  Die Laufzeit wird den Typ als Einführung des Protokolls automatisch registriert.

Beachten Sie, dass die Schnittstelle nur die erforderlichen Methoden aufgelistet und die optionalen Methoden macht.  Dies bedeutet, dass Klassen, die das Protokoll übernimmt vollständige typüberprüfung für die erforderlichen Methoden erhalten, müssen aber schwache Typisierung verwenden (manuell mit [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribute und die Signatur) für das optionale Protokollmethoden.

Zum erleichtern die eine API nutzen, die Protokolle verwendet, wird das Tool für die Bindung auch eine Erweiterungen Methode Klasse erstellen, die alle die optionalen Methoden verfügbar macht.  Dies bedeutet, dass so lange Sie eine API nutzen, um Protokolle zu behandeln, als dass alle Methoden werden kann.

Wenn Sie die Definitionen des Protokolls in Ihrer API verwenden möchten, müssen Sie das Gerüst eines leere Schnittstellen definieren Sie in der API schreiben.  Wenn Sie die MyProtocol in einer API verwenden möchten, müssen Sie dazu:

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

Oben ist erforderlich, da zur Bindungszeit der `IMyProtocol` ist nicht vorhanden, d. h. Warum müssen Sie eine leere Schnittstelle bereitstellen.

#### <a name="adopting-protocol-generated-interfaces"></a>Protokoll generierten Schnittstellen eingeführt

Wenn Sie eine der Schnittstellen, die für die Protokolle, wie folgt generiert implementieren:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Die Implementierung für die Schnittstellenmethoden ruft automatisch mit dem richtigen Namen exportiert, damit sie dies entspricht:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Es spielt keine Rolle, wenn die Schnittstelle, implizit oder explizit implementiert wird.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Bindungserweiterungen-Klasse

In Objective-C ist es möglich, neue Methoden, wie Willen # Erweiterungsmethoden Klassen erweitern. Wenn eine der folgenden Methoden vorhanden ist, können Sie die [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Attribut, um die Methode als die Empfänger der Nachricht Objective-C-flag.

Im Xamarin.iOS gebunden wir z. B. die Erweiterungsmethoden, die auf definiert `NSString` Wenn `UIKit` wird importiert als Methoden in der `NSStringDrawingExtensions`, wie folgt:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Argumentlisten Objective-C-Bindung

Objective-C unterstützt Variadic-Argumente. Zum Beispiel:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Zum Aufrufen dieser Methode in c# sollten Sie zum Erstellen einer Signatur wie folgt:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Damit wird die Methode als interne, Ausblenden von der oben genannten-API von Benutzern, aber in der Bibliothek eine Offenlegung deklariert. Anschließend können Sie eine Methode wie folgt schreiben:

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

In einigen Fällen möchten den Zugriff auf öffentliche Felder, die in einer Bibliothek deklariert wurden.

In der Regel enthalten diese Felder Zeichenfolgen oder Ganzzahlen Werte, die auf die verwiesen werden müssen. Sie werden häufig verwendet, als Zeichenfolge, die eine bestimmte Benachrichtigung darstellt und als Schlüssel im Wörterbuch.

Um ein Feld zu binden, Hinzufügen einer Eigenschaft zu Ihrer Definition Schnittstellendatei und ergänzen Sie die Eigenschaft mit dem [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Attribut. Dieses Attribut nimmt einen Parameter: den C-Namen des Symbols für die Suche. Zum Beispiel:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Sollen die verschiedenen Felder in einer statischen Klasse zu umschließen, die nicht von abgeleitet ist `NSObject`, können Sie die [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) -Attribut für die Klasse, wie folgt:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Der oben genannten generiert eine `LonelyClass` nicht von abgeleitet ist `NSObject` und enthält eine Bindung an die `NSSomeEventNotification` 
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

Neben dem Feldnamen des nativen können Sie den Bibliotheksnamen angeben, in dem das Feld geschützt durch Übergeben der Bibliotheksname:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Wenn Sie statisch verknüpft sind, ist keine Bibliothek zum Binden an, daher Sie verwenden müssen die `__Internal` Name:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Binden von Enumerationen

Sie können hinzufügen `enum` direkt in eine Bindung zu Dateien erleichtert es, in der API-Definitionen - einsetzen, ohne Verwendung einer anderen Quelldatei (die in den Bindungen und fertigen Projekt kompiliert werden muss).

Beispiel:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Es ist auch möglich, erstellen Sie eine eigene Enumerationen ersetzen `NSString` Konstanten. In diesem Fall wird von der Generator **automatisch** die Methoden für das Konvertieren von Werten für Enumerationen und NSString Konstanten für Sie erstellen.

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

Im obigen Beispiel könnten Sie ergänzen entscheiden `void Perform (NSString mode);` mit einem [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut. Dadurch werden **ausblenden** die Konstante-basierte API aus Ihren Consumern Bindung.

Jedoch dadurch eingeschränkt würden, Erstellen von Unterklassen für den Typ als alternative verwendet der nützlicher API eine [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) Attribut. Die generierten Methoden sind nicht `virtual`, d. h. Sie wird nicht mehr auf diese - überschreiben, oder nicht kann, gute Wahl.

Eine Alternative besteht darin, markieren Sie die ursprüngliche `NSString`-basierte Definition als `[Protected]`. Dadurch Unterklassen funktioniert im Bedarfsfall, und die wrap'ed Version weiterhin funktioniert und rufen Sie die Methode außer Kraft gesetzte.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Binden von `NSValue`, `NSNumber`, und `NSString` auf eine bessere Typ

Die [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Attribut ermöglicht die Bindung `NSNumber`, `NSValue` und `NSString`(Enumerationen) in genauere C#-Typen. Das Attribut kann verwendet werden, erstellen Sie eine bessere und genauere .NET API gegenüber einer systemeigenen API.

Sie können Methoden (Rückgabewert), Parameter und Eigenschaften mit ergänzen [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Die einzige Einschränkung besteht Ihr Mitglied ist, **muss nicht** werden innerhalb einer [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) oder [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) Schnittstelle.

Zum Beispiel:

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

Intern werden wir führen die `bool?`  <->  `NSNumber` und `CGRect`  <->  `NSValue` Konvertierungen.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) unterstützt auch Arrays mit `NSNumber` `NSValue` und `NSString`(Enumerationen).

Zum Beispiel:

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

`CAScroll` ist eine `NSString` Enum gesichert werden wir das Recht fetch `NSString` Wert und die Konvertierung vom Typ behandeln.

Finden Sie unter der [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Dokumentation zur Konvertierung von unterstützten Datentypen finden Sie unter.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Binden von Benachrichtigungen

Benachrichtigungen sind Meldungen, die an gesendet werden die `NSNotificationCenter.DefaultCenter` und dienen als Mechanismus zum Senden von Nachrichten von einem Teil der Anwendung in eine andere. Entwickler Abonnieren von Benachrichtigungen, die in der Regel mithilfe der [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)des [Fehler bei AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) Methode. Wenn eine Anwendung eine Nachricht an die mitteilungszentrale zurückgesendet, in der Regel enthält eine Nutzlast in gespeicherten der [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) Wörterbuch. Dieses Wörterbuch ist schwach typisiert und Abrufen von Informationen aus, es ist fehleranfällig, wie in der Regel müssen Benutzer in der Dokumentation zu lesen, welche Schlüssel stehen für das Wörterbuch und Typen der Werte, die im Wörterbuch gespeichert werden können. Das Vorhandensein von Schlüsseln in einigen Fällen dient als auch ein boolescher Wert.

Der Xamarin.iOS Bindung-Generator bietet Unterstützung für Entwickler, um Benachrichtigungen zu binden. Zu diesem Zweck legen Sie die [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) Attribut auf eine Eigenschaft, die auch wurde mit markiert ein [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Eigenschaft (es kann öffentlich oder privat sein).

Dieses Attribut kann verwendet werden, ohne Argumente für Benachrichtigungen, die keine Nutzlast enthalten, oder Sie können angeben, eine `System.Type` , die auf einer anderen Schnittstelle in der API-Definition, in der Regel verweist, mit dem Namen mit "EventArgs" endet. Der Generator wandeln der Schnittstelle in einer Klasse, Unterklassen `EventArgs` und schließt alle aufgeführten Eigenschaften. Die [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Attribut sollte in der EventArgs-Klasse verwendet werden, um den Namen des Schlüssels zum Nachschlagen der Objective-C-Wörterbuch, das den Wert abzurufen, aufzulisten.

Zum Beispiel:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Der obige Code generiert eine geschachtelte Klasse `MyClass.Notifications` mit den folgenden Methoden:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Benutzer Ihres Codes können dann problemlos Abonnieren von Benachrichtigungen, die veröffentlicht werden, um die [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) mithilfe von Code wie folgt:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Der zurückgegebene Wert von `ObserveDidStart` können verwendet werden, um problemlos den Empfang von Benachrichtigungen, wie folgt:

```csharp
token.Dispose ();
```

Oder Sie rufen [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) und übergeben Sie das Token. Wenn Ihre Benachrichtigung Parameter enthält, müssen Sie angeben, dass eine Hilfsprogramm `EventArgs` -Schnittstelle, wie folgt:

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

Der oben genannten generiert eine `MyScreenChangedEventArgs` -Klasse mit der `ScreenX` und `ScreenY` Eigenschaften, die die Daten aus abzurufen, werden der [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) Wörterbuch mit den Schlüsseln "ScreenXKey" und "ScreenYKey" bzw. und die richtigen Konvertierungen gelten. Die `[ProbePresence]` -Attributs für den Generator Prüfpunkt, wenn der Schlüssel, in festgelegt ist der `UserInfo`, anstatt zu versuchen, den Wert zu extrahieren. Dies ist für Fälle verwendet, in denen das Vorhandensein des Schlüssels der Wert (in der Regel für boolesche Werte) ist.

Dadurch können Sie Code wie folgt schreiben:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Binden von Kategorien

Kategorien sind ein Objective-C-Mechanismus verwendet, um den Satz von Methoden und Eigenschaften verfügbar, in einer Klasse zu erweitern.   In der Praxis, sie werden entweder Erweitern der Funktionalität von einer Basisklasse verwendet (z. B. `NSObject`) bei ein bestimmtes Frameworks in verknüpft ist (z. B. `UIKit`), machen ihre Methoden verfügbar, jedoch nur, wenn das neue Framework verknüpft ist.   In einigen Fällen werden sie zum Organisieren von Funktionen in einer Klasse nach Funktionalität.   Sie ähneln Willen C#-Erweiterungsmethoden. Dies ist eine Kategorie wie im Ziel-"c:" Suchen

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Im obigen Beispiel wenn finden Sie auf eine Bibliothek erweitern Instanzen `UIView` mit der Methode `makeBackgroundRed`.

Um diese zu binden, können Sie die [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) Attribut in einer Schnittstellendefinition.  Bei Verwendung der [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) Attribut, die Bedeutung von der [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Attribut ändert sich von verwendet wird, an die Basisklasse erweitern, um den Typ erweitert werden.

Im folgenden gezeigt wie die `UIView` Erweiterungen gebunden und in C#-Erweiterungsmethoden umgewandelt werden:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Die oben genannten erstellt eine `MyUIViewExtension` einer Klasse enthält die `MakeBackgroundRed` Erweiterungsmethode.  Dies bedeutet, dass Sie nun "MakeBackgroundRed" aufrufen können, auf einem beliebigen `UIView` -Unterklasse, und Sie haben die gleiche Funktionalität erhalten Sie in Objective-C. In einigen Fällen werden die Kategorien nicht so erweitern Sie eine Systemklasse, aber zum Organisieren von Funktionen, die ausschließlich für die Ergänzung verwendet.  Und zwar so:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Sie können zwar die [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) Attribut für diesen Stil Decoration Deklarationen Sie möglicherweise auch nur hinzufügen oder alle der Klassendefinition.  Beide würde dasselbe erreichen:

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

Blöcke sind ein neues Konstrukt eingeführt, die von Apple, um die funktionelles Äquivalent anonymer Methoden in Objective-c zu versetzen Z. B. die `NSSet` Klasse macht jetzt diese Methode:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Die Beschreibung oben deklariert eine Methode namens `enumerateObjectsUsingBlock:` , akzeptiert ein Argument mit dem Namen `block`. Dieser Block ähnelt eine anonyme C#-Methode, da sie die Unterstützung für das Erfassen der aktuellen Umgebung (die "this" Zeiger, den Zugriff auf lokale Variablen und Parameter) hat. Die oben dargestellte Methode in `NSSet` Ruft den Block mit zwei Parametern ein `NSObject` (die `id obj` Teil) und einen Zeiger auf einen booleschen Wert (der `BOOL *stop`) Teil.

Um diese Art der API mit Btouch zu binden, müssen Sie zuerst die Typsignatur Block deklarieren, wie ein C#-delegieren und verweisen sie dann einen Einstiegspunkt API wie folgt:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Und jetzt Ihren Code die Funktion in c# aufrufen kann:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Falls gewünscht, können Sie auch Lambda-Ausdrücke verwenden:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Asynchrone Methoden

Der Generator Bindung kann eine bestimmten Form von Methoden in Async-Methoden aktivieren (Methoden, die eine Aufgabe oder eine Aufgabe zurückgeben&lt;T&gt;).

Sie können die [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) Attribut auf Methoden, die "void" zurückgeben und deren letzte Argument ist ein Rückruf.  Wenn Sie diese an eine Methode angewendet wird, generiert der Bindung-Generator eine Version dieser Methode mit dem Suffix `Async`.  Wenn der Rückruf keine Parameter akzeptiert, wird der Rückgabewert eine `Task`, wenn der Rückruf auf einen Parameter verwendet, wird das Ergebnis sein eine `Task<T>`.  Wenn der Rückruf mehrere Parameter akzeptiert, legen Sie die `ResultType` oder `ResultTypeName` die gewünschten Namen des generierten Typs an, der alle Eigenschaften enthalten soll.

Beispiel:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Der obige Code generiert sowohl die LoadFile-Methode sowie:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Einbringen starke Typen für unsichere NSDictionary-Parameter

In vielen Stellen in der Objective-C-API sind Parameter übergeben, wie schwach typisierte `NSDictionary` mit bestimmten Schlüssel und Werte, aber diese APIs sind fehleranfällig (Ungültiger Schlüssel übergeben werden können, und keine Warnungen erhalten, können Sie ungültige Werte übergeben, und erhalten Sie keine Warnungen) und frustrierend sein zu verwenden, wie sie erfordern, dass mehrere Roundtrips in der Dokumentation zu den möglichen Schlüsselnamen und Werte zu suchen.

Die Lösung besteht darin, eine stark typisierte Version bereitzustellen, die bereitstellt, die stark typisierte Version der API und im Hintergrund der zugrunde liegenden verschiedene Schlüssel und Werte zugeordnet.

Angenommen, wenn die Objective-C-API akzeptiert ein `NSDictionary` diese dokumentiert sind, dass Sie den Schlüssel erwartet `XyzVolumeKey` nimmt eine `NSNumber` mit einem Volume-Wert von 0,0 bis 1,0 und eine `XyzCaptionKey` , die eine Zeichenfolge akzeptiert, sollten die Benutzer Sie verwenden eine nice-API Das sieht wie folgt:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

Die `Volume` Eigenschaft wird als NULL-Werte zulassen "float", definiert, wie die Konvention in Objective-C nicht, dass diese Wörterbücher erfordert, den den Wert, damit es Szenarien gibt, in denen der Wert nicht festgelegt werden kann.

Zu diesem Zweck müssen Sie einige Schritte ausführen:

* Erstellen Sie eine stark typisierte Klasse, Unterklassen [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) und stellt die verschiedenen Getter und Setter für jede Eigenschaft.
* Deklarieren Sie Überladungen für die Methoden, denen `NSDictionary` die neue stark typisierte Version ausführen.

Sie können die stark typisierte Klasse entweder manuell erstellen oder verwenden Sie den Generator, um die Arbeit für Sie übernimmt.  Wir untersuchen zunächst, wie Sie dies manuell tun, damit Sie verstehen, was passiert, und klicken Sie dann automatisch erfolgt.

Müssen Sie dafür eine unterstützende Datei erstellen, geht nicht in den API-Vertrag.  Dies ist, müssen Sie beim Schreiben in die XyzOptions-Klasse zu erstellen:

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

Sie sollten dann eine Wrappermethode bereitstellen, die die allgemeine-API, über die Low-Level-API bereitstellt.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Wenn Ihre API nicht überschrieben werden muss, können Sie die NSDictionary-basierte API sicher ausblenden, indem die [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut.

Wie Sie sehen können, verwenden wir die [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) Attribut, um einen neuen Einstiegspunkt für die API-Oberfläche, und wir Oberfläche mithilfe unserer stark typisierte `XyzOptions` Klasse.  Die Wrappermethode ermöglicht auch Null übergeben wird.

Jetzt ist dabei, in denen wir nicht erwähnt, wenn die `XyzOptionsKeys` Werte stammt.  Sie würden die Schlüssel, die eine API in einer statischen Klasse wie Flächen in der Regel gruppieren `XyzOptionsKeys`, wie folgt:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Betrachten Sie wir die automatische Unterstützung für das Erstellen dieser stark typisierte Wörterbücher aus.  Dies vermeidet ausreichend der Textbaustein, und Sie können das Wörterbuch definieren, direkt in der API-Vertrag, anstatt von einer externen Datei.

Um ein stark typisiertes Wörterbuch zu erstellen, führen Sie eine Schnittstelle in Ihrer API und ergänzen mit dem [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Attribut.  Dies weist dem Generator an, dass eine Klasse mit dem gleichen Namen wie Ihre Schnittstelle erstellt werden sollte, die abgeleitet wird `DictionaryContainer` und stellen sichere typisierte Accessoren für sie bereit.

Die [ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Attribut nimmt einen Parameter, dies ist der Name der statischen Klasse, die die Wörterbuchschlüssel enthält.  Anschließend wird jede Eigenschaft der Schnittstelle eine stark typisierte Accessor sein.  Standardmäßig wird der Code den Namen der Eigenschaft mit dem Suffix "Schlüssel" in die statische Klasse verwenden, zum Erstellen des Accessors.

Dies bedeutet, dass eine externe Datei oder Getter und Setter für jede Eigenschaft manuell erstellen noch die Schlüssel manuell suchen müssen die stark typisierte Accessor erstellen nicht mehr benötigt werden selbst.

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

Für den Fall, dass Sie Sie in verweisen müssen Ihre `XyzOption` Mitglieder ein anderes Feld (also nicht den Namen der Eigenschaft mit dem Suffix `Key`), können Sie die Eigenschaft mit dem ergänzen ein [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut mit dem Namen, den Sie verwendet werden soll.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Datentypzuordnungen

Dieser Abschnitt behandelt wie Objective-C-Typen für C#-Typen zugeordnet werden.

<a name="Simple_Types" />

### <a name="simple-types"></a>Einfache Typen

Die folgende Tabelle zeigt, wie die Typen aus dem Objective-C und CocoaTouch World Xamarin.iOS weltweit zugeordnet werden sollen:

|Objective-C-Typname|Xamarin.iOS einheitliche API-Typ|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([Bindung NSString weitere](~/ios/internals/api-design/nsstring.md))|`string`|
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

Die Laufzeit Xamarin.iOS automatisch übernimmt die C#-Arrays zum Konvertieren `NSArrays` und auf diese Weise der Konvertierung zurück, z. B. die imaginäre Objective-C-Methode gibt ein `NSArray` von `UIViews`:

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

Die Idee ist eine stark typisierte C#-Array verwenden, wie dies die IDE ordnungsgemäße codevervollständigung, mit dem tatsächlichen Typ bereitstellen, ohne den Benutzer zu erraten, oder suchen Sie die Dokumentation zu den tatsächlichen Typ im Array enthaltenen Objekte zwingen zugelassen wird.

In Fällen, in denen Sie nach unten der tatsächliche am stärksten abgeleitete Typ im Array enthalten nicht verfolgen können, können Sie `NSObject []` als Rückgabewert.

<a name="Selectors" />

### <a name="selectors"></a>Selektoren

Selektoren angezeigt werden, auf die Objective-C-API wie den Typ spezielle `SEL`. Beim Binden einer Auswahl, würden Sie den Zuordnungstyp zu `ObjCRuntime.Selector`.  Selektoren werden in der Regel in einer API mit sowohl ein Objekt, das Zielobjekt und ein Selektor für die aufzurufende in das Zielobjekt verfügbar gemacht. Entspricht der C#-Delegat beide im Grunde bereitstellen: etwas, das sowohl für die aufzurufende Methode als auch das Objekt zum Aufrufen der Methode in kapselt.

Dies ist die Bindung aussieht:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

Und dies ist wie die Methode in der Regel in einer Anwendung verwendet werden würde:

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

Um die Bindung für C#-Entwickler nützlicher zu gestalten, Sie in der Regel werden eine Methode bereitstellen, die akzeptiert ein `NSAction` -Parameter, der C#-Delegaten und Lambda-Ausdrücke anstelle von verwendet werden soll, können die `Target+Selector`. Dazu würden Sie in der Regel Ausblenden der `SetTarget` Methode kennzeichnen Sie ihn mit einer [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut aus, und klicken Sie dann Sie würden verfügbar zu machen eine neue Hilfsmethode wie folgt:

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

Wenn Sie eine Methode, die Bindung sind ein `NSString`, können Sie, dass mit einem C#-Zeichenfolgen-Datentyp, beide Komponenten auf Typen und die Parameter zurückgeben ersetzen.

Der einzige Fall, wenn Sie verwenden möchten, können ein `NSString` direkt ist, wenn die Zeichenfolge als Token verwendet wird. Weitere Informationen zu Zeichenfolgen und `NSString`erhalten Sie unter der [API-Entwurf auf NSString](~/ios/internals/api-design/nsstring.md) Dokument.

In einigen seltenen Fällen möglicherweise eine API verfügbar. eine C-ähnlichen Zeichenfolge (`char *`) anstelle einer Objective-C-Zeichenfolge (`NSString *`). In diesen Fällen können Sie den Parameter mit versehen der [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) Attribut.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>Out / Ref-Parameter

Einige APIs Rückgabewerte in ihren Parametern oder Parameter als Verweis übergeben.

In der Regel sieht die Signatur:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

Das erste Beispiel zeigt eine allgemeine Objective-C-Idiom zum Zurückgeben von Fehlercodes, ein Zeiger auf ein `NSError` Zeiger übergeben wird, und bei der Rückgabe wird der Wert festgelegt.   Die zweite Methode veranschaulicht, wie eine Objective-C-Methode kann ein Objekt annehmen und seinen Inhalt zu ändern.   Dies wäre ein Durchlauf durch Verweis, anstatt eine reine Ausgabewert.

Eine Bindung würde wie folgt aussehen:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Speicher-Management-Attribute

Bei Verwendung der [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) -Attribut, und Sie werden Daten, die von der aufgerufenen Methode beibehalten werden bestanden, können Sie die Argument-Semantik angeben, indem Sie z. B. als zweiten Parameter übergeben:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Die oben aufgeführten würde den Wert mit der Semantik "Beibehalten" kennzeichnen. Die Semantik verfügbar sind:

-  Zuweisen
-  Kopieren
-  Beibehalten

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Stilrichtlinien

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>Mithilfe von [interne]

Sie können die [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) Attribut um eine Methode aus der öffentlichen API auszublenden. Möglicherweise möchten dies in Fällen, in denen die verfügbar gemachten API zu auf niedriger Ebene ist und Sie eine allgemeine Implementierung in einer separaten Datei auf der Grundlage dieser Methode bereitstellen möchten.

Sie können dies auch verwenden, wenn in Einschränkungen in der Bindung Generator ausführen, z. B. einige erweiterten Szenarien ist zur Verfügung stellt Typen, die nicht gebunden sind und Sie auf eigene Weise binden möchten und diese Typen selbst auf eigene Weise umbrochen werden soll.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Rückrufe und Ereignishandler

Objective-C-Klassen in der Regel Senden von Benachrichtigungen oder Anfordern von Informationen durch Senden einer Nachricht auf eine Delegatklasse (Objective-C-Delegaten).

Dieses Modell kann während vollständig unterstützt und Diagnoseinformationen werden von Xamarin.iOS manchmal mühselig. Xamarin.iOS verfügbar macht, die c#-Ereignismuster und eine Methode Rückruf-System für die Klasse, die in diesen Situationen verwendet werden kann. Dies ermöglicht Code wie folgt ausgeführt:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Die Bindung-Generator ist der reduziert der Menge der Eingabe erforderlich, um das Objective-C-Muster in das C#-Muster zuordnen kann.

Starten mit Xamarin.iOS 1.4 wird möglicherweise auch festlegen, dass den Generator Erzeugen von Bindungen für eine bestimmte Objective-C-Delegaten und der Delegat als C#-Ereignisse und Eigenschaften auf dem Host verfügbar machen.

Es gibt zwei Klassen in diesem Prozess Beteiligten, die Hostklasse ist dasjenige, das derzeit ausgibt, Ereignisse und sendet diese in die `Delegate` oder `WeakDelegate` und die tatsächliche Delegate-Klasse.

Beachten Sie die folgenden Setup:

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

Die Klasse umschließen müssen Sie folgende Aktionen ausführen:

-  In Ihrer Hostklasse hinzufügen, um Ihre [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   die Deklaration der Typ, der agiert als seines Delegaten und dem C#-Namen, den Sie verfügbar gemacht. In unserem oben genannten Beispiel sind `typeof (MyClassDelegate)` und `WeakDelegate` bzw.
-  In der Delegatklasse auf jede Methode, die mehr als zwei Parameter verfügt, müssen Sie den Typ angeben, den Sie für die automatisch generierten EventArgs-Klasse verwenden möchten.

Der Generator für die Bindung ist nicht nur ein einzelnes Ereignis Ziel umschließen beschränkt, es ist möglich, dass einige Objective-C-Klassen zum Ausgeben von Nachrichten an mehrere zu delegieren, daher müssen Sie Arrays zur Unterstützung dieses Setup enthalten. Die meisten Installationen nicht erforderlich, aber der Generator ist bereit, um die Fälle zu unterstützen.

Der resultierende Code werden verwendet:

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

Die `EventArgs` wird verwendet, um den Namen der angeben der `EventArgs` zu generierenden Klasse. Verwenden Sie eine Signatur (in diesem Beispiel wird die `EventArgs` enthält eine `With` Eigenschaft des Typs Nint).

Mit der obigen Definitionen liefert der Generator folgende Ereignis in das generierte MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Daher können Sie nun den Code wie folgt:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Rückrufe Verhalten sich genauso wie Ereignisaufrufe, der Unterschied besteht darin, anstatt mehrere potenzielle Abonnenten (z. B. können mehrere Methoden in Verknüpfen einer `Clicked` Ereignis oder eine `DownloadFinished` Ereignis) Rückrufe können nur einen einzelnen Abonnenten verfügen.

Der Prozess ist identisch, der einzige Unterschied ist, anstatt das Verfügbarmachen von den Namen des der `EventArgs` Klasse, die generiert werden, der EventArgs tatsächlich wird verwendet, um die resultierende C#-Delegat Namen geben.

Wenn die Methode in die Delegate-Klasse einen Wert zurückgibt, ordnet der Bindung Generator dies in einer Delegatenmethode in der übergeordneten Klasse anstelle eines Ereignisses. In diesen Fällen müssen Sie den Standardwert bereitstellen, der von der Methode zurückgegeben werden soll, wenn der Benutzer nicht einbinden wird an dem Delegaten. Verwenden Sie dazu die [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) oder [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) Attribute.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) wird eine hartcodierung einen Rückgabewert während [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) wird verwendet, um anzugeben, welche Eingabeargument zurückgegeben werden.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Enumerationen und Basistypen

Sie können auch verweisen Enumerationen oder die Basistypen, die vom System Btouch Interface Definition nicht direkt unterstützt werden. Zu diesem Zweck die Enumerationen und die grundlegenden Typen in einer separaten Datei abgelegt, und schließen Sie diese als Teil eines die zusätzlichen Dateien, die Sie Btouch bereitstellen.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Verknüpfen die Abhängigkeiten

Wenn Sie APIs, die nicht Teil der Anwendung sind binden, müssen Sie sicherstellen, dass die ausführbare Datei für diese Bibliotheken verknüpft ist.

Sie Xamarin.iOS darüber informieren, wie Ihre Bibliotheken verknüpfen müssen, können dazu entweder durch Ändern der Buildkonfiguration zum Aufrufen von der `mtouch` Befehl mit einigen zusätzlichen build Argumente, die angeben, wie zur Verknüpfung mit den neuen Bibliotheken, die mit der "-Gcc_flags"-Option gefolgt von einer Zeichenfolge in Anführungszeichen, die alle zusätzlichen Bibliotheken enthält, die für das Programm, wie dies erforderlich sind:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Im obigen Beispiel wird verknüpfen `libMyLibrary.a`, `libSystemLibrary.dylib` und `CFNetwork` Framework-Klassenbibliothek in die endgültige ausführbare Datei.

Oder Sie können nutzen auf Assemblyebene [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), die Sie in Ihrem Vertrag einbetten können (z. B. `AssemblyInfo.cs`).
Bei Verwendung der [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), müssen Sie die systemeigene Bibliothek, die zum Zeitpunkt verfügbar haben, Sie stellen die Bindung, da dies die systemeigene Bibliothek mit der Anwendung eingebettet wird. Zum Beispiel:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Sie vielleicht, warum müssen Sie `-force_load` Befehl und der Grund ist, dass die ObjC - flag zwar kompiliert den Code in behält nicht die Metadaten erforderlich, um Kategorien (die Linker-Compiler totem Code für Eliminierung von Duplikaten entfernt) unterstützen die zur Laufzeit für Xamarin.iOS benötigen.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Unterstützte Verweise

Einige vorübergehende Objekte wie Warnung Feldern und Aktion Blätter mühsam Nachverfolgen für Entwickler von der Bindung-Generator kann identifizieren und etwas hier.

Beispielsweise mussten Sie eine Klasse, die eine Meldung angezeigt, und klicken Sie dann generiert eine `Done` Ereignis, die herkömmlichen Verfahren zur Verarbeitung von dies wäre:

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

Im vorangehenden Szenario muss der Entwickler halten Sie dem Verweis auf das Objekt selbst und die entweder Speicherverlust oder deaktivieren Sie den Verweis für das Feld auf einem eigenen aktiv.  Bei der Bindungscode, der Generator unterstützt Keeping Nachverfolgen des Verweises für Sie und deaktivieren, wenn eine spezielle Methode aufgerufen wird, der obige Code würde dann sicherungsfestlegungen:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Beachten Sie, wie es nicht mehr erforderlich ist, der Variable in einer Instanz zu halten, dass er mit einer lokalen Variablen funktioniert und dass es nicht erforderlich, um den Verweis zu löschen, wenn das Objekt nicht mehr aktiv ist.

Davon profitieren, müssen Ihre Klasse eine Eigenschaftengruppe Ereignisse in der [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) Deklaration und auch die `KeepUntilRef` Variable festgelegt wird, auf den Namen der Methode, die aufgerufen wird, wenn das Objekt wie z. B. seine Arbeit abgeschlossen hat Dies:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Erben von Protokollen

Zum Zeitpunkt der Xamarin.iOS v3. 2, wir unterstützen erben von Protokollen, die mit markiert wurden die [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) Eigenschaft. Dies eignet sich für bestimmte API-Muster, wie z. B. im `MapKit` , in dem die `MKOverlay` Protokoll, erbt die `MKAnnotation` -Protokolls und wird durch eine Reihe von Klassen, die von erben übernommen `NSObject`.

In der Vergangenheit wir benötigt das Protokoll in jede Implementierung kopiert, aber in diesen Fällen nun wir können die `MKShape` Klasse erben die `MKOverlay` Protokoll, und es werden die erforderlichen Methoden automatisch generieren.

## <a name="related-links"></a>Verwandte Links

- [Bindung-Beispiel](https://developer.xamarin.com/samples/BindingSample/)
