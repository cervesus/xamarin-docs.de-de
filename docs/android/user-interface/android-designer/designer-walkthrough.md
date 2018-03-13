---
title: Verwenden die Android-Designer
description: "Dieses Thema bietet eine exemplarische Vorgehensweise des Designers Xamarin.Android. Es zeigt, wie eine Benutzeroberfläche für eine kleine Farbe-Browser-app zu erstellen; Diese Benutzeroberfläche wird vollständig in den Designer erstellt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: c9ec0d3bc9c3278f097b925ccb755323df950c62
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="using-the-android-designer"></a>Verwenden die Android-Designer

_Dieses Thema bietet eine exemplarische Vorgehensweise des Designers Xamarin.Android. Es zeigt, wie eine Benutzeroberfläche für eine kleine Farbe-Browser-app zu erstellen; Diese Benutzeroberfläche wird vollständig in den Designer erstellt._


## <a name="overview"></a>Übersicht

Android-Benutzer-Schnittstellen können deklarativ mithilfe von XML-Dateien oder programmgesteuert durch Schreiben von Code erstellt werden. Der Xamarin.Android Designer ermöglicht Entwicklern das Erstellen und ändern deklarative Layouts visuell, ohne dass für den Umgang mit den zeitlichen Aufwand für die manuelle Bearbeitung von XML-Dateien. Der Designer bietet auch Echtzeitinformationen, der den Entwickler Änderungen der Benutzeroberfläche zu bewerten, ohne die Anwendung auf einem Gerät oder einen Emulator erneut bereitstellen zu können. Entwicklung von Android Benutzeroberflächen können erheblich beschleunigt werden. In diesem Artikel stellen wir eine exemplarische Vorgehensweise, die veranschaulicht, wie der Xamarin.Android-Designer verwenden, um eine Benutzeroberfläche visuell zu erstellen.


## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Das Ziel dieser exemplarischen Vorgehensweise werden die Android-Designer verwenden, um eine Benutzeroberfläche für eine Beispiel-Farbe-Browser-app zu erstellen, in dem eine Liste von Farben, deren Namen und die RGB-Werte dargestellt. Sehen wir uns zum Hinzufügen von Widgets auf die Entwurfsoberfläche und wie diese Widgets visuell Layout. Danach erläutern wir, wie zum Ändern von Widgets interaktiv auf die Entwurfsoberfläche ziehen oder mithilfe des Designers **Eigenschaft Pad**. Abschließend sehen wir, wie unser Design aussieht, wenn wir die app ausführen.

Fangen wir an!


### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Der erste Schritt ist zum Erstellen eines neuen Xamarin.Android-Projekts.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Starten Sie Visual Studio, und klicken Sie auf **neues Projekt...**  wählen Sie dann die **Visual C\# > Android > leere App (Android)** Vorlage:

