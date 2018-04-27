---
title: Anpassen einer ViewCell
description: Eine Xamarin.Forms-ViewCell ist eine Zelle, die hinzugefügt werden kann, eine ListView oder TableView, die eine Entwickler benutzerdefinierte Sicht enthält. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer für eine ViewCell, die in einem Xamarin.Forms ListView-Steuerelement gehostet wird. Dies beendet die Xamarin.Forms-Layout-Berechnungen nicht während des Bildlaufs ListView wiederholt aufgerufen.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 4d1d4323e42df6240fee7be42ae8fac70a2b3f1f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="customizing-a-viewcell"></a>Anpassen einer ViewCell

_Eine Xamarin.Forms-ViewCell ist eine Zelle, die hinzugefügt werden kann, eine ListView oder TableView, die eine Entwickler benutzerdefinierte Sicht enthält. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer für eine ViewCell, die in einem Xamarin.Forms ListView-Steuerelement gehostet wird. Dies beendet die Xamarin.Forms-Layout-Berechnungen nicht während des Bildlaufs ListView wiederholt aufgerufen._

Jede Zelle Xamarin.Forms verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) einer Xamarin.Forms-Anwendung in iOS gerendert wird die `ViewCellRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `UITableViewCell` Steuerelement. Auf der Android-Plattform die `ViewCellRenderer` Klasse instanziiert ein systemeigenes `View` Steuerelement. Auf die universelle Windows-Plattform (UWP), die `ViewCellRenderer` Klasse instanziiert ein systemeigenes `DataTemplate`. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) und das entsprechende systemeigene Steuerelemente, die ihn implementieren:

![](viewcell-images/viewcell-classes.png "Beziehung zwischen dem ViewCell-Steuerelement und die implementierende Native Steuerelemente")

Des Renderingprozesses kann Vorteil ausgeführt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Cell) eine benutzerdefinierte Xamarin.Forms-Zelle.
1. [Nutzen](#Consuming_the_Custom_Cell) die benutzerdefinierte Zelle von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für die Zelle auf jeder Plattform.

Jedes Element wird nun wiederum zum Implementieren der besprochen werden eine `NativeCell` Renderer an, der einen plattformspezifischen Layout für jede Zelle in einer Xamarin.Forms gehostet nutzt [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement. Dies beendet die Xamarin.Forms Layout Berechnungen wiederholt aufgerufen wird, während der `ListView` Durchführen eines Bildlaufs.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Erstellen die benutzerdefinierte Zelle

Ein Zellensteuerelement benutzerdefinierte Unterklassen erstellt werden die [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) Klasse, wie im folgenden Codebeispiel gezeigt:

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
Die `NativeCell` Klasse wird im Projekt portablen Klassenbibliothek (PCL) erstellt und definiert die API für die benutzerdefinierte Zelle. Die benutzerdefinierte Zelle macht `Name`, `Category`, und `ImageFilename` Eigenschaften, die über die Datenbindung angezeigt werden können. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Nutzen die benutzerdefinierte Zelle

Die `NativeCell` benutzerdefinierte Zelle kann verwiesen werden in Xaml in der PCL-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix auf die benutzerdefinierte Cell-Element. Im folgenden Codebeispiel wird veranschaulicht wie die `NativeCell` benutzerdefinierte Zelle durch die entsprechende Verwendung von XAML-Seite genutzt werden kann:

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

Die `local` nichts Namespacepräfix kann benannt werden. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details des benutzerdefinierten Steuerelements übereinstimmen. Sobald der Namespace deklariert ist, wird das Präfix verwendet, auf die benutzerdefinierte Zelle verweisen.

Im folgenden Codebeispiel wird veranschaulicht wie die `NativeCell` benutzerdefinierte Zelle durch die entsprechende Seite c# genutzt werden kann:

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

Ein Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement wird verwendet, um eine Liste der Daten anzuzeigen, wodurch die über aufgefüllt wird die [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) Eigenschaft. Die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie versucht, minimieren Sie die `ListView` Arbeitsspeicherbedarf und die Ausführung zu beschleunigen, indem Sie die Listenzellen wiederverwenden. Weitere Informationen finden Sie unter [Zwischenspeichern Strategie](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Jede Zeile in der Liste enthält drei Elemente von Daten – einen Namen, eine Kategorie und einen Bilddateinamen. Das Layout der jede Zeile in der Liste wird definiert, indem die `DataTemplate` , verwiesen wird, über die [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) bindbare Eigenschaft. Die `DataTemplate` definiert, dass jede Datenzeile in der Liste kann eine `NativeCell` , anzeigt seine `Name`, `Category`, und `ImageFilename` Eigenschaften über die Datenbindung. Weitere Informationen zu den `ListView` steuern, finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt zum Anpassen des Layouts plattformspezifischen für jede Zelle hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `ViewCellRenderer` -Klasse, die die benutzerdefinierte Zelle gerendert wird.
1. Überschreiben Sie die plattformspezifischen-Methode, die die benutzerdefinierte Zelle rendert und Schreiben Sie Logik, um ihn anzupassen.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass verwendet die benutzerdefinierte Xamarin.Forms-Zelle gerendert wird. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms Elemente ist optional, geben Sie einen benutzerdefinierten Renderer in jedem plattformprojekt. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Basisklasse für das Steuerelement verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in jede plattformprojekt beim Rendern einer [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) Element.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](viewcell-images/solution-structure.png "NativeCell benutzerdefinierten Renderer Projekt Zuständigkeiten")

Die `NativeCell` plattformspezifischen, alle abgeleiteten Renderingklassen benutzerdefinierte Zelle gerendert wird die `ViewCellRenderer` Klasse für jede Plattform. Dies führt in den einzelnen `NativeCell` benutzerdefinierte Zelle mit plattformspezifischen Layout, gerendert wird, wie in den folgenden Screenshots dargestellt:

![](viewcell-images/screenshots.png "NativeCell auf jeder Plattform")

Die `ViewCellRenderer` -Klasse macht Clientplattform-spezifische Methoden zum Rendern der benutzerdefinierten Zelle. Dies ist die `GetCell` Methode für die iOS-Plattform, die `GetCellCore` Methode für die Android-Plattform und die `GetTemplate` Methode für die universelle Windows-Plattform.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: der Typname der Zelle Xamarin.Forms gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

In den folgenden Abschnitten erläutern die Implementierung der einzelnen benutzerdefinierten Renderers Clientplattform-spezifische Klassen.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

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

Die `GetCell` Methode wird aufgerufen, um jede Zelle anzuzeigenden zu erstellen. Jede Zelle einer `NativeiOSCell` -Instanz, die das Layout der Zelle und seine Daten definiert. Der Vorgang von der `GetCell` Methode ist abhängig von der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie:

- Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie ist [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)die `GetCell` Methode wird aufgerufen, die für jede Zelle. Ein `NativeiOSCell` für jede Instanz erstellt werden `NativeCell` -Instanz, die anfänglich auf dem Bildschirm angezeigt wird. Da der Benutzer einen Bildlauf der `ListView`, `NativeiOSCell` Instanzen werden erneut verwendet werden. Weitere Informationen zu iOS Zelle erneut verwenden, finden Sie unter [Zelle wiederverwenden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Dieser Code benutzerdefinierter Renderer führt einige Zelle erneut verwenden, auch wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Zellen beibehalten festgelegt ist.

  Die angezeigten Daten durch jede `NativeiOSCell` Instanz, ob neu erstellt oder erneut verwendet, aktualisiert mit den Daten aus allen `NativeCell` Instanz, indem die `UpdateCell` Methode.

  > [!NOTE]
  > Die `OnNativeCellPropertyChanged` Methode werden nie aufgerufen wird, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie Zellen beibehalten festgelegt ist.

- Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie ist [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)die `GetCell` Methode wird aufgerufen, die für jede Zelle, die anfänglich auf dem Bildschirm angezeigt wird. Ein `NativeiOSCell` für jede Instanz erstellt werden `NativeCell` -Instanz, die anfänglich auf dem Bildschirm angezeigt wird. Von jedem angezeigten Daten `NativeiOSCell` Instanz aktualisiert mit den Daten aus der `NativeCell` Instanz, indem die `UpdateCell` Methode. Allerdings die `GetCell` Methode wird nicht aufgerufen werden, als der Benutzer einen Bildlauf der `ListView`. Stattdessen die `NativeiOSCell` Instanzen werden erneut verwendet werden. `PropertyChanged` auf Ereignisse ausgelöst werden soll die `NativeCell` -Instanz auf, wenn die Daten geändert werden, und die `OnNativeCellPropertyChanged` Ereignishandler aktualisiert die Daten in den einzelnen wiederverwendet `NativeiOSCell` Instanz.

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

Aktualisiert diese Methode wird vom angezeigten Daten erneut verwendet `NativeiOSCell` Instanzen. Eine Überprüfung für die Eigenschaft, die geändert wird wird durchgeführt, wie die Methode kann mehrfach ausgerufen werden.

Die `NativeiOSCell` Klasse definiert das Layout für jede Zelle, und wird im folgenden Codebeispiel gezeigt:

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

Diese Klasse definiert die Steuerelemente verwendet, um den Inhalt der Zelle, und ihr Layout zu rendern. Die Klasse implementiert die [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) -Schnittstelle, ist erforderlich, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie. Diese Schnittstelle gibt an, dass die Klasse implementieren muss, die [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) -Eigenschaft, die die benutzerdefinierte Zelldaten wiederverwendet Zellen zurückgeben soll.

Die `NativeiOSCell` Konstruktor initialisiert die Darstellung der `HeadingLabel`, `SubheadingLabel`, und `CellImageView` Eigenschaften. Diese Eigenschaften werden verwendet, um die in gespeicherten Daten anzeigen der `NativeCell` Instanz, mit der `UpdateCell` Methode wird aufgerufen, um den Wert jeder Eigenschaft festzulegen. Darüber hinaus, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie, die angezeigten Daten durch die `HeadingLabel`, `SubheadingLabel`, und `CellImageView` Eigenschaften möglich aktualisiert die `OnNativeCellPropertyChanged` -Methode in der benutzerdefinierten Renderer.

Zellenlayout wird ausgeführt, indem die `LayoutSubviews` zu überschreiben, wodurch die Koordinaten der `HeadingLabel`, `SubheadingLabel`, und `CellImageView` in der Zelle.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Das folgende Codebeispiel zeigt die benutzerdefinierten Renderers für das Android-Plattform:

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

Die `GetCellCore` Methode wird aufgerufen, um jede Zelle anzuzeigenden zu erstellen. Jede Zelle einer `NativeAndroidCell` -Instanz, die das Layout der Zelle und seine Daten definiert. Der Vorgang von der `GetCellCore` Methode ist abhängig von der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie:

- Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie ist [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)die `GetCellCore` Methode wird aufgerufen, die für jede Zelle. Ein `NativeAndroidCell` erstellt werden, für die einzelnen `NativeCell` -Instanz, die anfänglich auf dem Bildschirm angezeigt wird. Da der Benutzer einen Bildlauf der `ListView`, `NativeAndroidCell` Instanzen werden erneut verwendet werden. Weitere Informationen zu Android Zelle erneut verwenden, finden Sie unter [Zeile Sicht erneut verwenden](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Beachten Sie, dass einige Zelle erneut verwenden diesen benutzerdefinierten Renderers Code ausgeführt wird, auch wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Zellen beibehalten festgelegt ist.

  Die angezeigten Daten durch jede `NativeAndroidCell` Instanz, ob neu erstellt oder erneut verwendet, aktualisiert mit den Daten aus allen `NativeCell` Instanz, indem die `UpdateCell` Methode.

  > [!NOTE]
  > Beachten Sie, dass der `OnNativeCellPropertyChanged` Methode wird aufgerufen, wenn der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ist festgelegt, Zellen beizubehalten, er wird nicht aktualisiert die `NativeAndroidCell` Eigenschaftswerte.

- Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cachingstrategie ist [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)die `GetCellCore` Methode wird aufgerufen, die für jede Zelle, die anfänglich auf dem Bildschirm angezeigt wird. Ein `NativeAndroidCell` für jede Instanz erstellt werden `NativeCell` -Instanz, die anfänglich auf dem Bildschirm angezeigt wird. Von jedem angezeigten Daten `NativeAndroidCell` Instanz aktualisiert mit den Daten aus der `NativeCell` Instanz, indem die `UpdateCell` Methode. Allerdings die `GetCellCore` Methode wird nicht aufgerufen werden, als der Benutzer einen Bildlauf der `ListView`. Stattdessen die `NativeAndroidCell` Instanzen werden erneut verwendet werden.  `PropertyChanged` auf Ereignisse ausgelöst werden soll die `NativeCell` -Instanz auf, wenn die Daten geändert werden, und die `OnNativeCellPropertyChanged` Ereignishandler aktualisiert die Daten in den einzelnen wiederverwendet `NativeAndroidCell` Instanz.

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

Aktualisiert diese Methode wird vom angezeigten Daten erneut verwendet `NativeAndroidCell` Instanzen. Eine Überprüfung für die Eigenschaft, die geändert wird wird durchgeführt, wie die Methode kann mehrfach ausgerufen werden.

Die `NativeAndroidCell` Klasse definiert das Layout für jede Zelle, und wird im folgenden Codebeispiel gezeigt:

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

Diese Klasse definiert die Steuerelemente verwendet, um den Inhalt der Zelle, und ihr Layout zu rendern. Die Klasse implementiert die [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) -Schnittstelle, ist erforderlich, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie. Diese Schnittstelle gibt an, dass die Klasse implementieren muss, die [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) -Eigenschaft, die die benutzerdefinierte Zelldaten wiederverwendet Zellen zurückgeben soll.

Die `NativeAndroidCell` Konstruktor vergrößert die `NativeAndroidCell` Layout und initialisiert die `HeadingTextView`, `SubheadingTextView`, und `ImageView` Eigenschaften für die Steuerelemente im vergrößerte Layout. Diese Eigenschaften werden verwendet, um die in gespeicherten Daten anzeigen der `NativeCell` Instanz, mit der `UpdateCell` Methode wird aufgerufen, um den Wert jeder Eigenschaft festzulegen. Darüber hinaus, wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) verwendet die [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) cachingstrategie, die angezeigten Daten durch die `HeadingTextView`, `SubheadingTextView`, und `ImageView` Eigenschaften möglich aktualisiert die `OnNativeCellPropertyChanged` -Methode in der benutzerdefinierten Renderer.

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

Dieses Layout gibt an, dass zwei `TextView` Steuerelemente und ein `ImageView` Steuerelement werden verwendet, um den Inhalt der Zelle angezeigt. Die beiden `TextView` Steuerelemente sind vertikal ausgerichtet, innerhalb einer `LinearLayout` Steuerelement alle Steuerelemente, die eigenständig sind innerhalb einer `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen von benutzerdefinierten Renderers für universelle Windows-Plattform

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für universelle Windows-Plattform:

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

