---
title: Anpassen einer ListView
description: "Eine Xamarin.Forms ListView ist eine Sicht, die eine Auflistung von Daten als einer vertikalen Liste angezeigt wird. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer, der plattformspezifischen List-Steuerelemente und systemeigene Zelle Layouts kapselt mehr Kontrolle über die einheitlichen Liste Steuerelement Leistung ermöglicht."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 722bedd039a53d1244972ac9f0b98d87cc5d386a
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="customizing-a-listview"></a>Anpassen einer ListView

_Eine Xamarin.Forms ListView ist eine Sicht, die eine Auflistung von Daten als einer vertikalen Liste angezeigt wird. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer, der plattformspezifischen List-Steuerelemente und systemeigene Zelle Layouts kapselt mehr Kontrolle über die einheitlichen Liste Steuerelement Leistung ermöglicht._

Jede Ansicht Xamarin.Forms verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) einer Xamarin.Forms-Anwendung in iOS gerendert wird die `ListViewRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `UITableView` Steuerelement. Auf der Android-Plattform die `ListViewRenderer` Klasse instanziiert ein systemeigenes `ListView` Steuerelement. Auf Windows Phone und die universelle Windows-Plattform (UWP) die `ListViewRenderer` Klasse instanziiert ein systemeigenes `ListView` Steuerelement. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement und den entsprechenden systemeigenen Steuerelementen, die ihn implementieren:

![](listview-images/listview-classes.png "Beziehung zwischen ListView-Steuerelement und den implementierenden systemeigenen Steuerelementen")

Des Renderingprozesses kann Vorteil ausgeführt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_ListView_Control) ein benutzerdefiniertes Steuerelement mit Xamarin.Forms.
1. [Nutzen](#Consuming_the_Custom_Control) das benutzerdefinierte Steuerelement aus Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das Steuerelement auf jeder Plattform.

Jedes Element wird nun wiederum zum Implementieren der besprochen werden eine `NativeListView` Renderer an, der plattformspezifischen List-Steuerelemente und systemeigene Zelle Layouts nutzt. Dieses Szenario ist hilfreich, wenn Portieren eine vorhandene systemeigene app, die Liste enthält und die Zelle Code, der nicht erneut verwendet werden kann. Darüber hinaus können sie detaillierte Anpassung von Listen-Steuerelement-Funktionen, die Leistung zu erzielen, wie z. B. Data Virtualization beeinflussen können.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Erstellen von benutzerdefinierten ListView-Steuerelement

Eine benutzerdefinierte [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Steuerelement erstellt werden, indem die Erstellung von Unterklassen von der `ListView` Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die `NativeListView` wird im Projekt portablen Klassenbibliothek (PCL) erstellt und definiert die API für das benutzerdefinierte Steuerelement. Dieses Steuerelement macht eine `Items` -Eigenschaft, die zum Auffüllen verwendet wird die `ListView` mit Daten und die sein können datengebundene für Anzeigezwecke. Macht auch ein `ItemSelected` Ereignis, das ausgelöst wird, wenn ein Element in eine plattformspezifische systemeigene Strukturelement-Steuerelement ausgewählt ist. Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen das benutzerdefinierte Steuerelement

Die `NativeListView` benutzerdefiniertes Steuerelement im verwiesen werden kann in Xaml PCL-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix für das Steuerelement. Im folgenden Codebeispiel wird veranschaulicht wie die `NativeListView` benutzerdefiniertes Steuerelement genutzt werden kann, durch die entsprechende Verwendung von XAML-Seite:

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

Die `local` nichts Namespacepräfix kann benannt werden. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details des benutzerdefinierten Steuerelements übereinstimmen. Nach der Namespace deklariert ist, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

Im folgenden Codebeispiel wird veranschaulicht wie die `NativeListView` benutzerdefiniertes Steuerelement genutzt werden kann, um eine C#-Seite:

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

Die `NativeListView` benutzerdefiniertes Steuerelement verwendet plattformspezifischen benutzerdefinierten Renderer, um eine Liste der Daten anzuzeigen, wodurch die über aufgefüllt wird die `Items` Eigenschaft. Jede Zeile in der Liste enthält drei Elemente von Daten – einen Namen, eine Kategorie und einen Bilddateinamen. Das Layout der jede Zeile in der Liste wird vom benutzerdefinierten Renderer plattformspezifischen definiert.

> [!NOTE]
> Da die `NativeListView` benutzerdefiniertes Steuerelement wird mit plattformspezifischen List-Steuerelemente, die enthalten, Durchführen eines Bildlaufs Möglichkeit gerendert werden, das benutzerdefinierte Steuerelement sollte nicht gehostet werden in bildlauffähigen Layout-Steuerelemente wie z. B. die [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/).

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt zum Erstellen von plattformspezifische List-Steuerelemente und systemeigene Zelle Layouts hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `ListViewRenderer` -Klasse, die das benutzerdefinierte Steuerelement gerendert wird.
1. Überschreiben Sie die `OnElementChanged` -Methode, die benutzerdefinierte Logik für Steuerelement und schreiben, um ihn anpassen rendert. Diese Methode wird aufgerufen, wenn die entsprechende Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird erstellt.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des benutzerdefinierten Steuerelements mit Xamarin.Forms verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> **Hinweis**: ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Zelle Basisklasse verwendet werden.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](listview-images/solution-structure.png "NativeListView benutzerdefinierten Renderer Projekt Zuständigkeiten")

Die `NativeListView` plattformspezifischen Renderingklassen, die alle abgeleitet benutzerdefiniertes Steuerelement gerendert wird die `ListViewRenderer` Klasse für jede Plattform. Dies führt in den einzelnen `NativeListView` benutzerdefiniertes Steuerelement mit plattformspezifischen List-Steuerelemente und systemeigene Zelle Layouts gerendert werden, wie in den folgenden Screenshots dargestellt:

![](listview-images/screenshots.png "NativeListView auf jeder Plattform")

Die `ListViewRenderer` -Klasse verfügbar macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das benutzerdefinierte Steuerelement mit Xamarin.Forms erstellt wird, um das entsprechende systemeigene Steuerelement rendern. Diese Methode nimmt ein `ElementChangedEventArgs` Parameter, die enthält `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, den Renderer *ist* angefügt sind, bzw. In der beispielanwendung der `OldElement` -Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `NativeListView` Instanz.

