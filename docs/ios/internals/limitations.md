---
title: Einschränkungen von xamarin. IOS
description: In diesem Dokument werden die Einschränkungen von xamarin. IOS beschrieben. dabei werden Generika, generische Unterklassen von NSObjects, P/Aufrufe in generischen Objekten und mehr erläutert.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/09/2018
ms.openlocfilehash: 91513936a0223af0e4220154d0fe65ee0a599a4f
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78911546"
---
# <a name="limitations-of-xamarinios"></a>Einschränkungen von xamarin. IOS

Da Anwendungen, die xamarin. IOS verwenden, in statischen Code kompiliert werden, ist es nicht möglich, Funktionen zu verwenden, die zur Laufzeit Codegenerierung erfordern.

Dies sind die xamarin. IOS-Einschränkungen im Vergleich zu Desktop Mono:

 <a name="Limited_Generics_Support" />

## <a name="limited-generics-support"></a>Eingeschränkte Generika Unterstützung

Im Gegensatz zu herkömmlichem Mono/. net wird Code auf dem iPhone statisch kompiliert, anstatt bei Bedarf von einem JIT-Compiler kompiliert zu werden.

Die [vollständige AOT](https://www.mono-project.com/docs/advanced/aot/#full-aot) -Technologie von Mono weist einige Einschränkungen in Bezug auf Generika auf. diese sind darauf zurückzuführen, dass nicht jede mögliche generische Instanziierung im Vordergrund zur Kompilierzeit bestimmt werden kann. Dies ist kein Problem für reguläre .net-oder Mono-Laufzeiten, da der Code immer zur Laufzeit mithilfe des Just-in-Time-Compilers kompiliert wird. Dies stellt jedoch eine Herausforderung für einen statischen Compiler wie xamarin. IOS dar.

Zu den häufigsten Problemen, die Entwickler ausführen können, gehören:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />

### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Generische Unterklassen von NSObjects sind beschränkt.

Xamarin. IOS bietet derzeit eingeschränkte Unterstützung für das Erstellen von generischen Unterklassen der NSObject-Klasse, z. b. keine Unterstützung generischer Methoden. Ab 7.2.1 ist die Verwendung generischer Unterklassen von NSObjects wie folgt möglich:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Generische Unterklassen von NSObjects sind zwar möglich, es gibt jedoch einige Einschränkungen. Weitere Informationen finden Sie [in den generischen Unterklassen des NSObject](~/ios/internals/api-design/nsobject-generics.md) -Dokuments.

 <a name="No_Dynamic_Code_Generation" />

## <a name="no-dynamic-code-generation"></a>Keine dynamische Code Generierung

Da der IOS-Kernel verhindert, dass eine Anwendung Code dynamisch generiert, unterstützt xamarin. IOS keine Form der dynamischen Codegenerierung. Dazu gehören:

- Die System. Reflection.-Ausgabe ist nicht verfügbar.
- Keine Unterstützung für System. Runtime. Remoting.
- Keine Unterstützung für die dynamische Erstellung von Typen ("No Type. GetType" ("MyType ' 1")), obwohl das Nachschlagen vorhandener Typen (Type. GetType ("System. String") beispielsweise problemlos funktioniert).
- Umgekehrte Rückrufe müssen zur Kompilierzeit bei der Laufzeit registriert werden.

 <a name="System.Reflection.Emit" />

### <a name="systemreflectionemit"></a>System.Reflection.Emit

Das Fehlen von System. Reflection. Ausgabe **bedeutet,** dass kein Code funktioniert, der von der Generierung von Lauf Zeit Code abhängt. Dazu zählen etwa:

- Die Dynamic Language Runtime.
- Alle Sprachen, die auf der Dynamic Language Runtime erstellt wurden.
- Der TransparentProxy von Remoting oder alles andere, das dazu führen würde, dass die Laufzeit Code dynamisch generiert.

  > [!IMPORTANT]
  > **Reflektion** nicht verwechseln. Ausgabe mit **Reflektion**. "Reflection. Ausgabe" geht über die dynamische Generierung von Code und die Kompilierung des Codes in nativem Code aus. Aufgrund der Einschränkungen bei IOS (keine JIT-Kompilierung) wird dies nicht unterstützt.

Die gesamte Reflection-API, einschließlich Type. GetType ("SomeClass"), Auflistungs Methoden, Auflistungs Eigenschaften, Abrufen von Attributen und Werten funktioniert problemlos.

### <a name="using-delegates-to-call-native-functions"></a>Verwenden von Delegaten zum Abrufen nativer Funktionen

Um eine native Funktion über einen C# Delegaten aufzurufen, muss die Deklaration des Delegaten mit einem der folgenden Attribute versehen werden:

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (bevorzugt, da Sie plattformübergreifend und mit .NET Standard 1.1 + kompatibel ist)
- [Mononativefunctionwrapperattribute](xref:ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Wenn Sie eines dieser Attribute nicht bereitstellen, führt dies zu einem Laufzeitfehler, wie z. b.:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```

 <a name="Reverse_Callbacks" />

### <a name="reverse-callbacks"></a>Umgekehrte Rückrufe

In Standard-Mono ist es möglich, C# Delegatinstanzen anstelle eines Funktions Zeigers an nicht verwalteten Code zu übergeben. Die Laufzeit wandelt diese Funktionszeiger in der Regel in einen kleinen Thunk um, der es nicht verwaltetem Code ermöglicht, den verwalteten Code wieder aufzurufen.

In Mono werden diese Bridges durch den Just-in-Time-Compiler implementiert. Wenn Sie den für das iPhone erforderlichen Ahead-of-Time-Compiler verwenden, gibt es an dieser Stelle zwei wichtige Einschränkungen:

- Sie müssen alle Rückruf Methoden mit dem " [monopinvokecallbackattribute](xref:ObjCRuntime.MonoPInvokeCallbackAttribute) " markieren.
- Bei den Methoden handelt es sich um statische Methoden, es gibt keine Unterstützung für Instanzmethoden.

<a name="No_Remoting" />

## <a name="no-remoting"></a>Kein Remoting

Der Remote Stapel ist in xamarin. IOS nicht verfügbar.

 <a name="Runtime_Disabled_Features" />

## <a name="runtime-disabled-features"></a>Funktionen zur Laufzeit deaktiviert

Die folgenden Features wurden in der IOS-Laufzeit von Mono deaktiviert:

- Profiler
- Reflektion. ausgeben
- Reflection. Ausgabe. Save-Funktionalität
- COM-Bindungen
- Die JIT-Engine
- Metadatenüberprüfung (da keine JIT vorhanden ist)

 <a name=".NET_API_Limitations" />

## <a name="net-api-limitations"></a>Einschränkungen der .NET-API

Die bereitgestellte .NET-API ist eine Teilmenge des vollständigen Frameworks, da nicht alles in ios verfügbar ist. Eine [Liste der derzeit unterstützten](~/cross-platform/internals/available-assemblies.md)Assemblys finden Sie in den FAQ.

Insbesondere das von xamarin. IOS verwendete API-Profil enthält nicht "System. Configuration", sodass es nicht möglich ist, externe XML-Dateien zu verwenden, um das Verhalten der Laufzeit zu konfigurieren.
