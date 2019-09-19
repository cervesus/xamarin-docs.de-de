---
title: Anpassen einer ViewCell
description: Eine Xamarin.Forms-ViewCell ist eine Zelle, die einer ListView oder TableView hinzugefügt werden kann und die eine vom Entwickler definierte Ansicht enthält. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für eine ViewCell erstellen können, die in einem Xamarin.Forms-ListView-Steuerelement gehostet wird.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 5cd0a1ec43f0e56ec1ec72ebd614a7e0a5fa2225
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998047"
---
# <a name="customizing-a-viewcell"></a>Anpassen einer ViewCell

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)

_Eine Xamarin.Forms-ViewCell ist eine Zelle, die einer ListView oder TableView hinzugefügt werden kann und die eine vom Entwickler definierte Ansicht enthält. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für eine ViewCell erstellen können, die in einem Xamarin.Forms-ListView-Steuerelement gehostet wird. Das verhindert, dass die Xamarin.Forms-Layoutberechnungen wiederholt beim Scrollen in ListView aufgerufen werden._

Jede Xamarin.Forms-Zelle verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Objekts durch eine Xamarin.Forms-App wird in iOS die `ViewCellRenderer`-Klasse instanziiert, wodurch wiederum ein natives `UITableViewCell`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `ViewCellRenderer`-Klasse ein natives `View`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `ViewCellRenderer`-Klasse eine native `DataTemplate`-Klasse. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Renderer Base Classes and Native Controls (Rendererbasisklassen und native Steuerelemente)](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Objekt und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](viewcell-images/viewcell-classes.png "Beziehungen zwischen dem ViewCell-Steuerelement und den nativen Steuerelementen, die dieses implementieren")

