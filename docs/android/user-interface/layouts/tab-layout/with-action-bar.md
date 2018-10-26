---
title: Layouts im Registerkartenformat mit ActionBar
description: Dieses Handbuch stellt, und es wird erläutert, wie die ActionBar-APIs verwenden, um eine Benutzeroberfläche mit Registerkarten in einer Xamarin.Android-Anwendung zu erstellen.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: af5554d08ac6c45fc0c392bd17cef5d91251bb1a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106506"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Layouts im Registerkartenformat mit ActionBar

_Dieses Handbuch stellt, und es wird erläutert, wie die ActionBar-APIs verwenden, um eine Benutzeroberfläche mit Registerkarten in einer Xamarin.Android-Anwendung zu erstellen._


## <a name="overview"></a>Übersicht

Die Aktionsleiste ist ein Android-Benutzeroberflächen-Muster, das verwendet wird, um eine einheitliche Benutzeroberfläche für wichtige Features wie z. B. Registerkarten, Anwendungsidentität, Menüs und Suche bereitzustellen. In Android 3.0 (API-Ebene 11), eingeführt Google die ActionBar-APIs für die Android-Plattform. Die ActionBar-APIs führen Benutzeroberflächendesigns Bereitstellen eines konsistenten Aussehens und Verhaltens und Klassen, die im Registerkartenformat Benutzeroberflächen zu ermöglichen. Dieser Leitfaden erläutert, wie eine Xamarin.Android-Anwendung Aktionsleiste Registerkarten hinzugefügt. Es wird erläutert, wie der Android-Unterstützungsbibliothek v7 Backport ActionBar Registerkarten für Xamarin.Android-Anwendungen für Android 2.1 auf Android 2.3 verwenden. 

Beachten Sie, dass `Toolbar` ist eine neuere und allgemeineres Aktion Leiste-Komponente, die Sie nicht verwenden, sollten `ActionBar` (`Toolbar` wurde entworfen, um ersetzen `ActionBar`). Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="requirements"></a>Anforderungen

Alle Xamarin.Android-Anwendung, die Zielversion der API-Ebene 11 (Android 3.0) oder höher verfügt über Zugriff auf die ActionBar-APIs, die im Rahmen der systemeigenen Android-APIs. 

