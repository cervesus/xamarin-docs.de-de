---
title: Xamarin.Android-Leistung
description: Es gibt viele Techniken zum Verbessern der Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren. In diesem Artikel wird Folgendes erläutert.
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 88e1acecdc96af596a0151bbd3f64dc4547d4cce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753819"
---
# <a name="xamarinandroid-performance"></a>Xamarin.Android-Leistung

_Es gibt viele Methoden zum Verbessern der Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren. In diesem Artikel werden die Methoden beschrieben und erläutert._

## <a name="performance-overview"></a>Überblick über Leistung

Eine schlechte Anwendungsleistung kann sich auf unterschiedliche Weise bemerkbar machen. Die Anwendung reagiert scheinbar nicht mehr, der Bildlauf ist möglicherweise verlangsamt, und auch die Akkulaufzeit kann abnehmen. Leistungsoptimierung umfasst jedoch mehr als das bloße Implementieren eines effizienten Codes. Es muss ebenfalls berücksichtigt werden, wie der Benutzer die Leistung der Anwendung wahrnimmt. Wenn beispielsweise Vorgänge ausgeführt werden können, ohne dass der Benutzer daran gehindert wird andere Aktivitäten auszuführen, kann dies dazu beitragen, die Benutzerfreundlichkeit zu verbessern.

Es gibt viele Techniken zum Verbessern der Leistung und der wahrnehmbaren Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden. Dazu zählen:

