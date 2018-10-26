---
title: Problembehandlung von Bindungen
description: Dieser Artikel beschreibt einige häufig auftretende Fehler beim Generieren von Bindungen, zusammen mit möglichen Ursachen und Vorschläge zu deren Behebung.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: f54da980834b44bbca7dc8619943769f8f429a7a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115288"
---
# <a name="troubleshooting-bindings"></a>Problembehandlung von Bindungen

_Dieser Artikel beschreibt einige häufig auftretende Fehler beim Generieren von Bindungen, zusammen mit möglichen Ursachen und Vorschläge zu deren Behebung._


## <a name="overview"></a>Übersicht

Binden eine Android-Bibliothek (eine **aar** oder **JAR**) Datei ist nur selten eine einfache Angelegenheit, die sie in der Regel erfordert zusätzlichen Aufwand zur Milderung von Problemen, die aus der Unterschiede zwischen Java und .NET.
Diese Probleme werden verhindert, dass Xamarin.Android binden die Android-Bibliothek und sich selbst als Fehlermeldungen in das Buildprotokoll vorhanden. Dieses Handbuch wird bieten einige Tipps für die Problembehandlung, sind einige der häufigsten Probleme/Szenarien und mögliche Lösungen für die Bindung erfolgreich auf der Android-Bibliothek bereitstellen.

Wenn Sie eine vorhandene Android-Bibliothek zu binden, ist es erforderlich, die folgenden Punkte bedenken:

- **Die externen Abhängigkeiten für die Bibliothek** &ndash; beliebige Java-Abhängigkeiten, die erforderlich sind, von der Android-Bibliothek enthalten sein müssen, in das Xamarin.Android-Projekt als eine **ReferenceJar** oder als ein  **EmbeddedReferenceJar**.

- **Die Android-API-Ebene angegeben, wird die Android-Bibliothek** &ndash; sie ist nicht möglich, die Android-API-Ebene "heruntergestuft", und stellen Sie sicher, dass die Bindung Xamarin.Android-Projekt die gleiche API (oder höher) als Ziel verwendet als die Android-Bibliothek.

- **Die Version des Android JDK, der verwendet wurde, um die Android-Bibliothek zu verpacken** &ndash; Bindungsfehlern auftreten, wenn die Android-Bibliothek mit einer anderen Version des JDK als bei Verwendung von Xamarin.Android erstellt wurde. Wenn möglich, kompilieren Sie erneut mit der Android-Bibliothek mit derselben Version des JDK, die durch die Installation von Xamarin.Android verwendet wird.

