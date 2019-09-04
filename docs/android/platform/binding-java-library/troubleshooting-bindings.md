---
title: Problembehandlung von Bindungen
description: In diesem Artikel werden häufige Server Fehler zusammengefasst, die beim Erstellen von Bindungen auftreten können, sowie mögliche Ursachen und empfohlene Möglichkeiten, diese zu beheben.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 2137ff95e65c6841b3e525f0c9755e013310c7e0
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70225594"
---
# <a name="troubleshooting-bindings"></a>Problembehandlung von Bindungen

_In diesem Artikel werden häufige Server Fehler zusammengefasst, die beim Erstellen von Bindungen auftreten können, sowie mögliche Ursachen und empfohlene Möglichkeiten, diese zu beheben._


## <a name="overview"></a>Übersicht

Das Binden einer Android-Bibliothek (eine **. Aar** -oder **jar**-Datei) ist selten eine einfache Angelegenheit. Dies erfordert in der Regel zusätzlichen Aufwand, um Probleme zu beheben, die sich aus den Unterschieden zwischen Java und .net ergeben.
Diese Probleme verhindern, dass xamarin. Android die Android-Bibliothek bindet und als Fehlermeldungen im Buildprotokoll angezeigt wird. Dieses Handbuch enthält einige Tipps zur Problembehandlung, eine Liste der gängigeren Probleme/Szenarios sowie mögliche Lösungen für eine erfolgreiche Bindung der Android-Bibliothek.

Beim Binden einer vorhandenen Android-Bibliothek müssen folgende Punkte berücksichtigt werden:

- **Die externen Abhängigkeiten für die Bibliothek** Alle Java-Abhängigkeiten, die für die Android-Bibliothek erforderlich sind, müssen im xamarin. Android-Projekt als **referencejar** oder als **embeddedreferencejar**enthalten sein. &ndash;

- **Die Android-API-Ebene, auf die die Android-Bibliothek** abzielt &ndash; Es ist nicht möglich, die Android-API-Ebene zu "herabstufen". Stellen Sie sicher, dass das xamarin. Android-Bindungs Projekt auf dieselbe API-Ebene (oder höher) wie die Android-Bibliothek abzielt.

- **Die Version des Android JDK, das zum Verpacken der Android-Bibliothek verwendet wurde** . &ndash; Bindungs Fehler können auftreten, wenn die Android-Bibliothek mit einer anderen Version von JDK erstellt wurde, als Sie von xamarin. Android verwendet wird. Wenn möglich, kompilieren Sie die Android-Bibliothek mit derselben Version des JDK neu, das von Ihrer Installation von xamarin. Android verwendet wird.

Der erste Schritt bei der Behebung von Problemen bei der Bindung einer xamarin. Android-Bibliothek besteht darin, die [Diagnose-MSBuild-Ausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)zu aktivieren.
Erstellen Sie nach dem Aktivieren der Diagnoseausgabe das xamarin. Android-Bindungs Projekt neu, und überprüfen Sie das Buildprotokoll, um Hinweise zur Ursache des Problems zu finden.

Außerdem kann es hilfreich sein, die Android-Bibliothek zu dekompilieren und die Typen und Methoden zu untersuchen, die von xamarin. Android gebunden werden. Dies wird später in diesem Handbuch ausführlicher behandelt.


## <a name="decompiling-an-android-library"></a>Dekompilieren einer Android-Bibliothek

