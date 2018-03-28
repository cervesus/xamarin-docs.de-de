---
title: Binden einer Java-Bibliothek
description: Die Android-Community hat viele Java-Bibliotheken, die Sie in Ihrer app verwenden möchten. Dieses Handbuch erläutert, wie Java-Bibliotheken in Ihrer Anwendung Xamarin.Android integriert werden, durch das Erstellen einer Bibliothek Bindungen.
ms.topic: article
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 47e32a68b7b10a2d02ee41a9abf234be6f002f7b
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="binding-a-java-library"></a>Binden einer Java-Bibliothek

_Die Android-Community hat viele Java-Bibliotheken, die Sie in Ihrer app verwenden möchten. Dieses Handbuch erläutert, wie Java-Bibliotheken in Ihrer Anwendung Xamarin.Android integriert werden, durch das Erstellen einer Bibliothek Bindungen._

## <a name="overview"></a>Übersicht

Das Ökosystem Drittanbieter-Bibliothek für Android ist massive. Aus diesem Grund sinnvoll es häufig zu eine vorhandene Android-Bibliothek als So erstellen ein neues Konto zu verwenden. Xamarin.Android bietet zwei Möglichkeiten, diese Bibliotheken verwenden:

-   Erstellen einer *Bindungen Bibliothek* , automatisch die Bibliothek mit C#-Wrapper umschließt, damit Sie über C#-Aufrufe Java-Code aufrufen können.

-   Verwenden der *Java Native Interface* (*JNI*) in Java-Bibliothekscode ruft direkt aufrufen. JNI ist ein Programmierframework, mit der Java-Code aufrufen und von systemeigenen Anwendungen oder Bibliotheken aufgerufen werden kann.

