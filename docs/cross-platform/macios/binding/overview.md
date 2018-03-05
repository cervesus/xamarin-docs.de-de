---
title: "Übersicht"
description: Details der Funktionsweise des Bindungsvorgangs
ms.topic: article
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e5b0cd151d282869cc9272f5ea3b0a83a6d04369
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="overview"></a>Übersicht

_Details der Funktionsweise des Bindungsvorgangs_

Binden einer Objective-C-Bibliothek für die Verwendung mit Xamarin akzeptiert drei Schritte:

1. Schreiben Sie eine C#-"API-Definition" wird beschrieben, wie die systemeigene API wird verfügbar gemacht in .NET und wie die zugrunde liegenden Objective-c zugeordnet Dies erfolgt mithilfe von standardmäßigen c# erstellt, z. B. `interface` und verschiedene Bindung **Attribute** (finden Sie in diesem [einfaches Beispiel](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Einmal haben Sie "API-Definition" in c# geschrieben, kompilieren, um eine "Bindung"-Assembly zu erzeugen. Dies kann auf die [ **Befehlszeile** ](#commandline) oder über eine [ **bindungsprojekt** ](#bindingproject) in Visual Studio für Mac oder im Visual Studio.

3. "Bindung" Assembly wird dann in das Anwendungsprojekt Xamarin hinzugefügt, damit Sie die systemeigene Funktionalität, die mithilfe der API, die Sie definiert zugreifen können.
  Das bindungsprojekt ist vollständig unabhängig von Ihrer Anwendungsprojekte.

**Hinweis:** Schritt 1 kann automatisiert werden, mit Unterstützung des [ **Ziel Sharpie**](#objectivesharpie). Er überprüft die Objective-C-API und generiert eine vorgeschlagene c# "API-Definition". Sie können anpassen, die Dateien vom Ziel Sharpie erstellt und in einem bindungsprojekt (oder in der Befehlszeile) verwenden die bindungsassembly zu erstellen. Objektive Sharpie erstellt keine Bindungen selbst, sondern lediglich einem optionalen Teil eines größeren Prozesses.

Sie können auch weitere technische Details zu lesen [Funktionsweise](#howitworks), die hilft Ihnen, Ihre Bindungen zu schreiben.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Command Line-Bindungen

Sie können die `btouch-native` für Xamarin.iOS (oder `bmac-native` bei Verwendung von Xamarin.Mac) Bindungen direkt zu erstellen. Es funktioniert, indem Sie übergeben die C#-API-Definitionen, die Sie manuell erstellt haben (oder Ziel Sharpie) an das Tool über die Befehlszeile (`btouch-native` für iOS oder `bmac-native` für Mac).


Die allgemeine Syntax zum Aufrufen dieser Tools ist:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Der obige Befehl generiert die Datei `cocos2d.dll` im aktuellen Verzeichnis ab, und es enthält die vollständig gebundene Bibliothek, die Sie in Ihrem Projekt verwenden können. Dies ist das Tool, das Visual Studio für Mac verwendet wird, um die Bindungen zu erstellen, wenn Sie ein bindungsprojekt verwenden (beschrieben [unten](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Bindungsprojekt

Eine bindungsprojekt in Visual Studio für Mac oder Visual Studio (Visual Studio unterstützt nur Bindungen für iOS) erstellt werden kann und erleichtert es, bearbeiten und erstellen Sie die API-Definitionen für die Bindung (im Gegensatz zu über die Befehlszeile).

Führen Sie [Handbuch mit ersten Schritten](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) Informationen zum Erstellen und verwenden ein bindungsprojekt, um eine Bindung zu erstellen.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objektive Sharpie

Objektive Sharpie ist einer anderen, separaten Befehlszeilentool, mit denen mit den ersten Schritten zum Erstellen einer Bindung kann. Eine Bindung wird nicht von sich selbst erstellt, sondern Standardsite automatisch des ersten Schritts generieren Sie eine API-Definition für die systemeigene Zielbibliothek.

Lesen der [Ziel Sharpie Docs](~/cross-platform/macios/binding/objective-sharpie/index.md) erfahren Sie, wie zum Analysieren von systemeigenen Bibliotheken, systemeigene Frameworks und CocoaPods in API-Definitionen, die in den Bindungen erstellt werden können.

<a name="howitworks" />

# <a name="how-binding-works"></a>Funktionsweise der Bindung

Es ist möglich, verwenden Sie die [[registrieren]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Attribut [[Exportieren]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) -Attribut, und [manuelle Objective-C-Selektor-Aufruf](~/ios/internals/objective-c-selectors.md) zusammen, um manuell neu (zuvor binden ungebundene) Objective-C-Typen.

Suchen Sie zunächst einen Typ, den Sie binden möchten. Erläuterung Zwecke (und Einfachheit), wir außerdem binden Sie die [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) Typ (die bereits gebunden wurde [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); die Implementierung, die unten ist beispielsweise nur Zwecke).

Zweitens muss den C#-Typ erstellt werden. Wir möchten wahrscheinlich dadurch in einem anderen Namespace platziert; Da Objective-C-Namespaces nicht unterstützt, benötigen wir verwenden die `[Register]` Attribut, um den Typnamen zu ändern, die mit der Objective-C-Laufzeit Xamarin.iOS registriert werden. Der C#-Typ erben muss ebenfalls aus [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Im dritten, überprüfen Sie die Dokumentation für Objective-C und erstellen Sie [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) Instanzen für jede Auswahl, die Sie verwenden möchten. Platzieren Sie diese innerhalb der Klassendefinition:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Vierte, müssen der Typ Konstruktoren angeben. Sie *müssen* Verketten der Konstruktoraufruf an den Konstruktor der Basisklasse. Die `[Export]` Attribute ermöglichen Objective-C-Code zum Aufrufen von Konstruktoren mit dem angegebenen Selektor Namen:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Fünfter, stellen Sie in Schritt 3 für jede der Selektoren Methoden deklariert. Verwendet diese `objc_msgSend()` um die Auswahl für das systemeigene Objekt aufzurufen. Beachten Sie die Verwendung von [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) Konvertieren einer `IntPtr` in ein geeignetes typisiertes `NSObject` (Sub)-Typ. Wenn Sie die Methode aus Objective-C-Code, der Member aufgerufen werden soll *müssen* werden **virtuellen**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Gesamtbild:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

