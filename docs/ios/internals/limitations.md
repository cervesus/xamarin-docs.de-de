---
title: Einschränkungen von Xamarin.iOS
description: Dieses Dokument beschreibt die Einschränkungen von Xamarin.iOS, Erörterung von Generika, generischen Unterklassen des NSObjects, P/Invokes in generischen Objekte und vieles mehr.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: 8eb2cd5a749beab6f089479f5992fe3fbc16dd0a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786228"
---
# <a name="limitations-of-xamarinios"></a>Einschränkungen von Xamarin.iOS

Da Anwendungen auf dem iPhone mithilfe von Xamarin.iOS für statischen Code kompiliert werden, ist es nicht möglich, alle Funktionen verwenden zu können, die codegenerierung zur Laufzeit erfordern.

Dies sind die im Vergleich zum Desktop Mono Xamarin.iOS-Einschränkungen:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Begrenzte Generikunterstützung

Im Gegensatz zu herkömmlichen Mono/.NET wird Code auf dem iPhone statisch voraus anstatt bei Bedarf kompiliert wird, die vom JIT-Compiler kompiliert.

Mono [vollständige AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot) Technologie gelten einige Einschränkungen in Bezug auf Generika, werden diese verursacht, da nicht alle möglichen generische Instanziierung Vorfeld zur Kompilierzeit bestimmt werden kann. Dies ist kein Problem für den regulären .NET oder Mono-Laufzeiten, wie der Code immer zur Laufzeit mithilfe von der Just in Time-Compiler kompiliert wird. Aber dies stellt eine Herausforderung für einen statischen Compiler wie Xamarin.iOS.

Einige der am häufigsten Probleme, die Entwickler, die auftreten können, gehören:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Generische Unterklassen NSObjects sind beschränkt.

Xamarin.iOS wurde Unterstützung für das Erstellen von generischen Unterklassen der NSObject-Klasse, z. B. keine Unterstützung für generische Methoden derzeit beschränkt. Zum Zeitpunkt 7.2.1 ist mit generischen Unterklassen des NSObjects möglich, wie diese:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Beim generischen Unterklassen des NSObjects möglich sind, stehen einige Einschränkungen. Lesen der [generische Unterklassen des NSObject](~/ios/internals/api-design/nsobject-generics.md) Dokument Weitere Informationen



### <a name="pinvokes-in-generic-types"></a>P/ruft in generischen Typen

P/Invokes in generischen Klassen werden nicht unterstützt:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo auf dem Nullable-Typ wird nicht unterstützt.

Mithilfe der Reflektion Property.SetInfo zum Festlegen des Werts auf ein Typ Nullable&lt;T&gt; wird derzeit nicht unterstützt.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Werttypen als Wörterbuchschlüssel

Verwenden einen Werttyp als Wörterbuch&lt;TKey, TValue&gt; Schlüssel problematisch ist, wird als Standard versucht Wörterbuch Konstruktor EqualityComparer verwenden&lt;TKey&gt;. Standardmäßig verwendet. EqualityComparer&lt;TKey&gt;. Standardmäßig versucht wiederum die Reflexion verwenden, um einen neuen Typ zu instanziieren, die IEqualityComparer implementiert&lt;TKey&gt; Schnittstelle.

Dies funktioniert für Referenztypen (wie die Reflektion + Erstellen eines neuen Schritt wird übersprungen,), aber für Wert Typen es Abstürze (crashes), und stattdessen schnell brennt, sobald Sie versuchen, auf dem Gerät zu verwenden.

 **Problemumgehung**: manuell implementieren die [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) -Schnittstelle in einen neuen Typ, und geben Sie eine Instanz dieses Typs der [Wörterbuch&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) Konstruktor.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Keine dynamische Codegenerierung

Da das iPhone Kernel eine Anwendung über das Generieren von Code dynamisch wird verhindert, dass unterstützt Mono auf dem iPhone keine Form von dynamische codegenerierung. Dazu gehören:

