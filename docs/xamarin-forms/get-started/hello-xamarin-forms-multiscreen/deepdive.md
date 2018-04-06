---
title: 'Xamarin.Forms Multiscreen: Ausführliche Erläuterungen'
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: f4e6dace59c14a0e30ef4cdea2c7dd490d645057
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms Multiscreen: Ausführliche Erläuterungen

In dem Artikel [Xamarin.Forms Multiscreen Quickstart (Xamarin.Forms Multiscreen: Schnellstart)](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md) wurde die Phoneword-Anwendung dahingehend erweitert, dass eine zweite Anzeige hinzugefügt wurde, über die der Anrufverlauf der Anwendung verfolgt wird. In diesem Artikel wird betrachtet, welche Elemente erstellt wurden, um ein Verständnis der Seitennavigation und der Datenbindung in einer Xamarin.Forms-Anwendung zu entwickeln.

## <a name="navigation"></a>Navigation

Xamarin.Forms stellt ein integriertes Navigationsmodell bereit, mit dem die Navigation und die Benutzerfreundlichkeit von mehreren Seiten verwaltet wird. Dieses Modell implementiert einen LIFO-Stapel mit `Page`-Objekten. Um von einer Seite zu einer anderen zu wechseln, überträgt die Anwendung eine neue Seite auf diesen Stapel. Um zur vorherigen Seite zurückzugehen, entfernt die Anwendung die aktuelle Seite per Pop von dem Stapel.

Xamarin.Forms verfügt über eine [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Klasse, mit der der Stapel mit [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Objekten verwaltet wird. Die `NavigationPage`-Klasse platziert außerdem eine Navigationsleiste oben auf der Seite, in der ein Titel und eine für die Plattform angemessene <span class="uiitem">Zurück</span>-Schaltfläche, über die man zurück auf die vorherige Seite wechseln kann, angezeigt wird. Im folgenden Codebeispiel wird gezeigt, wie man eine `NavigationPage` um die erste Seite in einer Anwendung schließt:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Alle [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Instanzen (Inhaltsseiteninstanzen) beinhalten eine [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)-Eigenschaft zum Ändern des Seitenstapels. Diese Methoden sollten nur angewendet werden, wenn die Anwendung eine [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) einschließt. Um zur `CallHistoryPage` navigieren zu können, muss die [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/)-Methode wie im folgenden Codebeispiel angewendet werden:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

Dadurch wird das neue `CallHistoryPage`-Objekt auf den Navigationsstapel übertragen. Um programmgesteuert zur ursprünglichen Seite zurückgehen zu können, muss das `CallHistoryPage`-Objekt die [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/)-Methode anwenden. Dies wird im folgenden Codebeispiel dargestellt:

```csharp
await Navigation.PopAsync();
```

Demgegenüber ist dieser Code in der Phoneword-Anwendung nicht erforderlich, da die [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Klasse eine Navigationsleiste oben auf der Seite platziert, die auch einen für die Plattform angemessene Schaltfläche <span class="uiitem">Zurück</span> beinhaltet, über die man zurück auf die vorherige Seite kommt.

## <a name="data-binding"></a>Datenbindung

Datenbindung wird verwendet, um die Vorgänge zu vereinfachen, über die in einer Xamarin.Forms-Anwendung Daten angezeigt werden und interagieren. Sie stellt eine Verbindung zwischen der Benutzeroberfläche und der zugrundeliegenden Anwendung her. Die [`BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)-Klasse beinhaltet einen Großteil der Infrastruktur, um die Datenbindung zu unterstützen.

Durch die Datenbindung wird die Beziehung zwischen zwei Objekten definiert. Das *Quellobjekt* stellt die Daten zur Verfügung. Das *Zielobjekt* verarbeitet die Daten aus dem Quellobjekt (und zeigt sie häufig an). In der Phoneword-Anwendung ist das Steuerelement [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), über das Telefonnummern angezeigt werden, das Bindungsziel. Gleichzeitig dient die `PhoneNumbers`-Sammlung als Bindungsquelle.

Die `PhoneNumbers`-Sammlung wird in der `App`-Klasse deklariert und initialisiert. Dies zeigt das folgende Codebeispiel:

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

Die Instanz [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird in der `CallHistoryPage`-Klasse deklariert und initialisiert. Dies zeigt das folgende Codebeispiel:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

In diesem Beispiel zeigt das Steuerelement [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) die `IEnumerable`-Sammlung von Daten, an die die Eigenschaft [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) gebunden ist. Die Datensammlung kann aus Objekten jeglicher Art bestehen. Standardmäßig verwendet `ListView` die `ToString`-Methode jedes Elements, um dieses anzuzeigen. Die Markuperweiterung [`x:Static`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) wird verwendet, um anzuzeigen, dass die Eigenschaft `ItemsSource` an die statische Eigenschaft `PhoneNumbers`der `App`-Klasse, die im `local`- Namespace gefunden werden kann, gebunden ist.

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics (Datenbindungsgrundlagen)](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Weitere Informationen über XAML-Markuperweiterungen finden Sie unter [XAML Markup Extensions (XAML-Markuperweiterungen)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Zusätzliche in Phoneword eingeführte Konzepte

Über die [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird eine Sammlung von Elementen auf dem Bildschirm angezeigt. Jedes Element in der `ListView` ist in einer einzelnen Zelle enthalten. Weitere Informationen zum Steuerelement `ListView` finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Seitennavigation und Datenbindung in einer Xamarin.Forms-Anwendung thematisiert, und deren Verwendung in einer plattformübergreifenden Multiscreen-Anwendung erläutert.
