---
title: Übergeben von Argumenten in XAML
description: In diesem Artikel wird veranschaulicht, wie die XAML-Attribute, die verwendet werden können, um Argumente an nicht standardmäßige Konstruktoren, die zum Aufrufen der Factorymethoden, und geben Sie ein generisches Argument übergeben.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 1baad2e2edfb661fff9f3ef0ccf52c9922e9f351
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178155"
---
# <a name="passing-arguments-in-xaml"></a>Übergeben von Argumenten in XAML

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)

_In diesem Artikel wird veranschaulicht, wie die XAML-Attribute, die verwendet werden können, um Argumente an nicht standardmäßige Konstruktoren, die zum Aufrufen der Factorymethoden, und geben Sie ein generisches Argument übergeben._

## <a name="overview"></a>Übersicht

Es ist häufig erforderlich, Instanziieren von Objekten mit Konstruktoren, die Argumente erforderlich sind, oder durch Aufrufen einer statischen Erstellungsmethode. Dies kann in XAML mit erreicht werden die `x:Arguments` und `x:FactoryMethod` Attribute:

- Die `x:Arguments` Attribut wird verwendet, um die Konstruktorargumente für einen nicht standardmäßigen Konstruktor oder für eine Factory-Methodendeklaration Objekt angeben. Weitere Informationen finden Sie unter [Konstruktorargument übergeben](#constructor_arguments).
- Die `x:FactoryMethod` Attribut wird verwendet, um eine Factorymethode angeben, die zum Initialisieren eines Objekts verwendet werden kann. Weitere Informationen finden Sie unter [Aufrufen der Factorymethoden](#factory_methods).

Darüber hinaus die `x:TypeArguments` -Attribut kann verwendet werden, um die generischen Typargumente an den Konstruktor eines generischen Typs anzugeben. Weitere Informationen finden Sie unter [er ein generisches Typargument angibt](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Übergeben von Argumenten-Konstruktor

Argumente übergeben werden können, an einem nicht standardmäßigen Konstruktor mit der `x:Arguments` Attribut. Jedes Konstruktorargument muss in einem XML-Element getrennt werden, die den Typ des Arguments darstellt. Xamarin.Forms unterstützt die folgenden Elemente für grundlegende Typen:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

Das folgende Codebeispiel veranschaulicht die Verwendung der `x:Arguments` Attribut mit drei [ `Color` ](xref:Xamarin.Forms.Color) Konstruktoren:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Die Anzahl der Elemente in der `x:Arguments` Tag und die Typen dieser Elemente, müssen einem der entsprechen der [ `Color` ](xref:Xamarin.Forms.Color) Konstruktoren. Die `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double)) mit einem einzigen Parameter erfordert einen graustufentextur-Wert von 0 (Schwarz) auf 1 (weiß). Die `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) mit drei Parametern erfordert einen Zahl zwischen 0 und 1 roten, grünen und blauen-Wert. Die `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) mit vier Parametern einen alpha-Kanal als vierten Parameter hinzugefügt.

Die folgenden Screenshots zeigen das Ergebnis des Aufrufs jedes [ `Color` ](xref:Xamarin.Forms.Color) Konstruktor mit dem angegebenen Argument-Werte:

![](passing-arguments-images/passing-arguments.png "BoxView.Color mit X: Arguments angegeben wird")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Aufrufen der Factorymethoden

Factorymethoden können in XAML aufgerufen werden, durch Angeben der Methode mithilfe der `x:FactoryMethod` -Attribut und dessen Argumente mithilfe der `x:Arguments` Attribut. Eine Factorymethode ist eine `public static` -Methode, die zurückgegeben werden, Objekte oder Werte desselben Typs wie die Klasse oder Struktur, die die Methoden definiert.

Die [ `Color` ](xref:Xamarin.Forms.Color) Struktur definiert eine Reihe von Factorymethoden und im folgenden Codebeispiel wird veranschaulicht, aufrufende drei Parameter:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Die Anzahl der Elemente in der `x:Arguments` übereinstimmen Tag und die Typen dieser Elemente, die Argumente von der Factory-Methode aufgerufen wird. Die [ `FromRgba` ](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) Factorymethode erfordert vier [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) -Parameter, die die roten, grünen, blauen und alpha-Werte, zwischen 0 und 255 bzw. darstellen. Die [ `FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Factorymethode erfordert vier [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) -Parameter, die den Farbton, Sättigung, Helligkeit und alpha-Werte zwischen 0 und 1 bzw. darstellen. Die [ `FromHex` ](xref:Xamarin.Forms.Color.FromHex(System.String)) Factorymethode erfordert eine [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) , die den Hexadezimalwert darstellt (A) RGB-Farbe.

Die folgenden Screenshots zeigen das Ergebnis des Aufrufs jedes [ `Color` ](xref:Xamarin.Forms.Color) Factorymethode, die mit den Werten des angegebenen Arguments:

![](passing-arguments-images/factory-methods.png "BoxView.Color mit X: FactoryMethod- und X: Arguments angegeben wird")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Ein generisches Typargument angeben

Generische Typargumente für den Konstruktor eines generischen Typs können angegeben werden, mithilfe der `x:TypeArguments` Attribut, wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

Die [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse und instanziiert werden müssen, mit einer `x:TypeArguments` -Attribut, das mit dem Zieltyp übereinstimmt. In der [ `On` ](xref:Xamarin.Forms.On) -Klasse, die [ `Platform` ](xref:Xamarin.Forms.On.Platform) Attribut kann eine einzelne akzeptieren `string` Wert oder mehrere durch Kommas getrennte `string` Werte. In diesem Beispiel die [ `StackLayout.Margin` ](xref:Xamarin.Forms.View.Margin) -Eigenschaftensatz auf eine plattformspezifische [ `Thickness` ](xref:Xamarin.Forms.Thickness).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, mithilfe der XAML-Attribute, die verwendet werden können, um Argumente an nicht standardmäßige Konstruktoren, die zum Aufrufen der Factorymethoden, und geben Sie ein generisches Argument übergeben.


## <a name="related-links"></a>Verwandte Links

- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Übergeben von Argumenten-Konstruktor (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Aufrufen der Factorymethoden (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
