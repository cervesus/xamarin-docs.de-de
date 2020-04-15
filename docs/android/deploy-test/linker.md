---
title: Verknüpfung unter Android
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/30/2018
ms.openlocfilehash: 729dede97a9b153738ca72af499d4d1e9d77fae0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73021447"
---
# <a name="linking-on-android"></a>Verknüpfung unter Android

Xamarin.Android-Anwendungen verwenden einen *Linker*, um die Größe der Anwendung zu minimieren. Der Linker verwendet die statische Analyse Ihrer Anwendung, um zu bestimmen, welche Assemblys, Typen und Member tatsächlich verwendet werden. Der Linker verhält sich daraufhin wie ein *Garbage Collector* und sucht ständig nach Assemblys, Typen und Membern, auf die verwiesen wird, bis der gesamte Abschluss von Assemblys, Typen und Membern, auf die verwiesen wird, gefunden wird. Alles außerhalb dieses Abschlusses wird anschließend *verworfen*.

Wie z.B. das [Hallo, Android](https://docs.microsoft.com/samples/xamarin/monodroid-samples/hellom4a)-Beispiel:

|Konfiguration|Größe 1.2.0|Größe 4.0.1|
|---|---|---|
|Release ohne Verknüpfung:|14,0 MB|16,0 MB|
|Release mit Verknüpfung:|4,2 MB|2,9 MB|

Die Verknüpfung führt zu einem Paket, das 30 % der Größe des (nicht verknüpften) Originalpakets in 1.2.0 und 18 % der Größe des nicht verknüpften Pakets in 4.0.1 entspricht.

## <a name="control"></a>Steuerelement

Die Verknüpfung basiert auf *statischer Analyse*. Das bedeutet, dass alles, was von der Laufzeitumgebung abhängig ist, nicht erkannt wird:

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```

### <a name="linker-behavior"></a>Linkerverhalten

Der primäre Mechanismus für die Steuerung des Linkers ist das Dropdownmenü **Linkerverhalten** (*Verknüpfen* in Visual Studio) im Dialogfeld **Projektoptionen**. Es gibt drei Optionen:

1. **Nicht verknüpfen** (*Keine* in Visual Studio)
1. **SDK-Assemblys verknüpfen** (*nur SDK-Assemblys*)
1. **Alle Assemblys verknüpfen** (*SDK- und Benutzerassemblys*)

Die Option **Nicht verknüpfen** deaktiviert den Linker. Für die Größe der Anwendung „Release ohne Verknüpfen“ oben wurde dieses Verhalten verwendet. Dies ist für die Problembehandlung von Laufzeitfehlern nützlich, um zu sehen, ob der Linker dafür verantwortlich ist. Diese Einstellung wird in der Regel nicht für Produktionsbuilds empfohlen.

Die Option **Link SDK Assemblies** (SDK-Assemblys verknüpfen) verknüpft nur [Assemblys, die in Xamarin.Android enthalten sind](~/cross-platform/internals/available-assemblies.md).
Alle anderen Assemblys (z.B. Code) sind nicht verknüpft.

Die Option **Alle Assemblys verknüpfen** verknüpft alle Assemblys. Das bedeutet, dass Ihr Code womöglich auch entfernt wird, wenn es keine statischen Verweise gibt.

Das obige Beispiel funktioniert mit den Optionen *Nicht verknüpfen* und *Link SDK Assemblies* (SDK-Assemblys verknüpfen) und schlägt mit *Alle Assemblys verknüpfen* fehl und generiert folgenden Fehler:

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```

### <a name="preserving-code"></a>Beibehalten von Code

Der Linker entfernt manchmal Code, den Sie beibehalten möchten. Zum Beispiel:

- Sie verfügen möglicherweise über Code, den Sie dynamisch über `System.Reflection.MemberInfo.Invoke` aufrufen können.

- Wenn Sie Typen dynamisch instanziieren, sollten Sie den Standardkonstruktor Ihrer Typen beibehalten.

- Wenn Sie die XML-Serialisierung verwenden, sollten Sie die Eigenschaften Ihrer Typen beibehalten.

In diesen Fällen können Sie das [Android.Runtime.Preserve](xref:Android.Runtime.PreserveAttribute)-Attribut verwenden. Jeder Member, der nicht statisch von der Anwendung verknüpft wird, wird entfernt. Dieses Attribut kann demnach verwendet werden, um Member zu kennzeichnen, auf die nicht statisch verwiesen wird, die aber noch immer von Ihrer Anwendung verwendet werden. Sie können dieses Attribut auf alle Member eines Typs oder auf den Typ selbst anwenden.

Im folgenden Beispiel wird dieses Attribut zum Beibehalten des Konstruktors der `Example`-Klasse verwendet.

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

Wenn Sie den gesamten Typ beibehalten möchten, können Sie die folgende Attributsyntax verwenden:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

Im folgenden Codefragment wird die gesamte `Example`-Klasse für die XML-Serialisierung verwendet:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

In einigen Fällen sollten Sie bestimmte Member beibehalten, jedoch nur, wenn der Containertyp beibehalten wurde. Verwenden Sie in diesen Fällen die folgende Attributsyntax:

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Wenn Sie keine Abhängigkeit von den Xamarin-Bibliotheken &ndash; übernehmen möchten, z.B. wenn Sie eine plattformübergreifende Klassenbibliothek (PCL) erstellen, können Sie weiterhin das `Android.Runtime.Preserve`-Attribut verwenden. Deklarieren Sie dazu eine `PreserveAttribute`-Klasse innerhalb des `Android.Runtime`-Namespace wie folgt:

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```

In den obigen Beispielen wird das Attribut `Preserve` im Namespace `Android.Runtime` deklariert. Sie können das Attribut `Preserve` aber in einem beliebigen Namespace verwenden, weil der Linker das Attribut anhand des Typnamens nachschlägt.

### <a name="falseflag"></a>falseflag

Wenn das Attribut [Preserve] nicht verwendet werden kann, ist es oft sinnvoll, einen Codeblock bereitzustellen, sodass der Linker davon ausgeht, dass der Typ verwendet wird, und verhindert, dass der Codeblock zur Laufzeit ausgeführt wird. Gehen Sie folgendermaßen vor, um diese Methode zu nutzen:

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```

### <a name="linkskip"></a>linkskip

Es ist möglich, anzugeben, dass mehrere vom Benutzer bereitgestellte Assemblys gar nicht verknüpft werden sollen, während gleichzeitig erlaubt wird, dass andere Benutzerassemblys mit dem *Link SDK Assemblies*-Verhalten (SDK-Assemblys verknüpfen) übersprungen werden, indem die [MSBuild-Eigenschaft „AndroidLinkSkip“](~/android/deploy-test/building-apps/build-process.md) verwendet wird:

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```

### <a name="linkdescription"></a>LinkDescription

Der [`@(LinkDescription)`](~/android/deploy-test/building-apps/build-process.md)
**Buildvorgang** kann für Dateien verwendet werden, die eine [benutzerdefinierte Linkerkonfigurationsdatei](~/cross-platform/deploy-test/linker.md) enthalten können.
hinzu. Benutzerdefinierte Linkerkonfigurationsdateien können zum Beibehalten von `internal`- oder `private`-Membern erforderlich sein, die beibehalten werden müssen.

### <a name="custom-attributes"></a>Benutzerdefinierte Attribute

Wenn eine Assembly verknüpft wird, werden die nachstehenden benutzerdefinierten Attributtypen aus allen Membern entfernt:

- System.ObsoleteAttribute
- System.MonoDocumentationNoteAttribute
- System.MonoExtensionAttribute
- System.MonoInternalNoteAttribute
- System.MonoLimitationAttribute
- System.MonoNotSupportedAttribute
- System.MonoTODOAttribute
- System.Xml.MonoFIXAttribute

Wenn eine Assembly verknüpft wird, werden die nachstehenden benutzerdefinierten Attributtypen aus allen Membern in Releasebuilds entfernt:

- System.Diagnostics.DebuggableAttribute
- System.Diagnostics.DebuggerBrowsableAttribute
- System.Diagnostics.DebuggerDisplayAttribute
- System.Diagnostics.DebuggerHiddenAttribute
- System.Diagnostics.DebuggerNonUserCodeAttribute
- System.Diagnostics.DebuggerStepperBoundaryAttribute
- System.Diagnostics.DebuggerStepThroughAttribute
- System.Diagnostics.DebuggerTypeProxyAttribute
- System.Diagnostics.DebuggerVisualizerAttribute

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Linkerkonfiguration](~/cross-platform/deploy-test/linker.md)
- [Verknüpfung unter iOS](~/ios/deploy-test/linker.md)
