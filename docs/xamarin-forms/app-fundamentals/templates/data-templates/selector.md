---
title: Erstellen eine DataTemplateSelector
description: Eine DataTemplateSelector kann verwendet werden, zum Auswählen einer DataTemplate zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere DataTemplates auf den gleichen Typ des Objekts, zum Anpassen der Darstellung bestimmter Objekte angewendet werden soll. In diesem Artikel wird das Erstellen und nutzen einen DataTemplateSelector veranschaulicht.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ad8cce32cb9cfe2ea5e78f11b1250440371a9851
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847191"
---
# <a name="creating-a-datatemplateselector"></a>Erstellen eine DataTemplateSelector

_Eine DataTemplateSelector kann verwendet werden, zum Auswählen einer DataTemplate zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere DataTemplates auf den gleichen Typ des Objekts, zum Anpassen der Darstellung bestimmter Objekte angewendet werden soll. In diesem Artikel wird das Erstellen und nutzen einen DataTemplateSelector veranschaulicht._

Eine Vorlage Datenauswahl ermöglicht Szenarien wie z. B. eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Binden an eine Auflistung von Objekten, in denen die Darstellung der einzelnen Objekte in der `ListView` zur Laufzeit durch das Zurückgeben von Datenvorlagenauswahl ausgewählt werden kann ein bestimmte [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/).

## <a name="creating-a-datatemplateselector"></a>Erstellen eine DataTemplateSelector

Eine Vorlage Datenauswahl wird durch Erstellen einer Klasse, die von Erben implementiert [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). Die `OnSelectTemplate` Methode wird überschrieben, um einen bestimmten zurückzugeben [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), wie im folgenden Codebeispiel gezeigt:

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

Die `OnSelectTemplate` Methodenrückgabe die entsprechende Vorlage basierend auf dem Wert, der die `DateOfBirth` Eigenschaft. Die Vorlage, die zurückgegeben wird der Wert des der `ValidTemplate` Eigenschaft oder die `InvalidTemplate` -Eigenschaft, die festgelegt werden, bei der Nutzung der `PersonDataTemplateSelector`.

Eine Instanz der Vorlagenklasse Selektor Daten kann dann zugewiesen werden Xamarin.Forms Steuerelementeigenschaften wie z. B. [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/). Eine Liste der gültigen Eigenschaften, finden Sie unter [Erstellen einer DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Einschränkungen

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) Instanzen weisen die folgenden Einschränkungen:

- Die `DataTemplateSelector` Unterklasse muss stets die gleiche Vorlage für die gleichen Daten zurück, wenn mehrere Male abgefragt.
- Die `DataTemplateSelector` Unterklasse muss nicht zurückgeben, eine andere `DataTemplateSelector` Unterklasse.
- Die `DataTemplateSelector` Unterklasse muss nicht zurückgeben neue Instanzen einer `DataTemplate` bei jedem Aufruf. Stattdessen muss die gleiche Instanz zurückgegeben werden. Bei unterlassen erstellt einen Speicherverlust und Virtualisierung wird deaktiviert.
- Auf Android-Geräten werden nicht mehr als 20 verschiedene Datenvorlagen pro `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Nutzen eine DataTemplateSelector in XAML

In XAML wird die `PersonDataTemplateSelector` instanziiert werden, indem deren Deklaration als eine Ressource, wie im folgenden Codebeispiel gezeigt:

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

Diese Seitenebene [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert zwei [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Instanzen und eine `PersonDataTemplateSelector` Instanz. Die `PersonDataTemplateSelector` Instanz legt seine `ValidTemplate` und `InvalidTemplate` -Eigenschaften auf die entsprechende `DataTemplate` Instanzen mithilfe der `StaticResource` Markuperweiterung. Beachten Sie, dass die Ressourcen auf der Seite definiert sind [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), kann auch auf das Steuerelement oder Anwendungsebene definiert werden.

Die `PersonDataTemplateSelector` Instanz durch Zuweisung zu belegt die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Eigenschaft, wie im folgenden Codebeispiel gezeigt:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Zur Laufzeit die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Aufrufe der `PersonDataTemplateSelector.OnSelectTemplate` für jedes der Elemente in der zugrunde liegenden Auflistung verwendet werden, mit dem Aufruf übergeben das Datenobjekt als Methode der `item` Parameter. Die [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) der zurückgegeben wird, indem die Methode auf das Objekt angewendet.

Die folgenden Screenshots zeigen das Ergebnis der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Anwenden der `PersonDataTemplateSelector` auf jedes Objekt in der zugrunde liegenden Auflistung:

![](selector-images/data-template-selector.png "ListView mit einer Vorlage Datenauswahl")

Alle `Person` Objekt mit einem `DateOfBirth` Eigenschaftswert größer als oder gleich 1980 wird grün angezeigt, mit der übrigen Objekte, die in Rot angezeigt wird.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Nutzen eine DataTemplateSelector in C&num;

In c# ist die `PersonDataTemplateSelector` instanziiert und zugeordnet werden kann die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) -Eigenschaft verwenden, wie im folgenden Codebeispiel wird gezeigt:

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

Die `PersonDataTemplateSelector` Instanz legt seine `ValidTemplate` und `InvalidTemplate` -Eigenschaften auf die entsprechende [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Instanzen erstellt, indem die `SetupDataTemplates` Methode. Zur Laufzeit die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Aufrufe der `PersonDataTemplateSelector.OnSelectTemplate` für jedes der Elemente in der zugrunde liegenden Auflistung verwendet werden, mit dem Aufruf übergeben das Datenobjekt als Methode der `item` Parameter. Die `DataTemplate` der zurückgegeben wird, indem die Methode auf das Objekt angewendet.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie erstellen und nutzen einen [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). Ein `DataTemplateSelector` dienen zum Auswählen einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere `DataTemplate` Instanzen auf den gleichen Typ des Objekts, zum Anpassen der Darstellung bestimmter Objekte angewendet werden soll.


## <a name="related-links"></a>Verwandte Links

- [Vorlage Datenauswahl (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
