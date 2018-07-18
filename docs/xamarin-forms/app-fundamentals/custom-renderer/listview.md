---
title: Anpassen einer ListView
description: Eine Xamarin.Forms-ListView ist eine Ansicht, die eine Sammlung von Daten als vertikale Liste angezeigt. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer, der plattformspezifische Listensteuerelementen und native Zelle Layouts, kapselt ermöglicht mehr Kontrolle über die Steuerung der Leistung des systemeigenen Liste.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997483"
---
# <a name="customizing-a-listview"></a>Anpassen einer ListView

_Eine Xamarin.Forms-ListView ist eine Ansicht, die eine Sammlung von Daten als vertikale Liste angezeigt. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer, der plattformspezifische Listensteuerelementen und native Zelle Layouts, kapselt ermöglicht mehr Kontrolle über die Steuerung der Leistung des systemeigenen Liste._

Alle Xamarin.Forms-Ansicht ist eine begleitende-Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `ListView` ](xref:Xamarin.Forms.ListView) gerendert wird, von einer Xamarin.Forms-Anwendung unter iOS die `ListViewRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `UITableView` Steuerelement. Auf der Android-Plattform die `ListViewRenderer` Klasse instanziiert ein systemeigenes `ListView` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `ListViewRenderer` Klasse instanziiert ein systemeigenes `ListView` Steuerelement. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `ListView` ](xref:Xamarin.Forms.ListView) Steuerelement und den entsprechenden systemeigenen Steuerelementen, die sie implementieren:

![](listview-images/listview-classes.png "Beziehung zwischen dem ListView-Steuerelement und die implementierenden Native Steuerelemente")

Das Rendern zu kann nutzen erstellt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `ListView` ](xref:Xamarin.Forms.ListView) auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_ListView_Control) eines benutzerdefinierten Xamarin.Forms-Steuerelements.
1. [Nutzen](#Consuming_the_Custom_Control) das benutzerdefinierte Steuerelement von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das Steuerelement auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer `NativeListView` Renderer, der plattformspezifischen Listensteuerelementen und native Zelle Layouts nutzt. Dieses Szenario ist nützlich, beim Portieren eine vorhandene native app, die Liste enthält und die Zelle-Code, der nicht erneut verwendet werden kann. Darüber hinaus können sie detaillierte Anpassung der Listenfunktionen, die Leistung zu erzielen, z. B. Datenvirtualisierung beeinflussen können.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Erstellen des benutzerdefinierten ListView-Steuerelements

Eine benutzerdefinierte [ `ListView` ](xref:Xamarin.Forms.ListView) Steuerelement kann erstellt werden, indem Unterklassen der `ListView` Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die `NativeListView` wird in .NET Standard Library-Projekt erstellt und definiert die API für das benutzerdefinierte Steuerelement. Dieses Steuerelement stellt eine `Items` -Eigenschaft, die verwendet wird, für das Auffüllen der `ListView` mit Daten, und hierzu für die gebundenen Daten Anzeigezwecken. Macht auch eine `ItemSelected` -Ereignis, das ausgelöst wird, wenn ein Element in einem plattformspezifischen native Steuerelement ausgewählt ist. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen das benutzerdefinierte Steuerelement

Die `NativeListView` benutzerdefiniertes Steuerelement kann verwiesen werden in Xaml in .NET Standard Library-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix für das Steuerelement. Das folgende Codebeispiel zeigt die `NativeListView` benutzerdefiniertes Steuerelement kann von einer XAML-Seite verwendet werden:

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

Die `local` Namespacepräfix kann eine beliebige Bezeichnung. Allerdings die `clr-namespace` und `assembly` -Werte müssen die Details des benutzerdefinierten Steuerelements entsprechen. Sobald der Namespace deklariert wird, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

Das folgende Codebeispiel zeigt die `NativeListView` benutzerdefiniertes Steuerelement durch einen C# -Code genutzt werden kann:

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

Die `NativeListView` benutzerdefinierte Steuerelement verwendet plattformspezifische benutzerdefinierte Renderer anzuzeigende eine Liste der Daten, die über aufgefüllt wird die `Items` Eigenschaft. Jede Zeile in der Liste enthält drei Datenelemente: einen Namen, eine Kategorie und ein Dateiname des Bilds. Das Layout der einzelnen Zeilen in der Liste wird vom benutzerdefinierte plattformspezifische Renderer definiert.

> [!NOTE]
> Da die `NativeListView` benutzerdefiniertes Steuerelement wird gerendert werden, mithilfe von plattformspezifischen Listensteuerelemente, die enthalten, scrollen die Möglichkeit, das benutzerdefinierte Steuerelement nicht gehostet werden soll in bildlauffähigen Layout-Steuerelemente wie z. B. die [ `ScrollView` ](xref:Xamarin.Forms.ScrollView).

Ein benutzerdefinierter Renderer kann jedes Anwendungsprojekt plattformspezifische Liste und native Zelle Layouts erstellt nun hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `ListViewRenderer` -Klasse, die das benutzerdefinierte Steuerelement rendert.
1. Überschreiben der `OnElementChanged` -Methode, die benutzerdefinierte Steuerelement und Schreiben-Logik, um es anzupassen rendert. Diese Methode wird aufgerufen, wenn der entsprechende Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des benutzerdefinierten Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Dies ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse der Zelle verwendet werden.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](listview-images/solution-structure.png "Zuständigkeiten von NativeListView benutzerdefinierten Renderer-Projekt")

Die `NativeListView` benutzerdefiniertes Steuerelement gerendert wird, durch das plattformspezifische, alle abgeleiteten Renderingklassen der `ListViewRenderer` -Klasse für jede Plattform. Dies führt in jeder `NativeListView` benutzerdefiniertes Steuerelement mit plattformspezifischen Listensteuerelementen und systemeigenen Zelle Layouts, gerendert wird, wie in den folgenden Screenshots gezeigt:

![](listview-images/screenshots.png "NativeListView auf jeder Plattform")

Die `ListViewRenderer` -Klasse macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das benutzerdefinierte Steuerelement mit Xamarin.Forms erstellt wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert eine `ElementChangedEventArgs` Parameter, mit `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, die den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, die den Renderer *ist* angefügt wird, bzw. In der beispielanwendung die `OldElement` Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `NativeListView` Instanz.