- [Optimieren der Layout-Hierarchien](#optimizelayout)
- [Optimieren von Listenansichten](#optimizelistviews)
- [Entfernen von Ereignishandlern in Aktivitäten](#removeeventhandlers)
- [Begrenzen der Lebensdauer von Diensten](#limitservices)
- [Freigeben von Ressourcen bei Benachrichtigung](#releaseresources)
- [Freigeben von Ressourcen, wenn die Benutzeroberfläche ausgeblendet wird](#releaseresourcesuihidden)
- [Optimieren von Bildressourcen](#optimizeimages)
- [Löschen von nicht verwendeten Bildressourcen](#disposeimages)
- [Vermeiden der arithmetischen Gleitkommaoperatoren](#avoidfloats)
- [Schließen von Dialogfeldern](#dismissdialogs)

> [!NOTE]
> Bevor Sie diesen Artikel lesen, sollten Sie zuerst den Artikel [Cross-Platform Performance (Plattformübergreifende Leistung)](~/cross-platform/deploy-test/memory-perf-best-practices.md) lesen, der nicht-plattformspezifische Methoden zur Verbesserung der Arbeitsspeicherauslastung und Leistung von Anwendungen beschreibt, die mit der Xamarin-Plattform erstellt wurden.

<a name="optimizelayout" />

## <a name="optimize-layout-hierarchies"></a>Optimieren der Layout-Hierarchien

Jedes Layout, das zu einer Anwendung hinzugefügt wurde, erfordert Initialisierung, Layout und Zeichnen. Der Layoutdurchlauf kann teuer sein, wenn Sie [`LinearLayout`](xref:Android.Widget.LinearLayout)-Instanzen schachteln, die die `weight`-Parameter verwenden, da jedes untergeordnete Element zweimal gemessen wird. Mithilfe von geschachtelten Instanzen der `LinearLayout` kann eine umfassenden Ansichtshierarchie angezeigt werden. Dies kann zu einer schlechten Leistung von Layouts führen, die mehrere Male vergrößert wird, z.B. in [`ListView`](xref:Android.Widget.ListView). Aus diesem Grund ist es wichtig, dass solche Layouts optimiert sind, da die Leistungsvorteile dann multipliziert werden.

Betrachten Sie beispielsweise die Klasse [`LinearLayout`](xref:Android.Widget.LinearLayout) als Listenansichtszeile mit einem Symbol, einem Titel und einer Beschreibung. Die Klasse `LinearLayout` enthält eine [`ImageView`](xref:Android.Widget.ImageView) und eine vertikale `LinearLayout` enthält zwei [`TextView`](xref:Android.Widget.TextView)-Instanzen:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

Dieses Layout verfügt über 3 Ebenen und ist verschwenderisch, wenn es für jede [`ListView`](xref:Android.Widget.ListView)-Zeile vergrößert wird. Allerdings kann es verbessert werden, indem Sie das Layout vereinfachen, wie im folgenden Codebeispiel gezeigt:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

Die vorherige Hierarchie mit 3 Ebenen wurde auf 2 Ebenen reduziert, und eine einzelne [`RelativeLayout`](xref:Android.Widget.RelativeLayout) ersetzt zwei [`LinearLayout`](xref:Android.Widget.LinearLayout)-Instanzen. Wenn Sie das Layout für jede [`ListView`](xref:Android.Widget.ListView)-Zeile vergrößern, erhalten Sie eine erhebliche Leistungssteigerung.

<a name="optimizelistviews" />

## <a name="optimize-list-views"></a>Optimieren von Listenansichten

Benutzer erwarten einen sanften Bildlauf und schnelle Ladezeiten für [`ListView`](xref:Android.Widget.ListView)-Instanzen. Die Bildlaufleistung kann jedoch beeinträchtigt werden, wenn jede Listenansichtszeile tief geschachtelte Ansichtshierarchien oder komplexe Layouts enthält. Es gibt jedoch Techniken, die verwendet werden können, um eine schlechte `ListView`-Leistung zu vermeiden:

- Zeilenansichten wiederverwenden: Weitere Informationen finden Sie unter [Reuse Row Views (Wiederverwenden von Zeilenansichten)](#reuserowviews).
- Vereinfachen Sie die Layouts, sofern möglich.
- Nehmen Sie eine Zwischenspeicherung des Zeileninhalts vor, der von einem Webdienst abgerufen wird.
- Vermeiden Sie die Bildskalierung.

Zusammenfassend können diese Techniken zum reibungslosen Bildlauf von [`ListView`](xref:Android.Widget.ListView)-Instanzen beitragen.

<a name="reuserowviews" />

### <a name="reuse-row-views"></a>Wiederverwenden von Zeilenansichten

Beim Anzeigen von Hunderten von Zeilen in einer [`ListView`](xref:Android.Widget.ListView)-Instanz, wäre es eine Verschwendung des Arbeitsspeichers, Hunderte von [`View`](xref:Android.Views.View)-Objekten zu erstellen, wenn nur eine kleine Anzahl von ihnen gleichzeitig auf dem Bildschirm angezeigt wird. Stattdessen können nur die `View`-Objekte, die in den Zeilen auf dem Bildschirm sichtbar sind, in den Arbeitsspeicher geladen werden, und der **Inhalt** wird in diese wiederverwendeten Objekte geladen. Dies verhindert die Instanziierung von Hunderten von zusätzlichen Objekten, wobei sowohl Zeit als auch Arbeitsspeicher gespart wird.

Aus diesem Grund kann eine Zeile, wenn sie vom Bildschirm verschwindet, in einer Warteschlange für die Wiederverwendung platziert werden, wie im folgenden Codebeispiel gezeigt wird:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Wenn der Benutzer einen Bildlauf durchführt, ruft die [`ListView`](xref:Android.Widget.ListView) die `GetView`-Überschreibung zur Anforderung neuer Ansichten auf. Gegebenenfalls wird eine nicht verwendete Ansicht im `convertView`-Parameter übergeben. Wenn dieser Wert `null` entspricht, dann erstellt der Code anschließend eine neue [`View`](xref:Android.Views.View)-Instanz. Andernfalls können die `convertView`-Eigenschaften zurückgesetzt und wiederverwendet werden.

Weitere Informationen finden Sie unter [Row View Re-Use (Wiederverwenden der Zeilenansicht)](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use) in [Populating a ListView with Data (Eine Listenansicht mit Daten auffüllen)](~/android/user-interface/layouts/list-view/populating.md).

<a name="removeeventhandlers" />

## <a name="remove-event-handlers-in-activities"></a>Entfernen von Ereignishandlern in Aktivitäten

Wenn eine Aktivität in der Android-Laufzeit zerstört wird, kann sie immer noch in der Mono-Laufzeit aktiv sein. Entfernen Sie deshalb Ereignishandler für externe Objekte in `Activity.OnPause`, um zu verhindern, dass die Laufzeit einen Verweis auf eine Aktivität beibehält, die zerstört wurde.

Deklarieren Sie einen oder mehrere Ereignishandler in einer Aktivität auf Klassenebene:

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

Implementieren Sie dann die Handler in der Aktivität, z.B. in `OnResume`:

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

Wenn die Aktivität den Ausführungsstatus beendet, wird `OnPause` aufgerufen. Entfernen Sie in der `OnPause`-Implementierung die Handler wie folgt:

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices" />

## <a name="limit-the-lifespan-of-services"></a>Begrenzen der Lebensdauer von Diensten

Wenn ein Dienst gestartet wird, führt Android den Dienstprozess aus. Dadurch wird der Prozess aufwendig, da der Speicher nicht ausgelagert oder an anderer Stelle verwendet werden kann. Wenn Sie einen Dienst weiterhin ausführen, obwohl dies nicht erforderlich ist, wird daher das Risiko erhöht, dass eine Anwendung aufgrund von Arbeitsspeicherbeschränkungen schlechte Leistungen aufweist. Die Anwendungsumschaltung kann ebenfalls weniger effizient sein, da sie die Anzahl von Prozessen reduziert, die Android zwischenspeichern kann.

Die Lebensdauer eines Diensts kann mithilfe eines `IntentService` begrenzt werden. Letzterer beendet sich selbst, sobald sein Ziel erreicht ist.

<a name="releaseresources" />

## <a name="release-resources-when-notified"></a>Freigeben von Ressourcen bei Benachrichtigung

Während des Lebenszyklus stellt der [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*)-Rückruf eine Benachrichtigung zur Verfügung, wenn der Gerätespeicher gering ist. Dieser Rückruf sollte zum Abhören folgender Arbeitsspeicherbenachrichtigungen implementiert werden:

- [`TrimMemoryRunningModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate): Die Anwendung wird *möglicherweise* einige nicht benötigte Ressourcen freigegeben.
- [`TrimMemoryRunningLow`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningLow): Die Anwendung *sollte* einige nicht benötigte Ressourcen freigeben.
- [`TrimMemoryRunningCritical`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical): Die Anwendung *sollte* so viele unkritische Prozesse wie möglich freigeben.

Wenn der Anwendungsprozess zwischengespeichert wird, werden die folgenden Arbeitsspeicherbenachrichtigungen darüber hinaus möglicherweise durch den [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*)-Rückruf empfangen:

- [`TrimMemoryBackground`](xref:Android.Content.ComponentCallbacks2.TrimMemoryBackground): Gibt Ressourcen frei, die schnell und effizient erstellt werden können, wenn der Benutzer zur App zurückkehrt.
- [`TrimMemoryModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryModerate): Durch das Freigeben von Ressourcen können andere Prozesse im System zwischengespeichert werden. Dies sorgt für eine höhere Gesamtleistung.
- [`TrimMemoryComplete`](xref:Android.Content.ComponentCallbacks2.TrimMemoryComplete): Der Anwendungsprozess wird bald beendet, wenn nicht mehr Arbeitsspeicher bald wiederhergestellt wird.

Benachrichtigungen sollten durch die Freigabe von Ressourcen auf der Grundlage der empfangenen Ebenen beantwortet werden.

<a name="releaseresourcesuihidden" />

## <a name="release-resources-when-the-user-interface-is-hidden"></a>Freigeben von Ressourcen, wenn die Benutzeroberfläche ausgeblendet wird

Geben Sie alle Ressourcen frei, die von der Benutzeroberfläche der App verwendet werden, wenn der Benutzer zu einer anderen App navigiert. Dies kann die Android-Kapazität für zwischengespeicherte Prozesse signifikant erhöhen, was sich wiederum auf die Qualität der Benutzerfreundlichkeit auswirken kann.

Um eine Benachrichtigung zu erhalten, wenn der Benutzer die Benutzeroberfläche verlässt, implementieren Sie den [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*)-Rückruf in `Activity`-Klassen, und merken Sie sich die [`TrimMemoryUiHidden`](xref:Android.Content.ComponentCallbacks2.TrimMemoryUiHidden)-Ebene, die angibt, dass die Benutzeroberfläche aus der Ansicht ausgeblendet ist. Diese Benachrichtigung wird nur empfangen, wenn *alle* UI-Komponenten der Anwendung für den Benutzer ausgeblendet werden. Die Freigabe der UI-Ressourcen bei Empfang dieser Benachrichtigung stellt sicher, dass die UI-Ressourcen weiterhin für eine schnelle Fortsetzung der Aktivität verfügbar sind, wenn der Benutzer von einer anderen Aktivität in der App zurückkehrt.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimieren von Bildressourcen

Bilder gehören zu den speicherintensivsten Ressourcen, die Anwendungen verwenden, und werden häufig in hoher Auflösung aufgenommen. Verwenden Sie deshalb für die Bildanzeige die Auflösung, die für den Bildschirm des Geräts erforderlich ist. Wenn das Bild über eine höhere Auflösung als der Bildschirm verfügt, sollte sie nach unten skaliert werden.

Weitere Informationen finden Sie unter [Optimize Image Resources (Optimieren von Bildressourcen)](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) im Leitfaden [Cross Plattform Performance (Plattformübergreifende Leistung)](~/cross-platform/deploy-test/memory-perf-best-practices.md).

<a name="disposeimages" />

## <a name="dispose-of-unused-image-resources"></a>Löschen von nicht verwendeten Bildressourcen

Sie sollten größere Bildressourcen, die Sie nicht mehr benötigen, löschen, um die Speicherauslastung zu reduzieren. Dabei ist es allerdings wichtig, dass Sie darauf achten, dass die Bilder ordnungsgemäß gelöscht werden. Anstelle eines expliziten Aufrufs von `.Dispose()` können Sie [using](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/using-statement)-Anweisungen verwenden, um die ordnungsgemäße Verwendung von `IDisposable`-Objekten zu gewährleisten. 

Beispielsweise implementiert die [Bitmap](xref:Android.Graphics.Bitmap)-Klasse `IDisposable`. Wenn Sie die Instanziierung eines `BitMap`-Objekts in einem `using`-Block umschließen, stellen Sie sicher, dass es beim Beenden des Blocks ordnungsgemäß gelöscht wird:

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

Weitere Informationen zum Freigeben von Ressourcen, die gelöscht werden können, finden Sie unter [Freigeben von IDisposable-Ressourcen](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).  

<a name="avoidfloats" />

## <a name="avoid-floating-point-arithmetic"></a>Vermeiden der arithmetischen Gleitkommaoperatoren

Auf Android-Geräten sind arithmetische Gleitkommaoperatoren ca. doppelt so langsam wie Ganzzahlarithmetik. Ersetzen Sie daher nach Möglichkeit die arithmetischen Gleitkommaoperatoren durch Ganzzahlarithmetik. Es gibt jedoch keinen Unterschied in der Ausführungszeit zwischen der `float` und `double`-Arithmetik auf aktueller Hardware.

> [!NOTE]
> Für Ganzzahlarithmetik fehlt einigen CPUs auch die Fähigkeit, Hardware aufzuteilen. Aus diesem Grund werden Ganzzahldivisionen und Modulooperationen häufig in der Software ausgeführt.

<a name="dismissdialogs" />

## <a name="dismiss-dialogs"></a>Schließen von Dialogfeldern

Rufen Sie bei Verwendung der [`ProgressDialog`](xref:Android.App.ProgressDialog)-Klasse (oder andere Dialogfelder oder Warnungen) anstelle der [`Hide`](xref:Android.App.Dialog.Hide*)-Methode die [`Dismiss`](xref:Android.App.Dialog.Dismiss*)-Methode auf, wenn das Dialogfeld abgeschlossen wurde. Andernfalls bleibt das Dialogfeld aktiv und wird die Aktivität durch einen Verweis darauf preisgeben.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat viele Techniken zum Verbessern der Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden, beschrieben und erläutert. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren.

## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)
