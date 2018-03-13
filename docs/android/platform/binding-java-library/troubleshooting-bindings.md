---
title: Problembehandlung von Bindungen
description: "Dieser Artikel beschreibt mehrere häufige Fehler, die auftreten können, während der Generierung Bindungen wird zusammen mit möglichen Ursachen und Vorschläge zur Problembehebung."
ms.topic: article
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6d31e2a22c63f8d46893dd1928b561e1a06b19b4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshooting-bindings"></a>Problembehandlung von Bindungen

_Dieser Artikel beschreibt mehrere häufige Fehler, die auftreten können, während der Generierung Bindungen wird zusammen mit möglichen Ursachen und Vorschläge zur Problembehebung._


## <a name="overview"></a>Übersicht

Binden eine Bibliothek für Android (ein **aar** oder ein **JAR**) Datei ist nur selten eine einfache Angelegenheit; in der Regel ist erforderlich, um Probleme zu verringern, die durch die Unterschiede zwischen Java- und verursacht zusätzlichen Aufwand.
Diese Probleme werden verhindert, dass Xamarin.Android binden die Bibliothek für Android und selbst als Fehlermeldungen im Buildprotokoll präsentieren. Dieses Handbuch bieten einige Tipps für die Problembehandlung, sind einige der häufigeren Probleme/Szenarien und mögliche Lösungen für die Bindung erfolgreich die Bibliothek für Android bereitstellen.

Wenn Sie eine vorhandene Android-Bibliothek zu binden, ist es notwendig, die folgenden Punkte bedenken:

- **Die externen Abhängigkeiten für die Bibliothek** &ndash; im Projekt Xamarin.Android als beliebige Java-Abhängigkeiten von der Android-Bibliothek erforderlich enthalten sein müssen eine **ReferenceJar** oder als ein  **EmbeddedReferenceJar**.

- **Die Android-API-Ebene, dass die Bibliothek für Android Anwendung ist** &ndash; es ist nicht möglich, "ein downgrade die Android-API-Ebene"; Stellen Sie sicher, dass die Xamarin.Android Bindung-Projekt die gleiche API (oder höher) abzielt als der Android-Bibliothek.

- **Die Version des JDK Android, die verwendet wurde, so verpacken Sie die Bibliothek für Android** &ndash; Bindungsfehlern auftreten, wenn die Android-Bibliothek mit einer anderen Version des JDK als die Verwendung von Xamarin.Android erstellt wurde. Wenn möglich, kompilieren Sie erneut mit der Android-Bibliothek mit der gleichen Version des JDK, die durch die Installation von Xamarin.Android verwendet wird.

