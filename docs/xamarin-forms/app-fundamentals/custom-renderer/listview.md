---
title: Anpassen einer ListView
description: Beim Xamarin.Forms-Element „ListView“ handelt es sich um eine Ansicht, in der eine Sammlung von Daten als vertikale Liste angezeigt wird. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer erstellen, der plattformspezifische Listensteuerelemente und natives Zellenlayout kapselt, sodass die Kontrolle über die Leistung nativer Listensteuerelemente erhöht wird.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8892a49f2d7d93f8310293bc70d5e1acdfabe3f5
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937084"
---
# <a name="customizing-a-listview"></a>Anpassen einer ListView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-listview)

_Beim Xamarin.Forms-Element „ListView“ handelt es sich um eine Ansicht, in der eine Sammlung von Daten als vertikale Liste angezeigt wird. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer erstellen, der plattformspezifische Listensteuerelemente und natives Zellenlayout kapselt, sodass die Kontrolle über die Leistung nativer Listensteuerelemente erhöht wird._

Jede Xamarin.Forms-Ansicht verfügt über einen entsprechenden Renderer für jede Plattform, die eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`ListView`](xref:Xamarin.Forms.ListView)-Objekts durch eine Xamarin.Forms-App wird in iOS die `ListViewRenderer`-Klasse instanziiert, wodurch wiederum ein natives `UITableView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `ListViewRenderer`-Klasse ein natives `ListView`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `ListViewRenderer`-Klasse ein natives `ListView`-Steuerelement. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Rendererbasisklassen und native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`ListView`](xref:Xamarin.Forms.ListView)-Steuerelement und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![Beziehungen zwischen dem ListView-Steuerelement und den nativen Steuerelementen, die dieses implementieren](listview-images/listview-classes.png)

Der Renderprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für eine [`ListView`](xref:Xamarin.Forms.ListView)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#creating-the-custom-listview-control) Sie ein benutzerdefiniertes Xamarin.Forms-Steuerelement.
1. [Nutzen](#consuming-the-custom-control) Sie das benutzerdefinierte Steuerelement über Xamarin.Forms.
1. [Erstellen](#creating-the-custom-renderer-on-each-platform) Sie den benutzerdefinierten Renderer für das Steuerelement auf jeder Plattform.

Jedes dieser Elemente wird nachfolgend noch genauer für die Implementierung eines `NativeListView`-Renderers erläutert, der ein plattformspezifisches Listensteuerelement und native Zellenlayouts nutzt. Dieses Szenario ist nützlich, wenn Sie eine vorhandene native App portieren, die Code für Listen und Zellen enthält, der wiederverwendet werden kann. Darüber hinaus können Sie detaillierte Anpassungen der Listensteuerelementfeatures vornehmen, die die Leistung beeinflussen können, in etwa Datenvirtualisierungen.

## <a name="creating-the-custom-listview-control"></a>Erstellen des benutzerdefinierten ListView-Steuerelements

Das benutzerdefinierte Steuerelement [`ListView`](xref:Xamarin.Forms.ListView) kann erstellt werden, indem Sie die `ListView`-Klasse wie in folgendem Codebeispiel als Unterklasse verwenden:

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

Das Steuerelement `NativeListView` wird im .NET Standard-Bibliotheksprojekt erstellt und definiert die API für das benutzerdefinierte Steuerelement. Dieses Steuerelement macht eine `Items`-Eigenschaft verfügbar, die zum Auffüllen der `ListView` mit Daten verwendet wird und zu Anzeigezwecken an Daten gebunden werden kann. Das Steuerelement stellt ebenfalls ein `ItemSelected`-Ereignis zur Verfügung, das ausgelöst wird, wenn ein Element in einem plattformspezifischen, nativen Listensteuerelement ausgewählt wird. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

## <a name="consuming-the-custom-control"></a>Nutzen des benutzerdefinierten Steuerelements

Sie können auf das benutzerdefinierte Steuerelement `NativeListView` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das Steuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `NativeListView` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf das benutzerdefinierte Steuerelement zu verweisen.

Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `NativeListView` von einer C#-Seite genutzt werden kann:

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
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

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Das benutzerdefinierte Steuerelement `NativeListView` verwendet plattformspezifische, benutzerdefinierte Renderer, um eine Liste von Daten anzuzeigen, die mithilfe der `Items`-Eigenschaft aufgefüllt wird. Jede Zeile in der Liste enthält drei Datenelemente: einen Namen, eine Kategorie und einen Dateinamen für ein Bild. Das Layout jeder Zeile in der Liste wird vom plattformspezifischen, benutzerdefinierten Renderer definiert.

> [!NOTE]
> Da das benutzerdefinierte Steuerelement `NativeListView` mithilfe der plattformspezifischen Listensteuerelemente gerendert wird, die gescrollt werden können, darf das benutzerdefinierte Steuerelement nicht in scrollbaren Layoutsteuerelementen wie [`ScrollView`](xref:Xamarin.Forms.ScrollView) gehostet werden.

Ein benutzerdefinierter Renderer kann nun zu jedem Anwendungsprojekt hinzugefügt werden, um plattformspezifische Listensteuerelemente und native Zellenlayouts zu erstellen.

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `ListViewRenderer`-Klasse, die das benutzerdefinierte Steuerelement rendert.
1. Überschreiben Sie die `OnElementChanged`-Methode, die das benutzerdefinierte Steuerelement rendert, und schreiben Sie Logik, um dieses anzupassen. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement [`ListView`](xref:Xamarin.Forms.ListView) erstellt wird.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Optional kann ein benutzerdefinierter Renderer in jedem Plattformprojekt bereitgestellt werden. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse der Zelle verwendet.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![Projektzuständigkeiten beim benutzerdefinierten NativeListView-Renderer](listview-images/solution-structure.png)

Das benutzerdefinierte Steuerelement `NativeListView` wird von plattformspezifischen Rendererklassen gerendert, die alle von der `ListViewRenderer`-Klasse für jede Plattform abgeleitet werden. Das führt dazu, dass jedes benutzerdefinierte `NativeListView`-Steuerelement mit plattformspezifischen Listensteuerelementen und nativen Zellenlayouts gerendert wird. Dies wird in folgenden Screenshots veranschaulicht:

![NativeListView auf jeder Plattform](listview-images/screenshots.png)

Die `ListViewRenderer`-Klasse stellt die `OnElementChanged`-Methode zur Verfügung, die bei der Erstellung des benutzerdefinierten Xamarin.Forms-Steuerelements aufgerufen wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert einen `ElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element dar, an das der Renderer angefügt *ist*. In der Beispielanwendung ist die `OldElement`-Eigenschaft `null`, und die `NewElement`-Eigenschaft enthält einen Verweis auf die `NativeListView`-Instanz.

