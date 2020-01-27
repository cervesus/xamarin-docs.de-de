---
title: Übersicht über Ziel-C-Bindungen
description: Dieses Dokument bietet eine Übersicht über die verschiedenen Methoden zum C# Erstellen von Bindungen für den Ziel-C-Code, einschließlich Befehlszeilen Bindungen, Bindungs Projekten und Ziel-Sharpie. Außerdem wird erläutert, wie die Bindung funktioniert.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: be2f7f555b76d472f7a66d95e661bb2f5884c58f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725341"
---
# <a name="overview-of-objective-c-bindings"></a>Übersicht über Ziel-C-Bindungen

_Details zur Funktionsweise des Bindungs Prozesses_

Das Binden einer Ziel-C-Bibliothek für die Verwendung mit xamarin führt drei Schritte aus:

1. Schreiben Sie C# eine "API-Definition", um zu beschreiben, wie die Native API in .net verfügbar gemacht wird, und wie Sie dem zugrunde liegenden Ziel "-C" zugeordnet wird. Dies erfolgt über standardkonstrukte C# wie `interface` und verschiedene Bindungs **Attribute** (Weitere Informationen finden Sie in diesem [einfachen Beispiel](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Nachdem Sie die "API-Definition" in C#geschrieben haben, kompilieren Sie Sie, um eine "Binding"-Assembly zu erstellen. Dies kann über die [**Befehlszeile**](#commandline) oder mithilfe eines [**Bindungs Projekts**](#bindingproject) in Visual Studio für Mac oder Visual Studio erfolgen.

3. Diese "Binding"-Assembly wird dann dem xamarin-Anwendungsprojekt hinzugefügt, sodass Sie mit der von Ihnen definierten API auf die native Funktionalität zugreifen können.
   Das Bindungs Projekt ist vollständig von den Anwendungsprojekten getrennt.

   > [!NOTE]
   > Schritt 1 kann mit der Unterstützung des [**Ziels "Sharpie**](#objectivesharpie)" automatisiert werden. Die API untersucht die Ziel-C-API und C# generiert eine vorgeschlagene "API-Definition". Sie können die von Ziel-Sharpie erstellten Dateien anpassen und in einem Bindungs Projekt (oder in der Befehlszeile) verwenden, um die bindungsassembly zu erstellen. Der Ziel-Sharpie erstellt keine Bindungen allein, sondern lediglich einen optionalen Teil des größeren Prozesses.

Sie können auch weitere technische Details zur [Funktionsweise](#howitworks)lesen, die Sie beim Schreiben von Bindungen unterstützen.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Befehlszeilen Bindungen

Sie können die `btouch-native` für xamarin. IOS (oder `bmac-native` bei Verwendung von xamarin. Mac) verwenden, um Bindungen direkt zu erstellen. Dies funktioniert, indem die C# API-Definitionen, die Sie von Hand (oder mithilfe von Ziel-Sharpie) erstellt haben, an das Befehlszeilen Tool (`btouch-native` für IOS oder `bmac-native` für Mac) übergeben werden.

Die allgemeine Syntax zum Aufrufen dieser Tools lautet wie folgt:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Der obige Befehl generiert die Datei `cocos2d.dll` im aktuellen Verzeichnis, und Sie enthält die vollständig gebundene Bibliothek, die Sie in Ihrem Projekt verwenden können. Dies ist das Tool, das Visual Studio für Mac verwendet, um die Bindungen zu erstellen, wenn Sie ein Bindungs Projekt verwenden (siehe [unten](#bindingproject)).

<a name="bindingproject" />

## <a name="binding-project"></a>Bindungs Projekt

Ein Bindungs Projekt kann in Visual Studio für Mac oder Visual Studio erstellt werden (Visual Studio unterstützt nur IOS-Bindungen) und erleichtert das Bearbeiten und Erstellen von API-Definitionen für die Bindung (im Gegensatz zur Verwendung der Befehlszeile).

Befolgen Sie diese [Anleitung](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) für die ersten Schritte, um zu erfahren, wie Sie ein Bindungs Projekt erstellen und verwenden, um eine Bindung zu erstellen.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Objektive Sharpie

Ziel-Sharpie ist ein weiteres, separates Befehlszeilen Tool, das die ersten Phasen der Erstellung einer Bindung unterstützt. Er erstellt nicht allein eine Bindung, sondern automatisiert den ersten Schritt der Generierung einer API-Definition für die native Ziel Bibliothek.

Lesen Sie die [Ziel-Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md) -Dokumentation, um zu erfahren, wie Sie Native Bibliotheken, Native Frameworks und cocoapods in API-Definitionen analysieren können, die in Bindungen integriert werden können.

<a name="howitworks" />

## <a name="how-binding-works"></a>Funktionsweise der Bindung

Es ist möglich, das Attribut [[Register]](xref:Foundation.RegisterAttribute) , [[Export]](xref:Foundation.ExportAttribute) und den [manuellen Aufruf der Ziel-c-Auswahl](~/ios/internals/objective-c-selectors.md) zu verwenden, um neue (zuvor nicht gebundene) Ziel-c-Typen manuell zu binden.

Suchen Sie zuerst einen Typ, den Sie binden möchten. Zu Diskussions Zwecken (und Vereinfachung) binden wir den [nsenum](https://developer.apple.com/documentation/foundation/nsenumerator) -Typ (der bereits in [Foundation. nsenübererator](xref:Foundation.NSEnumerator)gebunden wurde. die folgende Implementierung ist nur zu Beispiel Zwecken).

Zweitens müssen wir den C# Typ erstellen. Wir möchten dies wahrscheinlich in einem Namespace platzieren. Da Ziel-c keine Namespaces unterstützt, müssen wir das `[Register]`-Attribut verwenden, um den Typnamen zu ändern, den xamarin. IOS bei der Ziel-c-Laufzeit registriert. Der C# Typ muss auch von [Foundation. NSObject](xref:Foundation.NSObject)erben:

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Überprüfen Sie die Ziel-C-Dokumentation, und erstellen Sie [objcruntime. Selector](xref:ObjCRuntime.Selector) -Instanzen für jede Auswahl, die Sie verwenden möchten. Platzieren Sie diese innerhalb des Klassen Texts:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Viertens muss Ihr Typ Konstruktoren bereitstellen. Sie *müssen* den Konstruktoraufruf mit dem Basisklassenkonstruktor verketten. Die `[Export]` Attribute ermöglichen es dem Ziel-C-Code, die Konstruktoren mit dem angegebenen Auswahl Namen aufzurufen:

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

Fünftens stellen Sie Methoden für jeden der in Schritt 3 deklarierten Selektoren bereit. Diese verwenden `objc_msgSend()`, um die Auswahl für das Native Objekt aufzurufen. Beachten Sie, dass [Runtime. getnsobject ()](xref:ObjCRuntime.Runtime.GetNSObject*) verwendet wird, um eine `IntPtr` in einen entsprechend typisierten `NSObject` (Sub-)-Typ zu konvertieren. Wenn Sie möchten, dass die Methode von Ziel-C-Code aufgerufen werden kann, *muss* der Member **virtuell**sein.

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

Alles:

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
