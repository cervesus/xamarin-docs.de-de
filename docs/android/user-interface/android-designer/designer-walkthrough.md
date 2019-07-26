---
title: Verwenden von xamarin. Android Designer
description: Dieser Artikel ist eine exemplarische Vorgehensweise der xamarin. Android Designer. Es wird veranschaulicht, wie eine Benutzeroberfläche für eine kleine Farb Browser-App erstellt wird. Diese Benutzeroberfläche wird vollständig im Designer erstellt.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 9387b44419af87785d45a25ab254d3361a5615a3
ms.sourcegitcommit: c75c1d2132a4f46a7b38e454d5f24705165026bd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485924"
---
# <a name="using-the-xamarinandroid-designer"></a>Verwenden von xamarin. Android Designer

_Dieser Artikel ist eine exemplarische Vorgehensweise der xamarin. Android Designer. Es wird veranschaulicht, wie eine Benutzeroberfläche für eine kleine Farb Browser-App erstellt wird. Diese Benutzeroberfläche wird vollständig im Designer erstellt._


## <a name="overview"></a>Übersicht

Android-Benutzeroberflächen können deklarativ mithilfe von XML-Dateien oder Programm gesteuert durch Schreiben von Code erstellt werden. Die xamarin. Android Designer ermöglicht Entwicklern, deklarative Layouts visuell zu erstellen und zu ändern, ohne dass eine Hand Bearbeitung von XML-Dateien erforderlich ist. Der Designer bietet auch Echtzeitfeedback, mit dem der Entwickler Änderungen an der Benutzeroberfläche auswerten kann, ohne die Anwendung auf einem Gerät oder einem Emulator erneut bereitstellen zu müssen. Diese Designer Features können die Entwicklung von Android-UI enorm beschleunigen.
In diesem Artikel wird veranschaulicht, wie Sie xamarin. Android Designer verwenden, um eine Benutzeroberfläche visuell zu erstellen.

> [!TIP]
> Neuere Versionen von Visual Studio unterstützen das Öffnen von XML-Dateien innerhalb der Android Designer.
>
> Beide axml-und XML-Dateien werden in der Android Designer unterstützt.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Das Ziel dieser exemplarischen Vorgehensweise besteht darin, die Android Designer zum Erstellen einer Benutzeroberfläche für eine Beispiel-Farb Browser-APP zu verwenden. Die Farb Browser-APP zeigt eine Liste mit Farben, Ihren Namen und ihren RGB-Werten an. Sie erfahren, wie Sie Widgets zum **Designoberfläche** hinzufügen und wie Sie diese Widgets visuell aufstellen. Anschließend erfahren Sie, wie Sie Widgets interaktiv auf dem **Designoberfläche** oder im **Eigenschaften** Bereich des Designers ändern. Schließlich sehen Sie, wie der Entwurf aussieht, wenn die APP auf einem Gerät oder Emulator ausgeführt wird.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt besteht darin, ein neues xamarin. Android-Projekt zu erstellen. Starten Sie Visual Studio, klicken Sie auf **Neues Projekt...** , und wählen Sie die Vorlage **Visual\# C > Android > Android-App (xamarin)** aus.
Benennen Sie die neue APP **designerwalkthrough** , und klicken Sie auf **OK**.

