---
title: Einführung in C# für Objective-C-Entwickler
description: Dieses Dokument enthält eine Einführung in C# für Objective-C-Entwickler. Dabei werden die beiden Sprachen verglichen und gegenübergestellt, indem unter anderem Protokolle und Schnittstellen, Kategorien und Erweiterungsmethoden, Frameworks und Assemblys und Selektoren und benannte Parameter untersucht werden.
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: a55d1d9848d3f1378ccbc4a24e1748eb146a6a35
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291987"
---
# <a name="c-primer-for-objective-c-developers"></a>Einführung in C# für Objective-C-Entwickler

_Xamarin.iOS ermöglicht, in C# geschriebenen plattformunabhängigen Code auf verschiedenen Plattformen gemeinsam zu nutzen. Bestehende iOS-Anwendungen möchten jedoch möglicherweise bereits erstellten Objective-C-Code nutzen. Dieser Artikel dient als kurze Einführung für Objective-C-Entwickler, die zu Xamarin und zur Sprache C# wechseln möchten._

iOS- und OS X-Anwendungen, die in Objective-C entwickelt wurden, können von Xamarin profitieren, indem C# an Stellen eingesetzt wird, an denen plattformspezifischer Code nicht erforderlich ist. Dadurch kann dieser Code auf Geräten verwendet werden, die nicht von Apple stammen. Elemente wie Webdienste, JSON- und XML-Analysen sowie benutzerdefinierte Algorithmen können dann plattformübergreifend verwendet werden.

Um die Vorteile von Xamarin unter Beibehaltung bestehender Objective-C-Ressourcen zu nutzen, können diese für C# in einer Technologie von Xamarin verfügbar gemacht werden, die als Bindungen bekannt ist. Diese hat den Zweck, den Objective-C-Code der verwalteten C#-Welt zugänglich zu machen. Auf Wunsch kann Code auch zeilenweise nach C# portiert werden. Unabhängig davon, ob der Bindungs- oder Portierungsansatz gewählt wird, sind Objective-C- und C#-Kenntnisse erforderlich, um den vorhandenen Objective-C-Code mit Xamarin.iOS effektiv zu nutzen.

## <a name="objective-c-interop"></a>Objective-C-Interop

Es gibt derzeit keinen unterstützten Mechanismus zur Erstellung einer Bibliothek in C# unter Verwendung von Xamarin.iOS, die in Objective-C aufgerufen werden kann. Der Hauptgrund dafür ist, dass neben der Bindung auch die Mono-Laufzeit benötigt wird. Sie können jedoch den größten Teil Ihrer Logik in Objective-C erstellen, einschließlich Benutzeroberflächen. Umschließen Sie zu diesem Zweck den Objective-C-Code in einer Bibliothek, und erstellen Sie eine Bindung damit. Xamarin.iOS ist für das Bootstrapping der Anwendung erforderlich (d. h., es muss den `Main`-Einstiegspunkt erstellen). Danach kann sich jede andere Logik in Objective-C befinden, die C# durch die Bindung (oder über P/Invoke) zur Verfügung gestellt wird. Auf diese Weise können Sie die plattformspezifische Logik in Objective-C beibehalten und die plattformunabhängigen Teile in C# entwickeln.

Dieser Artikel hebt einige wichtige Gemeinsamkeiten hervor und stellt einige Unterschiede in beiden Sprachen gegenüber. Diese gilt es bei der Umstellung auf C# mit Xamarin.iOS unabhängig davon zu beachten, ob eine Bindung an bestehenden Objective-C-Code oder dessen Portierung zu C# erfolgt.

