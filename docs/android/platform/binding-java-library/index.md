---
title: Binden einer Java-Bibliothek
description: 'Die Android-Community hat viele Java-Bibliotheken, die Sie in Ihrer app verwenden möchten. Dieses Handbuch wird erläutert, wie Java-Bibliotheken in Ihre Xamarin.Android-Anwendung zu integrieren, indem Sie eine Bibliothek Bindungen erstellen.'
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
---

# <a name="binding-a-java-library"></a>Binden einer Java-Bibliothek

_Die Android-Community hat viele Java-Bibliotheken, die Sie in Ihrer app verwenden möchten. Dieses Handbuch wird erläutert, wie Java-Bibliotheken in Ihre Xamarin.Android-Anwendung zu integrieren, indem Sie eine Bibliothek Bindungen erstellen._

## <a name="overview"></a>Übersicht

Das Ökosystem der Drittanbieter-Bibliothek für Android ist massive. Aus diesem Grund ist es häufig sinnvoll, eine vorhandene Android-Bibliothek als So erstellen eine neue Ressourcengruppe zu verwenden. Xamarin.Android bietet zwei Möglichkeiten, diese Bibliotheken verwenden:

-   Erstellen Sie eine *Bindungsbibliothek* , die automatisch dient als Wrapper für die Bibliothek mit C# Wrapper, damit Sie, Java aufrufen können-code über C# aufrufen.

-   Verwenden der *Java Native Interface* (*JNI*) Aufrufe im Code der Java-Bibliothek direkt aufrufen. JNI ist ein Programmierframework, mit dem Sie Java-Code zum Aufrufen und von systemeigenen Anwendungen oder Bibliotheken aufgerufen werden kann.