Der Renderprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für eine [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Custom_Cell) Sie eine benutzerdefinierte Xamarin.Forms-Zelle.
1. [Nutzen](#Consuming_the_Custom_Cell) Sie die benutzerdefinierte Xamarin.Forms-Zelle.
1. [Erstellen](#Creating_the_Custom_Renderer_on_each_Platform) Sie den benutzerdefinierten Renderer für die Zelle auf jeder Plattform.

Jedes dieser Elemente wird nachfolgend noch genauer für die Implementierung eines `NativeCell`-Renderers erläutert, der ein plattformspezifisches Layout für jede Zelle verwendet, die in einem Xamarin.Forms-[`ListView`](xref:Xamarin.Forms.ListView)-Steuerelement gehostet wird. Das verhindert, dass die Xamarin.Forms-Layoutberechnungen wiederholt beim Scrollen in `ListView` aufgerufen werden.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Erstellen der benutzerdefinierten Zelle

Eine benutzerdefinierte Zelle kann erstellt werden, indem Sie die [`ViewCell`](xref:Xamarin.Forms.ViewCell)-Klasse wie in folgendem Codebeispiel als Unterklasse verwenden:

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```

Die Klasse `NativeCell` wird im .NET Standard-Bibliotheksprojekt erstellt und definiert die API für die benutzerdefinierte Zelle. Die benutzerdefinierte Zelle macht die Eigenschaften `Name`, `Category` und `ImageFilename` verfügbar, die über eine Datenbindung angezeigt werden können. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Nutzen der benutzerdefinierten Zelle

Sie können auf die benutzerdefinierte Zelle `NativeCell` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das benutzerdefinierte Zellenelement verwenden. Das folgende Codebeispiel veranschaulicht, wie die benutzerdefinierte Zelle `NativeCell` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <StackLayout>
              <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
              <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                  <ListView.ItemTemplate>
                      <DataTemplate>
                          <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                      </DataTemplate>
                  </ListView.ItemTemplate>
              </ListView>
          </StackLayout>
      </ContentPage.Content>
</ContentPage>
```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf die benutzerdefinierte Zelle zu verweisen.

Das folgende Codebeispiel veranschaulicht, wie die benutzerdefinierte Zelle `NativeCell` von einer C#-Seite genutzt werden kann:

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Ein [`ListView`](xref:Xamarin.Forms.ListView)-Steuerelement von Xamarin.Forms wird zum Anzeigen einer Liste der Daten verwendet, die über die [`ItemSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)-Eigenschaft aufgefüllt wird. Die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)-Zwischenspeicherstrategie versucht, den `ListView`-Speicherbedarf sowie die Ausführungsgeschwindigkeit durch die Wiederverwendung von Listenzellen zu minimieren. Weitere Informationen finden Sie unter [Zwischenspeicherstrategie](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

Jede Zeile in der Liste enthält drei Datenelemente: einen Namen, eine Kategorie und einen Dateinamen für ein Bild. Das Layout jeder Zeile in der Liste wird durch eine `DataTemplate` definiert, auf die über die bindbare [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)-Eigenschaft verwiesen wird. Die `DataTemplate` definiert, dass jede Datenzeile in der Liste eine `NativeCell` sein wird, die die Eigenschaften `Name`, `Category` und `ImageFilename` über eine Datenbindung darstellt. Weitere Informationen zum `ListView`-Steuerelement finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

Ein benutzerdefinierter Renderer kann nun zu jedem Anwendungsprojekt hinzugefügt werden, um das plattformspezifische Layout für jede Zelle anzupassen.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `ViewCellRenderer`-Klasse, die die benutzerdefinierte Zelle rendert.
1. Überschreiben Sie die plattformspezifische Methode, die die benutzerdefinierte Zelle rendert, und schreiben Sie Logik, um dieses anzupassen.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern der benutzerdefinierten Xamarin.Forms-Zelle verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Bei den meisten Xamarin.Forms-Elementen ist das Angeben eines benutzerdefinierten Renderers in jedem Plattformprojekt optional. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet. Benutzerdefinierte Renderer sind jedoch in jedem Plattformprojekt erforderlich, wenn ein [ViewCell](xref:Xamarin.Forms.ViewCell)-Element gerendert wird.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](viewcell-images/solution-structure.png "Projektzuständigkeiten beim benutzerdefinierten NativeCell-Renderer")

Die benutzerdefinierte Zelle `NativeCell` wird von plattformspezifischen Rendererklassen gerendert, die alle von der `ViewCellRenderer`-Klasse für jede Plattform abgeleitet werden. Das führt dazu, dass jede benutzerdefinierte `NativeCell`-Zelle mit dem plattformspezifischen Layout gerendert wird. Dies wird in folgenden Screenshots veranschaulicht:

![](viewcell-images/screenshots.png "NativeCell auf jeder Plattform")

Die `ViewCellRenderer`-Klasse macht plattformspezifische Methoden zum Rendern der benutzerdefinierten Zelle verfügbar. Auf der iOS-Plattform ist dies die `GetCell`-Methode, auf der Android-Plattform die `GetCellCore`-Methode und auf der UWP die `GetTemplate`-Methode.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen der zu rendernden Xamarin.Forms-Zelle und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird die Implementierung jeder plattformspezifischen, benutzerdefinierten Rendererklasse erläutert.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

Die `GetCell`-Methode wird aufgerufen, sodass jede anzuzeigende Zelle erstellt wird. Jede Zelle ist eine `NativeiOSCell`-Instanz, die das Layout der Zelle und deren Daten definiert. Der Vorgang der `GetCell`-Methode ist von der [`ListView`](xref:Xamarin.Forms.ListView)-Strategie für die Zwischenspeicherung abhängig.

- Wenn die [`ListView`](xref:Xamarin.Forms.ListView)-Strategie zur Zwischenspeicherung [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) lautet, wird die `GetCell`-Methode für jede Zelle aufgerufen. Für jede `NativeCell`-Instanz, die zunächst auf dem Bildschirm angezeigt wird, wird eine `NativeiOSCell`-Instanz erstellt. Wenn der Benutzer durch die `ListView` scrollt, werden `NativeiOSCell`-Instanzen erneut verwendet. Weitere Informationen zum Wiederverwenden von Zellen in iOS finden Sie unter [Auffüllen einer Tabelle mit Daten](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Dieser benutzerdefinierte Renderercode führt einige Vorgänge zum Wiederverwenden der Zellen durch, auch wenn [`ListView`](xref:Xamarin.Forms.ListView) zum Beibehalten der Zellen festgelegt wurde.

  Die von jeder `NativeiOSCell`-Instanz dargestellten Daten – egal, ob es sich um neu erstellte oder wiederverwendete Daten handelt – werden mit den Daten aus jeder `NativeCell`-Instanz mithilfe der `UpdateCell`-Methode aktualisiert.

  > [!NOTE]
  > Die `OnNativeCellPropertyChanged`-Methode wird nie aufgerufen, wenn die [`ListView`](xref:Xamarin.Forms.ListView)-Strategie zur Zwischenspeicherung zum Beibehalten von Zellen festgelegt ist.

- Wenn die [`ListView`](xref:Xamarin.Forms.ListView)-Strategie zur Zwischenspeicherung [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) lautet, wird die `GetCell`-Methode für jede Zelle aufgerufen, die ursprünglich auf dem Bildschirm angezeigt wurde. Für jede `NativeCell`-Instanz, die zunächst auf dem Bildschirm angezeigt wird, wird eine `NativeiOSCell`-Instanz erstellt. Die von jeder `NativeiOSCell`-Instanz dargestellten Daten werden mit den Daten aus der `NativeCell`-Instanz mithilfe der `UpdateCell`-Methode aktualisiert. Die `GetCell`-Methode wird allerdings nicht aufgerufen, wenn der Benutzer durch `ListView` scrollt. Stattdessen werden die `NativeiOSCell`-Instanzen wiederverwendet. `PropertyChanged`-Ereignisse werden für die `NativeCell`-Instanz ausgelöst, wenn sich deren Daten ändern, und der `OnNativeCellPropertyChanged`-Ereignishandler aktualisiert die Daten in jeder wiederverwendeten `NativeiOSCell`-Instanz.

Im folgenden Codebeispiel ist die `OnNativeCellPropertyChanged`-Methode dargestellt, die aufgerufen wird, wenn ein `PropertyChanged`-Ereignis ausgelöst wird:

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Mit dieser Methode werden die Daten aktualisiert, die durch wiederverwendete `NativeiOSCell`-Instanzen dargestellt werden. Die Eigenschaft, die geändert wird, wird überprüft, da die Methode mehrere Male aufgerufen werden kann.

Die `NativeiOSCell`-Klasse definiert das Layout für jede Zelle und ist im folgenden Codebeispiel dargestellt:

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Diese Klasse definiert die Steuerelemente, die zum Rendern der Inhalte der Zellen sowie deren Layout verwendet werden. Die Klasse implementiert die Schnittstelle [`INativeElementView`](xref:Xamarin.Forms.INativeElementView), die benötigt wird, wenn die [`ListView`](xref:Xamarin.Forms.ListView) die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)-Strategie zur Zwischenspeicherung verwendet. Diese Schnittstelle gibt an, dass die Klasse die Eigenschaft [`Element`](xref:Xamarin.Forms.INativeElementView.Element) implementieren muss. Diese wiederum muss die benutzerdefinierten Zellendaten für wiederverwendete Zellen zurückgeben.

Der `NativeiOSCell`-Konstruktor initialisiert die Darstellung der Eigenschaften `HeadingLabel`, `SubheadingLabel` und `CellImageView`. Diese Eigenschaften werden zum Anzeigen der in der `NativeCell`-Instanz gespeicherten Daten verwendet. Die `UpdateCell`-Methode wird aufgerufen, um den Wert jeder Eigenschaft festzulegen. Wenn die [`ListView`](xref:Xamarin.Forms.ListView) die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)-Strategie zur Zwischenspeicherung verwendet, können darüber hinaus die Daten, die von den Eigenschaften `HeadingLabel`, `SubheadingLabel` und `CellImageView` dargestellt werden, von der `OnNativeCellPropertyChanged`-Methode im benutzerdefinierten Renderer aktualisiert werden.

Das Zellenlayout erfolgt durch die `LayoutSubviews`-Überschreibung, die die Koordinaten von `HeadingLabel`, `SubheadingLabel` und `CellImageView` innerhalb der Zelle festlegt.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

Die `GetCellCore`-Methode wird aufgerufen, sodass jede anzuzeigende Zelle erstellt wird. Jede Zelle ist eine `NativeAndroidCell`-Instanz, die das Layout der Zelle und deren Daten definiert. Der Vorgang der `GetCellCore`-Methode ist von der [`ListView`](xref:Xamarin.Forms.ListView)-Strategie für die Zwischenspeicherung abhängig.

- Wenn die [`ListView`](xref:Xamarin.Forms.ListView)-Strategie zur Zwischenspeicherung [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) lautet, wird die `GetCellCore`-Methode für jede Zelle aufgerufen. Für jede `NativeCell`-Instanz, die zunächst auf dem Bildschirm angezeigt wird, wird eine `NativeAndroidCell` erstellt. Wenn der Benutzer durch die `ListView` scrollt, werden `NativeAndroidCell`-Instanzen erneut verwendet. Weitere Informationen zur Wiederverwendung der Android-Zelle finden Sie unter [Auffüllen von ListView mit Daten](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Beachten Sie, dass dieser benutzerdefinierte Renderercode einige Vorgänge zum Wiederverwenden der Zellen durchführt, auch wenn [`ListView`](xref:Xamarin.Forms.ListView) zum Beibehalten der Zellen festgelegt wurde.

  Die von jeder `NativeAndroidCell`-Instanz dargestellten Daten – egal, ob es sich um neu erstellte oder wiederverwendete Daten handelt – werden mit den Daten aus jeder `NativeCell`-Instanz mithilfe der `UpdateCell`-Methode aktualisiert.

  > [!NOTE]
  > Obwohl die `OnNativeCellPropertyChanged`-Methode aufgerufen wird, wenn die [`ListView`](xref:Xamarin.Forms.ListView) zum Beibehalten von Zellen festgelegt wurde, werden darüber keine `NativeAndroidCell`-Eigenschaftswerte aktualisiert.

- Wenn die [`ListView`](xref:Xamarin.Forms.ListView)-Strategie zur Zwischenspeicherung [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) lautet, wird die `GetCellCore`-Methode für jede Zelle aufgerufen, die ursprünglich auf dem Bildschirm angezeigt wurde. Für jede `NativeCell`-Instanz, die zunächst auf dem Bildschirm angezeigt wird, wird eine `NativeAndroidCell`-Instanz erstellt. Die von jeder `NativeAndroidCell`-Instanz dargestellten Daten werden mit den Daten aus der `NativeCell`-Instanz mithilfe der `UpdateCell`-Methode aktualisiert. Die `GetCellCore`-Methode wird allerdings nicht aufgerufen, wenn der Benutzer durch `ListView` scrollt. Stattdessen werden die `NativeAndroidCell`-Instanzen wiederverwendet.  `PropertyChanged`-Ereignisse werden für die `NativeCell`-Instanz ausgelöst, wenn sich deren Daten ändern, und der `OnNativeCellPropertyChanged`-Ereignishandler aktualisiert die Daten in jeder wiederverwendeten `NativeAndroidCell`-Instanz.

Im folgenden Codebeispiel ist die `OnNativeCellPropertyChanged`-Methode dargestellt, die aufgerufen wird, wenn ein `PropertyChanged`-Ereignis ausgelöst wird:

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Mit dieser Methode werden die Daten aktualisiert, die durch wiederverwendete `NativeAndroidCell`-Instanzen dargestellt werden. Die Eigenschaft, die geändert wird, wird überprüft, da die Methode mehrere Male aufgerufen werden kann.

Die `NativeAndroidCell`-Klasse definiert das Layout für jede Zelle und ist im folgenden Codebeispiel dargestellt:

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Diese Klasse definiert die Steuerelemente, die zum Rendern der Inhalte der Zellen sowie deren Layout verwendet werden. Die Klasse implementiert die Schnittstelle [`INativeElementView`](xref:Xamarin.Forms.INativeElementView), die benötigt wird, wenn die [`ListView`](xref:Xamarin.Forms.ListView) die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)-Strategie zur Zwischenspeicherung verwendet. Diese Schnittstelle gibt an, dass die Klasse die Eigenschaft [`Element`](xref:Xamarin.Forms.INativeElementView.Element) implementieren muss. Diese wiederum muss die benutzerdefinierten Zellendaten für wiederverwendete Zellen zurückgeben.

Der `NativeAndroidCell`Konstruktor vergrößert das `NativeAndroidCell`-Layout und initialisiert die Eigenschaften `HeadingTextView`, `SubheadingTextView` und `ImageView` der Steuerelemente im vergrößerten Layout. Diese Eigenschaften werden zum Anzeigen der in der `NativeCell`-Instanz gespeicherten Daten verwendet. Die `UpdateCell`-Methode wird aufgerufen, um den Wert jeder Eigenschaft festzulegen. Wenn die [`ListView`](xref:Xamarin.Forms.ListView) die [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)-Strategie zur Zwischenspeicherung verwendet, können darüber hinaus die Daten, die von den Eigenschaften `HeadingTextView`, `SubheadingTextView` und `ImageView` dargestellt werden, von der `OnNativeCellPropertyChanged`-Methode im benutzerdefinierten Renderer aktualisiert werden.

Im folgenden Codebeispiel ist die Layoutdefinition für die Layoutdatei `NativeAndroidCell.axml` dargestellt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Dieses Layout gibt an, dass zwei `TextView`-Steuerelemente sowie ein `ImageView`-Steuerelement zum Anzeigen des Zelleninhalts verwendet werden. Die zwei `TextView`-Steuerelemente sind innerhalb eines `LinearLayout`-Steuerelements vertikal ausgerichtet, wobei alle Steuerelemente innerhalb einer `RelativeLayout` enthalten sind.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen des benutzerdefinierten Renderers auf der UWP

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die UWP veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

Die `GetTemplate`-Methode wird aufgerufen, um die Zelle zurückzugeben, die für jede Datenzeile in der Liste gerendert werden soll. Für jede `NativeCell`-Instanz wird eine `DataTemplate` erstellt, die auf dem Bildschirm angezeigt wird, und die `DataTemplate` definiert die Darstellung sowie die Inhalte der Zelle.

Die `DataTemplate` wird im Ressourcenverzeichnis auf Anwendungsebene gespeichert und ist im folgenden Codebeispiel dargestellt:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

Die `DataTemplate` gibt die zum Anzeigen der Zelleninhalte verwendeten Steuerelemente sowie deren Layout und Darstellung an. Zwei `TextBlock`-Steuerelemente sowie ein `Image`-Steuerelement werden zum Anzeigen des Zelleninhalts über die Datenbindung verwendet. Zusätzlich wird eine Instanz von `ConcatImageExtensionConverter` verwendet, um die Dateierweiterung `.jpg` an jeden Bilddateinamen zu ketten. So wird sichergestellt, dass das Steuerelement `Image` das Bild laden und rendern kann, wenn dessen `Source`-Eigenschaft festgelegt ist.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen benutzerdefinierten Renderer für eine [`ViewCell`](xref:Xamarin.Forms.ViewCell) erstellen können, die in einem Xamarin.Forms-[`ListView`](xref:Xamarin.Forms.ListView)-Steuerelement gehostet wird. Das verhindert, dass die Xamarin.Forms-Layoutberechnungen wiederholt beim Scrollen in `ListView` aufgerufen werden.

## <a name="related-links"></a>Verwandte Links

- [ListView-Leistung](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
