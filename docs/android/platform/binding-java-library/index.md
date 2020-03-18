---
title: Binden einer Java-Bibliothek
description: Die Android-Community verfügt über viele Java-Bibliotheken, die Sie möglicherweise in Ihrer App verwenden möchten. In diesem Handbuch wird erläutert, wie Sie Java-Bibliotheken in Ihre Xamarin.Android-Anwendung integrieren, indem Sie eine Bindungsbibliothek erstellen.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: d40a23076ec8f405e57ec40de47ec9ad2261d85d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027604"
---
# <a name="binding-a-java-library"></a>Binden einer Java-Bibliothek

_Die Android-Community verfügt über viele Java-Bibliotheken, die Sie möglicherweise in Ihrer App verwenden möchten. In diesem Handbuch wird erläutert, wie Sie Java-Bibliotheken in Ihre Xamarin.Android-Anwendung integrieren, indem Sie eine Bindungsbibliothek erstellen._

## <a name="overview"></a>Übersicht

Das Ökosystem für Bibliotheken von Drittanbietern für Android ist enorm. Aus diesem Grund ist es oft sinnvoller, eine vorhandene Android-Bibliothek zu verwenden, als eine neue zu erstellen. Xamarin.Android bietet zwei Möglichkeiten, diese Bibliotheken zu verwenden:

- Sie können eine *Bindungsbibliothek* erstellen, die die Bibliothek automatisch mit C#-Wrappern umschließt, damit Sie Java-Code über C#-Aufrufe aufrufen können.

- Sie verwenden *Java Native Interface* (*JNI*), um Aufrufe direkt im Java-Bibliothekscode aufzurufen. JNI ist ein Programmierframework, mit dem Java-Code native Anwendungen oder Bibliotheken aufrufen bzw. von diesen aufgerufen werden kann.