Dieser Leitfaden erläutert die erste Option: Vorgehensweise: Erstellen einer *Bindungsbibliothek* , umschließt eine oder mehrere vorhandene Java-Bibliotheken in eine Assembly, die Sie in Ihrer Anwendung mit verknüpfen können. Weitere Informationen zur Verwendung von JNI finden Sie unter [arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implementiert Bindungen mit *Callable Wrapper verwaltet* (*MCW*). MCW ist eine JNI-Brücke, die verwendet wird, wenn verwalteter Code zum Aufrufen von Java-Code muss. Verwaltete callable Wrapper bieten auch Unterstützung für das Erstellen von Unterklassen für Java-Typen und zum Überschreiben von virtueller Methoden in Java-Typen. Ebenso, wenn Android-Laufzeit (ART) Code verwalteten Code aufrufen möchte, geschieht dies über eine andere JNI-Brücke, die als Android Callable Wrapper (Inhaltsfehler) bezeichnet. Dies [Architektur](~/android/internals/architecture.md) ist in der folgenden Abbildung dargestellt:

[![Android JNI-Bridge-Architektur](images/architecture.png)](images/architecture.png#lightbox)

Bindungsbibliothek ist eine Assembly mit verwalteten Callable Wrapper für die Java-Typen. Hier ist beispielsweise eine Java-Typen – `MyClass`, der in einer Bibliothek Bindungen umschlossen werden soll:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Nachdem wir eine Bibliothek mit Bindungen generiert die **JAR** , enthält `MyClass`, können wir instanziieren und rufen Sie Methoden dafür aus C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Um diese Bibliothek Bindungen zu erstellen, verwenden Sie die Xamarin.Android *Java Bindungsbibliothek* Vorlage. Das resultierende bindungsprojekt erstellt eine .NET Framework-Assembly mit den Klassen MCW **JAR** Dateien und Ressourcen für die Bibliothek für Android-Projekte, die in den es eingebettet. Sie können auch Bindungen Bibliotheken erstellen, für die Android-Archiv (. AAR)-Dateien und Eclipse-Bibliothek-Android-Projekte. Verweise auf die resultierende Bindings Library-DLL-Assembly, können Sie eine vorhandene Java-Bibliothek in Ihr Xamarin.Android-Projekt wiederverwenden.

Wenn Sie Typen in der Bibliothek Bindung verweisen, müssen Sie den Namespace der bindungsbibliothek verwenden. In der Regel fügen Sie eine `using` -Direktive am Anfang Ihrer C# Quelldateien, die .NET Namespace-Version von der Java-Paketname. Wenn der name des Java-Pakets für die gebundenen z. B. **JAR** lautet wie folgt:

```csharp
com.company.package
```

Würden Sie platzieren Sie die folgenden `using` Anweisung am Anfang Ihrer C# Quelldateien Zugriff auf Typen in den gebundenen **JAR** Datei:

```csharp
using Com.Company.Package;
```


Wenn Sie eine vorhandene Android-Bibliothek zu binden, ist es erforderlich, die folgenden Punkte bedenken:

* **Gibt es keine externen Abhängigkeiten für die Bibliothek?** &ndash; Alle Java-Abhängigkeiten, die erforderlich sind, von der Android-Bibliothek enthalten sein müssen, in das Xamarin.Android-Projekt als eine **ReferenceJar** oder als ein **EmbeddedReferenceJar**. Alle systemeigenen Assemblys müssen hinzugefügt werden, für das bindungsprojekt "als eine **EmbeddedNativeLibrary**.  

* **Welche Version der Android-API ist, wird das Ziel Android-Bibliothek?** &ndash; Es ist nicht möglich, die Android-API-Ebene "heruntergestuft" Stellen Sie sicher, dass die Bindung Xamarin.Android-Projekt die gleiche API (oder höher) als Ziel verwendet als die Android-Bibliothek.

* **Welche Version des JDK wurde verwendet, um die Bibliothek zu kompilieren?** &ndash; Bindungsfehler kann auftreten, wenn die Android-Bibliothek mit einer anderen Version des JDK als in der Verwendung von Xamarin.Android erstellt wurde. Wenn möglich, kompilieren Sie erneut mit der Android-Bibliothek mit derselben Version des JDK, die durch die Installation von Xamarin.Android verwendet wird.


## <a name="build-actions"></a>Buildvorgänge

Bei der Erstellung einer Bindungsbibliothek festlegen *Buildvorgänge* auf die **JAR** oder. Zusätzlich zum AAR-Dateien, die Sie in Ihrem Projekt Bindungsbibliothek integrieren &ndash; bestimmt jeder Buildaktion wie die **JAR** oder. Zusätzlich zum AAR-Datei wird in eingebettet (oder verweist) Ihre Bindungen-Bibliothek. Die folgende Liste enthält diese Aktionen zu erstellen:

* `EmbeddedJar` &ndash; Bettet die **JAR** in die resultierende Bindings Library-DLL als eingebettete Ressource. Dies ist die einfachste und die meisten häufig verwendeten Buildvorgang. Verwenden Sie diese Option aus, wenn Sie möchten die **JAR** automatisch in Bytecode kompiliert und in der Bindings-Bibliothek gepackt.

* `InputJar` &ndash; Können Sie nicht einbetten der **JAR** in die resultierende Bindungen-Bibliothek. DLL. Ihre Bindungsbibliothek. DLL weist eine Abhängigkeit dazu **JAR** zur Laufzeit. Verwenden Sie diese Option aus, wenn Sie nicht einschließen möchten die **JAR** in Ihrer Bibliothek Bindungen (z. B. für aus Gründen der Lizenzierung). Wenn Sie diese Option verwenden, Sie müssen sicherstellen, dass die Eingabe **JAR** steht auf dem Gerät, das Ihre app ausgeführt wird.

* `LibraryProjectZip` &ndash; Bettet eine. Zusätzlich zum AAR-Datei in die resultierende Bindungen-Bibliothek. DLL. Dies ähnelt der EmbeddedJar, außer dass Sie in der gebundenen Ressourcen (sowie Code) zugreifen können. Zusätzlich zum AAR-Datei. Verwenden Sie diese Option aus, wenn Sie einbetten möchten ein. Zusätzlich zum AAR in die Bindungsbibliothek.

* `ReferenceJar` &ndash; Gibt einen Verweis **JAR**: ein Verweis **JAR** ist eine **JAR** , dass eine mit Ihrem Grenze **JAR** oder. Hängt von AAR-Dateien. Dieser Verweis **JAR** dient ausschließlich zum Kompilieren Abhängigkeit erfüllen. Bei Verwendung dieser Buildaktion C# Bindungen werden nicht für den Verweis erstellt **JAR** und es wird nicht in der resultierenden Bindungen-Bibliothek eingebettet. DLL. Verwenden Sie diese Option aus, wenn Sie eine Bindungsbibliothek für den Verweis machen werden **JAR** , aber nicht noch geschehen. Diese Buildaktion ist nützlich für das Packen mehrerer **JAR**s (und/oder. AARs) in mehrere voneinander abhängige Bibliotheken von Bindungen.

* `EmbeddedReferenceJar` &ndash; Bettet einen Verweis **JAR** in die resultierende Bindungen-Bibliothek. DLL. Verwenden Sie diese Buildaktion aus, wenn Sie erstellen möchten C# Bindungen für die beiden **JAR** (oder). Zusätzlich zum AAR) und alle zugehörigen Verweis **JAR**(s) in der Bindings-Bibliothek.

* `EmbeddedNativeLibrary` &ndash; Bettet eine systemeigene **so** in die Bindung. Diese Buildaktion wird zum **so** Dateien, die erforderlich sind die **JAR** Datei gebunden wird. Möglicherweise manuell laden, müssen die **so** Bibliothek vor der Ausführung von Code aus der Java-Bibliothek. Dies wird nachfolgend beschrieben.

Diese Aktionen werden ausführlicher in den folgenden Handbüchern Build.

Darüber hinaus werden die folgenden Buildaktionen verwendet, um Hilfe Java-API-Dokumentation importieren und Konvertieren in C# XML-Dokumentation:

* `JavaDocJar` wird verwendet, um die Javadoc-Archiv-JAR-Datei für eine Java-Bibliothek zu verweisen, die auf ein Maven-Paket-Format entspricht (in der Regel `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` wird verwendet, um zu zeigen `index.html` Datei innerhalb der API-Referenzdokumentation HTML.
* `JavaSourceJar` Dient als Ergänzung zu `JavaDocJar`JavaDoc zuerst aus Quellen zu generieren und behandeln Sie die Ergebnisse als `JavaDocIndex`, Paket-Stil für eine Java-Bibliothek, die eine Maven entspricht (in der Regel `FOOBAR-sources**.jar**`).

Die API-Dokumentation sollte die standardmäßige Doclet Java8, Java7 oder Java6-SDK (sie sind alle anderen Format), oder den DroidDoc-Stil.

## <a name="including-a-native-library-in-a-binding"></a>Eine Native Bibliothek einzuschließen, in einer Bindung

Es kann erforderlich sein, enthalten eine **so** -Bibliothek in eine Xamarin.Android-Bindung-Projekt als Teil der Bindung einer Java-Bibliothek. Wenn die umschlossene Java-Code ausgeführt wird, Xamarin.Android der JNI-Aufruf und die Fehlermeldung fehl _java.lang.UnsatisfiedLinkError: Native Methode wurde nicht gefunden:_ wird in der Logcat, für die Anwendung angezeigt.

Die Lösung manuell laden, wird die **so** Bibliothek mit einem Aufruf von `Java.Lang.JavaSystem.LoadLibrary`. Zum Beispiel davon aus, dass ein Xamarin.Android-Projekt über eine freigegebene Bibliothek verfügt **libpocketsphinx_jni.so** enthalten, die im bindungsprojekt mit einer Buildaktion von **EmbeddedNativeLibrary**, der folgende Codeausschnitt (wird ausgeführt, bevor Sie die freigegebene Bibliothek verwenden) lädt die **so** Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Anpassen von Java-APIs in C&eparsl;

Der Generator für die Xamarin.Android-Bindung ändert einige Java-Ausdrücke und Mustern mit .NET Mustern entsprechen. Die folgende Liste beschreibt, wie Java zugeordnet wird C#/.NET:

-   _Setter/Abrufmethoden_ in Java werden _Eigenschaften_ in .NET.

-   _Felder_ in Java werden _Eigenschaften_ in .NET.

-   _Listener/Listenerschnittstellen_ in Java werden _Ereignisse_ in .NET. Die Parameter der Methoden in den Rückrufschnittstellen durch dargestellt werden ein `EventArgs` Unterklasse.

-   Ein _statische geschachtelte Klasse_ in Java ist eine _der geschachtelten Klasse_ in .NET.

-   Ein _inneren Klasse_ in Java ist eine _der geschachtelten Klasse_ mit einem Instanzenkonstruktor in C#.



## <a name="binding-scenarios"></a>Datenbindungsszenarien

Die folgenden Anleitungen zu Szenarien Bindung können Sie eine Java-Bibliothek (oder Bibliotheken) für die Integration in Ihre app zu binden:

-   [Bindung ein. JAR-](~/android/platform/binding-java-library/binding-a-jar.md) ist eine exemplarische Vorgehensweise zum Erstellen von Bindungen-Bibliotheken für **JAR** Dateien.

-   [Bindung ein. Zusätzlich zum AAR](~/android/platform/binding-java-library/binding-an-aar.md) ist eine exemplarische Vorgehensweise zum Erstellen von Bindungen für Bibliotheken. Zusätzlich zum AAR-Dateien. Lesen Sie diese exemplarische Vorgehensweise zum erfahren, wie Sie Android Studio-Bibliotheken binden.

-   [Binden eine Eclipse-Bibliotheksprojekts](~/android/platform/binding-java-library/binding-a-library-project.md) ist eine exemplarische Vorgehensweise zum Erstellen von Bibliotheken der Bindung von Android-Bibliotheksprojekten. Lesen Sie diese exemplarische Vorgehensweise, um Informationen zum Binden von Eclipse Android-Bibliotheksprojekten.

-   [Anpassen von Bindungen](~/android/platform/binding-java-library/customizing-bindings/index.md) wird erläutert, wie Sie manuelle Änderungen an die Bindung für den Buildfehler zu beheben und die Form vom Typ der resultierenden-API, sodass ist "C#-wie".

-   [Problembehandlung von Bindungen](~/android/platform/binding-java-library/troubleshooting-bindings.md) Listet allgemeine Fehlerszenarien für die Bindung, erläutert mögliche Ursachen und Vorschläge zur Behebung dieser Fehler bietet.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI-Metadaten](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Verwenden nativer Bibliotheken](~/android/platform/native-libraries.md)