-  Die System.Reflection.Emit ist nicht verfügbar.
-  Keine Unterstützung für System.Runtime.Remoting.
-  Keine Unterstützung für das Erstellen von Typen dynamisch (keine Type.GetType ("MyType" 1")), obwohl das Nachschlagen von vorhandenen Typen (Type.GetType ("System.String") z. B. funktioniert einwandfrei). 
-  Umgekehrte Rückrufe müssen zum Zeitpunkt der Kompilierung mit der Laufzeit registriert werden.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Der Mangel an System.Reflection. **Ausgeben** bedeutet, die keinen Code, der auf die codegenerierung für Laufzeit abhängt geeignet ist. Dies umfasst z. B.:

-  Die Dynamic Language Runtime.
-  Alle Sprachen, baut auf den der Dynamic Language Runtime.
-  Remoting TransparentProxy oder etwas anderes, das die Common Language Runtime zum dynamischen Generieren von Code führen würde. 


 **Wichtig:** Verwechseln Sie nicht **"Reflection.Emit"** mit **Reflektion**. "Reflection.Emit" zum Generieren von Code dynamisch und diese JIT-kompilierten und zu systemeigen kompilierten Code. Aufgrund von Beschränkungen auf dem iPhone (keine JIT-Kompilierung) ist dies nicht unterstützt.

Aber der gesamte Reflektions-API, einschließlich der Type.GetType ("" SomeClass "bereitgestellt"), Auflisten von Methoden, Eigenschaften, Attribute und Werte abrufen genügt.

### <a name="using-delegates-to-call-native-functions"></a>Verwenden von Delegaten aufrufen systemeigener Funktionen

Um eine systemeigene Funktion über einen Delegaten c# aufrufen zu können, muss der Delegatdeklaration überein mit einem der folgenden Attribute ausgestattet sein:

- [UnmanagedFunctionPointerAttribute](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute/) (bevorzugt, da er über Plattformen hinweg und mit .NET Standard 1.1 höher kompatibel ist)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Wegen eines Fehlers beim Bereitstellen eines dieser Attribute führt zu einem Laufzeitfehler, wie z. B.:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Umkehren von Rückrufen

In standard Mono ist es möglich, C#-Delegatinstanzen zu nicht verwaltetem Code anstelle einer Funktionszeiger übergeben. Die Common Language Runtime transformiert diese Funktionszeiger in der Regel in einem kleinen Thunk, die nicht verwalteten Codes zurück an verwalteten Code aufrufen kann.

In Mono diesen Bridges implementiert werden, durch den Just-in-Time Compiler. Wenn mit den ahead des Time-Compiler das iPhone erforderlich, dass es zu diesem Zeitpunkt sind zwei wichtige Einschränkungen:

-  Sie müssen alle Ihre Rückrufmethoden mit kennzeichnen die [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Die Methoden verfügen über statische Methoden sein, es gibt keine Unterstützung für die Instanz Methoden. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Keine Remoting

Der Stapel Remoting ist nicht auf Xamarin.iOS verfügbar.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Common Language Runtime Features deaktiviert.

Die folgenden Features wurden in Mono des iOS-Laufzeit deaktiviert:

-  Profiler
-  "Reflection.Emit"
-  Reflection.Emit.Save-Funktion
-  Com-Bindungen
-  Das JIT-Modul
-  Metadaten Verifier (da es keine JIT ist)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API-Einschränkungen

Die .NET API verfügbar gemacht ist eine Teilmenge des vollständigen Frameworks, wie nicht alles, was in iOS verfügbar ist. Siehe die FAQ für eine [Liste der derzeit unterstützten Assemblys](~/cross-platform/internals/available-assemblies.md).



Das Profil API Xamarin.iOS gehören insbesondere nicht "System.Configuration", daher es nicht möglich ist, externe XML-Dateien verwenden, um das Verhalten der Laufzeit zu konfigurieren.