[![Leere Android-app](designer-walkthrough-images/vs/01-android-app-sml.png)](designer-walkthrough-images/vs/01-android-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Starten Sie Visual Studio für Mac, und klicken Sie auf **neue Projektmappe...** . Wählen Sie die **Android-App** Vorlage, und klicken Sie auf **Weiter**:

[![Leere Android-app](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Benennen Sie die neue app **DesignerWalkthrough** , und klicken Sie auf **OK**.

[![Name-app](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Benennen Sie die neue app **DesignerWalkthrough**. Unter **Zielplattformen**Option **neueste und größte** , und klicken Sie auf **Weiter**:

[![Name-app](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

Klicken Sie im nächsten Dialogfeld-Bildschirm auf **erstellen**.

-----



### <a name="adding-a-layout"></a>Hinzufügen eines Layouts

Erstellen wir eine **LinearLayout** , dass wir mithilfe unserer Benutzer Elemente der Benutzeroberfläche enthalten.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Visual Studio mit der Maustaste **Ressourcen/Layout** in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Element...** . In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Android Layout**. Nennen Sie die Datei **ListItem.axml** , und klicken Sie auf **hinzufügen**:

[![Das neue layout](designer-walkthrough-images/vs/03-new-layout-sml.png)](designer-walkthrough-images/vs/03-new-layout.png#lightbox)

Die neue **"ListItem"** Layout im Designer angezeigt wird:

[![Designeransicht](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

Klicken Sie auf die **Quelle** Registerkarte am unteren Rand der Designer, um die XML-Quelle für dieses Layout anzuzeigen:

[![XML-Designer](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

Aus der **Ansicht** im Menü klicken Sie auf **Weitere Fenster > Dokumentgliederung** So öffnen die **Dokumentgliederung**. Die **Dokumentgliederung** zeigt an, dass das Layout derzeit ein einzelnes enthält **LinearLayout** Widget:

[![Dokumentgliederung](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In Visual Studio für Mac, mit der Maustaste **Ressourcen/Layout** in der **Lösung** aufzufüllen, und wählen Sie **hinzufügen > neuen Datei...** . In der **neue Datei** wählen Sie im Dialogfeld **Android > Layout**. Nennen Sie die Datei **"ListItem"** , und klicken Sie auf **neu**:

[![Das neue layout](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

Die neue **"ListItem"** Layout im Designer angezeigt wird:

[![Designeransicht](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

Klicken Sie auf die **Quelle** Registerkarte am unteren Rand der Designer, um die XML-Quelle für dieses Layout anzuzeigen. Beim Klicken auf die **Dokumentgliederung** Registerkarte auf der rechten Seite zeigt, dass das Layout derzeit ein einzelnes enthält **LinearLayout** Widget:

[![XML-Designer](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>Erstellen der Benutzeroberfläche der Liste-Element

Als Nächstes werden wir die Benutzeroberfläche für die Farbe-Browser-app zu erstellen.
Klicken Sie auf die **Designer** Registerkarte, um auf die Entwurfsoberfläche zurückzukehren.
In der **Toolbox**, führen Sie einen Bildlauf nach unten zu der **Bilder & Media** Abschnitt und jedes Element Instrumentationsdaten, bis Sie finden ein *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView suchen](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![ImageView suchen](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

Alternativ können Sie eingeben *ImageView* in der Suchleiste zum Suchen der `ImageView` Widget:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView suchen](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![ImageView suchen](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

Ziehen Sie diese `ImageView` auf die Entwurfsoberfläche:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView Zeichenbereich](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![ImageView Zeichenbereich](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

Dies `ImageView` wird verwendet, um Farbe in dieser Farbe-Browser-app angezeigt.

Ziehen Sie anschließend eine `LinearLayout (Vertical)` Widget aus der **Toolbox** in den Designer. Beachten Sie, dass eine blaue Kontur gibt die Grenzen der hinzugefügten an `LinearLayout`, und die **Dokumentgliederung** an, dass der direkt unter dem er sich `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Blauen Rahmen](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Blauen Rahmen](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

Bei Auswahl der `ImageView` im Designer bewegt der blaue Rahmen zum Umschließen der `ImageView`, in der **Dokumentgliederung**, die Auswahl wechselt zum `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wählen Sie ImageView](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Wählen Sie ImageView](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

Ziehen Sie anschließend eine `Text (Large)` Widget aus der **Toolbox** in der neu hinzugefügten `LinearLayout`. Beachten Sie, dass der Designer Grün verwendet wird hervorgehoben, um anzugeben, wo das neue Widget eingefügt wird:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Grün hervorgehoben.](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Grün hervorgehoben.](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

Als Nächstes fügen Sie eine `Text (Small)` Widget unten die `Text (Large)` Widget:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kleine Text Widget "hinzufügen"](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Kleine Text Widget "hinzufügen"](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

An diesem Punkt sollte der Designer den folgenden Screenshot ähneln:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Layout für Aktivitätsdesigner](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Layout für Aktivitätsdesigner](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

Wenn die beiden `textView` Widgets befinden sich nicht in `linearLayout1`, ziehen Sie Sie sie auf `linearLayout1` in der **Dokumentgliederung** und positionieren Sie sie an, damit sie angezeigt werden, wie in der vorherigen Bildschirmaufnahme dargestellt (eingezogen unter `linearLayout1`).



### <a name="arranging-the-user-interface"></a>Anordnen von der Benutzeroberfläche

Wir ändern die Benutzeroberfläche zum Anzeigen der `ImageView` auf der linken Seite mit den beiden `TextView` Widgets gestapelte rechts neben der `ImageView`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  In der **Fenster "Eigenschaften"**, klicken Sie auf die **nach Kategorien** Symbol und einen Bildlauf bis zum Sortieren der **Layout - ' ViewGroup '** Abschnitt.

3.  Ändern der `layout_width` auf `wrap_content`:

![Legen Sie Wrap Inhalt](designer-walkthrough-images/vs/15-wrap-content.png "Set Wrap Content")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Mit der `ImageView` ausgewählt haben, klicken Sie auf die **Eigenschaften** Registerkarte.

2.  Direkt unterhalb der **Eigenschaften** auf **Layout**.

3.  Führen Sie einen Bildlauf nach unten bis zum **' ViewGroup '** , und ändern Sie die `Width` auf `wrap_content`:

[![Set-Wrap-Inhalt](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

Eine weitere Möglichkeit zum Ändern der `Width` Einstellung wird auf das Dreieck auf der rechten Seite des Widgets seine breiteneinstellung zu aktivieren bzw. deaktivieren `wrap_content`:

[![Ziehen Sie zum Festlegen der Breite](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

Klicken Sie erneut auf das Dreieck gibt die `Width` auf `match_parent`.

Als Nächstes wechseln Sie zu der **Dokumentgliederung** , und wählen Sie im Stammverzeichnis `LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wählen Sie das Stammverzeichnis LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Wählen Sie das Stammverzeichnis LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Mit dem Stamm `LinearLayout` ausgewählt ist, zurück an die **Eigenschaften** Fenster, klicken Sie auf die **alphabetisch** Symbol, und führen Sie einen Bildlauf zu sortieren, bis Sie gefunden `orientation`. Ändern der `orientation` auf `horizontal`:

![Wählen Sie die horizontalen Ausrichtung](designer-walkthrough-images/vs/17-horizontal-orientation.png "wählen Sie die horizontalen Ausrichtung")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Mit dem Stamm `LinearLayout` ausgewählt ist, zurück an die **Eigenschaften** Registerkarte, und klicken Sie auf **Widget**. Ändern der `Orientation` auf `horizontal`:

[![Wählen Sie die horizontalen Ausrichtung](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

An diesem Punkt sollte der Designer den folgenden Screenshot ähneln:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Layout für Aktivitätsdesigner](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Layout für Aktivitätsdesigner](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>Ändern den Abstand

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ändern Sie als Nächstes Ränder und Abstände Einstellungen in der Benutzeroberfläche, um mehr Abstand zwischen den Widgets bereitzustellen. Wählen Sie die `ImageView`, klicken Sie auf die **nach Kategorien** Symbol "Suche" in der **Eigenschaften** Fenster und führen Sie einen Bildlauf nach unten, um die **Layout** Abschnitt. Ändern der `Min Height` auf `70dp`, die `Min Width` auf `50dp`, und die `padding` auf `10dp`. Dies gilt Abstands um alle Seiten des der `ImageView` und es vertikal elongates:

[![Legen Sie die Auffüllung](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Ändern Sie als Nächstes Ränder und Abstände Einstellungen in der Benutzeroberfläche, um mehr Abstand zwischen den Widgets bereitzustellen. Wählen Sie die `ImageView` , und klicken Sie auf die **Layout** Registerkarte **Eigenschaften**. Ändern der `Padding` auf `10dp`, die `Min Width` auf `50dp`, und die `Min Height` auf `70dp`. Dies gilt Abstands um alle Seiten des der `ImageView` und es vertikal elongates:

[![Legen Sie die Auffüllung](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Mit der rechten Maustaste unten, links und Einstellungen Auffüllung oben kann unabhängig voneinander festgelegt werden, durch Eingeben von Werten in der `paddingBottom`, `paddingLeft`, `paddingRight`, und `paddingTop` Feldern.
Legen Sie z. B. die `paddingLeft` Feld `5dp` und `paddingBottom`, `paddingRight`, und `paddingTop` Felder `10dp`:

[![Benutzerdefinierte Einstellungen](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die oben, rechts, unten und Links padding Einstellungen einzeln festgelegt werden können durch Eingeben von Werten in der `Top`, `Right`, `Bottom`, und `Left` Auffüllung der Felder, bzw. Legen Sie z. B. die `Left` padding-Wert, der `5dp` und `Top`, `Right`, und `Bottom` Auffüllung von Werten, `10dp`. Beachten Sie, dass die `Padding` Einstellung ändert sich in einer durch Trennzeichen getrennte Liste der folgenden Werte:

[![Benutzerdefinierte Einstellungen](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Optimieren Sie die Position des als Nächstes die `LinearLayout` Widget, die die beiden enthält `TextView` Widgets. In der **Dokumentgliederung**Option `linearLayout1`. In der **Eigenschaften** Fenster einen Bildlauf zu der **Layout - ' ViewGroup '** Abschnitt. Legen Sie `layout_marginBottom`, `layout_marginLeft`, `layout_marginRight`, und `layout_marginTop` auf `5dp`, `5dp`, `0dp`, und `5dp` bzw.:

[![Festlegen der Seitenränder](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Optimieren Sie die Position des als Nächstes die `LinearLayout` Widget, die die beiden enthält `TextView` Widgets. In der **Dokumentgliederung**Option `linearLayout1`. In der **Eigenschaften** klicken Sie im Bereich der **Layout** Registerkarte. Führen Sie einen Bildlauf nach unten zu der **' ViewGroup '** Abschnitt, und legen Sie die `Left`, `Top`, `Right`, und `Bottom` Ränder um `5dp`, `5dp`, `0dp`, und `5dp` bzw.:

[![Festlegen der Seitenränder](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>Entfernen das Standardabbild

Da wir verwenden, die `ImageView` Farben (anstelle von Bildern) anzeigen, sehen wir entfernen Bildquelle standardmäßig von der Vorlage hinzugefügt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  In **Eigenschaften**, suchen die `src` Feld.

3.  Deaktivieren der `src` festlegen, sodass es leer ist:

![Deaktivieren Sie die Einstellung der ImageView Src](designer-walkthrough-images/vs/22-clear-img-src.png "ImageView-Src-Einstellung deaktivieren")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Wählen Sie das `ImageView`-Steuerelement aus.

2.  Klicken Sie auf die **Widget** Registerkarte **Eigenschaften**.

3.  Deaktivieren der `Src` festlegen, sodass es leer ist:

[![Deaktivieren Sie die ImageView Src-Einstellung](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>Hinzufügen eines Containers ListView

Nun, dass die **"ListItem"** Layout definiert ist, wir fügen eine `ListView` in die Main-Layout. Dies `ListView` enthält eine Liste der **ListItems**.
In der **ToolBox**, suchen Sie die `ListView` Widget und ziehen Sie sie auf der Entwurfsoberfläche angezeigt. Die `ListView` im Designer werden außer blaue Linien, die eine Rahmenlinie beschreiben, wenn diese Option ausgewählt ist. Sehen Sie die **Dokumentgliederung** zu überprüfen, ob die **ListView** ordnungsgemäß hinzugefügt wurde:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Neue ListView](designer-walkthrough-images/vs/23-new-listview.png "neue ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Neue ListView](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

Wird standardmäßig die `ListView` erhält eine `Id` Wert `@+id/listView1`.
Öffnen der **Widget** Registerkarte **Eigenschaften** , und ändern Sie die `Id` auf `@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Benennen Sie die Id in MyListView](designer-walkthrough-images/vs/24-change-id.png "MyListView Rename-Id")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Benennen Sie die Id in myListView](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

An diesem Punkt ist unsere Benutzeroberfläche einsatzbereit.



### <a name="running-the-application"></a>Ausführen der Anwendung


Open **MainActivity.cs** und Ersetzen Sie den Code durch Folgendes:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
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
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
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

Dieser Code verwendet eine benutzerdefinierte `ListView` Adapter Farbinformationen laden und Anzeigen von Daten in der Benutzeroberfläche haben soeben erstellt wurde. Um dieses Beispiel kurz zu halten, die Farbinformationen ist hartcodiert in einer Liste, aber der Adapter kann geändert werden, Farbe-Informationen aus einer Datenquelle extrahiert werden, oder es im Handumdrehen zu berechnen. Weitere Informationen zu `ListView` -Adapter, finden Sie unter [Listenansichten und Adapter](~/android/user-interface/layouts/list-view/index.md).

Erstellen Sie die Anwendung, und führen Sie sie aus. Der folgende Screenshot ist ein Beispiel, wie die app angezeigt wird, wenn auf einem Gerät ausführen:

[![Endgültigen Screenshots](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>Zusammenfassung

In diesem Artikel durchlaufen, wird durch die Schritte zum Xamarin.Android-Designer in Visual Studio für Mac verwenden, um eine Benutzeroberfläche zu erstellen. Als Nächstes wurde es veranschaulicht, wie die Schnittstelle für ein einzelnes Element in einer Liste erstellen.
Nebenbei erläutert wir, wie Sie Widgets hinzufügen und wie sie das visuelle Layout, und wie um Ressourcen zuzuweisen und verschiedene Eigenschaften für diese Widgets festlegen. Zusammenfassung, können wir veranschaulicht, wie die Schnittstelle, das im Designer erstellt wurde in einer beispielanwendung ausgeführt wird.
