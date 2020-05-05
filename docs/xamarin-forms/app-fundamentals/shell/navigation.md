---
title: Navigation in der Xamarin.Forms-Shell
description: Xamarin.Forms-Shell-Anwendungen können eine URI-basierte Navigationsumgebung verwenden, die die Navigation zu einer beliebigen Seite in der Anwendung ermöglicht, ohne einer festen Navigationshierarchie folgen zu müssen.
ms.prod: xamarin
ms.assetid: 57079D89-D1CB-48BD-9FEE-539CEC29EABB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: a40a2dc01c37773539089287d561f4c52ef7f6de
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516515"
---
# <a name="xamarinforms-shell-navigation"></a>Navigation in der Xamarin.Forms-Shell

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms-Shell enthält eine URI-basierte Navigationsumgebung, die Routen verwendet, um zu einer beliebigen Seite in der Anwendung zu navigieren, ohne einer festen Navigationshierarchie folgen zu müssen. Darüber hinaus bietet es auch die Möglichkeit, rückwärts zu navigieren, ohne alle Seiten des Navigationsstapels aufrufen zu müssen.

`Shell` definiert die folgenden navigationsbezogenen Eigenschaften:

- `BackButtonBehavior` vom Typ `BackButtonBehavior`: eine angefügte Eigenschaft, die das Verhalten der Zurück-Schaltfläche definiert.
- `CurrentItem` vom Typ `FlyoutItem`: das aktuell ausgewählte `FlyoutItem`.
- `CurrentState` vom Typ `ShellNavigationState`: der aktuelle Navigationszustand der `Shell`.
- `Current` vom Typ `Shell`: ein Typ-umgewandelter Alias für `Application.Current.MainPage`.

Die Eigenschaften `BackButtonBehavior`, `CurrentItem` und `CurrentState` werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass diese Eigenschaften Ziele von Datenverbindungen sein können.

Navigation erfolgt durch Aufrufen der `GoToAsync`-Methode aus der `Shell`-Klasse. Kurz bevor die Navigation erfolgt, wird ein `Navigating`-Ereignis ausgelöst, und ein `Navigated`-Ereignis wird ausgelöst, wenn die Navigation abgeschlossen ist.