[![Leere Android-App](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

Wählen Sie im Dialogfeld **neue Android-App** **leere App** aus, und klicken Sie auf **OK**:

[![Auswählen der Vorlage für eine leere Android-App](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)


### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Der nächste Schritt besteht darin, ein **LinearLayout** zu erstellen, das die Elemente der Benutzeroberfläche enthält. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Ressourcen/Layout** , und wählen Sie **> Neues Element hinzufügen...** aus. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Android-Layout**aus. Benennen Sie die Datei **list_item** und klicken Sie auf **Hinzufügen**:

[![Neues Layout](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

Das neue **list_item** -Layout wird im-Designer angezeigt. Beachten Sie, dass zwei Bereiche &ndash; angezeigt werden: die *Designoberfläche* für die **list_item** ist im linken Bereich sichtbar, während die zugehörige XML-Quelle im rechten Bereich angezeigt wird. Sie können die Positionen der Bereiche **Designoberfläche** und **Quelle** austauschen, indem Sie auf das Symbol Bereiche **austauschen** klicken, das sich zwischen den beiden Bereichen befindet:

[![Designer Ansicht](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

Klicken Sie im Menü **Ansicht** auf **Weitere Fenster > Dokument** Gliederung, um die **Dokument**Gliederung zu öffnen. Die **Dokument** Gliederung zeigt, dass das Layout derzeit ein einzelnes **linearlayoutwidget** enthält:

[![Dokument Gliederung](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

Der nächste Schritt besteht darin, die Benutzeroberfläche für die Farb Browser-app in `LinearLayout`diesem zu erstellen.

### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche für das Listenelement

Wenn der Bereich **Toolbox** nicht angezeigt wird, klicken Sie auf der linken Seite auf die Registerkarte **Toolbox** . Führen Sie in der **Toolbox**einen `ImageView`Bildlauf nach unten bis zum **Bild & Medien** Abschnitt durch, und Scrollen Sie nach unten, bis Sie Folgendes suchen:

[![ImageView suchen](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

Alternativ können Sie *ImageView* in die Suchleiste eingeben, um Folgendes zu `ImageView`finden:

[![ImageView-Suche](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

Ziehen Sie `ImageView` diese auf die Designoberfläche `ImageView` (Sie wird verwendet, um ein Farbmuster in der Farb Browser-App anzuzeigen):

[![ImageView in Canvas](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

Ziehen Sie als nächstes `LinearLayout (Vertical)` ein Widget aus der **Toolbox** in den Designer. Beachten Sie, dass ein blauer Umriss die Grenzen des hinzu `LinearLayout`gefügten angibt. Die **Dokument** Gliederung zeigt, dass es sich um `LinearLayout`ein untergeordnetes `imageView1 (ImageView)`Element von handelt, das sich unter

[![Blauer Umriss](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

Wenn Sie `ImageView` im Designer auswählen, wird der blaue Umriss zum `ImageView`umschließen von verschoben. Außerdem wird die Auswahl `imageView1 (ImageView)` in der **Dokument**Gliederung in verschoben:

[![Auswählen von ImageView](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

Ziehen Sie als nächstes `Text (Large)` ein Widget aus der **Toolbox** in das neu hinzu `LinearLayout`gefügte. Beachten Sie, dass der Designer grüne Highlights verwendet, um anzugeben, wo das neue Widget eingefügt wird:

[![Grüne Highlights](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

Fügen Sie als nächstes `Text (Small)` ein Widget unterhalb `Text (Large)` des Widgets hinzu:

[![Kleines textwidget hinzufügen](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

An diesem Punkt sollte die Designer Oberfläche dem folgenden Screenshot ähneln:

[![Designer Layout](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

Wenn sich die `textView` beiden Widgets `linearLayout1`nicht in befinden, können Sie Sie in `linearLayout1` der **Dokument** Gliederung auf ziehen und positionieren, damit Sie wie im vorherigen Screenshot (Einzug unter `linearLayout1`) angezeigt werden.


### <a name="arranging-the-user-interface"></a>Anordnen der Benutzeroberfläche

Der nächste Schritt besteht darin, die Benutzeroberfläche so zu `ImageView` ändern, dass Sie auf der linken `TextView` Seite angezeigt wird, wobei die beiden `ImageView`Widgets rechts neben gestapelt sind.

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  Geben Sie im **Eigenschaftenfenster**im Suchfeld *Width* ein, und suchen Sie die **Layoutbreite**.

3.  Ändern Sie die Einstellung **Layout Width** in `wrap_content`:

![Wrap-Inhalt festlegen](designer-walkthrough-images/vs/15-wrap-content-w158.png)

Eine andere Möglichkeit, die `Width` Einstellung zu ändern, besteht darin, auf das Dreieck auf der rechten Seite des Widgets zu klicken, um die Breite `wrap_content`der Einstellung auf Folgendes zu schalten:

![Zum Festlegen der Breite ziehen](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

Wenn Sie erneut auf das Dreieck `Width` klicken, `match_parent`wird die Einstellung auf zurückgesetzt Navigieren Sie als nächstes zum Bereich **Dokument** Gliederung, und wählen `LinearLayout`Sie das Stammverzeichnis aus:

[![Root-LinearLayout auswählen](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

Nachdem Sie den `LinearLayout` Stamm ausgewählt haben, kehren Sie zum Bereich **Eigenschaften** zurück, geben Sie die *Ausrichtung* in das Suchfeld ein, und suchen Sie nach der Einstellung **Ausrichtung** . **Ausrichtung** ändern in `horizontal`:

![Horizontale Ausrichtung auswählen](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

An diesem Punkt sollte die Designer Oberfläche dem folgenden Screenshot ähneln.
Beachten Sie, `TextView` dass die Widgets nach rechts `ImageView`von verschoben wurden:

[![Designer Layout](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>Ändern des Abstands

Der nächste Schritt besteht darin, die Auffüll-und Rand Einstellungen in der Benutzeroberfläche zu ändern, um zwischen den Widgets mehr Platz zu schaffen. Wählen Sie `ImageView` auf der Entwurfs Oberfläche aus. Geben Sie  `min` im Bereich "Eigenschaften" in das Suchfeld ein. Geben `70dp` Sie für **min height** und `50dp` für **minimale Breite**ein:

[![Höhe und Breite festlegen](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

Geben Sie  `padding` im Bereich "Eigenschaften" in das Suchfeld ein, `10dp` und geben **Sie für die**Auffüll Zeichen ein. Diese `minHeight`Einstellungen und`padding`Einstellungen fügen den`ImageView` Leerraum um alle Seiten des hinzu und verlängern ihn vertikal. `minWidth` Beachten Sie, dass sich die Layout-XML ändert, wenn Sie diese Werte eingeben:

[![Auffüllen festlegen](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

Die unteren, linken, rechten und oberen Leerraum Einstellungen können unabhängig voneinander festgelegt werden, indem Sie Werte in den Leerraum **unten**, den Abstand **Links**, den Leerraum **Rechts**und die oberen Felder der **oberen** Felder eingeben.
Legen Sie z. b. das **linke** Feld für `5dp` die Auffüll Zeichen auf fest, und klicken Sie dann auf den **unteren Rand**, `10dp`das Auffüll **Recht**und die **oberen** Felder in die

[![Benutzerdefinierte Auffüll Einstellungen](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

Passen Sie als nächstes die Position des `LinearLayout` Widgets an, das die `TextView` beiden Widgets enthält. Wählen Sie  `linearLayout1`in der Dokument Gliederung aus. Geben Sie  `margin` im Fenster Eigenschaften in das Suchfeld ein. Legen Sie den **layoutrand unten**, den **layoutrand Links**und `5dp`den **layoutrand oben** auf fest. **Layoutrand rechts** festlegen `0dp`auf:

[![Ränder festlegen](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>Entfernen des Standard Bilds

`ImageView` Da zum Anzeigen von Farben (anstelle von Bildern) verwendet wird, besteht der nächste Schritt darin, die von der Vorlage hinzugefügte Standardbild Quelle zu entfernen.

1.  Wählen Sie auf der **Designer Oberfläche aus.** `ImageView`

2.  Geben Sie im Suchfeld im Feld **Eigenschaften**den Wert *src* ein.

3.  Klicken Sie auf das kleine Quadrat rechts neben der Eigenschaften Einstellung **src** , und wählen Sie **Zurücksetzen**aus:

[![Löschen der Einstellung von Image View src](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

Dadurch wird `android:src="@android:drawable/ic_menu_gallery"` aus der XML `ImageView`-Quelldatei entfernt.

### <a name="adding-a-listview-container"></a>Hinzufügen eines ListView-Containers

Nachdem das **list_item** -Layout definiert ist, ist der nächste Schritt das Hinzufügen `ListView` eines zum Hauptlayout. Diese `ListView` enthält eine Liste von **list_item**. 

Öffnen Sie in der Projektmappen-Explorer **Ressourcen/Layout/activity_main. axml**. Suchen Sie in der **Toolbox**das `ListView` Widget, und ziehen Sie es auf den **Designoberfläche**. Der `ListView` im Designer ist leer, außer für blaue Linien, die seinen Rahmen bei der Auswahl gliedern. Sie können die **Dokument** Gliederung anzeigen, um zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

[![Neue ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

Standardmäßig `ListView` `@+id/listView1`erhält der den Wert. `Id`
Wenn `listView1` in der **Dokument**Gliederung immer noch ausgewählt ist, öffnen Sie den Bereich **Eigenschaften** , klicken Sie auf **Anordnen nach**, und wählen Sie **Kategorie**aus.
Öffnen Sie **Main**, suchen Sie die **ID** -Eigenschaft, und ändern `@+id/myListView`Sie Ihren Wert in:

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

In diesem Code wird ein `ListView` benutzerdefinierter Adapter zum Laden von Farbinformationen und zum Anzeigen dieser Daten in der soeben erstellten Benutzeroberfläche verwendet. Um dieses Beispiel kurz zu halten, sind die Farbinformationen in einer Liste hart codiert, der Adapter kann jedoch so geändert werden, dass er Farbinformationen aus einer Datenquelle extrahiert oder Sie im laufenden Betrieb berechnet. Weitere Informationen zu `ListView` Adaptern finden Sie unter [ListView](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Der folgende Screenshot zeigt ein Beispiel für die Anzeige der APP, wenn Sie auf einem Gerät ausgeführt wird:

[![Abschließender Screenshot](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt besteht darin, ein neues xamarin. Android-Projekt zu erstellen.

Starten Sie Visual Studio für Mac, und klicken Sie auf **Neues Projekt...** . Wählen Sie die Vorlage **Android-App** , und klicken Sie auf **weiter**:

[![Leere Android-App](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

Benennen Sie die neue APP **designerwalkthrough**. Wählen Sie unter **Zielplattformen**die Option **neueste und höchste** aus, und klicken Sie auf **weiter**:

[![Name der APP](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

Klicken Sie im nächsten Dialogfeld auf **Erstellen**.

### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Der nächste Schritt besteht darin, ein **LinearLayout** zu erstellen, das die Elemente der Benutzeroberfläche enthält.

Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Ressourcen/Layout** im lösungspad, und wählen Sie  **> neue Datei hinzufügen...** aus. Wählen Sie im Dialogfeld **neue Datei** die Option **Android-> Layout**aus. Benennen Sie die Datei **list_item** , und klicken Sie auf **neu**:

[![Neues Layout](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

Nachdem diese Datei hinzugefügt wurde, wird das neue **list_item** -Layout auf dem **Designoberfläche** angezeigt (wenn die Meldung angezeigt wird, *enthält dieses Projektressourcen, die nicht erfolgreich kompiliert wurden, was möglicherweise beeinträchtigt*ist, und klicken Sie auf **Erstellen > Alle erstellen** , um das Projekt zu erstellen):

[![Designer Ansicht](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

Klicken Sie unten im Designer auf die Registerkarte **Quelle** , um die XML-Quelle für dieses Layout anzuzeigen. Wenn Sie auf der rechten Seite auf die Registerkarte **Dokument** Gliederung klicken, wird angezeigt, dass das Layout derzeit ein einzelnes **linearlayoutwidget** enthält:

[![Designer-XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

Der nächste Schritt besteht darin, die Benutzeroberfläche für die Farb Browser-APP zu erstellen.

### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche für das Listenelement

Klicken Sie unten auf dem Bildschirm auf die Registerkarte **Designer** , um zur **Oberfläche des Designers**zurückzukehren. Scrollen Sie im Bereich **Toolbox** auf der rechten Seite nach unten zum Abschnitt **Images & Medium** , und `ImageView`suchen Sie Folgendes:

[![ImageView suchen](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

Alternativ können Sie *ImageView* in die Suchleiste eingeben, um Folgendes zu `ImageView`finden:

[![ImageView-Suche](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

Ziehen Sie `ImageView` diese auf  die `ImageView` Designoberfläche (Sie wird verwendet, um ein Farbmuster in der Farb Browser-App anzuzeigen):

[![ImageView in Canvas](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

Ziehen Sie als nächstes `LinearLayout (Vertical)` ein Widget aus der **Toolbox** in die **Designoberfläche**. Beachten Sie, dass ein blauer Umriss die Grenzen des hinzu `LinearLayout`gefügten angibt. Die **Dokument** Gliederung zeigt, dass es sich um `LinearLayout`ein unter `imageView1 (ImageView)`geordnetes Element von handelt

[![Blauer Umriss](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

Wenn Sie `ImageView` im Designer auswählen, wird der blaue Umriss zum `ImageView`umschließen von verschoben. Außerdem wird die Auswahl `imageView1 (ImageView)` in der **Dokument**Gliederung in verschoben:

[![Auswählen von ImageView](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

Ziehen Sie als nächstes `Text (Large)` ein Widget aus der **Toolbox** in das neu hinzu `LinearLayout`gefügte. Beachten Sie, dass beim Ziehen mit der Maus auf die **Designoberfläche**die Position des neuen Widgets hervorgehoben wird.
Das `Text (Large)` Widget sollte sich in `linearLayout1` befinden, wie hier gezeigt:

[![Ein großes Text-Widget hinzufügen](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

Fügen Sie als nächstes `Text (Small)` ein Widget unterhalb `Text (Large)` des Widgets hinzu. An diesem Punkt sollte die **Designoberfläche** dem folgenden Screenshot ähneln:

[![Kleines textwidget hinzufügen](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

Wenn sich die `textView` beiden Widgets `linearLayout1`nicht in befinden, können Sie Sie in `linearLayout1` der **Dokument** Gliederung auf ziehen und positionieren, damit Sie wie im vorherigen Screenshot (eingerückt unter `linearLayout1`) angezeigt werden.


### <a name="arranging-the-user-interface"></a>Anordnen der Benutzeroberfläche

Der nächste Schritt besteht darin, die Benutzeroberfläche so zu `ImageView` ändern, dass Sie auf der linken `TextView` Seite angezeigt wird, wobei die beiden `ImageView`Widgets rechts neben gestapelt sind.

1.  Klicken Sie bei  ausgewähltemaufdieRegisterkarte`ImageView` Eigenschaften.

2.  Klicken Sie direkt unterhalb der Registerkarte **Eigenschaften** auf **Layout**.

3.  Scrollen Sie nach unten zu **viewgroup** , `Width` und ändern `wrap_content`Sie die Einstellung in:

[![Wrap-Inhalt festlegen](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

Eine andere Möglichkeit, die `Width` Einstellung zu ändern, besteht darin, auf das Dreieck auf der rechten Seite des Widgets zu klicken, um die Breite `wrap_content`der Einstellung auf Folgendes zu schalten:

[![Zum Festlegen der Breite ziehen](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

Wenn Sie erneut auf das Dreieck `Width` klicken, `match_parent`wird die Einstellung auf zurückgesetzt Navigieren Sie als nächstes zum Bereich **Dokument** Gliederung, und wählen `LinearLayout`Sie das Stammverzeichnis aus:

[![Root-LinearLayout auswählen](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

Wenn der `LinearLayout` Stamm ausgewählt ist, kehren Sie zur Registerkarte **Eigenschaften** zurück, und klicken Sie auf **Widget**. Ändern Sie `Orientation` die Einstellung `horizontal` in, wie unten gezeigt. An diesem Punkt sollte die **Designoberfläche** dem folgenden Screenshot ähneln. Beachten Sie, `TextView` dass die Widgets nach rechts `ImageView`von verschoben wurden:

[![Horizontale Ausrichtung auswählen](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)


### <a name="modifying-the-spacing"></a>Ändern des Abstands

Der nächste Schritt besteht darin, die Auffüll-und Rand Einstellungen in der Benutzeroberfläche zu ändern, um zwischen den Widgets mehr Platz zu schaffen. Wählen Sie aus,  undklickenSieunterEigenschaftenaufdieRegisterkarte`ImageView` **Layout** . `Min Width` Ändern Sie in `50dp`, `Min Height` und`Padding` in. `70dp` `10dp`
Dadurch wird die `ImageView` Auffüll Richtung um alle Seiten der angewendet und vertikal gestreckt:

[![Auffüllen festlegen](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

Die Einstellungen oben, rechts, unten und Links können unabhängig voneinander festgelegt werden, indem Sie Werte in die `Top`Felder `Right`, `Bottom`, und `Left` auffüllen. Legen Sie z. b `Left` . den Füll Wert `5dp` auf und `Top`die `Right`Füll Werte `Bottom` , und auf `10dp`fest. Beachten Sie, `Padding` dass sich die Einstellung in eine durch Trennzeichen getrennte Liste dieser Werte ändert:

[![Benutzerdefinierte Auffüll Einstellungen](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

Passen Sie als nächstes die Position des `LinearLayout` Widgets an, das die `TextView` beiden Widgets enthält. Wählen Sie  `linearLayout1`in der Dokument Gliederung aus. Wählen Sie im Bereich **Eigenschaften** die Registerkarte **Layout** aus. Scrollen Sie nach unten zum Abschnitt **viewgroup** , und `Left`legen `Top`Sie `Right`die Ränder `Bottom` ,, `5dp`und `5dp`auf `0dp`,, `5dp` bzw. fest:

[![Ränder festlegen](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>Entfernen des Standard Bilds

`ImageView` Da zum Anzeigen von Farben (anstelle von Bildern) verwendet wird, besteht der nächste Schritt darin, die von der Vorlage hinzugefügte Standardbild Quelle zu entfernen.

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  Klicken Sie unter **Eigenschaften**auf die Registerkarte **Widget** .

3.  Deaktivieren Sie `Src` die Einstellung, sodass Sie leer ist:

[![Löschen der Einstellung von Image View src](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

Dadurch wird `android:src="@android:drawable/ic_menu_gallery"` aus der XML `ImageView`-Quelldatei entfernt.

### <a name="adding-a-listview-container"></a>Hinzufügen eines ListView-Containers

Nachdem das **list_item** -Layout definiert ist, ist der nächste Schritt das Hinzufügen `ListView` eines zum Hauptlayout. Diese `ListView` enthält eine Liste von **list_item**. 

Öffnen Sie im **Projektmappen-Explorer** **Ressourcen/Layout/Main. axml**.
Klicken Sie `Button` auf das Widget (sofern vorhanden), und löschen Sie es. Suchen Sie in der **Toolbox**das `ListView` Widget, und ziehen Sie es auf den **Designoberfläche**.
Der `ListView` im Designer ist leer, außer für blaue Linien, die seinen Rahmen bei der Auswahl gliedern. Sie können die **Dokument** Gliederung anzeigen, um zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

[![Neue ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

Standardmäßig `ListView` `@+id/listView1`erhält der den Wert. `Id`
Wenn `listView1` in der **Dokument**Gliederung immer noch ausgewählt ist, öffnen Sie den Bereich **Eigenschaften** , klicken Sie auf **Anordnen nach**, und wählen Sie **Kategorie**aus.
Öffnen Sie **Main**, suchen Sie die **ID** -Eigenschaft, und ändern `@+id/myListView`Sie Ihren Wert in:

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

In diesem Code wird ein `ListView` benutzerdefinierter Adapter zum Laden von Farbinformationen und zum Anzeigen dieser Daten in der soeben erstellten Benutzeroberfläche verwendet. Um dieses Beispiel kurz zu halten, sind die Farbinformationen in einer Liste hart codiert, der Adapter kann jedoch so geändert werden, dass er Farbinformationen aus einer Datenquelle extrahiert oder Sie im laufenden Betrieb berechnet. Weitere Informationen zu `ListView` Adaptern finden Sie unter [ListView](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Der folgende Screenshot zeigt ein Beispiel für die Anzeige der APP, wenn Sie auf einem Gerät ausgeführt wird:

[![Abschließender Screenshot](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde der Prozess der Verwendung von xamarin. Android Designer in Visual Studio zum Erstellen einer Benutzeroberfläche für eine einfache APP erläutert.
Es wurde gezeigt, wie Sie die Schnittstelle für ein einzelnes Element in einer Liste erstellen, und es wurde gezeigt, wie Sie Widgets hinzufügen und visuell anordnen.
Außerdem wurde erläutert, wie Ressourcen zugewiesen werden und anschließend verschiedene Eigenschaften für diese Widgets festgelegt werden.
