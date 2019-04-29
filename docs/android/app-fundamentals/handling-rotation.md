---
title: Verarbeiten der Drehung
description: Dieses Thema beschreibt, wie Änderungen an Device-Ausrichtung in Xamarin.Android behandelt wird. Es wird beschrieben, wie mit dem Android-Ressourcen-System, um Ressourcen für ein bestimmtes geräteausrichtung auch automatisch zu laden, wie an die programmgesteuerte Behandlung der Ausrichtung Änderungen funktioniert.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 62e64be89e26e5a8412cd34221da581e99fc5e6a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61017331"
---
# <a name="handling-rotation"></a>Verarbeiten der Drehung

_Dieses Thema beschreibt, wie Änderungen an Device-Ausrichtung in Xamarin.Android behandelt wird. Es wird beschrieben, wie mit dem Android-Ressourcen-System, um Ressourcen für ein bestimmtes geräteausrichtung auch automatisch zu laden, wie an die programmgesteuerte Behandlung der Ausrichtung Änderungen funktioniert._


## <a name="overview"></a>Übersicht

Da es sich bei mobile Geräten einfach gedreht werden, ist die integrierte Rotation ein Standardfeature in mobilen Betriebssystemen. Android bietet eine ausgereifte Framework für den Umgang mit einer Drehung von Anwendungen,, ob die Benutzeroberfläche in XML oder programmgesteuert im Code deklarativ erstellt wird. Bei der Behandlung von automatisch auf einem Gerät gedreht deklarative layoutänderungen kann eine Anwendung die enge Integration mit dem Android-Ressourcen-System nutzen. Für programmgesteuerte Layouts müssen Änderungen manuell verarbeitet werden. Dies ermöglicht eine präzisere Kontrolle zur Laufzeit allerdings auf Kosten mehr Arbeit für Entwickler. Eine Anwendung kann auch auswählen, deaktivieren den Neustart der Aktivität und manuelle Steuerung für Änderungen der bildschirmausrichtung.

Dieses Handbuch untersucht die folgenden Themen der Ausrichtung:

-   **Deklarative Layout Drehung** &ndash; wie Android-Ressourcen anhand der um Ausrichtung-fähigen Anwendungen, einschließlich Informationen zum Laden von sowohl Layouts und zeichenbarer Ressourcen für bestimmte Ausrichtungen zu erstellen.

-   **Rotation für programmgesteuerte Layouts** &ndash; Gewusst wie: Programmgesteuertes Hinzufügen von Steuerelementen sowie das Durchführen von Änderungen der bildschirmausrichtung manuell.


## <a name="handling-rotation-declaratively-with-layouts"></a>Verarbeiten der Drehung deklarativ mit Layouts

Dateien in Ordnern, die Benennungskonventionen einschließen, lädt Android automatisch die entsprechenden Dateien, wenn die Ausrichtung ändert.
Dies umfasst Unterstützung für:

-   *Layout Ressourcen* &ndash; angeben, welche Layoutdateien für jede Ausrichtung vergrößert werden.

-   *Zeichenbare Ressourcen* &ndash; angeben, welche zeichenbarer Ressourcen für jede Ausrichtung geladen werden.


### <a name="layout-resources"></a>Layout-Ressourcen

Standardmäßig Android XML (AXML)-Dateien enthalten, der **Ressourcen/Layout** Ordner für das Rendern von Ansichten für eine Aktivität verwendet werden. Dieses Ordners Ressourcen werden für sowohl hoch-und Querformat verwendet, wenn keine zusätzliche Layout Ressourcen speziell für Querformat bereitgestellt werden. Beachten Sie, die von der Standard-Projektvorlage erstellte Projektstruktur:

