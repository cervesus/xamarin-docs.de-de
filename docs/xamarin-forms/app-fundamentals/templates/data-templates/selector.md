---
title: Erstellen einer Xamarin.Forms-DataTemplateSelector
description: Dieser Artikel veranschaulicht das Erstellen und nutzen eine DataTemplateSelector, die verwendet werden kann, wählen Sie ein DataTemplate-Element zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994516"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Erstellen einer Xamarin.Forms-DataTemplateSelector

_Eine DataTemplateSelector kann verwendet werden, wählen Sie ein DataTemplate-Element zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere DataTemplates auf den gleichen Typ des Objekts, Anpassen die Darstellung bestimmter Objekte angewendet werden. Dieser Artikel veranschaulicht das Erstellen und eine DataTemplateSelector nutzen._

Eine Datenvorlagenauswahl ermöglicht Szenarien wie z. B. eine [ `ListView` ](xref:Xamarin.Forms.ListView) Binden an eine Auflistung von Objekten, in dem die Darstellung der einzelnen Objekte in der `ListView` kann zur Laufzeit ausgewählt werden, durch die Datenvorlagenauswahl Zurückgeben einer bestimmte [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

## <a name="creating-a-datatemplateselector"></a>Erstellen eine DataTemplateSelector

Eine Datenvorlagenauswahl wird durch Erstellen einer Klasse, die von Erben implementiert [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). Die `OnSelectTemplate` Methode wird dann überschrieben, um eine bestimmte zurückzugeben [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), wie im folgenden Codebeispiel gezeigt:

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

Die `OnSelectTemplate` Methode gibt zurück, die entsprechende Vorlage basierend auf den Wert der `DateOfBirth` Eigenschaft. Die Vorlage, die zurückgegeben wird der Wert des der `ValidTemplate` Eigenschaft oder das `InvalidTemplate` -Eigenschaft, die festgelegt werden, bei der Nutzung der `PersonDataTemplateSelector`.

Eine Instanz der Daten Selector-Klasse können Sie dann auf Eigenschaften von Xamarin.Forms-Steuerelements wie folgt zugewiesen werden [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1). Eine Liste der gültigen Eigenschaften, finden Sie unter [erstellen eine DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Einschränkungen

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) Instanzen weisen die folgenden Einschränkungen:

- Die `DataTemplateSelector` Unterklasse muss immer die gleiche Vorlage für dieselben Daten zurückgeben, wenn mehrere Male abgefragt.
- Die `DataTemplateSelector` Unterklasse muss nicht zurückgeben, eine andere `DataTemplateSelector` Unterklasse.
- Die `DataTemplateSelector` Unterklasse muss keine neue Instanzen von Zurückgeben einer `DataTemplate` bei jedem Aufruf. Stattdessen muss die gleiche Instanz zurückgegeben werden. Bei unterlassen einen Speicherverlust erstellt, und es wird die Virtualisierung deaktiviert.
- Unter Android werden nicht mehr als 20 verschiedene Datenvorlagen pro `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Nutzen eine DataTemplateSelector in XAML

In XAML die `PersonDataTemplateSelector` können instanziiert werden, indem deren Deklaration als eine Ressource, wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

Diese auf Seitenebene [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definiert zwei [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Instanzen und einem `PersonDataTemplateSelector` Instanz. Die `PersonDataTemplateSelector` Instanz legt seine `ValidTemplate` und `InvalidTemplate` -Eigenschaften auf die entsprechende `DataTemplate` Instanzen mithilfe der `StaticResource` Markuperweiterung. Beachten Sie, dass die Ressourcen in der Seite definiert sind [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), sie können auch auf der Steuerungsebene oder Anwendungsebene definiert werden.

Die `PersonDataTemplateSelector` Instanz wird durch die Zuweisung zu verbraucht die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) -Eigenschaft, wie im folgenden Codebeispiel gezeigt:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Zur Laufzeit die [ `ListView` ](xref:Xamarin.Forms.ListView) Aufrufe der `PersonDataTemplateSelector.OnSelectTemplate` -Methode für jedes der Elemente in der zugrunde liegenden Auflistung verwendet werden, mit dem Aufruf aus, und übergeben das Objekt als die `item` Parameter. Die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) das zurückgegeben wird, indem die Methode klicken Sie dann auf das Objekt angewendet.

Die folgenden Screenshots zeigen das Ergebnis der [ `ListView` ](xref:Xamarin.Forms.ListView) Anwenden der `PersonDataTemplateSelector` für jedes Objekt in der zugrunde liegenden Auflistung:

![](selector-images/data-template-selector.png "ListView mit eine Datenvorlagenauswahl")

Alle `Person` Objekt mit einem `DateOfBirth` Eigenschaftswert größer als oder gleich 1980 wird in grün angezeigt, mit der übrigen Objekte, die in Rot angezeigt wird.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Nutzen eine DataTemplateSelector in C&num;

In c# die `PersonDataTemplateSelector` instanziiert und zugewiesen werden können die [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) -Eigenschaft, wie im folgenden Codebeispiel gezeigt:

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

Die `PersonDataTemplateSelector` Instanz legt die `ValidTemplate` und `InvalidTemplate` -Eigenschaften auf die entsprechende [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) von erstellten Instanzen der `SetupDataTemplates` Methode. Zur Laufzeit die [ `ListView` ](xref:Xamarin.Forms.ListView) Aufrufe der `PersonDataTemplateSelector.OnSelectTemplate` -Methode für jedes der Elemente in der zugrunde liegenden Auflistung verwendet werden, mit dem Aufruf aus, und übergeben das Objekt als die `item` Parameter. Die `DataTemplate` das zurückgegeben wird, indem die Methode klicken Sie dann auf das Objekt angewendet.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat veranschaulicht das Erstellen und nutzen einen [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector). Ein `DataTemplateSelector` dienen zum Auswählen einer [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere `DataTemplate` -Instanzen, die in den gleichen Typ des Objekts, Anpassen die Darstellung bestimmter Objekte angewendet werden.


## <a name="related-links"></a>Verwandte Links

- [Datenvorlagenauswahl (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