In diesem Leitfaden wird die erste Option erläutert: Sie erfahren, wie man eine *Bindungsbibliothek* erstellt, die eine oder mehrere vorhandene Java-Bibliotheken in eine Assembly umschließt, die Sie mit Ihrer Anwendung verknüpfen können. Weitere Informationen zum Verwenden von JNI finden Sie unter [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implementiert Bindungen mithilfe von *verwalteten Aufrufwrappern* (*Managed Callable Wrappers, MCW*). MCW ist eine JNI-Brücke, die verwendet wird, wenn verwalteter Code Java-Code aufrufen muss. Verwaltete Aufrufwrapper bieten auch Unterstützung für das Erstellen von Unterklassen für Java-Typen und das Überschreiben virtueller Methoden von Java-Typen. Wenn ART-Code (Android Runtime) verwalteten Code aufrufen möchte, wird dieser Vorgang ebenso über eine andere JNI-Brücke ausgeführt, die als Android-Aufrufwrapper (Android Callable Wrappers, ACW) bezeichnet wird. Diese [Architektur](~/android/internals/architecture.md) wird in der nachfolgenden Abbildung dargestellt:

[![Architektur der Android-JNI-Brücke](images/architecture.png)](images/architecture.png#lightbox)

Eine Bindungsbibliothek ist eine Assembly, die verwaltete Aufrufwrapper für Java-Typen enthält. Hier sehen Sie beispielsweise einen Java-Typ (`MyClass`), den Sie in eine Bindungsbibliothek einschließen möchten:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Nachdem wir eine Bindungsbibliothek für die **JAR**-Datei generiert haben, die `MyClass` enthält, können wir diese instanziieren und Methoden von C# für sie aufrufen:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Sie verwenden die Xamarin.Android-Vorlage *Bibliothek für Java-Bindungen*, um diese Bindungsbibliothek zu erstellen. Das resultierende Bindungsprojekt erstellt eine .NET-Assembly mit den MCW-Klassen, **JAR**-Dateien und Ressourcen für Android-Bibliotheksprojekte, die darin eingebettet sind. Sie können auch Bindungsbibliotheken für Android Archiv-Dateien (AAR) und Eclipse-Android-Bibliotheksprojekte erstellen. Wenn Sie auf die resultierende DLL-Assembly der Bindungsbibliothek verweisen, können Sie eine vorhandene Java-Bibliothek in Ihrem Xamarin.Android-Projekt wiederverwenden.

Wenn Sie auf Typen in der Bindungsbibliothek verweisen, müssen Sie den Namespace der Bindungsbibliothek verwenden. In der Regel fügen Sie am Anfang Ihrer C#-Quelldateien eine `using`-Direktive hinzu, die die .NET-Namespaceversion des Java-Paketnamens ist. Beispiel: Der Java-Paketname für die gebundene **JAR**-Datei ist der folgende:

```csharp
com.company.package
```

Dann fügen Sie die folgende `using`-Anweisung am Anfang der C#-Quelldateien ein, um auf Typen in der gebundenen **JAR**-Datei zuzugreifen:

```csharp
using Com.Company.Package;
```

Beim Binden einer vorhandenen Android-Bibliothek müssen folgende Punkte beachtet werden:

- **Gibt es externe Abhängigkeiten für die Bibliothek?** &ndash; Alle Java-Abhängigkeiten, die von der Android-Bibliothek benötigt werden, müssen im Xamarin.Android-Projekt als **ReferenceJar** oder als **EmbeddedReferenceJar** enthalten sein. Alle nativen Assemblys müssen dem Bindungsprojekt als **EmbeddedNativeLibrary** hinzugefügt werden.  

- **Welche Version der Android-API ist das Ziel der Android-Bibliothek?** &ndash; Es ist nicht möglich, die Android-API-Ebene „herabzustufen“. Das Xamarin.Android-Bindungsprojekt muss auf dieselbe API-Ebene (oder höher) wie die Android-Bibliothek abzielen.

- **Welche Version des JDK wurde verwendet, um die Bibliothek zu kompilieren?** &ndash; Bindungsfehler können auftreten, wenn die Android-Bibliothek mit einer anderen als der von Xamarin.Android verwendeten JDK-Version erstellt wurde. Kompilieren Sie die Android-Bibliothek möglichst mit derselben JDK-Version neu, die von der jeweiligen Xamarin.Android-Installation verwendet wird.

## <a name="build-actions"></a>Buildvorgänge

Wenn Sie eine Bindungsbibliothek erstellen, legen Sie *Buildaktionen* für die **JAR**- oder AAR-Dateien fest, die Sie in Ihr Bindungsbibliothekprojekt integrieren. Jede Buildaktion bestimmt, wie die **JAR**- oder AAR-Datei in Ihre Bindungsbibliothek eingebettet bzw. von dieser referenziert wird. In der folgenden Liste werden diese Buildaktionen zusammengefasst:

- `EmbeddedJar` &ndash; Hiermit wird die **JAR**-Datei in die resultierende DLL-Datei der Bindungsbibliothek als eingebettete Ressource eingebettet. Dies ist die einfachste und am häufigsten verwendete Buildaktion. Verwenden Sie diese Option, wenn Sie möchten, dass die **JAR**-Datei automatisch in Bytecode kompiliert und in die Bindungsbibliothek gepackt wird.

- `InputJar` &ndash; Hiermit wird die **JAR**-Datei nicht in die resultierende DLL-Datei der Bindungsbibliothek eingebettet. Die DLL-Datei Ihrer Bindungsbibliothek weist zur Laufzeit eine Abhängigkeit von dieser **JAR**-Datei auf. Verwenden Sie diese Option, wenn Sie die **JAR**-Datei nicht in Ihre Bindungsbibliothek einschließen möchten (z. B. aus Lizenzierungsgründen). Wenn Sie diese Option verwenden, müssen Sie sicherstellen, dass die **JAR**-Eingabedatei auf dem Gerät verfügbar ist, das Ihre App ausführt.

- `LibraryProjectZip` &ndash; Hiermit wird eine AAR-Datei in die resultierende DLL-Datei der Bindungsbibliothek eingebettet. Dies ist vergleichbar mit „EmbeddedJar“, mit dem Unterschied, dass Sie auf Ressourcen (und Code) in der gebundenen AAR-Datei zugreifen können. Verwenden Sie diese Option, wenn Sie eine AAR-Datei in Ihre Bindungsbibliothek einbetten möchten.

- `ReferenceJar` &ndash; Hiermit wird eine **JAR**-Verweisdatei angegeben: Eine **JAR**-Verweisdatei ist eine **JAR**-Datei, von der eine Ihrer gebundenen **JAR**- oder AAR-Dateien abhängig ist. Diese **JAR**-Verweisdatei wird nur verwendet, um Abhängigkeiten der Kompilierzeit zu erfüllen. Wenn Sie diese Buildaktion verwenden, werden keine C#-Bindungen für die **JAR**-Verweisdatei erstellt und diese nicht in die resultierende DLL-Datei der Bindungsbibliothek eingebettet. Verwenden Sie diese Option, wenn Sie eine Bindungsbibliothek für die **JAR**-Verweisdatei erstellen möchten, dies aber noch nicht getan haben. Diese Buildaktion ist beim Packen mehrerer **JAR**-Dateien (und/oder AAR-Dateien) in mehrere abhängige Bindungsbibliotheken hilfreich.

- `EmbeddedReferenceJar` &ndash; Hiermit wird eine **JAR**-Verweisdatei in die resultierende DLL-Datei der Bindungsbibliothek eingebettet. Verwenden Sie diese Buildaktion, wenn Sie C#-Bindungen für die **JAR**-Eingabedatei (bzw. AAR-Datei) und alle zugehörigen **JAR**-Verweisdateien in der Bindungsbibliothek erstellen möchten.

- `EmbeddedNativeLibrary` &ndash; Hiermit wird eine native **SO**-Datei in die Bindung eingebettet. Diese Buildaktion wird für **SO**-Dateien verwendet, die für die Bindung von **JAR**-Dateien erforderlich sind. Es kann erforderlich sein, die **SO**-Bibliothek vor dem Ausführen von Code aus der Java-Bibliothek manuell zu laden. Dies wird im Folgenden beschrieben.

Diese Buildaktionen werden in den folgenden Leitfäden ausführlicher erläutert.

Außerdem werden die folgenden Buildaktionen verwendet, um die Java-API-Dokumentation zu importieren und in die C#-XML-Dokumentation zu konvertieren:

- `JavaDocJar` wird verwendet, um auf eine JAR-Datei in einem JavaDoc-Archiv für eine Java-Bibliothek zu verweisen, die einem Maven-Paketstil entspricht (normalerweise `FOOBAR-javadoc**.jar**`).
- `JavaDocIndex` wird verwendet, um auf eine `index.html`-Datei in der HTML-API-Referenzdokumentation zu verweisen.
- `JavaSourceJar` wird zum Ergänzen von `JavaDocJar` verwendet, um zunächst JavaDoc aus Quellen zu generieren und die Ergebnisse dann als `JavaDocIndex` für eine Java-Bibliothek zu behandeln, die einem Maven-Paketstil entspricht (normalerweise `FOOBAR-sources**.jar**`).

Die API-Dokumentation sollte das Standard-Doclet des Java8-, Java7- oder Java6-SDK sein (alle in unterschiedlichen Formaten) oder dem DroidDoc-Stil entsprechen.

## <a name="including-a-native-library-in-a-binding"></a>Einschließen einer nativen Bibliothek in eine Bindung

Es kann erforderlich sein, eine **SO**-Bibliothek in ein Xamarin.Android-Bindungsprojekt als Teil der Bindung einer Java-Bibliothek einzuschließen. Wenn der Java-Wrappercode ausgeführt wird, kann Xamarin.Android den JNI-Aufruf nicht durchführen, sodass die Fehlermeldung _java.lang.UnsatisfiedLinkError: Native Methode nicht gefunden:_ in der Logcat-Ausgabe für die Anwendung angezeigt wird.

Diese Problem kann behoben werden, indem die **SO**-Bibliothek mit einem Aufruf von `Java.Lang.JavaSystem.LoadLibrary` manuell geladen wird. Wenn ein Xamarin.Android-Projekt beispielsweise über die freigegebene Bibliothek **libpocketsphinx_jni.so** verfügt, die im Bindungsprojekt mit der Buildaktion **EmbeddedNativeLibrary** enthalten ist, lädt der folgende Codeausschnitt (der vor der Verwendung der freigegebenen Bibliothek ausgeführt wird) die **SO**-Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Anpassen von Java-APIs an C&eparsl;

Der Xamarin.Android-Bindungsgenerator ändert einige Java-Ausdrücke und -Muster, um den .NET-Mustern zu entsprechen. In der folgenden Liste wird beschrieben, wie Java C#/.NET zugeordnet wird:

- _Setter-/Getter-Methoden_ in Java sind _Eigenschaften_ in .NET.

- _Felder_ in Java sind _Eigenschaften_ in .NET.

- _Listener/Listenerschnittstellen_ in Java sind _Ereignisse_ in .NET. Die Parameter der Methoden in den Rückrufschnittstellen werden durch eine `EventArgs`-Unterklasse dargestellt.

- Eine _statische, geschachtelte Klasse_ in Java ist eine _geschachtelte Klasse_ in .NET.

- Eine _innere Klasse_ in Java ist eine _geschachtelte Klasse_ mit einem Instanzkonstruktor in C#.

## <a name="binding-scenarios"></a>Bindungsszenarios

Die folgenden Leitfäden für Bindungsszenarios können Ihnen helfen, eine oder mehrere Java-Bibliotheken für die Einbindung in Ihre App zu binden:

- Der Leitfaden zum [Binden von JAR-Dateien](~/android/platform/binding-java-library/binding-a-jar.md) enthält eine exemplarische Vorgehensweise zum Erstellen von Bindungsbibliotheken für **JAR**-Dateien.

- Der Leitfaden zum [Binden von AAR-Dateien](~/android/platform/binding-java-library/binding-an-aar.md) enthält eine exemplarische Vorgehensweise zum Erstellen von Bindungsbibliotheken für AAR-Dateien. Lesen Sie diese exemplarische Vorgehensweise, um zu erfahren, wie Sie Android Studio-Bibliotheken binden.

- Der Leitfaden zum [Binden eines Eclipse-Bibliotheksprojekts](~/android/platform/binding-java-library/binding-a-library-project.md) enthält eine exemplarische Vorgehensweise zum Erstellen von Bindungsbibliotheken aus Android-Bibliotheksprojekten. Lesen Sie diese exemplarische Vorgehensweise, um zu erfahren, wie Sie Eclipse-Android-Bibliotheksprojekte binden.

- Im Leitfaden zum [Anpassen von Bindungen](~/android/platform/binding-java-library/customizing-bindings/index.md) wird erläutert, wie manuelle Änderungen an der Bindung vorgenommen werden, um Buildfehler zu beheben und die resultierende API so zu strukturieren, dass sie mit C# konsistent ist.

- Im Leitfaden zur [Problembehandlung von Bindungen](~/android/platform/binding-java-library/troubleshooting-bindings.md) werden häufige Bindungsfehlerszenarios aufgelistet, mögliche Ursachen erläutert und Vorschläge zum Beheben dieser Fehler angeboten.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI-Metadaten](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Verwenden nativer Bibliotheken](~/android/platform/native-libraries.md)