Der erste Schritt beim Behandeln von Problemen mit eine Bibliothek Xamarin.Android Bindung ist so aktivieren Sie [Diagnoseausgabe MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Nach der Aktivierung der Diagnoseausgabe an, das Xamarin.Android Bindung-Projekt neu, und untersuchen Sie das Buildprotokoll suchen Hinweise dazu, was die Ursache des Problems ist.

Es kann auch nachweisen hilfreich, zu dekompilieren, die Bibliothek für Android aus, und untersuchen Sie die Typen und Methoden, die Xamarin.Android versucht zu binden. Dieser Vorgang wird später in diesem Handbuch ausführlicher beschrieben.


## <a name="decompiling-an-android-library"></a>Dekompilieren eine Bibliothek für Android

Überprüfen die Klassen und Methoden von der Java-Klassen kann wertvolle Informationen bereitstellen, der bei der Bindung einer Bibliotheks helfen.
[JD GUI](http://jd.benow.ca/) ist ein grafisches Hilfsprogramm, das von Java-Quellcode anzeigen kann die **Klasse** in eine JAR-Datei enthaltenen Dateien. Sie kann als eigenständige Anwendung oder als ein plug-in für IntelliJ oder Eclipse ausgeführt werden.

Eine Bibliothek für Android Open dekompiliert die **. JAR-** -Datei mit der Java-dekompilieren. Wenn die Bibliothek ist ein **. AAR** Datei, ist es erforderlich, beim Extrahieren der Datei **classes.jar** aus der Archivdatei. Im folgenden finden Sie einen Screenshot Beispiel der Verwendung von JD-GUI zum Analysieren der [Picasso](http://square.github.io/picasso/) JAR:

![Verwenden die Java-dekompilieren zur Analyse von Picasso 2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Nachdem Sie die Bibliothek für Android dekompiliert haben, überprüfen Sie den Quellcode einfügen. Im Allgemeinen suchen:

- **Klassen, die Merkmale des obfuskation** &ndash; Eigenschaften von verborgenen Klassen umfassen:

    - Der Name der Klasse enthält eine  **$** , d. h. **eine$ .class**
    - Der Name der Klasse vollständig gefährdet ist von Kleinbuchstaben, d. h. **a.class**      

- **`import` Anweisungen für die Unreferenzierte Bibliotheken** &ndash; identifizieren die nicht referenzierte Bibliothek, und fügen Sie diese Abhängigkeiten zum Projekt Xamarin.Android Bindung mit einer **Buildvorgang** von **ReferenceJar**  oder **EmbedddedReferenceJar**.

> [!NOTE]
> Dekompilieren einer Java-Bibliothek ist möglicherweise nicht zulässig oder unterliegen rechtlichen Grundlage örtlich anwendbaren Gesetzen oder die Lizenz, unter der die Java-Bibliothek veröffentlicht wurde. Bei Bedarf eingetragen Sie werden die Dienste von einer rechtliche Professional vor dem Versuch, dekompilieren einer Java-Bibliothek, und überprüfen den Quellcode einfügen.


## <a name="inspect-apixml"></a>Überprüfen Sie die API. XML

Als Teil der Erstellung eines Projekts für die Bindung, Xamarin.Android generiert ein XML-Dateiname **obj/Debug/api.xml**:

![Generierte api.xml unter Obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Diese Datei enthält eine Liste aller Java-APIs, die Xamarin.Android Bindung versucht wird. Der Inhalt dieser Datei können identifizieren fehlender Typen oder Methoden, die doppelte Bindung. Auch Prüfung dieser Datei mühsam und zeitaufwändig ist, bieten sie Hinweise auf die Bindungsprobleme verursachen könnte. Beispielsweise **api.xml** kann offenlegen, dass eine Eigenschaft einen ungeeigneten Typ zurückgeben ist oder, es zwei gibt dieser Freigabe verwalteter gleichnamigen Typen.


## <a name="known-issues"></a>Bekannte Probleme

In diesem Abschnitt werden einige der allgemeinen Fehlermeldungen oder Symptome aufgeführt, die meine auftreten, wenn Sie versuchen, eine Bibliothek für Android zu binden.


### <a name="problem-java-version-mismatch"></a>Problem: Java-Versionskonflikt

In einigen Fällen Typen nicht generiert werden oder unerwartete abstürzen können auftreten, weil Sie verwenden eine neuere und ältere Version von Java-im Vergleich zu, was mit die Bibliothek kompiliert wurde. Kompilieren Sie erneut die Android-Bibliothek mit der gleichen Version des JDK, die Ihr Projekt Xamarin.Android verwendet wird.


### <a name="problem-at-least-one-java-library-is-required"></a>Problem: mindestens eine Java-Bibliothek ist erforderlich

Die Fehlermeldung "mindestens eine Java-Bibliothek ist ein Pflichtfeld,", obwohl ein. JAR-Datei wurde hinzugefügt.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Stellen Sie sicher, dass die Buildaktion auf festgelegt ist `EmbeddedJar`. Da es sich um mehrere Buildvorgänge für. JAR-Dateien (z. B. `InputJar`, `EmbeddedJar`, `ReferenceJar` und `EmbeddedReferenceJar`), der Generator für die Bindung kann nicht automatisch zu erraten, welcher Typ in der Standardeinstellung verwendet. Weitere Informationen zu den Buildvorgängen, finden Sie unter [erstellen Aktionen](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problem: Binden von Tools kann nicht geladen werden die. JAR-Bibliothek

Die Bindung Library-Generator kann nicht geladen der. JAR-Bibliothek.

#### <a name="possible-causes"></a>Mögliche Ursachen

Einige. JAR-Bibliotheken, die Code Verbergung (über Tools, z. B. Proguard) verwenden, können von der Java-Tools geladen werden. Da unserem Werkzeug Verwendung von Java-Reflektion und der technischen Bibliothek ASM Bytecode vornimmt, können diese abhängigen Tools verborgenen Bibliotheken ablehnen, Android Laufzeittools übergeben werden können. Dieses Problem zu umgehen, werden von Hand diese Bibliotheken anstelle des Generators für die Bindung binden.



### <a name="problem-missing-c-types-in-generated-output"></a>Problem: Fehlende C#-Typen in der generierten Ausgabe.

Die Bindung **DLL** Builds Cachefehler jedoch einige Java-Typen, oder der generierte C#-Quellcode werden aufgrund eines Fehlers, der darauf hinweist, fehlende stehen keine erstellt.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dieser Fehler kann verschiedene Ursachen auftreten, wie im folgenden aufgeführt:

-   Die Bibliothek gebunden wird, kann eine zweite Java-Bibliothek verweisen. Wenn die öffentliche API für die Bibliothek gebundene Typen aus der zweiten Bibliothek verwendet wird, müssen Sie eine verwaltete Bindung für den zweiten Bibliothek als auch verweisen.

-   Es ist möglich, dass eine Bibliothek aufgrund von Java-Reflektion, die den Grund für die Bibliothek-Ladefehler oben, verursacht das unerwartete Laden der Metadaten ähnelt eingeschleust wurde. Diese Situation kann nicht von Xamarin.Android des Tools derzeit aufgelöst werden. In diesem Fall muss die Bibliothek manuell gebunden werden.

-   Es wurde ein Fehler in .NET 4.0-Laufzeit, die Fehler beim Laden von Assemblys bei verfügen sollte. Dieses Problem wurde in .NET 4.5-Runtime behoben wurde.

-   Java kann nicht öffentliche Klasse eine öffentliche Klasse ableiten, aber dies wird in .NET nicht unterstützt. Da die Bindung-Generator keine Bindungen für den nicht öffentlichen Klassen generiert, abgeleitete Klassen wie z. B., wenn diese nicht ordnungsgemäß generiert werden können. Um dieses Problem zu beheben, entfernen Sie entweder der Metadateneintrag für diese abgeleiteten Klassen, die mithilfe des Remove-Knotens im **Metadata.xml**, oder korrigieren Sie die Metadaten, die die Klasse nicht öffentlich öffentliche ausgeführt hat. Obwohl die zweite Lösung, damit der C#-Quellcode erstellen, wird die Bindung erstellen, sollte die nicht öffentlichen Klasse nicht verwendet werden.

    Zum Beispiel:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Tools, die Bibliotheken für Java zu verbergen können mit der Bindung Xamarin.Android-Generator und der Fähigkeit zum Generieren von C#-Wrapperklassen beeinträchtigen. Der folgende Codeausschnitt zeigt die Vorgehensweise beim Aktualisieren **Metadata.xml** auf einen Klassennamen unobfuscate:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problem: Generierten C#-Quellcode werden keine Parameter Datentypkonflikt erstellt.

Der generierte C#-Quellcode werden keine erstellt. Überschreiben der Methodenparameter, die Datentypen nicht übereinstimmen.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Xamarin.Android umfasst eine Vielzahl von Java-Feldern, die Enumerationen in den C#-Bindungen zugeordnet sind. Diese können dazu führen, dass Typ Inkompatibilitäten in den generierten Bindungen. Zum Beheben dieses Problems müssen die Methodensignaturen erstellt, die von der Bindung-Generator geändert werden, um die Enumerationen zu verwenden. Weitere Informationen (englischsprachig) finden Sie unter [Korrigieren von Enumerationen](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problem: NoClassDefFoundError-Paket

`java.lang.NoClassDefFoundError` in der Verpackung Schritt ausgelöst.

#### <a name="possible-causes"></a>Mögliche Ursachen:

Die wahrscheinlichste Ursache für diesen Fehler ist, dass eine obligatorische Java-Bibliothek das Anwendungsprojekt hinzugefügt werden muss (**csproj**). . JAR-Dateien werden nicht automatisch aufgelöst. Die Bindung einer Java-Bibliothek wird nicht immer generiert für eine Benutzerassembly, die in das Zielgerät oder den Emulator nicht vorhanden ist (z. B. Google Maps **maps.jar**). Dies ist nicht der Fall für die Bibliothek für Android-projektunterstützung, wie der Bibliothek. JAR-Datei wird in der Library-Dll eingebettet. Zum Beispiel: [Fehler 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problem: Doppelte benutzerdefinierte EventArgs-Typen

Buildfehler aufgrund doppelter benutzerdefinierte EventArgs-Typen. Fehler wie folgt:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Mögliche Ursachen:

Dies ist da es Konflikte zwischen Ereignistypen, die aus mehr als ein Schnittstellentyp "Listener" stammen, die Methoden, die über identische Namen gemeinsam ist. Z. B. wenn es zwei Java-Schnittstellen, gibt wie im folgenden Beispiel dargestellt, der Generator erstellt `DismissScreenEventArgs` für beide `MediationBannerListener` und `MediationInterstitialListener`, wodurch des Fehlers.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Dies ist beabsichtigt, damit, dass lange Namen auf Ereignisargumenttypen vermieden werden. Um diese Konflikte zu vermeiden, ist die Transformation für einige Metadaten erforderlich. Bearbeiten Sie [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) und Hinzufügen einer `argsType` Attribut auf eine der Schnittstellen (oder auf die Schnittstellenmethode):

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

### <a name="problem-class-does-not-implement-interface-method"></a>Problem: Klasse ist nicht mit dem Schnittstellenmethode implementiert.

Gibt an, dass eine generierte Klasse mit eine Methode nicht implementiert, die für eine Schnittstelle erforderlich ist, mit die generierte Klasse implementiert eine Fehlermeldung ausgegeben. Betrachten den generierten Code ein, können Sie jedoch sehen, dass die Methode implementiert wird.

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

Dies ist ein Problem mit einer Bindung von Java-Methoden mit covariant-Rückgabetypen. In diesem Beispiel wird die Methode `Oauth.Signpost.Http.IHttpRequest.UnWrap()` zurückgeben muss `Java.Lang.Object`. Allerdings die Methode `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` verfügt über einen Rückgabetyp von `HttpURLConnection`. Es gibt zwei Möglichkeiten, um dieses Problem zu beheben:

-   Fügen Sie eine Deklaration der partiellen Klasse für `HttpURLConnectionRequestAdapter` und Explizites Implementieren von `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   Entfernen Sie die Kovarianz, aus dem generierten C#-Code. Dies umfasst das Hinzufügen der folgenden Transformation **Transforms\Metadata.xml** wird die dazu führen, dass des generierten C#-Codes über einen Rückgabetyp verfügen `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problem: Namenskonflikte für innere Klassen / Eigenschaften

In Konflikt stehende Sichtbarkeit für geerbte Objekte.

In Java ist es nicht erforderlich, eine abgeleitete Klasse haben die gleiche Sichtbarkeit wie das übergeordnete Objekt. Java werden nur die für Sie behoben werden kann. Daher Sie sicherstellen müssen, haben alle Klassen in der Hierarchie in c#, die explizit sein, dem entsprechenden Sichtbarkeitsstatus. Im folgende Beispiel wird gezeigt, wie aus einen Java-Paketnamen ändern `com.evernote.android.job` auf `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problem: Ein **.so** Bibliothek, die von der Bindung erforderlich ist, nicht laden.

Bei einigen Projekten Bindung können auch Funktionalität abhängig, in einem **.so** Bibliothek. Es ist möglich, dass Xamarin.Android nicht automatisch geladen werden die **.so** Bibliothek. Wenn die umschlossene Java-Code ausgeführt wird, schlägt fehl Xamarin.Android der JNI-Aufruf und die Fehlermeldung zu _java.lang.UnsatisfiedLinkError: systemeigene Methode wurde nicht gefunden:_ wird in der Logcat, für die Anwendung angezeigt.

Die Lösung zu finden, manuell laden die **.so** Bibliothek mit einem Aufruf von `Java.Lang.JavaSystem.LoadLibrary`. Z. B. vorausgesetzt, dass es sich bei einem Projekt Xamarin.Android Bibliothek freigegebenen **libpocketsphinx_jni.so** im bindungsprojekt mit einem Buildvorgang enthalten **EmbeddedNativeLibrary**, die die folgenden Ausschnitt (wird ausgeführt, bevor Sie die freigegebene Bibliothek verwenden) lädt die **.so** Bibliothek:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden allgemeine Problembehandlung bei Problemen im Zusammenhang mit Java-Bindungen aufgeführt und erläutert, wie diese zu lösen.


## <a name="related-links"></a>Verwandte Links

- [Library-Projekte](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Aktivieren der Speicherdiagnose](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin für Android-Entwickler](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
