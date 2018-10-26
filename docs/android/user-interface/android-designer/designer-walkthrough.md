---
title: Mithilfe des Designers für Xamarin.Android
description: Dieser Artikel enthält eine exemplarische Vorgehensweise für den Xamarin.Android-Designer. Es wird veranschaulicht, wie eine Benutzeroberfläche für eine kleine Farbe-Browser-app zu erstellen; Diese Benutzeroberfläche wird vollständig in den Designer erstellt.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1174fe5cb417d4977fd6519086e6c4942e74c10b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120269"
---
# <a name="using-the-xamarinandroid-designer"></a>Mithilfe des Designers für Xamarin.Android

_Dieser Artikel enthält eine exemplarische Vorgehensweise für den Xamarin.Android-Designer. Es wird veranschaulicht, wie eine Benutzeroberfläche für eine kleine Farbe-Browser-app zu erstellen; Diese Benutzeroberfläche wird vollständig in den Designer erstellt._


## <a name="overview"></a>Übersicht

Android-Benutzeroberflächen können deklarativ mithilfe von XML-Dateien oder programmgesteuert durch Schreiben von Code erstellt werden. Der Xamarin.Android-Designer ermöglicht Entwicklern das Erstellen und ändern deklarative Layouts visuell, ohne dass manuelle Bearbeitung von XML-Dateien. Der Designer bietet auch Feedback in Echtzeit, in dem den Entwickler, die Änderungen an der Benutzeroberfläche zu bewerten, ohne die Anwendung auf einem Gerät oder einen Emulator erneut bereitstellen zu können. Diese Designer-Funktionen können Android-Benutzeroberflächen-Entwicklung erheblich beschleunigt werden.
In diesem Artikel veranschaulicht, wie die Xamarin.Android-Designer verwenden, um eine Benutzeroberfläche visuell zu erstellen.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise wird zum Android Designer zu verwenden, um eine Benutzeroberfläche für eine Beispiel-Color-Browser-app zu erstellen. Die Farbe-Browser-app zeigt eine Liste der Farben, deren Namen und ihren RGB-Werten. Erfahren Sie, wie Sie Widgets zum Hinzufügen der **Entwurfsoberfläche** und wie Sie diese Widgets visuell anzuordnen. Anschließend wird beschrieben, wie Sie zum Ändern von Widgets, die interaktiv auf den **Entwurfsoberfläche** oder mithilfe des Designers **Eigenschaften** Bereich. Zum Schluss sehen Sie, wie das Design aussieht, wenn die app auf einem Gerät oder Emulator ausgeführt wird.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt ist die Erstellung ein neues Xamarin.Android-Projekt. Starten Sie Visual Studio, klicken Sie auf **neues Projekt...** , und wählen Sie die **Visual C\# > Android > Android-App (Xamarin)** Vorlage.
Benennen Sie die neue app **DesignerWalkthrough** , und klicken Sie auf **OK**.

