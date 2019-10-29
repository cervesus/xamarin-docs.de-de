---
title: Verwenden von xamarin. Android Designer
description: Dieser Artikel ist eine exemplarische Vorgehensweise der xamarin. Android Designer. Es wird veranschaulicht, wie eine Benutzeroberfläche für eine kleine Farb Browser-App erstellt wird. Diese Benutzeroberfläche wird vollständig im Designer erstellt.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: df83bdfcc847b07754a349060c9be1613efd0b08
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029521"
---
# <a name="using-the-xamarinandroid-designer"></a>Verwenden von xamarin. Android Designer

_Dieser Artikel ist eine exemplarische Vorgehensweise der xamarin. Android Designer. Es wird veranschaulicht, wie eine Benutzeroberfläche für eine kleine Farb Browser-App erstellt wird. Diese Benutzeroberfläche wird vollständig im Designer erstellt._

## <a name="overview"></a>Übersicht

Android-Benutzeroberflächen können deklarativ mithilfe von XML-Dateien oder Programm gesteuert durch Schreiben von Code erstellt werden. Die xamarin. Android Designer ermöglicht Entwicklern, deklarative Layouts visuell zu erstellen und zu ändern, ohne dass eine Hand Bearbeitung von XML-Dateien erforderlich ist. Der Designer bietet auch Echtzeitfeedback, mit dem der Entwickler Änderungen an der Benutzeroberfläche auswerten kann, ohne die Anwendung auf einem Gerät oder einem Emulator erneut bereitstellen zu müssen. Diese Designer Features können die Entwicklung von Android-UI enorm beschleunigen.
In diesem Artikel wird veranschaulicht, wie Sie xamarin. Android Designer verwenden, um eine Benutzeroberfläche visuell zu erstellen.

> [!TIP]
> Neuere Releases von Visual Studio unterstützen das Öffnen von XML-Dateien in Android Designer.
>
> Sowohl AXML- als auch XML-Dateien werden in Android Designer unterstützt.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Das Ziel dieser exemplarischen Vorgehensweise besteht darin, die Android Designer zum Erstellen einer Benutzeroberfläche für eine Beispiel-Farb Browser-APP zu verwenden. Die Farb Browser-APP zeigt eine Liste mit Farben, Ihren Namen und ihren RGB-Werten an. Sie erfahren, wie Sie Widgets zum **Designoberfläche** hinzufügen und wie Sie diese Widgets visuell aufstellen. Anschließend erfahren Sie, wie Sie Widgets interaktiv auf dem **Designoberfläche** oder im **Eigenschaften** Bereich des Designers ändern. Schließlich sehen Sie, wie der Entwurf aussieht, wenn die APP auf einem Gerät oder Emulator ausgeführt wird.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt besteht darin, ein neues xamarin. Android-Projekt zu erstellen. Starten Sie Visual Studio, klicken Sie auf **Neues Projekt...** , und wählen Sie die Vorlage **Visual C\# > Android-> Android-App (xamarin)** aus.
Benennen Sie die neue APP **designerwalkthrough** , und klicken Sie auf **OK**.