Die `GetTemplate` Methode wird aufgerufen, um die Zelle für jede Datenzeile in der Liste wiedergegeben werden zurückgegeben. Erstellt eine `DataTemplate` für jede `NativeCell` -Instanz, die auf dem Bildschirm mit angezeigt wird, die `DataTemplate` definieren die Darstellung und Inhalt der Zelle.

Die `DataTemplate` wird in der Anwendungsebene Ressourcenwörterbuch gespeichert und wird im folgenden Codebeispiel gezeigt:

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

Die `DataTemplate` bestimmt die Steuerelemente verwendet, um den Inhalt der Zelle, und das Layout und die Darstellung anzuzeigen. Zwei `TextBlock` Steuerelemente und ein `Image` -Steuerelement zum Anzeigen von Inhalt der Zelle durch die Datenbindung verwendet werden. Darüber hinaus eine Instanz von der `ConcatImageExtensionConverter` dient zum Verketten der `.jpg` Dateierweiterung auf jedes Bilddateinamen. Dadurch wird sichergestellt, dass die `Image` Steuerelement laden und das Bild zu rendern, wenn er sich `Source` festgelegt wird.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat zum Erstellen eines benutzerdefinierten Renderers für gezeigt eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) gehostet wird, innerhalb einer Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement. Dies beendet die Xamarin.Forms Layout Berechnungen wiederholt aufgerufen wird, während der `ListView` Durchführen eines Bildlaufs.


## <a name="related-links"></a>Verwandte Links

- [ListView-Leistung](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
