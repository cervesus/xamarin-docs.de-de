---
title: Problembehandlung von Bindungen
description: In diesem Artikel werden mehrere häufige Fehler, die beim Erstellen von Bindungen auftreten können, sowie mögliche Ursachen und empfohlene Möglichkeiten, diese zu beheben, behandelt.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3ec01d9322d4b6706fccd0959ae5944f71e82664
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853026"
---
# <a name="troubleshooting-bindings"></a>Problembehandlung von Bindungen

> [!IMPORTANT]
> Wir untersuchen derzeit die Nutzung benutzerdefinierter Bindungen auf der Xamarin-Plattform. Nehmen Sie an [**dieser Umfrage**](https://www.surveymonkey.com/r/KKBHNLT) teil, um zukünftige Entwicklungsarbeiten zu unterstützen.

_In diesem Artikel werden mehrere häufige Fehler, die beim Erstellen von Bindungen auftreten können, sowie mögliche Ursachen und empfohlene Möglichkeiten, diese zu beheben, behandelt._

## <a name="overview"></a>Übersicht

Das Binden einer Android-Bibliotheksdatei (**AAR**- oder **JAR**-Datei) ist meistens kompliziert. In der Regel ist ein zusätzlicher Aufwand erforderlich, um Probleme zu beheben, die sich aufgrund der Unterschiede zwischen Java und .NET ergeben.
Diese Probleme verhindern, dass Xamarin.Android die Android-Bibliothek bindet, und werden im Buildprotokoll als Fehlermeldungen angezeigt. Diese Anleitung enthält einige Tipps zur Problembehandlung, eine Liste der häufigeren Probleme/Szenarios sowie mögliche Lösungen für eine erfolgreiche Bindung der Android-Bibliothek.

Beim Binden einer vorhandenen Android-Bibliothek müssen folgende Punkte beachtet werden:

- **Die externen Abhängigkeiten für die Bibliothek** &ndash; Alle Java-Abhängigkeiten, die von der Android-Bibliothek benötigt werden, müssen im Xamarin.Android-Projekt als **ReferenceJar** oder als **EmbeddedReferenceJar** enthalten sein.

- **Die Android-API-Zielebene der Android-Bibliothek** &ndash; Es ist nicht möglich, die Android-API-Ebene „herabzustufen“. Das Xamarin.Android-Bindungsprojekt muss auf dieselbe API-Zielebene (oder höher) wie die Android-Bibliothek aufweisen.

- **Die Version des Android JDKs, das zum Packen der Android-Bibliothek verwendet wurde** &ndash; Wenn die Android-Bibliothek mit einer anderen JDK-Version erstellt wurde als die von Xamarin.Android verwendete Version, können Bindungsfehler auftreten. Kompilieren Sie die Android-Bibliothek möglichst mit derselben JDK-Version neu, das von der jeweiligen Xamarin.Android-Installation verwendet wird.

Der erste Schritt bei der Behebung von Problemen beim Binden einer Xamarin.Android-Bibliothek besteht darin, die [MSBuild-Diagnoseausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) zu aktivieren.
Kompilieren Sie das Xamarin.Android-Bindungsprojekt nach dem Aktivieren der Diagnoseausgabe erneut, und überprüfen Sie das Buildprotokoll, um Hinweise zur Ursache des Problems zu finden.

Außerdem kann es hilfreich sein, die Android-Bibliothek zu dekompilieren und die Typen und Methoden zu untersuchen, die Xamarin.Android binden möchte. Dies weiter unten in dieser Anleitung ausführlicher beschrieben.

## <a name="decompiling-an-android-library"></a>Dekompilieren einer Android-Bibliothek

Die Überprüfung der Klassen und Methoden der Java-Klassen kann wertvolle Informationen liefern, die beim Binden einer Bibliothek von Nutzen sind.
[JD-GUI](http://jd.benow.ca/) ist ein grafisches Hilfsprogramm, das Java-Quellcode aus den **CLASS**-Dateien anzeigen kann, die in einer JAR-Datei enthalten sind. Es kann als eigenständige Anwendung oder als Plug-In für IntelliJ oder Eclipse ausgeführt werden.

Um eine Android-Bibliothek zu dekompilieren, öffnen Sie die **JAR** Datei mit dem Java Decompiler. Wenn es sich bei der Bibliothek um eine **AAR**-Datei handelt, muss die Datei **classes.jar** aus der Archivdatei extrahiert werden. Der folgende Beispielscreenshot veranschaulicht die Verwendung von JD-GUI zum Analysieren der [Picasso](https://square.github.io/picasso/)-JAR:

![Verwenden von Java Decompiler zum Analysieren von „picasso-2.5.2.jar“](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Untersuchen Sie den Quellcode, nachdem Sie die Android-Bibliothek dekompiliert haben. Suchen Sie generell nach:

- **Klassen, die Merkmale einer Verschleierung aufweisen** &ndash; Zu den Merkmalen von verschleierten Klassen gehören:

  - Der Klassenname enthält ein **$** , z. B. **a$.class**.
  - Der Klassenname besteht ausschließlich aus Kleinbuchstaben, z. B. **a.class**.      

- **`import`-Anweisungen für nicht referenzierte Bibliotheken** &ndash; Ermitteln Sie die nicht referenzierte Bibliothek und deren Abhängigkeiten zum Xamarin.Android-Bindungsprojekt mit **ReferenceJar** oder **EmbedddedReferenceJar** als **Buildvorgang**.

> [!NOTE]
> Das Dekompilieren einer Java-Bibliothek ist möglicherweise unzulässig oder unterliegt aufgrund von lokalen Gesetzen oder der Lizenz, unter der die Java-Bibliothek veröffentlicht wurde, rechtlichen Beschränkungen. Nehmen Sie ggf. die Dienste eines spezialisierten Juristen in Anspruch, bevor Sie versuchen, eine Java-Bibliothek zu dekompilieren und den Quellcode zu untersuchen.

## <a name="inspect-apixml"></a>Untersuchen von API.XML

Bei der Erstellung eines Bindungsprojekts generiert Xamarin.Android eine XML-Datei namens **obj/Debug/api.xml**:

![Generierte „api.xml“ unter „obj/Debug“](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Diese Datei enthält eine Liste aller Java-APIs, die Xamarin.Android binden möchte. Der Inhalt dieser Datei kann hilfreich sein, um fehlende Typen oder Methoden bzw. doppelte Bindungen zu ermitteln. Obwohl die Überprüfung dieser Datei mühsam und zeitaufwändig ist, kann sie Hinweise auf mögliche Ursachen für Bindungsprobleme liefern. Beispielsweise kann **api.xml** zeigen, dass eine Eigenschaft einen ungeeigneten Typ zurückgibt oder zwei Typen mit demselben verwalteten Namen vorhanden sind.

## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden einige der häufigsten Fehlermeldungen oder Symptome aufgelistet, die beim Binden einer Android-Bibliothek auftreten können.

### <a name="problem-java-version-mismatch"></a>Problem: Java-Versionskonflikt

Manchmal werden Typen nicht generiert, oder es können unerwartete Abstürze auftreten, da Sie eine andere (neuere oder eine ältere) Version von Java verwenden als die Version, mit der die Bibliothek kompiliert wurde. Kompilieren Sie die Android-Bibliothek mit der gleichen JDK-Version neu, die vom Xamarin.Android-Projekt verwendet wird.

### <a name="problem-at-least-one-java-library-is-required"></a>Problem: Mindestens eine Java-Bibliothek erforderlich

Sie erhalten die Fehlermeldung, dass mindestens eine Java-Bibliothek erforderlich ist, obwohl eine JAR-Datei hinzugefügt wurde.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Vergewissern Sie sich, dass der Buildvorgang auf `EmbeddedJar` festgelegt ist. Da es mehrere Buildvorgänge für JAR-Dateien gibt (z. B. `InputJar`, `EmbeddedJar`, `ReferenceJar` und `EmbeddedReferenceJar`), kann der Bindungsgenerator nicht automatisch erahnen, welcher standardmäßig verwendet werden soll. Weitere Informationen zu Buildvorgängen finden Sie unter [Buildvorgänge](~/android/platform/binding-java-library/index.md).

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problem: Bindungstools können die JAR-Bibliothek nicht laden

Der Bindungsbibliotheksgenerator kann die JAR-Bibliothek nicht laden.

#### <a name="possible-causes"></a>Mögliche Ursachen

Einige JAR-Bibliotheken, die Codeverschleierung verwenden (über Tools wie Proguard), können nicht von den Java-Tools geladen werden. Da unser Tool Java-Reflexion und die ASM Byte Code Engineering-Bibliothek verwendet, können die betreffenden abhängigen Tools die verschleierten Bibliotheken im Gegensatz zu Android-Runtimetools ablehnen. Um dieses Problem zu umgehen, können Sie diese Bibliotheken manuell binden, anstatt den Bindungsgenerator zu verwenden.

### <a name="problem-missing-c-types-in-generated-output"></a>Problem: Fehlende C# Typen in der generierten Ausgabe

Die Bindungs-**DLL** wird zwar erstellt, es fehlen aber einige Java-Typen, oder der generierte C#-Quellcode wird mit der Fehlermeldung, dass Typen fehlen, nicht kompiliert.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dieser Fehler kann aus verschiedenen Gründen auftreten, die nachstehend aufgelistet sind:

- Die gebundene Bibliothek kann auf eine zweite Java-Bibliothek verweisen. Wenn die öffentliche API für die gebundene Bibliothek Typen aus der zweiten Bibliothek verwendet, müssen Sie eben falls auf eine verwaltete Bindung für die zweite Bibliothek verweisen.

- Es ist möglich, dass eine Bibliothek aufgrund von Java-Reflexion eingefügt wurde, was ähnlich wie beim oben beschriebenen Bibliotheksladefehler ein unerwartetes Laden von Metadaten zur Folge hat. Die Xamarin.Android-Tools können diese Situation derzeit nicht lösen. In einem solchen Fall muss die Bibliothek manuell gebunden werden.

- Es gab einen Fehler in der .NET 4.0-Runtime, sodass erforderliche Assemblys nicht geladen wurden. Dieses Problem wurde in der .NET-Runtime 4.5 behoben.

- Java ermöglicht das Ableiten einer öffentlichen Klasse von einer nicht öffentlichen Klasse, dies wird aber in .NET nicht unterstützt. Da der Bindungsgenerator keine Bindungen für nicht öffentliche Klassen generiert, können abgeleitete Klassen wie diese nicht ordnungsgemäß generiert werden. Um dieses Problem zu beheben, entfernen Sie entweder den Metadateneintrag für die abgeleiteten Klassen mit „remove-node“ in **Metadata.xml**, oder korrigieren Sie die Metadaten, die die nicht öffentliche Klasse als öffentlich festlegen. Obwohl die zweite Lösung die Bindung so erstellt, dass der C#-Quellcode erstellt wird, sollte die nicht öffentliche Klasse nicht verwendet werden.

  Zum Beispiel:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Tools, die Java-Bibliotheken verschleiern, können den Xamarin.Android-Bindungsgenerator und seine Fähigkeit zum Generieren von C#-Wrapperklassen beeinträchtigen. Der folgende Codeausschnitt zeigt, wie **Metadata.xml** aktualisiert werden muss, um die Verschleierung eines Klassennamens aufzuheben:

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problem: Der generierte C#-Quellcode wird aufgrund eines Parametertypkonflikts nicht kompiliert

Der generierte C#-Quellcode wird nicht kompiliert. Die Parametertypen der überschriebenen Methode stimmen nicht überein.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Xamarin.Android enthält eine Vielzahl von Java-Feldern, die Enumerationen in den C#-Bindungen zugeordnet sind. Diese können Typinkompatibilitäten in den generierten Bindungen verursachen. Um dieses Problem zu beheben, müssen die vom Bindungsgenerator erstellten Methodensignaturen so geändert werden, dass die Enumerationen verwendet werden. Weitere Informationen finden Sie im Abschnitt zum [Korrigieren von Enumerationen](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problem: NoClassDefFoundError bei der Paketerstellung

Im Paketerstellungsschritt wird `java.lang.NoClassDefFoundError` ausgelöst.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Der wahrscheinlichste Grund für diesen Fehler besteht darin, dass eine erforderliche Java-Bibliothek zum Anwendungsprojekt ( **.csproj**) hinzugefügt werden muss. JAR-Dateien werden nicht automatisch aufgelöst. Für eine Benutzerassembly, die auf dem Zielgerät oder im Emulator nicht vorhanden ist (z. B. **maps.jar** für Google Maps), wird nicht immer eine Java-Bibliotheksbindung generiert. Dies gilt nicht für die Unterstützung von Android-Bibliotheksprojekten, da die JAR-Bibliotheksdatei in die Bibliotheks-DLL eingebettet ist. Zum Beispiel: [Bug 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problem: Doppelte benutzerdefinierte EventArgs-Typen

Der Buildvorgang schlägt aufgrund doppelter benutzerdefinierter EventArgs-Typen fehl. Die Fehlermeldung sieht in etwa folgendermaßen aus:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dies liegt daran, dass ein Konflikt zwischen Ereignistypen besteht, die von mehreren Schnittstellenlistenertypen stammen, die Methoden mit identischen Namen aufweisen. Wenn beispielsweise wie im folgenden Beispiel zwei Java-Schnittstellen vorhanden sind, erstellt der Generator `DismissScreenEventArgs` für `MediationBannerListener` und `MediationInterstitialListener`, was zu einem Fehler führt.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Dies ist systembedingt, um sehr lange Namen für Ereignisargumenttypen zu vermeiden. Um diese Konflikte zu vermeiden, ist eine Metadatentransformation erforderlich. Bearbeiten Sie **Transforms\Metadata.xml**, und fügen Sie ein `argsType`-Attribut für beide Schnittstellen (oder für die Schnittstellenmethode) hinzu:

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>Problem: Klasse implementiert Schnittstellenmethode nicht

Es wird eine Fehlermeldung mit dem Hinweis erzeugt, dass eine Methode, die für eine Schnittstelle erforderlich ist, die die generierte Klasse implementiert, von der generierten Klasse nicht implementiert wird. Wenn Sie den generierten Code betrachten, sehen Sie jedoch, dass die-Methode implementiert ist.

Hier sehen Sie ein Beispiel für den Fehler:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dieses Problem tritt bei der Bindung von Java-Methoden mit kovarianten Rückgabetypen auf. In diesem Beispiel muss die Methode `Oauth.Signpost.Http.IHttpRequest.UnWrap()` den Typ `Java.Lang.Object` zurückgeben. Die Methode `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` gibt jedoch den Typ `HttpURLConnection` zurück. Es gibt zwei Möglichkeiten, dieses Problem zu beheben:

- Fügen Sie eine partielle Klassendeklaration für `HttpURLConnectionRequestAdapter` hinzu, und implementieren Sie `IHttpRequest.Unwrap()`explizit:

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- Entfernen Sie die Kovarianz aus dem generierten C#-Code. Dazu muss die folgende Transformation in **Transforms\Metadata.xml** hinzugefügt werden, die bewirkt, dass der generierte C#-Code den Rückgabetyp `Java.Lang.Object`aufweist:

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problem: Namenskonflikte in inneren Klassen/Eigenschaften

In Konflikt stehende Sichtbarkeit für geerbte Objekte.

In Java muss eine abgeleitete Klasse nicht die gleiche Sichtbarkeit wie das zugehörige übergeordnete Element aufweisen. Java löst dies automatisch. In C# muss dies explizit erfolgen. Sie müssen also sicherstellen, dass alle Klassen in der Hierarchie die entsprechende Sichtbarkeit aufweisen. Im folgenden Beispiel wird gezeigt, wie ein Java-Paketname von `com.evernote.android.job` in `Evernote.AndroidJob` geändert wird:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problem: Eine von der Bindung benötigte **SO**-Bibliothek wird nicht geladen

Einige Bindungsprojekte können auch von Funktionen in einer **SO**-Bibliothek. Es ist möglich, dass Xamarin.Android die **SO**-Bibliothek nicht automatisch lädt. Wenn der Java-Wrappercode ausgeführt wird, kann Xamarin.Android den JNI-Aufruf nicht durchführen, sodass die Fehlermeldung _java.lang.UnsatisfiedLinkError: Native Methode nicht gefunden:_ in der Logcat-Ausgabe für die Anwendung angezeigt wird.

Diese Problem kann behoben werden, indem die **SO**-Bibliothek mit einem Aufruf von `Java.Lang.JavaSystem.LoadLibrary` manuell geladen wird. Wenn ein Xamarin.Android-Projekt beispielsweise über die freigegebene Bibliothek **libpocketsphinx_jni.so** verfügt, die im Bindungsprojekt mit dem Buildvorgang **EmbeddedNativeLibrary** enthalten ist, lädt der folgende Codeausschnitt (der vor der Verwendung der freigegebenen Bibliothek ausgeführt wird) die **SO**-Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige häufig auftretende Probleme im Zusammenhang mit Java-Bindungen aufgelistet und deren Behebung erläutert.

## <a name="related-links"></a>Verwandte Links

- [Bibliotheksprojekte](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Aktivieren der Diagnoseausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin für Android-Entwickler](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