[leere Android-App![](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

Wählen Sie im Dialogfeld **neue Android-App** **leere App** aus, und klicken Sie auf **OK**:

[![auswählen der Vorlage für eine leere Android-App](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)

### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Der nächste Schritt besteht darin, ein **LinearLayout** zu erstellen, das die Elemente der Benutzeroberfläche enthält. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Ressourcen/Layout** , und wählen Sie **> Neues Element hinzufügen...** aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Android-Layout**aus. Benennen Sie die Datei **list_item** und klicken Sie auf **Hinzufügen**:

[Neues Layout![](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

Das neue **list_item** -Layout wird im-Designer angezeigt. Beachten Sie, dass zwei Bereiche angezeigt werden, &ndash; die *Designoberfläche* für das **list_item** im linken Bereich sichtbar ist, während die zugehörige XML-Quelle im rechten Bereich angezeigt wird. Sie können die Positionen der Bereiche **Designoberfläche** und **Quelle** austauschen, indem Sie auf das Symbol Bereiche **austauschen** klicken, das sich zwischen den beiden Bereichen befindet:

[![-Designer-Ansicht](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

Klicken Sie im Menü **Ansicht** auf **Weitere Fenster > Dokument** Gliederung, um die **Dokument**Gliederung zu öffnen. Die **Dokument** Gliederung zeigt, dass das Layout derzeit ein einzelnes **linearlayoutwidget** enthält:

[Dokument Gliederung![](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

Der nächste Schritt besteht darin, die Benutzeroberfläche für die Farb Browser-APP innerhalb dieses `LinearLayout`zu erstellen.

### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche für das Listenelement

Wenn der Bereich **Toolbox** nicht angezeigt wird, klicken Sie auf der linken Seite auf die Registerkarte **Toolbox** . Führen Sie in der **Toolbox**einen Bildlauf nach unten bis zum **Bild & Medien** Abschnitt durch, und Scrollen Sie nach unten, bis Sie eine `ImageView`finden:

[![nach ImageView suchen](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

Alternativ können Sie *ImageView* in die Suchleiste eingeben, um die `ImageView`zu finden:

[![der ImageView-Suche](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

Ziehen Sie diese `ImageView` auf die Designoberfläche (diese `ImageView` wird verwendet, um ein Farbmuster in der Farb Browser-App anzuzeigen):

[![von ImageView in Canvas](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

Ziehen Sie als nächstes ein `LinearLayout (Vertical)` widget aus der **Toolbox** in den Designer. Beachten Sie, dass ein blauer Umriss die Grenzen des hinzugefügten `LinearLayout`angibt. Die **Dokument** Gliederung zeigt, dass es sich um ein untergeordnetes Element von `LinearLayout`unter `imageView1 (ImageView)`handelt:

[![blaue Gliederung](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

Wenn Sie die `ImageView` im Designer auswählen, wird der blaue Umriss zum Umschließen der `ImageView`verschoben. Außerdem wird die Auswahl auf `imageView1 (ImageView)` in der **Dokument**Gliederung verschoben:

[![auswählen von "ImageView"](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

Ziehen Sie als nächstes ein `Text (Large)` widget aus der **Toolbox** in das neu hinzugefügte `LinearLayout`. Beachten Sie, dass der Designer grüne Highlights verwendet, um anzugeben, wo das neue Widget eingefügt wird:

[grüne Highlights![](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

Fügen Sie als nächstes ein `Text (Small)`-Widget unterhalb des `Text (Large)` Widgets hinzu:

[![kleines textwidget hinzufügen](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

An diesem Punkt sollte die Designer Oberfläche dem folgenden Screenshot ähneln:

[Layout des![Designers](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

Wenn sich die beiden `textView` Widgets nicht in `linearLayout1`befinden, können Sie Sie auf `linearLayout1` in der **Dokument** Gliederung ziehen und positionieren, damit Sie wie im vorherigen Screenshot (eingerückt unter `linearLayout1`) angezeigt werden.

### <a name="arranging-the-user-interface"></a>Anordnen der Benutzeroberfläche

Der nächste Schritt besteht darin, die Benutzeroberfläche so zu ändern, dass die `ImageView` auf der linken Seite angezeigt wird, wobei die beiden `TextView` Widgets auf der rechten Seite des `ImageView`gestapelt sind.

1. Wählen Sie das `ImageView`-Steuerelement aus.

2. Geben Sie im **Eigenschaftenfenster**im Suchfeld *Width* ein, und suchen Sie die **Layoutbreite**.

3. Ändern Sie die Einstellung **Layout Width** in `wrap_content`:

![Wrap-Inhalt festlegen](designer-walkthrough-images/vs/15-wrap-content-w158.png)

Eine andere Möglichkeit, die `Width` Einstellung zu ändern, besteht darin, auf das Dreieck auf der rechten Seite des Widgets zu klicken, um die Breite auf `wrap_content`zu ändern:

![Zum Festlegen der Breite ziehen](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

Wenn Sie erneut auf das Dreieck klicken, wird die `Width` Einstellung `match_parent`. Navigieren Sie als nächstes zum Bereich **Dokument** Gliederung, und wählen Sie den Stamm `LinearLayout`aus:

[!["root-LinearLayout" auswählen](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

Nachdem Sie die Stamm `LinearLayout` ausgewählt haben, kehren Sie zum Bereich **Eigenschaften** zurück, geben Sie die *Ausrichtung* in das Suchfeld ein, und suchen Sie nach der Einstellung **Ausrichtung** . Ändern Sie die **Ausrichtung** in `horizontal`:

![Horizontale Ausrichtung auswählen](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

An diesem Punkt sollte die Designer Oberfläche dem folgenden Screenshot ähneln.
Beachten Sie, dass die `TextView` Widgets nach rechts vom `ImageView`verschoben wurden:

[Layout des![Designers](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>Ändern des Abstands

Der nächste Schritt besteht darin, die Auffüll-und Rand Einstellungen in der Benutzeroberfläche zu ändern, um zwischen den Widgets mehr Platz zu schaffen. Wählen Sie die `ImageView` auf der Entwurfs Oberfläche aus. Geben Sie im **Eigenschaften** Bereich `min` in das Suchfeld ein. Geben Sie `70dp` für die **Mindesthöhe** und `50dp` für die **minimale Breite**ein:

[![Höhe und-Breite festlegen](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

Geben Sie im **Eigenschaften** Bereich `padding` in das Suchfeld ein, und geben Sie `10dp` für die Auffüll Vorgänge **ein.** Mit diesen Einstellungen für `minHeight`, `minWidth` und `padding` werden alle Seiten der `ImageView` Auffüll Zeichen hinzugefügt und vertikal gestreckt. Beachten Sie, dass sich die Layout-XML ändert, wenn Sie diese Werte eingeben:

[Auffüll![festlegen](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

Die unteren, linken, rechten und oberen Leerraum Einstellungen können unabhängig voneinander festgelegt werden, indem Sie Werte in den Leerraum **unten**, den Abstand **Links**, den Leerraum **Rechts**und die oberen Felder der **oberen** Felder eingeben.
Legen Sie z. b. das **linke** Feld für die Auffüll Zeichen auf `5dp` fest, und legen Sie die Felder **unten**, Auffüll **Rechts**und Auffüllen der **oberen** Felder auf `10dp`fest:

[![benutzerdefinierte Auffüll Einstellungen](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

Passen Sie als nächstes die Position des `LinearLayout` Widgets an, das die beiden `TextView` Widgets enthält. Wählen Sie in der **Dokument**Gliederung `linearLayout1`aus. Geben Sie im Fenster **Eigenschaften** im Suchfeld `margin` ein. Legen Sie **layoutrand unten**, **layoutrand Links**und **layoutrand oben** auf `5dp`fest. Legen Sie **layoutrand rechts** auf `0dp`fest:

[![festgelegte Ränder](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>Entfernen des Standard Bilds

Da der `ImageView` zum Anzeigen von Farben (anstelle von Bildern) verwendet wird, besteht der nächste Schritt darin, die von der Vorlage hinzugefügte Standardbild Quelle zu entfernen.

1. Wählen Sie die `ImageView` auf der **Designer Oberfläche**aus.

2. Geben Sie im Suchfeld im Feld **Eigenschaften**den Wert *src* ein.

3. Klicken Sie auf das kleine Quadrat rechts neben der Eigenschaften Einstellung **src** , und wählen Sie **Zurücksetzen**aus:

[![die Einstellung "Image View src" Löschen](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

Hierdurch werden `android:src="@android:drawable/ic_menu_gallery"` aus der Quell-XML für diese `ImageView`entfernt.

### <a name="adding-a-listview-container"></a>Hinzufügen eines ListView-Containers

Nachdem das **list_item** -Layout definiert ist, besteht der nächste Schritt darin, dem Hauptlayout eine `ListView` hinzuzufügen. Diese `ListView` enthält eine Liste von **list_item**. 

Öffnen Siein der Projektmappen-Explorer **Ressourcen/Layout/activity_main. axml**. Suchen Sie in der **Toolbox**das `ListView`-Widget, und ziehen Sie es auf den **Designoberfläche**. Der `ListView` im Designer ist leer, außer für blaue Linien, die den Rahmen bei der Auswahl gliedern. Sie können die **Dokument** Gliederung anzeigen, um zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

[neue ListView![](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

Standardmäßig wird dem `ListView` ein `Id` Wert von `@+id/listView1`zugewiesen.
Wenn `listView1` in der **Dokument**Gliederung noch immer ausgewählt ist, öffnen Sie den Bereich **Eigenschaften** , klicken Sie auf **Anordnen nach**, und wählen Sie **Kategorie**aus.
Öffnen Sie **Main**, suchen Sie die **ID** -Eigenschaft, und ändern Sie Ihren Wert in `@+id/myListView`:

[![ID in myListView umbenennen](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

An diesem Punkt kann die Benutzeroberfläche verwendet werden.

### <a name="running-the-application"></a>Ausführen der Anwendung

Öffnen Sie **MainActivity.cs** , und ersetzen Sie den Code durch Folgendes:

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using Android.Support.V7.App;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
        }
    }

    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

In diesem Code wird ein benutzerdefinierter `ListView` Adapter zum Laden von Farbinformationen und zum Anzeigen dieser Daten in der soeben erstellten Benutzeroberfläche verwendet. Um dieses Beispiel kurz zu halten, sind die Farbinformationen in einer Liste hart codiert, der Adapter kann jedoch so geändert werden, dass er Farbinformationen aus einer Datenquelle extrahiert oder Sie im laufenden Betrieb berechnet. Weitere Informationen zu `ListView` Adaptern finden Sie unter [ListView](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Der folgende Screenshot zeigt ein Beispiel für die Anzeige der APP, wenn Sie auf einem Gerät ausgeführt wird:

[![abschließende Bildschirm Abbildung](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt besteht darin, ein neues xamarin. Android-Projekt zu erstellen.

Starten Sie Visual Studio für Mac, und klicken Sie auf **Neues Projekt...** . Wählen Sie die Vorlage **Android-App** , und klicken Sie auf **weiter**:

[leere Android-App![](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

Benennen Sie die neue APP **designerwalkthrough**. Wählen Sie unter **Zielplattformen**die Option **neueste und höchste** aus, und klicken Sie auf **weiter**:

[App mit![Namen](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

Klicken Sie im nächsten Dialogfeld auf **Erstellen**.

### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Der nächste Schritt besteht darin, ein **LinearLayout** zu erstellen, das die Elemente der Benutzeroberfläche enthält.

Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Ressourcen/Layout** im **lösungspad** , und wählen Sie **> neue Datei hinzufügen...** aus. Wählen Sie im Dialogfeld **neue Datei** die Option **Android-> Layout**aus. Benennen Sie die Datei **list_item** , und klicken Sie auf **neu**:

[Neues Layout![](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

Nachdem diese Datei hinzugefügt wurde, wird das neue **list_item** -Layout auf dem **Designoberfläche** angezeigt (wenn die Meldung angezeigt wird, *enthält dieses Projektressourcen, die nicht erfolgreich kompiliert wurden, was möglicherweise beeinträchtigt*ist, und klicken Sie auf **Erstellen > Alle erstellen** , um das Projekt zu erstellen):

[![-Designer-Ansicht](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

Klicken Sie unten im Designer auf die Registerkarte **Quelle** , um die XML-Quelle für dieses Layout anzuzeigen. Wenn Sie auf der rechten Seite auf die Registerkarte **Dokument** Gliederung klicken, wird angezeigt, dass das Layout derzeit ein einzelnes **linearlayoutwidget** enthält:

[![-Designer-XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

Der nächste Schritt besteht darin, die Benutzeroberfläche für die Farb Browser-APP zu erstellen.

### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche für das Listenelement

Klicken Sie unten auf dem Bildschirm auf die Registerkarte **Designer** , um zur **Oberfläche des Designers**zurückzukehren. Scrollen Sie im Bereich **Toolbox** auf der rechten Seite nach unten zum Abschnitt **Images & Medium** , und suchen Sie `ImageView`:

[![nach ImageView suchen](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

Alternativ können Sie *ImageView* in die Suchleiste eingeben, um die `ImageView`zu finden:

[![der ImageView-Suche](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

Ziehen Sie diese `ImageView` auf die **Designoberfläche** (diese `ImageView` wird verwendet, um ein Farbmuster in der Farb Browser-App anzuzeigen):

[![von ImageView in Canvas](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

Ziehen Sie als nächstes ein `LinearLayout (Vertical)` widget aus der **Toolbox** in den **Designoberfläche**. Beachten Sie, dass ein blauer Umriss die Grenzen des hinzugefügten `LinearLayout`angibt. Die **Dokument** Gliederung zeigt, dass es sich um ein `imageView1 (ImageView)`untergeordnetes Element von `LinearLayout`handelt.

[![blaue Gliederung](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

Wenn Sie die `ImageView` im Designer auswählen, wird der blaue Umriss zum Umschließen der `ImageView`verschoben. Außerdem wird die Auswahl auf `imageView1 (ImageView)` in der **Dokument**Gliederung verschoben:

[![auswählen von "ImageView"](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

Ziehen Sie als nächstes ein `Text (Large)` widget aus der **Toolbox** in das neu hinzugefügte `LinearLayout`. Beachten Sie, dass beim Ziehen mit der Maus auf die **Designoberfläche**die Position des neuen Widgets hervorgehoben wird.
Das `Text (Large)`-Widget sollte sich in `linearLayout1` befinden, wie hier zu sehen:

[![Ein großes Text-Widget hinzufügen](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

Fügen Sie als nächstes ein `Text (Small)`-Widget unterhalb des `Text (Large)` Widgets hinzu. An diesem Punkt sollte die **Designoberfläche** dem folgenden Screenshot ähneln:

[![kleines textwidget hinzufügen](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

Wenn sich die beiden `textView` Widgets nicht in `linearLayout1`befinden, können Sie Sie auf `linearLayout1` in der **Dokument** Gliederung ziehen und positionieren, damit Sie wie im vorherigen Screenshot (eingerückt unter `linearLayout1`) angezeigt werden.

### <a name="arranging-the-user-interface"></a>Anordnen der Benutzeroberfläche

Der nächste Schritt besteht darin, die Benutzeroberfläche so zu ändern, dass die `ImageView` auf der linken Seite angezeigt wird, wobei die beiden `TextView` Widgets auf der rechten Seite des `ImageView`gestapelt sind.

1. Wenn die `ImageView` ausgewählt ist, klicken Sie auf die Registerkarte **Eigenschaften** .

2. Klicken Sie direkt unterhalb der Registerkarte **Eigenschaften** auf **Layout**.

3. Scrollen Sie nach unten zu **viewgroup** , und ändern Sie die Einstellung `Width` in `wrap_content`:

[![Satz Umbruch Inhalt](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

Eine andere Möglichkeit, die `Width` Einstellung zu ändern, besteht darin, auf das Dreieck auf der rechten Seite des Widgets zu klicken, um die Breite auf `wrap_content`zu ändern:

[zum Festlegen der Breite![ziehen](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

Wenn Sie erneut auf das Dreieck klicken, wird die `Width` Einstellung `match_parent`. Navigieren Sie als nächstes zum Bereich **Dokument** Gliederung, und wählen Sie den Stamm `LinearLayout`aus:

[!["root-LinearLayout" auswählen](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

Wenn die Stamm `LinearLayout` ausgewählt ist, kehren Sie zur Registerkarte **Eigenschaften** zurück, und klicken Sie auf **Widget**. Ändern Sie die `Orientation` Einstellung in `horizontal`, wie unten gezeigt. An diesem Punkt sollte die **Designoberfläche** dem folgenden Screenshot ähneln. Beachten Sie, dass die `TextView` Widgets nach rechts vom `ImageView`verschoben wurden:

[horizontale Ausrichtung![auswählen](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)

### <a name="modifying-the-spacing"></a>Ändern des Abstands

Der nächste Schritt besteht darin, die Auffüll-und Rand Einstellungen in der Benutzeroberfläche zu ändern, um zwischen den Widgets mehr Platz zu schaffen. Wählen Sie die `ImageView` und klicken Sie unter **Eigenschaften**auf die Registerkarte **Layout** . Ändern Sie die `Min Width` in `50dp`, die `Min Height` in `70dp`und die `Padding` `10dp`.
Dadurch wird die Auffüll Linie um alle Seiten des `ImageView` und eine vertikale Zeit Breite erhöht:

[Auffüll![festlegen](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

Die Einstellungen oben, rechts, unten und Links können unabhängig voneinander festgelegt werden, indem Sie Werte in die Felder `Top`, `Right`, `Bottom`und `Left` Auffüllen eingeben. Legen Sie z. b. den Wert für `Left` Füll Zeichen auf `5dp` und die Auffüll Werte `Top`, `Right`und `Bottom` auf `10dp`fest. Beachten Sie, dass sich die `Padding` Einstellung in eine durch Trennzeichen getrennte Liste dieser Werte ändert:

[![benutzerdefinierte Auffüll Einstellungen](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

Passen Sie als nächstes die Position des `LinearLayout` Widgets an, das die beiden `TextView` Widgets enthält. Wählen Sie in der **Dokument**Gliederung `linearLayout1`aus. Wählen Sie im Bereich " **Eigenschaften** " die Registerkarte **Layout** aus. Scrollen Sie nach unten zum Abschnitt **viewgroup** , und legen Sie die `Left`-, `Top`-, `Right`-und `Bottom` Ränder auf `5dp`, `5dp`, `0dp`bzw. `5dp` fest. :

[![festgelegte Ränder](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>Entfernen des Standard Bilds

Da der `ImageView` zum Anzeigen von Farben (anstelle von Bildern) verwendet wird, besteht der nächste Schritt darin, die von der Vorlage hinzugefügte Standardbild Quelle zu entfernen.

1. Wählen Sie das `ImageView`-Steuerelement aus.

2. Klicken Sie unter **Eigenschaften**auf die Registerkarte **Widget** .

3. Löschen Sie die `Src` Einstellung, sodass Sie leer ist:

[![die Einstellung "Image View src" Löschen](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

Hierdurch werden `android:src="@android:drawable/ic_menu_gallery"` aus der Quell-XML für diese `ImageView`entfernt.

### <a name="adding-a-listview-container"></a>Hinzufügen eines ListView-Containers

Nachdem das **list_item** -Layout definiert ist, besteht der nächste Schritt darin, dem Hauptlayout eine `ListView` hinzuzufügen. Diese `ListView` enthält eine Liste von **list_item**. 

Öffnen Sie im **Projektmappen-Explorer** **Ressourcen/Layout/Main. axml**.
Klicken Sie auf das Widget `Button` (sofern vorhanden), und löschen Sie es. Suchen Sie in der **Toolbox**das `ListView`-Widget, und ziehen Sie es auf den **Designoberfläche**.
Der `ListView` im Designer ist leer, außer für blaue Linien, die den Rahmen bei der Auswahl gliedern. Sie können die **Dokument** Gliederung anzeigen, um zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

[neue ListView![](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

Standardmäßig wird dem `ListView` ein `Id` Wert von `@+id/listView1`zugewiesen.
Wenn `listView1` in der **Dokument**Gliederung noch immer ausgewählt ist, öffnen Sie den Bereich **Eigenschaften** , klicken Sie auf **Anordnen nach**, und wählen Sie **Kategorie**aus.
Öffnen Sie **Main**, suchen Sie die **ID** -Eigenschaft, und ändern Sie Ihren Wert in `@+id/myListView`:

[![ID in myListView umbenennen](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

An diesem Punkt kann die Benutzeroberfläche verwendet werden.

### <a name="running-the-application"></a>Ausführen der Anwendung

Öffnen Sie **MainActivity.cs** , und ersetzen Sie den Code durch Folgendes:

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", MainLauncher = true)]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
        }
    }

    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}
```

In diesem Code wird ein benutzerdefinierter `ListView` Adapter zum Laden von Farbinformationen und zum Anzeigen dieser Daten in der soeben erstellten Benutzeroberfläche verwendet. Um dieses Beispiel kurz zu halten, sind die Farbinformationen in einer Liste hart codiert, der Adapter kann jedoch so geändert werden, dass er Farbinformationen aus einer Datenquelle extrahiert oder Sie im laufenden Betrieb berechnet. Weitere Informationen zu `ListView` Adaptern finden Sie unter [ListView](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Der folgende Screenshot zeigt ein Beispiel für die Anzeige der APP, wenn Sie auf einem Gerät ausgeführt wird:

[![abschließende Bildschirm Abbildung](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde der Prozess der Verwendung von xamarin. Android Designer in Visual Studio zum Erstellen einer Benutzeroberfläche für eine einfache APP erläutert.
Es wurde gezeigt, wie Sie die Schnittstelle für ein einzelnes Element in einer Liste erstellen, und es wurde gezeigt, wie Sie Widgets hinzufügen und visuell anordnen.
Außerdem wurde erläutert, wie Ressourcen zugewiesen werden und anschließend verschiedene Eigenschaften für diese Widgets festgelegt werden.
