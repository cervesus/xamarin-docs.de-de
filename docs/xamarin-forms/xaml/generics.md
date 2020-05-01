---
title: Generika in xamarin. Forms-XAML
description: Xamarin. Forms XAML bietet Unterstützung für die Verwendung generischer CLR-Typen, indem die generischen Einschränkungen als Typargumente angegeben werden.
ms.prod: xamarin
ms.assetid: 97B73048-4F90-41AD-AB48-8EB804C4998B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2020
ms.openlocfilehash: 9cda08a3bab0e25db2315c9795721e25d47d2429
ms.sourcegitcommit: 154a3e7aec775327565bb54eda1a610976af1d6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82624708"
---
# <a name="generics-in-xamarinforms-xaml"></a>Generika in xamarin. Forms-XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin. Forms XAML bietet Unterstützung für die Verwendung generischer CLR-Typen, indem die generischen Einschränkungen als Typargumente angegeben werden. Diese Unterstützung wird von der `x:TypeArguments` -Direktive bereitgestellt, die die einschränkenden Typargumente eines generischen Typs an den Konstruktor des generischen Typs übergibt.

> [!IMPORTANT]
> Das Definieren von generischen Klassen in xamarin. Forms XAML `x:TypeArguments` mit der-Direktive wird nicht unterstützt.

Typargumente werden als Zeichenfolge angegeben und sind normalerweise als Präfix `sys:String` fest `sys:Int32`gelegt, z. b. und. Eine vorab Korrektur ist erforderlich, da die typischen Typen von generischen CLR-Einschränkungen aus Bibliotheken stammen, die nicht dem xamarin. Forms-Standard Namespace zugeordnet sind. Allerdings können die in XAML 2009 integrierten Typen, wie `x:String` z `x:Int32`. b. und, auch als Typargumente angegeben werden, wobei `x` der XAML-sprach Namespace für XAML 2009 ist. Weitere Informationen zu den integrierten XAML 2009-Typen finden Sie unter [XAML 2009 Language Primitives](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives).

Mehrere Typargumente können mithilfe eines Komma Trennzeichens angegeben werden. Wenn eine generische Einschränkung generische Typen verwendet, sollten außerdem die geschachtelten Einschränkungs Typargumente in Klammern eingeschlossen werden.

> [!NOTE]
> Die `x:Type` Markup Erweiterung stellt einen CLR-Typverweis für einen generischen Typ bereit und verfügt über eine ähnliche `typeof` Funktion wie der Operator in c#. Weitere Informationen finden Sie unter [x:Type Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#type).

## <a name="single-primitive-type-argument"></a>Einzelnes Primitives Typargument

Ein einzelnes Primitives Typargument kann mithilfe der `x:TypeArguments` -Direktive als Präfix-Zeichen folgen Argument angegeben werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="x:String">
                <x:String>Baboon</x:String>
                <x:String>Capuchin Monkey</x:String>
                <x:String>Blue Monkey</x:String>
                <x:String>Squirrel Monkey</x:String>
                <x:String>Golden Lion Tamarin</x:String>
                <x:String>Howler Monkey</x:String>
                <x:String>Japanese Macaque</x:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

In diesem Beispiel `System.Collections.Generic` ist als `scg` XAML-Namespace definiert. Die `CollectionView.ItemsSource` -Eigenschaft wird auf einen `List<T>` festgelegt, der mit einem `string` Typargument mit dem integrierten XAML 2009- `x:String` Typ instanziiert wird. Die `List<string>` Sammlung wird mit mehreren `string` Elementen initialisiert.

Alternativ kann die `List<T>` Auflistung gleichwertig mit dem CLR `String` -Typ instanziiert werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="sys:String">
                <sys:String>Baboon</sys:String>
                <sys:String>Capuchin Monkey</sys:String>
                <sys:String>Blue Monkey</sys:String>
                <sys:String>Squirrel Monkey</sys:String>
                <sys:String>Golden Lion Tamarin</sys:String>
                <sys:String>Howler Monkey</sys:String>
                <sys:String>Japanese Macaque</sys:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

## <a name="single-object-type-argument"></a>Einzelnes Objekttyp Argument

Ein einzelnes Objekttypargument kann mithilfe der `x:TypeArguments` -Direktive als Präfix-Zeichen folgen Argument angegeben werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="models:Monkey">
                <models:Monkey Name="Baboon"
                               Location="Africa and Asia"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                <models:Monkey Name="Capuchin Monkey"
                               Location="Central and South America"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />
                <models:Monkey Name="Blue Monkey"
                               Location="Central and East Africa"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage>
```

`GenericsDemo.Models` In diesem Beispiel ist `models` als XAML-Namespace definiert, und wird `System.Collections.Generic` als `scg` XAML-Namespace definiert. Die `CollectionView.ItemsSource` -Eigenschaft wird auf einen `List<T>` festgelegt, der mit einem `Monkey` Typargument instanziiert wird. Die `List<Monkey>` -Auflistung wird mit mehreren `Monkey` -Elementen initialisiert, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) und ein-Objekt, das `Monkey` die Darstellung der einzelnen- `ItemTemplate` Objekte definiert [`CollectionView`](xref:Xamarin.Forms.CollectionView), wird als des festgelegt.

## <a name="multiple-type-arguments"></a>Mehrere Typargumente

Mehrere Typargumente können als Präfix-Zeichen folgen Argumente, getrennt durch ein Komma, mithilfe `x:TypeArguments` der-Direktive angegeben werden. Wenn eine generische Einschränkung generische Typen verwendet, werden die Argumente der geschachtelten Einschränkungs Typen in Klammern eingeschlossen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="scg:KeyValuePair(x:String,models:Monkey)">
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Baboon</x:String>
                        <models:Monkey Location="Africa and Asia"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Capuchin Monkey</x:String>
                        <models:Monkey Location="Central and South America"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />   
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Blue Monkey</x:String>
                        <models:Monkey Location="Central and East Africa"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding Value.ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Key}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Value.Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage    
```

`GenericsDemo.Models` In diesem Beispiel ist `models` als XAML-Namespace definiert, und wird `System.Collections.Generic` als `scg` XAML-Namespace definiert. Die `CollectionView.ItemsSource` -Eigenschaft wird auf einen `List<T>` festgelegt, der mit einer `KeyValuePair<TKey, TValue>` -Einschränkung mit den inneren Einschränkungs Typargumenten `string` und `Monkey`instanziiert wird. Die `List<KeyValuePair<string,Monkey>>` Auflistung wird mit mehreren `KeyValuePair` Elementen initialisiert, wobei der nicht `KeyValuePair` Standardkonstruktor verwendet wird, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) und ein, der die Darstellung `Monkey` der einzelnen-Objekte definiert `ItemTemplate` , wird [`CollectionView`](xref:Xamarin.Forms.CollectionView)als der des festgelegt. Informationen zum Übergeben von Argumenten an einen nicht Standardkonstruktor finden Sie unter [übergeben von Konstruktorargumenten](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments).

## <a name="related-links"></a>Verwandte Links

- [Generika in XAML (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [XAML 2009-Sprachprimitive](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [x:typmarkup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)
- [Übergeben von Konstruktorargumenten](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)