Eine außer Kraft gesetzte Version von der `OnElementChanged` -Methode, in den einzelnen plattformspezifischen rendererklassen an, ist der Ort für die Anpassung von systemeigenen Steuerelementen führen. Ein typisierter Verweis auf das native Steuerelement verwendet wird, auf der Plattform möglich über die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

Vorsicht beim Abonnieren von Ereignishandlern in die `OnElementChanged` -Methode, wie im folgenden Codebeispiel gezeigt:

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

Das native Steuerelement sollte nur dann konfiguriert und Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Ebenso sollten alle Ereignishandler, die abonniert wurden abbestellt werden, nur, wenn das Element der Renderer Änderungen zugeordnet ist. Diese Vorgehensweise können Sie einen benutzerdefinierten Renderer erstellen, der durch Speicherverluste beeinträchtigt nicht.

Eine außer Kraft gesetzte Version von der `OnElementPropertyChanged` -Methode, in den einzelnen plattformspezifischen rendererklassen an, ist der Ort, an die Reaktion auf Änderungen der bindbare Eigenschaft des benutzerdefinierten Xamarin.Forms-Steuerelements. Überprüft, ob die geänderte Eigenschaft sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: den Typnamen des benutzerdefinierten Xamarin.Forms-Steuerelements gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Den folgenden Abschnitten werden die Implementierung der einzelnen plattformspezifischen benutzerdefinierten Renderer-Klasse.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die iOS-Plattform:

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

