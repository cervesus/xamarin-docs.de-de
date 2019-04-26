---
title: Erste Schritte mit Android
description: Dieses Dokument beschreibt, wie die ersten Schritte mit Android mit Einbetten von .NET. Installieren von .NET einbetten, erstellen ein Android-Bibliotheksprojekt, verwenden die generierte Ausgabe in eine Android Studio-Projekt und andere Aspekte werden erörtert.
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: e60892edfcf73f3e7cada8923e16bcc1be2c203e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60876901"
---
# <a name="getting-started-with-android"></a>Erste Schritte mit Android

Zusätzlich zu den Anforderungen von der [erste Schritte mit Java](~/tools/dotnet-embedding/get-started/java/index.md) Anleitung, die Sie auch benötigen:

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) oder höher
* [Android Studio 3.x](https://developer.android.com/studio/index.html) mit Java 1.8

Einen Überblick über wird Folgendes beschrieben:

* Erstellen Sie eine C# Android-Bibliotheksprojekt
* Einbetten von .NET über NuGet zu installieren
* Führen Sie für die Android-Bibliothek-Assembly Einbetten von .NET
* Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio

## <a name="create-an-android-library-project"></a>Erstellen eines Projekts für Android-Bibliothek

Öffnen Sie Visual Studio für Windows oder Mac zu erstellen, Erstellen eines neuen Projekts für Android-Klassenbibliothek, nennen Sie es **Hello-aus-Csharp**, und speichern sie **~/Projects/hello-from-csharp** oder **%USERPROFILE%\ Projects\hello-aus-Csharp**.

Fügen Sie eine neue Android-Aktivität, die mit dem Namen **"helloactivity.cs"**, gefolgt von einer Android-Layout auf **Resource/layout/hello.axml**.

Fügen Sie einen neuen `TextView` Ihr Layout und die Änderung der Text, der etwas unterhaltsam.

Die Quelle Layout sollte etwa wie folgt aussehen:

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

In der Aktivität sicherzustellen, dass Sie aufrufen `SetContentView` mit das neue Layout:

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

Erstellen Sie das Projekt. Die resultierende Assembly wird in gespeichert `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) installieren und konfigurieren Sie für Ihr Projekt Einbetten von .NET.

Der Befehlsaufruf, die, den Sie konfigurieren sollten, wird wie folgt aussehen:

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

1. Öffnen Sie Android Studio, und erstellen Sie ein neues Projekt mit einem **Empty Activity**.
2. Mit der rechten Maustaste auf Ihre **app** Modul, und wählen Sie **neu > Modul**.
3. Wählen Sie **importieren. JAR /. Zusätzlich zum AAR-Paket**.
4. Verwenden Sie den Directory-Browser gefunden **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** , und klicken Sie auf **Fertig stellen**.

![Import AAR in Android Studio](android-images/androidstudioimport.png)

Dadurch wird die AAR-Datei kopiert, in ein neues Modul mit dem Namen **Hello_from_csharp**.

![Android Studio Project](android-images/androidstudioproject.png)

Verwenden Sie das neue Modul aus Ihrem **app**mit der rechten Maustaste auf, und wählen Sie **Moduleinstellungen öffnen**. Auf der **Abhängigkeiten** Registerkarte, fügen Sie einen neuen **Modul Abhängigkeit** , und wählen Sie **: Hello_from_csharp**.

![Android Studio-Abhängigkeiten](android-images/androidstudiodependencies.png)

Fügen Sie in der Aktivität eine neue `onResume` -Methode, und verwenden Sie den folgenden code zum Starten der C# Aktivität:

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

Eine weitere Änderung ist für das Einbetten von .NET in Ihre Android Studio-Projekt erforderlich.

Öffnen Sie Ihre app die **build.gradle** -Datei und fügen Sie die folgende Änderung:

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin.Android lädt derzeit .NET Assemblys direkt aus dem APK, aber es erfordert, dass die Assemblys nicht komprimiert werden.

Wenn Sie nicht mit diesem Setup verfügen, wird die app beim Start stürzt ab und etwa wie folgt auf der Konsole ausgegeben:

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Ausführen der App

Beim Starten der app an:

![Hello aus C# Beispiel im Emulator](android-images/hello-from-csharp-android.png)

Beachten Sie, was hier passiert ist:

* Wir haben eine C# -Klasse, `HelloActivity`, diese Unterklassen Java
* Wir haben die Android-Ressourcen-Dateien
* Verwendet diese aus Java in Android Studio

Für dieses Beispiel funktioniert sind alle folgenden in das endgültige APK eingerichtet:

* Xamarin.Android wird beim Start der Anwendung konfiguriert.
* .NET Assemblys, die in enthaltenen **Assets/Assemblys**
* **"Androidmanifest.xml"** Änderungen für Ihre C# usw. Aktivitäten.
* Android-Ressourcen und Objekte aus den Bibliotheken für .NET
* [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) für alle `Java.Lang.Object` Unterklasse

Wenn Sie eine zusätzliche exemplarische Vorgehensweise suchen, sehen Sie sich Folgendes Video, das die veranschaulicht, Einbetten von Charles petzolds [FingerPaint Demo](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) in einem Android Studio-Projekt:

[![Embeddinator-4000 für Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Mithilfe von Java 1.8

Zum Zeitpunkt der Erstellung dieses Artikels, die beste Option ist die Verwendung von Android Studio 3.0 ([hier herunterladen](https://developer.android.com/studio/index.html)).

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

Sie können außerdem sehen Sie sich ein [Android Studio-Testprojekt](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) Weitere Details. 

Wenn Sie Android Studio 2.3.x stabile verwenden möchten, müssen Sie die veraltete Jack-toolkette zu aktivieren:

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Aktuelle Einschränkungen unter Android

Moment, wenn Sie eine Unterklasse von `Java.Lang.Object`, Xamarin.Android, generiert den Java-Stub (Android Callable Wrapper) anstelle von Einbetten von .NET. Aus diesem Grund müssen Sie gelten dieselben Regeln für den Export von C# auf Java als Xamarin.Android. Zum Beispiel:

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

* `[Register]` ist erforderlich, um einen gewünschten Java-Paket-Namen zuordnen
* `[Export]` ist erforderlich, um eine Methode für Java sichtbar zu machen

Wir können `ViewSubclass` in Java wie folgt:

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Erfahren Sie mehr über [Java-Integration mit Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Mehrere Assemblys

Eine einzelne Assembly einbetten ist einfach. Es ist jedoch sehr viel wahrscheinlicher müssen mehrere C# Assembly. Oft müssen Sie Abhängigkeiten von NuGet-Paketen, z. B. das Android-Unterstützungsbibliotheken oder Google Play-Dienste, die Dinge weiter zu erschweren.

Dies führt zu einem Dilemma, da Einbetten von .NET auf viele Arten von Dateien in der endgültigen AAR wie z. B. enthalten muss:

* Android-Objekten
* Android-Ressourcen
* Android native Bibliotheken
* Android Java-Ressourcen

Sie in den meisten Fällen möchten nicht diese Dateien über die Android Support-Bibliothek oder Google Play-Dienste in Ihrer AAR werden sollen, sondern vielmehr die offizielle Version von Google in Android Studio verwenden.

Hier ist die empfohlene Vorgehensweise:

* Übergeben Sie Einbetten von .NET eine beliebige Assembly, die Sie besitzen (Quelle haben) und von Java aufgerufen werden soll
* Übergeben Sie Einbetten von .NET eine beliebige Assembly, die Sie Android-Objekten, die nativen Bibliotheken oder Ressourcen aus
* Fügen Sie, dass die Java-Abhängigkeiten, wie der Android-Bibliothek oder Google Play-Dienste in Android Studio unterstützen

Der Befehl könnte folgendermaßen aussehen:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Sollten Sie nichts aus NuGet ausschließen, es sei denn, Sie herausfinden, sondern Android-Objekten, Ressourcen usw., die Sie im Android Studio-Projekt müssen. Sie können auch weglassen, Abhängigkeiten, die Sie nicht benötigen, Aufrufen von Java und der Linker _sollten_ sind z.B. Teile Ihrer Bibliothek, die erforderlich sind.

Hinzufügen von jeder Java-Abhängigkeiten, die in Android Studio benötigt Ihre **build.gradle** Datei könnte folgendermaßen aussehen:

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Weiterführende Themen

* [Rückrufe, die unter Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Vorläufige Android Research](~/tools/dotnet-embedding/android/index.md)
* [Einbetten von .NET-Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirkung an open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Verwandte Links

- [Wetter-Beispiel (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
