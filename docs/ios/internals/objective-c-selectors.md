---
title: Objective-C-Selektoren in Xamarin.iOS
description: In diesem Dokument wird erläutert, wie für die Interaktion mit Objective-C-Selektoren in c# wird. Es wird beschrieben, wie zum Aufrufen der Auswahlzeiger und technische Aspekte, die berücksichtigt werden müssen dabei.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/12/2017
ms.openlocfilehash: 15db59945f482728f760006095e294bc5628c8bd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036353"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Objective-C-Selektoren in Xamarin.iOS

Die Objective-C-Sprache basiert auf *Selektoren*. Auswahl wird eine Meldung, die auf ein Objekt gesendet werden kann oder ein *Klasse*. [Xamarin.iOS](~/ios/internals/api-design/index.md) Zuordnungen Selektoren in Instanzmethoden-Instanz und Klasse Selektoren auf statische Methoden.

Im Gegensatz zu normalen C#-Funktionen (und wie die C++-Memberfunktionen) Sie nicht direkt aufrufen, eine Auswahl mit [P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/) stattdessen Selektoren gesendet werden, auf ein Objective-C-Klasse oder Instanz mithilfe der [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
-Funktion.

Weitere Informationen zu Meldungen, die in Objective-C, sehen Sie sich von Apple [arbeiten mit Objekten](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) Guide.

## <a name="example"></a>Beispiel

