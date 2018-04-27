---
title: Architektur
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 9ce1d790f5dea00ac47d5639ae8424793006445a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="architecture"></a>Architektur

Xamarin.Android-Anwendungen werden innerhalb der Mono-ausführungsumgebung ausgeführt.
Dieser Ausführung Umgebung ausgeführt wird Seite-an-Seite mit dem Android-Laufzeit (Grafik) virtuellen Computer. Beide Laufzeitumgebungen führen Sie zusätzlich zu den Linux-Kernel und verschiedenen APIs, die Entwicklern ermöglicht, Zugriff auf das zugrunde liegende System Benutzercode verfügbar machen. Mono-/ Runtime ist in der Programmiersprache C geschrieben.

Sie können unter Verwendung der [System](http://msdn.microsoft.com/en-us/library/system.aspx), [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx), [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx) und der Rest des .NET Klassenbibliotheken auf die zugrunde liegenden Linux-Betriebssystem-Funktionen zuzugreifen.

Auf Android-Geräten die meisten der System-Einrichtungen wie Audio, Grafiken und Telefonie OpenGL stehen nicht direkt in systemeigenen Anwendungen, sie sind nur verfügbar, über die Android-Laufzeit Java-APIs befinden, die in einem der der [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * Namespaces oder der [Android](https://developer.xamarin.com/api/namespace/Android/). * Namespaces. Die Architektur ist ungefähr wie folgt:

[![Diagramm der Mono und Grafiken über den Kernel und unter .NET/Java + Bindungen](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android Entwickler Zugriff auf die verschiedenen Funktionen im Betriebssystem durch Aufrufe in .NET APIs, die sie kennen (für den Zugriff auf niedriger Ebene) oder mithilfe der Klassen, die verfügbar gemacht werden, in der Android-Namespaces die Brücke an die Java-APIs bereitstellt, die von verfügbar gemacht werden die Android-Laufzeit.

Weitere Informationen wie die Android-Klassen mit dem Android-Runtime-Klassen kommunizieren finden Sie unter der [API-Entwurf](~/android/internals/api-design.md) Dokument.


## <a name="application-packages"></a>Anwendungspakete

Android-Anwendungspakete sind ZIP-Container mit einem *.apk* Dateierweiterung. Anwendungspakete Xamarin.Android haben die gleiche Struktur und das Layout als normale Android Pakete mit der folgenden Erweiterungen:

-   Werden die Assemblys der Anwendung (mit IL) *gespeicherten* unkomprimierten innerhalb der *Assemblys* Ordner. Während der Prozess starten in Version erstellt die *.apk* ist *mmap()* Ed in den Prozess und die Assemblys aus dem Arbeitsspeicher geladen werden. Dies ermöglicht die schnellere app-Starts, wie Assemblys nicht vor der Ausführung extrahiert werden müssen. - *Hinweis:* Speicherort Assemblyinformationen wie z. B. [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/) und [Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *nicht zuverlässig* in Version erstellt. Diese sind nicht als distinct Filesystem-Einträge vorhanden und keine verwendbaren Speicherort haben.


-   Systemeigene Bibliotheken, die mit der Mono / Runtime sind vorhanden, in der *.apk* . Eine Anwendung Xamarin.Android muss systemeigene Bibliotheken für die gewünschte/Ziel Android Architekturen enthalten, z. B. *Armeabi* , *Armeabi v7a* , *X86* . Xamarin.Android Anwendungen können nicht auf einer Plattform ausgeführt, es sei denn, der entsprechenden Common Language Runtime-Bibliotheken.


Xamarin.Android Anwendungen auch enthalten *Android Aufrufwrappern* Android verwalteten Code aufrufen können.



## <a name="android-callable-wrappers"></a>Android Callable Wrapper

- **Android Aufrufwrappern** sind eine [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) Bridge die jederzeit verwendet werden, die Android-Laufzeit benötigt, um verwalteten Code aufrufen. Android callable Wrapper sind wie virtuelle Methoden überschrieben werden kann und Java-Schnittstellen implementiert werden können. Finden Sie unter der [Übersicht über die Integration von Java](~/android/platform/java-integration/index.md) Doc länger.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Verwaltete callable Wrapper sind einer JNI-Bridge verwalteter Code muss Android Code aufrufen und bieten Unterstützung für die virtuelle Methoden überschreiben und Implementieren von Java-Schnittstellen verwendet werden. Die gesamte [Android](https://developer.xamarin.com/api/namespace/Android/). * und die zugehörigen Namespaces sind verwaltete callable Wrapper generiert wurde, über [JAR Bindung](~/android/platform/binding-java-library/index.md).
Verwaltete callable Wrapper sind verantwortlich für das Konvertieren zwischen verwalteten und Android-Typen und die zugrunde liegenden Methoden für Android-Plattform über JNI aufrufen.

Jede verwaltete Aufrufwrapper enthält erstellt, die einen globalen Java-Verweis, der über den möglich ist die [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) Eigenschaft. Globale Verweise werden verwendet, um die Zuordnung zwischen den Java-Instanzen und verwaltete Instanzen bereitstellen. Globale Verweise sind nur eine eingeschränkte Ressource: Emulatoren können nur 2000 globale Verweise zu einem Zeitpunkt vorhanden sein, wenn Sie die meisten Hardware über 52.000 globalen Verweise zu einem Zeitpunkt vorhanden sein kann.

Um nachzuverfolgen, wenn globale Verweise erstellt und zerstört werden, legen Sie die [debug.mono.log](~/android/troubleshooting/index.md) Systemeigenschaft enthalten [Gref](~/android/troubleshooting/index.md).

Globale Verweise können explizit freigegeben werden, indem Aufrufen [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) auf den verwalteten callable Wrapper. Hiermit entfernen Sie die Zuordnung zwischen den Java-Instanz und die verwaltete Instanz und die Java-Instanz gesammelt werden können. Wenn die Java-Instanz neu aus verwaltetem Code zugegriffen wird, wird eine neue verwaltete Aufrufwrapper dafür erstellt werden.

Beim Freigeben von Aufrufwrappern verwaltet, wenn die Instanz versehentlich kann, können Sie zwischen Threads freigegeben werden, als die Instanz freigeben Verweise von anderen Threads auswirkt, muss sorgfältig ausgenutzt werden. Maximale aus Sicherheitsgründen nur `Dispose()` Instanzen, die über zugewiesen wurden `new` *oder* von Methoden die *wissen* immer neue Instanzen und nicht zwischengespeicherten-Instanzen, die möglicherweise zuordnen Führen Sie die versehentliche Instanz freigeben zwischen Threads.



## <a name="managed-callable-wrapper-subclasses"></a>Verwaltet von Unterklassen Callable Wrapper

Verwaltete Aufrufwrapper Unterklassen sind, in dem die "interessante" anwendungsspezifische Logik Leben kann. Dazu zählen benutzerdefinierte [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) Unterklassen (z. B. die ["Activity1"](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) Typ in der Standard-Projektvorlage). (Insbesondere Hierbei handelt es sich *Java.Lang.Object* Unterklassen was *nicht* enthalten eine [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) benutzerdefinierten Attributs oder [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) ist *"false"*, dies ist die Standardeinstellung.)

Wie verwaltet callable Wrapper, verwaltet Aufrufwrapper Unterklassen enthalten auch einen globalen Verweis, über die [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) Eigenschaft. Ebenso wie globale Verweise mit verwalteten callable Wrapper, explizit durch Aufrufen freigegeben werden können [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Im Gegensatz zum verwalteten callable Wrapper *datenverschiebungsmechanismus* sollten erstellt werden, bevor das Freigeben von solchen Instanzen als *Dispose()*-Verknüpfung der Instanz wird die Zuordnung zwischen den Java-Instanz unterbrochen (einer Instanz von einer Android Callable Wrapper) und die verwaltete Instanz.


### <a name="java-activation"></a>Java-Aktivierung

Wenn ein [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (Inhaltsfehler) aus Java erstellt wird, der Inhaltsfehler-Konstruktor führt dazu, dass des entsprechenden C#-Konstruktors aufgerufen werden. Z. B. der Inhaltsfehler für *Verwendung des Layoutnamens* enthält einen Standardkonstruktor, die aufrufen, wird *Verwendung des Layoutnamens*des Standardkonstruktors. (Dies erfolgt über die *TypeManager.Activate()* innerhalb der Inhaltsfehler Konstruktoren aufrufen.)

Es wird eine andere Konstruktorsignatur folgen,: die *(IntPtr, JniHandleOwnership)* Konstruktor. Die *(IntPtr, JniHandleOwnership)* Konstruktor wird aufgerufen, sobald eine Java-Objekt an verwalteten Code verfügbar gemacht wird und eine verwaltete Callable Wrapper erstellt werden, um das Handle JNI verwalten muss. Dies ist in der Regel automatisch konfiguriert.

Es gibt zwei Szenarien, in denen die *(IntPtr, JniHandleOwnership)* Konstruktor muss manuell auf eine Unterklasse Callable Wrapper verwaltet angegeben werden:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) ist eine Unterklasse. *Anwendung* ist besondere; der Standardwert *Fehlerrate* Konstruktor wird *nie* aufgerufen werden, und die [(IntPtr, JniHandleOwnership) Konstruktor muss stattdessen angegeben werden ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Virtuelle Methodenaufruf von einem Konstruktor der Basisklasse.


Beachten Sie, dass (2) ist eine Abstraktion leaky. In Java, wie in c# rufen Sie Aufrufe von virtuellen Methoden in einem Konstruktor immer die am stärksten abgeleitete Implementierung der Methode. Z. B. die [TextView (Kontext AttributeSet, Int)-Konstruktor](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) Ruft die virtuelle Methode [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), das gebunden ist, als die [ TextView.DefaultMovementMethod Eigenschaft](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Daher, wenn ein Typ [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1) wurden [Unterklasse TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView.DefaultMovementMethod überschreiben](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45), und (3) [aktivieren Sie eine Instanz dieses die Klasse über XML](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) der überschriebenen *DefaultMovementMethod* Eigenschaft würde aufgerufen, bevor der Inhaltsfehler Konstruktor Gelegenheit hatten, führen Sie aus, und es kommt es, bevor der C#-Konstruktor ausgeführt werden konnten.

Dies wird durch Instanziieren einer Instanz LogTextBox unterstützt über die [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) Konstruktor zuerst gelangt die Inhaltsfehler LogTextBox Instanz verwalteten Code, und klicken Sie dann das Ergebnis der [ LogTextBox (Kontext IAttributeSet, Int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) Konstruktor *in der gleichen Instanz* wann der Inhaltsfehler Konstruktor ausgeführt wird.

Die Reihenfolge der Ereignisse:

1.  Layout-XML laden in ein [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android Objektdiagramm Layout instanziiert und instanziiert eine Instanz des *monodroid.apidemo.LogTextBox* , der Inhaltsfehler für *LogTextBox* .

3.  Die *monodroid.apidemo.LogTextBox* Konstruktor führt die [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) Konstruktor.

4.  Die *TextView* Konstruktor ruft *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* prüft, ob es bereits eine entsprechende C# ist-Instanz, für *behandeln* . Wenn Sie vorhanden ist, wird es zurückgegeben. In diesem Szenario nicht vorhanden ist, sodass *Object.GetObject&lt;T&gt;()* müssen eines erstellen.

7.  *Object.GetObject&lt;T&gt;()* sucht nach der *LogTextBox (IntPtr, JniHandleOwneship)* Konstruktor wird aufgerufen, erstellt eine Zuordnung zwischen *behandeln* und die erstellte Instanz und die erstellte Instanz zurückgibt.

8.  *TextView.n_GetDefaultMovementMethod()* Ruft die *LogTextBox.DefaultMovementMethod* Getter für eine Eigenschaft.

9.  Steuerung wird an die *android.widget.TextView* -Konstruktor, der Ausführung beendet wird.

10. Die *monodroid.apidemo.LogTextBox* Konstruktor ausgeführt wird, Aufrufen von *TypeManager.Activate()* .

11. Die *LogTextBox (Kontext IAttributeSet, Int)* Konstruktor führt *in der gleichen Instanz erstellt (7)* .

12. Wenn die (IntPtr, JniHandleOwnership) Konstruktor kann nicht gefunden werden, klicken Sie dann eine System.MissingMethodException] (https://developer.xamarin.com/api/type/System.MissingMethodException/) ausgelöst.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Vorzeitige Dispose() Aufrufe

Es besteht eine Zuordnung zwischen einer JNI-Handle und die entsprechenden C#-Instanz. Java.Lang.Object.Dispose() wird diese Zuordnung unterbrochen. Wenn ein Handle JNI verwalteten Code eingibt, nachdem die Zuordnung unterbrochen wurde, sieht wie Java-Aktivierung und die *(IntPtr, JniHandleOwnership)* Konstruktor gesucht und aufgerufen werden. Wenn der Konstruktor nicht vorhanden ist, wird eine Ausnahme ausgelöst.

Betrachten Sie z. B. die folgenden verwalteten aufrufbaren Wraper-Unterklasse:

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

Wenn wir erstellen Sie eine Instanz, Dispose() entstehen, und führen den verwalteten Callable Wrapper neu erstellt werden:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Das Programm wird Abbrechen:

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

Wenn die Unterklasse enthält ein *(IntPtr, JniHandleOwnership)* Konstruktor ein *neue* Instanz des Typs erstellt werden. Daher wird die Instanz "alle Instanzdaten verloren" angezeigt, da es sich um eine neue Instanz ist. (Beachten Sie, dass der Wert null ist.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Nur *Dispose()* von Aufrufwrapper Unterklassen verwaltet, wenn Sie wissen, dass die Java-Objekt nicht mehr verwendet werden wird, oder die Unterklasse keine Instanzdaten enthält und *(IntPtr, JniHandleOwnership)* Konstruktor wurde angegeben.



## <a name="application-startup"></a>Anwendungsstart

Wenn eine Aktivität, der Dienst wird usw. gestartet Android prüft zunächst um festzustellen, ob es bereits ein Prozess ausgeführt wird ist, um die Aktivität/Service/usw. zu hosten. Wenn kein solcher Prozess vorhanden ist, wird ein neuer Prozess erstellt, die [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) gelesen, und im angegebenen Typ der [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) Attribut geladen und instanziiert wird. Anschließend werden alle Typen, die gemäß der [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) Attributwerte werden instanziiert und haben ihre [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) aufgerufene Methode. Xamarin.Android Hooks in diese durch Hinzufügen einer *Mono /. MonoRuntimeProvider* *ContentProvider* auf AndroidManifest.xml während des Buildprozesses. Die *Mono /. MonoRuntimeProvider.attachInfo()* Methode ist verantwortlich für das Laden der Mono / Runtime in den Prozess.
Versuch Mono vor diesem Zeitpunkt zu verwenden, schlägt fehl. ( *Hinweis*: deswegen Typen die Unterklasse [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) bereitstellen müssen eine [(IntPtr, JniHandleOwnership) Konstruktor](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), da die Anwendungsinstanz erstellt, bevor Mono initialisiert werden kann.)

Nach Abschluss der Initialisierung `AndroidManifest.xml` wird gehört zum Ermitteln des Namens der Klasse von der Aktivität/Service/usw. zu starten. Z. B. die [ /manifest/application/activity/@android:name Attribut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) wird verwendet, um zu bestimmen, den Namen einer Aktivität geladen. Dieser Typ muss für Aktivitäten, erben [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Über der angegebene Typ geladen wird [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (die erfordert, dass der Typ eines-Java geben Sie daher die Android Callable Wrapper), dann instanziiert. Erstellung einer Android Callable Wrapper-Instanz werden die Erstellung einer Instanz des entsprechenden C#-Typs ausgelöst. Android wird dann aufgerufen [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , wodurch das entsprechende [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) aufgerufen werden soll, und Sie sind deaktiviert, das Rennen.
