---
title: Ziel-C-Selektoren in xamarin. IOS
description: In diesem Dokument wird erläutert, wie mit Ziel-C-Selektoren aus C#interagiert wird. Darin wird beschrieben, wie Selektoren und technische Überlegungen aufgerufen werden, die in diesem Fall berücksichtigt werden müssen.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/12/2017
ms.openlocfilehash: 79f226c137c3ab6b1dd2de9f92cb868056aa9d59
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022287"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Ziel-C-Selektoren in xamarin. IOS

Die Sprache "Ziel-C" basiert auf *Selektoren*. Eine Auswahl ist eine Nachricht, die an ein Objekt oder eine *Klasse*gesendet werden kann. [Xamarin. IOS](~/ios/internals/api-design/index.md) ordnet instanzselektoren Instanzmethoden und Klassenselektoren zu statischen Methoden zu.

Im Gegensatz zu normalen C-Funktionen C++ (und wie Element Funktionen) ist es nicht möglich, einen Selektor direkt mithilfe von [P/Aufruf](https://www.mono-project.com/docs/advanced/pinvoke/) aufzurufen. Selektoren werden stattdessen mithilfe des [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
Funktion.

Weitere Informationen zu Nachrichten in Ziel-C finden Sie im Handbuch [Arbeiten mit Objekten](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) von Apple.

## <a name="example"></a>Beispiel

Angenommen, Sie möchten den [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
Auswahl auf [`NSString`](https://developer.apple.com/documentation/foundation/nsstring).
Die-Deklaration (aus der Apple-Dokumentation) lautet:

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

Diese API weist die folgenden Eigenschaften auf:

- Der Rückgabetyp ist für die Unified API `CGSize`.
- Der `font`-Parameter ist ein [uifont](xref:UIKit.UIFont) -Objekt (und ein Typ (indirekt), der von [NSObject](xref:Foundation.NSObject)abgeleitet und [System. IntPtr](xref:System.IntPtr)zugeordnet wird.
- Der `width`-Parameter, ein `CGFloat`, wird `nfloat`zugeordnet.
- Der `lineBreakMode`-Parameter ( [`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc)) wurde in xamarin. IOS bereits als [`UILineBreakMode`](xref:UIKit.UILineBreakMode) gebunden.
Enumeration.

Wenn Sie alles zusammenbringen, sollte die `objc_msgSend` Deklaration entsprechen:

```csharp
CGSize objc_msgSend(
    IntPtr target, 
    IntPtr selector, 
    IntPtr font, 
    nfloat width, 
    UILineBreakMode mode
);
```

Deklarieren Sie Sie wie folgt:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Um diese Methode aufzurufen, verwenden Sie Code wie den folgenden:

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, 
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

Hätte der zurückgegebene Wert eine Struktur mit einer Größe von weniger als 8 Bytes (z. b. der ältere `SizeF`, die vor dem Wechsel zu den vereinheitlichten APIs verwendet wurde), hätte der obige Code im Simulator ausgeführt, aber auf dem Gerät abgestürzt. Um einen Selektor aufzurufen, der einen Wert kleiner als 8 Bits zurückgibt, deklarieren Sie die `objc_msgSend_stret`-Funktion:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

Um diese Methode aufzurufen, verwenden Sie Code wie den folgenden:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, 
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## <a name="invoking-a-selector"></a>Aufrufen eines Selektor

Das Aufrufen eines Selektor umfasst drei Schritte:

1. Das Auswahl Ziel erhalten.
2. Den Auswahl Namen erhalten.
3. Wenden Sie `objc_msgSend` mit den entsprechenden Argumenten an.

### <a name="selector-targets"></a>Auswahl Ziele

Ein Auswahl Ziel ist entweder eine Objektinstanz oder eine Ziel-C-Klasse. Wenn das Ziel eine Instanz ist und von einem gebundenen xamarin. IOS-Typ stammt, verwenden Sie die [`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle) -Eigenschaft.

Wenn das Ziel eine Klasse ist, verwenden Sie [`ObjCRuntime.Class`](xref:ObjCRuntime.Class) , um einen Verweis auf die Klasseninstanz zu erhalten, und verwenden Sie dann die [`Class.Handle`](xref:ObjCRuntime.Class.Handle) -Eigenschaft.

### <a name="selector-names"></a>Auswahl Namen

Selektor-Namen sind in der Apple-Dokumentation aufgeführt. [`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc) enthält z. b. [`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) -und [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) -Selektoren. Die eingebetteten und nachfolgenden Doppelpunkte sind Teil des Auswahl namens und können nicht ausgelassen werden.

Wenn Sie einen Auswahl Namen haben, können Sie dafür eine [`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector) Instanz erstellen.

### <a name="calling-objc_msgsend"></a>Aufrufen von objc_msgSend

`objc_msgSend` sendet eine Meldung (Selector) an ein-Objekt. Diese Funktions Familie erfordert mindestens zwei erforderliche Argumente: das Auswahl Ziel (eine Instanz oder ein Klassen handle), den Selektor selbst und alle Argumente, die für die Auswahl erforderlich sind. Die Instance-und Selector-Argumente müssen `System.IntPtr`werden, und alle verbleibenden Argumente müssen mit dem Typ, der vom Selektor erwartet wird, identisch sein, z. b. eine `nint` für einen `int`oder eine `System.IntPtr` für alle `NSObject`abgeleiteten Typen. Verwenden Sie die [`NSObject.Handle`](xref:Foundation.NSObject.Handle)
Eigenschaft zum Abrufen eines `IntPtr` für eine Instanz des Typs "Instance-C".

Es gibt mehr als eine `objc_msgSend`-Funktion:

- Verwenden Sie [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) für Selektoren, die eine Struktur zurückgeben. Bei ARM umfasst dies alle Rückgabe Typen, die keine Enumeration sind, oder einen der integrierten C-Typen (`char`, `short`, `int`, `long`, `float`, `double`). Bei x86 (dem Simulator) muss diese Methode für alle Strukturen verwendet werden, die größer als 8 Bytes sind (`CGSize` beträgt 8 Bytes und `objc_msgSend_stret` im Simulator nicht verwendet). 
- Verwenden Sie [`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) für Selektoren, die nur einen Gleit Komma Wert auf x86 zurückgeben. Diese Funktion muss nicht auf Arm verwendet werden. Verwenden Sie stattdessen `objc_msgSend`. 
- Die Main [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) -Funktion wird für alle anderen Selectors verwendet.

Nachdem Sie entschieden haben, welche `objc_msgSend` Funktionen Sie aufrufen müssen (Simulator und Gerät können jeweils eine andere Methode erfordern), können Sie eine normale [`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute) -Methode verwenden, um die Funktion für den späteren Aufruf zu deklarieren.

Eine Reihe Vorgemachter `objc_msgSend` Deklarationen finden Sie in `ObjCRuntime.Messaging`.

## <a name="different-invocations-on-simulator-and-device"></a>Verschiedene Aufrufe auf Simulator und Gerät

Wie oben beschrieben, hat Ziel-C drei Arten von `objc_msgSend` Methoden: eine für reguläre Aufrufe, eine für Aufrufe, die Gleit Komma Werte zurückgeben (nur x86) und eine für Aufrufe, die Struktur Werte zurückgeben. Letztere enthält das Suffix `_stret` in `ObjCRuntime.Messaging`.

Wenn Sie eine Methode aufrufen, die bestimmte Strukturen zurückgibt (Regeln, die unten beschrieben werden), müssen Sie die Methode mit dem Rückgabewert als ersten Parameter als `out` Wert aufrufen:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Die Regel für den Verwendungs Zeitpunkt der `_stret_` Methode unterscheidet sich von x86 und Arm.
Wenn Sie möchten, dass die Bindungen sowohl auf dem Simulator als auch auf dem Gerät funktionieren, fügen Sie Code wie den folgenden hinzu:

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
} 
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### <a name="using-the-objc_msgsend_stret-method"></a>Verwenden der objc_msgSend_stret-Methode

Wenn Sie für Arm aufbauen, verwenden Sie die [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
für jeden Werttyp, der keine Enumeration ist, oder einen der Basis Typen für eine Enumeration (`int`, `byte`, `short`, `long`, `double`, `float`).

Verwenden Sie bei der Erstellung für x86 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
für jeden Werttyp, der keine Enumeration ist, oder einen der Basis Typen für eine Enumeration (`int`, `byte`, `short`, `long`, `double`, `float`) und dessen systemeigene Größe größer als 8 Bytes ist.

### <a name="creating-your-own-signatures"></a>Erstellen eigener Signaturen

Der folgende [GIST](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) kann verwendet werden, um bei Bedarf eigene Signaturen zu erstellen.

## <a name="related-links"></a>Verwandte Links

- Beispiel für [Ziel-C-Selektoren](https://developer.xamarin.com/samples/mac-ios/Objective-C/)