Angenommen, Sie möchten, rufen Sie die [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
Selektor auf [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring).
Die Deklaration (von Apple Dokumentation) ist:

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

Diese API weist folgende Merkmale auf:

- Der Rückgabetyp ist `CGSize` für die Unified API.
- Die `font` -Parameter ist ein [UIFont](xref:UIKit.UIFont) (und ein Typ (indirekt) abgeleitet [NSObject](xref:Foundation.NSObject), und wird [System.IntPtr](xref:System.IntPtr).
- Die `width` -Parameter ein `CGFloat`, zugeordnet ist `nfloat`.
- Die `lineBreakMode` -Parameter ein [ `UILineBreakMode` ](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc), bereits in Xamarin.iOS als gebunden wurde die [`UILineBreakMode`](xref:UIKit.UILineBreakMode)
-Enumeration.

Letzte Schritte, die `objc_msgSend` Deklaration sollte übereinstimmen:

```csharp
CGSize objc_msgSend(
    IntPtr target, 
    IntPtr selector, 
    IntPtr font, 
    nfloat width, 
    UILineBreakMode mode
);
```

Deklarieren Sie sie wie folgt:

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

Um diese Methode aufrufen, verwenden Sie Code wie den folgenden aus:

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

War der zurückgegebene Wert eine Struktur, die weniger als 8 Bytes groß war (z. B. die ältere `SizeF` vor dem Wechsel zu Unified-APIs verwendet), der obige Code würde auf dem Simulator aber abgestürzte auf dem Gerät ausgeführt haben. Um einen Selektor aufzurufen, die einen Wert kleiner als 8 Bits Größe zurückgibt, deklarieren die `objc_msgSend_stret` Funktion:

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

Um diese Methode aufrufen, verwenden Sie Code wie den folgenden aus:

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

## <a name="invoking-a-selector"></a>Aufrufen einer Auswahl

Aufrufen einer Auswahl umfasst drei Schritte:

1. Das Ziel für die Auswahl zu erhalten.
2. Ruft den Auswahlnamen.
3. Rufen Sie `objc_msgSend` mit den entsprechenden Argumenten.

### <a name="selector-targets"></a>Selector-Ziele

Ein Ziel für die Auswahl ist eine Instanz oder eine Objective-C-Klasse. Wenn das Ziel eine Instanz ist und stammt von einer Xamarin.iOS-Typ gebunden wird, verwenden Sie die [ `ObjCRuntime.INativeObject.Handle` ](xref:ObjCRuntime.INativeObject.Handle) Eigenschaft.

Wenn das Ziel einer Klasse ist, verwenden Sie [ `ObjCRuntime.Class` ](xref:ObjCRuntime.Class) verwenden, um einen Verweis auf die Instanz der Klasse zu erhalten, klicken Sie dann die [ `Class.Handle` ](xref:ObjCRuntime.Class.Handle) Eigenschaft.

### <a name="selector-names"></a>Auswahlnamen

Selector-Namen werden in der Apple Dokumentation aufgeführt. Z. B. [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring?language=objc) enthält [ `sizeWithFont:` ](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) und [ `sizeWithFont:forWidth:lineBreakMode:` ](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) Selektoren. Die eingebetteten und nachfolgende Doppelpunkte sind Teil des Namens der Auswahl und können nicht ausgelassen werden.

Wenn Sie einen Auswahlnamen haben, können Sie erstellen eine [ `ObjCRuntime.Selector` ](xref:ObjCRuntime.Selector) Instanz dafür.

### <a name="calling-objcmsgsend"></a>Aufrufen von objc_msgSend

`objc_msgSend` sendet eine Nachricht (Selektor) auf ein Objekt an. Diese Gruppe von Funktionen wird über mindestens zwei erforderliche Argumente: das Selector-Ziel (eine Instanz oder eine Klasse behandelt), der Selektor selbst und alle Argumente, die für die Auswahl erforderlich. Die Instanz und der Auswahlzeiger-Argumente müssen sein `System.IntPtr`, und alle übrigen Argumente müssen den Typ der Selektor, z. B. erwartet mit einer `nint` für eine `int`, oder ein `System.IntPtr` für alle `NSObject`-abgeleiteten Typen. Verwenden der [`NSObject.Handle`](xref:Foundation.NSObject.Handle)
-Eigenschaft zum Abrufen einer `IntPtr` für eine Instanz der Objective-C-Typ.

Es gibt mehr als ein `objc_msgSend` Funktion:

- Verwendung [ `objc_msgSend_stret` ](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) für Selektoren, die eine Struktur zurück. Auf ARM, dazu gehören alle Rückgabetypen, die keine Enumeration sind oder keines der integrierten C#-Typen (`char`, `short`, `int`, `long`, `float`, `double`). Diese Methode auf X86 (der Simulator), muss für alle Strukturen, die größer als 8 Bytes Groß verwendet werden (`CGSize` 8 Bytes und nicht `objc_msgSend_stret` im Simulator). 
- Verwendung [ `objc_msgSend_fpret` ](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) für Selektoren, die eine Gleitkommazahl zurückgeben Wert nur X86 zeigen. Diese Funktion muss nicht auf ARM verwendet werden. Verwenden Sie stattdessen `objc_msgSend`. 
- Die Main [Objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) Funktion wird für alle anderen Selektoren verwendet.

Nachdem Sie die entschieden haben `objc_msgSend` Funktion(en), die Sie aufrufen müssen (Simulator und Gerät jeweils erfordern möglicherweise eine andere Methode), können Sie einen normalen [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) Methode, um die Funktion für den späteren Aufruf deklarieren.

Eine Reihe von vorgefertigte `objc_msgSend` Deklarationen finden Sie im `ObjCRuntime.Messaging`.

## <a name="different-invocations-on-simulator-and-device"></a>Andere Aufrufe im Simulator sowie Geräte

Objective-C verfügt über drei Arten wie oben beschrieben, der `objc_msgSend` Methoden: eine für den regulären aufrufen, eine für Aufrufe, die Gleitkommawerte (nur X86) zurückgeben und eine für Aufrufe, die Strukturwerte zurückgeben. Letztere enthält das Suffix `_stret` in `ObjCRuntime.Messaging`.

Wenn Sie eine Methode aufrufen, die bestimmte Strukturen (Regeln, die unten beschriebenen) zurückgibt, rufen Sie die Methode mit dem Rückgabewert als erster Parameter als ein `out` Wert:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Die Regel für die Verwendung der `_stret_` Methode unterscheidet sich auf X86- und ARM.
Wenn Sie Ihre Bindungen auf den Simulator und dem Gerät arbeiten möchten, fügen Sie Code wie den folgenden hinzu:

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

### <a name="using-the-objcmsgsendstret-method"></a>Mithilfe der Objc_msgSend_stret-Methode

Verwenden Sie beim Erstellen für ARM die [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
für jeden Werttyp, der nicht auf eine Enumeration oder einer der Basistypen für eine Enumeration ist (`int`, `byte`, `short`, `long`, `double`, `float`).

Verwenden Sie beim Erstellen für x86 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
für jeden Werttyp, der nicht auf eine Enumeration oder einer der Basistypen für eine Enumeration ist (`int`, `byte`, `short`, `long`, `double`, `float`), deren systemeigener Größe ist größer als 8 Bytes.

### <a name="creating-your-own-signatures"></a>Erstellen Ihre eigenen Signaturen

Die folgenden [gist-Archiv](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) können verwendet werden, um Ihre eigenen Signaturen, erstellen Sie bei Bedarf.

## <a name="related-links"></a>Verwandte Links

- [Objective-C-Selektoren](https://developer.xamarin.com/samples/mac-ios/Objective-C/) Beispiel
