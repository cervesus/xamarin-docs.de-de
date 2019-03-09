---
title: Einschränkungen von Xamarin.iOS
description: Dieses Dokument beschreibt die Einschränkungen von Xamarin.iOS, Erörterung von Generika, generische Unterklassen von NSObjects, P/Invokes in generische Objekte und mehr.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/09/2018
ms.openlocfilehash: a6a4ef9fb36fde067fa58fec9a6206b1dbc1fbf0
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668347"
---
# <a name="limitations-of-xamarinios"></a>Einschränkungen von Xamarin.iOS

Da Anwendungen, die unter Verwendung von Xamarin.iOS in statischen Code kompiliert werden, ist es nicht möglich, alle Funktionen verwenden zu können, für die Generierung von Code zur Laufzeit erforderlich.

Dies sind die Xamarin.iOS-Einschränkungen, die im Vergleich zum Desktop Mono:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Eingeschränkte Generikunterstützung

Im Gegensatz zu herkömmlichen Mono und .NET wird Code auf dem iPhone statisch voraus statt bei Bedarf kompiliert wird, die von einem JIT-Compiler kompiliert.

Mono [vollständige AOT](https://www.mono-project.com/docs/advanced/aot/#full-aot) Technologie gelten einige Einschränkungen in Bezug auf Generika, diese werden verursacht, da nicht alle möglichen generische Instanziierung voraus zum Zeitpunkt der Kompilierung nicht bestimmt werden kann. Dies ist kein Problem für reguläre .NET oder Mono-Laufzeiten, wie der Code immer zur Laufzeit über den Just-in-Time-Compiler kompiliert wird. Dies stellt eine Herausforderung für einen statischen Compiler wie Xamarin.iOS aber.

Einige der häufigsten Probleme, denen Entwickler konfrontiert sind:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Generische Unterklassen von NSObjects sind beschränkt.

Xamarin.iOS verfügt über Unterstützung für das Erstellen von generischer Unterklassen von NSObject-Klasse, z. B. keine Unterstützung für generische Methoden derzeit begrenzt. Ab 7.2.1 ist die generische Unterklassen von NSObjects mit möglich ist, wie die folgende:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Da generische Unterklassen von NSObjects möglich sind, gelten einige Einschränkungen. Lesen der [generische Unterklassen von NSObject](~/ios/internals/api-design/nsobject-generics.md) Dokument Weitere Informationen


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Keine dynamische Codegenerierung

Da der iOS-Kernel wird verhindert, eine Anwendung dynamisch generieren von Code dass, wird jede Form von dynamische codegenerierung von Xamarin.iOS nicht unterstützt. Dazu gehören:

-  Die System.Reflection.Emit ist nicht verfügbar.
-  Keine Unterstützung für System.Runtime.Remoting.
-  Keine Unterstützung für das Erstellen von Typen dynamisch (keine Type.GetType ("MyType" 1")), obwohl das Nachschlagen von vorhandener Typen (Type.GetType ("System.String") beispielsweise gesehen gut funktioniert). 
-  Reverse-Rückrufe müssen zum Zeitpunkt der Kompilierung mit der Laufzeit registriert werden.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Das Fehlen von System.Reflection. **Ausgeben** bedeutet, die keinen Code, der von der Common Language Runtime-codegenerierung abhängt geeignet ist. Dazu zählen z. B.:

-  Die Dynamic Language Runtime.
-  Alle Sprachen, die die Dynamic Language Runtime aufsetzt.
-  Remoting TransparentProxy o. ä., die die Laufzeit zum Generieren von Code dynamisch verursachen würde. 


 **Wichtig:** Verwechseln Sie nicht **Reflection.Emit** mit **Reflektion**. Reflection.Emit zum Generieren von Code dynamisch ist und dieser Code JIT-kompilierten und zu systemeigen kompilierten Code. Aufgrund von Beschränkungen für iOS (keine JIT-Kompilierung) ist dies nicht unterstützt.

Aber der gesamte Reflektions-API, einschließlich der Type.GetType ("" SomeClass "bereitgestellt"), Auflisten von Methoden, Eigenschaften, auflisten, Abrufen von Attributen und Werten funktioniert problemlos.

### <a name="using-delegates-to-call-native-functions"></a>Verwenden von Delegaten zum Aufrufen von systemeigenen Funktionen

Um eine native Funktion über einen C#-Delegaten aufzurufen, muss der Stellvertretung-Deklaration mit einem der folgenden Attribute versehen werden:

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (bevorzugt, da es sich um verschiedene Plattformen und kompatibel mit .NET Standard 1.1 und höher ist)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Fehler beim Bereitstellen eines dieser Attribute führt zu einem Laufzeitfehler, wie z. B.:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Rückrufe umkehren

In standard Mono ist es möglich, Instanzen für c#-Delegaten zu nicht verwaltetem Code durch einen Funktionszeiger übergeben. Die Laufzeit würde in der Regel dieser Funktionszeiger in einen kleinen Thunk umgewandelt werden, die nicht verwalteten Code für den Rückruf in verwaltetem Code ermöglicht wird.

In Mono diesen Bridges implementiert werden, durch den Just-in-Time-Compiler. Wenn mit dem ahead-of-Time-Compiler das iPhone erforderlich, dass es an diesem Punkt sind zwei wichtige Einschränkungen:

-  Sie müssen alle Ihre Rückrufmethoden mit kennzeichnen die [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Die Methoden statische Methoden sein müssen, besteht keine Unterstützung für die Instanz Methoden. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Keine-Remoting

Der remotingstapel ist nicht verfügbar, in Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Runtime deaktiviert Features

Die folgenden Features wurden in iOS-Mono Laufzeit deaktiviert:

-  Profiler
-  Reflection.Emit
-  Reflection.Emit.Save-Funktion
-  Com-Bindungen
-  Die JIT-engine
-  Metadaten-Verifier (da es keine JIT-Kompilierung gibt)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API-Einschränkungen

Die .NET API verfügbar gemacht ist eine Teilmenge des vollständigen Frameworks, da nicht alle in iOS verfügbar ist. Finden Sie in den häufig gestellten Fragen ein [Liste der derzeit unterstützten Assemblys](~/cross-platform/internals/available-assemblies.md).



Insbesondere enthält das Xamarin.iOS-API-Profil "System.Configuration", keine daher es nicht möglich ist, externe XML-Dateien zu verwenden, um das Verhalten der Laufzeit zu konfigurieren.
