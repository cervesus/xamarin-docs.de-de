---
title: Erstellen einer DataTemplateSelector-Klasse in Xamarin.Forms
description: In diesem Artikel wird veranschaulicht, wie eine DataTemplateSelector-Klasse erstellt und genutzt wird, die zum Auswählen einer DataTemplate-Klasse zur Laufzeit verwendet werden kann, basierend auf dem Wert einer datengebundenen Eigenschaft.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7cbeeb9a0eed37ec109b2e71c46e3f04cd08822d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936447"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Erstellen einer DataTemplateSelector-Klasse in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplateselector)

_Eine DataTemplateSelector-Klasse kann verwendet werden zum Auswählen einer DataTemplate-Klasse zur Laufzeit, basierend auf dem Wert der datengebundenen Eigenschaft. Dadurch können mehrere DataTemplate-Klassen auf den gleichen Objekttyp angewendet werden, um die Darstellung bestimmter Objekte anzupassen. In diesem Artikel wird veranschaulicht, wie Sie eine DataTemplateSelector-Klasse erstellen und nutzen können._

Eine Datenvorlagenauswahl ermöglicht Szenarios wie eine [`ListView`](xref:Xamarin.Forms.ListView)-Bindung an eine Sammlung von Objekten, bei der die Darstellung jedes Objekts in der `ListView`-Klasse zur Laufzeit ausgewählt werden kann, indem durch die Datenvorlagenauswahl eine bestimmte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zurückgegeben wird.

## <a name="creating-a-datatemplateselector"></a>Erstellen einer DataTemplateSelector-Klasse

Eine Datenvorlagenauswahl wird implementiert durch das Erstellen einer Klasse, die von der [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)-Klasse erbt. Die `OnSelectTemplate`-Methode wird dann überschrieben, um eine bestimmte [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zurückzugeben, wie im folgenden Codebeispiel zu sehen ist:

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

Die `OnSelectTemplate`-Methode gibt, basierend auf dem Wert der `DateOfBirth`-Eigenschaft, die entsprechende Vorlage zurück. Die Vorlage, die zurückgegeben wird, ist der Wert der `ValidTemplate`-Eigenschaft oder der `InvalidTemplate`-Eigenschaft, welche bei der Nutzung der `PersonDataTemplateSelector`-Klasse festgelegt werden.

Eine Instanz der Datenvorlagenauswahl-Klasse kann dann Xamarin.Forms-Steuereigenschaften wie [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) zugewiesen werden. Eine Liste gültiger Eigenschaften finden Sie unter [Erstellen von Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Einschränkungen

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)-Instanzen weisen die folgenden Einschränkungen auf:

- Die `DataTemplateSelector`-Unterklasse muss bei mehrmaligem Abfragen immer die gleiche Vorlage für dieselben Daten zurückgeben.
- Die `DataTemplateSelector`-Unterklasse darf keine andere `DataTemplateSelector`-Unterklasse zurückgeben.
- Die `DataTemplateSelector`-Unterklasse darf bei jedem Aufruf keine neuen Instanzen einer `DataTemplate`-Klasse zurückgeben. Stattdessen muss die gleiche Instanz zurückgegeben werden. Die Unterlassung führt zu einem Arbeitsspeicherverlust und zur Deaktivierung der Benutzeroberflächenvirtualisierung.
- Unter Android sind nicht mehr als 20 verschiedene Datenvorlagen pro `ListView`-Klasse möglich.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Nutzen einer DataTemplateSelector-Klasse in XAML

In XAML kann die `PersonDataTemplateSelector`-Klasse instanziiert werden, indem sie als Ressource erklärt wird, wie im folgendem Codebeispiel zu sehen ist:

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

Diese [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Klasse auf Seitenebene definiert zwei [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Instanzen und eine `PersonDataTemplateSelector`-Instanz. Die `PersonDataTemplateSelector`-Instanz legt ihre `ValidTemplate`- und `InvalidTemplate`-Eigenschaften auf die entsprechenden `DataTemplate`-Instanzen fest, indem die `StaticResource`-Markuperweiterung verwendet wird. Beachten Sie, dass die Ressourcen zwar in der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Klasse der Seite definiert werden, sie aber dennoch auch auf Steuerelementebene oder Anwendungsebene definiert werden können.

Die `PersonDataTemplateSelector`-Instanz wird genutzt, indem sie der [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft zugewiesen wird, wie im folgenden Codebeispiel zu sehen ist:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Zur Laufzeit ruft die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse die `PersonDataTemplateSelector.OnSelectTemplate`-Methode für jedes Element in der zugrunde liegenden Sammlung. Dabei wird das Datenobjekt als `item`-Parameter übergeben. Die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse, welche durch die Methode zurückgegeben wird, wird dann auf dieses Objekt angewendet.

Die folgenden Screenshots zeigen das Ergebnis der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse bei Anwendung des `PersonDataTemplateSelector`-Klasse auf jedes Objekt in der zugrunde liegenden Sammlung:

![ListView mit einer Datenvorlagenauswahl](selector-images/data-template-selector.png)

Jedes `Person`-Objekt, das einen `DateOfBirth`-Eigenschaftswert größer oder gleich 1980 aufweist, wird in grün angezeigt, alle übrigen Objekte werden in rot angezeigt.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Nutzen einer DataTemplateSelector-Klasse in C&num;

In C# kann die `PersonDataTemplateSelector`-Klasse instanziiert und der [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1)-Eigenschaft zugewiesen werden, wie im folgenden Codebeispiel zu sehen ist:

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

Die `PersonDataTemplateSelector`-Instanz legt ihre `ValidTemplate`- und `InvalidTemplate`-Eigenschaften auf die entsprechenden, durch die `SetupDataTemplates`-Methode erstellten, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Instanzen fest. Zur Laufzeit ruft die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse die `PersonDataTemplateSelector.OnSelectTemplate`-Methode für jedes Element in der zugrunde liegenden Sammlung. Dabei wird das Datenobjekt als `item`-Parameter übergeben. Die `DataTemplate`-Klasse, welche durch die Methode zurückgegeben wird, wird dann auf dieses Objekt angewendet.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie eine [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)-Klasse erstellt und genutzt wird. Eine `DataTemplateSelector`-Klasse kann verwendet werden, um eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zur Laufzeit auszuwählen, basierend auf dem Wert der datengebundenen Eigenschaft. Dadurch können mehrere `DataTemplate`-Instanzen auf den gleichen Objekttyp angewendet werden, um die Darstellung bestimmter Objekte anzupassen.

## <a name="related-links"></a>Verwandte Links

- [Data Template Selector (Datenvorlagenauswahl (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplateselector)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
