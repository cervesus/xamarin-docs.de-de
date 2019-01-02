---
title: Fallbacks für Xamarin.Forms-Bindungen
description: In diesem Artikel wird erläutert, wie Bindungen stabiler gemacht werden können, indem Fallback-Werte definiert werden, die verwendet werden, wenn der Bindungsprozess fehlschlägt.
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 505b5bfb9681e5bc30ff84aa90c8e148ed6db4b1
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058284"
---
# <a name="xamarinforms-binding-fallbacks"></a>Fallbacks für Xamarin.Forms-Bindungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)

Manchmal schlagen Datenbindungen fehl, weil die Bindungsquelle nicht aufgelöst werden kann oder weil eine erfolgreiche Bindung einen `null`-Wert zurückgibt. Während diese Szenarios mit Wertkonvertern behandelt werden können, oder einem anderen zusätzlichen Code, können Datenbindungen stabiler gemacht werden, indem Fallback-Werte festgelegt werden, die verwendet werden, wenn der Bindungsprozess fehlschlägt. Dies kann erreicht werden, indem die Eigenschaften [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) und [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) in einem Bindungsausdruck definiert werden. Da sich diese Eigenschaften in der [`BindingBase`](xref:Xamarin.Forms.BindingBase)-Klasse befinden, können sie mit Bindungen, kompilierten Bindungen und mit der `Binding`-Markuperweiterung verwendet werden.

> [!NOTE]
> Die Verwendung der Eigenschaften [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) und [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) in einem Bindungsausdruck ist optional.

## <a name="defining-a-fallback-value"></a>Definieren von Fallbackwerten

Durch die Eigenschaft [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) kann ein Fallbackwert definiert werden, der verwendet wird, wenn die Bindung *Quelle* nicht gelöst werden kann. Ein häufiges Szenario zur Einstellung dieser Eigenschaft ist die Bindung an Quelleigenschaften, die möglicherweise nicht für alle Objekte in einer gebundenen Sammlung heterogener Typen vorhanden sind.

Auf der Seite **MonkeyDetail (Affe Details)** wird das Festlegen der [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue)-Eigenschaft veranschaulicht:

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

Die Bindung auf der [`Label`](xref:Xamarin.Forms.Label) definiert einen [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue)-Wert, der auf das Ziel festgelegt wird, wenn die Bindungsquelle nicht gelöst werden kann. Deshalb wird der durch die `FallbackValue`-Eigenschaft definierte Wert angezeigt, falls die `Population`-Eigenschaft nicht auf dem gebundenen Objekt vorhanden ist. Beachten Sie, dass hier der `FallbackValue`-Eigenschaftswert durch einfache Anführungszeichen (Apostrophe) getrennt ist.

Anstatt [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue)-Eigenschaftswerte inline zu definieren, wird empfohlen, diese als Ressourcen in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) zu definieren. Der Vorteil dieses Ansatzes ist, dass solche Werte einmal an einem einzigen Speicherort definiert werden und einfacher lokalisierbar sind. Die Ressourcen können dann mithilfe der `StaticResource`-Markuperweiterung abgerufen werden:

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> Es ist nicht möglich, die `FallbackValue`-Eigenschaft mit einem Bindungsausdruck festzulegen.

Dies ist das Programm, das ausgeführt wird:

![FallbackValue Binding](binding-fallbacks-images/bindingunavailable-detail-cropped.png "Fallbackwert-Bindung")

Wenn die `FallbackValue`-Eigenschaft nicht in einem Bindungsausdruck festgelegt ist, und der Bindungspfad oder Teil des Pfads nicht gelöst wurde, wird [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) auf das Ziel festgelegt. Wenn die `FallbackValue`-Eigenschaft jedoch festgesetzt wird und der Bindungspfad oder Teil des Pfads nicht gelöst wurde, wird der Wert der `FallbackValue`-Werteigenschaft auf das Ziel festgelegt. Deshalb wird auf der Seite **MonkeyDetail (Affe Details)** vom [`Label`](xref:Xamarin.Forms.Label) „Population size unknown“ (Populationsgröße unbekannt) angezeigt, weil dem gebundenen Objekt eine `Population`-Eigenschaft fehlt.

> [!IMPORTANT]
> Ein definierter Wertkonverter wird in einem Bindungsausdruck nicht ausgeführt, wenn die [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue)-Eigenschaft festgelegt ist.

## <a name="defining-a-null-replacement-value"></a>Definieren von NULL-Ersatzwerten

Durch die [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue)-Eigenschaft kann ein Ersatzwert definiert werden, der verwendet wird, wenn die Bindung *Quelle* zwar gelöst wurde, der Wert aber `null` ist. Ein häufiges Szenario zum Festlegen dieser Eigenschaft ist die Bindung an Quelleigenschaften, die in einer gebundenen Sammlung `null` sein könnten.

Die Seite **Monkeys (Affen)** veranschaulicht das Festlegen der [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue)-Eigenschaft:

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

Die Bindungen auf den [`Image`](xref:Xamarin.Forms.Image) und [`Label`](xref:Xamarin.Forms.Label) definieren beide [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue)-Werte, die angewendet werden, wenn der Bindungspfad `null` zurückgibt. Daher werden die durch die `TargetNullValue`-Eigenschaften definierten Werte für jedes Objekt in der Sammlung angezeigt, bei dem die Eigenschaften `ImageUrl` und `Location` nicht definiert sind. Beachten Sie, dass hier die `TargetNullValue`-Eigenschaftswerte durch einfache Anführungszeichen (Apostrophe) getrennt sind.

Anstatt [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue)-Eigenschaftswerte inline zu definieren, wird empfohlen, diese als Ressourcen in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) zu definieren. Der Vorteil dieses Ansatzes ist, dass solche Werte einmal an einem einzigen Speicherort definiert werden und einfacher lokalisierbar sind. Die Ressourcen können dann mithilfe der `StaticResource`-Markuperweiterung abgerufen werden:

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> Es ist nicht möglich, die `TargetNullValue`-Eigenschaft mit einem Bindungsausdruck festzulegen.

Dies ist das Programm, das ausgeführt wird:

[![TargetNullValue Binding](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue Binding")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue Binding")

Wenn die `TargetNullValue`-Eigenschaft nicht in einem Bindungsausdruck festgelegt ist, wird ein Quellwert von `null` konvertiert, falls ein Wertkonverter definiert ist, und formatiert, falls ein `StringFormat` definiert ist. Das Ergebnis wird dann auf das Ziel festgelegt. Wenn die `TargetNullValue`-Eigenschaft festgelegt ist, wird ein Quellwert von `null` konvertiert, falls ein Wertkonverter definiert ist, und wenn dieser nach der Konvertierung immer noch `null` ist, wird der Wert der `TargetNullValue`-Eigenschaft auf das Ziel festgelegt.

> [!IMPORTANT]
> Zeichenfolgen werden in einem Bindungsausdruck nicht formatiert, wenn die `TargetNullValue`-Eigenschaft festgelegt ist.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