Wenn Sie die Klassen und Methoden der Java-Klassen überprüfen, können Sie wertvolle Informationen bereitstellen, die beim Binden einer Bibliothek helfen.
[JD-GUI](http://jd.benow.ca/) ist ein grafisches Hilfsprogramm, mit dem Java-Quellcode aus den in einer JAR-Datei enthaltenen **Klassen** Dateien angezeigt werden kann. Sie kann als eigenständige Anwendung oder als Plug-in für IntelliJ oder Eclipse ausgeführt werden.

Öffnen Sie zum dekompilieren einer Android-Bibliothek das **. JAR** -Datei mit dem Java-Decompiler. , Wenn die Bibliothek eine ist **. AAR** -Datei, muss die Datei **Classes. jar** aus der Archivdatei extrahiert werden. Im folgenden finden Sie ein Beispiel für die Verwendung von JD-GUI zum Analysieren der [Picasso](http://square.github.io/picasso/) -JAR-Datei:

![Verwenden des Java-decocompilers zum Analysieren von Picasso-2.5.2. jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Nachdem Sie die Android-Bibliothek dekompiliert haben, untersuchen Sie den Quellcode. Im Allgemeinen sollten Sie Folgendes suchen:

- **Klassen, die Merkmale der Verschleierung** aufweisen &ndash; Zu den Merkmalen von verfugenten Klassen gehören:

  - Der Klassenname enthält eine **$** , d. h. **eine $.-Klasse** .
  - Der Klassenname ist vollständig in Kleinbuchstaben gefährdet, d. h. **eine.-Klasse** .      

- &ndash; **Anweisungen für nicht referenzierte Bibliotheken identifizieren die Bibliothek, auf die nicht verwiesen wird, und fügen diese Abhängigkeiten dem xamarin. Android-Bindungs Projekt mit einer Buildaktion von referencejar oder hinzu. `import`**   **Embedddebug ferencejar**.

> [!NOTE]
> Das dekompilieren einer Java-Bibliothek ist möglicherweise nicht zulässig oder unterliegt den rechtlichen Beschränkungen, die auf lokalen Gesetzen oder der Lizenz basieren, in der die Java-Bibliothek veröffentlicht wurde. Eintragen Sie ggf. die Dienste eines juristischen Spezialisten ein, bevor Sie versuchen, eine Java-Bibliothek zu dekompilieren und den Quellcode zu überprüfen.


## <a name="inspect-apixml"></a>API überprüfen. Basi

Im Rahmen der Erstellung eines Bindungs Projekts generiert xamarin. Android den XML-Dateinamen **obj/Debug/API. XML**:

!["API. xml" wurde unter obj/Debug generiert.](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Diese Datei enthält eine Liste aller Java-APIs, die von xamarin. Android gebunden werden. Der Inhalt dieser Datei kann dabei helfen, fehlende Typen oder Methoden zu identifizieren, doppelte Bindung. Obwohl die Überprüfung dieser Datei mühsam und zeitaufwändig ist, kann Sie Hinweise darauf liefern, was zu Bindungs Problemen führen könnte. Beispielsweise kann " **API. XML** " anzeigen, dass eine Eigenschaft einen unpassenden Typ zurückgibt, oder dass zwei Typen vorhanden sind, die denselben verwalteten Namen haben.


## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden einige der häufigsten Fehlermeldungen oder Symptome aufgelistet, die bei dem Versuch auftreten, eine Android-Bibliothek zu binden.


### <a name="problem-java-version-mismatch"></a>Problem: Nicht übereinstimmende Java-Version

Manchmal werden Typen nicht generiert, oder es können unerwartete Abstürze auftreten, da Sie im Vergleich zu dem, mit dem die Bibliothek kompiliert wurde, eine neuere oder eine ältere Version von Java verwenden. Kompilieren Sie die Android-Bibliothek mit der gleichen Version des JDK neu, die das xamarin. Android-Projekt verwendet.


### <a name="problem-at-least-one-java-library-is-required"></a>Problem: Mindestens eine Java-Bibliothek ist erforderlich.

Sie erhalten den Fehler "mindestens eine Java-Bibliothek ist erforderlich", auch wenn eine. JAR wurde hinzugefügt.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Stellen Sie sicher, dass die Buildaktion auf `EmbeddedJar`festgelegt ist. Da mehrere Buildaktionen für vorhanden sind. JAR-Dateien (z `InputJar`. `EmbeddedJar`b `ReferenceJar` . `EmbeddedReferenceJar`, und), kann der Bindungs Generator nicht automatisch erraten, welcher standardmäßig verwendet werden soll. Weitere Informationen zu Buildaktionen finden Sie unter [Erstellen von Aktionen](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problem: Die Bindungs Tools können das nicht laden. JAR-Bibliothek

Der Bindungs Bibliotheks Generator kann die nicht laden. JAR-Bibliothek.

#### <a name="possible-causes"></a>Mögliche Ursachen

Einige. JAR-Bibliotheken, die Code Verschleierung verwenden (über Tools wie proguard), können nicht von den Java-Tools geladen werden. Da unser Tool Java-Reflektion und die ASM-Byte Code Engineering-Bibliothek verwendet, können diese abhängigen Tools die verborgenen Bibliotheken ablehnen, während Android-Lauf Zeit Tools erfolgreich sein können. Um dieses Problem zu umgehen, binden Sie diese Bibliotheken in Hand, anstatt den Bindungs Generator zu verwenden.



### <a name="problem-missing-c-types-in-generated-output"></a>Problem: Fehlende C# Typen in der generierten Ausgabe.

Die Datei "Binding **. dll** " wird erstellt, es werden jedoch einige C# Java-Typen verpasst, oder die generierte Quelle wird aufgrund eines Fehlers nicht erstellt, weil fehlende Typen vorhanden sind.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dieser Fehler kann aus verschiedenen Gründen auftreten, wie unten aufgeführt:

- Die Bibliothek, die gebunden wird, kann auf eine zweite Java-Bibliothek verweisen. Wenn die öffentliche API für die gebundene Bibliothek Typen aus der zweiten Bibliothek verwendet, müssen Sie auch auf eine verwaltete Bindung für die zweite Bibliothek verweisen.

- Es ist möglich, dass eine Bibliothek aufgrund von Java-Reflektion eingefügt wurde, ähnlich wie beim Ladefehler der Bibliothek, was das unerwartete Laden von Metadaten zur Folge hat. Die Tools von xamarin. Android können diese Situation derzeit nicht lösen. In einem solchen Fall muss die Bibliothek manuell gebunden werden.

- In der .NET 4,0-Laufzeit ist ein Fehler aufgetreten, bei dem Assemblys nicht geladen werden konnten. Dieses Problem wurde in der .NET 4,5-Laufzeit behoben.

- Java ermöglicht das Ableiten einer öffentlichen Klasse von einer nicht öffentlichen Klasse, aber dies wird in .net nicht unterstützt. Da der Bindungs Generator keine Bindungen für nicht öffentliche Klassen generiert, können abgeleitete Klassen wie diese nicht ordnungsgemäß generiert werden. Um dieses Problem zu beheben, entfernen Sie entweder den Metadateneintrag für die abgeleiteten Klassen mithilfe des Remove-Node in der Datei " **Metadata. XML**", oder korrigieren Sie die Metadaten, die die nicht öffentliche Klasse öffentlich macht. Obwohl die zweite Lösung die Bindung so erstellt, dass die C# Quelle erstellt wird, sollte die nicht öffentliche Klasse nicht verwendet werden.

  Beispiel:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Tools, die Java-Bibliotheken verbergen, können den xamarin. Android-Bindungs Generator und seine Fähigkeit zum Generieren C# von Wrapper Klassen beeinträchtigen. Der folgende Code Ausschnitt zeigt, wie Sie **Metadata. XML** aktualisieren, um einen Klassennamen zu verbergen:

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problem: Die C# generierte Quelle wird aufgrund eines Parametertyp Konflikts nicht erstellt.

Die generierte C# Quelle wird nicht erstellt. Die Parametertypen der überschriebenen Methode stimmen nicht ab.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Xamarin. Android enthält eine Vielzahl von Java-Feldern, die den-Auflistungen C# in den-Bindungen zugeordnet werden. Diese können typinkompatibilitäten in den generierten Bindungen verursachen. Um dieses Problem zu beheben, müssen die vom Bindungs Generator erstellten Methoden Signaturen so geändert werden, dass Sie die-enums verwenden. Weitere Informationen finden Sie unter [korrigieren](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)von Aufständen.

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problem: Noclassde ffounderror beim Verpacken

`java.lang.NoClassDefFoundError`wird im Paketierungs Schritt ausgelöst.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Der wahrscheinlichste Grund für diesen Fehler besteht darin, dass dem Anwendungsprojekt ( **. csproj**) eine erforderliche Java-Bibliothek hinzugefügt werden muss. . JAR-Dateien werden nicht automatisch aufgelöst. Eine Java-Bibliotheks Bindung wird nicht immer für eine Benutzerassembly generiert, die auf dem Zielgerät oder Emulator nicht vorhanden ist (z. b. Google Maps **Maps. jar**). Dies gilt nicht für die Unterstützung von Android-Bibliotheks Projekten als Bibliothek. JAR ist in die Bibliotheks-DLL eingebettet. Beispiel: [Fehler 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problem: Doppelte benutzerdefinierte EventArgs-Typen

Der Buildvorgang schlägt aufgrund doppelter benutzerdefinierter EventArgs-Typen fehl. Ein solcher Fehler tritt auf:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dies liegt daran, dass ein Konflikt zwischen Ereignis Typen besteht, die von mehr als einem Listenertyp der Schnittstelle stammen, der Methoden mit identischen Namen freigibt. Wenn beispielsweise zwei Java-Schnittstellen vorhanden sind, wie im folgenden Beispiel gezeigt, erstellt `DismissScreenEventArgs` der Generator sowohl `MediationBannerListener` für als auch `MediationInterstitialListener`für, was zu einem Fehler führt.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Dies ist beabsichtigt, damit lange Namen für Ereignis Argument Typen vermieden werden. Um diese Konflikte zu vermeiden, sind einige metadatentransformationen erforderlich. Bearbeiten Sie [**Transforms\Metadata.XML**](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) , und `argsType` fügen Sie ein Attribut für eine der Schnittstellen (oder für die Schnittstellen Methode) hinzu:

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

### <a name="problem-class-does-not-implement-interface-method"></a>Problem: Klasse implementiert keine Schnittstellen Methode.

Eine Fehlermeldung wird erzeugt, die anzeigt, dass eine generierte Klasse keine Methode implementiert, die für eine Schnittstelle erforderlich ist, die von der generierten Klasse implementiert wird. Wenn Sie sich jedoch den generierten Code ansehen, sehen Sie, dass die-Methode implementiert ist.

Im folgenden finden Sie ein Beispiel für den Fehler:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dies ist ein Problem, das bei der Bindung von Java-Methoden mit kovarianten Rückgabe Typen auftritt. In diesem Beispiel muss die- `Oauth.Signpost.Http.IHttpRequest.UnWrap()` Methode zurückgeben `Java.Lang.Object`. Die-Methode `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` weist jedoch den Rückgabetyp auf `HttpURLConnection`. Es gibt zwei Möglichkeiten, dieses Problem zu beheben:

- Fügen Sie eine partielle Klassen `HttpURLConnectionRequestAdapter` Deklaration für `IHttpRequest.Unwrap()`und explizit implementieren:

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- Entfernen Sie die Kovarianz aus C# dem generierten Code. Dies umfasst das Hinzufügen der folgenden Transformation in **Transforms\Metadata.XML** , die dazu C# führt, dass der generierte Code den `Java.Lang.Object`Rückgabetyp hat:

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problem: Namenskonflikte bei inneren Klassen/Eigenschaften

In Konflikt stehende Sichtbarkeit für geerbte Objekte.

In Java ist es nicht erforderlich, dass eine abgeleitete Klasse die gleiche Sichtbarkeit wie Ihr übergeordnetes Element hat. Java wird dies einfach für Sie beheben. In C#muss dies explizit sein, daher müssen Sie sicherstellen, dass alle Klassen in der Hierarchie über die entsprechende Sichtbarkeit verfügen. Im folgenden Beispiel wird gezeigt, wie ein Java-Paketname `Evernote.AndroidJob`von `com.evernote.android.job` in geändert wird:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problem: A **. so** wird die Bibliothek, die für die Bindung erforderlich ist, nicht geladen

Einige Bindungs Projekte können auch von Funktionen in einer **. so** -Bibliothek abhängen. Es ist möglich, dass xamarin. Android die **. so** -Bibliothek nicht automatisch lädt. Wenn der umschließende Java-Code ausgeführt wird, kann xamarin. Android den jni-Befehl nicht ausführen _und die Fehlermeldung "java. lang. unbefriefedlinkerror": Native Methode nicht gefunden:_ wird in der logcat-Ausgabe für die Anwendung angezeigt.

Die Behebung hierfür besteht darin, die **. so** -Bibliothek mit einem-Befehl manuell `Java.Lang.JavaSystem.LoadLibrary`zu laden. Angenommen, ein xamarin. Android-Projekt hat eine freigegebene Bibliothek libpocketsphinx_jni, die im Bindungs Projekt mit einer Buildaktion von **embeddednativelibrary**enthalten ist, den folgenden Code Ausschnitt (vor der Verwendung der freigegebenen Bibliothek ausgeführt) **.** lädt die **. so** -Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir allgemeine Problem Behandlungs Probleme im Zusammenhang mit Java-Bindungen aufgelistet und erläutert, wie Sie aufgelöst werden.


## <a name="related-links"></a>Verwandte Links

- [Bibliotheks Projekte](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Arbeiten mit jni](~/android/platform/java-integration/working-with-jni.md)
- [Diagnoseausgabe aktivieren](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin für Android-Entwickler](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
