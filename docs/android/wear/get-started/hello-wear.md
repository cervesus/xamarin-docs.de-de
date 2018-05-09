---
title: Hello, Abnutzung
description: Erstellen Sie Ihrer ersten Android Dach app, und führen Sie sie auf ein Gerät oder Emulator Abnutzung. Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen eines kleinen Dach Android-Projekts, das auf eine Schaltfläche behandelt und zeigt einen Leistungsindikator klicken Sie auf dem Gerät Abnutzung. Es wird erläutert, wie Debuggen Sie die Anwendung mit einem Emulator Abnutzung oder ein Abnutzung-Gerät, das mit einem Android-Telefon über Bluetooth verbunden ist. Darüber hinaus eine Reihe von Tipps zum Debuggen für Android Dach.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 17c12c4ec818c21d6697932315874ea4f63e6109
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="hello-wear"></a>Hello, Abnutzung

_Erstellen Sie Ihrer ersten Android Dach app, und führen Sie sie auf ein Gerät oder Emulator Abnutzung. Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen eines kleinen Dach Android-Projekts, das auf eine Schaltfläche behandelt und zeigt einen Leistungsindikator klicken Sie auf dem Gerät Abnutzung. Es wird erläutert, wie Debuggen Sie die Anwendung mit einem Emulator Abnutzung oder ein Abnutzung-Gerät, das mit einem Android-Telefon über Bluetooth verbunden ist. Darüber hinaus eine Reihe von Tipps zum Debuggen für Android Dach._

![Screenshot der Abnutzung-app in diesem Lernprogramm durchgeführt werden](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Der erste Abnutzung-app

Gehen Sie zum Erstellen Ihrer ersten Xamarin.Android Abnutzung app wie folgt vor:

### <a name="1-create-a-new-android-project"></a>1. Erstellen eines neuen Android-Projekts

Erstellen Sie ein neues **Android Dach Anwendung**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Erstellen einer neuen Android Dach Anwendung klicken Sie im Dialogfeld "Neues Projekt"](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Erstellen einer neuen Android Dach-Anwendung im Dialogfeld "neue Projektmappe"](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Diese Vorlage enthält, die automatisch die **Wearable Bibliothek für Xamarin Android** NuGet (und Abhängigkeiten), damit Sie Zugriff auf spezifische Abnutzung Widgets haben. Wenn die Vorlage Abnutzung nicht angezeigt wird, überprüfen Sie die [Installation und Einrichtung](~/android/wear/get-started/installation.md) Handbuch zum Überprüfen, dass Sie unterstützten Android-SDK installiert haben. 

### <a name="2-choose-the-correct-target-framework"></a>2. Wählen Sie den richtigen **Zielframework**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sicherstellen, dass **mindestens Android Ziel** festgelegt ist, um **Android 5.0.x (Lollipop)** oder höher: 

[![Festlegen des Zielframeworks für Android 5.0 in Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Stellen Sie sicher, das Zielframework auf festgelegt ist **Android 5.0.x (Lollipop)** oder höher:

[![Festlegen des Zielframeworks für Android 5.0 in Visual Studio für Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Weitere Informationen zum Festlegen des Zielframeworks finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Bearbeiten der **Main.axml** Layout

Konfigurieren Sie das Layout enthält eine `TextView` und ein `Button` für das Beispiel: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. Bearbeiten der **MainActivity.cs** Quelle

Fügen Sie den Code, um einen Zähler erhöht und zeigen Sie es bei jedem Klicken auf die Schaltfläche hinzu: 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. Einem Emulator oder Gerät einrichten

Der nächste Schritt, richten einen Emulator oder Gerät aus bereitstellen und Ausführen der app. Wenn Sie noch nicht vertraut sind, mit der Prozess der Bereitstellung und Ausführung von Xamarin.Android apps im Allgemeinen finden Sie unter der [Hello, Android Schnellstart](~/android/get-started/hello-android/hello-android-quickstart.md).

Wenn Sie nicht über ein Gerät Android Dach, z. B. ein Android Dach Smartwatch verfügen, können Sie die app in einem Emulator ausführen. Weitere Informationen zum Debuggen von Abnutzung apps in einem Emulator finden Sie unter [Debuggen Android Dach in einem Emulator](~/android/wear/deploy-test/debug-on-emulator.md).

Wenn Sie ein Gerät Android Dach, z. B. ein Android Dach Smartwatch verfügen, können Sie die app auf dem Gerät, anstatt einen Emulator ausführen. Weitere Informationen zum Debuggen auf einem Gerät Abnutzung finden Sie unter [Debuggen auf einem Gerät Dach](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Führen Sie die app für Android Abnutzung

Das Gerät mit Android Dach sollte in der Pulldownmenü Gerät angezeigt werden. Achten Sie darauf, dass Sie die richtige Dach Android-Gerät oder AVD auswählen, bevor Sie das Debuggen starten. Klicken Sie nachdem das Gerät auswählen auf die wiedergeben-Schaltfläche, um die app auf dem Emulator oder Gerät bereitzustellen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Auswählen einer Dach AVD in Visual Studio-Gerät-Menü](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Ein AVD Dach auswählen in Visual Studio für Mac-Geräts im Menü](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Sie wird möglicherweise eine **nur eine Minute...**  Nachricht (oder einige andere interstitial Bildschirms) auf den ersten: 

![Sehen Sie sich, dass der Emulator nur eine Minute zeigt...](hello-wear-images/please-wait.png)

Wenn Sie einen Emulator Überwachung verwenden, dauert es eine Weile, um die app zu starten. Bei Verwendung von Bluetooth dauert es mehr Zeit für die app bereitstellen, als würde über USB an. (Z. B. dauert etwa 5 Minuten, diese app auf ein LG G Watch bereitzustellen, die auf einem Telefon zu Nexus 5 Bluetooth verbunden ist.)

Nachdem die app erfolgreich bereitgestellt hat, sollte auf dem Bildschirm des Geräts Abnutzung einen Bildschirm wie folgt angezeigt:

[![Startbildschirm von Abnutzung-app](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Tippen Sie auf die **CLICK ME!** Schaltfläche in der Abnutzung Gerät und zum finden Sie die Erhöhung der Anzahl mit jeder tippen:

[![Screenshot der Dach app 3 Klicks](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Dach Beispiele](https://developer.xamarin.com/samples/android/Android%20Wear/) einschließlich Dach Android-apps mit Begleit-Phone-apps.

Wenn Sie Ihre app zu verteilen möchten, finden Sie unter [arbeiten mit Verpackung](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Verwandte Links

- [Klicken Sie auf die App Me (Beispiel)](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
