---
title: Behandlung von Drehung
description: "In diesem Thema wird beschrieben, wie Gerät Ausrichtung Änderungen in Xamarin.Android behandelt wird. Sie erfahren, wie Arbeiten Sie mit dem Ressourcensystem Android, um Ressourcen für ein bestimmtes geräteausrichtung ebenfalls automatisch zu laden, wie an der Ausrichtung programmgesteuert zu verarbeiten Änderungen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: eb310b13a97e345bab68bf4e878f81a6187da691
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="handling-rotation"></a>Behandlung von Drehung

_In diesem Thema wird beschrieben, wie Gerät Ausrichtung Änderungen in Xamarin.Android behandelt wird. Sie erfahren, wie Arbeiten Sie mit dem Ressourcensystem Android, um Ressourcen für ein bestimmtes geräteausrichtung ebenfalls automatisch zu laden, wie an der Ausrichtung programmgesteuert zu verarbeiten Änderungen._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Da auf mobile Geräten einfache gedreht werden, ist integrierte Drehung standard-Funktion in mobilen Betriebssystemen. Android bietet ein anspruchsvolle Framework für den Umgang mit innerhalb von Anwendungen, Drehung, ob die Benutzeroberfläche in XML oder programmgesteuert im Code deklarativ erstellt wird. Bei der Behandlung von deklarativen layoutänderungen auf einem Gerät gedreht automatisch kann eine Anwendung die enge Integration in das Ressourcensystem Android profitieren. Für das programmgesteuerte Layout müssen Änderungen manuell verarbeitet werden. Dies ermöglicht eine genauere Steuerung des zur Laufzeit jedoch auf Kosten der Arbeit für den Entwickler. Eine Anwendung können auch ablehnen des Aktivität Neustarts und ergreifen Sie die manuelle Steuerung der Ausrichtung Änderungen.

Dieses Handbuch untersucht die folgenden Themen der Ausrichtung:

-   **Deklarative Layout Drehung** &ndash; wie Android Ressource anhand der um Ausrichtung-fähigen Anwendungen, einschließlich Informationen zum Laden des Layouts und Drawables für bestimmte Ausrichtungen zu erstellen.

-   **Programmgesteuerte Layout Drehung** &ndash; Gewusst wie: Programmgesteuertes Hinzufügen von Steuerelementen sowie Ausrichtung Änderungen manuell zu behandeln.

<a name="Handling_Rotation_Declaratively_with_Layouts" />

## <a name="handling-rotation-declaratively-with-layouts"></a>Behandlung von Drehung deklarativ mit Layouts

Dateien in Ordnern, die Namenskonventionen entsprechen einschließen, lädt Android automatisch die entsprechenden Dateien eine Änderung die Ausrichtung an.
Dies umfasst Unterstützung für Folgendes:

-   *Layout Ressourcen* &ndash; angeben, welche Layoutdateien für jede Ausrichtung vergrößert werden.

-   *Zeichenbaren Ressourcen* &ndash; angeben, welche Drawables für jede Ausrichtung geladen werden.

<a name="Layout_Resources" />

### <a name="layout-resources"></a>Layout-Ressourcen

Standardmäßig Android XML (AXML)-Dateien enthalten, der **Ressourcen/Layout** Ordner dienen zum Rendern von Ansichten für eine Aktivität. Dieser Ordner Ressourcen werden für hoch-und Querformat verwendet, wenn keine zusätzlichen Layout Ressourcen speziell für Querformat bereitgestellt werden. Betrachten Sie die von der Standardprojektvorlage erstellte Projektstruktur aus:

[ ![Vorlage für die Standardprojektstruktur](handling-rotation-images/00.png)](handling-rotation-images/00.png)

Dieses Projekt erstellt ein einzelnes **Main.axml** in der Datei die **Ressourcen/Layout** Ordner. Wenn der Aktivitätssymbols `OnCreate` -Methode aufgerufen wird, es vergrößert die Ansicht im definierten **Main.axml,** die eine Schaltfläche deklariert wird, wie in der XML-Code unten gezeigt:

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

Wenn das Gerät an, die Aktivität des Querformat gedreht wird `OnCreate` -Methode erneut aufgerufen wird und die gleiche **Main.axml** Datei vergrößert wird, wie im folgenden Screenshot gezeigt:

[ ![Gleichen Bildschirm jedoch im Querformat](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png)

<a name="Orientation-Specific_Layouts" />

#### <a name="orientation-specific-layouts"></a>Ausrichtung-spezifische Layouts

Zusätzlich zu dem Ordner Layout (die standardmäßig auf Hochformat fest und können auch explizit benannt *Layout-Port* dazu einen Ordner namens `layout-land`), eine Anwendung kann die Ansichten im Querformat ohne Code muss definieren ändert.

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

Wenn ein Ordner Layout Land mit dem Namen, die einen zusätzlichen enthält **Main.axml** Datei wird dem Projekt hinzugefügt, das Layout im Querformat überhöhte führt zu laden, die neu hinzugefügte Android jetzt **Main.axml.** Betrachten Sie die Querformat-Version von der **Main.axml** Datei mit dem folgenden Code (aus Gründen der Einfachheit, diese XML-Daten ist Hochformat Standardversion des Codes ähnelt, verwendet jedoch eine andere Zeichenfolge in der `TextView`):

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

Ausführen dieses Codes, und drehen das Gerät im Hochformat, Querformat wird das neue XML-laden veranschaulicht, wie unten dargestellt:

[ ![Hochformat- und querformatausrichtung Screenshots der Hochformat drucken](handling-rotation-images/02.png)](handling-rotation-images/02.png)

<a name="Drawable_Resources" />

### <a name="drawable-resources"></a>Zeichenbaren Ressourcen

Während der Drehung behandelt Android zeichenbare Ressourcen auf ähnliche Weise zu Layout-Ressourcen. In diesem Fall das System Ruft den Drawables aus der **Ressourcen und Ausgaben möglich** und **Ressourcen/zeichenbaren-Land** Ordnern bzw.

Angenommen, das Projekt enthält ein Bild mit dem Namen Monkey.png in der **Ressourcen und Ausgaben möglich** Ordner, in dem die zeichenbaren verwiesen wird ein `ImageView` im XML-Code wie folgt:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Nehmen wir weiter an, die eine andere Version von **Monkey.png** ist unter enthalten **Ressourcen/zeichenbaren-Land**. So wie mit den Layoutdateien, wenn das Gerät wird gedreht, die zeichenbaren Änderungen an der angegebenen Ausrichtung wie unten dargestellt:

[ ![Andere Version von Monkey.png Hochformat- und querformatausrichtung Modi angezeigt](handling-rotation-images/03.png)](handling-rotation-images/03.png)

<a name="Handling_Rotation_Programmatically" />

## <a name="handling-rotation-programmatically"></a>Behandlung von Drehung programmgesteuert

In einigen Fällen wird im Code Layouts definieren. Dies kann eine Vielzahl von Gründen, einschließlich der technischen Beschränkungen, "Developer-Preference" usw. haben. Wenn wir programmgesteuert Steuerelemente hinzufügen, muss eine Anwendung manuell geräteausrichtung, berücksichtigt der automatisch behandelt wird, wenn wir die XML-Ressourcen verwenden.

<a name="Adding_Controls_in_Code" />

### <a name="adding-controls-in-code"></a>Hinzufügen von Steuerelementen in Code

Um Steuerelemente programmgesteuert hinzuzufügen, muss eine Anwendung die folgenden Schritte ausführen:

-  Erstellen Sie ein Layout.
-  Festlegen Sie layoutparameter.
-  Erstellen Sie Steuerelemente.
-  Festlegen Sie Steuerelement-Layout-Parameter.
-  Fügen Sie Steuerelemente, um das Layout.
-  Legen Sie das Layout, als der Inhaltsansicht.

Betrachten Sie beispielsweise eine Benutzeroberfläche mit einer einzelnen `TextView` ein Steuerelement hinzugefügt wurde, um eine `RelativeLayout`, wie im folgenden Code gezeigt.

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

Dieser Code erstellt eine Instanz von einem `RelativeLayout` -Klasse und legt dessen `LayoutParameters` Eigenschaft. Die `LayoutParams` Klasse lässt Android kapseln, wie Steuerelemente in eine wieder verwendbare Weise angeordnet sind. Nachdem eine Instanz eines Layouts erstellt wird, können Steuerelemente erstellt und hinzugefügt werden. Steuerelemente verfügen auch über `LayoutParameters`, wie z. B. die `TextView` in diesem Beispiel. Nach der `TextView` erstellt wird, hinzufügen zu der `RelativeLayout` verwendet wird und der `RelativeLayout` als die Ergebnisse der Inhaltsansicht angezeigt, die Anwendung die `TextView` wie gezeigt:

[ ![Schaltfläche für einen Leistungsindikator in Hochformat- und querformatausrichtung Modi angezeigt](handling-rotation-images/04.png)](handling-rotation-images/04.png)

<a name="Detecting_Orientation_in_Code" />

### <a name="detecting-orientation-in-code"></a>Erkennen von Ausrichtung im Code

Wenn eine Anwendung versucht, eine andere Benutzeroberfläche für jede Ausrichtung Laden bei `OnCreate` aufgerufen wird (Dies geschieht jedes Mal ein Gerät rotiert wird), es muss die Ausrichtung zu erkennen und Laden Sie dann den gewünschten Code für die Benutzeroberfläche. Android bietet eine Klasse mit dem Namen der `WindowManager`, die verwendet werden können, um zu bestimmen, die aktuellen geräterotation über die `WindowManager.DefaultDisplay.Rotation` -Eigenschaft verwenden, wie unten dargestellt:

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

In diesem Code wird die `TextView` positionierte 100 werden Links Pixel vom oberen Rand des Bildschirms automatisch an das neue Layout animieren, wenn gedreht, um Querformat, wie hier gezeigt:

[ ![Ansichtszustand bleiben bei Hochformat- und querformatausrichtung Modi ist erhalten.](handling-rotation-images/05.png)](handling-rotation-images/05.png)

<a name="Preventing_Activity_Restart" />

### <a name="preventing-activity-restart"></a>Aktivität Neustart verhindern

Neben der Handhabung von alles in `OnCreate`, eine Anwendung kann auch verhindern, dass eine Aktivität neu gestartet wird, wenn die Ausrichtung durch Festlegen von ändert `ConfigurationChanges` in der `ActivityAttribute` wie folgt:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Nun, wenn das Gerät gedreht wird, wird die Aktivität nicht neu gestartet. Um die Änderung der Ausrichtung manuell in diesem Fall zu behandeln, kann eine Aktivität überschreiben die `OnConfigurationChanged` Methode und bestimmen Sie die Ausrichtung aus der `Configuration` Objekt, in, wie die neue Implementierung der folgenden Aktivität übergeben wird:

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

Hier die `TextView's` layoutparameter für quer- und Hochformat initialisiert werden. Klassenvariablen halten Sie die Parameter zusammen mit den `TextView` selbst, da die Aktivität nicht neu erstellt wird, wenn Ausrichtung ändert. Der Code noch verwendet die `surfaceOrientartion` in `OnCreate` festzulegende das anfängliche Layout für die `TextView`. Danach `OnConfigurationChanged` alle nachfolgenden layoutänderungen behandelt.

Wenn die Anwendung ausgeführt wird, lädt Android Änderungen an der Schnittstelle wie geräterotation tritt auf, und die Aktivität wird nicht neu gestartet.

<a name="Preventing_Activity_Restart_for_Declarative_Layouts" />

## <a name="preventing-activity-restart-for-declarative-layouts"></a>Verhindern von Aktivität Neustart für deklarative Layouts

Aktivität Neustarts geräterotation zurückzuführen sind, können auch verhindert werden, wenn wir das Layout in XML definieren. Beispielsweise können wir diesen Ansatz verwenden, wenn wir einen Aktivität Neustart verhindern möchten (aus Gründen der Leistung vielleicht) und wir neue Ressourcen für die verschiedenen Ausrichtungen geladen werden müssen.

Zu diesem Zweck führen wir die gleiche Prozedur, die wir mit einem programmgesteuerten Layout verwenden Sie. Legen Sie einfach `ConfigurationChanges` in der `ActivityAttribute`, wie der `CodeLayoutActivity` weiter oben. Jeglicher Code, der für die Änderung der Ausrichtung erneut in implementiert werden, kann ausgeführt werden, muss die `OnConfigurationChanged` Methode.

<a name="Maintaining_State_During_Orientation_Changes" />

## <a name="maintaining-state-during-orientation-changes"></a>Beibehalten des Zustands während der Änderung der Ausrichtung

Ob die Drehung deklarativ oder programmgesteuert zu behandeln, sollten alle Android-Anwendungen implementieren, die gleichen Techniken für die Verwaltung von Zustand, wenn geräteausrichtung ändert. Verwalten des Zustands ist wichtig, da das System eine ausgeführte Aktivität wird neu gestartet, wenn ein Android-Gerät gedreht wird. Android dient der Sicherstellung erleichtern das Laden der alternativer Ressourcen, z. B. Layouts und Drawables, die speziell für eine bestimmte Ausrichtung vorgesehen sind. Beim Neustart, verliert die Aktivität flüchtigen Zustands, die möglicherweise im lokalen Klassenvariablen gespeichert haben. Aus diesem Grund muss ein eine Aktivität Zustand angewiesen ist, dessen Status auf Anwendungsebene beibehalten. Eine Anwendung behandeln muss speichern und Wiederherstellen von alle Status der Anwendung, die mehrere Ausrichtung Änderungen beibehalten werden sollen.

Weitere Informationen zum Beibehalten von Status im Android, finden Sie in der [Aktivitätenlebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md) Handbuch.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt wie Android integrierten Funktionen zum Arbeiten mit Drehung verwenden. Zuerst, es wurde erläutert, wie Android Ressource anhand der Ausrichtung fähigen Anwendungen erstellen. Klicken Sie dann präsentiert sie zum Hinzufügen von Steuerelementen im Code als auch Ausrichtung Änderungen manuell zu behandeln.



## <a name="related-links"></a>Verwandte Links

- [Drehung Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Behandlung von Änderungen zur Laufzeit](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Schnelle Bildschirm Ausrichtung ändern](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
