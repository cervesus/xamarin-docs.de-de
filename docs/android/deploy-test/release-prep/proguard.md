---
title: ProGuard
description: Xamarin.Android ProGuard ist ein Tool zur Verkleinerung, Optimierung und Vorüberprüfung von Java-Klassendateien. Es erkennt nicht verwendeten Code und entfernt diesen und analysiert und optimiert Bytecode. Dieser Leitfaden erläutert, wie ProGuard funktioniert, wie Sie dieses Tool in Ihrem Projekt aktivieren und konfigurieren. Darüber hinaus werden einige Beispiele für ProGuard-Konfigurationen bereitgestellt.
ms.prod: xamarin
ms.assetid: 29C0E850-3A49-4618-9078-D59BE0284D5A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f6f3377c4fdeedefa3277d05012ec868f6626c41
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669998"
---
# <a name="proguard"></a>ProGuard

_Xamarin.Android ProGuard ist ein Tool zur Verkleinerung, Optimierung und Vorüberprüfung von Java-Klassendateien. Es erkennt nicht verwendeten Code und entfernt diesen und analysiert und optimiert Bytecode. Dieser Leitfaden erläutert, wie ProGuard funktioniert, wie Sie dieses Tool in Ihrem Projekt aktivieren und konfigurieren. Darüber hinaus werden einige Beispiele für ProGuard-Konfigurationen bereitgestellt._


## <a name="overview"></a>Übersicht

ProGuard erkennt und entfernt nicht verwendete Klassen, Felder, Methoden und Attribute aus der gepackten Anwendung. Er kann sogar dasselbe für die Bibliotheken tun, auf die verwiesen wird (damit können Sie die 64 KB-Verweisbeschränkung vermeiden). Das ProGuard-Tool aus dem Android SDK optimiert ebenfalls Bytecode und entfernt nicht verwendete Codeanweisungen. ProGuard liest **Eingabe-JAR-Dateien** und verkleinert, optimiert und überprüft diese im Voraus. Die Ergebnisse schreibt das Tool anschließend in eine oder mehrere **Ausgabe-JAR-Dateien**. 

ProGuard verarbeitet Eingabe-APKs mithilfe der folgenden Schritte: 

1.  **Verkleinerungsschritt**: ProGuard bestimmt rekursiv, welche Klassen und Klassenmember verwendet werden. Alle anderen Klassen und Klassenmember werden verworfen. 

2.  **Optimierungsschritt**: ProGuard optimiert den Code weiter. 
    Optimierungen, Klassen und Methoden, die keine Einstiegspunkte sind, können privat, statisch oder abschließend gemacht, nicht verwendete Parameter können entfernt, und einige Methoden als Inlinefunktionen umgesetzt werden. 

3.  **Obfuskationsschritt:** Bei der nativen Android-Entwicklung benennt ProGuard Klassen und Klassenmember um, die keine Einstiegspunkte sind. Das Beibehalten von Einstiegspunkten stellt sicher, dass auf sie immer noch von deren ursprünglichen Namen zugegriffen werden kann. Dieser Schritt wird jedoch nicht von Xamarin.Android unterstützt, da die App in eine Zwischensprache kompiliert wird.

4.  **Vorüberprüfungsschritt**: Führt Überprüfungen von Java-Bytecode vor der Laufzeit aus und kommentiert Klassendateien für die Java-VM. Dies ist der einzige Schritt, der die Einstiegspunkte nicht kennen muss. 

Jeder dieser Schritte ist *optional*. Wie im nächsten Abschnitt erläutert wird, verwendet Xamarin.Android ProGuard nur eine Teilmenge der folgenden Schritte. 



## <a name="proguard-in-xamarinandroid"></a>ProGuard in Xamarin.Android

Die Xamarin.Android ProGuard-Konfiguration verbirgt das APK nicht. Es ist tatsächlich nicht möglich, die Verschleierung über ProGuard zu aktivieren (auch nicht durch Verwendung von benutzerdefinierten Konfigurationsdateien). Daher führt das ProGuard-Tool von Xamarin.Android nur die Schritte **Verkleinern** und **Optimieren** aus: 

