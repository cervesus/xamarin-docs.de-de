---
title: Anpassen einer ViewCell
description: Einer Xamarin.Forms ViewCell ist eine Zelle, die hinzugefügt werden kann, zu einer ListView oder TableView, die eine Entwickler benutzerdefinierte Sicht enthält. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für einer ViewCell erstellt, die in einem Xamarin.Forms-ListView-Steuerelement gehostet wird.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 3cb4d7f152e0f9540275f12f0ade568cd0552784
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935575"
---
# <a name="customizing-a-viewcell"></a>Anpassen einer ViewCell

_Einer Xamarin.Forms ViewCell ist eine Zelle, die hinzugefügt werden kann, zu einer ListView oder TableView, die eine Entwickler benutzerdefinierte Sicht enthält. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für einer ViewCell erstellt, die in einem Xamarin.Forms-ListView-Steuerelement gehostet wird. Dies beendet die Xamarin.Forms-layoutberechnungen wird wiederholt aufgerufen, während des Bildlaufs ListView._

Alle Xamarin.Forms-Zellen verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) gerendert wird, von einer Xamarin.Forms-Anwendung unter iOS die `ViewCellRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `UITableViewCell` Steuerelement. Auf der Android-Plattform die `ViewCellRenderer` Klasse instanziiert ein systemeigenes `View` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `ViewCellRenderer` Klasse instanziiert ein systemeigenes `DataTemplate`. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) und die entsprechenden systemeigenen Steuerelemente, die sie implementieren:

![](viewcell-images/viewcell-classes.png "Beziehung zwischen dem ViewCell-Steuerelement und die implementierenden Native Steuerelemente")

Das Rendern zu kann nutzen erstellt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Cell) einer benutzerdefinierten Xamarin.Forms-Zelle.
1. [Nutzen](#Consuming_the_Custom_Cell) die benutzerdefinierte Zelle von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierte Renderer für die Zelle auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer `NativeCell` Renderer, der ein plattformspezifisches Layout für jede Zelle in einer Xamarin.Forms gehostet nutzt [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement. Dadurch wird wiederholt aufgerufen wird, während die Xamarin.Forms-layoutberechnungen beendet `ListView` scrollen.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Erstellen die benutzerdefinierte Zelle

Ein Steuerelement für die benutzerdefinierte Zelle erstellt werden, indem Unterklassen der [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) Klasse, wie im folgenden Codebeispiel gezeigt:

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
Die `NativeCell` -Klasse in .NET Standard Library-Projekt erstellt und definiert die API für die benutzerdefinierte Zelle. Stellt die benutzerdefinierte Zelle `Name`, `Category`, und `ImageFilename` Eigenschaften, die mithilfe von Datenbindung angezeigt werden können. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Nutzen die benutzerdefinierte Zelle

Die `NativeCell` benutzerdefinierte Zelle kann verwiesen werden in Xaml in .NET Standard Library-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix für die benutzerdefinierte Cell-Element. Das folgende Codebeispiel zeigt die `NativeCell` benutzerdefinierte Zelle kann von einer XAML-Seite verwendet werden:

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

Die `local` Namespacepräfix kann eine beliebige Bezeichnung. Allerdings die `clr-namespace` und `assembly` -Werte müssen die Details des benutzerdefinierten Steuerelements entsprechen. Sobald der Namespace deklariert wird, wird das Präfix verwendet, auf die benutzerdefinierte Zelle verweisen.

Das folgende Codebeispiel zeigt die `NativeCell` benutzerdefinierte Zelle durch einen C# -Code genutzt werden kann:

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

Eine Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) Steuerelement wird verwendet, um eine Liste der Daten, anzeigen, die über aufgefüllt wird die [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) Eigenschaft. Die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie versucht, die Minimierung der `ListView` Speicherbedarf und die Ausführung zu beschleunigen, durch Wiederverwenden von Listenzellen. Weitere Informationen finden Sie unter [Strategie für die Zwischenspeicherung](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Jede Zeile in der Liste enthält drei Datenelemente: einen Namen, eine Kategorie und ein Dateiname des Bilds. Das Layout der einzelnen Zeilen in der Liste wird definiert, durch die `DataTemplate` auf den verwiesen wird durch die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) bindbare Eigenschaft. Die `DataTemplate` definiert, dass jede Zeile der Daten in der Liste werden eine `NativeCell` , anzeigt seine `Name`, `Category`, und `ImageFilename` Eigenschaften mithilfe der Datenbindung. Weitere Informationen zu den `ListView` steuern, finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

Ein benutzerdefinierter Renderer kann jetzt jedem Projekt der Anwendung zum Anpassen des Layouts plattformspezifische für jede Zelle hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `ViewCellRenderer` -Klasse, die die benutzerdefinierte Zelle gerendert wird.
1. Überschreiben Sie die plattformspezifischen-Methode, die die benutzerdefinierte Zelle rendert und Schreiben Sie Logik, um es anzupassen.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass er zum Rendern der benutzerdefinierten Xamarin.Forms-Zelle verwendet wird. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms-Elemente ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse des Steuerelements verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in dem jedes plattformprojekt beim Rendern einer [ViewCell](xref:Xamarin.Forms.ViewCell) Element.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](viewcell-images/solution-structure.png "Zuständigkeiten von NativeCell benutzerdefinierten Renderer-Projekt")

Die `NativeCell` benutzerdefinierte Zelle gerendert wird, durch das plattformspezifische, alle abgeleiteten Renderingklassen der `ViewCellRenderer` -Klasse für jede Plattform. Dies führt in jeder `NativeCell` benutzerdefinierte Zelle, die mit Layout mit Ausrichtung von plattformspezifischen gerendert wird, wie in den folgenden Screenshots gezeigt:

![](viewcell-images/screenshots.png "NativeCell auf jeder Plattform")

Die `ViewCellRenderer` Klasse verfügbar macht, plattformspezifische Methoden für die benutzerdefinierte Zelle zu rendern. Dies ist die `GetCell` Methode für die iOS-Plattform, die `GetCellCore` Methode für die Android-Plattform und die `GetTemplate` Methode für die UWP.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: der Typname der Xamarin.Forms-Zelle gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Den folgenden Abschnitten werden die Implementierung der einzelnen plattformspezifischen benutzerdefinierten Renderer-Klasse.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die iOS-Plattform:

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

Die `GetCell` aufgerufen, um jede Zelle anzuzeigende zu erstellen. Jede Zelle ist eine `NativeiOSCell` -Instanz, die das Layout der Zelle und die Daten definiert. Der Vorgang von der `GetCell` Methode richtet sich nach der [ `ListView` ](xref:Xamarin.Forms.ListView) cachingstrategie:

- Wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) cachingstrategie ist [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCell` Methode wird für jede Zelle aufgerufen werden. Ein `NativeiOSCell` Instanz erstellt werden soll, für die einzelnen `NativeCell` -Instanz, die auf dem Bildschirm angezeigt wird. Wenn der Benutzer einen Bildlauf der `ListView`, `NativeiOSCell` Instanzen werden erneut verwendet werden. Weitere Informationen zu iOS Zelle erneut verwenden, finden Sie unter [Zelle wiederverwenden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Dieser benutzerdefinierten Renderer-Code führt einige Zelle erneut verwenden, auch wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) Zellen beibehalten festgelegt ist.

  Die angezeigten Daten durch jede `NativeiOSCell` -Instanz, ob neu erstellt oder erneut verwendet wird, wird aktualisiert mit den Daten aus jedem `NativeCell` Instanz, indem die `UpdateCell` Methode.

  > [!NOTE]
  > Die `OnNativeCellPropertyChanged` Methode werden nie aufgerufen wird, wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) cachingstrategie Zellen beibehalten festgelegt ist.

- Wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) cachingstrategie ist [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCell` Methode wird aufgerufen werden, für jede Zelle, die auf dem Bildschirm angezeigt wird. Ein `NativeiOSCell` Instanz erstellt werden soll, für die einzelnen `NativeCell` -Instanz, die auf dem Bildschirm angezeigt wird. Die angezeigten Daten durch jede `NativeiOSCell` Instanz wird aktualisiert mit den Daten aus der `NativeCell` Instanz die `UpdateCell` Methode. Allerdings die `GetCell` Methode wird nicht aufgerufen werden, als der Benutzer einen Bildlauf durch die `ListView`. Stattdessen die `NativeiOSCell` Instanzen werden erneut verwendet werden. `PropertyChanged` Ereignisse werden ausgelöst werden, auf die `NativeCell` Instanz, wenn Sie die Daten geändert werden, und die `OnNativeCellPropertyChanged` Ereignishandler aktualisiert die Daten in den einzelnen wiederverwendet `NativeiOSCell` Instanz.

Das folgende Codebeispiel zeigt die `OnNativeCellPropertyChanged` -Methode, die beim aufgerufen hat eine `PropertyChanged` Ereignis wird ausgelöst:

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

Aktualisiert die Daten, die angezeigt wird, indem Sie für diese Methode erneut verwendet `NativeiOSCell` Instanzen. Überprüft, ob die Eigenschaft, die geändert wird. wird vorgenommen, wie die-Methode mehrere Male aufgerufen werden kann.

Die `NativeiOSCell` Klasse definiert das Layout für jede Zelle, und wird im folgenden Codebeispiel dargestellt:

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

Diese Klasse definiert die Steuerelemente verwendet, um den Inhalt der Zelle, und ihr Layout rendern. Die Klasse implementiert die [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) -Schnittstelle, die erforderlich, wenn ist die [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie. Diese Schnittstelle gibt an, dass die Klasse implementieren muss, die [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) -Eigenschaft, die die benutzerdefinierte Zellendaten wiederverwendet Zellen zurückgeben soll.

Die `NativeiOSCell` Konstruktor initialisiert die Darstellung der `HeadingLabel`, `SubheadingLabel`, und `CellImageView` Eigenschaften. Diese Eigenschaften werden verwendet, um die Anzeige der Daten in die `NativeCell` -Instanz mit der `UpdateCell` Methode aufgerufen wird, um den Wert jeder Eigenschaft festzulegen. Darüber hinaus, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie, die angezeigten Daten durch die `HeadingLabel`, `SubheadingLabel`, und `CellImageView` Eigenschaften können sein aktualisiert die `OnNativeCellPropertyChanged` -Methode in der der benutzerdefinierte Renderer.

Zellenlayout erfolgt durch die `LayoutSubviews` zu überschreiben, wodurch die Koordinaten der `HeadingLabel`, `SubheadingLabel`, und `CellImageView` in der Zelle.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die Android-Plattform:

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

Die `GetCellCore` aufgerufen, um jede Zelle anzuzeigende zu erstellen. Jede Zelle ist eine `NativeAndroidCell` -Instanz, die das Layout der Zelle und die Daten definiert. Der Vorgang von der `GetCellCore` Methode richtet sich nach der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie:

- Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie ist [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCellCore` Methode wird für jede Zelle aufgerufen werden. Ein `NativeAndroidCell` erstellt werden, für die einzelnen `NativeCell` -Instanz, die auf dem Bildschirm angezeigt wird. Wenn der Benutzer einen Bildlauf der `ListView`, `NativeAndroidCell` Instanzen werden erneut verwendet werden. Weitere Informationen zu Android Zelle erneut zu verwenden, finden Sie unter [Zeile View RE-use](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Beachten Sie, dass dieser benutzerdefinierten Renderer-Code einige Zelle erneut verwenden führt, auch wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Zellen beibehalten festgelegt ist.

  Die angezeigten Daten durch jede `NativeAndroidCell` -Instanz, ob neu erstellt oder erneut verwendet wird, wird aktualisiert mit den Daten aus jedem `NativeCell` Instanz, indem die `UpdateCell` Methode.

  > [!NOTE]
  > Beachten Sie, dass die `OnNativeCellPropertyChanged` Methode wird aufgerufen, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ist festgelegt Zellen beibehalten, es wird nicht aktualisiert die `NativeAndroidCell` Eigenschaftswerte.

- Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie ist [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCellCore` Methode wird aufgerufen werden, für jede Zelle, die auf dem Bildschirm angezeigt wird. Ein `NativeAndroidCell` Instanz erstellt werden soll, für die einzelnen `NativeCell` -Instanz, die auf dem Bildschirm angezeigt wird. Die angezeigten Daten durch jede `NativeAndroidCell` Instanz wird aktualisiert mit den Daten aus der `NativeCell` Instanz die `UpdateCell` Methode. Allerdings die `GetCellCore` Methode wird nicht aufgerufen werden, als der Benutzer einen Bildlauf durch die `ListView`. Stattdessen die `NativeAndroidCell` Instanzen werden erneut verwendet werden.  `PropertyChanged` Ereignisse werden ausgelöst werden, auf die `NativeCell` Instanz, wenn Sie die Daten geändert werden, und die `OnNativeCellPropertyChanged` Ereignishandler aktualisiert die Daten in den einzelnen wiederverwendet `NativeAndroidCell` Instanz.

Das folgende Codebeispiel zeigt die `OnNativeCellPropertyChanged` -Methode, die beim aufgerufen hat eine `PropertyChanged` Ereignis wird ausgelöst:

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

Aktualisiert die Daten, die angezeigt wird, indem Sie für diese Methode erneut verwendet `NativeAndroidCell` Instanzen. Überprüft, ob die Eigenschaft, die geändert wird. wird vorgenommen, wie die-Methode mehrere Male aufgerufen werden kann.

Die `NativeAndroidCell` Klasse definiert das Layout für jede Zelle, und wird im folgenden Codebeispiel dargestellt:

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

Diese Klasse definiert die Steuerelemente verwendet, um den Inhalt der Zelle, und ihr Layout rendern. Die Klasse implementiert die [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) -Schnittstelle, die erforderlich, wenn ist die [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie. Diese Schnittstelle gibt an, dass die Klasse implementieren muss, die [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) -Eigenschaft, die die benutzerdefinierte Zellendaten wiederverwendet Zellen zurückgeben soll.

Die `NativeAndroidCell` Konstruktor vergrößert die `NativeAndroidCell` Layout und initialisiert die `HeadingTextView`, `SubheadingTextView`, und `ImageView` Eigenschaften zu den Steuerelementen im Layout zunehmen. Diese Eigenschaften werden verwendet, um die Anzeige der Daten in die `NativeCell` -Instanz mit der `UpdateCell` Methode aufgerufen wird, um den Wert jeder Eigenschaft festzulegen. Darüber hinaus, wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) cachingstrategie, die angezeigten Daten durch die `HeadingTextView`, `SubheadingTextView`, und `ImageView` Eigenschaften können sein aktualisiert die `OnNativeCellPropertyChanged` -Methode in der der benutzerdefinierte Renderer.

Im folgenden Codebeispiel wird veranschaulicht, die Layoutdefinition für die `NativeAndroidCell.axml` Layoutdatei:

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

Dieses Layout gibt an, dass zwei `TextView` Steuerelemente und ein `ImageView` Steuerelement werden verwendet, um den Inhalt der Zelle angezeigt. Die beiden `TextView` Steuerelemente sind vertikal ausgerichtet ist, in eine `LinearLayout` Steuerelement alle Steuerelemente, die eigenständig sind innerhalb einer `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen benutzerdefinierten Renderer auf UWP

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für UWP:

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

Die `GetTemplate` aufgerufen, um die Zelle für jede Zeile der Daten in der Liste gerendert werden zurückgegeben. Erstellt eine `DataTemplate` für jede `NativeCell` -Instanz, die mit auf dem Bildschirm angezeigt wird der `DataTemplate` definieren die Darstellung und Inhalt der Zelle.

Die `DataTemplate` befindet sich im Ressourcenverzeichnis auf Anwendungsebene und ist im folgenden Codebeispiel gezeigt:

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

Die `DataTemplate` gibt an, die Steuerelemente verwendet, um den Inhalt der Zelle, und das Layout und die Darstellung anzuzeigen. Zwei `TextBlock` Steuerelemente und ein `Image` Steuerelement zur Anzeige von den Inhalt der Zelle mithilfe der Datenbindung verwendet werden. Darüber hinaus eine Instanz von der `ConcatImageExtensionConverter` wird verwendet, um das Verketten der `.jpg` Dateierweiterung jedes Image-Dateinamen. Dadurch wird sichergestellt, dass die `Image` Steuerelement geladen und der Abbildung dargestellt, bei `Source` festgelegt wird.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde beschrieben, wie zum Erstellen eines benutzerdefinierten Renderers für eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) , gehostet in einer Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement. Dadurch wird wiederholt aufgerufen wird, während die Xamarin.Forms-layoutberechnungen beendet `ListView` scrollen.


## <a name="related-links"></a>Verwandte Links

- [ListView-Leistung](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
