---
title: Eine Einführung in RenderScript
description: In diesem Leitfaden wird RenderScript eingeführt und erläutert, wie die systeminternen RenderScript-APIs in einer Xamarin.Android-Anwendung verwendet werden, die auf API-Ebene 17 oder höher ausgerichtet ist.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 884b69b0cdecf4f979cec314b6440974c5bac97d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019797"
---
# <a name="an-introduction-to-renderscript"></a>Eine Einführung in RenderScript

_In diesem Leitfaden wird RenderScript eingeführt und erläutert, wie die systeminternen RenderScript-APIs in einer Xamarin.Android-Anwendung verwendet werden, die auf API-Ebene 17 oder höher ausgerichtet ist._

## <a name="overview"></a>Übersicht

RenderScript ist ein von Google erstelltes Programmierframework zum Verbessern der Leistung von Android-Anwendungen, die umfangreiche Rechenressourcen erfordern. Es handelt sich um eine leistungsstarke Low-Level-API, die auf [C99](https://en.wikipedia.org/wiki/C99) basiert. Da es sich um eine Low-Level-API handelt, die auf CPUs, GPUs oder DSPs ausgeführt wird, eignet sich RenderScript gut für Android-Apps, die möglicherweise eine der folgenden Aktionen ausführen müssen:

- Grafik
- Bildverarbeitung
- Verschlüsselung
- Signalverarbeitung
- Mathematische Routinen

RenderScript verwendet `clang` und kompiliert die Skripts in LLVM-Bytecode, der im APK gebündelt ist. Wenn die App zum ersten Mal ausgeführt wird, wird der LLVM-Bytecode für die Prozessoren auf dem Gerät in Computercode kompiliert. Diese Architektur ermöglicht einer Android-Anwendung, die Vorteile von Computercode auszunutzen, ohne dass die Entwickler ihn für jeden Prozessor auf dem Gerät selbst schreiben müssen.

Eine RenderScript-Routine umfasst zwei Komponenten:

1. **Die RenderScript-Runtime** &ndash; dies sind die nativen APIs, die für die Ausführung von RenderScript zuständig sind. Dies schließt alle RenderScripts ein, die für die Anwendung geschrieben wurden.

2. **Verwaltete Wrapper aus dem Android-Framework** &ndash; verwaltete Klassen, die einer Android-App ermöglichen, die RenderScript-Runtime und Skripts zu steuern und mit ihnen zu interagieren. Zusätzlich zu den vom Framework bereitgestellten Klassen zum Steuern der RenderScript-Runtime untersucht die Android-Toolkette den RenderScript-Quellcode und generiert verwaltete Wrapperklassen zur Verwendung durch die Android-Anwendung.

Das folgende Diagramm veranschaulicht, wie diese Komponenten miteinander in Beziehung stehen:

![Diagramm, das veranschaulicht, wie das Android-Framework mit der RenderScript-Runtime interagiert](renderscript-images/renderscript-01.png)

Es gibt drei wichtige Konzepte für die Verwendung von RenderScripts in einer Android-Anwendung:

1. **Ein Kontext** &ndash; eine verwaltete, vom Android SDK bereitgestellte API, die Ressourcen für RenderScript zuweist und der Android-App ermöglicht, Daten an das RenderScript zu übergeben und von dort zu empfangen.

2. **Ein _Computekernel_** &ndash; auch als _Root-Kernel_ oder _Kernel_ bezeichnet. Dies ist eine Routine, die die Arbeit erledigt. Der Kernel ähnelt stark einer C-Funktion. Dabei handelt es sich um eine parallelisierbare Routine, die für alle Daten im zugeordneten Arbeitsspeicher ausgeführt wird.

3. **Zugeordneter Arbeitsspeicher** &ndash; Daten werden über eine _[Zuordnung](xref:Android.Renderscripts.Allocation)_ an einen und von einem Kernel übergeben. Ein Kernel kann über eine Eingabe- und/oder Ausgabezuordnung verfügen.

Der [Android.Renderscripts](xref:Android.Renderscripts)-Namespace enthält die Klassen für die Interaktion mit der RenderScript-Runtime. Insbesondere die [`Renderscript`](xref:Android.Renderscripts.RenderScript)-Klasse verwaltet den Lebenszyklus und die Ressourcen der RenderScript-Engine. Initialisieren muss die Android-App ein oder mehrere [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
-Objekte. Eine Zuordnung ist eine verwaltete API, die für die Zuordnung von Arbeitsspeicher, der von der Android-App und der RenderScript-Runtime gemeinsam genutzt wird, und den Zugriff darauf verantwortlich ist. In der Regel wird eine Zuordnung für die Eingabe erstellt, und optional wird eine andere Zuordnung erstellt, um die Ausgabe des Kernel zu speichern. Das RenderScript-Runtimemodul und die zugehörigen verwalteten Wrapperklassen verwalten den Zugriff auf den Arbeitsspeicher, der von den Zuordnungen belegt wird. Es ist nicht erforderlich, dass ein Android-App-Entwickler zusätzliche Aufgaben ausführt.

Eine Zuordnung enthält ein oder mehrere [Android.Renderscripts.Elements](xref:Android.Renderscripts.Element).
Elemente sind ein spezieller Typ, der Daten in jeder Zuordnung beschreibt.
Die Elementtypen der Ausgabezuordnung müssen mit den Typen des Eingabeelements identisch sein. Bei der Ausführung durchläuft ein RenderScript alle Elemente in der Eingabezuordnung parallel und schreibt die Ergebnisse in die Ausgabezuordnung. Es gibt zwei Typen von Elementen:

- **einfacher Typ** &ndash; konzeptionell ist dieser mit einem C-Datentyp, `float` oder `char` identisch.

- **komplexer Typ** &ndash; dieser Typ ähnelt einem C-`struct`.

Die RenderScript-Engine führt eine Runtimeüberprüfung durch, um sicherzustellen, dass die Elemente in den einzelnen Zuordnungen mit den für den Kernel erforderlichen Informationen kompatibel sind. Wenn der Datentyp der Elemente in der Zuordnung nicht mit dem Datentyp, der vom Kernel erwartet wird, identisch ist, wird eine Ausnahme ausgelöst.

Der Typ, der alle RenderScript-Kernel umschließt, ist ein Nachfolger der [`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script)
-Klasse. Die `Script`-Klasse wird verwendet, um Parameter für ein RenderScript festzulegen, die entsprechenden `Allocations` festzulegen und RenderScript auszuführen. Im Android SDK gibt es zwei `Script`-Unterklassen:

- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; einige der gängigeren RenderScript-Aufgaben werden im Android SDK gebündelt und können von einer Unterklasse der [ScriptIntrinsic](xref:Android.Renderscripts.ScriptIntrinsic)-Klasse aufgerufen werden. Ein Entwickler muss keine zusätzlichen Schritte durchführen, um diese Skripts in seiner Anwendung zu verwenden, da sie bereits bereitgestellt wurden.

- **`ScriptC_XXXXX`** &ndash; diese Skripts werden auch als _Benutzerskripts_ bezeichnet und von Entwicklern geschrieben und im APK verpackt. Zum Zeitpunkt der Kompilierung generiert die Android-Toolkette verwaltete Wrapperklassen, mit denen die Skripts in der Android-App verwendet werden können.
  Der Name dieser generierten Klassen ist der Name der RenderScript-Datei mit dem Präfix `ScriptC_`. Das Schreiben und Einbinden von Benutzerskripts wird von Xamarin.Android nicht offiziell unterstützt und überschreitet den Rahmen dieses Handbuchs.

Von diesen beiden Typen wird nur `StringIntrinsic` von Xamarin.Android unterstützt. In diesem Handbuch wird erläutert, wie systeminterne Skripts in einer Xamarin.Android-Anwendung verwendet werden.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch behandelt Xamarin.Android-Anwendungen, die auf API-Ebene 17 oder höher abzielen. Die Verwendung von _Benutzerskripts_ wird in diesem Handbuch nicht behandelt.

Die [Xamarin.Android V8-Unterstützungsbibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) erledigt die Rückportierung der internen RenderScript-APIs für Apps, die auf ältere Versionen des Android SDK abzielen. Das Hinzufügen dieses Pakets zu einem Xamarin.Android-Projekt soll Apps, die auf ältere Versionen des Android SDK abzielen, die Nutzung systeminterner Skripts ermöglichen.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Verwenden von systeminternen RenderScripts in Xamarin.Android

Die systeminternen Skripts eignen sich hervorragend zum Durchführen intensiver Rechenaufgaben mit minimalem zusätzlichem Code. Sie wurden individuell angepasst, um auf einem großen Querschnitt von Geräten optimale Leistung zu ermöglichen.
Es ist nicht ungewöhnlich, dass ein systeminternes Skript zehnmal schneller als verwalteter Code und 2-3 Mal nach einer benutzerdefinierten C-Implementierung ausgeführt wird. Viele typische Verarbeitungsszenarien werden durch die systeminternen Skripts abgedeckt. In dieser Liste der systeminternen Skripts werden die aktuellen Skripts in Xamarin.Android beschrieben:

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; konvertiert RGB mit einer 3D-Nachschlagetabelle in RGBA. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; stellt Hochleistungs-RenderScript-APIs für [BLAS](http://www.netlib.org/blas/) bereit. Die BLAS (Basic Linear Algebra Subprograms, grundlegende lineare Algebraunterprogramme) sind Routinen, die Standardbausteine zum Durchführen grundlegender Vektor- und Matrixvorgänge bereitstellen. 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; kombiniert zwei Zuordnungen.

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; wendet einen Gaußschen Weichzeichner auf eine Zuordnung an.

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; wendet eine Farbmatrix auf eine Zuordnung an (d. h. Farben ändern, Farbton anpassen).

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; wendet eine 3x3-Farbmatrix auf eine Zuordnung an.

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; wendet eine 5x5-Farbmatrix auf eine Zuordnung an.

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) &ndash; ein systeminterner Histogrammfilter.

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; wendet eine Nachschlagetabelle für die einzelnen Kanäle auf einen Puffer an.

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash; Skript zur Änderung der Größe einer 2D-Zuordnung.

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; konvertiert einen YUV-Puffer in RGB.

Ausführliche Informationen zu den einzelnen systeminternen Skripts finden Sie in der API-Dokumentation.

Im Folgenden werden die grundlegenden Schritte zum Verwenden von RenderScript in einer Android-Anwendung beschrieben.

**Erstellen eines RenderScript-Kontexts** &ndash; die [`Renderscript`](xref:Android.Renderscripts.RenderScript)
-Klasse ist ein verwalteter Wrapper, der den RenderScript-Kontext umschließt und Initialisierung, Ressourcenverwaltung und Bereinigung steuert. Das RenderScript-Objekt wird mit der `RenderScript.Create`-Factorymethode erstellt, die einen Android-Kontext (z. B. eine Aktivität) als Parameter annimmt. Die folgende Codezeile veranschaulicht, wie der RenderScript-Kontext initialisiert wird:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Erstellen von Zuordnungen** &ndash; abhängig vom systeminternen Skript kann es erforderlich sein, eine oder zwei `Allocation`s zu erstellen. Die [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
-Klasse verfügt über mehrere Factorymethoden zum Instanziieren einer Zuordnung für ein systeminternes Skript. Als Beispiel wird im folgenden Codeausschnitt die Vorgehensweise zum Erstellen einer Zuordnung für Bitmaps veranschaulicht.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Häufig muss eine `Allocation` erstellt werden, um die Ausgabedaten eines Skripts zu speichern. Der folgende Codeausschnitt zeigt, wie Sie das `Allocation.CreateTyped`-Hilfsprogramm verwenden, um eine zweite `Allocation` zu instanziieren, die den gleichen Typ wie das Original hat:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Instanziieren des Skriptwrappers** &ndash; jede der systeminternen Skriptwrapperklassen sollte über Hilfsprogrammmethoden (in der Regel `Create` genannt) zum Instanziieren eines Wrapperobjekts für dieses Skript verfügen. Der folgende Codeausschnitt ist ein Beispiel für das Instanziieren eines `ScriptIntrinsicBlur`-Weichzeichnerobjekts. Die `Element.U8_4`-Hilfsprogrammmethode erstellt ein Element, das einen 4 Felder mit ganzzahligen 8-Bit-Werten ohne Vorzeichen enthaltenden Datentyp beschreibt, der zur Speicherung der Daten eines `Bitmap`-Objekts geeignet ist:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Zuweisen von Zuordnungen, Festlegen von Parametern und Ausführen eines Skripts** &ndash; die `Script`-Klasse stellt eine `ForEach`-Methode bereit, um das RenderScript tatsächlich auszuführen. Diese Methode durchläuft jedes `Element` in der `Allocation`, die die Eingabedaten speichert. In einigen Fällen kann es erforderlich sein, eine `Allocation` bereitzustellen, die die Ausgabe speichert.
`ForEach` überschreibt den Inhalt der Ausgabezuordnung. Um mit den Codeausschnitten aus den vorherigen Schritten fortzufahren, wird in diesem Beispiel gezeigt, wie Sie eine Eingabezuordnung zuweisen, einen Parameter festlegen und schließlich das Skript ausführen (Kopieren der Ergebnisse in die Ausgabezuordnung):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Vielleicht möchten Sie das Rezept [Blur an Image with Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) (Weichzeichnen eines Bilds mit Renderscript) ausprobieren. Es ist ein vollständiges Beispiel für die Verwendung eines systeminternen Skripts in Xamarin.Android.

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde RenderScript eingeführt und erläutert, wie Sie es in einer Xamarin.Android-Anwendung verwenden. Es wurde kurz erläutert, was RenderScript ist, und wie es in einer Android-Anwendung funktioniert. Es wurden einige der Hauptkomponenten in RenderScript und der Unterschied zwischen _Benutzerskripts_ und _systeminternen Skripts_ beschrieben. Schließlich wurden in diesem Handbuch die Schritte zur Verwendung eines systeminternen Skripts in einer Xamarin.Android-Anwendung beschrieben.

## <a name="related-links"></a>Verwandte Links

- [Android.Renderscripts-Namespace](xref:Android.Renderscripts)
- [Blur an Image with Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) (Weichzeichnen eines Bilds mit Renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Tutorial: Erste Schritte mit RenderScript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
