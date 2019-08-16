---
title: Layouts im Registerkarten Format mit der Aktionsleiste
description: In diesem Handbuch wird erläutert und erläutert, wie mit den Aktionsleisten-APIs eine Benutzeroberfläche im Registerkarten Format in einer xamarin. Android-Anwendung erstellt wird.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 4673b178512a886e5fdb154c57c8d659276bb392
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522329"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Layouts im Registerkarten Format mit der Aktionsleiste

_In diesem Handbuch wird erläutert und erläutert, wie mit den Aktionsleisten-APIs eine Benutzeroberfläche im Registerkarten Format in einer xamarin. Android-Anwendung erstellt wird._


## <a name="overview"></a>Übersicht

Die Aktionsleiste ist ein Android-UI-Muster, das verwendet wird, um eine konsistente Benutzeroberfläche für wichtige Features wie Registerkarten, Anwendungs Identität, Menüs und suchen bereitzustellen. In Android 3,0 (API-Ebene 11) hat Google die Aktionsleisten-APIs für die Android-Plattform eingeführt. Die Aktionsleisten-APIs führen Benutzeroberflächen Designs ein, um ein konsistentes Erscheinungsbild und eine konsistente Benutzeroberfläche bereitzustellen. In diesem Handbuch wird erläutert, wie Sie einer xamarin. Android-Anwendung Aktionsleiste Registerkarten hinzufügen. Außerdem wird erläutert, wie Sie die Android-Unterstützungs Bibliothek V7 zum rückportieren von Aktionsleisten-Registerkarten zu xamarin. Android-Anwendungen verwenden, die auf Android 2,1 für Android 2,3 abzielen. 

Beachten Sie `Toolbar` , dass es sich bei um eine neuere und allgemeinere Aktionsleisten Komponente handelt `ActionBar` ,`Toolbar` die anstelle von verwendet `ActionBar`werden soll (wurde zum Ersetzen entworfen). Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="requirements"></a>Anforderungen

Alle xamarin. Android-Anwendungen, die auf API Level 11 (Android 3,0) oder höher ausgerichtet sind, haben Zugriff auf die Aktionsleisten-APIs als Teil der nativen Android-APIs. 