Das native Steuerelement sollte in der überschriebenen Version der `OnElementChanged`-Methode in jeder plattformspezifischen Rendererklasse angepasst werden. Über die `Control`-Eigenschaft können Sie auf einen typisierten Verweis auf das native Steuerelement zugreifen, das auf der Plattform verwendet wird. Zusätzlich kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird, über die `Element`-Eigenschaft abgerufen werden.

Sie müssen vorsichtig sein, wenn Sie Ereignishandler in der `OnElementChanged`-Methode abonnieren. Sehen Sie sich dazu das folgende Codebeispiel an:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Das native Steuerelement sollte nur dann konfiguriert und Ereignishandler sollten nur dann abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Mit diesem Ansatz kann ein benutzerdefinierter Renderer erstellt werden, der nicht durch Speicherverluste beeinträchtigt wird.

Auf Änderungen am benutzerdefinierten `OnElementPropertyChanged`-Steuerelement in bindbaren Eigenschaften muss in einer überschriebenen Version der Methode Xamarin.Forms in den einzelnen plattformspezifischen Rendererklassen reagiert werden. Eine geänderte Eigenschaft sollte immer überprüft werden, da die Möglichkeit besteht, dass diese Überschreibung häufig aufgerufen wird.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen des zu rendernden benutzerdefinierten Xamarin.Forms-Steuerelements und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird die Implementierung jeder plattformspezifischen, benutzerdefinierten Rendererklasse erläutert.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

