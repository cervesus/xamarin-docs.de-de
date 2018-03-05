---
title: "Übergeben von Argumenten in XAML"
description: "Dieser Artikel beschreibt die Verwendung von XAML-Attributen, die verwendet werden können, Argumente an nicht standardmäßige Konstruktoren, Factorymethoden aufrufen, und geben Sie ein generisches Argument übergeben."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: a30dd9b33466ac6907322f8c6b586c012452a44f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="passing-arguments-in-xaml"></a>Übergeben von Argumenten in XAML

_Dieser Artikel beschreibt die Verwendung von XAML-Attributen, die verwendet werden können, Argumente an nicht standardmäßige Konstruktoren, Factorymethoden aufrufen, und geben Sie ein generisches Argument übergeben._

## <a name="overview"></a>Übersicht

Es ist häufig zum Instanziieren von Objekten mit Konstruktoren, die Argumente erforderlich sind, oder durch Aufrufen einer statischen Erstellungsmethode. Dies kann in XAML mit erreicht werden die `x:Arguments` und `x:FactoryMethod` Attribute:

- Die `x:Arguments` Attribut wird verwendet, um Konstruktorargumente für einen nicht trivialen Konstruktor oder eine Objektdeklaration der Factory-Methode angeben. Weitere Informationen finden Sie unter [Konstruktorargumente übergeben](#constructor_arguments).
- Die `x:FactoryMethod` Attribut wird verwendet, um eine Factorymethode angeben, die verwendet werden kann, um ein Objekt zu initialisieren. Weitere Informationen finden Sie unter [Aufrufen der Factorymethoden](#factory_methods).

Darüber hinaus die `x:TypeArguments` Attribut kann verwendet werden, um die generischen Typargumente an den Konstruktor eines generischen Typs angeben. Weitere Informationen finden Sie unter [generischen Typargument angeben](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Übergeben von Argumenten-Konstruktor

Argumente übergeben werden können, in einen nicht standardmäßigen Konstruktor mit der `x:Arguments` Attribut. Jedes Konstruktorargument muss in einem XML-Element getrennt werden, die den Typ des Arguments darstellt. Xamarin.Forms unterstützt die folgenden Elemente für grundlegende Typen:

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

Das folgende Codebeispiel veranschaulicht die Verwendung der `x:Arguments` Attribut mit drei [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Konstruktoren:

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

Die Anzahl der Elemente innerhalb der `x:Arguments` Tag und die Arten von diesen Elementen müssen einem der entsprechen den [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Konstruktoren. Die `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) mit einem einzelnen Parameter erfordert einen Graustufenwert von 0 (Schwarz) und 1 (weiß). Die `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) mit drei Parametern erfordert einen Rot-, Grün- und blauen-Wert im Bereich von 0 bis 1. Die `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) mit vier Parametern einen alpha-Kanal als vierten Parameter hinzugefügt.

Die folgenden Screenshots zeigen das Ergebnis des Aufrufs jedes [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Konstruktor mit den Werten des angegebenen Arguments:

![](passing-arguments-images/passing-arguments.png "X: Arguments angegeben BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Aufrufen der Factorymethoden

Factorymethoden können in XAML aufgerufen werden, indem Sie der Methode angeben mithilfe der `x:FactoryMethod` Attribut und seine Argumente, die mit der `x:Arguments` Attribut. Eine Factorymethode ist eine `public static` Methode, die Objekte oder Werte desselben Typs wie die Klasse oder Struktur, die die Methoden definiert zurückgibt.

Die [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Struktur definiert eine Reihe von Factorymethoden, und im folgenden Codebeispiel wird veranschaulicht, aufrufenden drei Elemente:

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

Die Anzahl der Elemente innerhalb der `x:Arguments` Tag und die Typen der diese Elemente müssen die Argumente der aufgerufenen Factorymethode entsprechen. Die [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) Factorymethode erfordert vier [ `Int32` ](https://developer.xamarin.com/api/type/System.Int32/) Parameter, die die Werte Rot, Grün, Blau und alpha, die zwischen 0 und 255 bzw. darstellen. Die [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) Factorymethode erfordert vier [ `Double` ](https://developer.xamarin.com/api/type/System.Double/) Parameter, die den Farbton, Sättigung Helligkeit und Alphawerte, zwischen 0 und 1 bzw. darstellen. Die [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) Factorymethode erfordert eine [ `String` ](https://developer.xamarin.com/api/type/System.String/) , die die Hexadezimalwerte darstellt (A) RGB-Farbe.

Die folgenden Screenshots zeigen das Ergebnis des Aufrufs jedes [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Factory-Methode mit den Werten des angegebenen Arguments:

![](passing-arguments-images/factory-methods.png "X: FactoryMethod-und X: Arguments angegeben BoxView.Color")

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
        <On Platform="WinPhone, Windows" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

Die [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Klasse ist eine generische Klasse und müssen instanziiert werden, mit einer `x:TypeArguments` Attribut, das den Zieltyp entspricht. In der [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) -Klasse, die [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) Attribut akzeptiert ein einzelnes `string` Wert oder mehrere durch Kommas getrennte `string` Werte. In diesem Beispiel wird die [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Eigenschaftensatz an einen plattformspezifischen [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht die Verwendung von XAML-Attributen, die verwendet werden können, Argumente an nicht standardmäßige Konstruktoren, Factorymethoden aufrufen, und geben Sie ein generisches Argument übergeben.


## <a name="related-links"></a>Verwandte Links

- [XAML-Namespaces](~/xamarin-forms/xaml/namespaces.md)
- [Übergeben von Konstruktorargumente (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Aufrufen der Factorymethoden (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
