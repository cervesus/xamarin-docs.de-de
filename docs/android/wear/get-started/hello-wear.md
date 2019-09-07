---
title: Hello, Wear
description: Erstellen Sie Ihre erste Android Wear-APP, und führen Sie Sie auf einem Wear-Emulator oder-Gerät aus. Diese exemplarische Vorgehensweise enthält Schritt-für-Schritt-Anleitungen zum Erstellen eines kleinen Android Wear-Projekts, das Schaltflächen Klicks behandelt und einen Klick-Counter auf dem Wear-Gerät anzeigt. Darin wird erläutert, wie Sie die APP mit einem Wear-Emulator oder einem Wear-Gerät Debuggen, das über Bluetooth mit einem Android-Telefon verbunden ist. Außerdem bietet es eine Reihe von Tipps zum Debuggen für Android Wear.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: 4c3c0e51348d2435ce5042485b214e6e5fe159b2
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758424"
---
# <a name="hello-wear"></a>Hello, Wear

_Erstellen Sie Ihre erste Android Wear-APP, und führen Sie Sie auf einem Wear-Emulator oder-Gerät aus. Diese exemplarische Vorgehensweise enthält Schritt-für-Schritt-Anleitungen zum Erstellen eines kleinen Android Wear-Projekts, das Schaltflächen Klicks behandelt und einen Klick-Counter auf dem Wear-Gerät anzeigt. Darin wird erläutert, wie Sie die APP mit einem Wear-Emulator oder einem Wear-Gerät Debuggen, das über Bluetooth mit einem Android-Telefon verbunden ist. Außerdem bietet es eine Reihe von Tipps zum Debuggen für Android Wear._

![Screenshot der Wear-APP, die in diesem Tutorial abgeschlossen werden soll](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Ihre erste Wear-App

Führen Sie die folgenden Schritte aus, um Ihre erste xamarin. Android Wear-APP zu erstellen:

### <a name="1-create-a-new-android-project"></a>1. Erstellen eines neuen Android-Projekts

Erstellen Sie eine neue **Android Wear-Anwendung**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Erstellen einer neuen Android Wear-Anwendung im Dialogfeld "Neues Projekt"](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Erstellen einer neuen Android Wear-Anwendung im Dialogfeld "neue Projekt Mappe"](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----

Diese Vorlage enthält automatisch das nuget (und die Abhängigkeiten) der **xamarin Android-Wearable-Bibliothek** , sodass Sie auf Wear-spezifische Widgets zugreifen können. Wenn die Vorlage "Wear" nicht angezeigt wird, überprüfen Sie die [Installations-und Setup](~/android/wear/get-started/installation.md) Anleitung, um zu überprüfen, ob Sie eine unterstützte Android SDK installiert haben. 

### <a name="2-choose-the-correct-target-framework"></a>2. Wählen Sie das richtige **Ziel Framework** aus.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Stellen Sie sicher, dass **Android to target mindestens** auf **Android 5,0 (Lollipop)** oder höher festgelegt ist: 

[![Festlegen des Ziel-Frameworks auf Android 5,0 in Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Stellen Sie sicher, dass das Ziel Framework auf **Android 5,0 (Lollipop)** oder höher festgelegt ist:

[![Festlegen des Ziel-Frameworks auf Android 5,0 in Visual Studio für Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Weitere Informationen zum Festlegen des Ziel-Frameworks finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

### <a name="3-edit-the-mainaxml-layout"></a>3. Bearbeiten des **Main. axml** -Layouts

Konfigurieren Sie das Layout so, `TextView` dass ein `Button` und ein für das Beispiel enthalten sind: 

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

### <a name="4-edit-the-mainactivitycs-source"></a>4. **MainActivity.cs** Quelle bearbeiten

Fügen Sie den Code hinzu, um einen Zählers zu erhöhen und anzuzeigen, wenn auf die Schaltfläche geklickt wird: 

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

### <a name="5-setup-an-emulator-or-device"></a>5. Einrichten eines Emulators oder Geräts

Der nächste Schritt ist das Einrichten eines Emulators oder Geräts zum Bereitstellen und Ausführen der app. Wenn Sie noch nicht mit dem Prozess der Bereitstellung und Ausführung von xamarin. Android-Apps im allgemeinen vertraut sind, lesen Sie den [Schnellstart zu Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).

Wenn Sie nicht über ein Android Wear-Gerät wie z. b. Android Wear Smartwatch verfügen, können Sie die APP auf einem Emulator ausführen. Informationen zum Debuggen von Wear-apps auf einem Emulator finden Sie unter [Debuggen von Android Wear in einem Emulator](~/android/wear/deploy-test/debug-on-emulator.md).

Wenn Sie über ein Android Wear-Gerät wie z. b. eine Android Wear-Smartwatch verfügen, können Sie die APP auf dem Gerät ausführen, anstatt einen Emulator zu verwenden. Weitere Informationen zum Debuggen auf einem Wear-Gerät finden Sie unter [Debuggen auf einem Wear-Gerät](~/android/wear/deploy-test/debug-on-device.md).

### <a name="6-run-the-android-wear-app"></a>6. Ausführen der Android Wear-App

Das Android Wear-Gerät sollte im Pulldownmenü des Geräts angezeigt werden. Stellen Sie sicher, dass Sie das richtige Android Wear-Gerät oder AVD auswählen, bevor Sie das Debugging starten. Nachdem Sie das Gerät ausgewählt haben, klicken Sie auf die Schaltfläche Wiedergabe, um die APP auf dem Emulator oder Gerät bereitzustellen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Auswählen von "Wear AVD" im Visual Studio-Geräte Menü](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Auswählen von "Wear AVD" im Menü "Visual Studio für Mac Gerät"](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Möglicherweise wird eine **Meldung (oder ein anderer** Bildschirm mit Interstitial) an erster Stelle angezeigt: 

![Der Überwachungs Emulator zeigt nur eine Minute...](hello-wear-images/please-wait.png)

Wenn Sie einen Überwachungs Emulator verwenden, kann es eine Weile dauern, bis die APP gestartet wird. Wenn Sie Bluetooth verwenden, benötigt die Bereitstellung der APP mehr Zeit als über USB. (Beispiel: die Bereitstellung dieser APP auf einer LG G-Uhr, die mit einem Nexus 5-Telefon verbunden ist, dauert ungefähr 5 Minuten.)

Nachdem die APP erfolgreich bereitgestellt wurde, sollte auf dem Bildschirm des Wear-Geräts ein Bildschirm wie der folgende angezeigt werden:

[![Ursprünglicher Bildschirm der Wear-App](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Tippen **Sie auf den Klick!** auf der Vorderseite des Wear-Geräts, und sehen Sie sich die Anzahl Inkrement mit jeder Tap an:

[![Screenshot der Wear-App nach 3 Klicks](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Wear-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear) einschließlich Android Wear-apps mit Begleit telefonapps an.

Wenn Sie zur Verteilung Ihrer APP bereit sind, finden Sie weitere Informationen unter [Arbeiten mit der Paket](~/android/wear/deploy-test/packaging.md)Erstellung.

## <a name="related-links"></a>Verwandte Links

- [Klicken Sie auf me-app (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-weartest)
