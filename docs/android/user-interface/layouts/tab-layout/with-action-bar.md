---
title: Im Registerkartenformat Layouts mit den ActionBar
description: "Dieses Handbuch stellt, und es wird erläutert, wie die ActionBar-APIs verwenden, um eine Schnittstelle im Registerkartenformat Benutzer in einer Anwendung Xamarin.Android zu erstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: afaa02168dcac54115e8fca53683725926e4baed
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Im Registerkartenformat Layouts mit den ActionBar

_Dieses Handbuch stellt, und es wird erläutert, wie die ActionBar-APIs verwenden, um eine Schnittstelle im Registerkartenformat Benutzer in einer Anwendung Xamarin.Android zu erstellen._


## <a name="overview"></a>Übersicht

Die Aktionsleiste ist eine Android-UI-Muster, das verwendet wird, um eine einheitliche Benutzeroberfläche für wichtige Features wie die Registerkarten, Identität, Menüs und Suche bereitzustellen. Android 3.0 (API-Ebene 11) eingeführt Google ActionBar-APIs für die Android-Plattform. Die ActionBar APIs einführen UI Designs zur Bereitstellung eines konsistenten Aussehens und von Klassen, die im Registerkartenformat Benutzeroberflächen zu ermöglichen. Dieses Handbuch erläutert, wie eine Anwendung Xamarin.Android Aktionsleiste Registerkarten hinzu. Es wird erläutert, wie die Android-Unterstützungsbibliothek v7 Backport ActionBar Registerkarten Xamarin.Android von Anwendungen für Android 2.1, die auf Android 2.3 verwenden. 

Beachten Sie, dass `Toolbar` ist eine neuere und allgemeineren Aktion Leiste-Komponente, mit denen Sie statt der sollten `ActionBar` (`Toolbar` wurde entwickelt, um ersetzen `ActionBar`). Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="requirements"></a>Anforderungen

Jede Xamarin.Android-Anwendung, die Ziel-API-Ebene 11 (Android 3.0) oder höher, hat Zugriff auf die ActionBar-APIs im Rahmen der systemeigenen Android-APIs. 

Einige der ActionBar APIs wurden wieder auf API-Ebene 7 (Android 2.1) portiert und können über die [V7 AppCompat Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat), dem Xamarin.Android apps über zur Verfügung gestellt der [Xamarin Android-Unterstützungsbibliothek – V7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket.



## <a name="introducing-tabs-in-the-actionbar"></a>Einführung in die Registerkarten in der ActionBar

Die Aktionsleiste versucht, alle Registerkarten gleichzeitig anzeigen, und stellen alle Registerkarten basierend auf die Breite des breitesten registerkartenbezeichnung Größe. Dies wird durch das folgende Bildschirmfoto veranschaulicht: 

![Beispiel-Screenshot des ActionBar mit allen gleich große Registerkarten angezeigt](with-action-bar-images/image1.png)

Wenn die ActionBar alle Registerkarten anzeigen kann, wird er die Registerkarten in einer Ansicht mit horizontal bildlauffähigem einrichten. Der Benutzer möglicherweise navigieren Sie nach links oder rechts, um die verbleibenden Registerkarten anzuzeigen. Diese bildschirmabbildung von Google Play zeigt ein Beispiel dafür: 

![Beispiel-Screenshot der Registerkarten in einer Ansicht mit horizontal bildlauffähigem](with-action-bar-images/image2.png)

Jede Registerkarte in der Aktionsleiste zugeordnet sein sollte eine [ *Fragment*](~/android/platform/fragments/index.md). Wenn der Benutzer auf eine Registerkarte auswählt, wird die Anwendung das Fragment angezeigt, das der Registerkarte "zugeordnet ist. Die ActionBar ist nicht verantwortlich für das entsprechende Fragment dem Benutzer angezeigt. Stattdessen benachrichtigt den ActionBar eine Anwendung zu statusänderungen in einer Registerkarte über eine Klasse, die die ActionBar.ITabListener-Schnittstelle implementiert. Diese Schnittstelle bietet drei Rückrufmethoden, mit denen Android aufgerufen wird, bei der statusänderung von der Registerkarte ": 

-  **OnTabSelected** – diese Methode wird aufgerufen, wenn der Benutzer die Registerkarte auswählt. Das Fragment sollte angezeigt werden.

-  **OnTabReselected** – diese Methode wird aufgerufen, wenn die Registerkarte bereits ausgewählt ist, aber vom Benutzer wieder aktiviert wird. Dieser Rückruf wird normalerweise verwendet, die angezeigten Fragment aktualisieren/zu aktualisieren.

-  **OnTabUnselected** – diese Methode wird aufgerufen, wenn der Benutzer eine andere Registerkarte auswählt. Dieser Rückruf wird verwendet, um den Status im angezeigten Fragment speichern, bevor er gelöscht wird.

Xamarin.Android dient als Wrapper für die `ActionBar.ITabListener` mit Ereignissen auf der `ActionBar.Tab` Klasse. Anwendungen können eine oder mehrere dieser Ereignisse Ereignishandler zuweisen. Es gibt drei Ereignisse (eine für jede Methode in `ActionBar.ITabListener`), die eine Aktion Leiste Registerkarte löst: 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>Hinzufügen von Registerkarten, um die ActionBar

Die ActionBar ist native Android 3.0 (API-Ebene 11) und höher und für jede Xamarin.Android-Anwendung, die diese API mindestens ausgerichtet ist verfügbar. 

Die folgenden Schritte veranschaulichen, wie eine Aktivität Android ActionBar Registerkarten hinzugefügt: 

1. In der `OnCreate` Methode einer Aktivität &ndash; *vor dem Initialisieren alle UI-Widgets* &ndash; muss eine Anwendung festlegen, die `NavigationMode` auf die `ActionBar` auf `ActionBar.NavigationModeTabs` wie in diesem Code gezeigt Codeausschnitt:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Erstellen Sie eine neue Registerkarte mit `ActionBar.NewTab()`.

3. Weisen Sie Ereignishandler, oder geben Sie eine benutzerdefinierte `ActionBar.ITabListener` Implementierung, die die Ereignisse reagiert, die ausgelöst werden, wenn der Benutzer mit den Registerkarten ActionBar interagiert.

4. Die Registerkarte, die im vorherigen Schritt erstellt wurde "hinzufügen" die `ActionBar`.


Der folgende Code ist ein Beispiel mit den folgenden Schritten zum Hinzufügen von Registerkarten zu einer Anwendung, die Ereignishandler für die Reaktion auf Zustandsänderungen verwendet: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       }
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       }
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>Ereignis-Handler Vs ActionBar.ITabListener