Eine überschriebene Version von den `OnElementChanged` Methode in jeder Rendererklasse plattformspezifischen bietet die Möglichkeit zum Durchführen der systemeigenen-Steuerelement. Durch ein typisierter Verweis auf das systemeigene Steuerelement verwendet wird, auf der Plattform zugegriffen werden kann die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

Muss darauf geachtet werden beim Abonnieren von Ereignishandlern in der `OnElementChanged` Methode, wie im folgenden Codebeispiel gezeigt:

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

Das systemeigene Steuerelement sollte nur dann konfiguriert werden und Ereignishandler abonniert, wenn benutzerdefinierte Renderer auf ein neues Xamarin.Forms-Element zugeordnet ist. Ebenso sollten alle Ereignishandler, die abonniert wurden gekündigt werden nur, wenn das Element den Renderer an Änderungen angefügt ist. Dieser Ansatz hilft ein benutzerdefiniertes Renderers erstellen, das von Arbeitsspeicherverlusten nicht beeinträchtigt werden.

Eine überschriebene Version von den `OnElementPropertyChanged` Methode in jeder Rendererklasse plattformspezifischen bietet die Möglichkeit zum Reagieren auf bindbare eigenschaftenänderungen auf das benutzerdefinierte Steuerelement mit Xamarin.Forms. Ein Kontrollkästchen für die Eigenschaft, die geändert werden sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: der Typname, der das benutzerdefinierte Xamarin.Forms-Steuerelement gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

In den folgenden Abschnitten erläutern die Implementierung der einzelnen benutzerdefinierten Renderers Clientplattform-spezifische Klassen.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

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

