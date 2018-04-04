---
title: Exemplarische Vorgehensweise – erstellen einer Benutzeroberflächenautomatisierungs mit Registerkarten mit TabHost
description: Dieser Artikel leitet Sie durch Erstellen einer Benutzeroberfläche im Registerkartenformat Xamarin.Android mithilfe der TabHost-API.
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: ca9a3f3d31707205cdcd4e0d8e74fa303ccba047
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>Exemplarische Vorgehensweise – erstellen einer Benutzeroberflächenautomatisierungs mit Registerkarten mit TabHost

_Dieser Artikel leitet Sie durch Erstellen einer Benutzeroberfläche im Registerkartenformat Xamarin.Android mithilfe der TabHost-API._

> [!NOTE]
> `TabHost` ist eine alte API, die von Google als veraltet markiert wurden. Entwicklern wird empfohlen, die im Registerformat mithilfe Anwendungen erstellen, die [ActionBar](~/android/user-interface/controls/action-bar.md). Die `ActionBar` in allen Android-Version verfügbar ist. Es wurde erstmals in Android 3.0 (API-Ebene 11) und portiert wurde wieder Android 2.2 (API-Ebene 8) und Android 2.3 (API-Ebene 10) in der [V7 AppCompat Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat), steht auf Xamarin.Android über die [Xamarin Android-Unterstützungsbibliothek – V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket.

Dieser Artikel leitet Sie durch Erstellen einer Benutzeroberflächenautomatisierungs mit Registerkarten im Xamarin.Android mithilfe der `TabHost` API. Dies ist eine ältere API, die in allen Versionen von Android verfügbar ist. In diesem Beispiel wird eine Anwendung mit drei Registerkarten, mit der Logik für die einzelnen Registerkarten, die in einer Aktivität verkapselt erstellt.
Der folgende Screenshot ist ein Beispiel für die Anwendung, die wir erstellen:

![Beispiel-Screenshot der app mit mehreren Registerkarten](creating-a-tabbed-ui-images/image02.png)


## <a name="creating-the-application"></a>Erstellen der Anwendung

Herunterladen und entzippen [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/).
Dieses Projekt dient als Ausgangspunkt für die Anwendung, und einige Bilder enthält. Wenn Sie dieses Projekt untersuchen, sehen Sie sich, dass wir die zeichenbaren Ressourcen für die Registerkarte "-Symbole bereits erstellt haben.

Erste aktualisieren wir die Layoutdatei **Resources/Layout/Main.axml** , die die Registerkarten hostet. Bearbeiten Sie **Resources/Layout/Main.axml** , und fügen Sie die folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

Das folgende Bildschirmfoto zeigt das Layout im Xamarin-Designer:

[![Screenshot des Layouts TabHost im Xamarin-Designer](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png#lightbox)

Die TabHost benötigen zwei untergeordnete Ansichten darin: eine `TabWidget` und ein `FrameLayout`. Position der `TabWidget` und `FrameLayout` vertikal innerhalb der `TabHost`, eine `LinearLayout` verwendet wird. Die FrameLayout ist, in dem der Inhalt für jede Registerkarte geht also leer, da die `TabHost` wird jede Aktivität zur Laufzeit automatisch eingebettet. Es gibt mehrere Regeln, die beachtet werden müssen, wenn es darum geht, das Layout im Registerkartenformat Benutzeroberflächen erstellen:

-   Die `TabHost` benötigen Sie die Id `@android:id/tabhost`.

-   Die `TabWidget` benötigen Sie die Id `@android:id/tabs`.

-   Die `FrameLayout` benötigen Sie die Id `@android:id/tabcontent`.

-   `TabHost` erfordert Aktivität, die er verwaltet zu vererben `TabActivity`. Daher ist es wichtig, Unterklasse `TabActivity` hier &ndash; eine reguläre Aktivität funktioniert nicht.

Bearbeiten Sie die Datei **MainActivity.cs** , damit die Klasse `MainActivity` Unterklassen `TabActivity` wie im folgenden Codeausschnitt gezeigt:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

Erstellen von vier separaten Aktivität Klassen in Ihrem Projekt: `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, und `WhatsOnActivity`. Die Benutzeroberfläche einer Registerkarte wird jede Aktivität bilden. Für jetzt diese Aktivitäten einen Stub sein, die zeigt eine `TextView` mit einer einfachen Meldung. Bearbeiten Sie den Code in jeder Aktivität, die Folgendes enthalten `OnCreate` Implementierung:

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

Beachten Sie, dass der obige Code eine Layoutdatei nicht verwendet. Erstellt nur eine `TextView` mit Text, und legt fest, die `TextView` als der Inhaltsansicht. Duplizieren Sie dies für die verbleibenden drei Aktivitäten.

Als Nächstes weisen wir die Symbole zur Registerkarte "Jeder" zu. Jede Registerkarte erfordert zwei Symbole &ndash; eine für den ausgewählten Zustand und eine für den nicht aktivierten Status. Ein Beispiel für diese zwei unterschiedliche Symbole kann in die folgenden beiden Abbilder angezeigt werden (die erforderlichen Symbole für diese Anwendung bereits das Beispielprojekt hinzugefügt wurde):

![Screenshot des ausgewählten Zustand und die Symbole nicht ausgewählt](creating-a-tabbed-ui-images/tab-icons.png)


Wir werden die Registerkarten Symbol zeichenbaren Ressourcen zuweisen, indem Sie definieren eine *ListeZustand Drawable*. Liste Zustand Drawables sind eine besondere zeichenbaren Ressourcen, die in XML definiert werden und ermöglichen es Ihnen, verschiedene Bilder angeben, die spezifisch für den Zustand des Elements sind. In diesem Beispiel besteht ein Bild, das verwendet wird, wenn eine Registerkarte ausgewählt ist, und ein anderes, das verwendet wird, wenn die Registerkarte nicht ausgewählt ist. Sie Zeit sparen haben erforderlichen Zustand-List-Drawables zum Projekt hinzugefügt wurde. Die folgende Liste enthält die Dateien und die XML enthält:

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

Registerkarten werden hinzugefügt, um die `TabHost` programmgesteuert, die eine sehr sich wiederholende Aufgabe ist. Fügen Sie die folgende Methode der Klasse, um dabei zu helfen, `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

Jede Registerkarte in der `TabHost` wird dargestellt, von einer Instanz von den von der `TabHost.TabSpec` Klasse. Diese Instanz enthält die Metadaten die Registerkarte speziell gerendert:

-   **Text und Symbol** &ndash; anzuzeigenden der `TabWidget`.

-   **Registerkarte Inhalt** &ndash; dabei kann es sich um eine `Activity` oder ein `View` und wird angezeigt, wenn die Registerkarte ausgewählt ist.

-   **Eindeutiges Tag** &ndash; für jede Registerkarte muss ein eindeutiges Tag zugewiesen werden.

Wir müssen Hinzufügen einer `TabHost.TabSpec` -Instanz für jede Registerkarte in der vorliegenden Anwendung. Können dies im nächsten Schritt.

Aktualisieren Sie die Methode `OnCreate` in `MainActivity` , damit sie den folgenden Code ähnelt:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

Führen Sie die Anwendung aus. Ihre Anwendung sollte am Anfang des in dieser exemplarischen Vorgehensweise gezeigten Screenshot aussehen.

Das ist alles! Wir haben eine Anwendung im Registerkartenformat erstellt, die den Benutzer eine einfache Möglichkeit navigieren Sie zu unterschiedlichen Teilen einer Anwendung gewährt.



## <a name="summary"></a>Zusammenfassung

In diesem Kapitel im Registerkartenformat Layouts erörtert, und Sie durch Prozess der Erstellung einer Anwendung im Registerkartenformat geführt. Die exemplarische Vorgehensweise veranschaulicht, wie eine `TabActivity` vergrößert werden soll ein Layout Datei, hostet ein `TabHost` und ein `TabWidget`. Die `TabHost` wurde daraufhin eine Auflistung von `TabHost.TabSpec` -Objekte, die von verwendet werden würde die `TabHost` zur Laufzeit, um die Aktivitäten zu instanziieren, die auf jeder Registerkarte verwendet werden würde.



## <a name="related-links"></a>Verwandte Links

- [TabHostWalkthrough (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [NuGet-Paket für Android, unterstützen Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 Appcompat-Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