Anwendungen sollten Ereignishandler verwenden und `ActionBar.ITabListener` für verschiedene Szenarien. Ereignishandler bieten eine bestimmte Menge an syntaktische halber; Speichern sie Sie nicht mehr zum Erstellen einer Klasse und implementieren `ActionBar.ITabListener`. Diese Vereinfachung ist möglich, eine Kosten &ndash; Xamarin.Android führt diese Transformation für eine Klasse erstellen und implementieren Sie `ActionBar.ITabListener` für Sie. Dies ist gut, wenn eine Anwendung eine begrenzte Anzahl von Registerkarten verfügt. 

Beim Umgang mit vieler Registerkarten oder Freigabe allgemeine Funktionen zwischen den Registerkarten des ActionBar, es kann effizienter sein im Hinblick auf die Arbeitsspeicher- und Leistungsproblemen, eine benutzerdefinierte Klasse zu erstellen, implementiert `ActionBar.ITabListener`, und eine einzelne Instanz der Klasse freigeben. Dies reduziert die Anzahl der GREF, die eine Anwendung Xamarin.Android verwendet wird. 



### <a name="backwards-compatibility-for-older-devices"></a>Abwärtskompatibilität-Kompatibilität für ältere Geräte

Die [Android Unterstützungsbibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Back ports ActionBar Registerkarten, um Android 2.1 (API-Ebene 7). Registerkarten sind in einer Anwendung Xamarin.Android zugegriffen werden kann, sobald diese Komponente zum Projekt hinzugefügt wurde.

Um die ActionBar verwenden zu können, muss eine Aktivität Unterklasse `ActionBarActivity` und das Design AppCompat verwenden, wie im folgenden Codeausschnitt gezeigt:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Eine Aktivität kann einen Verweis auf die ActionBar von erhalten die `ActionBarActivity.SupportingActionBar` Eigenschaft. Der folgende Codeausschnitt zeigt ein Beispiel für das Einrichten der ActionBar in einer Aktivität:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```


## <a name="summary"></a>Zusammenfassung

In diesem Handbuch erläutert wir erstellen eine Benutzeroberfläche im Registerkartenformat in ein Xamarin.Android mithilfe der ActionBar. Es behandelt, wie die ActionBar Registerkarten hinzugefügt und wie eine Aktivität mit der Registerkarte Ereignisse über interagieren kann die `ActionBar.ITabListener` Schnittstelle. Wir haben gesehen, wie Android-Unterstützungsbibliothek v7 AppCompat Paket Backports der ActionBar älteren Versionen von Android Registerkarten. 


## <a name="related-links"></a>Verwandte Links

- [ActionBarTabs (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md)
- [Fragmente](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Aktionsleiste-Muster](http://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android Support Library v7 AppCompat NuGet-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
