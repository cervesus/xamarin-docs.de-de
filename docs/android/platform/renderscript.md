---
title: Eine Einführung in die Renderscript
description: Dieses Handbuch stellt Renderscript, und es wird erläutert, wie der systeminterne Renderscript-API in einer Anwendung Xamarin.Android dieser Ziele API-Ebene 17 oder höher verwenden.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772015"
---
# <a name="an-introduction-to-renderscript"></a>Eine Einführung in die Renderscript

_Dieses Handbuch stellt Renderscript, und es wird erläutert, wie der systeminterne Renderscript-API in einer Anwendung Xamarin.Android dieser Ziele API-Ebene 17 oder höher verwenden._

## <a name="overview"></a>Übersicht

RenderScript ist ein Programmierframework von Google zum Verbessern der Leistung von Android-Anwendungen, die eine umfangreiche Verarbeitungsressourcen erfordern erstellt. Es ist eine low-Level, hohe Leistung-API basierend auf [C99](http://en.wikipedia.org/wiki/C99). Da es sich um eine niedrige Sicherheitsstufe API, die auf CPUs, GPUs oder DSP ausgeführt wird handelt, eignet sich Renderscript für Android-apps, die möglicherweise eine der folgenden ausführen:

* Grafik
* Image-Verarbeitung
* Verschlüsselung
* Signalverarbeitung
* Mathematische Routinen

RenderScript verwenden `clang` und kompilieren Sie die Skripts LLVM Bytecode in der APK inbegriffen ist. Wenn die app zum ersten Mal ausgeführt wird, wird der Bytecode LLVM in Computercode für die Prozessoren auf dem Gerät kompiliert werden. Diese Architektur ermöglicht eine Android-Anwendung die Vorteile der Computercode müssen die Entwickler selbst müssen sie für jeden Prozessor auf dem Gerät selbst schreiben.

Es gibt zwei Komponenten an eine Routine Renderscript:

1. **Die Laufzeit Renderscript** &ndash; Dies ist der systemeigene APIs, die zum Ausführen der Renderscript verantwortlich sind. Dies schließt alle Renderscripts für die Anwendung geschrieben.

2. **Die verwaltete Wrapper aus dem Android-Framework** &ndash; verwaltete Klassen, die eine Android-app steuern und die Interaktion mit den Renderscript Common Language Runtime und Skripts ermöglichen. Zusätzlich zu den bereitgestellten Framework-Klassen zum Steuern von der Laufzeit Renderscript die Android-toolkette den Quellcode Renderscript untersucht und Generieren von verwalteten Wrapperklassen für die Verwendung von Android-Anwendung.

Das folgende Diagramm veranschaulicht, wie diese Komponenten beziehen:

![Diagramm zur Veranschaulichung, wie das Android-Framework mit der Renderscript Runtime interagiert](renderscript-images/renderscript-01.png)

Es gibt drei wichtige Konzepte für die Verwendung von Renderscripts in einer Android-Anwendung ein:

1. **Ein Kontext** &ndash; eine verwaltete API, die vom Android SDK, die Ressourcen Renderscript reserviert und können die Android-app übergeben und Empfangen von Daten aus der Renderscript bereitgestellt.

2. **Ein _berechnen Kernel_**  &ndash; auch bekannt als die _Stamm Kernel_ oder _Kernel_, gibt diese eine Routine, die die Arbeit ausführt. Der Kernel ist vergleichbar mit dem eine C-Funktion; Es handelt sich um eine parallelisiert Routine, die über alle Daten im zugeordneten Arbeitsspeicher ausgeführt werden.

3. **Reservierter Speicher** &ndash; Daten werden in und aus einem Kernel durch Übergeben einer  _[Zuordnung](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Ein Kernel möglicherweise eine Eingabe und/oder ein ausgabedataset Zuordnung.

Die [Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) -Namespace enthält Klassen für die Interaktion mit der Laufzeit Renderscript. Insbesondere die [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) Klasse verwaltet den Lebenszyklus und die Ressourcen des Renderscript-Moduls. Die Android-app muss initialisiert werden, eine oder mehrere [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Objekte. Eine Zuordnung ist eine verwaltete API, die verantwortlich für die Zuweisung und den Zugriff auf den Speicher, der zwischen der Android-app und die Laufzeit Renderscript freigegeben werden. In der Regel wird eine Zuordnung für die Eingabe erstellt, und optional eine andere Zuordnung wird erstellt, um die Ausgabe des Kernels halten. Das Laufzeitmodul Renderscript und die zugeordneten verwalteten Wrapperklassen Zugriff auf den Arbeitsspeicher frei, die für die Zuordnungen verwalten, besteht keine Notwendigkeit für ein Android-app-Entwickler zusätzliche Arbeit leisten.

Eine Zuordnung enthält eine oder mehrere [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Elemente sind ein spezialisierter Typ, in denen Daten in jeder Zuordnung beschrieben.
Die Elementtypen der Zuordnung übereinstimmen Ausgabe die die Typen des input-Elements. Bei der Ausführung einer Renderscript wird jedes Element in der Eingabe Zuweisung parallel durchlaufen, und die Ergebnisse in die Ausgabe geschrieben Zuordnung. Es gibt zwei Arten von Elementen:

- **einfacher Typ** &ndash; konzeptionell entspricht dies dem als C-Datentyp, `float` oder ein `char`.

- **komplexer Typ** &ndash; dieses Typs ähnelt einer C `struct`.

Das Modul Renderscript führt eine Überprüfung zur Laufzeit, um sicherzustellen, dass die Elemente in jeder Zuordnung sind kompatibel mit was vom Kernel erforderlich ist. Wenn der Datentyp der Elemente in der Zuordnung nicht den Datentyp übereinstimmen, den der Kernel erwartet wird, wird eine Ausnahme ausgelöst.

Alle Renderscript-Kernels werden von einem Typ, der ein Nachfolger des umschlossen werden die [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) Klasse. Die `Script` Klasse dient zum Festlegen von Parametern für eine Renderscript, legen Sie die geeignete `Allocations`, und führen Sie die Renderscript. Es gibt zwei `Script` Unterklassen im Android SDK:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Einige der häufigeren Renderscript Aufgaben werden im Android SDK gebündelt und eine Unterklasse von zugänglich sind die [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) Klasse. Es ist nicht erforderlich, für die ein Entwickler zusätzliche Schritte, die diese Skripts in ihrer Anwendung verwenden, wie sie bereits bereitgestellt werden.

- **`ScriptC_XXXXX`** &ndash; Auch bekannt als _Benutzerskripts_, sind Skripts, die von Entwicklern geschrieben wurde und in der APK verpackt werden. Zum Zeitpunkt der Kompilierung wird die Android-toolkette verwalteten Wrapperklassen generiert, mit dem die Skripts in der Android-app verwendet werden können.
  Der Name dieser generierten Klassen ist der Name der Renderscript-Datei, die mit dem Präfix `ScriptC_`. Schreiben und Integrieren von Benutzerskripts wird nicht offiziell von Xamarin.Android und würde den Rahmen dieses Handbuchs sprengen unterstützt.

Dieser zwei Typen, nur die `StringIntrinsic` von Xamarin.Android unterstützt wird. Dieses Handbuch besprechen wie systeminterne Skripts in einer Anwendung Xamarin.Android verwendet.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch ist für Anwendungen Xamarin.Android dieser Ziel-API-Ebene 17 oder höher. Die Verwendung von _Benutzerskripts_ wird in diesem Handbuch nicht behandelt.

Die [Xamarin.Android V8 Unterstützungsbibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) Backports die intrinsische Renderscript-API für apps, die frühere Versionen von Android SDK als Ziel. Dieses Paket zu einem Projekt Xamarin.Android hinzufügen sollten-apps, die auf frühere Versionen von Android SDK, nutzen die systeminterne Skripts ermöglichen.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Verwenden von systeminternen Renderscripts in Xamarin.Android

Die systeminterne Skripts sind eine hervorragende Möglichkeit, rechenintensive computing Aufgaben im Zusammenhang mit einer minimalen Menge an zusätzlichem Code. Sie wurden optimiert, um eine optimale Leistung auf einen großen Querschnitt der Geräte bieten Hand.
Es ist nicht ungewöhnlich, dass eine systeminterne Funktion Skript 10-fach verwaltetem Code schneller und 2 bis 3 X Mal nach der als eine benutzerdefinierte Implementierung von C ausgeführt. Viele der normalen Verarbeitungsszenarien werden mit den systeminternen Skripts behandelt. Diese Liste mit den systeminternen Skripts werden die aktuellen Skripts in Xamarin.Android beschrieben:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; RGB zu verwenden eine Nachschlagetabelle 3D RGBA konvertiert. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh Leistung Renderscript APIs zum [BLAS](http://www.netlib.org/blas/). BLAS (grundlegende linearen Algebraunterprogramme) sind Routinen, die Standardbausteinen Geben Sie zum Ausführen von grundlegenden Vektor- und Matrix-Vorgänge. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; zwei Zuordnungen miteinander kombiniert.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; eine Belegung ein Gauß Blur betrifft.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; eine Zuordnung (d. h. Farben ändern, passen Sie Farbton) eine Farbmatrix betrifft.

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; eine Zuordnung eine 3 x 3-Farbmatrix betrifft.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; eine Zuordnung eine 5 x 5 Farbmatrix betrifft.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; eine systeminterne Histogramm-Filter.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; gilt eine Nachschlagetabelle pro Kanal in einen Puffer.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; -Skript für die Größe der Reservierung 2D ausführen.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; konvertiert einen YUV-Puffer in RGB.

Details zu jeder der systeminternen Skripts finden Sie in der API-Dokumentation.

Die grundlegenden Schritte für die Verwendung von Renderscript in einer Android-Anwendung werden nachfolgend beschrieben.

**Erstellen Sie einen Kontext Renderscript** &ndash; der [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) Klasse ist ein verwalteter Wrapper für den Kontext Renderscript und Initialisierung der ressourcenverwaltung, steuern und bereinigen. Das Renderscript-Objekt erstellt wird, mithilfe der `RenderScript.Create` Factorymethode, die einem Android-Kontext (wie etwa eine Aktivität) als Parameter akzeptiert. Die folgende Codezeile veranschaulicht, wie die Renderscript Kontext initialisiert werden:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Erstellen von Zuordnungen** &ndash; abhängig von der systeminterne Skript es möglicherweise erforderlich sein, erstellen Sie ein oder zwei `Allocation`s. Die [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) -Klasse verfügt über mehrere Factorymethoden unterstützen Sie beim Instanziieren einer Zuordnung für eine systeminterne Funktion. Beispielsweise veranschaulicht der folgende Codeausschnitt, wie für Ihre Bitmaps zu erstellen.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Häufig, es wird erforderlich sein, erstellen Sie eine `Allocation` zum Speichern der Ausgabedaten eines Skripts. Diese folgende Codeausschnitt zeigt, wie die `Allocation.CreateTyped` Hilfsmethode zum Instanziieren einer Sekunde `Allocation` , dass die gleiche wie beim ursprünglichen eingeben:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Instanziieren Sie das Skriptwrapper** &ndash; muss jeweils die systeminterne Skript Wrapperklassen Hilfsmethoden (in der Regel aufgerufen `Create`) zum Instanziieren ein Wrapperobjekt für dieses Skript. Der folgende Codeausschnitt ist ein Beispiel zum Instanziieren einer `ScriptIntrinsicBlur` blur-Objekt. Die `Element.U8_4` Hilfsmethode erstellt ein Element, das beschreibt, einen Datentyp, der 4 Feldern von 8-Bit-Ganzzahl ohne Vorzeichen-Werten, die für die Aufnahme der Daten geeignet ist ein `Bitmap` Objekt:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Weisen Sie Allocation(s), legen Sie Parameter & Skript ausführen** &ndash; der `Script` Klasse bietet eine `ForEach` Methode, um die Renderscript tatsächlich auszuführen. Diese Methode wird jedes durchlaufen `Element` in die `Allocation` halten die Eingabedaten. In einigen Fällen ist es möglicherweise erforderlich, eine `Allocation` , die die Ausgabe enthält.
`ForEach` überschreibt den Inhalt der Ausgabe Zuordnung. Um mit den Codeausschnitten in den vorherigen Schritten durchzuführen auf, in diesem Beispiel wird gezeigt, wie weisen eine Eingabe Reservierung, einen Parameter festlegen und schließlich führen Sie das Skript (kopieren die Ergebnisse an die Ausgabe Zuweisung):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Sie möchten sehen Sie sich die [ein Bild mit Renderscript Weichzeichnen](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/) Rezept, es ist ein vollständiges Beispiel zur Verwendung einer systeminternen Skript in Xamarin.Android.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch eingeführt Renderscript und wie es in einer Anwendung Xamarin.Android verwendet. Es wurden kurz behandelt, was Renderscript ist und seine Funktionsweise in einer Android-Anwendung. Es beschrieben einige der Hauptkomponenten Renderscript und den Unterschied zwischen _Benutzerskripts_ und _intrinsische Skripts_. Dieses Handbuch erläutert schließlich Schritte bei der Verwendung einer systeminternen Skript in einer Anwendung Xamarin.Android.



## <a name="related-links"></a>Verwandte Links

- [Android.Renderscripts namespace](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Ein Bild mit Renderscript Weichzeichnen](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Lernprogramm: Erste Schritte mit Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