Die `UITableView` Steuerelement ist so konfiguriert, durch das Erstellen einer Instanz von der `NativeiOSListViewSource` Klasse, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Diese Klasse stellt Daten für die `UITableView` -Steuerelements durch Überschreiben der `RowsInSection` und `GetCell` Methoden aus der `UITableViewSource` Klasse und das Verfügbarmachen von von einer `Items` -Eigenschaft, die die Liste der anzuzeigenden Daten enthält. Die Klasse bietet auch eine `RowSelected` Methode außer Kraft setzen, die aufruft der `ItemSelected` Ereignis bereitgestellt werden, indem die `NativeListView` benutzerdefiniertes Steuerelement. Weitere Informationen über die Methode überschreibt, finden Sie unter [Unterklassen UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Die `GetCell` Methode gibt eine `UITableCellView` , die mit Daten für jede Zeile in der Liste aufgefüllt wird, und wird im folgenden Codebeispiel dargestellt:

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

Diese Methode erstellt eine `NativeiOSListViewCell` Instanz für jede Zeile der Daten, die auf dem Bildschirm angezeigt werden. Die `NativeiOSCell` Instanz definiert das Layout der einzelnen Zellen und die Zelldaten. Wenn eine Zelle auf dem Bildschirm eines Bildlaufs durch geschlossen wird, wird die Zelle für die Wiederverwendung verfügbar gestellt. Dies vermeidet verschwenden Arbeitsspeicher Leuchten, die nur `NativeiOSCell` Instanzen für die Daten auf dem Bildschirm, statt alle Daten in der Liste angezeigt wird. Weitere Informationen zu Zelle Wiederverwendung, finden Sie unter [Zelle wiederverwenden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Die `GetCell` Methode liest auch die `ImageFilename` jede Zeile der Daten, die angegeben, dass es vorhanden ist, und liest das Image und speichert es als Eigenschaft einer `UIImage` Instanz vor dem Aktualisieren der `NativeiOSListViewCell` -Instanz mit den Daten (Name, Kategorie und Image) für die Zeile.

Die `NativeiOSListViewCell` Klasse definiert das Layout für jede Zelle, und wird im folgenden Codebeispiel dargestellt:

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

Diese Klasse definiert die Steuerelemente verwendet, um den Inhalt der Zelle, und ihr Layout rendern. Die `NativeiOSListViewCell` -Konstruktor erstellt Instanzen von `UILabel` und `UIImageView` steuert und ihre Darstellung initialisiert. Diese Steuerelemente werden verwendet, um jede Zeile der Daten mit anzeigen, die `UpdateCell` Methode, die für diese Daten festgelegt wird, die `UILabel` und `UIImageView` Instanzen. Der Speicherort dieser Instanzen festgelegt ist von der überschriebenen `LayoutSubviews` -Methode, von ihren Koordinaten in der Zelle angibt.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf die Änderung einer Eigenschaft des benutzerdefinierten Steuerelements

Wenn die `NativeListView.Items` Eigenschaft geändert wird, aufgrund der Elemente, die hinzugefügt wird, oder aus der Liste entfernt, der benutzerdefinierte Renderer reagieren, indem Sie die Änderungen angezeigt werden soll. Dies kann erreicht werden, durch Überschreiben der `OnElementPropertyChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Die Methode erstellt eine neue Instanz der der `NativeiOSListViewSource` Klasse, die Daten in eine der `UITableView` steuern, bereitgestellt, die bindbare `NativeListView.Items` -Eigenschaft geändert hat.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die Android-Plattform:

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

Das systemeigene `ListView` Steuerelement ist so konfiguriert werden, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Diese Konfiguration umfasst das Erstellen einer Instanz der `NativeAndroidListViewAdapter` -Klasse, die Daten, das dem systemeigenen bereitstellt `ListView` steuern, und registrieren einen Ereignishandler zum Verarbeiten der `ItemClick` Ereignis. Dieser Handler wird aufgerufen, wiederum die `ItemSelected` Ereignis bereitgestellt werden, indem der `NativeListView` benutzerdefiniertes Steuerelement. Die `ItemClick` Ereignis ist abbestellt, wenn Änderungen der Xamarin.Forms-Element der Renderer angefügt ist.

Die `NativeAndroidListViewAdapter` leitet sich von der `BaseAdapter` -Klasse und stellt eine `Items` -Eigenschaft, die die Liste der anzuzeigenden Daten enthält, als auch überschreiben die `Count`, `GetView`, `GetItemId`, und `this[int]` Methoden. Weitere Informationen zu diesen methodenüberschreibungen, finden Sie unter [implementieren eine ListAdapter](~/android/user-interface/layouts/list-view/populating.md). Die `GetView` Methode gibt eine Ansicht für jede Zeile mit Daten aufgefüllt und wird im folgenden Codebeispiel dargestellt:

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

Die `GetView` aufgerufen, um die Zelle gerendert werden muss, zurückzugeben als ein `View`, für jede Zeile der Daten in der Liste. Erstellt eine `View` Instanz für jede Zeile der Daten, die auf dem Bildschirm, mit die Darstellung der angezeigt werden der `View` Instanz in eine Layoutdatei definiert wird. Wenn eine Zelle auf dem Bildschirm eines Bildlaufs durch geschlossen wird, wird die Zelle für die Wiederverwendung verfügbar gestellt. Dies vermeidet verschwenden Arbeitsspeicher Leuchten, die nur `View` Instanzen für die Daten auf dem Bildschirm, statt alle Daten in der Liste angezeigt wird. Weitere Informationen zu den Ansicht-Wiederverwendung, finden Sie unter [Zeile View RE-use](~/android/user-interface/layouts/list-view/populating.md).

Die `GetView` Methode füllt ebenfalls das `View` -Instanz mit Daten, einschließlich der beim Lesen der Bilddaten aus dem Dateinamen im angegebenen die `ImageFilename` Eigenschaft.

Das Layout der jede Zelle Dispayed durch die Native `ListView` wird definiert, der `NativeAndroidListViewCell.axml` die Layoutdatei, die von aufgebläht ist die `LayoutInflater.Inflate` Methode. Im folgenden Codebeispiel wird veranschaulicht, die Layout-Definition:

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

Dieses Layout gibt an, dass zwei `TextView` Steuerelemente und ein `ImageView` Steuerelement werden verwendet, um den Inhalt der Zelle angezeigt. Die beiden `TextView` Steuerelemente sind vertikal ausgerichtet ist, in eine `LinearLayout` Steuerelement alle Steuerelemente, die eigenständig sind innerhalb einer `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf die Änderung einer Eigenschaft des benutzerdefinierten Steuerelements

Wenn die `NativeListView.Items` Eigenschaft geändert wird, aufgrund der Elemente, die hinzugefügt wird, oder aus der Liste entfernt, der benutzerdefinierte Renderer reagieren, indem Sie die Änderungen angezeigt werden soll. Dies kann erreicht werden, durch Überschreiben der `OnElementPropertyChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Die Methode erstellt eine neue Instanz der der `NativeAndroidListViewAdapter` -Klasse, die Daten, das dem systemeigenen bereitstellt `ListView` steuern, bereitgestellt, die bindbare `NativeListView.Items` -Eigenschaft geändert hat.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen benutzerdefinierten Renderer auf UWP

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für UWP:

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

Das systemeigene `ListView` Steuerelement ist so konfiguriert werden, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Diese Konfiguration umfasst das wie das systemeigene `ListView` Steuerelement reagiert auf Elemente, die ausgewählt wird, füllen die Daten vom Steuerelement angezeigt wird, definieren die Darstellung und den Inhalt jeder Zelle, und registrieren einen Ereignishandler zum Verarbeiten der `SelectionChanged` Ereignis. Dieser Handler wird aufgerufen, wiederum die `ItemSelected` Ereignis bereitgestellt werden, indem der `NativeListView` benutzerdefiniertes Steuerelement. Die `SelectionChanged` Ereignis ist abbestellt, wenn Änderungen der Xamarin.Forms-Element der Renderer angefügt ist.

Die Darstellung und Inhalt der einzelnen systemeigenen `ListView` Zelle definieren, indem eine `DataTemplate` mit dem Namen `ListViewItemTemplate`. Dies `DataTemplate` befindet sich im Ressourcenverzeichnis auf Anwendungsebene und ist im folgenden Codebeispiel gezeigt:

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

Die `DataTemplate` gibt an, die Steuerelemente verwendet, um den Inhalt der Zelle, und das Layout und die Darstellung anzuzeigen. Zwei `TextBlock` Steuerelemente und ein `Image` Steuerelement zur Anzeige von den Inhalt der Zelle mithilfe der Datenbindung verwendet werden. Darüber hinaus eine Instanz von der `ConcatImageExtensionConverter` wird verwendet, um das Verketten der `.jpg` Dateierweiterung jedes Image-Dateinamen. Dadurch wird sichergestellt, dass die `Image` Steuerelement geladen und der Abbildung dargestellt, bei `Source` festgelegt wird.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf die Änderung einer Eigenschaft des benutzerdefinierten Steuerelements

Wenn die `NativeListView.Items` Eigenschaft geändert wird, aufgrund der Elemente, die hinzugefügt wird, oder aus der Liste entfernt, der benutzerdefinierte Renderer reagieren, indem Sie die Änderungen angezeigt werden soll. Dies kann erreicht werden, durch Überschreiben der `OnElementPropertyChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

Die Methode füllt erneut das systemeigene `ListView` Steuerelement mit den geänderten Daten bereitgestellt, die bindbare `NativeListView.Items` -Eigenschaft geändert hat.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie einen benutzerdefinierten Renderer, der plattformspezifische Listensteuerelementen und native Zelle Layouts, kapselt ermöglicht mehr Kontrolle über die Leistungsfähigkeit der native Steuerelement zu erstellen.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererListView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
