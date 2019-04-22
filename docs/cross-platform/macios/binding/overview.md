---
title: Übersicht über die Objective-C-Bindungen
description: Dieses Dokument bietet einen Überblick über verschiedene Möglichkeiten zum Erstellen C# Bindungen für Objective-C-Code, einschließlich der Befehlszeilen-Bindungen, bindungsprojekte und Ziel Sharpie. Es wird auch erläutert, wie die Bindung funktioniert.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: asb3993
ms.author: amburns
ms.date: 11/25/2015
ms.openlocfilehash: d29239d986ebfe153381915dbe0f4bfbbe738007
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870338"
---
# <a name="overview-of-objective-c-bindings"></a>Übersicht über die Objective-C-Bindungen

_Details der Funktionsweise des Bindungsvorgangs_

Binden eine Objective-C-Bibliothek für die Verwendung mit Xamarin verwendet drei Schritte:

1. Schreiben einer C# "API-Definition" für beschrieben, wie die systemeigene API ist verfügbar gemacht in .NET, und wie sie die zugrunde liegenden Objective-c zugeordnet Dies erfolgt mit standardmäßigen C# Konstrukte wie `interface` und verschiedene Bindung **Attribute** (finden Sie in diesem [einfaches Beispiel](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Nachdem Sie die "API-Definition", im geschrieben haben C#, Sie kompilieren, um eine "Bindung"-Assembly zu erzeugen. Dies kann erfolgen auf der [ **Befehlszeile** ](#commandline) oder eine [ **bindungsprojekt** ](#bindingproject) in Visual Studio für Mac oder Visual Studio.

3. Die Assembly "Bindung" wird dann an Ihre Xamarin-Application-Projekt hinzugefügt, damit Sie die systemeigene Funktionalität, die mit der API, die Sie definiert zugreifen können.
  Das bindungsprojekt erfolgt unabhängig von der Ihre Anwendungsprojekte.

**HINWEIS:** Schritt 1 kann automatisiert werden, mit der Hilfe von [ **Ziel Sharpie**](#objectivesharpie). Er untersucht die Objective-C-API und generiert eine vorgeschlagene C# "API-Definition". Sie können anpassen, die Dateien vom Ziel Sharpie erstellt und sie in einem bindungsprojekt (oder in der Befehlszeile) die bindungsassembly zu erstellen. Objektive Sharpie erstellt keine Bindungen selbst, sondern lediglich ein optionaler Teil eines größeren Prozesses.

Lesen Sie auch weitere technische Details von [Funktionsweise](#howitworks), derer Sie Ihre Bindungen zu schreiben.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Befehls-Line-Bindungen

Sie können die `btouch-native` für Xamarin.iOS (oder `bmac-native` bei Verwendung von Xamarin.Mac) Bindungen direkt zu erstellen. Dies erfolgt durch das Übergeben der C# -API-Definitionen, die Sie manuell erstellt haben (oder mit der Ziel-Sharpie), das Befehlszeilentool (`btouch-native` für iOS oder `bmac-native` für Mac).


Die allgemeine Syntax zum Aufrufen dieser Tools ist:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Der obige Befehl generiert die Datei `cocos2d.dll` im aktuellen Verzeichnis, und es enthält die vollständig gebundene Bibliothek, die Sie in Ihrem Projekt verwenden können. Dies ist das Tool, das Visual Studio für Mac verwendet, um die Bindungen zu erstellen, bei der Verwendung von ein bindungsprojekt (beschrieben [unten](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Projekt wird gebunden

Ein bindungsprojekt kann in Visual Studio für Mac oder Visual Studio (Visual Studio unterstützt nur iOS-Bindungen) erstellt werden, und erleichtert es, bearbeiten und Erstellen von API-Definitionen für die Bindung (und nicht über die Befehlszeile).

Gehen Sie folgendermaßen vor [Handbuch mit ersten Schritten](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) Informationen zum Erstellen und verwenden Sie ein bindungsprojekt, um eine Bindung zu erstellen.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objektive Sharpie

Objektive Sharpie ist ein weiterer, separate-Befehlszeilentools, die mit den ersten Schritten zum Erstellen einer Bindung unterstützen. Eine Bindung wird nicht von sich selbst erstellt, sondern den ersten Schritt zum Generieren von einer API-Definition für die native Zielbibliothek automatisiert.

Lesen der [Ziel Sharpie-Dokumentation](~/cross-platform/macios/binding/objective-sharpie/index.md) zu erfahren, wie Sie native Bibliotheken, nativen Frameworks und CocoaPods in API-Definitionen zu analysieren, die in den Bindungen erstellt werden können.

<a name="howitworks" />

## <a name="how-binding-works"></a>Funktionsweise der Bindung

Es ist möglich, verwenden Sie die [[registrieren]](xref:Foundation.RegisterAttribute) Attribut [[Export]](xref:Foundation.ExportAttribute) -Attribut und [manuellen Objective-C-Selektor-Aufruf](~/ios/internals/objective-c-selectors.md) zusammen, um manuell neu (zuvor binden ungebunden) Objective-C-Typen.

Suchen Sie zunächst einen Typ, den Sie binden möchten. Diskussion zu (und Einfachheit), wird eine Bindung wird der [NSEnumerator](https://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) Typ (die bereits im gebundenen [Foundation.NSEnumerator](xref:Foundation.NSEnumerator); die nachfolgende Implementierung ist einfach, z. B. Zwecke).

Zweitens müssen wir erstellen den C# Typ. Wir möchten wahrscheinlich dies in einem Namespace platzieren; Da Objective-C-Namespaces nicht unterstützt, müssen wir verwenden die `[Register]` Attribut so ändern Sie den Typnamen, die Xamarin.iOS mit Objective-C-Laufzeit registriert werden. Die C# Typ muss auch eine Vererbung von [Foundation.NSObject](xref:Foundation.NSObject):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Drittens: Überprüfen Sie die Objective-C-Dokumentation und erstellen Sie [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) Instanzen für jede Auswahl, die Sie verwenden möchten. Platzieren Sie diese in den Text der Klasse:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Viertens: Ihr Typ müssen den Konstruktoren. Sie *müssen* Ihre Konstruktoraufruf an den Konstruktor der Basisklasse zu verketten. Die `[Export]` Attribute ermöglichen, Objective-C-Code aufrufen, die Konstruktoren, mit dem angegebenen Selektor-Namen:

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

Fünfter, geben Sie, dass die Methoden für jede der Selektoren in Schritt 3 deklariert. Verwendet diese `objc_msgSend()` die Auswahl für das systemeigene Objekt aufzurufende. Beachten Sie die Verwendung von [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) konvertieren eine `IntPtr` in einem ordnungsgemäß typisierten `NSObject` (Sub)-Typ. Wenn Sie möchten, dass die Methode, die in Objective-C-Code, der Member aufgerufen werden können *müssen* werden **virtuellen**.

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

Zusammenfassung:

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

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen eine Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen Sie eine Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)