> [!NOTE]
> Die Navigation kann in einer Xamarin.Forms Shell-Anwendung weiterhin unter Verwendung der Eigenschaft [Navigation](xref:Xamarin.Forms.NavigableElement.Navigation) durchgeführt werden. Weitere Informationen finden Sie unter [Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="routes"></a>Routen

Die Navigation erfolgt in einer Shell-Anwendung durch Angabe eines URI, zu dem navigiert werden soll. Navigation-URIs können drei Komponenten aufweisen:

- Eine *Route*, die den Pfad zu Inhalt definiert, der als Teil der visuellen Shell-Hierarchie vorhanden ist.
- Eine *Seite*. Seiten, die in der visuellen Shell-Hierarchie nicht vorhanden sind, können von überall innerhalb einer Shell-Anwendung per Push an den Navigationsstapel übertragen werden. Beispielsweise wird eine Elementdetailseite nicht in der visuellen Shell-Hierarchie definiert, sondern kann bei Bedarf per Push an den Navigationsstapel übertragen werden.
- Mindestens ein *Abfrageparameter*. Abfrageparameter sind Parameter, die während der Navigation an die Zielseite übergeben werden können.

Wenn ein Navigations-URI alle drei Komponenten enthält, lautet die Struktur „//route/page?queryParameters“.

### <a name="register-routes"></a>Registrieren von Routen

Routen können für `FlyoutItem`, `Tab` und `ShellContent`-Objekte über ihre `Route`-Eigenschaften definiert werden:

```xaml
<Shell ...>
    <FlyoutItem ...
                Route="animals">
        <Tab ...
             Route="domestic">
            <ShellContent ...
                          Route="cats" />
            <ShellContent ...
                          Route="dogs" />
        </Tab>
        <ShellContent ...
                      Route="monkeys" />
        <ShellContent ...
                      Route="elephants" />  
        <ShellContent ...
                      Route="bears" />
    </FlyoutItem>
    <ShellContent ...
                  Route="about" />                  
    ...
</Shell>
```

> [!NOTE]
> Allen Elementen in der Shell-Hierarchie ist eine Route zugeordnet. Wenn der Entwickler keine Route festgelegt, wird sie zur Laufzeit generiert. Die Konsistenz der generierten Routen wird jedoch nicht über verschiedene Anwendungssitzungen hinweg garantiert.

Dieses Beispiel aus der Beispielanwendung erstellt die folgende Routenhierarchie, die in der programmgesteuerten Navigation verwendet werden kann:

```
animals
  domestic
    cats
    dogs
  monkeys
  elephants
  bears
about
```

Um zum `ShellContent`-Objekt für die `dogs`-Route zu navigieren, lautet der absolute Routen-URI `//animals/domestic/dogs`. Um zum `ShellContent`-Objekt für die `about`-Route zu navigieren, lautet der absolute Routen-URI entsprechend `//about`.

> [!IMPORTANT]
> Eine `ArgumentException` wird beim Start der Anwendung ausgelöst, wenn eine doppelte Route erkannt wird. Diese Ausnahme wird auch ausgelöst, wenn mindestens zwei Routen auf derselben Ebene in der Hierarchie einen Routennamen gemeinsam verwenden.

#### <a name="register-page-routes"></a>Registrieren von Seitenrouten

Im Shell-Unterklassenkonstruktor oder an jedem anderen Ort, der vor dem Aufruf einer Route ausgeführt wird, können zusätzliche Routen explizit für alle Seiten registriert werden, die in der visuellen Shell-Hierarchie nicht repräsentiert werden:

```csharp
Routing.RegisterRoute("monkeydetails", typeof(MonkeyDetailPage));
Routing.RegisterRoute("beardetails", typeof(BearDetailPage));
Routing.RegisterRoute("catdetails", typeof(CatDetailPage));
Routing.RegisterRoute("dogdetails", typeof(DogDetailPage));
Routing.RegisterRoute("elephantdetails", typeof(ElephantDetailPage));
```

Dieses Beispiel registriert Elementdetailseiten, die nicht in der Shell-Unterklasse definiert sind, als Routen. Diese Seiten können dann von überall in der Anwendung über eine URI-basierte Navigation aufgerufen werden. Die Routen für solche Seiten werden als *globale Routen* bezeichnet.

> [!NOTE]
> Die Registrierung von Seiten, deren Routen mit der `Routing.RegisterRoute`-Methode registriert wurden, kann bei Bedarf mit der `Routing.UnRegisterRoute`-Methode aufgehoben werden.

Alternativ können Seiten bei Bedarf bei verschiedenen Routenhierarchien registriert werden:

```csharp
Routing.RegisterRoute("monkeys/details", typeof(MonkeyDetailPage));
Routing.RegisterRoute("bears/details", typeof(BearDetailPage));
Routing.RegisterRoute("cats/details", typeof(CatDetailPage));
Routing.RegisterRoute("dogs/details", typeof(DogDetailPage));
Routing.RegisterRoute("elephants/details", typeof(ElephantDetailPage));
```

Dieses Beispiel ermöglicht eine kontextbezogene Seitennavigation, bei der die Navigation zur `details`-Route von der Seite für die `monkeys`-Route die `MonkeyDetailPage` anzeigt. Entsprechend zeigt die Navigation zur `details`-Route von der Seite für die `elephants`-Route die `ElephantDetailPage` an.

> [!IMPORTANT]
> Eine `ArgumentException` wird ausgelöst, wenn die `Routing.RegisterRoute`-Methode versucht, dieselbe Route für mindestens zwei unterschiedliche Typen zu registrieren.

## <a name="perform-navigation"></a>Durchführen der Navigation

Um die Navigation durchzuführen, muss zuerst ein Verweis auf die Unterklasse `Shell` abgerufen werden. Dieser Verweis kann durch Umwandeln der `App.Current.MainPage`-Eigenschaft in ein `Shell`-Objekt oder über die `Shell.Current`-Eigenschaft erzielt werden. Die Navigation kann dann durch Aufrufen der `GoToAsync`-Methode für das `Shell`-Objekt durchgeführt werden. Diese Methode navigiert zu einem `ShellNavigationState` und gibt eine `Task` zurück, die nach Ende der Navigationsanimation abgeschlossen wird. Das `ShellNavigationState`-Objekt wird durch die `GoToAsync`-Methode aus einer `string` oder einem `Uri` gebildet, und verfügt über eine `Location`-Eigenschaft, die auf das Argument `string` oder `Uri` festgelegt ist.

Erfolgt die Navigation zu einer Route aus der visuellen Shell-Hierarchie, wird kein Navigationsstapel erstellt. Erfolgt die Navigation jedoch zu einer Seite, die sich nicht in der visuellen Shell-Hierarchie befindet, wird kein Navigationsstapel erstellt.

> [!NOTE]
> Der aktuelle Navigationsstatus der `Shell` kann über die `Shell.Current.CurrentState`-Eigenschaft abgerufen werden, die den URI der angezeigten Route in der `Location`-Eigenschaft enthält.

### <a name="absolute-routes"></a>Absolute Routen

Die Navigation kann durch Angeben eines gültigen absoluten URIs als Argument für die `GoToAsync`-Methode durchgeführt werden:

```csharp
await Shell.Current.GoToAsync("//animals/monkeys");
```

In diesem Beispiel erfolgt die Navigation zur Seite für die `monkeys`-Route, wobei die Route im `ShellContent`-Objekt definiert wird. Das `ShellContent`-Objekt, das die `monkeys`-Route darstellt, ist ein untergeordnetes Objekt eines `FlyoutItem`-Objekts mit der Route `animals`.

### <a name="relative-routes"></a>Relative Routen

Die Navigation kann auch durch Angeben eines gültigen relativen URIs als Argument für die `GoToAsync`-Methode durchgeführt werden. Das Routingsystem versucht daher, den URI mit einem `ShellContent`-Objekt abzugleichen. Wenn also alle Routen in einer Anwendung eindeutig sind, kann die Navigation durchgeführt werden, indem nur der eindeutige Routenname als relativer URI angegeben wird:

```csharp
await Shell.Current.GoToAsync("monkeydetails");
```

In diesem Beispiel erfolgt die Navigation zur Seite für die `monkeydetails`-Route.

Darüber hinaus werden die folgenden relativen Routenformate unterstützt:

| Format | Beschreibung |
| --- | --- |
| //*route* | In der Routenhierarchie wird die angegebene Route von der aktuell angezeigten Route nach oben gesucht. |
| ///*route* | In der Routenhierarchie wird die angegebene Route von der aktuell angezeigten Route nach unten gesucht. |

#### <a name="contextual-navigation"></a>Kontextbezogene Navigation

Relative Routen ermöglichen kontextbezogene Navigation. Nehmen wir beispielsweise die folgende Routenhierarchie:

```
monkeys
  details
bears
  details
```

Wenn die registrierte Seite für die `monkeys`-Route angezeigt wird, wird durch die Navigation zur `details`-Route die registrierte Seite für die `monkeys/details`-Route angezeigt. Wenn die registrierte Seite für die `bears`-Route angezeigt wird, wird durch die Navigation zur `details`-Route entsprechend die registrierte Seite für die `bears/details`-Route angezeigt. Informationen zum Registrieren der Routen in diesem Beispiel finden Sie unter [Registrieren von Seitenrouten](#register-page-routes).

### <a name="backwards-navigation"></a>Rückwärtsnavigation

Die Rückwärtsnavigation kann erfolgen, indem Sie ".." als Argument zur `GotoAsync`-Methode hinzufügen.

```csharp
await Shell.Current.GoToAsync("..");
```

Die Rückwärtsnavigation mit ".." kann auch folgendermaßen mit einer Route kombiniert werden:

```csharp
await Shell.Current.GoToAsync("../route");
```

In diesem Beispiel besteht der Gesamteffekt darin, rückwärts und dann zur angegebenen Route zu navigieren.

> [!IMPORTANT]
> Die Rückwärtsnavigation und die Navigation zu einer angegebenen Route sind nur möglich, wenn die Rückwärtsnavigation Sie zur aktuellen Position in der Routenhierarchie bringt, damit Sie zur angegebenen Route navigieren können.

Ebenso ist es möglich, mehrmals rückwärts und dann zu einer bestimmten Route zu navigieren:

```csharp
await Shell.Current.GoToAsync("../../route");
```

In diesem Beispiel besteht der Gesamteffekt darin, zweimal rückwärts und dann zur angegebenen Route zu navigieren.

> [!NOTE]
> Bei der Navigation mit ".." können auch Daten übergeben werden. Weitere Informationen finden Sie unter [Übergeben von Daten](#pass-data).

### <a name="invalid-routes"></a>Ungültige Routen

Die folgenden Routenformate sind ungültig:

| Format | Erklärung |
| --- | --- |
| *route* oder /*route* | Routen in der visuellen Hierarchie können nicht per Push an den Navigationsstapel übertragen werden. |
| //*page* oder ///*page* | Globale Routen dürfen derzeit nicht die einzige Seite im Navigationsstapel sein. Aus diesem Grund wird das absolute Routing zu globalen Routen nicht unterstützt. |

Die Verwendung einer der beiden Routenformate führt dazu, dass eine `Exception` ausgelöst wird.

> [!IMPORTANT]
> Der Versuch, zu einer nicht vorhandenen Route zu navigieren führt dazu, dass eine `ArgumentException` ausgelöst wird.

### <a name="debugging-navigation"></a>Debuggen der Navigation

Einige der Shell-Klassen werden mit dem `DebuggerDisplayAttribute` ergänzt, das festlegt, wie eine Klasse oder das Feld vom Debugger angezeigt wird. Dies kann helfen, Navigationsanforderungen zu debuggen, indem Daten angezeigt werden, die sich auf die Navigationsanforderung beziehen. Der folgende Screenshot zeigt beispielsweise die Eigenschaften `CurrentItem` und `CurrentState` des `Shell.Current`-Objekts an:

![Screenshot des Debuggers](navigation-images/debugger.png "Debugger")

In diesem Beispiel zeigt die `CurrentItem`-Eigenschaft vom Typ `FlyoutItem` den Titel und die Route des `FlyoutItem`-Objekts an. Entsprechend zeigt die `CurrentState`-Eigenschaft vom Typ `ShellNavigationState` den URI der angezeigten Route in der Shell-Anwendung an.

### <a name="tab-class"></a>Tab-Klasse

Die `Tab`-Klasse definiert eine `Stack`-Eigenschaft vom Typ `IReadOnlyList<Page>`, die für den aktuellen mithilfe von Push übertragenen Navigationsstapel innerhalb der `Tab` steht. Die Klasse stellt außerdem die folgenden überschreibbaren Navigationsmethoden zur Verfügung:

- `GetNavigationStack`: Gibt `IReadOnlyList<Page`> zurück, der aktuelle Navigationsstapel.
- `OnInsertPageBefore`: Wird aufgerufen, wenn `INavigation.InsertPageBefore` aufgerufen wird.
- `OnPopAsync`: Gibt `Task<Page>` zurück, und wird aufgerufen, wenn `INavigation.PopAsync` aufgerufen wird.
- `OnPopToRootAsync`: Gibt `Task` zurück, und wird aufgerufen, wenn `INavigation.OnPopToRootAsync` aufgerufen wird.
- `OnPushAsync`: Gibt `Task` zurück, und wird aufgerufen, wenn `INavigation.PushAsync` aufgerufen wird.
- `OnRemovePage`: Wird aufgerufen, wenn `INavigation.RemovePage` aufgerufen wird.

## <a name="navigation-events"></a>Navigationsereignisse

Die `Shell`-Klasse definiert ein `Navigating`-Ereignis, das ausgelöst wird, kurz bevor die Navigation entweder aufgrund von programmgesteuerter Navigation oder Benutzerinteraktion erfolgt. Das `ShellNavigatingEventArgs`-Objekt, das das `Navigating`-Ereignis begleitet, stellt die folgenden Eigenschaften bereit:

| Eigenschaft | Typ | Beschreibung |
|---|---|---|
| `Current` | `ShellNavigationState` | Der URI der aktuellen Seite. |
| `Source` | `ShellNavigationSource` | Der Typ der erfolgten Navigation. |
| `Target` | `ShellNavigationState`  | Der URI, der darstellt, wohin die Navigation führt. |
| `CanCancel`  | `bool` | Ein Wert, der angibt, ob ein Abbruch der Navigation möglich ist. |
| `Cancelled`  | `bool` | Ein Wert, der angibt, ob die Navigation abgebrochen wurde. |

Darüber hinaus stellt die `ShellNavigatingEventArgs`-Klasse eine `Cancel`-Methode bereit, die zum Abbrechen der Navigation verwendet werden kann.

> [!NOTE]
> Das `Navigated`-Ereignis wird durch die überschreibbare `OnNavigating`-Methode in der `Shell`-Klasse ausgelöst.

Die `Shell`-Klasse definiert außerdem ein `Navigated`-Ereignis, das ausgelöst wird, wenn die Navigation abgeschlossen ist. Das `ShellNavigatedEventArgs`-Objekt, das das `Navigating`-Ereignis begleitet, stellt die folgenden Eigenschaften bereit:

| Eigenschaft | Typ | Beschreibung |
|---|---|---|
| `Current` | `ShellNavigationState` | Der URI der aktuellen Seite. |
| `Previous`| `ShellNavigationState` | Der URI der vorherigen Seite. |
| `Source`  | `ShellNavigationSource` | Der Typ der erfolgten Navigation. |

> [!NOTE]
> Das `Navigating`-Ereignis wird durch die überschreibbare `OnNavigated`-Methode in der `Shell`-Klasse ausgelöst.

Die `ShellNavigatedEventArgs`-Klasse und die `ShellNavigatingEventArgs`-Klasse verfügen beide über `Source`-Eigenschaften vom Typ `ShellNavigationSource`. Diese Enumeration stellt die folgenden Werte bereit:

- `Unknown`
- `Push`
- `Pop`
- `PopToRoot`
- `Insert`
- `Remove`
- `ShellItemChanged`
- `ShellSectionChanged`
- `ShellContentChanged`

Aus diesem Grund ist es in einem Handler für das `Navigating`-Ereignis möglich, die Navigation abzufangen und Aktionen basierend auf der Navigationsquelle auszuführen. Der folgende Code zeigt z.B., wie die Rückwärtsnavigation abgebrochen wird, wenn die Daten auf der Seite nicht gespeichert wurden:

```csharp
void OnNavigating(object sender, ShellNavigatingEventArgs e)
{
    // Cancel back navigation if data is unsaved
    if (e.Source == ShellNavigationSource.Pop && !dataSaved)
    {
        e.Cancel();
    }
}
```

## <a name="pass-data"></a>Übergeben von Daten

Daten können als Abfrageparameter übergeben werden, wenn programmgesteuerte Navigation ausgeführt wird. Der folgende Code wird beispielsweise in der Beispielanwendung ausgeführt, wenn ein Benutzer einen Elefanten auf der `ElephantsPage` auswählt:

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    await Shell.Current.GoToAsync($"//animals/elephants/elephantdetails?name={elephantName}");
}
```

Dieses Codebeispiel ruft den aktuell ausgewählten Elefanten in der `elephantdetails` ab, und navigiert zur [`CollectionView`](xref:Xamarin.Forms.CollectionView)-Route, wobei `elephantName` als Abfrageparameter übergeben wird. Beachten Sie, dass die Abfrageparameter für die Navigation URL-codiert sind, sodass „Indischer Elefant“ zu „Indischer%20Elefant“ wird.

Um Daten abzurufen, muss die Klasse, die Seite, zu der navigiert wird, darstellt oder die Klasse für den [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite um ein `QueryPropertyAttribute` für jeden Suchparameter ergänzt werden:

```csharp
[QueryProperty("Name", "name")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            BindingContext = ElephantData.Elephants.FirstOrDefault(m => m.Name == Uri.UnescapeDataString(value));
        }
    }
    ...
}
```

Das erste Argument für das `QueryPropertyAttribute` gibt den Namen der Eigenschaft, die die Daten empfangen soll, und das zweite Argument die ID des Abfrageparameters an. Das `QueryPropertyAttribute` im oben stehenden Beispiel legt somit fest, dass die `Name`-Eigenschaft die im `name`-Abfrageparameter übergebenen Daten vom URI im `GoToAsync`-Methodenaufruf erhält. Die `Name`-Eigenschaft decodiert dann den Wert des Abfrageparameters und legt damit den [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der Seite auf das Objekt fest, das angezeigt wird.

> [!NOTE]
> Eine Klasse kann mit mehreren `QueryPropertyAttribute`-Objekten ergänzt werden.

## <a name="back-button-behavior"></a>Verhalten der Zurück-Schaltfläche

Die `BackButtonBehavior`-Klasse definiert die folgenden Eigenschaften, die die Darstellung und das Verhalten der Zurück-Schaltfläche steuern:

- `Command` vom Typ `ICommand`: Wird ausgeführt, wenn die Zurück-Schaltfläche gedrückt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.
- `IconOverride` vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource): das Symbol für die Zurück-Schaltfläche.
- `IsEnabled` vom Typ `boolean`: Gibt an, ob die Zurück-Schaltfläche aktiviert ist. Der Standardwert ist `true`.
- `TextOverride` vom Typ `string`: der Text für die Zurück-Schaltfläche.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

Die `BackButtonBehavior`-Klasse kann durch Festlegen der angefügten Eigenschaft `Shell.BackButtonBehavior` eines `BackButtonBehavior` Objekts verarbeitet werden:

```xaml
<ContentPage ...>    
    <Shell.BackButtonBehavior>
        <BackButtonBehavior Command="{Binding BackCommand}"
                            IconOverride="back.png" />   
    </Shell.BackButtonBehavior>
    ...
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
Shell.SetBackButtonBehavior(this, new BackButtonBehavior
{
    Command = new Command(() =>
    {
        ...
    }),
    IconOverride = "back.png"
});
```

Die `Command`-Eigenschaft wird auf einen `ICommand` festgelegt, der ausgeführt wird, wenn die Zurück-Taste gedrückt wird, und die `IconOverride`-Eigenschaft wird auf das Symbol festgelegt, das für die Zurück-Schaltfläche verwendet wird:

[![Screenshot der Überschreibung des Symbols für die Zurück-Schaltfläche der Shell unter iOS und Android](navigation-images/back-button.png "Überschreibung des Symbols für die Zurück-Schaltfläche der Shell")](navigation-images/back-button-large.png#lightbox "Überschreibung des Symbols für die Zurück-Schaltfläche der Shell")

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