Einige der Aktionsleisten-APIs wurden wieder auf API-Ebene 7 (Android 2,1) portiert und sind über die [Bibliothek "V7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)" verfügbar, die für xamarin. Android-Apps über das [xamarin Android-Support Library-V7-](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket verfügbar gemacht wird.



## <a name="introducing-tabs-in-the-actionbar"></a>Einführung in Registerkarten in der Aktionsleiste

Die Aktionsleiste versucht, alle zugehörigen Registerkarten gleichzeitig anzuzeigen und die Größe aller Registerkarten entsprechend der Breite der breitesten Registerkarten Bezeichnung zu ändern. Dies wird im folgenden Screenshot veranschaulicht: 

![Beispiel Bildschirm Abbildung von "Aktionsleiste" mit allen angezeigten Registerkarten](with-action-bar-images/image1.png)

Wenn die Aktionsleiste nicht alle Registerkarten anzeigen kann, werden die Registerkarten in einer horizontalen scrollbaren Ansicht eingerichtet. Der Benutzer kann nach links oder rechts schwenken, um die restlichen Registerkarten anzuzeigen. Dieser Screenshot von Google Play zeigt ein Beispiel dafür: 

![Beispiel Bildschirm Abbildung von Registerkarten in einer horizontalen scrollbaren Ansicht](with-action-bar-images/image2.png)

Jede Registerkarte in der Aktionsleiste sollte einem [*Fragment*](~/android/platform/fragments/index.md)zugeordnet werden. Wenn der Benutzer eine Registerkarte auswählt, zeigt die Anwendung das Fragment an, das der Registerkarte zugeordnet ist. Die Aktionsleiste ist nicht für die Anzeige des passenden Fragments für den Benutzer verantwortlich. Stattdessen benachrichtigt die Aktionsleiste eine Anwendung über Zustandsänderungen auf einer Registerkarte über eine Klasse, die die Aktionsleiste. itablistener-Schnittstelle implementiert. Diese Schnittstelle stellt drei Rückruf Methoden bereit, die von Android aufgerufen werden, wenn sich der Status der Registerkarte ändert: 

- **Ontabselected** : Diese Methode wird aufgerufen, wenn der Benutzer die Registerkarte auswählt. Das Fragment sollte angezeigt werden.

- **Ontabreselected** : Diese Methode wird aufgerufen, wenn die Registerkarte bereits ausgewählt ist, aber vom Benutzer erneut ausgewählt wird. Dieser Rückruf wird normalerweise verwendet, um das angezeigte Fragment zu aktualisieren/zu aktualisieren.

- **Ontabunselected** : Diese Methode wird aufgerufen, wenn der Benutzer eine andere Registerkarte auswählt. Dieser Rückruf wird zum Speichern des Zustands im angezeigten Fragment verwendet, bevor es verschwindet.

Xamarin. Android umschließt `ActionBar.ITabListener` den mit Ereignissen für `ActionBar.Tab` die-Klasse. Anwendungen können einem oder mehreren dieser Ereignisse Ereignishandler zuweisen. Es gibt drei Ereignisse (eine für jede Methode in `ActionBar.ITabListener`), die von einer Aktionsleisten Registerkarte angezeigt wird: 

- Tabselected
- Tabreselected
- Tabunselected



### <a name="adding-tabs-to-the-actionbar"></a>Hinzufügen von Registerkarten zur Aktionsleiste

Die Aktionsleiste ist für Android 3,0 (API-Ebene 11) und höher und für jede xamarin. Android-Anwendung verfügbar, die diese API als Minimalziel hat. 

Die folgenden Schritte veranschaulichen, wie Sie einer Android-Aktivität Aktionsleisten-Registerkarten hinzufügen: 

1. In der `OnCreate` -Methode einer Aktivität &ndash; vor dem `NavigationMode` *Initialisieren von UI-Widgets* &ndash; muss eine Anwendung für die `ActionBar` auf `ActionBar.NavigationModeTabs` festlegen, wie im folgenden Code Ausschnitt gezeigt:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Erstellen Sie mithilfe `ActionBar.NewTab()`von eine neue Registerkarte.

3. Weisen Sie Ereignishandler zu, oder geben `ActionBar.ITabListener` Sie eine benutzerdefinierte Implementierung an, die auf die Ereignisse reagiert, die ausgelöst werden, wenn der Benutzer mit den Registerkarten für die Aktionsleiste interagiert.

4. Fügen Sie die Registerkarte hinzu, die im vorherigen Schritt `ActionBar`erstellt wurde.


Der folgende Code ist ein Beispiel für die Verwendung dieser Schritte zum Hinzufügen von Registerkarten zu einer Anwendung, die Ereignishandler verwendet, um auf Zustandsänderungen zu reagieren: 

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


#### <a name="event-handlers-vs-actionbaritablistener"></a>Ereignishandler im Vergleich zu "Aktionsleiste. itablistener"

Anwendungen sollten Ereignishandler und `ActionBar.ITabListener` für verschiedene Szenarios verwenden. Ereignishandler bieten eine bestimmte Menge syntaktischer Möglichkeiten. Sie speichern die Erstellung einer Klasse und die Implementierung `ActionBar.ITabListener`von. Dies hat &ndash; den Vorteil, dass xamarin. Android diese Transformation für Sie durchführt, indem eine Klasse erstellt und `ActionBar.ITabListener` für Sie implementiert wird. Dies ist in Ordnung, wenn eine Anwendung über eine begrenzte Anzahl von Registerkarten verfügt. 

Beim Umgang mit vielen Registerkarten oder der gemeinsamen Nutzung allgemeiner Funktionen zwischen Aktionsleisten-Registerkarten kann es im Hinblick auf Arbeitsspeicher und Leistung effizienter sein, eine Benutzer `ActionBar.ITabListener`definierte Klasse zu erstellen, die implementiert und eine einzelne Instanz der Klasse freigibt. Dies reduziert die Anzahl der Gref-Anwendungen, die von einer xamarin. Android-Anwendung verwendet werden. 



### <a name="backwards-compatibility-for-older-devices"></a>Abwärtskompatibilität für ältere Geräte

Die Registerkarten für die Aktionsleiste von [Android-Unterstützungs Bibliotheken V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) an Android 2,1 (API-Ebene 7). Auf Registerkarten kann in einer xamarin. Android-Anwendung zugegriffen werden, nachdem diese Komponente dem Projekt hinzugefügt wurde.

Um die Aktionsleiste zu verwenden, muss eine Aktivität unter `ActionBarActivity` Klasse sein und das AppCompat-Design verwenden, wie im folgenden Code Ausschnitt gezeigt:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Eine Aktivität kann einen Verweis auf Ihre Aktionsleiste aus der `ActionBarActivity.SupportingActionBar` -Eigenschaft abrufen. Der folgende Code Ausschnitt veranschaulicht ein Beispiel für das Einrichten der Aktionsleiste in einer Aktivität:

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

In dieser Anleitung wurde erläutert, wie Sie eine Benutzeroberfläche im Registerkarten Format in einer xamarin. Android-Datei mithilfe der Aktionsleiste erstellen. Es wurde beschrieben, wie Sie der Aktionsleiste Registerkarten hinzufügen und wie eine Aktivität über die `ActionBar.ITabListener` -Schnittstelle mit Tabstopp Ereignissen interagieren kann. Wir haben auch gesehen, wie das Paket "Android Support Library V7 AppCompat" die Aktions Anzeige Registerkarten auf ältere Android-Versionen zurückportiert. 


## <a name="related-links"></a>Verwandte Links

- [Aktions Registerkarten (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-actionbartabs)
- [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md)
- [Fragmente](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Aktionsleiste Muster](https://developer.android.com/design/patterns/actionbar.html)
- [Android V7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin. Android-Unterstützungs Bibliothek V7 AppCompat nuget-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
