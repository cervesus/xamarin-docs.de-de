---
title: Fallbacks für Xamarin.Forms-Bindung
description: In diesem Artikel wird erläutert, wie Bindungen stabiler machen, durch die Definition von fallback-Werte, die verwendet werden, wenn die Bindung fehlschlägt.
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 2a4b29df9148ce695f8f3ca5377e5848af1b775a
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171598"
---
# <a name="xamarinforms-binding-fallbacks"></a>Fallbacks für Xamarin.Forms-Bindung

Manchmal datenbindungen, die fehlschlagen, weil die Quelle der Bindung nicht aufgelöst werden kann oder die Bindung erfolgreich ist, jedoch gibt eine `null` Wert. Während dieser Szenarien mit Wertkonverter oder anderen zusätzlichen Code verarbeitet werden können, können von datenbindungen werden robuster gestaltet werden alternative Werte verwenden, wenn die Bindung kann nicht ausgeführt werden. Dies kann erreicht werden, indem Sie definieren die [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) und [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) Eigenschaften in einem Bindungsausdruck. Da diese Eigenschaften im befinden die [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) -Klasse, können sie verwendet werden, mit Bindungen, kompilierte Bindungen, und die `Binding` Markuperweiterung.

> [!NOTE]
> Verwenden der [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) und [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) Eigenschaften in einem Bindungsausdruck ist optional.

## <a name="defining-a-fallback-value"></a>Definieren einen fallback-Wert

Die [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) Eigenschaft kann ein fallback definiert werden, die verwendet werden, wenn die Bindung *Quelle* kann nicht aufgelöst werden. Ein häufiges Szenario für das Festlegen dieser Eigenschaft wird beim Binden an Eigenschaften der Quelle, die möglicherweise nicht für alle Objekte in einer bindungsauflistung heterogene Typen vorhanden.

Die **MonkeyDetail** Seite veranschaulicht die Einstellung der [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) Eigenschaft:

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

Die Bindung für die [ `Label` ](xref:Xamarin.Forms.Label) definiert eine [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) -Wert, der auf dem Ziel festgelegt wird, wenn die Bindungsquelle nicht aufgelöst werden kann. Deshalb ist der Wert von definiert die `FallbackValue` Eigenschaft wird angezeigt, wenn die `Population` Eigenschaft ist nicht vorhanden, für das gebundene Objekt. Beachten Sie, dass hier die `FallbackValue` Eigenschaftswert durch einfache Anführungszeichen (Apostroph)-Zeichen begrenzt wird.

Anstatt definieren [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) Eigenschaft Werte Inline, es wird empfohlen, diese als Ressourcen im Definieren einer [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Der Vorteil dieses Ansatzes ist, dass solche Werte in einem zentralen Ort einmal definiert werden, und sind einfacher lokalisierbar. Die Ressourcen können dann abgerufen werden mithilfe der `StaticResource` Markuperweiterung:

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> Es ist nicht möglich, Sie legen die `FallbackValue` Eigenschaft mit einem Bindungsausdruck.

Hier wird das Programm ausgeführt wird:

![FallbackValue Bindung](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue-Bindung")

Wenn die `FallbackValue` Eigenschaftensatz wird nicht in einem Bindungsausdruck und des Bindungspfads oder Teil des Pfads nicht aufgelöst, [ `BindableProperty.DefaultValue` ](xref:Xamarin.Forms.BindableProperty.DefaultValue) festgelegt ist, auf dem Ziel. Jedoch, wenn die `FallbackValue` Teil des Pfads nicht behoben bzw.-Eigenschaft festgelegt ist und den Bindungspfad den Wert des der `FallbackValue` Value-Eigenschaft festgelegt ist, auf dem Ziel. Daher auf die **MonkeyDetail** Seite die [ `Label` ](xref:Xamarin.Forms.Label) "Unbekannter Auffüllung Size" angezeigt, da das gebundene Objekt verfügt nicht über eine `Population` Eigenschaft.

> [!IMPORTANT]
> Ein definierten Wertkonverter wird nicht ausgeführt, in einem Bindungsausdruck bei der [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) festgelegt wird.

## <a name="defining-a-null-replacement-value"></a>Definieren einen null-Ersatzwert

Die [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) Eigenschaft können Sie einen Ersatzwert definiert werden, die verwendet werden, wenn die Bindung *Quelle* aufgelöst wurde, aber der Wert ist `null`. Ein häufiges Szenario für das Festlegen dieser Eigenschaft wird beim Binden an der Datenquelleneigenschaften, die möglicherweise `null` in einer bindungsauflistung.

Die **Affen** Seite veranschaulicht die Einstellung der [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) Eigenschaft:

```xaml
<ListView ItemsSource="{Binding Monkeys}"
          ...>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Image Source="{Binding ImageUrl, TargetNullValue='https://upload.wikimedia.org/wikipedia/commons/2/20/Point_d_interrogation.jpg'}"
                           ... />
                    ...
                    <Label Text="{Binding Location, TargetNullValue='Location unknown'}"
                           ... />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Die Bindungen für die [ `Image` ](xref:Xamarin.Forms.Image) und [ `Label` ](xref:Xamarin.Forms.Label) beide definieren [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) Werte, die angewendet werden, wenn die Bindungspfad gibt`null`. Aus diesem Grund den vom definierten Werte der `TargetNullValue` Eigenschaften werden für alle Objekte in der Sammlung angezeigt, in denen die `ImageUrl` und `Location` Eigenschaften sind nicht definiert. Beachten Sie, dass hier die `TargetNullValue` Eigenschaftswerte durch einfache Anführungszeichen (Apostroph)-Zeichen getrennt sind.

Anstatt definieren [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) Eigenschaft Werte Inline, es wird empfohlen, diese als Ressourcen im Definieren einer [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Der Vorteil dieses Ansatzes ist, dass solche Werte in einem zentralen Ort einmal definiert werden, und sind einfacher lokalisierbar. Die Ressourcen können dann abgerufen werden mithilfe der `StaticResource` Markuperweiterung:

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> Es ist nicht möglich, Sie legen die `TargetNullValue` Eigenschaft mit einem Bindungsausdruck.

Hier wird das Programm ausgeführt wird:

[![TargetNullValue Bindung](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue Bindung")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue-Bindung")

Bei der `TargetNullValue` Eigenschaftensatz wird nicht in einem Bindungsausdruck, der Quelle den Wert `null` wird konvertiert, wenn Sie ein Wertkonverter formatiert, wenn definiert ist, eine `StringFormat` definiert ist, und klicken Sie dann das Ergebnis auf dem Ziel festgelegt ist. Jedoch, wenn die `TargetNullValue` Eigenschaft festgelegt ist, wird einen Quelle den Wert `null` wird konvertiert werden, wenn ein Wertkonverter definiert ist, und wenn dieser noch `null` nach der Konvertierung den Wert des der `TargetNullValue` Eigenschaft wird festgelegt, auf dem Ziel.

> [!IMPORTANT]
> Formatierung von Zeichenfolgen wird nicht in einem Bindungsausdruck angewendet Wenn die `TargetNullValue` festgelegt wird.

## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
