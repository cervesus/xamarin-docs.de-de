---
title: Erste Schritte mit Android
description: Dieses Dokument beschreibt, wie die ersten Schritte mit Android mit .NET einbetten. Es wird erläutert, installieren .NET einbetten, erstellen eine Bibliothek für Android-Projekt mit der generierten Ausgabe in einem Android Studio-Projekt und andere Aspekte.
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 0bb27df901d306e97151cfeaad5c26130508e5b1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066480"
---
# <a name="getting-started-with-android"></a>Erste Schritte mit Android

Zusätzlich zu den von der [erste Schritte mit Java](~/tools/dotnet-embedding/get-started/java/index.md) Handbuch Sie auch müssen:

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) oder höher
* [Android Studio 3.x](https://developer.android.com/studio/index.html) mit Java 1.8

Als eine Übersicht über wird Folgendes beschrieben:

* Erstellen Sie eine C#-Android-Steuerelementbibliothek-Projekt
* Installieren Sie .NET einbetten über NuGet
* Führen Sie für die Bibliothek für Android-Assembly Einbetten von .NET
* Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio

## <a name="create-an-android-library-project"></a>Erstellen Sie eine Bibliothek für Android-Projekt

Öffnen Sie Visual Studio für Windows- oder Mac, Erstellen eines neuen Projekts für Android-Klassenbibliothek, nennen Sie sie **Hello aus Csharp**, und speichern Sie es **~/Projects/hello-from-csharp** oder **%USERPROFILE%\ Projects\hello aus Csharp**.

Hinzufügen einer neuen Android-Aktivität, die mit dem Namen **"helloactivity.cs"**, gefolgt von einer Android Layout zur **Resource/layout/hello.axml**.

Fügen Sie einen neuen `TextView` auf das Layout und die Änderung der Text eine angenehme.

Quellcode Layout sollte etwa wie folgt aussehen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

In Ihre Aktivität sicher, dass Sie aufrufen `SetContentView` mit das neue Layout:

```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```

> [!NOTE]
> Vergessen Sie nicht die `[Register]` Attribut. Weitere Informationen finden Sie unter [Einschränkungen](#current-limitations-on-android).

Erstellen Sie das Projekt. Die resultierende Assembly im gespeichert `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) installieren und Konfigurieren von .NET einbetten für das Projekt.

Der Befehl aufrufen, die Sie konfigurieren, sollten wird wie folgt aussehen:

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Verwenden Sie die generierte Ausgabe in einem Android Studio-Projekt

1. Android Studio öffnen und erstellen Sie ein neues Projekt mit einer **leere Aktivität**.
2. Mit der rechten Maustaste auf die **app** Modul, und wählen Sie **neu > Modul**.
3. Wählen Sie **importieren. JAR /. AAR Paket**.
4. Verwenden Sie den Directory-Browser um zu suchende **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** , und klicken Sie auf **Fertig stellen**.

![Import AAR in Android Studio](android-images/androidstudioimport.png)

Hierbei werden die AAR-Datei kopiert, in ein neues Modul mit dem Namen **Hello_from_csharp**.

![Android Studio-Projekt](android-images/androidstudioproject.png)

Verwenden Sie das neue Modul aus Ihrem **app**mit der rechten Maustaste auf, und wählen Sie **öffnen Moduleinstellungen**. Auf der **Abhängigkeiten** Registerkarte, fügen Sie einen neuen **Modul Abhängigkeit** , und wählen Sie **: Hello_from_csharp**.

![Android Studio Abhängigkeiten](android-images/androidstudiodependencies.png)

Fügen Sie in Ihre Aktivität einen neuen `onResume` -Methode, und verwenden Sie den folgenden Code hinzu, die C#-Aktivität zu starten:

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Assembly-Komprimierung (*wichtig*)

Eine weitere Änderung ist für das Einbetten von .NET in Ihrem Projekt Android Studio erforderlich.

Öffnen Sie die app **build.gradle** Datei, und fügen Sie die folgende Änderung hinzu:

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin.Android lädt .NET Assemblys derzeit direkt aus der APK, aber es erfordert, dass die Assemblys nicht komprimiert werden.

Wenn Sie keine des Setups wird die app stürzt beim Start ab und etwa wie folgt auf der Konsole ausgegeben:

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Ausführen der App

Beim Starten der app an:

![Hallo von C#-Beispiel in den Emulator ausführen](android-images/hello-from-csharp-android.png)

Beachten Sie, was hier geschehen ist:

* Wir haben eine C#-Klasse, `HelloActivity`, diese Unterklassen Java
* Wir haben Android Ressourcendateien
* Wird diese von Java in Android Studio

Für dieses Beispiel funktioniert werden alle folgenden in der endgültigen APK einrichten:

* Xamarin.Android wird beim Start der Anwendung konfiguriert.
* In enthaltenen .NET Assemblys **Bestand-Assemblys**
* **AndroidManifest.xml** Änderungen für die C#-Aktivitäten usw.
* Android-Ressourcen und Ressourcen von Bibliotheken für .NET
* [Android Aufrufwrappern](~/android/platform/java-integration/android-callable-wrappers.md) für eine beliebige `Java.Lang.Object` Unterklasse

Wenn Sie eine zusätzliche exemplarischen Vorgehensweise suchen, sehen Sie sich das folgende Video an, die beim Einbetten des Charles veranschaulicht [FingerPaint Demo](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) in Android Studio-Projekt:

[![Embeddinator-4000 für Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Mithilfe von Java 1.8

Zum Schreiben von dies die beste Option ist die Verwendung von Android Studio 3.0 ([hier herunterladen](https://developer.android.com/studio/index.html)).

So aktivieren Sie Java 1.8 in Ihrem app-Moduls **build.gradle** Datei:

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Sie können außerdem sehen Sie sich ein [Android Studio Testprojekt](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) Weitere Details. 

Wenn Sie Android Studio 2.3.x stabile verwenden möchte, müssen Sie die veraltete Stecker-toolkette zu aktivieren:

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Aktuelle Einschränkungen für Android

Mit der rechten Maustaste, wenn Sie eine Unterklasse von `Java.Lang.Object`, Xamarin.Android generiert die Java-Stub (Android Callable Wrapper) anstatt .NET einzubetten. Aus diesem Grund müssen Sie die gleichen Regeln zum Exportieren von c#, Java als Xamarin.Android folgen. Zum Beispiel:

```csharp
[Register("mono.embeddinator.android.ViewSubclass")]
public class ViewSubclass : TextView
{
    public ViewSubclass(Context context) : base(context) { }

    [Export("apply")]
    public void Apply(string text)
    {
        Text = text;
    }
}
```

* `[Register]` ist erforderlich, um eine gewünschte Java-Paketname zuordnen
* `[Export]` ist erforderlich, um eine Methode für Java sichtbar zu machen.

Wir können `ViewSubclass` in Java wie folgt:

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Erfahren Sie mehr über [Java-Integration mit Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Mehrere Assemblys

Einbetten von einer einzelnen Assembly ist einfach; Es ist jedoch weitaus höheren Wahrscheinlichkeit, dass Sie über mehr als ein C#-Assembly verfügen werden. Häufig müssen Sie Abhängigkeiten auf NuGet-Pakete, z. B. die Bibliotheken für Android, unterstützen oder Google Play-Dienste, die weitere Dinge erschweren.

Dies bewirkt, dass ein Dilemma, da .NET Einbetten in der endgültigen AAR wie z. B. viele Arten von Dateien enthalten muss:

* Android-Objekte
* Android-Ressourcen
* Android systemeigene Bibliotheken
* Android-Java-Quellcode

Sie sehr wahrscheinlich nicht diese Dateien aus der Bibliothek für Android, unterstützen oder Google Play-Dienste in Ihre AAR einschließen möchten, aber verwenden stattdessen die offizielle Version von Google in Android Studio.

Hier ist die empfohlene Vorgehensweise:

* Übergeben Sie die .NET einbetten jede Assembly, die Sie besitzen (Quelle für haben) und aus Java aufgerufen werden soll
* Übergeben Sie jede Assembly, der Sie benötigen, Android, systemeigene Bibliotheken oder Ressourcen aus .NET einbetten
* Das Hinzufügen eines Java-Abhängigkeiten wie der Android-Bibliothek oder Google Play-Dienste in Android Studio unterstützen

Der Befehl könnte folgendermaßen aussehen:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Sollten Sie alles von NuGet ausschließen, es sei denn, Sie feststellen, enthält Android Bestand, Ressourcen usw., die Sie im Android Studio-Projekt müssen. Sie können auch weglassen, Abhängigkeiten, die nicht von Java und der Linker aufgerufen werden muss _sollten_ enthalten die Teile der Bibliothek, die erforderlich sind.

Hinzufügen von Abhängigkeiten Java in Android Studio benötigt Ihre **build.gradle** Datei kann folgendermaßen aussehen:

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Weiterführende Themen

* [Rückrufe für Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Vorläufige Android Research](~/tools/dotnet-embedding/android/index.md)
* [.NET Einbetten von Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Verwandte Links

- [Wetter-Beispiel (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