Informationen zum Erstellen von Bindungen finden Sie in den anderen Dokumenten unter [Binden von Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Sprachenvergleich

Objective-C und C# sind sehr unterschiedliche Sprachen, sowohl syntaktisch als auch unter Laufzeitgesichtspunkten. Objective-C ist eine dynamische Sprache und verwendet ein Nachrichtenübergabeschema, während C# statisch typisiert ist. Syntaktisch gesehen ist Objective-C wie Smalltalk, während bei C# ein Großteil der grundlegenden Syntax von Java abgeleitet ist, obwohl diese Sprache in den letzten Jahren soweit gereift ist, dass sie viele über Java hinausgehende Funktionen bietet.

Dessen ungeachtet gibt es mehrere Sprachmerkmale von Objective-C und C#, die in ihrer Funktionsweise ähnlich sind. Bei der Erstellung einer Bindung zu Objective-C-Code aus C# oder bei Portierung von Objective-C nach C# ist es hilfreich, diese Ähnlichkeiten zu verstehen.

### <a name="protocols-vs-interfaces"></a>Protokolle im Vergleich zu Schnittstellen

Objective-C und C# sind Sprachen mit einfacher Vererbung. Beide Sprachen unterstützen jedoch die Implementierung mehrerer Schnittstellen in einer bestimmten Klasse. In Objective-C werden diese logischen Schnittstellen *Protokolle* genannt, während sie in C# *Schnittstellen* heißen. Implementierungstechnisch ist der Hauptunterschied zwischen einer C#-Schnittstelle und einem Objective-C-Protokoll, dass letzteres optionale Methoden haben kann. Weitere Informationen finden Sie im Artikel [Ereignisse, Delegaten und Protokolle](~/ios/app-fundamentals/delegates-protocols-and-events.md).

### <a name="categories-vs-extension-methods"></a>Kategorien im Vergleich zu Erweiterungsmethoden

Objective-C erlaubt das Hinzufügen von Methoden zu einer Klasse, für die Sie möglicherweise nicht über den Implementierungscode verfügen, mithilfe von *Kategorien*. In C# ist ein ähnliches Konzept in Form so genannter *Erweiterungsmethoden* verfügbar.

Mithilfe von Erweiterungsmethoden können Sie einer Klasse statische Methoden hinzufügen, wobei statische Methoden in C# analog zu Klassenmethoden in Objective-C sind. Zum Beispiel fügt der folgende Code der `UITextView`-Klasse eine Methode namens `ScrollToBottom` hinzu, die wiederum eine verwaltete Klasse ist, die an die Objective-C `UITextView`-Klasse aus UIKit gebunden ist:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Wenn dann eine Instanz von `UITextView` im Code erstellt wird, steht die Methode in der Liste für die automatische Vervollständigung zur Verfügung, wie unten gezeigt:

 ![](primer-images/01-extensionmethodintellisense.png "Die für AutoVervollständigen verfügbare Methode")

Beim Aufruf der Erweiterungsmethode wird die Instanz an das Argument übergeben, wie z. B. `textView` in diesem Beispiel.

### <a name="frameworks-vs-assemblies"></a>Frameworks im Vergleich zu Assemblys

Objective-C packt verwandte Klassen in speziellen Verzeichnissen, die als Frameworks bezeichnet werden. In C# und .NET werden hingegen Assemblys verwendet, um wiederverwendbare Bits von vorkompiliertem Code bereitzustellen. In Umgebungen außerhalb von iOS enthalten Assemblys Zwischensprachcode (Intermediate Language, IL), der Just-In-Time (JIT) zur Laufzeit kompiliert wird. Apple lässt jedoch in iOS-Anwendungen JIT nicht zu. Daher wird C#-Code, der auf iOS mit Xamarin abzielt, AOT-kompiliert (Ahead of time) und erzeugt eine einzelne ausführbare Unix-Datei zusammen mit Metadatendateien, die im Anwendungspaket enthalten sind.

### <a name="selectors-vs-named-parameters"></a>Selektoren im Vergleich zu benannten Parametern

Objective-C-Methoden schließen grundsätzlich Parameternamen in Selektoren ein. Zum Beispiel macht ein Selektor wie `AddCrayon:WithColor:` deutlich, was jeder Parameter bedeutet, wenn er im Code verwendet wird. C# unterstützt optional auch benannte Argumente.

Beispielsweise sieht ähnlicher Code in C# mit benannten Argumenten wie folgt aus:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

Obwohl in C# diese Unterstützung in Version 4.0 der Sprache hinzugefügt wurde, wird sie in der Praxis nicht sehr häufig verwendet. Wenn Sie jedoch in Ihrem Code explizit sein möchten, ist die Unterstützung dafür vorhanden.

### <a name="headers-and-namespaces"></a>Header und Namespaces

Da Objective-C eine Obermenge von C ist, verwendet Objective-C Header für öffentliche Deklarationen, die von der Implementierungsdatei getrennt sind. C# verwendet keine Headerdateien. Im Gegensatz zu Objective-C ist C#-Code in Namespaces enthalten. Wenn Sie Code, der in einem bestimmten Namespace verfügbar ist, einbinden möchten, fügen Sie entweder eine „using“-Anweisung am Anfang der Implementierungsdatei hinzu, oder qualifizieren Sie den Typ mit dem vollen Namespace.

Beispielsweise enthält der folgende Code den Namespace `UIKit`, wodurch jede Klasse in diesem Namespace der Implementierung zur Verfügung steht:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Außerdem legt das Schlüsselwort „namespace“ im obigen Code den Namespace fest, der für die Implementierungsdatei selbst verwendet wird. Wenn sich mehrere Implementierungsdateien denselben Namespace teilen, ist es nicht notwendig, den Namespace auch in eine „using“-Anweisung aufzunehmen, wie dies impliziert wird.

### <a name="properties"></a>Eigenschaften

Sowohl Objective-C als auch C# arbeiten mit dem Eigenschaftskonzept, um eine allgemeine Abstraktion rund um Accessormethoden zu ermöglichen. In Objective-C wird die Compileranweisung @property verwendet, um die Accessormethoden effektiv zu generieren. Im Gegensatz dazu bietet C# Unterstützung für Eigenschaften in der Programmiersprache selbst. Eine C#-Eigenschaft kann entweder mit einem längeren Stil, der auf ein Unterstützungsfeld zugreift, oder mit einer kürzeren, automatischen Eigenschaftssyntax implementiert werden, wie in den folgenden Beispielen gezeigt:

```csharp
// automatic property syntax
public string Name { get; set; }

// property implemented with a backing field
string address;
public string Address {
    get {
        // could add additional code here
        return address;
    }
    set {
        address = value;
    }
}
```

### <a name="static-keyword"></a>Schlüsselwort „static“

Das Schlüsselwort *static* hat in Objective-C und C# eine sehr unterschiedliche Bedeutung. In Objective-C werden statische Funktionen verwendet, um den Umfang einer Funktion auf die aktuelle Datei zu beschränken. In C# hingegen wird der Geltungsbereich durch die Schlüsselwörter *public*, *private* und *internal* bestimmt.

Wenn das Schlüsselwort „static“ auf eine Variable in Objective-C angewendet wird, behält die Variable ihren Wert funktionsaufrufübergreifend hinweg bei.

C# weist auch das Schlüsselwort „static“ auf. Wird es auf eine Methode angewendet, geschieht quasi dasselbe wie beim Modifizierer `+` in Objective-C. Es erstellt eine Klassenmethode. Ähnlich verhält es sich, wenn es auf andere Konstrukte wie Felder, Eigenschaften und Ereignisse angewendet wird. Es macht diese zu einem Teil des Typs, in dem sie deklariert sind, und nicht zu einer Instanz dieses Typs. Sie können auch eine statische Klasse erstellen, in der alle Methoden, die in der Klasse definiert sind, ebenfalls statisch sein müssen.

### <a name="nsarray-vs-list-initialization"></a>NSArray im Vergleich zu Listeninitialisierung

Objective-C enthält jetzt Literalsyntax für die Verwendung mit `NSArray`, was die Initialisierung erleichtert. C# weist jedoch einen umfangreicheren Typ namens `List` auf, der *generisch* ist, was bedeutet, dass der Typ, den die Liste enthält, durch den Code bereitgestellt werden kann, der die Liste erstellt (wie Vorlagen in C++). Darüber hinaus unterstützen Listen Syntax für die automatische Initialisierung, wie unten gezeigt:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Blöcke im Vergleich zu Lambda-Ausdrücke

Objective-C verwendet *Blöcke* zum Erstellen von Abschlüssen, wobei Sie eine Funktion inline erstellen können, die den Zustand, in dem sie eingeschlossen ist, nutzen kann. C# arbeitet mit einem ähnlichen Konzept, nämlich Lambda-Ausdrücken. In C# werden Lambdaausdrücke mit dem Operator `=>` erstellt, wie nachstehend gezeigt:

```csharp
(args) => {
    //  implementation code
};
```

Weitere Informationen zu Lambda-Ausdrücken finden Sie im [C#-Programmierhandbuch](https://msdn.microsoft.com/library/vstudio/bb397687.aspx) von Microsoft.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden eine Vielzahl von Sprachmerkmalen von Objective-C und C# verglichen. In einigen Fällen wurden analoge Merkmale hervorgehoben, die in beiden Sprachen vorhanden sind, wie z. B. Blöcke für Lambda-Ausdrücke und Kategorien für Erweiterungsmethoden. Darüber hinaus wurden Punkte unterschieden, an denen die Sprachen voneinander abweichen, wie z. B. Namespaces in C# und die Bedeutung des Schlüsselworts „static“.
