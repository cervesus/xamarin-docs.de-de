---
title: Einstieg in Android
description: In diesem Dokument wird beschrieben, wie Sie mit dem Einbetten von .net mit Android beginnen. Es wird erläutert, wie Sie die .net-Einbettung, das Erstellen eines Android-Bibliotheks Projekts, die Verwendung der generierten Ausgabe in einem Android Studio-Projekt und weitere Überlegungen
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: bcda03d41cb3bafcfb3ee4b92046014cc5b0c119
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029768"
---
# <a name="getting-started-with-android"></a>Einstieg in Android

Zusätzlich zu den Anforderungen aus dem Leitfaden für die ersten Schritte [mit Java](~/tools/dotnet-embedding/get-started/java/index.md) benötigen Sie auch Folgendes:

- [Xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) oder höher
- [Android Studio 3. x](https://developer.android.com/studio/index.html) mit Java 1,8

Als Übersicht werden folgende Aktionen durchführt:

- Erstellen eines C# Android-Bibliotheks Projekts
- Installieren von .net-Einbettungen über nuget
- Ausführen der .net-Einbettung in der Android-Bibliotheksassembly
- Verwenden Sie die generierte Aar-Datei in einem Java-Projekt in Android Studio

## <a name="create-an-android-library-project"></a>Erstellen eines Android-Bibliotheks Projekts

Öffnen Sie Visual Studio für Windows oder Mac, erstellen Sie ein neues Android-Klassen Bibliotheksprojekt, benennen Sie es mit **Hello-from-CSharp**, und speichern Sie es in **~/projects/Hello-from-CSharp** oder **%UserProfile%\project\hello-from-CSharp**.

Fügen Sie eine neue Android-Aktivität mit dem Namen **HelloActivity.cs**gefolgt von einem Android-Layout unter **Resource/Layout/Hello. axml**hinzu.

Fügen Sie dem Layout eine neue `TextView` hinzu, und ändern Sie den Text in etwas unterhaltsames.

Die layoutquelle sollte in etwa wie folgt aussehen:

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

Stellen Sie in Ihrer Aktivität sicher, dass Sie `SetContentView` mit dem neuen Layout aufrufen:

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
> Vergessen Sie nicht das `[Register]` Attribut. Weitere Informationen finden Sie unter [Einschränkungen](#current-limitations-on-android).

Erstellen Sie das Projekt. Die resultierende Assembly wird in `bin/Debug/hello-from-csharp.dll`gespeichert.

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .net-Einbettungen über nuget

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) , um .net-Einbettungen für Ihr Projekt zu installieren und zu konfigurieren.

Der Befehls Aufruf, den Sie konfigurieren sollten, sieht wie folgt aus:

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

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Verwenden der generierten Ausgabe in einem Android Studio Projekt

1. Öffnen Sie Android Studio, und erstellen Sie ein neues Projekt mit einer **leeren Aktivität**.
2. Klicken Sie mit der rechten Maustaste auf das **App** -Modul, und wählen Sie **Neu > Modul**.
3. Wählen Sie **Importieren aus. JAR/. AAR-Paket**.
4. Verwenden Sie den Verzeichnis Browser, um **~/projects/Hello-from-CSharp/Output/hello_from_csharp.Aar** zu suchen, und klicken Sie auf **Finish**.

![Importieren von Aar in Android Studio](android-images/androidstudioimport.png)

Dadurch wird die Aar-Datei in ein neues Modul namens **hello_from_csharp**kopiert.

![Android Studio Projekt](android-images/androidstudioproject.png)

Wenn Sie das neue Modul aus Ihrer **App**verwenden möchten, klicken Sie mit der rechten Maustaste, und wählen Sie **Moduleinstellungen öffnen**. Fügen Sie auf der Registerkarte **Abhängigkeiten** eine neue **Modul Abhängigkeit** hinzu, und wählen Sie Folgendes aus **: hello_from_csharp**.

![Abhängigkeiten Android Studio](android-images/androidstudiodependencies.png)

Fügen Sie in Ihrer Aktivität eine neue `onResume`-Methode hinzu, und verwenden Sie den folgenden Code C# , um die-Aktivität zu starten:

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

### <a name="assembly-compression-important"></a>Assemblykomprimierung (*wichtig*)

Eine weitere Änderung ist für die .net-Einbettung in das Android Studio-Projekt erforderlich.

Öffnen Sie die Datei " **Build. gradle** " Ihrer APP, und fügen Sie die folgende Änderung hinzu:

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin. Android lädt derzeit .NET-Assemblys direkt aus dem APK, erfordert jedoch, dass die Assemblys nicht komprimiert werden.

Wenn Sie nicht über dieses Setup verfügen, stürzt die APP beim Start ab und druckt in der Konsole etwa wie folgt:

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Ausführen der App

Beim Starten der APP:

![Hello from C# Sample-Ausführung im Emulator](android-images/hello-from-csharp-android.png)

Beachten Sie, was hier passiert ist:

- Wir haben eine C# Klasse,`HelloActivity`, die Unterklassen Java
- Wir verfügen über Android-Ressourcen Dateien.
- Wir haben diese aus Java in Android Studio verwendet.

Damit dieses Beispiel funktioniert, werden alle folgenden Schritte im endgültigen APK eingerichtet:

- Xamarin. Android ist beim Anwendungsstart konfiguriert.
- In **Assets/** Assemblys enthaltene .NET-Assemblys
- " **Androidmanifest. XML** "- C# Änderungen für ihre Aktivitäten usw.
- Android-Ressourcen und-Assets aus .NET-Bibliotheken
- [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) für beliebige `Java.Lang.Object` Unterklassen

Wenn Sie nach einer zusätzlichen exemplarischen Vorgehensweise suchen, sehen Sie sich das folgende Video an, in dem das Einbetten der [fingerpaint-Demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) von Charles Petzold in ein Android Studio Projekt veranschaulicht wird:

[![embeddinator-4000 für Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Verwenden von Java 1,8

Zum Zeitpunkt der Erstellung dieses Artikels ist die beste Option die Verwendung von Android Studio 3,0 ([hier herunterladen](https://developer.android.com/studio/index.html)).

So aktivieren Sie Java 1,8 in der Datei " **Build. gradle** " Ihres App-Moduls:

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Weitere Informationen finden Sie auch in einem [Android Studio Testprojekt](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) . 

Wenn Sie Android Studio 2.3. x-stabil verwenden möchten, müssen Sie die als veraltet markierte Jack-Toolkette aktivieren:

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Aktuelle Einschränkungen bei Android

Wenn Sie in der Unterklasse `Java.Lang.Object`, generiert xamarin. Android den Java-Stub (Android Callable Wrapper) anstelle von .net-Einbettungen. Aus diesem Grund müssen Sie die gleichen Regeln für den Export C# in Java als xamarin. Android befolgen. Beispiel:

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

- `[Register]` ist erforderlich, um den Namen eines gewünschten Java-Pakets zuzuordnen.
- `[Export]` ist erforderlich, um eine Methode für Java sichtbar zu machen.

Wir können `ViewSubclass` in Java wie folgt verwenden:

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Erfahren Sie mehr über die [Java-Integration mit xamarin. Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Mehrere Assemblys

Das Einbetten einer einzelnen Assembly ist unkompliziert. Es ist jedoch viel wahrscheinlicher, dass Sie mehr als eine C# Assembly haben. Häufig haben Sie Abhängigkeiten von nuget-Paketen, wie z. b. den Android-Unterstützungs Bibliotheken oder Google Play Services, die die Dinge komplizierter machen.

Dies verursacht ein Problem, da .net-Einbettungen viele Typen von Dateien in das abschließende Aar einschließen müssen, wie z. b.:

- Android-Ressourcen
- Android-Ressourcen
- Native Android-Bibliotheken
- Android-Java-Quelle

Sie möchten diese Dateien höchstwahrscheinlich nicht aus der Android-Unterstützungs Bibliothek einschließen oder Google Play Services in Ihr Aar, sondern verwenden stattdessen die offizielle Version von Google in Android Studio.

Dies ist die empfohlene Vorgehensweise:

- Übergeben Sie .net Einbettungen für eine beliebige Assembly, deren Besitzer Sie sind (für die Sie über eine Quelle verfügen), und möchten Sie
- Übergeben Sie .net-Einbettungen für beliebige Assemblys, für die Sie Android-Objekte, Native Bibliotheken oder Ressourcen benötigen
- Fügen Sie Java-Abhängigkeiten wie die Android-Unterstützungs Bibliothek hinzu, oder Google Play Services in Android Studio

Der Befehl könnte also wie folgt lauten:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Sie sollten alle Elemente von nuget ausschließen, es sei denn, Sie haben feststellen, dass Sie Android-Assets, Ressourcen usw. enthalten, die Sie in Ihrem Android Studio Projekt benötigen. Sie können auch Abhängigkeiten weglassen, die nicht von Java aufgerufen werden müssen, und der Linker _sollte_ die benötigten Teile der Bibliothek einschließen.

Zum Hinzufügen von Java-Abhängigkeiten, die in Android Studio benötigt werden, könnte die Datei **Build. gradle** wie folgt aussehen:

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Weiterführende Themen

- [Rückrufe unter Android](~/tools/dotnet-embedding/android/callbacks.md)
- [Vorläufige Android-Recherche](~/tools/dotnet-embedding/android/index.md)
- [.Net-Einbettungs Einschränkungen](~/tools/dotnet-embedding/limitations.md)
- [Mitwirken am Open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Verwandte Links

- [Wetter Beispiel (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
