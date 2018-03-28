---
title: Selektoren Objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7b7f64288695ecc0f9f57ec670c4e9ff2e44804c
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="objective-c-selectors"></a>Selektoren Objective-C

Die Objective-C-Sprache basiert auf *Selektoren*. Ein Selektor ist eine Nachricht, die auf ein Objekt gesendet werden können oder eine *Klasse*. [Xamarin.iOS](~/ios/internals/api-design/index.md) Maps Selektoren auf Instanzmethoden Instanz und Klasse Selektoren statischen Methoden.

Im Gegensatz zu normalen C-Funktionen (und wie die C++-Memberfunktionen) Sie nicht direkt aufrufen, eine Auswahl mit [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Reserviert*: theoretisch können Sie P/Invoke für nicht virtuelle C++-Memberfunktionen, jedoch müssen Sie kümmern pro Compiler namenszerlegung, also in einer Welt mit Probleme besser ignoriert.) Stattdessen Selektoren werden gesendet, um eine Objective-C-Klasse oder Instanz mithilfe der [ `objc_msgSend` Funktion](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Möglicherweise ist [dabei hilfreich für Objective-C-messaging](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) nützlich.

<a name="Example" />

## <a name="example"></a>Beispiel

Angenommen, Sie möchten zum Aufrufen der [-[NSString SizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) Selektor.
Die Deklaration (von der Apple-Dokumentation) ist:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Der Rückgabetyp ist *CGSize* für die einheitliche API.
-  Die *Schriftart* Parameter ist ein [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (und ein Typ (indirekt) abgeleitet [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ), und ist daher zugeordnet [System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/) .
-  Die *Breite* Parameter eine *CGFloat* , zugeordnet ist *Nfloat*.
-  Die *LineBreakMode* Parameter, eine *UILineBreakMode* , bereits in Xamarin.iOS als gebunden wurde die [UILineBreakMode Enumeration](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Fügen Sie sie alle zusammen, und wir möchten eine Objc_msgSend-Deklaration, die entspricht:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Wir müssen es deklariert:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Sobald deklariert werden, können wir es aufgerufen, sobald wir die entsprechenden Parametern haben:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

War der zurückgegebene Wert eine Struktur, die weniger als 8 Bytes Groß wurde (wie das ältere `SizeF` vor dem Wechsel zu den APIs Unified) der obige Code würde im Simulator jedoch auf dem Gerät abgestürzten ausgeführt haben. Damit einen Selektor aufrufen, einen Wert kleiner als 8 Bits, Größe, daher wir zurückgibt *auch* deklarieren müssen die `objc_msgSend_stret()` Funktion:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Aufruf würde dann werden:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>Einen Selektor aufrufen

Aufrufen einer Auswahl besteht aus drei Schritten:

1.  Rufen Sie das Ziel der Selektor.
1.  Ruft den Namen der Selektor.
1.  Rufen Sie objc_msgSend() mit den entsprechenden Argumenten auf.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Auswahl-Ziele

Ein Selektor Ziel ist eine Objektinstanz oder eine Objective-C-Klasse. Wenn das Ziel eine Instanz ist und ein Xamarin.iOS gebundenen stammt, verwenden die [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) Eigenschaft.

Wenn das Ziel einer Klasse ist, verwenden Sie [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) verwenden, um einen Verweis auf die Klasseninstanz zu erhalten, klicken Sie dann die [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) Eigenschaft.


<a name="Selector_Names" />

### <a name="selector-names"></a>Auswahlnamen

Auswahlnamen sind in der Apple-Dokumentation aufgeführt. Z. B. die [UIKit NSString Erweiterungsmethoden](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) enthalten [SizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) und [SizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Die eingebetteten und nachfolgende Doppelpunkte sind wichtig, und sind Teil des Namens der Selektor.

Nachdem Sie einen Auswahlnamen haben, können Sie erstellen eine [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) für diese Instanz.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Calling objc_msgSend()

 `objc_msgSend()` Dient zum Senden einer Nachricht (Selektor) an ein Objekt. Diese Funktionsreihe dauert mindestens zwei erforderlichen Argumente: dem Selektor Ziel (eine Instanz oder die Klasse behandeln), dem Selektor selbst und dann alle Argumente, die für die bestimmte Auswahl erforderlich. Müssen die Instanz und der Auswahlzeiger Argumente `System.IntPtr`, und alle übrigen Argumente müssen mit dem Typ überein der Selektor, z. B. erwartet ein `nint` für eine `int`, oder ein `System.IntPtr` für alle `NSObject`-abgeleitete Typen. Verwenden der [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) -Eigenschaft zum Abrufen einer `IntPtr` für eine Instanz der Objective-C-Typ.

Leider ist mehr als eine `objc_msgSend()` Funktion.

Verwendung [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) für Selektoren, die eine Struktur zurückgegeben.
Behalten Sie Dinge "interessante", klicken Sie auf ARM dies enthält alle Rückgabetypen, die *nicht* eine Enumeration oder den C "Vordefiniert"-Typen (Char, short, Int, long, float, double). Auf X86 (Simulator) muss dies für alle Strukturen, die größer als 8 Bytes Groß verwendet werden. (CGSize--verwendet, die im Beispiel oben – 8 Bytes ist, und daher nicht verwendet `objc_msgSend_stret()` im Simulator.)

Verwendung [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) für Selektoren, die einen Gleitkommawert zurückgeben Wert nur X86 zeigen. Diese Funktion muss es sich nicht auf ARM verwendet werden. Verwenden Sie stattdessen `objc_msgSend()`.

Die Main [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) Funktion wird für alle anderen Selektoren verwendet.

Nachdem Sie die entschieden haben `objc_msgSend()` ausgeschlossener aufrufen, müssen Sie (und möglicherweise mehrere, z. B. für den Simulator und das Gerät), können Sie ein normaler [ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/) Methode, um die Funktion für den späteren Aufruf deklarieren.

Eine Reihe von vorbereiteten `objc_msgSend()` Deklarationen finden Sie im [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Die schlechten

Objective-C verfügt über drei Arten von `objc_msgSend` Methoden: eine für normale Aufrufe, Aufrufe, die Gleitkommawerte (nur X86) zurückgeben und Aufrufe, die Strukturwerte zurückgeben. Die letztgenannten umfassen das Suffix `_stret` in `ObjCRuntime.Messaging`.

Wenn Sie eine Methode aufrufen, die bestimmte Strukturen zurückgibt (Regeln unten beschrieben werden), müssen Sie die Methode, bei dem Rückgabewert als erster Parameter als Wert nicht mehr wie folgt aufrufen:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Alles problematischen hier und da die Regel für die Verwendung müssen _Stret_ unterscheidet sich auf X86- und ARM, wenn Sie Ihre Bindungen an den Simulator und das Gerät arbeiten sollen müssen Sie Code hinzufügen, die wie folgt aussieht:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Verwenden die Objc\_MsgSend\_Stret-Methode

Die Regel für die Verwendung der [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) sind wie folgt für **ARM**:

-  Ein Typ, der nicht auf eine Enumeration oder alle der Basistypen für eine Enumeration ist Wert (Int, Byte, short, long, double, float).


Die Regel für die Verwendung der [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) sind wie folgt für **X86**:

-  Jeder Werttyp für eine Enumeration oder alle der Basistypen für eine Enumeration ist nicht (Int, Byte, short, long, double, float) und dessen systemeigener Größe ist größer als 8 Bytes.


### <a name="creating-your-own-signatures"></a>Erstellen eine eigene Signaturen.

Die folgenden [gist-Archiv](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) können zum Erstellen Ihre eigenen Signaturen verwendet werden, falls erforderlich.



## <a name="related-links"></a>Verwandte Links

- [Selektoren-Beispiel](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
