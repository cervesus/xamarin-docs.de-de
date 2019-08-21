---
title: Architektur
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 2b8e524d95fb60c8eb45b3dd5b64b68469d97ad1
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510730"
---
# <a name="architecture"></a>Architektur

Xamarin. Android-Anwendungen werden innerhalb der Mono-Ausführungsumgebung ausgeführt.
Diese Ausführungsumgebung wird parallel zum virtuellen Computer der Android-Laufzeit (Art) ausgeführt. Beide Laufzeitumgebungen werden oberhalb des Linux-Kernels ausgeführt und stellen dem Benutzercode verschiedene APIs zur Verfügung, mit denen Entwickler auf das zugrunde liegende System zugreifen können. Die Mono-Laufzeit wird in der Programmiersprache C geschrieben.

Sie können das [System](xref:System), [System.IO](xref:System.IO), [System.net](xref:System.Net) und den Rest der .NET-Klassenbibliotheken verwenden, um auf die zugrunde liegenden Linux-Betriebs System Funktionen zuzugreifen.

Unter Android sind die meisten Systemfunktionen wie Audio, Grafiken, OpenGL und Telefonie nicht direkt für native Anwendungen verfügbar. Sie werden nur über die Android-Runtime-Java-APIs verfügbar gemacht, die sich in einem der [Java](xref:Java.Lang).*-Namespaces oder in [Android](xref:Android).*-Namespaces befinden. Die Architektur sieht ungefähr wie folgt aus:

[![Diagramm von Mono und Kunst oberhalb des Kernels und unter .net/java +-Bindungen](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin. Android-Entwickler greifen auf die verschiedenen Features im Betriebssystem zu, indem Sie entweder .NET-APIs aufrufen, die Sie kennen (für den Zugriff auf niedriger Ebene) oder die in den Android-Namespaces verfügbar gemachten Klassen verwenden, die eine Brücke zu den Java-APIs bereitstellen, die von verfügbar gemacht werden. die Android-Laufzeit.

Weitere Informationen zur Kommunikation zwischen den Android-Klassen und den Android-Lauf Zeit Klassen finden Sie im [API-Entwurfs](~/android/internals/api-design.md) Dokument.


## <a name="application-packages"></a>Anwendungspakete

Android-Anwendungspakete sind ZIP-Container mit der Dateierweiterung *. apk* . Xamarin. Android-Anwendungspakete verfügen über die gleiche Struktur und das gleiche Layout wie normale Android-Pakete mit den folgenden Ergänzungen:

-   Die Anwendungsassemblys (mit IL) werden im Assemblyordner unkomprimiert *gespeichert* . Beim Prozessstart in Releasebuilds *wird die* *APK* -Datei in den Prozess integriert, und die Assemblys werden aus dem Arbeitsspeicher geladen. Dies ermöglicht einen schnelleren App-Start, da Assemblys nicht vor der Ausführung extrahiert werden müssen.  
-   *Hinweis*: Informationen zum Assemblyspeicherort, z [. b. Assembly. Location](xref:System.Reflection.Assembly.Location) und [Assembly. CodeBase](xref:System.Reflection.Assembly.CodeBase)
    , können in Releasebuilds*nicht darauf basieren* Sie sind nicht als eindeutige Dateisystem Einträge vorhanden, und Sie haben keinen verwendbaren Speicherort.


-   Native Bibliotheken, die die Mono-Laufzeit enthalten, sind in der *APK* -Datei vorhanden. Eine xamarin. Android-Anwendung muss systemeigene Bibliotheken für die gewünschten/Ziel-Android-Architekturen enthalten, z. b. *ARMEABI* , *ARMEABI-v7a* , *x86* . Xamarin. Android-Anwendungen können nur auf einer Plattform ausgeführt werden, wenn Sie die entsprechenden Laufzeitbibliotheken enthält.


Xamarin. Android-Anwendungen enthalten auch *Android Callable Wrapper* , um Android das Aufrufen von verwaltetem Code zu ermöglichen.



## <a name="android-callable-wrappers"></a>Android Callable Wrapper

- Bei **Android Callable Wrapper** handelt es sich um eine [jni](https://en.wikipedia.org/wiki/Java_Native_Interface) -Bridge, die immer dann verwendet wird, wenn die Android-Laufzeit verwalteten Code aufrufen muss. Bei Android Callable Wrapper können virtuelle Methoden überschrieben werden, und Java-Schnittstellen können implementiert werden. Weitere Informationen finden Sie in der [Übersicht über die Java-Integration](~/android/platform/java-integration/index.md) .


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Verwaltete Callable Wrapper

Verwaltete Callable Wrapper sind eine jni-Bridge, die immer dann verwendet wird, wenn verwalteter Code Android-Code aufrufen muss und Unterstützung für das Überschreiben von virtuellen Methoden und die Implementierung von Java-Schnittstellen bietet Die gesamten [Android](xref:Android). *-und zugehörigen Namespaces sind verwaltete Aufruf Bare Wrapper, die über die [jar-Bindung](~/android/platform/binding-java-library/index.md)generiert werden.
Verwaltete Callable Wrapper sind für die Typumwandlung von verwalteten und Android-Typen und das Aufrufen der zugrunde liegenden Android-Platt Form Methoden über jni verantwortlich.

Jeder erstellte verwaltete Aufruf Bare Wrapper enthält einen globalen Java-Verweis, der über die Eigenschaft " [Android. Runtime. ijavaobject. handle](xref:Android.Runtime.IJavaObject.Handle) " zugänglich ist. Globale Verweise werden verwendet, um die Zuordnung zwischen Java-Instanzen und verwalteten Instanzen bereitzustellen. Globale Verweise sind eine begrenzte Ressource: Emulatoren ermöglichen es, dass nur 2000 globale Verweise gleichzeitig vorhanden sind, während die meisten Hardware mehr als 52.000 globale Verweise gleichzeitig zulässt.

Wenn Sie nachverfolgen möchten, wann globale Verweise erstellt und zerstört werden, können Sie die System Eigenschaft [Debug. Mono. log](~/android/troubleshooting/index.md) auf [Gref](~/android/troubleshooting/index.md)festlegen.

Globale Verweise können explizit freigegeben werden, indem " [java. lang. Object. verwerfen ()](xref:Java.Lang.Object.Dispose) " für den verwalteten Callable Wrapper aufgerufen wird. Dadurch wird die Zuordnung zwischen der Java-Instanz und der verwalteten Instanz entfernt, und die Java-Instanz kann gesammelt werden. Wenn der Zugriff auf die Java-Instanz von verwaltetem Code aus wieder hergestellt wird, wird ein neuer verwalteter Aufruf barer Wrapper dafür erstellt.

Beim Verwerfen von verwalteten Aufruf baren Wrappern muss sorgfältig vorgegangen werden, wenn die Instanz versehentlich zwischen Threads freigegeben werden kann, da die verwerfen der Instanz sich auf Verweise von anderen Threads auswirkt. Für maximale Sicherheit nur `Dispose()` für Instanzen, die über `new` oder von Methoden zugeordnet wurden, denen Sie *wissen* , dass Sie immer neue Instanzen zuordnen, nicht zwischengespeicherte Instanzen, die eine versehentliche instanzfreigabe verursachen können. Threads.



## <a name="managed-callable-wrapper-subclasses"></a>Verwaltete Aufruf Bare Wrapper-Unterklassen

Verwaltete Aufruf Bare Wrapper-Unterklassen sind, wo die "interessante" anwendungsspezifische Logik möglicherweise Live ist. Hierzu gehören benutzerdefinierte [Android. app. Activity](xref:Android.App.Activity) -Unterklassen (z. b. der [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) -Typ in der Standard Projektvorlage). (Insbesondere alle *java. lang. Object* -Unterklassen, die kein Benutzer definiertes [Register Attribute](xref:Android.Runtime.RegisterAttribute) -Attribut oder [RegisterAttribute enthalten. donotgenerateacw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) ist *false*. Dies ist die Standardeinstellung.)

Wie verwaltete Callable Wrapper enthalten auch verwaltete Aufruf Bare Wrapper-Unterklassen einen globalen Verweis, auf den über die [java. lang. Object. handle](xref:Java.Lang.Object.Handle) -Eigenschaft zugegriffen werden kann. Ebenso wie bei verwalteten Callable Wrappern können globale Verweise explizit freigegeben werden, indem [java. lang. Object. verwerfen ()](xref:Java.Lang.Object.Dispose)aufgerufen wird.
Anders als verwaltete Callable Wrapper sollten Sie vor dem Freigeben solcher Instanzen *sehr sorgfältig* Vorgehen *, da die*Zuordnung der Instanz die Zuordnung zwischen der Java-Instanz (einer Instanz eines Android Callable Wrapper) und der verwalteten lichen.


### <a name="java-activation"></a>Java-Aktivierung

Wenn ein [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) aus Java erstellt wird, bewirkt der ACW-Konstruktor, dass C# der entsprechende Konstruktor aufgerufen wird. Der ACW für *mainactivity* enthält z. b. einen Standardkonstruktor, der den Standardkonstruktor von *mainactivity*aufruft. (Dies erfolgt über den *typemanager. Aktivierung ()* -Befehl innerhalb der ACW-Konstruktoren.)

Es gibt eine andere Konstruktorsignatur der Folge: den Konstruktor *(IntPtr, jnihandownership)* . Der *(IntPtr, jnishandownership)* -Konstruktor wird immer dann aufgerufen, wenn ein Java-Objekt für verwalteten Code verfügbar gemacht wird und ein verwalteter Aufruf barer Wrapper erstellt werden muss, um das jni-Handle zu verwalten. Dies erfolgt in der Regel automatisch.

Es gibt zwei Szenarien, in denen der Konstruktor *(IntPtr, jnigsownership)* manuell für eine verwaltete Aufruf Bare Wrapper-Unterklasse bereitgestellt werden muss:

1. [Android. app. Application](xref:Android.App.Application) wird unter klassifiziert. *Anwendung* ist speziell. der standardanwenderkonstruktor wird *nie* aufgerufen, und [stattdessen muss der Konstruktor (IntPtr, jnitorownership) bereitgestellt werden](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Der Aufruf der virtuellen Methode von einem Basisklassenkonstruktor.

Beachten Sie, dass (2) eine undichte Abstraktion ist. In Java C#rufen Aufrufe von virtuellen Methoden aus einem Konstruktor immer die am weitesten abgeleitete Methoden Implementierung auf. Beispielsweise ruft der [TextView-Konstruktor (context, AttributeSet, int)](xref:Android.Widget.TextView#ctor*) die virtuelle Methode " [TextView. getdefaultmovementmethod ()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod())" auf, die als [TextView. defaultmovementmethod-Eigenschaft](xref:Android.Widget.TextView.DefaultMovementMethod)gebunden ist.
Wenn also eine [typlogtextbox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1) [Unterklasse TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView. defaultmovementmethod überschreiben](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45), und (3) [eine Instanz dieser Klasse über XML aktivieren](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) , würde die überschriebene *defaultmovementmethod* -Eigenschaft wird aufgerufen, bevor der ACW-Konstruktor ausgeführt werden konnte, und es würde eintreten, bevor C# der Konstruktor eine Ausführungs Chance hätte.

Dies wird durch das Instanziieren eines instanzlogtextfelds über den [logtextview-Konstruktor (IntPtr, jnitorownership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) unterstützt, wenn die ACW logtextbox-Instanz zuerst verwalteten Code eingibt, und dann das [logtextbox-Element (context, iattributeset, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) -Konstruktor *für die gleiche Instanz* , wenn der ACW-Konstruktor ausgeführt wird.

Reihenfolge der Ereignisse:

1.  Layout-XML wird in eine [contentview](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41)geladen.

2.  Android instanziiert das layoutobjektdiagramm und instanziiert eine Instanz von *monodroid. apidemo. logtextbox* , dem ACW für *logtextbox* .

3.  Der *monodroid. apidemo. logtextbox* -Konstruktor führt den [Android. Widget. TextView](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) -Konstruktor aus.

4.  Der *TextView* -Konstruktor ruft *monodroid. apidemo. logtextbox. getdefaultmovementmethod ()* auf.

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](xref:Java.Lang.Object.GetObject*) .

6.  *Java. lang. Object. GetObject&lt;TextView&gt;()* prüft, ob bereits eine entsprechende C# Instanz für *handle* vorhanden ist. Wenn dies der Fall ist, wird der Wert zurückgegeben. In diesem Szenario gibt es nicht, daher muss *Object.&lt;GetObject&gt;t ()* eine erstellen.

7.  *Object. GetObject&lt;T&gt;()* sucht nach dem Konstruktor *logtextbox (IntPtr, jnishandowneship)* , ruft ihn auf, erstellt eine Zuordnung zwischen *handle* und der erstellten Instanz und gibt die erstellte Instanz zurück.

8.  *TextView. n_GetDefaultMovementMethod ()* Ruft die *logtextbox. defaultmovementmethod* -Eigenschaft Getter auf.

9.  Die Steuerung wird an den *Android. Widget. TextView* -Konstruktor zurückgegeben, der die Ausführung beendet.

10. Der *monodroid. apidemo. logtextbox* -Konstruktor wird ausgeführt, wobei *typemanager. Aktivierung ()* aufgerufen wird.

11. Der *logtextbox-Konstruktor (context, iattributeset, int)* wird *für dieselbe Instanz ausgeführt, die in (7) erstellt wurde* .

12. Wenn der Konstruktor (IntPtr, jnitorownership) nicht gefunden werden kann, wird eine System. MissingMethodException] (Xref: System. MissingMethodException) ausgelöst.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Vorzeitige verwerfen ()-Aufrufe

Es gibt eine Zuordnung zwischen einem jni-Handle und der C# entsprechenden-Instanz. Java. lang. Object. verwerfen () bricht diese Zuordnung. Wenn ein jni-handle verwalteten Code eingibt, nachdem die Zuordnung beschädigt wurde, sieht er wie die Java-Aktivierung aus, und der-Konstruktor *(IntPtr, jnishandownership)* wird überprüft und aufgerufen. Wenn der Konstruktor nicht vorhanden ist, wird eine Ausnahme ausgelöst.

Angenommen, Sie haben die folgende verwaltete Aufruf Bare wraper-Unterklasse:

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

Wenn eine Instanz erstellt wird, löschen Sie Sie (), und bewirken Sie, dass der verwaltete Aufruf Bare Wrapper neu erstellt wird:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Das Programm wird angezeigt:

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

Wenn die Unterklasse einen-Konstruktor *(IntPtr, jnilenker Ownership)* enthält, wird eine *neue* Instanz des Typs erstellt. Folglich wird die Instanz so angezeigt, dass alle Instanzdaten "verloren gehen", da es sich um eine neue Instanz handelt. (Beachten Sie, dass der Wert NULL ist.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Verwerfen *()* von verwalteten Aufruf baren Wrapper-Unterklassen, wenn Sie wissen, dass das Java-Objekt nicht mehr verwendet wird, oder wenn die Unterklasse keine Instanzdaten enthält und ein-Konstruktor *(IntPtr, jnitorownership)* bereitgestellt wurde.



## <a name="application-startup"></a>Anwendungsstart

Wenn eine Aktivität, ein Dienst usw. gestartet wird, prüft Android zunächst, ob bereits ein Prozess zum Hosten von Aktivität/Dienst/usw. ausgeführt wird. Wenn kein solcher Prozess vorhanden ist, wird ein neuer Prozess erstellt, " [androidmanifest. XML](https://developer.android.com/guide/topics/manifest/manifest-intro.html) " gelesen, und der im [/manifest/application/@android:name](https://developer.android.com/guide/topics/manifest/application-element.html#nm) -Attribut angegebene Typ wird geladen und instanziiert. Anschließend werden alle durch die [/manifest/application/provider/@android:name](https://developer.android.com/guide/topics/manifest/provider-element.html#nm) Attributwerte angegebenen Typen instanziiert, und die [Contentprovider. attachinfo% 28](xref:Android.Content.ContentProvider.AttachInfo*) -Methode wird aufgerufen. Xamarin. Android verknüpft dies durch Hinzufügen eines *Mono. Der monoruntimeprovider* - *Contentprovider* ist während des Buildprozesses "androidmanifest. xml". Der *Mono. Die monoruntimeprovider. attachinfo ()* -Methode ist dafür verantwortlich, die Mono-Laufzeit in den Prozess zu laden.
Alle Versuche, Mono vor diesem Punkt zu verwenden, schlagen fehl. ( *Hinweis*: Dies ist der Grund, warum die Unterklasse [Android. app. Application](xref:Android.App.Application) einen- [Konstruktor (IntPtr, jnilenker Ownership)](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103)bereitstellen muss, da die Anwendungs Instanz erstellt wird, bevor Mono initialisiert werden kann.)

Nachdem die Prozess Initialisierung abgeschlossen wurde `AndroidManifest.xml` , wird die untersucht, um den Klassennamen der zu startenden Aktivität/des Diensts bzw. des Diensts zu ermitteln. Beispielsweise wird das [ /manifest/application/activity/@android:name -Attribut](https://developer.android.com/guide/topics/manifest/activity-element.html#nm) verwendet, um den Namen einer Aktivität zu ermitteln, die geladen werden soll. Für Aktivitäten muss dieser Typ " [Android. app. Activity](xref:Android.App.Activity)" erben.
Der angegebene Typ wird über " [Class. forName ()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) " geladen (was erfordert, dass der Typ ein Java-Typ ist, daher die für Android Callable Wrapper) und dann instanziiert werden. Durch die Erstellung einer Android Callable Wrapper-Instanz wird die Erstellung einer Instanz des entsprechenden C# Typs auslöst. Android ruft dann [Activity. OnCreate (Bundle)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) auf, was bewirkt, dass die entsprechende [Aktivität. OnCreate (Bundle)](xref:Android.App.Activity.OnCreate*) aufgerufen wird, und Sie sind zu den Races.