[![Verkleinerungs- und Optimierungsschritte](proguard-images/01-xa-chain-sml.png)](proguard-images/01-xa-chain.png#lightbox)

Ein wichtiges Element, das Sie kennen sollten, bevor Sie ProGuard verwenden, ist die Funktionsweise innerhalb des `Xamarin.Android`-Buildprozesses. Dieser Prozess verwendet zwei separate Schritte: 

1.  Xamarin Android-Linker

2.  ProGuard

Jeder dieser Schritte wird nachfolgend beschrieben.



### <a name="linker-step"></a>Linker-Schritt

Der Xamarin.Android-Linker führt die folgende statische Analyse der Anwendung aus, um Folgendes zu ermitteln: 

-   Welche Assemblys tatsächlich verwendet werden.

-   Welche Typen tatsächlich verwendet werden.

-   Welche Mitglieder tatsächlich verwendet werden. 

Der Linker wird stets vor dem ProGuard-Schritt ausgeführt werden. Aus diesem Grund kann der Linker eine Assembly, einen Typ, ein Mitglied entfernen, auf dem ProGuard ausgeführt werden sollte. (Weitere Informationen zum Verknüpfen von Xamarin.Android finden Sie unter [Linking on Android (Unter Android verknüpfen)](~/android/deploy-test/linker.md).)



### <a name="proguard-step"></a>ProGuard-Schritt

Nach dem erfolgreichem Abschluss des Linker-Schritts wird ProGuard ausgeführt, um nicht verwendeten Java Bytecode zu entfernen. Dies ist der Schritt, der das APK optimiert. 



## <a name="using-proguard"></a>Verwenden von ProGuard

Um ProGuard im Anwendungsprojekt zu verwenden, müssen Sie zuerst ProGuard aktivieren. Als Nächstes können Sie den Xamarin.Android-Buildprozess entweder eine ProGuard-Standardkonfigurationsdatei verwenden lassen, oder Sie können eigene ProGuard-Standardkonfigurationsdateien für die Verwendung erstellen. 



### <a name="enabling-proguard"></a>Aktivieren von ProGuard

Verwenden Sie die folgenden Schritte, um ProGuard im Anwendungsprojekt zu aktivieren:

1.  Stellen Sie sicher, dass das Projekt auf die Konfiguration **Freigabe** festgelegt ist (Dies ist wichtig, da der Linker für die Ausführung von ProGuard ausgeführt werden muss): 

    [![Releasekonfiguration auswählen](proguard-images/02-set-release-sml.png)](proguard-images/02-set-release.png#lightbox)
   
2.  Aktivieren Sie ProGuard, indem Sie die Option **ProGuard aktivieren** in der Registerkarte **Verpackung** unter **Eigenschaften > Android-Optionen** aktivieren: 

    [![Option „ProGuard aktivieren“ ausgewählt](proguard-images/03-enable-proguard-sml.png)](proguard-images/03-enable-proguard.png#lightbox)

Für die meisten Xamarin.Android-Anwendungen ist die von Xamarin.Android unterstützte ProGuard-Standardkonfigurationsdatei ausreichend, um den gesamten (und nur) nicht verwendeten Code zu entfernen. Zur Anzeige der ProGuard-Standardkonfiguration öffnen Sie die Datei unter **obj\\Release\\proguard\\proguard_xamarin.cfg**. Im nächsten Abschnitt wird beschrieben, wie eine benutzerdefinierte ProGuard-Konfigurationsdatei erstellt wird. 



### <a name="customizing-proguard"></a>Anpassen von ProGuard

Optional können Sie eine benutzerdefinierte ProGuard-Konfigurationsdatei hinzufügen, um mehr Kontrolle über die ProGuard-Tools auszuüben. Beispielsweise empfiehlt es sich, ProGuard explizit mitzuteilen, welche Klassen beibehalten werden sollen. Zu diesem Zweck erstellen Sie eine neue **.cfg**-Datei und wenden die `ProGuardConfiguration`-Buildaktion im Bereich **Eigenschaften** des **Projektmappen-Explorers** an: 

[![Buildvorgang „ProguardConfiguration“ ausgewählt](proguard-images/04-build-action-sml.png)](proguard-images/04-build-action.png#lightbox)

Beachten Sie, dass diese Konfigurationsdatei nicht die Datei **proguard_xamarin.cfg** von Xamarin.Android ersetzt, da beide von ProGuard verwendet werden. 

Das folgende Beispiel veranschaulicht eine typische ProGuard-Konfigurationsdatei:
    

    # This is Xamarin-specific (and enhanced) configuration.

    -dontobfuscate

    -keep class mono.MonoRuntimeProvider { *; <init>(...); }
    -keep class mono.MonoPackageManager { *; <init>(...); }
    -keep class mono.MonoPackageManager_Resources { *; <init>(...); }
    -keep class mono.android.** { *; <init>(...); }
    -keep class mono.java.** { *; <init>(...); }
    -keep class mono.javax.** { *; <init>(...); }
    -keep class opentk.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk.GameViewBase { *; <init>(...); }
    -keep class opentk_1_0.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk_1_0.GameViewBase { *; <init>(...); }

    -keep class android.runtime.** { <init>(***); }
    -keep class assembly_mono_android.android.runtime.** { <init>(***); }
    # hash for android.runtime and assembly_mono_android.android.runtime.
    -keep class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }
    -keepclassmembers class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }

    # Android's template misses fluent setters...
    -keepclassmembers class * extends android.view.View {
       *** set*(***);
    }

    # also misses those inflated custom layout stuff from xml...
    -keepclassmembers class * extends android.view.View {
       <init>(android.content.Context,android.util.AttributeSet);
       <init>(android.content.Context,android.util.AttributeSet,int);
    }
    

Es gibt möglicherweise Fälle, in denen ProGuard die Anwendung nicht ordnungsgemäß analysieren kann. Das Tool könnte potenziell Code entfernen, den die Anwendung tatsächlich benötigt. In diesem Fall können Sie eine `-keep`-Zeile zu Ihrer benutzerdefinierten ProGuard-Konfigurationsdatei hinzufügen: 

    -keep public class MyClass

In diesem Beispiel wird `MyClass` auf den tatsächlichen Namen der Klasse festgelegt, die ProGuard überspringen soll.

Sie können auch eigene Namen mit `[Register]`-Kommentaren registrieren und diese Namen verwenden, um ProGuard-Regeln anzupassen. Sie können Namen für Adapter, Ansichten, BroadcastReceivers, Dienste, ContentProviders, Aktivitäten und Fragmente registrieren. Weitere Informationen zum Verwenden des benutzerdefinierten `[Register]`-Attributs finden Sie unter [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md).


### <a name="proguard-options"></a>ProGuard-Optionen

ProGuard bietet eine Reihe von Optionen, die Sie konfigurieren können, um eine präzisere Steuerung des Vorgangs bereitzustellen. Das [ProGuard-Handbuch](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/introduction.html) enthält die vollständige Referenzdokumentation für die Verwendung von ProGuard. 

Xamarin.Android unterstützt die folgenden ProGuard-Optionen: 


-    [Eingabe-/Ausgabeoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#iooptions)

-    [Beibehaltungsoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptions)

-    [Verkleinerungsoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#shrinkingoptions)

-    [Allgemeine Optionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#generaloptions)

-    [Klassenpfade](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classpath)

-    [Dateinamen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filename)

-    [Dateifilter](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filefilters)

-    [Filter](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filters)

-    [Übersicht über `Keep`-Optionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoverview)

-    [Modifizierer von Beibehaltungsoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptionmodifiers)

-    [Klassenspezifikationen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classspecification)

Die folgenden Optionen werden von Xamarin.Android *ignoriert*:

-    [Optimierungsoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#optimizationoptions)

-    [Verschleierungsoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#obfuscationoptions) 

-    [Vorüberarbeitungsoptionen](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#preverificationoptions)



## <a name="proguard-and-android-nougat"></a>ProGuard und Android Nougat

Wenn Sie ProGuard mit Android 7.0 oder höher verwenden möchten, müssen Sie eine neuere Version von ProGuard herunterladen, da das Android SDK nicht über eine neue Version verfügt, die mit JDK 1.8 kompatibel ist.

Sie können dieses [NuGet-Paket](https://www.nuget.org/packages/name.atsushieno.proguard.facebook/5.3.0) verwenden, um eine neuere `proguard.jar`-Version zu installieren. Weitere Informationen zum Aktualisieren des Standard-Android SDKs `proguard.jar`, finden Sie in dieser [Stack Overflow](https://stackoverflow.com/questions/39514518/xamarin-android-proguard-unsupported-class-version-number-52-0/39514706#39514706)-Erläuterung.

Sie finden alle Versionen von ProGuard auf der [SourceForge-Seite](https://sourceforge.net/projects/proguard/files/). 



## <a name="example-proguard-configurations"></a>ProGuard-Beispielkonfigurationen

Zwei ProGuard-Beispielkonfigurationsdateien sind unten aufgeführt. Bitte beachten Sie, dass der Xamarin.Android-Erstellungsvorgang in diesen Fällen die Jars **Eingabe**, **Ausgabe** und **Bibliothek** bereitstellt. Daher können Sie sich auf andere Optionen wie `-keep` konzentrieren. 

### <a name="a-simple-android-activity"></a>Eine einfache Android-Aktivität

Im folgenden Beispiel wird die Konfiguration für eine einfache Android-Aktivität veranschaulicht:

    -injars  bin/classes
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic

    -keep public class mypackage.MyActivity

### <a name="a-complete-android-application"></a>Eine vollständige Android-Anwendung

Im folgenden Beispiel wird die Konfiguration für eine vollständige Android-App veranschaulicht:

    -injars  bin/classes
    -injars  libs
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic
    -keepattributes *Annotation*

    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider

    -keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
    }

    -keepclassmembers class * implements android.os.Parcelable {
    static android.os.Parcelable$Creator CREATOR;
    }

    -keepclassmembers class **.R$* {
    public static <fields>;
    }


## <a name="proguard-and-the-xamarinandroid-build-process"></a>ProGuard und Xamarin.Android-Buildprozess

In den folgenden Abschnitten wird erläutert, wie ProGuard ein Xamarin.Android-**Releasebuild** ausführt.


### <a name="what-command-is-proguard-running"></a>Welchen Befehl führt ProGuard aus?

ProGuard ist einfach ein `.jar`, der mit dem Android SDK geliefert wird. Daher wird in einem Befehl Folgendes aufgerufen: 

```shell
java -jar proguard.jar options ...
```

### <a name="the-proguard-task"></a>Die ProGuard-Aufgabe

Die ProGuard-Aufgabe befindet sich innerhalb der **Xamarin.Android.Build.Tasks.dll**-Assembly. Sie ist Teil des `_CompileToDalvikWithDx`-Ziels, das Bestandteil des `_CompileDex`-Ziels ist. 

Die folgende Liste enthält ein Beispiel für die Standardparameter, die generiert werden, nachdem Sie ein neues Projekt mit **Datei > Neues Projekt** erstellen: 

    ProGuardJarPath = C:\Android\android-sdk\tools\proguard\lib\proguard.jar
    AndroidSdkDirectory = C:\Android\android-sdk\
    JavaToolPath = C:\Program Files (x86)\Java\jdk1.8.0_92\\bin
    ProGuardToolPath = C:\Android\android-sdk\tools\proguard\
    JavaPlatformJarPath = C:\Android\android-sdk\platforms\android-25\android.jar
    ClassesOutputDirectory = obj\Release\android\bin\classes
    AcwMapFile = obj\Release\acw-map.txt
    ProGuardCommonXamarinConfiguration = obj\Release\proguard\proguard_xamarin.cfg
    ProGuardGeneratedReferenceConfiguration = obj\Release\proguard\proguard_project_references.cfg
    ProGuardGeneratedApplicationConfiguration = obj\Release\proguard\proguard_project_primary.cfg
    ProGuardConfigurationFiles

      {sdk.dir}tools\proguard\proguard-android.txt;
      {intermediate.common.xamarin};
      {intermediate.references};
      {intermediate.application};
      ;
     
    JavaLibrariesToEmbed = C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar
    ProGuardJarInput = obj\Release\proguard\__proguard_input__.jar
    ProGuardJarOutput = obj\Release\proguard\__proguard_output__.jar
    DumpOutput = obj\Release\proguard\dump.txt
    PrintSeedsOutput = obj\Release\proguard\seeds.txt
    PrintUsageOutput = obj\Release\proguard\usage.txt
    PrintMappingOutput = obj\Release\proguard\mapping.txt

Das folgende Beispiel veranschaulicht einen typischen ProGuard-Befehl, der von der IDE ausgeführt wird:

```cmd
C:\Program Files (x86)\Java\jdk1.8.0_92\\bin\java.exe -jar C:\Android\android-sdk\tools\proguard\lib\proguard.jar -include obj\Release\proguard\proguard_xamarin.cfg -include obj\Release\proguard\proguard_project_references.cfg -include obj\Release\proguard\proguard_project_primary.cfg "-injars 'obj\Release\proguard\__proguard_input__.jar';'C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar'" "-libraryjars 'C:\Android\android-sdk\platforms\android-25\android.jar'" -outjars "obj\Release\proguard\__proguard_output__.jar" -optimizations !code/allocation/variable
```

## <a name="troubleshooting"></a>Problembehandlung

### <a name="file-issues"></a>Dateiprobleme

Die folgende Fehlermeldung kann angezeigt werden, wenn ProGuard die Konfigurationsdatei liest: 

    Unknown option '-keep' in line 1 of file 'proguard.cfg'

Dieses Problem tritt in der Regel unter Windows auf, da die `.cfg`-Datei über die falsche Codierung verfügt. ProGuard kann die _Bytereihenfolge-Marke_ (BOM) nicht verarbeiten, die möglicherweise in Textdateien vorhanden ist. Wenn eine BOM vorhanden ist, wird ProGuard mit dem oben angegebenen Fehler beendet. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um dieses Problem zu verhindern, bearbeiten Sie die benutzerdefinierte Konfigurationsdatei aus einem Text-Editor, der Ihnen das Speichern der Datei ohne die BOM ermöglicht. Um dieses Problem zu beheben, stellen Sie sicher, dass Ihr Text-Editor die Codierung auf `UTF-8` festgelegt hat. Der Text-Editor [Notepad++](https://notepad-plus-plus.org/) kann beispielsweise Dateien ohne BOM speichern, indem Sie **Codierung &gt; In UTF-8 ohne BOM codieren** beim Speichern der Datei auswählen. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um dieses Problem zu verhindern, speichern Sie die benutzerdefinierte Konfigurationsdatei aus einem Text-Editor, der Ihnen ermöglicht, die BOM wegzulassen. 

-----


### <a name="other-issues"></a>Andere Probleme

Die ProGuard-Seite [Problembehandlung](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html) beschreibt bei der Verwendung von ProGuard häufig auftretende Probleme (und Lösungen).


## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie ProGuard in Xamarin.Android funktioniert, wie Sie das Tool in Ihrem Anwendungsprojekt aktivieren und konfigurieren. Er stellt ProGuard-Beispielkonfigurationen bereit, und beschreibt Lösungen für häufig auftretende Probleme. Weitere Informationen zum ProGuard-Tool und Android finden Sie unter [Verkleinern des Codes und der Ressourcen](https://developer.android.com/tools/help/proguard.html). 


## <a name="related-links"></a>Verwandte Links

- [Preparing an Application for Release (Vorbereiten einer Anwendung auf die Veröffentlichung)](~/android/deploy-test/release-prep/index.md)