Dieses Handbuch erklärt die erste Option: Gewusst wie: Erstellen einer *Bindungen Bibliothek* , umschließt eine oder mehrere vorhandene Java-Bibliotheken in eine Assembly, die Sie mit der Anwendung verknüpfen können. Weitere Informationen zur Verwendung von JNI finden Sie unter [arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implementiert Bindungen mit *Aufrufwrappern verwaltet* (*MCW*). MCW ist einer JNI-Bridge, die verwendet wird, wenn verwalteter Code Java-Code aufrufen muss. Verwaltete Aufrufwrappern bieten auch Unterstützung für das Erstellen von Unterklassen für Java-Typen und zum Überschreiben von virtueller Methoden in Java-Typen. Ebenso, wenn Android Laufzeitcode (Grafik) verwalteten Code aufrufen möchte, tut sie über eine andere JNI-Bridge, die als Android Callable Wrapper (Inhaltsfehler) bezeichnet. Dies [Architektur](~/android/internals/architecture.md) wird in der folgenden Abbildung veranschaulicht:

[![Android JNI-Bridge-Architektur](images/architecture.png)](images/architecture.png#lightbox)

Eine Bindungen-Bibliothek ist eine Assembly, die verwaltete Callable Wrapper für Java-Typen enthält. Hier ist z. B. eine Java-Datentyp `MyClass`, die in einer Bibliothek Bindungen umbrochen werden soll:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Nachdem wir eine Bibliothek mit Bindungen generiert die **JAR** enthält `MyClass`, können wir instanziiert und Methoden dafür aus c# aufrufen:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Um diese Bibliothek Bindungen zu erstellen, verwenden Sie die Xamarin.Android *Java Bindungen Bibliothek* Vorlage. Das daraus resultierende bindungsprojekt eine .NET Framework-Assembly erstellt, mit den Klassen MCW **JAR** Dateien und Ressourcen für die Bibliothek für Android-Projekte, die in den es eingebettet. Sie können auch Bindungen Bibliotheken für Android Archiv erstellen (. AAR)-Dateien und Projekten für Android Eclipse-Bibliothek. Durch Verweisen auf die resultierende Bindungen Library-DLL-Assembly, können Sie eine vorhandene Java-Bibliothek in Ihrem Projekt Xamarin.Android wiederverwenden.

Wenn Sie in der Bibliothek Bindung Typen verweisen, müssen Sie den Namespace der Bindung Bibliothek verwenden. Normalerweise fügen Sie eine `using` Direktive am Anfang der C#-Quellcode Dateien, die .NET Namespace-Version von der Java-Paketname. Angenommen, der name des Java-Pakets für die gebundenen **JAR** lautet wie folgt:

```csharp
com.company.package
```

Sie Sie die folgenden platzieren, würden `using` -Anweisung am Anfang der C#-Quelldateien auf Typen in den gebundenen **JAR** Datei:

```csharp
using Com.Company.Package;
```


Wenn Sie eine vorhandene Android-Bibliothek zu binden, ist es notwendig, die folgenden Punkte bedenken:

* **Gibt es keine externen Abhängigkeiten für die Bibliothek?** &ndash; Alle Java-Abhängigkeiten von der Android-Bibliothek erforderlich enthalten sein müssen, in das Projekt Xamarin.Android als eine **ReferenceJar** oder als ein **EmbeddedReferenceJar**. Alle systemeigenen Assemblys müssen hinzugefügt werden, um das bindungsprojekt als eine **EmbeddedNativeLibrary**.  

* **Welche Version der Android-API ist, wird die Bibliothek für Android-Ziel?** &ndash; Es ist nicht möglich "ein Downgrade die Android-API-Ebene;" Stellen Sie sicher, dass die Xamarin.Android Bindung-Projekt die gleiche API (oder höher) abzielt als der Android-Bibliothek.

* **Welche Version des JDK wurde verwendet, um die Bibliothek zu kompilieren?** &ndash; Binden Fehler kann auftreten, wenn die Android-Bibliothek mit einer anderen Version des JDK als verwendet vom Xamarin.Android erstellt wurde. Wenn möglich, kompilieren Sie erneut mit der Android-Bibliothek mit der gleichen Version des JDK, die durch die Installation von Xamarin.Android verwendet wird.


## <a name="build-actions"></a>Buildvorgänge

Bei der Erstellung einer Bibliothek Bindungen legen Sie *Buildvorgänge* auf die **JAR** oder. AAR-Dateien, die Sie in dem Bindungen-Bibliotheksprojekt integrieren &ndash; jedes Buildvorgang bestimmt, wie die **JAR** oder. AAR-Datei wird in eingebettet (oder verweist) Ihre Bindungen-Bibliothek. Die folgende Liste fasst diese Aktionen zu erstellen:

* `EmbeddedJar` &ndash; Bettet die **JAR** in die resultierende Bindungen Library-DLL als eingebettete Ressource. Dies ist die einfachste und die meisten häufig verwendete Buildvorgang. Verwenden Sie diese Option aus, wenn Sie möchten, dass die **JAR** automatisch in Byte-Code kompiliert und in der Bibliothek Bindungen gepackt.

* `InputJar` &ndash; Bettet nicht die **JAR** in die resultierende Bindungen-Bibliothek. DLL. Ihre Bibliothek Bindungen. Haben die DLL eine Abhängigkeit für dieses **JAR** zur Laufzeit. Verwenden Sie diese Option aus, wenn Sie nicht einschließen möchten die **JAR** in Ihrer Bibliothek Bindungen (z. B. für die Lizenzierung Gründe). Wenn Sie diese Option verwenden, Sie müssen sicherstellen, dass die Eingabe **JAR** steht auf dem Gerät, das Ihre app ausgeführt wird.

* `LibraryProjectZip` &ndash; Bettet eine. AAR-Datei in die resultierende Bindungen-Bibliothek. DLL. Dies ähnelt der EmbeddedJar, außer dass Sie in der gebundenen Ressourcen (ebenso wie Code) zugreifen können. AAR-Datei. Verwenden Sie diese Option aus, wenn Sie einbetten möchten ein. AAR in Ihrer Bibliothek Bindungen.

* `ReferenceJar` &ndash; Gibt einen Verweis **JAR**: ein Verweis **JAR** ist ein **JAR** , dass eine der die gebundenen **JAR** oder. AAR-Dateien abhängig. Diese Referenz **JAR** dient nur zum Zeitpunkt der Kompilierung Abhängigkeiten zu erfüllen. Bei Verwendung dieser Buildvorgang C#-Bindungen nicht für den Verweis erstellt **JAR** und nicht in der resultierenden Bindungen Bibliothek eingebettet. DLL. Verwenden Sie diese Option aus, wenn Sie eine Bibliothek Bindungen für den Verweis durchführen **JAR** jedoch nicht noch geschehen. Dieser Buildvorgang ist hilfreich, um mehrere Packen **JAR**s (und/oder. AARs) in mehrere voneinander abhängige Bindungen-Bibliotheken.

* `EmbeddedReferenceJar` &ndash; Einen Verweis bettet **JAR** in die resultierende Bindungen-Bibliothek. DLL. Verwenden Sie diese Buildvorgang aus, wenn Sie C#-Bindungen für sowohl die Eingabe erstellen möchten **JAR** (oder). AAR) und alle zugehörigen Verweis **JAR**(s) in Ihrer Bibliothek Bindungen.

* `EmbeddedNativeLibrary` &ndash; Bettet eine systemeigene **.so** in die Bindung. Dieser Buildvorgang wird zum **.so** Dateien, die erforderlich sind die **JAR** Datei gebunden wird. Es kann erforderlich sein, manuell laden die **.so** Bibliothek vor dem Ausführen des Codes von der Java-Bibliothek. Dies wird im folgenden beschrieben.

Diese Aktionen werden ausführlicher in den folgenden Handbüchern Build.

Darüber hinaus die folgenden Buildvorgänge verwendet, um Java-API-Dokumentation importieren und konvertieren sie in C#-XML-Dokumentation:

* `JavaDocJar` wird verwendet, um auf Javadoc-Archiv JAR-Datei für eine Java-Bibliothek verweist, die auf ein Maven-Paket-Format entspricht (in der Regel `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` wird verwendet, um zu zeigen `index.html` innerhalb der API-Referenzdokumentation HTML-Datei.
* `JavaSourceJar` Dient als Ergänzung zu `JavaDocJar`, um zuerst JavaDoc aus Quellen generieren und behandeln Sie die Ergebnisse als `JavaDocIndex`, Paket-Stil für eine Java-Bibliothek, die eine Maven entspricht (in der Regel `FOOBAR-sources**.jar**`).

Die API-Dokumentation muss die Standard-Doclet aus Java8, Java7 oder Java6-SDK (sie sind alle anderen Format), oder den DroidDoc-Stil.

## <a name="including-a-native-library-in-a-binding"></a>Eine systemeigene Bibliothek einzuschließen, in einer Bindung

Es kann erforderlich sein, enthalten eine **.so** Bibliothek in einem Xamarin.Android Bindung-Projekt als Teil der Bindung einer Java-Bibliothek. Wenn die umschlossene Java-Code ausgeführt wird, schlägt fehl Xamarin.Android der JNI-Aufruf und die Fehlermeldung zu _java.lang.UnsatisfiedLinkError: systemeigene Methode wurde nicht gefunden:_ wird in der Logcat, für die Anwendung angezeigt.

Die Lösung zu finden, manuell laden die **.so** Bibliothek mit einem Aufruf von `Java.Lang.JavaSystem.LoadLibrary`. Z. B. vorausgesetzt, dass es sich bei einem Projekt Xamarin.Android Bibliothek freigegebenen **libpocketsphinx_jni.so** im bindungsprojekt mit einem Buildvorgang enthalten **EmbeddedNativeLibrary**, die die folgenden Ausschnitt (wird ausgeführt, bevor Sie die freigegebene Bibliothek verwenden) lädt die **.so** Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Anpassen von Java-APIs in C&eparsl;

Der Xamarin.Android binden Generator ändert einige Java Idiome und Mustern in .NET Muster entsprechen. Die folgende Liste beschreibt, wie Java zugeordnet ist, in c# / .NET:

-   _Setter/Abrufmethoden_ in Java werden _Eigenschaften_ in .NET.

-   _Felder_ in Java werden _Eigenschaften_ in .NET.

-   _Listener/Listenerschnittstellen_ in Java werden _Ereignisse_ in .NET. Die Parameter der Methoden in der Rückrufschnittstelle dargestellte ein `EventArgs` Unterklasse.

-   Ein _statische geschachtelte Klasse_ in Java ist eine _der geschachtelten Klasse_ in .NET.

-   Ein _inneren Klasse_ in Java ist eine _der geschachtelten Klasse_ mit Instanzkonstruktor in C# geschrieben.



## <a name="binding-scenarios"></a>Binden von Szenarien

Die folgenden Handbüchern Bindung helfen bei der Java-Bibliothek (oder Bibliotheken) für die Einbindung in Ihre app zu binden:

-   [Binden von ein. JAR-](~/android/platform/binding-java-library/binding-a-jar.md) ist eine exemplarische Vorgehensweise zum Erstellen von Bindungen-Bibliotheken für **JAR** Dateien.

-   [Binden von ein. AAR](~/android/platform/binding-java-library/binding-an-aar.md) ist eine exemplarische Vorgehensweise zum Erstellen von Bindungen für Bibliotheken. AAR-Dateien. Lesen Sie diese exemplarische Vorgehensweise, um Informationen zum Binden von Android Studio Bibliotheken.

-   [Binden eine Eclipse-Bibliotheksprojekt](~/android/platform/binding-java-library/binding-a-library-project.md) ist eine exemplarische Vorgehensweise zum Erstellen der Bindung Bibliotheken aus Projekten für Android-Bibliothek. Lesen Sie diese exemplarische Vorgehensweise zum Erlernen der Eclipse-Android-Bibliotheksprojekte binden.

-   [Anpassen von Bindungen](~/android/platform/binding-java-library/customizing-bindings/index.md) wird erläutert, wie Sie manuelle Änderungen an der Bindung zum Auflösen von Buildfehler und Form der resultierenden API so, dass er größer ist "c#-wie".

-   [Problembehandlung bei Bindungen](~/android/platform/binding-java-library/troubleshooting-bindings.md) Listet allgemeine Fehlerszenarien für die Bindung, erläutert mögliche Ursachen und Vorschläge zur Behebung dieser Fehler bietet.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI Metadaten](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Verwenden nativer Bibliotheken](~/android/platform/native-libraries.md)
