---
title: Hallo, Wear
description: Erstellen Sie Ihrer ersten Android Wear-app, und führen Sie es auf einem Wear-Emulator oder Gerät. Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen von eines kleinen Projekts Android Wear, das auf eine Schaltfläche verarbeitet und zeigt einen Indikator klicken Sie auf dem Wear-Gerät. Es wird erläutert, wie zum Debuggen der app mit einem Wear-Emulator oder auf einem Wear-Gerät, das auf einem Android-Telefon über Bluetooth verbunden ist. Darüber hinaus eine Reihe von Tipps zum Debuggen für Android Wear.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: a8e27063040ff91f72a1cbf932b1b277a5dee63d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104920"
---
# <a name="hello-wear"></a>Hallo, Wear

_Erstellen Sie Ihrer ersten Android Wear-app, und führen Sie es auf einem Wear-Emulator oder Gerät. Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen von eines kleinen Projekts Android Wear, das auf eine Schaltfläche verarbeitet und zeigt einen Indikator klicken Sie auf dem Wear-Gerät. Es wird erläutert, wie zum Debuggen der app mit einem Wear-Emulator oder auf einem Wear-Gerät, das auf einem Android-Telefon über Bluetooth verbunden ist. Darüber hinaus eine Reihe von Tipps zum Debuggen für Android Wear._

![Screenshot der Wear-app in diesem Lernprogramm ausgeführt werden](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Ihre erste Wear-app

Um Ihre erste Xamarin.Android-Wear-app erstellen, gehen Sie wie folgt vor:

### <a name="1-create-a-new-android-project"></a>1. Erstellen eines neuen Android-Projekts

Erstellen Sie ein neues **Android Wear-Anwendung**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Erstellen eine neue Android Wear-Anwendung in das Dialogfeld "Neues Projekt"](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Erstellen eine neue Android Wear-Anwendung in das Dialogfeld "neue Projektmappe"](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Diese Vorlage enthält automatisch die **tragbare Xamarin Android-Bibliothek** NuGet (und Abhängigkeiten), damit Sie Zugriff auf Wear-spezifische Widgets müssen. Wenn die Wear-Vorlage nicht angezeigt wird, überprüfen Sie die [Installations- und Einrichtungsanleitung](~/android/wear/get-started/installation.md) Anleitung zum Überprüfen, dass Sie eine unterstützte Android-SDK installiert haben. 

### <a name="2-choose-the-correct-target-framework"></a>2. Wählen Sie den richtigen **Zielframework**

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Sicherstellen, dass **Android-Mindestversion** nastaven NA hodnotu **Android 5.0 (Lollipop)** oder höher: 

[![Festlegen des Zielframeworks auf Android 5.0 in Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Stellen Sie sicher, das Zielframework nastaven NA hodnotu **Android 5.0 (Lollipop)** oder höher:

[![Festlegen des Zielframeworks auf Android 5.0 in Visual Studio für Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Weitere Informationen zum Festlegen des Zielframeworks finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Bearbeiten der **Main.axml** Layout

Konfigurieren Sie das Layout enthält eine `TextView` und `Button` für das Beispiel: 

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

### <a name="4-edit-the-mainactivitycs-source"></a>4. Bearbeiten der **"mainactivity.cs"** Quelle

Fügen Sie den Code, um einen Zähler zu erhöhen und das anzeigen, wenn die Schaltfläche geklickt wird: 

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

### <a name="5-setup-an-emulator-or-device"></a>5. Richten Sie einen Emulator oder Gerät

Im nächste Schritt wird festgelegt, um einen Emulator oder Gerät, bereitstellen und Ausführen der app. Wenn Sie noch nicht vertraut sind, mit dem Prozess der Bereitstellung und Ausführung von Xamarin.Android-apps im Allgemeinen finden Sie unter den [Hello, Android: Schnellstart](~/android/get-started/hello-android/hello-android-quickstart.md).

Wenn Sie nicht über ein Gerät Android Wear, wie eine Android Wear Smartwatch verfügen, können Sie die app in einem Emulator ausführen. Weitere Informationen zum Debuggen von Wear-apps in einem Emulator finden Sie unter [Android Wear in einem Emulator Debuggen](~/android/wear/deploy-test/debug-on-emulator.md).

Wenn Sie ein Android Wear-Gerät, z.B. einer Android Wear Smartwatch verfügen, können Sie die app auf dem Gerät anstatt mit einem Emulator ausführen. Weitere Informationen zum Debuggen auf einem Wear-Gerät finden Sie unter [Debuggen auf einem Wear-Gerät](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Führen Sie die Android Wear-app

Das Android Wear-Gerät sollte im Gerät Pulldown-Menü angezeigt werden. Achten Sie darauf, dass auf den richtigen Android Wear-Gerät oder AVD vor dem Debuggen. Klicken Sie nach der Auswahl des Geräts, auf die entsprechende Schaltfläche, um die app für den Emulator oder Gerät bereitzustellen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Ein AVD Wear auswählen in Visual Studio im Menü](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Auswählen von einem Wear AVD in Visual Studio für Mac-Gerät im Menü](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Sie sehen möglicherweise eine **gleich...**  Nachricht (oder einige andere interstitial Bildschirms) auf den ersten: 

![Sehen Sie sich, dass der Emulator nur eine Minute zeigt...](hello-wear-images/please-wait.png)

Wenn Sie einen Watch-Emulator verwenden, dauert es eine Weile, um die app zu starten. Bei der Verwendung von Bluetooth dauert mehr Zeit für die app bereitstellen, als wäre Sie eine per USB. (Z. B. dauert etwa fünf Minuten lang diese app auf einer LG G Watch bereit, die auf einem Telefon zu Nexus 5 Bluetooth verbunden ist.)

Nachdem die app erfolgreich bereitgestellt wurde, sollte der Bildschirm des Geräts Wear einen Bildschirm ähnlich dem folgenden angezeigt werden:

[![Startbildschirm des Wear-app](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Tippen Sie auf die **CLICK ME!** Schaltfläche der Rückgabe die Wear-Gerät und die Erhöhung der Anzahl jedes durch Tippen auf:

[![Screenshot der Wear-app nach 3 Klicks](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Wear Beispiele](https://developer.xamarin.com/samples/android/Android%20Wear/) einschließlich Android Wear-apps mit Begleit-Phone-apps.

Wenn Sie Ihre app verteilen möchten, finden Sie unter [arbeiten mit der Paketerstellung](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Verwandte Links

- [Klicken Sie auf App Me (Beispiel)](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