[![Leere Android-app](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

In der **neue Android-App** Dialogfeld Wählen Sie **leere App** , und klicken Sie auf **OK**:

[![Auswählen der Vorlage der leeren Android-App](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)


### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Der nächste Schritt ist die Erstellung einer **LinearLayout** , wird den Benutzer Elemente der Benutzeroberfläche aufnehmen. Mit der rechten Maustaste **Ressourcen/Layout** in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Element...** . In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Android-Layout**. Nennen Sie die Datei **List_item** , und klicken Sie auf **hinzufügen**:

[![Neues layout](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

Die neue **List_item** Layout wird im Designer angezeigt. Beachten Sie, dass zwei Bereiche angezeigt werden &ndash; der *Entwurfsoberfläche* für die **List_item** im linken Bereich sichtbar ist, während die XML-Quelle im rechten Bereich angezeigt wird. Tauschen Sie die Positionen der **Entwurfsoberfläche** und **Quelle** Bereiche, indem Sie auf die **Swap Bereiche** Symbol befindet sich zwischen den beiden Fenstern:

[![Ansicht-Designers](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

Aus der **Ansicht** im Menü klicken Sie auf **andere Windows > Dokumentgliederung** zum Öffnen der **Dokumentgliederung**. Die **Dokumentgliederung** zeigt an, dass das Layout derzeit einen einzelnen enthält **LinearLayout** Widgets:

[![Dokumentgliederung](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

Der nächste Schritt ist die Erstellung von die Benutzeroberfläche für die Color-Browser-app in dieser `LinearLayout`.

### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche des Listenelements

Wenn die **Toolbox** Bereich wird nicht angezeigt wird, klicken Sie auf die **Toolbox** links auf die Registerkarte. In der **Toolbox**, führen Sie einen Bildlauf nach unten zu der **Bilder und Medien** aus, und scrollen Sie weiter auf, bis Sie finden ein `ImageView`:

[![Suchen Sie ImageView](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

Alternativ können Sie eingeben *ImageView* in der Suchleiste, suchen Sie die `ImageView`:

[![ImageView suchen](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

Ziehen Sie `ImageView` auf die Entwurfsoberfläche (dies `ImageView` wird verwendet, um ein Farbfeld in der Color-Browser-app angezeigt):

[![ImageView Zeichenbereich](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

Ziehen Sie jetzt eine `LinearLayout (Vertical)` Widget aus der **Toolbox** in den Designer. Beachten Sie, dass es sich bei einem blauen Rahmen gibt an, die Grenzen des hinzugefügten `LinearLayout`. Die **Dokumentgliederung** zeigt, dass es ein untergeordnetes Element des `LinearLayout`auf `imageView1 (ImageView)`:

[![Blau](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

Bei der Auswahl der `ImageView` im Designer bewegt der blaue Rahmen zum Umschließen der `ImageView`. Darüber hinaus die Auswahl verschoben wird, um `imageView1 (ImageView)` in die **Dokumentgliederung**:

[![Wählen Sie ImageView](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

Ziehen Sie jetzt eine `Text (Large)` Widget aus der **Toolbox** in die neu hinzugefügte `LinearLayout`. Beachten Sie, dass der Designer Grün verwendet wird hervorgehoben, um anzugeben, in dem das neue Widget eingefügt wird:

[![Grün hervorgehoben.](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

Fügen Sie als Nächstes eine `Text (Small)` Widget unten die `Text (Large)` Widgets:

[![Kleine Text-Widget "hinzufügen"](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

An diesem Punkt sollte die Designer-Oberfläche im folgenden Screenshot entsprechen:

[![Layout für Aktivitätsdesigner](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

Wenn die beiden `textView` Widgets sind nicht in `linearLayout1`, können Sie sie ziehen `linearLayout1` in die **Dokumentgliederung** und positionieren Sie sie an, damit sie angezeigt werden, wie im vorherigen Screenshot dargestellt (eingerückt unter `linearLayout1`).


### <a name="arranging-the-user-interface"></a>Anordnen von der Benutzeroberfläche

Der nächste Schritt ist so ändern Sie die Benutzeroberfläche zum Anzeigen der `ImageView` auf der linken Seite, wobei die beiden `TextView` Widgets gestapelt, rechts neben der `ImageView`.

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  In der **Fenster "Eigenschaften"**, geben Sie *Breite* in die Suche ein, und suchen Sie **Layoutbreite**.

3.  Ändern der **Layoutbreite** auf `wrap_content`:

![Set-Wrap-Inhalt](designer-walkthrough-images/vs/15-wrap-content-w158.png)

Eine weitere Möglichkeit zum Ändern der `Width` Einstellung ist auf das Dreieck auf der rechten Seite des Widgets zum Umschalten der breiteneinstellung zum klicken `wrap_content`:

![Ziehen Sie zum Festlegen der Breite](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

Gibt zurück, klicken Sie erneut auf das Dreieck der `Width` auf `match_parent`. Navigieren Sie anschließend auf die **Dokumentgliederung** Bereich, und wählen Sie den Stamm `LinearLayout`:

[![Wählen Sie das Stammverzeichnis LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

Mit dem Stamm `LinearLayout` ausgewählt ist, zurück zum die **Eigenschaften** Bereich geben Sie *Ausrichtung* in das Suchfeld ein, und suchen Sie die **Ausrichtung** Einstellung. Änderung **Ausrichtung** zu `horizontal`:

![Wählen Sie die horizontalen Ausrichtung](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

An diesem Punkt sollte die Designer-Oberfläche im folgenden Screenshot entsprechen.
Beachten Sie, dass die `TextView` Widgets verschoben wurden, die rechts neben der `ImageView`:

[![Layout für Aktivitätsdesigner](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>Den Abstand ändern

Der nächste Schritt besteht, Ränder und Abstände in der Benutzeroberfläche mehr Abstand zwischen den Widgets zu ändern. Wählen Sie die `ImageView` auf der Entwurfsoberfläche angezeigt. In der **Eigenschaften** Bereich eingeben `min` in das Suchfeld. Geben Sie `70dp` für **Mindesthöhe** und `50dp` für **Mindestbreite**:

[![Set-Höhe und Breite](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

In der **Eigenschaften** Bereich eingeben `padding` in die Suche und geben Sie `10dp` für **Padding**. Diese `minHeight`, `minWidth` und `padding` Einstellungen hinzufügen der Auffüllung, um alle Seiten des der `ImageView` und Strecken sie vertikal. Beachten Sie, dass die Layout-XML ändert, während der Eingabe dieser Werte:

[![Legen Sie die Auffüllung](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

Die unteren, linken, rechten und oberen Textabstand kann unabhängig voneinander festgelegt werden, durch die Eingabe von Werten in der **Auffüllung unten**, **Auffüllung Links**, **Auffüllung rechts**, und  **Auffüllung oben** Feldern.
Legen Sie z. B. die **Auffüllung Links** Feld `5dp` und **Auffüllung unten**, **Auffüllung rechts**, und **Padding-Top** Felder um `10dp`:

[![Benutzerdefinierte Einstellungen](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

Passen Sie die Position des als Nächstes die `LinearLayout` Widgets, die die beiden enthält `TextView` Widgets. In der **Dokumentgliederung**Option `linearLayout1`. In der **Eigenschaften** Fenster eingeben `margin` in das Suchfeld. Legen Sie **Layout Margin-Bottom**, **Layout Margin-Left**, und **Layout Margin-Top** zu `5dp`. Legen Sie **Layout Rand rechts** zu `0dp`:

[![Festlegen von Rändern](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>Entfernen das Standardimage

Da die `ImageView` wird verwendet, um die Anzeige von Farben (anstatt Bilder), ist der nächste Schritt entfernen Sie die standardmäßige Bild-Quelle, die von der Vorlage hinzugefügt.

1.  Wählen Sie die `ImageView` auf die **Designeroberfläche**.

2.  In **Eigenschaften**, geben Sie *Src* in das Suchfeld.

3.  Klicken Sie auf die kleine Rechteck rechts neben der **Src** Eigenschaft festlegen, und wählen **zurücksetzen**:

[![Deaktivieren Sie die ImageView Src-Einstellung](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

Dies entfernt `android:src="@android:drawable/ic_menu_gallery"` aus der XML-Quelle für diesen `ImageView`.

### <a name="adding-a-listview-container"></a>Hinzufügen eines ListView-Containers

Nun, dass die **List_item** Layout definiert ist, ist des nächsten Schritts zum Hinzufügen einer `ListView` dem Hauptlayout. Dies `ListView` enthält eine Liste der **List_item**. 

In der **Projektmappen-Explorer**öffnen **Resources/layout/activity_main.axml**. In der **ToolBox**, suchen Sie die `ListView` Widget, und ziehen Sie es auf die **Entwurfsoberfläche**. Die `ListView` im Designer werden außer blaue Linien, die den Rahmen zu beschreiben, wenn es ausgewählt wird. Sehen Sie die **Dokumentgliederung** zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

[![Neue ListView.](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

In der Standardeinstellung die `ListView` erhält eine `Id` Wert `@+id/listView1`.
Während `listView1` immer noch ausgewählt ist die **Dokumentgliederung**öffnen die **Eigenschaften** Bereich klicken Sie auf **anordnen, indem Sie**, und wählen Sie **Kategorie**.
Open **Main**, suchen Sie die **Id** -Eigenschaft, und ändern Sie seinen Wert in `@+id/myListView`:

[![Benennen Sie die Id in myListView](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

An diesem Punkt kann die Benutzeroberfläche verwenden.

### <a name="running-the-application"></a>Ausführen der Anwendung

Open **"mainactivity.cs"** und Ersetzen Sie den Code durch Folgendes:

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

Dieser Code verwendet ein benutzerdefiniertes `ListView` Adapter Farbinformationen geladen und zum Anzeigen der Daten in der Benutzeroberfläche, die gerade erstellt haben. Um dieses Beispiel kurz zu halten, die die Farbinformationen ist in einer Liste hartcodiert, aber der Adapter kann geändert werden, Farbinformationen aus einer Datenquelle extrahiert und es dynamisch zu berechnen. Weitere Informationen zu `ListView` -Adapter, finden Sie unter [ListView](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Im folgende Screenshot wird verdeutlicht, wie die app wird angezeigt, wenn auf einem Gerät ausgeführt wird:

[![Endgültigen Screenshots](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt ist die Erstellung ein neues Xamarin.Android-Projekt.

Starten Sie Visual Studio für Mac, und klicken Sie auf **neues Projekt...** . Wählen Sie die **Android-App** Vorlage, und klicken Sie auf **Weiter**:

[![Leere Android-app](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

Benennen Sie die neue app **DesignerWalkthrough**. Klicken Sie unter **Zielplattformen**Option **neueste Informationen und Materialien** , und klicken Sie auf **Weiter**:

[![Name-app](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

Klicken Sie im Dialogfeld im nächsten Bildschirm auf **erstellen**.

### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Der nächste Schritt ist die Erstellung einer **LinearLayout** , wird den Benutzer Elemente der Benutzeroberfläche aufnehmen.

In Visual Studio für Mac, mit der Maustaste **Ressourcen/Layout** in die **Lösung** aufzufüllen, und wählen Sie **hinzufügen > neue Datei...** . In der **neue Datei** wählen Sie im Dialogfeld **Android > Layout**. Nennen Sie die Datei **List_item** , und klicken Sie auf **neu**:

[![Neues layout](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

Nach dem Hinzufügen dieser Datei den neuen **List_item** Layout wird angezeigt, auf die **Entwurfsoberfläche** (Wenn Sie die Meldung *dieses Projekt enthält Ressourcen, die nicht erfolgreich kompiliert wurden Rendering wird möglicherweise beeinträchtigt*, klicken Sie auf **erstellen > alle erstellen** zum Erstellen des Projekts):

[![Ansicht-Designers](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

Klicken Sie auf die **Quelle** Registerkarten im unteren Bereich des Designers, um die XML-Quelle für dieses Layout anzuzeigen. Beim Klicken auf die **Dokumentgliederung** Registerkarte auf der rechten Seite zeigt, dass das Layout derzeit einen einzelnen enthält **LinearLayout** Widgets:

[![Designer-XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

Der nächste Schritt besteht darin die Benutzeroberfläche für die Color-Browser-app zu erstellen.

### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche des Listenelements

Klicken Sie auf die **Designer** Registerkarte am unteren Rand des Bildschirms zum Zurückgeben der **Designer-Oberfläche**. In der **Toolbox** Bereich auf der rechten Seite einen Bildlauf nach unten, um die **Bilder & Media** aus, und suchen Sie `ImageView`:

[![Suchen Sie ImageView](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

Alternativ können Sie eingeben *ImageView* in der Suchleiste, suchen Sie die `ImageView`:

[![ImageView suchen](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

Ziehen Sie `ImageView` auf die **Entwurfsoberfläche** (dies `ImageView` wird verwendet, um ein Farbfeld in der Color-Browser-app angezeigt):

[![ImageView Zeichenbereich](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

Ziehen Sie jetzt eine `LinearLayout (Vertical)` Widget aus der **Toolbox** in die **Entwurfsoberfläche**. Beachten Sie, dass es sich bei einem blauen Rahmen gibt an, die Grenzen des hinzugefügten `LinearLayout`. Die **Dokumentgliederung** zeigt, dass es ein untergeordnetes Element des `LinearLayout`unter `imageView1 (ImageView)`:

[![Blau](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

Bei der Auswahl der `ImageView` im Designer bewegt der blaue Rahmen zum Umschließen der `ImageView`. Darüber hinaus die Auswahl verschoben wird, um `imageView1 (ImageView)` in die **Dokumentgliederung**:

[![Wählen Sie ImageView](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

Ziehen Sie jetzt eine `Text (Large)` Widget aus der **Toolbox** in die neu hinzugefügte `LinearLayout`. Beachten Sie, dass beim Ziehen der Maus auf die **Entwurfsoberfläche**, hervorgehoben, in dem das neue Widget eingefügt wird.
Die `Text (Large)` Widget sollte sich in befinden `linearLayout1` wie hier gezeigt:

[![Ein großes Text-Widget hinzufügen](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

Fügen Sie als Nächstes eine `Text (Small)` Widget unten die `Text (Large)` Widget. An diesem Punkt die **Entwurfsoberfläche** sollte etwa den folgenden Screenshot aussehen:

[![Kleine Text-Widget "hinzufügen"](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

Wenn die beiden `textView` Widgets sind nicht in `linearLayout1`, können Sie sie ziehen `linearLayout1` in die **Dokumentgliederung** und positionieren Sie sie an, sodass diese angezeigt werden, wie im vorherigen Screenshot dargestellt (eingerückt unter `linearLayout1`).


### <a name="arranging-the-user-interface"></a>Anordnen von der Benutzeroberfläche

Der nächste Schritt ist so ändern Sie die Benutzeroberfläche zum Anzeigen der `ImageView` auf der linken Seite, wobei die beiden `TextView` Widgets gestapelt, rechts neben der `ImageView`.

1.  Mit der `ImageView` ausgewählt ist, klicken Sie auf die **Eigenschaften** Registerkarte.

2.  Direkt unterhalb der **Eigenschaften** auf **Layout**.

3.  Führen Sie einen Bildlauf nach unten zum **ViewGroup** , und ändern Sie die `Width` auf `wrap_content`:

[![Set-Wrap-Inhalt](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

Eine weitere Möglichkeit zum Ändern der `Width` Einstellung ist auf das Dreieck auf der rechten Seite des Widgets zum Umschalten der breiteneinstellung zum klicken `wrap_content`:

[![Ziehen Sie zum Festlegen der Breite](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

Gibt zurück, klicken Sie erneut auf das Dreieck der `Width` auf `match_parent`. Navigieren Sie anschließend auf die **Dokumentgliederung** Bereich, und wählen Sie den Stamm `LinearLayout`:

[![Wählen Sie das Stammverzeichnis LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

Mit dem Stamm `LinearLayout` ausgewählt ist, zurück an die **Eigenschaften** Registerkarte, und klicken Sie auf **Widget**. Ändern der `Orientation` auf `horizontal` wie unten dargestellt. An diesem Punkt die **Entwurfsoberfläche** sollte etwa den folgenden Screenshot aussehen. Beachten Sie, dass die `TextView` Widgets verschoben wurden, die rechts neben der `ImageView`:

[![Wählen Sie die horizontalen Ausrichtung](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)


### <a name="modifying-the-spacing"></a>Den Abstand ändern

Der nächste Schritt ist zum Ändern der Einstellungen für Ränder und Abstände in der Benutzeroberfläche, um mehr Abstand zwischen den Widgets bereitzustellen. Wählen Sie die `ImageView` , und klicken Sie auf die **Layout** Registerkarte **Eigenschaften**. Ändern der `Min Width` zu `50dp`, `Min Height` zu `70dp`, und die `Padding` zu `10dp`.
Dies gilt auffüllen, um alle Seiten des der `ImageView` und es vertikal elongates:

[![Legen Sie die Auffüllung](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

Die oben, rechts, unten und den Textabstand links einzeln festgelegt werden können durch Eingabe von Werten in der `Top`, `Right`, `Bottom`, und `Left` bzw. Auffüllen von Feldern. Legen Sie z. B. die `Left` padding-Wert, der `5dp` und die `Top`, `Right`, und `Bottom` -Abstandswerts zu `10dp`. Beachten Sie, dass die `Padding` Einstellung ändert, um eine durch Trennzeichen getrennte Liste der folgenden Werte:

[![Benutzerdefinierte Einstellungen](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

Passen Sie die Position des als Nächstes die `LinearLayout` Widgets, die die beiden enthält `TextView` Widgets. In der **Dokumentgliederung**Option `linearLayout1`. In der **Eigenschaften** wählen Sie im Bereich der **Layout** Registerkarte. Führen Sie einen Bildlauf nach unten, um die **ViewGroup** aus, und legen Sie die `Left`, `Top`, `Right`, und `Bottom` Ränder auf `5dp`, `5dp`, `0dp`, und `5dp` bzw.:

[![Festlegen von Rändern](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>Entfernen das Standardimage

Da die `ImageView` wird verwendet, um die Anzeige von Farben (anstatt Bilder), ist der nächste Schritt entfernen Sie die standardmäßige Bild-Quelle, die von der Vorlage hinzugefügt.

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  Klicken Sie auf die **Widget** Registerkarte **Eigenschaften**.

3.  Deaktivieren der `Src` festlegen, sodass es leer ist:

[![Deaktivieren Sie die ImageView Src-Einstellung](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

Dies entfernt `android:src="@android:drawable/ic_menu_gallery"` aus der XML-Quelle für diesen `ImageView`.

### <a name="adding-a-listview-container"></a>Hinzufügen eines ListView-Containers

Nun, dass die **List_item** Layout definiert ist, ist des nächsten Schritts zum Hinzufügen einer `ListView` dem Hauptlayout. Dies `ListView` enthält eine Liste der **List_item**. 

In der **Projektmappen-Explorer**öffnen **Resources/layout/Main.axml**.
Klicken Sie auf die `Button` Widget (sofern vorhanden) und löschen Sie sie. In der **ToolBox**, suchen Sie die `ListView` Widget, und ziehen Sie es auf die **Entwurfsoberfläche**.
Die `ListView` im Designer werden außer blaue Linien, die den Rahmen zu beschreiben, wenn es ausgewählt wird. Sehen Sie die **Dokumentgliederung** zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

[![Neue ListView.](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

In der Standardeinstellung die `ListView` erhält eine `Id` Wert `@+id/listView1`.
Während `listView1` immer noch ausgewählt ist die **Dokumentgliederung**öffnen die **Eigenschaften** Bereich klicken Sie auf **anordnen, indem Sie**, und wählen Sie **Kategorie**.
Open **Main**, suchen Sie die **Id** -Eigenschaft, und ändern Sie seinen Wert in `@+id/myListView`:

[![Benennen Sie die Id in myListView](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

An diesem Punkt kann die Benutzeroberfläche verwenden.

### <a name="running-the-application"></a>Ausführen der Anwendung

Open **"mainactivity.cs"** und Ersetzen Sie den Code durch Folgendes:

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

Dieser Code verwendet ein benutzerdefiniertes `ListView` Adapter Farbinformationen geladen und zum Anzeigen der Daten in der Benutzeroberfläche, die gerade erstellt haben. Um dieses Beispiel kurz zu halten, die die Farbinformationen ist in einer Liste hartcodiert, aber der Adapter kann geändert werden, Farbinformationen aus einer Datenquelle extrahiert und es dynamisch zu berechnen. Weitere Informationen zu `ListView` -Adapter, finden Sie unter [ListView](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Im folgende Screenshot wird verdeutlicht, wie die app wird angezeigt, wenn auf einem Gerät ausgeführt wird:

[![Endgültigen Screenshots](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie durch den Prozess mit der Xamarin.Android-Designer in Visual Studio zum Erstellen einer Benutzeroberfläche für eine einfache app.
Es wurde gezeigt, wie die Benutzeroberfläche für ein einzelnes Element in einer Liste erstellen, und sie das Hinzufügen von Widgets und ordnen sie Sie visuell dargestellt.
Darüber hinaus wurde erläutert, wie Sie Ressourcen zuzuweisen, und legen Sie verschiedene Eigenschaften für diese Widgets werden.
