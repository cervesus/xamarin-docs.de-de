---
Title: "Generika in Xamarin.Forms XAML" Description: " Xamarin.Forms XAML bietet Unterstützung für die Verwendung generischer CLR-Typen, indem die generischen Einschränkungen als Typargumente angegeben werden."
ms. Prod: xamarin ms. assetid: 97b73048-4f 90-41ad-AB48-8eb804c4998b ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 04/28/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="generics-in-xamarinforms-xaml"></a>Generika in Xamarin.Forms XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin.FormsXAML bietet Unterstützung für die Verwendung generischer CLR-Typen durch Angeben der generischen Einschränkungen als Typargumente. Diese Unterstützung wird von der- `x:TypeArguments` Direktive bereitgestellt, die die einschränkenden Typargumente eines generischen Typs an den Konstruktor des generischen Typs übergibt.

> [!IMPORTANT]
> Das Definieren von generischen Klassen in Xamarin.Forms XAML mit der- `x:TypeArguments` Direktive wird nicht unterstützt.

Typargumente werden als Zeichenfolge angegeben und sind normalerweise als Präfix festgelegt, z `sys:String` `sys:Int32` . b. und. Eine vorab Korrektur ist erforderlich, da die typischen Typen von generischen CLR-Einschränkungen aus Bibliotheken stammen, die nicht dem Standard Xamarin.Forms Namespace zugeordnet sind. Allerdings können die in XAML 2009 integrierten Typen, wie z. b. `x:String` und `x:Int32` , auch als Typargumente angegeben werden, wobei `x` der XAML-sprach Namespace für XAML 2009 ist. Weitere Informationen zu den integrierten XAML 2009-Typen finden Sie unter [XAML 2009 Language Primitives](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives).

Mehrere Typargumente können mithilfe eines Komma Trennzeichens angegeben werden. Wenn eine generische Einschränkung generische Typen verwendet, sollten außerdem die geschachtelten Einschränkungs Typargumente in Klammern eingeschlossen werden.

> [!NOTE]
> Die `x:Type` Markup Erweiterung stellt einen CLR-Typverweis für einen generischen Typ bereit und verfügt über eine ähnliche Funktion wie der `typeof` Operator in c#. Weitere Informationen finden Sie unter [x:Type Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension).

## <a name="single-primitive-type-argument"></a>Einzelnes Primitives Typargument

Ein einzelnes Primitives Typargument kann mithilfe der-Direktive als Präfix-Zeichen folgen Argument angegeben werden `x:TypeArguments` :

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

In diesem Beispiel `System.Collections.Generic` ist als `scg` XAML-Namespace definiert. Die `CollectionView.ItemsSource` -Eigenschaft wird auf einen festgelegt, der `List<T>` mit einem Typargument mit `string` dem integrierten XAML 2009-Typ instanziiert wird `x:String` . Die `List<string>` Sammlung wird mit mehreren Elementen initialisiert `string` .

Alternativ kann die Auflistung gleichwertig `List<T>` mit dem CLR-Typ instanziiert werden `String` :

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

Ein einzelnes Objekttypargument kann mithilfe der-Direktive als Präfix-Zeichen folgen Argument angegeben werden `x:TypeArguments` :

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

In diesem Beispiel `GenericsDemo.Models` ist als `models` XAML-Namespace definiert, und `System.Collections.Generic` wird als `scg` XAML-Namespace definiert. Die `CollectionView.ItemsSource` -Eigenschaft wird auf einen festgelegt `List<T>` , der mit einem Typargument instanziiert wird `Monkey` . Die `List<Monkey>` -Auflistung wird mit mehreren `Monkey` -Elementen initialisiert, und ein- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt, das die Darstellung der einzelnen-Objekte definiert, `Monkey` wird als des festgelegt `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView) .

## <a name="multiple-type-arguments"></a>Mehrere Typargumente

Mehrere Typargumente können als Präfix-Zeichen folgen Argumente, getrennt durch ein Komma, mithilfe der- `x:TypeArguments` Direktive angegeben werden. Wenn eine generische Einschränkung generische Typen verwendet, werden die Argumente der geschachtelten Einschränkungs Typen in Klammern eingeschlossen:

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

In diesem Beispiel `GenericsDemo.Models` ist als `models` XAML-Namespace definiert, und `System.Collections.Generic` wird als `scg` XAML-Namespace definiert. Die `CollectionView.ItemsSource` -Eigenschaft wird auf einen festgelegt, der `List<T>` mit einer `KeyValuePair<TKey, TValue>` -Einschränkung mit den inneren Einschränkungs Typargumenten und instanziiert wird `string` `Monkey` . Die Auflistung `List<KeyValuePair<string,Monkey>>` wird mit mehreren Elementen initialisiert `KeyValuePair` , wobei der nicht Standardkonstruktor verwendet wird `KeyValuePair` , und ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der die Darstellung der einzelnen-Objekte definiert, `Monkey` wird als der des festgelegt `ItemTemplate` [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Informationen zum Übergeben von Argumenten an einen nicht Standardkonstruktor finden Sie unter [übergeben von Konstruktorargumenten](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments).

## <a name="related-links"></a>Verwandte Links

- [Generika in XAML (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [XAML 2009-Sprachprimitive](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [x:Type-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#xtype-markup-extension)
- [Übergeben von Konstruktorargumenten](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments)