Einige der ActionBar APIs wurden wieder auf API-Ebene 7 (Android 2.1) portiert und können über die [V7 AppCompat-Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat), die Xamarin.Android-apps zur Verfügung gestellt wird die [Xamarin Android Support-Bibliothek - V7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket.



## <a name="introducing-tabs-in-the-actionbar"></a>Einführung in die Registerkarten in die ActionBar

Die Aktionsleiste versucht, alle Registerkarten gleichzeitig anzeigen, und stellen alle Registerkarten Größe basierend auf der Breite der größtmögliche registerkartenbezeichnung. Dies ist im folgenden Screenshot dargestellt: 

![Beispielscreenshot des ActionBar mit allen gleich große Registerkarten angezeigt](with-action-bar-images/image1.png)

Wenn die ActionBar alle Registerkarten anzeigen kann, wird es die Registerkarten in einer horizontal bildlauffähigem einrichten. Der Benutzer möglicherweise navigieren Sie nach links oder rechts, um die verbleibenden Registerkarten anzuzeigen. Dieser Screenshot von Google Play zeigt ein Beispiel hierfür: 

![Beispielhafter Screenshot der Registerkarten in einer horizontal bildlauffähigem](with-action-bar-images/image2.png)

Jede Registerkarte in der Aktionsleiste zugeordnet sein sollte eine [ *Fragment*](~/android/platform/fragments/index.md). Wenn der Benutzer eine Registerkarte auswählt, zeigt die Anwendung das Fragment, das von der Registerkarte "zugeordnet ist. Die ActionBar ist nicht verantwortlich für das entsprechende Fragment dem Benutzer angezeigt. Stattdessen benachrichtigt ActionBar eine Anwendung über Änderungen des Ansichtszustands auf einer Registerkarte durch eine Klasse, die die ActionBar.ITabListener-Schnittstelle implementiert. Diese Schnittstelle bietet drei Rückrufmethoden, die Android aufruft, wenn sich der Zustand der Registerkarte ändert: 

-  **OnTabSelected** – diese Methode wird aufgerufen, wenn der Benutzer auf die Registerkarte auswählt. Sie sollten das Fragment angezeigt werden.

-  **OnTabReselected** – diese Methode wird aufgerufen, wenn die Registerkarte bereits ausgewählt ist, aber vom Benutzer wieder aktiviert wird. Dieser Rückruf wird normalerweise verwendet, das Fragment angezeigte Aktualisierung/zu aktualisieren.

-  **OnTabUnselected** – diese Methode wird aufgerufen, wenn der Benutzer eine andere Registerkarte auswählt. Dieser Rückruf wird verwendet, um den Status im angezeigten Fragment speichern, bevor er gelöscht wird.

Xamarin.Android dient als Wrapper für die `ActionBar.ITabListener` mit Ereignissen, die auf die `ActionBar.Tab` Klasse. Anwendungen können eine oder mehrere dieser Ereignisse Ereignishandlern zuweisen. Es gibt drei Ereignisse (eine für jede Methode in `ActionBar.ITabListener`), die eine Aktion Leiste-Registerkarte wird auslösen: 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>Hinzufügen von Registerkarten, das die ActionBar

Die ActionBar ist native in Android 3.0 (API-Ebene 11) und höher und für jede Xamarin.Android-Anwendung, die diese API mindestens ausgerichtet ist verfügbar. 

Die folgenden Schritte veranschaulichen, wie eine Android-Aktivität ActionBar Registerkarten hinzugefügt: 

1. In der `OnCreate` Methode einer Aktivität &ndash; *vor der Initialisierung keine UI-Widgets* &ndash; muss eine Anwendung festlegen, die `NavigationMode` auf die `ActionBar` zu `ActionBar.NavigationModeTabs` wie im folgenden Code gezeigt Codeausschnitt:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Erstellen Sie eine neue Registerkarte mit `ActionBar.NewTab()`.

3. Zuweisen von Ereignishandlern, oder stellen Sie ein benutzerdefiniertes `ActionBar.ITabListener` -Implementierung, die auf Ereignisse reagieren, die ausgelöst werden, wenn der Benutzer mit den Registerkarten ActionBar interagiert.

4. Fügen Sie die Registerkarte, die im vorherigen Schritt erstellte die `ActionBar`.


Der folgende Code ist ein Beispiel mithilfe der folgenden Schritte zum Hinzufügen von Registerkarten zu einer Anwendung, die Ereignishandler verwendet, um auf Änderungen des Ansichtszustands reagieren: 

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
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>Ereignis-Handler Vs ActionBar.ITabListener

Anwendungen sollten Ereignishandler verwenden und `ActionBar.ITabListener` für verschiedene Szenarien. Ereignishandler bieten eine gewisse syntaktische Bequemlichkeit; Speichern sie Sie erspart, erstellen Sie eine Klasse und implementieren Sie `ActionBar.ITabListener`. Diese Vereinfachung mit Nachteilen verbunden &ndash; Xamarin.Android führt diese Transformation für eine Klasse erstellen und implementieren Sie `ActionBar.ITabListener` für Sie. Dies ist in Ordnung, wenn eine Anwendung eine begrenzte Anzahl von Registerkarten verfügt. 

Beim Umgang mit vieler Registerkarten oder Freigabe von allgemeinen Funktionen zwischen ActionBar Registerkarten, im Hinblick auf Speicher und Leistung, eine benutzerdefinierte Klasse erstellen, implementiert eine effizientere `ActionBar.ITabListener`, und Teilen eine einzelne Instanz der Klasse. Dies reduziert die Anzahl der GREF, die eine Xamarin.Android-Anwendung verwendet werden. 



### <a name="backwards-compatibility-for-older-devices"></a>Rückwärts-Kompatibilität für ältere Geräte

Die [Android Support-Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Back ports ActionBar Registerkarten aus, um Android 2.1 (API-Ebene 7). Registerkarten sind in einer Xamarin.Android-Anwendung zugegriffen werden kann, nachdem diese Komponente zum Projekt hinzugefügt wurde.

Um die ActionBar verwenden zu können, muss eine Aktivität als Unterklasse `ActionBarActivity` und Verwenden des AppCompat-Designs, wie im folgenden Codeausschnitt gezeigt:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Eine Aktivität kann einen Verweis auf die ActionBar aus erhalten die `ActionBarActivity.SupportingActionBar` Eigenschaft. Der folgende Codeausschnitt zeigt ein Beispiel zum Einrichten der ActionBar in einer Aktivität:

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

In diesem Handbuch erläutert die Vorgehensweise: erstellen eine Benutzeroberfläche mit Registerkarten in eine xamarin.Android-Anwendung an, das die ActionBar verwenden. Wir behandelt, wie die ActionBar Registerkarten hinzugefügt und die Interaktion zwischen einer Aktivitäts und Registerkarte Ereignisse über die `ActionBar.ITabListener` Schnittstelle. Wir haben auch gesehen, wie Android Support-Bibliothek v7 AppCompat-Paket-Backports ActionBar auf ältere Versionen von Android Registerkarten. 


## <a name="related-links"></a>Verwandte Links

- [ActionBarTabs (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md)
- [Fragmente](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Muster für Aktionsleiste](http://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Unterstützung von Xamarin.Android Bibliothek v7 AppCompat-NuGet-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
