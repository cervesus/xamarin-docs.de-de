---
title: Erstellen einer DataTemplateSelector-Klasse in Xamarin.Forms
description: Erstellen einer DataTemplateSelector-Klasse in Xamarin.Forms
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 74650eb2c52f1da9d0c539b711784896267ed183
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135953"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplateselector)

_Eine DataTemplateSelector-Klasse kann verwendet werden zum Auswählen einer DataTemplate-Klasse zur Laufzeit, basierend auf dem Wert der datengebundenen Eigenschaft. Dadurch können mehrere DataTemplate-Klassen auf den gleichen Objekttyp angewendet werden, um die Darstellung bestimmter Objekte anzupassen. In diesem Artikel wird veranschaulicht, wie Sie eine DataTemplateSelector-Klasse erstellen und nutzen können._

Eine Datenvorlagenauswahl ermöglicht Szenarios wie eine [`ListView`](xref:Xamarin.Forms.ListView)-Bindung an eine Sammlung von Objekten, bei der die Darstellung jedes Objekts in der `ListView`-Klasse zur Laufzeit ausgewählt werden kann, indem durch die Datenvorlagenauswahl eine bestimmte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zurückgegeben wird.

Erstellen einer DataTemplateSelector-Klasse

## <a name="creating-a-datatemplateselector"></a>Eine Datenvorlagenauswahl wird implementiert durch das Erstellen einer Klasse, die von der [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)-Klasse erbt.

Die `OnSelectTemplate`-Methode wird dann überschrieben, um eine bestimmte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zurückzugeben, wie im folgenden Codebeispiel zu sehen ist: Die `OnSelectTemplate`-Methode gibt, basierend auf dem Wert der `DateOfBirth`-Eigenschaft, die entsprechende Vorlage zurück.

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

Die Vorlage, die zurückgegeben wird, ist der Wert der `ValidTemplate`-Eigenschaft oder der `InvalidTemplate`-Eigenschaft, welche bei der Nutzung der `PersonDataTemplateSelector`-Klasse festgelegt werden. Eine Instanz der Datenvorlagenauswahl-Klasse kann dann Xamarin.Forms-Steuereigenschaften wie [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) zugewiesen werden.

Eine Liste gültiger Eigenschaften finden Sie unter [Erstellen von Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md). Einschränkungen

### <a name="limitations"></a>[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)-Instanzen weisen die folgenden Einschränkungen auf:

Die `DataTemplateSelector`-Unterklasse muss bei mehrmaligem Abfragen immer die gleiche Vorlage für dieselben Daten zurückgeben.

- Die `DataTemplateSelector`-Unterklasse darf keine andere `DataTemplateSelector`-Unterklasse zurückgeben.
- Die `DataTemplateSelector`-Unterklasse darf bei jedem Aufruf keine neuen Instanzen einer `DataTemplate`-Klasse zurückgeben.
- Stattdessen muss die gleiche Instanz zurückgegeben werden. Die Unterlassung führt zu einem Arbeitsspeicherverlust und zur Deaktivierung der Benutzeroberflächenvirtualisierung. Unter Android sind nicht mehr als 20 verschiedene Datenvorlagen pro `ListView`-Klasse möglich.
- Nutzen einer DataTemplateSelector-Klasse in XAML

## <a name="consuming-a-datatemplateselector-in-xaml"></a>In XAML kann die `PersonDataTemplateSelector`-Klasse instanziiert werden, indem sie als Ressource erklärt wird, wie im folgendem Codebeispiel zu sehen ist:

Diese [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Klasse auf Seitenebene definiert zwei [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Instanzen und eine `PersonDataTemplateSelector`-Instanz.

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

Die `PersonDataTemplateSelector`-Instanz legt ihre `ValidTemplate`- und `InvalidTemplate`-Eigenschaften auf die entsprechenden `DataTemplate`-Instanzen fest, indem die `StaticResource`-Markuperweiterung verwendet wird. Beachten Sie, dass die Ressourcen zwar in der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Klasse der Seite definiert werden, sie aber dennoch auch auf Steuerelementebene oder Anwendungsebene definiert werden können. Die `PersonDataTemplateSelector`-Instanz wird genutzt, indem sie der [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft zugewiesen wird, wie im folgenden Codebeispiel zu sehen ist:

Zur Laufzeit ruft die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse die `PersonDataTemplateSelector.OnSelectTemplate`-Methode für jedes Element in der zugrunde liegenden Sammlung. Dabei wird das Datenobjekt als `item`-Parameter übergeben.

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse, welche durch die Methode zurückgegeben wird, wird dann auf dieses Objekt angewendet. Die folgenden Screenshots zeigen das Ergebnis der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse bei Anwendung des `PersonDataTemplateSelector`-Klasse auf jedes Objekt in der zugrunde liegenden Sammlung:

Jedes `Person`-Objekt, das einen `DateOfBirth`-Eigenschaftswert größer oder gleich 1980 aufweist, wird in grün angezeigt, alle übrigen Objekte werden in rot angezeigt.

![](selector-images/data-template-selector.png "ListView with a Data Template Selector")

Nutzen einer DataTemplateSelector-Klasse in C&num;

## <a name="consuming-a-datatemplateselector-in-cnum"></a>In C# kann die `PersonDataTemplateSelector`-Klasse instanziiert und der [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft zugewiesen werden, wie im folgenden Codebeispiel zu sehen ist:

Die `PersonDataTemplateSelector`-Instanz legt ihre `ValidTemplate`- und `InvalidTemplate`-Eigenschaften auf die entsprechenden, durch die `SetupDataTemplates`-Methode erstellten, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Instanzen fest.

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

Zur Laufzeit ruft die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse die `PersonDataTemplateSelector.OnSelectTemplate`-Methode für jedes Element in der zugrunde liegenden Sammlung. Dabei wird das Datenobjekt als `item`-Parameter übergeben. Die `DataTemplate`-Klasse, welche durch die Methode zurückgegeben wird, wird dann auf dieses Objekt angewendet. Zusammenfassung

## <a name="summary"></a>In diesem Artikel wurde veranschaulicht, wie eine [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)-Klasse erstellt und genutzt wird.

Eine `DataTemplateSelector`-Klasse kann verwendet werden, um eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zur Laufzeit auszuwählen, basierend auf dem Wert der datengebundenen Eigenschaft. Dadurch können mehrere `DataTemplate`-Instanzen auf den gleichen Objekttyp angewendet werden, um die Darstellung bestimmter Objekte anzupassen. Verwandte Links

## <a name="related-links"></a>[Data Template Selector (Datenvorlagenauswahl (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplateselector)

- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
- ListView mit einer Datenvorlagenauswahl
