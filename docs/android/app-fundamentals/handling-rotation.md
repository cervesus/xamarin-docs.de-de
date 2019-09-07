---
title: Verarbeiten der Drehung
description: In diesem Thema wird beschrieben, wie Änderungen an der Geräte Orientierung in xamarin. Android behandelt werden. Es wird erläutert, wie Sie mit dem Android-Ressourcensystem zum automatischen Laden von Ressourcen für eine bestimmte Geräte Orientierung und zum programmgesteuerten behandeln von Richtungsänderungen arbeiten.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 98acabec7132730304bf5e8b81e99f2727b6d50e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755570"
---
# <a name="handling-rotation"></a>Verarbeiten der Drehung

_In diesem Thema wird beschrieben, wie Änderungen an der Geräte Orientierung in xamarin. Android behandelt werden. Es wird erläutert, wie Sie mit dem Android-Ressourcensystem zum automatischen Laden von Ressourcen für eine bestimmte Geräte Orientierung und zum programmgesteuerten behandeln von Richtungsänderungen arbeiten._

## <a name="overview"></a>Übersicht

Da mobile Geräte problemlos gedreht werden, ist die integrierte Rotation ein Standard Feature in mobilen Betriebssystemen. Android stellt ein ausgereiftes Framework für den Umgang mit der Drehung innerhalb von Anwendungen bereit, unabhängig davon, ob die Benutzeroberfläche deklarativ in XML oder Programm gesteuert in Code erstellt wird. Bei der automatischen Verarbeitung von deklarativen Layoutänderungen auf einem gedrehten Gerät kann eine Anwendung von der engen Integration in das Android-Ressourcensystem profitieren. Für das programmgesteuerte Layout müssen Änderungen manuell behandelt werden. Dies ermöglicht eine präzisere Steuerung zur Laufzeit, aber auf Kosten der Arbeit für den Entwickler. Eine Anwendung kann sich auch entscheiden, den Neustart der Aktivität zu beenden und die Richtungsänderungen manuell zu steuern.

In diesem Leitfaden werden die folgenden Themen zur Orientierung erläutert:

- **Deklarative layoutdrehung** &ndash; Verwenden des Android-ressourcensystems zum Erstellen von nach Orientierung unterstützenden Anwendungen, einschließlich des Ladens von Layouts und drawables für bestimmte Ausrichtungen.

- Programmgesteuerte **layoutdrehung** &ndash; Gewusst wie: Programm gesteuertes Hinzufügen von Steuerelementen und Gewusst wie: Manuelles behandeln von Orientierungsänderungen.

## <a name="handling-rotation-declaratively-with-layouts"></a>Deklarative Behandlung der Drehung mit Layouts

Wenn Sie Dateien in Ordner einschließen, die den Benennungs Konventionen folgen, lädt Android automatisch die entsprechenden Dateien, wenn sich die Ausrichtung ändert.
Dies umfasst die Unterstützung für:

- *Layoutressourcen* &ndash; Angeben, welche Layoutdateien für jede Ausrichtung aufgeblasen werden.

- *Drawable-Ressourcen* &ndash; Angeben, welche drawables für jede Ausrichtung geladen werden.

### <a name="layout-resources"></a>Layoutressourcen

Standardmäßig werden im Ordner " **Resources/Layout** " enthaltene Android XML (axml)-Dateien zum Rendern von Ansichten für eine Aktivität verwendet. Die Ressourcen dieses Ordners werden sowohl für hoch-als auch für Querformat verwendet, wenn keine zusätzlichen layoutressourcen speziell für das Querformat bereitgestellt werden. Beachten Sie die Projektstruktur, die von der Standard Projektvorlage erstellt wurde:

[![Standardmäßige Projektvorlagen Struktur](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Dieses Projekt erstellt eine einzelne Datei " **Main. axml** " im Ordner " **Resources/Layout** ". Wenn die- `OnCreate` Methode der Aktivität aufgerufen wird, wird die in **Main. axml** definierte Sicht vergrößert, die eine Schaltfläche deklariert, wie im folgenden XML-Code dargestellt:

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

Wenn das Gerät in Querformat gedreht wird, wird die- `OnCreate` Methode der Aktivität erneut aufgerufen, und die gleiche " **Main. axml** "-Datei wird aufgeblasen, wie im folgenden Screenshot zu sehen:

[![Gleicher Bildschirm, aber in Querformat](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)

#### <a name="orientation-specific-layouts"></a>Orientierungs spezifische Layouts

Zusätzlich zum layoutordner (der standardmäßig Hochformat ist und auch explizit als *layoutport* bezeichnet werden kann, indem ein `layout-land`Ordner namens verwendet wird), kann eine Anwendung die Ansichten definieren, die im Querformat ohne Codeänderungen erforderlich sind.

Angenommen, die Datei **Main. axml** enthält den folgenden XML-Code:

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

Wenn ein Ordner mit dem Namen Layout-Land, der eine zusätzliche Datei " **Main. axml** " enthält, dem Projekt hinzugefügt wird, führt die Vergrößerung des Layouts im Querformat jetzt dazu, dass Android das neu hinzugefügte " **Main. axml** " lädt. Sehen Sie sich die quer Version der Datei " **Main. axml** " an, die den folgenden Code enthält (aus Gründen der Einfachheit ist dieser XML-Code der standardmäßigen Hochformat Version des Codes ähnlich `TextView`, verwendet aber eine andere Zeichenfolge in):

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

Wenn Sie diesen Code ausführen und das Gerät vom Hochformat in das Querformat drehen, wird das neue XML-laden veranschaulicht, wie unten dargestellt:

[![Hochformat-und Querformat-Screenshots Drucken des hoch Modus](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)

### <a name="drawable-resources"></a>Drawable-Ressourcen

Während der Rotation behandelt Android drawable-Ressourcen ähnlich wie layoutressourcen. In diesem Fall ruft das System die drawables aus den Ordnern **Resources/drawable** bzw. **Resources/drawable-Land** ab.

Nehmen Sie beispielsweise an, das Projekt enthält ein Bild mit dem Namen "Monkey. png" im Ordner " **Resources/drawable** ", in `ImageView` dem in XML wie folgt auf die drawable verwiesen wird:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Nehmen wir weiter an, dass eine andere Version von " **Monkey. png** " unter " **Resources/drawable-Land**" enthalten ist. Ebenso wie bei den Layoutdateien ändert sich das drawable-Element für die angegebene Ausrichtung, wie unten dargestellt:

[![Andere Version von "Monkey. png" im hoch-und Querformat](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)

## <a name="handling-rotation-programmatically"></a>Programm gesteuertes behandeln der Rotation

Manchmal werden Layouts im Code definiert. Dies kann aus verschiedenen Gründen geschehen, wie z. b. technische Einschränkungen, Entwickler Präferenz usw. Wenn Sie Steuerelemente Programm gesteuert hinzufügen, muss eine Anwendung die Geräte Ausrichtung manuell berücksichtigen. diese wird automatisch bei der Verwendung von XML-Ressourcen behandelt.

### <a name="adding-controls-in-code"></a>Hinzufügen von Steuerelementen im Code

Zum programmgesteuerten Hinzufügen von Steuerelementen muss eine Anwendung die folgenden Schritte ausführen:

- Erstellen Sie ein Layout.
- Legen Sie Layoutparameter fest.
- Erstellen von Steuerelementen
- Festlegen von Steuerelement Layout-Parametern
- Fügen Sie dem Layout Steuerelemente hinzu.
- Legen Sie das Layout als Inhaltsansicht fest.

Angenommen, eine Benutzeroberfläche besteht aus einem einzelnen `TextView` Steuerelement, das einem `RelativeLayout`hinzugefügt wurde, wie im folgenden Code gezeigt.

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

Mit `RelativeLayout` diesem Code wird eine Instanz einer-Klasse erstellt und `LayoutParameters` die-Eigenschaft festgelegt. Die `LayoutParams` -Klasse ist die Art und Weise, wie Steuerelemente auf wiederverwendbare Weise positioniert werden. Nachdem eine Instanz eines Layouts erstellt wurde, können Steuerelemente erstellt und hinzugefügt werden. Steuerelemente haben `LayoutParameters`auch, wie `TextView` z. b. in diesem Beispiel. Nachdem der `TextView` erstellt wurde, wird er `RelativeLayout` dem hinzugefügt und der `RelativeLayout` als `TextView` Inhaltsansicht festgelegt, sodass die Anwendung wie folgt anzeigt:

[![Schaltfläche "Inkrement Counter" im hoch-und Querformat](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)

### <a name="detecting-orientation-in-code"></a>Erkennen der Ausrichtung im Code

Wenn eine Anwendung versucht, eine andere Benutzeroberfläche für jede Ausrichtung zu laden `OnCreate` , wenn aufgerufen wird (Dies geschieht bei jedem Drehen eines Geräts), muss Sie die Ausrichtung erkennen und dann den gewünschten Benutzeroberflächen Code laden. Android verfügt über eine Klasse namens `WindowManager`. diese kann verwendet werden, um die aktuelle Geräte Drehung mithilfe der `WindowManager.DefaultDisplay.Rotation` -Eigenschaft zu bestimmen, wie unten dargestellt:

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

`TextView` Mit diesem Code wird festgelegt, dass 100 Pixel von oben links auf dem Bildschirm positioniert werden. dabei wird automatisch eine Animation zum neuen Layout durchlaufen, wie hier gezeigt:

[![Der Ansichts Zustand wird im hoch-und Querformat beibehalten.](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)

### <a name="preventing-activity-restart"></a>Verhindern von Neustart der Aktivität

Zusätzlich zur Behandlung von Elementen in `OnCreate`kann eine Anwendung auch verhindern, dass eine Aktivität neu gestartet wird, wenn sich die Ausrichtung `ConfigurationChanges` ändert, `ActivityAttribute` indem Sie wie folgt in der festgelegt wird:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```

Wenn das Gerät nun gedreht wird, wird die Aktivität nicht neu gestartet. Um die Orientierungs Änderung in diesem Fall manuell zu verarbeiten, kann eine Aktivität die `OnConfigurationChanged` -Methode überschreiben und die Ausrichtung `Configuration` des übergebenen-Objekts, wie in der neuen Implementierung der folgenden Aktivität, bestimmen:

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

Hier werden `TextView's` die Layoutparameter sowohl für Querformat als auch für Hochformat initialisiert. Klassen Variablen enthalten die Parameter zusammen mit dem `TextView` selbst, da die Aktivität bei einer Änderung der Ausrichtung nicht neu erstellt wird. Der Code verwendet weiterhin den `surfaceOrientartion` in `OnCreate` , um das anfängliche Layout für das `TextView`festzulegen. `OnConfigurationChanged` Danach verarbeitet alle nachfolgenden Layoutänderungen.

Wenn Sie die Anwendung ausführen, lädt Android die Änderungen an der Benutzeroberfläche, während die Geräte Drehung erfolgt, und startet die Aktivität nicht neu.

## <a name="preventing-activity-restart-for-declarative-layouts"></a>Verhindern des Neustarts von Aktivitäten für deklarative Layouts

Aktivitäts Neustarts, die durch die Geräte Drehung verursacht werden, können auch verhindert werden, wenn das Layout in XML definiert wird. Beispielsweise können wir diesen Ansatz verwenden, wenn wir einen Aktivitäts Neustart verhindern möchten (aus Leistungsgründen), und wir müssen keine neuen Ressourcen für verschiedene Ausrichtungen laden.

Zu diesem Zweck befolgen wir das gleiche Verfahren wie bei einem programmatischen Layout. Legen Sie `ConfigurationChanges` einfach `ActivityAttribute`in fest, wie in der `CodeLayoutActivity` vorherigen Version. Sämtlicher Code, der für die Ausrichtung der Ausrichtung ausgeführt werden muss, kann in der `OnConfigurationChanged` -Methode wieder implementiert werden.

## <a name="maintaining-state-during-orientation-changes"></a>Beibehalten des Zustands bei der Ausrichtung von Änderungen

Unabhängig davon, ob die Rotation deklarativ oder Programm gesteuert verarbeitet wird, sollten alle Android-Anwendungen dieselben Verfahren zum Verwalten des Zustands implementieren, wenn sich die Geräte Ausrichtung ändert. Das Verwalten des Zustands ist wichtig, da das System eine laufende Aktivität neu startet, wenn ein Android-Gerät gedreht wird. Dies erleichtert Android das Laden alternativer Ressourcen, z. b. Layouts und drawables, die speziell für eine bestimmte Ausrichtung entworfen wurden. Beim Neustart verliert die Aktivität jeden vorübergehenden Zustand, der möglicherweise in lokalen Klassen Variablen gespeichert wurde. Wenn eine Aktivität Zustands abhängig ist, muss Sie daher ihren Zustand auf Anwendungsebene beibehalten. Eine Anwendung muss das Speichern und Wiederherstellen von Anwendungs Zuständen durchführen, die über Richtungsänderungen hinweg beibehalten werden sollen.

Weitere Informationen zum Beibehalten des Zustands in Android finden Sie im Handbuch zum [Aktivitäts Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md) .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie die integrierten Funktionen von Android für die Rotation verwendet werden. Zunächst wurde erläutert, wie das Android-Ressourcensystem zum Erstellen von Richtungs fähigen Anwendungen verwendet wird. Anschließend haben Sie erfahren, wie Sie Steuerelemente im Code hinzufügen und wie Sie die Ausrichtung von Änderungen manuell behandeln können.

## <a name="related-links"></a>Verwandte Links

- [Rotations Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-rotationdemo)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Behandeln von Lauf Zeit Änderungen](https://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Änderung der schnellen Bildschirm Ausrichtung](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
