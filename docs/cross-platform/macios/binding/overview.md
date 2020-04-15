---
title: Übersicht über Objective-C-Bindungen
description: Das vorliegende Dokument bietet einen Überblick über verschiedene Möglichkeiten, C#-Bindungen für Objective-C-Code zu erstellen, Befehlszeilenbindungen, Bindungsprojekte und Objective Sharpie eingeschlossen. Darüber hinaus wird erläutert, wie Bindungen funktionieren.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: be2f7f555b76d472f7a66d95e661bb2f5884c58f
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725341"
---
# <a name="overview-of-objective-c-bindings"></a>Übersicht über Objective-C-Bindungen

_Details zur Funktionsweise des Bindungsprozesses_

Die Bindung einer Objective-C-Bibliothek zur Verwendung mit Xamarin erfolgt in drei Schritten:

1. Schreiben Sie eine „API-Definition“ in C#, um die Offenlegung der nativen API in .NET und deren Zuordnung im zugrunde liegenden Objective-C zu beschreiben. Dies geschieht unter Verwendung von C#-Standardkonstrukten wie `interface` und verschiedenen **Bindungsattributen** (siehe dieses [einfache Beispiel](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Nachdem Sie die „API-Definition“ in C# geschrieben haben, kompilieren Sie sie, um eine „Bindungsassembly“ zu generieren. Die Kompilierung kann über die [**Befehlszeile**](#commandline) oder mithilfe eines [**Bindungsprojekts**](#bindingproject) in Visual Studio für Mac oder Visual Studio erfolgen.

3. Diese „Bindungsassembly“ wird anschließend Ihrem Xamarin-Anwendungsprojekt hinzugefügt, damit Sie über die definierte API auf die native Funktionalität zugreifen können.
   Das Bindungsprojekt ist vollständig getrennt von Ihren Anwendungsprojekten.

   > [!NOTE]
   > Schritt 1 kann mithilfe von [**Objective Sharpie**](#objectivesharpie) automatisiert werden. Dieses Tool untersucht die Objective-C-API und generiert eine vorgeschlagene C#-API-Definition. Sie können die von Objective Sharpie erstellten Dateien anpassen und sie in einem Bindungsprojekt (oder in der Befehlszeile) verwenden, um Ihre Bindungsassembly zu erstellen. Objective Sharpie selbst erstellt keine Bindungen, dies ist lediglich ein optionaler Bestandteil des übergeordneten Prozesses.

Sie können sich über weitere technische Details zur [Funktionsweise](#howitworks) informieren, um Unterstützung beim Schreiben von Bindungen zu erhalten.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Befehlszeilenbindungen

Sie können `btouch-native` für Xamarin.iOS (oder `bmac-native` bei Verwendung von Xamarin.Mac) verwenden, um Bindungen direkt zu erstellen. Hierbei werden die manuell (oder mit Objective Sharpie) erstellten C#-API-Definitionen an das Befehlszeilentool (`btouch-native` für iOS oder `bmac-native` für Mac) übergeben.

Die allgemeine Syntax zum Aufrufen dieser Tools lautet folgendermaßen:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Der obige Befehl erzeugt die Datei `cocos2d.dll` im aktuellen Verzeichnis, und sie enthält die vollständig gebundene Bibliothek, die Sie in Ihrem Projekt verwenden können. Dies ist das Tool, mit dem Visual Studio für Mac Ihre Bindungen erstellt, wenn Sie ein Bindungsprojekt verwenden (siehe Beschreibung [unten](#bindingproject)).

<a name="bindingproject" />

## <a name="binding-project"></a>Bindungsprojekt

Ein Bindungsprojekt kann in Visual Studio für Mac oder Visual Studio (Visual Studio unterstützt nur iOS-Bindungen) erstellt werden und vereinfacht das Bearbeiten und Kompilieren von API-Definitionen für Bindungen (im Gegensatz zur Verwendung der Befehlszeile).

Dieser [Leitfaden für die ersten Schritte](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) zeigt, wie Sie ein Bindungsprojekt erstellen und verwenden, um eine Bindung zu erzeugen.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objektive Sharpie

Objective Sharpie ist ein weiteres, separates Befehlszeilentool, das Unterstützung in den ersten Phasen der Erstellung einer Bindung bietet. Das Tool selbst erstellt keine Bindungen, sondern automatisiert den ersten Schritt zum Generieren einer API-Definition für die native Zielbibliothek.

In der [Objective Sharpie-Dokumentation](~/cross-platform/macios/binding/objective-sharpie/index.md) erfahren Sie, wie Sie native Bibliotheken, native Frameworks und CocoaPods durch Parsen in API-Definitionen umwandeln, die in Bindungen kompiliert werden können.

<a name="howitworks" />

## <a name="how-binding-works"></a>Funktionsweise von Bindungen

Es ist möglich, die Attribute [[Register]](xref:Foundation.RegisterAttribute) und [[Export]](xref:Foundation.ExportAttribute) sowie einen [manuellen Objective-C-Selektoraufruf](~/ios/internals/objective-c-selectors.md) gemeinsam zu verwenden, um neue (bisher ungebundene) Objective-C-Typen manuell zu binden.

Im ersten Schritt ermitteln Sie einen Typ, den Sie binden möchten. Zur Erläuterung (und der Einfachheit halber) binden wir den Typ [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator) (der bereits in [Foundation.NSEnumerator](xref:Foundation.NSEnumerator) gebunden wurde; die nachstehende Implementierung dient lediglich als Beispiel).

Im zweiten Schritt muss der C#-Typ erstellt werden. Die Platzierung erfolgt vorzugsweise in einem Namespace. Da Objective-C aber keine Namespaces unterstützt, müssen wir mit dem `[Register]`-Attribut den Typnamen ändern, den Xamarin.iOS bei der Objective-C-Runtime registriert. Der C#-Typ muss außerdem von [Foundation.NSObject](xref:Foundation.NSObject) erben:

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Im dritten Schritt konsultieren wir die Objective-C-Dokumentation und erstellen [ObjCRuntime.Selector](xref:ObjCRuntime.Selector)-Instanzen für jeden Selektor, der verwendet werden soll. Platzieren Sie diese innerhalb des Klassentexts:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Im vierten Schritt muss Ihr Typ Konstruktoren bereitstellen. Sie *müssen* Ihren Konstruktoraufruf mit dem Basisklassenkonstruktor verketten. Das `[Export]`-Attribut erlaubt es dem Objective-C-Code, die Konstruktoren mit dem angegebenen Selektornamen aufzurufen:

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

Im fünften Schritt geben Sie Methoden für jeden in Schritt 3 deklarierten Selektor an. Diese verwenden `objc_msgSend()` zum Aufruf des Selektors für das native Objekt. Beachten Sie die Verwendung von [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) zum Konvertieren von `IntPtr` in ein `NSObject` eines geeigneten Typs (Untertyps). Wenn die Methode vom Objective-C-Code aus aufrufbar sein soll, *muss* es sich um einen **virtuellen** Member handeln.

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