Die `UITableView` Steuerelement ist so konfiguriert, durch das Erstellen einer Instanz von der `NativeiOSListViewSource` -Klasse, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Diese Klasse stellt Daten für die `UITableView` Steuerelement durch Überschreiben der `RowsInSection` und `GetCell` Methoden aus der `UITableViewSource` Klasse und das Verfügbarmachen von von einer `Items` Eigenschaft, die die Liste der anzuzeigenden Daten enthält. Die Klasse bietet auch eine `RowSelected` Überschreibung der Ruft die `ItemSelected` gebotenen Ereignis der `NativeListView` benutzerdefiniertes Steuerelement. Weitere Informationen über die Methode überschreibt, finden Sie unter [Unterklassen UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Die `GetCell` Methode gibt ein `UITableCellView` , die mit Daten für jede Zeile in der Liste aufgefüllt wird, und wird im folgenden Codebeispiel gezeigt:

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

Diese Methode erstellt eine `NativeiOSListViewCell` Instanz für jede Zeile der Daten, die auf dem Bildschirm angezeigt werden. Die `NativeiOSCell` Instanz definiert das Layout der einzelnen Zellen und Daten der Zelle. Wenn eine Zelle im Bildschirm aufgrund Durchführen eines Bildlaufs geschlossen wird, wird die Zelle für die Wiederverwendung verfügbar gemacht werden. Dadurch wird vermieden, Verschwendung Arbeitsspeicher Leuchten, die es nur sind `NativeiOSCell` Instanzen für die Daten auf dem Bildschirm, statt alle Daten in der Liste angezeigt wird. Weitere Informationen zu der Zelle Wiederverwendung, finden Sie unter [Zelle wiederverwenden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Die `GetCell` Methode auch liest die `ImageFilename` Eigenschaft jeder Zeile der Daten, sofern noch vorhanden ist, und das Bild liest und speichert es als ein `UIImage` -Instanz, vor dem Aktualisieren der `NativeiOSListViewCell` -Instanz mit den Daten (Name, Kategorie und Image) für die Zeile.

Die `NativeiOSListViewCell` Klasse definiert das Layout für jede Zelle, und wird im folgenden Codebeispiel gezeigt:

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

Diese Klasse definiert die Steuerelemente verwendet, um den Inhalt der Zelle, und ihr Layout zu rendern. Die `NativeiOSListViewCell` Konstruktor erstellt Instanzen von `UILabel` und `UIImageView` steuert und ihre Darstellung initialisiert. Diese Steuerelemente werden zum Anzeigen von Daten für jede Zeile, mit der `UpdateCell` Methode verwendet wird, um diese Daten für Festlegen der `UILabel` und `UIImageView` Instanzen. Der Speicherort dieser Instanzen festgelegt, wird von der überschriebenen `LayoutSubviews` -Methode, von ihren Koordinaten in der Zelle angibt.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf die Änderung einer Eigenschaft für das benutzerdefinierte Steuerelement

Wenn die `NativeListView.Items` Eigenschaft geändert wird, aufgrund Elemente hinzugefügt oder aus der Liste entfernt, die benutzerdefinierten Renderers reagieren, indem Sie die Änderungen anzeigen muss. Dies geschieht durch Überschreiben der `OnElementPropertyChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Die Methode erstellt eine neue Instanz der dem `NativeiOSListViewSource` -Klasse, die Daten bereitstellt der `UITableView` steuern, sofern die bindungsfähigen `NativeListView.Items` -Eigenschaft geändert hat.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Das folgende Codebeispiel zeigt die benutzerdefinierten Renderers für das Android-Plattform:

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

Das systemeigene `ListView` Steuerelement ist so konfiguriert werden, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Diese Konfiguration umfasst das Erstellen einer Instanz von der `NativeAndroidListViewAdapter` Klasse, die Daten, das dem systemeigenen bereitstellt `ListView` steuern sowie das Registrieren eines ereignishandlers zum Verarbeiten der `ItemClick` Ereignis. Dagegen wird dieser Handler aufgerufen der `ItemSelected` gebotenen Ereignis der `NativeListView` benutzerdefiniertes Steuerelement. Die `ItemClick` Ereignis ist abbestellt, wenn das Xamarin.Forms-Element der Renderer Änderungen angefügt ist.

Die `NativeAndroidListViewAdapter` leitet sich von der `BaseAdapter` Klasse und stellt eine `Items` Eigenschaft, die die Liste der anzuzeigenden Daten enthält, als auch überschreiben die `Count`, `GetView`, `GetItemId`, und `this[int]` Methoden. Weitere Informationen zu dieser Methode überschreibt, finden Sie unter [implementieren eine ListAdapter](~/android/user-interface/layouts/list-view/populating.md). Die `GetView` Methode gibt eine Ansicht für jede Zeile mit Daten aufgefüllt und wird im folgenden Codebeispiel gezeigt:

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

Die `GetView` Methode wird aufgerufen, um die Zelle gerendert werden soll, zurückzugeben als ein `View`, für jede Zeile der Daten in der Liste. Erstellt eine `View` Instanz für jede Zeile der Daten, die auf dem Bildschirm, mit der Darstellung der angezeigt werden der `View` Instanz in eine Layoutdatei definiert wird. Wenn eine Zelle im Bildschirm aufgrund Durchführen eines Bildlaufs geschlossen wird, wird die Zelle für die Wiederverwendung verfügbar gemacht werden. Dadurch wird vermieden, Verschwendung Arbeitsspeicher Leuchten, die es nur sind `View` Instanzen für die Daten auf dem Bildschirm, statt alle Daten in der Liste angezeigt wird. Weitere Informationen zur Wiederverwendung Ansicht finden Sie unter [Zeile Sicht erneut verwenden](~/android/user-interface/layouts/list-view/populating.md).

Die `GetView` Methode auch füllt die `View` -Instanz mit Daten, einschließlich des Lesens der Image-Daten aus dem angegebenen Dateinamen der `ImageFilename` Eigenschaft.

Das Layout der jeder Zelle Dispayed von systemeigenen `ListView` wird definiert, der `NativeAndroidListViewCell.axml` Layoutdatei, die vom vergrößert werden die `LayoutInflater.Inflate` Methode. Das folgende Codebeispiel zeigt die Layoutdefinition:

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

Dieses Layout gibt an, dass zwei `TextView` Steuerelemente und ein `ImageView` Steuerelement werden verwendet, um den Inhalt der Zelle angezeigt. Die beiden `TextView` Steuerelemente sind vertikal ausgerichtet, innerhalb einer `LinearLayout` Steuerelement alle Steuerelemente, die eigenständig sind innerhalb einer `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf die Änderung einer Eigenschaft für das benutzerdefinierte Steuerelement

Wenn die `NativeListView.Items` Eigenschaft geändert wird, aufgrund Elemente hinzugefügt oder aus der Liste entfernt, die benutzerdefinierten Renderers reagieren, indem Sie die Änderungen anzeigen muss. Dies geschieht durch Überschreiben der `OnElementPropertyChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (Forms.Context as Android.App.Activity, Element as NativeListView);
  }
}
```

Die Methode erstellt eine neue Instanz der dem `NativeAndroidListViewAdapter` Klasse, die Daten, das dem systemeigenen bereitstellt `ListView` steuern, sofern die bindungsfähigen `NativeListView.Items` -Eigenschaft geändert hat.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Erstellen von benutzerdefinierten Renderers für Windows Phone und universelle Windows-Plattform

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für Windows Phone-und uwp:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeWinPhoneListViewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class NativeWinPhoneListViewRenderer : ListViewRenderer
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

Das systemeigene `ListView` Steuerelement ist so konfiguriert werden, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Diese Konfiguration umfasst die Einstellung wie das systemeigene `ListView` Steuerelement reagiert auf Elemente, die ausgewählt wird, füllen die Daten im Steuerelement angezeigt wird, definieren die Darstellung und den Inhalt jeder Zelle, und registrieren Sie einen Ereignishandler zum Verarbeiten der `SelectionChanged` Ereignis. Dagegen wird dieser Handler aufgerufen der `ItemSelected` gebotenen Ereignis der `NativeListView` benutzerdefiniertes Steuerelement. Die `SelectionChanged` Ereignis ist abbestellt, wenn das Xamarin.Forms-Element der Renderer Änderungen angefügt ist.

Die Darstellung und Inhalt der einzelnen systemeigenen `ListView` Zelle werden definiert, indem eine `DataTemplate` mit dem Namen `ListViewItemTemplate`. Dies `DataTemplate` wird in der Anwendungsebene Ressourcenwörterbuch gespeichert und wird im folgenden Codebeispiel gezeigt:

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

Die `DataTemplate` bestimmt die Steuerelemente verwendet, um den Inhalt der Zelle, und das Layout und die Darstellung anzuzeigen. Zwei `TextBlock` Steuerelemente und ein `Image` -Steuerelement zum Anzeigen von Inhalt der Zelle durch die Datenbindung verwendet werden. Darüber hinaus eine Instanz von der `ConcatImageExtensionConverter` dient zum Verketten der `.jpg` Dateierweiterung auf jedes Bilddateinamen. Dadurch wird sichergestellt, dass die `Image` Steuerelement laden und das Bild zu rendern, wenn er sich `Source` festgelegt wird.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagieren auf die Änderung einer Eigenschaft für das benutzerdefinierte Steuerelement

Wenn die `NativeListView.Items` Eigenschaft geändert wird, aufgrund Elemente hinzugefügt oder aus der Liste entfernt, die benutzerdefinierten Renderers reagieren, indem Sie die Änderungen anzeigen muss. Dies geschieht durch Überschreiben der `OnElementPropertyChanged` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

Die Methode erneut füllt das systemeigene `ListView` Steuerelement mit der geänderten Daten bereitgestellt, die die bindungsfähigen `NativeListView.Items` -Eigenschaft geändert hat.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers, das plattformspezifischen List-Steuerelemente und systemeigene Zelle Layouts kapselt mehr Kontrolle über die einheitlichen Liste Steuerelement Leistung ermöglicht.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererListView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