Der erste Schritt zur Behandlung von Problemen mit der Bindung einer Xamarin.Android-Bibliothek ist zu [Diagnoseausgabe von MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Nach der Aktivierung der Diagnoseausgabe an, die Xamarin.Android-Bindung-Projekt neu, und überprüfen Sie das Buildprotokoll um Hinweise zu den neuerungen die Ursache des Problems zu suchen.

Sie können auch als hilfreich erweisen, dekompilieren von Android-Bibliothek, und untersuchen Sie die Typen und Methoden, die von Xamarin.Android versucht zu binden. Dies wird später in diesem Handbuch ausführlicher behandelt.


## <a name="decompiling-an-android-library"></a>Dekompilieren von Android-Bibliothek

Überprüfen die Klassen und Methoden der Java-Klassen kann wertvolle Informationen bereitstellen, der in der Bindung einer Bibliotheks dabei helfen.
[JD-GUI](http://jd.benow.ca/) ist ein grafisches Hilfsprogramm, die Java-Quellcode aus anzeigen können die **Klasse** in eine JAR-Datei enthaltenen Dateien. Es kann als eigenständige Anwendung oder einem-Plug-In für IntelliJ oder Eclipse ausgeführt werden.

Öffnen Sie ein Android-Bibliothek dekompiliert die **. JAR-** -Datei mit der Java-Decompiler. Wenn die Bibliothek ist eine **. Zusätzlich zum AAR** -Datei, ist es erforderlich, die Datei extrahieren **classes.jar** aus der Archivdatei. Im folgenden finden Sie eine Beispiel-Screenshot der JD-GUI verwenden, zum Analysieren der [Picasso](http://square.github.io/picasso/) JAR:

![Verwenden die Java-Decompiler zur Analyse von Picasso-2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Nachdem Sie die Android-Bibliothek dekompiliert haben, überprüfen Sie den Quellcode. Suchen Sie im Allgemeinen nach:

- **Klassen, die Merkmale der Verschleierung** &ndash; Merkmale der verborgenen Klassen enthalten:

    - Enthält den Namen der Klasse eine **$**, d. h. **eine$ .class**
    - Den Namen der Klasse von Kleinbuchstaben, d. h. vollständig gefährdet **a.class**      

- **`import` Anweisungen für die Unreferenzierte Bibliotheken** &ndash; identifizieren die nicht referenzierte Bibliothek, und fügen Sie diese Abhängigkeiten zu dem Xamarin.Android-Bindung-Projekt mit einem **Buildvorgang** von **ReferenceJar**  oder **EmbedddedReferenceJar**.

> [!NOTE]
> Eine Java-Bibliothek dekompilieren ist möglicherweise nicht zulässig oder gelten gesetzliche Einschränkungen basierend auf den lokalen Gesetze verstößt oder die Lizenz, die unter der die Java-Bibliothek veröffentlicht wurde. Bei Bedarf eingetragen Sie die Dienste von einem rechtliche Supportmitarbeiter, die vor dem Versuch, eine Java-Bibliothek dekompilieren, und überprüfen den Quellcode werden.


## <a name="inspect-apixml"></a>Überprüfen Sie die API. XML

Im Rahmen der Erstellung eines Projekts für die Bindung, wird Xamarin.Android eine XML-Dateiname generiert **obj/Debug/api.xml**:

![Generierte api.xml unter Obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Diese Datei enthält einer Liste aller Java-APIs, dass Xamarin.Android Bindung versucht. Der Inhalt dieser Datei können keine fehlenden Typen oder Methoden zu identifizieren, doppelte Bindung. Obwohl die Überprüfung dieser Datei mühsam und zeitaufwändig ist, kann es für Hinweise auf die Bindungsprobleme verursachen könnte, bereitstellen. Z. B. **api.xml** möglicherweise, dass eine Eigenschaft ein nicht geeigneten Typs zurückgibt, oder werden zwei dieser Freigabe verwalteter gleichnamigen Typen anzuzeigen.


## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden einige der allgemeinen Fehlermeldungen oder Symptome aufgeführt, die meine auftreten, wenn versucht wird, um eine Android-Bibliothek zu binden.


### <a name="problem-java-version-mismatch"></a>Problem: Java-Versionskonflikt

Manchmal Typen nicht generiert werden, oder unerwarteten Abstürze auftreten, da Sie verwenden entweder eine neuere oder frühere Version von Java, die im Vergleich zu, was die Bibliothek mit kompiliert wurde. Kompilieren Sie die Android-Bibliothek mit der gleichen Version des JDK, die Ihrem Xamarin.Android-Projekt verwendet wird.


### <a name="problem-at-least-one-java-library-is-required"></a>Problem: mindestens Java-Clientbibliothek ist erforderlich

Sie erhalten den Fehler "mindestens eine Java-Clientbibliothek ist erforderlich,", obwohl ein. JAR-Datei wurde hinzugefügt.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Stellen Sie sicher, dass der Buildvorgang nastaven NA hodnotu `EmbeddedJar`. Da es sich um mehrere Buildaktionen für. JAR-Dateien (z. B. `InputJar`, `EmbeddedJar`, `ReferenceJar` und `EmbeddedReferenceJar`), der Generator für die Bindung kann nicht automatisch erraten, welcher Typ in der Standardeinstellung verwendet. Weitere Informationen zu den Buildvorgängen, finden Sie unter [erstellen Aktionen](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problem: Binden von Tools kann nicht geladen werden die. JAR-Bibliothek

Die Bindung-Bibliothek-Generator kann nicht geladen der. JAR-Bibliothek.

#### <a name="possible-causes"></a>Mögliche Ursachen

Einige. JAR-Bibliotheken, mit denen codeobfuskation (mit Tools wie Proguard) können nicht von der Java-Tools nicht geladen werden. Da unser Tool Verwendung von Java-Reflektion und den ASM-Bytecode, die technische Bibliothek ist, können diese abhängigen Tools die verborgenen Bibliotheken ablehnen, während der Tools von Android-Runtime übergeben werden können. Dieses Problem zu umgehen, werden von Hand diese Bibliotheken anstelle des Generators für die Bindung binden.



### <a name="problem-missing-c-types-in-generated-output"></a>Problem: Fehlende C# Typen in der generierten Ausgabe.

Die Bindung **DLL** builds sind jedoch einige Java-Typen verwendet werden soll, oder die generierte-Fehler C# Quelle nicht aufgrund eines Fehlers, der darauf hinweist, es gibt fehlende Typen erstellt.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dieser Fehler kann verschiedene Ursachen auftreten, wie im folgenden aufgeführt:

-   Die Bibliothek gebunden wird, kann eine zweite Java-Bibliothek verweisen. Wenn die öffentliche API für die Bibliothek des gebundenen Typen aus der zweiten-Bibliothek verwendet, müssen Sie eine verwaltete Bindung für den zweiten Bibliothek als auch verweisen.

-   Es ist möglich, dass eine Bibliothek aufgrund von Java-Reflektion, ähnlich wie der Grund für die Bibliothek Ladefehler oben, wodurch unerwartete Laden von Metadaten eingefügt wurde. Der Xamarin.Android-Tools kann nicht gerade dieses Problem zu beheben. In diesem Fall muss die Bibliothek manuell gebunden werden.

-   Gab es ein Fehler in .NET 4.0-Laufzeit, die Fehler beim Laden von Assemblys, wenn sie haben soll. Dieses Problem wurde in der Runtime von .NET 4.5 behoben wurde.

-   Java kann nicht öffentlichen Klasse eine öffentliche Klasse abgeleitet, aber dies wird in .NET nicht unterstützt. Da der Generator für die Bindung keine Bindungen für den nicht öffentlichen Klassen generiert, abgeleitete Klassen wie z. B., wenn diese nicht ordnungsgemäß generiert werden können. Um dieses Problem zu beheben, entfernen Sie entweder der Metadateneintrag für diese abgeleiteten Klassen, die mithilfe des Remove-Knotens im **Metadata.xml**, oder korrigieren Sie die Metadaten, die den nicht öffentlichen Klasse öffentliche stammt. Obwohl die zweite Lösung, die Bindung erstellt wird, damit die C# Datenquelle erstellen, die nicht öffentlichen Klasse sollte nicht verwendet werden.

    Zum Beispiel:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Tools, mit denen Verschleiern von Java-Bibliotheken der Generator für die Xamarin.Android-Bindung und die Möglichkeit zum Generieren beeinträchtigen C# Wrapperklassen. Der folgende Codeausschnitt zeigt, wie Sie update **Metadata.xml** um einen Klassennamen unobfuscate:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problem: Generiert C# Quelle wird aufgrund eines typenkonflikts Parameter nicht erstellt.

Die generierte C# Quelle wird nicht erstellt werden. Überschreiben Parameter der Methode, die Typen nicht übereinstimmen.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Xamarin.Android enthält eine Reihe von Java-Felder, die Enumerationen in zugeordnet sind, die C# Bindungen. Dies können dazu führen, dass inkompatible Typen in den generierten Bindungen. Dieses Problem lösen, müssen die Methodensignaturen, die von der Bindung-Generator erstellt geändert werden, um die Enumerationen zu verwenden. Weitere Informationen (englischsprachig), finden Sie unter [Korrigieren von Enumerationen](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problem: NoClassDefFoundError als Paket

`java.lang.NoClassDefFoundError` in die paketerstellung ausgelöst.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Die wahrscheinlichste Ursache für diesen Fehler ist, dass das Anwendungsprojekt hinzugefügt werden, eine erforderliche Java-Bibliothek muss (**csproj**). . JAR-Dateien werden nicht automatisch aufgelöst. Eine Java-Bibliothek-Bindung ist nicht immer generiert für eine Benutzerassembly, die in dem Gerät oder Emulator nicht vorhanden ist (z. B. Google Maps **maps.jar**). Dies ist nicht der Fall für die Bibliothek für Android-projektunterstützung, die der Typbibliothek. JAR-Datei wird in der Library-Dll eingebettet. Zum Beispiel: [Fehler 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problem: Doppelte benutzerdefinierte EventArgs-Typen

Buildfehler aufgrund von doppelten benutzerdefinierte EventArgs-Typen. Fehler wie folgt:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dies ist, da Konflikte zwischen Ereignistypen, die mehr als eine Schnittstellentyps "Listener" enthalten sind, die Methoden, die mit identischen Namen freigibt. Z. B. Wenn Sie zwei Java-Schnittstellen vorhanden sind, wie im folgenden Beispiel dargestellt, die der Generator erstellt `DismissScreenEventArgs` für beide `MediationBannerListener` und `MediationInterstitialListener`, wodurch des Fehlers.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Dies ist beabsichtigt, damit an, dass lange Namen auf Ereignisargumenttypen vermieden werden. Um diese Konflikte zu vermeiden, ist einige Metadaten-Transformationen erforderlich. Bearbeiten Sie [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) und Hinzufügen einer `argsType` Attribut auf eine der Schnittstellen (oder auf die Schnittstellenmethode):

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

### <a name="problem-class-does-not-implement-interface-method"></a>Problem: Klasse Schnittstellenmethode nicht implementiert

Gibt an, dass es sich bei eine generierte Klasse eine Methode nicht implementiert, die für eine Schnittstelle erforderlich ist, die die von die generierte Klasse implementiert eine Fehlermeldung ausgegeben. Allerdings sehen möchten den generierten Code, Sie, dass die Methode implementiert wird.

Hier ist ein Beispiel des Fehlers:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dies ist ein Problem mit einer Java-Methoden mit kovariante Rückgabetypen Bindung. In diesem Beispiel ist die Methode `Oauth.Signpost.Http.IHttpRequest.UnWrap()` zurückgeben muss `Java.Lang.Object`. Jedoch die Methode `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` hat einen Rückgabetyp von `HttpURLConnection`. Es gibt zwei Möglichkeiten, um dieses Problem zu beheben:

-   Fügen Sie die Deklaration einer partiellen Klasse für `HttpURLConnectionRequestAdapter` und explizit `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   Entfernen Sie die Kovarianz, aus der generierten C# Code. Dies umfasst die folgende Transformation hinzufügen **Transforms\Metadata.xml** das führt dazu, dass die generierte C# Code einen Rückgabetyp haben `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problem: Namenskonflikte auf innere Klassen / Eigenschaften

In Konflikt stehende Sichtbarkeit für geerbte Objekte.

In Java ist es nicht erforderlich, dass eine abgeleitete Klasse haben die gleiche Sichtbarkeit wie das übergeordnete Element. Java wird nur für Sie behoben. In C#, die explizit sein muss, daher müssen Sie sicherstellen, haben alle Klassen in der Hierarchie die geeignete Sichtbarkeit. Das folgende Beispiel zeigt, wie Sie einen Java-Paketnamen aus ändern `com.evernote.android.job` zu `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problem: Ein **so** Bibliothek, die von der Bindung erforderlich ist, nicht geladen.

Einige bindungsprojekte hängt auch von Funktionen in einer **so** Bibliothek. Es ist möglich, dass Xamarin.Android nicht automatisch geladen werden die **so** Bibliothek. Wenn die umschlossene Java-Code ausgeführt wird, Xamarin.Android der JNI-Aufruf und die Fehlermeldung fehl _java.lang.UnsatisfiedLinkError: Native Methode wurde nicht gefunden:_ wird in der Logcat, für die Anwendung angezeigt.

Die Lösung manuell laden, wird die **so** Bibliothek mit einem Aufruf von `Java.Lang.JavaSystem.LoadLibrary`. Zum Beispiel davon aus, dass ein Xamarin.Android-Projekt über eine freigegebene Bibliothek verfügt **libpocketsphinx_jni.so** enthalten, die im bindungsprojekt mit einer Buildaktion von **EmbeddedNativeLibrary**, der folgende Codeausschnitt (wird ausgeführt, bevor Sie die freigegebene Bibliothek verwenden) lädt die **so** Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden allgemeine Fragen, die zur Problembehandlung in Zusammenhang mit der Java-Bindungen aufgeführt und erläutert, wie Sie zu lösen.


## <a name="related-links"></a>Verwandte Links

- [Library-Projekten](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Aktivieren Sie die Diagnoseausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin für Android-Entwickler](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
