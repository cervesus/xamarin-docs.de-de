---
title: Eine Einführung in RenderScript
description: In diesem Leitfaden wird RenderScript eingeführt und erläutert, wie die intrinsischen RenderScript-APIs in einer xamarin. Android-Anwendung verwendet werden, die auf API Level 17 oder höher ausgerichtet ist.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: fee63906d30f68d397b60a465a87bd96d4106d14
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757431"
---
# <a name="an-introduction-to-renderscript"></a>Eine Einführung in RenderScript

_In diesem Leitfaden wird RenderScript eingeführt und erläutert, wie die intrinsischen RenderScript-APIs in einer xamarin. Android-Anwendung verwendet werden, die auf API Level 17 oder höher ausgerichtet ist._

## <a name="overview"></a>Übersicht

RenderScript ist ein von Google erstelltes Programmier Framework zum Verbessern der Leistung von Android-Anwendungen, die umfangreiche Rechenressourcen erfordern. Es handelt sich um eine leistungsstarke API, die auf [C99](https://en.wikipedia.org/wiki/C99)basiert. Da es sich um eine API auf niedriger Ebene handelt, die auf CPUs, GPUs oder DSPs ausgeführt wird, eignet sich RenderScript gut für Android-Apps, die möglicherweise eine der folgenden Aktionen ausführen müssen:

- Grafik
- Bildverarbeitung
- Verschlüsselungs
- Signal Verarbeitung
- Mathematische Routinen

RenderScript verwendet `clang` und kompiliert die Skripts für den llvm-Bytecode, der in das APK gebündelt ist. Wenn die APP zum ersten Mal ausgeführt wird, wird der llvm-Bytecode für die Prozessoren auf dem Gerät in den Computercode kompiliert. Diese Architektur ermöglicht es einer Android-Anwendung, die Vorteile von Computercode auszunutzen, ohne dass die Entwickler selbst diese für jeden Prozessor auf dem Gerät selbst schreiben müssen.

Eine RenderScript-Routine umfasst zwei Komponenten:

1. **Die RenderScript-Laufzeit** &ndash; Dies sind die nativen APIs, die für die Ausführung von RenderScript zuständig sind. Dies schließt alle renderscripts ein, die für die Anwendung geschrieben wurden.

2. **Verwaltete Wrapper aus dem Android-Framework** &ndash; Verwaltete Klassen, die es einer Android-App ermöglichen, die RenderScript-Laufzeit und Skripts zu steuern und damit zu interagieren. Zusätzlich zu den vom Framework bereitgestellten Klassen zum Steuern der RenderScript-Laufzeit untersucht die Android-Toolkette den RenderScript-Quellcode und generiert verwaltete Wrapper Klassen für die Verwendung durch die Android-Anwendung.

Das folgende Diagramm veranschaulicht die Beziehung zwischen diesen Komponenten:

![Diagramm, das veranschaulicht, wie das Android-Framework mit der RenderScript-Laufzeit interagiert](renderscript-images/renderscript-01.png)

Es gibt drei wichtige Konzepte für die Verwendung von renderscripts in einer Android-Anwendung:

1. **Ein Kontext** &ndash; Eine verwaltete API, die von der Android SDK bereitgestellt wird, die Ressourcen für RenderScript zuordnet, und ermöglicht der Android-App das übergeben und empfangen von Daten aus dem RenderScript.

2. **Ein _computekernel_**  Dies wird auch als _kernkernel_ oder Kernel bezeichnet. Dies ist eine Routine, die die Arbeit erledigt. &ndash; Der Kernel ähnelt einer C-Funktion. Dabei handelt es sich um eine parallelisierbare Routine, die über alle Daten im zugeordneten Arbeitsspeicher ausgeführt wird.

3. **Zugewiesener Arbeitsspeicher** &ndash; Daten werden über eine _[Zuordnung](xref:Android.Renderscripts.Allocation)_ an den und aus einem Kernel übermittelt. Ein Kernel kann über eine Eingabe und/oder eine Ausgabe Zuordnung verfügen.

Der [Android. renderscripts](xref:Android.Renderscripts) -Namespace enthält die Klassen für die Interaktion mit der RenderScript-Laufzeit. Insbesondere verwaltet die [`Renderscript`](xref:Android.Renderscripts.RenderScript) -Klasse den Lebenszyklus und die Ressourcen der RenderScript-Engine. Die Android-App muss mindestens eine Initialisierung durch[`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
Gütern. Eine Zuordnung ist eine verwaltete API, die für die Zuordnung und den Zugriff auf den Arbeitsspeicher verantwortlich ist, der von der Android-App und der RenderScript-Laufzeit gemeinsam genutzt wird. In der Regel wird eine Zuordnung für die Eingabe erstellt, und optional wird eine andere Zuordnung erstellt, um die Ausgabe des Kernels zu speichern. Das RenderScript-Lauf Zeit Modul und die zugeordneten verwalteten Wrapper Klassen verwalten den Zugriff auf den Arbeitsspeicher, der von den Belegungen belegt wird, es ist nicht erforderlich, dass ein Android-App-Entwickler zusätzliche Aufgaben ausführt.

Eine Zuordnung enthält mindestens ein [Android. renderscripts. Elements](xref:Android.Renderscripts.Element).
Elemente sind ein spezieller Typ, der Daten in jeder Zuordnung beschreibt.
Die Element Typen der Ausgabe Zuordnung müssen mit den Typen des input-Elements identisch sein. Bei der Ausführung durchläuft ein RenderScript alle Elemente in der Eingabe Zuordnung parallel und schreibt die Ergebnisse in die Ausgabe Zuordnung. Es gibt zwei Arten von Elementen:

- **einfacher Typ** Konzeptionell ist dies das gleiche wie ein C- `float` Datentyp oder ein `char`. &ndash;

- **komplexer Typ** Dieser Typ ähnelt einem C `struct`. &ndash;

Die RenderScript-Engine führt eine Lauf Zeit Überprüfung durch, um sicherzustellen, dass die Elemente in den einzelnen Zuordnungen mit den für den Kernel erforderlichen Informationen kompatibel sind. Wenn der Datentyp der Elemente in der Zuordnung nicht mit dem Datentyp, der vom Kernel erwartet wird, identisch ist, wird eine Ausnahme ausgelöst.

Alle RenderScript-Kernel werden von einem Typ umschließt, der ein Nachfolger von ist.[`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script)
-Klasse. Die `Script` -Klasse wird verwendet, um Parameter für ein RenderScript festzulegen, `Allocations`den entsprechenden festzulegen und das RenderScript auszuführen. Der Android SDK enthält `Script` zwei Unterklassen:

- **`Android.Renderscripts.ScriptIntrinsic`** Einige der gängigeren RenderScript-Aufgaben werden in der Android SDK gebündelt und können von einer Unterklasse der [scriptintrinsic](xref:Android.Renderscripts.ScriptIntrinsic) -Klasse aufgerufen werden. &ndash; Es ist nicht erforderlich, dass ein Entwickler zusätzliche Schritte durchführt, um diese Skripts in Ihrer Anwendung zu verwenden, da Sie bereits bereitgestellt wurden.

- **`ScriptC_XXXXX`** Diese Skripts werden auch als _Benutzer Skripts_bezeichnet und sind Skripts, die von Entwicklern geschrieben und im APK verpackt werden. &ndash; Zum Zeitpunkt der Kompilierung generiert die Android-Toolkette verwaltete Wrapper Klassen, mit denen die Skripts in der Android-App verwendet werden können.
  Der Name dieser generierten Klassen ist der Name der RenderScript-Datei mit `ScriptC_`dem Präfix. Das Schreiben und Einbinden von Benutzer Skripts wird von xamarin. Android nicht offiziell unterstützt und geht über den Rahmen dieses Handbuchs hinaus.

Von diesen beiden Typen wird nur der `StringIntrinsic` von xamarin. Android unterstützt. In dieser Anleitung wird erläutert, wie systeminterne Skripts in einer xamarin. Android-Anwendung verwendet werden.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch richtet sich an xamarin. Android-Anwendungen, die auf API Level 17 oder höher abzielen. Die Verwendung von _Benutzer Skripts_ wird in diesem Handbuch nicht behandelt.

Die [xamarin. Android-Unterstützung für die V8-Unterstützung](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) für apps, die auf ältere Versionen der Android SDK abzielen, wird von der xamarin. Android V8-Unterstützungs Bibliothek zurückportiert. Durch das Hinzufügen dieses Pakets zu einem xamarin. Android-Projekt sollten apps, die auf ältere Versionen des Android SDK abzielen, die systeminternen Skripts nutzen können.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Verwenden von systeminternen renderscripts in xamarin. Android

Die systeminternen Skripts eignen sich hervorragend zum Durchführen intensiver Rechenaufgaben mit minimalem zusätzlichen Code. Sie wurden Hand optimiert, um eine optimale Leistung für einen großen Kreuz Abschnitt von Geräten zu ermöglichen.
Es ist nicht ungewöhnlich, dass ein System internes Skript zehnmal schneller als verwalteter Code ausgeführt wird und 2-3-Mal nach der Implementierung einer benutzerdefinierten C-Implementierung ausgeführt wird. Viele der typischen Verarbeitungs Szenarien werden durch die systeminternen Skripts abgedeckt. In dieser Liste der systeminternen Skripts werden die aktuellen Skripts in xamarin. Android beschrieben:

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; Konvertiert RGB in RGBA mithilfe einer 3D-Nachschlage Tabelle. 

- [Scriptintrinsicblas](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) Provideshigh Performance RenderScript-APIs an [Blas.](http://www.netlib.org/blas/) &ndash; Die Klassen (grundlegende lineare Algebra-Unterprogramme) sind Routinen, die Standard Bausteine zum Durchführen von grundlegenden Vektor-und Matrix Vorgängen bereitstellen. 

- [Scriptintrinsicblend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; Kombiniert zwei Zuordnungen.

- [Scriptintrinsicblur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; Wendet einen gauschen Weichzeichner auf eine Zuordnung an.

- [Scriptintrinsiccolormatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; Wendet eine Farbmatrix auf eine Zuordnung an (d.h. Farben ändern, Farbton anpassen).

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; Wendet eine 3X3-Farbmatrix auf eine Zuordnung an.

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; Wendet eine 5 x 5-Farbmatrix auf eine Zuordnung an.

- [Scriptintrinsichistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) &ndash; Ein System interner histogrammfilter.

- [Scriptintrinsiclut](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; Wendet eine Such Tabelle pro Kanal auf einen Puffer an.

- [Scriptintrinsikresize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash; Skript zum Durchführen der Größenänderung einer 2D-Zuordnung.

- [Scriptintrinsicyuvtor GB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; Konvertiert einen YUV-Puffer in RGB.

Ausführliche Informationen zu den einzelnen systeminternen Skripts finden Sie in der API-Dokumentation.

Im folgenden werden die grundlegenden Schritte zum Verwenden von RenderScript in einer Android-Anwendung beschrieben.

**Erstellen eines RenderScript-Kontexts** &ndash; Das[`Renderscript`](xref:Android.Renderscripts.RenderScript)
die Klasse ist ein verwalteter Wrapper um den RenderScript-Kontext und steuert die Initialisierung, Ressourcenverwaltung und Bereinigung. Das RenderScript-Objekt wird mithilfe der `RenderScript.Create` Factorymethode erstellt, die einen Android-Kontext (z. b. eine Aktivität) als Parameter annimmt. Die folgende Codezeile veranschaulicht, wie der RenderScript-Kontext initialisiert wird:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

Zuordnungen **Erstellen** Abhängig vom systeminternen Skript kann es erforderlich sein, ein oder zwei `Allocation`n zu erstellen. &ndash; Die[`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
die-Klasse verfügt über mehrere Factorymethoden zum Instanziieren einer Zuordnung für eine systeminterne. Der folgende Code Ausschnitt veranschaulicht beispielsweise, wie eine Zuordnung für Bitmaps erstellt wird.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Häufig muss ein `Allocation` erstellt werden, um die Ausgabedaten eines Skripts zu speichern. Der folgende Code Ausschnitt zeigt, wie das `Allocation.CreateTyped` -Hilfsprogramm verwendet wird, um ein zweites `Allocation` zu instanziieren, das den gleichen Typ wie das ursprüngliche hat:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Instanziieren des Skript Wrappers** Jede der systeminternen Skript Wrapper Klassen sollte über Hilfsmethoden (in der `Create`Regel genannt) zum Instanziieren eines Wrapper Objekts für dieses Skript verfügen. &ndash; Der folgende Code Ausschnitt ist ein Beispiel für das Instanziieren eines `ScriptIntrinsicBlur` weichzeichenobjekts. Die `Element.U8_4` -Hilfsmethode erstellt ein-Element, das einen-Datentyp beschreibt, der vier Felder mit 8-Bit-ganzzahligen Werten ohne Vorzeichen enthält, die `Bitmap` zum Speichern der Daten eines-Objekts geeignet sind:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Zuweisen von Zuordnungen, Festlegen von Parametern, & Ausführen eines Skripts** Die-Klasse stellt `ForEach` eine Methode bereit, um das RenderScript-Skript tatsächlich auszuführen. `Script` &ndash; Diese Methode durchläuft alle `Element` in der `Allocation` , die die Eingabedaten enthält. In einigen Fällen kann es erforderlich sein, eine `Allocation` bereitzustellen, die die Ausgabe enthält.
`ForEach`der Inhalt der Ausgabe Zuordnung wird von überschrieben. Um mit den Code Ausschnitten aus den vorherigen Schritten fortfahren zu können, wird in diesem Beispiel gezeigt, wie Sie eine Eingabe Zuordnung zuweisen, einen Parameter festlegen und schließlich das Skript ausführen (Kopieren der Ergebnisse in die Ausgabe Zuordnung):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Vielleicht möchten Sie das [Bild mit der RenderScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) -Rezept-Grafik weich sehen. es ist ein vollständiges Beispiel für die Verwendung eines intrinsischen Skripts in xamarin. Android.

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde RenderScript eingeführt und erläutert, wie Sie es in einer xamarin. Android-Anwendung verwenden. Es wurde kurz erläutert, was RenderScript ist und wie es in einer Android-Anwendung funktioniert. Es wurden einige der Hauptkomponenten in RenderScript und der Unterschied zwischen _Benutzer_ Skripts und _INSTRINC_-Skripts beschrieben. Schließlich wurden in diesem Handbuch die Schritte zur Verwendung eines intrinsischen Skripts in einer xamarin. Android-Anwendung erläutert.

## <a name="related-links"></a>Verwandte Links

- [Android. renderscripts-Namespace](xref:Android.Renderscripts)
- [Zeichnen eines Bilds mit RenderScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Tutorial: Einstieg in RenderScript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
