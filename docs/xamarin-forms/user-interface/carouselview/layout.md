---
title: Xamarin. Forms carouselview
description: Standardmäßig werden die Elemente in einer "carouselview" in einer horizontalen Liste angezeigt. Sie hat jedoch auch Zugriff auf dieselben Layouts wie CollectionView, einschließlich vertikaler Ausrichtung.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 5bbaeead524089ea604cfd9a9fd7f3a85d04e724
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908343"
---
# <a name="xamarinforms-carouselview-layouts"></a>Xamarin. Forms carouselview-Layouts

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)

## <a name="introduction"></a>Einführung

Ein Großteil der Layoutfunktionen, die für carouselview verfügbar sind, stammt aus CollectionView. In der [layoutdokumentation](../collectionview/layout.md) von CollectionView finden Sie Informationen zur Verwendung verschiedener Layouts.

## <a name="differences-from-collectionview"></a>Unterschiede zu CollectionView

Standardmäßig werden Elemente in einer carouselview horizontal ausgerichtet, ebenso wie die typische Funktion eines Karussell in-apps.

Carouselview bietet auch einige zusätzliche Eigenschaften:

> [!IMPORTANT]
> Weitere Eigenschaften für "carouselview" befinden sich noch in der Entwicklungsphase, und diese Liste ist noch nicht fertig.

| API | Funktion |
|---|---|---|
| Anzahl von Elementen | Legt die Anzahl der Elemente fest, die jeder Seite des aktuellen Elements angezeigt werden. Der Standardwert ist 0.
| "Peer"-Gruppen | Bietet eine Möglichkeit, dem Benutzer sichtbar zu machen, dass die carouselview über zusätzliche Elemente verfügt, zu denen Sie scrollen können, indem Sie den Grad der Sichtbarkeit neben dem aktuellen Element anpassen.

## <a name="setting-the-number-of-fully-visible-items"></a>Festlegen der Anzahl von vollständig sichtbaren Elementen

Standardmäßig zeigt carouselview ein Element vollständig auf dem Bildschirm an. Benutzer können die `NumberOfSideItems` -Eigenschaft so festlegen, dass neben dem aktuellen Element weitere Elemente angezeigt werden. Beachten Sie, dass jeder Wert `PeekAreaInsets` , der auf festgelegt ist, immer noch

```xaml
<StackLayout Margin="20">
    <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" NumberOfSideItems="1">
        <CarouselView.ItemTemplate>
            <DataTemplate>
                <Frame BorderColor="Black">
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Label Grid.Column="1"
                            Text="{Binding Name}"
                            FontAttributes="Bold"
                            FontSize="14"/>
                        <Label Grid.Row="1"
                            Grid.Column="1"
                            Text="{Binding Location}"
                            FontAttributes="Italic"
                            FontSize="12"
                            VerticalOptions="End" />
                    </Grid>
                </Frame>
            </DataTemplate>
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

[ ![Screenshot eines carouselview-Elements mit neben Elementen auf Android](carouselview-images/side-items.png "carouselview-Elementen") ] (carouselview-images/side-items-large.png#lightbox "Elemente der carouselview-Seite")

## <a name="making-adjacent-items-partially-visible"></a>Partielle sichtbar machen von angrenzenden Elementen

Wenn Sie eine carouselview in der App verwenden, kann es hilfreich sein, dem Benutzer mitzuteilen, dass die carouselview in einer solchen Weise funktioniert, indem `PeekAreaInsets` die-Eigenschaft auf einen Wert ungleich 0 (Standard) festgelegt wird, der Sie teilweise auf dem Bildschirm verfügbar macht.

```xaml
<StackLayout Margin="20">
  <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" PeekAreaInsets="100">
      <CarouselView.ItemTemplate>
          <DataTemplate>
              <Frame BorderColor="Black">
                  <Grid>
                      <Grid.RowDefinitions>
                          <RowDefinition Height="Auto" />
                          <RowDefinition Height="Auto" />
                      </Grid.RowDefinitions>
                      <Grid.ColumnDefinitions>
                          <ColumnDefinition Width="Auto" />
                          <ColumnDefinition Width="Auto" />
                      </Grid.ColumnDefinitions>
                      <Label Grid.Column="1"
                          Text="{Binding Name}"
                          FontAttributes="Bold"
                          FontSize="14"/>
                      <Label Grid.Row="1"
                          Grid.Column="1"
                          Text="{Binding Location}"
                          FontAttributes="Italic"
                          FontSize="12"
                          VerticalOptions="End" />
                  </Grid>
              </Frame>
          </DataTemplate>
      </CarouselView.ItemTemplate>
  </CarouselView>
</StackLayout>
```

[ ![Screenshot eines carouselview-Elements mit neben Elementen auf Android](carouselview-images/peek-area-insets.png "carouselview-Elementen") ] (carouselview-images/peek-area-insets-large.png#lightbox "Elemente der carouselview-Seite")

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)
- [Dokumentation zum CollectionView-Layout](../collectionview/layout.md)