[![Vorlage für die Standardprojektstruktur](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Dieses Projekt erstellt einen einzigen **Main.axml** Datei die **Ressourcen/Layout** Ordner. Wenn der Aktivitäts `OnCreate` Methode aufgerufen wird, wird es vergrößert die Ansicht, die in definierten **Main.axml,** die eine Schaltfläche deklariert wird, wie in den folgenden XML-Code dargestellt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

Wenn das Gerät an, die Aktivität des Querformat gedreht wird `OnCreate` -Methode erneut aufgerufen und die gleichen **Main.axml** Datei vergrößert wird, wie im folgenden Screenshot gezeigt:

[![Gleichen Bildschirm jedoch im Querformat](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>Ausrichtung-spezifischen Layouts

Zusätzlich zu dem Ordner "Layout" (die standardmäßig auf Hochformat fest und können auch explizit benannt werden *Layout-Port* dazu einen Ordner namens `layout-land`), eine Anwendung kann die Ansichten im Querformat ohne Code muss definieren ändert.

Nehmen Sie an der **Main.axml** Datei enthält die folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Wenn ein Ordner zusteuere Layout mit dem Namen, die eine zusätzliche enthält **Main.axml** Datei wird dem Projekt hinzugefügt, jedoch das Layout im Querformat führt jetzt zu laden, die neu hinzugefügte Android **Main.axml.** Betrachten Sie die Version im Querformat der **Main.axml** -Datei mit den folgenden Code (aus Gründen der Einfachheit dieser XML-Code ist vergleichbar mit der Standardversion Hochformat des Codes, verwendet jedoch eine andere Zeichenfolge in die `TextView`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Das Ausführen des Codes, und drehen das Gerät vom Hochformat zum Querformat zeigt das neue XML-laden, wie unten dargestellt:

[![Hoch- und Querformat Screenshots Drucken der Hochformat](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>Drawable-Ressourcen

Während der Drehung behandelt Android zeichenbare Ressourcen auf ähnliche Weise Layout-Ressourcen. In diesem Fall ruft das System die zeichenbarer Ressourcen aus der **Ressourcen/drawable** und **Ressourcen/drawable-Land** Ordner bzw.

Beispiel: das Projekt enthält ein Image namens Monkey.png in die **Ressourcen/drawable** Ordner, in denen die drawable verwiesen wird ein `ImageView` im XML-Code wie folgt:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Nehmen wir weiter an, die eine andere Version von **Monkey.png** befindet sich unter **Ressourcen/drawable-Land**. Genau wie bei der Layoutdateien, wenn das Gerät wird gedreht, die drawable Änderungen für die angegebene Ausrichtung, wie unten dargestellt:

[![Andere Version von Monkey.png im Hochformat und Querformat angezeigt](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>Programmgesteuerte Behandlung von Drehung

Manchmal wird im Code Layouts definieren. Dies kann eine Vielzahl von Gründen, einschließlich technischer Einschränkungen, die Developer-Einstellungen usw. haben. Wenn wir Steuerelemente programmgesteuert hinzufügen, muss eine Anwendung manuell geräteausrichtung, berücksichtigt der automatisch behandelt wird, wenn wir die XML-Ressourcen verwenden.


### <a name="adding-controls-in-code"></a>Hinzufügen von Steuerelementen in Code

Um Steuerelemente programmgesteuert hinzuzufügen, muss eine Anwendung die folgenden Schritte ausführen:

-  Erstellen Sie ein Layout.
-  Festlegen Sie layoutparameter.
-  Erstellen Sie Steuerelemente.
-  Festlegen Sie Steuerelement-Layout-Parameter.
-  Hinzufügen von Steuerelementen auf das Layout.
-  Legen Sie das Layout, als die Ansicht "Inhalt".

Betrachten Sie beispielsweise eine Benutzeroberfläche, die aus einer einzelnen `TextView` Steuerelement hinzugefügt, um eine `RelativeLayout`, wie im folgenden Code gezeigt.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Dieser Code erstellt eine Instanz einer `RelativeLayout` -Klasse und legt seine `LayoutParameters` Eigenschaft. Die `LayoutParams` -Klasse ist von Android-Methode kapseln, wie Steuerelemente in eine wieder verwendbare Weise angeordnet sind. Sobald eine Instanz eines Layouts erstellt wurde, können Steuerelemente erstellt und hinzugefügt werden. Steuerelemente haben außerdem `LayoutParameters`, z. B. die `TextView` in diesem Beispiel. Nach der `TextView` erstellt wird, hinzugefügt wird die `RelativeLayout` und Einstellung der `RelativeLayout` wie die Ansicht "Inhalt" Ergebnisse in der Anwendung Anzeigen der `TextView` wie gezeigt:

[![Schaltfläche für einen Leistungsindikator in sowohl hoch-und Querformat angezeigt](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>Erkennen von Ausrichtung im Code

Wenn eine Anwendung versucht, laden eine andere Benutzeroberfläche für jede Ausrichtung bei `OnCreate` aufgerufen wird (Dies geschieht jedes Mal, die ein Gerät gedreht wird), sie erkennen die Ausrichtung muss, und Laden Sie den gewünschten Code für die Benutzeroberfläche. Android hat eine Klasse mit dem Namen der `WindowManager`, die verwendet werden können, um zu bestimmen, die aktuellen geräterotation über die `WindowManager.DefaultDisplay.Rotation` -Eigenschaft, wie unten dargestellt:

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Dieser Code legt die `TextView` positionierte 100 Pixel vom oberen überlassen des Bildschirms, automatisch an das neue Layout, animieren, wenn zum Querformat gedreht wird, wie hier gezeigt:

[![Der Ansichtszustand wird über hoch-und Querformat beibehalten.](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>Aktivität-Neustart verhindern

Neben der Handhabung von alles in `OnCreate`, eine Anwendung kann auch verhindern, dass eine Aktivität wird neu gestartet, wenn die Ausrichtung durch die Einstellung ändert `ConfigurationChanges` in die `ActivityAttribute` wie folgt:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Nun, wenn das Gerät gedreht wird, wird die Aktivität nicht neu gestartet. Um die Änderung der geräteausrichtung manuell in diesem Fall zu behandeln, kann eine Aktivität überschreiben die `OnConfigurationChanged` Methode und bestimmen Sie die Ausrichtung aus der `Configuration` -Objekt, das, wie die neue Implementierung der folgenden Aktivität übergeben wird:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

Hier die `TextView's` layoutparameter für quer- und Hochformat initialisiert werden. Klassenvariablen enthalten die Parameter zusammen mit den `TextView` selbst, da die Aktivität nicht erneut erstellt wird, wenn Ausrichtung ändert. Der Code weiterhin verwendet die `surfaceOrientartion` in `OnCreate` das anfängliche Layout für das Festlegen der `TextView`. Danach `OnConfigurationChanged` verarbeitet alle nachfolgenden layoutänderungen vor.

Wenn die Anwendung ausgeführt wird, lädt Android den Änderungen an der Benutzeroberfläche an, wie geräterotation tritt ein, und die Aktivität nicht neu gestartet.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>Verhindern, dass Aktivität Neustart für deklarative Layouts

Aktivität-Neustarts durch geräterotation verursacht können auch verhindert werden, wenn wir das Layout im XML-Code zu definieren. Beispielsweise können wir diesen Ansatz verwenden, wenn wir, um zu verhindern, dass einen Neustart der Aktivität möchten (zur Verbesserung der Leistung, z. B.) und neue Ressourcen für verschiedene Ausrichtungen Laden nicht erforderlich.

Zu diesem Zweck gehen wir genauso vor, die wir mit einem programmgesteuerten Layout verwenden. Legen Sie einfach `ConfigurationChanges` in die `ActivityAttribute`, wie der `CodeLayoutActivity` weiter oben. Jeglicher Code, der für die Änderung der geräteausrichtung erneut in implementiert werden, kann ausgeführt werden, muss die `OnConfigurationChanged` Methode.


## <a name="maintaining-state-during-orientation-changes"></a>Beibehalten des Zustands bei Änderungen der Bildschirmausrichtung

Ob die Drehung deklarativ oder programmgesteuert zu behandeln, sollten alle Android-Anwendungen die gleichen Techniken zum Verwalten des Zustands bei Änderung der geräteausrichtung implementieren. Verwalten des Zustands ist wichtig, da das System eine ausgeführte Aktivität neu startet, wenn ein Android-Gerät gedreht wird. Android ist diese Option, um erleichtern das Laden von anderer Ressourcen, z. B. Layouts und zeichenbarer Ressourcen, die speziell für eine bestimmte Ausrichtung entwickelt wurden. Wenn diese neu gestartet wird, verliert die Aktivität flüchtigen Zustand, die, den diese an lokale Klasse-Variablen gespeichert haben, kann. Wenn eine Activity Status abhängig ist, muss es daher seinen Zustand auf Anwendungsebene beibehalten. Eine Anwendung muss behandeln speichern und Wiederherstellen von jedem Zustand der Anwendung, die es mehrere Änderungen der bildschirmausrichtung beibehalten möchte.

Weitere Informationen zum Beibehalten von Status in Android, finden Sie in der [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md) Guide.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie Sie die integrierten Funktionen von Android zu verwenden, um mit einer Drehung von arbeiten wird. Zuerst, es wurde erklärt, wie Android-Ressourcen anhand der Ausrichtung Anwendungen erstellen. Dann präsentiert er Gewusst wie: Hinzufügen von Steuerelementen im Code als auch für das Durchführen von Änderungen der bildschirmausrichtung manuell.



## <a name="related-links"></a>Verwandte Links

- [Drehung-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Behandeln von Runtime-Änderungen](https://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Änderung der Geräteausrichtung schnell Bildschirm](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