Das `UITableView`-Steuerelement wird durch Erstellen einer Instanz der `NativeiOSListViewSource`-Klasse konfiguriert, vorausgesetzt der benutzerdefinierte Renderer wird an ein neues Xamarin.Forms-Element angefügt. Diese Klasse stellt Daten für das `UITableView`-Steuerelement bereit, indem die Methoden `RowsInSection` und `GetCell` der `UITableViewSource`-Klasse überschrieben werden und eine `Items`-Eigenschaft verfügbar gemacht wird, die die anzuzeigende Liste der Daten enthält. Mithilfe dieser Klasse kann auch die `RowSelected`-Methode überschrieben werden, die das vom benutzerdefinierten Steuerelement `NativeListView` bereitgestellte `ItemSelected`-Ereignis aufruft. Weitere Informationen zur Überschreibung von Methoden finden Sie unter [Erstellen von Unterklassen für UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Die `GetCell`-Methode gibt eine `UITableCellView` zurück, für die jede Zeile in der Liste mit Daten aufgefüllt wird und die im folgenden Codebeispiel dargestellt wird:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

Diese Methode erstellt eine `NativeiOSListViewCell`-Instanz für jede Datenzeile, die auf dem Bildschirm angezeigt wird. Die `NativeiOSCell`-Instanz definiert das Layout jeder Zelle sowie der Daten in der Zelle. Wenn eine Zelle durch Scrollen vom Bildschirm, wird diese für die Wiederverwendung verfügbar gemacht. So wird verhindert, dass Arbeitsspeicher verschwendet wird, indem sichergestellt wird, dass nur `NativeiOSCell`-Instanzen für die Daten auf dem Bildschirm angezeigt werden und nicht alle Daten in der Liste. Weitere Informationen zum Wiederverwenden von Zellen finden Sie unter [Cell Reuse (Wiederverwenden von Zellen)](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Die `GetCell`-Methode liest auch die `ImageFilename`-Eigenschaft jeder Datenzeile (falls vorhanden) sowie das Bild und speichert dieses als `UIImage`-Instanz, bevor die `NativeiOSListViewCell`-Instanz mit den Daten für die Zeile (Name, Kategorie und Bild) aktualisiert wird.

Die `NativeiOSListViewCell`-Klasse definiert das Layout für jede Zelle und ist im folgenden Codebeispiel dargestellt:

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Diese Klasse definiert die Steuerelemente, die zum Rendern der Inhalte der Zellen sowie deren Layout verwendet werden. Der Konstruktor `NativeiOSListViewCell` erstellt Instanzen der Steuerelemente `UILabel` und `UIImageView` und initialisiert deren Darstellung. Diese Steuerelemente werden zum Anzeigen der Daten jeder Zeile verwendet. Dabei wird die `UpdateCell`-Methode verwendet, um diese Daten an die Instanzen `UILabel` und `UIImageView` zu übermitteln. Die Position dieser Instanzen wird durch Überschreibung der `LayoutSubviews`-Methode festgelegt, indem deren Koordinaten innerhalb der Zelle angegeben werden.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf eine Eigenschaftsänderung im benutzerdefinierten Steuerelement

Wenn sich die `NativeListView.Items`-Eigenschaft ändert, weil Elemente zur Liste hinzugefügt bzw. aus dieser entfernt werden, muss der benutzerdefinierte Renderer darauf reagieren, indem er diese Änderungen anzeigt. Dies kann wie im folgenden Codebeispiel dargestellt erreicht werden, indem Sie die `OnElementPropertyChanged`-Methode überschreiben:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Die Methode erstellt eine neue Instanz der `NativeiOSListViewSource`-Klasse, die Daten für das Steuerelement `UITableView` bereitstellt, vorausgesetzt, dass sich die bindbare `NativeListView.Items`-Eigenschaft geändert hat.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

Das native `ListView`-Steuerelement wird konfiguriert, vorausgesetzt der benutzerdefinierte Renderer wird an ein neues Xamarin.Forms-Element angefügt. Die Konfiguration umfasst das Erstellen einer Instanz der `NativeAndroidListViewAdapter`-Klasse, die Daten für das native `ListView`-Steuerelement bereitstellt, sowie das Registrieren eines Ereignishandlers zum Verarbeiten des `ItemClick`-Ereignisses. Im Gegenzug ruft der Handler das vom benutzerdefinierten `NativeListView`-Steuerelement bereitgestellte `ItemSelected`-Ereignis auf. Das Abonnement des `ItemClick`-Ereignisses wird gekündigt, wenn das Xamarin.Forms-Element, an das der Renderer angefügt ist, geändert wird.

Die `NativeAndroidListViewAdapter`-Klasse leitet sich von der `BaseAdapter`-Klasse ab und macht eine `Items`-Eigenschaft verfügbar, die die Liste der anzuzeigenden Daten enthält, und überschreibt die Methoden `Count`, `GetView`, `GetItemId` und `this[int]`. Weitere Informationen zu diesen Methodenüberschreibungen finden Sie unter [Implementing a ListAdapter (Implementieren einer ListAdapter-Eigenschaft)](~/android/user-interface/layouts/list-view/populating.md). Die `GetView`-Methode gibt eine Ansicht für jede Zeile zurück, die mit Daten aufgefüllt wird und die im folgenden Codebeispiel dargestellt wird:

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

Die `GetView`-Methode wird aufgerufen, um die Zelle zurückzugeben, die für jede Datenzeile in der Liste als `View` gerendert werden soll. Sie erstellt eine `View`-Instanz für jede Datenzeile, die auf dem Bildschirm angezeigt wird. Die Darstellung der `View`-Instanz wird in einer Layoutdatei definiert. Wenn eine Zelle durch Scrollen vom Bildschirm, wird diese für die Wiederverwendung verfügbar gemacht. So wird verhindert, dass Arbeitsspeicher verschwendet wird, indem sichergestellt wird, dass nur `View`-Instanzen für die Daten auf dem Bildschirm angezeigt werden und nicht alle Daten in der Liste. Weitere Informationen zur Wiederverwendung der Ansicht finden Sie unter [Row View Re-use (Wiederverwenden von Zeilenansichten)](~/android/user-interface/layouts/list-view/populating.md).

Die `GetView`-Methode füllt auch die `View`-Instanz mit Daten auf. Zudem liest sie die Bilddaten aus dem in der `ImageFilename`-Eigenschaft angegeben Dateinamen.

Das Layout jeder von der nativen `ListView` angezeigten Zelle wird in der Layoutdatei `NativeAndroidListViewCell.axml` definiert, die von der `LayoutInflater.Inflate`-Methode vergrößert wird. Im folgenden Codebeispiel wird die Layoutdefinition veranschaulicht:

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
            android:id="@+id/Text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf eine Eigenschaftsänderung im benutzerdefinierten Steuerelement

Wenn sich die `NativeListView.Items`-Eigenschaft ändert, weil Elemente zur Liste hinzugefügt bzw. aus dieser entfernt werden, muss der benutzerdefinierte Renderer darauf reagieren, indem er diese Änderungen anzeigt. Dies kann wie im folgenden Codebeispiel dargestellt erreicht werden, indem Sie die `OnElementPropertyChanged`-Methode überschreiben:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Die Methode erstellt eine neue Instanz der `NativeAndroidListViewAdapter`-Klasse, die Daten für das native Steuerelement `ListView` bereitstellt, vorausgesetzt dass sich die bindbare `NativeListView.Items`-Eigenschaft geändert hat.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen des benutzerdefinierten Renderers auf der UWP

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die UWP veranschaulicht:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

Das native `ListView`-Steuerelement wird konfiguriert, vorausgesetzt der benutzerdefinierte Renderer wird an ein neues Xamarin.Forms-Element angefügt. Bei dieser Konfiguration muss festgelegt werden, wie das native `ListView`-Steuerelement auf Elemente reagiert, die ausgewählt werden. Zudem müssen die vom Steuerelement angezeigten Daten aufgefüllt werden, die Darstellung und Inhalte jeder Zelle definiert und ein Ereignishandler zum Verarbeiten des `SelectionChanged`-Ereignisses registriert werden. Im Gegenzug ruft der Handler das vom benutzerdefinierten `NativeListView`-Steuerelement bereitgestellte `ItemSelected`-Ereignis auf. Das Abonnement des `SelectionChanged`-Ereignisses wird gekündigt, wenn das Xamarin.Forms-Element, an das der Renderer angefügt ist, geändert wird.

Die Darstellung und Inhalte jeder nativen `ListView`-Zelle werden von einer `DataTemplate`-Klasse mit der Bezeichnung `ListViewItemTemplate` definiert. Die `DataTemplate` wird im Ressourcenverzeichnis auf Anwendungsebene gespeichert und ist im folgenden Codebeispiel dargestellt:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf eine Eigenschaftsänderung im benutzerdefinierten Steuerelement

Wenn sich die `NativeListView.Items`-Eigenschaft ändert, weil Elemente zur Liste hinzugefügt bzw. aus dieser entfernt werden, muss der benutzerdefinierte Renderer darauf reagieren, indem er diese Änderungen anzeigt. Dies kann wie im folgenden Codebeispiel dargestellt erreicht werden, indem Sie die `OnElementPropertyChanged`-Methode überschreiben:

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

Die Methode füllt das native `ListView`-Steuerelement erneut mit den geänderten Daten auf, vorausgesetzt, dass sich die bindbare `NativeListView.Items`-Eigenschaft geändert hat.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen benutzerdefinierten Renderer erstellen, der plattformspezifische Listensteuerelemente und natives Zellenlayout kapselt, sodass die Kontrolle über die Leistung nativer Listensteuerelemente erhöht wird.

## <a name="related-links"></a>Verwandte Links

- [CustomRendererListView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-listview)
