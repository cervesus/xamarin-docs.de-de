---
title: Eine Einführung in die Renderscript
description: Dieses Handbuch stellt Renderscript, und es wird erläutert, wie die systeminterne Renderscript APIs in einer Xamarin.Android-Anwendung dieser Ziele API-Ebene 17 oder höher verwenden.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 8364310d23739c05ff97ea8aa8fa4c56f89ea40c
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670726"
---
# <a name="an-introduction-to-renderscript"></a>Eine Einführung in die Renderscript

_Dieses Handbuch stellt Renderscript, und es wird erläutert, wie die systeminterne Renderscript APIs in einer Xamarin.Android-Anwendung dieser Ziele API-Ebene 17 oder höher verwenden._

## <a name="overview"></a>Übersicht

RenderScript ist ein Programmierframework zum Verbessern der Leistung des Android-Anwendungen, die umfangreiche Compute-Ressourcen erfordern von Google entwickelt. Es ist eine low-Level, high-Performance-API basierend auf [C99](https://en.wikipedia.org/wiki/C99). Da es sich um eine niedrige Sicherheitsstufe API, die auf CPUs oder GPUs DSPs ausgeführt wird handelt, ist Renderscript gut geeignet für Android-apps, die möglicherweise eine der folgenden ausführen:

* Grafik
* Bildverarbeitung
* Verschlüsselung
* Signalverarbeitung
* Mathematische Routinen

RenderScript verwenden `clang` und kompilieren Sie die Skripts LLVM-Bytecode, der in der APK im Paket enthalten ist. Wenn die app zum ersten Mal ausgeführt wird, wird der LLVM-Byte-Code in Computercode für die Prozessoren auf dem Gerät kompiliert werden. Diese Architektur ermöglicht eine Android-Anwendung, die Vorteile der Computercode müssen die Entwickler müssen sie für jeden Prozessor auf dem Gerät selbst schreiben auszunutzen.

Es gibt zwei Komponenten, die an eine Routine Renderscript:

1. **Die Laufzeit Renderscript** &ndash; Dies ist die nativen APIs, die für die Ausführung der Renderscript verantwortlich sind. Dies schließt alle Renderscripts für die Anwendung geschrieben.

2. **Die verwaltete Wrapper aus dem Android-Framework** &ndash; verwaltete Klassen, mit denen eine Android-app zu steuern und Interaktion mit den Renderscript Common Language Runtime und Skripts. Zusätzlich zu den für die Steuerung der Renderscript Runtime bereitgestellte Framework-Klassen die Android-toolkette den Quellcode Renderscript untersucht und verwaltete Wrapperklassen für die Verwendung von der Android-Anwendung zu generieren.

Das folgende Diagramm veranschaulicht, wie diese Komponenten beziehen:

![Diagramm zur Veranschaulichung der Interaktion zwischen der Android-Framework mit der Renderscript-Runtime](renderscript-images/renderscript-01.png)

Es gibt drei wichtige Konzepte für die Verwendung von Renderscripts in einer Android-Anwendung:

1. **Ein Kontext** &ndash; eine verwaltete API, die vom Android SDK, die Ressourcen Renderscript zugewiesen und können die Android-app übergeben und Empfangen von Daten aus der Renderscript bereitgestellt.

2. **Ein _compute-Kernel_**  &ndash; auch bekannt als die _Stamm Kernel_ oder _Kernel_wird in diesem eine Routine, die die Arbeit. Der Kernel ist eine C-Funktion sehr ähnlich, Es ist eine parallelisierbar Routine, die über alle Daten im zugeordneten Speicher ausgeführt wird.

3. **Zugeordneter Arbeitsspeicher** &ndash; Daten in und aus einem Kernel durch Übergeben einer  _[Zuordnung](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Ein Kernel ist möglicherweise eine Eingabe und/oder ein ausgabedataset Zuordnung.

Die [Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) Namespace enthält Klassen für die Interaktion mit der Laufzeit Renderscript. Insbesondere die [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) Klasse verwaltet den Lebenszyklus und die Ressourcen der Renderscript-Engine. Die Android-app muss mindestens eine initialisiert. [`Android.Renderscripts.Allocation`](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)
-Objekte. Eine Zuordnung ist eine verwaltete API, die für die Belegung und den Zugriff auf den Arbeitsspeicher, der von der Android-app und die Laufzeit Renderscript gemeinsam verantwortlich ist. Klicken Sie in der Regel nur einer Zuordnung wird erstellt, für die Eingabe, und optional eine andere Zuordnung erstellt, die die Ausgabe des Kernels enthält. Die Renderscript-Runtime-Engine und die zugehörigen verwalteten Wrapperklassen Zugriff auf den Speicher frei, die Zuordnungen verwalten, besteht keine Notwendigkeit für ein Android-app-Entwickler zusätzliche Arbeit leisten.

Eine Zuordnung enthält eine oder mehrere [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Elemente sind ein spezieller Typ, die Daten in jeder Zuordnung beschreiben.
Die Elementtypen der Ausgabe Zuordnung den Typen des input-Elements entsprechen muss. Bei der Ausführung eine Renderscript wird jedes Element in der Eingabe Zuordnungseinheit parallel durchlaufen, und die Ergebnisse in die Ausgabe geschrieben Zuordnung. Es gibt zwei Arten von Elementen:

- **einfacher Typ** &ndash; konzeptionell Dies entspricht der C-Datentyp, `float` oder `char`.

- **komplexer Typ** &ndash; dieser Typ ist ein von C ähnelt `struct`.

Die Renderscript-Engine führt eine Überprüfung zur Laufzeit, um sicherzustellen, dass die Elemente in jeder Zuordnung sind kompatibel mit, was durch den Kernel erforderlich ist. Wenn Sie der Datentyp der Elemente in der Zuordnung, den der Kernel erwartet wird den Datentyp nicht übereinstimmen, wird eine Ausnahme ausgelöst werden.

Alle Renderscript Kernel werden von einem Typ, der ein Nachfolger des umschlossen werden die [`Android.Renderscripts.Script`](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/)
-Klasse. Die `Script` Klasse dient zum Festlegen von Parametern für eine Renderscript legen Sie die entsprechende `Allocations`, und führen Sie die Renderscript. Es gibt zwei `Script` Unterklassen im Android SDK:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Einige der häufigeren Renderscript Aufgaben werden im Android SDK gebündelt, und durch eine Unterklasse von zugegriffen werden kann die [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) Klasse. Besteht keine Notwendigkeit für Entwickler zusätzliche Schritte, die diese Skripts in ihrer Anwendung verwenden, wie sie bereits bereitgestellt werden.

- **`ScriptC_XXXXX`** &ndash; Auch bekannt als _Benutzerskripts_, sind Skripts, die von Entwicklern geschrieben wurde und in das APK verpackt werden. Zum Zeitpunkt der Kompilierung wird die Android-toolkette verwalteten Wrapperklassen generiert, mit dem die Skripts in der Android-app verwendet werden können.
  Der Name dieser generierten Klassen ist der Name der Renderscript-Datei mit dem Präfix `ScriptC_`. Schreiben und Integrieren von Benutzerskripts wird nicht offiziell von Xamarin.Android und über den Rahmen dieses Handbuchs hinaus unterstützt.

Diese zwei Typen, nur die `StringIntrinsic` von Xamarin.Android unterstützt wird. Dieses Handbuch wird wie systeminterne-Skripts in einer Xamarin.Android-Anwendung verwendet wird.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch ist für Xamarin.Android-Anwendungen, Ziel-API-Ebene 17 oder höher. Die Verwendung von _Benutzerskripts_ wird in diesem Handbuch nicht behandelt.

Die [Xamarin.Android V8-Unterstützungsbibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) Backports die intrinsische Renderscript-APIs für apps, die ältere Versionen des Android SDK als Ziel. Dieses Paket zu einer Xamarin.Android-Projekt hinzufügen sollten, frühere Versionen abzielen des Android SDK, die systeminterne Skripts nutzen apps zulassen.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Verwenden von systeminternen Renderscripts in Xamarin.Android

Die systeminterne Skripts sind eine hervorragende Möglichkeit zum Ausführen rechenintensiver Aufgaben mit einer minimalen Menge an zusätzlichem Code. Sie wurden manuell optimiert, um eine optimale Leistung auf einen großen Querschnitt von Geräten zu bieten.
Es ist nicht ungewöhnlich, dass für eine systeminterne Skript, das 10 Mal schneller als verwalteten Code und 2 und 3 X Mal nach der als eine benutzerdefinierte C#-Implementierung ausgeführt. Viele der normalen Verarbeitungsszenarios werden von den systeminternen Skripts behandelt. Diese Liste mit den systeminternen Skripts werden die aktuellen Skripts in Xamarin.Android beschrieben:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; RGB konvertiert, um eine 3D Nachschlagetabelle RGBA. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh Leistung Renderscript APIs zum [BLAS](http://www.netlib.org/blas/). Die BLAS (einfache lineare Algebraunterprogramme) sind Routinen, die Standardbausteinen zum Ausführen von grundlegenden Vektor- und matrixoperationen angeben. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; zwei Zuordnungen miteinander kombiniert.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; eine Zuordnung einer Gaußschen Blur gilt.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; eine Zuordnung (d. h. Farben ändern, passen Sie Hue) eine Farbmatrix gilt.

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; eine Zuordnung eine 3 x 3-Farbmatrix gilt.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; eine Zuordnung eine Farbmatrix von 5 x 5 gilt.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; einen systeminternen Histogramm-Filter.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; gilt eine Nachschlagetabelle pro Kanal in einen Puffer.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; Skript zum Ändern der Größe des der 2D Reservierung ausführen.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; konvertiert einen Puffer für die YUV in RGB.

Details zu den einzelnen systeminterne Skripts finden Sie in der API-Dokumentation.

Die grundlegenden Schritte für die Verwendung von Renderscript in einer Android-Anwendung beschrieben werden.

**Erstellen Sie einen Kontext Renderscript** &ndash; der [`Renderscript`](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/)
Klasse ist, einen verwalteten Wrapper für den Kontext Renderscript und steuern Sie die Initialisierung der ressourcenverwaltung, und bereinigt wird. Das Renderscript-Objekt erstellt wird, mit der `RenderScript.Create` -Factorymethode, die einem Android-Kontext (z. B. eine Aktivität) als Parameter annimmt. Die folgende Codezeile veranschaulicht, wie den Renderscript-Kontext zu initialisieren:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Erstellen von Zuordnungen** &ndash; abhängig von der systeminterne Skript, es kann erforderlich sein, erstellen Sie ein oder zwei `Allocation`s. Die [`Android.Renderscripts.Allocation`](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)
Klasse verfügt über mehrere Factorymethoden beim Instanziieren einer Zuordnung für eine systeminterne Funktion. Beispielsweise veranschaulicht der folgende Codeausschnitt, wie Zuordnung für Bitmaps erstellen.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Häufig ist es werden zum Erstellen einer `Allocation` zum Speichern der Ausgabedaten eines Skripts. Diese folgende Codeausschnitt zeigt, wie Sie mit der `Allocation.CreateTyped` Hilfsmethode zum Instanziieren Sie ein zweites `Allocation` , dass vom selben, wie das Original Typ:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Instanziieren Sie den Skript-Wrapper** &ndash; die systeminterne Skript-Wrapper-Klassen müssen Helper-Methoden (in der Regel aufgerufen `Create`) für das Instanziieren ein Wrapperobjekt für das Skript. Der folgende Codeausschnitt ist ein Beispiel zum Instanziieren einer `ScriptIntrinsicBlur` blur-Objekt. Die `Element.U8_4` Hilfsmethode erstellt ein Element, einen Datentyp, der 4 Feldern von 8-Bit-Ganzzahl ohne Vorzeichen-Werten, die zur Aufnahme der Daten geeignet ist, beschreibt, eine `Bitmap` Objekt:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Weisen Sie Allocation(s), Festlegen von Parametern und Skript ausführen** &ndash; der `Script` -Klasse stellt eine `ForEach` Methode, um die Renderscript tatsächlich auszuführen. Diese Methode führt eine Iteration für alle `Element` in die `Allocation` , der die Eingabedaten enthält. In einigen Fällen ist es möglicherweise notwendig, eine `Allocation` , die die Ausgabe enthält.
`ForEach` überschreibt den Inhalt der Ausgabe Zuordnung. Um mit den Codeausschnitten in den vorherigen Schritten ausführen, dieses Beispiel zeigt, wie weisen eine Eingabe Zuordnung und Festlegen eines Parameters zum Schluss führen Sie das Skript (kopieren die Ergebnisse in der Ausgabe Zuweisung):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Möglicherweise möchten Sie sehen Sie sich die [Weichzeichnen ein Abbilds mit Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) Rezept, es ist ein vollständiges Beispiel zur Verwendung einer systeminternen Skript in Xamarin.Android.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden Renderscript und wie Sie es in einer Xamarin.Android-Anwendung verwenden. Es wurden kurz behandelt, was Renderscript ist und deren Funktionsweise in einer Android-Anwendung. Es wurde beschrieben, einige der wichtigsten Komponenten in Renderscript und den Unterschied zwischen _Benutzerskripts_ und _intrinsische Skripts_. Dieses Handbuch erläutert schließlich Schritte bei der Verwendung einer systeminternen Skript in einer Xamarin.Android-Anwendung.



## <a name="related-links"></a>Verwandte Links

- [Android.Renderscripts namespace](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Ein Bild mit Renderscript Blur](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Tutorial: Erste Schritte mit Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
