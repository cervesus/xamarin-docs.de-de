---
title: Übergeben von Argumenten in XAML
description: In diesem Artikel wird die Verwendung der XAML-Attribute veranschaulicht, die verwendet werden können, um Argumente an nicht standardmäßige Konstruktoren zu übergeben, Factorymethoden aufzurufen und den Typ eines generischen Arguments anzugeben.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 84d8901b7f8dee8ffd6c3ba22d30c76b456555f0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84571505"
---
# <a name="passing-arguments-in-xaml"></a>Übergeben von Argumenten in XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_In diesem Artikel wird die Verwendung der XAML-Attribute veranschaulicht, die verwendet werden können, um Argumente an nicht standardmäßige Konstruktoren zu übergeben, Factorymethoden aufzurufen und den Typ eines generischen Arguments anzugeben._

## <a name="overview"></a>Übersicht

Es ist häufig erforderlich, Objekte mit Konstruktoren zu instanziieren, die Argumente erfordern, oder indem Sie eine statische Erstellungs Methode aufrufen. Dies kann in XAML mit dem `x:Arguments` -Attribut und dem-Attribut erreicht werden `x:FactoryMethod` :

- Das- `x:Arguments` Attribut wird verwendet, um Konstruktorargumente für einen nicht Standardkonstruktor oder für eine Factorymethoden-Objekt Deklaration anzugeben. Weitere Informationen finden Sie unter [übergeben von Konstruktorargumenten](#passing-constructor-arguments).
- Das- `x:FactoryMethod` Attribut wird verwendet, um eine Factorymethode anzugeben, die zum Initialisieren eines Objekts verwendet werden kann. Weitere Informationen finden Sie unter [Call Factory Methods](#calling-factory-methods).

Außerdem kann das- `x:TypeArguments` Attribut verwendet werden, um die generischen Typargumente für den Konstruktor eines generischen Typs anzugeben. Weitere Informationen finden Sie unter [Angeben eines generischen Typarguments](#specifying-a-generic-type-argument).

## <a name="passing-constructor-arguments"></a>Übergeben von Konstruktorargumenten

Argumente können mithilfe des-Attributs an einen nicht Standardkonstruktor übergeben werden `x:Arguments` . Jedes Konstruktorargument muss innerhalb eines XML-Elements getrennt werden, das den Typ des Arguments darstellt. Xamarin.Formsunterstützt die folgenden Elemente für grundlegende Typen:

- `x:Array`
- `x:Boolean`
- `x:Byte`
- `x:Char`
- `x:DateTime`
- `x:Decimal`
- `x:Double`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Object`
- `x:Single`
- `x:String`
- `x:TimeSpan`

Im folgenden Codebeispiel wird die Verwendung des- `x:Arguments` Attributs mit drei [`Color`](xref:Xamarin.Forms.Color) Konstruktoren veranschaulicht:

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

Die Anzahl der Elemente im `x:Arguments` -Tag und die Typen dieser Elemente müssen mit einem der- [`Color`](xref:Xamarin.Forms.Color) Konstruktoren identisch sein. Der `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double)) mit einem einzelnen Parameter erfordert einen Graustufenwert von 0 (schwarz) bis 1 (weiß). Der `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) mit drei Parametern erfordert einen roten, grünen und blauen Wert im Bereich von 0 bis 1. Der `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) mit vier Parametern fügt einen Alphakanal als vierten Parameter hinzu.

Die folgenden Screenshots zeigen das Ergebnis des Aufrufs jedes [`Color`](xref:Xamarin.Forms.Color) Konstruktors mit den angegebenen Argument Werten:

![Boxview. Color mit x:Arguments angegeben](passing-arguments-images/passing-arguments.png)

## <a name="calling-factory-methods"></a>Aufrufen von Factory-Methoden

Factorymethoden können in XAML aufgerufen werden, indem der Name der Methode mit dem- `x:FactoryMethod` Attribut und den Argumenten mithilfe des- `x:Arguments` Attributs angegeben wird. Eine Factorymethode ist eine `public static` Methode, die Objekte oder Werte desselben Typs zurückgibt wie die Klasse oder Struktur, die die Methoden definiert.

Die [`Color`](xref:Xamarin.Forms.Color) -Struktur definiert eine Reihe von Factorymethoden, und im folgenden Codebeispiel wird das Aufrufen von drei von Ihnen veranschaulicht:

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

Die Anzahl der Elemente im `x:Arguments` -Tag und die Typen dieser Elemente müssen mit den Argumenten der aufgerufenen Factorymethode identisch sein. Die [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) Factorymethode erfordert vier [`Int32`](https://docs.microsoft.com/dotnet/api/system.int32) Parameter, die die Werte für Rot, grün, blau und Alpha darstellen, die zwischen 0 und 255 liegen. Die [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Factorymethode benötigt vier [`Double`](https://docs.microsoft.com/dotnet/api/system.double) Parameter, die die Werte für Farbton, Sättigung, Helligkeit und Alpha darstellen, die zwischen 0 und 1 liegen. Die [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) Factorymethode erfordert eine [`String`](https://docs.microsoft.com/dotnet/api/system.string) , die die hexadezimale RGB-Farbe (a) darstellt.

Die folgenden Screenshots zeigen das Ergebnis des Aufrufs jeder [`Color`](xref:Xamarin.Forms.Color) Factorymethode mit den angegebenen Argument Werten:

![Boxview. Color mit "x:factorymethod" und "x:Arguments" angegeben](passing-arguments-images/factory-methods.png)

## <a name="specifying-a-generic-type-argument"></a>Angeben eines generischen Typarguments

Generische Typargumente für den Konstruktor eines generischen Typs können mithilfe des- `x:TypeArguments` Attributs angegeben werden, wie im folgenden Codebeispiel gezeigt:

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

Die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -Klasse ist eine generische Klasse und muss mit einem Attribut instanziiert werden `x:TypeArguments` , das mit dem Zieltyp übereinstimmt. In der- [`On`](xref:Xamarin.Forms.On) Klasse kann das- [`Platform`](xref:Xamarin.Forms.On.Platform) Attribut einen einzelnen `string` Wert oder mehrere durch Trennzeichen getrennte `string` Werte annehmen. In diesem Beispiel wird die [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin) -Eigenschaft auf eine plattformspezifische festgelegt [`Thickness`](xref:Xamarin.Forms.Thickness) .

Weitere Informationen zu generischen Typargumenten finden Sie unter [Generika in Xamarin.Forms XAML](generics.md).

## <a name="related-links"></a>Verwandte Links

- [Übergeben von Konstruktorargumenten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [Aufrufen von Factorymethoden (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Generika in Xamarin.Forms XAML](generics.md)
