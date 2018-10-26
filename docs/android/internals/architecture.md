---
title: Architektur
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 219c6bb4cd5718c969ba83a55596ad7b0bab8baf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121125"
---
# <a name="architecture"></a>Architektur

Xamarin.Android-Anwendungen, die in der Mono-ausführungsumgebung ausgeführt werden.
Diese Ausführung-Umgebung ausgeführt wird Seite-an-Seite mit dem Android-Laufzeit (ART) virtuellen Computer. Beide Laufzeitumgebungen werden auf Linux-Kernels ausgeführt und verfügbar machen verschiedene APIs für die der Code des Benutzer, der Entwicklern den Zugriff auf die zugrunde liegende System ermöglicht. Die Mono-Laufzeit wird in der Programmiersprache C geschrieben.

Sie können verwenden werden die [System](xref:System), [System.IO](xref:System.IO), [System.Net](xref:System.Net) und der Rest des .NET Klassenbibliotheken auf die zugrunde liegenden Funktionen des Linux-Betriebssystem.

Unter Android werden die meisten der System-Funktionen wie Audio, Grafiken, OpenGL und Telefonie sind nicht verfügbar ist, direkt in systemeigenen Anwendungen, sie sind nur verfügbar, über die Android-Runtime-Java-APIs befinden, die in einem der der [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * Namespaces oder [Android](https://developer.xamarin.com/api/namespace/Android/). * Namespaces. Die Architektur sieht ungefähr folgendermaßen aus:

[![Diagramm von Mono und Grafiken, über den Kernel und unter .NET/Java und Bindungen](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android-Entwickler Zugriff auf die verschiedenen Funktionen im Betriebssystem durch Aufrufen von .NET APIs, die sie kennen (für den Zugriff auf niedriger Ebene) oder mithilfe der Klassen, die verfügbar gemacht werden, in der Android-Namespaces, die Brücke auf die Java-APIs bereitstellt, die von verfügbar gemacht werden die Android-Laufzeit.

Weitere Informationen zu den Android-Klassen wie mit der Android-Runtime-Klassen zu kommunizieren finden Sie unter den [API-Design](~/android/internals/api-design.md) Dokument.


## <a name="application-packages"></a>Anwendungspakete

Android-Anwendungspakete sind ZIP-Container mit einem *.apk* Dateierweiterung. Xamarin.Android-Anwendungspakete haben die gleiche Struktur und das Layout als normale Android-Pakete mit den folgenden Ergänzungen:

-   Werden die Assemblys der Anwendung (mit IL) *gespeicherten* unkomprimierten innerhalb der *Assemblys* Ordner. Während der Prozess erstellt beim Start in der Version der *.apk* ist *mmap()* Ed in den Prozess und die Assemblys aus dem Arbeitsspeicher geladen werden. Dies ermöglicht schnellere app-Starts, wie Assemblys nicht vor der Ausführung extrahiert werden müssen. - *Hinweis:* Assembly Standortinformationen wie z. B. [Assembly.Location](xref:System.Reflection.Assembly.Location) und [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *kann nicht als zuverlässig betrachtet werden, werden* in Version erstellt. Diese sind nicht als distinct Filesystem-Einträge vorhanden, und sie haben kein Speicherort verwendet werden.


-   Native Bibliotheken, die die Mono-Laufzeit enthält sind vorhanden, in der *.apk* . Eine Xamarin.Android-Anwendung muss native Bibliotheken für die gewünschten/Ziel Android-Architekturen enthalten, z. B. *Armeabi* , *Armeabi-v7a* , *X86* . Xamarin.Android-Anwendungen können nicht auf einer Plattform ausgeführt, es sei denn, die entsprechenden Common Language Runtime-Bibliotheken.


Xamarin.Android-Anwendungen auch enthalten *Android Callable Wrapper* zu Android für verwalteten Code aufzurufen.



## <a name="android-callable-wrappers"></a>Android Callable Wrapper

- **Android callable Wrapper** sind eine [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) Bridge die jederzeit verwendet werden, die Android-Laufzeit muss verwalteter Code aufgerufen. Android callable Wrapper sind, wie virtuelle Methoden überschrieben werden kann und Java-Schnittstellen implementiert werden können. Finden Sie unter den [Java-Integration (Übersicht)](~/android/platform/java-integration/index.md) Dokument Weitere Informationen.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Verwaltete callable Wrapper sind eine JNI-Bridge an verwalteter Code muss Android-Code aufrufen und bieten Unterstützung für die virtuelle Methoden überschreiben und Implementieren von Java-Schnittstellen verwendet werden. Die gesamte [Android](https://developer.xamarin.com/api/namespace/Android/). * und zugehörigen Namespaces sind verwaltete callable Wrapper generiert wurde, über [JAR-Bindung](~/android/platform/binding-java-library/index.md).
Verwaltete callable Wrapper sind verantwortlich für das Konvertieren zwischen verwalteten und Android-Typen und die zugrunde liegende Android-Plattform-Methoden über JNI aufgerufen.

Erstellt jedes verwaltete callable Wrapper enthält einen Java globalen Verweis zugegriffen werden über kann die [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) Eigenschaft. Globale Verweise werden verwendet, um die Zuordnung zwischen Java-Instanzen und verwaltete Instanzen bereitzustellen. Globale Verweise sind eine eingeschränkte Ressource: Emulatoren können nur 2000 globalen Verweise zu einem Zeitpunkt vorhanden sein, während die meisten Hardware über 52.000 globalen Verweise zu einem Zeitpunkt vorhanden sein kann.

Um nachzuverfolgen, wenn globale Verweise erstellt und zerstört werden, Sie können festlegen, die [debug.mono.log](~/android/troubleshooting/index.md) Systemeigenschaft enthalten [Gref](~/android/troubleshooting/index.md).

Globale Verweise freigegeben werden können, explizit durch Aufrufen von [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) für den verwalteten callable Wrapper. Entfernt die Zuordnung zwischen Java-Instanz und die verwaltete Instanz und die Java-Instanz gesammelt werden können. Wenn Java-Instanz erneut über verwalteten Code zugegriffen wird, wird ein neuer verwalteter callable Wrapper dafür erstellt werden.

Beim Freigeben von Aufrufwrappern verwaltet werden, ob die Instanz versehentlich kann, können Sie zwischen Threads freigegeben werden, als die Instanz freigegeben Verweise von anderen Threads beeinträchtigt wird, muss sorgfältig vorgegangen werden. Für maximale Sicherheit, nur `Dispose()` Instanzen, die über zugeordnet wurden `new` *oder* von Methoden die Sie *wissen* immer zuordnen neuer Instanzen und nicht zwischengespeicherte Instanzen, die möglicherweise Führen Sie die versehentliche Datenfreigabe zwischen Threads Instanz.



## <a name="managed-callable-wrapper-subclasses"></a>Verwaltete Aufrufwrapper Unterklassen

Verwaltete Aufrufwrapper Unterklassen sind, in dem die "interessante" anwendungsspezifische Logik gespeichert sein kann. Enthalten diese benutzerdefinierte [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) Unterklassen (z. B. die ["Activity1"](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) Typ in der Standardvorlage für das Projekt). (Insbesondere Hierbei handelt es sich *Java.Lang.Object* Unterklassen wozu *nicht* enthalten eine [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) benutzerdefinierten Attributs oder [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) ist *"false"*, dies ist die Standardeinstellung.)

Wie verwaltet callable Wrapper, verwaltete Aufrufwrapper Unterklassen enthalten auch einen globalen Verweis, der über die [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) Eigenschaft. Ebenso wie globale Verweise mit verwalteten callable Wrapper, explizit durch Aufrufen von freigegeben werden können [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Im Gegensatz zu verwalteten callable Wrapper *mit großer Sorgfalt* entnommen werden soll, vor dem Freigeben von solchen Fällen ist als *Dispose()*- Wert der Instanz unterbricht die Zuordnung zwischen Java-Instanz (eine Instanz von einer Android Callable Wrapper) und die verwaltete Instanz.


### <a name="java-activation"></a>Java-Aktivierung

Wenn ein [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (Inhaltsfehler) aus Java erstellt wird, der Inhaltsfehler Konstruktor führt dazu, dass des entsprechenden C#-Konstruktors aufgerufen werden. Z. B. der Inhaltsfehler für *MainActivity* enthält einen standardmäßigen Konstruktor die aufrufen wird *MainActivity*des Standard-Konstruktor. (Dies erfolgt mithilfe der *TypeManager.Activate()* innerhalb der Inhaltsfehler Konstruktoren aufrufen.)

Es gibt eine andere Konstruktorsignatur Auswirkungen: die *(IntPtr, JniHandleOwnership)* Konstruktor. Die *(IntPtr, JniHandleOwnership)* Konstruktor wird aufgerufen, sobald eine Java-Objekt an verwalteten Code verfügbar gemacht wird und einen verwalteten Callable Wrapper werden, um die JNI-Handle zu verwalten erstellt muss. Dies wird normalerweise automatisch vorgenommen.

Es gibt zwei Szenarien, in denen die *(IntPtr, JniHandleOwnership)* Konstruktor muss auf eine Unterklasse verwaltet Callable Wrapper manuell bereitgestellt werden:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) ist eine Unterklasse. *Anwendung* ist besondere; der Standardwert *Anwendung* Konstruktor wird *nie* aufgerufen werden, und die [(IntPtr, JniHandleOwnership) Konstruktor muss stattdessen angegeben werden ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Virtuelle Methodenaufruf von einem Konstruktor der Basisklasse.


Beachten Sie, dass (2) ist eine Abstraktion vor lückenhaften. In Java, wie in c# invoke-Aufrufe an virtuellen Methoden von einem Konstruktor immer die am stärksten abgeleitete Implementierung der Methode. Z. B. die [TextView (Kontext AttributeSet, Int) Konstruktor](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) Ruft die virtuelle Methode [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), das gebunden ist, als die [ TextView.DefaultMovementMethod Eigenschaft](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Daher, wenn ein Typ [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1), würden [Unterklasse TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView.DefaultMovementMethod überschreiben](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45), und (3) [aktivieren Sie eine Instanz dieses Klasse per XML](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) der überschriebenen *DefaultMovementMethod* Eigenschaft würde aufgerufen, bevor der Inhaltsfehler Konstruktor Gelegenheit hatten, führen Sie aus, und es kommt es, bevor Sie die C# Konstruktor Gelegenheit hatten, ausgeführt werden.

Dies wird durch das Instanziieren einer Instanz LogTextBox unterstützt über den [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) Konstruktor, wenn die Instanz Inhaltsfehler LogTextBox zuerst eintritt verwalteten Code, und klicken Sie dann Aufrufen der [ LogTextBox (Kontext IAttributeSet, Int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) Konstruktor *auf derselben Instanz* Wenn Inhaltsfehler Konstruktor ausgeführt wird.

Die Reihenfolge der Ereignisse:

1.  Laden von Layout-XML in eine [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android des Layout-Objektdiagramms instanziiert und instanziiert eine Instanz des *monodroid.apidemo.LogTextBox* , der Inhaltsfehler für *LogTextBox* .

3.  Die *monodroid.apidemo.LogTextBox* Konstruktor führt die [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) Konstruktor.

4.  Die *TextView* Konstruktor ruft *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* überprüft, ob es bereits eine entsprechende c# ist-Instanz für *behandeln* . Wenn vorhanden ist, wird es zurückgegeben. In diesem Szenario nicht vorhanden ist, also *Object.GetObject&lt;T&gt;()* erstellen müssen.

7.  *Object.GetObject&lt;T&gt;()* sucht nach dem *LogTextBox (IntPtr, JniHandleOwneship)* Konstruktor aufgerufen wird, erstellt eine Zuordnung zwischen *behandeln* und die erstellte Instanz, und gibt die erstellte Instanz zurück.

8.  *TextView.n_GetDefaultMovementMethod()* Ruft die *LogTextBox.DefaultMovementMethod* Eigenschaftengetter.

9.  Die Kontrolle wieder an die *android.widget.TextView* -Konstruktor, der Ausführung abgeschlossen ist.

10. Die *monodroid.apidemo.LogTextBox* Konstruktor ausgeführt wird, Aufrufen von *TypeManager.Activate()* .

11. Die *LogTextBox (Kontext IAttributeSet, Int)* Konstruktor führt *auf die gleiche Instanz (7)* .

12. Wenn die (IntPtr, JniHandleOwnership) Konstruktor kann nicht gefunden werden, und klicken Sie dann eine System.MissingMethodException](xref:System.MissingMethodException) ausgelöst.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Vorzeitige Dispose() aufrufen

Es gibt eine Zuordnung zwischen einer JNI-Handle und die entsprechende C#-Instanz. Java.Lang.Object.Dispose() wird diese Zuordnung. Wenn ein Handle JNI verwalteten Code eingibt, nachdem die Zuordnung unterbrochen wurde, sieht Sie Java-Aktivierung und die *(IntPtr, JniHandleOwnership)* Konstruktor überprüft und aufgerufen werden. Wenn der Konstruktor nicht vorhanden ist, wird eine Ausnahme ausgelöst werden.

Betrachten Sie z. B. die folgende verwaltete aufrufbare Wraper-Unterklasse:

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

Erstellen Sie eine Instanz, die Dispose(), und führen den verwalteten Callable Wrapper neu erstellt werden:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Das Programm wird sterben:

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

Wenn die Unterklasse enthält ein *(IntPtr, JniHandleOwnership)* Konstruktor ein *neue* Instanz des Typs erstellt werden. Daher wird die Instanz auf "alle Instanzdaten verloren gehen" angezeigt, da es sich um eine neue Instanz ist. (Beachten Sie, dass der Wert null ist.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Nur *Dispose()* von Aufrufwrapper Unterklassen verwaltet, wenn Sie wissen, dass die Java-Objekt nicht mehr verwendet werden wird oder die Unterklasse keine Instanzdaten enthält und ein *(IntPtr, JniHandleOwnership)* Konstruktor wurde angegeben.



## <a name="application-startup"></a>Anwendungsstart

Wenn eine Aktivität, Dienst wird usw. gestartet Android überprüft zuerst, um festzustellen, ob es bereits ein Prozess ausgeführt wird ist, um die Aktivität, Dienst usw. zu hosten. Wenn kein solcher Prozess vorhanden ist, und klicken Sie dann ein neuer Prozess erstellt wird, die ["androidmanifest.xml"](http://developer.android.com/guide/topics/manifest/manifest-intro.html) gelesen, und im angegebenen Typ der [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) Attribut geladen und instanziiert wird. Weiter, aber alle Typen, die gemäß der [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) Attributwerte instanziiert sind und haben ihre [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) aufgerufene Methode. Xamarin.Android-Hooks in die durch das Hinzufügen einer *mono. MonoRuntimeProvider* *ContentProvider* auf die Datei "androidmanifest.xml" während des Buildprozesses. Die *mono. MonoRuntimeProvider.attachInfo()* Methode ist verantwortlich für die Mono-Laufzeit in den Prozess geladen.
Jeder Versuch, Mono vor diesem Zeitpunkt nutzen schlägt fehl. ( *Hinweis*: Dies ist deshalb Typen die Unterklassen [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) bereitstellen müssen eine [(IntPtr, JniHandleOwnership) Konstruktor](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), wie die Anwendungsinstanz ist erstellt, bevor Mono initialisiert werden kann.)

Nach Abschluss der Initialisierung `AndroidManifest.xml` wird verwendet, finden Sie den Namen der die Aktivität, Dienst usw. zu starten. Z. B. die [ /manifest/application/activity/@android:name Attribut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) wird verwendet, um zu bestimmen, den Namen einer Aktivität geladen. Dieser Typ muss für Aktivitäten erben [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Der angegebene Typ wird geladen, über [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (erfordert, dass der Typ einer Java geben Sie daher die Android Callable Wrapper), dann instanziiert. Erstellen einer Android Callable Wrapper-Instanz löst die Erstellung einer Instanz von den entsprechenden C#-Typ. Android wird dann aufgerufen [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , wodurch das entsprechende [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) aufgerufen werden soll, und du anfangen Races.
